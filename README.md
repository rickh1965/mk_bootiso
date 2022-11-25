# Create Rocky Linux Host with kickstart on USB
This super simple playbook gives you a repeatable way to create a minimal Rocky Linux server non-interactively (mostly) by creating an ISO suitable for burning onto a USB drive.

## Non-free firmware included on PXE for Debian servers

I have quite a few older servers in my environment. So that I would not have to think about it after downloading Debian, I added the nonfree firmware in the boot file so you won't have to. I don't understand why Debian does this with their firmware but at least there is a fix.

## System Requirements
This playbook will create a bootable ISO file in the current working directory.

You will need to have at least twice the amount of disk space as is needed for the download of of the distribution ISO. In the default case, the Rocky Linux ISO is 2.2 GB. Do not run this playbook on a partition that has less than 4.4 GB available.

## Instructions
1. Edit the ```roles/bootiso/defaults/main.yml``` file for your environment. You will need the follwing to configure this file correctly.
    - Network Device name (i.e. eth0,enp1s0,en0 etc...)
    - Server IP
    - Gateway
    - Netmask
    - DNS Server
    - Server hostname
    - Privileged User Name
    - Privileged User Full Name (GECOS)
    - User and Root Passwords

    - To obtain the encrypted password for entry in the configuration files execute this command:

```bash
python -c 'import crypt,getpass;pw=getpass.getpass();print(crypt.crypt(pw) if (pw==getpass.getpass("Confirm: ")) else exit())'
```

```text
Password:
Confirm:
"Encrypted password is displayed"
This generates a sha512 crypt-compatible hash of your password using a random salt.
```

   Place the value that is displayed in the configuration file in the place of the string "Redacted"


2. Once the the defaults file has been populated with the appropriate values for your environment, create the bootable ISO with the following

```bash
ansible-playbook bootiso.yml
```

3. This playbook runs on the local machine and the resulting ISO file will be located in the present working directory. It's name will be the hostname you entered and have the iso extension. i.e.

```<<hostname>>.iso```

4. Once you have completed this step, use your favorite tool to create a bootable USB image from this file and boot this very plain minimal install of Rocky Linux.

## Next Steps
Once you are ready, you can proceed to make this your PXE boot server with the playbook:

https://github.com/rickh1965/pxebootserver

## Sources

This is loosely based on the Redhat solution for creating a USB boot drive:

https://access.redhat.com/solutions/60959
