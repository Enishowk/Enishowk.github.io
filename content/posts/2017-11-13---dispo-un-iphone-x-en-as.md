---
title: "Disponibilité d'un iPhone X en AS"
date: "2017-11-13"
template: "post"
draft: false
slug: "dispo-un-iphone-x-en-as"
category: "Dev"
tags:
  - "Dev"
  - "Apple"
description: "L'iPhone X est sorti le 3 novembre, et pourtant, encore aujourd'hui, il est en rupture partout. Néanmoins, certain Apple Store reçoivent quotidiennement du stock. Il faut donc être très rapide pour arriver à en commander un."
# socialImage: "/media/image-0.jpg"
---

Plutôt que de guetter le site toute la journée, on peut simplement faire tourner un script qui fera l'appel à notre place. 

Voici un exemple de function :

```JS
function apple() {
    axios.get("https://www.apple.com/fr/shop/retail/pickup-message?pl=true&parts.0=MQAD2ZD/A&location=75000")
    .then(response => {
        response.data.body.stores.forEach(store => {
            if (store.partsAvailability["MQAD2ZD/A"].storeSelectionEnabled){
                // Envoie de mail, notif, bot
            }
        });
    })
}
```

Il suffit juste de remplacer le code postal par le vôtre, ainsi que le modèle de l'iPhone voulu (dans l'URL et le code) par un de ceux-ci :
+ Noir 64Go : **MQAC2ZD/A**
+ Noir 128Go : **MQAF2ZD/A**
+ Blanc 64Go : **MQAD2ZD/A**
+ Blanc 128Go : **MQAG2ZD/A**

Cette fonction peut être utilisée via un Bot sous Discord, qui l'éxécute toutes les 5 secondes par exemple. Dès lors que le site affiche des dispos, vous serez notifié en MP directement.

À vous de l'adapter selon votre cas.