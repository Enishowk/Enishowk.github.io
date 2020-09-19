---
title: "Mocker son API REST avec Json-server"
date: "2017-11-23"
template: "post"
draft: false
slug: "mocker-son-api-rest-avec-json-server"
category: "Dev"
tags:
  - "Blog"
  - "Dev"
description: "Lorsque l'on développe le front d'une application web basé sur une API REST, il peut arriver que les ressources back ne soient pas encore disponibles. Ou que le serveur qui héberge notre back tombe en rade. On se retrouve donc bloqué, avec un front sans ressource disponible."
# socialImage: "/media/image-0.jpg"
---

L'intérêt de cet article et de vous faire découvrir une manière de mocker vos API REST, grâce à un petit outil, **[json-server](https://github.com/typicode/json-server)**.

**json-server** est un mini serveur, fonctionnant avec nodeJS permettant d'exposer des ressources, via une URL.

La phrase de présentation est simple :
> Get a full fake REST API with zero coding in less than 30 seconds (seriously)

Et il faut avouer qu'ils ont raison. 

# Installation 

Tout d'abord, pour l'installation, rien de plus simple :
`$ npm install -g json-server`

Ensuite, créez-vous un fichier `db.json` qui contiendra vos données avec par exemple :
```js
{
  "posts": [
    { "id": 1, "title": "json-server", "author": "typicode" }
  ],
  "comments": [
    { "id": 1, "body": "some comment", "postId": 1 }
  ],
  "profile": { "name": "typicode" }
}
```

Pour finir, lancez le serveur grâce à cette commande : 
`$ json-server --watch db.json`
![launch-json-server](/media/launch-json-server.png)

Et voilà.

Vous pouvez également vous rendre sur http://localhost:3000 afin d'obtenir une liste des ressources et méthodes HTTP disponible. 
![json-server-localhost](/media/json-server-localhost.png)

Vous l'aurez compris, une simple requête GET sur l'url http://localhost:3000/posts/1 et vous obtiendrez votre ressource "posts" qui correspond à l'id 1.

# Customisation

La documentation de **json-server** est très bien fournie, et vous permettra de tout personnaliser afin que cela ressemble exactement à votre réelle API.

Personnellement, je l'utilise couplé à **[Axios](https://github.com/axios/axios)** sur des projets React par exemple. Cela permet, en changeant une seule ligne de config, de passer de notre vrai back, au serveur json afin de mocker les données.

> Si vous travaillez avec **React** et **Browser-sync**, il y a de fortes chances pour que votre port 3000 soit déjà utilisé. 

Vous pouvez donc au choix, exécuter cette commande :
`$ json-server --watch db.json --port 3100`  

Ou bien créer un fichier de config `json-server.json` et y ajouter :
```js
{
  "port": 3100
}
```

Afin de coller exactement avec votre back, il est possible de défini ses propres [routes](https://github.com/typicode/json-server#add-custom-routes) afin de récupérer ses ressources.
Les routes sont à ajouter dans un fichier `routes.json`. Voici un exemple de routes custom :
```js
{
  "/posts/:category": "/posts?category=:category",
  "/articles\\?id=:id": "/posts/:id"
}
```

Cela va définir les routes comme ceci :
```js
/posts/javascript # → /posts?category=javascript
/articles?id=1 # → /posts/1
```


Enfin, il ne reste plus qu'à ajouter un petit alias NPM/Yarn dans votre `package.json` afin de lancer le server plus simplement :
```js
"scripts": {
    "json:server": "json-server --watch db.json -c json-server.json --routes routes.json"
  },
```

Toute modification des fichiers de config est prise en charge instannément. Inutile de relancer le serveur.  

Pour finir, voici quelques utilisations possibles :  
**Filter** : `GET /posts?title=json-server&author=typicode`  
**Paginate** or **Limit** : `GET /posts?_page=7&_limit=20`  
**Sort** / **Order** : `GET /posts?_sort=views&_order=asc`  
**Search** : `GET /posts?q=internet`  


Quelques liens pour générer des données au format JSON :
https://github.com/Marak/faker.js
https://github.com/boo1ean/casual
https://www.json-generator.com/

Si vous avez des questions ou des suggestions, je suis disponible sur [Twitter](https://twitter.com/Enish__). 😉