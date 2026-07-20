# Stage 6: Dream Vacation App Deployment to AWS EC2


![](</screenshots/stage6img/basket ball player shooting a ball into the net.png>)


## Setting up Networking

**VPC**

![](</screenshots/stage6img/vpc 2.png>)

**Internet Gateway**

![](</screenshots/stage6img/intergnet gateway.png>)



![](</screenshots/stage6img>)




![](</screenshots/stage6img/cd success.png>)


**The App running at port 8080**
![](</screenshots/stage6img/app runs on port 8080.png>)


**I updated the Security Group to allow traffic on port 8080** </br>
At first, when I visited public ip address of the EC2 instance, I got an error. I was confused because I had already run `docker ps` to see if image containers were running, which they were.

I did a little "AI consulting" and that's when I remembered the frontend service is running on port 8080. For that reason, I added port 8080 to the instance's security group.

![](</screenshots/stage6img/security group.png>)

**I also modified the docker-compose file**
![](</screenshots/stage6img/updated the docker compose file.png>)
