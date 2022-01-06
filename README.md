# loadbalancer HA Proxy
Load Balancer concepts

Two type of load balancers

- layer 4
based on ip
cannot see packet
single TCP connection
Expensive
dedicated network device 
Not round robit in some cases



implementation

PORT=4444 node index.js

PORT=5555 node index.js

Load balancer listening on port something

brew install haproxy

tcp.cfg for layer 4 loadbalancing

haproxy -f tcp.cfg

# to check for correct config
haproxy -c -f tcp.cfg

mode tcp is the key there

localhost:8888 --> localhost:4444 or localhost:5555
url doesnt change


- layer 7
software load balancer
can see packet and then decide where to go
round robbin mostly

acl app1 path_end -i /app1
acl app2 path_end -i /app2
use_backend app1servers if app1
use_backend app2servers if app2

it means that go to app1 if <url>/app1
and app1 to use app1servers

example
backend app1servers
    mode http
    server app1server1 127.0.0.1:4444 check
    server app1server2 127.0.0.1:6666 check


haproxy -f http.conf

localhost:9999/app1

ges to either 4444 or 6666 rounf robit

same for /app2



# End of HA PROXY
