# ansible-ec2-sts

Following is the process to scan multiple accounts via STS:

- Step 1: Create a role in the destination account from which we would like to scan the instances from and authorize the user and the account from the source account to AssumeRole

aws iam create-role --path /service/ --role-name cross-access-role --assume-role-policy-document '{ "Version": "2008-10-17",
 "Statement": [ { "Sid": "", "Effect": "Allow", "Principal": { "AWS": "arn:aws:iam::<sourceaccountid>:user/<username>" }, "Action": "sts:AssumeRole" } ] }'

- Step 2: Attach a policy to the role to only list instances and describe tags

aws iam put-role-policy --policy-name 'cross-access-policy' --role-name cross-access-role --policy-document '{ "Statement": [ { "Sid": "", "Action": 
[ "ec2:DescribeInstances", "ec2:DescribeTags", "route53:Get*", "route53:List*", "rds:Describe*", "elasticloadbalancing:Describe*" ], "Effect": "Allow", "Resource": "*" } ] }'

- Step3 : Using the access and secret keys of the <sourceaccountid>:user/<username>.

Place the keys in the section on the ec2.ini

[master-account]
key = xxxxx
secret = xxxxxx

