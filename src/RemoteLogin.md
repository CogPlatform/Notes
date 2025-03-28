Remote desktop can be a security hole, so we must be careful, especially for lab machines!!! This should only take a few minutes.

We can use Zerotier to access machines using a secure and private IP address, and we then run SSH and nomachine only within the Zerotier network to allow terminal and GUI access, both using SSH keys only. This is much more secure than using the normal remote desktops.

# Steps

## Zerotier (needed for NoMachine and SSH)

1. Install ZeroTier: `curl -s https://install.zerotier.com | sudo bash`
2. Get details: `sudo zerotier-cli info`
3. If you know the network ID: `sudo zerotier-cli join #ID`
4. Register this to CogPlatform network <https://my.zerotier.com>

## No Machine

No machine is a fast cross-platform remote desktop that can be secured by zerotier:

5. Install NoMachine: https://downloads.nomachine.com/linux/?id=1

## SSHD (macOS and Linux)

6. FOR SSHD: Install openssh-server `sudo apt install openssh-server` -- you may need to use Ubuntu settings to enable, otherwise `sudo systemctl enable ssh`
7. FOR SSHD: Configure `/etc/ssh/sshd_config` to only listen to the Zerotier IP for this machine via `ListenAddress`, e.g. `ListenAddress 172.22.22.22`

# Setup NoMachine to Secure Login

See https://kb.nomachine.com/AR02L00785 for instructions. Basically copy a public key to `~/.nx/config/authorized.crt` file to register that key to NX. 

```shell
### on the client
> ssh-keygen -f ~/.ssh/cogp -t rsa -b 4096 -C "your personal comment or email"
> scp ~/.ssh/cogp.pub username@server_hostname_or_ip:~/.ssh/cogp.pub
### on the server
> cat ~/.ssh/cogp.pub >> ~/.nx/config/authorized.crt
> chmod 0600 ~/.nx/config/authorized.crt
```

For client, make sure the private key installed in `~/.ssh` as nxclient will use this file.

Now, for the **server** computer to use only zerotier IP and only ssh key login, edit the main config (`/usr/NX/etc/server.cfg` on Linux, not sure on Windows) and change:

```
NXdListenAddress "172.23.23.X" # set to the IP of zerotier only
AcceptedAuthenticationMethods NX-private-key # only accept SSH key login
```

# Setup SSHD to only accept SSH keys

To use SSH keys, add the public key to the `~/.ssh/authorized_keys` file to register your key on that system.

```shell
cat ~/.ssh/cogp.pub | ssh username@remote_host "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"
```

See https://www.digitalocean.com/community/tutorials/how-to-configure-ssh-key-based-authentication-on-a-linux-server

To make a SSH key on windows: https://www.purdue.edu/science/scienceit/ssh-keys-windows.html — basically you can install ssh-keygen and use that.

Once the key is working, you can now disable passwords by editing `/etc/ssh/sshd_config`:

```
ChallengeResponseAuthentication no
PasswordAuthentication no
PermitRootLogin no
PermitRootLogin prohibit-password
```

You can also ensure `sshd` starts after `zerotier` by adding this:

```
[Unit]
After=network-online.target network.target zerotier-one.service
```

using `sudo systemctl edit ssd.service`

