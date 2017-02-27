#Ansible playbook for Kafka producer and consumer

The purpose of the ansible playbook was to produce and consume messages from a kafka queue. 

## Producer
Add application built from Dev branch (DEV|M.ear) to kafka queue for scheduled deployment
<pre>ansible-playbook -i inventory --extra-vars "landscape_name=DEV deploy_artifact_name=M.ear" jpk-producer.yml</pre>
Add application built from Release branch (RELEASE|M.ear) to kafka queue for scheduled deployment
<pre>ansible-playbook -i inventory --extra-vars "landscape_name=RELEASE deploy_artifact_name=M.ear" jpk-producer.yml</pre>
## Consumer
Fetch application built from Dev branch (DEV|M.ear) from kafka queue for scheduled deployment after 20 minutes into DEV app server
<pre>ansible-playbook -i inventory --extra-vars "landscape_name=DEV deploy_artifact_name=M.ear schedule_count=20 schedule_units=minutes" --limit kafkaclientdev jpk-consumer.yml</pre>
Fetch application built from Dev branch (DEV|M.ear) from kafka queue for scheduled deployment after 150 minutes into QA/INT app servers
<pre>ansible-playbook -i inventory --extra-vars "landscape_name=DEV deploy_artifact_name=M.ear schedule_count=150 schedule_units=minutes" --limit kafkaclientqa,kafkaclientint jpk-consumer.yml</pre>
Fetch application built from Release branch (RELEASE|M.ear) from kafka queue for scheduled deployment after 3 hours into STAGE app server
<pre>ansible-playbook -i inventory --extra-vars "landscape_name=RELEASE deploy_artifact_name=M.ear schedule_count=3 schedule_units=hours" --limit kafkaclientstage jpk-consumer.yml</pre>
