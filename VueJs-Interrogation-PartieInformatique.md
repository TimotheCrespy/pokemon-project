# Vue Js - Interrogation

## Partie informatique

### Mise en place

Récupérer le projet `pokemon-project` disponible à cette URL : https://github.com/TimotheCrespy/pokemon-project

Installer toutes les dépendances requises dans le `package.json`.

Pour lancer le projet, utiliser un terminal et se rendre dans le répertoire du projet et lancer la commande `npm run serve`. Cette commande propose du « Hot-Module-Replacement (HMR) », permettant de recompiler tout le code dès qu'il est modifié, de la même manière que la commande `npm run watch` sur d'autres proets. Se rendre ensuite sur l'URL indiquée dans le terminal (le plus souvent, `http://localhost:8080/`) dans un navigateur.

Il s'agit d'un projet utilisant Vue CLI (https://cli.vuejs.org/guide/).

Installer `axios`(https://github.com/axios/axios).

Le design est à ignorer, sauf lorsque demandé explicitement.

Les bonnes pratiques sont à respecter : casse, indentation, orthographe, etc.

### Initialisation

Supprimer le composant `HelloWorld.vue`.
Supprimer dans la vue `App.vue` toutes les références à `HelloWorld.vue`.

Modifier le logo de Vue Js par le logo Pokémon suivant, au mêmes dimensions : https://1000logos.net/wp-content/uploads/2017/05/Pokemon-Logo.png (si l'URL renvoie une erreur, utiliser un autre logo Pokémon au choix).

L'application doit alors répondre correctement dans le navigateur, en affichant simplement le logo Pokémon.

### Menu principal

Créer un nouveau composant `Title.vue` ayant en tant que `prop` une chaine de caractère obligatoire appelée `text`. Ce composant affiche le texte `text` dans un titre HTML de niveau 1, ayant la classe CSS `title`, comportant le CSS suivant :
```css
transform: skew(-8deg) rotate(-8deg);
grid-area: text;
font-family: "Sarpanch", sans-serif;
font-size: 64px;
margin: 0;
padding: 16px;
color: #1d9099;
text-shadow: 4px 4px 0 #e79c10, -4px -4px 0 #d53a33;
```

Créer 3 nouveaux composants dont l'élément racine est une `<section>` :
- `FullListTab.vue`
- `ClassicSearchTab.vue`
- `EvolutionSearchTab.vue`

Ces 3 composants importent, mettent à disposition et utilisent chacun le composant `Title.vue` , avec pour texte respectivement :
- Liste complète
- Recherche classique
- Recherche des évolutions

Ils importent aussi chacun le package `axios`.

Dans la vue `App.vue`, créer un menu simple uilisant les composants dynamiques (https://vuejs.org/v2/guide/components.html#Dynamic-Components). Pour cela, la propriété `data` dans `<script>` est seulement :
```js
data() {
  const tabs = [
    {
      name: "Liste complète",
      component: FullListTab
    },
    {
      name: "Recherche classique",
      component: ClassicSearchTab
    },
    {
      name: "Recherche des évolutions",
      component: EvolutionSearchTab
    }
  ];
  return {
    tabs: tabs,
    currentTab: tabs[0]
  };
}
```

Concernant la partie `<template>`, s'inspirer de : https://codesandbox.io/s/github/vuejs/vuejs.org/tree/master/src/v2/examples/vue-20-dynamic-components-with-binding?file=/index.html

### Onglet liste complète

Dans le composant `FullListTab.vue`, récupérer via l'API PokéApi (https://pokeapi.co/) un objet dans une variable du state comportant seulement les noms des 151 premiers pokémons, sous cette forme :
```json
{
  "count": 964,
  "next": "https://pokeapi.co/api/v2/pokemon?offset=151&limit=151",
  "previous": null,
  "results": [
    {
      "name": "wobbuffet",
      "url": "https://pokeapi.co/api/v2/pokemon/202/"
    },
    {
      "name": "girafarig",
      "url": "https://pokeapi.co/api/v2/pokemon/203/"
    },
    ...
    {
      "name": "larvitar",
      "url": "https://pokeapi.co/api/v2/pokemon/246/"
    },
    {
      "name": "pupitar",
      "url": "https://pokeapi.co/api/v2/pokemon/247/"
    }
  ]
}
```

S'aider de la documentation de l'API pour déterminer l'URL complète à appeler : https://pokeapi.co/docs/v2#info

Dans une propriété calculée du même composant, transformer le tableau `results` en tableau de chaines de caractères dont les valeurs sont les propriété `name` de chacun des éléments de celui-ci (afin d'obtenir `["bulbasaur", "ivysaur", ..., "mewtwo", "mew"]`).

Afficher à l'aide de la directive appropriée tous les éléments de ce nouveau tableau sous forme de liste HTML numérotée.

### Onglet recherche classique

Dans le composant `ClassicSearchTab.vue`, créer un formulaire simple comportant un champ textuel et un bouton de validation du formulaire intitulé « Rechercher ».

Lier le contenu du champ textuel à une variable du state nommée `pokename`.

Faire en sorte que la phrase suivante s'affiche lors du clic sur « Rechercher » : « La recherche est effectuée pour : [pokename] » (en remplaçant [pokename] par le texte entré dans le champ textuel). Si le contenu du champ textuel est vide ou composé uniquement d'espaces, afficher : « Veuillez renseigner un nom de pokémon. ».

Compléter le composant pour lancer la recherche lorsque le formulaire est validé (pa un clic sur « Rechercher »). Pour cela, déterminer l'URL auprès de API PokéApi (https://pokeapi.co/), qui devra bien sûr comporter le contenu du champ textuel.

Si le retour de l'API est en erreur, i.e. si le pokémon semble ne pas exister, afficher la phrase : « Le pokémon n'a pas été trouvée... ».

Si le retour de l'API est en succès (pour un nom de pokémon valide en anglais), récupérer dans une variable du state l'objet renvoyé, de la forme :
```json
{
  "abilities": [...],
  "base_experience": 101,
  "forms": [...],
  "game_indices": [...],
  "height": 3,
  "held_items": [...],
  "id": 132,
  "is_default": true,
  "location_area_encounters": "https://pokeapi.co/api/v2/pokemon/132/encounters",
  "moves": [...],
  "name": "ditto",
  "order": 203,
  "species": {...},
  "sprites": {...},
  "stats": [...],
  "types": [...],
  "weight": 40
}
```

Afficher une image de l'avant du pokémon et une autre de l'arrière du pokémon.

Afficher les informations suivantes :
- La première « habilité » du pokémon
- Sa taille et son poids
- Sa position dans le pokédex
- Tous ses types sous forme de liste dont les élements sont séparés par des virgules.

### Onglet recherche des évolutions

> Cette partie est en bonus. Il est inutile de l'entamer si le reste n'est pas complètement opérationnel et bien implémenté.

Proposer pour cet onglet lié au composant `EvolutionSearchTab.vue` une recherche identique à celle de `ClassicSearchTab.vue`, mais présantant les noms des pokémons de la même famille d'évolution de celui recherché. Par exemple, si la recherche est « Abra », afficher « Abra > Kadabra > Alakazam » ; si la recherche est « Pinsir », afficher « Pinsir n'a pas dévolution » ; si la recherche est « Jigglypuff », afficher « Igglybuff > Jigglypuff > Wigglytuff ».

Prendre en compte le fait qu'il faut effectuer deux appels API l'un après l'autre :
- le premier pour récupérer l'`id` du pokémon à partir de son nom (`pokename`)
- le second pour récupérer sa chaine d'évolution `chain` à partir de son identifiant `id`.

### Rendu

Zipper le projet `pokemon-project` pour créer le fichier `[prenom]-[nom].zip`. L'envoyer d'une part à l'adresse `timothe.crespy@lyon.ort.asso.fr`, et d'autre part à `Timothé Crespy` en message privé sur Microsoft Teams, avant 12h05.
