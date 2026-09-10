---
description: Reset a forgotten Bots.Business password, change it from Profile, and manage sessions and account API access.
---


# Reset or change your password

Use the sign-in reset form when you cannot sign in. Use **Profile → Update password** when you know your current password and want to change it.

{% tabs %}

{% tab title="Reset by email" %}

## Reset a forgotten password

1. On the sign-in screen, tap **Forgot your password?**
2. Enter the email registered with your Bots.Business account.
3. Tap **Reset my password**.
4. Wait for the result, then check your inbox and spam folder. The current reset flow sends a new password by email.
5. Sign in with that password and change it from **Profile** if needed.

Check for a typing error if the account is not found. A failed reset can require support; repeated requests do not resolve an account restriction. If you cannot access the registered email, open the [Recover with linked Telegram](#recover-access-without-your-email) tab. Contact support if you do not meet its requirements.

{% endtab %}

{% tab title="Recover with linked Telegram" %}

## Recover access without your email

The official [@BotsBusinessAdminBot](https://t.me/BotsBusinessAdminBot) provides **Password Recovery** for an eligible account. You need:

- A Telegram account that was **already linked to the same Bots.Business account**. Having a bot token alone is not enough.
- A valid token for a bot owned by that account.
- An account that is not a Cloud account; this recovery path excludes Cloud.

1. Open the official bot using the link above and check its exact username, **BotsBusinessAdminBot**.
2. Choose **Password Recovery**. Follow the recovery prompts in that private conversation using your previously linked Telegram identity.
3. When the official recovery flow asks for the bot token, provide the token for your own bot. Do not post it to a support group, send it to a person, or include it in a screenshot.
4. After a successful check, use the new password to sign in to the account shown by the recovery flow. Change the password from Profile if needed.
5. Reconnect integrations that used the old Bots.Business API key.

{% hint style="warning" %}
Successful Telegram recovery **logs out existing app sessions and replaces the account API key**. This differs from [changing a known password](#change-a-known-password). Do not assume an existing MCP or VS Code connection still has valid credentials afterward.
{% endhint %}

If the bot cannot find the token, confirm that it belongs to the intended bot and has not been replaced. If Telegram is unlinked or linked to a different account, use email recovery or support; repeating the token will not establish ownership. If **Password Recovery** is unavailable in the official bot's current menu, contact support through the app's support links.

See [linking your Telegram account](linked-accounts.md) while you still have access to your account.

{% endtab %}

{% tab title="Change a known password" %}

## Change a known password

1. Open **Settings → Profile**.
2. Find **Update password**.
3. Enter **Current password**, **New password**, and **New password confirmation**.
4. Tap **Update password** and wait for **Password updated**.

The current-password check and matching confirmation must succeed. If the form fails, read its error rather than assuming the password changed.

{% endtab %}

{% endtabs %}

## Sessions and API keys

Profile also has **Log out of all devices** and **Reset Api Key**. These are separate actions from editing your password.

Logging out of all devices removes active app sessions. Resetting the account API key replaces the credential used by integrations; integrations using the old key need to be updated. Keep these actions separate when investigating lost access so that you know which credential changed.

Never send a password, API key, or bot token in a public support message. You can describe the failed step, show its error, and identify the account through the support process without publishing credentials.

See also [linked Telegram accounts](linked-accounts.md) and [Bots.Business MCP](../integrations/mcp.md).

For changing the account email or finding account access controls, see [Manage your profile and email](profile.md).
