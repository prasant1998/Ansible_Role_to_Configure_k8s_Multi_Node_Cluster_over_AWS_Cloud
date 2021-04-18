# Ansible_Role_to_Configure_k8s_Multi_Node_Cluster_over_AWS_Cloud

![1](https://user-images.githubusercontent.com/67523396/115137340-3bb67b80-a043-11eb-9283-b2f8c1cee965.png)

Created a k8s multinode cluster over AWS cloud, then created an Ansible Playbook to launch 3 AWS EC2 Instance, Ansible Playbook to configure Docker over those instances & created Playbook to configure k8s Master, k8s Worker Nodes on the above created EC2 Instances using kubeadm.

Created a WorkSpace for all the practicals to be done which would be more manageable.

![2](https://user-images.githubusercontent.com/67523396/115137361-58eb4a00-a043-11eb-96fb-4dd8593dafdb.jpeg)


STEP 1

Created a Configuration File.

![3](https://user-images.githubusercontent.com/67523396/115137385-6e607400-a043-11eb-9dbb-d821ae3c6f8f.jpeg)
![4](https://user-images.githubusercontent.com/67523396/115137387-6f91a100-a043-11eb-89e1-c2f7f76e973a.jpeg)


Configuration file 

So, I want all the IP to be fetched from AWS.

To achieve that I will be using dynamic inventory instead of static inventory.

To create Dynamic Inventory.

Step 1

Created a directory named dynamic_inventory 

Step 2

As, we all know Ansible is created on top of the python, So it has the capability to fetch IP from the python file.

In that folder, I downloaded and created two files. 

This command will create a ec2.py dynamic inventory file 

wget https://raw.githubusercontent.com/ansible/ansible/stable-2.9/contrib/inventory/ec2.py
This command will create a ec2.ini dynamic inventory file

wget https://raw.githubusercontent.com/ansible/ansible/stable-2.9/contrib/inventory/ec2.ini
Two executable file in my dynamic inventory folder.

![5](https://user-images.githubusercontent.com/67523396/115137405-8e903300-a043-11eb-91cb-7bec58d7e755.jpeg)


Step 3

We need to make those two files executable.

chmod +x ec2.py 

chmod +x ec2.ini

Step 4

In ec2.py file, I added an interpreter name on the top , and then commented the 172th line.

![6](https://user-images.githubusercontent.com/67523396/115137414-a2d43000-a043-11eb-818d-b3106f2db105.jpeg)
![7](https://user-images.githubusercontent.com/67523396/115137417-a4055d00-a043-11eb-9d66-f2a3b2788657.jpeg)


Changed in ec2.py file

In ec2.ini file, I added aws_access_key_id and aws_ secret_access_key and the region.

![8](https://user-images.githubusercontent.com/67523396/115137433-b8e1f080-a043-11eb-8096-4bd1410955a6.jpeg)
![9](https://user-images.githubusercontent.com/67523396/115137436-ba131d80-a043-11eb-9e2d-c15a6c1fb7dc.jpeg)


Changed in ec2.ini file

Step5 

Now I provided environmental Variables.

export AWS_ACCESS_KEY_ID=<Your_access_key>

export AWS_SECRET_ACCESS_KEY=<Your_secret_access_key> 

export AWS_REGION=<Your_AWS_region>


STEP 6 

Looking for the all the hosts on AWS Cloud .

![10](https://user-images.githubusercontent.com/67523396/115137450-c9926680-a043-11eb-942d-589f15a44769.jpeg)


IP of all the ec2 instance in my ap-south-1 region.

STEP7

Used ping module in adhoc command to check the connectivity.

![11](https://user-images.githubusercontent.com/67523396/115137456-d747ec00-a043-11eb-8c02-3af2755d753d.jpeg)
![12](https://user-images.githubusercontent.com/67523396/115137457-d7e08280-a043-11eb-9c99-980808356c6e.jpeg)


Checking the connectivity.Now, We are done with dynamic inventory. 

STEP 3

Created the role for multiple used cases step by step as following 

For Launching a VPC, Igw, Subnet, Route table 

Tasks file 

![13](https://user-images.githubusercontent.com/67523396/115137463-e890f880-a043-11eb-9adc-ffcbf05e261e.jpeg)
![14](https://user-images.githubusercontent.com/67523396/115137464-e9c22580-a043-11eb-80dd-f8ad7bf22fe9.jpeg)


Task in Tasks file.

Vars file

![15](https://user-images.githubusercontent.com/67523396/115137478-fb0b3200-a043-11eb-8eee-a4abd18d6fed.jpeg)


All variables in Vars file.Provisioning an AWS ec2 Instance in that VPC.


Tasks file

![16](https://user-images.githubusercontent.com/67523396/115137491-0a8a7b00-a044-11eb-9e94-ab40fe2ef79f.jpeg)


Provisioning AWS ec2 instance

Vars file 

![17](https://user-images.githubusercontent.com/67523396/115137493-124a1f80-a044-11eb-8996-d0035927f89d.jpeg)


Variables used in Tasks file .

Configuring Kubernetes master on one of the instance.

![18](https://user-images.githubusercontent.com/67523396/115137499-27bf4980-a044-11eb-9074-f71f81d071a3.jpeg)


Configuring k8s Master.Configuring Kubernetes slave on two of the instance. 

![19](https://user-images.githubusercontent.com/67523396/115137509-39085600-a044-11eb-9ad6-68ba690c441e.jpeg)
![20](https://user-images.githubusercontent.com/67523396/115137512-3b6ab000-a044-11eb-958c-6a785ba776b2.jpeg)


Configuring k8s Slave.Launching Wordpress & mysql server on kubernetes slave by using k8s Master node.

![21](https://user-images.githubusercontent.com/67523396/115137534-55a48e00-a044-11eb-97e7-abf3bdbab415.jpeg)


Wordpress and mysql server.

STEP 4


Playbook to launch a VPC on AWS and ec2 instance simultaneously.

![22](https://user-images.githubusercontent.com/67523396/115137558-67863100-a044-11eb-9e0e-3fb566ebcfd8.jpeg)

Playbook for VPC and ec2-instance.

![23](https://user-images.githubusercontent.com/67523396/115137566-7240c600-a044-11eb-99cd-8a4687a6bb64.jpeg)

Running Playbook

STEP 5


Playbook to launch kubernetes cluster ie, master-slave model and then launch Wordpress & mysql simultaneously.

![24](https://user-images.githubusercontent.com/67523396/115137578-85ec2c80-a044-11eb-8190-aa295e1e16fd.jpeg)

Playbook for kuberntes Cluster, WordPress and mysql.

![25](https://user-images.githubusercontent.com/67523396/115137664-0743bf00-a045-11eb-92a9-e51c86e47355.jpeg)
![26](https://user-images.githubusercontent.com/67523396/115137665-0874ec00-a045-11eb-88e6-bf13f60c8701.jpeg)
![27](https://user-images.githubusercontent.com/67523396/115137666-090d8280-a045-11eb-8e60-530f31b1d7c9.jpeg)
![28](https://user-images.githubusercontent.com/67523396/115137667-090d8280-a045-11eb-84a8-0ea1a300f771.jpeg)
![29](https://user-images.githubusercontent.com/67523396/115137668-09a61900-a045-11eb-9e71-1547493f6428.jpeg)
![30](https://user-images.githubusercontent.com/67523396/115137669-0a3eaf80-a045-11eb-9b48-bc41437cbc10.jpeg)


Running the Playbook.


VPC, Subnet, Internet Gateway, Route Table and ec2 instance now has been successfully created. 

![31](https://user-images.githubusercontent.com/67523396/115137702-365a3080-a045-11eb-944e-1e62f1d615d4.jpeg)
![32](https://user-images.githubusercontent.com/67523396/115137705-3823f400-a045-11eb-8964-27eab933d134.jpeg)
![33](https://user-images.githubusercontent.com/67523396/115137706-3823f400-a045-11eb-91d3-5aa38a2a7913.jpeg)
![34](https://user-images.githubusercontent.com/67523396/115137707-38bc8a80-a045-11eb-9887-0db5e0250bb4.jpeg)
![35](https://user-images.githubusercontent.com/67523396/115137708-39552100-a045-11eb-884a-be151f780e6c.jpeg)


VPC, Subnet, Internet Gateway, Route Table and ec2 instance

STEP 6 

Next, I have updated the details WordPress required and then providing MySQL details, It brings to the end of my practical and leads to the great used cases to be solved.

![36](https://user-images.githubusercontent.com/67523396/115137726-4d991e00-a045-11eb-9716-4c97e8c75364.jpeg)


Wordpress server 

Open for any Queries and suggestions. 

THANKYOU
