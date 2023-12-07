$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$Ansible$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$

-->install ansible

	sudo apt update -y
	sudo apt install ansible -y


---->create the ansible key in .ssh folder
	:cd .ssh
	:ll
		authorised_keys
	
	:vim ansible-keys
 
	-----Paste PRIVATE KEY-----

--->connect to the slave node using ansible created.

	:sudo ssh -i ~/.ssh/ansible_keys ubuntu@52.66.120.212(public-IP of slave)


--->create an inventory as below or ---we can create mkdir ansible--->vim hosts any where inthe ec2-server.
	:cd /etc/ansible.
	:vim hosts
		
 		[servers]
		Host1 server_host=52.66.120.212
		Host2 server_host=43.205.178.227
		Host3 server_host=15.206.124.171
		
	---> To install python also in all 3 servers.

		[all:vars]
		ansible_python_interpreter=/usr/bin/python3


--->To check the host by using below command

	:ansible-inventory --list -y

		all:
 		 children:
   		 servers:
      		hosts:
        		server1:
          		ansible_host: 52.66.120.212
          		ansible_python_interpreter: /usr/bin/python3
        		server2:
          		ansible_python_interpreter: /usr/bin/python3
          		server_host-2: 43.205.178.227
        		server3:
          		ansible_python_interpreter: /usr/bin/python3
          		server_host-3: 15.206.124.171
    		ungrouped: {}
----> ping all the remot server connected

	:ansible all -m ping.

	--->Getting below error

		server2 | UNREACHABLE! => {
       		"changed": false,
      		"msg": "Failed to connect to the host via ssh: ssh: Could not resolve hostname server2: Temporary failure in name resolution",
      		"unreachable": true
		}
		The authenticity of host '52.66.120.212 (52.66.120.212)' can't be established.
		ECDSA key fingerprint is SHA256:GQZtRyM/v+ByDOdAaL5ImQCp2fsz18bAMxLTsjBMD4A.
		Are you sure you want to continue connecting (yes/no/[fingerprint])? server3 | UNREACHABLE! => {
		    "changed": false,
		    "msg": "Failed to connect to the host via ssh: ssh: Could not resolve hostname server3: Temporary failure in name resolution",
		    "unreachable": true
		}
		yes
		server1 | UNREACHABLE! => {
		    "changed": false,
    		"msg": "Failed to connect to the host via ssh: Warning: Permanently added '52.66.120.212' (ECDSA) to the list of known hosts.\r\nubuntu@52.66.120.212: Permission denied (publickey).",
    		"unreachable": true
		}
	
      ------>>We did not added the private key in all the servers so we have to sepecify the private key and the hosts path in the command.
		:ansible all -m ping -i /etc/ansible/hosts  --private-key=~/.ssh/ansible_keys
			ERROR:Pivate key files are NOT accessible by others

	----->>Go to home and give access to the .ssh and ansible keys
			 chmod 700 ~/.ssh
			 chmod 600 ~/.ssh/ansible_keys

	------>>We have to run the command to check the connection between the servers.
		:ansible all -m ping -i /etc/ansible/hosts  --private-key=~/.ssh/ansible_keys.

		o/p:::--
			server2 | SUCCESS => {
 			   "changed": false,
 			   "ping": "pong"
			}
			server1 | SUCCESS => {
			    "changed": false,
			    "ping": "pong"
			}
			server3 | SUCCESS => {
			    "changed": false,
			    "ping": "pong"
			}

----->by above we sucessfully connect 3 server using ansible master and now we can update the configurations tasks using the yml files....

---> To check all servers disk space from the master ansible.

	:ansible all -a "free -h" -i /etc/ansible/hosts  --private-key=~/.ssh/ansible_keys
		
		o/p::--

		server3 | CHANGED | rc=0 >>
		               total        used        free      shared  buff/cache   available
		Mem:           949Mi       169Mi       322Mi       0.0Ki       457Mi       623Mi
		Swap:             0B          0B          0B
		server1 | CHANGED | rc=0 >>
		               total        used        free      shared  buff/cache   available
		Mem:           949Mi       170Mi       320Mi       0.0Ki       458Mi       622Mi
		Swap:             0B          0B          0B
		server2 | CHANGED | rc=0 >>
		               total        used        free      shared  buff/cache   available
		Mem:           949Mi       175Mi       283Mi       0.0Ki       490Mi       615Mi
		Swap:             0B          0B          0B 			
			

---> To check all servers uptime from the master ansible.

	:ansible all -a "uptime" -i /etc/ansible/hosts  --private-key=~/.ssh/ansible_keys

	o/p::--
		server2 | CHANGED | rc=0 >>
		 06:56:20 up  3:36,  1 user,  load average: 0.00, 0.00, 0.00
		server1 | CHANGED | rc=0 >>
		 06:56:20 up  3:36,  1 user,  load average: 0.00, 0.00, 0.00
		server3 | CHANGED | rc=0 >>
		 06:56:20 up  3:36,  1 user,  load average: 0.00, 0.00, 0.00
