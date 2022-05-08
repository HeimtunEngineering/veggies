# Ansible

## Setting up hosts

Find the hosts file in `/etc/ansible/hosts` and add the following:

```
[control]
control01 ansible_connection=local var_hostname=control01
control02 ansible_connection=ssh var_hostname=control02
...


[workers]
cube01 ansible_connection=ssh var_hostname=cube01

[cube:children]
control
workers
```

## Add passwordless login

1. Generate a SSH key, e.g. `ssh-keygen -t rsa`
2. Copy the SSH public key onto each node using `ssh-copy-id -i ~/.ssh/<key>.pub ubuntu@<node>`
3. Verify your setup by signing in to the node using ssh, `ssh ubuntu@<node>`

## Test your ansible setup

Run the ansible ping module on all hosts to make sure they are all reachable, `ansible cube -m ping`.
