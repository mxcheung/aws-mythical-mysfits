# aws-mythical-mysfits
aws-mythical-mysfits

# Objectives
-  AWS Modern application workshop
- Familiarise with AWS Service
- AWS clouds 9
- AWS S3 


# Module 1
https://github.com/aws-samples/aws-modern-application-workshop/tree/java/module-1

aws-cloud9-MythicalMysfitsIDE-cheungm-f5c898c6e65144dd843332c102e79e82

aws s3 mb s3://cheungm-bucket-name-20201227

aws s3 ls cheungm-bucket-name-20201227

```
cheungm:~/environment/aws-modern-application-workshop (java) $ aws s3 ls  
2020-12-25 00:20:57 cf-templates-nkcon4duurt8-ap-southeast-2
2020-12-26 21:28:00 cheungm-bucket-name-20201227
2020-12-23 22:47:51 cheungms3
```


aws s3 cp ~/environment/aws-modern-application-workshop/module-1/web/index.html s3://cheungm-bucket-name-20201227/index.html

```
cheungm:~/environment/aws-modern-application-workshop (java) $ curl -I "https://cheungm-bucket-name-20201227.s3-$(aws configure get region).amazonaws.com/index.html"
HTTP/1.1 403 Forbidden
```

website-bucket-policy
```
{
  "Id": "MyPolicy",
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadForGetBucketObjects",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::cheungm-bucket-name-20201227/*"
    }
  ]
}
```

Apply S3 Bucket Policy
```
aws s3api delete-bucket-policy --bucket cheungm-bucket-name-20201227
aws s3api put-bucket-policy --bucket cheungm-bucket-name-20201227 --policy file://~/environment/aws-modern-application-workshop/module-1/aws-cli/website-bucket-policy.json
cheungm:~/environment/aws-modern-application-workshop (java) $ curl -I "https://cheungm-bucket-name-20201227.s3-$(aws configure get region).amazonaws.com/index.html"
HTTP/1.1 200 OK
aws s3 website s3://cheungm-bucket-name-20201227 --index-document index.html
```

http://cheungm-bucket-name-20201227.s3-website-ap-southeast-2.amazonaws.com/


# Module 2
Creating a Service with AWS Fargate
https://github.com/aws-samples/aws-modern-application-workshop/tree/java/module-2

```
aws cloudformation create-stack --stack-name MythicalMysfitsCoreStack --capabilities CAPABILITY_NAMED_IAM --template-body file://~/environment/aws-modern-application-workshop/module-2/cfn/core.yml   

{
    "StackId": "arn:aws:cloudformation:ap-southeast-2:918300033687:stack/MythicalMysfitsCoreStack/9a0ec240-47c5-11eb-85da-02e88e2d9048"
}

cheungm:~/environment/aws-modern-application-workshop (java) $ aws cloudformation wait stack-create-complete --stack-name MythicalMysfitsCoreStack && echo "stack created"
stack created

```


Install Maven and build
```
sudo yum -y install java-1.8.0-openjdk-devel
sudo alternatives --set java /usr/lib/jvm/java-1.8.0-openjdk.x86_64/bin/java
sudo alternatives --set javac /usr/lib/jvm/java-1.8.0-openjdk.x86_64/bin/javac
sudo wget http://repos.fedorapeople.org/repos/dchen/apache-maven/epel-apache-maven.repo -O /etc/yum.repos.d/epel-apache-maven.repo
sudo sed -i s/\$releasever/6/g /etc/yum.repos.d/epel-apache-maven.repo
sudo yum install -y apache-maven
cd ~/environment/aws-modern-application-workshop/module-2/app/service
mvn clean install

cheungm:~/environment/aws-modern-application-workshop (java) $ sudo alternatives --set java /usr/lib/jvm/java-1.8.0-openjdk.x86_64/bin/java
/usr/lib/jvm/java-1.8.0-openjdk.x86_64/bin/java has not been configured as an alternative for java
cheungm:~/environment/aws-modern-application-workshop (java) $ sudo alternatives --set javac /usr/lib/jvm/java-1.8.0-openjdk.x86_64/bin/javac
Installed:
  apache-maven.noarch 0:3.5.2-1.el6          
  

[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 02:34 min
[INFO] Finished at: 2020-12-26T22:15:33Z
[INFO] Final Memory: 31M/90M
[INFO] ------------------------------------------------------------------------
cheungm:~/environment/aws-modern-application-workshop/module-2/app/service (java) $
```

## Links
https://github.com/aws-samples/aws-modern-application-workshop
