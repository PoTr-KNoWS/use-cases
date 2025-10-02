# Information regarding the policies

These policies were crafted for the use case:
- [Delfour-Scanner](delfour-scanner-policy.ttl): The policy that allows the scanner to see all the products of Delfour
- [Instantiated Preferences](./preferences-policy(inst).ttl): Instantiated policy that allows shopping preferences to be read to show alternative products
- [Legitimate interest](./legitimate-interest-policy.ttl): Instantiated policy that allows Delfour to keep pseudo-anomymised data about users' purchasing habits under legitimate interest

## Questions that need to be resolved

- What is the actual location for the preferences? -> these need to replace `ex:preferences`
- Is a scanner an individual entity or part of delfour?
  - Is the assignee of a permission: `the scanner of delfour` or just `delfour`
  - This question came up while creating a policy for delfour that allows the products to be seen at scanner level
    - can be resolved/ignored if that data is public
- What is the location/description for "pseudo-anonymised data about purchasing habits"?
  - Should techniques be mentioned for which pseudonimisation techniques were used to do that? Or is that part of the trust envelope?
    - e.g. k-anonymity or differential privacy
      - k-anon: Ensures each record is indistinguishable from at least k–1 others
      - differential privacy: Adds mathematical noise to data queries to obscure individual contributions
  - this problem resembles the [member derived view problem (unresolved)](https://docs.google.com/presentation/d/1Kq4hZvxRBozEqqmKWtdJ_O87PYArPBIpzO6g9BewBkc/edit?slide=id.g36b90bd1b63_0_590#slide=id.g36b90bd1b63_0_590)
    - who owns the source data: the shopping list information
- There is only a left operand for full anonymisation; not pseudoanonymisation.

Beatriz comments:
- Why are the policies all `odrl:Offer`?
  - ODRL FS spec says that offers MUST NOT be considered by the evaluator. The justification was that in the offer term definition the following is stated "The Offer Policy MAY contain a Party with Assignee function, but MUST not grant any privileges to that Party.". However, `odrl:Set` policies contain the following in the definition "No privileges are granted to any Party (if defined).", and MUST be considered by the evaluator. I don't really agree with this, should we push more in the CG to understand why this is in the spec?
- Should the instantiated policy that allows shopping preferences to be read to show alternative products be an `odrl:Agreement`? Also, a legal basis should be recorded -- is it still consent in this case?
- Regarding the legitimate interest policy, `dpv:LegitimateInterest` should be the legal basis and not the purpose. To be even more specific we can GDPR legitimate interest (https://w3id.org/dpv/legal/eu/gdpr#A6-1-f). Purpose can be something like `dpv:dpv:OptimisationForConsumer` or `dpv:ServiceUsageAnalytics`. Anonymisation should be a duty?

```turtle
@prefix dpv: <http://www.w3.org/ns/dpv#>.
@prefix odrl: <http://www.w3.org/ns/odrl/2/>.
@prefix ex: <http://example.org/>.
@prefix dct: <http://purl.org/dc/terms/>.

<urn:uuid:legitimate-interest-policy> a odrl:Offer;
  dct:description "Instantiated policy that allows Delfour to keep pseudo-anomymised data about users' purchasing habits under legitimate interest.";
  odrl:permission <urn:uuid:purchasing-habits-permission>.

<urn:uuid:purchasing-habits-permission> a odrl:permission ;
  odrl:action odrl:read, odrl:archive;
  odrl:target ex:shoppingList; # TBD: the actual location is still required
  odrl:assignee ex:delfour;
  odrl:assigner <https://ruben.verborgh.org/profile/#me>;
  odrl:constraint <urn:uuid:legitimate-interest-legal-basis>, <urn:uuid:legitimate-interest-purpose> ;
  odrl:duty <urn:uuid:pseudo-anonymisation-duty> .

<urn:uuid:legitimate-interest-legal-basis> a odrl:Constraint;
  odrl:leftOperand dpv:LegalBasis ;
  odrl:operator odrl:eq ;
  odrl:rightOperand dpv:LegitimateInterest.

<urn:uuid:legitimate-interest-purpose> a odrl:Constraint;
  odrl:leftOperand odrl:purpose ;
  odrl:operator odrl:eq ;
  odrl:rightOperand dpv:OptimisationForConsumer.

<urn:uuid:pseudo-anonymisation-constraint> a odrl:Duty;
  odrl:action dpv:Pseudonymisation;
  odrl:target ex:shoppingList;
  odrl:assignee ex:delfour;
  odrl:assigner <https://ruben.verborgh.org/profile/#me>;
  odrl:constraint <urn:uuid:temporalConstraint>.

<urn:uuid:temporalConstraint> a odrl:Constraint;
  odrl:leftOperand odrl:dateTime;
  odrl:operator odrl:lt;
  odrl:rightOperand "2024-02-12T11:20:10.999Z"^^<http://www.w3.org/2001/XMLSchema#dateTime>. # NOTE: should represent the timestamp after scanner is put back in place; This time could be better encoded with the dynamic policy feature (but for now that might complicate stuff) 
```