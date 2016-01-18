---
title: Louchebem
layout: exercise
---

# Louchebem

Le Louchebem est un langage d'argot utilisé par les bouchers français au 19ème siècle.
Le but de cet exercice est de créer un script qui prend une phrase et la traduit en Louchebem pour que vous puissiez enfin communiquer avec votre boucher ;)

Saviez vous que certaines expressions populaires comme "larfeuille", "loufiah", "l'argot", ou "loufoque" viennent du Louchebem ?

## Consigne

Une phrase est donnée, composée de un ou plusieurs mots.

Pour chaque mot :

- Si le mot n'est composé que d'une seule lettre, il est ignoré
- Si celui-ci commence par une voyelle:
  - On rajoute un "L" au début du mot
  - On rajoute un suffixe aléatoire à la fin du mot
- Si celui-ci commence par une consonne:
  - On prend le premier groupe de consonne et on le rajoute à la fin du mot
  - On rajoute un L en début de mot
  - On rajoute un suffixe àlatoire à la fin du mot


Les suffixes aléatoires sont les suivants :

```
-em, -ème, -ji, -oc, -ic, -uche, -ès
```


* Pour bien comprendre le principe du Louchebem, n'hesitez pas à lire [cet article](http://fr.wikipedia.org/wiki/Louch%C3%A9bem)
* Demandez vous quels sont les principaux challenges pour construire ce traducteur (quelles méthodes allez vous créer, comment savoir si un mot commence par une consonne ou une voyelle, quels sont les différents scénarios possible pour un mot donné...)


## Resources & Tips

* **Utilisez IRB pour tester vos bouts de code**
* Créez deux méthodes minimum, une pour traduire un mot et une pour traduire une phrase (qui appèlera la méthode pour traduire un mot plusieurs fois)
* Pour séparer une phrase en un tableau de mot, il existe une méthode simple, [cherchez par ici](http://ruby-doc.org/core-2.2.0/String.html) ou faites une simple recherche google ;)
* Vous pouvez prendre une valeur aléatoire depuis un tableau en utilisant la méthode [#sample](http://ruby-doc.org/core-2.2.0/Array.html#method-i-sample)
* Si vous avez un tableau, vous pouvez en extraire plusieurs éléments de la sorte:

```ruby
letters = ["r", "e", "l", "o", "u"]
letters[0..2] #=> ["r", "e", "l"]

# De la lettre à l'index 3 jusqu'à la fin du tableau
letters[3..(letters.size - 1)] #=> ["o", "u"]
# Ou (`...` exclu la dernière valeur)
letters[3...letters.size] #=> ["o", "u"]
```
