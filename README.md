# React Events in Detail

## Overview

We'll look at how we can use event data and event pooling.

## Objectives

1. Access event data
2. Describe how event pooling works

## Accessing event data

Let's take a deeper look at the actual event being passed through. A
`SyntheticEvent` event has all of its usual properties and methods. These
include its type, target, mouse coordinates, and so on. As a reminder, we add an
event handler to a JSX tag, and then we can use the event's data like this:

```jsx
export default class Clicker extends React.Component {
  handleClick = (event) => {
    console.log(event); // prints the event object
    console.log(event.type); // prints 'click'
  };

  render() {
    return <button onClick={this.handleClick}>Click me!</button>;
  }
}
```

For example, if we wanted to get the target of an event, we'd use
`event.target`. If we want to prevent a default action whenever an event
happens, we call `event.preventDefault()`. This is all super similar to regular
browser events and should feel very familiar!

## Event pooling

![Chilling in the pool](https://media.giphy.com/media/38Gl1kiklmY5W/giphy.gif)

Since React has its own implementation for events (called `SyntheticEvent`), it
allows us to take advantage of several techniques to increase the performance of
our applications. One such technique is called 'event pooling'. It might sound
complicated, but it's actually pretty simple.

Event pooling means that whenever an event fires, its event data (an object) is
sent to the callback. The object is then immediately cleaned up for later use.
This is what we mean by 'pooling': the event object is in effect being sent back
to the pool for use in a later event. It's something that trips up a lot of
people, and you might have run into it yourself when inspecting `SyntheticEvent`
in the browser.

If we click the button of our `Clicker` component and then inspect the logged
out object in our console, we notice that all properties are `null` again. By
the time we inspect the object in our browser, the event object will have
already been returned to the pool. This means that we can't access event data in
an asynchronous manner by saving it in the state, or running a timeout and
_then_ accessing the event again.

You usually don't need to access your event data in an asynchronous manner like
described above, but if you do, there are two options: you either store the data
you need in a variable (e.g. `const target = event.target`), _or_ we can make
the event persistent by calling that method: `event.persist()`.

> **Note**: As of React version 17, React
> [no longer uses pooling][remove event pooling] for synthetic events.
> Therefore, for newer React applications, using `event.persist()` is no longer
> required. It's still important to learn this concept as many companies still
> use versions of React prior to React 17, and the topic of event pooling may
> come up on React job interviews as well.

[remove event pooling]: https://blog.saeloun.com/2021/04/06/react-17-removes-event-pooling-in-modern-system.html#event-pooling

## Resources

- [React Events](https://reactjs.org/docs/events.html)
