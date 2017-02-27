#Ansible playbook for Kafka producer and consumer

The purpose of the ansible playbook was to produce and consume messages from a kafka queue. 

## Producer
Add application built from Dev branch (M-1.2.0.ear) to kafka queue for scheduled deployment
<pre>ansible-playbook -i inventory --extra-vars "deploy_artifact_name=M deploy_artifact_version=1.2.0 deploy_artifact_ext=ear" jpk-producer.yml</pre>
Add application built from Release branch (M-1.1.0.ear) to kafka queue for scheduled deployment
<pre>ansible-playbook -i inventory --extra-vars "deploy_artifact_name=M deploy_artifact_version=1.1.0 deploy_artifact_ext=ear" jpk-producer.yml</pre>
## Consumer
Fetch application built from Dev branch (M-1.2.0.ear) from kafka queue for scheduled deployment after 20 minutes into DEV app server
<pre>ansible-playbook -i inventory --extra-vars "deploy_artifact_name=M deploy_artifact_version=1.2.0 deploy_artifact_ext=ear schedule_count=20 schedule_units=minutes" --limit kafkaclientdev jpk-consumer.yml</pre>
Fetch application built from Dev branch (M-1.2.0.ear) from kafka queue for scheduled deployment after 150 minutes into QA/INT app servers
<pre>ansible-playbook -i inventory --extra-vars "deploy_artifact_name=M deploy_artifact_version=1.2.0 deploy_artifact_ext=ear schedule_count=150 schedule_units=minutes" --limit kafkaclientqa,kafkaclientint jpk-consumer.yml</pre>
Fetch application built from Release branch (M-1.1.0.ear) from kafka queue for scheduled deployment after 3 hours into STAGE app server
<pre>ansible-playbook -i inventory --extra-vars "deploy_artifact_name=M deploy_artifact_version=1.1.0 deploy_artifact_ext=ear schedule_count=3 schedule_units=hours" --limit kafkaclientstage jpk-consumer.yml</pre>
