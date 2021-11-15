# Cloud_computing_aws_infrastructure
## Amazon Web Services (AWS)
### AWS Global Infrastructure
#### AWS Regions
#### AWS Availability Zones
#### Advantages of cloud computing and AWS

### Public Cloud - Private cloud and Hybrid cloud use cases
- IaaS - PaaS - SaaS
- Cloud data centres

## AWS Services
- Elastic Compute Service `EC2`
- Simple Storage Service `S3`
- Virtual Private Network `VPC`
- Internet Gateway `IG`
- Route Tables `RT`
- Subnet `sn`
- Network Access Control `NACL`
- Security Groups `SG`
- Cloudwatch `CW`
- Simple Notification Service `SNS`
- Simple Queue Service `SQS`
- Load Balancers `LB` - `ALB` - `ELB` - `NLB`
- Autoscaling Groups `ASG`
- Amazon Machine Image `AMI`
- DynamoDB - MongoDB

### EC2 App instance
- Create and instance using the ubuntu 16.04 AMI
- Configure the app storage and memory
- Create our security group. For the app we want to configure it to be open to port 22, 80 and 3000.
- We now will launch our instance and ssh in on port 22.
- We will now configure our app instance with the commands we had in our provisioning file
```
sudo systemctl restart nginx
sudo systemctl enable nginx
curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
sudo apt-get install python-software-properties
sudo apt-get install -y nodejs
sudo npm install
```
- we now need our app code to be copied into our instance. We do this with the code `scp -i keyfile.pem -r path/file or directory user@ipaddressofinstance:destinationlocation`
- we then replace the default site with the one we had configured before with the commands

```
sudo rm /etc/nginx/sites-available/default
sudo cp path/default /etc/nginx/sites-available
sudo systemctl restart nginx
sudo systemctl enable nginx
```
- we now navigate to the app folder and run npm start and our nginx site should show the app running

### EC2 Db instance
- We follow all of the steps used before for slaunching an instance but change the security groups as we dont need the ports 800 and 300. we only need the port 27017 for the app to be open and the ssh port.
- We then will ssh into our instance and copy in our data to be configured
```
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv D68FA50FEA312927
echo "deb https://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/3.2 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.2.list
sudo apt-get install -y mongodb-org=3.2.20 mongodb-org-server=3.2.20 mongodb-org-shell=3.2.20 mongodb-org-mongos=3.2.20 mongodb-org-tools=3.2.20
sudo rm /etc/mongod.conf
sudo cp /home/vagrant/app/app/mongod.conf /etc/
sudo systemctl restart mongod
sudo systemctl enable mongod

```

### Connecting the app instance to the DB Instance
- ssh back into the app Instance
- Set up the environment variable with the commands
```
sudo echo 'export DB_HOST="mongodb://ipofdb:27017/posts"' >> .bashrc
source ~/.bashrc
path/seed.js
```
- Now if we navigate to the app folder and run the app we should be connected to the DB


### Cloud-watch
- Questions to ask
  - What should we monitor
  - What resources will we monitor
  - How often we monitor
  - What tools are we going to use to perform these tasks
  - Who will perform  
  - Who should be notified  in an emergency if something goes wrong
- 4 Golden signals of monitoring
  - Latency
    - The time it takes to service a request
  - Traffic
    - How much demand is being placed on your system
  - Errors
    - The rate of requests that fail
  - Saturation
    - How full your service is
- Automate the process
  - Application Load Balancers
  - Auto-scaling groups
  - Launch template config - how many instances at all times
  - 2 instances - Min=2 and Max=3
  - policies of scaling out - and scaling in to min=2

- Scaling on demand?
 - Scaling up vs Scaling out
 - Scaling up: increasing the size of your instances
 - Scaling out: increasing the amount of instances in your auto scaling group
![Screenshot_(157)](https://user-images.githubusercontent.com/89383713/141775549-e8e45d87-942a-480f-bc68-49b021e52cce.png)
