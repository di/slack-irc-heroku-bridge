# Slack/IRC Heroku Bridge

This project will allow you to deploy the
[aeirola/slack-irc-client](https://github.com/aeirola/slack-irc-client) project
to Heroku to act as an IRC bouncer and as a bridge between IRC and Slack.

This means that a) Your IRC username will always be online, and b) any messages
in the channels you join will be forwarded to Slack.

## Setup

1.  Create new Slack team at <https://slack.com/create>
    -  **Don't invite anyone else to the team**
1.  Rename the `#general` Slack channel to the IRC channel you wish to join.
    - Delete Slack channels that you don't want to join on IRC (like `#random`)
    - Add any other IRC channels you want to join as new Slack channels
1.  Edit your Slack user profile at <https://slack.com/account/profile>
    - Set the "What I do field" to JSON formatted string
    - For example, for Freenode, you will need the following:
      ```json
      {
        "server": "irc.freenode.net",
        "port": 6697,
        "userName": "NICKNAME",
        "nsPassword": "PASSWORD",
        "sasl": true,
        "nick": "NICKNAME",
        "password": "PASSWORD",
        "realName": "Your Real Name",
        "secure": true
      }
      ```
    - You may also want to ignore some common IRC actions. Add this to the JSON
      configuration:
      ```
        "muteIrcEvents": ["join", "part", "quit", "kick", "kill"]
      ```
1.  Clone this repository:
    ```
    $ git clone git@github.com:di/slack-irc-heroku-bridge.git
    ```
1.  Create a new Heroku app:
    ```
    $ heroku create <SOME APP NAME>
    ```
1.  Scale down the web dyno:
    ```
    $ heroku ps:scale web=0
    ```
1.  Generate a test token for your newly created team at <http://aeirola.github.io/slack-irc-client/>
1.  Set an environmental variable for the token you just created:
    ```
    $ heroku config:set SLACK_API_TOKEN=<YOUR GENERATED TOKEN>
    ```
1.  Push the app to heroku:
    ```
    git push heroku master
    ```
1.  Scale up a single worker dyno:
    ```
    $ heroku ps:scale worker=1
    ```

You should start receiving messages from Slackbot and in the Slack channels
you've created!

## Debugging

To debug, take a look at `heroku logs` to see if there's any connection issues.
