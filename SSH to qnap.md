## QNAP NAS login with SSH

Workaround is to use same public key as your other servers.

For example from ubuntu server copy authorized_keys file to home directory

```{bash}
$ cp ~/.ssh/authorized_keys /home/username/public.key
```

Download with public.key file then import in NAS or another linux server. now copy it as
authorized_key file in the new server.

```{bash}
$ cp /path/to/public.key ~/.ssh/authorized_keys
```

view file to make sure it copied correctly. cat \~/.ssh/authorized_keys

Now you can use same private key for authentication to different server.

===============================================

SSH: How To Set Up Authorized Keys Below is a quick how-to for implementing public / private key
authentication for SSH. This is by no means an exhaustive examination of the subject. \*nix
distributions vary slightly and further research may be needed.

Contents 1 Why use Public Key Authentication? 2 How does it work? 3 Setup Procedures 3.1 Step by
Step Example Why use Public Key Authentication? Public key authentication is considered a more
secure methods of authenticating the Secure Shell than the simple password challenge routine, a
method often broken by brute-force attacks. In addition, public key authentication allows for
automated login routines between machines, thus enabling a range of scripted jobs (think rsync or
port tunneling). It can also simplify the login process without compromising password security.

How does it work? Public key authentication uses a pair of computer generated keys - one public and
one private -- to authenticate between a host and a client. The public key is derived from the
private key. When authenticating, the host machine compares the public key to the private key in
order to verify the veracity of the public key. If the two match, access is granted. Security of the
system is predicated on the security of the private key.

Setup Procedures Generate the needed Public and Private keys on the host. Transfer / append the
public key to the authorized_keys file on the client. Login via Public Key Authentication. Step by
Step Example The below setup description assumes that you are able to run terminal or a terminal
application like Putty, and that you are familiar with basic commands. It doesn't take much.

Let's setup SSH public key authentication between your home computer (hereafter referred to as the
"Host") and your QNAP device (hereafter referred to as the "Client").

1.  Login to the Host via SSH using your preferred terminal application and generate the public /
    private key pair. In terminal type the following at the command prompt:

```{bash}
$ ssh-keygen -t rsa -C "server comment field"

```

Note: the -C switch is not required. It allows you to insert a comment that will appear in the
authorized_keys file. It is helpful for identifying and managing keys within the authorized_keys
file on the Client in the event that you have multiple key logins. Replace "server comment field"
with a machine name, IP address, date, or task name so that you can easily identify where and why a
given key was created.

2.  Execute the command and you should see the following output:

Generating public/private rsa key pair.

Enter file in which to save the key (/Users/UserName/.ssh/id_rsa):

Note: "UserName" is the user account that you have logged into via SSH. Also note that the actual
suggested path may vary slightly depending your system. You should accept the suggested location
unless you have reason to do otherwise.

You will then be prompted for a passphrase that will be associated with this key.

Enter passphrase (empty for no passphrase):

Enter same passphrase again:

The passphrase can be thought of as a password for the private key - it serves as an extra layer of
protection as described below. If you leave this field blank you will generate keys that do not
prompt for a passphrase. In other words, you will be logged in automatically via the secure public /
private key handshake that you are in the process of setting up. It is highly recommended that you
enter a passphrase unless you are setting up automated routines that require automatic login. See
more detail below in "Security Notes."

The keys have now been generated and are stored in the .ssh folder associated with the user account
on the Host machine.

3.  The final steps are to copy the public key to the Client and append it to the authorization_keys
    file. There are a number of ways to do this -- you can copy the file to the Client and then
    append it (I like this method being the relative noob that I am. It involves more steps but is
    the easiest to complete without error. Those proficient with terminal commands will do it all in
    one step from the Host). Alternatively, you can open the id_rsa.pub file via a text editor like
    iv and copy the key text - then open the authorized_keys file on the Client and paste the text
    directly into the file. I will walk you through the noob method.

In the terminal navigate to the folder that contains the newly created keys -- cd
/User/UserName/.ssh and use the "ls" list comand to see what is in that directory. You should see
the following files:

id_rsa id_rsa.pub known_hosts. Type the following at the command prompt:

```{bash}
$ scp id_rsa.pub admin@ClientIPAddress /etc/config/ssh/
```

[admin\@ClientIPAddress](mailto:admin@ClientIPAddress){.email} is the address of your QNAP NAS (the
Client) followed by the path on the Client where the public key needs to reside. You will be ask you
for the password of the "admin" account to login to the Client.

password:

Enter the password to complete secure copy.

4.  On the Client (QNAP NAS) navigate to the /etc/config/ssh folder and "ls" to reveal the contents
    of the directory. You should see your id_rsa.pub file.

5.  Now let's append this file to the authorized_keys file which needs to reside in this directory.
    Do not worry if authorized_keys file is not present. We will create it.

Type the following at the command prompt:

```{bash}
$ cat id_rsa.pub >> authorized_keys
```

Note: You have to cut the key in the file to one line, and add ssh-rsa in at the very beginning.

Example

ssh-rsa
AAAAB3NzaC1kc3MAAACBAPY8ZOHY2yFSJA6XYC9HRwNHxaehvx5wOJ0rzZdzoSOXxbETW6ToHv8D1UJ/z+zHo9Fiko5XybZnDIaBDHtblQ+Yp7Stx...4loWgV=
Set correct permissions

SSH daemon is really peacky about permissions. Create a .ssh folder within your home folder, copy
/etc/config/ssh/authorized_keys to this folder and then make sure you have set your permissions as
follows:

```{bash}
$ chmod 0711 ~ 
$ chmod 0700 ~/.ssh 
$ chmod 0600 ~/.ssh/authorized_keys 
```

That's it. You should now be able to login using key authentication. Logout of the Client and
attempt to login. If you created a passphrase for your id_rsa private key then you will be prompted
for the passphrase. If you left the passphrase field blank when generating the keys then you will be
logged in automatically. The first time you login you may encounter a promoted message like below.

The authenticity of host 'xxx.xxx.xxx.xxx' can't be established. RSA key fingerprint is
XX:XX:XX:XX:XX:XX:XX:XX:XX:XX:XX:XX:XX:XX:XX:XX. Are you sure you want to continue connecting
(yes/no) ? Type "yes" to continue. After you accept the authenticity of the RSA key, the Client
information is saved in the /Users/UserName/.ssh/known_hosts file. You will not be prompted again
unless you remove this file.

Important If the destination Client is a x86 series model, please execute the following command on
the destination Client (TS-509) to make sure the folder permission has been set correctly. Because
there might be a permission problem in earlier firmware versions in x86 model (e.g. TS-509)

```{bash}
$ chown admin.administrators /mnt/HDA_ROOT/.config
```

Note The procedure is the same when setting up key authentication between two NAS boxes. Simply
follow the steps above substituting a NAS device for the Host and Client as per the above example.

Trouble Shooting O'Reilly provides an excellent reasource for troubleshooting.

Security Notes You must keep your private key secure! Security of public key authentication is
dependent on your ability to secure the private key. You should avoid generating a key without a
passphrase. If you have an unencrypted private key (no passphrase) stored on your workstation and if
your workstation is compromised, your Client machines have been compromised too! If a hacker can
obtain the private key they will have access to any client machines accessible with the public key.
Further, in the event that your machine is compromised you should consider strongly the possibility
that your keystrokes are being logged and therefore any private keys that ar protected by
passphrases have likely been compromised.

In addition, you should never allow root-to-root trust between systems. Unfortunately, the firmware
version of the QNAP NAS series is hardwired with a variant of OpenSSH that only permits ssh login by
the root (admin) user. You are strongly encourage to update the SSH Daemon With OpenSSH as per How
To Replace_SSH Daemon With OpenSSH. You can then setup named accounts for users or roles, granting
as little root access as possible via sudo. You should also limit the "from" access of the public
keys, and if possible only allow specific public keys to run specific commands.

Original Example Here is an example which tells you how to set up authorized_keys between two QNAP
NAS units. For example, we have one TS-209 (10.8.12.209) & a TS-509 (10.8.12.33), and now I want to
make it possible to SSH login to TS-509 from TS-209 without password.

1.  SSH login to TS-209 with a console application (e.g. Putty)

2.  Execute the following command, secure copy the id_rsa.pub from TS-209 to TS-509, and save it as
    "authorized_keys".

```{bash}
$ scp ~/.ssh/id_rsa.pub .ssh/authorized_keys
```

3.  You might see a promoted message like below if this is the first time you are trying to access
    one NAS from another. Simply type "yes" and go to the next step.

The authenticity of host '10.8.12.33'(10.8.12.33) can't be established. RSA key fingerprint is
XX:XX:XX:XX:XX:XX:XX:XX:XX:XX:XX:XX:XX:XX:XX:XX. Are you sure you want to continue connecting
(yes/no) ? 4. And it will ask you for the password of the "admin" account to login the destination
host (10.8.12.33), just enter it and finish the secure copy command.

\$ password:

5.  That's it! Now you should be able to SSH login to the TS-509 from TS-209 without password, the
    authorized_keys has been saved in the destination host (TS-509) under
    /etc/config/ssh/authorized_keys

Note If the destination host is a x86 series model, please execute the following command in the
destination host (TS-509) to make sure the folder permission has been set correctly. Because there
might be a permission problem in earlier firmware versions in x86 model (e.g. TS-509)

```{bash}
$ chown admin.administrators /mnt/HDA_ROOT/.config
```
