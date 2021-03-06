---
title: React Autocomplete component
components: TextField, Popper, Autocomplete
githubLabel: 'component: Autocomplete'
waiAria: 'https://www.w3.org/TR/wai-aria-practices/#combobox'
---

# Autocomplete (Auto-complétion)

<p class="description">L'auto-complétion est un input normal améliorer par un panneau de suggestion.</p>

Le widget est utile pour définir la valeur d'une zone de texte simple d'une seule ligne dans l'un des deux types de scénarios suivants:

1. La valeur de la textbox doit être préalablement définit, ex: un champ de location doit contenir de valide nom de location: [combo box](#combo-box).
2. La zone de texte peut contenir n'importe quelle valeur arbitraire, mais il est avantageux de proposer des valeurs à l'utilisateur, par exemple, un champ de recherche peut suggérer similaires ou recherches antérieures pour gagner du temps aux utilisateurs: [gratuit solo](#free-solo).

C'est censé être une version améliorée de la "react-select" et de "downshift".

{{"component": "modules/components/ComponentLinkHeader.js"}}

## Combo box

La valeur doit être choisie à partir d'un ensemble prédéfini de valeurs autorisées.

{{"demo": "pages/components/autocomplete/ComboBox.js"}}

### Options structure

By default, the component accepts the following options structures:

```ts
const filterOptions = createFilterOptions({
  matchFrom: 'start',
  stringify: option => option.title,
});

<Autocomplete filterOptions={filterOptions} />
```

par exemple:

```js
const options = [
  { label: 'The Godfather', id: 1 },
  { label: 'Pulp Fiction', id: 2 },
];
// or
const options = ['The Godfather', 'Pulp Fiction'];
```

However, you can use different structures by providing a `getOptionLabel` prop.

### Playground

Chacun des exemples suivants illustrent l'une des caractéristiques de la saisie semi-automatique de composant.

{{"demo": "pages/components/autocomplete/Playground.js"}}

### Sélection de pays

Choisissez l'un des 248 pays.

{{"demo": "pages/components/autocomplete/CountrySelect.js"}}

### États contrôlables

Le composant a deux états qui peuvent être contrôlés :

1. l'état "valeur" avec la combinaison `valeur`/`onChange` props. Cet état représente la valeur sélectionnée par l'utilisateur, par exemple en appuyant sur <kbd>Enter</kbd>.
2. l'état "input value" avec la combinaison `inputValue`/`onInputChange`. Cet état représente la valeur affichée dans la zone de texte.

> ⚠ Ces deux états sont isolés, ils doivent être contrôlés de manière indépendante.

{{"demo": "pages/components/autocomplete/ControllableStates.js"}}

## Free solo

Définissez `freeSolo` à true pour que le textbox puisse contenir n'importe quelle valeur arbitraire.

### Search input

La prop est conçue pour couvrir le cas d'utilisation principal d'une **search input** avec des suggestions, par exemple la recherche Google ou le react-autowhatever.

{{"demo": "pages/components/autocomplete/FreeSolo.js"}}

### Créable

Si vous avez l'intention d'utiliser ce mode pour une [combo box](#combo-box) comme expérience (une version améliorée d'un élément sélectionné) nous vous recommandons de régler :

- `selectOnFocus` pour aider l'utilisateur à effacer la valeur sélectionnée.
- `clearOnBlur` pour aider l'utilisateur à entrer une nouvelle valeur.
- `handleHomeEndKeys` pour déplacer le focus à l'intérieur de la fenêtre pop-up avec les touches <kbd>Accueil</kbd> et <kbd>Fin</kbd>.
- Une dernière option, par exemple `Ajouter "VOTRE RECHERCH"`.

{{"demo": "pages/components/autocomplete/FreeSoloCreateOption.js"}}

Vous pouvez également afficher une boîte de dialogue lorsque l'utilisateur souhaite ajouter une nouvelle valeur.

{{"demo": "pages/components/autocomplete/FreeSoloCreateOptionDialog.js"}}

## Groupé

You can group the options with the `groupBy` prop. If you do so, make sure that the options are also sorted with the same dimension that they are grouped by, otherwise you will notice duplicate headers.

{{"demo": "pages/components/autocomplete/Grouped.js"}}

## Options désactivées

{{"demo": "pages/components/autocomplete/DisabledOptions.js"}}

## `useAutocomplete`

For advanced customization use cases, we expose a headless `useAutocomplete()` hook. Il accepte presque les mêmes options que le composant de saisie automatique moins tous les props liés au rendu de JSX. Le composant Autocomplete utilise ce hook en interne.

```jsx
import useAutocomplete from '@material-ui/core/useAutocomplete';
```

- 📦 [4.5 kB gzippé](/size-snapshot).

{{"demo": "pages/components/autocomplete/UseAutocomplete.js", "defaultCodeOpen": false}}

### Hook personnalisé

{{"demo": "pages/components/autocomplete/CustomizedHook.js"}}

Head to the [Customized Autocomplete](#customized-autocomplete) section for a customization example with the `Autocomplete` component instead of the hook.

## Demandes asynchrones

The component supports two different asynchronous use-cases:

- [Load on open](#load-on-open): it waits for the component to be interacted with to load the options.
- [Search as you type](#search-as-you-type): a new request is made for each keystroke.

### Load on open

It displays a progress state as long as the network request is pending.

{{"demo": "pages/components/autocomplete/Asynchronous.js"}}

### Search as you type

If your logic is fetching new options on each keystroke and using the current value of the textbox to filter on the server, you may want to consider throttling requests.

Additionally, you will need to disable the built-in filtering of the `Autocomplete` component by overriding the `filterOptions` prop:

```jsx
import matchSorter from 'match-sorter';

const filterOptions = (options, { inputValue }) =>
  matchSorter(options, inputValue);

<Autocomplete filterOptions={filterOptions} />
```

### Lieu de Google Maps

Une interface utilisateur personnalisée pour la saisie automatique des lieux Google Maps.

{{"demo": "pages/components/autocomplete/GoogleMaps.js"}}

Pour cette démo, nous avons besoin de charger l'API JavaScript [Google Maps](https://developers.google.com/maps/documentation/javascript/tutorial).

> ⚠ Avant de pouvoir commencer à utiliser l'API JavaScript de Google Maps, vous devez vous inscrire et créer un compte de facturation.

## Valeurs multiples

Aussi connu sous le nom de tags, l'utilisateur est autorisé à entrer plus d'une valeur.

{{"demo": "pages/components/autocomplete/Tags.js"}}

### Options fixes

Si vous avez besoin de verrouiller certaines balises pour qu'elles ne puissent pas être supprimées dans l'interface, vous pouvez désactiver les chip.

{{"demo": "pages/components/autocomplete/FixedTags.js"}}

### Cases à cocher

{{"demo": "pages/components/autocomplete/CheckboxesTags.js"}}

### Limiter les tags

Vous pouvez utiliser la prop `limitTags` pour limiter le nombre d'options affichées quand la cible n'est pas sélectionnée.

{{"demo": "pages/components/autocomplete/LimitTags.js"}}

## Tailles

Fancy smaller inputs? Use the `size` prop.

{{"demo": "pages/components/autocomplete/Sizes.js"}}

## Personnalisation

### Custom input

La propriété `renderInput` vous permet de personnaliser l'entrée rendue. Le premier argument de cette prop de rendu contient des props que vous devez avancer. Faites une attention particulière aux clés `ref` et `inputProps`.

{{"demo": "pages/components/autocomplete/CustomInputAutocomplete.js"}}

### Sélecteur de GitHub

Cette démo reproduit le sélecteur d'étiquettes de GitHub :

{{"demo": "pages/components/autocomplete/GitHubLabel.js"}}

Rendez-vous à la section [Crochet personnalisé](#customized-hook) pour un exemple de personnalisation avec le hook `useAutocomplete` au lieu du composant.

## Highlights

La démo suivante repose sur [autosuggest-highlight](https://github.com/moroshko/autosuggest-highlight), un petit utilitaire (1 ko) pour mettre en évidence le texte dans les composants de suggestion automatique et la saisie automatique.

{{"demo": "pages/components/autocomplete/Highlights.js"}}

## Filtre personnalisé

Le composant expose une usine pour créer une méthode de filtre qui peut être fournie à la propriété `filterOptions`. Vous pouvez l'utiliser pour modifier le comportement de filtre par défaut.

```js
import { createFilterOptions } from '@material-ui/core/Autocomplete';
```

### `createFilterOptions(config) => filterOptions`

#### Paramètres

1. `config` (*Object* [optional]):

- `config.ignoreAccents` (*Boolean* [optional]): Defaults to `true`. Supprimer les diacritiques.
- `config.ignoreCase` (*Boolean* [optional]): Defaults to `true`. Tout en minuscule.
- `config.limit` (*Number* [optional]): Default to null. Limiter le nombre d'options suggérées à afficher. Par exemple, si `config.limite` est `100`, seule les premières `100` options correspondantes sont affichées. Cela peut être utile si beaucoup d'options de correspondance et de virtualisation n'ont pas été mises en place.
- `config.matchFrom` (*'any' | 'start'* [optional]): Defaults to `'any'`.
- `config.stringify` (*Func* [optional]): Controls how an option is converted into a string so that it can be matched against the input text fragment.
- `config.trim` (*Boolean* [optional]): Valeur par défaut `false`. Supprimer les espaces suivants.

#### Valeur de retour

`filterOptions`: la méthode de filtre retournée peut être fournie directement à la prop `filterOptions` du composant `Auto-complétion` , ou le paramètre du même nom pour le hook.

Dans la démo suivante, les options doivent commencer par le préfixe de requête :

```jsx
const filterOptions = createFilterOptions({
  matchFrom: 'start',
  stringify: (option) => option.title,
});

<Autocomplete filterOptions={filterOptions} />;
```

{{"demo": "pages/components/autocomplete/Filter.js", "defaultCodeOpen": false}}

### Avancé

Pour les mécanismes de filtrage plus riches, comme les correspondances floues, il est recommandé de regarder [matchs-sorter](https://github.com/kentcdodds/match-sorter). Par exemple:

```jsx
import matchSorter from 'match-sorter';

const filterOptions = (options, { inputValue }) =>
  matchSorter(options, inputValue);

<Autocomplete filterOptions={filterOptions} />;
```

## Virtualisation

Recherchez dans 10 000 options générées aléatoirement. La liste est virtualisée grâce à [react-window](https://github.com/bvaughn/react-window).

{{"demo": "pages/components/autocomplete/Virtualize.js"}}

## Limites

### autocomplete/autofill

Les navigateurs ont des heuristiques pour aider les utilisateurs à remplir les entrées de formulaire. Cependant, cela peut nuire à l'UX du composant.

By default, the component disables the **autocomplete** feature (remembering what the user has typed for a given field in a previous session) with the `autoComplete="off"` attribute. Google Chrome does not currently support this attribute setting ([Issue 587466](https://bugs.chromium.org/p/chromium/issues/detail?id=587466)). A possible workaround is to remove the `id` to have the component generate a random one.

In addition to remembering past entered values, the browser might also propose **autofill** suggestions (saved login, address, or payment details). Dans le cas où vous voulez le remplissage automatique, vous pouvez essayer ce qui suit :

- Nommez l'input sans fuir les informations que le navigateur peut utiliser. par exemple `id="field1"` au lieu de `id="country"`. Si vous laissez l'id vide, le composant utilise un id aléatoire.
- Set `autoComplete="new-password"` (some browsers will suggest a strong password for inputs with this attribute setting):

  ```jsx
  inputProps={{
        ...params.inputProps,
        autoComplete: 'new-password',
      }}
      /&#062;
  ```

Read [the guide on MDN](https://developer.mozilla.org/en-US/docs/Web/Security/Securing_your_site/Turning_off_form_autocompletion) for more details.

### voiceOver iOS

VoiceOver sur iOS Safari ne supporte pas très bien l'attribut `aria-owns`. Vous pouvez contourner le problème avec la propriété `disablePortal`.

### ListboxComponent

Si vous fournissez une prop `ListboxComponent` personnalisée, vous devez vous assurer que le conteneur de défilement prévu a l'attribut `role` défini à `listbox`. Cela assure le comportement correct du défilement, par exemple lorsque vous utilisez le clavier pour naviguer.

## Accessibilité

(WAI-ARIA: https://www.w3.org/TR/wai-aria-practices/#combobox)

Nous encourageons l'utilisation d'un label pour la zone de texte. Le composant implémente les pratiques de création de WAI-ARIA.
