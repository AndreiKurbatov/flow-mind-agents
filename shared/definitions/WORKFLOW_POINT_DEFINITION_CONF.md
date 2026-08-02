### WORKFLOW_POINT_DEFINITION

A workflow point is a high-level structural point in the normal workflow where the primary process object reaches a meaningful new state, phase, route, branch, merge, or completion condition.

A workflow point is not a small task, UI action, actor action, document operation, message, or technical step. It represents the movement of the primary process object through the parent workflow.

The agent must build workflow points from the perspective of the primary process object.

A good workflow point should answer one of these questions:

* What important state has the primary process object reached?
* What major phase of the workflow has the primary process object entered?
* Has the primary process object reached a normal decision or routing point?
* Has the primary process object entered a normal route variant?
* Have normal route variants converged?
* Has the primary process object reached a normal completion or exit state?

Good workflow points are usually object-centric, not actor-centric.

Good examples:

* “Discharge case enters the workflow”

* “Discharge case is assessed for discharge readiness”

* “Discharge case is routed to standard discharge planning”

* “Discharge case is routed to complex discharge planning”

* “Normal discharge routes converge before final completion”

* “Discharge case reaches normal discharge completion”

* “Claim is received for assessment”

* “Claim is checked for basic eligibility”

* “Claim follows standard evaluation route”

* “Claim follows manual review route”

* “Claim reaches final decision state”

* “Claim is closed after normal processing”

* “Contract enters approval workflow”

* “Contract is assessed for approval requirements”

* “Contract follows legal review route”

* “Contract follows direct approval route”

* “Approval routes converge”

* “Contract reaches approved completion state”

Bad workflow points are too local, too technical, or too actor-centric.

Bad examples:

* “User clicks the submit button”
* “Nurse opens the system”
* “Doctor sends an email”
* “Admin prints the document”
* “Manager fills a field”
* “System saves the record”
* “Operator uploads a file”
* “Notification is sent”
* “PDF is generated”
* “Database row is updated”

These may be real tasks, but they are too detailed for normal-flow topology.

A workflow point may represent different logical roles even if the node_type is always MAIN_FLOW_POINT:

* entry point: the primary process object enters the parent workflow;
* flow point: the primary process object reaches a major normal phase or state;
* route split: the primary process object can proceed through multiple normal route variants;
* route variant: the primary process object follows one expected normal path;
* merge point: multiple normal variants converge into a shared continuation;
* exit point: the primary process object reaches a normal end state of the parent workflow.

The agent should not create separate node types for these roles unless the output schema supports them. Instead, the role should be understandable from the node name, description, previous_node_ids, and next_node_ids.

Abstraction rule:

A workflow point should be more specific than “workflow starts”, “workflow continues”, or “workflow ends”, but more abstract than individual operational tasks.

Too vague:

* “Workflow starts”
* “Processing happens”
* “Workflow ends”

Too detailed:

* “User opens the form”
* “Field is validated”
* “Email is sent”
* “Document is printed”

Appropriate level:

* “Request enters the workflow”
* “Request is assessed for completeness”
* “Request follows standard processing route”
* “Request follows additional review route”
* “Processing routes converge”
* “Request reaches normal completion”

Primary object rule:

Every workflow point must be checked against the primary_process_object.

The node should describe what happens to the primary process object, not merely what an actor does.

Prefer:

* “Claim is assessed for eligibility”

Avoid:

* “Analyst checks the claim”

Prefer:

* “Discharge case reaches documentation-ready state”

Avoid:

* “Nurse prepares documents”

Prefer:

* “Contract reaches legal-review-ready state”

Avoid:

* “Legal team receives an email”

Alternative candidate rule:

Alternative process object candidates are not additional primary objects.

Use alternative_candidates only to avoid confusing the primary process object with supporting documents, actors, requests, outputs, systems, or downstream objects.

Do not build the normal-flow topology around an alternative candidate unless the boundary result explicitly changes the primary object.

For example, if the primary process object is “discharge case” and “patient” is an alternative candidate, the topology should follow the discharge case lifecycle, not the full physical or clinical lifecycle of the patient.

If the primary process object is “claim” and “medical report” is an alternative candidate, the topology should follow the claim lifecycle, not the document lifecycle of the medical report.

If the primary process object is “contract” and “approval request” is an alternative candidate, the topology should follow the contract approval lifecycle unless the boundary result clearly says that the approval request is the real object being modeled.

Normal route variant rule:

A workflow point may represent a normal route variant if the route is expected, non-exceptional, and part of the standard operating model.

Examples of normal route variants:

* “Case follows standard processing route”
* “Case follows complex review route”
* “Request follows digital submission route”
* “Request follows paper-based submission route”
* “Claim follows automatic assessment route”
* “Claim follows manual assessment route”

Do not create normal-flow workflow points for failures, errors, rejected paths, missing information, rework, delays, or exceptional handling unless the boundary context clearly defines them as normal expected outcomes.

If it is unclear whether a route is a normal variant or an exception, create a clarification question instead of silently including it.

Boundary rule:

Workflow points must stay inside the parent workflow boundary.

Do not create workflow points for areas that the boundary result classifies as upstream_workflows, downstream_workflows, adjacent_workflows, or excluded_scope.

If such an area appears necessary to complete the normal flow, do not silently include it. Ask a clarification question.

Quality check:

Before finalizing a workflow point, the agent should verify:

1. Does this point describe the primary process object?
2. Does it represent a meaningful state, phase, route, branch, merge, or completion condition?
3. Is it inside the parent workflow boundary?
4. Is it part of normal non-exceptional behavior?
5. Is it high-level enough to avoid task-level details?
6. Is it specific enough to be useful for later topology agents?

If the answer to any of these questions is unclear, the agent should either generalize the node, remove it, or ask a clarification question.

