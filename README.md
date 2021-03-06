# ![alt text](https://user-images.githubusercontent.com/22354631/72322510-82653b00-3674-11ea-9101-1b4f9d57cc8c.png "Plex-Discord Role Management Bot") Plex-Discord Role Management and Notifier Bot


## Pre-Installation Steps
1. Make sure you have Tautulli and Sonarr setup.
2. Get a Discord Bot Token. To do that go here: https://discordapp.com/developers/applications/me
    1. Log in or create an account
    2. Click **New Application**
    3. Fill in an Application Name and click create.
    4. This will bring you to the **General Information** section, enter in a description and upload an app icon if you want.
    5. Go to the **Bot** section in the settings sidebar. Next to **Build-A-Bot**, click the **Add Bot** button.
        * Scroll down to the **Privileged Gateway Intents** section.
        * Check the box for **SERVER MEMBERS INTENT**
        * Check the box for **PRESENCE INTENT** <- (***Optional Step!*** *The bot does not use this yet but if you enable it now and I use it later you won't have to worry.*)
        * Click **Save Changes** on the bottom of the page.
    5. At the top of the **Bot** section in the settings sidebar, click the button that says **Click to Reveal Token**. Copy this and keep it safe for later, this is your Bot Token.
3. Once you have created your bot, you'll need to authorize your bot on a server you have administrative access to. For documentation, you can read: https://discordapp.com/developers/docs/topics/oauth2#bots. The steps are as follows:
    1. Go to `https://discordapp.com/api/oauth2/authorize?client_id=[CLIENT_ID]&permissions=8&scope=bot` where [CLIENT_ID] is the Discord App Client ID found under the **OAuth2** section in the settings sidebar
    2. Select **Add a bot to a server** and select the server to add it to
    3. Click **Authorize**
    4. You should now see your bot in your server listed as *Offline*


## Important Note for existing users who upgrade from bot v1.x.x to v2.0.0 and higher.
With **discord.js v12**, privileged gateway intents for server members were introduced. What this means for you is that the @Watching role will no longer work until you edit your bot to have the server members intent. The instructions to do so are below:

1. Login to the [Discord Developer Portal](https://discord.com/developers/applications)
2. Go to **Applications** in the sidebar.
3. Find this bot and click on it.
4. Click on **Bot** under the settings sidebar.
5. Look for the **Privileged Gateway Intents** section.
6. Check the box for **SERVER MEMBERS INTENT**
7. Check the box for **PRESENCE INTENT** <- (***Optional Step!*** *The bot does not use this yet but if you enable it now and I use it later you won't have to worry.*)
8. Click **Save Changes** on the bottom of the page.
9. Restart the bot to ensure changes take effect.


## Important Note about Configuration
If you have changed your webroot of Sonarr or Tautulli, leave the `sonarr_port` and or `tautulli_port` fields blank. Specify the entire base URL in the ip section instead. So for example, if you normally access Tautulli at `http://192.168.0.101:8181/tautulli/home` instead of the default `http://192.168.0.101:8181/home`, enter `192.168.0.101:8181/tautulli` as the `tautulli_ip` and leave the `tautulli_port` field blank. Also, if you access Sonarr or Tautulli with TLS encryption, specify `https://` in the beginning of the ip field.


## Important Note about Images
If you want to have images included in your Discord notifications, image hosting must be enabled in Tautulli. Go to `Settings > 3rd Party APIs` and select an option from the `Image Host` drop down. 


## Normal Installation

1. Install Node.js: https://nodejs.org/
2. Clone the repo or download a zip and unpackage it.
3. Navigate to the root folder and in the console, type `npm install`
    * You should see packages beginning to install
4. Make a copy of `config/config.example.json` and save it as `config.json` in the same `config` directory.
4. Take your bot token from the Pre-Installation section and enter it into `config/config.json`.
5. Inside the `config/config.json` file, replace the placeholders with your Tautulli and Sonarr information. The `node_hook_ip` and `node_hook_port` are referring to the machine running this bot so tautulli knows where to send webhooks. The `node_hook_port` can be any available open port.
6. Once you have the config set up correctly, it is time to bring your bot *Online*. Navigate to the root of the app (where `index.js` is located) and in your console, type `node index.js`
    * This will start your server. The console will need to be running for the bot to run.

If I am missing any steps, feel free to reach out or open an issue/bug in the Issues for this repository.


## Docker Installation

```
docker run -d \
  --name='plex_discord_role_management' \
  -e TZ=<timezone> \
  -e 'botToken'='YOUR_DISCORD_BOT_TOKEN' \
  -e 'defaultPrefix'='!' \
  -e 'node_hook_ip'='THE_IP_ADDRESS_OF_THE_HOST_MACHINE' \
  -e 'node_hook_port'='YOUR_HOST_PORT' \
  -e 'tautulli_ip'='YOUR_TAUTULLI_IP_ADDRESS' \
  -e 'tautulli_port'='YOUR_TAUTULLI_PORT' \
  -e 'tautulli_api_key'='YOUR_TAUTULLI_API_KEY' \
  -e 'sonarr_ip'='YOUR_SONARR_IP_ADDRESS' \
  -e 'sonarr_port'='YOUR_SONARR_PORT' \
  -e 'sonarr_api_key'='YOUR_SONARR_API_KEY' \
  -e 'sonarr_ip_2'='OPTIONAL_ADDITIONAL_SONARR_IP_ADDRESS' \
  -e 'sonarr_port_2'='OPTIONAL_ADDITIONAL_SONARR_PORT' \
  -e 'sonarr_api_key_2'='OPTIONAL_ADDITIONAL_SONARR_API_KEY' \
  -e 'sonarr_ip_3'='OPTIONAL_ADDITIONAL_SONARR_IP_ADDRESS' \
  -e 'sonarr_port_3'='OPTIONAL_ADDITIONAL_SONARR_PORT' \
  -e 'sonarr_api_key_3'='OPTIONAL_ADDITIONAL_SONARR_API_KEY' \
  -e 'DEBUG_MODE'='0' \
  -p 'YOUR_HOST_PORT:3000/tcp' \
  -v '/PATH/TO/YOUR/HOST/APPDATA':'/app/config':'rw'   \
  cyaondanet/plex_discord_role_management:latest
```

***

## Usage

1. This bot allows a mentionable Role to be set as a watching role for users. It gets auto-assigned during playback and removed when playback ends. This is useful for other notifications, like if you need to reboot the Plex server you can ping @watching so they know it's not a problem on their end. To use this feature, the Discord account must be linked to the plex username with `!link @DiscordUser PlexUsername`. 

2. The second usage for this bot is getting relevant recently added notifications. All recently added movies and TV shows are filtered through the bot to add their respective @mentions and then sent in the specified channel. A user chooses their respective notification settings by clicking on emojis and receiving a react-role from the bot. A library can be excluded with the `!notifications library` command, like with a 4k library that most or all of the other users don't have access to.

***
## Getting Started

1. After all cofiguration settings have been applied, the bot has been invited to the server, and the bot is online with no errors in the console; you are ready to configure its discord settings. First off, if you want to change the prefix, do it with `!bot prefix newprefix` where newprefix is the actual new prefix.

2. In some sort of Admin only channel, do all the following stuff until the last step. We want to run `!link @DiscordUser PlexUsername` for every user in the server, you can verify the settings with `!linklist` and use `!unlink @DiscordUser` to unlink someone. If you don't know the exact spelling of someone's plex username, the command `!users` will pull a list for you. Now create a Watching Role in Discord and make sure the Mentionable box is ticked. Run this command with the Role you just created `!role @WatchingRole`. If you want to see if its working, run `!notify` and setup the logchannel to get a log of the auto-assigning of the role. Run `!notify` again if you want to disable logging.

3. Run the `!notifications edit` command to setup some notification preferences now. If you have any custom Discord Roles you would like to be React-Roles (so users can opt in and out of them) use the `!notifications custom add @mentionedRole Optional Description` command. Run `!notifications library` to exclude any Plex libraries from recently added notifications.

4. Test the Sonarr connection and generate the database with `!notifications preview`. A database with all shows on Sonarr was just created but only continuing (airing) shows automatically received a discord Role. Now you can edit this list with `!notifications exclude show` or `!notifications include show`. You can even group or ungroup common shows with `!notifications group New Group Name for Shows [show1] [show2] [etc.]` and `!notifications ungroup [show1] [show2] [etc.]`. You can re-call `!notifications preview` to see your changes and once you are satisfied with this list you can continue onto the final step.

5. Finally, you should setup a `#notifications` channel and a `#notifications_settings` channel in Discord. In the channel permissions, non-Admin users should be able to read these messages but not be able to send anything. Also, default server notification settings should be set to @mentions only so people are not spammed by `#notifications` but rather only get notifications on things they want. Use `!notify` in the desired `#notifications` channel and select the emoji option corresponding with `Content Notifications` to set the bots recently added notification alerts to go to the current channel. Head over to `#notification_settings` channel and type `!notifications list` to generate all the React-Role embeds. You are now done, and people can receive show specific notifications in your Discord Server!

***

## Discord Bot Commands
-  `!help` : Lists information about commands.
-  `!help [command name]` : Lists information about a specific command.
-  `!bot [subcommand]` : Various bot commands
      - `!bot info` : Lists current info like logging channel, recently added channel, etc.
      - `!bot prefix newprefix` : Allows you to change the bot prefix
      - `!bot recentlyadded on/off` : Allows you to set whether recently added shows automatically receive a discord role with role react page displayed in the same channel as `!notifications list`.
-  `!notify` : Different notification options that can be enabled in the channel that it was called in.
-  `!showlist` : Lists all the shows on Sonarr that are still marked as continuing.
-  `!link @DiscordUser PlexUsername` : Links a Discord User Tag with their respective Plex username
-  `!unlink @DiscordUser` : Unlinks a Discord User Tag with a Plex username
-  `!linklist` : Shows a list of all linked Plex-Discord Users
-  `!users` : Lists all Plex usernames that have shared access to the Server, to be used to easily call the `!link @DiscordUser PlexUsername` command.
-  `!notifications [subcommand]` : 
      - `!notifications edit` : Allows you to edit the page 1 react role options.
      - `!notifications custom add @mentionedRole Optional Description` : Allows you to add up to 6 custom React-Roles that can be used on page 1 of the `!notifications list` 
      - `!notifications custom remove` : Allows you to remove a custom React-Role
      - `!notifications library` : Allows you to include or exclude a library from sending recently added notifications. Ex. To exclude a 4k Movie Library from pinging in the `!notifications channel`
      - `!notifications exclude show` : Excludes a show from getting it's own Role
      - `!notifications include show` : Includes a show in getting it's own Role
      - `!notifications group New Group Name for Shows [show1] [show2] [etc.]` : Groups shows as one React-Role
      - `!notifications ungroup [show1] [show2] [etc.]` : Ungroups previously grouped shows.
      - `!notifications list` : Lists the react-role embeds to be used for role specified notifications. Should be called in its own channel that others can view but not send in. For now, it needs to be recalled to reflect new changes.
      - `!notifications preview` : Same as `!notifications list` but does not store embeds in database for emoji recheck on start. Should be used when experimenting in a different channel than the primary `#notification_settings`.
-  `!role @WatchingRole` : Assigns the Watching Role that the bot assigns to Users when they are watching Plex. *NOTE: The Bot's Role needs to be higher than the Watching Role*
-  `!delete` : Deletes all Discord Roles managed by this bot, to be used prior to removing the bot from Server for easy cleanup.

***

## To Do:
* [ ] Add quiet hours setting as discord server setting. Allows for caching of messages during certain hours and then sending out those messages after quiet hours are over.
* [ ] Add inactive plex user role/notification. Set an inacticve length in a discord server (like say 1 month). Take Tautulli user data and whenever a user hasn't watched anything in the set period of time, give them an inactive role and potentially a notification to specified channel. This allows the server owner to easily identify inactive users.