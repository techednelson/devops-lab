# vagrant-devops-lab

This project provision an ubuntu vm with pre-installed Jenkins, Docker, Kubernetes & git. The main purpose is to test locally
CI/CD pipelines before deploying on production. You can check this repo [spring-boot-hello-world-devsecops-ci-cd](https://github.com/techednelson/spring-boot-hello-world-devsecops-ci-cd)
to see a devops CI/CD pipeline in action

## Run the project

From root folder run the following commands:

- `vagrant up`
- `vagrant ssh`
- `microk8s status --wait-ready && microk8s config > .kube/config`
- Run `sudo cat /var/lib/jenkins/secrets/initialAdminPassword` & copy the admin password you see in the CLI

## Setup Jenkins

- Open in the web browser `http://192.168.70.10:8080`. Enter `admin` as username and `the password you jut copied before`.
- Create a new Jenkins user.
- install recommended plugins & you are ready to start using jenkins, create and test pipelines locally.

## Important Note
Everytime you stop & start the vagrant instance you need to run `vagrant ssh` from root folder and once inside the vm instance, run the command `sudo chmod 666 /var/run/docker.sock` to avoid issues with docker

Happy CI/CD pipelines!!!