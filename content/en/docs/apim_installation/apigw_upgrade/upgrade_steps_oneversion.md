{
"title": "Update from API Gateway One Version",
  "linkTitle": "Update from API Gateway One Version",
  "weight": 25,
  "date": "2020-09-30",
  "description": "Learn how to update from API Gateway One Version to the latest delivery."
}

After you **upgrade** your [API Gateway 7.5.x or 7.6.x](/docs/apim_installation/apigw_upgrade/upgrade_steps_extcass/) to [API Gateway One Version](https://community.axway.com/s/question/0D52X00008WUjgeSAD/introducing-one-version-for-api-management), follow the instructions on this page to **update** your API Gateway One Version to the latest delivery.

* The [sysupgrade](/docs/apim_installation/apigw_upgrade/upgrade_script/) script is not used when updating to API Gateway One Version.
* If you have installed a licensed version of API Gateway One Version or API Manager One Version, you do not require a new license to install updates.

## Before you start

You must perform the following procedures to update from API Gateway One Version:

{{% alert title="Note" %}}All these steps are mandatory, and must be followed in this order.{{% /alert %}}

1. [Back up customized files](#back-up-customized-files).
2. [Install an API Gateway server update](#install-an-api-gateway-server-update).
3. [Install a Policy Studio update](#install-a-policy-studio-update).
4. [Install a Configuration Studio update](#install-a-configuration-studio-update).
5. [Install an API Gateway Analytics update](#install-an-api-gateway-analytics-update).

## Back up customized files

Before updating your product, ensure to back up any customized files from your `INSTALL_DIR` directory.

The following is a list of directories that might contain customized files:

```
webapps/apiportal/vordel/apiportal
webapps/emc/vordel/manager/app
webapps/emc
system/conf/apiportal/email
system/conf
samples/scripts/
tools/filebeat-VERSION-PLATFORM
```

When you are restoring the files, ensure that you merge any updated files instead of copying them back directly, to avoid any regex matching issues.

## Install an API Gateway server update

If you have installed an existing version of API Manager, installing the **API Gateway server** update automatically also installs the updates and fixes for API Manager.

You can easily install API Gateway server update by running the `update_apigw.sh` script. The script performs the following:

* Check that the installation directory is valid.
* Check that the user who owns the API Gateway binaries is the same user running the `update_apigw.sh` script.
* Check that the Node Manager and API Gateway instances that run on the local machine are not running.
* Take a backup unless `--no_backup` has been specified.
* Install the update content into the API Gateway installation directory.

The script also performs all of the steps that were performed by a post installation script in earlier updates. This includes:

* JRE cleanup
* `system/lib/modules` cleanup
* Third-party and Axway JAR cleanup
* Apply `acl.json` fix
* Apply passphrase obfuscation fix
* Apply modifications to Node Manager entity store configuration.

Fixes are only applied if they have not previously been applied.

To install the update on your existing API Gateway 7.7 server installation, perform the following steps:

1. Ensure that your existing API Gateway instance and Node Manager have been stopped.
2. Remove any previous patches from your `INSTALL_DIR/ext/lib` and `INSTALL_DIR/META-INF` directories (or the `ext/lib` directory in an API Gateway instance). All patches have already been included in API Gateway One Version, so you do not need to copy patches from a previous version.
3. If you have used `setcap` to grant API Gateway permission to use privileged ports (see, [Allow the API Gateway to listen on privileged ports](#allow-the-api-gateway-to-listen-on-privileged-ports)), remove these permissions now because thy might prevent files from being overwritten.

   ```
   setcap -r INSTALL_DIR/apigateway/platform/bin/vshell
   ```

4. Download and unpack the API Gateway 7.7 server update file into a new directory. For example:

   ```
   mkdir 77update
   tar xzvf APIGateway_7.7.YYYYMMDD_Core_linux-x86-64_BNnn.tar.gz -C 77update
   ```

   {{< alert title="Note" color="primary" >}}You must extract the file into a new directory, and not into the existing API Gateway installation directory.{{< /alert >}}
5. Run the `update_apigw.sh` script from the directory into which you extracted the update file (for example, `77update`) and specify your API Gateway installation directory using the `--install_dir` option. For example:

   ```
   ./update_apigw.sh --install_dir /opt/Axway-7.7/
   ```
6. Restart your Node Manager and API Gateway instances on the local machine.

Run the `update_apigw.sh` script with the `--help` option to see the available options:

```
./update_apigw.sh --help
```

To run the script without user interaction, specify `--mode unattended` option.

### Change the trace level

The `update_apigw.sh` script generates a trace file in the `update-output/trace` directory. Use the `--tracelevel` option to [change the level of tracing](/docs/apim_administration/apigtw_admin/tracing/#set-api-gateway-trace-levels).

### Specify a directory to back up your installation

The script takes a backup of your entire API Gateway installation directory and places it in a `tar` file in the `update-output/backups` directory. Specify a different directory using the `--backup_dir` option. To manage your own backups, use the `--no_backup` option.

### Complete zero downtime update steps (Optional)

If you were using zero downtime update (ZDU), you must discard the old keyspace in cqlsh now. For more information see, [Update the name of the target keyspace](#update-the-name-of-the-target-keyspace)

## Install a Policy Studio update

You can run the `update_policy_studio.sh` script, which is available from API Gateway One Version Policy Studio update pack (for example, `APIGateway_7.7.YYYYMMDD_PolicyStudio_linux-x86-64_BNnn.tar.gz`) to update your existing Policy Studio installation.

Running this script performs the following:

* Back up your existing `INSTALL_DIR/policystudio` directory. The backup of the installation is created at `INSTALL_DIR/backups/policystudio/<date_time>`.
* Remove old JRE versions by deleting the `INSTALL_DIR/policystudio/jre` directory.
* Unzip and extract API Gateway 7.7 Policy Studio update over the `policystudio` directory in your existing API Gateway 7.7 installation directory.
* Start Policy Studio with `policystudio -clean`.

To install the Policy Studio update, download and unpack the API Gateway 7.7 Policy Studio update file into a new directory. For example:

```
mkdir 77update
tar -xzvf APIGateway_7.7.YYYYMMDD_PolicyStudio_linux-x86-64_BNnn.tar.gz -C 77update
```

* You must execute the update script using the same user who installed Policy Studio.
* You must extract the tar.gz file into a new directory, and not into the existing API Gateway installation directory.
* You must also remove the `libeay32.dll` and `ssleay32.dll` files if they exist in the `INSTALL_DIR/policystudio` directory.

Run the `update_policy_studio.sh` script from the directory into which you extracted the update file (for example, `77update`), and specify your API Gateway installation directory as an argument:

```
./update_policy_studio.sh $INSTALL_DIR
```

`$INSTALL_DIR` is the base API Gateway 7.7 installation directory that contains the `policystudio` directory.

For example:

```
./update_policy_studio.sh /opt/Axway-7.7/
```

If you had applied any modifications to the `policystudio.ini` file, you must reapply them after upgrade.

### Install a Policy Studio update on Windows

For installations running on Windows 7, you must manually unzip the Policy Studio update.

The `update_policy_studio.bat` update script is available for Windows. It is located in the API Gateway 7.7 Policy Studio Update pack (`.zip`) for Windows.

## Install a Configuration Studio update

You can run the `update_configuration_studio.sh` script, which is available from API Gateway One Version Configuration Studio update pack (for example, `APIGateway_7.7.YYYYMMDD_ConfigurationStudio_linux-x86-64_BNnn.tar.gz`), to update your existing Configuration Studio installation.

Running this script performs the following:

* Back up your existing `INSTALL_DIR/configurationstudio` directory. The backup of the installation is created at `INSTALL_DIR/backups/configurationstudio/<date_time>`.
* Remove old JRE versions by deleting the `INSTALL_DIR/configurationstudio/jre` directory.
* Unzip and extract API Gateway 7.7 Configuration Studio Update over the `configurationstudio` directory in your existing API Gateway 7.7 installation directory.
* Start Configuration Studio with `configurationstudio -clean`

To install the Configuration Studio update, download and unpack the API Gateway 7.7 Configuration Studio update file into a new directory. For example:

```
mkdir 77update
tar -xzvf APIGateway_7.7.YYYYMMDD_ConfigurationStudio_linux-x86-64_BNnn.tar.gz -C 77update
```

* You must execute the update script using the same user who installed Configuration Studio.
* You must extract the file into a new directory and not into the existing API Gateway installation directory.
* You must remove the `libeay32.dll` and `ssleay32.dll` files if they exist in the `INSTALL_DIR/configurationstudio` directory.

Run the `update_configuration_studio.sh` script from the directory into which you extracted the Update file (for example, `77update`), and specify your API Gateway installation directory as an argument:

```
./update_configuration_studio.sh $INSTALL_DIR
```

`$INSTALL_DIR` is the base API Gateway 7.7 installation directory that contains the `configurationstudio` directory.

For example:

```
./update_configuration_studio.sh /opt/Axway-7.7/
```

If you had applied any modifications to the `configurationstudio.ini` file, you must reapply them after upgrade.

### Install a Configuration Studio update on Windows

For installations running on Windows 7, you must manually unzip the Configuration Studio update.

The `update_configuration_studio.bat` update script is available for Windows. It is located in the API Gateway 7.7 Configuration Studio update pack (`.zip`) for Windows.

## Install an API Gateway Analytics update

To install the update on your existing API Gateway Analytics 7.7 installation, perform the following steps:

1. Ensure that your existing API Gateway Analytics instance and Node Manager have been stopped.
2. Remove old third-party libraries by deleting the following directories:

   ```
   INSTALL_DIR/analytics/system/lib/modules
   ```
3. Remove old JRE versions by deleting the following directories:

   ```
   INSTALL_DIR/analytics/platform/jre
   ```
4. Verify the owners of API Gateway binaries before extracting the update.

   ```
   ls -l INSTALL_DIR/analytics/posix/bin
   ```
5. Using the same user who owns the API Gateway Analytics binaries, unzip and extract API Gateway 7.7 Analytics update over the `analytics` directory in your existing API Gateway 7.7 installation directory. For example:

   ```
   tar -xzvf APIGateway_7.7.YYYYMMDD_Analytics_linux-x86-64_BNnn.tar.gz -C /opt/Axway-7.7/analytics/
   ```
6. Change to the `analytics` directory in your installation:

   ```
   cd $INSTALL_DIR/analytics
   ```
7. Run the post-install script for API Gateway Analytics.

   ```
   apigw_analytics_sp_post_install.sh
   ```

## Allow API Gateway to listen on privileged ports

If you are running API Gateway as a non-root user, you must perform the following steps to grant the required privileges to API Gateway processes to use privileged ports, ports below `1024`.

These steps are not required if API Gateway only needs to use ports numbered `1024` and above.

After updating your installation, perform the following steps:

1. Add the following line to the `INSTALL_DIR/system/conf/jvm.xml` file:

   ```
   <VMArg name="-Djava.library.path=$VDISTDIR/$DISTRIBUTION/jre/lib/amd64/server:$VDISTDIR/$DISTRIBUTION/jre/lib/amd64:$VDISTDIR/$DISTRIBUTION/lib/engines:$VDISTDIR/ext/$DISTRIBUTIONlib:$VDISTDIR/ext/lib:$VDISTDIR/$DISTRIBUTION/jre/lib:system/lib:$VDISTDIR/$DISTRIBUTION/lib"/>
   ```

2. Allow API Gateway to listen on privileged ports:

   ```
   setcap 'cap_net_bind_service=+ep cap_sys_rawio=+ep' INSTALL_DIR/platform/bin/vshell
   ```

## Update API Manager

Updating API Manager is now carried out during the application of the latest API Gateway update. This will update the API Manager `config` script, which can then be pulled into a Policy Studio project. However, it is still required to perform one of the following three options to ensure that the update of API Manager performed by the API Gateway update is not overwritten by older deployments or policies.

* Policy Studio project upgrades. Importing an existing API Manager Policy Studio project will upgrade API Manager. The upgrade is also applied when creating a new project from an existing `fed` file.
* API Manager `.fed` files can be upgraded using the [upgradeconfig](/docs/apim_installation/apigw_upgrade/upgrade_analytics#upgradeconfig-options) script.
* The [projupgrade](/docs/apim_reference/devopstools_ref#projupgrade-command-options) script will apply API Manager updates to any existing projects.

## Update cipher scheme

The cipher scheme for all encrypted data in the system (such as Database/LDAP passwords, private keys, and so on) uses PBKDF2 (Password based key derivation function 2) with more secure parameters. This reduces vulnerability to brute force attacks. The cipher scheme is backwards compatible with previous versions of the cipher scheme, and it is able to decrypt data, which was encrypted in the old cipher scheme.

The Entity store is re-encrypted with PBKDF2 as part of API Gateway update process, but the Key property store will not be re-encrypted.

To make use of a more secure cipher scheme, you must re-encrypt your KPS data using the `kpsadmin` command. For more information see, [Re-encrypt the KPS data](/docs/apim_policydev/apigw_kps/how_to_use_kpsadmin_command/#re-encrypt-the-kps-data).

### Transfer KPS data between environments

Encrypted KPS data cannot be transferred directly between environments even when the passphrase in use is the same in both environments. Instead, you must use one of the following options:

* Use the [KPS Admin Backup and Restore](/docs/apim_policydev/apigw_kps/how_to_use_kpsadmin_command/#back-up-and-restore) process. The restore command decrypts the data from the source environment and re-encrypts the data for the target environment.
* Use the [Cassandra Backup and Restore](/docs/cass_admin/cassandra_bur/) process and run [KPS Admin Re-Encrypt](/docs/apim_policydev/apigw_kps/how_to_use_kpsadmin_command/#re-encrypt-the-kps-data).

### Use the cipher scheme with custom code

When instantiating a `PasswordCipher` in custom libraries or in Policy Studio script filters, be aware that this enhanced cipher algorithm will result in longer generation times. While not recommended, it is possible to revert to the previous weaker cipher if absolutely necessary, should performance be the higher concern.

To revert to the weaker cipher algorithm, you can use the `setMode` method, such as in the following `jython` snippet:

   ```
   from com.vordel.common.crypto import PasswordCipher
   
   def invoke(msg):
      cipher = PasswordCipher("ThisIsAPassword!123".toCharArray())
      cipher.setMode(PasswordCipher.Mode.SHA1_3DES)
   ```

## Zero downtime update

This section describes any steps or considerations to account for a zero downtime update (ZDU).

To easily accomplish a zero downtime update, you can set up your API Gateway in a different environment. After this environment is fully configured and the data has been migrated to it, promote this environment to production.

### Cassandra schema ZDU

When there is a change in the Cassandra schema in API Manager, you must take a special precaution to allow a zero downtime update, because Cassandra schema changes cause API Manager instances, which are not yet upgraded, to no longer perform read or write to the changed tables.

The `clone-cassandra-keyspace.sh` script clones the keyspace, so that two versions of the keyspace can exist during the upgrade process. The original keyspace allows API Gateway nodes, which have not been updated, to still serve requests, and the cloned keyspace is used for API Gateway nodes, which have been updated.

The keyspace schema is updated after the first API Gateway node is updated. If there is no change to the keyspace schema in your update, then you do not need to clone the keyspace.

You must copy the script, which is located in `INSTALL_DIR/apigateway/tools/clone-cassandra-keyspace.sh`, to one Cassandra node, then run the script.

#### Prerequisites

* Ensure that you create a [full backup of Cassandra data](/docs/cass_admin/cassandra_bur/) and archive it before attempting to duplicate the keyspace.
* Ensure that there is enough space on disk to duplicate the keyspace data. The size of disk space needed is approximately the same as the space taken up by `cassandra/data/data/$KEYSPACE_NAME`.
* The maximum number of rows in any table is 2 million rows.
* After the keyspace is duplicated, new data written in the original keyspace will not be rewritten into the new keyspace. Therefore, we recommend you to restrict any changes to the Cassandra data during the ZDU process.
* Do not make any changes to users and APIs during the update process.
* You must disable API Portal during the update process.
* The keyspace name must be environmentalized within the API Gateway configuration.

    ![Getting Started - sample User data in KPS browser](/Images/UpgradeGuide/upgrade_environmentalized_keyspace.png)

{{< alert title="Caution" color="warning" >}}
If OAuth tokens are being using to access protected resources, it is important to note that during the update process:

* An updated API Gateway, configured to point to the new cloned keyspace (new schema), will not be able to access any newly generated tokens stored in the original keyspace.
* An API Gateway that has not yet been updated, and is still configured to use the original keyspace, will not be able to access any newly generated tokens stored in the new cloned keyspace.

Therefore, you must ensure that load balancer settings are configured to allow OAuth traffic through the appropriate API Gateway with access to the correct OAuth tokens.
{{< /alert >}}

#### Configure cqlsh

To start configuring your new API Gateway environment, add the following to the `cassandra/conf/cqlshrc.sample` file to set up authentication, SSL trust, and CSV file parameters.

```
[authentication]
;; If Cassandra has auth enabled, fill out these options
username = cassandra
password = cassandra
; keyspace = ks1
...
[ssl]
certfile = ~/.cassandra/cassandra-ca.pem
...
[csv]
;; The size limit for parsed fields
; default: field_size_limit = 131072
; size limit is 10 MB
field_size_limit = 10485760
...
```

#### Configure the clone keyspace script

You must update the following variables within the `clone-cassandra-keyspace.sh` script to suit your environment:

| Name | Description |
|------|-------------|
|cqlsh|Location of cqlsh utility|
|sourceCassClusterHost|Hostname of the source cluster|
|sourceCassClusterPort|Port of the source cluster|
|sourceCassClusterUsr|Username of the source cluster|
|sourceCassClusterPwd|Password of the source cluster|
|targetCassClusterHost|Hostname of the target cluster|
|targetCassClusterPort|Port of the target cluster|
|targetCassClusterUsr|Username of the target cluster|
|targetCassClusterPwd|Password of the target cluster|
|apimSourceKeyspace|Name of the source keyspace|
|apimTargetKeyspace|Name of the target keyspace|

#### Clone the keyspace

Follow these steps to clone the keyspace:

1. Log in to one of the nodes in the source cluster.
2. Run `nodetool status` to check the status of the node. Ensure all nodes in the cluster are up and normal.
3. Copy the cloning script to the Cassandra node.
4. Review the restrictions, ensuring you are ready to run the script.
5. Run the script `./clone-cassandra-keyspace.sh` to clone the keyspace.
6. In case of any warnings or errors, check the logs.

The keyspace should now be cloned.

#### Update the name of the target keyspace

This newly cloned keyspace is the keyspace that the upgraded nodes will use to upgrade the schema. Before upgrading an API Gateway node, change its `INSTALL_DIR/apigateway/groups/group-x/instance-y/envSettings.props` to reflect the cloned target keyspace name. The following example, sets the the name as `apim_zdu_test`.

```
env.CASSANDRA.KEYSPACE=apim_zdu_test
```

After this is complete, follow the instructions to [Install an API Gateway server update](#install-an-api-gateway-server-update).

After the update is fully installed on all API Gateway nodes, discard the old keyspace in cqlsh.

```
cqlsh -e "DROP KEYSPACE old_keyspace_name"
```

### Zero Downtime Update Example Flow

The following is an example a topology might use in order to acheive a zero downtime update.

{{< alert title="Note" color="primary" >}}If not already done, environmentalize the keyspace name in the API Gateway configuration, such that you can configure it on a per server basis. See [Configure environment variables](/docs/apigtw_devops/promotion_arch/#configure-environment-variables){{< /alert >}}

Given a sample topology of:

* 2 API Gateway nodes
* 3 Cassandra nodes in a single cluster
* A load balancer

The follow steps would achieve a zero downtime update:

1. Execute the steps to [Clone the keyspace](#clone-the-keyspace).
2. Configure the load balancer to only send traffic to API Gateway node 2.
3. Update envSettings.props for API Gateway node 1 such that the keyspace used is the newly cloned keyspace.
4. [Install an API Gateway server update](#install-an-api-gateway-server-update) on API Gateway node 1. At this point, the cloned keyspace will be upgraded to the new schema.
5. Configure the load balancer to only send traffic to API Gateway node 1.
6. Update envSettings.props for API Gateway node 2 such that the keyspace used is the newly cloned keyspace.
7. [Install an API Gateway server update](#install-an-api-gateway-server-update) on API Gateway node 2.
8. Configure the load balancer to use all API Gateway nodes.
9. Once everything is validated to be up and running, drop the original keyspace.