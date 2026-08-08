# 9. Troubleshooting and Common Issues

## 9.1 Troubleshooting Approach

Troubleshooting should follow the dependency order of the laboratory instead of changing several settings at once.

```text
Network Adapter
      │
      ▼
IP Configuration
      │
      ▼
DNS Resolution
      │
      ▼
Domain Controller
      │
      ▼
Domain Membership
      │
      ▼
Authentication
      │
      ▼
Group Policy
```

This approach helps isolate the source of a problem before making additional configuration changes.

## 9.2 Client Cannot Reach the Server

Check:

- Whether both virtual machines are connected to the intended VMware network.
- Whether the client has a valid IP configuration.
- Whether the server has the expected static IP.
- Whether basic connectivity works with `ping <server-ip>`.

> **Evidence placeholder:** Add the actual error and resolution if one occurred during the laboratory.

## 9.3 Client Cannot Resolve `Yasha.local`

If `nslookup Yasha.local` fails, check the client's preferred DNS server first. The client should use the Windows Server's IP address as its DNS server for the Active Directory environment.

Useful commands include:

```cmd
ipconfig /all
nslookup Yasha.local
```

Do not treat a failed DNS lookup as merely a ping problem. DNS resolution and network reachability are separate tests.

## 9.4 Client Cannot Join the Domain

Check the following in order:

- Confirm that the Domain Controller is running.
- Confirm that the client can communicate with the server.
- Confirm that the client uses the Domain Controller for DNS.
- Confirm that `nslookup Yasha.local` resolves successfully.
- Confirm that the domain name is entered exactly as `Yasha.local`.
- Confirm that appropriate domain credentials are available.

## 9.5 Group Policy Does Not Apply

If a policy does not appear to apply, verify:

- The GPO is configured and linked to the intended scope.
- The user or computer is located in the intended OU.
- The client is actually joined to `Yasha.local`.
- `gpupdate /force` completes successfully.
- `gpresult /r` lists the expected policy.
- There is no conflicting or higher-priority policy overriding the setting.

The first goal should be to determine whether the GPO was processed at all before troubleshooting the individual setting.

## 9.6 Desktop Wallpaper Policy Does Not Work

Check the policy's configured wallpaper path and the scope where the policy is linked. Then refresh the client policy and verify the applied GPO with `gpresult /r`.

If the policy appears in `gpresult` but the wallpaper does not behave as expected, inspect the exact policy setting and its scope rather than immediately creating another GPO.

## 9.7 Control Panel Restriction Does Not Work

Confirm that the restriction was configured in the correct User or Computer Configuration section and that the policy is applied to the correct scope. Run `gpupdate /force` and then `gpresult /r` before testing again.

## 9.8 Documenting Actual Problems

The final project should distinguish between **problems that actually occurred** and general troubleshooting advice. When recording a real issue, use this format:

### Problem
`[Describe the actual error.]`

### Investigation
`[Describe what was checked.]`

### Cause
`[State the confirmed cause, if known.]`

### Solution
`[Describe the change that fixed the issue.]`

### Verification
`[Explain how the fix was confirmed.]`

> **Important:** Do not invent troubleshooting incidents for the project. If no issue occurred, state that the corresponding configuration was verified successfully.
