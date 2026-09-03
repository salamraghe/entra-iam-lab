# Entra IAM Lab

Hands-on Microsoft Entra ID lab in a real tenant: users, groups, P2 licensing, Conditional Access MFA on a security group, and a leaver offboard.

Only items with a dump are marked done. Finish-user screens that showed temporary passwords were not used. AD and Okta live in other repos.

## Scope

| # | Item | Dump |
| --- | --- | --- |
| 1 | Tenant, Entra ID P2 trial, labadmin | Done (2 Sep 2026) |
| 2 | testuser, leaver, MFA-Test-Group | Done (2 Sep 2026) |
| 3 | Conditional Access / MFA on MFA-Test-Group | Done (3 Sep 2026) |
| 4 | Offboard leaver | Done (3 Sep 2026) |
| 5 | One enterprise app (SAML or OIDC) | Not dumped |

## Environment

| Object | Value |
| --- | --- |
| Tenant | Default Directory, `salamraghegmail.onmicrosoft.com` |
| Portal | Microsoft Entra admin center and Microsoft 365 admin center, signed in as Lab Admin |
| P2 trial | Microsoft Entra ID P2 Managed Trial, Active, purchased quantity 100, expires **Sep 30, 2026** |
| Also listed | Microsoft Entra ID Free (purchased quantity 1) |
| Security defaults | Disabled (required before a Conditional Access policy can be created) |
| CA policy | `Require MFA for MFA-Test-Group` — State **On**, include `MFA-Test-Group`, All resources, Grant Require multifactor authentication. Created 3 Sep 2026 2:20 PM, modified 2:25 PM |
| labadmin | `labadmin@salamraghegmail.onmicrosoft.com` |
| testuser | `testuser@salamraghegmail.onmicrosoft.com` — Entra ID P2 still assigned, role User, member of MFA-Test-Group |
| leaver | `Leaver@salamraghegmail.onmicrosoft.com` — Disabled, sessions revoked, P2 unassigned, 0 groups |
| guest | `salamraghe_gmail.com#EXT#@salamraghegmail.onmicrosoft.com` |
| Group | `MFA-Test-Group` — Security, cloud, role assignment disabled, description `Users required to use MFA in the lab`, created Sep 2, 2026 8:29 PM |

## Screenshots (2 Sep 2026 dump)

- [Users list](screenshots/01-admin-center-users.png)
- [P2 trial on Products](screenshots/02-p2-trial-products.png)
- [Test User review / P2 assigned](screenshots/06-testuser-review.png)
- [MFA-Test-Group review](screenshots/16-group-review.png)
- [MFA-Test-Group created](screenshots/17-group-created.png)
- [Security groups list](screenshots/18-security-groups-list.png)
- [Add members picker](screenshots/22-add-members-picker.png)
- [testuser is the member](screenshots/23-testuser-member.png)

## Screenshots (3 Sep 2026 dump)

- [CA create blocked until security defaults are disabled](screenshots/24-ca-blocked-by-security-defaults.png)
- [Security defaults set to Disabled](screenshots/25-security-defaults-disabled.png)
- [Policy includes MFA-Test-Group](screenshots/26-policy-include-mfa-test-group.png)
- [Target resources: All resources](screenshots/27-policy-all-resources.png)
- [Grant: Require multifactor authentication](screenshots/28-grant-require-mfa.png)
- [Policy details: 1 group, All resources, Require MFA](screenshots/29-policy-details-one-group.png)
- [Enable toast vs list still Report-only](screenshots/30-policy-enable-toast-list-lag.png)
- [Policy State On](screenshots/31-policy-state-on.png)
- [What if: Test User, Office 365 Exchange Online](screenshots/32-what-if-testuser-exo.png)
- [What if: policy applies, Grant Require MFA, State On](screenshots/33-what-if-require-mfa-applies.png)
- [Leaver User Disabled](screenshots/34-leaver-disabled.png)
- [Leaver sessions revoked](screenshots/35-leaver-sessions-revoked.png)
- [P2 unassigned from leaver; Test User still licensed](screenshots/36-p2-unassigned-testuser-licensed.png)

## What was built

1. Confirmed the P2 Managed Trial on **Products** (expires Sep 30, 2026).
2. Created **Test User** with Entra ID P2 and no admin role.
3. Created **Leaver User**. Same P2. Not added to the group.
4. Created security group **MFA-Test-Group**. Role assignment left off. Added **testuser** as the only member.
5. Disabled **security defaults** so a custom Conditional Access policy could be created.
6. Created **Require MFA for MFA-Test-Group**: include that group, All resources, Grant Require MFA. Enabled the policy (State On).
7. Ran **What if** for Test User against Office 365 Exchange Online. The policy applies with Grant Require multifactor authentication.
8. Offboarded **Leaver User**: account Disabled, sessions revoked, Entra ID P2 unassigned in Microsoft 365 admin center. Test User kept the license.

## How to redo it

Portals: [admin.microsoft.com](https://admin.microsoft.com) and [entra.microsoft.com](https://entra.microsoft.com) as `labadmin@salamraghegmail.onmicrosoft.com`.

Track 0 (users and group):

1. **Home → Your organization → Products.** Confirm **Microsoft Entra ID P2 Managed Trial** is Active and note the expiration date.
2. **Add user.** Basics: display name Test User, username `testuser`, domain `salamraghegmail.onmicrosoft.com`. Product licenses: assign **Microsoft Entra ID P2**, location United States. Optional settings: leave **Roles** as User (no administration access). Review and finish.
3. Repeat Add user for **Leaver User** / `Leaver`. Same P2 license. Do not make this account an admin.
4. **Groups → Active groups → Security groups → Add a security group.** Name `MFA-Test-Group`. Leave **Azure AD roles can be assigned to the group** unchecked. Create.
5. Open the group → **Members → Add members.** Select **Test User** only. Save. Do not add labadmin or leaver.

Conditional Access:

6. **Entra ID → Conditional Access → Policies → New policy.** If the form warns that security defaults must be disabled first, open **Properties → Manage security defaults**, set **Disabled**, choose reason **My organization is planning to use Conditional Access**, Save.
7. Name the policy `Require MFA for MFA-Test-Group`. Users: Select users and groups → **MFA-Test-Group**. Target resources: **All resources**. Grant: **Require multifactor authentication**. Enable policy: **On**. Create / Save.
8. Confirm the list shows State **On**. If it still says Report-only after the success toast, wait and Refresh.
9. **What if.** Identity: Test User. Cloud app: Office 365 Exchange Online (or another All-resources app). Run What if. Expect **Require MFA for MFA-Test-Group** under policies that will apply, Grant Require multifactor authentication, State On.

Offboard:

10. Open **Leaver User**. Set **Account enabled** off. Save. Confirm Overview shows **Disabled**.
11. **Revoke sessions.** Confirm the success toast.
12. Unassign P2 from the M365 admin center (**Billing → Licenses → Microsoft Entra ID P2**), not the Entra user Licenses tab. Confirm leaver is gone from the assigned list and **Test User** remains.

Docs used: [Create users](https://learn.microsoft.com/en-us/entra/fundamentals/how-to-create-delete-users), [Manage groups](https://learn.microsoft.com/en-us/entra/fundamentals/how-to-manage-groups), [Assign licenses](https://learn.microsoft.com/en-us/microsoft-365/admin/manage/assign-licenses-to-users), [Security defaults](https://learn.microsoft.com/en-us/entra/fundamentals/security-defaults), [Conditional Access](https://learn.microsoft.com/en-us/entra/identity/conditional-access/overview), [What if](https://learn.microsoft.com/en-us/entra/identity/conditional-access/what-if-tool), [Revoke sessions](https://learn.microsoft.com/en-us/entra/identity/users/users-revoke-access).

## What broke

- **Security defaults blocked the New policy form** with “You must first disable security defaults.” Defaults started Enabled. The dump shows the dropdown set to Disabled; the Save click after picking a reason was not captured. Policy create after that is the proof they were off.
- **Policy list lagged after enable.** Toast: “Successfully updated Require MFA for MFA-Test-Group. Policy will be enabled in a few minutes.” The same sitting’s list still showed **Report-only**. Later refresh (3 Sep, modified 2:25 PM) shows State **On**.
- **Entra user Licenses tab cannot unassign.** Banner: add/remove licenses only in Microsoft 365 admin center. Offboard of P2 was done there. After unassign, toast “Licenses have been unassigned successfully.” Leaver is gone from the P2 assigned list; Test User remains.
- **No MFA challenge screenshot.** Sign-in logs filtered to `testuser@salamraghegmail.onmicrosoft.com` returned no interactive sign-ins. Proof the policy hits Test User is **What if**, not a live MFA prompt.
- **Products tab showed 0 assigned** on 2 Sep after Test User was created with Entra ID P2 on the review screen. Treated as lag. Proof of assignment is the user wizard, then the 3 Sep P2 assigned list.
- **Active groups** defaulted to the **Microsoft 365 groups** tab, which stayed empty. The group is a **Security group**.
- **MFA-Test-Group** started at 0 members. Member add is a separate step after create. 3 Sep dump did not reopen the members list; membership proof remains the 2 Sep member shot. What if applying to Test User is consistent with that membership.

## Hiring screen-share

Walk this in entra.microsoft.com and admin.microsoft.com, not a slide deck.

1. **Users:** Lab Admin, Test User, Leaver User (Disabled), plus the Gmail guest. Point at testuser as a normal user with P2 and no admin role.
2. **Products / Licenses:** P2 Managed Trial, expiration Sep 30, 2026. Test User still assigned. Leaver not on the P2 assigned list.
3. **Groups → Security groups:** `MFA-Test-Group`, cloud, created Sep 2, 2026 8:29 PM, role assignment disabled. Test User is the member.
4. **Conditional Access → Policies:** `Require MFA for MFA-Test-Group`, State On. Details: 1 group, All resources, Require multifactor authentication.
5. **What if:** Test User, Office 365 Exchange Online. Policy applies, Grant Require MFA.
6. **Leaver User:** Account status Disabled. Sessions revoked toast. P2 unassigned from M365 licenses.

Stop there. This dump does not show an enterprise app (SAML or OIDC).

## Dated progress

| When | What the dump shows |
| --- | --- |
| 2026-09-02 | P2 Managed Trial Active, expires Sep 30, 2026 |
| 2026-09-02 | Test User created with Entra ID P2, not admin |
| 2026-09-02 | Leaver User present in the tenant, P2 assigned, not in the group |
| 2026-09-02 8:29 PM | MFA-Test-Group created (Security, role assignment off) |
| 2026-09-02 | testuser added as the sole group member |
| 2026-09-03 ~1:54–2:25 PM | Security defaults Disabled; policy created include MFA-Test-Group, All resources, Grant Require MFA. List lagged Report-only after enable toast |
| 2026-09-03 2:25 PM | Policy modified; later list shows State On |
| 2026-09-03 ~3:07–3:46 PM | What if Test User: policy applies, Grant Require MFA, State On |
| 2026-09-03 ~3:46 PM | Leaver Disabled, sessions revoked, P2 unassigned. Test User still licensed |
