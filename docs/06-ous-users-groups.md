# Phase 6: Organizational Units, Users, and Groups

## What I did
- Created 3 OUs under corp.local: IT, Sales, HR
- Created 2 regular users in each OU (6 total)
- Created a separate dedicated admin account in IT, distinct from regular employee accounts
- Created a security group (IT-Admins) and added the admin account as a member

## Why I made these choices

**Why separate OUs instead of one flat list:**
I created separate OUs instead of one flat list because OUs are the level at which Group Policies get applied in Active Directory. Having IT, Sales, and HR as distinct OUs means I can later apply different policies to each department individually (e.g., different desktop restrictions for IT vs Sales) rather than one policy affecting everyone.

**Why a dedicated admin account instead of using regular employee accounts:**
I created a dedicated admin account separate from my regular employee accounts to reflect the principle of separating privileged access from day-to-day use. In a real environment, if a regular employee's account is compromised (phishing, weak password, etc.), an attacker only gains regular-user access, not admin rights — because admin privileges live on a separate account rather than being attached to someone's everyday login.

## Problems I ran into
None — this phase went smoothly start to finish.
