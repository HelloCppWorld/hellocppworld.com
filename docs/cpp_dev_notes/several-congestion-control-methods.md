# 计算机网络中几种拥塞控制的方法

1999年公布的因特网建议标准`RFC 2581`定义了进行拥塞控制的四种算法，即**慢开始**（`slow-start`）、**拥塞避免**（`congestion avoidance`）、**快重传**（`fast retransmit`）和**快恢复**（`fast recovery`）。

## 慢开始

发送方维持一个叫**拥塞窗口**`cwnd`（`congestion window`）的状态变量。拥塞窗口的大小取决于网络的拥塞成都，并动态的调整。发送发让自己的发送窗口小于等于拥塞窗口。（考虑到发送窗口可能小于拥塞窗口）

**慢开始**的思路是，在主机刚开始发送数据时，先把拥塞窗口`cwnd`设定为一个最大报文段`MSS`的数值（默认是`536`）。如果在规定时间内接收到了此报文的确认，就把拥塞窗口`cwnd*2`，重复上述过程直到`cwnd`到达慢开始门限（`ssthresh`）。

## 拥塞避免

当`cwnd  >=ssthresh`，此时`cwnd`的增长方式变为线性增长，每次只增长固定的值。无论在慢开始阶段还是拥塞避免阶段，只要发送方无法按时收到报文确认（发生重传），就把慢开始门限（`ssthresh`）设置为当前拥塞窗口`cwnd`的一半（但一般不小于2），然后把拥塞窗口`cwnd`设置为一个最大报文段`MSS`的数值，然后重新启动慢开始的流程。

![拥塞避免](https://s2.loli.net/2022/03/13/jNdLYyhO9AVkR3i.jpg)

## 快重传

快重传算法要求接收方没收到一个失序报文段后立即发出重复确认（而不是等待自己发送数据时才进行捎带确认）。如果发送方依次发送了`M1、M2、M3、M4、M5、M6`数据段，接收方收到了`M1、M2`并对其进行了确认，但是`M3`却丢失了。后续收到的`M4、M5、M6`都会对`M2`进行确认（四次确认，三次重复）。快重传算法规定，发送方只要连续收到三个重复确认就立即重传对方尚未收到的报文段（本例中为`M3`），而不是等待报文的超时定时器到时才重传超时报文。由于发送方能尽早重传未被确认的报文段，因此采用快重传后可使整个网络的吞吐量提高约20%。

![快重传](https://s2.loli.net/2022/03/13/yvZCi9dSjhnPugW.jpg)

## 快恢复

当网络发生拥塞时（重传发生），快恢复算法要求把当前的发送窗口`cwnd`减半，同时把慢开始门限（`ssthresh`）也设置为发送窗口`cwnd`的一半，然后开始拥塞避免算法，`cwnd`线性增加（而非指数增加）。

![快恢复](https://s2.loli.net/2022/03/13/w4WCyN6c8qd9ALO.jpg)

由图可见，慢开始（主要指基数小）不慢（指数增加），快恢复（主要指基数大）不快（线性增加）。

`现实中发送窗口的上限 = Min(cwnd,rwnd)`，所以发送窗口取决于拥塞窗口`cwnd`和接收窗口`rwnd`中最小的那个。

文章来源：《计算机网络》谢希仁