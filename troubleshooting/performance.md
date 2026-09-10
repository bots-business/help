---
description: Diagnose slow commands and timeouts by separating BJS work, external requests, scheduled tasks, broadcasts, and
  quota limits.
---


# Fix slow commands and timeouts

{% hint style="info" %}
A slow reply can come from the command itself, an external service, repeated background work, or unavailable resources. Begin by comparing a plain Answer command with the slow scenario in the same bot.
{% endhint %}

## Narrow down the delay

1. Open **Errors** and look for timeouts or repeated failures around the affected time.
2. Check whether a simple command responds normally.
3. Inspect the slow command's external requests and commands it starts.
4. Inspect **Broadcast** for large active tasks and your command metadata for Auto Retry.
5. Check **Iterations** and the current plan status.

If a simple answer works and one integration is slow, investigate that integration before changing the bot's hosting plan.

## Common causes

- **Long command chains:** each command can perform additional work. Remove unnecessary hops and circular calls.
- **Repeated scheduling:** starting the same loop repeatedly creates more work than its interval suggests. Use a deliberate lifecycle and the documented cancellation options.
- **Many external requests:** a slow or failing service adds latency. Handle success and error callbacks and avoid polling unchanged data too often.
- **Large broadcasts:** test the sending command and inspect failed delivery separately from overall progress.
- **Oversized synchronous work:** split work into bounded tasks only where the runtime supports that execution pattern.

See [HTTP](../bjs/http.md), [background work](../bjs/background.md), [broadcasting](../bjs/broadcasts.md), and [caching](../bjs/caching.md) for the actual options.

## Understand limits

Each [HTTP network request](../bjs/http.md#timing-redirects-and-retries) has a fixed **5-second timeout**, with no supported BJS override. Each redirect request has the same limit. Overall command execution has a separate budget; scheduling work does not remove either limit. Changing the bot plan does not increase the fixed HTTP timeout.

For a service operation that takes longer, request a job ID and check its status in a later scheduled command. Confirm the overall command budget for your current configuration; historical plan tables are not a reliable source for a newly deployed configuration.

An iteration count measures accounted work, not elapsed time or successful deliveries. A large allowance does not correct an infinite loop.

## Verify an improvement

Repeat the same input with the same dependencies, check that the result is still correct, and inspect new errors. Avoid calling a change successful just because the bot sent an earlier acknowledgement while its actual work still fails later.

If you need support, provide the bot ID, command, times, whether the problem affects all commands, and relevant errors. Do not include credentials or full private payloads.
