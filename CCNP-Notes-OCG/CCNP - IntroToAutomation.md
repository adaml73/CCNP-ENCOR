CCNP - Introduction to Automation Tools
859 - 889


## Embedded Event Manager (EEM)
	split into building blocks. Events and Actions
	example:
	
	looks for a specific syslog message, checking to see if loopback0 went down. 
	uses regex to match for condition

	event manager applet LOOP0
	 event syslog pattern "Interface Loopback0.* down" period 1
	 action 1.0 cli command "enable"
	 action 2.0 cli command "config terminal"
	 action 3.0 cli command "interface loopback 0"
	 action 4.0 cli command "shutdown"
	 action 5.0 cli command "no shutdown"
	 action 5.5 cli command "show interface loopback0"
	 action 6.0 syslog msg "I've fallen and I cant get up!"
	 action 7.0 mail server "I've fallen and I cant get up!"
	   from no-reply@yourcompany.com subject "Loopback0 issues!"
	   body "the loopback0 interface was bounced, please monitor
	   accordingly. "$_cli_result"
	

	need to use "enable" and "config t", applet assumes user is in exec mode, not priv exec or config mode. 
	if using AAA, need to use "event manager session cli username [username]"
	actions are parsed as strings, so 10.0 would come after 1.0, not 9.0
	
	troubleshoot/verify
	debug event manager action cli
	debug event manager all
	debug event manager action mail


	common eem email variables
	|eem variable | description | example |
	|-------------|-------------|---------|
	|_email_server | SMTP server ip address or dns name | 10.1.1.1.1 or mailserv01
	|_email_to | email address to send to | netadmin@company.com |
	| _email_from | email address of sending party | no-reply@company.com |
	| _email_cc | email address of additional email receivers | helpdesk@company.com | 
		
	### eem and tcl scripts
		use eem to call tcl scripts
		event none = will only run when triggered manually
		example: 
		
		event manager applet Ping
		 event none 
		 action 1.0 cli command "enable" 
		 action 1.1 cli command "tclsh flash:/ping.tcl"

		trigger manually
		event manager run ping
		ping.tcl
		foreach address { 
		192.168.0.2
		192.168.0.3
		192.168.0.4
		192.168.0.5
		192.168.0.6
		} { ping $address}

	### EEM Summary
	
	## Agent-Based Automation Tools
	
	### Puppet
		supported on catalyst, nexus, and UCS. 
		uses Puppet master (server) to communicate with devices that have the puppet agent (client)
		changes and automation are initiated with the puppet console
		changes are stored in the puppet database (puppetDB), which allows the tasks to be saved so they can be pushed at a later date/time
	
		master replica - Optional backup master server 
		
		agents communicate to master using different tcp connections, each tcp port represents a unique communication path from an agent running on a device or node. 
		
		Installtions mode
		monolithic - up to 4000 nodes
		monolithic with compile masters - 4000 to 20,000 nodes
		monolithic with compile masters and standalone PE-postgreSQL - mode than 20,000 nodes
		
		master of masters - manage distributed puppet masters and their associated databases. 
		
		modules - allow for the configuration of practically anything that can be configured manually
			including: 
				* manifests
				* templates
				* files
			Are code that configures the clients or nodes running the puppet agent.pushed to devices using ssl and need certificates. 

			module - cisco_ios

			manifests - can be saved as individual files, have file extension .pp

			uses DSL - domain specific language, largely based on ruby. 
			
			
	### Chef
		open source config management tool
		written in ruby and erlang
		
		has cookbooks and recipies
		cookbook = collection of code or files
		recipe = code being deployed to make config changes
		
		knife is the command line tool used to upload cookbooks to the chef server. 
		knife upload [cookbookname]
		
		chef solo - chef server is hosted locally on a workstation
		chef client and server - typical deployment with distributed components
		hosted chef - chef server is hosted in the cloud
		private chef - all chef components are within the same enterprise network
		
		OHAI - collects the current state of a node to send the info back to the chef server through the chef client service. 

		kitchen - where all recipes and cookbooks can automatically be executed and tested prior to hitting any prod nodes. 
		supports the following:
			* BATS
			* Minitest
			* RSpec
			* Serverspec
			
	### Saltstack (Agent and Server Mode)
		built on python, and has a python interface users can use to directly to saltstack using python
		mostly written in yaml or DSL, called salt formulas
		
		uses concept of systems - has masters and minions
		uses zeroMQ, for fast reliable messaging. 
		event driven components called reactors and beacons. 
		reactor - lives on a master and listens for any type of changes in the node or device that differ from the desired state, including
			* command-line configuration
			* disk/memory/processor utilization
			* status of services
		beacons live on minions
		uses pillars and grains, grains are run on minions to gather system information to report back to the master. typically gathered by the salt-minion daemon
		can provide specifics ot the master. (on request) 

		pillars store data that a minion can retrieve from the master. pillars can have minions assiged to them, and other minions that are not assigned to a pillar would not have 		access that data. can be helpful when handling sensitive or confidential data. 

		syndic - enterprise gui 

		use an asterix to target all systems, or a specific minionID to target a specific minion. 

			salt '*' cmd.run 'ls -l /etc'
			salt '*' network.interfaces

	

	## Agentless automation tools

		### Ansible
		
		PPDIOO - (Prepare, Plan, Design, Implement, Observe, Optimize) 
		
		uses playbooks. 
		| component | description | use case |
		|----------|--------------|----------|
		| playbook | set of plays for remote systems | enforcing config and/or deployment steps|
		| Play | set of tasks applied to a single host or group of hosts | grouping a set of hosts to apply policy or configuration to them | 
		| Task | A call to an ansible module | logging in to a device to issue a show command to retrieve output | 


		written in yaml
		start with three dashes 
		---
		end with three periods
		...
		
		comments start with octothope #
		key value paris 
		---
		# HR Employee record
		Employee1: 
		 	Name: John dough
			Title: Developer
			Skills: 
			  - Python
			  - YAML 
			  - JSON
		yaml lint - free tool to check yaml format
		

		ansible cli commands 
		| cli command | use case | 
		|-------------|----------|
		| ansible | runs modules against targeted hosts |
		| ansible-playbook | runs playbooks |
		| ansible-docs | provides documentation | 
		| ansible-pull | changes ansible clients from default push model to pull model | 
		| ansible-vault | encrypts YAML files that contain sensitive date | 

		uses inventory file to keep track of hosts
	 
		
		
		
		
		 
		
	
	
		
		

		
		
		
	
	
	
	
	
	
	
	