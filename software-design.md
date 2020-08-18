# Designing Software

## Strategic vs Tactical Design:

- Prioritize design based on - Value & Impact
- Strategic design should be evolving. Create enough design upfront, but not too much. It should evolve as we know more about the domain.
- Tactical design is the fine grains of the strategic design.
- Tactical Design Documentation 
	- UML
	- Automated Tests (Unit, Functional, BDD)

- From Requirements to Design 

                             Requirements
                                  |
                    Strategic Design during start of sprint	
                                  |
	    Tactical Design when implementing them (by writing test cases)

- Develop a prototype, make changes to it as we evolve and know more along the journey.
- Architecture will also evolve as we go along.


## Design by Contract vs Design by Capability:

### Design by Contract:
- Service provider can dictate terms and service receiver can abide by those. This is the case in statically typed languages like Java, C#, etc. for example how Interfaces & Abstract Classes work.
- Useful in compile time verification
- Sometimes it becomes cumbersome with too many contracts and things are less flexible. If contract not properly defined, at times might have to raise the abstraction to a higher level as well.

### Design by Capability:
- There are no contracts in this case. We can pass objects at runtime and will be resolved without any pre-defined contract. This is in case of dynamically typed language like Groovy, Ruby, etc.
- No compile time check
- Design is lightweight and flexible. Hierarchy is relatively flat.
- Disadvantage: As it is resolved at runtime, irrelevant objects maybe passed and hence if not explicitly handled fails at runtime.