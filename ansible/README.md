#Ansible playbook for Kafka producer and consumer

The purpose of the ansible playbook was to produce and consume messages from a kafka queue

## Produce
<pre>ansible-playbook -i inventory --extra-vars "landscape_name=DEV deploy_artifact_name=M.ear" jpk-producer.yml</pre>
## Consumer
<pre>ansible-playbook -i inventory --extra-vars "landscape_name=DEV deploy_artifact_name=M.ear" jpk-consumer.yml</pre>
