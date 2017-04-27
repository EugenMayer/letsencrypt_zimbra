
## Installation

### Preconditions
1. put this script to `/usr/loca/bin/install_zimbra_le` and make it executable
```
wget https://raw.githubusercontent.com/EugenMayer/letsencrypt_zimbra/master/install_zimbra_le
mv install_zimbra_le /usr/local/bin/
chmod +x /usr/local/bin/install_zimbra_le
````

2. install acme (this will place the cronjob we need for the renewals): 

```curl https://get.acme.sh | sh```

3. Download the LetsEncrypt root certificate
a) either pick it from here https://gist.github.com/EugenMayer/8396b811f9649c4ee0840862262639b4
b) or pick it from here et it from https://www.identrust.com/certificates/trustid/root-download-x3.html 

and then put it to ```/etc/ssl/le_root.cer```

Also see https://letsencrypt.org/certificates/
TODO: how to script this? 

### Get your certificate

Replace <domain> ( 2 times! )

```
./acme.sh --issue --dns dns_cf --dnssleep 25 -d <domain> --reloadcmd "/usr/local/bin/install_zimbra_le <domain>"
```

In the upper case iam using DNS verification, you can though pick what you want, read https://github.com/Neilpang/acme.sh

Thats it, this command is a one-shot. The cronjob takes care of the renewal and of the automatic install into zimbra
