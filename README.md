<div align="center">
    <h1>haproxy-windows haproxy-3.1-dev1</h1>
    <img src="./HAProxyCE.png" width="250" />
    <p align="center">
        HAProxy Community Edition For Windows. 
    </p>    
</div>

<br>

**HAProxy** The Reliable, High Performance TCP/HTTP Load Balancer

HAProxy is a free, very fast and reliable solution offering [high availability](http://en.wikipedia.org/wiki/High_availability),
 [load balancing](http://en.wikipedia.org/wiki/Load_balancer), and proxying for TCP and HTTP-based applications. 
 
 It is particularly suited for very high traffic web sites and powers quite a number of the world's most visited ones. 
 Over the years it has become the de-facto standard opensource load balancer, is now shipped with most mainstream 
 Linux distributions, and is often deployed by default in cloud platforms.

#### QuickStart(use quiet mode)

    haproxy.exe -f haproxy.config.json -q -D
    or
    haproxy.exe -f haproxy.config.cfg -q -D

```
Usage : haproxy [-f <cfgfile>]* [ -vdVD ] [ -n <maxconn> ] [ -N <maxpconn> ]
        [ -p <pidfile> ] [ -m <max megs> ] [ -C <dir> ] [-- <cfgfile>*]
        -v displays version ; -vv shows known build options.
        -d enters debug mode ; -db only disables background mode.
        -dM[<byte>] poisons memory with <byte> (defaults to 0x50)
        -V enters verbose mode (disables quiet mode)
        -D goes daemon ; -C changes to <dir> before loading files.
        -q quiet mode : don't display messages
        -c check mode : only check config files and exit
        -n sets the maximum total # of connections (2000)
        -m limits the usable amount of memory (in MB)
        -N sets the default, per-proxy maximum # of connections (2000)
        -L set local peer name (default to hostname)
        -p writes pids of all children to this file
        -de disables epoll() usage even when available
        -dp disables poll() usage even when available
        -dS disables splice usage (broken on old kernels)
        -dV disables SSL verify on servers side
        -sf/-st [pid ]* finishes/terminates old pids.
```


## How to build

Compilation and installation of HAProxy in Windows

1. Download and Install cygwin (<https://cygwin.com/setup-x86_64.exe>), after installation, copy `setup-x86_64.exe` to `c:\cygwin64`
2. On terminal (cmd), install gcc, make and libs
    ```
    cd c:\cygwin64
    setup-x86_64.exe -q -P wget -P gcc-g++ -P make -P libssl-devel -P zlib-devel -P ldd
    ```
3. Open the cygwin64 terminal
    ```
    Cygwin.bat
    ```
4. Download source code (inside cygwin64 terminal)
    ```
    wget https://www.haproxy.org/download/3.1/src/devel/haproxy-3.1-dev1.tar.gz
    tar xzvf haproxy-3.1-dev1.tar.gz
    rm -rf ./haproxy-3.1-dev1.tar.gz 
    cd haproxy-3.1-dev1
    ```
5. Build (inside cygwin64 terminal)
    ```
    make TARGET=cygwin USE_OPENSSL=3.0 USE_ZLIB=1.3.1
    ```
6. Copy `haproxy.exe` and all dependencies (.dll)
    ```
    # show dependencies
    dependencies=$(ldd ./haproxy.exe | awk '{print $3}')
   
    # create a folder in C:/
    mkdir -p /cygdrive/c/haproxy-3.1
   
    # copy all
    cp ./haproxy.exe /cygdrive/c/haproxy-3.1
    for dep in $dependencies; do
        cp "$dep" /cygdrive/c/haproxy-3.1
    done
   
    # delete trash
    cd ..
    rm -rf ./haproxy-3.1-dev1
   
    # exit cygwin64 terminal
    exit
    ```
7. On cmd, you can test haproxy
    ```
    cd C:\haproxy-3.1
    haproxy.exe -v
    ```
