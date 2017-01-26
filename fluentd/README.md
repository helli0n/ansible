# Fluentd opportunities

### td-agent(Fluentd) monitoring:

* /var/log/messages
* /var/log/secure

# How to use Fluentd playbook <br/> 
***
### Install td-agent on host for collect loggs

**td.yml** - use this playbook for install and configure [Fluentd](http://www.fluentd.org/) cluster on CentOS 6/7 

* Playbook is doing several steps:
    1. Add docker repository from td.repo to /etc/yum.repos.d/
    2. Yum install **td-agent**
    3. Copy config **td-agent.conf** using td-agent.conf.j2 template. 
       <br/> Template has variables: 
       * {{ ansible_nodename }}
       * {{ elastic_ip }}
    4. Install fluent-plugin-elasicsearch
    5. Change TD_AGENT_USER to root (for full access to all logs)
    6. Change TD_AGENT_GROUP to wheel (for full access to all logs)
    7. Restart service and add to startup
    
+ Playbook has patameters:

Name | Parameter (extra-vars)  | Variable | Value by Default
------------- | ------------ | ------------ | ------------
Node name for tags | -  |{{ ansible_nodename }} | {{ ansible_nodename }}
Elasticsearch IP | esip | {{ elastic_ip }} | 10.1.1.10

***
### Examlples how to use playbook

add to **hosts** file parameters for authorisation: **e.g.:** 10.1.1.1 ansible_ssh_user=user1 ansible_ssh_pass=123456 ansible_sudo_pass=123456
<br/>change hosts parameter in playbook

Run in console:
~~~~
# ansible-playbook -i hosts td.yml --extra-vars "esip=10.1.1.10"
~~~~

### To do:
* add parameter for Elasticsearch port
* add to fluentd new loggs for monitoring
