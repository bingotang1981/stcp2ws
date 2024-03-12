# stcp2ws

## Introduction

This is a modification based on the good work which is at https://github.com/zanjie1999/tcp-over-websocket. The key functionality is to transfer the tcp over websocket/http.

We have made the following changes:
(1) Comment out the regular check on the server ip by using the TCPPing feature.
(2) Add an authorization (bearertoken like) parameter to enhance the security.
(3) Provide the target ip:port on the server so that we can easily navigate the services.

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

(2) Suppose you have a host with ip as 192.168.1.15 with only http/https (80/443) port open, but you want to access your host via ssh (with port as 22). Besides you want to make the access on the internet, so that you use Cloudflare as the CDN, we suppose the domain name is aa.yourdomain.com.

On server side:
`./stcp2ws server 80 yourCustomizedBearerToken`

On Client side:
`./stcp2ws client https://aa.yourdomain.com 1022 yourCustomizedBearerToken 127.0.0.1:22`

Then you can access the host via ssh 127.0.0.1:1022

(3) Suppose you have a host with ip as 192.168.5.15 and it has an HTTP server at port 8080. You want to access the HTTP server via internet, but you do not want to expose it to internet directly with some reason (e.g. the HTTP server is not very robust). To do this, you use Cloudflare as the CDN which points to our stcp2ws server instead of the original http server, we suppose the domain name is aa.yourdomain.com.

On server side:
`./stcp2ws server 80 yourCustomizedBearerToken`

On Client side:
`./stcp2ws client https://aa.yourdomain.com 8088 yourCustomizedBearerToken 127.0.0.1:8080`

Then you can access the website which is at http://127.0.0.1:8088 by using a browswer.

