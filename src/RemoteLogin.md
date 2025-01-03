Remote desktop can be a security hole, so we must be careful, especially for lab machines!!!

We can use Zerotier to access machines using a secure and private IP address, and we run SSH and nomachine to allow terminal and GUI access, both using SSH keys only. This is much more secure than using the normal remote desktops.

# Steps

## Zerotier (needed for NoMachine and SSH)

1. Install ZeroTier: `curl -s https://install.zerotier.com | sudo bash`
2. Get details: `sudo zerotier-cli info`
3. Register machine to CogPlatform network https: <https://my.zerotier.com>

## No Machine

No machine is a fast cross-platform remote desktop that can be secured by zerotier:

4. Install NoMachine: https://downloads.nomachine.com/linux/?id=1

## SSHD (macOS and Linux)

5. FOR SSHD: Install openssh-server `sudo apt install openssh-server` -- you may need to use Ubuntu settings to enable, otherwise `sudo systemctl enable ssh`
6. FOR SSHD: Configure `/etc/ssh/sshd_config` to only listen to Zerotier IPs via `ListenAddress`

# Setup NoMachine to Limit login

Edit `/usr/NX/etc/server.cfg` and change: 

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

See https://kb.nomachine.com/AR02L00785 for instructions.
