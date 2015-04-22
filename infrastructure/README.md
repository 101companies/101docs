# Infrastructure of 101companies project

The 101companies project uses several Virtual Machines acting as servers. However, there are currently plans to move the project into the cloud. Thoughts about this move are described in the server.odp presentation. 

Currently, we have the following Virtual Machines:

* worker: Runs the 101worker infrastructure and web services related to 101worker
* wiki: Runs 101wiki, the [wiki used by 101companies](http://www.101companies.org/wiki/)
* mongo: Runs the MongoDB serving as a database for 101wiki
* triples: Runs a instance of Sesame which is used for ontology development in a 101companies context

Additionally, we have the following VMs which are currently not used:

* build: Used to investigate build management in a bachelor thesis
* black42: Former server for 101worker
* sesame: Former public Sesame endpoint containing triplified data of 101wiki