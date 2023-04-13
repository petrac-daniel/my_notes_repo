# AWS Commands

export AWS_CA_BUNDLE=/mnt/c/certs/amazon.aws.pem
export AWS_CA_BUNDLE=/etc/ssl/certs/ca-certificates.crt
export AWS_CA_BUNDLE=/mnt/c/certs/RITcerts.pem

```aws --endpoint-url http://localhost:4566 sns create-topic --name topic```

## AWS CONFIGURE
```aws configure list``` returns the current list of items for the account.
```aws configure``` - configure access key and secret for the CLI

## AWS S3
```aws s3 mb s3://my-buck-123``` create bucket
```aws s3 ls``` lists buckets
```aws s3 ls <bucket_name>``` - lists items in a bucket
```aws s3 presign``` temporary access to a bucket
```aws s3api``` more detailed than `aws s3` 
```aws s3api create-bucket --bucket dpetracbucket --create-bucket-configuration LocationConstraint=eu-central-1``` creates a bucket
```aws s3api delete-bucket --bucket dpetracbucket``` deletes a bucket
```aws s3 cp <local_filepath> <s3://bucketname/filepath> --sse AES256``` copy content to a bucket
```aws s3 rm s3://my-buck-123/notes.txt``` Delete file in bucket
```aws s3 rb s3://my-buck-123``` Delete bucket
ca
```aws sts get-caller-identity``` whoami equivalent

```sudo yum install -y amazon-efs-utils``` - installs the EFS utils in the EC2 instance

## AWS EC2
```aws ec2 authorize-security-group-ingress --group-id <GROUP_ID> --port <PORT> --protocol tcp --cidr <CIDR>``` authorizing security group 

```curl -s http://169.254.169.254/latest/meta-data``` the ec2 metadata service

## AWS DynamoDB
```
aws dynamodb list-tables --endpoint-url http://localhost:8000
```

```
aws dynamodb delete-table \
  --endpoint-url http://localhost:8000 \
  --table-name MY_AWESOME_DYNAMO_TABLE_NAME
```

```
aws dynamodb create-table --endpoint-url http://localhost:8000 \
--table-name MY_AWESOME_DYNAMO_TABLE_NAME \
--attribute-definitions AttributeName=HASH_KEY,AttributeType=S AttributeName=RANGE_KEY,AttributeType=S \
--key-schema AttributeName=HASH_KEY,KeyType=HASH AttributeName=RANGE_KEY,KeyType=RANGE \
--provisioned-throughput ReadCapacityUnits=5,WriteCapacityUnits=5 \
--region eu-central-1
```


```
aws dynamodb scan \
  --table-name MY_AWESOME_DYNAMO_TABLE_NAME \
  --attributes-to-get '["HASH_KEY", "RANGE_KEY"]' \
  --endpoint-url http://localhost:8000 \
  --region eu-central-1

```

```
aws dynamodb query --table-name MY_AWESOME_DYNAMO_TABLE_NAME \
--region eu-central-1 \
--key-condition-expression "HASH_KEY = :hk" \
--expression-attribute-values  '{":hk":{"S":"4eb80219-c4c9-4ebd-8da3-03672817d868_LOCK"}}'
```

```
aws dynamodb delete-item --endpoint-url http://localhost:8000 \
  --table-name MY_AWESOME_DYNAMO_TABLE_NAME \
  --key "{\"HASH_KEY\":{\"S\":\"c96ded1a-e109-450b-a5a5-6b83fbf74e64\"}, \"RANGE_KEY\":{\"S\": \"DUMMY_TOKEN_ID\"}}"
```

```
aws dynamodb put-item \
  --endpoint-url http://localhost:8000 \
  --table-name MY_AWESOME_DYNAMO_TABLE_NAME \
  --item '{"DATA":{"M":{"refreshTokenExpiresAtMs":{"N":"1629191323823"},"refreshTokenReceivedAtMs":{"N":"1621416572094"},"code":{"S":"1067c74f-91a4-4191-a6a3-b907d227e043"},"refreshTokenExpiresIn":{"N":"7776000"},"token":{"M":{"access_token":{"S":"66493852-5a65-483d-89f1-3f77d497e7d0"},"token_type":{"S":"Bearer"},"expires_in":{"N":"30"},"refresh_token":{"S":"be96919e-1790-47e4-935d-6c57be1c2747"},"scope":{"S":"AISP"}}}}},"EXPIRATION_TIME":{"N":"1629191323"},"CREATION_TIME":{"N":"1621415323"},"HASH_KEY":{"S":"4eb80219-c4c9-4ebd-8da3-03672817d868"},"RANGE_KEY":{"S":"812b91b5-dd10-4145-8a76-caae79104aa5"}}'
```

```aws sqs --endpoint=http://localhost:9324 list-queues```
```aws sqs --endpoint=http://localhost:9324 purge-queue --queue-url=http://localhost:9324/queue/MY-LOCAL_QUEUE```

```
aws sqs --endpoint=http://localhost:9324 send-message \
--queue-url=http://localhost:9324/queue/MY-LOCAL_QUEUE \
--message-attributes='{"correlationId":{"DataType":"String", "StringValue":"CORRELATION1"},"extendedCorrelationId":{"DataType":"String", "StringValue":"EXTCORRELATION1"},"subscriptionId":{"DataType":"String", "StringValue":"5a261c96-e6fe-452f-bf34-652fea3f4831"}}' \
--message-body='{"accountId":"D46D876174C9725C7196F568EDECFB8BAA16D1EFFC4435306BB82D0E1A123AA6D8FCF1B2D524C4D1EB4ACE803072D4DF824D188AE5302946F9A3E556A55D8C37587D7063C01E08C9AA5A704A05FBA5A6","tokenId":"3864c855-1430-416b-93d8-6268cf2f5824","transactionsFrom":"2020-08-27","backgroundSyncAllowed":true,"originalResourceId":"SK2802000000000000000007","batchId":"21dcad09-6ebe-464d-bb11-b431d9095269","resourceType":"TRANSACTIONS","withBalances":true,"allTokens":false,"batchType":"TRANSACTIONS","transactionsInitLoad":false}'
```

```
aws sqs create-queue --queue-name MY-LOCAL_QUEUE --region eu-central-1 --endpoint-url http://localhost:9324
```

## AWS REST
```aws ecr get-login-password | docker login --username AWS --password-stdin <ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com``` register to ECR (Docker registry)



# DOCKER
```docker build .``` Build the docker image from the current folder
```docker build --build-arg HTTP_PROXY="http://something.somethingelse.com"``` adding environment vars to docker build
```docker build --no-cache``` force a new clean build of the image
```docker tag  <IMAGE_ID> <TAG_NAME>``` here the TAG_NAME can be a repo e.g. 622217171717.dkr.ecr.us-east-1.amazonaws.com/my-project-9 
```docker push <TAG_NAME>``` docker push

```sudo docker ps``` list running docker VMs
```sudo docker ps --all``` list all docker VMs

```docker container prune``` removes all stopped containers

```sudo docker exec -ti <docker_instance_id> bash```

```docker images``` or ```docker image ls``` list docker images
```docker image remove``` remove a docker image 

```docker run -p 5432:5432 --name some-postgres -d <docker_image_name> -e ENV_VAR=env_var_val```
```docker run -it --entrypoint /bin/bash <some_docker_image>``` sets the command which to run after the container has been started

```docker-compose up -d``` runs a docker-compose configuration

## POSTGRESQL
```\dt```
```explain <SQL SELECT QUERY>``` explains what postgres needs to do to execute a query
```explain analyse <SQL SELECT QUERY>``` -||-

# SSH
   when it sees that one of the parent dirs to ~/.ssh are writable by group or owner, it refuses the connection.

```ssh admdape@100.64.164.188 -J jumphost.one,jumphost.two -L 8080:localhost:49154``` -L creates tunnel to local port, so as then request can be executed to localhost:8080
 ```ssh -J admdape@nyx.openapi-test.merlin -L 9092:kafka.test.merlin:9092 admdape@testexecution-dev59.openapi-test.merlin``` example of ssh tunneling with jumphost
 ```ssh -A admdape@nyx.openapi-test.merlin``` example of forwarding agent. TIPP: agent needs to be running, TIP2: private key needs to be added to the agent in order to work
 ```eval 'ssh-agent.exe'``` evaluate if ssh agent is running
 ```ssh-add key file``` add key to agent
 ```ssh admdape@nyx.openapi-test.merlin -D4444``` creates a SOCKS5 tunnel on port 4444
 ```scp admdape@nyx.openapi-test.merlin:/home/admdape/mytempfile.txt .``` copy file from remote server
 ```ssh-copy-id -i ~/.ssh/raspberry_id_rsa.pub raspberry@raspberrypi``` example of copying key to remote
```less /var/log/secure``` read ssh logs (on server)

# KUBERNETES
```kubectl get nodes``` gets the status
```kubectl get pod``` gets running pods
```kubectl get pod -o wide``` gets running pods with more info
```kubectl get pods <POD_NAME> -o jsonpath='{.spec.containers[*].name}'``` list containers inside a pod
```kubectl get replicaset``` gets replica set of pods
```kubectl get services``` gets running services
```kubectl get events -o custom-columns=FirstSeen:.firstTimestamp,LastSeen:.lastTimestamp,Count:.count,From:.source.component,Type:.type,Reason:.reason,Message:.message``` get events with timestamp
```kubectl <command> -h``` help command for kubectl create
```kubectl create <depl_name> --image=image``` creates a deployment image
```kubectl logs <POD_NAME>``` gets logs of that pod
```kubectl describe pod <POD_NAME>``` displays state changes of a pod
```kubectl exec -it <POD_NAME> -- /bin/bash``` opens a console of the pod
```kubectl delete deployment <deployment_name>``` deletes a deployment
```kubectl apply -f <file>``` reads a file and applies the commads in the file 
```kubectl edit deployment <deployment_name>``` edit the yaml file of the deployment
```kubectl scale deploy my-awesome-deployment --replicas=0``` scale a deployment 
```kubectl set env deployment my-awesome-deployment ENV_VAR_NAME=some_env_var_value``` set environment variable on deployment
```kubectl port-forward deployment/my-awesome-deployment 28015:27017```

# BASH
```find . -name "*foo*"``` find file with wildcard pattern
```ps -ef | grep ssh``` check running ssh processes
```| jq someJson.propertyPath``` json query path in WSL, needs to be installed
```pbpaste``` paste something from clipboard, needs to be installed
```| xargs -n1``` execute each line as a separate command
```| xargs -n1 -I {} cp {} ../some/destination/folder```  xargs executes every line for a copy, the -I tells that {} should be the placehodler in the command
```pbcopy``` paste something from clipboard, needs to be installed
```export HTTP_PROXY=<http_proxy_location>``` setting up env vars.
```ps``` list processes
```ip a``` list network settings, alternative ```ifconfig```
```cat /etc/services``` shows active open ports
```cat /etc/services | grep 43``` shows active open ports
```tail -f <file>``` write out and keep writing out contents of the file while it is changing
```sudo iptables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT``` allow established and related connections
```sudo iptables -A INPUT -p tcp --dport 80 -m conntrack --ctstate NEW,ESTABLISHED -j ACCEPT``` open port 80 (webserver)
```| jq .``` format json
```| jq -r .``` format raw json
```| jq --arg ARG "$1" '. | map(.[] | select(.someString | contains($ARG)))'``` format json and select elements in array that have someString containing ARG
```find . -type f -print0 | xargs -0 dos2unix``` replace CRLF to LF in a folder recursively
```watch -n 1 <some_command>``` will execute every second <some_command>, if the command contains pipes single quotes can be used to wrap the  command

```$ sudo systemctl start httpd```     [On Systemd]
```$ sudo service httpd start``` 	 [On SysVInit]
```sudo netstat -tlnp``` show listening ports

```sudo lsof -iTCP -sTCP:LISTEN -P``` lists listening services
```sudo -su ec2-user``` switch user

```sudo chkconfig httpd on``` - start apache on restart of the EC2 instance
```touch myfilename``` creates an empty file
```echo <ZIPPED_BASE64_STRING> | base64 --decode | gunzip```
```bash -x <SCRIPT>``` e.g. "bash -x ./my_awesome_shell_script with parameters" gives out additional logging
```bla.sh | bash -x ``` execute the file
```free``` or ```free -h``` see memory information
```date``` show clock information of the maschine
```less <some text file>``` good for reading logs
```<some command> | tee some_tee.log``` writes the output of <some_command> to stdout and also into the log file
```tail -f somelogfile.log``` will write out the last lines as the log file is been written out
```<some_command> | grep 'GET' | cut -b 100``` cuts off first 100 characters from the grep output
```<some_command> | grep 'GET' | sed 'some_regex'``` sed applies some regex to a string in the pipe
```<some_command> | sed 1d``` remove first line from output
```<some_command> | awk '{print $1;}'``` get only first word out 
```<some_command> | sort``` sorts the output
```<some_command> | sort | uniq -c``` uniq gets all the unique values
```chmod +x some_script.sh``` makes the file executable
```top``` see system statistics... running processes and so on
```pidof vi``` tells the PID of a certain executable
```df -h``` write info about local storage
```mount``` display info where is what mounted
```du -xk | sort -n``` writes out all the directories and files and sorts them by size
```. <SOME_SHELL_SCRIPT>.sh``` runs the shell script in the current process
```arp -a``` scan local network for addresses

## OPENSSL - save certificate
```openssl s_client -connect foo.bar.com:443 -showcerts```
```cat server_cer.pem | openssl x509 -noout -text``` describe certificate

# ANSIBLE
```ansible all -m ping``` list all modules 
/ -m defines the module
 -a defines the arguments to pass to the module
 -u defines the user to use to execute this commands
 
```ansible webservers -m command -a "ls"``` execute the given command on all servers within a group -> webservers is from the /etc/ansible/hosts file
```ansible all -m setup``` get info about hosts

# WINDOWS
```netstat -ano | Select-String "65406"``` check if port is listening

# WSL
wsl --list --verbose
wsl --shutdown

# GRADLE
gradle tasks --all

# GIT
```git update-index --chmod=+x envwrapper.sh``` update executable flag on file in git repo
```git log | less``` open git history with vi like editor so you can search it

## handy gitconfig aliases
```
[alias]
	st = status
	co = checkout
	lg1 = log --graph --abbrev-commit --decorate --format=format:'%C(bold blue)%h%C(reset) - %C(bold green)(%ar)%C(reset) %C(white)%s%C(reset) %C(dim white)- %an%C(reset)%C(bold yellow)%d%C(reset)' --all
	lg2 = log --graph --abbrev-commit --decorate --format=format:'%C(bold blue)%h%C(reset) - %C(bold cyan)%aD%C(reset) %C(bold green)(%ar)%C(reset)%C(bold yellow)%d%C(reset)%n''          %C(white)%s%C(reset) %C(dim white)- %an%C(reset)' --all
	lg = log --all --decorate --oneline --graph
	adog = log --all --decorate --oneline --graph
	lm = log -1 --pretty=%B
	pusho = git push -u origin HEAD
```


# PYTHON
```python -c "import ssl; print(ssl.get_default_verify_paths())"``` check where python is taking ssl certificates from

# KUBERNETES
```kubectl get nodes``` - gets nodes from the k8s cluster
```kubectl get pod``` - gets status for pods
```kubectl get services``` - gets current services
```kubectl create deployment <NAME> --image=<IMAGE_NAME>``` - create deployment
```kubectl get deployment``` - gets current deployments
```kubectl get replicaset``` - gets current replica sets
```kubectl edit deployment <DEPLOYMENT_NAME>``` - edit the deployment .yaml file
```kubectl logs <POD_NAME>``` - get logs from the specific pod
```kubectl describe pod <POD_NAME>``` - describe specific pod
```kubectl exec -it <POD_NAME> -- bin/bash``` - open terminal on that pod
```kubectl delete deployment <DEPLOYMENT_NAME>``` deletes a deployment
```kubectl apply -f <FILE_NAME>``` applies a configuration file to the k8s cluster
 
