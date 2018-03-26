# A Modern Web Development Setup for Phaser 3

Lots of tutorials for Phaser assume all your games' code will be in one JavaScript file and that your dependencies will be manually downloaded. In a real world development environment, this would not be an ideal setup.

More than likely, you'll be using something like Yarn to manage your dependencies and Webpack to bundle your code. In some cases, you might also be using a tool to transpile your code.

So let's go over what a modern Phaser development set up would look like. This tutorial is for people who understand some JavaScript, but are unfamiliar with how a full JavaScript project is typically set up.

If you understand these concepts already, head down to the [Putting It All Together](#putting-it-all-together) section where we actually go through our development environment set up.

## Dependency Management

You already have one dependency for your Phaser game: Phaser. You can download the library manually, but if there is any kind of automated build or deployment for your game, that can't be automated. Instead, you should use a tool to take care of downloading your dependencies for you. We'll use [Yarn](https://yarnpkg.com/en/), one of the most popular tools for this job.

Follow [Yarn's official installation instructions](https://yarnpkg.com/en/docs/install) to download and install it to your machine. Yarn runs on Node, so that will also need to be installed on your system (Yarn's documentation will help you with that too).

## Code Bundling

Big games don't happen in one file. It gets hard to keep track of everything in your codebase without having some sort of organizational structure. This means you'll end up with tens or hundreds of source files by the time your game is complete. It's unrealistic to include each of these JavaScript files by hand, as well as maintain that list. Instead, we'll use [Webpack](https://webpack.js.org/) to bundle our code into a single source file. Then we'll only have one JavaScript file for our game to include on our HTML page.

In a slightly fancier maneuver, we'll also tell Webpack to bundle our dependencies separately into a second file. This way when we make changes to our game, Webpack won't have to rebundle our dependencies each time it rebundles our source files.

Webpack itself will be one of our dependencies for our project, so we'll take care of installing that later with Yarn.

## Transpiling

JavaScript lives in an interesting world. A specification exists for the language that has defined versions to it. Modern browsers fully support the ES5 version. The next version, ES6 (or ES2015, or ESNext, there are a lot of names for it) is only partially supported by browsers. However, since the specification is fully defined, people have written "transpilers" which take ES6 code and turn it back into ES5 code. This allows developers to use the latest version of the JavaScript specification and still allow their code to be run in browsers.

One of the most popular transpilers is a tool called [Babel](https://babeljs.io/). It works nicely with Webpack, so we'll use the two together.

## Putting It All Together

We made it to the fun part! We're going to set up our development environment now.

You've got Yarn installed now, right? If not, head over to [their website](https://yarnpkg.com/en/docs/install) and follow their instructions to install it.

Make an empty folder somewhere on your computer where we'll make our game, then `cd` into it.

```bash
mkdir ~/src/snowbillr/phaser3-webpack-es6-dev-starter
```

Like we said before, Yarn will be our tool to manage our dependencies. It keeps track of them using a `package.json` file. We could make this file ourselves, but why not have Yarn do the legwork for us? From your game's directory, run the following command:

```bash
yarn init
```

Yarn will run through some questions about your project. Answer them however you want, I included mine for reference below. You can press <kbd>Enter</kbd> to accept the default answer for each question.

![yarn init](/blog/img/posts/a-modern-web-development-setup-for-phaser-3/yarn-init.png)

Yarn just created a `package.json` file for you with all the answers to the questions you answered. The `main` field isn't important for our project (it has to do with someone else installing your project as a dependency), so we can delete it.

### Installing Our Dependencies

Now that Yarn knows we have a project, let's use it to install our dependencies. Projects have a list of their development dependencies and their production dependencies. We'll need to install Webpack and Babel for our development dependencies, and Phaser for our production ones.

Let's first tell Yarn to install Webpack as a development dependency.

```bash
yarn add --dev webpack@^3.0.0
```

![yarn add webpack](/blog/img/posts/a-modern-web-development-setup-for-phaser-3/yarn-add-webpack.png)

We use the `--dev` flag to tell Yarn that this is a development dependency. We could also use the equivalent `-D` flag. We're also telling Yarn to install version 3 of Webpack. The latest version is actually 4, but the Webpack website hasn't updated its documentation to reflect the new version, so it's much easier to set up and configure version 3.

Babel works with Webpack through a [loader](https://webpack.js.org/loaders/), so we'll need to install `babel-loader` (specifically version 7, which is the version compatible with Webpack 3).

```bash
yarn add -D babel-loader@^7.0.0
```

![yarn add babel-loader](/blog/img/posts/a-modern-web-development-setup-for-phaser-3/yarn-add-babel-loader.png)

What's this, a warning?

> "babel-loader@7.1.4" has unmet peer dependency "babel-core@6"

Looks like we need to install a peer dependency of `babel-loader`'s. This is a third type of dependency in addition to production and development. Some packages also require you to install peer dependencies of theirs. In this case, we need to install `babel-core@6`.

```bash
yarn add -D babel-core@6
```

Last, but definitely not least, we need to install Phaser. This time, we are installing it as a production dependency. This is Yarn's default mode, so we don't need to tell it anything special.

```bash
yarn add phaser@^3.0.0
```

And that's it! We are done installing our dependencies. So let's move on to...

### Configuring Webpack

Okay, here's a quick Webpack 101: your configuration goes in a `webpack.config.js` file, you tell Webpack where your source files are and where you want the single bundled output file to go, and you tell Webpack to run your files through `babel-loader`.

Webpack expects this file to export a JSON object describing everything we talked about before. Make a `webpack.config.js` file at the root of your project's directory and export an empty JSON object from it.

```javascript
module.exports = {

};
```

Let's start with our source files. Our project is going to have a single file that starts the game, and imports all the other files it needs. Let's say we'll put that file at `phaser3-webpack-es6-dev-starter/src/index.js`. We'll tell Webpack about it by defining it as our `entry` file.

```javascript
module.exports = {
  entry: {
    app: './src/index.js'
  }
};
```

We also are calling this entry point `app`. This isn't entirely important for our project, but for more complex Webpack configs it matters.

And we'll tell Webpack to put the output our bundled file to the `build/` directory (don't forget to require `path`).

```javascript
const path = require('path');

module.exports = {
  entry: {
    app: './src/index.js'
  },

  output: {
    path: path.resolve(__dirname, 'build'),
    filename: 'app.bundle.js'
  }
}
```

The funkiness around the `path` variable is because Webpack requires an absolute path to the file and not a relative one.

Now we need to tell Webpack to run our code through the `babel-loader` when bundling it. Bear with me on this one:

```javascript
const path = require('path');

module.exports = {
  entry: {
    app: './src/index.js'
  },

  output: {
    path: path.resolve(__dirname, 'build'),
    filename: 'app.bundle.js'
  },

  module: {
    rules: [
      {
        test: /\.js$/,
        include: path.resolve(__dirname, 'src/'),
        use: {
          loader: 'babel-loader',
          options: {
            presets: ['@babel/preset-env']
          }
        }
      }
    ]
  }
}
```

*deep breath* Webpack bundles code by traversing through your code base and resolving all it's **modules** (this term comes from the concept of importing and exporting code from different source files). So here, we are telling Webpack about special **rules** to follow when it's traversing our code. This special rule says that for all the source files whose name's match the regular expression `/\.js$/` and that are found in the `src/` directory, **use** the `babel-loader` for them. We're also giving `babel-loader` some instructions in the form of a preset. This preset is another development dependency that we'll need to install with Yarn.

```bash
yarn add -D @babel/preset-env
```

Before we go any further and bring Phaser into the mix, let's test out our config and make sure it's working. We'll make a file for our entry point (`src/index.js`) as well as another file that we'll import to make sure our bundling is working correctly.

Create the `src/index.js` file and open it up. For now, we'll just test the basics and alert a message.