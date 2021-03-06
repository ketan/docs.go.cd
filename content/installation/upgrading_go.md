---
description: To upgrade from a previous version of GoCD, it is only necessary to upgrade the Server. Agents will automatically update to the correct version of GoCD.
keywords: upgrade gocd, gocd server upgrade, configuration backup, database backup, build artifacts,
title: Upgrading GoCD
---

# Upgrading GoCD

While upgrading GoCD agents is not necessary (agents will usually auto-upgrade if they detect a new version deployed on the server). Going forward, we will ensure that agents are backward compatible with atleast 1

### Before you start

- Before you begin upgrading, it is recommended that you turn on maintainance mode on your GoCD server (available since version 19.1). Maintainance mode can be turned on under _Admin > Server Maintenance Mode_
- Navigate to the backup page under _Admin > Backups_ and take a backup. This may take several minutes, depending on the size of your GoCD database and configuration.
- The GoCD server acts as a repository for all your build artifacts. While it is not essential to backup the artifacts before an upgrade, it is good practice to make regular backups of this directory.

    You can configure where GoCD stores build artifacts. The following are the default locations of the artifacts if you have not customized its location:

    | Operating System  | Location                        |
    | ----------------- | ------------------------------- |
    | Linux             | `/var/lib/go-server/artifacts`  |
    | Windows, Mac OS X | `${GOCD_INSTALL_DIR}/artifacts` |

### Upgrading to the new version

You do not need to stop the Agents to perform an upgrade. GoCD agents will automatically update to the correct version of the software. You do not need to upgrade the GoCD agents. Any builds in progress will be rescheduled, and the existing pipelines will complete as expected. We ensure that the installed server will remain compatible with a version of a GoCD agent that is atleast 1 year old.

GoCD will perform upgrades of its configuration and database when it starts. This process may take some time for installations with a large number of historical builds (10 to 15 minutes on very large installations). If you suspect that there is a problem with the upgrade, check the go-server.log to see if there are any reported errors. This is a one-time migration and subsequent restarts will be much faster.

#### Windows

Run the GoCD installer. Make sure that you specify the same directory as your previously installed version.

If you have changed the GoCD Server Windows service to run as a different user, you will need to repeat this configuration change.

The installer will automatically start the service. Once GoCD completes its internal data changes, you should be able to see the GoCD webpage. Any existing Agents should automatically reconnect. Any builds in progress should continue, or be rescheduled.

#### Linux

#### Debian based distributions (i.e. Ubuntu)

Run the GoCD installer as described

```bash
sudo dpkg -i /path/to/go-server-${version}.deb
# OR
sudo apt-get update
sudo apt-get install go-server
```

#### RPM based distributions (i.e. RedHat)

Run the GoCD installer as described
```bash
sudo rpm -Uvh /path/to/go-server-${version}.deb
sudo yum upgrade go-server
```

#### Macintosh OSX

The Macintosh OSX edition of Go does not support upgrades. You should simply extract a new version of the GoCD server into a new location and copy over the config and artifacts directory from the old location.
