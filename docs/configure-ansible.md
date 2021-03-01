# Configure Ansible

Once you have made a copy of the example/ directory for your project, let's say you called it playbooks/, you can edit the configuration file for Ansible for this deployment.

The `ansible.cfg` file contains Ansible configuration.

Set the directory for the temp directory for Ansible on the host machine. This must match the directory you create when [setting up the host](setup-host.md).

```
remote_tmp = /home/myuser/ansible/.ansible/tmp
```

The `nocows` setting simply disables the cute ASCII art cows that Ansible displays.

TIP: To silence output of skipped tasks, set in your ansible.cfg:
`stdout_callback = skippy`
