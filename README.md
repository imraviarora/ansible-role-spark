# ansible-spark
Ansible role to deploy a spark cluster

# Installation

0) Install dependencies, this role requires java be installed on the nodes. My advice would be to use galaxy:

```
sudo ansible-galaxy install geerlingguy.java
```

1) Simply clone this repo to /etc/ansible/roles

```
sudo git clone git@github.com:imraviarora/ansible-role-spark.git /etc/ansible/roles/ansible-spark
```

2) modify your hosts inventory file (either /etc/ansible/hosts or your own file somewhere to be referenced by the playbook with -i) to something like this:

```
[all]
mn1
dn1
dn2

[masters]
mn1

[zookeepers]
mn1

[spark-masters]
mn1

[slaves]
dn1
dn2

[spark-workers]
dn1
dn2
```

3) create a playbook file somewhere (eg. ~/spark.yml):

```
- name: spark master setup
  hosts: all
  roles:
    - role: ansible-spark
```

4) run the playbook

```
ansible-playbook -i hosts spark.yaml
```

5) validate cluster

navigate a web browser to http://<master>:7077/ and you should see the spark master panel and all of the workers defined registered.

6) test cluster

```
/opt/spark/spark-2.0.0-bin-hadoop2.7/bin/spark-submit  \
  --master spark://<master>:7077  \
  --supervise   
  --class org.apache.spark.examples.SparkPi  \
  /opt/spark/spark-2.0.0-bin-hadoop2.7/examples/jars/spark-examples_2.11-2.0.0.jar   1000
```

# to-do

- integrate zookeeper
- add ubuntu support
