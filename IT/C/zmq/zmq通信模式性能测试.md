# 目标： 测试zmq库中REQ-REP通信机制下各种模式的数据处理性能

* 模式： 基础模式，海盗模式，管家模式，泰塔尼克模式 

## 1. 基础模式
* 测试条件:
	* client端采用 ZMQ_DEALER 模式
	* 服务端采用 zmq_proxy 模式
	* worker采用 ZMQ_DEALER 模式
	* 同步发送接收10000条数据
	* 异步发送接收100000条数据
	* worker数量： 1
	* vm cpu cores: 2
	* no log, no printf
	* socket 连接方式： ipc

* 测试程序： tripping
* 测试代码： 


* 测试结果：
```
[localhost@bin(PUNK-BLJ)] ./tripping
Setting up test...
Synchronous round-trip test...		10000
 7686 calls/second

Asynchronous round-trip test...		100000
 348432 calls/second
```


## 2. 海盗模式(PPP)
* 测试条件：
	* 客户端采用 ZMQ_REQ 模式
	* 服务端采用 ZMQ_ROUTER 模式
	* 工人worker采用： ZMQ_DEALER 模式
	* 同步发送接收100000条数据
	* 异步发送接收100000条数据
	* worker数量： 1
	* vm cpu cores: 2
	* no log, no printf
	* socket 连接方式： ipc
	* 心跳间隔10秒： HEARTBEAT_INTERVAL  10000    //  msecs
	* 发送失败 retry 3 次， 每次间隔2.5秒

* 测试程序： hwclient, lpclient, ppqueue, ppworker
* 测试代码： 


* 测试结果： 
	* sync (同步): lpclient
```
1. 
[localhost@test_PPP] ./lpclient
I: connecting to server...
100000 replies received
lpclient use: 31413 msecs
lpclient calls 3183 data per second

2.
[localhost@test_PPP] ./lpclient
I: connecting to server...
100000 replies received
lpclient use: 21656 msecs
lpclient calls 4617 data per second

3.
[localhost@test_PPP] ./lpclient
I: connecting to server...
100000 replies received
lpclient use: 27534 msecs
lpclient calls 3631 data per second

```

	* async (异步): hwclient

```
5. 
100000 replies received
hwclient use: 1185 msecs
hwclient calls 84388 data per second

6.
100000 replies received
hwclient use: 1060 msecs
hwclient calls 94339 data per second

7.
100000 replies received
hwclient use: 1140 msecs
hwclient calls 87719 data per second

8.
100000 replies received
hwclient use: 1086 msecs
hwclient calls 92081 data per second

```



## 3. 管家模式(MDP)
* 测试条件：
	* 客户端采用 ZMQ_REQ 模式
	* 服务端采用 ZMQ_ROUTER 模式
	* worker采用 ZMQ_DEALER 模式
	* 同步发送接收100000条数据
	* 异步发送接收100000条数据
	* 发送模拟数据
	* worker数量： 1
	* vm cpu cores: 2
	* no log, no printf, no verbose
	* socket 连接方式： ipc://mdp.ipc
	* 心跳间隔10秒： HEARTBEAT_INTERVAL  10000    //  msecs

* 测试程序： mdclient, mdclient2, mdbroker, mdworker
* 测试代码： 


* 测试结果： 
	* sync (同步): mdclient
```
1.
[localhost@test_MDP] ./mdclient
100000 requests/replies processed
mdclient use: 29407 msecs
mdclient calls 3400 data per second

2.
[localhost@test_MDP] ./mdclient
100000 requests/replies processed
mdclient use: 24099 msecs
mdclient calls 4149 data per second

3.
[localhost@test_MDP] ./mdclient
100000 requests/replies processed
mdclient use: 29843 msecs
mdclient calls 3350 data per second

```


	* async (异步): mdclient2
```
4.
[localhost@test_MDP] ./mdclient2
100000 replies received
mdclient2 use: 18979 msecs
mdclient2 calls 5268 data per second

5.
[localhost@test_MDP] ./mdclient2
100000 replies received
mdclient2 use: 8809 msecs
mdclient2 calls 11352 data per second

6.
[localhost@test_MDP] ./mdclient2
100000 replies received
mdclient2 use: 14368 msecs
mdclient2 calls 6959 data per second

7.
[localhost@test_MDP] ./mdclient2
100000 replies received
mdclient2 use: 15233 msecs
mdclient2 calls 6564 data per second

8.
[localhost@test_MDP] ./mdclient2
100000 replies received
mdclient2 use: 10216 msecs
mdclient2 calls 9788 data per second

9.
[localhost@test_MDP] ./mdclient2
100000 replies received
mdclient2 use: 21721 msecs
mdclient2 calls 4603 data per second

10.
[localhost@test_MDP] ./mdclient2
100000 replies received
mdclient2 use: 10480 msecs
mdclient2 calls 9541 data per second

11. 发送"Hello world"；
[localhost@test_MDP] ./mdclient2
100000 replies received
mdclient2 use: 13820 msecs
mdclient2 calls 7235 data per second

12. 发送"Hello world"；
[localhost@test_MDP] ./mdclient2
100000 replies received
mdclient2 use: 16081 msecs
mdclient2 calls 6218 data per second


```


## 4. 泰塔尼克模式(TSP)
* 测试条件：
	* 客户端采用 ZMQ_REQ 模式
	* 服务端采用 ZMQ_ROUTER 模式
	* worker采用 ZMQ_DEALER 模式
	* 同步发送接收1000条数据
	* 异步发送接收10000条数据
	* 发送模拟数据
	* worker数量： 1
	* vm cpu cores: 2
	* no log, no printf, no verbose
	* socket 连接方式： ipc://punk_broker.ipc
	* 心跳间隔10秒： HEARTBEAT_INTERVAL 10000  //  msecs
	* 接收延迟 20 msec： zclock_sleep(20);

* 测试程序： dnv, reader, reader_async, punk
		(ticlient, titanic, mdbroker, mdworker)
* 测试代码： 


* 测试结果： 

	* sync (同步): reader(mdclient)
```
1. 
[localhost@bin(PUNK-BLJ)] ./reader
reader Synchronous round-trip 1000 items test...
1000 replies received
reader req 1000 items need: 30063 msecs
reader req: 33 calls/second


2.
reader Synchronous round-trip 1000 items test...
1000 replies received
reader req 1000 items need: 29747 msecs
reader req: 33 calls/second

```

	* async (异步): zclock_sleep(2);
```
3.
[localhost@bin(PUNK-BLJ)] ./reader_async
reader Asynchronous round-trip test...
reader requests use time: 29868 msecs
reader send data num: 10000
reader use: 72974 msecs
reader calls 137 data per second

4.
[localhost@bin(PUNK-BLJ)] ./reader_async
reader Asynchronous round-trip test...
reader requests use time: 24502 msecs
reader send data num: 10000
reader use: 63602 msecs
reader calls 157 data per second

5.
[localhost@bin(PUNK-BLJ)] ./reader_async
reader Asynchronous round-trip test...
reader requests use time: 29635 msecs
reader send data num: 10000
reader use: 72699 msecs
reader calls 137 data per second

6. 
[localhost@bin(PUNK-BLJ)] ./reader_async
reader Asynchronous round-trip test...
reader requests use time: 22694 msecs
reader send data num: 10000
reader use: 65402 msecs
reader calls 152 data per second

```