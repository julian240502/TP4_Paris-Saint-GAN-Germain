# TP4_Paris-Saint-GAN-Germain
# TP4 – Modèles Autoregressifs (PixelCNN)

## Introduction
Ce TP a pour objectif la mise en œuvre d’un modèle autoregressif inspiré du PixelCNN afin de générer des images. En se basant sur le jeu de données FashionMNIST, le modèle apprend la distribution conjointe des pixels en la décomposant en distributions conditionnelles. Les images sont prétraitées pour être réduites à une résolution de 16×16 pixels et quantifiées en 4 niveaux, simplifiant ainsi la tâche d’apprentissage.

## Architecture du Modèle

### Prétraitement des Données
- **Redimensionnement et Quantification :**  
  Les images de FashionMNIST sont redimensionnées en 16×16 pixels et chaque pixel est quantifié en 4 niveaux.

### Couches de Convolution Masquée
- **Principe :**  
  Les couches de convolution utilisent des masques pour empêcher l’utilisation de pixels « futurs » lors de la prédiction.
- **Types de Masques :**
  - **Type A :** Utilisé dans la première couche pour exclure le pixel courant.
  - **Type B :** Appliqué dans les couches suivantes pour autoriser le pixel courant sans compromettre l’ordre de génération.

### Blocs Résiduels
Les blocs résiduels combinent des convolutions masquées, des activations non linéaires et des connexions de saut. Cette structure facilite la circulation des gradients et améliore la stabilité pendant l’apprentissage.

### Empilement Global
- Le réseau débute par une couche de convolution masquée de type A.
- Il est suivi d’une série de blocs résiduels qui raffinent l’extraction des caractéristiques.
- Les couches finales consistent en des convolutions masquées de type B, conclues par une couche linéaire générant les logits des différents niveaux de pixels.

## Implémentation

### Modules Clés
1. **Préparation des Données**  
   Chargement du jeu FashionMNIST, redimensionnement des images et quantification des niveaux de pixels.
2. **Définition des Couches Personnalisées**  
   - *Convolution Masquée :* Une classe dédiée ajuste dynamiquement les poids via un masque fixe, garantissant que chaque pixel dépend uniquement de ceux générés précédemment.  
   - *Bloc Résiduel :* Intègre des convolutions masquées et des activations non linéaires, tout en ajoutant une connexion directe pour faciliter l’apprentissage.
3. **Assemblage du Modèle**  
   Construction du réseau par empilement séquentiel des couches définies précédemment, offrant une structure modulaire et facilement modifiable.
4. **Entraînement du Réseau**  
   Utilisation de l’algorithme Adam pour optimiser les paramètres et une fonction de perte basée sur l’entropie croisée, mesurant l’écart entre la distribution prédite et la valeur réelle des pixels. La boucle d’entraînement ajuste progressivement les poids à travers des lots d’images.

## Génération et Visualisation des Images

### Processus de Génération
- **Initialisation :**  
  Un tenseur d’images initialisé à zéro sert de point de départ.
- **Itération Pixel par Pixel :**  
  Pour chaque position (i, j), le modèle calcule la distribution conditionnelle à l’aide des pixels déjà générés, permettant ainsi d’échantillonner la valeur du pixel courant.
- **Contrôle par Température :**  
  Le paramètre de température module l’aléa de l’échantillonnage, ajustant l’équilibre entre diversité et fidélité des images générées.

### Visualisation
Les images générées, une fois normalisées, sont affichées à l’aide de bibliothèques telles que `matplotlib` pour une évaluation qualitative du modèle.

## Conclusion
Ce projet démontre l’efficacité des modèles autoregressifs pour la génération d’images. La combinaison de convolutions masquées et de blocs résiduels permet de modéliser rigoureusement la dépendance entre pixels. Malgré une résolution réduite et une quantification limitée, le modèle parvient à produire des images cohérentes par rapport aux données d’entraînement.
