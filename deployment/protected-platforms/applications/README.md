# Applications

This section describes vProtect's **generic mechanism** for multiple scenarios where VM-level backup may not be enough.

With vProtect you can prepare custom script or invoke any backup command that produces backup artefacts \(or just initiates an external backup process\) on a remote host and stores backups to your backup provider.

With Application backup you can extend your protection capabilities to:

* any remote applications with their own mechanisms
* hypervisor configuration
* files on remote hosts \(physical, virtual or containers\)
  * this includes shares, mounted object-storage buckets, LVM block devices or virtually anything which can be presented as a file
* initiating external backup processes such as RMAN

vProtect's internal DB backup mechanism also uses Application backup. The mechanism uses predefined artefacts that simply need to be configured according to your needs.

## Main concepts

There are 2 main concepts that vProtect uses to execute backups:

* Command Execution Configuration
* Application Definition

### Command Execution Configuration

This describes **how** to perform a backup operation. That is how to execute a command that produces a backup artefact which vProtect later stores in a backup provider. Multiple Application definitions share Command Execution Configuration but with different parameter values.

Command Execution Configuration properties come in several sections:

1. **General:**
   * **Name** - name of your configuration
   * **Execution type**:
     * Node - execute this command directly on the node
     * Remote SSH - execute this command over SSH using credentials provided in the Application definition
   * **Timeout** - fail execution if command doesn't complete within the time given
     * if you think that your  backup should take longer, increase this value
     * this timeout is for whole command execution - if you have several steps in your script and you need additional timeouts for these steps - add them to your script
2. **Command arguments:**
   * add arguments that contain spaces as separate arguments
   * the first argument is the path to your executable
   * make sure this command is accessible on the remote host, and vProtect credentials will suffice to execute it
   * remote commands \(over SSH\) will invoke shell so you can use bash-style expressions \(built-in commands such as `echo`, environmental variables or redirections\) within the command argument
   * commands executed on the node are executed natively by OS, so if you want to use bash-style expressions \(built-in commands such as `echo`, environmental variables or redirections\) you need to split your command at least into 3 arguments: `/bin/bash`, `-c` and `your command > with some redirections`
3. **Data export:**
   * **Export data** - when enabled, vProtect will expect artefacts to be collected as a result of a command
   * **Source type:**
     * FILE - result will be a file, directory or path with `*` wildcard
     * STREAM - output of your command
   * **Source path:**
     * path to your artefacts that need to be collected
     * file, directory or path with `*` wildcard - more than 1 file on the source will result in files being stored as a single tar archive
   * **Remove files after export:**
     * if artefacts \(files or source directory\) need to be removed once exported
     * be careful when providing a path in the source directory, the whole directory will be removed when this setting is enabled
4. **Applications:**
   * select which applications will use this command execution config
5. **Parameters:**
   * this section allows you to define the parameters that will be expected to be entered in each application definition
   * each parameter will eventually become an environment variable in the application definition
   * each parameter has several properties
     * Name - name of the resulting environmental variable
     * User-friendly hint - a hint what this parameter is to be shown later in the application definition
     * Default value - the default value, entered during initialization in the application definition form
     * Show in UI - if the value should be shown as dotted or not - useful for passwords
     * Obligatory - if we expect that its value should always be provided in the application definition form
6. **Error handling**
   * Standard error output stream handling \(when non-empty\):
     * Don't ignore - will fail if anything is in standard error output
     * Ignore without warning - will ignore it silently
     * Ignore with warning - will ignore it but a warning indicator in the backup history will contain this output
   * Ignored Exit Codes:
     * error codes that should be ignored and not treated by vProtect as errors
     * by default only 0 is assumed as a success

### Application Definition

Once you have your command execution configuration defined \(or you choose to use the predefined ones provided with vProtect\), you should define the instances of your application.

There are a few parameters for application definition that come in several sections:

1. **General:**
   * Name - name of your application instance
   * Choose node - which node is going to execute this command 
   * Backup policy - optionally set policy for scheduled backups
   * Command execution configuration
     * configuration of your command used for this application
     * **Note:** when you create a definition for the first time, you select a configuration and click **Save** - you will be redirected to the Settings tab for additional details
2. **Environment variables**
   * shown only when the definition has been saved on the Settings tab
   * defines a list of environment variables that will be passed to your command/script during its invocation
   * parameters from the command execution config will be populated automatically
   * each parameter has several properties:
     * Key - name of the environmental variable
     * Value - value of the environment variable
     * Show - if the value should be shown as dotted or not - useful for passwords
3. **SSH access:**
   * shown when Remote SSH is chosen as the execution type in command execution configuration
   * parameters:
     * SSH host - host where the command will be executed
     * SSH port - port on which the SSH service is running \(by default 22\)
     * SSH user - user used to connect via SSH
     * SSH key path:
       * path to your key - needs to be a file only accessible by vProtect with `400` permissions
       * alternatively you can use the password access method
4. **Password:**
   * shown when Remote SSH is chosen as the execution type in command execution configuration
   * set your SSH password here if you're not using the public-key authentication method

## Scheduled backups

Please check the [Scheduled Backup](../../../administration/virtual-environments/backup-slas/schedules.md) section for details.
