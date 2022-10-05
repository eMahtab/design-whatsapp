# Design WhatsApp/Facebook Messanger/Signal

## Functional requirements
1. Text messaging (1 to 1)
2. Sent, Delivered and Read reciept
3. User A should be able to send message to user B, even if user B is currently offline, and the message should get delivered to user B when he comes online
4. Group Messages

## Non functional requirements
1. Highly available
2. Low latency

# Billions of users use Whatsapp daily and Whatsapp delivers billions of messages everyday.

!["Whatsapp"](design.PNG?raw=true)

# Approach :

1. **Polling** : Client makes a request at a regular interval specified asking if there are any new messages. Not all the time, chat server will have the new messages for the user, so this will result in lot of unnecessary calls, also it will drain the mobile battery and will waste the server resources by making lot of HTTP calls to the server. Also it will not result in real time message delivery, as the new messages will only be delivered when the user makes the HTTP call for that.

2. **Long Polling** : The server doesn't close the connection immediately, but keeps the connection opened for sometime, and if during this time user have any new messages, server will send those messages and close the connection.

3. **WebSockets** : A WebSocket is a persistent connection between a client and a server. WebSockets provide a bidirectional, full-duplex communications channel that operates over HTTP through a single TCP/IP socket connection. At its core, the WebSocket protocol facilitates message passing between a client and server. WebSockets are definitely the preferred choice for Real Time messaging app like WhatsApp.


## Where does Whatsapp stores the messages :
WhatsApp does not store your chat history on its servers. When you send a message, WhatsApp **only keeps that message on its servers until the recipient receives the message or for a max of 30 days**. Even then, the end-to-end encryption prevents WhatsApp from reading your message. 

The only time a message is stored on WhatsApp’s servers is if the recipient cannot receive it (maybe they’re offline or don’t have a WhatsApp account). In cases like those, the message(s) you sent are automatically deleted off of WhatsApp’s server in 30 days’ time. 

**Once Whatsapp recieves a delivery acknowledgement for a message, the message gets deleted from Whatsapp's database.**

**Sent, Delivered and Read Notification :**
For each message, there can be up to three callbacks from the WhatsApp's chat servers. These callbacks are sent, delivered and read notifications.

Suppose User1 sends a message to User2 and immediately gets offline, now in this case , the WhatsApp chat server won't be able to send the above notification to User1, when User2 recieves the message or reads the message. So we need to store this data in some persistent storage at least for time till the notifications are not sent to User1 successfully, and once the notifications are sent to User1 successfully and chat server gets the acknowledgement, we can delete the data from persistent storage.

# WhatsApp allows 256 members in a group.

# WhatsApp Chat Server :
Each chat server has around millions of users connected through Websocket connection. Also both sender and recipient might not be connected to the same chat server.
So there should be some central location which can tell, to which chat server , a particular user is connected to.

# Chat Server Local Cache :
Each chat server will have local cache, e.g. if chat server queries the central connection repository first time, we can cache it in local cache (with very short time to live) at Chat server. By doing this, for a conversation a Chat server will not have to go to Central connection repository each time.

# Offline User coming Online :
Suppose if User1 was offline, comes online, the chat server to which User1 gets connected, will call the Message Service to check if there are any new messages which needs to be sent to User1. If there are messages to be sent to User1, chat server will send those messages to User1. 


# Forward Media :
When you forward media, documents, locations or contacts, you don't have to upload them again. Any forwarded messages that weren't originally sent by you will display a "Forwarded" label.


# References :

1. https://www.youtube.com/watch?v=RjQjbJ2UJDg

2. https://www.youtube.com/watch?v=uzeJb7ZjoQ4
