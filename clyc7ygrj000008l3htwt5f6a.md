---
title: "Devvit #1: Introduction to the Reddit Developer Platform"
datePublished: Mon Jul 08 2024 00:03:24 GMT+0000 (Coordinated Universal Time)
cuid: clyc7ygrj000008l3htwt5f6a
slug: introduction-to-the-reddit-developer-platform
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1720396588965/324affc9-e487-44b4-a5e6-835fb5822bec.png
tags: app-development, tutorial, introduction, software-development, javascript, nodejs, coding, typescript, social-media, chatbots, reddit

---

Most social media platforms offer developer platforms for third-party bots or apps to enhance functionality and engage with users. For example, Discord is known for its [Discord Bots](https://discord.com/developers/docs) that interact with Discord servers, and Twitch has [chatbots](https://dev.twitch.tv/docs/irc/) that serve various functions in a streamer's live chat. Historically, Reddit has lacked a robust developer platform compared to the other social media platforms. While they do have a data API and unofficial libraries to access it, they haven't had great support for third-party apps. This is all changing with the introduction of [Devvit](https://developers.reddit.com/), Reddit's Developer Platform that allows you to create third-party apps that can interact with Reddit.

In this tutorial series, I will teach you the basics of Devvit and how to build apps on it, as well as what support you have as a Devvit builder. I will cover how to create an app from scratch, execute code in event triggers, call outside APIs with fetch, and display custom posts through experiences. We'll start with how to install devvit and initialize a new Devvit App from scratch.

## Installing Devvit

At its core, Devvit is a JavaScript library that runs on Node.js. In order to install it, you must first have [Node.js](https://nodejs.org) and [Git](https://git-scm.com/) installed. Once you have them installed, you can then install Devvit with:

```bash
npm install -g devvit
```

That's it! You should have `npm` installed from installing Node.js, but if not, you should double-check your PATH and other environment variables. If you are using a different package manager like `yarn` or `pnpm`, install devvit globally using the corresponding package manager's install command. Now that you have installed devvit, you can then login using your Reddit account (you will need a Reddit account to build on Devvit):

```bash
devvit login
```

This command should prompt a new browser window to open, leading to an authorization page to authorize Devvit on your Reddit account. Once you have authorized Devvit, we are now ready to create our first Devvit App!

## Creating Your First Devvit App

To create a new Devvit App, run the following command:

```bash
devvit new {slug}
```

This slug is especially important as it is essentially the ID for your app. It will be reflected in the `devvit.yaml` configuration file as well as the URL for your uploaded app at `https://developers.reddit.com/apps/{slug}` . Make sure to choose one that you are OK with publishing the app as, or else you may have to re-upload it as a new app when you decide to publish it later! The Devvit CLI will ask you to choose one of multiple starter templates:

```bash
$ devvit new slug
? Choose a template: (Use arrow keys)
❯ app-settings
  empty
  experience-post
  experience-post-pro
  forms
  image-uploads
  intro-to-devvit
(Move up and down to reveal more choices)
```

In the future, you may select one of the templates to have an easier start to building new apps. For now though, we'll select `empty`. Once Devvit has created the app and installed your NPM dependencies, `cd` into the project folder and you'll see something similar to the following:

```bash
$ ls
devvit.yaml  node_modules/  package.json  package-lock.json  src/  tsconfig.json
```

`devvit.yaml` is a special configuration file for Devvit. `node_modules/` , `package.json`, and `package-lock.json` are files that are present in any NPM project. `tsconfig.json` is a configuration file for the TypeScript compiler. Your code will be written in `src/`, and you should have a `src/main.tsx` that contains the following:

```typescript
// Visit developers.reddit.com/docs to learn Devvit!

import { Devvit } from '@devvit/public-api';

Devvit.addMenuItem({
  location: 'post',
  label: 'Hello World',
  onPress: (event, context) => {
    console.log(`Pressed ${event.targetId}`);
    context.ui.showToast('Hello world!');
  },
});

export default Devvit;
```

Congrats! Now you have your own Reddit App. If you want to upload it and see it in action on a subreddit, you can either playtest it and/or upload it privately on the App Directory.

## Uploading

In order to test your app on a subreddit, you will first have to create a subreddit that you are a moderator on, and then you can install your app on the subreddit. You will first need to upload the app using:

```bash
devvit publish
```

You will then have to input a name for your app, indicate if your app is NSFW, and complete a CAPTCHA to verify that you are a human. Once you have uploaded your app, you will see the following message:

```bash
Creating app...... Successfully created your app in Reddit!
```

Now, you can see it online at https://developers.reddit.com/apps/{slug}! You can then install your uploaded app on your subreddit via the website. With the existing code, you should be able to see a "Hello World" menu item under the triple-dot menu for posts in your subreddit.

## Playtesting

Once you have uploaded your app, you can then playtest it on your subreddit. While the app has not been published yet, your subreddit must have less than 200 members in order to install new versions of your app on it. You can playtest new versions of the app by running the following command:

```bash
devvit playtest {subreddit}
```

This will install the new playtest version of your app on your subreddit. Any changes in the new playtest version will be reflected on your subreddit. Hot reloading is enabled when playtesting, so Devvit will build, upload, and install a new playtest version of your app every time you save new code in your project. Once you are done playtesting, you can revert back to the latest version of your app with the command:

```bash
devvit install {subreddit}
```

You have successfully created your own Devvit App, uploaded it, and installed it to your own subreddit to test it! You are now well on your way to developing a great Reddit App. To end this post, here are some resources you can refer to in case you get stuck. Good luck and happy coding! Share any Reddit Apps you make in the comments with a GitHub link or a Reddit App Directory link!

## Resources

* [Official Devvit Documentation](https://developers.reddit.com/docs/)
    
* [r/Devvit](https://www.reddit.com/r/Devvit/)
    
* [Reddit Devs Discord](https://discord.com/channels/1050224141732687912/1050227353311248404)
    
* [Example Devvit Apps](https://github.com/reddit/devvit/tree/main/packages/apps)
    
* [My Devvit App (Sibyl System)](https://github.com/dragonejt/sibyl-reddit)