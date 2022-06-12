# 1. Introduction / 引言

> HTTP semantics ([[HTTP](https://www.rfc-editor.org/rfc/rfc9114.html#RFC9110)]) are used for a broad range of services on the Internet. These semantics have most commonly been used with HTTP/1.1 and HTTP/2. HTTP/1.1 has been used over a variety of transport and session layers, while HTTP/2 has been used primarily with TLS over TCP. HTTP/3 supports the same semantics over a new transport protocol: QUIC.

HTTP 语义被广泛用于 Internet 上的服务。 这些语义最常用于 HTTP/1.1 和 HTTP/2。 HTTP/1.1 已被用于各种传输层和会话层之上，而 HTTP/2 主要结合 TCP 上的 TLS 来使用。 HTTP/3 通过新的传输协议 QUIC 以支持相同的语义。

## 1.1. Prior Versions of HTTP / HTTP的早期版本

> HTTP/1.1 ([[HTTP/1.1](https://www.rfc-editor.org/rfc/rfc9114.html#RFC9112)]) uses whitespace-delimited text fields to convey HTTP messages. While these exchanges are human readable, using whitespace for message formatting leads to parsing complexity and excessive tolerance of variant behavior.

HTTP/1.1 使用空格分隔的文本字段来传达 HTTP 消息。 虽然这些交换是人类可读的，但使用空格进行消息格式化会导致解析复杂性和对变体行为的过度容忍。

> Because HTTP/1.1 does not include a multiplexing layer, multiple TCP connections are often used to service requests in parallel. However, that has a negative impact on congestion control and network efficiency, since TCP does not share congestion control across multiple connections.

由于 HTTP/1.1 不包含多路复用层，因此通常使用多个 TCP 连接来并行处理请求。 然而，这对拥塞控制和网络效率有负面影响，因为 TCP 不跨多个连接共享拥塞控制。

> HTTP/2 ([[HTTP/2](https://www.rfc-editor.org/rfc/rfc9114.html#RFC9113)]) introduced a binary framing and multiplexing layer to improve latency without modifying the transport layer. However, because the parallel nature of HTTP/2's multiplexing is not visible to TCP's loss recovery mechanisms, a lost or reordered packet causes all active transactions to experience a stall regardless of whether that transaction was directly impacted by the lost packet.

HTTP/2 引入了二进制帧和多路复用层，以在不修改传输层的情况下改善延迟。 但是，由于 HTTP/2 多路复用的并行特性对 TCP 的丢失恢复机制不可见，因此丢失或重新排序的数据包会导致所有活动事务都经历停顿，无论该事务是否受到丢失数据包的直接影响。

## 1.2. Delegation to QUIC / 向 QUIC 委派

> The QUIC transport protocol incorporates stream multiplexing and per-stream flow control, similar to that provided by the HTTP/2 framing layer. By providing reliability at the stream level and congestion control across the entire connection, QUIC has the capability to improve the performance of HTTP compared to a TCP mapping. QUIC also incorporates TLS 1.3 ([[TLS](https://www.rfc-editor.org/rfc/rfc9114.html#TLS)]) at the transport layer, offering comparable confidentiality and integrity to running TLS over TCP, with the improved connection setup latency of TCP Fast Open ([[TFO](https://www.rfc-editor.org/rfc/rfc9114.html#TFO)]).

QUIC 传输协议结合了，类似于 HTTP/2 分帧层提供的，流多路复用和每个流的流量控制。通过提供流级别的可靠性和整个连接的拥塞控制，与 TCP 映射相比，QUIC 能够提高 HTTP 的性能。 QUIC 还在传输层整合了 TLS 1.3 ，提供与在 TCP 之上的 TLS 相当的机密性和完整性，并改进了 TCP Fast Open 的连接设置延迟。

> This document defines HTTP/3: a mapping of HTTP semantics over the QUIC transport protocol, drawing heavily on the design of HTTP/2. HTTP/3 relies on QUIC to provide confidentiality and integrity protection of data; peer authentication; and reliable, in-order, per-stream delivery. While delegating stream lifetime and flow-control issues to QUIC, a binary framing similar to the HTTP/2 framing is used on each stream. Some HTTP/2 features are subsumed by QUIC, while other features are implemented atop QUIC.

本文档定义了 HTTP/3：一个 HTTP 语义在 QUIC 传输协议上的映射，大量借鉴了 HTTP/2 的设计。 HTTP/3 依赖于 QUIC 来提供数据的机密性和完整性保护、对等认证、 和可靠有序按流的交付。 在将流生命周期和流量问题委托给 QUIC 时，每个流都使用类似于 HTTP/2 框架的二进制框架。一些 HTTP/2 功能包含在 QUIC 中，而其他功能则在 QUIC 之上实现。

> QUIC is described in [[QUIC-TRANSPORT](https://www.rfc-editor.org/rfc/rfc9114.html#QUIC-TRANSPORT)]. For a full description of HTTP/2, see [[HTTP/2](https://www.rfc-editor.org/rfc/rfc9114.html#RFC9113)].

QUIC 在 [QUIC-TRANSPORT] 中有描述。 有关 HTTP/2 的完整描述，请参阅 [HTTP/2]。

