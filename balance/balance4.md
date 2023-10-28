# 4.3 四层负载均衡概论

现在所说的“四层负载均衡”其实是多种均衡器工作模式的统称，“四层”的意思是这些工作的模式共同特点是维持同一个 TCP 连接，而不是说它只工作在 OSI 模型的四层。事实上，这些模式主要工作在第二层（数据链路层，改写 MAC 地址）和第三层（网络层，改写 IP 地址），而第四层（传输层，改写 UDP、TCP 等协议的内容端口号）单纯只处理数据，做 NAT 之类的功能，所以无法做到负载均衡的转发。

因为 OSI 模型的下三层是媒体层(Media Layer)，上四层是主机层（Host Layer），既然流量已经到了目标主机了，也谈不上什么流量转发，最多只能做代理，但出于习惯，现在所有的资料都把它们称为四层负载均衡，笔者也继续沿用这种惯例，不论是 IP 层处理还是链路层处理，统称四层负载均衡。


四层负载均衡器最典型的软件实现是 LVS（Linux Virtual Server，Linux 虚拟服务器），但笔者并不过多介绍 LVS 的相关特点和如何使用，而是从网络数据包的处理角度去解释 LVS 中 DR、NAT、Tunnel 模式是如何工作的。


介绍负载均衡前，我们先来了解部分术语。

Server 相关

- DS：Director Server 指的是前端负载均衡器节点，又称 Dispatcher、Balancer，主要接收用户请求
- RS：Real Server 后端真实的工作服务器

首先要解释的是 LVS 相关的几种 IP

- VIP：virtual IP，LVS 服务器上接收外网数据包的网卡 IP 地址，是用于接收用户请求的 IP。
- DIP：director IP，LVS 服务器上转发数据包到 realserver 的网卡 IP 地址，是用于连接内网的 IP。
- RIP：realserver(常简称为 RS)上接收 Director 转发数据包的 IP，即提供服务的服务器 IP。
- CIP：客户端的 IP