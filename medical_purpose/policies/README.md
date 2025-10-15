# Scenario Purpose-based medical data exchange

There are three kind of rules in this scenario under one policy.
An important assumption is that conflict resolution is in the loop.
Something else that is important, this scenario relies on both preventive usage control enforcement (access control) and detective usage control (monitoring).

The three rules are the following
- **Primary use permission** Doctors of Ruben are allowed to use his electronic health records (EHRs)
  - Party: Doctors of Ruben; represented by a Party Collection `ex:RubensDoctors`
    - **Comment Beatriz**: for simplicity we can assume just one doctor for now and then party collections are not needed
  - Target: EHRs; represented by an Asset Collection `ex:RubensEHRs`
  - Action: `odrl:use`
    - **Comment Beatriz**: we have to figure out to what types of access `use` translates to
  - Constraints: OR
    - primary care purpose, i.e., `sector-health:PrimaryCareManagement`
    - remote consultation purpose, i.e., `sector-health:PatientRemoteMonitoring`
- **Secondary use permission**: Researchers are allowed to use Ruben's EHRs for secondary purposes
  - Party: Researchers; represented by a Party collection `ex:specificResearchers`
    - The body creating this list of specific researchers is out of scope. We assume there is a list that contains at least one such researcher
  - Target: EHRs; represented by an Asset Collection `ex:RubensEHRs`
  - Action: `odrl:read`
  - Constraints:
    - `eu-ehds:HealthcareScientificResearch`
    - **Comment Beatriz**: there's a [whole list of purposes](https://w3c.github.io/dpv/2.2/legal/eu/ehds/#vocab-purposes) that the EHDS allows, unless people opt-out. So by default the policy should allow all of these until Ruben opts out of one, multiple or all of them
  - Duty:
    - Party: Researchers; represented by a Party collection `ex:specificResearchers`
    - Target: EHRs; represented by an Asset Collection `ex:RubensEHRs`
    - Action: `odrl:delete`
    - Constraint:
      - Opt out prohibition exists -> perhaps dynamic rightoperanderefence to make this happen?
- **Opting out prohibition**: A prohibition for Researchers to use Ruben his EHRs for secondary purposes
  - Party: Researchers; represented by a Party collection `ex:specificResearchers`
  - Target: EHRs; represented by an Asset Collection `ex:RubensEHRs`
  - Action: `odrl:read`

## First situation
Ruben has not yet decided to opt out.
So the policy only contains two rules (see [appendix](#appendix-full-policy) for actual rules)

```ttl
@prefix odrl: <http://www.w3.org/ns/odrl/2/> .
@prefix ex: <http://example.org/> .
@prefix dpv: <http://www.w3.org/ns/dpv#> .
@prefix xsd: <http://www.w3.org/2001/XMLSchema#> .

ex:RubenPolicy a odrl:Policy ;
    odrl:conflict odrl:prohibit ;
    odrl:permission ex:PrimaryUsePermission, ex:SecondaryUsePermission .
```

**Access Control checks**
Doctor requests access (with correct purpose) -> will produce (with ODRL Evaluator v0.5) an Active Rule report, so the doctor can access
Researcher requests access (with correct purpose) -> will produce (with ODRL Evaluator v0.5) an Active Rule report, so the researcher can access
- **NOTE**: An empty condition report SHOULD be generated that is `not-set`. This behaviour is currently not present in the evaluator

-> This condition report can then signal the internals of the UMA server that something should be done -> **monitoring this duty**

**Monitoring checks**
Will not do much, since nothing can go wrong through the access control with these rules

## Second situation
Ruben decided to opt-out. 
As a result a third rule is added that prohibits researchers to use the data (see [appendix](#appendix-full-policy) for actual rules)

```ttl
@prefix odrl: <http://www.w3.org/ns/odrl/2/> .
@prefix ex: <http://example.org/> .
@prefix dpv: <http://www.w3.org/ns/dpv#> .
@prefix xsd: <http://www.w3.org/2001/XMLSchema#> .

ex:RubenPolicy a odrl:Policy ;
    odrl:conflict odrl:prohibit ;
    odrl:permission ex:PrimaryUsePermission, ex:SecondaryUsePermission ;
    odrl:prohibition ex:OptOutProhibition .
```

**Access Control checks**
Doctor requests access (with correct purpose) -> will produce (with ODRL Evaluator v0.5) an Active Rule report, so the doctor can access
Researcher requests access (with correct purpose):
* both the prohibition (`ex:OptOutProhibition`) and permission (`ex:SecondaryUsePermission`) produce an **Active** Rule report
  * There is a need for **Conflict Resolution**, which will use the conflict strategy. As a result the Researcher no longer can access Ruben's EHRs

**Monitoring Checks**
Two possibilities
- The researcher has deleted the data of Ruben
- The researcher has **NOT** deleted the data of Ruben

**The researcher has deleted the data of Ruben**
Through some way (trust envelope?) the `sotw` contains a fulfilled duty. Because of that, the duty is fulfilled.

Calculating the deontic states results into no violations.

**The researcher has **NOT** deleted the data of Ruben**
As soon as the prohibition rule exists. 
Somehow we need to generate the fact that the **duty is violated**.

Because the duty is violated, the permission (`ex:SecondaryUsePermission`) is also **violated**.
**Comment Beatriz**: The correct terminology would be if the duty is violated then the permission is inactive.

### Appendix: full policy

Apart from the following two triples, this is a correct ODRL policy according to https://odrlapi.appspot.com/
- `ex:RubenPolicy odrl:conflict odrl:prohibit` (because no profile)
- `_:DutyConstraint odrl:leftOperand ex:OptOutStatus` (no correct ODRL Left Operand)

- **Comment Beatriz**: For `ex:PrimaryUsePermission` why not use `odrl:isAnyOf` and thus only one constraint?

```ttl
@prefix odrl:          <http://www.w3.org/ns/odrl/2/> .
@prefix ex:            <http://example.org/> .
@prefix dpv:           <http://www.w3.org/ns/dpv#> .
@prefix xsd:           <http://www.w3.org/2001/XMLSchema#> .
@prefix eu-ehds:       <https://w3id.org/dpv/legal/eu/ehds#> .
@prefix sector-health: <https://w3id.org/dpv/sector/health#> .

ex:RubenPolicy a odrl:Policy ;
    odrl:conflict odrl:prohibit ;
    odrl:permission ex:PrimaryUsePermission, ex:SecondaryUsePermission ;
    odrl:prohibition ex:OptOutProhibition .

# Primary use permission
ex:PrimaryUsePermission a odrl:Permission ;
    odrl:assigner ex:RubensDoctors ;# TODO: requires sotw party collections
    odrl:target ex:RubensEHRs ;# TODO: requires sotw asset collections
    odrl:action odrl:use ;
    odrl:constraint [
        a odrl:LogicalConstraint ;
        odrl:or [
            odrl:leftOperand dpv:Purpose ;
            odrl:operator odrl:eq ;
            odrl:rightOperand sector-health:PrimaryCareManagement
        ],
        [
            odrl:leftOperand dpv:Purpose ;
            odrl:operator odrl:eq ;
            odrl:rightOperand sector-health:PatientRemoteMonitoring
        ]
    ] .

# Secondary use permission with duty
ex:SecondaryUsePermission a odrl:Permission ;
    odrl:assigner ex:specificResearchers ; # TODO: requires sotw party collections
    odrl:target ex:RubensEHRs ; # TODO: requires sotw asset collections
    odrl:action odrl:read ;
    odrl:constraint [
        odrl:leftOperand dpv:Purpose ;
        odrl:operator odrl:eq ;
        odrl:rightOperand eu-ehds:HealthcareScientificResearch
    ] ;
    odrl:duty [
        a odrl:Duty ;
        odrl:assigner ex:specificResearchers ;
        odrl:target ex:RubensEHRs ;
        odrl:action odrl:delete ;
        odrl:constraint [
            odrl:leftOperand ex:OptOutStatus ; # TODO: needs to be a proper left operand
            odrl:operator odrl:eq ;
            odrl:rightOperandReference ex:RubenOptOutFlag #TODO: https://spec.knows.idlab.ugent.be/odrl3proposal/latest/#uc2
        ]
    ] .

# Opting out prohibition
ex:OptOutProhibition a odrl:Prohibition ;
    odrl:assigner ex:specificResearchers ;
    odrl:target ex:RubensEHRs ;
    odrl:action odrl:read .
```