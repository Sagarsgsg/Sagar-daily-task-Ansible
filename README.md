$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$Ansible$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$

-->install ansible

	sudo apt update -y
	sudo apt install ansible -y


---->create the ansible key in .ssh folder
	:cd .ssh
	:ll
		authorised_keys
	
	:vim ansible-keys
		
	-----BEGIN RSA PRIVATE KEY-----
MIIEogIBAAKCAQEAtPxSVNqrX4S4uA17H3byQ8O9bL1gyQ6Nlpl3/fc8DIBE3dB1
XcWZQJzD3FVj0JnSA1q/vxJXyGNK3jZVzHG73uRFUe43N28oQI0KPu4L1aR7h8+S
jcPOgNwQmEo9qQRxJHSGLBqktkWEg9SE4WI1aRzFd47qYxpw5QAYZ+SDRUtbBIOR
RpvTvynRowwH40uLhSMsvtHZWCLhzKjN8ikvWUMrg92caa8RRnjHgEWPG+r0PNA/
tkxxck0jYhOKFpeTyfvLFQevUN+uFRH/zzg0pDnVC4AdRgEpNvJRbLjSYkjoO2+Q
Px1aTeV2SgZ0PY6O1VLiXjdORTUz1htlMBf/vwIDAQABAoIBAFN/xgxYBpC/Dun7
bj6KBiO1fwNYK/sWt8Qvcei90/qAg0VDE6L7s0TYDpTs4GwxS284wxZIRC+zf6sd
rl/waRjggArYuKjeo9eEOqHl+ZfLlyKFZbv6Bp/058MbHW+JGRoUmIJomG1vjT+1
IMlLIhEosQID8adfX46HiLsF9npVDfD5DsuikaHgdGdyGK8HdmweQ01RVQ5180iT
wXxnzIDIWMuOTIE/qfYXc0Lne8iLEJl7kjM95eeVCLVRQ+Z2yhJrBm2dM8jpf6jo
vdg5JLDFp6lqp+0l9mCNtTIwjO1QGvGhJWxLwaY2pc+rOAPRdnK+ypUyZH81q4z+
bvdXnoECgYEA8WDEQRrIhkaT3PulWyRVMiWN1IJ3F1sHJlh9bzSXuoUfJSmD81Gy
JBfm62MPFxN3p7VsruTFHcu2cyhEW7KMuuiz1rMt2y7rWHDEqq+SV1de1C5D0xHy
QInHA+rtMM8R4CBSSnxPWP02etUe2KZyojW32Z9FWWZrY5fldhPfcNECgYEAv/MB
HljbWirX8WdNv2WnbUppJzvgAQRL6Pu/LoM9DWTqIgXk0bQ28nvxlxLK2dcmYKQj
92CmPM/Y9qYqrebqS+pqdTTg9nSuMPo4ExvdBqKwMSHl4k0jO6m2ySfuhYR5X/En
CMEin0D3fbkanTCu2olpixmH56YK+9/542k/C48CgYAlUSSGPDHMMJUjkPQbhx50
xkGLHTB0N+p/Dyc1Thg3CeWqxSRVNcgotFlLAuGOW8Af+Xh4AX5IAPqQCyWmV5BS
RS+ofcMVI8fWwHhnOwWQ4z0R6KUruJaPS72s6WEKLrlAwT53rYhG5b7KdrByJimK
0ms+BdWe4KqxlwYunxvoQQKBgBwa/MLwx82AEHZsZdnsjINYLeVswvPjKSpIEkpB
NSNM72tj6Yk7FgCTXWB9g+45rojf/9Qs9qpY1K/ozL8B3LSY8lWPFJGBrC/Hs4Y4
wjhCggHvsLVeDhaiVv1FN4udRhhiOxDxFpyy6ooiHF9/zVp6XFXduySUD2+p7/D4
bB7JAoGAGoE4LcDC6+YaZsDVqLLwdxwDVSrVX+MSEg+W38npFNuJNhs1eoMRhIXg
rpOcUWadbDFx+YEYU/5d4gu7JWTpLYB3ty1/qoCrb3M1n0w1ccaAHqeF8j0f4CIk
m80PNugLePPVRxyVHuSzkkmctLZmMi+pO3i1LLXKo0VlK8irx8U=
-----END RSA PRIVATE KEY-----

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
