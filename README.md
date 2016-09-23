# ChatBottle API

## Overview

1. What is ChatBottle?
1. What is the Developer Platform?
1. Why to integrate my bot into ChatBottle?
1. Who can help me build an integration?
1. [ChatBottle Node.js client](https://github.com/chatbottle/chatbottle-api#nodejs-via-npm) for Facebook Messenger
1. [ChatBottle REST API for Facebook Messenger](https://github.com/chatbottle/chatbottle-api#http-rest-for-messenger)
1. [ChatBottle REST API for Telegram](https://github.com/chatbottle/chatbottle-api#http-rest-for-telegram)
1. [ChatBottle Generic REST API](https://github.com/chatbottle/chatbottle-api#generic-messenger-telegram-slack-etc) (Messenger, Slack, Kik, Telegram Skype)

### What is ChatBottle?
[ChatBottle](https://chatbottle.co/?ref=github) is a tool to discover the best chatbots. It's like Google for chatbots.

You probably know thousands of developers are working on bots right now. In a few months, the market will be crowded and it will be much more difficult to find a chatbot. ChatBottle is one more channel to drive traffic to your bot. Integrate your bots with the API to join the club.
Whenever a bot receives or sends a message ChatBottle is notified. It allows to gather the data and measure how people actually use the bot. 
Based on the collected data ChatBottle ranks the bots.

### What is the Developer Platform?
The ChatBottle Developer Platform is a way for anyone to integrate their bot to ChatBottle. There are two main reasons for integrating:

1. ChatBottle Search drives a high-quality audience to you bot.
2. So that you can learn how your bots are used, review analytics and analyze conversations.

Integrating with ChatBottle is easy and should take a developer about a few minutes. What you need to do is just to **forward every message your bot sent and received to ChatBottle REST API**.  

### Why to integrate with ChatBottle?

ChatBottle is one more customer acquisition channel to drive traffic to your bot. Integration with ChatBottle allows to get news users and reactivate existing ones.

Promoting a chatbot with ChatBottle is cheaper than advertising, content marketing or SEO. Once the bot is integrated it will be automatically ranked and listed in ChatBottle Search.

### How it works?
Your bot forwards every message it sent or received to the ChatBottle API. The messages are analyzed by our ranking algorithms and the bot is ranked. 
ChatBottle bot rank works similar to Google PageRank. It’s important how people interact with a chatbot, how often and how helpful is the bot.

ChatBottle ranks bots based on retention and other metrics. Having not many but addicted users is a good sign for ChatBottle algorithms. As a result, your bot is ranked higher and more people discover it.


### Who can help me?
If you have questions, you can always [email](mailto:agamanuk@gmail.com) or skype us(gamanuk_alexander)! If you found a bug feel free to [create an issue](https://github.com/chatbottle/chatbottle-api/issues). 

## Getting Started
This is a simplified walkthrough to see the platform in action. Read the [API Guide](https://github.com/chatbottle/chatbottle-api#api) to learn about the features in more detail. Use curl to run the following code snippets.

1. Register at [chatbottle.co](https://chatbottle.co);
2. Register an existing chat bost at the dashboard;
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
npm install --save chatbottle
`

#### Include chatbottle
```
var chatbottle = require('./chatbottle')(process.env.CHATBOTTLE_API_TOKEN, process.env.CHATBOTTLE_BOTID).facebook;
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
        const requestId = chatbottle.logOutgoing(requestData.json);
        request(requestData);
    }
    res.sendStatus(200);
});
```

_For complete example see: https://github.com/chatbottle/chatbottle-api-nodejs/blob/master/src/facebook-example.js_

-------

### HTTP REST for Messenger

Make a `POST` request to `https://api.chatbottle.co/v2/updates/messenger/{token}/` with your ChatBottle Bot Authorization token. Make sure to set the 'Content-Type' header to 'application/json'.

#### Incoming messages

`POST https://api.chatbottle.co/v2/updates/messenger/{chatbottle bot's token}/?direction=in`

#### Full INCOMING request example(copy, paste and execute it)
```
POST https://api.chatbottle.co/v2/updates/messenger/9eb21ef4607778142802f597b3b44a3c383847ae/?direction=in HTTP/1.1
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
#### Outgoing messages

`POST https://api.chatbottle.co/v2/updates/messenger/{chatbottle token}/?direction=out&token={your token}`

#### Full OUTGOING request example(copy, paste and execute it)
```
POST https://api.chatbottle.co/v2/updates/messenger/9eb21ef4607778142802f597b3b44a3c383847ae/?direction=out HTTP/1.1
Content-Type: application/json
Host: api.chatbottle.co
Content-Length: 107

{
  "recipient": {
    "id": "1604391129817458"
  },
  "message": {
    "text": "hello, user!"
  }
}
```

Note: _ChatBottle accepts any Facebook Messenger structured message. Just forward everything you send or receive from Facebook._

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
