# Lab 5 : Afficher un grand catalogue avec FlatList (TechMarket App)

- **URL de la plateforme** : https://mliaedu.toubkalit.com/student/courses/38/labs/242
- **Formation** : React Native

---

## 1. Objectifs du lab
À la fin de ce lab, l’étudiant sera capable de :
- Définir une liste de données (tableau d’objets) pour un catalogue.
- Utiliser le composant `FlatList` pour afficher une longue liste de façon performante.
- Créer un composant d’élément (`Item`) réutilisable.
- Utiliser `keyExtractor` pour gérer les clés.
- Appliquer des styles avec `StyleSheet` pour séparer présentation et logique.

---

## 2. Contexte pédagogique
On veut développer une petite application mobile appelée **TechMarket**, qui affiche une liste de produits technologiques (smartphones, laptops, casques, etc.) avec :
- Le nom du produit,
- Le prix,
- Un identifiant unique (`id`).

Le catalogue peut contenir des dizaines d’articles, donc on veut un défilement fluide sans ralentir l’application. `FlatList` est donc le composant idéal.

---

## 3. Pré-requis
- Avoir un environnement React Native fonctionnel (Expo ou React Native CLI).
- Savoir créer un projet React Native basique.
- Avoir déjà vu les composants `View`, `Text`, `StyleSheet`.

---

## 4. Étape 0 – Créer le projet
Si l’étudiant utilise Expo :
```bash
npx create-expo-app techmarket-app
cd techmarket-app
npm start
```
Lancer ensuite l’application dans un émulateur ou sur un téléphone.
Pour la suite, tout le code se fera dans ce projet.

---

## 5. Étape 1 – Créer la liste de données (catalogue)
On va d’abord créer un composant dédié à l’affichage des produits.
1. Créer un dossier `components` à la racine du projet, s’il n’existe pas déjà.
2. Créer un fichier `components/ProductList.js`.
3. Dans ce fichier, commencer par définir un tableau d’objets représentant les produits :

```javascript
// components/ProductList.js
import React from 'react';
import { View, Text, FlatList, StyleSheet } from 'react-native';

const productsToDisplay = [
  { id: 'P01', name: 'Smartphone Nova X', price: '499 €' },
  { id: 'P02', name: 'Laptop Pro 15"', price: '1 199 €' },
  { id: 'P03', name: 'Casque Bluetooth AirSound', price: '149 €' },
  { id: 'P04', name: 'Montre Connectée FitTrack', price: '199 €' },
  { id: 'P05', name: 'Clavier Mécanique MX Blue', price: '89 €' },
  { id: 'P06', name: 'Souris Gamer LaserX', price: '59 €' },
  { id: 'P07', name: 'Écran 27" UltraHD', price: '349 €' },
  { id: 'P08', name: 'Disque SSD 1 To', price: '129 €' },
  { id: 'P09', name: 'Routeur WiFi 6', price: '179 €' },
  { id: 'P10', name: 'Casque VR Immersion', price: '399 €' },
  // on peut imaginer encore plus d’éléments pour simuler une “grande liste”
];
```
- `id` : identifiant unique (utilisé comme clé),
- `name` : nom du produit,
- `price` : prix affiché au format texte.

---

## 6. Étape 2 – Créer un composant ProductItem
On souhaite un sous-composant simple pour afficher un produit : nom + prix.
Dans le même fichier `ProductList.js`, ajouter ce composant :

```javascript
function ProductItem({ name, price }) {
  return (
    <View style={styles.itemContainer}>
      <Text style={styles.itemName}>{name}</Text>
      <Text style={styles.itemPrice}>{price}</Text>
    </View>
  );
}
```
`ProductItem` reçoit `name` et `price` en props. L’affichage est séparé dans ce composant pour garder `FlatList` lisible.

---

## 7. Étape 3 – Créer le composant ProductList avec FlatList
Maintenant, on va utiliser `FlatList` pour afficher tous les produits de `productsToDisplay`.
Toujours dans `ProductList.js` :

```javascript
export default function ProductList() {
  // Fonction qui décrit comment afficher chaque élément de la liste
  const renderProductItem = ({ item, index }) => {
    return (
      <ProductItem
        name={`${index + 1}. ${item.name}`}
        price={item.price}
      />
    );
  };

  return (
    <View style={styles.container}>
      <Text style={styles.headerText}>Catalogue TechMarket</Text>

      <FlatList
        data={productsToDisplay}
        renderItem={renderProductItem}
        keyExtractor={(item) => item.id}
      />
    </View>
  );
}
```
**Explication pédagogique :**
- `data={productsToDisplay}` : `FlatList` itère sur ce tableau.
- `renderItem={renderProductItem}` : pour chaque élément, React Native appelle cette fonction avec `{ item, index }`.
  - `item` contient par exemple `{ id: 'P01', name: 'Smartphone Nova X', price: '499 €' }`.
  - `index` commence à 0 ; on l’utilise ici pour numéroter les produits.
- `keyExtractor={(item) => item.id}` : indique que la clé unique de chaque élément est `item.id`.

---

## 8. Étape 4 – Ajouter les styles avec StyleSheet
On va maintenant nettoyer le code en plaçant tous les styles dans un `StyleSheet`.
Toujours dans `ProductList.js`, ajouter à la fin du fichier :

```javascript
const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#EDEFEE',
    paddingTop: 50,
    paddingHorizontal: 16,
  },
  headerText: {
    fontSize: 24,
    fontWeight: 'bold',
    color: '#333333',
    marginBottom: 16,
    textAlign: 'center',
  },
  itemContainer: {
    backgroundColor: '#FFFFFF',
    paddingVertical: 12,
    paddingHorizontal: 16,
    marginBottom: 8,
    borderRadius: 8,
    borderWidth: 1,
    borderColor: '#DDDDDD',
    flexDirection: 'row',
    justifyContent: 'space-between',
  },
  itemName: {
    fontSize: 16,
    color: '#333333',
    flex: 1,
    marginRight: 10,
  },
  itemPrice: {
    fontSize: 16,
    color: '#EE9972',
    fontWeight: 'bold',
  },
});
```

---

## 9. Étape 5 – Intégrer ProductList dans App.js
Enfin, on doit rendre ce composant depuis `App.js`.
Ouvrir `App.js` à la racine et remplacer son contenu par :

```javascript
// App.js
import * as React from 'react';
import { SafeAreaView, StyleSheet } from 'react-native';
import ProductList from './components/ProductList';

export default function App() {
  return (
    <SafeAreaView style={styles.appContainer}>
      <ProductList />
    </SafeAreaView>
  );
}

const styles = StyleSheet.create({
  appContainer: {
    flex: 1,
    backgroundColor: '#EDEFEE',
  },
});
```

---

## 10. Extension (optionnelle)
Pistes d’enrichissement suggérées :
- Ajouter un champ `category` pour chaque produit et l’afficher.
- Changer le style pour les produits de prix > 500 € (couleur différente).
- Ajouter un `ListHeaderComponent` pour afficher un sous-titre.
- Ajouter un `ItemSeparatorComponent` pour dessiner une ligne de séparation.
