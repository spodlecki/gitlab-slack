**gitlab-slack** is a service that receives outgoing webhook notifications from GitLab and posts information to an incoming webhook on Slack.
# Information
**gitlab-slack** processes GitLab webhook messages for **commits**, **branches**, **tags** and **issues**. **issue** messages
with an `action` of `update` are ignored.

Status and error messages are logged to `gitlab-slack.log` in the application directory.

### Configuration Syntax
**gitlab-slack** is configured by values in the `config.json` file. This file is expected to be in the application
directory. The configuration file has the following keys:

* `slack_webhook_url` - The URL of the Slack incoming webhook.
* `gitlab_api_base_url` - The GitLab API base URL.
* `gitlab_api_token` - The GitLab API token to use for GitLab API requests.
* `port` - The port on which to listen.
* `project_channel_map` - An object containing a mapping from GitLab project ID to Slack channel name. The channel name
						  must be preceded by an octothorpe (#).

Additional keys can be added for documentation or other purposes, but they will be ignored by the application.

# Installation
> _**nodejs** and **npm** are prerequisites to the installation of this application._

1. `cd /opt`
1. `git clone <repository-url> gitlab-slack`    
    Clone the **gitlab-slack** repository into the `/opt` directory.
1. `cd gitlab-slack`
1. `npm install`    
    Install the **nodejs** dependencies.
1. `cp scripts/init.d/gitlab-slack /etc/init.d`    
   Copy the service script to the `/etc/init.d` directory.
1. `chmod 755 /etc/init.d/gitlab-slack`    
   Allow the service script to run.
1. `cp _config.json config.json`
   Copy over the configuration for your slack
1. `update-rc.d gitlab-slack start 70 2 3 4 5 . stop 40 0 1 6 .`    
   On Ubuntu, set the start and stop priorities for the service script.
1. `service gitlab-slack start`

# Adding the GitLab Webhook
> _The **Master** or **Owner** permission level is required to modify webhooks in GitLab._

1. From the project home, click **Settings**.
1. Click **Web Hooks**.
1. If **gitlab-slack** is running on the same server as GitLab, enter `http://127.0.0.1:PORT` into the **URL** field.    
   Use the value of the `port` key from the `config.json` file in place of `PORT`.
1. If **gitlab-slack** is running on another server, enter the appropriate DNS or URI.
1. Check the **Push events**, **Tag push events** and **Issues events** options. **Merge Request events** are not supported.
1. Click **Add Web Hook**.

Once added, the webhook can be tested using the **Test Hook** button.
