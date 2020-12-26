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

## Links
https://github.com/aws-samples/aws-modern-application-workshop
