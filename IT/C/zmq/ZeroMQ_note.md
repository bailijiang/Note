## Chaptor 03

* 同步/异步模式下都会在 REQ 封包时添加返回地址(id);

* 分隔符帧采用空帧 empty；

* ROUTER 接收 REQ 时会添加 id（短整型）， zmq3.0以后版本在跨越多个 ROUTER 时会降低性能；

* ROUTER 内部维护了一个 REQ (id->fd/随机端口) 的 Hash 散列表；

* DEALER 是异步分发消息， 排队接收消息；

* DEALER 可以模拟 REQ 封包和多个 REP 进行交互, 无需等待应答；

* ROUTER 通过 zmq_setsockopt (identified, ZMQ_IDENTITY, "PEER2", 5); 为 REQ 设置 id；

* inproc://xxx 是本地进程内通信，不走网卡， wireshark抓不到包；

* ROUTER 默认对无法处理(路由)的 REQ 会丢弃；

  * ZMQ 3.2 开始可以在 ROUTER 上捕获这个错误 ZMQ_ROUTER_MANDATORY;
  * 设置捕获后， ROUTER(socket) 会发出 EHOSTUNREACH 错误;

* ROUTER 中有一个 worker 队列名单， 调取 worker 时采用 lru(least resently used) 原则；

* REQ: 先发一个空帧， 再发数据帧 (同步)；

  DEALER： 不发空帧 (异步);

* ROUTER 中的 worker 队列(worker_queue) 存放的是 worker_id (wid);

* lbbroker: 通过 proxy 传递给 worker 的帧为： client_id, empty, data;

  

## Chaptor 04

* zmq_test

* LPP: REQ 非阻塞， 超时重发， 多次重发失败后放弃；

  * ```
    [localhost@zmq_test] ./lpserver
    I: normal request (1)
    I: normal request (2)
    I: normal request (3)
    I: simulating CPU overload
    I: normal request (4)
    I: simulating a crash
    ```

  * ```
    [localhost@zmq_test] ./lpclient
    I: connecting to server...
    I: server replied OK (1)
    I: server replied OK (2)
    I: server replied OK (3)
    W: no response from server, retrying...
    I: reconnecting to server...
    W: no response from server, retrying...
    I: reconnecting to server...
    E: server seems to be offline, abandoning
    ```

  * zctx_interrupted : 代表收到中断消息;

  * 抓包参考wireshark： lpp.pcap

* SPQ:  lpclient.c, spqueue.c, spworker.c, spq.pcap

  * spqueue.c: 

    * zlist_t *workers = zlist_new ();
    * zlist_append (workers, identity);
    * zlist_pop (workers)
    * zlist_destroy (&workers);

  * ```
    [localhost@zmq_test] ./spworker
    I: (21C5-EA67) worker ready
    I: (21C5-EA67) normal reply
    I: (21C5-EA67) normal reply
    I: (21C5-EA67) normal reply
    I: (21C5-EA67) simulating CPU overload
    I: (21C5-EA67) normal reply
    I: (21C5-EA67) simulating CPU overload
    I: (21C5-EA67) normal reply
    I: (21C5-EA67) simulating a crash
    ```

  * ```
    [localhost@zmq_test] ./lpclient
    I: connecting to server...
    I: server replied OK (1)
    I: server replied OK (2)
    I: server replied OK (3)
    W: no response from server, retrying...
    I: reconnecting to server...
    W: no response from server, retrying...
    I: reconnecting to server...
    E: server seems to be offline, abandoning
    ```

* PPQ: lpclient.c  ppqueue.c  ppworker.c, ppq.pcap

  * Queue 和 DEALER(worker) 增加了 Heartbeat;

    * 该队列装置使用心跳机制扩展了LRU模式

  * ```
    [localhost@zmq_test] ./ppworker
    I: worker ready
    I: worker heartbeat
    I: worker heartbeat
    I: worker heartbeat
    I: worker heartbeat
    I: worker heartbeat
    I: worker heartbeat
    I: worker heartbeat
    I: worker heartbeat
    I: worker heartbeat
    I: normal reply
    I: worker heartbeat
    I: normal reply
    I: worker heartbeat
    I: normal reply
    I: worker heartbeat
    I: normal reply
    I: worker heartbeat
    I: normal reply
    I: worker heartbeat
    I: simulating a crash
    ```

  * ```
    [localhost@zmq_test] ./lpclient
    I: connecting to server...
    I: server replied OK (1)
    I: server replied OK (2)
    I: server replied OK (3)
    I: server replied OK (4)
    I: server replied OK (5)
    W: no response from server, retrying...
    I: reconnecting to server...
    W: no response from server, retrying...
    I: reconnecting to server...
    E: server seems to be offline, abandoning
    ```

  * MDP(面向服务):  mdcliapi.c, mdclient.c, mdwrkapi.c, mdworker.c
  
    * mdclient 会在 send request 时添加一个 service name (**"echo"**), 并要求 worker 注册特定的 service name：
      * mdclient: zmsg_t *reply = mdcli_send (session, "echo", &request);
      * mdworker: mdwrk_t *session = mdwrk_new ( "tcp://localhost:5555", "echo", verbose);
    * mdbroker: 每个 service 对应一个队列， 多个 service 为一组队列；
    * mdbroker 应该把超时后仍没有 worker 注册的 service 删除掉;
    * 处理非幂等操作(broker拒绝重复请求的解决方案)： 
      * client: client端加盖 唯一标识符； msg 唯一编号；
      * 服务端发REP之前将 client ID 和 msg 编号作为键存储；
      * 服务端读取客户端的 REP 时先检查是否已有该 REP， 如果有， 应不处理请求， 直接发送 REP；
  
  * TSP:  ticlient.c, titanic.c, mdbroker.c, mdworker.c
  
    * titanic既是 client 也是 worker
    * 缺少 ticlient2.c 异步 send, recv 客户端程序, 如果要改成异步的客户端， 那么在 titanic.c 中也要把 MDG client API 改成异步的；
    * 如何在实际的环境中让 TSP 运行更快：
      * 使用单个磁盘文件(大文件)；
      * 将磁盘文件组织为一个循环缓冲区；
      * 在内存中保持索引，并在启动时从磁盘缓存重建索引 (fsync)；
      * 使用固态硬盘；
      * 预分配整个文件；
      * 可以不使用文件的模式， 而是将 REQ, REP 存储在内存中 (TSP down掉会丢失 REQ/REP)；