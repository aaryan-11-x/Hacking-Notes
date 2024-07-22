# Linux
telnet [Target IP]  [Port Number (25)]
	- Enter The Sender Mail
	- Then, *Enumerate Users*
	- Value Around *200-300* Is *Successful*
	- Value Around *500* Is *Unsuccessful*
![[Pasted image 20221202130020.png]]

								OR
Use [[msfconsole]]
`use auxiliary/scanner/smtp/smtp_enum`
![[Pasted image 20221202134026.png]]

```sh
swaks -t recipient@example.com -t recipient2@example.com --from sender@example.com --attach @config.Library-ms --server SMTP_SERVER --body @body.txt --header "Subject: Need help" --suppress-data -ap
# OR
sendemail -f sender@example.com -t receiver@example.com -u "Subject text" -m "Message body text." -a FILE_ATTACHMENT -s SMTP_SERVER [-xu USERNAME -xp PASSWORD]
```
# Windows
```powershell
# Enable Telnet (If not already enabled)
dism /online /Enable-Feature /FeatureName:TelnetClient

telnet 192.168.50.8 25
```