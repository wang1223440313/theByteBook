# 4.3 四层负载均衡

所谓的“四层负载均衡”（Layer 4 Load Balancer，简称 L4），非仅指在 OSI 模型的第四层（也就是传输层）工作。而是指这类负载均衡器的工作模式的共同特点是，维持一条传输层连接（如 TCP、UDP）的特性。

OSI 模型的下三层（物理层、链路层和网络层）称为媒体层（Media Layer），处理的是数据传输的实际介质和路由，上四层（传输层、会话层、表示层和应用层）称为主机层（Host Layer），处理的是主机间的通信。四层负载均衡器的工作模式，主要工作在第二层（数据链路层，改写 MAC 地址）、第三层（网络层，改写 IP 地址）、第四层（传输层，改写 UDP 或 TCP 的连接端口和连接地址，做一些 NAT 之类的操作）。

既然请求已经到了主机层，只能算作请求转发。但出于习惯，所有的资料都把它们称为四层负载均衡。笔者继续沿用这种惯例，来自客户端的请求，无论是在网络层处理，还是在传输层的处理，统称为四层负载均衡处理。