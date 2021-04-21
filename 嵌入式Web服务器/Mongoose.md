# Mongoose(Server)移植

1. 初始化

```c
struct mg_mgr mgr;
mg_mgr_init(&mgr);
```

2. 创建连接

```c
struct mg_connection *c = mg_http_listen(&mgr, "0.0.0.0:8000", fn, arg);
```

2. 创建循环事件

```c
for (;;) {
  mg_mgr_poll(&mgr, 1000);
}
```

### Send and Receive buffers

### Event handle function

**Para**:

+ `struct mg_connection *c` :a connection that received an event

+ `int ev` :an event number, defined in mongoose.h. For example, when data arrives on an inbound connection, ev would be `MG_EV_RECV`

+ `void *ev_data` :points to the event-specific data, and it has a different meaning for different events.
+ `void \*fn_data` :a user-defined pointer for the connection, which is a placeholder for application-specific data

```c
static void fn(struct mg_connection *c, int ev, void *ev_data, void *fn_data) {
  switch (ev) {
    /* Event handler code that defines behavior of the connection */
    ...
  }
}
```

 ### Event

### Connection flags

`struct mg_connection` 的连接标识位

用户可改变的标志为是:`is_hexdumping`,`is_drainig`, `is_closing`

```C
struct mg_connection {
  ...
  unsigned is_listening : 1;   // Listening connection
  unsigned is_client : 1;      // Outbound (client) connection
  unsigned is_accepted : 1;    // Accepted (server) connection
  unsigned is_resolving : 1;   // Non-blocking DNS resolv is in progress
  unsigned is_connecting : 1;  // Non-blocking connect is in progress
  unsigned is_tls : 1;         // TLS-enabled connection
  unsigned is_tls_hs : 1;      // TLS handshake is in progress
  unsigned is_udp : 1;         // UDP connection
  unsigned is_websocket : 1;   // WebSocket connection
  unsigned is_hexdumping : 1;  // Hexdump in/out traffic
  unsigned is_draining : 1;    // Send remaining data, then close and free
  unsigned is_closing : 1;     // Close and free the connection immediately
  unsigned is_readable : 1;    // Connection is ready to read
  unsigned is_writable : 1;    // Connection is ready to write
};
```

### Configuration

| `MG_ENABLE_LWIP`                | 0                 | Use LWIP low-level API instead of BSD sockets                |
| ------------------------------- | ----------------- | ------------------------------------------------------------ |
| `MG_ENABLE_SOCKET`              | 1                 | Use BSD socket low-level API                                 |
| `MG_ENABLE_MBEDTLS`             | 0                 | Enable Mbed TLS library                                      |
| `MG_ENABLE_OPENSSL`             | 0                 | Enable OpenSSL library                                       |
| `MG_ENABLE_FS`                  | 1                 | Enable API that use filesystem, like `mg_http_send_file()`   |
| `MG_ENABLE_IPV6`                | 0                 | Enable IPv6                                                  |
| `MG_ENABLE_LOG`                 | 1                 | Enable `LOG()` macro                                         |
| `MG_ENABLE_MD5`                 | 0                 | Use native MD5 implementation                                |
| `MG_ENABLE_DIRECTORY_LISTING`   | 0                 | Enable directory listing for HTTP server                     |
| `MG_ENABLE_HTTP_DEBUG_ENDPOINT` | 0                 | Enable `/debug/info` debug URI                               |
| `MG_ENABLE_SOCKETPAIR`          | 0                 | Enable `mg_socketpair()` for multi-threading                 |
| `MG_IO_SIZE`                    | 512               | Granularity of the send/recv IO buffer growth<br>controls the maximum UDP message size |
| `MG_MAX_RECV_BUF_SIZE`          | (3 * 1024 * 1024) | Maximum recv buffer size                                     |
| `MG_MAX_HTTP_HEADERS`           | 40                | Maximum number of HTTP headers                               |