Hi Abel, here's how to get a tunnel into your AWS instance :D

can access the browser UI for Kubernetes now!
Need to setup an SSH tunnel in PuTTY
goto connection->SSH->Tunnels
in 'add a new forwarded port:' set source port, select dynamic and click add.
ATTENTION:
Don't have the PuTTY configuration panel open! it disables the tunnel D:

Then in browser:
For Firefox:
goto Settings->Advanced->Network->Connection
click settings and set 'Manual proxy configuration:'
Enter 'SOCKS Host:' as localhost on the port you configured PuTTY as.
(Might need to remove line about 'localhost' in 'No Proxy for' box)

Try searching for 'ip' in browser to check if the ip is correct,
should be the same as the one for the AWS instance.

If you've setup the Kubernetes Dashboard UI you can access it at:
127.0.0.1:(port set in UI setup)
