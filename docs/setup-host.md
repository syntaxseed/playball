# Setup The Host Machine

The host machine is the server you will be deploying to. Aka the target machine. You can deploy to multiple hosts at one by adding them to a group in the Ansible inventory and running your playbook on that group.

## 1. Install Python 3

- [Python installation guide](https://docs.python-guide.org/starting/install3/linux/).
- [Setting Up Python on DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-install-python-3-and-set-up-a-programming-environment-on-an-ubuntu-20-04-server).
- Ubuntu:
  ```bash
  sudo apt-get install software-properties-common
  sudo add-apt-repository ppa:deadsnakes/ppa
  sudo apt-get update
  sudo apt-get install python3.8
  ```

## 2. Install PyMySQL Module

- [PyMySQL installation docs using PIP](https://pypi.org/project/PyMySQL/#installation).
- Ubuntu:
  ```bash
  sudo apt-get install python3-pymysql
  ```

## 3. Enable SSH Key Forwarding

Edit: `/etc/ssh/sshd_config` and make sure `AllowAgentForwarding yes` exists and is uncommented.

## 4. Set Up Ansible's Work Directory

- Create a private (not under the web root) working directory for Ansible to do its work: ex: `/home/myuser/ansible/`.
- Inside this directory, create a tmp directory and a subdirectory for each site being deployed on this machine:
  - /home/myuser/ansible/
    - .ansible/
      - tmp/  (tmp dir for ansible.)
    - {examplecom}/
      - .my.cnf (mysql settings for ansible - see playbook):
        - ```
          [client]
              user=dbuser
              password=dbpassword
              host=localhost
          ```
- TIP: the /home/myuser/ansible/.ansible/tmp/ directory should be emptied periodically.

- Make sure the `.my.cnf` file is set with the proper MySQL user, password and host in the above YAML format.

## 5. Set Up The Deploy Directory

- Create the initial state of the deployment directory. For example if this deployment is called `example.com` you'll set up the following:

  - /var/www/example.com/deploy/
    - current/ (symlink to latest release)
    - releases/ (create this empty dir for the first time)
      - 20200101000000/ (Timestamp release directories created by Ansible.)
        - public/
        - ...
      - 20201214110051/
    - shared/ (files or folders that will be copied or symlinked to new releases, like configs or a logs directory)
      - secure/
        - config-STAGING.php

- Place an example index.php file in the public directory of the example release above (`/var/www/example.com/deploy/releases/20200101000000/public/index.php`), and put some example text in there.
- Create a symlink from current/ to the latest release:
  ```bash
  cd /var/www/example.com/deploy/
  ln -s /var/www/example.com/deploy/releases/20200101000000/ current
  ```
- Now if you change directory to `/var/www/example.com/deploy/current/` you will actually arrive in the example release we created. We will use this to test our document root settings in the next step.

## 6. Set The Site's Document Root

- Configure the site's DocumentRoot in its Apache settings for virtual hosts to be: `/var/www/example.com/deploy/current/public`.

- [DigitalOcean guide to Apache, Virtual Hosts section](https://www.digitalocean.com/community/tutorials/how-to-install-the-apache-web-server-on-ubuntu-18-04#step-5-%E2%80%94-setting-up-virtual-hosts-(recommended)).

Example (`/etc/apache2/sites-available/example.com.conf`):
```
<VirtualHost *:80>
    ServerAdmin webmaster@localhost
    ServerName example.com
    ServerAlias www.example.com
    DocumentRoot /var/www/example.com/deploy/current/public
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```
- Restart Apache: `sudo service apache2 restart`.

- [DigitalOcean Nginx guide to changing document root](https://www.digitalocean.com/community/tutorials/how-to-move-an-nginx-web-root-to-a-new-location-on-ubuntu-18-04).

## 7. Test Your Website

Visit your website. Check if the sample text you placed in the test index.php file is appearing. If not, double check a few things (below) or re-follow this guide.
  * Did you restart Apache or Nginx after changing Document Root?
  * Did you setup the symlink from current/ into the latest release? Don't make the symlink point to the public/ directory, just to the timestamp named release directory.
  * Make sure the DocumentRoot does point to the public/ directory.
