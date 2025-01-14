# synology-scripts

Scripts for Synology DSM

## `reconnect-vpn.sh`

This script can be used as a workaround for Synology DSM's less-than-ideal reconnect behavior when a VPN connection is lost. The actual magic in this script was originally written by users on a [thread on the Synology Forum](https://community.synology.com/enu/forum/17/post/53791). This script just provides a user-friendly wrapper to their code. For more information, including installation instructions, refer to my [blog post](https://blog.harrier.us/reconnecting-a-failed-vpn-connection-on-synology-dsm-6/).

### Version History

- **1.1.0**: Extra customization options are included at the beginning of the script. Feel free to customize these to your liking.
- **1.2.0**: The following exit codes are used:
	- `0`: reconnect not needed
	- `1`: reconnect successful
	- `2`: reconnect failed
	- `3`: configuration error
- **1.3.0**: An option is added to allow pinging a custom IP address or hostname to validate VPN connectivity.
- **1.4.0**: An option is added to choose a specific VPN profile to reconnect, if multiple profiles exist. In this configuration, you could run multiple instances of this script, each targeting a specific VPN profile.
- **1.5.0**: Options are added to run external scripts at various points. Note that the scripts must be executable, and if there are spaces in the script paths, you must either escape the spaces (e.g. `NO_RECONNECT_SCRIPT=/volume1/Scripts/script\ with\ spaces.sh`) or wrap the script paths in quotes (e.g. `NO_RECONNECT_SCRIPT='/volume1/Scripts/script with spaces.sh'`). *Community-maintained scripts compatible with these features are included in the `reconnect-vpn.sh Community Scripts` directory.*


#### Installation

We will be generally following a Synology KB article to install the script as a scheduled task in DSM.

Open the Control Panel and navigate to System > Task Scheduler. Under Create > Scheduled Task, click User-defined script. This will open up a Create task window.

On the General tab, name the task something like Reconnect VPN for easy identification. Make sure the task is running as root and that the Enabled box is selected.

On the Schedule tab, make sure the Date option is set to Run on the following days: Daily. To schedule the script to run every 5 minutes, set the Frequency to Every 5 minute(s). Note that by default, the script will only run for the first 55 minutes of the day (00:00 to 00:55). To ensure the script will run every 5 minutes for the entire day, change the Last run time to 23:55.

On the Task Settings tab, enable Send run details by email and enter your email address in the Email box. I would recommend enabling the Send run details only when the script terminates abnormally option. The script was written using different exit codes, allowing it to work well with this option. If you do not enable this option, you will receive an email every time the script runs, whether there is an error or not.

On the bottom of the Task Settings tab, paste the entire script into the User-defined script box.

Finally, click the OK button to close the Create task window and then click the Save button to save the changes to the Task Scheduler.
