# Entra IAM Lab

Hands-on lab, Sep 2026.

This is the everyday Microsoft Entra ID work behind access tickets, done in a real tenant: create users, put them in a security group, force MFA on that group with Conditional Access, offboard a leaver, then hook up two apps with single sign-on (SAML and OIDC). The group is what everything hangs on: it decides who gets MFA and who can open the apps. Tenant names, UPNs and IDs are blacked out in the screenshots.

## What I built

1. **Users and licensing:** Test User and Leaver User on an Entra ID P2 trial. Test User is a normal user with no admin role.
2. **Group:** security group **MFA-Test-Group** with Test User as the only member.
3. **Conditional Access:** policy **Require MFA for MFA-Test-Group** (All resources, Grant: Require multifactor authentication), State **On**. The What If tool confirms it applies to Test User.
4. **Leaver offboard:** disabled Leaver User, revoked their sign-in sessions, and removed the P2 license in the Microsoft 365 admin center. Test User kept theirs.
5. **SAML SSO:** added the Microsoft Entra SAML Toolkit app, assigned MFA-Test-Group, saved the SAML config, and signed in as Test User from My Apps.
6. **OIDC:** registered **Entra OIDC Lab App** with redirect `https://jwt.ms` and ID tokens on, assigned MFA-Test-Group with Assignment required = Yes, granted admin consent for Microsoft Graph `User.Read`, and decoded Test User's ID token at jwt.ms.

## What I learned

*Interview line:*

> "In Entra I required MFA for a test group with Conditional Access and gave that group SSO to a SAML app and an OIDC app. For a leaver, I disabled the account, revoked sessions, and removed the license and groups."

## Key screenshots

![](key/01-testuser-in-mfa-test-group.png)
Test User added as a member of MFA-Test-Group in Entra ID.

![](key/02-error-security-defaults-block-ca.png)
Error I hit: Security Defaults blocked Conditional Access until I turned it off.

![](key/03-ca-policy-on.png)
Conditional Access policy requiring MFA for MFA-Test-Group is turned on.

![](key/04-what-if-mfa-applies.png)
What If tool confirms the MFA policy applies to Test User.

![](key/05-leaver-disabled.png)
Leaver account disabled in Entra ID, confirmed by the success toast.

![](key/06-leaver-sessions-revoked.png)
Revoked the leaver's sign-in sessions so existing logins stop working.

![](key/07-saml-sso-success.png)
SAML Toolkit app signed Test User in through Entra single sign-on.

![](key/08-oidc-token-decoded.png)
Decoded OIDC ID token for Test User (name and ver 2.0 shown; IDs and username blacked out).

## Environment

| Object | Value |
| --- | --- |
| Tenant | Microsoft Entra ID, Default Directory (name hidden) |
| Licensing | Microsoft Entra ID P2 Managed Trial |
| Group | **MFA-Test-Group** (Security, cloud, role assignment off) |
| CA policy | **Require MFA for MFA-Test-Group**, All resources, Require MFA, State On |
| SAML app | Microsoft Entra SAML Toolkit, connection 22318, MFA-Test-Group assigned |
| OIDC app | Entra OIDC Lab App, redirect `https://jwt.ms`, ID tokens on, `User.Read` consented |

## What broke / issues I fixed

- **Security defaults blocked the new policy.** The CA form said "You must first disable security defaults." I set Security defaults to Disabled, and then the policy could be created.
- **Policy list lagged after enabling.** The toast said the policy "will be enabled in a few minutes" while the list still showed Report-only. A later refresh shows State On.
- **License removal lives in the M365 admin center.** The Entra user Licenses tab can't unassign, so I removed P2 under Billing > Licenses there.
- **SAML connection ID comes after the Toolkit config.** The final Reply and Sign on URLs use Consume/Login **22318** once the Toolkit SP config existed.
- **OIDC needed admin consent.** Test User hit "Need admin approval" until I granted admin consent for `User.Read`.

## Gaps

- **No live MFA prompt screenshot.** Sign-in logs showed no interactive sign-ins for Test User. The proof that the policy hits Test User is What If, not an MFA challenge.
- The Save click after picking a reason for disabling security defaults wasn't captured. The policy being created afterward is the proof they were off.
- The raw jwt.ms token shot was left out. The encoded token can be decoded back to tenant and object IDs, and blacking it out leaves nothing to see.

<details><summary>Full proof (all screenshots)</summary>

![](full-proof/01-admin-center-users.png)

![](full-proof/02-p2-trial-products.png)

![](full-proof/06-testuser-review.png)

![](full-proof/16-group-review.png)

![](full-proof/17-group-created.png)

![](full-proof/18-security-groups-list.png)

![](full-proof/22-add-members-picker.png)

![](full-proof/25-security-defaults-disabled.png)

![](full-proof/26-policy-include-mfa-test-group.png)

![](full-proof/27-policy-all-resources.png)

![](full-proof/28-grant-require-mfa.png)

![](full-proof/29-policy-details-one-group.png)

![](full-proof/30-policy-enable-toast-list-lag.png)

![](full-proof/32-what-if-testuser-exo.png)

![](full-proof/36-p2-unassigned-testuser-licensed.png)

![](full-proof/37-saml-toolkit-app-added.png)

![](full-proof/38-saml-mfa-test-group-assigned.png)

![](full-proof/39-saml-basic-config-22318-saved.png)

![](full-proof/40-saml-toolkit-sp-config-22318.png)

![](full-proof/41-myapps-testuser-toolkit-tile.png)

![](full-proof/42-saml-login-22318.png)

![](full-proof/44-oidc-lab-app-created.png)

![](full-proof/45-oidc-redirect-jwt-ms.png)

![](full-proof/46-oidc-id-tokens-enabled.png)

![](full-proof/47-oidc-mfa-test-group-assigned.png)

![](full-proof/48-oidc-assignment-required-yes.png)

![](full-proof/49-oidc-admin-consent-user-read.png)

</details>
