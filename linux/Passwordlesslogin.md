# Passwordless login

<https://www.linuxbabe.com/linux-server/setup-passwordless-ssh-login#>:\~:text=To%20disable%20password%20authentication%2C%20edit,file%20on%20the%20remote%20server.&text=Then%20find%20the%20ChallengeResponseAuthentication%20line,still%20use%20password%20to%20login.

# Allow root login using ssh keys only.

```{bash}

# login as gladiator Become root

sudo su -

# now go to ~/.ssh This is /root/.ssh directory for root

cd ~/.ssh

# check for authorized keys file

ls -al

# if does not exit then copy public key as authorized_key

# First get id_rsa.pub to that directory then
cat ~/id_rsa.pub >> ~/.ssh/authorized_keys

# or copy directly from github

ssh-import-id-gh <github username>

cat ~/.ssh/authorized_keys # check entry

rm ~/id_rsa.pub # remove key

# now you should be able to login as root

#if this does not work check the sshd_config file 

cd /etc/ssh/
cat sshd_config

# the comments are the default settings. 
# Once ssh login is possible you may want to change PasswordAuthentication to no

PasswordAuthentication no

```

# Notes

file /etc/ssh/sshd_config is for ssh server demon

file /etc/ssh/ssh_config is for ssh client

# key files

if you creat id_rsa and id_rsa.pub file they are not acceptable by WinSCP you may need to convert that to .ppk file before using it.

Windows Putty

create shortcut using

"C:\\Program Files\\PuTTY\\putty.exe" -load PuttySessionName

From:

<https://www.cyberciti.biz/faq/create-ssh-config-file-on-linux-unix/>

# [OpenSSH Config File Examples For Linux / Unix Users](https://www.cyberciti.biz/faq/create-ssh-config-file-on-linux-unix/)

Author: Vivek Gite Last updated: November 25, 2021 [16 comments](https://www.cyberciti.biz/faq/create-ssh-config-file-on-linux-unix/#comments)

[![](data:image/svg+xml;base64,PHN2ZyBoZWlnaHQ9IjUyIiB3aWR0aD0iODAiIHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8yMDAwL3N2ZyIgdmVyc2lvbj0iMS4xIi8+){alt=""}](https://www.cyberciti.biz/faq/category/openbsd/)

How do I create and setup an OpenSSH config file to create shortcuts for servers I frequently access under Linux or Unix desktop operating systems?\
\
We can set up a global or local configuration file for SSH clients can create shortcuts for sshd servers, including advanced ssh client options.\

| Tutorial details  |
|:-----------------:|
| Difficulty level  |
|  Root privileges  |
|   Requirements    |
| Est. reading time |

You can configure your OpenSSH ssh client using various files as follows to save time and typing frequently used ssh client command-line options such as port, user, hostname, identity-file, and much more to increase your productivity from Linux/macOS or Unix desktop:\
\
You can configure your OpenSSH ssh client to save typing time for frequently used ssh client command-line options such as port number, user name, hostname/IP address, identity file, and much more. In addition to that it will increase your productivity from Linux/macOS or Unix desktop.ADVERTISEMENT\

## System-wide OpenSSH config file client configuration

1.  /etc/ssh/ssh_config : This files set the default configuration for all users of OpenSSH clients on that desktop/laptop and it must be readable by all users on the system.

## User-specific OpenSSH file client configuration

1.  \~/.ssh/config or \$HOME/.ssh/config : This is user's own configuration file which, overrides the settings in the global client configuration file, /etc/ssh/ssh_config.

## \~/.ssh/config file rules

The rules are as follows to create an ssh config file:

-   You need to edit \~/.ssh/config with a text editor such as vi.

-   One config parameter per line is allowed in the configuration file with the parameter name followed by its value or values. The syntax is:

        config value
        config1 value1 value2

-   You can use an equal sign (=) instead of whitespace between the parameter name and the values.

        config=value
        config1=value1 value2

-   All empty lines and lines starting with the hash (\#) are ignored are ignored.

-   Please note that all values are case-sensitive, but parameter names are not.

Tip : If this is a brand new Linux, macOS/Unix box, or if you have never used ssh before create the \~/.ssh/ directory first using the following syntax:\
mkdir -p \$HOME/.ssh\
chmod 0700 \$HOME/.ssh

## Examples

For demonstration purpose my sample setup is as follows:

1.  Local desktop client -- Apple macOS/OS X/Ubuntu Linux.

2.  Remote Unix server -- OpenBSD server running latest OpenSSH server.

3.  OpenSSH remote server ip/host: 75.126.153.206 (server1.cyberciti.biz)

4.  Remote OpenSSH server user: nixcraft

5.  OpenSSH dest port: 4242

6.  Local ssh private key file path : /nfs/shared/users/nixcraft/keys/server1/id_rsa

Based upon the above information my ssh command is as follows:\
`$ ssh -i /nfs/shared/users/nixcraft/keys/server1/id_rsa -p 4242 nixcraft@server1.cyberciti.biz`\
OR\
`$ ssh -i /nfs/shared/users/nixcraft/keys/server1/id_rsa -p 4242 -l nixcraft server1.cyberciti.biz`\
See how much I need to type. I need to remember the remote hostname/IP, port number, the path to ssh key, username, etc. Too much typing and is not increasing my productivity. But fear not, there is an easy way out.

## Using the ssh config file

You can avoid typing all of the ssh command parameters while logging into a remote machine and/or for executing commands on a remote machine. All you have to do is create an ssh config file. Open the Terminal application and create your config file by typing the following command:

\
**Patreon supporters only guides 🤓**

-   No ads and tracking

-   In-depth guides for developers and sysadmins at [Opensourceflare](https://www.opensourceflare.com/)✨

-   Join my Patreon to support independent content creators and start reading latest guides:

    -   [How to set up Redis sentinel cluster on Ubuntu or Debian Linux](https://www.opensourceflare.com/how-to-set-up-redis-sentinel-cluster-on-ubuntu-or-debian-linux/)

    -   [How To Set Up SSH Keys With YubiKey as two-factor authentication (U2F/FIDO2)](https://www.opensourceflare.com/how-to-set-up-ssh-keys-with-yubikey-as-two-factor-authentication-u2f-fido2/)

    -   [How to set up Mariadb Galera cluster on Ubuntu or Debian Linux](https://www.opensourceflare.com/how-to-set-up-mariadb-galera-cluster-on-ubuntu-or-debian-linux/)

    -   [A podman tutorial for beginners -- part I (run Linux containers without Docker and in daemonless mode)](https://www.opensourceflare.com/a-podman-tutorial-for-beginners-part-i/)

    -   [How to protect Linux against rogue USB devices using USBGuard](https://www.opensourceflare.com/how-to-protect-linux-against-rogue-usb-devices-using-usbguard/)

    -   [If your domain is not sending email, set these DNS settings to avoid spoofing and phishing](https://www.opensourceflare.com/if-your-domain-is-not-sending-email-set-these-dns-settings-to-avoid-spoofing-and-phishing/)

[Join **Patreon** ➔](https://www.patreon.com/nixcraft)

    ## edit file in $HOME dir
     
    vi ~/.ssh/config

OR

    ## edit file in $HOME dir
     
    vi $HOME/.ssh/config

Add/Append the following config option for a shortcut to server1 as per our sample setup:

    Host server1
         HostName server1.cyberciti.biz
         User nixcraft
         Port 4242
         IdentityFile /nfs/shared/users/nixcraft/keys/server1/id_rsa

[Save and close the file in vi/vim](https://www.cyberciti.biz/faq/linux-unix-vim-save-and-quit-command/) by pressing Esc key, type :w and hit Enter key. To open your new SSH session to server1.cyberciti.biz by typing the following command:\
`$ ssh server1`

### Adding another host

Append the following to your \~/.ssh/config file:

    Host nas01
         HostName 192.168.1.100
         User root
         IdentityFile ~/.ssh/nas01.key

You can simply type:\
`$ ssh nas01`

## Understanding Host Patterns

A pattern for Host directive is nothing but IP address, DNS hostname, or combination of special wildcard characters. For example, ? wildcard that matches exactly one character. On the other hand, \* wildcard matches zero or more characters. It allows us to define the usage pattern. For instance, to specify and allow login from laptop.sweet.home, desktop.sweet.home, rpi.sweet.home, and corerouter.sweet.home, I could use the following pattern:

    Host *.sweet.home
         Hostname 192.168.2.17
         User vivek
         IdentityFile ~/.ssh/id_ed25519.pub

The following pattern would match any host in the 192.168.2.[0-9] network range:

    Host 192.168.2.?
         Hostname 192.168.2.18
         User admin
         IdentityFile ~/.ssh/id_ed25519.pub

We can also set a pattern list. It is a comma-separated list of patterns. Patterns within pattern lists may be negated by preceding them with an exclamation mark (!) in your authorized_keys. Here is an example from \~/.ssh/authorized_keys file on the remote server. First, login to the remote box:\
`$ ssh vivek@192.168.2.17`\
Now edit the file, run:\
`$ vim ~/.ssh/authorized_keys`\
Update it as follows:

    # Allow login from 192.168.2.0/24 subnet but not from 192.168.2.25
    from="!192.168.2.25,192.168.2.*" ssh-ed25519 my_random_pub_key_here vivek@nixcraft
    # Allow login from *.sweet.home but not from router.sweet.home
    from="!router.sweet.home,*.sweet.home" ssh-ed25519 my_random_pub_key_here vivek@nixcraft

[Save and close the file in vim.](https://www.cyberciti.biz/faq/linux-unix-vim-save-and-quit-command/)

## Putting it all together

Here is my sample \~/.ssh/config file that explains and create, design, and evaluate different needs for remote access using ssh client:

    ### default for all ##
    Host *
         ForwardAgent no
         ForwardX11 no
         ForwardX11Trusted yes
         User nixcraft
         Port 22
         Protocol 2
         ServerAliveInterval 60
         ServerAliveCountMax 30
     
    ## override as per host ##
    Host server1
         HostName server1.cyberciti.biz
         User nixcraft
         Port 4242
         IdentityFile /nfs/shared/users/nixcraft/keys/server1/id_rsa
     
    ## Home nas server ##
    Host nas01
         HostName 192.168.1.100
         User root
         IdentityFile ~/.ssh/nas01.key
     
    ## Login AWS Cloud ##
    Host aws.apache
         HostName 1.2.3.4
         User wwwdata
         IdentityFile ~/.ssh/aws.apache.key
     
    ## Login to internal lan server at 192.168.0.251 via our public uk office ssh based gateway using ##
    ## $ ssh uk.gw.lan ##
    Host uk.gw.lan uk.lan
         HostName 192.168.0.251
         User nixcraft
         ProxyCommand  ssh nixcraft@gateway.uk.cyberciti.biz nc %h %p 2> /dev/null
     
    ## Our Us Proxy Server ##
    ## Forward all local port 3128 traffic to port 3128 on the remote vps1.cyberciti.biz server ## 
    ## $ ssh -f -N  proxyus ##
    Host proxyus
        HostName vps1.cyberciti.biz
        User breakfree
        IdentityFile ~/.ssh/vps1.cyberciti.biz.key
        LocalForward 3128 127.0.0.1:3128

## Understanding \~/.ssh/config entries

-   **Host** : Defines for which host or hosts the configuration section applies. The section ends with a new Host section or the end of the file. A single \* as a pattern can be used to provide global defaults for all hosts.

-   **HostName** : Specifies the real host name to log into. Numeric IP addresses are also permitted.

-   **User** : Defines the username for the SSH connection.

-   **IdentityFile** : Specifies a file from which the user's DSA, ECDSA or DSA authentication identity is read. The default is \~/.ssh/identity for protocol version 1, and \~/.ssh/id_dsa, \~/.ssh/id_ecdsa and \~/.ssh/id_rsa for protocol version 2.

-   **ProxyCommand** : Specifies the command to use to connect to the server. The command string extends to the end of the line, and is executed with the user's shell. In the command string, any occurrence of %h will be substituted by the host name to connect, %p by the port, and %r by the remote user name. The command can be basically anything, and should read from its standard input and write to its standard output. This directive is useful in conjunction with nc(1) and its proxy support. For example, the following directive would connect via an HTTP proxy at 192.1.0.253:\
    ProxyCommand /usr/bin/nc -X connect -x 192.1.0.253:3128 %h %p

-   **LocalForward** : Specifies that a TCP port on the local machine be forwarded over the secure channel to the specified host and port from the remote machine. The first argument must be [bind_address:]port and the second argument must be host:hostport.

-   **Port** : Specifies the port number to connect on the remote host.

-   **Protocol** : Specifies the protocol versions ssh(1) should support in order of preference. The possible values are 1 and 2.

-   **ServerAliveInterval** : Sets a timeout interval in seconds after which if no data has been received from the server, ssh(1) will send a message through the encrypted channel to request a response from the server. See blogpost "[Open SSH Server connection drops out after few or N minutes of inactivity](https://www.cyberciti.biz/tips/open-ssh-server-connection-drops-out-after-few-or-n-minutes-of-inactivity.html)" for more information.

-   **ServerAliveCountMax** : Sets the number of server alive messages which may be sent without ssh(1) receiving any messages back from the server. If this threshold is reached while server alive messages are being sent, ssh will disconnect from the server, terminating the session.

## Speed up ssh session

Multiplexing is nothing but send more than one ssh connection over a single connection. OpenSSH can reuse an existing TCP connection for multiple concurrent SSH sessions. This results into reduction of the overhead of creating new TCP connections. Update your \~/.ssh/config:

    Host server1
            HostName server1.cyberciti.biz
            ControlPath ~/.ssh/controlmasters/%r@%h:%p
            ControlMaster auto

See "[Linux / Unix: OpenSSH Multiplexer To Speed Up OpenSSH Connections](https://www.cyberciti.biz/faq/linux-unix-osx-bsd-ssh-multiplexing-to-speed-up-ssh-connections/)" for more info. In this example, I go [through one host to reach another server i.e. jump host using ProxyCommand](https://www.cyberciti.biz/faq/linux-unix-ssh-proxycommand-passing-through-one-host-gateway-server/):

    ## ~/.ssh/config ##
    Host internal
      HostName 192.168.1.100
      User vivek
      ProxyCommand ssh vivek@vpn.nixcraft.net.in -W %h:%p
      ControlPath ~/.ssh/controlmasters/%r@%h:%p
      ControlMaster auto

For more info see the following tutorials:

-   [How To Reuse SSH Connection To Speed Up Remote Login Process Using Multiplexing](https://www.cyberciti.biz/faq/linux-unix-reuse-openssh-connection/)

-   [How To Setup SSH Keys on a Linux / Unix System](https://www.cyberciti.biz/faq/how-to-set-up-ssh-keys-on-linux-unix/)

## Overriding ssh config file option

The ssh command reads its configuration in the following order:

1.  ssh command line-option

2.  \~/.ssh/config option

3.  /etc/ssh/ssh_config options

Say you have the following options set in \~/.ssh/config:

    Host ln.openvpn-sg-vpn1 ln.wireguard-sg-vpn1
         Hostname 172.16.0.1
         User vivek
         port 22
         IdentityFile ~/.ssh/id_ed25519.pub
         StrictHostKeyChecking no

Now want to use all other options from \~/.ssh/config but to connect using admin user instead of vivek, then:\
`$ ssh -o "User=admin" ln.openvpn-sg-vpn1`\
We can specifies an alternative per-user configuration file such as /dev/null to disable \~/.ssh/config too by passing the -F:\
`$ ssh -F /dev/null admin@172.16.0.1$ ssh -F /dev/null vivek@172.16.0.1$ ssh -F /dev/null -i ~/.ssh/aws/id_ed25519.pub vivek@172.16.0.1`

## A note about shell aliases (outdated method)

**WARNING!** This bash shell aliased based setup may work out for you. However, I recommend that you use \~/.ssh/config file for better results in a long run. SSH config file is more advanced and elegant solutions. The alias command only used here for demo purpose and it is here due to historical reasons.

[An alias is nothing but shortcut to commands](https://www.cyberciti.biz/tips/bash-aliases-mac-centos-linux-unix.html) and you can [create the alias](https://bash.cyberciti.biz/guide/Create_and_use_aliases) use the following syntax in your [\~/.bashrc file](https://bash.cyberciti.biz/guide/~/.bashrc):

    ## create a new bash shell alias as follow ##
    alias server1="ssh -i /nfs/shared/users/nixcraft/keys/server1/id_rsa -p 4242 nixcraft@server1.cyberciti.biz"

Then, to ssh into the server1, instead of typing full ssh -i /nfs/shared/users/nixcraft/keys/server1/id_rsa -p 4242 nixcraft\@server1.cyberciti.biz command, you would only have to type the command 'server1' and press the [ENTER] key:\
`$ server1`

## Conclusion

This page explained the ssh client configuration file syntax and examples to increase your productivity at Linux, macOS, or Unix shell. See the following [resources](https://www.openssh.com/manual.html) or read it using the [man command](https://bash.cyberciti.biz/guide/Man_command "See Linux/Unix man command examples for more info"):\
`man 5 ssh_config`\
Also see:

-   [Top OpenSSH Server Best Security Practices](https://www.cyberciti.biz/tips/linux-unix-bsd-openssh-server-best-practices.html)

-   
