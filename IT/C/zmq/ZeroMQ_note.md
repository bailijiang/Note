## Chaptor 03

* ZeroMQ请求-应答封包： 

  * 同步/异步模式下都会在 REQ 封包时添加返回地址(id);

  * 分隔符帧采用空帧 empty；

  * ROUTER 接收 REQ 时会添加 id（短整型）， zmq3.0以后版本在跨越多个 ROUTER 时会降低性能；

  * ROUTER 内部维护了一个 REQ (id->fd) 的 Hash 散列表；

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

  * 