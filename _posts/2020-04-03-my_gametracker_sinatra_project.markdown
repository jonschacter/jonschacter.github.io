---
layout: post
title:      "My GameTracker Sinatra Project"
date:       2020-04-03 23:32:48 +0000
permalink:  my_gametracker_sinatra_project
---


I was a little bit amibitious with this project and instead of doing a simple game collection app I decided to add functionality of tracking individual play sessions with information like dates, players, and winners. A lot of my early hurdles were just setting up the complex relationships I required for this. I ran into one particular problem with recording winners that I wanted to discuss here.

My initial relationship set-up:

![](imgur.com/a/HNspdxN)

I was using a join table to connect game_sessions and players with a many-to-many relationship. I was using a winner_id column inside of game_sessions that would hold a player_id and created custom winner= and winner methods to connect to a player object.

My main issue with this was because SQLite3 cannot store arrays of data I could only have one winner. I wanted to be able to track cooperative or team games so had to find a way around this. 

My first thought was that I'd have to make a Winner class and a new join table but I didn't like this because it would be duplicate information with the Player class.

My solution ended up being to add a boolean winner? column to my existing game_sessions_players join table. I could say Player 1 was in GameSession 2 and was the winner. The difficulty here was that I needed to create a class for this join table so that I could connect to this data with activerecord.

This might not be the most elegant solution, I am not happy with some of the clunky code required but it achieved my desired result. What what would have tried differently? I am aware that there are options like PostgreSQL which would allow me to store arrays in my database. I am excited to learn and experiment with these tools in the future as I continue my growth as a developer.

