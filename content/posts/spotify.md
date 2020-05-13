
+++
title = "🎶 Optimizing Spotify Free"
date = "2020-01-02"
+++

The free version of Spotify Mobile is rather limited -- one has restricted access to tracks, cannot move to an arbitrary point in a track etc.. I test how much I can optimize my Spotify experience without signing up for Premium or using a cracked version of the service.

## Playlists 🧬🐑
Spotify Mobile only allows tracks to be played as part of a larger playlist of songs. I made a playlist with just one track, to see how Shuffle Play would deal with it. Spotify automatically added “suggested songs” to my playlist. (*happens when you are on mobile.) [1] 

![spotify_1.png](/spotify/spotify_cloning.png)

I made 14 duplicates of the same song and added them to the playlist. This was about as successful as you would expect. Duplicates are not considered  distinct songs. Which makes sense from a API design perspective -- "duplicate" songs are tagged as such, and not counted individually.


![IMG_2359.PNG](../../_resources/e3446a964de948c9b86272448698d1d3.PNG)
Fig 1: Playing any one of the tracks on mobile led to all of its duplicates being played as well.

(The # of tracks that Spotify adds is constant, and not a ratio -- i.e., regardless of how many times you duplicate a song, the number of automatically added songs will remain the same.)

## Web API: 💻📲
The Web / PC versions of Spotify do allow playlists with single tracks; but when one switches to the mobile app, this is overridden.

~~I discovered that if you're playing a song on one device, and pause from another device, you skip to the next track~~. I wanted to try playing a track on mobile, and switching to the Desktop version just before it ends. A cursory look at the Spotify Web API suggested that this could be done programmatically: I could use the current song length to determine the right instance to switch devices, and setting the playing device using the device  ID. [2]

I used the interactive REST snippet on the Spotify page -- generating the necessary OAuth token -- and executed the GET request using <curl> from the command line. My iPad and Laptop were detected, but my iPhone was not. (It seems the iPad version is more like the Desktop / Web than the iPhone version.)
![spotify_3._highlight.png](/spotify/spotify_3_e.png)


My initial guess was that you can only interface with the iPhone player using the iOS specific SDK. But a look at the Web API documentation revealed that "Smartphone" is a distinct Device type. My updated was guess that smartphones are only detected for Premium subscribers.

![spotify_4.png](/spotify/spotify_docs_e.png)

I signed up for a Premium trial to test my hypothesis, and the API finally detected my iPhone.

![spotify_premium_e.png](/spotify/spotify_premium_e.png)



Alas, it seems that I won't be able to execute any fun device switching tricks.



I toyed with using the Web API to mute my player when an ad begins playing, as there are [no real](http://jmeyers44.github.io/blog/2015/04/26/builder-beware-the-limitations-of-popular-apis/) [rate limits](https://stackoverflow.com/questions/46322838/any-ideas-about-rate-limit-request-minute-on-spotify-api/46348692) on the REST API. Unfortunately, it seems that only Premium users can set the volume programmatically (a fact which the documentation does not [explicitly mention](https://developer.spotify.com/console/put-volume/). From what I can tell, all the Player API queries are restricted to Premium users)


## Spotify Connect 

Fortunately, Spotify themselves offer a feature which vastly improves the mobile user experience: Spotify Connect!. You play your music on the PC, but control it from your phone -- retaining all the functionality of Desktop  while also sporting the flexibility of mobile.

i.e. 
- Choose tracks from a playlist instead of using Shuffle Play
- Skip to point x in track
- Repeat songs
- etc.

There are obviously some limitations to this approach, though -- the primary one being (i) advertisements, and to a lesser extent, (ii) the fact that I am limited by the range of bluetooth from my laptop to my headphones. It seems AirPlay would have solved this for me if I had a Mac computer, but I'm sure there are other solutions as well. This isn't really something that affects me right now, so my only concern is advertisements.

***.............................................***

## Addendum
Additional notes on the Spotify platform and its clients.

### Web
Many sites use the [User-Agent](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/User-Agent) parameter to check a user's platform. I tried 
changing the agent using [Sleipnir](https://apple.stackexchange.com/questions/40647/is-ther
e-a-web-browser-for-ios-that-will-allow-me-to-change-the-user-agent), but open.spotify.com seems to have other [countermeasures](https://apple.stackexchange.com/a/356902) in place.
- A few clever folks figured out a way to [counter these countermeasures](https://www.reddit.com/r/firefox/comments/81nlk6/is_it_possible_to_use_the_spotify_web_player_on/) by using browser resolution a few years ago, but they don't work anymore.
- I could look into intercepting the HTTP requests made to the Spotify API using BurpSuite, but that will have to wait.

### Mobile
- Spotify skip limits are probably based on system time...
- Apple doesn't allow you to make changes to device time programmatically (https://stackoverflow.com/questions/22107315/how-to-programmatically-set-ios-device-time)
- You can root Android devices...a lot of Spotify skips appa for rooted devices out there 
- it seems the free premium cracks are based on some other fact, since even ios has them : spotify++ https://www.fonecope.com/get-spotify-premium-free.html

## References

[1] https://community.spotify.com/t5/Accounts/Playlists-How-do-I-stop-Spotify-from-adding-tracks-to-my/td-p/4520490
[2] https://developer.spotify.com/console/get-users-available-devices/
