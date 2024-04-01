# stcp2ws (TCP Over HTTP/WebSocket)

## Introduction

This is a modification based on the good work which is at https://github.com/zanjie1999/tcp-over-websocket. The key functionality is to transfer the tcp over websocket/http.

We have made the following changes:

1. Make the original solution more stable, e.g. modify the reconnection mechanism, handle the concurrent access to the map, remove the TCPPing feature, etc.
2. Add an authorization (bearertoken like) parameter to enhance the security.
3. Provide the target ip:port on the server so that we can easily navigate the services.
4. Listen at multpile ports on the client side so that it can connect to multiple servers.

## Command

The new command is similar as follows:

For server side:
`./stcp2ws server tcp2wsPort yourCustomizedBearerToken`

For client side:
`./stcp2ws client http://tcp2wsUrl localPort yourCustomizedBearerToken yourTargetip:portOnServer`

## Sample Scenarios

Let's provide some sample scenarios:

(1) Suppose you have a host with ip as 192.168.1.15 with only http/https (80/443) port open, but you want to access your host via ssh (with port as 22).

On server side:
`./stcp2ws server 80 yourCustomizedBearerToken`

On Client side:
`./stcp2ws client http://192.168.1.15:80 1022 yourCustomizedBearerToken 127.0.0.1:22`

Then you can access the host via ssh 127.0.0.1:1022

(2) Suppose you have a host with ip as 192.168.1.15 with only http/https (80/443) port open, but you want to access your host via ssh (with port as 22). Besides, you have another hosts with ip as 192.168.2.15 with http/https (80/443) port open too.

On server side (192.168.1.15):
`./stcp2ws server 80 yourCustomizedBearerToken1`

On server side (192.168.2.15):
`./stcp2ws server 80 yourCustomizedBearerToken2`

On Client side:
`./stcp2ws client http://192.168.1.15:80 1022 yourCustomizedBearerToken1 127.0.0.1:22 http://192.168.2.15:80 1023 yourCustomizedBearerToken2 127.0.0.1:22`

Then you can access the host1 192.168.1.15 via ssh 127.0.0.1:1022 and host2 192.168.2.15 via ssh 127.0.0.1:1023

(3) Suppose you have a host with ip as 192.168.1.15 with only http/https (80/443) port open, but you want to access your host via ssh (with port as 22). Besides you want to make the access on the internet, so that you use Cloudflare as the CDN, we suppose the domain name is aa.yourdomain.com.

On server side:
`./stcp2ws server 80 yourCustomizedBearerToken`

On Client side:
`./stcp2ws client https://aa.yourdomain.com 1022 yourCustomizedBearerToken 127.0.0.1:22`

Then you can access the host via ssh 127.0.0.1:1022

(4) Suppose you have a host with ip as 192.168.5.15 and it has an HTTP server at port 8080. You want to access the HTTP server via internet, but you do not want to expose it to internet directly with some reason (e.g. the HTTP server is not very robust). To do this, you use Cloudflare as the CDN which points to our stcp2ws server instead of the original http server, we suppose the domain name is aa.yourdomain.com.

On server side:
`./stcp2ws server 80 yourCustomizedBearerToken`

On Client side:
`./stcp2ws client https://aa.yourdomain.com 8088 yourCustomizedBearerToken 127.0.0.1:8080`

Then you can access the website which is at http://127.0.0.1:8088 by using a browswer.
