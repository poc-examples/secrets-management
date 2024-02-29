You first need a secret after creating the identity in IAM. To create secret:

```oc create secret generic awssm-secret --from-literal=access-key=<access-key> --from-literal=secret-access-key=<secret-access-key> -n kubecost```

You also need to create a policy and attach it to the user:

```{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "secretsmanager:GetResourcePolicy",
                "secretsmanager:GetSecretValue",
                "secretsmanager:DescribeSecret",
                "secretsmanager:ListSecretVersionIds"
            ],
            "Resource": [
                "arn:aws:secretsmanager:us-east-2:<account>:secret:<secretname>"
            ]
        },
        {
            "Effect": "Allow",
            "Action": "secretsmanager:ListSecrets",
            "Resource": "*"
        }
    ]
}
```
