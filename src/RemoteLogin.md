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

# Setup NoMachine to Secure Login

For the **server** computer, edit the main config (`/usr/NX/etc/server.cfg` on Linux, not sure on Windows) and change:

```
NXdListenAddress "172.23.23.X" # set to the IP of zerotier only
AcceptedAuthenticationMethods NX-private-key # only accept SSH key login
```

See https://kb.nomachine.com/AR02L00785 for instructions. Basically copy a public key to `~/.nx/config/authorized.crt` file to register that key to NX. 

```shell
> sh-keygen -t rsa -b 4096 -C "your personal comment or email"
> scp ~/.ssh/id_rsa.pub username@server_hostname_or_ip:~/.ssh/id_rsa_nxclient.pub
> cat ~/.ssh/id_rsa_nxclient.pub >> ~/.nx/config/authorized.crt
> chmod 0600 ~/.nx/config/authorized.crt
```

For client, make sure you have your private key installed in `~/.ssh`

# Setup SSHD to only accept SSH keys

To use SSH keys, copy the public key to the `~/.ssh/authorized-keys` file to register your key on that system.

See https://www.digitalocean.com/community/tutorials/how-to-configure-ssh-key-based-authentication-on-a-linux-server

To make a SSH key on windows: https://www.purdue.edu/science/scienceit/ssh-keys-windows.html — basically you can install ssh-keygen and use that.

To let sshd only use key, edit `/etc/ssh/sshd_config`:

```
ChallengeResponseAuthentication no
PasswordAuthentication no
PermitRootLogin no
PermitRootLogin prohibit-password
```

