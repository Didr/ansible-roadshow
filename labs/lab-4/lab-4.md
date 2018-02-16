# Writing the load balancer playbook

In the previous lab, we created two wildfly swarm servers running our application. Next step is to setup a loadbalancer. We will use nginx as the loadbalancer in this lab.

In this case a role has already been written for installing nginx. The role can be found at the Ansible Galaxy site (https://galaxy.ansible.com/nginxinc/nginx/). [https://galaxy.ansible.com](Ansible Galaxy) is the place where roles and modules are shared. As always be critical when using content on the internet. In this case the nginx organisation has made an official role for nginx, so we should be good.

First we need to install the role for nginx. Run:

```
$ansible-galaxy install nginxinc.nginx
```

and wait for the role to be installed. When that is done, we can use the role in our playbooks.

To install nginx go to $WORK_DIR and create a new file with the following content:

```
---
- hosts: lbservers
  user: "{{host_user}}"
  become: true
  tasks:
  - include_role:
      name: nginxinc.nginx
```

run the playbook with the command

```
$ansible-playbook --extra-vars "host_user=$MY_HOST_USER" lb.yml
```

this will install nginx on the servers in the lbservers group. To verify the installation, go to the url *http://<lb server name>*. You should get the nginx default page.

Next step is to configure nginx as a loadbalancer for the two wildflyapp servers.