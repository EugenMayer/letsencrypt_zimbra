
## Installation

### Preconditions
1. put this script to `/usr/loca/bin/install_zimbra_le` and make it executable
```
wget https://raw.githubusercontent.com/EugenMayer/letsencrypt_zimbra/master/install_zimbra_le
mv install_zimbra_le /usr/local/bin/
chmod +x /usr/local/bin/install_zimbra_le
````

due to a bug in Zimbra, when dnscache is enabled, you cannot restart without providing a sudo password. Please add this to 

`vim /etc/sudoers.d/zimbra_sudo_fix`

```
%zimbra ALL=NOPASSWD:/opt/zimbra/libexec/zmdnscachealign
%zimbra ALL=NOPASSWD:/opt/zimbra/common/sbin/unbound
%zimbra ALL=NOPASSWD:/sbin/resolvconf
```

2. install acme (this will place the cronjob we need for the renewals): 

```
export LE_WORKING_DIR=/etc/ssl/letsencrypt
curl https://get.acme.sh | sh
```

3. Download the LetsEncrypt root certificate
a) either pick it from here https://gist.github.com/EugenMayer/8396b811f9649c4ee0840862262639b4
b) or pick it from here et it from https://www.identrust.com/certificates/trustid/root-download-x3.html 

and then put it to ```/etc/ssl/le_root.cer```

Also see https://letsencrypt.org/certificates/
TODO: how to script this? 

### Get your certificate

Replace <domain> ( 2 times! )

```
cd /etc/ssl/letsencrypt
export DOMAIN=yourdomain.tld
./acme.sh --issue --dns dns_cf --dnssleep 25 -d $DOMAIN --reloadcmd "/usr/local/bin/install_zimbra_le $DOMAIN"
```

In the upper case iam using DNS verification, you can though pick what you want, read https://github.com/Neilpang/acme.sh
I use https://www.cloudflare.com since they offer free accounts. If you want to do so, you need then, before running acme.sh do

```
export CF_Email=<youraccountmail>
export CF_Key=<yourglobalapikey>
acme.sh
```

After running acme once with the CF credentials, they will be saved and you do no longer need to use export

Thats it, this command is a one-shot. The cronjob takes care of the renewal and of the automatic install into zimbra

### cron

When you are finished, verify that your cron is configured properly. There might be 

```
crontab -e
4 0 * * * "/root/.acme.sh"/acme.sh --debug --cron --home "/root/.acme.sh"
```

which would be wrong. We need to fix the home parameter

```
crontab -e
4 0 * * * "/root/.acme.sh"/acme.sh --debug --cron --home "/etc/ssl/letsencrypt"
```
