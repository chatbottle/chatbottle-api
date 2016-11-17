# ChatBottle API

## Overview

1. Who can help me build an integration?
1. [ChatBottle Messages API](https://github.com/chatbottle/chatbottle-api#messages-api)
1. [ChatBottle Users API] (https://github.com/chatbottle/chatbottle-api#users-api)

### Who can help me?
If you have questions, you can always [email](mailto:agamanuk@gmail.com) or skype us(gamanuk_alexander)! If you found a bug feel free to [create an issue](https://github.com/chatbottle/chatbottle-api/issues). 

## Getting Started
This is a simplified walkthrough to see the platform in action. Read the [API Guide](https://github.com/chatbottle/chatbottle-api#api) to learn about the features in more detail.

1. Register on [ChatMetrics](https://chatmetrics.io/) using Facebook or Google account;
2. Register a bot;
3. Every time your bots send or receive a message call ChatBottle API. Use bot's unique generated **Authorization token** to make calls.

## API

### Schema
All API access is over HTTPS, and accessed from the `https://api.chatbottle.co`. All data is sent and received as JSON.


## Messages API
- To use the **Messages API** you need to know your **ChatBottle bot's authorization token**. You can find it at the developers dashboard.

### Node.js ChatBottle client for Facebook Messenger 

#### Node.js via NPM

#### Install NPM
ChatBottle is avialable via NPM

`
npm install --save chatbottle-api-nodejs
`

#### Include chatbottle
```
var chatbottle = require('./chatbottle-api-nodejs')(process.env.CHATBOTTLE_API_TOKEN).facebook;
```

#### Log whenever your webhook is called
```
app.post(webHookPath, function (req, res) {
    chatbottle.logIncoming(req.body);
    const messagingEvents = req.body.entry[0].messaging;
    if (messagingEvents.length && messagingEvents[0].message && messagingEvents[0].message.text) {
        const event = req.body.entry[0].messaging[0];
        const sender = event.sender.id;
        const text = event.message.text;
        const requestData = {
            url: 'https://graph.facebook.com/v2.6/me/messages',
            qs: { access_token: process.env.FACEBOOK_PAGE_TOKEN },
            method: 'POST',
            json: {
                recipient: { id: sender },
                message: {
                    text: 'ECHO: ' + text
                }
            }
        };
        request(requestData);
    }
    res.sendStatus(200);
});
```

_For complete example see: https://github.com/chatbottle/chatbottle-api-nodejs/blob/master/src/facebook-example.js_

-------

### HTTP REST for Messenger

Make a `POST` request to `https://api.chatbottle.co/v2/updates/messenger/{token}/` with your ChatBottle Bot Authorization token. Make sure to set the 'Content-Type' header to 'application/json'.

#### 1. Subscribe to messages, message_echoes and message_reads fields.

Go to your bot's webhook settings and edit Page Subscription Fields. 
Subscribe to messages, message_echoes and message_reads fields

- Go to https://developers.facebook.com/apps/
- Click to the bot you're connecting
- Open Webhooks tab.

Note: Your bot can be subscribed to other fields as well. ChatBottle requires only messages, message_echoes and message_reads fields.


![Subscibe to Facebook Messenger fields and forward to ChatBottle](http://photos.chatbottle.co/static/fb-subscription-fields.png)

#### 2. Send message_echoes, message_reads and incoming messages to ChatBottle API
Send the json content of message_echoes, message_reads and incoming messages received from Messenger to ChatBottle via the API. 
Learn more: 

https://developers.facebook.com/docs/messenger-platform/webhook-reference/message-echo 
https://developers.facebook.com/docs/messenger-platform/webhook-reference/message-received 
https://developers.facebook.com/docs/messenger-platform/webhook-reference/message-read

The example bellow is for incoming messages. Do the same for echo and read messages as well. 
Just forward to us everything you get from Facebook.

##### Full INCOMING, ECHO and READ request example(copy, paste and execute it)
```
POST https://api.chatbottle.co/v2/updates/messenger/9eb21ef4607778142802f597b3b44a3c383847ae/ HTTP/1.1
Content-Type: application/json
Host: api.chatbottle.co
Content-Length: 484

{
  "object": "page",
  "entry": [
    {
      "id": "1604391129817458",
      "time": 1469201060128,
      "messaging": [
        {
          "recipient": {
            "id": "1604391129817458"
          },
          "sender": {
            "id": "802455339884571"
          },
          "message": {
            "mid": "mid.1469201060037:8d651aa4d006895998",
            "text": "hello, bot!",
            "seq": 5466
          }
        }
      ]
    }
  ]
}
```

_NOTE: Outgoing messages are tracked using Facebook Messenger Echo messages._

### HTTP REST for Telegram

Make a `POST` request to `https://api.chatbottle.co/v2/updates/telegram/{token}/` with your ChatBottle Bot Authorization token. Make sure to set the 'Content-Type' header to 'application/json'.

#### Incoming messages

`POST https://api.chatbottle.co/v2/updates/telegram/{token}/?direction=in`

ChatBottle expects an [Update](https://core.telegram.org/bots/api#update) object as a body JSON.
Just send it to ChatBottle when receive an update from Telegram.


#### Full INCOMING request example(copy, paste and execute it)
```
POST https://api.chatbottle.co/v2/updates/telegram/79bc1e9c1e152ddccace522b96649c6adea398b6/?direction=in HTTP/1.1
Content-Type: application/json
Host: api.chatbottle.co
Content-Length: 413

{
  "update_id": 123456789,
  "message": {
    "message_id": 1,
    "from": {
      "id": 1234567890,
      "first_name": "John",
      "last_name": "Doe",
      "username": "JohnDoe"
    },
    "chat": {
      "id": 1234567890,
      "first_name": "John",
      "last_name": "Doe",
      "username": "JohnDoe",
      "type": "private"
    },
    "date": 1459957719,
    "text": "/start"
  }
}
```
#### Outgoing messages
`POST https://api.chatbottle.co/v2/updates/messenger/{token}/?direction=out`

ChatBottle expects a [Message](https://core.telegram.org/bots/api#message) object as a body JSON.
Send to us the response message object after executing [sendMessage](https://telegram-bot-sdk.readme.io/docs/sendmessage) or any other `send` method.


#### Full OUTGOING request example(copy, paste and execute it)
```
POST https://api.chatbottle.co/v2/updates/telegram/79bc1e9c1e152ddccace522b96649c6adea398b6/?direction=out HTTP/1.1
Content-Type: application/json
Host: api.chatbottle.co
Content-Length: 107

{
  "message_id": 1,
  "from": {
    "id": 234503788,
    "first_name": "YourBot",
    "username": "YourBot"
  },
  "chat": {
    "id": 1234567890,
    "first_name": "John",
    "last_name": "Doe",
    "username": "JohnDoe",
    "type": "private"
  },
  "date": 1459958199,
  "text": "Hello from Bot!"
}
```

Note: _ChatBottle accepts any Telegram structured message. Just forward everything you send or receive from Telegram._

------
### Generic (Messenger, Telegram, Slack, etc) 

Make a `POST` request to `https://api.chatbottle.co/v2/updates/{token}/` with your ChatBottle bot's unique authorization token. Make sure to set the 'Content-Type' header to 'application/json'.

`POST https://api.chatbottle.co/v2/updates/{token}/`
Request body
```
{
  "Messaging": [
    {
      "Id": "712329970",
      "Text": "Hello world!",
      "UserId": "1646261728",      
      "Direction": "Out" 
    }    
  ]
}
```

### Parameters 
|    Parameter     | Optional?                    | Description         |
 ----------------- | ---------------------------- | ------------------
| Messaging        | no				              | A list of messages your bot sent or received |
| Messaging.Id     | no					      | An external message id |
| Messaging.Text   | yes						  | A message text. Omitted for media messages    |
| Messaging.UserId | no 				     	  | An id of the user who sent or received a message from the bot |
| Messaging.Direction | no 						  | **In** for incoming and **Out** for outgoing messages|


#### Full request example(copy, paste and execute it)
```
POST https://api.chatbottle.co/v2/updates/9eb21ef4607778142802f597b3b44a3c383847ae/ HTTP/1.1
Content-Type: application/json
Host: api.chatbottle.co
Content-Length: 250

{
  "Messaging": [
    {
      "Id": "712329970",
      "Text": "Hello world!",
      "UserId": "1646261728",      
      "Direction": "Out" 
    }    
  ]
}
```

#### Response
```
HTTP/1.1 200 OK
Content-Length: 16
Content-Type: application/json

{"success":true}
```

## Users API
### Add users
_ATTENTION! POST overrides an existing user based on userId. Use it only for importing._

```
POST http://api.chatbottle.co/v2/bots/{chatbottle bot token}/users/ HTTP/1.1
Content-Type: application/json
Host: api.chatbottle.co
Content-Length: 250

[
  {
    "id": "802455339884571",
    "sessions": 71,
    "lastSessionTime": 1473861623047,
    "attributes": {
      "firstName": "John",
      "lastName": "Doe",
      "gender": 1,
      "timezone": -8,
      "locale": "en-US",
      "profilePic": "https://lh4.googleusercontent.com/-62fqwmOS7-U/AAAAAAAAAAI/AAAAAAAAHbo/BffLhPbHB5k/photo.jpg?sz=50"
    },
    "customAttributes": {
      "myCustomField": "field value",
      "age": 29,
      "isImported": true,
      "email": "john@chatbottle.co",
      "tags": [
        "visitor",
        "prospect"
      ]
    }
  }
]
```

Parameters

| Name                  | Type   | Optional | Description                                                                     |
|-----------------------|--------|----------|---------------------------------------------------------------------------------|
| id                    | string | no       | The bot user id for the current platform (Messenger, Telegram, Kik, etc)        |
| sessions              | long   | yes      | The number of session between the user and the bot                              |
| lastSessionTime       | long   | yes      | Last time the user had session with the bot                                     |
| attributes.firstName  | string | yes      | First name of the user                                                          |
| attributes.lastName   | string | yes      | Last name of the user                                                           |
| attributes.gender     | int    | yes      | Gender of the user. use 1 for male, 2 for female and leave it blank if unknown. |
| attributes.timezone   | int    | yes      | The user's timezone                                                             |
| attributes.locale     | string | yes      | The user's locale                                                               |
| attributes.profilePic | string | yes      | The url to the user's picture                                                   |
| customAttributes      | object | yes      | The object with user defined data. See the request JSON for more details.       |


#### Response
```
HTTP/1.1 200 OK
Content-Type: application/json

{
  "imported": 245225  
}
```

### Update user

```
PUT http://api.chatbottle.co/v2/bots/{chatbottle bot token}/users/{user id} HTTP/1.1
Content-Type: application/json

{
  "customAttributes": {
    "age": 28 
  }
}
```
 
#### Response
```
HTTP/1.1 200 OK
Content-Type: application/json 

{
  "CustomAttributes": {
    "age": 291
  }
}
```

### List users
List all users the bot had conversations with.

```
GET http://api.chatbottle.co/v2/bots/264219e7d1c9276d0bead94e40eabe78c0ecd7ef/users/ HTTP/1.1
```

Parameters

| Name   | Type   | Description                                                                           |
|--------|--------|---------------------------------------------------------------------------------------|
| offset | number | The offset parameter defines the offset from the first result you want to fetch.      |
| size   | number | The size parameter allows you to configure the maximum amount of hits to be returned. |

#### Response
```
HTTP/1.1 200 OK
Content-Type: application/json

{
  "Total": 2,
  "From": 0,
  "Size": 1000,
  "Items": [
    {
      "Sessions": 9,
      "LastSession": "2016-10-16T11:21:08.5748723Z",
      "Id": "1604391129817458",
      "Created": "2016-09-17T10:27:58.8343145Z"
    },
    {
      "Sessions": 116,
      "LastSession": "2016-10-25T12:48:21.5421458Z",
      "LastSessionTime": 1477561370618,
      "Id": "802455339884571",
      "Created": "2016-09-14T14:00:23.0630373Z"
    }
  ]
}
```

### Delete users

```
DELETE http://api.chatbottle.co/v2/bots/{chatbottle bot token}/users/{user ids} HTTP/1.1
```

Parameters

| Name   | Type   | Description                                                                           |
|--------|--------|---------------------------------------------------------------------------------------|
| user ids | string | The comma-separated list of platform (Messenger, Telegram, Kik) user ids you want to delete.     |

#### Response
```
HTTP/1.1 200 OK
Content-Type: application/json

{
  "removed": [
     "802455339884571",
     "802455573845370"
    }
  ]
}
```
