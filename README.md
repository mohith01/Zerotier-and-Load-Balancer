# Implementation of Highly Available Load Balance cluster using Nginx and Docker

## Abstract
In this digital world, there are high traffic websites which needs to serve millions of
clients and retrieve desired data from the servers in fast and reliable manner.
A load balancer acts as an interface between servers and client requests, it will
route the client requests across all servers by which we can fulfill those requests in
a way that maximizes speed, proper utilization and ensures that no server is
overloaded.
The load balancer uses NGINX which provides different load balancing mechanisms
or methods to effectively distribute traffic to several application servers and to
improve performance, scalability and reliability of web applications.
We will be hosting an small web application on multiple separate http servers and
demonstrate on how load balancer will take care of routing client requests to
servers which are running and how it achieves proper utilization even if one or more
servers goes down due to huge traffic/shutdown or after performing a DDos attack
to flood the bandwidth or resources to a targeted web server.

## Screenshots with Explanation

First the nginx is configured

![image](https://user-images.githubusercontent.com/42903811/141833974-361e582c-7e68-425b-92d3-3e9468fbd667.png)

The server ip address are configured in a round robin fashion however we added
max fails and fail timeout so if any server fails at some point it will reload
automatically to the next server


All three http servers are hosting website in different location and these are 
connected to a single network using zerotier one. The network is connected to another 
computer which acts as a load balancer.

![image](https://user-images.githubusercontent.com/42903811/141834330-b425a9bf-b708-41c9-96f1-b2fce48c4ac3.png)

Output of the frontend website used for load balancing:

![image](https://user-images.githubusercontent.com/42903811/141834435-d28d66f6-872b-464a-bed6-d8937e2de7a2.png)

Every time the page refreshes, Load balancer redirect automatically to the different
web page in round robin fashion.

Now we test how load balancer works and how it re-routes the traffic

#### Performing DDos attack on http://172.23.198.96:8080/ 
##### Before Performing DDoS attack
The number of requests from zerotier network is less

![image](https://user-images.githubusercontent.com/42903811/141834767-bfa074ab-ef9e-4777-89b0-43f91a3a2c7b.png)
![image](https://user-images.githubusercontent.com/42903811/141834869-267f8c7a-77c4-4886-a92a-54f366bdbebf.png)

##### After Performing DDoS attack for sometime
The number of requests to that particular server is more

![image](https://user-images.githubusercontent.com/42903811/141835041-6900ba1a-6c09-42bf-b071-eb7dc76204b4.png)

It jumps to 1820 visits in few seconds and then takes a lot of time to refresh due to DDos attack

Since we are using load balancer, it will route the client requests to the server which
are getting less number of requests to achieve proper utilization.
In this project, since we are performing DDoS attack on http server(mohan), it will start
routing only to http server(gokul) and http server(Dishanth) webpages ,because it’s being
spammed with requests in http server(mohan) and reload time configured in nginx would have 
exceeded for the attacked server.

![image](https://user-images.githubusercontent.com/42903811/141835430-2b622378-3085-4dd8-95e3-2d9e5442a7b5.png)

