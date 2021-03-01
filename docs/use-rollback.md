# Deploy Rollback

The `deployrollback-example.yml` playbook can be used to roll back to a previous release. This simply changes the `current/` symlink to point to the previous release.

> NOTE: This does not currently roll back the SQL migrations.

To roll back to a previous release:
```bash
ansible-playbook deployrollback-example.yml --extra-vars "releasenum=2"
```

Releasenum 2 is the second most recent. To return to the latest (newest), run the above again with `releasenum=1`.

This feature depends on having saved previous releases in the DeployPHP configuration.
