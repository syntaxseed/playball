# Quick Start

A quick overview of using PHP Playball.

## Setup Control Machine

[Detailed Guide](setup-control.md)

The machine you will run your playbooks from.

- Install Ansible
- Add the target host machine to Ansible's hosts file.
- Make sure you have an SSH key for connecting to the host.
- When running the playbooks, the SSH keys for the host and for the GitHub repo should be in your keyring if they have passwords.

## Ping The Server

Ping the host server with Ansible to see if it works.

Pings all servers in the specified group from the hosts file.

- `ansible clientABC -m ping`

You'll see info about the response. You're looking for a 'pong' response.

If the server does not have Python3 installed, you'll see a message.

## Set Up The Host Machine

[Detailed Guide](setup-host.md)

- Ensure Python3 and the PyMySQL module are installed.

## Grab PHP Playball From GitHub

Get a copy of the PHP [Playball repo from GitHub](https://github.com/syntaxseed/playball), either by cloning it, or downloading a zip. It is recommended to keep this inside your project's repo.

> Note that updates to PHP Playball might not be backward compatible, so pulling updates from this project into your local copy might require updates to your playbooks' configuration.

You'll also want to make a copy of the example/ directory for your site.

```bash
mkdir deploy
cd deploy
git clone git@github.com:syntaxseed/playball.git .
rm -r .git/
cp example/ playbooks/
```

## Configure The Playbooks

Now that you have a copy of the example playbooks inside `deploy/example/` you can edit these to add your project's settings.

- [Configure Ansible](configure-ansible.md)
- [Using DeployPHP](use-deploy.md)
- [Using DeployRollback](use-rollback.md)
- [Using MySQLMigration](use-mysqlmigration.md)

Once configured, run the deployment from within the playbooks/ directory:

```bash
ansible-playbook deploy-example.yml
```

---

> You're ready to go!\
> Like this project? Follow me on Twitter [@SyntaxSeed](https://twitter.com/syntaxseed) and share your success stories! :)
