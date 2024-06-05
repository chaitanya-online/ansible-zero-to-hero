# How to setup Passwordless Authentication

## EC2 Instances

### Using Public Key

```
ssh-copy-id -f "-o IdentityFile <PATH TO PEM FILE>" ubuntu@<INSTANCE-PUBLIC-IP>
```

- ssh-copy-id: This is the command used to copy your public key to a remote machine.
- -f: This flag forces the copying of keys, which can be useful if you have keys already set up and want to overwrite them.
- "-o IdentityFile <PATH TO PEM FILE>": This option specifies the identity file (private key) to use for the connection. The -o flag passes this option to the underlying ssh command.
- ubuntu@<INSTANCE-IP>: This is the username (ubuntu) and the IP address of the remote server you want to access.

### Using Password 

- Go to the file `/etc/ssh/sshd_config.d/60-cloudimg-settings.conf`
- Update `PasswordAuthentication yes`
- Restart SSH -> `sudo systemctl restart ssh`


### Using password in ansible inventory

1) Login to your managed node nd do below steps

- Go to the file `/etc/ssh/sshd_config.d/60-cloudimg-settings.conf`
- Update `PasswordAuthentication yes`
- Restart SSH -> `sudo systemctl restart ssh`

2) Create a password using below command 

Note :- Don't forgot the password

```
sudo passwd ubuntu
```

3) create inverntory file with  password in contole node

Syntax : 

<PUBLIC IP> ansible_user=<USER NAME> ansible_ssh_pass=<PASSWORD THAT YOU HAVE CREATED>


4) Testing the connection

admin2@Chaitanya-Nampalli:~$ ansible -i inventory.ini -m ping all

<PUBLIC IP> | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}

This approach is usefull when after creating password in managednode 
then try do below for passwordless in controlenode 

admin2@Chaitanya-Nampalli:~$ ssh-copy-id ubuntu@<PUBLIC IP>
/usr/bin/ssh-copy-id: ERROR: No identities found

