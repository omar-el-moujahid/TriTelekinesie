# TriTelekinesie

## Le projet
Jeu pédagogique sous **Unreal Engine 5.6.1**, réalisé **uniquement en Blueprint** (pas de C++), à partir du template **First Person**.
Projet étudiant évalué dans le module « Interactions multimodales en réalité étendue ».
L'auteur est **débutant** : tout doit rester simple, lisible et expliqué.

## Concept
Le joueur apprend à **trier les déchets** grâce à un pouvoir de **télékinésie**
(inspiration : le pouvoir d'Eleven dans *Stranger Things*, seulement le concept, aucun élément de la série).

- Des déchets (cubes de couleur avec une étiquette) sont dispersés dans une petite salle, certains hors de portée.
- Le joueur les attrape, puis les lance dans la bonne poubelle.
- Le jeu explique **pourquoi** le choix est juste ou faux.

## Commandes (clavier + souris, pas de casque VR)
| Action | Commande |
|---|---|
| Se déplacer | ZQSD (ou WASD selon le clavier) |
| Regarder | Souris |
| Attraper un déchet proche (main) | E — rapide mais il faut s'approcher (portée ~300) |
| Télékinésie | Viser un déchet éloigné + maintenir clic droit 2 s — lent mais à distance |
| Lancer | Clic gauche |

Les deux façons d'attraper doivent **changer la stratégie** du joueur (critère « multimodalité ») :
certains déchets ne sont accessibles qu'à distance, et un chronomètre oblige à choisir.

## Poubelles (consignes de tri en France)
- **Verte** : verre (bouteilles, pots, bocaux).
- **Jaune** : tous les emballages (plastique, métal, carton, papier).
- **Grise** : ordures ménagères non recyclables.

## Progression prévue
1. Manche 1 : 3 déchets, sans chronomètre (apprentissage des commandes).
2. Manche 2 : 5 déchets, dont certains seulement accessibles par télékinésie.
3. Manche 3 : 6 déchets plus difficiles, avec chronomètre.
Fin : score, rappel des erreurs, bouton Rejouer.

## Règles de travail
- Blueprints rangés dans `Content/Jeu/`, préfixes : `BP_` (Blueprint), `IA_` (Input Action), `WBP_` (widget), `M_` (matériau).
- Commandes via **Enhanced Input** (ajouter les mappings à l'Input Mapping Context du template).
- Graphismes en « grey box » (formes simples) tant que le jeu n'est pas fonctionnel.
- Avancer **une petite étape à la fois** ; après chaque étape :
  1. sauvegarder (Save All),
  2. dire précisément quoi tester dans l'éditeur,
  3. proposer un message de commit Git en français,
  4. **attendre la validation** avant l'étape suivante.
- Ne jamais supprimer ou renommer un fichier existant sans demander.
- Expliquer simplement chaque choix technique (utile pour le rapport final).

## Critères à respecter (grille d'évaluation)
Mécanique innovante, prototype jouable, versioning régulier, boucle de gameplay complète,
multimodalité réelle, jeu testable sans aide, dimension pédagogique visible, rapport sur les difficultés.
