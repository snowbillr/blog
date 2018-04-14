# Buttons in Phaser 3

Phaser 2 had buttons, but they aren't built-in to Phaser 3. Thankfully, they are simple to make on our own. In this tutorial we'll go over the basics of what a button is in Phaser-land and a couple of different ways to make them.

## Buttons Aren't Scary

Buttons aren't some magic black box of mystery. At its core concept, a button is something that the player can click on which then calls some function we've written.

That's it.

Hover states, animations, fancy effects, whatever, are just icing on the cake. Phaser makes it really easy to make something clickable, so let's start with buildling that.

## Project Set Up

I'll be using the project setup from [this blog post](https://snowbillr.github.io/blog/2018-04-09-a-modern-web-development-setup-for-phaser-3/). You can follow through that post to set up the project yourself, or you can clone the full setup [from GitHub](https://github.com/snowbillr/phaser3-webpack-es6-dev-starter) and follow the instructions in it's README.

Once you have the project running, you should see something like this in your browser:

![browser initial setup](/blog/img/posts/buttons-in-phaser-3/browser-initial-setup.png)

## The Most Basic Button

Lucky for us our project already has taken care of the first part of our most basic button. That ugly green text on that ugly black background? Looks like a perfect candidate for the most basic button we can make.

The ugly green text is an instance of `Phaser.GameObjects.Text` which extends the `Phaser.GameObjects.GameObject` class. Everything that gets added to our scene is an instance of `GameObject`, and this is the class that provides us with the method we need to make our text (or our image, or sprite, or any other `GameObject`) clickable.

We need tell Phaser that we want our player to be able to interact with our `GameObject`. [I wonder what method we should call on GameObject.](https://github.com/photonstorm/phaser/blob/v3.3.0/src/gameobjects/GameObject.js#L262).

Great guess! It *is* the `setInteractive` method. Don't worry about any of the arguments to the method for now. If we call that method on our text with no parameters, it will make the rectangular bounds of our text interactive, which is exactly what we want. So let's do that in `scenes/simple-scene.js`. We'll also keep a reference to our text object in a variable so we have access to it later.

```javascript
export class SimpleScene extends Phaser.Scene {
  create() {
    const helloButton = this.add.text(100, 100, 'Hello Phaser!', { fill: '#0f0' });
    helloButton.setInteractive();
  }
}
```

Great! But it doesn't seem like anything has changed in our browser. We can check that it is by listening to events that are emitted from our text object by adding event listeners. When we tell Phaser a `GameObject` is interactive, it will start to emit events whenever any player interaction occurs with that `GameObject`. These events are emitted in two ways:

1. Directly from the `GameObject` itself
1. From the `InputPlugin` object (`this.input` in a scene)

Since we only care about interaction events on our `GameObject` and not for the scene in general, we'll only listen to events coming directly from our text and ignore those coming from the `InputPlugin`.

To verify that we are actually getting events, let's log them as they get emitted. The events we care about are `pointerover`, `pointerout`, `pointerdown`, and `pointerup`. The full list of events can be found by browsing [the `InputPlugin`s source code](https://github.com/photonstorm/phaser/blob/master/src/input/InputPlugin.js). Just use your browser to search for "emit(" on that page.

```javascript
export class SimpleScene extends Phaser.Scene {
  create() {
    const helloButton = this.add.text(100, 100, 'Hello Phaser!', { fill: '#0f0' });
    helloButton.setInteractive();

    helloButton.on('pointerover', () => console.log('pointerover'));
    helloButton.on('pointerdown', () => console.log('pointerdown'));
    helloButton.on('pointerup', () => console.log('pointerup'));
    helloButton.on('pointerout', () => console.log('pointerout'));
  }
}
```

![browser pointer events](/blog/img/posts/buttons-in-phaser-3/browser-pointer-events.gif)

The Most Basic Button™ only cares about when the user clicks on it, so to start we'll only need to listen to the `pointerdown` event. And rather than logging something to the console, let's show an image when we click the button.