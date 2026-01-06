Data Base System basic architecture
- Levels:
	1. View Level
		- There are n possible views for people with different access levels/needs
		- Customized and restructured information
	2. Logical Level
		- in order to perform mathematical operations on the data, we need to ensure that the structure of our objects allow it and produce valid outputs
	3. Physical Level
		- Refers to the actual files, where its stored in memory/storage
		- How data is stored and processed
- Data Independence:
	- Logical and physical levels are independent
- Data Models
	- What makes up?
		1.  A data structure with clearly defined operations to be used on it
		2.  A bunch of operations to do on the data structure
		3.  A query language to manipulate the data
	- exampless:
	  - Entity relationship data model
		  - Models an application as a collection of entities and relationships
			- Entity
				- object that is distinguishable based on its attributes
			- Relationship
				- an association between entities
			- Represented diagrammatically
	  - Relational Model
		  - Every time you make a change you create a new instance
		  - Each instance is typically connected to a time stamp
- Data Definition Language (DDL)
	- A language for defining the database schema
- Data Manipulation Language (DML)
	- Language for accessing and modifying data
	- known as query or update language
	- SQL is most widely used query language (its declarative)
	- Two main classes of DML
		- Declarative
			- User specifies what data is required, without specifying how to get it
		- Procedural 
			- User specifies both what data is required and how to get it

Note:
- Post SQL
- Datagrip