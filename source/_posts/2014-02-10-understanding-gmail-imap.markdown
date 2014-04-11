---
layout: post
title: "Understanding Gmail's IMAP Server"
date: 2014-02-10 08:00:00
comments: true
published: true
categories: [howto, IMAP, thunderbird, gmail, email]

---
I recently abandoned the Gmail web interface in favor of good old mail user agents (MUA) like Thunderbird, K-9 Mail and mutt. This has been a wonderful experience all around, but there were a few settings I had to make that were not intuitive until I understood how Gmail operates. Namely, how it treats IMAP folders.

<!-- more -->
## Folders vs. Labels

At some point during its lifetime, Gmail introduced the concept of labels for conversations. Labeling a conversation would make it show up in a separate inbox on the left-hand side of the web UI, and it's easy to believe that these labels map to the IMAP concept of folders. This is compounded by the fact that Gmail's IMAP Server presents labels in this way, and lets you subscribe/unsubscribe from them through the IMAP folder mechanism.

However, labels are not folders. In IMAP, a message lived in only one folder at a time, and it is either in the Inbox, the Sent Mail folder or your recipe collection, but never in all of them. Gmail groups messages in conversations, and a message is found in all folders that correspond to the labels on any of its messages. Meaning if you sent an email to Mom about her cobbler recipe, and labeled her response with your Recipes label, or maybe even starred it, too, then both messages are now in the following folders of the Web UI:

1. Inbox (because you received an email from Mom)
2. Sent Mail (because you sent at least one message in this conversation)
3. All Mail (Any and all messages go here)
4. Recipes (the label you chose)
5. Starred

This is key to understanding why Gmail seems to be acting strange when used from another MUA.

* Send a message, and Gmail will automatically label it to appear in the **Sent Mail** folder. Instructing Thunderbird to keep copies of sent mails on the server will create duplicates.
* To delete a conversation from a folder means to remove that label from it. The message is not physically deleted, and always remains in the **All Mail** folder.
* Gmail's Archive command simply removes the **INBOX** label from the conversation. On a normal server, it would move the archived message from the inbox to a folder named Archives, and Thunderbird et al are built around that idea of archiving. If you specify an IMAP Archive folder in Thunderbird, that will simply add another Gmail label.
* If you let Thunderbird copy anything to the **All Mail** folder, it may create another copy of the conversation?
* Since **All Mail** is not a folder, but just a label that every conversation automatically carries, it is absolutely the worst choice for an Archive location (although I've seen setup guides recommend it).


## Gmail's not Email

I've used Thunderbird in my examples because it is the most widely used desktop MUA, but these findings are true for every other mail program out there that implements the IMAP protocol. The problem is not with them, it's with Gmail implementing the protocol to describe something very different from what it was intended for.

This reminds me of a time long past. There was a period in the 90's when we had two classes of email programs. The ones that followed standards (mutt, pine, elm, Thunderbird), and the ones that tried to implement Internet Mail on top of existing corporate mail systems (Outlook, Lotus Notes). Usually, the latter had bad abstractions that made them do things like send Word documents, HTML messages or unrecognizable headers. Interestingly, Outlook Express looks like Microsoft's attempt to write a pure Internet Mail software that was unencumbered by corporate mail infrastructure like Exchange server or Lotus Notes.

The problem with all of these approaches is that they pretend to be something they are not, and they tried to jump onto a growth trend by subverting the standards. Remember how we all hated Microsoft for that? It looks as if Gmail is the Outlook of the 21st century, though. Piggy-backing on a standard that they only marginally support, just poorly enough to lure users in and then make them switch to their own implementation, which has its own restful API and is not handicapped by having to speak to the server in a language it only barely speaks, i.e. IMAP.

It's not the first time we see Google doing this. They have used the RSS to prop up Reader and Currents, and their XMPP implementation has gone from decent (Google Talk) to legacy support (Hangouts) just this past year. Just like they are about to finally flip the switch on that, I would not be surprised if IMAP support for Gmail is going to disappear in the near future, too.
   
