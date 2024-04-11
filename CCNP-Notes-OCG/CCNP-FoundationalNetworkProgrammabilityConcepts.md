# CCNP- Foundationa Network Programmability Concepts
	
	## Application Programming Interface (API)
		Northbound and southbound are focus
		
		[applications]<--northbound_API-->[controller]<--southbound_API-->[Data Plane]

		Northbound api
			network controller to management software (EX: Cisco DNA center gui to manage the network controller)
			
		Southbound API
			controller management software to individual device (routers, switches, WAPs, ect)
			
	## Representational State Transfer (REST) APIs
		uses HTTP methods to gather and manipulate data
		|HTTP Function | Action | Use Case |
		|--------------|--------|----------|
		|GET  | Requests data | viewing a website|
		|POST | submits data  | submitting login creds|
		|PUT | replaces data | Updating an NTP server |
		| PATCH | appends data | adding an NTP server |
		| DELETE | removes data | removing an NTP server |
		
	CRUD = Create Read Update Delete
		
		|CRUD function | Action | Use Case |
		|--------------|--------|----------|
		| CREATE | inserts data | updating a customers home address |
		| READ | retrieves data | Pulling up a customers hom address from a DB |
		| UPDATE | modifies or replaces data in a database | changing a customers home address |
		| DELETE | Remove data | Removing a customer from a DB | 

	## API Tools and Resources
		intro to postman
			Builder portion
			History, collections, new tab, URL bar

		Data Formats (XML and JSON) 
			XML - tag based language. must begin with < and end with >. Tag must end with a /
				Example: 
				<user>
			 	 <name>jason</name>
				</user>

				indentation is not required, but helps with readability

			Javascript object notation (JSON)
				people say its easier to work with than XML
				
				{
				 "user": "root", 
				 "father": "Jason"
				}
				

			HTTP Status codes (common ones atleast)
				| 200 | ok | Using GET or POST to exchange data with an API|
				| 201 | Created | Creating resources by using a REST API call |
				| 400 | Bad Request | Request failed due to client-side issue | 
				| 401 | Unathorized | Client not authenticated to access site or API Call |
				| 403 | Forbidden | Access not granted based on supplied credentials | 
				| 404 | Not Found | page at http url location does not exist or is hidden |
				
				
			
			
	## Cisco vManage APIs
		Need to have
			* url bar must have api call to target auth api
			* the HTTP POST method is used to send credentials to cisco vmanage
			* headers content-type key must be appliation/x-www-form-urlencoded
			* body must contain keys with the j_ devnetuser and the j_password Cisco123!
			
			JSESSIONID = java session ID
			
	## Data Models and Supporting Protocols
		covers the following
		* Yet Another Next Generation (YANG) modeling language
		* Network Configuration Protocol (NETCONF)
		* RESTCONF

		### Yang Data Models
			alternative to SNMP MIBS
			RFC 6020
			use a tree structure
			constructed into hierarchical modules

		### NETCONF
			RFC 4741 and RFC 6241
			IEFT standard protocol that uses YANG models to communicate with various devices on the network. 
			can run on ssh, tls, and SOAP
			SNMP cannot distinguish between config data and operational data, but netconf can. 
			netconf uses paths where snmp uses OIDs (example interfaces/interface/eth0)

			capabilities = information exchanged by netconf
			
			netconf uses RPC messages in XML format to send information between hosts
		
		### RESTCONF
			
			RFC804, 
			provides a restful api experience while still leveraging the device abstraction capabilities provided by netconf. 
			
			Supports:
			* GET
			* POST
			* PUT
			* DELETE
			* OPTIONS
			
			requests and respondes can use either JSON or XML
			
	## Cisco DEVNET
		
	## Basic Python Components and Scripts
		
			
			
			
		
		