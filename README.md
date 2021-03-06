# NatSocks
lcx with socks server

## 用法
基本使用方法跟lcx无差别,增加了socks服务,除去了上传多文件的繁琐。  
### 监听   
-listen port1 port2  
#### 说明
同时监听port1端口和port2端口，当两个客户端主动连接上这两个监听端口之后，NatSocks负责这两个端口间的数据转发。  
#### 示例  
NatSocks -listen 1997 2017  
### 转发 
-tran port1 ip:port2  
#### 说明
本地开始监听port1端口，当port1端口上接收到来自客户端的主动连接之后，NatSocks将主动连接ip:port2，并且负责port1端口和ip:port2之间的数据转发。
#### 示例 
NatSocks -tran 1997 192.168.1.2:338  
### 主动连接
-slave ip1:port1 ip2:port2  
#### 说明
本地开始主动连接ip1:port1主机和ip2:port2主机，当连接成功之后，NatSocks负责这两个主机之间的数据转发。
#### 示例 
NatSocks -slave 127.0.0.1:3389 8.8.8.8:1997
### socks5 server
-socks ip:port
#### 说明
本地建立一个socks5服务，只需要提供本地ip:port
#### 示例 
NatSocks -socks 127.0.0.1:1080
### socks5 server & port forward  
-resocks ip1:port1 ip2:port2
#### 说明
本地建立一个socks5服务并且主动转发到公网服务器端口上
ps:由于go使用goroutine时,主进程必须堵塞,所以我直接使用了Scanln卡死进程,所以使用resocks参数时,进程启动后不要输入回车,不然会自动退出。  
#### 示例 
NatSocks -resocks 127.0.0.1:1080 8.8.8.8:1997  

## 组合
### 端口映射
将内网机器(127.0.0.1)的3389端口转发到公网机器(8.8.8.8)的2017端口:  
公网机器(8.8.8.8)：NatSocks -listen 1997 2017  
内网机器(127.0.0.1)：NatSocks -slave 127.0.0.1:3389 8.8.8.8:1997  

### socks5映射
在内网机器(127.0.0.1)的1080端口建立socks服务并转发到公网机器(8.8.8.8)的2017端口:    
公网机器(8.8.8.8)：NatSocks -listen 1997 2017  
内网机器(127.0.0.1)：NatSocks -resocks 127.0.0.1:1080 8.8.8.8:1997  

## 安装or下载  
### 自行安装  
安装go环境以及依赖库后  
go build NatSocks.go  
  
### windows,linux,osx (x64/x84) releases  
https://github.com/Lz1y/NatSocks/releases/tag/v1.0  

## 感谢
https://github.com/cw1997/NATBypass  
https://github.com/txthinking/socks5
