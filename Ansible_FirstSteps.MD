# Starting

To start we will have to create 2 different files:

- hosts.yml
- playbook.yml

### hosts.yml

We have to add our host, and the host has to have a category

Let's say our category is called terraform-ansible we will have then to pass this name inside `[ ]` and below it pass the ip address of our aws instance, which can be found selecting our instance on the EC2 > Instance page just under `Public IPv4 address`

```yml
[terraform-ansible]
34.217.206.110
```

> IMPORTANT: always use the public IP and not the private one

### playbook.yml

First we have to describe the host which we will be working on, in our case "terraform-ansible", we will then create our tasks in small blocks, all the tasks will be under the tag `tasks:` for each different task we will asign a `name:` for our on reference and will then add the instructions

```yml
- hosts: terraform-ansible
  tasks:
  - name: create file
    copy: 
      dest: /home/ubuntu/index.html
      content: <h1>Build on Terraform and Ansible</h1>
  - name: create server
    shell: "nohup busybox httpd -f -p 8080 &"
```

> Note: using the nohup at the beggigning and the &  at the end we can make sure our server will keep runing even if we close the shell

## Instructions guide

`copy: ` is used to create a new directory/file the copy instruction require 2 parameters `dest:` ( for "destination") we have the pass here the full path of the file we want to create, starting from home; and second parameter is `content:` used to actually add content to the file we have just created

`shell:`  is used to open a shell and execute the command passed here

# Running Ansible

To run ansible we have to use the following command:

```
ansible-playbook playbook.yml -u ubuntu --private-key IaC-Victor.pem -i hosts.yml
```

### We have now create a new file called index.html inside our aws machine

if we now run on any browser the following:

http://34.217.206.110:8080/

we will then access the index page we have created
