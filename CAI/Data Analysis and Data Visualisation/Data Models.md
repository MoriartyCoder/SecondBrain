### Entitity Relationship model

![[visualization_relationship model.png]]
#### Entity Type
Class of objects with properties in common and
an autonomous existence
![[Pasted image 20241015090428.png]]

##### Weak Entities
Some entities cannot be defined on their own and therefore dont have a primary key. They only have particial keys.

![[Pasted image 20241015092907.png]]

A room is a weak entitiy of a building.
#### Relationship Type
Logical links between two or more entities
![[Pasted image 20241015090435.png]]

#### Attribut
![[Pasted image 20241015090444.png]]
##### Key / Primary Key
A primary key identifies an instance (entity) of an entity type.
A key consists of one or more attributes, called key attributes.
**Notation:** Underline primary key attributes in diagram

##### Composite attribute type
![[Pasted image 20241015091416.png]]

##### Multivalued attribute type
![[Pasted image 20241015091458.png]]
e.g. a customer can have multiple phone numbers

#### Cardinality
Cardinalitys of a relationship can be modelled with either Chen or (min, max) Notation

![[Pasted image 20241015092205.png]]
One President belongs to one country / A country has zero or one president. Every President has a country.

![[Pasted image 20241015092421.png]]
One customer can have multiple order numbers, but one order must be bought by exactly one
customer.

### Storage of Data
#### Flat model (CSV etc.)
- one item per row
- each column is an attribute
- cell holds value for item-attribute pair

![[Pasted image 20241015093657.png]]

#### Relational model (SQL, Python Pandas)

* Declarative language for core data manipulations

![[Pasted image 20241015094013.png]]

![[Pasted image 20241015094024.png]]

#### Document model (xml, json)
* hierarchy of entities

![[Pasted image 20241015094325.png]]



