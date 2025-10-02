# Meeting:

## Attendees
* [Wout Slabbinck](https://pod.woutslabbinck.com/profile/card#me)
* Joachim
* Jos
* Beatriz
* Amira De Winter-El Yaghmouri
* Wouter
* 
## Agenda
* break down the tasks for the delfour use case

## Discussion
Beatriz
- created github organisation
- one repo there with Jos his work
- next steps
	- want policies there
	- want instantiated trust envelope using the ontology

Wout: Elaborate high level diagram
![](../diagrams/sequence_WS(high-level).svg)

Comments
- recommendation: stub
- Discovery
	- how far do we have to go
	- we can find the pod -> loyalty card (think about albert heyn)
	- Wouter: but how do we then find the location of those preferences?
- regarding knowing how to ask the questions
	- know the vocabulary a priori -> think OSLO or W3C standard

Seems like we have **three phases**
- registration/discovery: picking up the scanner
- loop: scanning an item
- end: return the scanner/data deletion

We assume the user already has a loyalty card with delfour (parts of discovery is solved)

How do we continue?
- create a script with lots of stubs/ no changes to UMA server
- TE -> made in the script; but should be in the UMA server
- scanner does not have to be a server, **the script is the scanner**
- Joachim creates first script
- Policy at Delfour: scanner is allowed to see everything related to products information

Generic policy: partycollection
-> must be instantiated for delfour: specific party
NOTE: this is related to PACSOI (PoC 3)

Needs to be a policy related to purpose?

Data: preferences related to sugar-free alternatives
There is other data that is not allowed to see: preferences related to carbon-free alternatives


JOS comments
- we need an architecture
	- right now its static
	- it should be dynamic
	- the data should be brought together just in time
		- opinion Jos: everything should be centralized
		- opinion Wouter and me: not every data should be at all places
	- we need to be specific: where does the reasoning happen?

Amira created some legal policies
(e.g. statistical purpose, delfour wants to get some aggregated results of Ruben shopping there)

## Next steps
* [ ] Task division
	* [ ] Joachim: create script
	* [ ] Wout: create policies
		* generic at pod
			* instantiated for our use case
		* delfour (scanner can see everything)
	* [ ] Wouter: create documentation repo
	* [ ] Amira: include legal policies into documentation repo as well