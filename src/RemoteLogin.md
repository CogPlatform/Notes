# Steps

1. Install ZeroTier: `curl -s https://install.zerotier.com | sudo bash`
2. Register machine to Cogplatform network
3. Install openssh-server `sudo apt install openssh-server` -- you may need to use Ubuntu settings to enable, otherwise `sudo systemctl enable ssh`
4. Configure `/etc/ssh/sshd_config` to only listen to Zerotier IPs
5. Install NoMachine: https://downloads.nomachine.com/linux/?id=1


# Setup SSHD to only accept SSH keys

See https://www.digitalocean.com/community/tutorials/how-to-configure-ssh-key-based-authentication-on-a-linux-server

To let sshd only use key, edit `/etc/ssh/sshd_config`:

```
ChallengeResponseAuthentication no
PasswordAuthentication no
PermitRootLogin no
PermitRootLogin prohibit-password
```

Note you can also edit /usr/NX/share/server.cfg to set `AcceptedAuthenticationMethods NX-private-key` so it also only uses SSH key login
