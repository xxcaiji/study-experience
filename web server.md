#web server开发学习

1.socket套接字符

```cpp
int sockfd = socket(AF_INET, SOCK_STREAM, 0);
```

- 第一个参数：IP地址类型，AF_INET表示使用IPv4，如果使用IPv6请使用AF_INET6。
- 第二个参数：数据传输方式，SOCK_STREAM表示流格式、面向连接，多用于TCP。SOCK_DGRAM表示数据报格式、无连接，多用于UDP。
- 第三个参数：协议，0表示根据前面的两个参数自动推导协议类型。设置为IPPROTO_TCP和IPPTOTO_UDP，分别表示TCP和UDP。


2.设置sockaddr_in结构体设置地址族、IP地址和端口：

```cpp
serv_addr.sin_family = AF_INET;

serv_addr.sin_addr.s_addr = inet_addr("127.0.0.1");

serv_addr.sin_port = htons(8888);
```
3.listen监听socket端口

4.accept函数进行接受连接

```cpp
int clnt_sockfd = accept(sockfd, (sockaddr*)&clnt_addr, &clnt_addr_len);
```
同时还得保存连接客户的套接字信息。

5.connect函数进行连接服务器

6.read和write函数，属于TCP连接的，UDP使用sendto 和recvfrom

```cpp
ssize_t read_bytes = read(clnt_sockfd, buf, sizeof(buf)); //从客户端socket读到缓冲区，返回已读数据大小
```

7.使用close函数将每一个使用过后的文件描述符关闭。