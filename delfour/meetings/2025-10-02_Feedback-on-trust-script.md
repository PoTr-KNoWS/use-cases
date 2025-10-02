# Meeting:

## Attendees
* [Wout Slabbinck](https://pod.woutslabbinck.com/profile/card#me)
* Ruben Verborgh
* Joachim
* Wouter
* Jos
## Agenda
* client meeting with Ruben V regarding delfour use case
follow up on [tuesdays meeting](./2025-09-30_breakdown-tasks.md)
## Discussion
Joachim starts with describing the current script and repo structure

Discovery regarding preferences: we are not gonna solve it now. 
RV: preference resource location: hard coded

Other comments
- scanner should forget the preferences at this point
	- have a check! (make it explicit, like how Jos has the checks)

Logging
- keep envelope, throw away data

Request
- make sure we can encode the purpose in a request
### comments
- Resource server must have all information to generate the trust envelope

JOS: van de insight kunt ge uw policies creeren?
- clients should reason, server should deliver data
RV: policies have to travel with data. How could you generate this policies?
- Jos: has meta rules to generate these policies
	- meta rules can be generalized

RV: alternative flows are not required right now

NOTE: Densitization: the process of causing someone to experience something, usually an emotion or a pain, less strongly than before ~ [dictionary](https://dictionary.cambridge.org/dictionary/english/desensitization)

## Next steps
* [ ] Jos: iterate over policy generation
* [ ] Focus on encoding purpose in a request (HTTP and/or policy)
	* Wout: ODRL policy, iterate over https://github.com/PoTr-KNoWS/use-cases/blob/main/delfour/example-data/policies.md
	* Someone else: purpose in HTTP request
* [ ] Joachim: make sure that Resource server must have all information to generate the trust envelope