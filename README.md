# stcp2ws

This is a modification based on the good work which is at https://github.com/zanjie1999/tcp-over-websocket. The key functionality is to transfer the tcp over websocket/http. 

We have made the following changes:
(1) Comment out the regular check on the server ip by using the TCPPing feature. 
(2) Add an authorization (bearertoken like) parameter to enhance the security.
(3) Provide the target ip:port on the server so that we can easily navigate the services.

The new command is similar as follows:
For server side:
./stcp2ws server tcp2wsPort yourCustomizedBearerToken

For client side:
./stcp2ws client ws://tcp2wsUrl localPort yourCustomizedBearerToken yourTargetip:portOnServer
