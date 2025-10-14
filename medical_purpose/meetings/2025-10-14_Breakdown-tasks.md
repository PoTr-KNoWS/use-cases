# Meeting:

## Attendees
* [Wout Slabbinck](https://pod.woutslabbinck.com/profile/card#me)
* Joachim
* Beatriz
* Jos
## Agenda
* https://knows-research.atlassian.net/browse/RS-505
Ruben allows doctors to have access to his electronic health records (EHRs) for primary care and remote consultations. Under the new EHDS regulation, this data can also be re-used for certain secondary purposes, however Ruben can opt-out, and opt-in again, from any use at at any point. Ruben also wants to get annual updates on how his data is being used to improve healthcare of diabetes patients.
## Discussion

Who transforms the data? a service in the middle, but we dont care about that right now

Primary

How do you let others notify to delete your data?

Permission with a duty (duty contains message, that if I withdraw consent, it has to be deleted)

**you need a notification system to let others now**
Comment: how does that trickle down when the data was used by other companies not in EU

How far does consent reach?
- does it mean models cannot longer train algorithms on the data?
- do they also have to retract the current models in use


By default (opt-out): researchers are allowed for secondary purpose. You can have a policy to not allow it
- note: it is only about medical data (EHDS regulation defines a set of data types)
	- but what if you have two similar data types; one that you allow, one that you not

Only certified people can use secondary purpose (otherwise, an attack vector)
Question: who decides who make those requests? because by default, they have access
If you self host, you could also block all third parties to not have default read access
-> should be a strategy

### How to tackle it
Craft policies for [timeline](https://knows-research.atlassian.net/browse/RS-515)
- one for the primary purpose:  on of my doctor-patient
- one for the secondary purpose: Researcher from ugent is allowed to use that data 

My opinion: have those purpose added as a constraint that coming with that purpose is allowed

So policy with two rules
- doctor allowed
- certified party with purpose constraint + duty (if that rule is deleted -> no more access -> remove data)

How to add duty report from odrl evaluator
It will not be fulfilled yet, but it later has to be fulfilled (but the original rule will be gone, right?)

Think about how permission with duties should be generated


## Next steps
* [ ] Joachim: work on timeline
* [ ] Wout: work on purpose odrl evaluator
* [ ] Beatriz: create the policies