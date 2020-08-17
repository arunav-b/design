# Domain Driven Design (DDD): 
	DDD is a methodology that covers the whole software development lifecycle, from gathering requirements to low-level design. Its core premise is that to build better software, we have to align its design with the business’s domain, needs, and strategy. It provides tools for making design decisions on two levels: strategic and tactical.
	
	
## Strategic Design:

	Strategic Design, introduces tools and techniques for business domain analysis, knowledge sharing, and overall strategic decision making. Strategic design can be broken into following steps - 
	a. Analyzing Business Domains
	b. Discovering domain knowledge by using a Ubiquitous language
	c. Managing complexity with Bounded Contexts
	d. Context Mapping
	
	Below are some of the key concepts in Strategic Design -

	1. Ubiquitous Language:
		A common language used by developers and the domain experts to refer to different parts of the business.
		
	2. Bounded Context:
		Bounded Context is a semantic contextual boundary, within which each component of the software model has a specific meaning and does specific things.
			
	3. Subdomains:
		 Subdomain is a sub-part of your overall business domain, which are discovered during the analysis phase.
		 3.a. Types of Subdomains:
		 	- Core Domain: 
				A core subdomain is what a company does differently from its competitors. This is the main well-defined domain where most of the investment of time and money goes in.
			- Supporting subdomain: 
				Things that the company is doing differently from its competitors, but they do not provide a competitive edge. Custom development required for certain models to support the core domain bcoz off-the-shelf solutions doesn't exist.
			- Generic subdomain: 
				Things that all companies do in the same way. This kind of solution may be available for purchase off the shelf but may also be outsourced or even developed in house by a team that doesn’t have the kind of elite developers that you assign to your Core Domain or even a lesser Supporting Subdomain. Eg - Logging Framework
		
		NOTE:
			 Subdomain vs Bounded-Context:
				- Subdomains are discovered, while bounded contexts are designed.
				- Subdomains are already there in a business domain. They are just discovered during analysis phase.
				- Choosing model boundaries (bounded-contexts) is a strategic design decision. We decide how to divide the business domain into smaller, manageable problem domains.
		
	4. Context Mapping:
		Integration of multiple bounded-contexts, i.e., how one bounded context maps to another bounded context. 

		4.a. Kinds of Context Mapping:
				
													Context Mapping
														   |
							   +---------------------------------------------------------------+
							   |                           |                  |			       |
					       Cooperation            Customer-Supplier    Separate Ways     Big Ball of Mud
						  	   |						  |
		                +--------------+        	+----------------------------------------+
					    |              |  			|                 |                      |
				   Partnership  Shared Kernel	Conformist     Anti-Corruption Layer   Open Host Service 	

			- Cooperation:
				Cooperation patterns relate to bounded contexts implemented by teams with well-established communication. It applies to teams that have dependent goals, where the success of one team depends on the other’s and vice versa. These are of two types -
				- Partnership: 
					In a partnership between 2 teams, each team is responsible for one Bounded-Context, each having a dependent set of goals. There is a commitment required between the two teams and they will succeed or fail together. Bounded contexts are integrated in an ad hoc manner.
				- Shared Kernel: 
					A relationship with 2 teams, where they share a small but common model. The shared kernel is both referenced and owned by multiple bounded contexts. It is difficult to maintain. A shared kernel is a natural fit for integrating bounded contexts that are owned and implemented by the same team.
					
			- Customer-Supplier:
				Here one bounded-context is a supplier (upstream) of services and the other one being a consumer (downstream). Unlike in the cooperation case, both teams (upstream and downstream) can succeed independently. These can be broadly classified as -
				- Conformist:
					Here in a supplier-consumer relationship, the supplier simply supplies information/data to downstream teams without much motivation on what the downstream team needs. The donwstream team conforms to the upstream model. Eg - Industry-standard or well-established model that doesn't change frequently.
				- Anticorruption Layer:
					The downstream creates a translation layer between its Ubiquitous Language and the Ubiquitous Language of upstream. The layer isolates the downstream model from the upstream model and translates between the two. Eg - Legacy-Bridge - Consumer protects itself from frequent changes by supplier's contract.
				- Open Host Service: 
					An Open Host Service defines a protocol or interface that gives access to the underlying Bounded Context as a set of services. The protocol is "open" hence allowing to integrate with other Bounded Contexts with relative ease. Supplier's public interface doesn't conform to its ubiquitous language, instead it is intended to expose a protocol convenient for consumers. Hence, the public protocol is called the "published language".
					
			- Separate Ways:
				Creating its own specialized solution and not integrating with other Bounded Context. This makes sense when it’s less expensive to duplicate particular functionality than to collaborate and integrate it. Should be avoided when integrating core subdomains.
				
			- Big Ball of Mud:
				Building all the bounded contexts as a part of one single application. Ex - Legacy systems.
				
		4.b. Integration Types:
			- RPC with SOAP
			- Restful HTTP 
			- Messaging
			
			
## Tactical Design:
	 
	 Tactical Design, touches on the low-level design concerns. It introduces different architectural and design patterns for implementing system components. Tactical design can be broken into following steps - 
	 a. Business Logic Implementation Patterns:
	 		We'll disucss 4 different ways to implement the business logic - 
			
			i. Transaction Script: 
				- This pattern is well adapted to the simplest problem domains, where the business logic resembles extract-transform-load (ETL) operations. 
				- It organizes the business logic by procedures. 
				- It can use a thin abstraction layer for integrating with storage mechanisms, but it is also free to access the databases directly.
				- Each operation should either succeed or fail, but can never be in an in-between state. If a process fails for some reason, the system should remain in a consistent state—hence the pattern’s name, transaction script.
				- Not suitable for core-subdomain.
			
			ii. Active Record:
				- This also supports cases where the business logic is simple but here the business logic may operate on more complex data structures.
				- This pattern uses dedicated objects to represent complicated data structures, referred as active records.
				- These objects also implement data access methods for doing CRUD operations, hence they are dependent on ORM or similar data access framework.
				- Here also the buisness logic is organized in procedures, but in this case the procedures are manipulating the active record objects instead of accessing the database.
				
			iii. Domain Model:
				- It is intended to cope with cases of complex business logic. 
				- Emphasis is on the business logic instead of technical concerns.
				- A domain model is an object model of the domain that incorporates both behavior and data. 
				- Object modeling should not introduce additional accidental complexities.
				- DDD's tactical patterns are the building blocks of such object model: 
					- Value Objects: 
						An immutable object that can be identified by its values.
					- Aggregates: 
						An entity representing a hierarchy of objects. Cannot solely be identified by values and requires an ID field. They are mutable. Some properties of aggregates are - 
						- Consistency: Only aggregate's business logic can mutate the state. All others can only read its state or execute its public methods.
						- Transactional: All changes to its state should be committed transactionally as one atomic operation.
						- Hierarchy of objects: An aggregate is not a flat record. Rather, it’s a hierarchy of related objects. There might be business scenarios where these related multiple objects share a transactional boundary.
						- Referencing other aggregates: Objects that can be eventually consistent should not belong to the aggregate and can be referenced by their ID.
						- Aggregate Root: Since an aggregate represents a hierarchy of objects, only one of them should be designated as the aggregate’s public interface—the aggregate root.
						- Some rules for designing aggregates: 
							1. Protect business invariants inside Aggregate boundaries.
							2. Design small Aggregates.
							3. Reference other Aggregates by identity only.
							4. Update other Aggregates using eventual consistency.							
					- Domain Events: 
						A domain event is a message describing a significant event that has occurred in the business domain. Since domain events are describing something that has already happened, their names should be formulated in the past tense. Domain events are a part of an aggregate’s public interface. An aggregate publishes its domain events. Other processes, aggregates, or even external systems can subscribe to and execute their own logic in response to the domain events.
				
			iv. Event Sourced Domain Model:
				- Similar to domain model, except how the aggregates' state is persisted. Uses event-sourcing for managing aggregates' state.
				- Event sourcing pattern uses domain events to introduce the dimension of time into the model. Every change to the system’s state should be expressed and recorded as a domain event.
				- Advantages of event-source domain model - 
					- Time machine
					- Deep insight
					- Audit log
				
	 b. Architectural Patterns:
	 		We'll look at the following patterns -
			
			i. Layered Architecture:
				Good fit for business logic implemented using active record pattern. The 3 main layers are -
					- Presentation Layer
					- Business logic Layer
					- Data access Layer
			
			ii. Ports & Adapters:
				Good fit for business logic implemented using domain model pattern. The core goal of the ports & adapters architecture is to decouple the system’s business logic from its infrastructural components. The key layers -
					- Business Layer: This layer defines “ports” that have to be implemented by the infrastructure layer. 
					- Infrastructure Layer: This implements “adapters”: concrete implementations of the ports’ interfaces for working with different technologies. 
					- Application Layer: This layer supplies adapters for the business logic’s ports through dependency injection.
			
			iii. Command-Query Responsibility Segregation (CQRS):
				- This pattern is obligatory for event-sourced domain model–based systems.
				- CQRS allows the use of multiple storage mechanisms for representing different models of the system’s data.
				- There are 2 types of models - 
					- Command Execution Model: Is the only model representing strongly consistent data—the system’s source of truth.
					- Read Model: System can define as many models as needed for presenting data to users or other systems. These models are read-only. None of the system’s operations can directly modify the read models’ data. For the read models to work, the system has to project changes from the command execution model to all its read models. There are two ways of generating projections -
						- Synchronous: Fetch changes to the OLTP data through the catch-up subscription model.
						- Asynchronous: Command execution model publishes all committed changes to a message bus. The system’s projection engines can subscribe to the published messages and use them to project the read models.
		
		
## Event Storming:
	
	- 1. Exploring: Unstructured exploration of domain events in past tense.
	- 2. Timelines: Order the domain events chronologically.
	- 3. Commands: Model commands that caused the domain events. A single command might trigger multiple domain events. The domain events triggered might be in parallel or sequential.
	- 4. Policies: Look out for automated policies that trigger commands, there might not be specific actors to trigger the command.
	- 5. External Systems: Identifying external system executing command or gets notified about events.
	- 6. Aggregates: Once all events and commands are identified, organize related concepts into aggregates.
	- 7. Bounded-Contexts: Look out for aggregates that are related to each other.
	- 8. Views: Identify views & other components
