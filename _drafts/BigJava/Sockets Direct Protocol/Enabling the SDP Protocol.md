
# Debugging SDP

You can enable debugging messages by using the `-Dcom.sun.sdp.debug` flag. If you specify a file, the messages will be output to that file. Otherwise, the messages will be printed to standard output.

This first example shows sample messages printed to standard output:

```

% java -Dcom.sun.sdp.conf=sdp.conf **-Dcom.sun.sdp.debug** **ExampleApplicaton**
BIND to 192.0.2.1:5000 (socket converted to SDP protocol)
CONNECT to 129.156.232.160:80 (no match)
CONNECT to 192.0.2.2:80 (socket converted to SDP protocol)

```

This second example shows the output redirected to a file named `debug.log`:

```

% java -Dcom.sun.sdp.conf=sdp.conf **-Dcom.sun.sdp.debug=debug.log** **ExampleApplication** 
[1] 27310
% tail -f debug.log
BIND to 192.0.2.1:5000 (socket converted to SDP protocol)

```
