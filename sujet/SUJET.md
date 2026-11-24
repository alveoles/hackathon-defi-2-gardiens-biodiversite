---
defi: 02
titre: FLOR LA RÉNYON
accroche: if (reconnue(photo)) { raconter(plante); }
sous_titre: On ne protège bien que ce que l'on sait reconnaître.
version: v1
---

## Introduction

La Réunion abrite des plantes qui ne poussent **nulle part ailleurs sur Terre**. Une partie de l'île est classée au Patrimoine mondial de l'UNESCO pour cette raison. Derrière chacune de ces plantes, il y a un nom créole, une histoire, des usages transmis à l'oral de génération en génération, et ce savoir se perd plus vite que les plantes elles-mêmes.

Aujourd'hui, pour savoir ce qu'on a sous les yeux en forêt, il faut un botaniste, un guide papier, ou une application en ligne qui ne fonctionne plus dès que le réseau tombe, c'est-à-dire là où poussent justement ces plantes.

Ta mission, avec ton équipe : construire **Flor La Rényon**, une application (Proof of Concept) qui reconnaît une plante de l'île à partir d'une photo, puis raconte son nom, son histoire et ses usages. Tu peux livrer une application mobile ou une application web : la technologie est libre.

Le cœur du défi, c'est **l'entraînement et la reconnaissance par IA**. Le reste de l'application donne du sens au modèle, mais ne le remplace pas.

> ℹ️ **Périmètre de la journée :** **4 à 5 plantes de l'île**, dont le bois de maman, la fleur jaune, le tamarin des Hauts et le fanjan. La liste définitive est dans le Starter Pack.

Le projet repose sur trois piliers :

- **Reconnaître** (le cœur du défi) : un modèle que tu entraînes toi-même, qui donne la plante et son niveau de certitude
- **Raconter** : une fiche par plante, avec son nom, son histoire et ses usages
- **Explorer** : un carnet des plantes rencontrées et une carte de l'île, utilisables sans réseau

> ℹ️ Les applications grand public reconnaissent des dizaines de milliers d'espèces, mais dans le cloud, et ne disent rien de ce que ces plantes représentent ici. Tu as une journée pour construire celle qui reconnaît les plantes de l'île hors ligne et raconte leur histoire.

## Considérations techniques

Tu es libre sur les technologies, avec une contrainte : **tout tourne en local, sur ton ordinateur, avec des outils gratuits et open source.** Aucune API payante, aucun service en ligne obligatoire. Il te faudra au minimum :

- un **modèle de vision** entraîné par ton équipe : embeddings pré-entraînés + classifieur, ou fine-tuning léger
- une **base de fiches locale** : le contenu des plantes est préparé à l'avance et lisible sans réseau
- une **interface mobile** (application ou web) qui suit les maquettes fournies

> ℹ️ L'open source est ton meilleur allié : des modèles de vision pré-entraînés existent déjà, certains spécialisés sur le vivant. Mais chaque heure passée à installer est une heure en moins pour construire. Choisis vite, et tiens-toi à ton choix.

> ℹ️ Un modèle à 99 % de précision sur ses propres photos d'entraînement ne prouve rien. Seul compte le score sur des photos qu'il n'a jamais vues.

> ℹ️ Dans un navigateur, la caméra en direct et la géolocalisation ne fonctionnent qu'en HTTPS ou sur `localhost`. Teste tôt sur un vrai téléphone.

> ℹ️ Branches, pull requests, commits lisibles. Ton historique git fait partie du rendu.

### Pistes locales

Ces pistes tournent sur un MacBook M1 Pro 16 Go. Elles sont un point de départ, pas une obligation.

| Étape | Piste | Autres pistes |
|---|---|---|
| Embeddings | `open_clip` (ViT-B/32) ou SigLIP | BioCLIP, DINOv2 |
| Classifieur | k-NN ou régression logistique (`scikit-learn`) sur les embeddings | Prototypes par classe (moyenne des embeddings) |
| Fine-tuning | `timm` (MobileNetV3, EfficientNet) sur le backend MPS de PyTorch | Apple Create ML, Keras |
| Cas « pas sûr » | Seuil sur la certitude ou sur l'écart entre les deux premières plantes | Classe « autre » entraînée sur des photos négatives |
| Fiches | Fichiers JSON ou SQLite, versionnés dans le repo | Markdown + front-matter |
| Interface | Page web mobile-first + FastAPI ou Flask | Flutter, React Native, Expo, Gradio |
| Photo sur téléphone | `<input type="file" accept="image/*" capture>` | `getUserMedia` en HTTPS (`mkcert`) |
| Hors ligne | Service worker (PWA) | Modèle exporté en ONNX (`onnxruntime-web`) ou Core ML |
| Carte | Leaflet + contour de l'île en GeoJSON local | Image SVG de l'île, MapLibre |

> ℹ️ Avec 5 plantes et quelques centaines de photos, les embeddings + k-NN donnent un premier résultat en moins d'une heure. Commence par là, puis décide si le fine-tuning vaut le temps qu'il coûte.

> ℹ️ Calcule les embeddings une seule fois et enregistre-les sur disque. Les recalculer à chaque essai te coûtera ta journée.

## Starter Pack

Dans le repo :

- `maquettes/` : les 10 écrans de l'application, numérotés dans l'ordre du parcours. Les textes entre crochets (`[XX] %`, `[PLANTE 2]`…) sont les valeurs que ton application doit remplir
- `starter-pack/ESPECES.md` : les plantes retenues, avec nom créole, nom scientifique et statut
- `starter-pack/data/` : un dossier de photos par plante, et un dossier `autre/` avec des plantes de l'île qui leur ressemblent
- `starter-pack/fiches/` : les textes et médias de chaque plante (nom, histoire, usages, où la voir), fournis par le porteur du défi
- `starter-pack/ATTRIBUTIONS.csv` : auteur, licence et lien source de chaque photo et de chaque texte

À la racine : `INSTALL.md` (installation pas à pas) et `check_setup.py` (vérifie que chaque brique tourne sur ta machine).

> ℹ️ Les photos viennent du terrain et de contributeurs bénévoles : cadrages variés, flou, mauvaise lumière, parfois plusieurs plantes sur la même image. Ton modèle doit faire avec.

> ℹ️ Photos, textes et vidéos ont chacun leur licence. Garde `ATTRIBUTIONS.csv` dans ton repo, et n'ajoute aucun contenu dont tu ne connais pas la licence, y compris venant de Wikipédia.

## Fonctionnalités

Les fonctionnalités sont formulées en **user stories** et regroupées par **palier**. À l'intérieur d'un palier, **elles ne sont pas triées** : à ton équipe de les prioriser. Elles sont validées par ton Product Owner pour le seul sprint de la journée.

> ℹ️ Un palier stable et démontrable vaut plus que trois paliers commencés. Ne passe au suivant qu'une fois le précédent solide.

> ℹ️ Le palier 1 est le cœur du défi. Une application superbe autour d'un modèle qui se trompe ne sert à rien. Un modèle fiable avec une interface simple, si.

> ℹ️ Toutes les user stories sont-elles claires ? Les comprenez-vous tous de la même façon ?

### Palier 1 : reconnaître (écrans 01 à 03)

- En tant que promeneur, je veux prendre une plante en photo ou choisir une photo dans ma galerie, afin de lancer la reconnaissance
- En tant que promeneur, je veux voir le nom de la plante reconnue et son niveau de certitude en pourcentage, afin de savoir si je peux m'y fier
- En tant que promeneur, je veux voir les autres possibilités avec leur pourcentage, afin de comparer
- En tant que promeneur, je veux que l'application me dise « pas encore sûr » quand elle hésite, ou quand la plante n'est pas l'une des plantes connues, afin de ne pas repartir avec un mauvais nom
- En tant que porteur du projet, je veux connaître la précision du modèle sur un jeu de test séparé, plante par plante, afin de savoir si je peux lui faire confiance

### Palier 2 : raconter (écrans 04 à 06)

- En tant que promeneur, je veux ouvrir la fiche de la plante reconnue, afin de découvrir l'histoire de son nom, où la voir et comment la reconnaître
- En tant que promeneur, je veux consulter les usages traditionnels de la plante, toujours accompagnés de la précaution santé, afin d'apprendre sans me mettre en danger
- En tant que promeneur, je veux parcourir le répertoire des plantes, filtrer (endémiques, indigènes, tisanes) et chercher par nom ou par usage, afin de découvrir les plantes de l'île sans photo
- En tant que porteur du projet, je veux que les fiches soient stockées dans une base locale, séparée du code, afin de les enrichir sans toucher à l'application

> ℹ️ Base de données préparée à l'avance ou recherche sur Internet ? L'une marche en forêt, l'autre dépend du réseau. Fais un choix d'équipe, et sache le défendre.

### Palier 3 : explorer (écrans 08 et 09)

- En tant que promeneur, je veux que chaque plante reconnue rejoigne mon carnet avec le lieu et la date de la rencontre, afin de garder la trace de mes découvertes
- En tant que promeneur, je veux voir sur une carte de l'île les zones où pousse chaque plante et l'endroit où j'ai pris mes photos, afin de savoir où la croiser
- En tant que gardien de la biodiversité, je veux que la carte n'affiche jamais de point exact pour une plante menacée, seulement une zone large, afin de ne pas guider les cueilleurs
- En tant que promeneur, je veux que la reconnaissance, les fiches et le carnet fonctionnent sans réseau, afin d'utiliser l'application sur le sentier

> ℹ️ Une position GPS précise d'une plante rare, publiée sans précaution, peut suffire à la faire disparaître. Le floutage n'est pas un détail d'affichage.

### Bonus : aller plus loin

- En tant que promeneur, je veux répondre à trois questions simples (le port, la sève, la feuille) quand la photo ne suffit pas, afin de trouver la plante avec la clé d'identification (écran 07)
- En tant qu'enseignant, je veux un quiz sur les plantes de l'île (photo, nom créole, usage) avec un score, afin de faire jouer ma classe (écran 10)
- En tant que promeneur, je veux regarder la vidéo du témoin depuis la fiche, afin d'entendre l'histoire de la plante racontée de vive voix
- En tant que porteur du projet, je veux ajouter une nouvelle plante à partir d'une vingtaine de photos et d'une fiche, sans réentraîner tout le modèle, afin de faire grandir le répertoire
- En tant que promeneur, je veux que la fiche se complète depuis Wikipédia quand le réseau est disponible, afin d'en savoir plus, sans que l'application en dépende

### …

Le porteur du défi a dessiné cette application et connaît ces plantes. Il sait lesquelles se confondent, quelles histoires méritent d'être racontées, et qui utilisera l'application sur le terrain. Tout n'est pas écrit dans ce sujet, ni dans les maquettes.

> ℹ️ Ton client est dans la salle. Va lui poser tes questions, et sois prêt à formuler de nouvelles user stories.

> ℹ️ À 17h00, ton application sera testée sur des photos qu'elle n'a jamais vues.

## Livrable : le parcours complet

À **17h00**, démonstration live, sur ton propre téléphone ou laptop (aucun matériel n'est prêté), du parcours des maquettes :

- une **photo prise en direct** d'une plante ou d'une photo de plante (écran 01)
- le **résultat** : nom de la plante, niveau de certitude, autres possibilités (écran 02)
- un cas **« pas encore sûr »** correctement géré (écran 03)
- la **fiche** de la plante et son onglet **Usage** (écrans 04 et 05)
- le **score sur le jeu de test**, plante par plante
- *(palier 3)* le **carnet** et la **carte**, la connexion coupée

Côté repo : un `README` avec les instructions de lancement et la procédure pour reproduire l'entraînement, des PR fusionnées et un tag **`v1.0-rendu`** avant la démo.
