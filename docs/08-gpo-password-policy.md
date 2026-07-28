# Phase 8: Group Policy — Password Policy (IT OU → Domain Root)

## What I did
- Created `IT-PasswordPolicy` GPO, linked to the IT OU, with an 8-character minimum password length
- Tested by trying to change an IT user's password to something short — the change was rejected, but this alone didn't confirm which policy was responsible (this GPO vs. the pre-existing default domain policy)
- Ran `gpresult /r` — the GPO did **not** show as applied
- Re-linked the GPO at `corp.local` (domain root) instead of the IT OU
- Still didn't show as applied via standard `gpresult /r` after further troubleshooting
- Moved CLIENT01's computer object from the default Computers container into the IT OU
- Confirmed successful application via `gpresult /r /scope:computer`, run as a domain admin account

## Problems I ran into

**1. GPO showed as rejecting short passwords, but gpresult /r didn't show it as applied**
- What happened: A short password was correctly rejected, but `gpresult /r` showed no record of `IT-PasswordPolicy` being applied — meaning the rejection could have been coming from the pre-existing default domain password policy instead of my GPO.
- Root cause: Account Policies (password/lockout settings) are only respected by Active Directory when linked at the **domain root** — an OU-level link is silently ignored for this specific policy category, regardless of correct configuration.
- Fix: Re-linked the GPO at the domain root (`corp.local`).

**2. GPO still didn't show as applied after re-linking at the domain root**
- What happened: Tried `gpupdate /force`, a full logoff/logon, and a full VM restart — none caused the GPO to appear as applied in `gpresult /r`. Also moved CLIENT01's computer object from the default Computers container into the IT OU, still no change in the result.
- Root cause: Password policy is a **Computer Configuration** setting. A standard (non-admin) `gpresult /r` doesn't reliably show Computer Configuration policy status — the issue was with the diagnostic check itself, not the actual policy application.
- Fix: Ran `gpresult /r /scope:computer` as a domain admin account, which confirmed the GPO was genuinely applied all along.

## Lesson
A GPO not showing as "applied" doesn't always mean it isn't working — the diagnostic tool/command used can have its own visibility limitations depending on scope and permission level. Also, password policy in Active Directory fundamentally cannot be scoped to a single OU using standard GPO settings; a true per-OU password policy requires a separate feature (Fine-Grained Password Policies). As a result, this GPO now applies domain-wide rather than only to the IT OU as originally intended.

## Key takeaway
I learned that a GPO not appearing in a diagnostic check doesn't necessarily mean it isn't applying — the account/permissions used to check can limit what's visible. The policy was working the whole time; it just needed to be checked with an admin account and the correct scope to actually see it.
