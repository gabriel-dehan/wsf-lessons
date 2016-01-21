---
title: HTML Compiler
layout: exercise
---

# HTML Compiler

Le but de cet exercice est de créer une méthode nous permettant de générer du HTML à partir de code écrit en Ruby.

## Consigne

Un exemple vaut milles mots:

```ruby
tag('h1', "Ruby", { class: "title", id: "wow" })
# => <h1 class="title" id="wow">Ruby</h1>

tag('a', "Cliquez ici", { href: "http://www.google.com", class: "link" })
# => <a href="http://www.google.com" class="link">Cliquez ici</a>
```

Vous devez donc coder une méthode `tag` qui aura la signature suivante:

```ruby
tag(tag_name, content, attributes)
```

- `tag_name` est une **string**
- `content` est une **string**
- `attributes` est un **hash** d'attributs HTML

## Resources & Tips

* **Utilisez IRB pour tester vos bouts de code**
* Utilisez l'interpolation plutôt que la concaténation pour générer votre String

## Vous avez fini ?

Bravo, vous avez réussi à créer un équivalent (limité, certes) de la librarie [markaby](https://github.com/markaby/markaby"), utilisée par le framework Web [camping](http://camping.io/) après quelques heures de Ruby.

## Aller plus loin

Pour ceux qui ont fini, améliorez la méthode `tag` en faisant en sorte que l'on puisse lui passer un block :

```ruby
tag('div', { class: "container", id: "wow" }) do
  tag('a', { href: "#add" } ) do
    tag('span', { class: "icon icon-add" }) do
      "Ajouter"
    end
  end
end
```

```html
<div class="container" id="wow">
  <a href="#add">
    <span class="icon icon-add">
      Ajouter
    </span>
  </a>
</div>
```

### Rappel sur les blocks

Pour rappel, un **block** est un bout de code réutilisable (comme une fonction anonyme) qui renvoie une valeur.

```ruby
tag("div", { class: "header" }) do
  "Bonjour"
end
```

Ici, le block

```ruby
do
  "Bonjour"
end
```

Renvoie simplement la valeur `"Bonjour"` lorsqu'il est appelé dans la fonction `tag`.


### Utiliser les blocks

Pour se faire vous allez donc avoir besoin de passer un **block en tant qu'argument de la méthode `tag`**.

Pour se faire on utilise `&block` dans les paramètres de la méthode. Lorsqu'on veut appeler le *block* on utilise `block.call()`.

La signature devient donc la suivante:

```ruby
def tag(tag_name, attributes, &block)
```

```ruby
def tag(tag_name, attributes, &block)
  # do something

  # On appelle le block, qui dans l'exemple du dessous
  # nous retourne "Bonjour" ou "Au revoir"
  if block != nil # Si on a un block
    content = block.call()
  end

  # return something
end

tag("div", { class: "header" }) do
  "Bonjour"
end
#=> <div class="header">Bonjour</div>

tag("div", { class: "footer" }) do
  "Au revoir"
end
#=> <div class="footer">Au revoir</div>
```

Pour vous aider, pensez que le fonctionnement est l'équivalent direct d'un callback javascript

```javascript
function tag(tag_name, attributes, callback) {
  // do something
  var content = callback();
  // return something
}

tag("div", { class: "container" }, function() {
  return "Bonjour";
});
// => <div class="container">Bonjour</div>
```
