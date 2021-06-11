# Get Playball

You have two options to use PHP PlayBall. Either place a copy of PHP Playball in your project's repository, or keep all your deployments together in one repository.

## Copy Repo Into Your Project

- Get a copy of the PHP [Playball repo from GitHub](https://github.com/syntaxseed/playball), either by cloning it, or downloading a zip.
- You will only need the `playball` and `example` directories.
- Rename the example directory however you wish.
- Place these 2 directories into your project's repo.

Example:
```
- my-project/
  - app-src/
  - deploy/
    - playball/
    - example/   (Rename this)
```

```bash
cd my-project
mkdir deploy
cd deploy
git clone git@github.com:syntaxseed/playball.git .
rm -r .git/
cp example/ playbooks/
```

> TIP: If you move the files from inside the example/ directory up one level instead of in a directory, you will have to fix a few paths to the roles found at the bottom of the deploy and deployrollback .yml files.

## All Deployments In One Repo

Another option is to copy or fork this project and keep duplicating the `example/` directory for each of your projects. This will make it easy to find all your deploy playbooks in one place and pull in changes from new releases of PHP Playball.

> WARNING: Updates to PHP Playball might not be backward compatible (major version number increases) with your project's files. Pulling updates from new PHP Playball releases into your local copy might require updates to your playbooks' configuration files (files copied from example/).

Example:
```
- php-playball/
    - playball/
    - example/
    - project-A/  (copy of example/)
    - project-B/  (copy of example/)
```