---
layout: post
title:      "Let's Talk Routes"
date:       2020-05-09 18:27:31 -0400
permalink:  lets_talk_routes
---


I just completed my Rails app, which is a Spa booking app, and wanted to give some insights into the way I chose to manage my routes. First I should talk a little bit about the models involved. The main model is Spa which has has_many associations with Technicians and Treatments (that work and are offered there). I also have a User model. When booking an Appointment a User can select a Treatment and Technician, so the Appointment model has a belongs_to relationship and foreign keys for those classes. So let's talk specifically for the seven RESTful routes for Appointments.

**INDEX**

I wanted my appointments#index controller action to be nested under a user, so that user can visit their appointment page and view any past and upcoming appointments they have made.

**SHOW**

I also wanted the appointments#show action to be nested under a user, so they can look up specific information about their appointments

**NEW**

I chose to have the appointments#new action nested under a spa. This made it easier to filter Treatment and Technician form select options to a specific spa, so that impossible combinations could not be created. 

**CREATE**

I did not want the appointments#create to be nested at all. I wanted the option to be able to submit a new appointment from a spa page, but also leave my options open to later submit them through treatment or user pages. I made sure my form had the approriate hidden fields to pass along information correctly to an unaffiliated create action

**EDIT**

Similar to new I chose to keep appointments#edit nested under a spa.

**UPDATE**

Similar to create I chose to keep appointments#update nested under a spa.

**DESTROY**

Since the index and show actions to access an appointment are nested under a user I chose to keep appointments#destroy nested under a user as well. It also gave me an easy way to match params[:user_id] and the current sessions user id to make sure users could not delete appointments that did not belong to them.

```
Rails.application.routes.draw do
  devise_for :users, :controllers => {:registrations => 'registrations', :omniauth_callbacks => "users/omniauth_callbacks"}
  # For details on the DSL available within this file, see https://guides.rubyonrails.org/routing.html
  root to: 'welcome#home'
    post 'zipcode_search', to: 'spas#zipsearch'
  resources :spas do
    resources :technicians, only: [:show, :new, :create, :edit, :update, :destroy]
    resources :treatments, only: [:show, :new, :create, :edit, :update, :destroy]
    resources :appointments, only: [:new, :edit]
  end

  resources :users, only: [] do
    resources :appointments, only: [:index, :show, :destroy]
  end

  resources :appointments, only: [:create, :update]
end
```

Here is how my config/routes.rb file turned out. I am not sure if this is the cleanest or most standard way to map these routes but for the reasons above it made sense with how I wanted to approach the URLs, and feed myself the information I needed. 
