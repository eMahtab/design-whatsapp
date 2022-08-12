# Design WhatsApp

## Functional requirements
1. Text messaging (1 to 1)
2. Sent, Delivered and Read reciept
3. User A should be able to send message to user B, even if user B is currently offline, and the message should get delivered to user B when he comes online
4. Group Messages

## Non functional requirements
1. Highly available
2. Low latency


!["Whatsapp"](whatsapp.PNG?raw=true)


## Questions :
1. Should users periodically poll the chat server and check for any new messages

