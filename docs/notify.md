# Setup for sending CYLC error notifications to Slack channel

Error notifications from model runs (task failures etc) can be sent to a designated Slack channel - this allows more rapid response to model failures than periodic checking of the Cylc tui/gui.

There are three steps to setting this up

## Turning on notifications in a Rose suite <suite-id>

In `~/rose/<suite-id>/flow.cylc` under `[[[events]]]` in the `[runtime][[root]]` section add the follwing two lines:

~~~
           handlers = notify.py
           handler events = submission retry, submission failed, submission timeout, retry, failed
~~~


So `[[[events]]]` should look something like:

~~~
        [[[events]]]
            # Events under individual tasks are unchanged in syntax.
      	    # mail events, submission timeout, execution timeout all still valid.
            handlers = notify.py
            handler events = submission retry, submission failed, submission timeout, retry, failed
~~~

Then do

~~~
cp /home4/home/n02-puma/lrdlrh/python/notify.py ~/roses/<suite-id>/bin/
~~~

Then reload the cylc suite: `cylc vr <suite-id>`
and commit the changes to the repository:

~~~
cd ~/roses/<suite-id>
fcm add bin/notify.py
fcm commit
~~~

## Creating a new Slack channel and Slack hook

Next, create a new private channel on Slack by clicking the + button by the Channels heading. Give it a name e.g. `TIPMIP errors`. Don't add anyone else to it yet.

Go to the [Slack App page](https://docs.slack.dev/messaging/sending-messages-using-incoming-webhooks/) and click on the Create an APP button.

Select `From scratch` in the Create an app dialogue box.

Give the App a name (e.g TIPMIP monitor) and select the NCAS workspace from the `Pick a workspace to develop in` dropdown box.

Click `Create App`

Then, select `Incoming Webhooks` under `Features` on the menu on the left hand side.

Toggle the `Activate Incoming Webhooks` slider to `ON`.

Click `Add New Webhook`

In the `Channel for webhook` box, search for the channel you created earlier (e.g `TIPMIP errors`)

Click `Allow`
This will return a page with the `Webhook URL` at the botton of the page (<SLACK-HOOK-URL>) 

Click `Copy` to copy this webhook URL.

You can test this webhook by copying the string in the box below `Sample curl request to post to a channel` and pasting it into a `puma2` terminal window
You should get a Slack notification in your new channel

## Adding the Slack hook to

Back on `puma2`, create a new directory `mkdir ~/.notify`

Then do:

~~~
cd ~/.notify
echo "<SLACK-HOOK-URL>" >webhook
chmod og-rwx webhook
~~~

Where <SLACK-HOOK-URL> is the Webhook URL from the previous section.


Now, any error in the rose suite should be reported directly to the new Slack channel you created.


