# SUNLINK OS

Page d'entrée de SUNLINK OS (Groupe SUNLINK).

Ce dépôt ne contient que la page publique d'entrée. L'application et toutes ses données sont stockées
dans un espace sécurisé (Supabase, hébergé à Paris), accessible uniquement après connexion aux personnes autorisées.

`env.js` contient l'adresse du projet et sa clé publique (anon), publiques par conception : la sécurité est
assurée par les règles d'accès côté base de données.

## Ce que fait la page

0. **Animation d'entrée** (avant le formulaire) : le soleil du logo officiel SUNLINK Energies se lève, puis le logo complet
   et « SUNLINK OS · Pilotage du Groupe » (2,8 s au plus ; fixe puis fondu si le système demande moins de mouvement).
   « Passer », un clic, Échap, Entrée ou Espace l'arrêtent ; la page se charge dessous. Une fois par onglet.
Apparence (5.7) : thème « Ciel d'été » des maquettes validées (slides 1 à 4) : barre blanche avec le logo officiel, page d'accueil
avec accroche, carte « Se connecter », espaces de l'application et garanties ; cartes société à leur couleur.

1. **Connexion** : adresse e-mail, puis lien ou code (6 à 10 chiffres) reçu par e-mail. Aucun mot de passe.
2. **Un seul appel** après connexion : `rpc('slos_accueil')` (niveau, sociétés autorisées, demande d'accès).
3. **Membres** : le sélecteur de société est la première page. La vue Groupe (sociétés consolidées) est proposée
   en option aux rôles Éditeur et Propriétaire, qui peuvent la masquer.
4. **Nouveaux inscrits** : formulaire de demande d'accès (`rpc('slos_demander_acces')`), puis suivi de la demande.
   Le propriétaire accepte ou refuse les demandes depuis le sélecteur (`rpc('slos_traiter_demande')`).
5. **Rien ne se perd** : avant d'ouvrir l'application, la page relit les données enregistrées dans la base
   (8 secondes au plus), les fusionne avec celles du navigateur (la plus récente gagne, copie de sécurité des
   valeurs jamais synchronisées), puis télécharge l'application depuis l'espace sécurisé.

## Ce que la page transmet à l'application (même onglet)

| Clé | Où | Contenu |
| --- | --- | --- |
| `slos.choix` | sessionStorage | `{id, nom, at, proprio}` : société choisie (`id` = `groupe` pour la vue Groupe) ; `proprio` : `true` pour le propriétaire (l'application choisit sa page d'arrivée). Présente au rechargement : l'application s'ouvre directement. Pour revenir au sélecteur : la retirer puis recharger. |
| `slos.intro` | sessionStorage | `1` : l'animation d'entrée a été jouée dans cet onglet ; l'application, écrite ensuite dans le même onglet, ne la rejoue pas. |
| `slos.introUi` | localStorage (lu) | `{"coupee":true}` si la personne a coupé l'animation dans l'application. |
| `slos.apousser` | sessionStorage | tableau JSON des clés (sans le préfixe `slos.`) présentes seulement dans ce navigateur ou plus récentes que la base : l'application les envoie. |
| `slos.sync.alerte` | sessionStorage | phrase écrite si les données de la base n'ont pas pu être relues (délai de 8 s, erreur) : l'application s'est ouverte avec les données du navigateur. |
| `slos.selecteur.groupe` | localStorage | `0` vue Groupe masquée, `1` affichée (par défaut). |
| `slos.selecteur.dernier` | localStorage | identifiant de la dernière société ouverte, mise en avant. |
| `slos.__bak.<clé>` | localStorage | copie de sécurité d'une valeur locale remplacée ou supprimée par une version plus récente de la base. |
| `slos.__uid` | localStorage | dernier compte synchronisé dans ce navigateur (si un autre compte se connecte, rien n'est envoyé sous son nom). |

Tests automatiques (dépôt privé de l'application) : `tests/t94-entree.js`, `tests/t99-vision-entree.js` (animation d'entrée).
