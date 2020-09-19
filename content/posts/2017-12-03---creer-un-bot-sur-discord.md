---
title: "Créer un bot sur Discord"
date: "2017-12-03"
template: "post"
draft: false
slug: "creer-un-bot-sur-discord"
category: "Dev"
tags:
  - "Blog"
  - "Dev"
  - "Bot"
  - "Discord"
description: "Créer un bot sur Discord est quelques chose de très simple. Bien qu'il existe plusieurs façon de faire, je vous propose de le faire fonctionner à l'aide de NodeJS et du module discord.js."
# socialImage: "/media/image-0.jpg"
---

![](/media/7-discord-bots-to-enhance-your-server.jpg)

Vous l'aurez donc compris, il fonctionnera en JavaScript mais il existe d'autres librairies, notamment en Python, Java, PHP, C#, etc. Voici une liste des librairies disponibles : https://discordapi.com/unofficial/libs.html

Le prérequis pour ce tutoriel est d'avoir un serveur NodeJS (minimum v.6.0.0) installé, ainsi qu'un compte discord.

# Création de l'application
Afin d'avoir un bot disponible sur son serveur, il faut se rendre sur [la partie developper](https://discordapp.com/developers/docs/intro) du site de Discord afin de créer une nouvelle application.
Pour ce faire, allez dans **"My Apps"** et cliquez sur **"New app"**.

![Screen-Shot-2017-12-03-at-16.58.12](/media/Screen-Shot-2017-12-03-at-16.58.12.png)

Remplissez le formulaire (voir screen) et cliquez sur **"Create App"**.
Il suffit ensuite de convertir votre App en Bot en cliquant sur le bouton **"Create a Bot User"** :
![Screen-Shot-2017-12-03-at-17.02.46](/media/Screen-Shot-2017-12-03-at-17.02.46.png)

Vous obtiendrez alors un écran spécifique avec le token de votre Bot. Notez le bien, on en aura besoin plus tard.

![Screen-Shot-2017-12-03-at-17.01.04](/media/Screen-Shot-2017-12-03-at-17.01.04.png)

# Initialisation et configuration

Démarrez un nouveau projet et installez `Discord.js` à l'aide de NPM :    
`$ npm install --save discord.js`

Créez ensuite un fichier `bot.js` et mettez ceci dedans :
```js
const Discord = require("discord.js");
const client = new Discord.Client();

client.on('ready', () => {
  console.log(`Logged in as ${client.user.tag}!`);
});

client.on('message', msg => {
  if (msg.content === 'ping') {
    msg.reply('Pong!');
  }
});

client.login('token');
```

Remplacez `token` par celui de votre application, et exécutez la commande :  
`$ node bot.js`

Votre bot est désormais connecté sur votre serveur. Pour le moment, il ne répond qu'au message **ping** mais libre à vous d'ajouter des commandes.

Vous pouvez diffuser de la musique ou des sons, récupérer des infos depuis des API, faire du scrapping ou vous notifier de différentes choses, les possibilités sont multiples.

Voici la [documentation complète de Discord.js](https://discord.js.org/#/docs/main/stable/general/welcome) afin de vous donner des idées. Ou bien encore mon bot, disponible sur [mon GitHub](https://github.com/Enishowk/Bot-Discord).

Je présenterai des fonctionnalités plus poussée dans une autre partie. Si vous avez des questions ou des suggestions, je suis disponible sur [Twitter](https://twitter.com/Enish__).