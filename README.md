# Entra IAM Lab

Hands-on Microsoft Entra ID lab: users, groups, P2 licensing, and an MFA-ready security group in a real tenant.

Only items with a dump are marked done. Proof is the 2026-09-02 sitting. Finish-user screens that showed temporary passwords were not used. AD and Okta live in other repos.

## Scope

| # | Item | Dump |
| --- | --- | --- |
| 1 | Tenant, Entra ID P2 trial, labadmin | Done (2 Sep 2026) |
| 2 | testuser, leaver, MFA-Test-Group | Done (2 Sep 2026) |
| 3 | Conditional Access / MFA on MFA-Test-Group | Not dumped |
| 4 | Offboard leaver | Not dumped |
| 5 | One enterprise app (SAML or OIDC) | Not dumped |

## Environment

| Object | Value |
| --- | --- |
| Tenant | Default Directory, `salamraghegmail.onmicrosoft.com` |
| Portal | Microsoft 365 admin center, simplified view, signed in as Lab Admin |
| P2 trial | Microsoft Entra ID P2 Managed Trial, Active, purchased quantity 100, expires **Sep 30, 2026** |
| Also listed | Microsoft Entra ID Free (purchased quantity 1) |
| labadmin | `labadmin@salamraghegmail.onmicrosoft.com` |
| testuser | `testuser@salamraghegmail.onmicrosoft.com` — Entra ID P2, role User (no admin center access), member of MFA-Test-Group |
| leaver | `Leaver@salamraghegmail.onmicrosoft.com` — created, P2 assigned, not in the group |
| guest | `salamraghe_gmail.com#EXT#@salamraghegmail.onmicrosoft.com` |
| Group | `MFA-Test-Group` — Security, cloud, role assignment disabled, description `Users required to use MFA in the lab`, created Sep 2, 2026 8:29 PM, 1 member (Test User) |


## Screenshots (2 Sep 2026 dump)

- [Users list](screenshots/01-admin-center-users.png)
- [P2 trial on Products](screenshots/02-p2-trial-products.png)
- [Test User review / P2 assigned](screenshots/06-testuser-review.png)
- [MFA-Test-Group review](screenshots/16-group-review.png)
- [MFA-Test-Group created](screenshots/17-group-created.png)
- [Security groups list](screenshots/18-security-groups-list.png)
- [Add members picker](screenshots/22-add-members-picker.png)
- [testuser is the member](screenshots/23-testuser-member.png)

## What was built

1. Confirmed the P2 Managed Trial on **Products** (expires Sep 30, 2026).
2. Created **Test User** with Entra ID P2 and no admin role.
3. Created **Leaver User** (visible in the group member picker).
4. Created security group **MFA-Test-Group**. Role assignment left off.
5. Added **testuser** as the only member. **leaver** was not added.

## How to redo it

Portals: [admin.microsoft.com](https://admin.microsoft.com) as `labadmin@salamraghegmail.onmicrosoft.com`.

1. **Home → Your organization → Products.** Confirm **Microsoft Entra ID P2 Managed Trial** is Active and note the expiration date.
2. **Add user.** Basics: display name Test User, username `testuser`, domain `salamraghegmail.onmicrosoft.com`. Product licenses: assign **Microsoft Entra ID P2**, location United States. Optional settings: leave **Roles** as User (no administration access). Review and finish.
3. Repeat Add user for **Leaver User** / `Leaver`. Same P2 license. Do not make this account an admin.
4. **Groups → Active groups → Security groups → Add a security group.** Name `MFA-Test-Group`. Leave **Azure AD roles can be assigned to the group** unchecked. Create.
5. Open the group → **Members → Add members.** Select **Test User** only. Save. Do not add labadmin or leaver.

Docs used in the sitting: [Create users](https://learn.microsoft.com/en-us/entra/fundamentals/how-to-create-delete-users), [Manage groups](https://learn.microsoft.com/en-us/entra/fundamentals/how-to-manage-groups), [Assign licenses](https://learn.microsoft.com/en-us/microsoft-365/admin/manage/assign-licenses-to-users).

## What broke

- **Products tab showed 0 assigned / 0 available** after Test User was created with Entra ID P2 on the review screen. Treated that table as lag. Proof of assignment is the user wizard (license selected, review shows Microsoft Entra ID P2), not the Products counters.
- **Active groups** defaulted to the **Microsoft 365 groups** tab, which stayed empty. The group is a **Security group**; it only appears on that tab.
- **MFA-Test-Group** started at 0 members / 0 owners. Member add is a separate step after create. The dump ends with testuser saved as the one member.

## Hiring screen-share

Walk this in admin.microsoft.com, not a slide deck.

1. **Users:** Lab Admin, Test User, Leaver User, plus the Gmail guest. Point at testuser as a normal user with P2 and no admin role.
2. **Products:** P2 Managed Trial, expiration Sep 30, 2026. Say the assigned-count column lagged in this sitting; show the user object if asked.
3. **Groups → Security groups:** `MFA-Test-Group`, cloud, created Sep 2, 2026 8:29 PM, role assignment disabled.
4. **Members:** Test User only. Leaver exists and is not in the group.

Stop there. This dump does not show Conditional Access, an enterprise app, or disabling an account.

## Dated progress

| When | What the dump shows |
| --- | --- |
| 2026-09-02 | P2 Managed Trial Active, expires Sep 30, 2026 |
| 2026-09-02 | Test User created with Entra ID P2, not admin |
| 2026-09-02 | Leaver User present in the tenant, P2 assigned, not in the group |
| 2026-09-02 8:29 PM | MFA-Test-Group created (Security, role assignment off) |
| 2026-09-02 | testuser added as the sole group member |
