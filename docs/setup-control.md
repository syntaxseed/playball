# Set Up Control Machine

The machine or server you will run your playbooks from. This might be your own development machine, or a server used specifically for running deployments.

## 1. Install Ansible

[Ansible installation docs.](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html)

- Ubuntu:
  ```bash
  sudo apt update
  sudo apt install software-properties-common
  sudo apt-add-repository --yes --update ppa:ansible/ansible
  sudo apt install ansible
  ```

## 2. Create SSH Keys

- Create an SSH key for for connecting to the host/target machine.
- Create a key for reading the repo on GitHub. Add the key to the repo.
- [Github SSH key docs.](https://docs.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh).

## 3. Add Ansible Hosts

- Create or edit the Ansible hosts file (ex `/etc/ansible/hosts`) to add the host (ex `server123.com`) you will be deploying to, to a new group (ex `[clientABC]`) in the inventory:
  ```
  [clientABC]
  server123.com ansible_port=22 ansible_user=serveruser ansible_ssh_private_key_file=~/.ssh/id_ansible_server123
  ```
  - This points to your SSH key for connecting to the server.
- [Ansible hosts documentation](https://docs.ansible.com/ansible/latest/user_guide/intro_inventory.html).

## 4. Add SSH Keys To Keyring

- If your SSH keys have passwords, add the SSH key for the host, and for the GitHub repo you are deploying from, to your keyring.
  ```bash
  eval `ssh-agent -s`
  ssh-add -t 1h ~/.ssh/id_ansible_server123
  ```
  - The above example will start the agent and remember the password for the given key for 1 hour.
- [Ansible SSH key docs](https://docs.ansible.com/ansible/latest/user_guide/connection_details.html#setting-up-ssh-keys).
