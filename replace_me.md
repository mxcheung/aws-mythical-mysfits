## Module-1

```
REPLACE_ME_BUCKET_NAME -> cheungm-bucket-name-20201227
```

## Module-2

```

REPLACE_ME_ACCOUNT_ID -> 918300033687
REPLACE_ME_SECURITY_GROUP_ID -> sg-09958e039f704f9be

REPLACE_ME_PRIVATE_SUBNET_ONE -> subnet-0113ab356d5ff1c92
REPLACE_ME_PRIVATE_SUBNET_TWO -> subnet-091f721f2629100a5


REPLACE_ME_PUBLIC_SUBNET_ONE -> subnet-076b3f6261201f184
REPLACE_ME_PUBLIC_SUBNET_TWO -> subnet-0f9781c1ea505e25c

REPLACE_ME_ECS_TASK_ROLE_ARN -> arn:aws:iam::918300033687:role/MythicalMysfitsCoreStack-ECSTaskRole-G4PBHNSAQ0SL
REPLACE_ME_REGION -> ap-southeast-2
REPLACE_ME_VPC_ID -> vpc-0d482f57e3ef741d5

REPLACE_ME_CODEBUILD_ROLE_ARN -> arn:aws:iam::918300033687:role/MythicalMysfitsServiceCodeBuildServiceRo
REPLACE_ME_CODEPIPELINE_ROLE_ARN -> arn:aws:iam::918300033687:role/MythicalMysfitsServiceCodePipelineServiceRole

REPLACE_ME_ECS_SERVICE_ROLE_ARN -> arn:aws:iam::918300033687:role/MythicalMysfitsCoreStack-EcsServiceRole-RAIYNIFQA302

REPLACE_ME_NLB_TARGET_GROUP_ARN -> arn:aws:elasticloadbalancing:ap-southeast-2:918300033687:targetgroup/MythicalMysfits-TargetGroup/7a0bbdb0201c083e

REPLACE_ME_NLB_ARN ->   arn:aws:elasticloadbalancing:ap-southeast-2:918300033687:loadbalancer/net/mysfits-nlb/7cfb1105f45d2351

REPLACE_ME_ARTIFACTS_BUCKET_NAME -> cheungm-artifacts-bucket-name-20201227

REPLACE_ME_WITH_DOCKER_IMAGE_TAG -> 918300033687.dkr.ecr.ap-southeast-2.amazonaws.com/mythicalmysfits/service:latest

```
## Module-3


## Module-4

api-swagger.json
```
REPLACE_ME_COGNITO_USER_POOL_ID -> ap-southeast-2_l38vvj3ws
REPLACE_ME_VPC_LINK_ID  -> y8s0ca
REPLACE_ME_NLB_DNS -> mysfits-nlb-7cfb1105f45d2351.elb.ap-southeast-2.amazonaws.com
```


```
REPLACE_ME_USER_POOL_ID ->  ap-southeast-2_l38vvj3ws   // example: 'us-east-1_abcd12345'
REPLACE_ME_USER_POOL_CLIENT -> 4n5ml5sa0jqn4ot2u3mcaedv3g  // example: 'abcd12345abcd12345abcd12345'

REPLACE_ME_MYSFITS_API_ENDPOINT -> 'https://j9fokp4mi6.execute-api.ap-southeast-2.amazonaws.com/prod'; // example: 'https://abcd12345.execute-api.us-east-1.amazonaws.com/prod'

```
    var mysfitsApiEndpoint = 'https://j9fokp4mi6.execute-api.ap-southeast-2.amazonaws.com/prod'; // example: 'https://abcd12345.execute-api.us-east-1.amazonaws.com/prod'
    var cognitoUserPoolId = 'ap-southeast-2_l38vvj3ws';  // example: 'us-east-1_abcd12345'
    var cognitoUserPoolClientId = '4n5ml5sa0jqn4ot2u3mcaedv3g'; // example: 'abcd12345abcd12345abcd12345'
    var awsRegion = 'ap-southeast-2'; // example: 'us-east-1' or 'eu-west-1' etc.


## Module-5

```
REPLACE_ME_WITH_ABOVE_CLONE_URL -> https://git-codecommit.ap-southeast-2.amazonaws.com/v1/repos/MythicalMysfitsStreamingService-Repository

REPLACE_ME_API_ENDPOINT ->  https://j9fokp4mi6.execute-api.ap-southeast-2.amazonaws.com/prod/    // eg: 'https://ljqomqjzbf.execute-api.us-east-1.amazonaws.com/prod/'
```
