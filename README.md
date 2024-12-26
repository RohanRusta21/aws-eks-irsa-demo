# aws-eks-irsa-demo


### Create an IAM OIDC identity provider for your cluster.

```
eksctl utils associate-iam-oidc-provider --cluster eks-irsa-cluster  --approve --region us-east-2
```

### Creating a policy where s3 bucket mentioned which has to be used by the pod.

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
                "s3:GetObjectVersion",
                "s3:PutObject"
            ],
            "Resource": [
                "arn:aws:s3:::eks-bucket-irsa-demo",
                "arn:aws:s3:::eks-bucket-irsa-demo/*"
            ]
        }
    ]
}

```

### trust-relationship.json

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


### Creating a role and appending the above trust policy.

```
aws iam create-role --role-name role-irsa-demo --assume-role-policy-document file://trust-relationship.json --description "irsa role description"
```

### Attaching the role witht the policy we created in above steps

```
aws iam attach-role-policy --role-name role-irsa-demo --policy-arn=arn:aws:iam::251620460948:policy/irsa-eks-demo-policy
```

### Appending Annotations in the ServiceAccount we have to use.

```
kubectl annotate serviceaccount -n test nginx-sa eks.amazonaws.com/role-arn=arn:aws:iam::251620460948:role/role-irsa-demo
```
