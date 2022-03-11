# Passwordless login

https://www.linuxbabe.com/linux-server/setup-passwordless-ssh-login#:~:text=To%20disable%20password%20authentication%2C%20edit,file%20on%20the%20remote%20server.&text=Then%20find%20the%20ChallengeResponseAuthentication%20line,still%20use%20password%20to%20login.


login as gladiator 

Become root

sudo su -

now go to ~/.ssh 
This is /root/.ssh direcotry for root

check authorized keys

ls -al

if does not exit then copy public key as authorized_key

ssh-import-id-gh <github username>

now you should be able to login as root
