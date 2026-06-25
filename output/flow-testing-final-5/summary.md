# Discovery: Flow_testing_final_5

## PART 1 - RELEVANT METADATA TYPES
- Flows (Scheduled-Triggered)
- SObjects (Opportunity, Task)
- Fields (Opportunity.CloseDate, Opportunity.StageName, Opportunity.OwnerId, Opportunity.Id; Task.Subject, Task.WhatId, Task.OwnerId, Task.Priority, Task.Status)
- Picklist / GlobalValueSets (Opportunity.StageName, Task.Priority, Task.Status)

## PART 2 - COMPONENT INVENTORY
### Flow
- Flow_testing_final_5 (Scheduled, Daily 09:00, Active)
  - GetOverdueOpps (Get Records / Opportunity)
  - LoopThroughOpps (Loop / GetOverdueOpps collection)
  - CreateReminderTask (Create Records / Task)

### SObject
- Opportunity (source)
- Task (target)

### Field
- Opportunity.CloseDate (Date)
- Opportunity.StageName (Picklist)
- Opportunity.OwnerId (Lookup(User))
- Opportunity.Id (Id)
- Task.Subject (Picklist)
- Task.WhatId (Lookup)
- Task.OwnerId (Lookup(User))
- Task.Priority (Picklist)
- Task.Status (Picklist)

### PicklistValue
- Opportunity.StageName: Closed Won
- Task.Priority: High
- Task.Status: Not Started

## PART 3 - DIRECT DEPENDENCIES
- Flow:Flow_testing_final_5 -> SObject:Opportunity [category=DataModel, evidence=GetOverdueOpps queries Opportunity]
- Flow:Flow_testing_final_5 -> SObject:Task [category=DataModel, evidence=CreateReminderTask creates Task]
- Flow.Element:GetOverdueOpps -> Field:Opportunity.CloseDate [category=DataModel, evidence=filter CloseDate < $Flow.CurrentDate]
- Flow.Element:GetOverdueOpps -> Field:Opportunity.StageName [category=DataModel, evidence=filter StageName != Closed Won]
- Flow.Element:CreateReminderTask -> Field:Task.Subject [category=DataModel, evidence=Subject = Overdue Opportunity Followup]
- Flow.Element:CreateReminderTask -> Field:Task.WhatId [category=DataModel, evidence=WhatId = LoopThroughOpps.Id]
- Flow.Element:CreateReminderTask -> Field:Task.OwnerId [category=DataModel, evidence=OwnerId = LoopThroughOpps.OwnerId]
- Flow.Element:CreateReminderTask -> Field:Task.Priority [category=DataModel, evidence=Priority = High]
- Flow.Element:CreateReminderTask -> Field:Task.Status [category=DataModel, evidence=Status = Not Started]

## PART 4 - INDIRECT DEPENDENCIES
- Flow.Element:GetOverdueOpps -> Flow.Element:LoopThroughOpps [category=Automation, evidence=connector]
- Flow.Element:LoopThroughOpps -> Flow.Element:CreateReminderTask [category=Automation, evidence=nextValueConnector]
- Flow.Element:CreateReminderTask -> Field:Opportunity.OwnerId [category=DataModel, evidence=Task.OwnerId derived from looped Opportunity.OwnerId]
- Flow.Element:CreateReminderTask -> Field:Opportunity.Id [category=DataModel, evidence=Task.WhatId derived from looped Opportunity.Id]

## PART 5 - MISSING INPUTS / ACCESS GAPS
- None. All fields and picklist values validated against org `omnistudio-org`. Deployment succeeded (id 0AfdL00000cSL22SAG).
- Note: Original blueprint requested resource-referenced Start filter; Salesforce disallows resource references in Scheduled Start filters, so the overdue-date filter was implemented in the GetOverdueOpps Get Records element (canonical pattern).

## PART 6 - CONFIDENCE
High
