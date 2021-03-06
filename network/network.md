<table>
    <tr>
        <td>OSI 七层网络模型</td>	 
        <td>TCP/IP 四层概念模型	</td> 
        <td>对应的网络协议</td>
    </tr>
    <tr>
        <td>应用层（Application</td>
        <td rowspan = "3">应用层</td>
        <td>HTTP, TFTP, FTP, NFS, WAIS, SMTP, Telnet, DNS, SNMP  </td> 
    </tr>
    <tr>
        <td>表示层（Presentation）</td>
        <td>TIFF, GIF, JPEG, PICT</td>       
    </tr>
    <tr>
        <td>会话层（Session）</td>
        <td>RPC, SQL, NFS, NetBIOS, names, AppleTalk</td>
    </tr>
    <tr>
        <td>传输层</td>
        <td>传输层</td>
        <td>TCP, UDP</td>
    </tr>
    <tr>
        <td>网络层（Network）</td>
        <td>网际层</td>
        <td>IP, ICMP, ARP, RARP, RIP, IPX</td>
    </tr>
    <tr>
        <td>数据链路层（Data Link）</td>
        <td rowspan = "2">数据链路层</td>
        <td>FDDI, Frame Relay, HDLC, SLIP, PPP</td>
    </tr>
    <tr>
        <td>物理层（Physical）</td>
        <td>EIA/TIA-232, EIA/TIA-499, V.35, 802.3</td>	
    </tr>     
</table>
<br><br>

## 数据如何在各层之间传输【数据的封装过程】
>在发送主机端，一个应用层报文被传送到运输层。在最简单的情况下，运输层收取到报文并附上附加信息，该首部将被接收端的运输层使用。应用层报文和运输层首部信息一道构成了运输层报文段。附加的信息可能包括：允许接收端运输层向上向适当的应用程序交付报文的信息以及差错检测位信息。该信息让接收端能够判断报文中的比特是否在途中已被改变。运输层则向网络层传递该报文段，网络层增加了如源和目的端系统地址等网络层首部信息，生成了网络层数据报。该数据报接下来被传递给链路层，在数据链路层数据包添加发送端 MAC 地址和接收端 MAC 地址后被封装成数据帧，在物理层数据帧被封装成比特流，之后通过传输介质传送到对端。

<br><br>

# 应用层

## HTTP

### GET 和 POST 的区别
<ul>
<li>get 提交的数据会放在 URL 之后，并且请求参数会被完整的保留在浏览器的记录里，由于参数直接暴露在 URL 中，可能会存在安全问题，因此往往用于获取资源信息。而 post 参数放在请求主体中，并且参数不会被保留，相比 get 方法，post 方法更安全，主要用于修改服务器上的资源。
<li>get 请求只支持 URL 编码，post 请求支持多种编码格式。
<li>get 只支持 ASCII 字符格式的参数，而 post 方法没有限制。
<li>get 提交的数据大小有限制（这里所说的限制是针对浏览器而言的），而 post 方法提交的数据没限制
<li>get 方式需要使用 Request.QueryString 来取得变量的值，而 post 方式通过 Request.Form 来获取。
<li>get 方法产生一个 TCP 数据包，post 方法产生两个（并不是所有的浏览器中都产生两个）。
</ul>

### HTTP 与 HTTPs 的工作方式【建立连接的过程】

#### HTTP

HTTP（Hyper Text Transfer Protocol: 超文本传输协议） 是一种简单的请求 - 响应协议，被用于在 Web 浏览器和网站服务器之间传递消息。HTTP 使用 TCP（而不是 UDP）作为它的支撑运输层协议。其默认工作在 TCP 协议 80 端口，HTTP 客户机发起一个与服务器的 TCP 连接，一旦连接建立，浏览器和服务器进程就可以通过套接字接口访问 TCP。客户机从套接字接口发送 HTTP 请求报文和接收 HTTP 响应报文。类似地，服务器也是从套接字接口接收 HTTP 请求报文和发送 HTTP 响应报文。其通信内容以明文的方式发送，不通过任何方式的数据加密。当通信结束时，客户端与服务器关闭连接。

#### HTTPS

HTTPS（Hyper Text Transfer Protocol over Secure Socket Layer）是以安全为目标的 HTTP 协议，在 HTTP 的基础上通过传输加密和身份认证的方式保证了传输过程的安全性。其工作流程如下：
<ol>
<li> 客户端发起一个 HTTPS 请求，并连接到服务器的 443 端口，发送的信息主要包括自身所支持的算法列表和密钥长度等；

<li> 服务端将自身所支持的所有加密算法与客户端的算法列表进行对比并选择一种支持的加密算法，然后将它和其它密钥组件一同发送给客户端。

<li> 服务器向客户端发送一个包含数字证书的报文，该数字证书中包含证书的颁发机构、过期时间、服务端的公钥等信息。

<li> 最后服务端发送一个完成报文通知客户端 SSL 的第一阶段已经协商完成。

<li> SSL 第一次协商完成后，客户端发送一个回应报文，报文中包含一个客户端生成的随机密码串，称为 pre_master_secret，并且该报文是经过证书中的公钥加密过的。

<li> 紧接着客户端会发送一个报文提示服务端在此之后的报文是采用pre_master_secret 加密的。

<li> 客户端向服务端发送一个 finish 报文，这次握手中包含第一次握手至今所有报文的整体校验值，最终协商是否完成取决于服务端能否成功解密。

<li>服务端同样发送与第 6 步中相同作用的报文，已让客户端进行确认，最后发送 finish 报文告诉客户端自己能够正确解密报文。
</ol>
当服务端和客户端的 finish 报文交换完成之后，SSL 连接就算建立完成了，之后就进行和 HTTP 相同的通信过程，唯一不同的是在 HTTP 通信过程中并不是采用明文传输，而是采用对称加密的方式，其中对称密钥已经在 SSL 的建立过程中协商好了。

<table>
    <tr>
        <td>HTTP</td>
        <td>HTTPS</td>
    </tr>
    <tr>
        <td>明文发送</td>
        <td>加密发送</td>
    </tr>
    <tr>
        <td>80端口</td>
        <td>443端口</td>
    </tr>
    <tr>
        <td>TCP三次握手</td>
        <td>TCP三次握手、SSL协商</td>
    </tr>
</table>

### HTTP如何保存用户状态
<ol>
<li>基于Session实现的会话保持
    <ul>
    在客户端第一次向服务器发送 HTTP 请求后，服务器会创建一个 Session 对象并将客户端的身份信息以键值对的形式存储下来，然后分配一个会话标识（SessionId）给客户端，这个会话标识一般保存在客户端 Cookie 中，之后每次该浏览器发送 HTTP 请求都会带上 Cookie 中的 SessionId 到服务器，服务器根据会话标识就可以将之前的状态信息与会话联系起来，从而实现会话保持。
<ul>
<li>优点：安全性高，因为状态信息保存在服务器端。
<li>缺点：由于大型网站往往采用的是分布式服务器，浏览器发送的 HTTP 请求一般要先通过负载均衡器才能到达具体的后台服务器，倘若同一个浏览器两次 HTTP 请求分别落在不同的服务器上时，基于 Session 的方法就不能实现会话保持了。
</ul>
解决方法：采用中间件，例如 Redis，我们通过将 Session 的信息存储在 Redis 中，使得每个服务器都可以访问到之前的状态信息
</ul>
<li> 基于 Cookie 实现的会话保持
<ul>
当服务器发送响应消息时，在 HTTP 响应头中设置 Set-Cookie 字段，用来存储客户端的状态信息。客户端解析出 HTTP 响应头中的字段信息，并根据其生命周期创建不同的 Cookie，这样一来每次浏览器发送 HTTP 请求的时候都会带上 Cookie 字段，从而实现状态保持。基于 Cookie 的会话保持与基于 Session 实现的会话保持最主要的区别是前者完全将会话状态信息存储在浏览器 Cookie 中。
<ul>
<li>优点：服务器不用保存状态信息， 减轻服务器存储压力，同时便于服务端做水平拓展。

<li>缺点：该方式不够安全，因为状态信息存储在客户端，这意味着不能在会话中保存机密数据。除此之外，浏览器每次发起 HTTP 请求时都需要发送额外的 Cookie 到服务器端，会占用更多带宽。
</ul>
拓展：Cookie被禁用了怎么办？
若遇到 Cookie 被禁用的情况，则可以通过重写 URL 的方式将会话标识放在 URL 的参数里，也可以实现会话保持。
</ul>
</ol>

### 状态码
|分类	|分类描述|
|---|---|
|1XX	|指示信息--表示请求正在处理|
|2XX	|成功--表示请求已被成功处理完毕|
|3XX	|重定向--要完成的请求需要进行附加操作|
|4XX	|客户端错误--请求有语法错误或者请求无法实现，服务器无法处理请求|
|5XX	|服务器端错误--服务器处理请求出现错误|

### socket() 套接字有哪些
>是对网络中不同主机上的应用进程之间进行双向通信的端点的抽象，网络进程通信的一端就是一个套接字，不同主机上的进程便是通过套接字发送报文来进行通信。例如 TCP 用主机的 IP 地址 + 端口号作为 TCP 连接的端点，这个端点就叫做套接字。

套接字主要有以下三种类型：
<ul>
<li>流套接字（SOCK_STREAM）：流套接字基于 TCP 传输协议，主要用于提供面向连接、可靠的数据传输服务。由于 TCP 协议的特点，使用流套接字进行通信时能够保证数据无差错、无重复传送，并按顺序接收，通信双方不需要在程序中进行相应的处理。
<li>数据报套接字（SOCK_DGRAM）：和流套接字不同，数据报套接字基于 UDP 传输协议，对应于无连接的 UDP 服务应用。该服务并不能保证数据传输的可靠性，也无法保证对端能够顺序接收到数据。此外，通信两端不需建立长时间的连接关系，当 UDP 客户端发送一个数据给服务器后，其可以通过同一个套接字给另一个服务器发送数据。当用 UDP 套接字时，丢包等问题需要在程序中进行处理。
<li>原始套接字（SOCK_RAW）：由于流套接字和数据报套接字只能读取 TCP 和 UDP 协议的数据，当需要传送非传输层数据包（例如 Ping 命令时用的 ICMP 协议数据包）或者遇到操作系统无法处理的数据包时，此时就需要建立原始套接字来发送。
</ul>

### 网页解析全过程【用户输入网址到显示对应页面的全过程】
           
<div align = center><img src = "./../pics/urlflow.png" width = "150"></div>
<ol>
<li>DNS 解析：当用户输入一个网址并按下回车键的时候，浏览器获得一个域名，而在实际通信过程中，我们需要的是一个 IP 地址，因此我们需要先把域名转换成相应 IP 地址。【具体细节参看问题 16，17】

<li>TCP 连接：浏览器通过 DNS 获取到 Web 服务器真正的 IP 地址后，便向 Web 服务器发起 TCP 连接请求，通过 TCP 三次握手建立好连接后，浏览器便可以将 HTTP 请求数据发送给服务器了。【三次握手放在传输层详细讲解】

<li> 发送 HTTP 请求：浏览器向 Web 服务器发起一个 HTTP 请求，HTTP 协议是建立在 TCP 协议之上的应用层协议，其本质是在建立起的TCP连接中，按照HTTP协议标准发送一个索要网页的请求。在这一过程中，会涉及到负载均衡等操作。

<ul>
拓展：什么是负载均衡？

负载均衡，英文名为 Load Balance，其含义是指将负载（工作任务）进行平衡、分摊到多个操作单元上进行运行，例如 FTP 服务器、Web 服务器、企业核心服务器和其他主要任务服务器等，从而协同完成工作任务。负载均衡建立在现有的网络之上，它提供了一种透明且廉价有效的方法扩展服务器和网络设备的带宽、增加吞吐量、加强网络处理能力并提高网络的灵活性和可用性。

负载均衡是分布式系统架构设计中必须考虑的因素之一，例如天猫、京东等大型用户网站中为了处理海量用户发起的请求，其往往采用分布式服务器，并通过引入反向代理等方式将用户请求均匀分发到每个服务器上，而这一过程所实现的就是负载均衡。
</ul>
<li> 处理请求并返回：服务器获取到客户端的 HTTP 请求后，会根据 HTTP 请求中的内容来决定如何获取相应的文件，并将文件发送给浏览器。

<li>浏览器渲染：浏览器根据响应开始显示页面，首先解析 HTML 文件构建 DOM 树，然后解析 CSS 文件构建渲染树，等到渲染树构建完成后，浏览器开始布局渲染树并将其绘制到屏幕上。

<li> 断开连接：客户端和服务器通过四次挥手终止 TCP 连接。【其中的细节放在传输层详细讲解】
</ol>

# 传输层
## 三次握手和四次挥手机制

### 三次握手

<div align = "center"><img src = "./../pics/TCP_shake.png" width = "400"></div>

三次握手是 TCP 连接的建立过程。在握手之前，主动打开连接的客户端结束 CLOSE 阶段，被动打开的服务器也结束 CLOSE 阶段，并进入 LISTEN 阶段。随后进入三次握手阶段：
<ol>
<li>首先客户端向服务器发送一个 SYN 包，并等待服务器确认，其中：
<ul>
<li>标志位为 SYN，表示请求建立连接；
<li>序号为 Seq = x（x 一般为 1）；
<li>随后客户端进入 SYN-SENT 阶段。
</ul>
<li> 服务器接收到客户端发来的 SYN 包后，对该包进行确认后结束 LISTEN 阶段，并返回一段 TCP 报文，其中：
<ul>
<li>标志位为 SYN 和 ACK，表示确认客户端的报文 Seq 序号有效，服务器能正常接收客户端发送的数据，并同意创建新连接；
<li>序号为 Seq = y；
<li>确认号为 Ack = x + 1，表示收到客户端的序号 Seq 并将其值加 1 作为自己确认号 Ack 的值，随后服务器端进入 SYN-RECV 阶段。
</ul>
<li> 客户端接收到发送的 SYN + ACK 包后，明确了从客户端到服务器的数据传输是正常的，从而结束 SYN-SENT 阶段。并返回最后一段报文。其中：
<ul>
<li>标志位为 ACK，表示确认收到服务器端同意连接的信号；
<li>序号为 Seq = x + 1，表示收到服务器端的确认号 Ack，并将其值作为自己的序号值；
<li>确认号为 Ack= y + 1，表示收到服务器端序号 seq，并将其值加 1 作为自己的确认号 Ack 的值。
<li>随后客户端进入 ESTABLISHED。
</ul>
当服务器端收到来自客户端确认收到服务器数据的报文后，得知从服务器到客户端的数据传输是正常的，从而结束 SYN-RECV 阶段，进入 ESTABLISHED 阶段，从而完成三次握手。
</ol>

### 四次挥手：
<div align = "center"><img src = "./../pics/TCP_bye.png" width = "400"></div>

四次挥手即 TCP 连接的释放，这里假设客户端主动释放连接。在挥手之前主动释放连接的客户端结束 ESTABLISHED 阶段，随后开始四次挥手：
<ol>
<li>首先客户端向服务器发送一段 TCP 报文表明其想要释放 TCP 连接，其中：
<ul>
<li>标记位为 FIN，表示请求释放连接；
<li>序号为 Seq = u；
<li>随后客户端进入 FIN-WAIT-1 阶段，即半关闭阶段，并且停止向服务端发送通信数据。
</ul>
<li> 服务器接收到客户端请求断开连接的 FIN 报文后，结束 ESTABLISHED 阶段，进入 CLOSE-WAIT 阶段并返回一段 TCP 报文，其中：
<ul>
<li>标记位为 ACK，表示接收到客户端释放连接的请求；
<li>序号为 Seq = v；
<li>确认号为 Ack = u + 1，表示是在收到客户端报文的基础上，将其序号值加 1 作为本段报文确认号 Ack 的值；
<li>随后服务器开始准备释放服务器端到客户端方向上的连接。
</ul>

客户端收到服务器发送过来的 TCP 报文后，确认服务器已经收到了客户端连接释放的请求，随后客户端结束 FIN-WAIT-1 阶段，进入 FIN-WAIT-2 阶段。

<li>服务器端在发出 ACK 确认报文后，服务器端会将遗留的待传数据传送给客户端，待传输完成后即经过 CLOSE-WAIT 阶段，便做好了释放服务器端到客户端的连接准备，再次向客户端发出一段 TCP 报文，其中：
<ul>
<li>标记位为 FIN 和 ACK，表示已经准备好释放连接了；
<li>序号为 Seq = w；
<li>确认号 Ack = u + 1，表示是在收到客户端报文的基础上，将其序号 Seq 的值加 1 作为本段报文确认号 Ack 的值。
<li>随后服务器端结束 CLOSE-WAIT 阶段，进入 LAST-ACK 阶段。并且停止向客户端发送数据。
</ul>
<li>客户端收到从服务器发来的 TCP 报文，确认了服务器已经做好释放连接的准备，于是结束 FIN-WAIT-2 阶段，进入 TIME-WAIT 阶段，并向服务器发送一段报文，其中：
<ul>
<li>标记位为 ACK，表示接收到服务器准备好释放连接的信号；
<li>序号为 Seq= u + 1，表示是在已收到服务器报文的基础上，将其确认号 Ack 值作为本段序号的值；
<li>确认号为 Ack= w + 1，表示是在收到了服务器报文的基础上，将其序号 Seq 的值作为本段报文确认号的值。
</ul>
随后客户端开始在 TIME-WAIT 阶段等待 2 MSL。服务器端收到从客户端发出的 TCP 报文之后结束 LAST-ACK 阶段，进入 CLOSED 阶段。由此正式确认关闭服务器端到客户端方向上的连接。客户端等待完 2 MSL 之后，结束 TIME-WAIT 阶段，进入 CLOSED 阶段，由此完成「四次挥手」。
</ol>

### 如果三次握手的时候每次握手信息对方都没有收到会怎么样？
<ul>
<li>若第一次握手服务器未接收到客户端请求建立连接的数据包时，服务器不会进行任何相应的动作，而客户端由于在一段时间内没有收到服务器发来的确认报文， 因此会等待一段时间后重新发送 SYN 同步报文，若仍然没有回应，则重复上述过程直到发送次数超过最大重传次数限制后，建立连接的系统调用会返回 -1。
<li>若第二次握手客户端未接收到服务器回应的 ACK 报文时，客户端会采取第一次握手失败时的动作，这里不再重复，而服务器端此时将阻塞在 accept() 系统调用处等待 client 再次发送 ACK 报文。
<li>若第三次握手服务器未接收到客户端发送过来的 ACK 报文，同样会采取类似于客户端的超时重传机制，若重传次数超过限制后仍然没有回应，则 accep() 系统调用返回 -1，服务器端连接建立失败。但此时客户端认为自己已经连接成功了，因此开始向服务器端发送数据，但是服务器端的 accept() 系统调用已返回，此时没有在监听状态。因此服务器端接收到来自客户端发送来的数据时会发送 RST 报文给 客户端，消除客户端单方面建立连接的状态。
</ul>

### 为什么要进行三次握手？两次握手可以吗？
>三次握手的主要目的是确认自己和对方的发送和接收都是正常的，从而保证了双方能够进行可靠通信。若采用两次握手，当第二次握手后就建立连接的话，此时客户端知道服务器能够正常接收到自己发送的数据，而服务器并不知道客户端是否能够收到自己发送的数据。<br><br>
我们知道网络往往是非理想状态的（存在丢包和延迟），当客户端发起创建连接的请求时，如果服务器直接创建了这个连接并返回包含 SYN、ACK 和 Seq 等内容的数据包给客户端，这个数据包因为网络传输的原因丢失了，丢失之后客户端就一直接收不到返回的数据包。由于客户端可能设置了一个超时时间，一段时间后就关闭了连接建立的请求，再重新发起新的请求，而服务器端是不知道的，如果没有第三次握手告诉服务器客户端能否收到服务器传输的数据的话，服务器端的端口就会一直开着，等到客户端因超时重新发出请求时，服务器就会重新开启一个端口连接。长此以往， 这样的端口越来越多，就会造成服务器开销的浪费。

### 为什么要四次挥手？
>释放 TCP 连接时之所以需要四次挥手，是因为 FIN 释放连接报文和 ACK 确认接收报文是分别在两次握手中传输的。 当主动方在数据传送结束后发出连接释放的通知，由于被动方可能还有必要的数据要处理，所以会先返回 ACK 确认收到报文。当被动方也没有数据再发送的时候，则发出连接释放通知，对方确认后才完全关闭TCP连接。

### CLOSE-WAIT 和 TIME-WAIT 的状态和意义
<ul>
<li>在服务器收到客户端关闭连接的请求并告诉客户端自己已经成功收到了该请求之后，服务器进入了 CLOSE-WAIT 状态，然而此时有可能服务端还有一些数据没有传输完成，因此不能立即关闭连接，而 CLOSE-WAIT 状态就是为了保证服务器在关闭连接之前将待发送的数据发送完成。<br><br>

<li>TIME-WAIT 发生在第四次挥手，当客户端向服务端发送 ACK 确认报文后进入该状态，若取消该状态，即客户端在收到服务端的 FIN 报文后立即关闭连接，此时服务端相应的端口并没有关闭，若客户端在相同的端口立即建立新的连接，则有可能接收到上一次连接中残留的数据包，可能会导致不可预料的异常出现。除此之外，假设客户端最后一次发送的 ACK 包在传输的时候丢失了，由于 TCP 协议的超时重传机制，服务端将重发 FIN 报文，若客户端并没有维持 TIME-WAIT 状态而直接关闭的话，当收到服务端重新发送的 FIN 包时，客户端就会用 RST 包来响应服务端，这将会使得对方认为是有错误发生，然而其实只是正常的关闭连接过程，并没有出现异常情况。<br>
</ul>

### TIME_WAIT 状态会导致什么问题，怎么解决
我们考虑高并发短连接的业务场景，在高并发短连接的 TCP 服务器上，当服务器处理完请求后主动请求关闭连接，这样服务器上会有大量的连接处于 TIME_WAIT 状态，服务器维护每一个连接需要一个 socket，也就是每个连接会占用一个文件描述符，而文件描述符的使用是有上限的，如果持续高并发，会导致一些正常的 连接失败。

解决方案：修改配置或设置 SO_REUSEADDR 套接字，使得服务器处于 TIME-WAIT 状态下的端口能够快速回收和重用。

### TIME-WAIT 为什么是 2MSL

当客户端发出最后的 ACK 确认报文时，并不能确定服务器端能够收到该段报文。所以客户端在发送完 ACK 确认报文之后，会设置一个时长为 2 MSL 的计时器。MSL（Maximum Segment Lifetime），指一段 TCP 报文在传输过程中的最大生命周期。2 MSL 即是服务器端发出 FIN 报文和客户端发出的 ACK 确认报文所能保持有效的最大时长。

若服务器在 1 MSL 内没有收到客户端发出的 ACK 确认报文，再次向客户端发出 FIN 报文。如果客户端在 2 MSL 内收到了服务器再次发来的 FIN 报文，说明服务器由于一些原因并没有收到客户端发出的 ACK 确认报文。客户端将再次向服务器发出 ACK 确认报文，并重新开始 2 MSL 的计时。

若客户端在 2MSL 内没有再次收到服务器发送的 FIN 报文，则说明服务器正常接收到客户端 ACK 确认报文，客户端可以进入 CLOSE 阶段，即完成四次挥手。

所以客户端要经历 2 MSL 时长的 TIME-WAIT 阶段，为的是确认服务器能否接收到客户端发出的 ACK 确认报文。

### TCP和UDP的区别
|类型|是否面向连接|传输可靠性|传输形式|传输效率	|所需资源|应用场景|首部字节
|---|---|---|---|---|---|---|---|
|TCP	|是	|可靠	|字节流	|慢	|多	|文件传输、邮件传输	|20~60
|UDP	|否	|不可靠	|数据报文段	|快	|少	|即时通讯、域名转换	|8个字节

### TCP 协议中的定时器

TCP中有七种计时器，分别为：
<ul>
<li>建立连接定时器：顾名思义，该定时器是在建立 TCP 连接的时候使用的，在 TCP 三次握手的过程中，发送方发送 SYN 时，会启动一个定时器（默认为 3 秒），若 SYN 包丢失了，那么 3 秒以后会重新发送 SYN 包，直到达到重传次数。

<li>重传定时器：该计时器主要用于 TCP 超时重传机制中，当TCP 发送报文段时，就会创建特定报文的重传计时器，并可能出现两种情况：

① 若在计时器截止之前发送方收到了接收方的 ACK 报文，则撤销该计时器；

② 若计时器截止时间内并没有收到接收方的 ACK 报文，则发送方重传报文，并将计时器复位。

<li>坚持计时器：我们知道 TCP 通过让接受方指明希望从发送方接收的数据字节数（窗口大小）来进行流量控制，当接收端的接收窗口满时，接收端会告诉发送端此时窗口已满，请停止发送数据。此时发送端和接收端的窗口大小均为0，直到窗口变为非0时，接收端将发送一个 确认 ACK 告诉发送端可以再次发送数据，但是该报文有可能在传输时丢失。若该 ACK 报文丢失，则双方可能会一直等待下去，为了避免这种死锁情况的发生，发送方使用一个坚持定时器来周期性地向接收方发送探测报文段，以查看接收方窗口是否变大。

<li>延迟应答计时器：延迟应答也被称为捎带 ACK，这个定时器是在延迟应答的时候使用的，为了提高网络传输的效率，当服务器接收到客户端的数据后，不是立即回 ACK 给客户端，而是等一段时间，这样如果服务端有数据需要发送给客户端的话，就可以把数据和 ACK 一起发送给客户端了。

<li>保活定时器：该定时器是在建立 TCP 连接时指定 SO_KEEPLIVE 时才会生效，当发送方和接收方长时间没有进行数据交互时，该定时器可以用于确定对端是否还活着。

<li>FIN_WAIT_2 定时器：当主动请求关闭的一方发送 FIN 报文给接收端并且收到其对 FIN 的确认 ACK后进入 FIN_WAIT_2状态。如果这个时候因为网络突然断掉、被动关闭的一端宕机等原因，导致请求方没有收到接收方发来的 FIN，主动关闭的一方会一直等待。该定时器的作用就是为了避免这种情况的发生。当该定时器超时的时候，请求关闭方将不再等待，直接释放连接。

<li>TIME_WAIT 定时器：我们知道在 TCP 四次挥手中，发送方在最后一次挥手之后会进入 TIME_WAIT 状态，不直接进入 CLOSE 状态的主要原因是被动关闭方万一在超时时间内没有收到最后一个 ACK，则会重发最后的 FIN，2 MSL（报文段最大生存时间）等待时间保证了重发的 FIN 会被主动关闭的一段收到且重新发送最后一个 ACK 。还有一个原因是在这 2 MSL 的时间段内任何迟到的报文段会被接收方丢弃，从而防止老的 TCP 连接的包在新的 TCP 连接里面出现。
</ul>

### TCP是如何保证可靠性的
<ul>
<li>数据分块
<li>序列号和确认应答
<li>校验和
<li>流量控制
<li>拥塞控制
</li>
<li>ARQ协议
<li>超时重传
</ul>

### UDP 为什么是不可靠的？bind 和 connect 对于 UDP 的作用是什么

UDP 只有一个 socket 接收缓冲区，没有 socket 发送缓冲区，即只要有数据就发，不管对方是否可以正确接收。而在对方的 socket 接收缓冲区满了之后，新来的数据报无法进入到 socket 接受缓冲区，此数据报就会被丢弃，因此 UDP 不能保证数据能够到达目的地，此外，UDP 也没有流量控制和重传机制，故UDP的数据传输是不可靠的。

和 TCP 建立连接时采用三次握手不同，UDP 中调用 connect 只是把对端的 IP 和 端口号记录下来，并且 UDP 可多多次调用 connect 来指定一个新的 IP 和端口号，或者断开旧的 IP 和端口号（通过设置 connect 函数的第二个参数）。和普通的 UDP 相比，调用 connect 的 UDP 会提升效率，并且在高并发服务中会增加系统稳定性。

当 UDP 的发送端调用 bind 函数时，就会将这个套接字指定一个端口，若不调用 bind 函数，系统内核会随机分配一个端口给该套接字。当手动绑定时，能够避免内核来执行这一操作，从而在一定程度上提高性能。

### 如果接收方滑动窗口满了，发送方会怎么做

基于 TCP 流量控制中的滑动窗口协议，我们知道接收方返回给发送方的 ACK 包中会包含自己的接收窗口大小，若接收窗口已满，此时接收方返回给发送方的接收窗口大小为 0，此时发送方会等待接收方发送的窗口大小直到变为非 0 为止，然而，接收方回应的 ACK 包是存在丢失的可能的，为了防止双方一直等待而出现死锁情况，此时就需要坚持计时器来辅助发送方周期性地向接收方查询，以便发现窗口是否变大【坚持计时器参考问题】，当发现窗口大小变为非零时，发送方便继续发送数据。
<br><br>

### TCP 拥塞控制采用的四种算法

<ul>
<li>慢开始

当发送方开始发送数据时，由于一开始不知道网络负荷情况，如果立即将大量的数据字节传输到网络中，那么就有可能引起网络拥塞。一个较好的方法是在一开始发送少量的数据先探测一下网络状况，即由小到大的增大发送窗口（拥塞窗口 cwnd）。慢开始的慢指的是初始时令 cwnd为 1，即一开始发送一个报文段。如果收到确认，则 cwnd = 2，之后每收到一个确认报文，就令 cwnd = cwnd* 2。

但是，为了防止拥塞窗口增长过大而引起网络拥塞，另外设置了一个慢开始门限 ssthresh。
<ul>

<li>当 cwnd < ssthresh 时，使用上述的慢开始算法；

<li>当 cwnd > ssthresh 时，停止使用慢开始，转而使用拥塞避免算法；

<li>当 cwnd == ssthresh 时，两者均可。
</ul></li><br>

<li>拥塞避免
拥塞控制是为了让拥塞窗口 cwnd 缓慢地增大，即每经过一个往返时间 RTT （往返时间定义为发送方发送数据到收到确认报文所经历的时间）就把发送方的 cwnd 值加 1，通过让 cwnd 线性增长，防止很快就遇到网络拥塞状态。

当网络拥塞发生时，让新的慢开始门限值变为发生拥塞时候的值的一半,并将拥塞窗口置为 1 ,然后再次重复两种算法（慢开始和拥塞避免）,这时一瞬间会将网络中的数据量大量降低。
</li><br>

<li>快重传
快重传算法要求接收方每收到一个失序的报文就立即发送重复确认，而不要等到自己发送数据时才捎带进行确认，假定发送方发送了 Msg 1 ~ Msg 4 这 4 个报文，已知接收方收到了 Msg 1，Msg 3 和 Msg 4 报文，此时因为接收到收到了失序的数据包，按照快重传的约定，接收方应立即向发送方发送 Msg 1 的重复确认。 于是在接收方收到 Msg 4 报文的时候，向发送方发送的仍然是 Msg 1 的重复确认。这样，发送方就收到了 3 次 Msg 1 的重复确认，于是立即重传对方未收到的 Msg 报文。由于发送方尽早重传未被确认的报文段，因此，快重传算法可以提高网络的吞吐量。</li><br>

<li>快恢复
快恢复算法是和快重传算法配合使用的，该算法主要有以下两个要点：
<ul>
<li>当发送方连续收到三个重复确认，执行乘法减小，慢开始门限 ssthresh 值减半；

<li>由于发送方可能认为网络现在没有拥塞，因此与慢开始不同，把 cwnd 值设置为 ssthresh 减半之后的值，然后执行拥塞避免算法，线性增大 cwnd。
</ul>
</li><br>
</ul>

### TCP 粘包问题
<ul>

#### 为什么会发生TCP粘包和拆包?
<ul>
<li>发送方写入的数据大于套接字缓冲区的大小，此时将发生拆包。

<li>发送方写入的数据小于套接字缓冲区大小，由于 TCP 默认使用 Nagle 算法，只有当收到一个确认后，才将分组发送给对端，当发送方收集了多个较小的分组，就会一起发送给对端，这将会发生粘包。

<li>进行 MSS （最大报文长度）大小的 TCP 分段，当 TCP 报文的数据部分大于 MSS 的时候将发生拆包。

<li>发送方发送的数据太快，接收方处理数据的速度赶不上发送端的速度，将发生粘包。
</ul>

#### 常见解决方法

<ul>
<li>在消息的头部添加消息长度字段，服务端获取消息头的时候解析消息长度，然后向后读取相应长度的内容。

<li>固定消息数据的长度，服务端每次读取既定长度的内容作为一条完整消息，当消息不够长时，空位补上固定字符。但是该方法会浪费网络资源。

<li>设置消息边界，也可以理解为分隔符，服务端从数据流中按消息边界分离出消息内容，一般使用换行符。
</ul>

#### 什么时候需要处理粘包问题？

>当接收端同时收到多个分组，并且这些分组之间毫无关系时，需要处理粘包；而当多个分组属于同一数据的不同部分时，并不需要处理粘包问题。

</ul>


### TCP 报文包含哪些信息

<ul>
TCP 报文是 TCP 传输的的数据单元，也叫做报文段，其报文格式如下图所示：
<div align = "center"><img src = "./../pics/TCP报文格式.png" width = "400"></div>

<li>源端口和目的端口号：它用于多路复用/分解来自或送往上层应用的数据，其和 IP 数据报中的源 IP 与目的 IP 地址一同确定一条 TCP 连接。

<li>序号和确认号字段：序号是本报文段发送的数据部分中第一个字节的编号，在 TCP 传送的流中，每一个字节一个序号。例如一个报文段的序号为 100，此报文段数据部分共有 100 个字节，则下一个报文段的序号为 200。序号确保了 TCP 传输的有序性。确认号，即 ACK，指明下一个想要收到的字节序号，发送 ACK 时表明当前序号之前的所有数据已经正确接收。这两个字段的主要目的是保证数据可靠传输。

<li>首部长度：该字段指示了以 32 比特的字为单位的 TCP 的首部长度。其中固定字段长度为 20 字节，由于首部长度可能含有可选项内容，因此 TCP 报头的长度是不确定的，20 字节是 TCP 首部的最小长度。

<li>保留：为将来用于新的用途而保留。

<li>控制位：URG 表示紧急指针标志，该位为 1 时表示紧急指针有效，为 0 则忽略；ACK 为确认序号标志，即相应报文段包括一个对已被成功接收报文段的确认；PSH 为 push 标志，当该位为 1 时，则指示接收方在接收到该报文段以后，应尽快将这个报文段交给应用程序，而不是在缓冲区排队； RST 为重置连接标志，当出现错误连接时，使用此标志来拒绝非法的请求；SYN 为同步序号，在连接的建立过程中使用，例如三次握手时，发送方发送 SYN 包表示请求建立连接；FIN 为 finish 标志，用于释放连接，为 1 时表示发送方已经没有数据发送了，即关闭本方数据流。

<li>接收窗口：主要用于 TCP 流量控制。该字段用来告诉发送方其窗口（缓冲区）大小，以此控制发送速率，从而达到流量控制的目的。

<li>校验和：奇偶校验，此校验和是对整个 TCP 报文段，包括 TCP 头部和 数据部分。该校验和是一个端到端的校验和，由发送端计算和存储，并由接收端进行验证，主要目的是检验数据是否发生改动，若检测出差错，接收方会丢弃该 TCP 报文。

<li>紧急数据指针：紧急数据用于告知紧急数据所在的位置，在URG标志位为 1 时才有效。当紧急数据存在时，TCP 必须通知接收方的上层实体，接收方会对紧急模式采取相应的处理。

<li>选项：该字段一般为空，可根据首部长度进行推算。主要有以下作用：
<ul>

<li>TCP 连接初始化时，通信双方确认最大报文长度。

<li>在高速数据传输时，可使用该选项协商窗口扩大因子。

<li>作为时间戳时，提供一个 较为精准的 RTT，主要为了更好的实现 TCP 流量控制协议。
</ul></li>

<li>数据：TCP 报文中的数据部分也是可选的，例如在 TCP 三次握手和四次挥手过程中，通信双方交换的报文只包含头部信息，数据部分为空，只有当连接成功建立后，TCP 包才真正携带数据。

</ul>

### SYN FLOOD 是什么
>SYN Flood 是种典型的 DoS（拒绝服务）攻击，其目的是通过消耗服务器所有可用资源使服务器无法用于处理合法请求。通过重复发送初始连接请求（SYN）数据包，攻击者能够压倒目标服务器上的所有可用端口，导致目标设备根本不响应合法请求。

### 为什么服务端易受到 SYN 攻击

在 TCP 建立连接的过程中，因为服务端不确定自己发给客户端的 SYN-ACK 消息或客户端反馈的 ACK 消息是否会丢在半路，所以会给每个待完成的半开连接状态设一个定时器，如果超过时间还没有收到客户端的 ACK 消息，则重新发送一次 SYN-ACK 消息给客户端，直到重试超过一定次数时才会放弃。

服务端为了维持半开连接状态，需要分配内核资源维护半开连接。当攻击者伪造海量的虚假 IP 向服务端发送 SYN 包时，就形成了 SYN FLOOD 攻击。攻击者故意不响应 ACK 消息，导致服务端被大量注定不能完成的半开连接占据，直到资源耗尽，停止响应正常的连接请求。

#### 解决方法：
<ul>
<li>直接的方法是提高 TCP 端口容量的同时减少半开连接的资源占用时间，然而该方法只是稍稍提高了防御能力；
<li>部署能够辨别恶意 IP 的路由器，将伪造 IP 地址的发送方发送的 SYN 消息过滤掉，该方案作用一般不是太大；
</ul>

上述两种方法虽然在一定程度上能够提高服务器的防御能力，但是没有从根本上解决服务器资源消耗殆尽的问题，而以下几种方法的出发点都是在发送方发送确认回复后才开始分配传输资源，从而避免服务器资源消耗殆尽。
<ul>
<li>SYN Cache：该方法首先构造一个全局 Hash Table，用来缓存系统当前所有的半开连接信息。在 Hash Table 中的每个桶的容量大小是有限制的，当桶满时，会主动丢掉早来的信息。当服务端收到一个 SYN 消息后，会通过一个映射函数生成一个相应的 Key 值，使得当前半连接信息存入相应的桶中。当收到客户端正确的确认报文后，服务端才开始分配传输资源块，并将相应的半开连接信息从表中删除。和服务器传输资源相比，维护表的开销要小得多。

<li>SYN Cookies：该方案原理和 HTTP Cookies 技术类似，服务端通过特定的算法将半开连接信息编码成序列号或者时间戳，用作服务端给客户端的消息编号，随 SYN-ACK 消息一同返回给连接发起方，这样在连接建立完成前服务端不保存任何信息，直到发送方发送 ACK 确认报文并且服务端成功验证编码信息后，服务端才开始分配传输资源。若请求方是攻击者，则不会向服务端会 ACK 消息，由于未成功建立连接，因此服务端并没有花费任何额外的开销。
</ul>
然而该方案也存在一些缺点，由于服务端并不保存半开连接状态，因此也就丧失了超时重传的能力，这在一定程度上降低了正常用户的连接成功率。此外，客户端发送给服务端的确认报文存在传输丢失的可能，当 ACK 确认报文丢失时，服务端和客户端会对连接的成功与否产生歧义，此时就需要上层应用采取相应的策略进行处理了。
<ul>
<li>SYN Proxy：在客户端和服务器之间部署一个代理服务器，类似于防火墙的作用。通过代理服务器与客户端进行建立连接的过程，之后代理服务器充当客户端将成功建立连接的客户端信息发送给服务器。这种方法基本不消耗服务器的资源，但是建立连接的时间变长了（总共需要 6 次握手）。
</ul>

### 高并发服务器客户端主动关闭连接和服务端主动关闭连接的区别

#### 以下是针对 TCP 服务来说的：

<li>服务端主动关闭连接
在高并发场景下，当服务端主动关闭连接时，此时服务器上就会有大量的连接处于 TIME-WAIT 状态

<li>客户端主动关闭连接
当客户端主动关闭连接时，我们并不需要关心 TIME-WAIT 状态过多造成的问题，但是需要关注服务端保持大量的 CLOSE-WAIT 状态时会产生的问题<ul>
<li>首先检查是不是自己的代码问题（看是否服务端程序忘记关闭连接），如果是，则修改代码。
<li>调整系统参数，包括句柄相关参数和 TCP/IP 的参数，一般一个 CLOSE_WAIT 会维持至少 2 个小时的时间，我们可以通过调整参数来缩短这个时间。</ul>


无论是客户端还是服务器主动关闭连接，从本质上来说，在高并发场景下主要关心的就是服务端的资源占用问题，而这也是采用 TCP 传输协议必须要面对的问题，其问题解决的出发点也是如何处理好**服务质量**和**资源消耗**之间的关系。
</ul>


# 网络层

## IP

### IP 协议的定义和作用
>IP 协议（Internet Protocol）又称互联网协议，是支持网间互联的数据包协议。该协议工作在网络层，主要目的就是为了提高网络的可扩展性，和传输层 TCP 相比，IP 协议提供一种无连接/不可靠、尽力而为的数据包传输服务，其与TCP协议（传输控制协议）一起构成了TCP/IP 协议族的核心。IP 协议主要有以下几个作用：
<ul>
<li>寻址和路由：在IP 数据包中会携带源 IP 地址和目的 IP 地址来标识该数据包的源主机和目的主机。IP 数据报在传输过程中，每个中间节点（IP 网关、路由器）只根据网络地址进行转发，如果中间节点是路由器，则路由器会根据路由表选择合适的路径。IP 协议根据路由选择协议提供的路由信息对 IP 数据报进行转发，直至抵达目的主机。
<li>分段与重组：IP 数据包在传输过程中可能会经过不同的网络，在不同的网络中数据包的最大长度限制是不同的，IP 协议通过给每个 IP 数据包分配一个标识符以及分段与组装的相关信息，使得数据包在不同的网络中能够传输，被分段后的 IP 数据报可以独立地在网络中进行转发，在到达目的主机后由目的主机完成重组工作，恢复出原来的 IP 数据包。</ul>

### 域名和 IP 的关系
>一个域名对应一个IP<br>
一个IP可以对应多个域名<br>
域名可以对应多个dns服务器实现对应多个IP

### IPV4 地址不够如何解决
<ul>
<li>DHCP：动态主机配置协议。动态分配 IP 地址，只给接入网络的设备分配IP地址，因此同一个 MAC 地址的设备，每次接入互联网时，得到的IP地址不一定是相同的，该协议使得空闲的 IP 地址可以得到充分利用。
<li>CIDR:无类别域间路由。CIDR 消除了传统的 A 类、B 类、C 类地址以及划分子网的概念，因而更加有效的分配 IPv4 的地址空间，但无法从根本上解决地址耗尽问题。
<li>NAT:网络地址转换协议。我们知道属于不同局域网的主机可以使用相同的 IP 地址，从而一定程度上缓解了 IP 资源枯竭的问题。然而主机在局域网中使用的 IP 地址是不能在公网中使用的，当局域网主机想要与公网进行通信时， NAT 方法可以将该主机 IP 地址转换成全球 IP 地址。该协议能够有效解决 IP 地址不足的问题。
<li>IPv6：作为接替 IPv4 的下一代互联网协议，其可以实现 2 的 128 次方个地址，而这个数量级，即使是给地球上每一颗沙子都分配一个IP地址，该协议能够从根本上解决 IPv4 地址不够用的问题。
</ul>

## 路由器的分组转发流程
<ol>
<li>从 IP 数据包中提取出目的主机的 IP 地址，找到其所在的网络；

<li>判断目的 IP 地址所在的网络是否与本路由器直接相连，如果是，则不需要经过其它路由器直接交付，否则执行 3；

<li>检查路由表中是否有目的 IP 地址的特定主机路由。如果有，则按照路由表传送到下一跳路由器中，否则执行 4；

<li>逐条检查路由表，若找到匹配路由，则按照路由表转发到下一跳路由器中，否则执行步骤 5；

<li>若路由表中设置有默认路由，则按照默认路由转发到默认路由器中，否则执行步骤 6；

<li>无法找到合适路由，向源主机报错。
</ol>

## ICMP 协议概念/作用
>ICMP（Internet Control Message Protocol）是因特网控制报文协议，主要是实现 IP 协议中未实现的部分功能，是一种网络层协议。该协议并不传输数据，只传输控制信息来辅助网络层通信。其主要的功能是验证网络是否畅通（确认接收方是否成功接收到 IP 数据包）以及辅助 IP 协议实现可靠传输（若发生 IP 丢包，ICMP 会通知发送方 IP 数据包被丢弃的原因，之后发送方会进行相应的处理）。
<ul>
<li>Ping
Ping（Packet Internet Groper），即因特网包探测器，是一种工作在网络层的服务命令，主要用于测试网络连接量。本地主机通过向目的主机发送 ICMP Echo 请求报文，目的主机收到之后会发送 Echo 响应报文，Ping 会根据时间和成功响应的次数估算出数据包往返时间以及丢包率从而推断网络是否通常、运行是否正常等。

<li>TraceRoute
TraceRoute 是 ICMP 的另一个应用，其主要用来跟踪一个分组从源点耗费最少 TTL 到达目的地的路径。TraceRoute 通过逐渐增大 TTL 值并重复发送数据报来实现其功能，首先，TraceRoute 会发送一个 TTL 为 1 的 IP 数据报到目的地，当路径上的第一个路由器收到这个数据报时，它将 TTL 的值减 1，此时 TTL = 0，所以路由器会将这个数据报丢掉，并返回一个差错报告报文，之后源主机会接着发送一个 TTL 为 2 的数据报，并重复此过程，直到数据报能够刚好到达目的主机。此时 TTL = 0，因此目的主机要向源主机发送 ICMP 终点不可达差错报告报文，之后源主机便知道了到达目的主机所经过的路由器 IP 地址以及到达每个路由器的往返时间。
</ul>

## ARP 地址解析协议的原理和地址解析过程

ARP（Address Resolution Protocol）是地址解析协议的缩写，该协议提供根据 IP 地址获取物理地址的功能，它工作在第二层，是一个数据链路层协议，其在本层和物理层进行联系，同时向上层提供服务。当通过以太网发送 IP 数据包时，需要先封装 32 位的 IP 地址和 48位 MAC 地址。在局域网中两台主机进行通信时需要依靠各自的物理地址进行标识，但由于发送方只知道目标 IP 地址，不知道其 MAC 地址，因此需要使用地址解析协议。 ARP 协议的解析过程如下：
<ol>
<li>首先，每个主机都会在自己的 ARP 缓冲区中建立一个 ARP 列表，以表示 IP 地址和 MAC 地址之间的对应关系；

<li>当源主机要发送数据时，首先检查 ARP 列表中是否有 IP 地址对应的目的主机 MAC 地址，如果存在，则可以直接发送数据，否则就向同一子网的所有主机发送 ARP 数据包。该数据包包括的内容有源主机的 IP 地址和 MAC 地址，以及目的主机的 IP 地址。

<li>当本网络中的所有主机收到该 ARP 数据包时，首先检查数据包中的 目的 主机IP 地址是否是自己的 IP 地址，如果不是，则忽略该数据包，如果是，则首先从数据包中取出源主机的 IP 和 MAC 地址写入到 ARP 列表中，如果已经存在，则覆盖，然后将自己的 MAC 地址写入 ARP 响应包中，告诉源主机自己是它想要找的 MAC 地址。

<li>源主机收到 ARP 响应包后。将目的主机的 IP 和 MAC 地址写入 ARP 列表，并利用此信息发送数据。如果源主机一直没有收到 ARP 响应数据包，表示 ARP 查询失败。
</ol>

## 网络地址转换 NAT
>NAT（Network Address Translation），即网络地址转换，它是一种把内部私有网络地址翻译成公有网络 IP 地址的技术。该技术不仅能解决 IP 地址不足的问题，而且还能隐藏和保护网络内部主机，从而避免来自外部网络的攻击。

### NAT 的实现方式主要有三种：
<ul>
<li>静态转换：内部私有 IP 地址和公有 IP 地址是一对一的关系，并且不会发生改变。通过静态转换，可以实现外部网络对内部网络特定设备的访问，这种方式原理简单，但当某一共有 IP 地址被占用时，跟这个 IP 绑定的内部主机将无法访问 Internet。
<li>动态转换：采用动态转换的方式时，私有 IP 地址每次转化成的公有 IP 地址是不唯一的。当私有 IP 地址被授权访问 Internet 时会被随机转换成一个合法的公有 IP 地址。当 ISP 通过的合法 IP 地址数量略少于网络内部计算机数量时，可以采用这种方式。
<li>端口多路复用：该方式将外出数据包的源端口进行端口转换，通过端口多路复用的方式，实现内部网络所有主机共享一个合法的外部 IP 地址进行 Internet 访问，从而最大限度地节约 IP 地址资源。同时，该方案可以隐藏内部网络中的主机，从而有效避免来自 Internet 的攻击。
</ul>

## TTL 是什么？有什么作用
>TTL 是指生存时间，简单来说，它表示了数据包在网络中的时间。每经过一个路由器后 TTL 就减一，这样 TTL 最终会减为 0 ，当 TTL 为 0 时，则将数据包丢弃。通过设置 TTL 可以避免这两个路由器之间形成环导致数据包在环路上死转的情况，由于有了 TTL ，当 TTL 为 0 时，数据包就会被抛弃。

## 运输层协议和网络层协议的区别
>网络层协议负责提供主机间的逻辑通信<br>
运输层协议负责提供进程间的逻辑通信

<br><br>

# 数据链路层

## MAC 地址和 IP 地址分别有什么作用
<li>MAC 地址是数据链路层和物理层使用的地址，是写在网卡上的物理地址。MAC 地址用来定义网络设备的位置
<li>IP 地址是网络层和以上各层使用的地址，是一种逻辑地址。IP 地址用来区别网络上的计算机

## 为什么有了 MAC 地址还需要 IP 地址
IP地址是地域相关，MAC地址是设备相关。维护IP地址的开销要远远低于MAC地址

## 以太网中的 CSMA/CD 协议
>CSMA/CD 为载波侦听多路访问/冲突检测，是像以太网这种广播网络采用的一种机制，我们知道在以太网中多台主机在同一个信道中进行数据传输，CSMA/CD 很好的解决了共享信道通信中出现的问题，它的工作原理主要包括两个部分：
<ul>
<li>载波监听：当使用 CSMA/CD 协议时，总线上的各个节点都在监听信道上是否有信号在传输，如果有的话，表明信道处于忙碌状态，继续保持监听，直到信道空闲为止。如果发现信道是空闲的，就立即发送数据。
<li>冲突检测：当两个或两个以上节点同时监听到信道空闲，便开始发送数据，此时就会发生碰撞（数据的传输延迟也可能引发碰撞）。当两个帧发生冲突时，数据帧就会破坏而失去了继续传输的意义。在数据的发送过程中，以太网是一直在监听信道的，当检测到当前信道冲突，就立即停止这次传输，避免造成网络资源浪费，同时向信道发送一个「冲突」信号，确保其它节点也发现该冲突。之后采用一种二进制退避策略让待发送数据的节点随机退避一段时间之后重新。
</ul>

**边发边听，忙则等待，闲则发送**

## 数据链路层上的三个基本问题
<ul>
<li>封装成帧：将网络层传下来的分组前后分别添加首部和尾部，这样就构成了帧。首部和尾部的一个重要作用是帧定界，也携带了一些必要的控制信息，对于每种数据链路层协议都规定了帧的数据部分的最大长度。
<li>透明传输：帧使用首部和尾部进行定界，如果帧的数据部分含有和首部和尾部相同的内容， 那么帧的开始和结束的位置就会判断错，因此需要在数据部分中出现有歧义的内容前边插入转义字符，如果数据部分出现转义字符，则在该转义字符前再加一个转义字符。在接收端进行处理之后可以还原出原始数据。这个过程透明传输的内容是转义字符，用户察觉不到转义字符的存在。
<li>差错检测：目前数据链路层广泛使用循环冗余检验（CRC）来检查数据传输过程中是否产生比特差错。</ul>

## PPP 协议
互联网用户通常需要连接到某个 ISP 之后才能接入到互联网，PPP（点对点）协议是用户计算机和 ISP 进行通信时所使用的数据链路层协议。点对点协议为点对点连接上传输多协议数据包提供了一个标准方法。该协议设计的目的主要是用来通过拨号或专线方式建立点对点连接发送数据，使其成为各种主机、网桥和路由器之间简单连接的一种解决方案。

### PPP 协议具有以下特点：
<ul>
<li>PPP 协议具有动态分配 IP 地址的能力，其允许在连接时刻协商 IP 地址。
<li>PPP 支持多种网络协议，例如 TCP/IP、NETBEUI 等。
<li>PPP 具有差错检测能力，但不具备纠错能力，所以 PPP 是不可靠传输协议。
<li>无重传的机制，网络开销小，速度快。
<li>PPP 具有身份验证的功能。
</ul>

### 为什么 PPP 协议不使用序号和确认机制
<ul>
<li>IETF 在设计因特网体系结构时把其中最复杂的部分放在 TCP 协议中，而网际协议 IP 则相对比较简单，它提供的是不可靠的数据包服务，在这种情况下，数据链路层没有必要提供比 IP 协议更多的功能。若使用能够实现可靠传输的数据链路层协议，则开销就要增大，这在数据链路层出现差错概率不大时是得不偿失的。
<li>即使数据链路层实现了可靠传输，但其也不能保证网络层的传输也是可靠的，当数据帧在路由器中从数据链路层上升到网络层后，仍有可能因为网络层拥塞而被丢弃。
<li>PPP 协议在帧格式中有帧检验序列，对每一个收到的帧，PPP 都会进行差错检测，若发现差错，则丢弃该帧。

# 网络安全

## ARP 攻击

在 ARP 的解析过程中，局域网上的任何一台主机如果接收到一个 ARP 应答报文，并不会去检测这个报文的真实性，而是直接记入自己的 ARP 缓存表中。并且这个 ARP 表是可以被更改的，当表中的某一列长时间不适使用，就会被删除。ARP 攻击就是利用了这一点，攻击者疯狂发送 ARP 报文，其源 MAC 地址为攻击者的 MAC 地址，而源 IP 地址为被攻击者的 IP 地址。通过不断发送这些伪造的 ARP 报文，让网络内部的所有主机和网关的 ARP 表中被攻击者的 IP 地址所对应的 MAC 地址为攻击者的 MAC 地址。这样所有发送给被攻击者的信息都会发送到攻击者的主机上，从而产生 ARP 欺骗。通常可以把 ARP 欺骗分为以下几种：
<ul>
<li>洪泛攻击</li>
攻击者恶意向局域网中的网关、路由器和交换机等发送大量 ARP 报文，设备的 CPU 忙于处理 ARP 协议，而导致难以响应正常的服务请求。其表现通常为：网络中断或者网速很慢。

<li>欺骗主机</li>
这种攻击方式也叫仿冒网关攻击。攻击者通过 ARP 欺骗使得网络内部被攻击主机发送给网关的信息实际上都发送给了攻击者，主机更新的 ARP 表中对应的 MAC 地址为攻击者的 MAC。当用户主机向网关发送重要信息使，该攻击方式使得用户的数据存在被窃取的风险。

<li>欺骗网关</li>
该攻击方式和欺骗主机的攻击方式类似，不过这种攻击的欺骗对象是局域网的网关，当局域网中的主机向网关发送数据时，网关会把数据发送给攻击者，这样攻击者就会源源不断地获得局域网中用户的信息。该攻击方式同样会造成用户数据外泄。

<li>中间人攻击</li>
攻击者同时欺骗网关和主机，局域网的网关和主机发送的数据最后都会到达攻击者这边。这样，网关和用户的数据就会泄露。

<li>IP 地址冲突</li>
攻击者对局域网中的主机进行扫描，然后根据物理主机的 MAC 地址进行攻击，导致局域网内的主机产生 IP 冲突，使得用户的网络无法正常使用。
</ul>
