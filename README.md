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

# trust-relationship.json
```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Federated": "arn:aws:iam::251620460948:oidc-provider/oidc.eks.us-east-2.amazonaws.com/id/8826F329E906762BAE166DE08D489F28"
      },
      "Action": "sts:AssumeRoleWithWebIdentity",
      "Condition": {
        "StringEquals": {
          "$oidc_provider:aud": "sts.amazonaws.com",
          "$oidc_provider:sub": "system:serviceaccount:test:nginx-sa"
        }
      }
    }
  ]
}


```
aws iam create-role --role-name role-irsa-demo --assume-role-policy-document file://trust-relationship.json --description "irsa role description"
```


```
