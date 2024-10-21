# OCI-UBUNTU-SSH
### Enable 2 factor Authentication  Certificate + Password for SSH Access ###

##### Edit file /etc/ssh/sshd_config.d #####
```bash
sudo nano /etc/ssh/sshd_config.d/60-cloudimg-settings.conf
```
It looks like this orignally

```bash
PasswordAuthentication no
```
Change it to enable password authentication
```console

PasswordAuthentication yes

```
Now edit the sshd_config file and replace/add following line 
```bash
sudo nano /etc/ssh/sshd_config
```
```console
PubkeyAuthentication yes
AuthenticationMethods publickey,password
```
Make sure you have correct keys in authorized_keys file.

```bash
cat .ssh/authorized_keys 
```

