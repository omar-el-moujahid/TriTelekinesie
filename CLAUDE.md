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

## Progression (implémentée)
1. Manche 1 : 3 déchets, sans chronomètre (apprentissage des commandes).
2. Manche 2 : 5 déchets, dont 2 en haut de piliers (seulement accessibles par télékinésie).
3. Manche 3 : 6 déchets plus difficiles (pièges : verre à boire cassé → grise, pot de yaourt → jaune…), chronomètre 90 s.
Fin : score sur 14, rappel des erreurs avec explication, rappel des consignes, bouton Rejouer.

## État actuel (12/11/2026)
Le jeu complet est construit, compile sans erreur et a été partiellement testé en jeu.

### Assets créés
| Asset | Rôle |
|---|---|
| `Content/Jeu/E_TypeDechet` | Énumération : Verre, Emballage, OrduresMenageres |
| `Content/Jeu/BP_Dechet` | Cube physique + étiquette tournée vers le joueur. Variables publiques : `TypeDechet`, `NomDechet`, `Manche`, `Explication`. Événement `Activer(Actif)` pour l'afficher/cacher selon la manche |
| `Content/Jeu/BP_Poubelle` | Corps + zone de détection (Box). `TypeAccepte` choisit la couleur. À l'entrée d'un déchet : compare les types, construit le message « BRAVO ! / ERREUR : » + explication, prévient `BP_GestionPartie`, détruit le déchet |
| `Content/Jeu/BP_GestionPartie` | Placé dans le niveau. Gère les manches (`DemarrerManche`), le score, la liste des erreurs, le chrono (`DureeChrono` = 90), la fin (`FinPartie`) |
| `Content/Jeu/UI/WBP_HUD` | Panneaux semi-transparents : infos (manche, score, déchets restants) en haut à gauche, consigne courte en haut au centre, chrono en haut à droite (manche 3), message BRAVO/ERREUR en bas (disparaît après 6 s). Bandeau central « MANCHE X / 3 » animé en fondu (`AfficherBandeau` + animation `AnimBandeau`, 3,5 s). Écran de fin dans le même style (bandeau sombre pleine largeur) : verdict **RÉUSSI !** en vert ou **ÉCHEC** en rouge (`AfficherVerdict`, animation `AnimFin`), détail défilant, bouton REJOUER ; le reste du HUD est masqué et le jeu est mis en pause après l'animation. Réussi si `Score >= SeuilReussite` (10, réglable dans `BP_GestionPartie`) |
| `Content/Jeu/Input/IA_Attraper` (E), `IA_Lancer` (clic gauche), `IA_Telekinesie` (clic droit) | Input Actions |
| `Content/Jeu/Materiaux/M_Couleur` + instances `MI_Poubelle{Verte,Jaune,Grise}`, `MI_Dechet`, `MI_Pilier` | Couleurs grey box |

### Fichiers du template modifiés (ajouts seulement)
- `BP_FirstPersonCharacter` : composant `PoigneePhysique` (Physics Handle), fonctions `ViserDechet` (line trace depuis la caméra), `Lacher`, `ObtenirGestion`, événement `Attraper`, logique E / clic gauche / clic droit maintenu, Tick qui place le déchet tenu devant la caméra. Réglages publics (catégorie TriTelekinesie) : `PorteeMain` 300, `PorteeTelekinesie` 3000, `DureeTelekinesie` 2, `ForceLancer` 1200, `DistanceTenue` 150.
- `IMC_Default` : ajout de E, clic gauche, clic droit, et **Z / Q** sur `IA_Move` (claviers AZERTY).
- Niveau `Lvl_FirstPerson` : 3 poubelles, 4 piliers, 14 déchets (préfixes `M2_`, `M3_` pour les manches 2 et 3), `BP_GestionPartie`.

### Choix techniques à retenir
- **Attraper** = Physics Handle : le déchet garde sa physique et suit un point devant la caméra.
- **Lancer** = `AddImpulse` (Vel Change) dans la direction de la caméra.
- **Télékinésie** = pendant que le clic droit est maintenu, on additionne le temps passé à viser le même déchet ; à 2 s il est attiré.
- **Bug contourné** : les fonctions avec paramètres d'entrée créées via l'outil MCP perdaient leurs entrées à l'actualisation des nœuds. Toute logique à paramètres est donc en **Custom Event** ; `ViserDechet` reste une fonction (sortie seulement) et lit sa portée dans la variable `PorteeVisee`.

### Reste à faire / à vérifier
- Tester en jeu : passage manche 1 → 2 → 3, fin du chrono, écran de fin, bouton Rejouer.
- Le projet n'est **pas encore un dépôt Git** : faire `git init` + `.gitignore` Unreal, puis le premier commit.
- Après validation : polish (sons, effets visuels de télékinésie, vrais modèles 3D à la place des cubes).

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
