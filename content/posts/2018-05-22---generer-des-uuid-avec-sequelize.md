---
title: "Générer des UUID avec Sequelize"
date: "2018-05-22"
template: "post"
draft: false
slug: "generer-des-uuid-avec-sequelize"
category: "Dev"
tags:
  - "Blog"
  - "Dev"
  - "UUID"
  - "Sequelize"
description: "Configurer la gestion des UUID avec l'ORM Sequelize."
# socialImage: "/media/image-0.jpg"
---

Salut, ça faisait longtemps que je n'avais pas posté. Je vais essayer de prendre un rythme de publication un peu plus régulier.

Petit post concernant un problème que j'ai rencontré hier avec [Sequelize](http://docs.sequelizejs.com/variable/index.html#static-variable-DataTypes) et la gestion des UUID.

En effet, la génération des ID peut être faite de manière automatique et en auto-increment - heureusement me direz-vous - cependant, lorsqu'il s'agit de générer des UUID, cela se complique.    
Si vous déclarez votre table avec une colonne UUID, sans la spécifier dans votre model, vous allez avoir des erreurs du type : "Unknown column 'id' in 'field list'". Car par défaut, Sequelize cherche une colonne ID, même si vous lui spécifiez une colonne UUID en Primary Key etc. 

Pour régler ce problème, premièrement, dans votre fichier de migration `migrations/create-users.js`, il faut spécifier le bon DataType et le bon nom de colonne que vous souhaitez utiliser :

```js
module.exports = {
  up: (queryInterface, Sequelize) =>
    queryInterface.createTable("Users", {
      uuid: {
        allowNull: false,
        primaryKey: true,
        type: Sequelize.UUID,
      },
      username: {
        allowNull: false,
        type: Sequelize.STRING,
      },
      password: {
        allowNull: false,
        type: Sequelize.STRING,
      },
      createdAt: {
        allowNull: false,
        type: Sequelize.DATE,
      },
      updatedAt: {
        allowNull: false,
        type: Sequelize.DATE,
      },
    }),
  down: queryInterface => queryInterface.dropTable("Users"),
};

```

Ensuite, il faut modifier votre User Model. Habituellement, si vous utilisez des ID, vous n'avez pas besoin de rajouter la colonne "ID". Avec les UUID, vous êtes obligés. Le fichier `models/users.js` doit être configuré comme ceci :  

```js
module.exports = (queryInterface, Sequelize) => {
  const User = queryInterface.define(
    "User",
    {
      uuid: {
        type: Sequelize.UUID,
        primaryKey: true,
        defaultValue: Sequelize.UUIDV4,
      },
      username: Sequelize.STRING,
      password: Sequelize.STRING,
    },
    {},
  );
  return User;
};

```


Cependant, il reste un problème lorsque l'on ajoute des datafixtures. En effet, Sequelize ne gère pas l'ajout dynamique des UUID, il faut donc ajouter la lib [node-uuid](https://github.com/kelektiv/node-uuid) et modifier le fichier `seeders/demo-user.js` comme ceci par exemple :

```js
const uuidv4 = require("uuid/v4");
const bcrypt = require("bcrypt");

module.exports = {
  up: queryInterface =>
    queryInterface.bulkInsert(
      "Users",
      [
        {
          uuid: uuidv4(),
          username: "user1",
          password: bcrypt.hashSync("password", 10),
          updatedAt: new Date(),
          createdAt: new Date(),
        },
      ],
      {},
    ),
  down: queryInterface => queryInterface.bulkDelete("Users", null, {}),
};

```

J'espère que cela vous aidera. Si vous avez des questions ou de meilleures façons de procéder, contactez-moi sur [Twitter](https://twitter.com/Enish__). 🙂
