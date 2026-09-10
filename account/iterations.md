---
description: Understand Bots.Business iterations, check your quota and extra points, and reduce repeated work that consumes your allowance.
---

# Understand and reduce iterations

Iterations measure bot work charged against the account's allowance. Incoming interactions, additional command executions, scheduled work, and bulk operations can all contribute. One user interaction can therefore cause more than one unit of work.

## Check your allowance

Open **Iterations** from the app menu. Read **Used iterations / Available**, the next renewal time, **Plan**, and **Extra points**.

Use the figures shown for your current account. Plan allowances and availability come from the account service; a copied historical price table or a demonstration screenshot is not your current entitlement.

When the base allowance runs out, available Extra Points can be used. Unused Extra Points carry forward. Renewal and paid-plan continuation depend on the account's plan and paid cycles, so use the displayed date and status when planning a renewal.

## What tends to increase usage

- A busy bot receives many messages or button interactions.
- One command repeatedly starts other commands.
- Every user creates a separate recurring or delayed task.
- A broadcast processes many recipient chats.
- Imports, exports, and other bulk operations perform additional work.

The counter is not a stopwatch and may be updated in batches. It is not a reliable way to measure the execution time of one BJS line.

## Reduce unnecessary work

1. Look for commands that call each other repeatedly. Draw the chain and give it a clear stopping condition.
2. Check **Auto retry time in seconds** in command metadata. Use an interval appropriate to the task and remove schedules you no longer need.
3. Review delayed jobs. Starting the same recurring loop several times can create several independent schedules.
4. Avoid repeatedly requesting unchanged external data. Use the documented [caching](../bjs/caching.md) behavior where it fits.
5. Test a broadcast on a small controlled audience before running it broadly.

For example, a single schedule that runs once every minute has 1,440 scheduled executions per day. Multiple users or duplicated schedules multiply that work; this arithmetic is not a guarantee of the final billed counter.

## When bots stop responding

Check available iterations and Extra Points, then inspect bot status and Errors. Do not assume every slow response is a quota problem. For timeouts or delayed work, follow [performance troubleshooting](../troubleshooting/performance.md).

Use **Upgrade** or **Buy more** in the app to see available options. For dedicated resources, read [Cloud](cloud.md).
