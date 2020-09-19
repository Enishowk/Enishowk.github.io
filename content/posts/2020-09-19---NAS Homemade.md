---
title: "NAS Homemade"
date: "2020-09-19"
template: "post"
draft: false
slug: "nas-homemade"
category: "Hardware"
tags:
  - "Hardware"
  - "NAS"
description: "Petit guide d'achat pour monter son propre NAS."
# socialImage: "/media/image-0.jpg"
---

Hello. Le blog est inactif en ce moment mais en décembre 2019 je me suis monté mon propre NAS et j'avais commencé à écrire un article dessus. Mieux vaut tard que jamais, je le publie donc maintenant. 

Tout d'abord, on pourrait se demander pourquoi s'embêter à faire soit-même son NAS alors que beaucoup de constructeur en propose des tout-faits. La raison principale est surtout le rapport puissance/prix. 

## Hardware

Ayant actuellement un NAS une baie de chez Synology (115J). Celui-ci met quasiment 30/40 secondes à afficher son interface d'admin... Ses performances n'ont rien d'exceptionnelles et il n'est pas évolutif. Je souhaitais donc le remplacer par quelque chose de beaucoup plus puissant, à un tarif assez bas et surtout je voulais quelque chose qui consomme peu et que l'on entend pas. Ça tombe bien car [ASRock](https://www.ldlc.com/informatique/pieces-informatique/carte-mere/c4293/+fv144-15386.html) propose des cartes mères avec des processeurs qui consomment peu (TDP de 10W) et fanless. 
Il ne reste plus qu'à ajouter une alimentation, un boitier avec plusieurs baies, de la RAM et nous avons tout le matos. Voici ma config finale :

| Type | Modèle |
|------------------|:--------------------------------------------:|
| Carte mère + CPU | [ASRock J5005-ITX](https://www.ldlc.com/fiche/PB00248155.html)|
| RAM              | [G.Skill 4 Go DDR4 2400Mhz](https://www.ldlc.com/fiche/PB00201793.html)|
| HDD              | [Western Digital WD Red 4 To](https://www.ldlc.com/fiche/PB00153210.html)|
| Alim             | [be quiet! Pure Power 11 300W 80PLUS Bronze](https://www.ldlc.com/fiche/PB00260983.html)|
| Boitier          | [Fractal Design Node 304](https://www.ldlc.com/fiche/PB00135558.html)|


Libre à vous d'ajouter de la RAM, changer le boitier et de prendre une meilleure alimentation. J'ai choisi ce boitier car j'ai pu en recupérer un quasi neuf pour presque rien et que l'on peut y installer jusqu'à 6 disques dur. Attention cependant, la CM n'a que 4 port sata.

Nous avons donc notre NAS pour environ 330€ (sans les disques). En cherchant le meilleur prix avec des promos, vous pouvez largement vous en tirer pour moins de 300€. J'ai eu quasiment cette config pour 220€ en décembre avec un boitier d'occasion et un SSD en plus pour le système.

Si on compare avec un NAS Synology, voici celui qui s'en rapproche niveau prix : [Synology DS218+](https://www.ldlc.com/fiche/PB00235845.html).
Il n'a que 2 Go de DDR3 et son processeur est [vraiment moins puissant](https://cpu.userbenchmark.com/Compare/Intel-Pentium-Silver-J5005-vs-Intel-Celeron-J3355/m487063vsm247225). Même les modèles à plus de 800€ chez Synology sont moins puissant que la config du dessus.

Alors, c'est bien d'avoir mieux pour moins cher mais nous sommes obligé de tout configurer nous-même. On a rien sans rien. 🙂

## Software

Concernant la distrib, nous avons plusieurs choix : 

* [OpenMediaVault](https://www.openmediavault.org/)
* [FreeNAS](https://www.freenas.org/)
* [Xpenology](https://xpenology.org/)
* ... et bien d'autres.

FreeNAS requiert 8Go de RAM. Xpenology est un clone (Illégal) de DSM de Synology. Il reste donc OMV que j'ai choisi pour mon NAS. C'est gratuit, open source, et légal donc c'est parfait pour mon utilisation.

## Conclusion

L'installation et la configuration du NAS s'est avéré très simple et plutôt rapide. Les performances sont plus que correct. Il ne fait pas de bruit et la consommation se rapproche de la conso d'un NAS Synology. D'après les retours que j'ai lu, on se situe environ à 15W en utilisation.
