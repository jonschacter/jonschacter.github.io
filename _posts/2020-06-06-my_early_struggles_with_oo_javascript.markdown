---
layout: post
title:      "My Early Struggles with OO Javascript"
date:       2020-06-06 15:17:13 -0400
permalink:  my_early_struggles_with_oo_javascript
---


My Single-Page-Javascript-Application project is a team-builder tool for a mobile game called Marvel Strike Force. I approached this project with the full intent of writing Object Oriented Javascript code but that quickly fell by the way-side for me. I began to hammer features out and tackle problems one by one and this was a bit more "fly by the seat of my pants" programming that, for me, lent itself to functional programming. I kept on soldiering through until Friday at 10am I was finished and happy with the finished app. 

I had a personal goal that I wanted this project to be OO, both for learning/understanding and personal preference (I think it's often much cleaner). So I decided to dig in and start to refactor. I knew along the way that it would come down to this but I underestimated how long and complicated this process would be. I found myself rethinking my logic, and completely rewriting almost every line of code.

At the end of the day Friday, I had a nearly identical product. However I am much prouder of my code. I think it turned out much cleaner and am thrilled with the changes I made. I think using OO made my logic much clearer and my code less repetitive. It allowed clearer separation and organization. If I could go back in time and approach this project again I would take a little more time planning. I dove in quickly building upon function after function without taking the time to set up clear infrastructure and organization, and while the project might not have changed much it took a significant amount of additional work for myself to feel proud of it. 

One specific way OO was able to help was using class methods to store, update, and delete object instances in memory and cut down on the amount of fetch requests I'd have to make. 

```
class Team {
	static all = []

	constructor({id, name}){
		this.id = id
		this.name = name
		Team.all.push(this)
	}

	get div(){
		return document.querySelectorAll(`*[team-id='${this.id}']`)[0]
	}
}
```

By setting up a simple .all class variable I am able to create and store all of my team objects after one fetch request. I can change and re-render DOM elements using Team.all and not have send a second fetch request and create each object again, as long as I keep my Team.all array up-to-date. I also used a custom getter function to abstract away the querySelectors when I was manipulating the DOM

```
class API {
	static deleteTeam(id){
		fetch(url, options)
			.then(resp => resp.json())
			.then((data) => {
				const index = Team.all.findIndex((team) => team.id === data.id)
				Team.all.splice(index, 1)
				Team.renderTeams()
			}
	}
}
```

Here is my API.deleteTeam method where I show how after processing a DELETE request I removed the corresponding team from Team.all and used that to re-render the team list instead of doing a new GET request to re-render the teams. 

With changes like these I was able to make my app a lot more efficient and flexible, while taking advantage of the computers memory and relying less on fetch requests and repetitive code.
