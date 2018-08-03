# Notes towards BIBFRAME to MARC21 bibliographic conversion

## What goes into a MARC record

MARC21 bibliographic records can be constructed from an RDF graph with the following characteristics:

* One or more `bf:Instance` subjects with one or more `bf:instanceOf` predicates with a `bf:Work` object.
  * The `bf:instanceOf` predicate can also be calculated as the inverse of a `bf:hasInstance` predicate of a `bf:Work` subject.
  * Each `bf:Instance` to `bf:Work` relationship can generate a new MARC record.
    * A single `bf:Work` can be related to multiple `bf:Instance` nodes. The combination of the predicates of the `bf:Work` and each of the `bf:Instance` nodes can be used to construct multiple unique MARC21 bibliographic records.
      * Series and linking relationships (e.g. `bf:otherPhysicalFormat`, `bf:reproductionOf`) can be used to infer whether or not a unique MARC21 bibliographic record should be constructed.
    * A single `bf:Instance` can be related to multiple `bf:Work` nodes, but in general this _should not_ result in the construction of multiple unique MARC21 bibliographic records.
      * Series and linking relationships should be used to infer which `bf:Work` node should be used in the construction of the record.
      * If a "primary" `bf:Work` node can not be inferred, multiple records will be constructed.

Required fields in a MARC21 bibliographic record are the leader and the control number field (`001`).
* Leader
  * The generated leader should be appropriate to the output format of the MARC21 record. For MARCXML:
    * Positions 00-04 should be blanks (`     `).
    * Position 09 should be `a` ("UCS/Unicode").
    * Positions 12-16 should be blanks (`     `).
  * Record status (LDR/05) should be `n` ("New").
  * The remainder of the leader can be constructed from triples in the graph.
* `001`
  * The standard requires that the control number field be "an ASCII graphic character string uniquely associated with the record by the organization transmitting the record"
  * This could be constructed as the triple `<bf:Instance> bf:instanceOf <bf:Work>` (substituting in the IRIs for the `bf:Instance` and `bf:Work`)
