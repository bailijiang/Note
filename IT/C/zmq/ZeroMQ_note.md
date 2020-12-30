## Chaptor 03

* ZeroMQ请求-应答封包： 
  * 同步/异步模式下都会在REQ封包时添加返回地址(id);
  * 分隔符帧采用空帧 empty；
  * 