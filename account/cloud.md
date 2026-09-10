---
description: Understand Bots.Business Cloud, review the current account offer, and diagnose slow bots before changing resources.
---

# Understand Bots.Business Cloud

Bots.Business Cloud provides a server instance for your bots. General hosting shares resources with other accounts; Cloud assigns resources to the bots using your Cloud instance.

## Review the current offer

Open **Cloud** from the app menu to see the available offers and their iterations. Use **Order now** for the offered ordering path. For an existing account, check **Iterations** and its plan status as well.

The app receives plan information from the service. Check the current offer for price, allowance, and available features rather than using a screenshot from an older article. A plan's name alone does not tell you the throughput of your particular bot.

## What determines bot performance?

A short command and a command that makes several network calls do different amounts of work. The same number of users can create very different loads depending on command frequency, external requests, broadcasts, and scheduled jobs.

An iteration allowance describes accounted work. It is not a guarantee that every command finishes in a fixed time or that the bot handles a particular number of simultaneous users.

## Before changing resources

1. Check **Errors** for timeouts and repeated failures.
2. Find command chains, repeated external calls, or duplicate schedules.
3. Check the state of broadcasts and delayed work.
4. Compare a simple command with the slow scenario.
5. Keep the bot ID, affected command, and approximate time for a support request.

See [performance and timeouts](../troubleshooting/performance.md) for the diagnostic sequence and [background execution](../bjs/background.md) for scheduling constraints. More resources do not correct an infinite command loop.
