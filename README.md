# Borne visserie Hubo : dossier de passation

`index.html` est une page unique, sans installation. Elle se publie telle quelle sur GitHub Pages (dépôt, `index.html` à la racine, Réglages > Pages).
Le code QR est intégré (aucun service externe). Hors de Claude, la synchronisation entre l'écran client et l'écran conseiller ne fonctionne pas : la page reste en « Démo locale ».

## Ce que fait la borne
Le client choisit son besoin (accrocher au mur, assembler du bois, cloison en plaque de plâtre, métal ou tôle, boulonner, identifier une vis), répond à 3 à 6 questions, puis reçoit une fiche : pièces à prendre, taille exacte, finition, allée, outils nécessaires, conseils. Les cas de sécurité (charge très lourde, élément porteur) sont orientés vers un conseiller.

| Écran | Rôle |
|---|---|
| Borne client | Questions à grosses cartes, schémas cotés, liste à emporter, avis « cela vous a aidé ? », appel d'un conseiller avec le résumé des réponses |
| Conseiller | Demandes triées par attente, avec ce que le client a déjà répondu |
| Analyse | Besoins fréquents, questions où les clients abandonnent, fiches les plus demandées (données simulées + essais en direct) |
| Règles | Les 93 recommandations de la borne, à valider une par une par l'expert visserie |

## Expérience client (nouveautés)
- **Accueil FR / NL** : écran de bienvenue avec le logo Hubo et 4 étapes, le client choisit sa langue en un geste. Retour automatique à cet écran après 75 s d'inactivité.
- **« Je ne sais pas »** sur les questions où le client hésite (poids, lieu, épaisseur, matériau, ossature…) : la borne retient une hypothèse prudente, l'affiche en haut de la fiche (« Hypothèses retenues ») et sur la liste imprimée. Les valeurs par défaut sont dans la constante `IDK`.
- **Ma liste de courses** (bouton en haut à droite) : « Tout ajouter » ou article par article, choix de la référence en rayon, matériel (embouts, forets…) ajoutable, quantités, total estimé, regroupement par allée. Récapitulatif en bas de la fiche avec **code QR** (téléphone), **impression** (PC) et **copie**. La liste est conservée 60 min si le client revient.
- **Recherche libre** à l'accueil (« télé », « terrasse », « miroir »…) vers le bon parcours, ou vers tout le catalogue. Les mots-clés sont dans `SEARCH`.
- **Comparateur** de 2 à 3 références (prix du paquet et à la pièce, diamètre, longueur, finition, marque, avis, allée).
- **Comment faire** : étapes de pose et durée estimée (constante `PLAN`).
- **Lecture facile (Aa)** : grosses cartes, texte agrandi, une colonne.
- **Démo guidée** (bouton « Démo guidée » de l'accueil) : parcours commenté de 35 s pour une présentation. Elle n'est pas comptée dans les statistiques.
- **Format tablette paysage** : tout l'écran est utilisé, sans défilement de la page.
- **Analyse** : où les clients cliquent sur « Je ne sais pas », matériel ajouté aux listes (ventes additionnelles), impressions, copies, comparaisons, et **tailles conseillées sans référence exacte au catalogue** (pistes d'assortiment).

## Avant tout pilote : valider les règles
Toutes les tailles (longueur de vis, diamètre de cheville, perçage, finition inox) viennent de règles de bricolage courantes et **n'ont pas été validées par Hubo**. L'écran « Règles » liste chaque situation et sa recommandation ; l'expert la valide ou la marque « à corriger ». L'export CSV sert de compte rendu.

## Fiches produit et emplacement en rayon
Les références s'affichent comme des fiches produit : photo, prix, nombre de pièces, diamètre et longueur visibles d'emblée. Toucher la vis ouvre toutes ses caractéristiques (marque, EAN, finition, empreinte, tête, filetage, usage, couleur, poids, avis). Le bouton « Emplacement dans le rayon » affiche l'allée et un plan schématique. **L'emplacement est un exemple** (allée selon la famille de vis) : il faut le remplacer par le plan réel du magasin (constantes `AISLES` et `FAMAISLE`).

## Catalogue complet consultable
Le bouton « Parcourir tout le catalogue » de l'accueil ouvre les 1 479 références : recherche par nom, référence ou taille (« 5x50 »), filtres par famille, diamètre, longueur et finition, tri par prix.

## Catalogue réel intégré (fichier hubo_vis_1.xlsx)
Les 1 479 vis du fichier et leurs photos sont intégrées (vignettes en 4 planches dans `index.html`, données dans le tableau `CAT`). Pour chaque vis recommandée, la borne propose jusqu'à 3 références Hubo : même diamètre et même longueur, bonne finition (inox pour l'extérieur humide ou le bord de mer), marque Hubo en premier, puis prix à la pièce croissant. Si la longueur exacte n'existe pas, elle propose la plus proche au-dessus et l'indique. Les 4 cas sans référence sont les vis de construction en inox Ø 8 × 120 (absentes du fichier).
Le fichier ne contient que des vis : **les chevilles, ancrages, boulons, écrous et rondelles n'ont pas encore de référence** (à fournir sous le même format). Le fichier ne donne pas les stocks par magasin ni l'allée réelle.
Pour mettre à jour : régénérer `CAT` et les planches depuis un nouvel export (même colonnes).

## Ce qu'il faut brancher
1. **Stocks et prix par magasin** : relier les références à l'API ou à l'export du magasin pilote.
2. **Rangement des allées** : les allées A à D sont un exemple (A vis à bois et plaques, B chevilles, C boulons et écrous, D métaux et rivets). À remplacer par le plan du magasin (constante `AISLES`).
3. **Règles** : les tables sont en tête du script (`STD_SCREW`, `PLUGS`, `CLEARANCE`, `TAPDRILL`…) et les parcours dans `FLOWS`. Une règle corrigée se change à un seul endroit.
4. **Demandes d'aide et évènements** : remplacer le stockage de démonstration par une API temps réel et des notifications sur téléphone.
5. **Production** : mode kiosque verrouillé, application installable, mises à jour à distance.

## Évènements enregistrés
`start` (parcours démarré), `step` (réponse à une question), `done` (fiche obtenue), `esc` (orienté conseiller), `fb` (avis oui/non), `help` (appel d'un conseiller), `idk` (« Je ne sais pas » : parcours et question), `proj` (liste : `add`, `tool`, `print`, `copy`, `open`), `cmp` (comparateur), `resume` (liste reprise). Aucune donnée personnelle, aucun compte.

## À fournir par Hubo
Charte graphique, plan du magasin pilote, extrait du catalogue visserie avec références, rangement réel des allées, validation des règles par un responsable visserie, type de tablette.
