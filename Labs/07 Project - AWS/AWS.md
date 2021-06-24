## <font color='red'> 4.1 AWS Playbooks </font>


Pre-requisites:
* Obtain SSH credentials from the AWS Console
* Install AWS CLI 2
* Set Credentials


#### <font color='red'> Obtain SSH credentials from AWS Console </font>



#### <font color='red'> Install & Configure AWS CLI version 2 </font>

```
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
```

```
$ aws configure
```

---

#### <font color='red'> 1_lauch_ec2 </font>

* Take a look at the Ansible AWS EC2 modules.
* Image is the AMI ref
* you will need to install boto boto3

View examples of lunching EC2 instances:

 > List of Ansible AWS modules: https://docs.ansible.com/ansible/2.9/modules/list_of_cloud_modules.html#amazon

install boto for just ansadmin user:
```
ansadmin $ pip install boto boto3 --user
```