<img src="/images/oooh.png" width = 300>

# OOOH
> an event sharing mobile app (iOS)


Link to [live site](https://expo.io/@tsquarius/oooh)  
Please note that the code itself is in a private repository. This is just a readme to discuss the project.

## Overview

OOO is an event sharing app that helps users find events near
them or organizers share their events. This is my first mobile
application, which I built entirely from scratch as part of my
learning experience.

### How it works

As a user, you are able to find events based on distance and by
tags that you're looking for. Additionally, you may simply just
browse through all available events. You can save events that
you're interested in by simply clicking on the blue square at
the bottom right of the event image. You will be able to scroll
through all your bookmarks at any time.

## Technologies

### Overview
- Frontend: React Native, Redux
- Backend: Ruby on Rails
- Database: Heroku / PostgreSQL
- Amazon AWS is used to host images


## Functionality

- [x] Search customizations or "custom finds" that a user can subscribe to
- [x] Autocomplete suggestions for when a user is typing a tag
- [x] Infinite scrolling and Pagination with sort functionality
- [x] Custom animations
- [x] Ability to bookmark/unbookmark events at a click of a button
- [x] Track unique view statistic of each event

### Search Customization and Subscription
A user is able to create and subscribe to custom event searches based on tags. 
These custom searches are then stored on the home page for a user to revisit and check if any new results appear.
Furthermore, the results are then filtered for events based on the user's address and preferences (with the help of ``geocoding``)

![Search](/images/custom_search_comp.gif?raw=true)

### Infinite Scrolling & Pagination
Pagination relies on the ``kaminari`` gem in Rails.
Infinite scrolling is achieved with the FlatList component.  
Flow: 
1. I keep track of the page number using a React hook. 
2. Whenever a user reaches the bottom of a scroll, it triggers a fetch call with the next page number. 
3. To ensure there are no duplicate events, the eventIds appended to a Set in the event reducer.
4. If the fetch returns no results, then I do not allow further fetch calls.  

The infinite scrolling component allows buttons for the user to pass in sorting parameters (asc/desc) and by attribute (i.e. event dates).

![Infnite Scroll](/images/infinite_scroll_comp.gif?raw=true)

### Custom Animations
All animations in the app are created by me using React Native's Animated API.  
The animations include:
1. Carousel for when scrolling through events
2. Animation when focusing an event
3. Stacked, sliding components for switching between Profile and Settings

The Carousel expanding effect was acheived through tracking the index of the visible event card.
I trigger an expanding animation whenever the visible card and the index number match. 
Conversely, I trigger a minimizing animation as the card leaves the center frame.

There is no built in way for us to track the visible card index. To workaround this, I back into the
index number by using the xOffset and dividing that by standard width of each card.  
Sample code:  
```
  const handleScroll = event => {
    let xOffset = event.nativeEvent.contentOffset.x;
    let contentWidth = cardwidth + margin;
    let value = Math.round(xOffset / contentWidth);
    setCurrentIndex(value);
  };
```

The calculation is pushed into the onScroll call back and thus whenever the user scrolls, the component calculates which index the user is currently on.

![Custom Animations](/images/custom_animations_comp.gif?raw=true)

### Bookmarking
To create a user-friendly bookmarking functionality, I created a reusable image component which contains a "bookmark" button.
The component tracks whether or not the user already contains the related event within their bookmark listing.
This then dictates whether or not the button sends a "bookmark" or "unbookmark" call.

![Bookmarks](/images/bookmarks_comp.gif?raw=true)