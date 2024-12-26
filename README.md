# aws-eks-irsa-demo



```
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Sid": "Statement1",
			"Effect": "Allow",
			"Action": [
				"s3:ListBucket",
				"s3:GetObject",
				"s3:GetObjectVersion"
			],
			"Resource": [
			    "arn:aws:s3:::eks-bucket-irsa-demo"
			    ]
		}
	]
}

```
