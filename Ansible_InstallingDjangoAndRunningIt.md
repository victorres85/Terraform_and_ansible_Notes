On the code below we will be installing Python and the virtualenv to our machine, we will then activate the virtualenv and pip install some libraries (Django and Django rest framework).

We will then open a new shell (everytime the instruction `shell:` is used a new shell is open) activate our virtualenviroment and start our django project

```
- hosts: terraform-ansible
  tasks:
  - name: installing python3 and virtualenv
    apt:
       pkg:
       - python3
       - virtualenv
       update_cache: yes
    become: yes
  - name: installing dependencies with pip (Django and DRF)
    pip:
      virtualenv: /home/ubuntu/project/venv
      name:
        - django
        - djangorestframework
  - name: Starting project
    shell: '. /home/ubuntu/project/venv/bin/activate; django-admin startproject setup /home/ubuntu/project/'
  - name: Entering the Settings.py and changing the Allowed_Hosts 
    lineinfile:
      path: /home/ubuntu/project/setup settings.py
      regexp: 'ALLOWED_HOSTS'
      line: 'ALLOWED_HOSTS = ["*"]'
      backrefs: yes
  - name: Applying runserver
    shell: '. /home/ubuntu/project/venv/bin/activate; nohup python /home/ubuntu/project/setup/manage.py runserver &'
```

`apt:` works like the apt-get from ubuntu, is the command to make a installation

`pkg:` tells ansible that we will be installing a package, this should then be followed by the the package names with a dash before the names

`update_cache: yes` will give the instruction to update the packages making sure we always use the most updated version

`become: yes` allows ansible to execute this instruction as sudo (admin)

`pip:` executes a pip install command

`virtualenv:` here we have to pass the full path where we want our viirtualenv to be created, the virtualenv will then be created and activate before the pip install start running, below it as we have seem before we have to pass the tag `name:` with the libraries to be installed under it with a dash before the name

`lineinfile:` used to modify a line inside a file we have to pass the following instructions to it: `path`, `regexp`, `line` and `backrefs`

- `path:` the full path to the file
- `regexp:` regular expression to locate the line
- `line:` the new line to be added the content has to be inside quotation mark
- `backrefers:` yes tells ansible that in case it does not find the regular expression it should do nothing
