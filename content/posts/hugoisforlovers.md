+++
title = "Google Account Switching Trick" 
date = "2014-04-02"
+++

Switching Google accounts is a pain. If you're logged in with account _#1_ and then want to switch to account _#2_, you have to *drag* your cursor to the bottom right corner of the screen, *click* on your profile icon to bring up the list of profiles, and *then* click on the desired account. The whole process can take 5-10 seconds, which is quite a bit of friction when you want to jot down a quick note. It doesn’t help that many people have multiple Google accounts they need to be logged into.

 ![url.png](/google/url.png)
Fortunately, there is a simpler way to switch accounts. The account id is embedded in the browser URL, so you can just modify the id param to quickly access your desired account. Note that the value of the id is based on the order you logged in with.

In general, Google sites seem to use the the GET / POST param `authuser=n` to store the account id. However, as depicted in the screenshot above, there is a more compact syntax for some GSites.

A non-comprehensive list:
- docs.google.com/spreadsheets/u/n
- docs.google.com/forms/u/n
- docs.google.com/document/u/n
- https://docs.google.com/presentation/u/n
- https://mail.google.com/mail/u/0/
 - https://keep.google.com/u/1/
 - https://photos.google.com/u/1/
 - https://photos.google.com/u/1/
 - https://drive.google.com/drive/u/1/

It seems GProducts with their own subdomain offer the more compact syntax. This is not the case for sites which are google.com but have no associated subdomain e.g. Search, Maps. YouTube does not  use the `authuser=n` param at all (although YouTube is a bit of an oddball, in that you have channels, instead of accounts) ***....I should actually look at other GAcquisitions and see how they fare.***

~~This isn't a novel discovery, or so Google tells me: http://blog.metamatt.com/blog/2013/01/31/google-default-account-for-multiple-sign-in/.~~

But it *is* a neat workaround to a nitpick I've had for a while now. I hope the reader finds it useful as well.
