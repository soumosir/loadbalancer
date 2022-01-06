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
no caching



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
caching
2 TCP connections

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


# PROXY VS REVERSE PROXY
             
   
   Proxy : server doesnot know who the client is
                              ______
client 1 ---------http:80 ---|      |
                             |      |
client 2 ---------http:80 ---|PROXY |-------http:80-----SERVER
                             |      |
client 3 ---------http:80 ---|______|

firewall
annonimity


  Reverse Proxy: Client doesnot know which server it is connecting to


                             |      |-------http:8080-----SERVER1
                             |      |
client 1 ---------http:80 ---|reverse
                             |PROXY |-------http:8080-----SERVER2
                             |      |
                             |______|-------http:8080-----SERVER3

dont know which service exist in which server
loadbalancing
caching
isolating internal traffic

both at same time  - service mesh

# nginx - one of few things that acts as web server and also as load balancer

