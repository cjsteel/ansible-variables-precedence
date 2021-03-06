ansible-variables-precedence
============================

Example to understand variables precedence of Ansible

# What I have do ?
- I have create a variable everywhere defined everywhere (see the first commit)
- I have rename the variable "everywhere" with the most precedence at each launch

# Result in the order :
- vars.yml
- 'include' in example.yml
- 'vars' in included.yml
- 'vars' in example.yml
- localhost, a french production server in prod/ansible_hosts
- a french server production in prod/ansible_hosts
- a european server production in prod/ansible_hosts
- a server production in prod/ansible_hosts
- host_vars/localhost
- groups_var/france
- groups_var/europe
- groups_var/all
- prod/host_vars/localhost
- prod/groups_var/france
- prod/groups_var/europe
- prod/groups_var/all

# Test test :
    ansible-playbook -i prod/ansible_hosts example.yml

# Playbook result :
```
PLAY [all] ******************************************************************** 

GATHERING FACTS *************************************************************** 
ok: [localhost]

TASK: [Who is the first priority variable ?] ********************************** 
ok: [localhost] => {
    "msg": "priority1 say = I am priority1 defined in vars.yml"
}

TASK: [Who is the second priority variable ?] ********************************* 
ok: [localhost] => {
    "msg": "priority2 say = I am priority2 defined during 'include' in example.yml"
}

TASK: [Who is the third priority variable ?] ********************************** 
ok: [localhost] => {
    "msg": "priority3 say = I am priority3 defined with 'vars' in included.yml"
}

TASK: [Who is the 4th priority variable ?] ************************************ 
ok: [localhost] => {
    "msg": "priority4 say = I am everywhere defined with 'vars' in example.yml"
}

TASK: [Who is the 5th priority variable ?] ************************************ 
ok: [localhost] => {
    "msg": "priority5 say = I am everywhere defined for localhost, a french production server in prod/ansible_hosts"
}

TASK: [Who is the 6th priority variable ?] ************************************ 
ok: [localhost] => {
    "msg": "priority6 say = I am everywhere defined for a french server production in prod/ansible_hosts"
}

TASK: [Who is the 7th priority variable ?] ************************************ 
ok: [localhost] => {
    "msg": "priority7 say = I am everywhere defined for a european server production in prod/ansible_hosts"
}

TASK: [Who is the 8th priority variable ?] ************************************ 
ok: [localhost] => {
    "msg": "priority8 say = I am everywhere defined for a server production in prod/ansible_hosts"
}

TASK: [Who is the 9th priority variable ?] ************************************ 
ok: [localhost] => {
    "msg": "priority9 say = I am everywhere defined in host_vars/localhost"
}

TASK: [Who is the 10th priority variable ?] *********************************** 
ok: [localhost] => {
    "msg": "priority10 say = I am everywhere defined in groups_var/france"
}

TASK: [Who is the 11th priority variable ?] *********************************** 
ok: [localhost] => {
    "msg": "priority11 say = I am everywhere defined in groups_var/europe"
}

TASK: [Who is the 12th priority variable ?] *********************************** 
ok: [localhost] => {
    "msg": "priority12 say = I am everywhere defined in groups_var/all"
}

TASK: [Who is the 13th priority variable ?] *********************************** 
ok: [localhost] => {
    "msg": "priority13 say = I am everywhere defined in prod/host_vars/localhost"
}

TASK: [Who is the 14th priority variable ?] *********************************** 
ok: [localhost] => {
    "msg": "priority14 say = I am everywhere defined in prod/groups_var/france"
}

TASK: [Who is the 15th priority variable ?] *********************************** 
ok: [localhost] => {
    "msg": "priority15 say = I am everywhere defined in prod/groups_var/europe"
}

TASK: [Who is the 16th priority variable ?] *********************************** 
ok: [localhost] => {
    "msg": "priority16 say = I am everywhere defined in prod/groups_var/all"
}

PLAY RECAP ******************************************************************** 
localhost                  : ok=17   changed=0    unreachable=0    failed=0   
```

# What is missing :
- Facts variables
- Roles variables
- Register variables
