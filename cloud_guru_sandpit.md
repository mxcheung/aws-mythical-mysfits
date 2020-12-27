
cloud_user

&1Vekhdvmd

https://283426106268.signin.aws.amazon.com/console?region=us-east-1

AKIAUD7MDFOOCNZ75P2Y

GpXZBA0tcnaJv51+NXtO4NYpKOy+XkiaW1GyvM7a

============================================================================
Module 1 8:22am
git clone -b java https://github.com/aws-samples/aws-modern-application-workshop.git

cd aws-modern-application-workshop


aws s3 mb s3://cheungm-bucket-name-20201228


aws s3 cp ~/environment/aws-modern-application-workshop/module-1/web/index.html s3://cheungm-bucket-name-20201228/index.html

aws s3api delete-bucket-policy --bucket cheungm-bucket-name-20201228 

aws s3api put-bucket-policy --bucket cheungm-bucket-name-20201228 --policy file://~/environment/aws-modern-application-workshop/module-1/aws-cli/website-bucket-policy.json

aws s3 website s3://cheungm-bucket-name-20201228 --index-document index.html


http://cheungm-bucket-name-20201228.s3-website.us-east-1.amazonaws.com

============================================================================
Module 2  8:38am

aws cloudformation create-stack --stack-name MythicalMysfitsCoreStack --capabilities CAPABILITY_NAMED_IAM --template-body file://~/environment/aws-modern-application-workshop/module-2/cfn/core.yml   

aws cloudformation describe-stacks --stack-name MythicalMysfitsCoreStack

aws cloudformation wait stack-create-complete --stack-name MythicalMysfitsCoreStack && echo "stack created"

aws cloudformation describe-stacks --stack-name MythicalMysfitsCoreStack > ~/environment/cloudformation-core-output.json



sudo yum -y install java-1.8.0-openjdk-devel
sudo alternatives --set java /usr/lib/jvm/java-1.8.0-openjdk.x86_64/bin/java
sudo alternatives --set javac /usr/lib/jvm/java-1.8.0-openjdk.x86_64/bin/javac

sudo wget http://repos.fedorapeople.org/repos/dchen/apache-maven/epel-apache-maven.repo -O /etc/yum.repos.d/epel-apache-maven.repo

sudo sed -i s/\$releasever/6/g /etc/yum.repos.d/epel-apache-maven.repo

sudo yum install -y apache-maven


cd ~/environment/aws-modern-application-workshop/module-2/app/service
mvn clean install

aws sts get-caller-identity


cd ..
docker build . -t 283426106268.dkr.ecr.us-east-1.amazonaws.com/mythicalmysfits/service:latest

docker run -p 8080:8080 283426106268.dkr.ecr.us-east-1.amazonaws.com/mythicalmysfits/service:latest

aws ecr create-repository --repository-name mythicalmysfits/service

```
{
    "repository": {
        "repositoryArn": "arn:aws:ecr:us-east-1:283426106268:repository/mythicalmysfits/service",
        "registryId": "283426106268",
        "repositoryName": "mythicalmysfits/service",
        "repositoryUri": "283426106268.dkr.ecr.us-east-1.amazonaws.com/mythicalmysfits/service",
        "createdAt": 1609105709.0,
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


docker push 283426106268.dkr.ecr.us-east-1.amazonaws.com/mythicalmysfits/service:latest

aws ecr describe-images --repository-name mythicalmysfits/service

aws ecs create-cluster --cluster-name MythicalMysfits-Cluster

```
{
    "cluster": {
        "clusterArn": "arn:aws:ecs:us-east-1:283426106268:cluster/MythicalMysfits-Cluster",
        "clusterName": "MythicalMysfits-Cluster",
        "status": "ACTIVE",
        "registeredContainerInstancesCount": 0,
        "runningTasksCount": 0,
        "pendingTasksCount": 0,
        "activeServicesCount": 0,
        "statistics": [],
        "tags": [],
        "settings": [
            {
                "name": "containerInsights",
                "value": "disabled"
            }
        ],
        "capacityProviders": [],
        "defaultCapacityProviderStrategy": []
    }
}
```
aws logs create-log-group --log-group-name mythicalmysfits-logs

aws ecs register-task-definition --cli-input-json file://~/environment/aws-modern-application-workshop/module-2/aws-cli/task-definition.json

aws elbv2 create-load-balancer --name mysfits-nlb --scheme internet-facing --type network --subnets subnet-0345046a5059e662e subnet-0e4d44781fafe7775 > ~/environment/nlb-output.json


aws elbv2 create-target-group --name MythicalMysfits-TargetGroup --port 8080 --protocol TCP --target-type ip --vpc-id vpc-00aa98d09ef098b16 --health-check-interval-seconds 10 --health-check-path / --health-check-protocol HTTP --healthy-threshold-count 3 --unhealthy-threshold-count 3 > opn


aws elbv2 create-listener --default-actions TargetGroupArn=arn:aws:elasticloadbalancing:us-east-1:283426106268:targetgroup/MythicalMysfits-TargetGroup/fc189a0645aadfb9,Type=forward --load-balancer-arn arn:aws:elasticloadbalancing:us-east-1:283426106268:loadbalancer/net/mysfits-nlb/7bbdb66a487a679d --port 80 --protocol TCP

aws iam create-service-linked-role --aws-service-name ecs.amazonaws.com


aws ecs create-service --cli-input-json file://~/environment/aws-modern-application-workshop/module-2/aws-cli/service-definition.json





aws s3 cp ~/environment/aws-modern-application-workshop/module-2/web/index.html s3://cheungm-bucket-name-20201228/index.html

http://mysfits-nlb-7bbdb66a487a679d.elb.us-east-1.amazonaws.com/mysfits


http://cheungm-bucket-name-20201228.s3-website-us-east-1.amazonaws.com


aws s3 mb s3://cheungm-artifacts-bucket-name-20201228


aws s3api put-bucket-policy --bucket cheungm-artifacts-bucket-name-20201228 --policy file://~/environment/aws-modern-application-workshop/module-2/aws-cli/artifacts-bucket-policy.json

aws codecommit create-repository --repository-name MythicalMysfitsService-Repository

aws codebuild create-project --cli-input-json file://~/environment/aws-modern-application-workshop/module-2/aws-cli/code-build-project.json

aws codepipeline create-pipeline --cli-input-json file://~/environment/aws-modern-application-workshop/module-2/aws-cli/code-pipeline.json


aws ecr set-repository-policy --repository-name mythicalmysfits/service --policy-text file://~/environment/aws-modern-application-workshop/module-2/aws-cli/ecr-policy.json


git config --global user.name "max.cheung"
git config --global user.email maxcheung@optusnet.com.au
git config --global credential.helper '!aws codecommit credential-helper $@'
git config --global credential.UseHttpPath true

cd ~/environment/


git clone https://git-codecommit.us-east-1.amazonaws.com/v1/repos/MythicalMysfitsService-Repository

cp -r ~/environment/aws-modern-application-workshop/module-2/app/* ~/environment/MythicalMysfitsService-Repository/

cd ~/environment/MythicalMysfitsService-Repository/

git add .
git commit -m "I changed the age of one of the mysfits."
git push


http://mysfits-nlb-7bbdb66a487a679d.elb.us-east-1.amazonaws.com/mysfits


http://cheungm-bucket-name-20201228.s3-website-us-east-1.amazonaws.com
============================================================================
Module 3  9:10am

aws dynamodb create-table --cli-input-json file://~/environment/aws-modern-application-workshop/module-3/aws-cli/dynamodb-table.json

aws dynamodb describe-table --table-name MysfitsTable

aws dynamodb batch-write-item --request-items file://~/environment/aws-modern-application-workshop/module-3/aws-cli/populate-dynamodb.json

aws dynamodb scan --table-name MysfitsTable


cp -r ~/environment/aws-modern-application-workshop/module-3/app/service/* ~/environment/MythicalMysfitsService-Repository/service/

cd ~/environment/MythicalMysfitsService-Repository

git add .

git commit -m "Add new integration to DynamoDB."

git push


aws s3 cp --recursive ~/environment/aws-modern-application-workshop/module-3/web/ s3://cheungm-bucket-name-20201228/

http://cheungm-bucket-name-20201228.s3-website-us-east-1.amazonaws.com

============================================================================
Module 4  9:31am


aws cognito-idp create-user-pool --pool-name MysfitsUserPool --auto-verified-attributes email


aws cognito-idp create-user-pool-client --user-pool-id us-east-1_NuTR61dnf --client-name MysfitsUserPoolClient


aws apigateway create-vpc-link --name MysfitsApiVpcLink --target-arns arn:aws:elasticloadbalancing:us-east-1:283426106268:loadbalancer/net/mysfits-nlb/7bbdb66a487a679d > ~/environment/api-gateway-link-output.json

aws apigateway import-rest-api --parameters endpointConfigurationTypes=REGIONAL --body file://~/environment/aws-modern-application-workshop/module-4/aws-cli/api-swagger.json --fail-on-warnings


cloud_user:~/environment/MythicalMysfitsService-Repository (master) $ aws apigateway import-rest-api --parameters endpointConfigurationTypes=REGIONAL --body file://~/environment/aws-modern-application-workshop/module-4/aws-cli/api-swagger.json --fail-on-warnings
{
    "id": "la5sfpqbk7",
    "name": "MysfitsApi",
    "createdDate": 1609109799,
    "apiKeySource": "HEADER",
    "endpointConfiguration": {
        "types": [
            "REGIONAL"
        ]
    },
    "disableExecuteApiEndpoint": false
}

aws apigateway create-deployment --rest-api-id REPLACE_ME_WITH_API_ID --stage-name prod
aws apigateway create-deployment --rest-api-id la5sfpqbk7 --stage-name prod

cloud_user:~/environment/MythicalMysfitsService-Repository (master) $ aws apigateway create-deployment --rest-api-id la5sfpqbk7 --stage-name prod
{
    "id": "mtmmby",
    "createdDate": 1609109914
}

https://REPLACE_ME_WITH_API_ID.execute-api.REPLACE_ME_WITH_REGION.amazonaws.com/prod
https://la5sfpqbk7.execute-api.us-east-1.amazonaws.com/prod

cd ~/environment/MythicalMysfitsService-Repository/
cp -r ~/environment/aws-modern-application-workshop/module-4/app/* .
git add .
git commit -m "Update service code backend to enable additional website features."
git push


aws s3 cp --recursive ~/environment/aws-modern-application-workshop/module-4/web/ s3://cheungm-bucket-name-20201228/


============================================================================
Module 5  10:16am

aws codecommit create-repository --repository-name MythicalMysfitsStreamingService-Repository



{
    "repositoryMetadata": {
        "accountId": "283426106268",
        "repositoryId": "03cd0716-bb3d-4a6f-8d2a-e1b30a3592e3",
        "repositoryName": "MythicalMysfitsStreamingService-Repository",
        "lastModifiedDate": 1609111066.135,
        "creationDate": 1609111066.135,
        "cloneUrlHttp": "https://git-codecommit.us-east-1.amazonaws.com/v1/repos/MythicalMysfitsStreamingService-Repository",
        "cloneUrlSsh": "ssh://git-codecommit.us-east-1.amazonaws.com/v1/repos/MythicalMysfitsStreamingService-Repository",
        "Arn": "arn:aws:codecommit:us-east-1:283426106268:MythicalMysfitsStreamingService-Repository"
    }
}

cd ~/environment/

git clone https://git-codecommit.us-east-1.amazonaws.com/v1/repos/MythicalMysfitsStreamingService-Repository

cd ~/environment/

cd ~/environment/MythicalMysfitsStreamingService-Repository/

cp -r ~/environment/aws-modern-application-workshop/module-5/app/streaming/* .

cp -r ~/environment/aws-modern-application-workshop/module-5/cfn/* .


git add .
git commit -m "New stream processing service."
git push


aws s3 mb s3://REPLACE_ME_YOUR_BUCKET_NAME/
sam package --template-file ./real-time-streaming.yml --output-template-file ./transformed-streaming.yml --s3-bucket REPLACE_ME_YOUR_BUCKET_NAME

aws s3 mb s3://cheungm-lambda-bucket-name-20201228/
sam package --template-file ./real-time-streaming.yml --output-template-file ./transformed-streaming.yml --s3-bucket cheungm-lambda-bucket-name-20201228
aws cloudformation deploy --template-file /home/ec2-user/environment/MythicalMysfitsStreamingService-Repository/transformed-streaming.yml --stack-name MythicalMysfitsStreamingStack --capabilities CAPABILITY_IAM
aws cloudformation describe-stacks --stack-name MythicalMysfitsStreamingStack

aws s3 cp ~/environment/aws-modern-application-workshop/module-5/web/index.html s3://cheungm-bucket-name-20201228/

============================================================================
Clean up  10:30am

aws cloudformation delete-stack --stack-name STACK-NAME-HERE
To remove all of the created resources, you can visit the following AWS Consoles, which contain resources you've created during the Mythical Mysfits workshop:

AWS Kinesis
AWS Lambda
Amazon S3
Amazon API Gateway
Amazon Cognito
AWS CodePipeline
AWS CodeBuild
AWS CodeCommit
Amazon DynamoDB
Amazon ECS
Amazon EC2
Amazon VPC
AWS IAM
AWS CloudFormation


Log In /Register --> unable to confirm as email is blocked in sand pit environment
http://cheungm-bucket-name-20201228.s3-website-us-east-1.amazonaws.com/


============================================================================

```
REPLACE_ME_BUCKET_NAME -> cheungm-bucket-name-20201228

REPLACE_ME_REGION -> us-east-1

REPLACE_ME_ACCOUNT_ID -> 283426106268

```

```
REPLACE_ME_SECURITY_GROUP_ID -> sg-0a48e2cafce8cb133

REPLACE_ME_PRIVATE_SUBNET_ONE -> subnet-0c5f5364ef5b7e048
REPLACE_ME_PRIVATE_SUBNET_TWO -> subnet-025b63ac588d65ca1


REPLACE_ME_PUBLIC_SUBNET_ONE -> subnet-0345046a5059e662e
REPLACE_ME_PUBLIC_SUBNET_TWO -> subnet-0e4d44781fafe7775

REPLACE_ME_ECS_SERVICE_ROLE_ARN -> arn:aws:iam::283426106268:role/MythicalMysfitsCoreStack-EcsServiceRole-1PAXTQQLOMD7G
REPLACE_ME_ECS_TASK_ROLE_ARN -> arn:aws:iam::283426106268:role/MythicalMysfitsCoreStack-ECSTaskRole-1NFS4W31UWGF8

REPLACE_ME_WITH_DOCKER_IMAGE_TAG -> 283426106268.dkr.ecr.us-east-1.amazonaws.com/mythicalmysfits/service:latest

REPLACE_ME_IMAGE_TAG_USED_IN_ECR_PUSH -> 283426106268.dkr.ecr.us-east-1.amazonaws.com/mythicalmysfits/service:latest



REPLACE_ME_VPC_ID -> vpc-00aa98d09ef098b16

REPLACE_ME_CODEBUILD_ROLE_ARN -> arn:aws:iam::283426106268:role/MythicalMysfitsServiceCodeBuildServiceRole
REPLACE_ME_CODEPIPELINE_ROLE_ARN -> arn:aws:iam::283426106268:role/MythicalMysfitsServiceCodePipelineServiceRole

REPLACE_ME_ECS_SERVICE_ROLE_ARN -> arn:aws:iam::283426106268:role/MythicalMysfitsCoreStack-EcsServiceRole-RAIYNIFQA302

REPLACE_ME_NLB_TARGET_GROUP_ARN -> arn:aws:elasticloadbalancing:us-east-1:283426106268:targetgroup/MythicalMysfits-TargetGroup/fc189a0645aadfb9

REPLACE_ME_NLB_ARN ->   arn:aws:elasticloadbalancing:us-east-1:283426106268:loadbalancer/net/mysfits-nlb/7bbdb66a487a679d

REPLACE_ME_NLB_DNS_NAME ->  mysfits-nlb-7bbdb66a487a679d.elb.us-east-1.amazonaws.com

REPLACE_ME_NLB_DNS ->  http://mysfits-nlb-283426106268-7bbdb66a487a679d.elb.us-east-1.amazonaws.com/

```

```
REPLACE_ME_ARTIFACTS_BUCKET_NAME -> cheungm-artifacts-bucket-name-20201228

REPLACE_ME_MYSFITS_API_ENDPOINT -> https://la5sfpqbk7.execute-api.us-east-1.amazonaws.com/prod

REPLACE_ME_COGNITO_USER_POOL_ID ->   us-east-1_NuTR61dnf

REPLACE_ME_COGNITO_USER_POOL_CLIENT_ID -> 



REPLACE_ME_COGNITO_USER_POOL_ID -> us-east-1_NuTR61dnf
REPLACE_ME_ARTIFACTS_USER_POOL_CLIENTID -> 3ksknjeakgg8glop22ga39vpfa 
REPLACE_ME_VPC_LINK_ID -> wzq72a

REPLACE_ME_WITH_API_ID  -> la5sfpqbk7

REPLACE_ME_WITH_ABOVE_CLONE_URL ->  https://git-codecommit.us-east-1.amazonaws.com/v1/repos/MythicalMysfitsStreamingService-Repository


REPLACE_ME_YOUR_LAMBDA_BUCKET_NAME -> cheungm-lambda-bucket-name-20201228


REPLACE_ME_WITH_STREAMING_API_ID  -> https://k3jnjymbua.execute-api.us-east-1.amazonaws.com/p
```
