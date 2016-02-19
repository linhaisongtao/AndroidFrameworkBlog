#Binder实现的完整的进程间IPC过程
1. Binder成为linux的设备驱动
2. ServiceManager成为服务管理者，Binder描述符为0
3. 服务端进程通过特定的Binder描述符0和binder驱动向ServiceManager进程注册服务
4. 客户端进程通过特定的Binder描述符0和binder驱动向ServiceManager进程获取服务进程的Binder描述符
5. 客户端进程通过拿到的服务端进程的Binder描述符和binder驱动，向服务端进行发起请求
6. 服务端进程接收到消息后，进行相应的处理。



```sequence
Title:binder进程通信
client->binder:目标服务的binder标示符以及数据
binder->binder:根据binder标示符找到对应的binder_node
binder->binder:将数据拷贝到内核地址空间中，\n同时将该实际的物理地址映射到目标进程的地址空间中
binder->binder:封装成一个binder_work，将其放入到todo队列中，\n同时唤醒目标进程
binder->server:唤醒
server->server:取出数据，进行相应的操作
```
