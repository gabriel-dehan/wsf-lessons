---
title: Soundcloud downloader
layout: exercise
---

# Soundcloud Downloader

Cet exercice est le principal exercice de ce cours.

Nous allons en écrire 3 versions, en y allant de façon incrémentale:

- Une version dans le terminal écrite en programmation impérative classique (ce que vous avez fait jusqu'à maintent et le but du présent exercice)
- Une version dans le terminal écrite en programmation orientée objet
- Une application web Ruby On Rails plus complète

## Consigne

Le but de cette application est d'écrire un client Soundcloud qui nous permettra de chercher un artiste et de télécharger ses musiques.

Pour se faire nous allons avoir besoin de passer par [l'API Soundcloud](https://developers.soundcloud.com/docs/api/reference).

Les étapes sont les suivantes:

- Afficher un message de bienvenue dans le terminal
- Demander à l'utilisateur le nom d'un artiste
- Faire une recherche via l'API SoundCloud pour trouver l'id de l'artiste
- Faire une recherche via l'API SoundCloud pour trouver les tracks de l'artiste
- Afficher la liste des tracks d'un artiste avec un index
- Demander à l'utilisateur l'index de la track à télécharger
- Télécharger le track


Essayez de séparer votre code en méthodes, par exemple une méthode pour demander des informations à l'utilisateur, une méthode pour chercher un artiste sur soundcloud, etc...

## Screenshots

Votre application ressemblera globalement à ça (vous pouvez l'afficher différement si vous le souhaitez)

{% img 'soundcloud-dl3.png' alt:'Example' class:'soundcloud-dl' %}

## Aide & Tips

### Pour demander des informations à l'utilisateur

On utilise la méthode `gets` qui va arrêter votre programme jusqu'à ce que l'utilisateur écrive quelque chose et appuie sur la touche entrée.

```ruby
puts "Entrez quelque chose"

# Ici l'execution s'arrête et quand l'utilisateur a appuyé sur entrée
# la variable value contient ce que l'utilisateur a écrit
value = gets()
```

Une chose à savoir, c'est que la méthode gets renvoie TOUT ce que l'utilisateur a écrit (même la touche entrée).

```ruby
puts "Entrez quelque chose"
value = gets # L'utilisateur écrit Bonjour
p value #=> "Bonjour\n" <-- un \n (retour à la ligne) qu'on ne veut pas
```

Pour enlever ce `\n` on utilise la méthode `#chomp`

```ruby
puts "Entrez quelque chose"
value = gets.chomp # L'utilisateur écrit Bonjour
p value #=> "Bonjour"
```

Aussi, `gets` et `gets.chomp` retournent **une string**, si on a besoin de convertir cette string en **integer** on peut utiliser `to_i`

```ruby
value = gets.chomp
value = value.to_i # On convertit en entier
```

### API Soundcloud

Afin d'utiliser l'API vous aurez besoin de créer un compte [Soundcloud](https://soundcloud.com/).

Ensuite il va falloir créer une application sur [SoundCloud for Developers](https://developers.soundcloud.com/) via [soundcloud.com/you/apps](https://soundcloud.com/you/apps) (vous devez être connecté pour y accéder).

{% img 'soundcloud-create-app-1.png' alt:'Click on the register button' class:'soundcloud' %}
Cliquez sur **Register a new Application** et remplissez le formulaire.

Ne mettez rien pour "Website of your app" ou "Redirect URI" pour cette version de l'exercice.

Notez le **Client ID** et le **Client Secret** quelque part, il s'agit de la clef d'API qui nous permettra d'accéder aux services de soundcloud.

Pour en savoir un peu plus sur l'API de Soundcloud allez faire un tour sur [le guide.](https://developers.soundcloud.com/docs/api/guide)

### La gem soundcloud

Pour accéder à l'API de Soundcloud nous pourrions faire des appels HTTP à l'API directement, un peu comme on ferait des requêtes AJAX en Javascript. Cela reste néanmoins assez fastidieux et [Soundcloud possède une gem](https://github.com/soundcloud/soundcloud-ruby) qui va nous simplifier la vie (faites une recherche soundcloud ruby, c'est un des premiers liens).

Pour l'installer:

```bash
$ gem install soundcloud
```

Pour l'utiliser:

```ruby
# En haut du fichier
require 'soundcloud'

# Ici on utilise une constante (en majuscule) car les constantes sont globales
# ce qui nous permettra d'appeler notre variable CLIENT depuis n'importe quelle méthode
CLIENT = SoundCloud.new({
  client_id: "VOTRE_CLIENT_ID",
  client_secret: "VOTRE_CLIENT_SECRET"
})

# Affiche les 10 tracks les plus en vogue
p CLIENT.get('/tracks', :limit => 10, :order => 'hotness')
```

Ou dans une méthode

```ruby
# ...

# Les 10 tracks les plus en vogue
def get_10_best_tracks
  CLIENT.get('/tracks', :limit => 10, :order => 'hotness')
end

p get_10_best_tracks
```

Dans notre cas nous n'irons pas chercher les tracks à la mode, mais **nous ferons une recherche via un nom d'artiste puis nous rechercherons les tracks de cet artiste**.

Pour trouver la requête à faire, cherchez comment faire une recherche via le [guide de l'API](https://developers.soundcloud.com/docs/api/guide) pour un exemple et consultez [la documentation de l'API](https://developers.soundcloud.com/docs/api/reference) afin de trouver comment trouver toutes les tracks d'un artiste.

Pour récupérer les artistes correspondant à un mot clef nous utiliserons:

```ruby
# ...

# renvoie un tableau d'artistes sous forme de hash
artists = CLIENT.get('/users', q: "LeNomDeLartiste")
artists.first["full_name"] # Nom complet du premier artiste trouvé
artists.first["id"] # ID du premier artiste trouvé
```

Pour récupérer la liste des tracks d'un artiste:

```ruby
# ...
artist_id = # ...

# renvoie un tableau de tracks sous form de hash
tracks = CLIENT.get("/users/#{artist_id}/tracks")

p tracks[1] # => Un hash contenant toutes les informations sur la track
puts tracks[1]["title"] # => Nom de la deuxième track
puts tracks[1]["stream_url"] # => Lien à suivre pour écouter la deuxième track
```

### Du debug plus joli

La méthode `p` est utile mais elle nous affiche les données sur une seule ligne, de façon illisible.
On peut utiliser la gem `awesome_print` pour avoir quelque chose de plus lisible et indenté.

```
$ gem install awesome_print
```

```ruby
# Dans votre fichier
require 'awesome_print'
require 'soundcloud'

CLIENT = SoundCloud.new({
  client_id: "VOTRE_CLIENT_ID",
  client_secret: "VOTRE_CLIENT_SECRET"
})

artists = CLIENT.get('/users', q: "LeNomDeLartiste")

ap artists # Ici on appelle `ap` au lieu de `p` et le debug est lisible
```

### Télécharger une track

Maintenant qu'on sait comment récupérer une track, nous voulons la télécharger.
Pour cela nous allons utiliser la gem [soundcloud-downloader](https://github.com/gabriel-dehan/soundcloud-downloader).

```
$ gem install soundcloud-downloader
```

Et dans votre fichier .rb

```ruby
# ...
require "soundcloud"
require "soundcloud-downloader"

CLIENT = SoundCloud.new({
  client_id: "VOTRE_CLIENT_ID",
  client_secret: "VOTRE_CLIENT_SECRET"
})

# ... On récupère l'artiste, la liste de tracks, etc...

# Et pour télécharger une track:
downloader = SoundCloud::Downloader::Client.new({
  client_id: "VOTRE_CLIENT_ID",
  # Le dossier où télécharger les tracks (ex: downloads)
  path: "downloads"
})

# Le nom du fichier une fois téléchargé (ex: le titre de la track)
name = # ...

# On appelle download qui va télécharger le fichier et afficher une bar de progression
# Il faut bien sur avoir déjà récupéré la liste des tracks, et récupéré le stream_url de l'une d'entre elles
downloader.download(track_stream_url, { file_name: name, display_progress: true })
```

Une fois que vous avez appelé la méthode `download` le fichier est téléchargé et vous pourrez le trouver dans le dossier que vous avez indiqué (le dossier est créé au même niveau que votre fichier .rb)

### Parcourir un tableau avec son index

On utilise `each_with_index` au lieu de `each`.
Cette méthode fonctionne aussi bien avec les `Hash` que les `Array`.
- [http://apidock.com/ruby/v1_9_3_392/Enumerator/each_with_index](http://apidock.com/ruby/v1_9_3_392/Enumerator/each_with_index)
- [http://ruby-doc.org/core-2.3.0/Enumerable.html#method-i-each_with_index](http://ruby-doc.org/core-2.3.0/Enumerable.html#method-i-each_with_index)

## Enfin

Lisez les documentations, relisez la consigne, re-relisez la consigne, re-re-relisez la consigne et d'utiliser `irb` et `awesome_print` pour vous aider dans le développement.

## Aller plus loin
(Plus loin, plus de points o/)

Vous pouvez améliorer l'apparence de l'interface en utilisant la gem [`colorize`](https://github.com/fazibear/colorize)

```
$ gem install colorize
```

```ruby
require 'colorize'

puts "unestring".orange # Affichera une string de couleur orange dans le terminal
```

La documentation est assez simple donc allez y faire un tour.

## .
## .
## .
## .

## Je n'y arrive vraiment pas

**Essayez à partir de la page blanche avant, ne vous précipitez pas sur ce squelette.**

En dernier recours, si vous êtes bloqué, en train de paniquer et que vous ne savez vraiment pas comment vous lancer dans l'exercice, vous pouvez vous aider du squelette suivant:
[https://gist.github.com/gabriel-dehan/e16915a34a737c006150](https://gist.github.com/gabriel-dehan/e16915a34a737c006150)
