- tool for [[TCP (transmission control protocol)]]/[[UDP (User Datagram Protocol)]] tunneling over [[HTTP (Hypertext Transfer Protocol)]]. Can be used to pivot in an Active Directory environment. Can also to bypass firewalls (inbound rules?)
- can create a `reverse` or `forward` SOCKS5 proxy over an HTTP connection.
- can be used with proxychains to use tools like [[nmap (network mapper)]] to scan an internal network.

- server: `chisel -p 8080 --reverse` -> accept client connections to establish the tunnel.
- client: `chisel <vps/attacker ip>:8080 R:localhost:80` -> connect to the chisel server and Reverse Port Forward 