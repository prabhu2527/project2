project2

Jenkins
ssh -i "demo.pem" ubuntu@ec2-18-118-198-107.us-east-2.compute.amazonaws.com

QAMatser node
ssh -i "PROD.pem" ubuntu@ec2-3-15-156-139.us-east-2.compute.amazonaws.com

QAWorkerNode
ssh -i "PROD.pem" ubuntu@ec2-3-12-85-187.us-east-2.compute.amazonaws.com

PRODMasterNode
ssh -i "PROD.pem" ubuntu@ec2-3-15-155-57.us-east-2.compute.amazonaws.com

PRODWorkerNode
 ssh -i "PROD.pem" ubuntu@ec2-3-131-151-148.us-east-2.compute.amazonaws.com


sudo ansible --inventory /etc/ansible/myinv_project2 all -m ping -u ubuntu --private-key /etc/ansible/PROD.pem

Jenkins

Package
cd /var/lib/jenkins/workspace/package/
sudo docker build --file Dockerfile --tag prabhu2527/samplejavaapp:$BUILD_NUMBER .
sudo docker login -u prabhu2527 -p $DOCKER_HUB_PWD
sudo docker push prabhu2527/samplejavaapp:$BUILD_NUMBER
cd ./install
sudo ansible-playbook --inventory /etc/ansible/myinv --private-key /etc/ansible/PROD.pem Docker_run.yml -u ubuntu --extra-vars "env=qa tag=prabhu2527/project1:$BUILD_NUMBER"
