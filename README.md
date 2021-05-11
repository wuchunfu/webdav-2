# webdav
small webdav server written in go. meant for network attached storage. just over 50 lines of code.

webdav is a technology related to http. Got tired of SMB / NFS compatibility issues and decided to give webdav a try. This is a pretty vanilla recipe, nothing mind blowing here.

I'd like to add user support but I dont need it at the moment since this all lives on a private network. Another thing to note: this code creates a memory lock to prevent it from paging. sometimes on sigint it'll trigger a wait time that can last a few minutes. you can safely kill it with 9.

```
rxlx ~ $ webdav -h
Usage of webdav:
  -d string
    	Directory to serve from. Default is CWD (default "./")
  -l string
    	path/file to log to (default "./webdav.log")
  -p int
    	Port to serve on (Plain HTTP) (default 8081)
  -ps int
    	Port to serve TLS on (default 8443)
  -s	Serve HTTPS. Default false
  
  nohup /home/rxlx/bin/webdav -s -d /Bstor/ -ps 8081 -l /home/rxlx/bin/logs/$(date | tr ' ' '-').log &
  
 **NOTE** you'll need a cert.pem and key.pem for tls to work
```

For some reason (as of big sur 11.3.1) macOS fails to negotiate for cipher suites and the handshake will always fail. It's possible my implementation is to blame *but*, it works on windows, android, ubuntu 20+, and rhel 8. So you will have to use plain http. **Additionally** on mac, I'm tracing down a an error where finder locks up trying to write to the remote location ( which is a stripped down webdav server running on EL8 ) dot_clean times out
