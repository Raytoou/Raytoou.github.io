---
title: "Red Notice"
date: 2025-10-19
tags: [osint, image, red-notice, nbctf]
---

# Write-up — Red Notice

**Challenge** : Red Notice  
**Point(s)** : 425  
**Difficulté** : Moyen (OSINT)  
**Auteur** : @migattenochauvui / @Raytoou (adapté)  

---

## Énoncé
À partir de l’image fournie, retrouvez les informations suivantes concernant la personne présente sur l’image :
- Sa nationalité.
- Sa taille en centimètres.
- En 2018, le compte d’un média a publié une publication mentionnant son nom sur un réseau social très connu. Retrouvez qui était le propriétaire du site de ce média en **2005**.

**Format du flag** : `NBCTF{Pays_Taille en cm_Prenom-Nom}`  
Exemple : `NBCTF{Chine_181_Carine-Dupont}`

---

## Résumé des découvertes (rapide)
- **Nom identifié** : *Aram Kohnepushi* (trouvé via Google Lens / recherche image).  
- **Preuve d’un post en 2018** : post X/Twitter du compte `@finnland` daté du 11 mai 2018 mentionnant le nom.  
- **Site du média mentionné sur le profil** : `finn-land.net` (site plus accessible mais archivé sur Wayback Machine).  
- **À compléter / vérifier** :
  - Taille en cm — **TODO** (méthodes proposées ci-dessous).
  - Propriétaire du site `finn-land.net` en **2005** — **TODO** (Wayback / whois / page source).

---

## Méthodologie détaillée & preuves

### 1) Identification via image recognition
Outils possibles : Google Lens (web ou mobile), TinEye, recherche image inversée Google Images.

**Procédure**  
1. Charger l’image dans Google Lens / Google Images → noter les occurrences similaires.  
2. On obtient des pages contenant le nom **Aram Kohnepushi**.  

**Preuve (à coller)**  
- Capture d’écran Google Lens montrant le nom trouvé.  
- Lien(s) des pages trouvées (si public).

---

### 2) Vérification du post 2018 (réseau social X/Twitter)
Outils : navigateur (recherche X), archives d’un tweet si supprimé.

**Procédure**  
- Recherche sur X/Twitter : chercher `Aram Kohnepushi` ou visiter le profil `@finnland` et chercher le tweet du 11 mai 2018.  
- Copier la capture d’écran du tweet (date + texte + lien vers article MTV cité).

**Extrait utilisé dans l’analyse**  
> `Der auf #Europol Liste ... 8:00 AM · May 11, 2018` — tweet du compte `@finnland` qui mentionne le nom et renvoie vers `mtv.fi/...`.

---

### 3) Trouver le site du média et récupérer le proprio en 2005
Le profil du média mentionne `finn-land.net`. Le site n’existe plus mais est archivé sur Wayback Machine.

**Procédure recommandée (Wayback + page source)**

1. Ouvrir la Wayback Machine :  
   - URL : `https://web.archive.org/web/*/finn-land.net`  
   - Sélectionner un snapshot proche de la période d’intérêt (par ex. 2005 ou page « à propos » si présentée ensuite).  

2. Charger le snapshot choisi. Dès que la page s’affiche, afficher le code source (Ctrl+U) et chercher des mentions de `owner`, `propriétaire`, `contact`, `©`, `made by`, `site owner`.  
   - Sur beaucoup de sites, les informations de contact / propriété apparaissent en bas de page.

3. Si le site affiche une page « whois » ou un pied de page avec le nom du propriétaire, le noter (capture d’écran).  
   - Si le site ne contient pas l’info, faire un `whois` historique (voir étape suivante).

**Commandes utiles (terminal)**

- Lister les snapshots :
```bash
# affiche la liste des snapshots (exemple avec curl + grep)
curl -s "https://web.archive.org/cdx/search/cdx?url=finn-land.net&output=json&fl=timestamp,original" | jq .
