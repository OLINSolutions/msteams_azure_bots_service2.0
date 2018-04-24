# xMatters MSTeams integration - Azure Bots as Service - 1.0
Stand alone MSTeams bot hosted by Azure

<kbd>
  <img src="https://github.com/xmatters/xMatters-Labs/raw/master/media/disclaimer.png">
</kbd>

# Pre-Requisites
* Microsoft Azure Portal credentials.  Create Free credentials here (https://azure.microsoft.com/en-us/free/search/?&WT.srch=1&WT.mc_id=AID631184_SEM_oUHhx2Yc&lnkd=Google_Azure_Brand&gclid=EAIaIQobChMIld67-dHT2gIVgqDsCh2ZuQJDEAAYASAAEgI2tfD_BwE)
* Microsoft Teams Account.  Create Free trial here (https://teamsdemo.office.com/)
* Microsoft Teams App Downloaded.  Download App here (https://teams.microsoft.com/downloads)
* xMatters account - If you don't have one, [get one](https://www.xmatters.com)!

# Files
* [ExampleCommPlan.zip](ExampleCommPlan.zip) - This is an example comm plan to help get started. 

# How it works
Once the bot is running on Azure, you can add it to your MS Teams Team Channels.  Once added, within a channel you can interact with the bot by typing @botxmatters [command]

<kbd>
  <img src="https://github.com/xmatters/xMatters-Labs/raw/master/media/Working MS Teams.png">
</kbd>

# Installation
We will run through the following setup sequence:
1. Setup template Azure bots as a service in the Azure portal.
3. Test out the bot in the Web Chat & Chat Emulator.  Generic Command (no xMatters integration).
4. Test out the bot 1 on 1 in the MS Teams App.  Generic command (no xMatters integration).
5. Import the xMatters Communication Plan and configure.
6. Point the bot from step one to Github repo for xMatters bot.
7. Configure the bot with specific xMatters information.
8. Configure MS Teams Channel with connector and update xMatters configuration.
9. Test out the bot in the Web Chat & Chat Emulator.  xMatters commands.
10. Test out the bot 1 on 1 in the MS Teams app.  xMatters commands.
11. Use MS Teams App Studio in MS Teams to create an app manifest.  This manifest will be used to add the bot to Teams/channels.

## 1. Setup template Azure bot as service. 
New to Microsoft Bots?  The following are the references we will be utilizing to set everything up.
* Setup a sample bot with Bot Service (https://docs.microsoft.com/en-us/azure/bot-service/bot-service-quickstart)
* Test the sample Bot in Web Chat (https://docs.microsoft.com/en-us/azure/bot-service/bot-service-manage-test-webchat)
* Test the sample Bot in MS Teams with 1 on 1 chat. (https://docs.microsoft.com/en-us/microsoftteams/platform/concepts/bots/bots-test)
* In MS Teams use Teams App Studio to access your bot in Teams. Essentially, in Teams App Studio you will define a Manifest that points to the bot in step 1.  
https://docs.microsoft.com/en-us/microsoftteams/platform/get-started/get-started-app-studio
* A couple things of note:
The bot frameworkid is the same as the Microsoft AppID.  You will enter this in two places.  You can get the Microsoft AppID of the bot from the Azure console.
* Export the manifest and import it to a Team.  You will be able to @botname a command to bot in that team.
https://docs.microsoft.com/en-us/microsoftteams/platform/concepts/apps/apps-upload

1. Login to the Azure Portal and create a new resource.  Select AI - Cognitive Services - > Web App Bot

<kbd>
  <img src="https://github.com/xmatters/xMatters-Labs/raw/master/media/Web App Bot.png">
</kbd>

2. Enter the Web App Bot Information.  Click here to get a definition of the information required(https://docs.microsoft.com/en-us/azure/bot-service/bot-service-quickstart)

<kbd>
  <img src="https://github.com/xmatters/xMatters-Labs/raw/master/media/create web app bot.png">
</kbd>

3. Bot Template - Choose Node.js and Form.

<kbd>
  <img src="https://github.com/xmatters/xMatters-Labs/raw/master/media/bot template.png">
</kbd>

4. Click Create.  A number of new resources are created in the All resouces View.

<kbd>
  <img src="https://github.com/xmatters/xMatters-Labs/raw/master/media/create web app bot.png">
</kbd>

## Deploy app from Github repo to Azure
1. Fork the sample repo.

2. Login to Azure portal.  Create a new bot.  This will create a new app service. 
https://docs.microsoft.com/en-us/azure/bot-service/bot-service-quickstart

3. Follow directions below to setup continous deliver at the app service level.
https://docs.microsoft.com/en-us/azure/app-service/app-service-continuous-deployment#comments
https://docs.microsoft.com/en-us/azure/bot-service/bot-service-build-continuous-deployment

## Deploy bot from Github repo to Node.js server (Google Cloud)
1. Generate the MSTeams installation file by running "gulp" in the route directory.

2. Host the files in a location and run using the following commands:

```

npm install
npm start

```
3. or To deploy your own version run the following command:

	gcloud app deploy --version <your version name> --no-promote
	

## Specific AppId's, webhooks and credentials that need to be modified.

1. Replace all instances of "********-****-****-****-************" with your microsoftAppId.

```
.env:
line 3

manifest.json:
line 5,
line 42,
line 76

default.json:
line 3

```

2. Replace all instances of "-----------------------" with your microsoftAppPassword.

```
.env:
line 4

/config/default.json:
line 4

```

3. Replace all instances of "https://xmatters-url.com" with your xMatters URL.

```

/config/default.json:
line 7

```

4. Add your xMatters rest username and password to /config/default.json

5. update the integration codes in /config/default.json to match the inbound integrations in your xMatters instance.

```
Example:

https://xmatters-url.com/api/integration/1/functions/d0b41e9a-dc8b-4620-93ec-03e96f5cabf8/triggers

Would be "d0b41e9a-dc8b-4620-93ec-03e96f5cabf8"

```

## MS Teams set up

1. In MS Teams use Teams App Studio to access your bot in Teams. Essentially, in Teams App Studio you will define a Manifest that points to the bot in step 1.  
https://docs.microsoft.com/en-us/microsoftteams/platform/get-started/get-started-app-studio

A couple things of note:
The bot frameworkid is the same as the Microsoft AppID.  You will enter this in two places.  You can get the Microsoft AppID of the bot from the Azure console.

<kbd>
  <img src="need picture">
</kbd>

2.Export the manifest and import it to a Team.  You will be able to @botname a command to bot in that team.
https://docs.microsoft.com/en-us/microsoftteams/platform/concepts/apps/apps-upload

## xMatters set up

1. Import a communication plan (link: http://help.xmatters.com/OnDemand/xmodwelcome/communicationplanbuilder/exportcommplan.htm)

2. Create the "MSTeams" Endpoint and add the url for the hosted bot.

3. Create the "MSTeams path" Constant and add the value "/api/response".

# Testing
This integration has had some testing but more is required, to do this follow these steps in windows:

1. download the msteams botframework emulator

	https://github.com/Microsoft/BotFramework-Emulator/releases

2. Download the ngrok executable to your local machine.

	https://ngrok.com/

3. Open the emulator's App Settings dialog, enter the path to ngrok, select whether or not to bypass ngrok for local addresses, and click Save.

4. connect to the url (localhost if testing locally)

	https://hosted-site.com/api/messages

5. type in help to get the list of commands

# Development

To run a dev environment locally run:

```

npm run dev

```

This will automatically restart the app when any files are changed.




