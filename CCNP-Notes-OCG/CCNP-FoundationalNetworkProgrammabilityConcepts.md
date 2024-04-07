CCNP- Foundationa Network Programmability Concepts
	
	Application Programming Interface (API)
		Northbound and southbound are focus
		
		[applications]<--northbound_API-->[controller]<--southbound_API-->[Data Plane]

		Northbound api
			network controller to management software (EX: Cisco DNA center gui to manage the network controller)
			
		Southbound API
			controller management software to individual device (routers, switches, WAPs, ect)
			
	Representational State Transfer (REST) APIs
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
			
		
		