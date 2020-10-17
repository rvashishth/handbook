a Jenkinsfile is always required to be present in project

we can either write pipeline dsl code in Jenkinsfile, or write a custom shared library which later needs to be referred from Jenkinsfile
https://www.jenkins.io/doc/book/pipeline/shared-libraries/

Define custom steps in shared library files(https://www.jenkins.io/doc/book/pipeline/shared-libraries/#defining-custom-steps)

we can modify shared library code from rply pipeline syntax

https://www.jenkins.io/doc/book/pipeline/development/

https://jenkins.org.com/project/blue/pipelines/ - create new pipeline on blueocean

- each file in vars define one entire pipeline in call() method

3e0648914784ba46353140aca2e285bc350859f9 - github personal token

Why do we have **IntelliJ IDEA Github Integration Plugin access token** added on github

may be we can use filesystem github

https://github.org.com/settings/tokens what are github personal access tokens

LAMU02Y80D8JG5J:~ rvashish\$ docker network create jenkins

http://localhost:8080/script - to test jenkins scripts

Kubernetes plugin - Jenkins plugin to run dynamic agents in a Kubernetes cluster. </br>
https://plugins.jenkins.io/kubernetes/ </br>
jenkins.steps.podTemplate() - define the pod spec with list of containers
jenkins.steps.container() - define the container in which below steps will run

Jenkins pipeline

- Each stage is being displayed as one column in UI
- https://www.jenkins.io/doc/pipeline/steps/workflow-basic-steps/ we can execute basis steps on `jenkins` object
- Pipeline > stages > stage > steps

  ```json
    pipeline {
      node {
        stages {
          stage {
            steps
          }
        }
      }
    }
  ```

- without node, a Pipeline cannot do any work! From within node, the first order of business will be to checkout the source code for this project.

Question -

- it seems declarative pipeline require agent directive, where do we have agent directive in our pipeline code?
- what to access using `jenkins.steps` and `jenkins` i.e. `jenkins.echo, jenkins.stash, jenkins.unstash`
- jenkins.steps.checkout - where do i get list of such steps
- can we invoke any plugin after jenkins.steps

https://hub.docker.com/r/alpine/helm - only helm
https://hub.docker.com/_/microsoft-azure-cli - only azure cli
https://hub.docker.com/r/ams0/az-cli-kubectl-helm/ - 2 years old
https://github.com/helm/docker-kubectl-helm-az - has helm 2
https://hub.docker.com/r/alpinelinux/docker-cli
https://hub.docker.com/r/ventx/terraform-azurecli-kubectl-helm - **working candidate** but not be verified publisher

https://hub.docker.com/r/vrahul/docker-terraform-kubectl-helm-azurecli

We can also define a docker file, so jenkins can build and run a docker image
Q. Can i use multiple containers as a single container in jenkins

# Local setup steps

1. must have docker desktop with kubernetes running in local
2. Create local volume mount for jenkins.Mount the Docker API socket as a volume. When the pipeline runs docker commands, they actually get executed on the same Docker Engine where Jenkins itself is running:
3. Create `jenkins_local` dir, under which create `jenkins_home` and keep this `docker-compose.yml` file
4. Create a docker compose file for jenkins to mount volume </br>
   `docker-compose up -d`
5. install kubernetes, azure credentials, blue ocean plugins, docker swarm
6. setup kubernetes cluster with local docker k8s
7. create copy of shared-libraries inside jenkins volumes
8. incase u shutdown then in jenkins u have to do chmod again for docker.sock `chmod 666 var/run/docker.sock`
9. add some config for pod template </br>
   `volumes: [jenkins.steps.hostPathVolume(hostPath: '/var/run/docker.sock', mountPath: '/var/run/docker.sock')]`
10. References
    1.  https://birthday.play-with-docker.com/jenkins-docker-hub/

Parameterized
Lockable resources
Build Steps
Post Build Actions
Build Agents
We can export test results on post build actions
One job can call other job, can also send parameters
Archive a file to display on jenkins job

Chmod/chown files/folder of a container in a pod template for my jenkins user
work as root user, gid, uid to 0

Example docker withCredentials
https://github.com/sixeyed/pi/blob/bday7/Jenkinsfile

DevOps 2.6 Toolkit Jenkins X Cloud Native Kubernetes - book