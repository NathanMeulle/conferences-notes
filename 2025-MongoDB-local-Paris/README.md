# MongoDB Local Paris 2025

## Table of Contents

- [Mardi](#mardi)
    - [Du local au cloud : une boîte à outils qui vous accompagne tout au long du cycle de développement](#du-local-au-cloud--une-boîte-à-outils-qui-vous-accompagne-tout-au-long-du-cycle-de-développement)
    - [Keynote](#keynote)
    - [Transactions ACID dans MongoDB: deep dive et meilleures pratiques](#transactions-acid-dans-mongodb-deep-dive-et-meilleures-pratiques)
    - [Index MongoDB et plans d'exécutions pour des requêtes optimales](#index-mongodb-et-plans-dexécutions-pour-des-requêtes-optimales)
    - [Passer à l’échelle : comment MongoDB Atlas accompagne votre croissance](#passer-à-léchelle--comment-mongodb-atlas-accompagne-votre-croissance)
    - [MongoDB: mythes vs. vérité, pour mieux comprendre le fonctionnement](#mongodb-mythes-vs-vérité-pour-mieux-comprendre-le-fonctionnement)
    - [La modélisation des données pour MongoDB : méthodologie et fondamentaux](#la-modélisation-des-données-pour-mongodb--méthodologie-et-fondamentaux)
    - [PostgreSQL ou MongoDB? Un regard technique au-delà des clichés](#postgresql-ou-mongodb-un-regard-technique-au-delà-des-clichés)

## Mardi

### Du local au cloud : une boîte à outils qui vous accompagne tout au long du cycle de développement
4 novembre 2025 - 09:00-09:45CET - Intermédiaire - Baobab 1
> #### Abstract
>
> Augmentez la productivité de vos développeurs avec MongoDB, la boîte à outils permettant de gérer vos workflows aussi bien en local qu’à grande échelle, en production.
>
>Depuis la mise en route via Atlas CLI jusqu’à l’exploration de données assistée par IA avec MongoDB MCP Server et Vector Search, MongoDB simplifie le prototypage, les tests, la CI/CD afin d’assurer un passage à l’échelle de vos applications sans friction.
>
>Découvrez dans cette session comment construire une application tirant partie de l’IA en quelques minutes, tout en apprenant les stratégies à appliquer pour rationaliser le développement avec MongoDB.

> #### Speaker Bio : Bruno Guedes
>Senior Solutions Architect
>MongoDB

```markdown
#### Notes
⸻
Developer Workflow :
- Configuration de l’environnement de développement et préparation des données
- Écriture et test du code applicatif avec des outils intégrés et des outils d’IA
- Automatisation des tests pour valider les fonctionnalités et garantir une intégration fluide dans le workflow

Environnement de développement :
- Nécessité d’un cluster MongoDB local
- Utilisation d’Atlas Search pour la recherche full-text, et Vector Search pour la recherche sémantique sur les datasets
- Déploiement via Atlas CLI avec la commande `atlas deployment setup` (nécessite Docker)
- Résultat de la vectorisation : création d’un champ `embeddedMyField` lié au champ `MyField` pour la recherche vectorielle
- La dernière version de MongoDB permet d’effectuer simultanément des recherches full-text et sémantiques
- Olama : permet d’utiliser des LLM et des embeddings open source (effectue la vectorisation des recherches)
- Mise en place du serveur MCP pour MongoDB Atlas et la base de données (connexion via connection string dans les paramètres MCP)
- Possibilité de taguer `@Mongo /query` dans Copilot
- Plugin MongoDB pour IntelliJ : recommandations d’index disponibles directement dans le plugin

Intégration continue :
- GitHub Actions : service MongoDB avec images Docker pour exécuter les tests dans un environnement MongoDB dockerisé
- Échec des tests E2E dû à une durée de requête trop longue
```
#### Avis ⭐⭐⭐☆☆
Plusieurs problèmes techniques lors de la démonstration, mais la présentation reste intéressante, bien qu’un peu en décalage avec la description initiale.


### Keynote
4 novembre 2025 - 10:00-11:00CET - Amphitéâtre Epicéa
> #### Abstract
>
> À quoi ressemble la base de données idéale à l'ère de l'IA ? Rejoignez les experts et leaders MongoDB pour le découvrir. Explorez les dernières nouveautés produit, que ce soit des fonctionnalités natives autour de l'IA comme Vector Search ou les modèles d'embedding Voyage AI, mais aussi les gains de performances qui s'appliquent via une présence cloud dans plus de 120 régions. Grâce à des cas d'utilisation concrets et aux analyses techniques approfondies de nos équipes produit et ingénierie, découvrez pourquoi MongoDB est la fondation pour la création d'applications modernes d'aujourd'hui et de demain.

> #### Speaker Bio : Boris Bialek (Field CTO, Industry Solutions), Fred Roma (Senior Vice President, Atlas Data Services), Daniel Coupal (Senior Staff Developer Advocate), Bastien Nurit (Regional Director, Enterprise, Growth) - MongoDB

```markdown
#### Notes
⸻
Keynote - Boris Bialek
- Introduction sur les changements fréquents autour de l’IA
- Notion d’agents

Daniel - MongoDB 8.2
- Queryable Encryption : ajout des recherches avec préfixe/suffixe
- Support de version sur 3 ans, 5 ans pour les versions Long Term Support (LTS - version 8)

Fred Roma - IA
- Différences entre le modèle de voyage pour construire l'embedding utilisé par les agents et la recherche
- Textuel : modèle de voyage 3.5
- Multimodal : modèle multimodal de voyage 3.5
- Contextuel : modèle de contexte 3
- Reranker : rerank 2.5
- Base de données d'embeddings, reranker vectoriel, texte déjà inclus dans une plateforme
- Cf. repo 2

Application Modernisation Platform

```
#### Avis ⭐⭐⭐⭐☆
Une keynote dynamique et intéressante, à l'image de celles d'Apple.


### Transactions ACID dans MongoDB: deep dive et meilleures pratiques
4 novembre 2025 - 11:15-12:00CET - Intermédiaire - Acacia 5
> #### Abstract
>
> Comment MongoDB implémente-t-il réellement les transactions? Nous explorerons le fonctionnement interne du moteur de stockage WiredTiger, la gestion des conflits dans les transactions single-document, et le comportement des transactions multi-documents — différents mais aussi puissants que les SGBD classiques. Une session riche en détails pour comprendre les choix techniques de MongoDB et en tirer le meilleur dans des applications hautement performantes.

> #### Speaker Bio : Franck Pachot
>Staff Developer Advocate
>MongoDB

```markdown
#### Notes
⸻
Transactions ACID dans MongoDB - Deep Dive

- Introduction sur les transactions dans les bases de données : notion d'isolation. Contrairement aux bases relationnelles, MongoDB ne verrouille pas, mais gère les conflits. Une opération multiple peut rencontrer les mêmes problèmes de concurrence.

- Transactions sur un seul document :
  - Pour initier une transaction (T1), il faut d'abord créer une session (`db.getMongo.startSession`, `session1.startTransaction`).
  - Une fois la transaction démarrée, les opérations effectuées (comme un insert) ne sont pas visibles par les autres sessions tant qu'elles ne sont pas validées (commit).
  - Si une autre session tente de modifier les mêmes données, un conflit d'écriture (Write Conflict) se produit, entraînant l'annulation de la transaction (abortTransaction) et la nécessité de recommencer.
  - La transaction ne démarre réellement qu'à la première écriture. Dans un système relationnel, cela aurait entraîné un verrouillage en attente.

- Démonstration 1 :
  - Session 1 : mise à jour avec T explicite (update x+1)
  - Session 2 : mise à jour sans T explicite (update one x+1) => semble bloquée
  - Plus de 300 conflits détectés, MongoDB réessaie en interne. Le timeout par défaut d'une transaction MongoDB est de 1 minute (modifiable avec `transactionLifeTimeLimitSeconds` pour toute la base de données).
  - Les retries sont transparents avec un backoff exponentiel. Les 4 premières tentatives se font sans délai. MongoDB est optimisé pour des transactions courtes. Toute transaction sera annulée si le timeout est atteint, entraînant une erreur au commit.

- Démonstration 2 : utilisation d'un pipeline d'agrégation pour visualiser le nombre de conflits.

- Transactions multi-documents :
  - Ce n'est généralement pas une bonne pratique, car MongoDB n'écrit pas sur disque tant que ce n'est pas validé (contrairement à PostgreSQL). Cela peut entraîner une pression mémoire.
  - Une solution possible évoquée serait une gestion au niveau applicatif (par batch et/ou avec un flag).

- Sur Atlas, comment analyser ces requêtes ? Comment s'assurer que nos transactions sont courtes ? Pas de solutions claires pour le moment.
- Le temps de rollback est-il toujours court ou dépend-il de l'opération ? Les modifications sont effectuées en mémoire et les versions sont conservées, ce qui permet un rollback rapide.

- Les transactions lentes ? Un benchmark entre PostgreSQL et MongoDB n'est pas pertinent dans ce contexte.
- Avec Spring Data : annotation `@Retryable`, activer `enableRetry`, utiliser `MongoTxManager`.

- En résumé :
  - ACID ? Atomicité : oui, visible au commit ou rollback automatique. Cohérence : oui, la validation du schéma s'applique immédiatement. Isolation : par snapshot (pas de sérialisation). Durabilité : oui, les écritures sont synchronisées sur le disque local.
  - Persistance : pas de force, pas de vol, pas de persistance des changements non validés.

- Pression mémoire : éviter les transactions longues, pas besoin de vacuum pour les changements non validés.
```
#### Avis ⭐⭐⭐⭐☆
Présentation très claire et détaillée sur les transactions dans MongoDB, avec des démonstrations pertinentes. Quelques détails supplémentaires sur les mécanismes internes de gestion des transactions auraient été appréciés.

##### [Lien des slides](https://docs.google.com/presentation/d/1jmQE-q8pNdHazQekYESQGTVFtJHwD8RmvXMQR8TYD4g/mobilepresent?slide=id.g39f8dceade9_0_3051)


### Index MongoDB et plans d'exécutions pour des requêtes optimales
4 novembre 2025 - 12:10-12:55CET - Intermédiaire - Acacia 5
> #### Abstract
>
> Les gains de performance les plus spectaculaires viennent souvent d’un meilleur usage des index, pour éviter de lire des données inutiles. Dans cette session, nous irons en profondeur sur: la conception d’index composés, l’analyse de plans d’exécution, l’utilisation des hints… sur des exemples concrets.

> #### Speaker Bio : Franck Pachot
>Staff Developer Advocate
>MongoDB

```markdown
#### Notes
⸻
- Dans MongoDB, tous les documents d'une collection sont stockés avec un identifiant interne et dans le champ `_id`.
- Utilisation du `record_id` en interne, basé sur un arbre B (B-tree). Le `record_id` est en fait une séquence qui s'incrémente. Ainsi, deux petits documents insérés simultanément auront des `record_id` proches.
- Bien que l'on ne contrôle pas la proximité des documents insérés, deux documents insérés en même temps auront des `record_id` assez proches.

- Single Collection Design :
  - Tout mettre dans la même collection n'assure pas nécessairement que cela ira au même endroit, sauf en cas d'insertion simultanée.

- Démonstration : création d'une collection, recherche sans tri, retour dans l'ordre d'insertion basé sur le `record_id`.
  - Possibilité d'afficher le `recordID` avec `find().showRecordId()` (incrémental par collection). Si le `record_id` 3 est supprimé, il ne sera pas réutilisé.
  - Le `record_id` peut être différent entre plusieurs réplicas.

- Analyse des plans de requête avec `find.explain().executionStats` :
  - Sans index : `COLLSCAN`.
  - Sans tri explicite, on peut voir quel index est utilisé en regardant l'ordre des résultats retournés.
  - Si `null` dans l'index, sauf si index sparse.

- Utilisation de `hint` pour forcer l'utilisation d'un index.

- En SQL, si le hint est incorrect, il n'est pas utilisé. En MongoDB, si le hint pointe vers un index inexistant, cela entraîne une erreur.

- Projection pour montrer la gestion de différents types de données dans l'index.

- Dans les plans de requête, on peut voir le `winning plan` et le `rejectedPlan`. MongoDB analyse les deux options et choisit la meilleure.
  - Avec `getPlanCache`, on peut retrouver le `winning plan`.

- MongoDB exécute-t-il toujours les requêtes jusqu'au bout ? Pas vraiment. Il s'arrête à 30% de la collection, 10K unités de travail ou 101 documents.
  - Le `winning plan` peut être jusqu'à 10 fois moins performant.

- L'index fonctionne également sur des opérateurs `$in`.

- Bonnes pratiques :
  - Minimiser le nombre total de documents examinés.
  - Minimiser le nombre total de clés examinées.
  - Éviter `sort.limit` pour la pagination.
  - Si plusieurs critères d'égalité pour filtrer, mettre le champ le plus restrictif en premier.
```
#### Avis ⭐⭐⭐☆☆
Présentation claire et intéressante sur les plans de requête et l'indexation dans MongoDB. Bien que certains concepts aient été déjà connus, d'autres ont apporté des précisions utiles.

##### [Lien des slides](https://docs.google.com/presentation/d/1jmQE-q8pNdHazQekYESQGTVFtJHwD8RmvXMQR8TYD4g/mobilepresent?slide=id.g39f8dceade9_0_3051)


### Passer à l’échelle : comment MongoDB Atlas accompagne votre croissance
4 novembre 2025 - 13:55-14:40CET - Intermédiaire - Baobab 1
> #### Abstract
>
> Découvrez comment MongoDB facilite la croissance de votre application, de sa création à son expansion à l’échelle mondiale. Nous commencerons par vous montrer comment tirer partie d'un cluster “Flex Tier” pour transformer vos idées en un MVP scalable. À mesure que les sollicitations de votre application augmentent, nous passerons à un déploiement multi-région et exploiterons la scalabilité horizontale de MongoDB Atlas via le “sharding”.
>
> Venez assister à des démonstrations de l’interface simple d’Atlas, ses APIs d’administration, sa compatibilité avec les outils d’IaC tels que Terraform, qui mettront en évidence la manière dont notre expérience DevOps intuitive vous permet de gérer et de faire évoluer votre base de données sans effort.

> #### Speaker Bio : Thomas Scott
>Senior Solutions Architect
>MongoDB

```markdown
#### Notes
⸻
Building for Scale

- De l'offre gratuite à l'offre flex avec M10 et autoscaling.
- Multi-régions, sharding.
- Exemple de Betclic :
  - Lecture après écriture.
  - Maintien de la connexion ouverte en définissant la taille minimale du pool de connexions.
  - Écriture en masse : création manuelle du document BSON pour plus de rapidité.
  - Tests avec Test Container.
```
#### Avis ⭐⭐⭐⭐☆
Intéressant, il manquait quelques indicateurs/métriques qui justifie les changements de cluster. REX betclic très intéressant.

### MongoDB: mythes vs. vérité, pour mieux comprendre le fonctionnement
4 novembre 2025 - 14:50-15:35CET - Intermédiaire - Amphitéâtre Epicéa
> #### Abstract
>
> Beaucoup de mythes entourant MongoDB datent de ses débuts… et sont aujourd’hui dépassés. Et si l’on confrontait ces idées reçues à la réalité technique? À travers des démonstrations concrètes, nous déconstruirons les préjugés les plus répandus et mettrons en lumière la vraie manière dont MongoDB gère la performance, la fiabilité et la cohérence des données.

> #### Speaker Bio : Franck Pachot
>Staff Developer Advocate
>MongoDB

```markdown
#### Notes
⸻
- Pas de schéma avec NoSQL ?
  En réalité, il y a un schéma flexible, avec possibilité de validation. Les bases relationnelles offrent également une certaine flexibilité (ex : `CREATE TABLE AS SELECT`).
  Lors de la création d'un index sur un champ `name`, cela implique implicitement l'existence d'un champ `name` dans le schéma. Des validateurs peuvent être mis en place pour s'assurer que les données respectent certaines regex.

- Seulement pour des cas clé-valeur ?

- Transactions lentes ?
  Depuis MongoDB 4.0, les benchmarks avec PostgreSQL montrent des résultats peu concluants en raison de fausses hypothèses et de tests inappropriés.

- Cohérence éventuelle ?
  MongoDB offre une garantie ACID par défaut.

- Fiabilité de MongoDB ?
  Utilisation des checksums WiredTiger, stockés dans chaque bloc et à chaque adresse.

- Jointures lentes ?
  Il est possible d'éviter les jointures en utilisant un modèle avec du "many" dans le "one".
  L'embedding simplifie les choses.
  La commande `lookup` est un peu plus rapide sur PostgreSQL, mais la flexibilité du schéma sur MongoDB peut rendre les choses légèrement plus lentes.

- Émulations MongoDB :
  Fonctionnent au-dessus des bases de données SQL, mais MongoDB présente plusieurs avantages en termes de stockage des documents.

- Limite de 16 Mo trop basse ?
  La taille idéale d'un document est bien en deçà de cette limite. Des documents de quelques centaines de Ko sont optimaux.

- Cohérence sans clé étrangère ?
  Les associations fortes sont intégrées.
```
#### Avis ⭐⭐⭐☆☆

##### [Lien des slides](https://docs.google.com/presentation/d/1jmQE-q8pNdHazQekYESQGTVFtJHwD8RmvXMQR8TYD4g/mobilepresent?slide=id.g39f8dceade9_0_3051)


### La modélisation des données pour MongoDB : méthodologie et fondamentaux
4 novembre 2025 - 15:50-16:35CET - Intermédiaire - Amphitéâtre Epicéa
> #### Abstract
>
> Si la création d'un schéma pour une base de données relationnelle est un processus simple, la conception d'un schéma pour une application MongoDB peut sembler un peu plus complexe. Cependant, ce n’est pas le cas si vous suivez la méthodologie et les principes fondamentaux que MongoDB a identifiés pour vous.
>
> Cette présentation passera en revue les principes de modélisation des données, et dévoilera des conseils supplémentaires issus d'innombrables “Design Reviews” que nous avons réalisées pour nos clients au cours des dernières années.

> #### Speaker Bio : Daniel Coupal
>Senior Staff Developer Advocate
>MongoDB

```markdown
#### Notes
⸻
- Introduction :
  Évolution des ensembles de données :
  - Années 1970 : 10 Mo
  - Années 1980 : 100 Mo
  - Années 1990 : 10 Go
  - Années 2000 : 1 To
  - Années 2010 : 100 To
  - Années 2020 : Po+

  Évolutions des besoins en performances (millions de clients), développement plus rapide (agile), sécurité (cyber et RGPD).

- Identification du type de projet :
  - Nombre d'opérations en écriture et en lecture, exigences de latence, volume de données, simplicité/agilité.

- Quatre points pour définir un schéma MongoDB :
  - Exigences/entités
  - Charge de travail
  - Relations
  - Patrons de conception

- Exigences :
  - Cas d'utilisation clients/produits.
  - Identification des entités fortes/principales (ex : clients, commandes, produits).
  - Tables de valeurs (ex : villes, pays).
  - Tables associatives (ex : contenant `orderId` et `productId`) : en MongoDB, on utilise des documents imbriqués (ex : `lineItems` pour les lignes de commandes).
  - Entités faibles (ex : adresses).

  => Le modèle résultant ne comportera pas autant de documents que prévu.

- Charges de travail :
  - Lister les opérations en écriture/lecture avec leur fréquence et latence.
  - Objectif : colocalisation des données pour des lectures rapides, précalcul des données.

- Relations :
  - Imbriquation (objets imbriqués, impossible en base relationnelle) ou référence (clé étrangère pointant vers un document, possible en base relationnelle).
  - Les données utilisées ensemble dans l'application doivent être stockées ensemble dans la base de données.

- Patrons de conception :
  Duplication des données : causes, impacts et stratégies de gestion. Liée à l'obsolescence. Quatre types de duplication :
  - Immuables (ex : date d'acquisition client).
  - Temporelles (ex : adresse client, sensibles à l'obsolescence).
  - Peu sensibles à l'obsolescence (prévoir des tâches périodiques pour mettre à jour les occurrences).

  - Compromis entre obsolescence et duplication des données (coût et complexité de maintien vs amélioration des performances et réduction des coûts).

  - Extended Reference Pattern : cf photo.

  - Computed Pattern : cf photo, ex : calcul de la note moyenne avec les différents documents de note.

  - Approximation Pattern : cf photo.

  - Archive Pattern : cf photo.

  - Schéma Versioning : cf photo, ajout d'un numéro de version dans le document.

  - Anti-pattern : ignorer la validation du schéma. Ajouter de la validation avec `jsonSchema` (ex : champs requis, propriétés sur les champs).

Conclusion :
Modélisation moderne, méthodologie de modélisation, patrons de conception.
```
#### Avis ⭐⭐⭐⭐☆
Présentation très intéressante sur la modélisation des données dans MongoDB, avec des exemples concrets et des bonnes pratiques.

### PostgreSQL ou MongoDB? Un regard technique au-delà des clichés
4 novembre 2025 - 16:45-17:15CET - Intermédiaire - Amphitéâtre Epicéa
> #### Abstract
>
> Les arbres de décision “SQL ou NoSQL” d’hier sont devenus obsolètes: aujourd’hui, les bases relationnelles savent stocker du JSON, et MongoDB gère les transactions ACID, les jointures et les requêtes complexes. Dans cette session, nous comparerons en profondeur les approches relationnelles et orientées documents, du point de vue de l’expérience développeur. Et comment choisir le bon modèle pour des applications modernes en Domain Driven Design.

> #### Speaker Bio : Franck Pachot
>Staff Developer Advocate
>MongoDB

```markdown
#### Notes
⸻
- Historique de MongoDB :
  En 2015, MongoDB a acquis WiredTiger pour optimiser le stockage des BSON.

- Le plus rapide ?
  Cela dépend du type et de la taille des documents.

- Créer du NoSQL sur PostgreSQL ?
  L'idée première de MongoDB n'est pas de gérer des tables, mais de stocker directement le document métier.
  La raison principale d'une base de données documentaire est la localité des données. MongoDB stocke l'ensemble du document au même endroit, contrairement à PostgreSQL qui stocke par défaut des blocs de 8 Ko, max 32 Ko.

- MongoDB : un seul type d'index physiquement (B-tree) ; PostgreSQL utilise des index Gin, plusieurs clés pour un document.
  - Fonctionne pour trouver une valeur dans un champ, mais pas pour des opérations supérieures à ou pour trier les valeurs.
  - Sur PostgreSQL, l'index du JSON n'est pas optimisé.

- Photos DDD

- Benchmarks publiés :
  Attention, comparaison difficile à faire. Sur du clé-valeur, ce n'est pas génial. Le schéma ne sera pas le même entre MongoDB et PostgreSQL.
  Haute disponibilité sur PostgreSQL pas optimale, des benchmarks sont nécessaires à ce sujet.

- Conclusion :
  Ce n'est pas simplement "j'ai du JSON à stocker" => MongoDB.
  Si DDD avec itérations fréquentes, pas besoin de synchronisation avec d'autres équipes => MongoDB peut être une bonne solution.
```
#### Avis ⭐⭐☆☆☆
Présentation réalisée en 30 minutes, très rapide, sans entrer dans les détails. Beaucoup de sujets intéressants méritant d'être approfondis.

##### [Lien des slides](https://docs.google.com/presentation/d/1jmQE-q8pNdHazQekYESQGTVFtJHwD8RmvXMQR8TYD4g/mobilepresent?slide=id.g39f8dceade9_0_3051)

