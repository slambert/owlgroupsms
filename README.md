# Owl Group SMS Application

Below are the documented steps to configure the Group SMS application using the Twilio Console.

After is documentation for developers.

This application setup steps, and program, are variations of the Broadcast setup in the blog:
[How To: Set Up SMS Broadcasts in Five Minutes](https://www.twilio.com/blog/2017/12/how-to-set-up-sms-broadcasts-in-five-minutes.html)

## Requirements:

- Twilio account. An upgraded account is required.
- For developers, Node.JS installed to run the Group SMS development programs locally on your computer.
The Node.JS programs require the [Twilio Node.JS Helper Library](https://www.twilio.com/docs/libraries/node) is required.

## Configure Group SMS

1. Open a Twilio account to manage your Twilio resources.
**For Group SMS, you are required you to [upgrade your account](https://support.twilio.com/hc/en-us/articles/223183208-Upgrading-to-a-paid-Twilio-Account)**.

[https://www.twilio.com](https://www.twilio.com)

2. Search and buy an SMS capable phone number:

[https://www.twilio.com/console/phone-numbers/incoming](https://www.twilio.com/console/phone-numbers/incoming)

The Twilio Phone Number is for members to send commands and broadcast messages.
In the following steps, I use +12223331234 as the example phone number.

3. Create a Function. It will run the Group SMS application.

[https://www.twilio.com/console/functions/manage](https://www.twilio.com/console/functions/manage)
````
Click the big red plus `+` icon.
In the New Function popup, click Blank and click Create.
Set the Function name to: Group SMS
Set the /path to: /groupsms.
````
Copy and paste the code from this link: [groupSms.js](groupSms.js), into the Code text area.

Click Save.

Function screen shot:

<img src="ScreenShotFunction.jpg"/>

4. Create a Messaging Service under Programmable SMS > SMS.

In a new tab **(keep the Function tab open)**, go to:

[https://www.twilio.com/console/sms/services](https://www.twilio.com/console/sms/services)
````
Click the big blue plus `+` icon.
Friendly name: GroupSMS
Select: Notifications, 2-way
Click Create.
Under Inbound Settings, select the SEND AN INCOMING_MESSAGE WEBHOOK radio button
Go back to the Functions tab you left open and copy the PATH under Properties
    It should look like this: https://about-time-1235.twil.io/groupsms.
Under Request Url, paste your Functions URL (PATH)
Click Save.
On the left menu, click Numbers (Programmable SMS/ Messaging Services / GroupSMS / Numbers).
Click Add an Existing Number.
Click on your Twilio phone number, example: (222)333-1234.
Click Add Selected. Your number is now listed.
````
Note, the Twilio Copilot Messaging Service has attributes you can set to control the sending of messages. For Group SMS, use the defaults.

Messaging Service screen shot:

<img src="ScreenShotMS.jpg"/>

5. Create a Notify Service to broadcast group messages:

[https://www.twilio.com/console/notify/services](https://www.twilio.com/console/notify/services)
````
Click the big red plus `+` icon.
Set, Friendly name: GroupSMS
Click Create. The Notify service configuration page is displayed.
Select your Messaging Service SID: GroupSMS.
Click Save.
````
*Keep this tab open* because the Notify service SID is used when configuring the Group SMS Twilio Function.

Notify Service screen shot:

<img src="ScreenShotNS.jpg"/>

6. Create a Sync Service to manage the member data:

[https://www.twilio.com/console/sync/services](https://www.twilio.com/console/sync/services)
````
Click the big red plus `+` icon.
Set, Friendly name: GroupSMS
Click Create.
````
*Keep this tab open* because the Sync service SID is used when configuring the Group SMS Twilio Function.

Sync Service screen shot:

<img src="ScreenShotSS.jpg"/>

7. Configure the Function to use the Sync and Notify services:

[https://www.twilio.com/console/functions/configure](https://www.twilio.com/console/functions/configure)
````
Click/Enable ACCOUNT_SID and AUTH_TOKEN.
   This allows your Functions to use your account’s SID and auth token environment variables.
Do this for each key-value pair: click the create icon and add the following:
Key: NOTIFY_SERVICE_SID and value: your_notify_service_SID
Key: SYNC_SERVICE_SID and value: your_sync_service_SID
Click Save.
````

Sync Service screen shot:

<img src="ScreenPrintFunctionConfig.jpg"/>

## Documentation for Developers

### Files

Programs to manage Sync service Map items:
- [1initDeleteMap.js](1initDeleteMap.js) : Delete a specific Sync Map Item, which deletes all the related Items.
- [2initCreateMap.js](2initCreateMap.js) : Given a Sync service, create a Map.
- [3initSmsSend.js](3initSmsSend.js)     : Send the initialization SMS message, "!init Harry".
- [4retrieveMaps.js](4retrieveMaps.js)   : List Sync Maps.
- [5smsList.js](5smsList.js)             : List received SMS message from the group phone number.
- [6smsSend.js](6smsSend.js)             : Send an SMS message to the group phone number.
- [groupSms.js](groupSms.js)             : Group SMS development program.
- [groupSmsFunction.js](groupSmsFunction.js) : Group SMS Twilio Function code based on groupSms.js.
- [createMapItem.js](createMapItem.js)   : Given a Sync service and a Map, create a Map Item.
- [retrieveMapItemsEach.js](retrieveMapItemsEach.js)       : Retrieve Sync service Map Items.
- [retrieveMapItemsForEach.js](retrieveMapItemsForEach.js) : Retrieve Sync service Map Items, method used in groupSms.js.
- [updateMapItemAuthorize.js](updateMapItemAuthorize.js)   : Update a specific Sync Map Item authorization value.
- [updateMapItem.js](updateMapItem.js)   : Update a specific Sync Map Item data.
- [echoVars.js](echoVars.js)             : Echo environment variables that are used in these Node.JS programs.
- [setvars.sh](setvars.sh)               : Set program environment variables.

After creating your Sync Service, create environment variables for use in the above listed programs:
````
export ACCOUNT_SID=your_account_SID
export AUTH_TOKEN=your_account_auth_token
export SYNC_SERVICE_SID=your_sync_service_SID
export SYNC_MAP_NAME=counters
````
Note, you can use the shell script to maintain your variables: [setvars.sh](setvars.sh). Run:
````
$ source setvars.sh
````

Thank you for using Owl Group SMS.
````
Cheers,
Stacy David
````
