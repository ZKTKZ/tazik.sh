
+++
title = "🎶 Spotify Freemium"
date = "2020-01-02"
+++

The free version of Spotify Mobile is rather limited -- one has restricted access to tracks, cannot move to an arbitrary point in a track, or play a track on repeat. I test how much I can optimize my Spotify experience without signing up for Premium (or selling out for an illegal crack). 

## Introduction
Spotify Mobile only allows tracks to be played as part of a larger playlist of songs. I made a playlist with just one track, to see how Shuffle Play would deal with it. Spotify automatically added “suggested songs” to my playlist. [1] 
I made 14 duplicates of the same song and added them to the playlist. This was about as successful as you would expect. Spotify might allow you to add duplicates as separate track in a playlist, but they are not considered distinct tracks. This makes sense from a API design perspective. 
(The # of tracks that Spotify adds is constant, not a ratio -- i.e., regardless of how many times you duplicate a song, the number of automatically added songs will remain the same.)
![spotify_1.png](/spotify/spotify_cloning.png)

## Web API: 💻📲
The Web / PC versions of Spotify do allow playlists with single tracks; but when one switches to the mobile app, this is overridden.
I wanted to try playing a track on mobile, and switching to the Desktop version just before it ends. A cursory look at the Spotify Web API suggested that this could be done programmatically: I could use the current song length to determine the right instance to switch devices, and setting the playing device using the device  ID. [2]

I used the interactive REST snippet on the Spotify page -- generating the necessary OAuth token -- and executed the GET request using <curl> from the command line. My iPad and Laptop were detected, but my iPhone was not. (It seems the iPad version is more like the Desktop / Web than the iPhone version.)
My initial guess was that you can only interface with the iPhone player using the iOS specific SDK. But a look at the Web API documentation revealed that "Smartphone" is a distinct Device type. My updated was guess that smartphones are only detected for Premium subscribers.
I signed up for a Premium trial to test my hypothesis, and the API finally detected my iPhone.
Alas, it seems that I won't be able to execute any fun device switching tricks.

![spotify_4.png](/spotify/spotify_docs_e.png)


I toyed with using the Web API to mute my player when an ad begins playing, as there are [no real](http://jmeyers44.github.io/blog/2015/04/26/builder-beware-the-limitations-of-popular-apis/) [rate limits](https://stackoverflow.com/questions/46322838/any-ideas-about-rate-limit-request-minute-on-spotify-api/46348692) on the REST API. Unfortunately, it seems that only Premium users can set the volume programmatically (a fact which the documentation does not [explicitly mention](https://developer.spotify.com/console/put-volume/). From what I can tell, all the Player API queries are restricted to Premium users)


## Spotify Connect 

Fortunately, Spotify themselves offer a feature which vastly improves the mobile user experience: Spotify Connect!. You play your music on the PC, but control it from your phone -- retaining all the functionality of Desktop  while also sporting the flexibility of mobile.
There are some limitations to this approach, however.

1. the fact that I am limited by the range of bluetooth from my laptop to my headphones. It seems AirPlay would have solved this for me if I had a Mac computer, but I'm sure there are other solutions as well. Real time audio streaming comes to mind. This isn't a common use case  for me though, so my *real* concern is 
2. advertisements 

## Conclusion 
Spotify Connect serves my purposes well enough that I probably won't spend more time with this.  

### Appendix 
Many sites use the [User-Agent](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/User-Agent) parameter to check a user's platform. I tried 
changing the agent using [Sleipnir](https://apple.stackexchange.com/questions/40647/is-there-a-web-browser-for-ios-that-will-allow-me-to-change-the-user-agent), but open.spotify.com seems to have other [countermeasures](https://apple.stackexchange.com/a/356902) in place.
 A few clever folks figured out a way to [counter these countermeasures](https://www.reddit.com/r/firefox/comments/81nlk6/is_it_possible_to_use_the_spotify_web_player_on/) by using browser resolution a few years ago, but they don't work anymore.
I could look into intercepting the HTTP requests made to the Spotify API using BurpSuite, but that will have to wait.


1. https://community.spotify.com/t5/Accounts/Playlists-How-do-I-stop-Spotify-from-adding-tracks-to-my/td-p/4520490
2. https://developer.spotify.com/console/get-users-available-devices/
