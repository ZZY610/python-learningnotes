# sshtunnel库

paramiko 是一个用于 Python 的 SSH 协议的实现库。可以创建 SSH 连接，执行远程命令，传输文件以及管理远程主机。**sshtunnel 库**在 paramiko 基础上创建 **SSH 隧道**，并通过隧道传递流量到远程服务器的某个端口上。

SSH隧道是通过SSH连接来创建的，但它们本身不是SSH连接。**SSH隧道是一种通过SSH连接到远程服务器并将本地端口映射到远程服务器上的服务端口的方法。** 这可以用于安全地访问远程服务器上的服务，而不必将这些服务直接暴露在公共网络上。

## SSH协议
SSH（Secure Shell）协议工作在计算机网络的应用层。它是一种加密的网络协议，用于安全地远程管理计算机系统和传输数据。

SSH 隧道（SSH Tunnel）是一种通过 SSH 连接在两个网络节点之间创建的加密通信通道。这个通道可用于安全地传输网络流量，将本地端口**映射**到远程服务器上的服务端口，或者用于加密和保护数据传输。

SSH 隧道有两端：

1. **本地端（Local Endpoint）**：位于本地计算机上。ssh可以将本地端口映射到远程服务器上，使得本地计算机上的应用程序可以通过该端口与远程服务器上的服务进行安全通信。

2. **远程端（Remote Endpoint）**：位于远程服务器上。它负责接收来自本地端口的连接请求，并将流量路由到相应的服务或资源。

SSH 隧道的主要目的是保护数据的机密性和完整性，以及绕过网络中的潜在威胁，如中间人攻击。常见的 SSH 隧道类型包括：

- **本地端口转发**：将本地计算机上的一个端口映射到远程服务器上的一个服务端口，使得本地应用程序可以通过这个端口连接远程服务，从而加密数据传输。

- **远程端口转发**：将远程服务器上的一个端口映射到本地计算机上的一个服务端口，通常用于访问本地计算机上的服务，例如数据库，而不直接将其暴露在互联网上。

- **动态端口转发**：创建一个 SOCKS 代理，通过该代理可以将所有网络流量路由到远程服务器上，用于保护你的网络通信和隐藏真实 IP 地址。

::: tip 注意
"远程 SSH 服务器" 和 "远程服务器" 可以是不同的服务器。

"远程 SSH 主机" 是你要通过 SSH 协议连接的服务器，它通常用于建立安全的通信通道，可以是你自己的服务器或者是第三方提供的 SSH 服务器。

"远程服务器" 则是你希望通过 SSH 隧道访问的实际服务所在的服务器，它可以位于 "远程 SSH 主机" 同一台服务器上，也可以是局域网中的其他服务器。

SSH 隧道的作用是通过 "远程 SSH 主机" 连接到 "远程服务器" 上的服务。
:::

## SSHTunnelForwarder类

核心类。用于创建SSH隧道。

### 关键参数

* **ssh_address_or_host (tuple or str)**
远程SSH服务器的IP地址（和ssh开放端口号）
    * 传入元组：`(主机IP,端口号)`
    * 传入字符串：主机IP

* **ssh_username (str)**
    * 在远程服务器登录的用户名。
    * 默认为当前本地用户名。
* **ssh_password (str)**：密码。

* **ssh_port (int)**：默认22，可以在第一个参数中指定。

* **local_bind_address** (tuple)
    * 传入元组（主机IP，端口号）。两个参数都是可选的，默认 `('0000',RANDOM_PORT)`
    * 本地绑定地址。代表隧道本端IP和端口。

* **remote_bind_address** (tuple)
    * 隧道远程端的IP和端口。
    * 通常IP设置为127.0.0.1或localhost。
    * 端口是远程服务器的服务端口，本地端口映射到的远程服务端口。

### 主要方法




```python
from sshtunnel import SSHTunnelForwarder
import pymysql

# SSH 连接信息
ssh_host = 'xxxx.xxxx.xxxx.xxxx'
ssh_port = 22
ssh_user = 'root'
ssh_password = 'xxxxxxxxx'

# 远程数据库信息
remote_db_host = 'localhost'
remote_db_port = 3306
remote_db_user = 'root'
remote_db_password = 'zhuzhenyang123'

# 本地端口，用于建立 SSH 隧道
local_port = 3307

# 创建 SSH 隧道
with SSHTunnelForwarder(
    (ssh_host, ssh_port),
    ssh_username=ssh_user,
    ssh_password=ssh_password,
    remote_bind_address=(remote_db_host, remote_db_port),
    local_bind_address=('127.0.0.1', local_port)
) as tunnel:
    # 创建 MySQL 连接
    conn = pymysql.connect(
        host='127.0.0.1',
        port=local_port,
        user=remote_db_user,
        passwd=remote_db_password,
        db='my_first_db',
        charset='utf8'
    )

    # 执行 SQL 查询
    with conn.cursor() as cursor:
        cursor.execute("SELECT * FROM Users")
        result = cursor.fetchall()
        print(result)
        print(cursor.fetchone())
    # 关闭连接
    conn.close()

# SSH 隧道会在退出上下文时自动关闭
```



```
D:\Python\Python311\python.exe C:\Users\zhuzy51567\PycharmProjects\pythonProject\one.py 
Help on class SSHTunnelForwarder in module sshtunnel:

class SSHTunnelForwarder(builtins.object)
 |  SSHTunnelForwarder(ssh_address_or_host=None, ssh_config_file='~/.ssh\\config', ssh_host_key=None, ssh_password=None, ssh_pkey=None, ssh_private_key_password=None, ssh_proxy=None, ssh_proxy_enabled=True, ssh_username=None, local_bind_address=None, local_bind_addresses=None, logger=None, mute_exceptions=False, remote_bind_address=None, remote_bind_addresses=None, set_keepalive=5.0, threaded=True, compression=None, allow_agent=True, host_pkey_directories=None, *args, **kwargs)
 |  
 |  **SSH tunnel class**
 |  
 |      - Initialize a SSH tunnel to a remote host according to the input
 |        arguments
 |  
 |      - Optionally:
 |          + Read an SSH configuration file (typically ``~/.ssh/config``)
 |          + Load keys from a running SSH agent (i.e. Pageant, GNOME Keyring)
 |  
 |  Raises:
 |  
 |      :class:`.BaseSSHTunnelForwarderError`:
 |          raised by SSHTunnelForwarder class methods
 |  
 |      :class:`.HandlerSSHTunnelForwarderError`:
 |          raised by tunnel forwarder threads
 |  
 |          .. note::
 |                  Attributes ``mute_exceptions`` and
 |                  ``raise_exception_if_any_forwarder_have_a_problem``
 |                  (deprecated) may be used to silence most exceptions raised
 |                  from this class
 |  
 |  Keyword Arguments:
 |  
 |      ssh_address_or_host (tuple or str):
 |          IP or hostname of ``REMOTE GATEWAY``. It may be a two-element
 |          tuple (``str``, ``int``) representing IP and port respectively,
 |          or a ``str`` representing the IP address only
 |  
 |          .. versionadded:: 0.0.4
 |  
 |      ssh_config_file (str):
 |          SSH configuration file that will be read. If explicitly set to
 |          ``None``, parsing of this configuration is omitted
 |  
 |          Default: :const:`SSH_CONFIG_FILE`
 |  
 |          .. versionadded:: 0.0.4
 |  
 |      ssh_host_key (str):
 |          Representation of a line in an OpenSSH-style "known hosts"
 |          file.
 |  
 |          ``REMOTE GATEWAY``'s key fingerprint will be compared to this
 |          host key in order to prevent against SSH server spoofing.
 |          Important when using passwords in order not to accidentally
 |          do a login attempt to a wrong (perhaps an attacker's) machine
 |  
 |      ssh_username (str):
 |          Username to authenticate as in ``REMOTE SERVER``
 |  
 |          Default: current local user name
 |  
 |      ssh_password (str):
 |          Text representing the password used to connect to ``REMOTE
 |          SERVER`` or for unlocking a private key.
 |  
 |          .. note::
 |              Avoid coding secret password directly in the code, since this
 |              may be visible and make your service vulnerable to attacks
 |  
 |      ssh_port (int):
 |          Optional port number of the SSH service on ``REMOTE GATEWAY``,
 |          when `ssh_address_or_host`` is a ``str`` representing the
 |          IP part of ``REMOTE GATEWAY``'s address
 |  
 |          Default: 22
 |  
 |      ssh_pkey (str or paramiko.PKey):
 |          **Private** key file name (``str``) to obtain the public key
 |          from or a **public** key (:class:`paramiko.pkey.PKey`)
 |  
 |      ssh_private_key_password (str):
 |          Password for an encrypted ``ssh_pkey``
 |  
 |          .. note::
 |              Avoid coding secret password directly in the code, since this
 |              may be visible and make your service vulnerable to attacks
 |  
 |      ssh_proxy (socket-like object or tuple):
 |          Proxy where all SSH traffic will be passed through.
 |          It might be for example a :class:`paramiko.proxy.ProxyCommand`
 |          instance.
 |          See either the :class:`paramiko.transport.Transport`'s sock
 |          parameter documentation or ``ProxyCommand`` in ``ssh_config(5)``
 |          for more information.
 |  
 |          It is also possible to specify the proxy address as a tuple of
 |          type (``str``, ``int``) representing proxy's IP and port
 |  
 |          .. note::
 |              Ignored if ``ssh_proxy_enabled`` is False
 |  
 |          .. versionadded:: 0.0.5
 |  
 |      ssh_proxy_enabled (boolean):
 |          Enable/disable SSH proxy. If True and user's
 |          ``ssh_config_file`` contains a ``ProxyCommand`` directive
 |          that matches the specified ``ssh_address_or_host``,
 |          a :class:`paramiko.proxy.ProxyCommand` object will be created where
 |          all SSH traffic will be passed through
 |  
 |          Default: ``True``
 |  
 |          .. versionadded:: 0.0.4
 |  
 |      local_bind_address (tuple):
 |          Local tuple in the format (``str``, ``int``) representing the
 |          IP and port of the local side of the tunnel. Both elements in
 |          the tuple are optional so both ``('', 8000)`` and
 |          ``('10.0.0.1', )`` are valid values
 |  
 |          Default: ``('0.0.0.0', RANDOM_PORT)``
 |  
 |          .. versionchanged:: 0.0.8
 |              Added the ability to use a UNIX domain socket as local bind
 |              address
 |  
 |      local_bind_addresses (list[tuple]):
 |          In case more than one tunnel is established at once, a list
 |          of tuples (in the same format as ``local_bind_address``)
 |          can be specified, such as [(ip1, port_1), (ip_2, port2), ...]
 |  
 |          Default: ``[local_bind_address]``
 |  
 |          .. versionadded:: 0.0.4
 |  
 |      remote_bind_address (tuple):
 |          Remote tuple in the format (``str``, ``int``) representing the
 |          IP and port of the remote side of the tunnel.
 |  
 |      remote_bind_addresses (list[tuple]):
 |          In case more than one tunnel is established at once, a list
 |          of tuples (in the same format as ``remote_bind_address``)
 |          can be specified, such as [(ip1, port_1), (ip_2, port2), ...]
 |  
 |          Default: ``[remote_bind_address]``
 |  
 |          .. versionadded:: 0.0.4
 |  
 |      allow_agent (boolean):
 |          Enable/disable load of keys from an SSH agent
 |  
 |          Default: ``True``
 |  
 |          .. versionadded:: 0.0.8
 |  
 |      host_pkey_directories (list):
 |          Look for pkeys in folders on this list, for example ['~/.ssh'].
 |  
 |          Default: ``None`` (disabled)
 |  
 |          .. versionadded:: 0.1.4
 |  
 |      compression (boolean):
 |          Turn on/off transport compression. By default compression is
 |          disabled since it may negatively affect interactive sessions
 |  
 |          Default: ``False``
 |  
 |          .. versionadded:: 0.0.8
 |  
 |      logger (logging.Logger):
 |          logging instance for sshtunnel and paramiko
 |  
 |          Default: :class:`logging.Logger` instance with a single
 |          :class:`logging.StreamHandler` handler and
 |          :const:`DEFAULT_LOGLEVEL` level
 |  
 |          .. versionadded:: 0.0.3
 |  
 |      mute_exceptions (boolean):
 |          Allow silencing :class:`BaseSSHTunnelForwarderError` or
 |          :class:`HandlerSSHTunnelForwarderError` exceptions when enabled
 |  
 |          Default: ``False``
 |  
 |          .. versionadded:: 0.0.8
 |  
 |      set_keepalive (float):
 |          Interval in seconds defining the period in which, if no data
 |          was sent over the connection, a *'keepalive'* packet will be
 |          sent (and ignored by the remote host). This can be useful to
 |          keep connections alive over a NAT. You can set to 0.0 for
 |          disable keepalive.
 |  
 |          Default: 5.0 (no keepalive packets are sent)
 |  
 |          .. versionadded:: 0.0.7
 |  
 |      threaded (boolean):
 |          Allow concurrent connections over a single tunnel
 |  
 |          Default: ``True``
 |  
 |          .. versionadded:: 0.0.3
 |  
 |      ssh_address (str):
 |          Superseded by ``ssh_address_or_host``, tuple of type (str, int)
 |          representing the IP and port of ``REMOTE SERVER``
 |  
 |          .. deprecated:: 0.0.4
 |  
 |      ssh_host (str):
 |          Superseded by ``ssh_address_or_host``, tuple of type
 |          (str, int) representing the IP and port of ``REMOTE SERVER``
 |  
 |          .. deprecated:: 0.0.4
 |  
 |      ssh_private_key (str or paramiko.PKey):
 |          Superseded by ``ssh_pkey``, which can represent either a
 |          **private** key file name (``str``) or a **public** key
 |          (:class:`paramiko.pkey.PKey`)
 |  
 |          .. deprecated:: 0.0.8
 |  
 |      raise_exception_if_any_forwarder_have_a_problem (boolean):
 |          Allow silencing :class:`BaseSSHTunnelForwarderError` or
 |          :class:`HandlerSSHTunnelForwarderError` exceptions when set to
 |          False
 |  
 |          Default: ``True``
 |  
 |          .. versionadded:: 0.0.4
 |  
 |          .. deprecated:: 0.0.8 (use ``mute_exceptions`` instead)
 |  
 |  Attributes:
 |  
 |      tunnel_is_up (dict):
 |          Describe whether or not the other side of the tunnel was reported
 |          to be up (and we must close it) or not (skip shutting down that
 |          tunnel)
 |  
 |          .. note::
 |              This attribute should not be modified
 |  
 |          .. note::
 |              When :attr:`.skip_tunnel_checkup` is disabled or the local bind
 |              is a UNIX socket, the value will always be ``True``
 |  
 |          **Example**::
 |  
 |              {('127.0.0.1', 55550): True,   # this tunnel is up
 |               ('127.0.0.1', 55551): False}  # this one isn't
 |  
 |          where 55550 and 55551 are the local bind ports
 |  
 |      skip_tunnel_checkup (boolean):
 |          Disable tunnel checkup (default for backwards compatibility).
 |  
 |          .. versionadded:: 0.1.0
 |  
 |  Methods defined here:
 |  
 |  __del__(self)
 |  
 |  __enter__(self)
 |  
 |  __exit__(self, *args)
 |  
 |  __init__(self, ssh_address_or_host=None, ssh_config_file='~/.ssh\\config', ssh_host_key=None, ssh_password=None, ssh_pkey=None, ssh_private_key_password=None, ssh_proxy=None, ssh_proxy_enabled=True, ssh_username=None, local_bind_address=None, local_bind_addresses=None, logger=None, mute_exceptions=False, remote_bind_address=None, remote_bind_addresses=None, set_keepalive=5.0, threaded=True, compression=None, allow_agent=True, host_pkey_directories=None, *args, **kwargs)
 |      Initialize self.  See help(type(self)) for accurate signature.
 |  
 |  __repr__(self)
 |      Return repr(self).
 |  
 |  __str__(self)
 |      Return str(self).
 |  
 |  check_tunnels(self)
 |      Check that if all tunnels are established and populates
 |      :attr:`.tunnel_is_up`
 |  
 |  close(self)
 |      Stop the an active tunnel, alias to :meth:`.stop`
 |  
 |  local_is_up(self, target)
 |      Check if a tunnel is up (remote target's host is reachable on TCP
 |      target's port)
 |      
 |      Arguments:
 |          target (tuple):
 |              tuple of type (``str``, ``int``) indicating the listen IP
 |              address and port
 |      Return:
 |          boolean
 |      
 |      .. deprecated:: 0.1.0
 |          Replaced by :meth:`.check_tunnels()` and :attr:`.tunnel_is_up`
 |  
 |  restart(self)
 |      Restart connection to the gateway and tunnels
 |  
 |  start(self)
 |      Start the SSH tunnels
 |  
 |  stop(self, force=False)
 |      Shut the tunnel down. By default we are always waiting until closing
 |      all connections. You can use `force=True` to force close connections
 |      
 |      Keyword Arguments:
 |          force (bool):
 |              Force close current connections
 |      
 |              Default: False
 |      
 |              .. versionadded:: 0.2.2
 |      
 |      .. note:: This **had** to be handled with care before ``0.1.0``:
 |      
 |          - if a port redirection is opened
 |          - the destination is not reachable
 |          - we attempt a connection to that tunnel (``SYN`` is sent and
 |            acknowledged, then a ``FIN`` packet is sent and never
 |            acknowledged... weird)
 |          - we try to shutdown: it will not succeed until ``FIN_WAIT_2`` and
 |            ``CLOSE_WAIT`` time out.
 |      
 |      .. note::
 |          Handle these scenarios with :attr:`.tunnel_is_up`: if False, server
 |          ``shutdown()`` will be skipped on that tunnel
 |  
 |  ----------------------------------------------------------------------
 |  Static methods defined here:
 |  
 |  get_agent_keys(logger=None)
 |      Load public keys from any available SSH agent
 |      
 |      Arguments:
 |          logger (Optional[logging.Logger])
 |      
 |      Return:
 |          list
 |  
 |  get_keys(logger=None, host_pkey_directories=None, allow_agent=False)
 |      Load public keys from any available SSH agent or local
 |      .ssh directory.
 |      
 |      Arguments:
 |          logger (Optional[logging.Logger])
 |      
 |          host_pkey_directories (Optional[list[str]]):
 |              List of local directories where host SSH pkeys in the format
 |              "id_*" are searched. For example, ['~/.ssh']
 |      
 |              .. versionadded:: 0.1.0
 |      
 |          allow_agent (Optional[boolean]):
 |              Whether or not load keys from agent
 |      
 |              Default: False
 |      
 |      Return:
 |          list
 |  
 |  read_private_key_file(pkey_file, pkey_password=None, key_type=None, logger=None)
 |      Get SSH Public key from a private key file, given an optional password
 |      
 |      Arguments:
 |          pkey_file (str):
 |              File containing a private key (RSA, DSS or ECDSA)
 |      Keyword Arguments:
 |          pkey_password (Optional[str]):
 |              Password to decrypt the private key
 |          logger (Optional[logging.Logger])
 |      Return:
 |          paramiko.Pkey
 |  
 |  ----------------------------------------------------------------------
 |  Readonly properties defined here:
 |  
 |  is_active
 |      Return True if the underlying SSH transport is up
 |  
 |  local_bind_address
 |  
 |  local_bind_addresses
 |      Return a list of (IP, port) pairs for the local side of the tunnels
 |  
 |  local_bind_host
 |  
 |  local_bind_hosts
 |      Return a list containing the IP addresses listening for the tunnels
 |  
 |  local_bind_port
 |  
 |  local_bind_ports
 |      Return a list containing the ports of local side of the TCP tunnels
 |  
 |  tunnel_bindings
 |      Return a dictionary containing the active local<>remote tunnel_bindings
 |  
 |  ----------------------------------------------------------------------
 |  Data descriptors defined here:
 |  
 |  __dict__
 |      dictionary for instance variables (if defined)
 |  
 |  __weakref__
 |      list of weak references to the object (if defined)
 |  
 |  ----------------------------------------------------------------------
 |  Data and other attributes defined here:
 |  
 |  daemon_forward_servers = True
 |  
 |  daemon_transport = True
 |  
 |  skip_tunnel_checkup = True


Process finished with exit code 0

```
