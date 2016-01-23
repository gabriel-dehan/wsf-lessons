---
title: Soundcloud downloader - OOP
layout: exercise
---

# Soundcloud Downloader

Vous avez déjà écrit une version simplifiée et impérative de ce programme.
L'idée est de garder le même programme mais d'adapter le code en programmation orienté objet.

## Consigne

A partir de votre solution à l'exercice précédent (ou à partir de la correction), recoder le programme en utilisant des classes.

Nous utiliserons pour cela un modèle ~MVC.

### Models

Nous aurons les modèles suivants:

- Artist

```ruby
class Artist
  # ...

  def initialize(hash_of_data_from_the_api)
    @id = data["id"]
    @full_name = data["full_name"]
    # ...
  end

  def self.find_by_name(name_of_the_artist)
    # Call the CLIENT
  end

  def tracks
    # Call the CLIENT
  end

  # ...
end

```

- Track

```ruby
class Track
  # ...

  def initialize(data)
    # ...
    @downloader = SoundCloud::Downloader::Client.new(client_id: CLIENT.client_id, path: 'downloads')
  end

  def download!
    # Download the file
  end

  # ...
end
```

### View

L'interface sera gérée par une classe `View` qui ressembler à quelque chose comme ça:

```ruby
class View
  def print_welcome
  end

  def ask_for_artist_name
  end

  def display_tracks(artist, tracks)
  end

  # ...
end
```

### Controller

Le "controlleur", qui appelera la view sera simplement une classe `Main`

```ruby
class Main
  def self.run
    view = View.new

    view.print_welcome
    # ... Use the models, call the view again...
    # ... some_track.download!
  end

end
```

### Client

Une classe `Client` sera aussi nécessaire afin d'interfacer les modèles à l'API (en français: l'instance de `Client` sera globale et donc accessible dans les models.)
Le client sera résponsable d'instancier le `SoundCloud.new` et de faire les requêtes nécessaires

```ruby
class Client
  attr_reader :client_id

  def initialize
    @client_id = "52c68972691d7085a0b7f83935a49750"
    @client_secret = "a68668f96f3c529f76f31ca37cc59f7f"

    @client = SoundCloud.new({ client_id: @client_id, client_secret: @client_secret })
  end

  # methode pour chercher des artistes, methode pour chercher des tracks, ...
end

# En majuscule = Constante = Accessible globalement
CLIENT = Client.new
```


## Organisation

Une classe par fichier, nom du fichier = `le nom de la classe en snake_case .rb`.
Exemple: `ProfileView -> profile_view.rb`

Le fichier `main.rb` (contenant la classe `Main`) devra require toutes les autres classes.
