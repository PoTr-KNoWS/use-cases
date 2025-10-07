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
- ODRL FS spec says that offers MUST NOT be considered by the evaluator. The justification was that in the offer term definition the following is stated "The Offer Policy MAY contain a Party with Assignee function, but MUST not grant any privileges to that Party.". However, `odrl:Set` policies contain the following in the definition "No privileges are granted to any Party (if defined).", and MUST be considered by the evaluator. I don't really agree with this, should we push more in the CG to understand why this is in the spec?