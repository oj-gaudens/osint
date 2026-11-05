# OSINT : le guide complet, étape par étape

> **OSINT** = *Open Source INTelligence*, soit le renseignement d'origine sources ouvertes.
> On collecte et on recoupe des informations **publiques et légalement accessibles** pour en tirer du renseignement exploitable.

⚠️ **Règle d'or** : ce guide est à but éducatif et défensif. Entraîne-toi sur **toi-même**, sur des **CTF** (Trace Labs, Sourcing Games, challenges OSINT FR) ou avec **l'accord écrit** de la personne ou de l'organisation. Chercher des infos sur quelqu'un qui n'est pas d'accord peut vite virer au harcèlement ou au doxing, et c'est puni par la loi.

---

## Sommaire

1. [Comprendre l'OSINT](#1-comprendre-losint)
2. [Le cadre légal (à lire avant tout)](#2-le-cadre-légal-à-lire-avant-tout)
3. [La méthode : le cycle du renseignement](#3-la-méthode--le-cycle-du-renseignement)
4. [Préparer son environnement](#4-préparer-son-environnement)
5. [Les outils mentionnés, un par un](#5-les-outils-mentionnés-un-par-un)
6. [Techniques avancées](#6-techniques-avancées)
7. [Études de cas réelles](#7-études-de-cas-réelles)
8. [Exercice pratique : ton auto-audit OSINT](#8-exercice-pratique--ton-auto-audit-osint)
9. [Se protéger de l'OSINT](#9-se-protéger-de-losint)
10. [Rédiger un rapport OSINT](#10-rédiger-un-rapport-osint)
11. [Pour aller plus loin](#11-pour-aller-plus-loin)
12. [Sources](#12-sources)

---

## 1. Comprendre l'OSINT

### C'est quoi, concrètement ?

Une information est « de source ouverte » quand n'importe qui peut y accéder sans pirater, sans contourner de protection, sans usurper une identité : réseaux sociaux publics, registres d'entreprises, images satellites, métadonnées de photos, archives web, forums, etc.

### Qui l'utilise ?

| Profil | Usage |
|---|---|
| **Journalistes / fact-checkers** | Vérifier une vidéo, géolocaliser un événement (ex. Bellingcat) |
| **Pentesters / Red Team** | Phase de reconnaissance : employés, emails, domaines, fuites |
| **Blue Team / RSSI** | Voir ce qu'un attaquant peut trouver sur l'entreprise |
| **Recruteurs / RH** | Vérification d'antécédents (dans le cadre légal) |
| **Enquêteurs / associations** | Retrouver des personnes disparues, traquer des arnaques |
| **Particuliers** | Vérifier un vendeur, une annonce, une arnaque |

### Ce que l'OSINT n'est PAS

- ❌ Pirater un compte
- ❌ Utiliser une base de données volée sans cadre légal
- ❌ Harceler, traquer, intimider
- ❌ Faire de l'ingénierie sociale pour obtenir des infos privées (c'est du *SOCINT / pretexting*, un autre sujet, avec ses propres risques légaux)

---

## 2. Le cadre légal (à lire avant tout)

### Doxing

Le **doxing** (ou « divulgation d'informations personnelles ») consiste à publier des informations permettant d'identifier ou de localiser quelqu'un dans le but de lui nuire. En France, c'est un délit prévu à l'**article 223-1-1 du Code pénal**, avec des peines pouvant aller jusqu'à plusieurs années de prison et des dizaines de milliers d'euros d'amende, et des peines plus lourdes si la victime est mineure ou dépositaire de l'autorité publique. *(Vérifie les montants exacts sur la page Service Public citée dans les sources.)*

### Autres textes à connaître

- **RGPD + loi Informatique et Libertés** (CNIL) : même si une donnée est publique, la **traiter** (la stocker, la croiser, la fichier) reste soumis à des règles : finalité, minimisation, durée de conservation.
- **Accès frauduleux à un système (art. 323-1 du Code pénal)** : entrer dans un espace non public (compte, serveur, base) sans autorisation est interdit, même si « la porte était mal fermée ».
- **Atteinte à la vie privée (art. 226-1)** et **usurpation d'identité (art. 226-4-1)**.
- **Harcèlement (art. 222-33-2-2)** : répéter des actions de surveillance ou de contact peut en relever.

### Les 5 questions éthiques avant chaque recherche

1. Ai-je un **objectif légitime** clairement défini ?
2. Les données sont-elles **réellement publiques** ?
3. Ai-je l'**accord** de la personne / de l'organisation ?
4. Ma recherche est-elle **proportionnée** à l'objectif ?
5. Que se passe-t-il si ces infos **tombent entre de mauvaises mains** ?

Si une réponse est « non » ou « je ne sais pas » : **stop**.

---

## 3. La méthode : le cycle du renseignement

L'OSINT sérieux n'est pas « je tape un nom sur Google ». Il suit un cycle :

```
 1. Objectif  →  2. Collecte  →  3. Traitement  →  4. Analyse  →  5. Rapport
      ↑                                                              │
      └──────────────────── retour / nouvelles pistes ───────────────┘
```

### Étape 1 : Définir l'objectif

Écris une question précise. Exemples :

- ❌ « Trouver des infos sur l'entreprise X »
- ✅ « Lister les employés de l'entreprise X visibles publiquement et le format de leurs adresses email, pour un audit de phishing autorisé »

### Étape 2 : Collecter

Pars de ce que tu as (un pseudo, un email, un nom, une photo, un domaine) et élargis :

```
Pseudo  ──►  autres comptes  ──►  photo de profil  ──►  recherche inversée
   │                                                          │
   ▼                                                          ▼
 email  ◄── mêmes pseudos sur forums ◄── lieux, habitudes, contacts
```

### Étape 3 : Traiter

- Note **chaque source** (URL, date, capture d'écran, hash si possible).
- Garde les données brutes, ne les modifie pas.
- Archive : `web.archive.org` (Wayback Machine) ou `archive.ph`, car les pages disparaissent.

### Étape 4 : Analyser

- **Recouper** : une info n'est fiable que si elle est confirmée par au moins **2 sources indépendantes**.
- **Attribuer un niveau de confiance** : confirmé / probable / possible / douteux.
- Attention aux **homonymes** et aux **faux comptes** (le piège n°1 des débutants).

### Étape 5 : Rapporter

Voir [section 10](#10-rédiger-un-rapport-osint).

---

## 4. Préparer son environnement

### Checklist de départ

- [ ] **Navigateur dédié** (Firefox ou Brave) avec un profil séparé de ton usage perso
- [ ] **Comptes « sock puppet »** si tu dois te connecter à des réseaux sociaux (jamais ton vrai compte, qui pourrait notifier la cible ou te lier à elle)
- [ ] **VPN** fiable pour ne pas exposer ton IP aux sites consultés
- [ ] **Machine virtuelle** (ex. Kali, Tails, ou la distro **Trace Labs OSINT VM**)
- [ ] **Outil de prise de notes** structuré (Obsidian, CherryTree, Joplin)
- [ ] **Captures horodatées** (extension *Fireshot*, *SingleFile*, ou `Ctrl+Shift+S`)

### Installation des outils en ligne de commande (Kali / Linux)

```bash
# Python et pip
sudo apt update && sudo apt install -y python3 python3-pip pipx git

# Sherlock
pipx install sherlock-project

# Holehe
pipx install holehe
```

Vérification :

```bash
sherlock --version
holehe --help
```

---

## 5. Les outils mentionnés, un par un

### 🔎 5.1 Sherlock : recherche de pseudo

- **Lien** : https://github.com/sherlock-project/sherlock
- **Rôle** : vérifie si un pseudo existe sur des centaines de sites (Instagram, GitHub, Reddit, TikTok, Steam, etc.).

**Pas à pas :**

```bash
# 1. Recherche simple
sherlock monpseudo

# 2. Plusieurs pseudos
sherlock pseudo1 pseudo2

# 3. Sauvegarder dans un fichier texte
sherlock monpseudo --output resultats.txt

# 4. Exporter en CSV (pratique pour trier)
sherlock monpseudo --csv

# 5. Passer par Tor (anonymat renforcé)
sherlock monpseudo --tor
```

**Comment interpréter les résultats :**

- Un résultat `[+]` = le pseudo **existe** sur le site, **mais ce n'est pas forcément la même personne**.
- Vérifie : photo de profil, date de création, style d'écriture, langue, centres d'intérêt, abonnés communs.
- Les **faux positifs** sont fréquents (sites qui renvoient une page 200 pour tout pseudo).

### 🔎 5.2 WhatsMyName : l'alternative web

- **Lien** : https://whatsmyname.app/
- **Rôle** : même principe que Sherlock, mais dans le navigateur, avec une base très maintenue par la communauté et des **filtres par catégorie** (gaming, rencontres, finance, etc.).

**Pas à pas :**

1. Ouvre le site, entre le pseudo.
2. Filtre par catégorie si tu veux cibler (ex. « gaming »).
3. Lance la recherche et attends la fin du scan.
4. Exporte les résultats (CSV/JSON) pour les garder dans ton dossier.

💡 **Astuce** : utilise les **deux** (Sherlock + WhatsMyName). Leurs bases de sites ne sont pas identiques, ça réduit les angles morts.

### 📧 5.3 Epieos : recherche inversée d'email

- **Lien** : https://epieos.com/
- **Rôle** : à partir d'un email, retrouve les comptes liés (Google, par exemple : nom du profil, photo, avis Google Maps, calendrier public, etc.), certains services, parfois des réseaux sociaux.

**Pas à pas :**

1. Entre l'adresse email.
2. Regarde les modules retournés (Google, Skype, services divers selon la version).
3. Si un compte Google existe, note l'**identifiant de profil** : il permet parfois de retrouver des **avis Google Maps**, qui révèlent des lieux fréquentés.
4. Croise avec ce que Sherlock a trouvé (même photo ? même nom ?).

### 📧 5.4 Holehe : sur quels sites cet email est-il inscrit ?

- **Lien** : https://github.com/megadose/holehe
- **Rôle** : teste si un email est enregistré sur des dizaines de sites, **sans envoyer d'email à la personne** (il exploite les messages d'erreur des formulaires « mot de passe oublié »).

```bash
# Test simple
holehe adresse@exemple.com

# N'afficher que les sites où le compte existe
holehe adresse@exemple.com --only-used

# Sortie CSV
holehe adresse@exemple.com --csv
```

**À savoir :**

- Résultat « Used » = un compte existe avec cet email sur ce site.
- Certains sites ont des protections anti-bot : un résultat « Rate limit » ne veut pas dire « non ».
- Utile aussi en défense : teste **tes propres emails** pour voir où tu es inscrit (et supprimer les vieux comptes).

### 🧩 5.5 OSINT Industries : l'outil tout-en-un

- **Lien** : https://osint.industries/
- **Rôle** : plateforme **payante** qui agrège beaucoup de sources (emails, téléphones, pseudos) dans un seul rapport.
- **Avantage** : gain de temps énorme.
- **Limites** : coût, dépendance à un outil tiers, et **responsabilité légale identique** : ce n'est pas parce qu'un outil le permet que c'est légal de l'utiliser sur n'importe qui.

### 🕸️ 5.6 Maltego : cartographier les liens

- **Lien** : https://www.maltego.com/
- **Rôle** : outil de **visualisation de graphes**. Tu poses une « entité » (domaine, email, personne, entreprise) et Maltego applique des **transforms** pour trouver les entités liées.

**Pas à pas (version Community, gratuite) :**

1. Crée un compte et télécharge Maltego.
2. Nouveau graphe : glisse une entité (ex. `Domain` → `exemple.fr`).
3. Clic droit → lance des *transforms* (DNS, WHOIS, sous-domaines, emails associés).
4. Les résultats s'affichent en **nœuds reliés** : tu vois en un coup d'œil la structure (serveurs, mails, personnes).
5. Nettoie le graphe (supprime les faux positifs) et **exporte** en image ou PDF pour ton rapport.

### ☀️ 5.7 SunCalc : analyse des ombres

- **Lien** : https://suncalc.org/
- **Rôle** : affiche la position du soleil (azimut, hauteur) pour **n'importe quel lieu, date et heure**.

**Pas à pas pour dater / localiser une photo :**

1. Repère une **ombre** dans la photo (poteau, bâtiment, personne).
2. Estime la **direction** de l'ombre par rapport aux repères (rue, bâtiment).
3. Sur SunCalc, place-toi au lieu supposé.
4. Fais varier la **date et l'heure** : la ligne jaune (direction du soleil) doit correspondre à l'ombre observée.
5. Tu obtiens une **plage horaire** ou une **saison** plausible, et tu peux écarter des lieux incompatibles.

💡 Combine avec la **longueur de l'ombre** : ombre courte = soleil haut (midi, été) ; ombre longue = soleil bas (matin/soir, hiver).

### 🗺️ 5.8 Overpass Turbo : interroger OpenStreetMap

- **Lien** : https://overpass-turbo.eu/
- **Rôle** : requêter la base **OpenStreetMap** pour trouver des lieux répondant à des critères précis (ex. « un pont à côté d'une église et d'une station-service »). C'est la technique reine de la **géolocalisation**.

**Exemple : trouver tous les châteaux d'eau dans une zone**

```
[out:json][timeout:25];
(
  node["man_made"="water_tower"]({{bbox}});
  way["man_made"="water_tower"]({{bbox}});
);
out body;
>;
out skel qt;
```

**Pas à pas :**

1. Zoome sur la zone de recherche (région, département).
2. Colle la requête, clique sur **Exécuter**.
3. Les résultats s'affichent sur la carte.
4. Compare avec les détails de ta photo (forme, environnement) pour affiner.

**Exemple combiné** (une station-service à moins de 200 m d'une église) : tu peux utiliser `around:200` pour chaîner deux critères.

```
[out:json][timeout:60];
node["amenity"="place_of_worship"]({{bbox}})->.eglises;
node(around.eglises:200)["amenity"="fuel"];
out body;
```

### 🏢 5.9 Pappers.fr et Societe.com : entreprises françaises

- **Liens** : https://www.pappers.fr/ et https://www.societe.com/
- **Rôle** : consulter les **données légales** d'une entreprise française (SIREN/SIRET, dirigeants, adresse du siège, capital, comptes annuels, annonces légales, procédures collectives).

**Pas à pas :**

1. Cherche par **nom**, **SIREN** ou **dirigeant**.
2. Note : forme juridique, date de création, adresse, dirigeants.
3. Regarde les **autres mandats** d'un dirigeant (réseau de sociétés).
4. Ouvre les **bilans** (si publiés) : chiffre d'affaires, effectifs.
5. Vérifie les **annonces BODACC** (modifications, radiations, liquidations).

💡 Utile pour : vérifier une société avant d'acheter/travailler avec elle, détecter une société « boîte aux lettres », cartographier les liens entre entités.

### 🇫🇷 5.10 OSINT FR : la communauté française

- **Lien** : https://osintfr.com/
- **Rôle** : association francophone, ressources, formations, événements et challenges. Un très bon point d'entrée pour progresser en France.

### 🧭 5.11 OSINT Framework : la carte de tous les outils

- **Lien** : https://osintframework.com/
- **Rôle** : arborescence interactive qui classe les outils par type de donnée (username, email, domaine, images, réseaux sociaux, etc.). Parfait pour ne pas se limiter aux mêmes outils.

---

## 6. Techniques avancées

### 6.1 Google Dorks

Des opérateurs pour affiner Google :

| Opérateur | Exemple | Effet |
|---|---|---|
| `site:` | `site:linkedin.com "ingénieur" "Cergy"` | Limite à un site |
| `filetype:` | `filetype:pdf "organigramme" site:exemple.fr` | Cherche un type de fichier |
| `intitle:` | `intitle:"index of" "backup"` | Mot dans le titre |
| `inurl:` | `inurl:admin site:exemple.fr` | Mot dans l'URL |
| `"..."` | `"jean.dupont@exemple.fr"` | Expression exacte |
| `-` | `exemple -site:exemple.fr` | Exclure |
| `before:` / `after:` | `after:2025-01-01` | Filtrer par date |

### 6.2 Recherche d'image inversée

Outils : **Google Lens**, **TinEye**, **Yandex Images** (très bon sur les visages et les lieux), **Bing Visual Search**.

**Pas à pas :**

1. Récupère l'image **en meilleure qualité possible**.
2. Lance-la dans **plusieurs moteurs** (leurs résultats diffèrent beaucoup).
3. Recadre sur un détail (enseigne, monument) et relance.
4. Cherche la **première apparition** de l'image (date la plus ancienne).

### 6.3 Métadonnées EXIF

Une photo peut contenir : modèle d'appareil, date, **coordonnées GPS**, logiciel d'édition.

```bash
sudo apt install -y libimage-exiftool-perl
exiftool photo.jpg
```

⚠️ La plupart des réseaux sociaux **suppriment** les EXIF à l'upload, mais pas toutes les messageries ni tous les sites (blogs, petites annonces, envoi par mail).

### 6.4 Géolocalisation visuelle (GEOINT)

Méthode en entonnoir :

1. **Large** : pays / région (langue des panneaux, côté de conduite, plaques, végétation, architecture).
2. **Moyen** : ville / quartier (relief, type de route, enseignes).
3. **Précis** : l'endroit exact (Google Street View, images satellites, Overpass Turbo).
4. **Confirmation** : ombres (SunCalc), angle de vue, détails fixes (poteaux, passages piétons).

### 6.5 Archives web

- **Wayback Machine** : https://web.archive.org/ : voir d'anciennes versions d'une page.
- **archive.ph** : capture instantanée.
- Utile pour retrouver du contenu **supprimé** (ancien profil, ancienne page « équipe »).

### 6.6 Domaines et infrastructure

| Besoin | Outils |
|---|---|
| WHOIS | `whois exemple.fr`, who.is |
| DNS | `dig`, `nslookup`, dnsdumpster.com |
| Sous-domaines | crt.sh (certificats), `subfinder` |
| Historique | SecurityTrails, ViewDNS |
| Technos du site | Wappalyzer, BuiltWith |
| Fuites / exposition | Have I Been Pwned (pour **toi**), Shodan, Censys |

---

## 7. Études de cas réelles

### 🛰️ Strava Heatmap et les bases militaires secrètes

En 2018, la carte de chaleur publique de Strava (qui agrège les parcours de sportifs) a révélé la **position et l'organisation interne de bases militaires** dans des zones sensibles : les militaires utilisaient l'appli en courant sur leurs bases, et les traces dessinaient des lieux censés rester discrets.

**Leçon** : une donnée anodine individuellement devient sensible **une fois agrégée**.

### 🛡️ Strava et les gardes du corps de Macron

Des investigations de presse ont montré que les profils Strava publics de membres de la sécurité présidentielle permettaient de **suivre les déplacements** du chef de l'État et d'anticiper des lieux de séjour.

**Leçon** : même les professionnels de la sécurité se font piéger par les **paramètres de confidentialité par défaut**.

### 📍 Google Maps Timeline

L'historique de position de Google (Timeline) enregistre les lieux visités. Mal configuré ou compromis, il révèle **domicile, travail, habitudes, horaires**.

**Leçon** : vérifie et limite ce que ton téléphone enregistre (voir [section 9](#9-se-protéger-de-losint)).

---

## 8. Exercice pratique : ton auto-audit OSINT

Le meilleur moyen d'apprendre, c'est de **s'auto-évaluer** : tu verras ce que *n'importe qui* peut trouver sur toi.

### Étape par étape

**1. Liste tes identifiants**
Pseudos, adresses email (anciennes comprises), numéro de téléphone, nom complet.

**2. Cherche tes pseudos**
```bash
sherlock tonpseudo --output audit_pseudo.txt
```
+ WhatsMyName dans le navigateur.

**3. Teste tes emails**
```bash
holehe ton.email@exemple.com --only-used
```
+ Epieos pour voir ce qui est exposé côté Google.

**4. Google Dorks sur toi**
```
"Prénom Nom"
"tonpseudo"
"ton.email@exemple.com"
"Prénom Nom" "ta ville"
```
Regarde aussi dans l'onglet **Images**.

**5. Recherche d'image inversée**
Passe ta photo de profil dans Google Lens, Yandex et TinEye : est-elle utilisée ailleurs ?

**6. Vérifie les fuites**
https://haveibeenpwned.com/ : tes emails apparaissent-ils dans des fuites ?

**7. Fais le bilan**

| Source | Info exposée | Sensibilité (1-5) | Action |
|---|---|---|---|
| Ex. : vieux compte forum | Pseudo + ville | 3 | Supprimer le compte |
| Ex. : profil Strava | Parcours depuis le domicile | 5 | Zone de confidentialité |

**8. Corrige** : supprime, rends privé, change les mots de passe, active la 2FA.

---

## 9. Se protéger de l'OSINT

### Mesures de base

- **Pseudos différents** selon les usages (jeux, pro, perso). Ne réutilise pas le même partout.
- **Emails différents** (alias) : un par catégorie de services.
- **Photo de profil** différente d'un site à l'autre, ou évite ta vraie tête sur les comptes anonymes.
- **Réglages de confidentialité** : revois-les tous les 6 mois, les défauts changent.
- **Supprime les vieux comptes** inutilisés (utilise Holehe sur toi pour les retrouver).
- **Ne poste pas en temps réel** : publie tes photos de vacances **après** ton retour.
- **Nettoie les EXIF** avant d'envoyer une photo hors réseaux sociaux.

### Sport et localisation (Strava & co.)

1. Crée des **zones de confidentialité** autour de ton domicile et de ton travail.
2. Passe ton profil en **privé** et règle la visibilité de chaque activité.
3. Désactive la participation aux **cartes de chaleur publiques**.
4. Ne publie pas tes trajets réguliers à heures fixes.

### Google Maps Timeline / historique de position

1. Ouvre les paramètres de **ton compte Google** → *Données et confidentialité*.
2. Désactive l'historique des positions ou règle la **suppression automatique** (3, 18 ou 36 mois).
3. Supprime l'historique existant si besoin.
4. Vérifie aussi les **avis Google Maps** publics sous ton vrai nom.

*(Voir la page de support Google citée dans les sources pour la procédure à jour.)*

### Droits RGPD

- **Droit d'accès / d'effacement / d'opposition** : tu peux demander à un site de supprimer tes données.
- **Déréférencement** : demande à Google de retirer certains résultats te concernant.
- **Plainte** : en cas de doxing ou de traitement abusif, la **CNIL** peut être saisie (et tu peux aussi porter plainte au commissariat ou en ligne).

### Si tu es victime de doxing

1. **Capture tout** (preuves horodatées, URL).
2. **Signale** le contenu à la plateforme.
3. **Porte plainte** (commissariat / gendarmerie, ou plainte en ligne).
4. Contacte **3018** (numéro national contre le harcèlement numérique) ou **Pharos** (signalement de contenus illicites).
5. Sécurise tes comptes (mots de passe, 2FA) et préviens ton entourage.

---

## 10. Rédiger un rapport OSINT

Structure type :

```
# Rapport OSINT : [Titre]

## 1. Contexte et objectif
Pourquoi cette recherche, qui la demande, cadre légal / autorisation.

## 2. Périmètre
Ce qui est inclus / exclu.

## 3. Méthodologie
Outils, sources, dates de collecte.

## 4. Résultats
Chaque constat avec :
 - Source (URL + date de consultation + capture)
 - Niveau de confiance (confirmé / probable / possible / douteux)

## 5. Analyse et recoupements
Ce que les éléments montrent ensemble.

## 6. Limites et biais
Homonymes possibles, données anciennes, sources uniques.

## 7. Recommandations
Actions correctives ou suite à donner.

## 8. Annexes
Captures, exports, logs.
```

**Bonnes pratiques :**

- Distingue toujours **faits** et **hypothèses**.
- Cite **toutes** tes sources avec la date de consultation.
- Ne conserve que ce qui est **nécessaire** (minimisation RGPD) et supprime le reste une fois la mission terminée.

---

## 11. Pour aller plus loin

### Se former

- **OSINT FR** (communauté et ressources francophones)
- **Trace Labs CTF** : trouver des personnes disparues dans un cadre éthique
- **Sourcing Games** et **Gralhix** (challenges de géolocalisation)
- **Bellingcat** : guides et cas d'enquête en accès libre
- **TryHackMe** : rooms OSINT pour débuter
- **Week-end OSINT / CTF** organisés par les associations cyber

### Roadmap de progression

| Niveau | Objectif | Pratique |
|---|---|---|
| 🟢 Débutant | Comprendre la méthode et le cadre légal | Auto-audit, Google Dorks, Sherlock |
| 🟡 Intermédiaire | Croiser plusieurs sources | Epieos, Holehe, Pappers, archives web |
| 🟠 Avancé | Géolocalisation et graphes | Overpass Turbo, SunCalc, Maltego |
| 🔴 Expert | Rapports et missions complètes | CTF Trace Labs, missions encadrées |

---

## 12. Sources

- **Strava heatmap reveals location of secret US military bases**, *The Guardian*, consulté le 5 mars 2026
  https://www.theguardian.com/world/201...
- **Doxing : définition et sanctions pénales**, *Service Public*, consulté le 5 mars 2026
  https://www.service-public.fr/particu...
- **OSINT Framework**, référence des outils de renseignement en sources ouvertes, consulté le 5 mars 2026
  https://osintframework.com/
- **Strava revealed location of Macron's bodyguards**, *The Times*, consulté le 5 mars 2026
  https://www.thetimes.co.uk/article/st...
- **Google Maps Timeline & Privacy**, *Google Support*, consulté le 5 mars 2026
  https://support.google.com/maps/answe...
- **Loi Informatique et Libertés**, *CNIL*, consulté le 5 mars 2026
  https://www.cnil.fr/fr/la-loi-informa...

> 📝 Les liens se terminant par `...` étaient tronqués dans la source d'origine : retrouve l'URL complète en cherchant le titre de l'article.

---

*Rappel final : l'OSINT est un outil puissant. Utilisé avec méthode et éthique, il protège et informe. Utilisé sans cadre, il blesse et il est puni par la loi.*
