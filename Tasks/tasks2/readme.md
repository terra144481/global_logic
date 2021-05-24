Task - hardening rule: 
1. Create and run a script (Ansible playbook) to harden users’ passwords 
by rejecting the ones that contain a username. Enforce this rule for ‘root’ 
as well.
2. [Optional] Try to implement the same hardening rule without PAM 
(pluggable authentication module).
Task 2. Configure TCP Wrappers 
• Task: 
1. Create and run a script (Ansible playbook) to configure TCP Wrappers so 
that SSH service on a test VM will be accessible only from your host 
machine and the test VM.

  
  
  Создал два плейбука, изменений не произошло, хотя плейбук отработал...   
    [playbook1](https://github.com/terra144481/global_logic/blob/main/Tasks/tasks2/ansible/playbookpam.yml)  
  Второй плейбук закрыл все соедиения от мастера к слейвам по SSH...  
    [playbook2](https://github.com/terra144481/global_logic/blob/main/Tasks/tasks2/ansible/playbookpamd.yml) 
    
   Пока не получается получить результаты целевых заданий. Буду дорабатывать...  
   
   
