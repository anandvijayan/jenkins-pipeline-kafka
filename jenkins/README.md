#Jenkinsfile groovy script

The purpose of this script was to build and deploy the source code from a jenkins 2.0 pipeline job. It performs three main steps
## Pulls code from git
<pre>git url: 'https://github.com/anandvijayan/jenkins-pipeline-kafka.git'</pre>
## Performs maven build
<pre>sh 'mvn clean install'</pre>
## Deploys code to repository
<pre>sh 'mvn deploy:deploy'</pre>