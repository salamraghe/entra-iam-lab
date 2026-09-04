# Entra IAM Lab

Hands-on Microsoft Entra ID lab in a real tenant: users, groups, P2 licensing, Conditional Access MFA on a security group, a leaver offboard, and one enterprise app SAML SSO.

Only items with a dump are marked done. Finish-user screens that showed temporary passwords were not used. AD and Okta live in other repos.

## Scope

| # | Item | Dump |
| --- | --- | --- |
| 1 | Tenant, Entra ID P2 trial, labadmin | Done (2 Sep 2026) |
| 2 | testuser, leaver, MFA-Test-Group | Done (2 Sep 2026) |
| 3 | Conditional Access / MFA on MFA-Test-Group | Done (3 Sep 2026) |
| 4 | Offboard leaver | Done (3 Sep 2026) |
| 5 | One enterprise app (SAML) — Microsoft Entra SAML Toolkit | Done (3 Sep 2026) |

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
| Enterprise app | `Microsoft Entra SAML Toolkit` — gallery enterprise app, Object ID `93e51ff6-c0b9-4e30-9906-8d578688414f` |
| SAML SSO | Identifier `https://samltoolkit.azurewebsites.net`; Reply `https://samltoolkit.azurewebsites.net/SAML/Consume/22318`; Sign on `https://samltoolkit.azurewebsites.net/SAML/Login/22318` |
| App assignment | `MFA-Test-Group` assigned (role `msiam_access`) |

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


## Screenshots (3 Sep 2026 SAML dump)

- [Microsoft Entra SAML Toolkit added](screenshots/37-saml-toolkit-app-added.png)
- [MFA-Test-Group assigned to the app](screenshots/38-saml-mfa-test-group-assigned.png)
- [Basic SAML saved: Consume/Login 22318](screenshots/39-saml-basic-config-22318-saved.png)
- [Toolkit SP config 22318](screenshots/40-saml-toolkit-sp-config-22318.png)
- [My Apps InPrivate: Toolkit tile as testuser](screenshots/41-myapps-testuser-toolkit-tile.png)
- [SP Login URL 22318](screenshots/42-saml-login-22318.png)
- [SSO proof: Toolkit signed in as testuser](screenshots/43-saml-sso-proof-testuser.png)

## What was built

1. Confirmed the P2 Managed Trial on **Products** (expires Sep 30, 2026).
2. Created **Test User** with Entra ID P2 and no admin role.
3. Created **Leaver User**. Same P2. Not added to the group.
4. Created security group **MFA-Test-Group**. Role assignment left off. Added **testuser** as the only member.
5. Disabled **security defaults** so a custom Conditional Access policy could be created.
6. Created **Require MFA for MFA-Test-Group**: include that group, All resources, Grant Require MFA. Enabled the policy (State On).
7. Ran **What if** for Test User against Office 365 Exchange Online. The policy applies with Grant Require multifactor authentication.
8. Offboarded **Leaver User**: account Disabled, sessions revoked, Entra ID P2 unassigned in Microsoft 365 admin center. Test User kept the license.

9. Added gallery enterprise app **Microsoft Entra SAML Toolkit**. Assigned **MFA-Test-Group**.
10. Configured SAML SSO: Identifier `https://samltoolkit.azurewebsites.net`, Reply `/SAML/Consume/22318`, Sign on `/SAML/Login/22318`. Saved successfully.
11. On the Toolkit site as **testuser**, created SP SAML config **22318** (Login URL, Entra Identifier, Logout URL, cert).
12. Proved SSO from InPrivate **myapps.microsoft.com** as testuser: Toolkit tile → Login/22318 → signed in as `testuser@salamraghegmail.onmicrosoft.com`.

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


Enterprise app SAML:

13. **Entra ID → Enterprise applications → New application → Microsoft Entra SAML Toolkit.** Create. Confirm the added-successfully toast.
14. **Users and groups → Add user/group.** Select **MFA-Test-Group**. Assign (Default Access / `msiam_access`).
15. **Single sign-on → SAML.** Edit Basic SAML Configuration. Identifier `https://samltoolkit.azurewebsites.net`. Reply URL `https://samltoolkit.azurewebsites.net/SAML/Consume/22318` (create the Toolkit SP config first if the connection id is not known yet). Sign on URL `https://samltoolkit.azurewebsites.net/SAML/Login/22318`. Save.
16. Download Certificate (Base64). Copy Login URL, Microsoft Entra Identifier, Logout URL from Set up Microsoft Entra SAML Toolkit.
17. In a browser as **testuser**, open `https://samltoolkit.azurewebsites.net`, register/sign in matching the UPN, **SAML Configuration → Create**, paste IdP values and upload the cert. Note Login/Consume **22318**. Paste those URLs back into Entra Basic SAML if needed and Save.
18. InPrivate: `https://myapps.microsoft.com` as testuser → **Microsoft Entra SAML Toolkit** → Log in on Login/22318. Expect Toolkit home signed in as testuser.

Docs used: [Create users](https://learn.microsoft.com/en-us/entra/fundamentals/how-to-create-delete-users), [Manage groups](https://learn.microsoft.com/en-us/entra/fundamentals/how-to-manage-groups), [Assign licenses](https://learn.microsoft.com/en-us/microsoft-365/admin/manage/assign-licenses-to-users), [Security defaults](https://learn.microsoft.com/en-us/entra/fundamentals/security-defaults), [Conditional Access](https://learn.microsoft.com/en-us/entra/identity/conditional-access/overview), [What if](https://learn.microsoft.com/en-us/entra/identity/conditional-access/what-if-tool), [Revoke sessions](https://learn.microsoft.com/en-us/entra/identity/users/users-revoke-access).

## What broke

- **Security defaults blocked the New policy form** with “You must first disable security defaults.” Defaults started Enabled. The dump shows the dropdown set to Disabled; the Save click after picking a reason was not captured. Policy create after that is the proof they were off.
- **Policy list lagged after enable.** Toast: “Successfully updated Require MFA for MFA-Test-Group. Policy will be enabled in a few minutes.” The same sitting’s list still showed **Report-only**. Later refresh (3 Sep, modified 2:25 PM) shows State **On**.
- **Entra user Licenses tab cannot unassign.** Banner: add/remove licenses only in Microsoft 365 admin center. Offboard of P2 was done there. After unassign, toast “Licenses have been unassigned successfully.” Leaver is gone from the P2 assigned list; Test User remains.
- **No MFA challenge screenshot.** Sign-in logs filtered to `testuser@salamraghegmail.onmicrosoft.com` returned no interactive sign-ins. Proof the policy hits Test User is **What if**, not a live MFA prompt.
- **Products tab showed 0 assigned** on 2 Sep after Test User was created with Entra ID P2 on the review screen. Treated as lag. Proof of assignment is the user wizard, then the 3 Sep P2 assigned list.
- **Active groups** defaulted to the **Microsoft 365 groups** tab, which stayed empty. The group is a **Security group**.
- **MFA-Test-Group** started at 0 members. Member add is a separate step after create. 3 Sep dump did not reopen the members list; membership proof remains the 2 Sep member shot. What if applying to Test User is consistent with that membership.

- **Toolkit connection id comes after SP create.** Early Basic SAML drafts used Reply `/SAML/Consume` without `/22318`. Final saved config uses Consume/Login **22318** after the Toolkit SP config row existed.
- **Gallery Test SSO vs My Apps proof.** Sitting used InPrivate My Apps as testuser for end-user SSO proof instead of relying only on the admin Test blade.

## Hiring screen-share

Walk this in entra.microsoft.com and admin.microsoft.com, not a slide deck.

1. **Users:** Lab Admin, Test User, Leaver User (Disabled), plus the Gmail guest. Point at testuser as a normal user with P2 and no admin role.
2. **Products / Licenses:** P2 Managed Trial, expiration Sep 30, 2026. Test User still assigned. Leaver not on the P2 assigned list.
3. **Groups → Security groups:** `MFA-Test-Group`, cloud, created Sep 2, 2026 8:29 PM, role assignment disabled. Test User is the member.
4. **Conditional Access → Policies:** `Require MFA for MFA-Test-Group`, State On. Details: 1 group, All resources, Require multifactor authentication.
5. **What if:** Test User, Office 365 Exchange Online. Policy applies, Grant Require MFA.
6. **Leaver User:** Account status Disabled. Sessions revoked toast. P2 unassigned from M365 licenses.

7. **Enterprise applications → Microsoft Entra SAML Toolkit:** Users and groups shows MFA-Test-Group. Single sign-on shows Identifier, Reply Consume/22318, Sign on Login/22318.
8. **My Apps InPrivate as testuser:** Toolkit tile → Login/22318 → signed in on the Toolkit home page.

Stop there. This dump does not show OIDC or a second enterprise app. AD and Okta are other repos.

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
| 2026-09-03 ~4:17–5:03 PM | Microsoft Entra SAML Toolkit created; MFA-Test-Group assigned; SAML Consume/Login 22318 saved; Toolkit SP config 22318; InPrivate My Apps SSO as testuser |
