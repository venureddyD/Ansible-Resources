Controll Server:
	yum install epel-release
	yum update
	yum install git python python-devel python-pip openssl ansible
	ansible --version
	
	-- Add ansible user  (Repeat the same step for remaining systems)
		-- useradd ansible
		-- passwd ansible
	-- visudo (Add the following line in root section)
		ansible ALL=(ALL)       NOPASSWD:  ALL
		
-- Password less login:
	-- login with ansible user
	-- run the 'ssh-keygen'
	-- run ssh-copy-id user@ip

	
	-- vi /etc/ansible/ansible.conf
		-- enable 
			sudo user:
			inventory location:
	-- No restarts Required
	
	Customizing Hosts file (/etc/ansible/hosts)
	[local]
	192.168.56.65
		
	[centos]
	192.168.56.65
	192.168.56.66
		
	[webservers]
	192.168.56.66		
	------------------------------------------------------------------------------------------------------------------
	Commands:

	Without Inventory:
	-------------------
	ansible all -i ip, -m ping

 	ansible all -i web01,web02 -m ping
 	To run the command on the remote system:
 	ansible webservers -m command --args 'uptime'
 	ansible all -i web01,web02 -m command --args 'sudo apt-get install -y apache2'

 	With Inventory:
	-------------------

		1 - ansible all -m ping
		list all the hosts configured in your environment
		-- ansible all --list-hosts
		2 - ansible all -a "ls -al /home/ansible"
		3 - ansible all -b -a "cat /var/log/messages"
		4 - ansible centos -m copy -a "src=test.txt dest=/tmp/test.txt "
		5 - ansible centos -b -m yum -a "name=elinks state=latest"
		6 - ansible centos -b -m user -a "user=test"
		  - ansible all -b -m user -a "name=test state=present password={{ 'welcome' | password_hash('sha512') }}"
		7 - ansible webservers -b -m yum -a "name=httpd state=latest"
		
		8 - Managing Services:
				ansible webservers -b -m service -a "name=httpd state=started"
				ansible webservers -b -m service -a "name=httpd state=restarted"
				ansible webservers -b -m service -a "name=httpd state=stopped"
			
		9 - Gathering Facts:
				ansible all -m setup | more
		To gather the Fact with format:
			- ansible centos -m setup -a 'filter=*ipv4*'
		Save the fact info in a dir(facts):
			- ansible centos -m setup --tree /tmp/facts
		
		Important Facts:
			-- ansible centos -m setup -a 'filter=ansible_architecture'
			-- ansible centos -m setup -a 'filter=ansible_distribution'
			-- ansible centos -m setup -a 'filter=ansible_distribution_version'
			-- ansible centos -m setup -a 'filter=ansible_domain'
			-- ansible centos -m setup -a 'filter=ansible_fqdn'
			-- ansible centos -m setup -a 'filter=ansible_interfaces'
			-- ansible centos -m setup -a 'filter=ansible_kernel'
			-- ansible centos -m setup -a 'filter=ansible_memtotal_mb'
			-- ansible centos -m setup -a 'filter=ansible_proc*'
	======================================================================
