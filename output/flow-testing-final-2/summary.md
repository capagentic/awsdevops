# Discovery: flow-testing-final-2

## PART 1 - RELEVANT METADATA TYPES
- Objects (Opportunity, Task)
- Fields (StageName, CloseDate, Amount, Id, OwnerId, Task.Subject, Task.WhatId, Task.OwnerId, Task.Priority, Task.Status)
- Picklists / GlobalValueSets (Opportunity.StageName, Task.Priority, Task.Status)
- Flows (Record-Triggered)
- WorkflowAlert (email alerts) - checked, none present on Opportunity

## PART 2 - COMPONENT INVENTORY
### Flow
- Flow_testing_final_2 (Record-Triggered, After Save, Opportunity, Active, id 301dL00002Gfv1jQAB)
### Flow Elements
- Decision: Evaluate_Stage_Change
- RecordUpdate: UpdateCloseDate
- RecordCreate: CreateThankYouTask
- RecordCreate: CreateLossTask
- RecordCreate: CreateManagerNotificationTask
- Formula: fx_StageNameChanged
### Objects
- Opportunity
- Task

## PART 3 - DIRECT DEPENDENCIES
- Flow_testing_final_2 -> Opportunity (start object / record trigger)
- Flow_testing_final_2 -> Opportunity.StageName (entry ISCHANGED + decision)
- Flow_testing_final_2 -> Opportunity.Amount (High Value Deal rule)
- Flow_testing_final_2 -> Opportunity.CloseDate (UpdateCloseDate)
- Flow_testing_final_2 -> Opportunity.Id / Opportunity.OwnerId (Task field mapping)
- Flow_testing_final_2 -> Task (record creates)

## PART 4 - INDIRECT DEPENDENCIES
- Evaluate_Stage_Change -> UpdateCloseDate -> CreateThankYouTask (Closed Won path)
- Evaluate_Stage_Change -> CreateLossTask (Closed Lost path)
- Evaluate_Stage_Change -> CreateManagerNotificationTask (High Value path)

## PART 5 - MISSING INPUTS / ACCESS GAPS
- No email alert (WorkflowAlert) exists on Opportunity and no emailAlert developer name was provided. Outcome 3 'SendManagerNotification' email action could not be created without fabricating metadata; implemented as a Create Task (CreateManagerNotificationTask) instead.
- Requested Task.Priority 'Medium' is not a valid API value in this org (valid: Low/High/Normal). Corrected CreateLossTask Priority to 'Normal'.

## PART 6 - CONFIDENCE
High - all fields and picklist values validated against org; deployment succeeded with 0 component errors.
