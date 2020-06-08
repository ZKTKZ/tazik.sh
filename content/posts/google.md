+++
title = "📌 Google Account Switching Trick" 
date = "2020-04-02"
+++

Switching Google accounts is a pain. If you're logged in with account _#1_ and then want to switch to account _#2_, you have to *drag* your cursor to the bottom right corner of the screen, *click* on your profile icon to bring up the list of profiles, and *then* click on the desired account. The whole process can take 5-10 seconds, which is quite a bit of friction when you want to jot down a quick note. It doesn’t help that many people have multiple Google accounts they need to be logged into.

Fortunately, there is a simpler way to switch accounts [1]. The account id is embedded in the browser URL, so you can just modify the id param to quickly access your desired account. Note that the value of the id is based on the order you logged in with.

 ![url.png](/google/url.png)

In general, Google sites seem to use the the GET / POST param `authuser=n` to store the account id. However, as depicted in the screenshot above, there is a more compact syntax for some GSites.


It seems GProducts with their own subdomain offer the more compact syntax. This is not the case for sites which are google.com but have no associated subdomain e.g. Search, Maps. YouTube does not  use the `authuser=n` param at all.

A non-comprehensive list:
- https://docs.google.com/spreadsheets/u/n
- https://docs.google.com/forms/u/n
- https://docs.google.com/document/u/n
- https://docs.google.com/presentation/u/n
- https://mail.google.com/mail/u/0/
- https://keep.google.com/u/1/
- https://photos.google.com/u/1/
- https://photos.google.com/u/1/
- https://drive.google.com/drive/u/1/

