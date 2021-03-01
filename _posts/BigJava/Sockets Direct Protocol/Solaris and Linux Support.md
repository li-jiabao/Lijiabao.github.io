
# Supported Java APIs

All of the APIs that use TCP can use SDP, specifically including the following classes:

<li>`java.net` package
<ul>
- `Socket`
- `ServerSocket`

- `SocketChannel`
- `ServerSocketChannel`
- `AsynchronousSocketChannel`
- `AsynchronousServerSocketChannel`

When SDP support is enabled, it just works without any change to your code. Compiling is not necessary. However, it is important to know that a socket is bound only once. A connection is an implicit bind. So, if the socket hasn't been previously bound and `connect` is invoked, the binding occurs at that time.

For example, consider the following code snippet:

```

AsynchronousSocketChannel ch = AsynchronousSocketChannel.open();
ch.bind(local);
Future&lt;Void&gt; result = ch.connect(remote);

```

In this snippet, the asynchronous socket channel is bound to a local TCP address when `bind` is invoked on the socket. Then, the code attempts to connect to a remote address by using the same socket. If the remote address uses InfiniBand, as specified in the configuration file, the connection will not be converted to SDP because the socket was previously bound.
