# Using The DeployPHP Role

The `deploy-example.yml` playbook contains the main features of PHP Playball. This file configures whether you are running PHP deployments and/or running MySQL Migrations.

The DeployPHP role uses the [Ansible Deploy Helper Module](https://docs.ansible.com/ansible/latest/modules/deploy_helper_module.html).

> At the bottom of this file, make sure the path to the included roles is correct in case you changed the directory structure.

You can rename this file if you wish.

Inside deploy-example.yml you will find very detailed comments about all the configuration values you can set for this deployment.

> In this file, some values can be/are commented out, this means they will use the default values found in `/playball/roles/deployphp/defaults/main.yml`.

## Name And Hosts

Here you can edit the name of this playbook and the host(s) you will deploy to. Set it to the specfic host or a group of hosts as configured in the Ansible inventory file on the control machine.

## Configuration

### Setup Paths

Set the path that Ansible will deploy to. It will create a releases/ directory in here and a current/ symlink.

Set Ansible's working directory for THIS deployment which you created on the host.

### Configure GitHub

Set the repo and branch name (default will be master).

### Configure Releases

Ansible will create directories with a timestamp as a name for each new deployment. These are releases. A symlink will point to the latest release. Configure how many OLD releases to keep (not including the current release).

### Configure Mysql Details

Set the name of the database to run migrations on, and the path to the `.my.cnf` file which must contain the user and password. It should look like this:
```
[client]
    user=dbuser
    password=dbpassword
    host=localhost
```

## Enable/Disable Entire Roles

Set the role configs to true or false to enable or disable the entire PHP deployment and/or MySQL Migrations. You can run either or both. If set to false, all the tasks in that role will be skipped.
```
RUN_PHP_DEPLOYMENT: true
RUN_MYSQL_MIGRATIONS: true
```

## Toggle And Set Deploy Tasks

Task configs take the form of an all-caps variable set to true or false (true will perform that task), and sometimes a related variable with values used in that task.

### CREATE_SHARED_DIRS

Ensure a list of directories exists to be shared by or copied into the releases (such as logs). Needed for symlinks, copying, etc. You should manually add files to these directories to be used by later tasks (like symlinking secret config files.) This task will simple confirm these exist in the `deploy/shared/` directory.

### CLONE_PROJECT_FROM_GIT

Whether or not to clone the project from Git.

### COMPOSER_INSTALL_DEPENDENCIES

If true, will run the command: `composer install --no-dev --ignore-platform-reqs` on the root of the project.

### COPY_FROM_PREV_RELEASE

If true, this task will copy the list of files or directories from the previous release into the new one. For example if you have wordpress installed in the blog/ directory of the project, it will copy this entire directory into the new release.

### COPY_FROM_SHARED_DIR

Copy items from the shared directory (src) to the new release (dest). Use this instead of symlink if desired (especially if it might change between releases).

### DECRYPT_VAULT_FILES

Decrypt files that were encrypted with Ansible vault (it will fetch the files locally temporarily then decrypt and copy back to the host).

If using Ansible vault to decrypt files you must add the `--ask-vault-pass` flag to prompt you for the vault password:
```bash
ansible-playbook deploy-example.yml --ask-vault-pass
```

### DELETE_FILES_FOLDERS

Delete files and directories under the deployed release. Items which are not used by the production site like tests and git files.

### CREATE_SYMLINKS

Create symlinks in the project release (path) to shared files and directories in the shared/ directory (src). Can be used for common items between releases like a shared logs/ directory. When used for files, they should not change between releases.

### EDIT_LINES_IN_FILES

Based on regex, edit lines in some files (located in release dir). Example: set environment to PRODUCTION.

### CHANGE_OWNER

If the user which Ansible connects to the server with, and the user which runs PHP are different, you might need to change the owner of the release directory. Performs a recursive chown command for owner and group.

## Including the Roles

The bottom of this playbook includes the roles which perform these tasks. Ensure the path to these roles is correct if you changed the relative path from the deploy playbook to these roles.
