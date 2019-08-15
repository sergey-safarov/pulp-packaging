**Client configration**

Thil manula describe pulp admin client usage packaged into docker image.
If you want use pulp client instaled on your host, pleach check https://docs.pulpproject.org/user-guide/installation/index.html#instructions

```sh
mkdir ~/.pulp
touch ~/.pulp/admin.conf
chmod 700 ~/.pulp
chmod 600 ~/.pulp/admin.conf
```

Update content of `~/.pulp/admin.conf` like example and set your login and password.
```
[server]
host: rpm.kamailio.org

[auth]
username: admin
password: admin
```

Test conectivity to pulp server
```sh
docker run -it --rm --network host -v ~/.pulp:/root/.pulp pulp/admin-client pulp-admin repo list
```

To simplify commands you can add into `~/.bashrc` file this string
```
alias pulp-admin='docker run -it --rm --network host -v ~/.pulp:/root/.pulp pulp/admin-client pulp-admin'
```

Then check simple pulp client command
```sh
pulp-admin repo list
```

**New release repo configration**

To create new release repo please execute
```sh
pulp-admin rpm repo create \
    --repo-id=kamailio-centos7-5.2.4 \
    --relative-url=centos/7/5.2/5.2.4 \
    --feed=https://download.opensuse.org/repositories/home:/kamailio:/v5.2.x-rpms/CentOS_7/
```

Check repo status
```sh
pulp-admin repo list --repo-id kamailio-centos7-5.2.4
```

Publish repo
```sh
pulp-admin rpm repo publish run --repo-id kamailio-centos7-5.2.4
```

New repo will be avaialable at http://rpm.kamailio.org/centos/7/5.2/5.2.4/

**Backup repo**

You can backup repo to server folder using command
```sh
pulp-admin rpm repo export run --repo-id kamailio-centos7-5.2.4 --export-dir /var/lib/pulp/backup
```
In this command usage of `/var/lib/pulp/backup` folder is importand.
If used other folder, then need to mount this backup folder into `worker` containers on rpm.kamailio.org server.

Then you can download this backup folder to your local PC using command
```sh
rsync -r root@rpm.kamailio.org:/var/lib/pulp/backup/kamailio-centos7-5.2.4 ~/your_local_pc_backup_folder
```

**Copy release repo to latest repo**

To copy content of new release repo to latest branch repo need execute this commnads
```sh
pulp-admin rpm repo delete --repo-id=kamailio-centos7-5.2
pulp-admin rpm repo create --repo-id=kamailio-centos7-5.2 --relative-url=centos/7/5.2/5.2
pulp-admin rpm repo copy rpm --from-repo-id=kamailio-centos7-5.2.4 --to-repo-id=kamailio-centos7-5.2
pulp-admin rpm repo publish run --repo-id kamailio-centos7-5.2
```