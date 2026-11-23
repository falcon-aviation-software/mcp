---
name: open-work-order
description: Open a FalconMX Work Order the way Juliette does, choosing the Customer from the FAA registry when none is named, then suggesting Work Items and Estimate investigation lines. Use when the user asks to open, start or create a Work Order on an aircraft.
---

# Opening a Work Order

## 1. The Customer

If the User names who it's for, use that Customer. Otherwise, when the aircraft has no Customer here (`aircraft findAircraft` finds no aircraft, or `customers listCustomers` with its `aircraftId` returns none), don't ask first:

1. Read the aircraft's FAA registered owner with `aircraft getRegistryEntry` and use `registrant.name` as the Customer: the Customer already named that, ignoring case, if there is one; otherwise create one with that name.
2. If `findAircraft` found no aircraft, add it under that Customer with the registry's make, model and serial, and don't mention adding it.
3. Open the Work Order. Tell the User it's for the registered owner, by name, and ask whether they want a different Customer. If they name one, change it with `work_orders updateWorkOrder` `customerId`.

If the registry has no registered owner for it, or doesn't have the aircraft, ask the User who the Customer is, giving only that reason: the FAA registry lists no registered owner for it, or doesn't list it.

Never tell the User an aircraft isn't on FalconMX, in this Organization or on file, in any words.

## 2. Suggest Work Items, don't add them

Right after you open it, suggest its Work Items and add only the ones the User picks. Suggest:

- each job or problem the User named: a Squawk for something wrong, an Inspection for an annual or 100-hour;
- each Work Item left Deferred on the aircraft's newest Closed Work Order (`work_orders listWorkOrders` with its `aircraftId` and status `closed`, then `work_items listWorkItems` on that one), saying which Work Order deferred it.

With each Squawk, suggest an investigation line for the Estimate: labor at the default Labor Rate, named for finding the cause (for example "Troubleshoot rough running"), 1.0 hour unless the User gave a time.

Offer them as one multi-select question, one option per suggestion, with its investigation line in the option's detail. Then add each one the User keeps with `work_items addWorkItem`, and each investigation line they kept with `estimates addEstimateLine`, kind `labor`, with its `workItemId`.

With nothing to suggest, don't invent work: ask what the job is.
