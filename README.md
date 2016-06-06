# ChatBottle API

## Overview

1. What is ChatBottle?
1. What is the Developer Platform?
1. Why to integrate my bot into ChatBottle?
1. Who can help me build an integration?

### What is ChatBottle?
[ChatBottle](https://chatbottle.co/?ref=github) is a tool to discover the best chatbots. The best means that people actually use it and the bots are really helpful. 
Whenever a bot receives or sends a message ChatBottle is notified. It allows to gather the data and measure how people actually use the bot. 
Based on the collected data ChatBottle rank the bots.

### What is the Developer Platform?
The ChatBottle Developer Platform is a way for anyone to integrate their bot to ChatBottle. There are two main reasons for integrating:

1. ChatBottle BotStore drives a high-quality audience to you bot.
2. So that you can learn how your bots are used, review analytics and setup metrics.

Integrating with ChatBottle is easy and should take a developer about a few minutes. What you need to do is just to forward sent and received messages to ChatBottle REST API.  

### Why to integrate with ChatBottle?
Thousands of people started developing bots after Messenger Platform was launched at F8. In a few months, hundreds of great bots will be released. For bots developers, it will be extremely hard to get people use a bot.

Integration with ChatBottle allows people discover your bot and drives traffic to the bot. 
Promoting a chatbot with ChatBottle is cheaper than advertising, content marketing or SEO. Once the bot is integrated it will be automatically ranked and listed in ChatBottle BotStore.


It’s important to integrate with ChatBottle as early as possible. Even before the official launch of a bot. The more data ChatBottle collects, the better our algorithms analyze your bot.

### How it works?
Your bot forwards every message it sent or received to ChatBottle API. The messages are analyzed by our ranking algorithms and the bot is ranked. 
ChatBottle bot rank works similar to Google PageRank. It’s important how people interact with a chatbot, how often and how helpful is the bot.

ChatBottle ranks bots based on retention and other metrics. Having not many but addicted users is a good sign for ChatBottle algorithms. As a result, your bot is ranked higher and more people find out about it.

### Who can help me?
If you have questions, you can always [contact us](mailto:agamanuk@gmail.com)! If you found a bug feel free to [create an issue](https://github.com/chatbottle/chatbottle.github.io/issues). 

## Getting Started
This is a simplified walkthrough to see the platform in action. Read the [API Guide](https://github.com/chatbottle/chatbottle.github.io#api) to learn about the features in more detail. Use curl to run tThe following code snippets.

1. Register at [chatbottle.co](chatbottle.co).
2. Create a bot at the dashboard
3. Enable chatbot platform. Messenger and Telegram are supported today.
4. Every time your bots sends or received a message call ChatBottle Updates API. Use generated Authorization token and ChatBottle Bot id to make calls like this:
```
curl -v http://api.chatbottle.co/v1/updates/573c704ef2104b11a8b910a2 \
-H "Authorization: 79bc1e9c1e152ddccace522b96649c6adea398b6" \
-H "Content-Type: application/json" \
-d '{
  "Platform": 2,
  "Messaging": [
    {
      "Id": "514829903",
      "Text": "Hello world!",
      "Recipient": {
        "Id": 1544489710
      },
      "Timestamp": 1464774483,
      "Date": "2016-05-26T11:41:49.1948699Z"
    }    
  ]
}
'
'
```


## API

### Schema
All API access is over HTTPS, and accessed from the `https://api.chatbottle.co`. All data is sent and received as JSON.
All timestamps are returned in ISO 8601 format:
```
YYYY-MM-DDTHH:MM:SSZ
```

### Root Endpoint
You can issue a GET request to the root endpoint to get all the endpoint categories that the API supports:
```
curl https://api.chatbottle.co
```
### Client Errors
There are three possible types of client errors on API calls that receive request bodies:
- Sending invalid JSON will result in a 400 Bad Request response.


### Authentication
There are three ways to authenticate through ChatBottle API. Requests that require authentication will return `403 Forbidden` and `401 Unauthorized.`
#### HMAC Authentication(sent in a header)
```
curl -H "Authorization: OAUTH-TOKEN" https://api.chatbottle.co

```

#### HMAC Authentication (sent as a parameter)
```
curl https://api.chatbottle.co/?token=OAUTH-TOKEN
```

This should only be used in server to server scenarios. Don't leak your authentification token to your users.

### Supported Chatbot platforms

There are a few predefined chatbot platforms in the system:
- Messenger = 1,
- Telegram = 2,
- Kik = 3, coming soon
- Slack = 4, coming soon

Use the string(case-insensitive) or the numeric value in your requests. If you're using any other platform please [let us know](mailto:agamanuk@gmail.com).

## Updates API
- To use updates API you need to know ChatBottle Bot Id. You can find it at the developers dashboard.
- Authorization token must be included in requests. Pass the token as `Authorization` header or query `token` parameter.


### Send an update  
`POST /updates/{bot-id}/`

Post a message or any other update to ChatBottle.
Make a `POST` request to `https://api.chatbottle.co/updates/{bot-id}/` with your ChatBottle Bot ID and Authorization token. In the body of the request, you must have the following data in a JSON format:

### Parameters 
|    Parameter     | Optional?                    | Description         |
 ----------------- | ---------------------------- | ------------------
| Platform         | no  			              | Chatbot platform. See the [list of supported platofrms](https://github.com/chatbottle/chatbottle.github.io#supported-chat-bot-platforms) for details   |
| Messaging        | no				              | A list of messages your bot sent or received |
| Messaging.Id     | yes					      | An external message id |
| Messaging.Text   | yes						  | A message text. Omitted for media messages    |
| Messaging.Recipient | no 					  | A bot id for incoming messages; A recipient id for outgoing messages |
| Messaging.Timestamp | no 					  | A message sent time 						  |


#### Full request
```
POST http://api.chatbottle.co/v1/updates/573c704ef2104b11a8b910a2/ HTTP/1.1
Content-Type: application/json
Host: api.chatbottle.co
Content-Length: 250
Authorization: 79bc1e9c1e152ddccace522b96649c6adea398b6

{
  "Platform": 2,
  "Messaging": [
    {
      "Id": "514829903",
      "Text": "Hello world!",
      "Recipient": {
        "Id": 1544489710
      },
      "Timestamp": 1464774483,
      "Date": "2016-05-26T11:41:49.1948699Z"
    }    
  ]
}
```

#### Response
```
HTTP/1.1 200 OK
```
