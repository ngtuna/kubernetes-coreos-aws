#Instruction

##Create security group

```
aws ec2 create-security-group --group-name kubernetes-coreos --description "Kubernetes on CoreOS Security Group"
aws ec2 authorize-security-group-ingress --group-name kubernetes-coreos --protocol tcp --port 22 --cidr 0.0.0.0/0
aws ec2 authorize-security-group-ingress --group-name kubernetes-coreos --protocol tcp --port 8080 --cidr 0.0.0.0/0
aws ec2 authorize-security-group-ingress --group-name kubernetes-coreos --source-security-group-name kubernetes-coreos
```

##Create kubernetes master
```
aws ec2 run-instances \
--image-id ami-cb8d6ba4 \
--key-name tuna-eu \
--region eu-central-1 \
--security-groups kubernetes-coreos \
--instance-type t2.small \
--user-data file://master.yaml
```

##Create kubernetes node

Note: Replace <master_private_ip> in node.yaml

```
aws ec2 run-instances \
--count 1 \
--image-id ami-cb8d6ba4 \
--key-name tuna-eu \
--region eu-central-1 \
--security-groups kubernetes-coreos \
--instance-type t2.small \
--user-data file://node.yaml
```
