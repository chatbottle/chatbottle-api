# ChatBottle API

## Overview

1. What is ChatBottle?
1. What is the Developer Platform?
1. Why to integrate my bot into ChatBottle?
1. Who can help me build an integration?

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
3. Every time your bots send or receive a message call ChatBottle API. Use generated **Authorization token** and **ChatBottle Bot Id** to make calls.

## API

### Schema
All API access is over HTTPS, and accessed from the `https://api.chatbottle.co`. All data is sent and received as JSON.

### Authentication
There are three ways to authenticate through ChatBottle API:

#### 1. Send an authorization token as a HTTP Authorization header
```
curl -H "Authorization: OAUTH-TOKEN" https://api.chatbottle.co

```
or
#### 2. Send an authorization token as a token parameter
```
curl https://api.chatbottle.co/?token=OAUTH-TOKEN
```

This should only be used in server to server scenarios. Don't leak your authentification token to your users.

## Messages API
- To use the **Messages API** you need to know your **ChatBottle Bot Id** and **ChatBottle authorization token**. You can find it at the developers dashboard.
- Pass your authorization token as `Authorization` header or query `token` parameter.

### Facebook Messenger
Make a `POST` request to `https://api.chatbottle.co/updates/messenger/{bot-id}/` with your ChatBottle Bot ID and Authorization token. 

#### Incoming messages
Make sure to set the 'Content-Type' header to 'application/json'
`POST https://api.chatbottle.co/v1/updates/messenger/**{bot-id}**/?direction=**in**&token=**{your token}**`

**Request body**
```
POST http://dev.api.chatbottle.co/v1/updates/messenger/573eb74ff210400438505235/?direction=in HTTP/1.1
Content-Type: application/json
Host: dev.api.chatbottle.co
Content-Length: 484
Authorization: 79bc1e9c1e152ddccace522b96649c6adea398b6

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

`POST https://api.chatbottle.co/v1/updates/messenger/**{bot-id}**/?direction=**out**&token=**{your token}**`

**Request body**
```
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

#### Full request example(copy, paste and execute it)
```
POST https://api.chatbottle.co/v1/updates/573eb74ff210400438505235/ HTTP/1.1
Content-Type: application/json
Host: api.chatbottle.co
Content-Length: 250
Authorization: 79bc1e9c1e152ddccace522b96649c6adea398b6

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

------
### Telegram 

Make a `POST` request to `https://api.chatbottle.co/updates/{bot-id}/` with your ChatBottle Bot ID and Authorization token. Make sure to set the 'Content-Type' header to 'application/json'.

`POST https://api.chatbottle.co/v1/updates/**{bot-id}**/?token=**{your token}**`
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
POST https://api.chatbottle.co/v1/updates/573eb74ff210400438505235/ HTTP/1.1
Content-Type: application/json
Host: api.chatbottle.co
Content-Length: 250
Authorization: 79bc1e9c1e152ddccace522b96649c6adea398b6

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
