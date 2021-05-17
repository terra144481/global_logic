## Task 1

## Prerequisites

- 4 linux machines (VMs, instances in Digital Ocean, GCP, AWS EC2, dosen't matter);
- Python installed (2.7 or 3.5+);
- established password-less connection from one machine (controller) to others;


* Create a inventory file with four groups, each provisioning VM should be in separate group and group named `iaas` what should include childrens from two first groups;
* Create reusable roles for that:
- creating a empty file `/etc/iaac` with rigths `0500`  
- fetch a linux distro name/version  
* Create playbook for:
- invoke the role for `/etc/iaac` for hosts group `iaas`
- invoke the role for defining variable for all hosts
- print in registered variables
- printing hostnames together with registered variables will be a plus.
* Create a repo in your GitHub account and commit code above



1) Creating 4 virtual PC in VirtualBox. Creating ssh keys. Check connections. Install ansible vs python on master and slaves machines, check version.   

`user@ubuntu:~$ pwd`  
`/home/user`  
`user@ubuntu:~$ sudo apt-get update`  
`[sudo] password for user:`  

`Hit:1 http://ppa.launchpad.net/ansible/ansible/ubuntu xenial InRelease`  
`Get:2 http://security.ubuntu.com/ubuntu xenial-security InRelease [109 kB]`  
`Hit:3 http://us.archive.ubuntu.com/ubuntu xenial InRelease`  
`Get:4 http://us.archive.ubuntu.com/ubuntu xenial-updates InRelease [109 kB]`  
`Get:5 https://esm.ubuntu.com/infra/ubuntu xenial-infra-security InRelease [7,494 B]`  
`Get:6 http://us.archive.ubuntu.com/ubuntu xenial-backports InRelease [107 kB]`  
`Get:7 https://esm.ubuntu.com/infra/ubuntu xenial-infra-updates InRelease [7,475 B]`  
`Get:8 http://us.archive.ubuntu.com/ubuntu xenial-updates/main amd64 Packages [2,049 kB]`  
`Get:9 http://us.archive.ubuntu.com/ubuntu xenial-updates/main i386 Packages [1,525 kB]`  
`Get:10 http://us.archive.ubuntu.com/ubuntu xenial-updates/universe amd64 Packages [1,219 kB]`  
`Get:11 http://us.archive.ubuntu.com/ubuntu xenial-updates/universe i386 Packages [1,086 kB]`  
`Fetched 6,219 kB in 3s (1,872 kB/s)`  
`Reading package lists... Done`

`user@ubuntu:~$ ansible --version`  
`ansible 2.9.21`  
 `config file = /etc/ansible/ansible.cfg`  
 `configured module search path = [u'/home/user/.ansible/plugins/modules', u'/usr/share/ansible/plugins/modules']`  
 `ansible python module location = /usr/lib/python2.7/dist-packages/ansible`  
 `executable location = /usr/bin/ansible`  
 `python version = 2.7.12 (default, Mar 1 2021, 11:38:31) [GCC 5.4.0 20160609]`  

`user@ubuntu:~$ ls -a`  
`. .. ansible .ansible .bash_history .bash_logout .bashrc .cache .nano .profile .ssh .sudo_as_admin_successful .Xauthority`  

`user@ubuntu:~$ ssh user@192.168.1.20`  
`Welcome to Ubuntu 16.04.7 LTS (GNU/Linux 4.4.0-186-generic x86_64)`  
``   
 `\* Documentation: https://help.ubuntu.com`  
 `\* Management:   https://landscape.canonical.com`  
 `\* Support:    https://ubuntu.com/advantage`  
``   
 `\* Pure upstream Kubernetes 1.21, smallest, simplest cluster ops!`  
`` 
   `https://microk8s.io/`  
``   
`120 packages can be updated.`  
`83 updates are security updates.`  
`` 
`New release '18.04.5 LTS' available.`  
`Run 'do-release-upgrade' to upgrade to it.`  
``   
`Last login: Mon May 17 09:45:49 2021 from 192.168.1.22`  

`user@ubuntu:~$ ansible --version`  
`ansible 2.9.20`  
 `config file = /etc/ansible/ansible.cfg`  
 `configured module search path = [u'/home/user/.ansible/plugins/modules', u'/usr/share/ansible/plugins/modules']`  
 `ansible python module location = /usr/lib/python2.7/dist-packages/ansible`  
 `executable location = /usr/bin/ansible`  
 `python version = 2.7.12 (default, Mar 1 2021, 11:38:31) [GCC 5.4.0 20160609]`  
`user@ubuntu:~$ exit`  
`logout`  
`Connection to 192.168.1.20 closed.`  

2) Creating a inventory file with four groups, each provisioning VM should be in separate group and group named `iaas` what should include childrens from two first groups  

`user@ubuntu:~$ pwd`  
`/home/user`  
`user@ubuntu:~$ cd ansible`  
`user@ubuntu:~/ansible$ ls -a`  
`. .. ansible.cfg group_vars hosts.txt playbook1.yml playbook_gl1.yml playbooktest.yml playbookvos.yml roles`  

`user@ubuntu:~/ansible$ cat ansible.cfg`  
`[defaults]`  
`host_key_cheking = false`  
`inventory    =./hosts.txt`  
`roles_path    =./roles`  

`user@ubuntu:~/ansible$ nano hosts.txt`  
`[group1]`  
`linux1 ansible_host=192.168.1.20`  
``   
`[group2]`  
`linux2 ansible_host=192.168.1.21`  
``   
`[group3]`  
`linux3 ansible_host=192.168.1.30`   

`[iaas:children]`  
`group1`  
`group2` 

`user@ubuntu:~/ansible/group_vars$ ls`  
`group1 group2 group3`  

`user@ubuntu:~/ansible/group_vars$ cat group1`  
`\---`  
`ansible_user        : user`  
`ansible_ssh_private_key_file: /home/user/.ssh/id_rsa`    

3) Create reusable roles for that:
- creating a empty file `/etc/iaac` with rigths `0500`  
- fetch a linux distro name/version  

`user@ubuntu:~/ansible/roles$ mkdir -p role_create_file/tasks && touch role_create_file/tasks/main.yml'
`user@ubuntu:~/ansible/roles$ mkdir -p role_vos/tasks && touch role_vos/tasks/main.yml`
`user@ubuntu:~/ansible/roles/role_create_file/tasks$ nano main.yml`
`\---`    
`\- name: create empty file`    
 `file:`    
  `path: "/etc/iaac"`    
  `state: touch`    
  `mode: 0500`  

`user@ubuntu:~/ansible/roles/role_vos/tasks$ nano main.yml`
`\---`    
 `\- name: Check Dist Version`    
  `shell: cat /etc/os-release`    
  `register: response`    
 `\- debug: msg="{{ response.stdout }}"`    

`user@ubuntu:~/ansible/roles$ tree`    
`.`    
`├── role_create_file`    
`│  └── tasks`    
`│    └── main.yml`    
`└── role_vos`    
  `└── tasks`    
​    `└── main.yml`    
`4 directories, 2 files`    
    
`user@ubuntu:~/ansible/roles/role_vos/tasks$ cat main.yml`    

`user@ubuntu:~/ansible$ tree` 
`.`    
`├── ansible.cfg`    
`├── group_vars`    
`│  ├── group1`    
`│  ├── group2`    
`│  └── group3`    
`├── hosts.txt`    
`├── playbook1.yml`    
`├── playbook_gl1.yml`    
`├── playbooktest.yml`    
`├── playbookvos.yml`    
`└── roles`    
  `├── role_create_file`    
  `│  └── tasks`    
  `│    └── main.yml`    
  `└── role_vos`    
​    `└── tasks`    
​      `└── main.yml`    
`6 directories, 11 files`    

4) Creating playbook for:
- invoke the role for `/etc/iaac` for hosts group `iaas`
- invoke the role for defining variable for all hosts

`user@ubuntu:~/ansible$ cat playbook_gl1.yml`  
`\---`  
`\- name: create empry file`  
 `hosts: iaas`  
 `become: yes`  
 `roles:`  
  `\- role_create_file`  
`\- name: print linux distro name/version`  
 `hosts: all`  
 `become: yes`  
 `roles:`  
  `\- role_vos`    

  Results: 
  `user@ubuntu:~/ansible$ cat playbook1.yml`  
`\---`  
`\- name: test ping all servers`  
 `hosts: all`  
 `become: yes`  
``   
 `tasks:`  
  `\- name: ping all servers`  
   `ping:`  
  
  `user@ubuntu:~/ansible$ ansible-playbook playbook1.yml`  
` `` `   
`PLAY [test ping all servers] ************************************************************************************************************`  
`TASK [Gathering Facts] ******************************************************************************************************************`  
`ok: [linux1]`  
`ok: [linux3]`  
`ok: [linux2]`  
``   
`TASK [ping all servers] *****************************************************************************************************************`  
`ok: [linux1]`  
`ok: [linux3]`  
`ok: [linux2]`  
` ``    
`PLAY RECAP ******************************************************************************************************************************`  
`linux1           : ok=2  changed=0  unreachable=0  failed=0  skipped=0  rescued=0  ignored=0`  
`linux2           : ok=2  changed=0  unreachable=0  failed=0  skipped=0  rescued=0  ignored=0`  
`linux3           : ok=2  changed=0  unreachable=0  failed=0  skipped=0  rescued=0  ignored=0`  
` `` `   


`user@ubuntu:~/ansible$ ansible-playbook playbook_gl1.yml`  
` `` `   
`PLAY [create empry file] ****************************************************************************************************************` 
` `` `    
`TASK [Gathering Facts] ******************************************************************************************************************`  
`ok: [linux1]`  
`ok: [linux2]`  
` ``    
`TASK [role_create_file : create empty file] *********************************************************************************************`  
`changed: [linux1]`  
`changed: [linux2]`  
``   
`PLAY [print linux distro name/version] **************************************************************************************************`  
` ``    
`TASK [Gathering Facts] ******************************************************************************************************************`  
`ok: [linux1]`  
`ok: [linux2]`  
`ok: [linux3]`  
` `` `  
`TASK [role_vos : Check Dist Version] ****************************************************************************************************`  
`changed: [linux2]`  
`changed: [linux3]`  
`changed: [linux1]`  
` `` `
`TASK [role_vos : debug] *****************************************************************************************************************`    
`ok: [linux3] => {`
  `"msg": "NAME=\"Ubuntu\"\nVERSION=\"16.04.7 LTS (Xenial Xerus)\"\nID=ubuntu\nID_LIKE=debian\nPRETTY_NAME=\"Ubuntu 16.04.7 LTS\"\nVERSION_ID=\"16.04\"\nHOME_URL=\"http://www.ubuntu.com/\"\nSUPPORT_URL=\"http://help.ubuntu.com/\"\nBUG_REPORT_URL=\"http://bugs.launchpad.net/ubuntu/\"\nVERSION_CODENAME=xenial\nUBUNTU_CODENAME=xenial"`
`}`    
`ok: [linux1] => {`
  `"msg": "NAME=\"Ubuntu\"\nVERSION=\"16.04.7 LTS (Xenial Xerus)\"\nID=ubuntu\nID_LIKE=debian\nPRETTY_NAME=\"Ubuntu 16.04.7 LTS\"\nVERSION_ID=\"16.04\"\nHOME_URL=\"http://www.ubuntu.com/\"\nSUPPORT_URL=\"http://help.ubuntu.com/\"\nBUG_REPORT_URL=\"http://bugs.launchpad.net/ubuntu/\"\nVERSION_CODENAME=xenial\nUBUNTU_CODENAME=xenial"`
`}`    
`ok: [linux2] => {`
  `"msg": "NAME=\"Ubuntu\"\nVERSION=\"16.04.7 LTS (Xenial Xerus)\"\nID=ubuntu\nID_LIKE=debian\nPRETTY_NAME=\"Ubuntu 16.04.7 LTS\"\nVERSION_ID=\"16.04\"\nHOME_URL=\"http://www.ubuntu.com/\"\nSUPPORT_URL=\"http://help.ubuntu.com/\"\nBUG_REPORT_URL=\"http://bugs.launchpad.net/ubuntu/\"\nVERSION_CODENAME=xenial\nUBUNTU_CODENAME=xenial"`
`}`  
` `` `   
`PLAY RECAP ******************************************************************************************************************************`  
`linux1           : ok=5  changed=2   unreachable=0  failed=0  skipped=0  rescued=0  ignored=0`  
`linux2           : ok=5  changed=2   unreachable=0  failed=0  skipped=0  rescued=0  ignored=0`  
`linux3           : ok=3  changed=1   unreachable=0  failed=0  skipped=0  rescued=0  ignored=0`  
