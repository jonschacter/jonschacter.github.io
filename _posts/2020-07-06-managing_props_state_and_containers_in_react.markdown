---
layout: post
title:      "Managing Props, State and Containers in React"
date:       2020-07-07 01:14:52 +0000
permalink:  managing_props_state_and_containers_in_react
---


This React/Redux project has taught me a lot about the benefits and challenges of working with React components, especially when managing fetch requests. Let me walk you through some troubleshooting I had to do when working on this project.

```
import React, { Component } from 'react'
import { connect } from 'react-redux'
import Park from './Park.js'

class Parks extends Component {
    constructor(props){
        super(props)
        
        this.state = {
            filteredParks: props.parks,
            query: "",
            queryType: "Name"
        }
    }
  
    renderParks = () => {
        return(
            this.state.filteredParks.map(park => {
                return <Park key={park.id} park={park} />
            })
        )
    }

    handleSelectChange = (event) => {
        ...
    }

    handleQueryChange = (event) => {
        ...
    }

    render(){
        return(
            <div className="container">
                <h2>PARKS</h2>
                <div className="list">
                    <form>
                        <input className="park-search" type="text" onChange={this.handleQueryChange} placeholder="search by name or state"/>
                        <select className="button" name="queryType" value={this.state.queryType} onChange={this.handleSelectChange}>
                            <option>Name</option>
                            <option>State</option>
                        </select>
                    </form>
                    {this.renderParks()}
                </div>
            </div>
        )
    }
}

const mapStateToProps = ({ parks }) => {
      return{
			      parks
			}
}

export default connect(mapStateToProps)(Parks)
```

This code was the basis of my Parks container component that would display an index list of Park components based on an array in my Redux store. There was a '/parks/ route rendering this component. I left out some of the controlled form and query logic above but I set up my initial state with a filteredProps key because I want to add a search input for the user to be able to filter a quite long (498 parks) list. 

This set up caused some issues when I was waiting for slow load times. The problem being that when the Component constructor set the initial state for this component it would still have an empty parks array. This led to the user not seeing any parks listed. It wasn't until the first change (the user typing into the query form) that would force the component to re-render and properly display the parks. My initial thought was to use componentDidMount or some lifecycle event to force a rerender or setState to parks at a later time, and while the component would rerender with the change in props it would not reinitialize the state. 

**The Solution**

I used a simple container component to hold the data back until it was ready to be displayed. This also lets me give the user loading feedback. I moved my redux connections to this container component and passed parks as a prop.

```
import React from 'react'
import { connect } from 'react-redux'
import Parks from './Parks.js'

const ParksContainer = ({ parks }) => {
    return(
        <div>
            { parks.length > 0 ? <Parks parks={parks} /> : <h3>LOADING...</h3>}
        </div>
    )
}

const mapStateToProps = ({ parks }) => {
    return {
        parks
    }
}

export default connect(mapStateToProps)(ParksContainer)
```

```
import React, { Component } from 'react'
import Park from './Park.js'

class Parks extends Component {
    constructor(props){
        super(props)
        
        this.state = {
            filteredParks: props.parks,
            query: "",
            queryType: "Name"
        }
    }

...

    render(){
        return(
            <div className="container">
                ...
            </div>
        )
    }
}

export default Parks
```

I got a lot of valuable experience troubleshooting during development of this app with creative use of containers, props and states. I used shared form components that received different props do distinguish between a New or Edit form, and a lot of conditional rendering. I wanted to always make sure that if users quickly navigated the site, or navigated their browser directly to a route I would always be waiting for fetch responses and displaying content appropriately.
