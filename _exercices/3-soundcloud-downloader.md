---
title: Soundcloud downloader
layout: exercise
---

# Soundcloud Downloader

Cet exercice est le principal exercice de ce cours.

Nous allons en écrire 3 versions, en y allant de façon incrémentale:

- Une version dans le terminal écrite en programmation impérative classique (ce que vous avez fait jusqu'à maintent)
- Une version dans le terminal écrite en programmation orientée objet
- Une application web Ruby On Rails plus complète

## Consigne

Le but de cette application est d'écrire un client Soundcloud qui nous permettra de chercher un artiste et de télécharger ses musiques.

Pour se faire nous allons avoir besoin de passer par [l'API Soundcloud](https://developers.soundcloud.com/docs/api/reference).

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

client = SoundClound.new({
                           client_id: "VOTRE_CLIENT_ID",
                           client_secret: "VOTRE_CLIENT_SECRET"
                         })

# Les 10 tracks les plus en vogue
hottest_tracks = client.get('/tracks', :limit => 10, :order => 'hotness')
```

Dans notre cas nous n'irons pas chercher les tracks à la mode, mais nous ferons une recherche via un nom d'artiste puis nous rechercherons les tracks de cet artiste.

Pour trouver la requête à faire cherchez comment faire une recherche avec le [guide de l'API](https://developers.soundcloud.com/docs/api/guide) pour un exemple et consultez [la documentation de l'API](https://developers.soundcloud.com/docs/api/reference) afin de trouver comment trouver toutes les tracks d'un artiste.
