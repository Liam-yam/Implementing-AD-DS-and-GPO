# 8. Testing and Verification

This chapter documents verification of server networking, DNS, domain membership, authentication, and Group Policy.

## 8.1 Network Verification

Run `ipconfig /all` on the server and client to verify the documented addressing and DNS configuration.

> **Screenshot placeholder:** Server `ipconfig /all`.

> **Screenshot placeholder:** Client `ipconfig /all`.

## 8.2 Connectivity

Use `ping <server-ip>` to test basic network reachability. A successful ping confirms network-level communication, but it does not by itself prove that Active Directory is functioning.

## 8.3 DNS Verification

Run:

```cmd
nslookup Yasha.local
```

`nslookup` verifies DNS name resolution. It is different from `ping`, which tests network reachability.

> **Screenshot placeholder:** Successful DNS lookup.

## 8.4 Domain Verification

Confirm that the Windows 11 client is a member of `Yasha.local` and that its computer object appears in the intended `Computers` OU.

> **Screenshot placeholder:** Domain membership.

> **Screenshot placeholder:** Active Directory computer object.

## 8.5 Domain Authentication

Use one of the domain user accounts created for the laboratory to verify that the client can authenticate against the Domain Controller. Never document passwords or other credentials in the repository.

## 8.6 Group Policy Verification

After configuring the GPOs, run:

```cmd
gpupdate /force
```

This forces Windows to refresh and reapply Group Policy immediately.

Then run:

```cmd
gpresult /r
```

This displays the Group Policy settings and GPOs applied to the current user and computer.

> **Screenshot placeholder:** `gpupdate /force`.

> **Screenshot placeholder:** `gpresult /r` showing the expected policies.

## 8.7 Functional GPO Tests

### Desktop Policy

Expected result: the configured laboratory wallpaper is applied and the user cannot change it when the restriction is active.

### Password Policy

Expected result: the password-related settings configured for the laboratory are enforced according to the actual GPO configuration.

### Control Panel Policy

Expected result: the configured User Accounts and Network and Sharing Center areas are restricted as intended.

> **Screenshot placeholder:** Final desktop wallpaper.

> **Screenshot placeholder:** Control Panel restrictions.

## 8.8 Verification Checklist

| Test | Expected Result | Actual Result |
|---|---|---|
| Server static IP | Stable address | `[TO COMPLETE]` |
| Client reaches server | Connectivity succeeds | `[TO COMPLETE]` |
| `nslookup Yasha.local` | DNS resolution succeeds | `[TO COMPLETE]` |
| Domain join | Client joins `Yasha.local` | `[TO COMPLETE]` |
| Domain authentication | User can authenticate | `[TO COMPLETE]` |
| Desktop GPO | Wallpaper is standardized | `[TO COMPLETE]` |
| Password GPO | Configured settings apply | `[TO COMPLETE]` |
| Control Panel GPO | Selected areas are restricted | `[TO COMPLETE]` |
| `gpupdate /force` | Policies refresh | `[TO COMPLETE]` |
| `gpresult /r` | Expected GPOs appear | `[TO COMPLETE]` |

The Actual Result column should be completed from the real laboratory observations rather than assumptions.
