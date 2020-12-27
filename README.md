## aws-mythical-mysfits
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
## Docker

```
cheungm:~/environment/aws-modern-application-workshop/module-2/app/service (java) $ aws sts get-caller-identity
cd ..
docker build . -t 918300033687.dkr.ecr.ap-southeast-2.amazonaws.com/mythicalmysfits/service:latest
docker run -p 8080:8080 918300033687.dkr.ecr.ap-southeast-2.amazonaws.com/mythicalmysfits/service:latest
{
    "UserId": "AIDA5LTXSQKL4OFK3G32D",
    "Account": "918300033687",
    "Arn": "arn:aws:iam::918300033687:user/cheungm"
}

Successfully built 0f554770e7d4
Successfully tagged 918300033687.dkr.ecr.ap-southeast-2.amazonaws.com/mythicalmysfits/service:latest

2020-12-26 22:19:52.663  INFO 1 --- [           main] o.s.j.e.a.AnnotationMBeanExporter        : Registering beans for JMX exposure on startup
2020-12-26 22:19:52.759  INFO 1 --- [           main] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat started on port(s): 8080 (http) with context path ''
2020-12-26 22:19:52.766  INFO 1 --- [           main] com.example.MythicalMysfitsApplication   : Started MythicalMysfitsApplication in 6.669 seconds (JVM running for 7.984)

https://f5c898c6e65144dd843332c102e79e82.vfs.cloud9.ap-southeast-2.amazonaws.com/mysfits
```


## ECR

```

cheungm:~/environment/aws-modern-application-workshop/module-2/app (java) $ aws ecr create-repository --repository-name mythicalmysfits/service
{
    "repository": {
        "repositoryArn": "arn:aws:ecr:ap-southeast-2:918300033687:repository/mythicalmysfits/service",
        "registryId": "918300033687",
        "repositoryName": "mythicalmysfits/service",
        "repositoryUri": "918300033687.dkr.ecr.ap-southeast-2.amazonaws.com/mythicalmysfits/service",
        "createdAt": 1609021411.0,
        "imageTagMutability": "MUTABLE",
        "imageScanningConfiguration": {
            "scanOnPush": false
        },
        "encryptionConfiguration": {
            "encryptionType": "AES256"
        }
    }
}

```
$(aws ecr get-login --no-include-email)
```
cheungm:~/environment/aws-modern-application-workshop/module-2/app (java) $ $(aws ecr get-login --no-include-email)
WARNING! Using --password via the CLI is insecure. Use --password-stdin.
WARNING! Your password will be stored unencrypted in /home/ec2-user/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credentials-store

Login Succeeded

```


docker push 918300033687.dkr.ecr.ap-southeast-2.amazonaws.com/mythicalmysfits/service:latest
aws ecr describe-images --repository-name mythicalmysfits/service

```
cheungm:~/environment/aws-modern-application-workshop/module-2/app (java) $ docker push 918300033687.dkr.ecr.ap-southeast-2.amazonaws.com/mythicalmysfits/service:latest
The push refers to repository [918300033687.dkr.ecr.ap-southeast-2.amazonaws.com/mythicalmysfits/service]



cheungm:~/environment/aws-modern-application-workshop/module-2/app (java) $ aws ecr describe-images --repository-name mythicalmysfits/service
{
    "imageDetails": [
        {
            "registryId": "918300033687",
            "repositoryName": "mythicalmysfits/service",
            "imageDigest": "sha256:3feaf9e7336b57f60e4861a0a3de244f7090a8a97a5233d9f66d5b0a720526a9",
            "imageTags": [
                "latest"
            ],
            "imageSizeInBytes": 276280045,
            "imagePushedAt": 1609021761.0,
            "imageManifestMediaType": "application/vnd.docker.distribution.manifest.v2+json",
            "artifactMediaType": "application/vnd.docker.container.image.v1+json"
        }
    ]
}
```
aws ecs create-cluster --cluster-name MythicalMysfits-Cluster
```
aws logs create-log-group --log-group-name mythicalmysfits-logs
aws ecs register-task-definition --cli-input-json file://~/environment/aws-modern-application-workshop/module-2/aws-cli/task-definition.json
aws elbv2 create-load-balancer --name mysfits-nlb --scheme internet-facing --type network --subnets subnet-076b3f6261201f184  subnet-0f9781c1ea505e25c  > ~/environment/nlb-output.json
aws elbv2 create-target-group --name MythicalMysfits-TargetGroup --port 8080 --protocol TCP --target-type ip --vpc-id vpc-0d482f57e3ef741d5 --health-check-interval-seconds 10 --health-check-path / --health-check-protocol HTTP --healthy-threshold-count 3 --unhealthy-threshold-count 3 > opn
aws elbv2 create-listener --default-actions TargetGroupArn=arn:aws:elasticloadbalancing:ap-southeast-2:918300033687:targetgroup/MythicalMysfits-TargetGroup/7a0bbdb0201c083e,Type=forward --load-balancer-arn arn:aws:elasticloadbalancing:ap-southeast-2:918300033687:loadbalancer/net/mysfits-nlb/7cfb1105f45d2351 --port 80 --protocol TCP

```

### Test the service

http://mysfits-nlb-7cfb1105f45d2351.elb.ap-southeast-2.amazonaws.com/mysfits

```
{"mysfits":[{"mysfitId":"4e53920c-505a-4a90-a694-b9300791f0ae","name":"Evangeline","species":"Chimera","age":43,"description":"Evangeline is the global sophisticate of the mythical world. You’d be hard pressed to find a more seductive, charming, and mysterious companion with a love for neoclassical architecture, and a degree in medieval studies. Don’t let her beauty and brains distract you. While her mane may always be perfectly coifed, her tail is ever-coiled and ready to strike. Careful not to let your guard down, or you may just find yourself spiraling into a dazzling downfall of dizzying dimensions.","goodevil":"Evil","lawchaos":"Lawful","thumbImageUri":"https://www.mythicalmysfits.com/images/chimera_thumb.png","profileImageUri":"https://www.mythicalmysfits.com/images/chimera_hover.png"},{"mysfitId":"2b473002-36f8-4b87-954e-9a377e0ccbec","name":"Pauly","species":"Cyclops","age":2,"description":"Naturally needy and tyrannically temperamental, Pauly the infant cyclops is searching for a parental figure to call friend. Like raising any precocious tot, there may be occasional tantrums of thunder, lightning, and 100 decibel shrieking. Sooth him with some Mandrake root and you’ll soon wonder why people even bother having human children. Gaze into his precious eye and fall in love with this adorable tyke.","goodevil":"Neutral","lawchaos":"Lawful","thumbImageUri":"https://www.mythicalmysfits.com/images/cyclops_thumb.png","profileImageUri":"https://www.mythicalmysfits.com/images/cyclops_hover.png"},{"mysfitId":"0e37d916-f960-4772-a25a-01b762b5c1bd","name":"CoCo","species":"Dragon","age":501,"description":"CoCo wears sunglasses at night. His hobbies include dressing up for casual nights out, accumulating debt, and taking his friends on his back for a terrifying ride through the mesosphere after a long night of revelry, where you pick up the bill, of course. For all his swagger, CoCo has a heart of gold. His loyalty knows no bounds, and once bonded, you’ve got a wingman (literally) for life.","goodevil":"Good","lawchaos":"Chaotic","thumbImageUri":"https://www.mythicalmysfits.com/images/dragon_thumb.png","profileImageUri":"https://www.mythicalmysfits.com/images/dragon_hover.png"},{"mysfitId":"da5303ae-5aba-495c-b5d6-eb5c4a66b941","name":"Gretta","species":"Gorgon","age":31,"description":"Young, fun, and perfectly mischievous, Gorgon is mostly tail. She's currently growing her horns and hoping for wings like those of her high-flying counterparts. In the meantime, she dons an umbrella and waits for gusts of wind to transport her across space-time. She likes to tell jokes in fluent Parseltongue, read the evening news, and shoot fireworks across celestial lines. If you like high-risk, high-reward challenges, Gorgon will be the best pet you never knew you wanted.","goodevil":"Evil","lawchaos":"Neutral","thumbImageUri":"https://www.mythicalmysfits.com/images/gorgon_thumb.png","profileImageUri":"https://www.mythicalmysfits.com/images/gorgon_hover.png"},{"mysfitId":"b41ff031-141e-4a8d-bb56-158a22bea0b3","name":"Snowflake","species":"Yeti","age":13,"description":"While Snowflake is a snowman, the only abomination is that he hasn’t been adopted yet. Snowflake is curious, playful, and loves to bound around in the snow. He likes winter hikes, hide and go seek, and all things Christmas. He can get a bit agitated when being scolded or having his picture taken and can occasionally cause devastating avalanches, so we don’t recommend him for beginning pet owners. However, with love, care, and a lot of ice, Snowflake will make a wonderful companion.","goodevil":"Evil","lawchaos":"Neutral","thumbImageUri":"https://www.mythicalmysfits.com/images/yeti_thumb.png","profileImageUri":"https://www.mythicalmysfits.com/images/yeti_hover.png"},{"mysfitId":"3f0f196c-4a7b-43af-9e29-6522a715342d","name":"Gary","species":"Kraken","age":2709,"description":"Gary loves to have a good time. His motto? “I just want to dance.” Give Gary a disco ball, a DJ, and a hat that slightly obscures the vision from his top eye, and Gary will dance the year away which, at his age, is like one night in humanoid time. If you're looking for a low-maintenance, high-energy creature companion that never sheds and always shreds, Gary is just the kraken for you.","goodevil":"Neutral","lawchaos":"Chaotic","thumbImageUri":"https://www.mythicalmysfits.com/images/kraken_thumb.png","profileImageUri":"https://www.mythicalmysfits.com/images/kraken_hover.png"}]}
```


## Links
https://github.com/aws-samples/aws-modern-application-workshop
