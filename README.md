#Jenkins Pipeline using Kafka

The purpose of this wiki page was to demonstrate the complete release management workflow using Jenkins pipelining using Kafka to asynchronously deploy applications to servers in different landscapes. Ansible is used to schedule the deployment to occur at a stipulated time in future. There are several steps involved in accomplishing this.
Please visit the [Wiki Page](https://github.com/anandvijayan/jenkins-pipeline-kafka/wiki) for a more details.

##Pre-requisites
1. Jenkins 2.0 has already been installed on jenkinshost 
2. Ansible has been installed on jenkinshost and ssh tunnel has been setup from jenkinshost to apphost.
3. Kafka server and topics are running on kafkahost.

   3.1 Start Server
<pre>bin/kafka-server-start.sh config/server.properties</pre>
   3.2 Start topic
<pre>bin/kafka-topics.sh --create --zookeeper zoohost:2181 --replication-factor 3-partitions 1 --topic JenkinsPipeline
4. Zookeper server is running on installed on zoohost
<pre>bin/zookeeper-server-start.sh config/zookeeper.properties</pre>
5. Kafka producer is available on jenkinshost and kafka consumer is available on apphost

##Steps involved in the setup
1. Jenkins checkout code from scm, runs a maven build and deploys artifacts (ear/war/zip) to a repository.
2. After the deployment a kafka producer queues up a message in format LANDSCAPE|ARTIFACTNAME (e.g. DEV|M.ear)
<pre>echo -e "DEV|M.ear" | bin/kafka-console-producer.sh --broker-list kafkahost:9092 --topic JenkinsPipeline</pre>
3. Ansible sends a message to apphost for a scheduled deployment in 20 minutes
<pre>
-at:
  script:bash -lc "bin/kafka-console-consumer.sh --zookeeper kafkahost:2181 --topic JenkinsPipeline --from-beginning |  grep DEV|M.ear > /tmp/DeployStatus"
  unique:true
  count:20
  units:minutes
</pre>
4. After 20 minutes the kafka consumer wakes up and checks the kafka queue for a pertinent message in format LANDSCAPE|ARTIFACTNAME (e.g. DEV|M.ear)
<pre>bin/kafka-console-consumer.sh --zookeeper kafkahost:2181 --topic JenkinsPipeline --from-beginning | grep DEV|M.ear</pre>
5. If a message is found then the app deployment is performed on app host and the logs are pushed to log monitoring tool (Splunk/ELK) that in turn sends out email/text notifications.

## References
1. [Jenkins 2.0](https://jenkins.io/2.0/)
2. [Jenkins Pipeline ](https://github.com/jenkinsci/pipeline-plugin/blob/master/TUTORIAL.md)
3. [Ansible Documentation] (https://docs.ansible.com/ansible/index.html)
4. [Ansible Playbook Examples](https://github.com/ansible/ansible-examples)
5. [Kafka Quick Start] (http://kafka.apache.org/documentation.html#quickstart)
