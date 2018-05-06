# Buttons in Phaser 3

Phaser 2 had buttons but they aren't built-in to Phaser 3. Thankfully, they are simple to make on our own. In this tutorial we'll go over the basics of what a button is in Phaser-land and a couple of different ways to make them.

## Buttons Aren't Scary

Buttons aren't some magic black box of mystery. At its core concept a button is something that responds to clicks.

That's it.

Hover states, animations, fancy effects, whatever, are just icing on the cake. Phaser makes it really easy to make something clickable, so let's start with building that.

## Project Set Up

I'll be using the project setup from [this blog post](https://snowbillr.github.io/blog/2018-04-09-a-modern-web-development-setup-for-phaser-3/). You can follow through that post to set up the project yourself, or you can clone the full setup [from GitHub](https://github.com/snowbillr/phaser3-webpack-es6-dev-starter) and follow the instructions in its README.

Once you have the project running you should see something like this in your browser:

![browser initial setup](/blog/img/posts/buttons-in-phaser-3/browser-initial-setup.png)

## The Most Basic Button

Lucky for us our project already has taken care of the first part of our most basic button. That ugly green text on that ugly black background? Looks like a perfect candidate for an ugly button!.

The ugly green text is an instance of `Phaser.GameObjects.Text` which extends the `Phaser.GameObjects.GameObject` class. Everything that gets added to our scene is an instance of `GameObject`, and this is the class that provides us with the method we need to make our text (or our image, or sprite, or any other `GameObject`) clickable.

We need tell Phaser that we want our player to be able to interact with our `GameObject`. [I wonder what method we should call on GameObject.](https://github.com/photonstorm/phaser/blob/v3.3.0/src/gameobjects/GameObject.js#L262).

Great guess! It *is* the `setInteractive` method. Don't worry about any of the arguments to the method for now. If we call that method on our text with no parameters, it will make the rectangular bounds of our text interactive, which is exactly what we want. So let's do that in `scenes/simple-scene.js`. We'll also keep a reference to our text object in a variable so we have access to it later. Since we don't need the coke can sprite, we can remove that as well.

```javascript
export class SimpleScene extends Phaser.Scene {
  create() {
    const helloButton = this.add.text(100, 100, 'Hello Phaser!', { fill: '#0f0' });
    helloButton.setInteractive();
  }
}
```

Great! But it doesn't seem like anything has changed in our browser. We can check that our text is interactive by adding event listeners to it. When we tell Phaser a `GameObject` is interactive it will start to emit events whenever any player interaction occurs with that `GameObject`. These events are emitted in two ways:

1. Directly from the `GameObject` itself
1. From the `InputPlugin` object (`this.input` in a scene)

Since we only care about interaction events on our `GameObject` and not for the scene in general, we'll only listen to events coming directly from our text and ignore those coming from the `InputPlugin`.

All `GameObject`s extend from [EventEmitter3](https://github.com/primus/eventemitter3), so we can listen for the input events directly on the `GameObject` with the [`on`](https://nodejs.org/api/events.html#events_emitter_on_eventname_listener) method. Let's verify our text is actually emitting the input events by listening for the `pointerover` event, which is fired when the cursor first overlaps with the game object.

```javascript
export class SimpleScene extends Phaser.Scene {
  create() {
    const helloButton = this.add.text(100, 100, 'Hello Phaser!', { fill: '#0f0' });
    helloButton.setInteractive();

    helloButton.on('pointerover', () => { console.log('pointerover'); });
  }
}
```

![browser pointerover event](/blog/img/posts/buttons-in-phaser-3/browser-pointerover-event.gif)

Great, we know our "button" is firing off input events that we can listen for and respond to. The other events that we care about are:

- `pointerout` - this is the opposite of `pointerover`. It fires when the cursor *leaves* the area of the game object.
- `pointerdown` - this fires at the start of a click or a touch on the game object, literally when the mouse button is pushed down or when a finger touches down.
- `pointerup` - this is the opposite of `pointerdown`. It fires when the mouse button is released or the finger is lifted up from the game object.

The Most Basic Button™ only cares about when the user clicks on it, so to start we'll only need to listen to the `pointerdown` event. Let's have a click counter that increases everytime we click on our button.

```javascript
export class SimpleScene extends Phaser.Scene {
  create() {
    let clickCount = 0;
    this.clickCountText = this.add.text(100, 200, '');

    const clickButton = this.add.text(100, 100, 'Click me!', { fill: '#0f0' })
      .setInteractive()
      .on('pointerdown', () => this.updateClickCountText(++clickCount) );

    this.updateClickCountText(clickCount);
  }

  updateClickCountText(clickCount) {
    this.clickCountText.setText(`Button has been clicked ${clickCount} times.`);
  }
}
```

So there's the Most Basic Button™ that we can possibly make.

## The Slightly More Complicated Button

The Slightly More Complicated Button™ does more than just respond to clicks. Just like a button on a website, this button has a [`hover` state](https://developer.mozilla.org/en-US/docs/Web/CSS/:hover) and an [`active` state](https://developer.mozilla.org/en-US/docs/Web/CSS/:active). These two states give the user an indication of their interaction with the button.

Didn't it feel weird when you moused over The Most Basic Button™ and nothing happened? This is because you were expecting the button to have some kind of hover state. We can detect when a user is hovering over a button with the `pointerover` event, and detect when the user leaves the hover state with the `pointerout` event. Let's have our button change its text color when the user hovers over it. We can add listeners for those two events to change our button's color.

```javascript
export class SimpleScene extends Phaser.Scene {
  create() {
    let clickCount = 0;
    this.clickCountText = this.add.text(100, 200, '');

    this.clickButton = this.add.text(100, 100, 'Click me!', { fill: '#0f0' })
     .setInteractive()
     .on('pointerdown', () => this.updateClickCountText(++clickCount) )
     .on('pointerover', () => this.enterButtonHoverState() )
     .on('pointerout', () => this.exitButtonHoverState() );

    this.updateClickCountText(clickCount);
  }

  updateClickCountText(clickCount) {
    this.clickCountText.setText(`Button has been clicked ${clickCount} times.`);
  }

  enterButtonHoverState() {
    this.clickButton.setStyle({ fill: '#ff0 '});
  }

  exitButtonHoverState() {
    this.clickButton.setStyle({ fill: '#0f0' });
  }
}
```

![browser simple hover](/blog/img/posts/buttons-in-phaser-3/browser-simple-hover.gif)

Similarly, our Slightly More Complicated Button™ should have an active state too. We can tell when our button becomes active by listening to the `pointerdown` event. We'll also change our existing listener that updates the counter to run on the `pointerup` event so we can make our button feel like it's correctly transitioning through its states.

```javascript
export class SimpleScene extends Phaser.Scene {
  create() {
    let clickCount = 0;
    this.clickCountText = this.add.text(100, 200, '');

    this.clickButton = this.add.text(100, 100, 'Click me!', { fill: '#0f0' })
      .setInteractive()
      .on('pointerover', () => this.enterButtonHoverState() )
      .on('pointerout', () => this.exitButtonHoverState() )
      .on('pointerdown', () => this.enterButtonActiveState() )
      .on('pointerup', () => {
        this.updateClickCountText(++clickCount);
        this.enterButtonHoverState();
    });

    this.updateClickCountText(clickCount);
  }

  updateClickCountText(clickCount) {
    this.clickCountText.setText(`Button has been clicked ${clickCount} times.`);
  }

  enterButtonHoverState() {
    this.clickButton.setStyle({ fill: '#ff0 '});
  }

  exitButtonHoverState() {
    this.clickButton.setStyle({ fill: '#0f0' });
  }

  enterButtonActiveState() {
    this.clickButton.setStyle({ fill: '#0ff' });
  }
}
```

![browser simple hover and active](/blog/img/posts/buttons-in-phaser-3/browser-simple-hover-and-active.gif)