
## Installation

### Preconditions
1. put this script to `/usr/loca/bin/install_zimbra_le` and make it executable

2. install acmei (this will place the cronjob we need for the renewals): 

```curl https://get.acme.sh | sh```

3. Download the LetsEncrypt root certificate
Get it from https://www.identrust.com/certificates/trustid/root-download-x3.html and put it to /etc/ssl/le_root.cer
Also see https://letsencrypt.org/certificates/

### Get your certificate

Replace <domain> ( 2 times! )
 ./acme.sh --issue --dns dns_cf --dnssleep 25 -d <domain> --reloadcmd "/usr/loca/bin/install_zimbra_le <domain>"


