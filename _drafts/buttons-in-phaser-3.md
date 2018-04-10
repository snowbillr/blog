# Buttons in Phaser 3

Phaser 2 had buttons, but they aren't around in Phaser 3. Thankfully, they are simple to make on our own. In this tutorial we'll go over the basics of what a button is in Phaser-land, and a couple different ways to make them.

## Buttons Aren't Scary

Buttons aren't some magic black box of mystery. At its core concept, a button is something that the player can click on, which then calls some function we've written.

That's it.

Hover states, animations, fancy effects, whatever, are just icing on the cake. Phaser makes it really easy to make something clickable, so let's start with buildling that.

## Project Set Up

I'll be using the project set up from [this blog post](https://snowbillr.github.io/blog/2018-04-09-a-modern-web-development-setup-for-phaser-3/). You can follow through that post to set up the project yourself, or you can clone the full set up [from GitHub](https://github.com/snowbillr/phaser3-webpack-es6-dev-starter) and follow the instructions in it's README.

## The Most Basic Button

Lucky for us our project already has taken care of the first part of our most basic button. That ugly green text on that ugly black background? Looks like a perfect candidate for the most basic button we can make.

That ugly green text is an instance of `Phaser.GameObjects.Text` which extends the `Phaser.GameObjects.GameObject` class. Everything that gets added to our scene is an instance of `GameObject`, and this is the class that provides us with the method we need to call to make our text (or our image, or sprite, or any other `GameObject`) clickable.

dWe need tell Phaser that our `GameObject` should be interactive. [I wonder what the method's name is that we should call](https://github.com/photonstorm/phaser/blob/v3.3.0/src/gameobjects/GameObject.js#L262).