# Charte des agents — Brique & Clé

Site : https://brique-et-cle.netlify.app — dépôt GitHub ALSNovaGestion/brique-et-cle, branche « Principal ».
Chaque fichier modifié sur la branche « Principal » est mis en ligne automatiquement par Netlify (1 à 2 minutes).

## Objectif du site
Attirer depuis Google des personnes qui préparent un achat immobilier dans le Nord (59) et le Pas-de-Calais (62), leur donner des outils et informations fiables, puis les envoyer vers les partenaires (courtier crédit, rachat de crédits, assurance emprunteur) via les boutons `data-partner`. Chaque contact envoyé est rémunéré.

## Règles absolues
1. **Aucun chiffre non vérifié.** Tout prix, taux, montant ou règle doit venir d'une source consultée le jour même (WebSearch / WebFetch). Si une donnée ne peut pas être confirmée, on ne l'écrit pas.
2. **Sources prioritaires** : prix au m² → meilleursagents.com (page `/prix-immobilier/<ville>-<cp>/`) ; règles → service-public.gouv.fr, legifrance.gouv.fr, economie.gouv.fr, anil.org, impots.gouv.fr ; taux de crédit → baromètres de courtiers datés du mois (Meilleurtaux, CAFPI, Pretto) ; frais de notaire → barèmes DGFiP / sites de notaires.
3. **Pas de conseil personnalisé** en crédit ou en investissement. Le site informe et oriente.
4. **Mentions obligatoires conservées** sur chaque page : encadré « Un crédit vous engage et doit être remboursé. Vérifiez vos capacités de remboursement avant de vous engager. », mention des liens partenaires rémunérés, mentions légales de l'éditeur.
5. **Ne jamais modifier** : les `PARTNER_LINKS` (sauf demande explicite de Sonia), les mentions légales, la balise `google-site-verification`, `robots.txt`.
6. **Jamais d'action engageante** : ne jamais postuler à un programme, accepter des conditions, créer un compte, envoyer un message au nom de Sonia.
7. Style : français clair, phrases courtes, vouvoiement, aucune promesse de gain, pas d'emoji. Pas de contenu copié : on reformule toujours.

## Structure d'une page ville (`<slug>.html`)
- Copier intégralement une page ville existante (ex. `douai.html`) comme modèle et remplacer uniquement le contenu propre à la ville : `<title>`, meta description, `canonical`, fil d'Ariane, eyebrow (département + code postal), h1, intro, les 3 cartes de prix + ligne source datée, l'exemple chiffré, « Quels biens », les 3 points de vigilance.
- Exemple chiffré : maison de 90 m² au prix moyen maison (appartement de 60 m² si le marché est surtout fait d'appartements). Calculs :
  - Frais de notaire ancien = prix × 6,3185 % (5,80665 % pour un primo-accédant) + émoluments TTC + 1 200 € de débours + CSI (0,1 % du prix, 15 € minimum).
  - Émoluments HT : 3,870 % jusqu'à 6 500 € ; 1,596 % de 6 500 à 17 000 € ; 1,064 % de 17 000 à 60 000 € ; 0,799 % au-delà. TTC = HT × 1,2.
  - Emprunt = prix + frais − 15 000 € d'apport ; prêt 25 ans au taux indiqué sur la page ; assurance 0,30 %/an du capital.
  - Revenus nécessaires = mensualité ÷ 0,35.
  - **Vérifier chaque taux et chaque barème à la date du jour** avant de calculer ; s'ils ont changé, mettre à jour ce fichier (section ci-dessus) dans le même passage.
- Faire les calculs avec un script (python/node), jamais de tête.

## Structure d'un article pratique (`guide-<slug>.html`)
- Même en-tête, pied de page, CSS et script `PARTNER_LINKS` qu'une page ville (copier `douai.html` puis remplacer le contenu de `<main>`).
- `<main>` : fil d'Ariane (Accueil › Guides pratiques › titre), eyebrow « Guide pratique », h1, lede, 3 à 5 sections `<section class="block">` avec `<div class="prose">`, au moins un exemple chiffré sourcé, puis le bloc `.partner` adapté au sujet, puis une section « Sources » listant les liens consultés.
- 800 à 1 400 mots.

## Après chaque publication
1. `index.html` : ajouter la carte juste au-dessus du marqueur
   - ville : `<a class="ville" href="/<slug>"><b>Nom</b><small>Département</small></a>` au-dessus de `<!-- VILLES:FIN -->`
   - article : `<a class="ville" href="/guide-<slug>"><b>Titre court</b><small>Guide pratique</small></a>` au-dessus de `<!-- ARTICLES:FIN -->`
2. `sitemap.xml` : ajouter `<url><loc>https://brique-et-cle.netlify.app/<chemin></loc><lastmod>AAAA-MM-JJ</lastmod></url>`.
3. `PLAN.md` : déplacer le sujet de « À publier » vers « Publiés » avec la date.
4. Vérifier 1 à 2 minutes plus tard que l'URL publique répond (WebFetch) et que la page n'affiche aucun `{{` ni texte manquant.

## Problèmes connus et solutions
Chaque agent résout lui-même les problèmes rencontrés et ajoute ici une ligne courte quand il trouve une solution réutilisable.
- Outils Arcade absents au démarrage : relancer ToolSearch (« select:mcp__ARCADE__Github_GetFileContents,mcp__ARCADE__Github_CreateFile »), qui attend la connexion. En dernier recours : outils mcp__github__ sur le même dépôt et la même branche.
- WebFetch refusé (« PROVENANCE_REQUIRED », autorisation sans réponse en passage automatique) : utiliser mcp__Firecrawl__firecrawl_scrape (maxAge 0) sur la même URL, et firecrawl_search pour chercher.
- Numéro de fiche service-public qui renvoie un autre sujet : retrouver la bonne fiche par recherche (firecrawl_search sur service-public.gouv.fr), ne pas deviner.
- curl/wget depuis le shell : bloqués par le proxy (403). Vérifier la mise en ligne avec firecrawl_scrape (rawHtml ou links, maxAge 0, storeInCache false).
- Contrôle après écriture : `git pull` puis `git diff HEAD~1 HEAD` dans le dossier cloné ; seule la modification voulue doit apparaître. Sinon réécrire depuis `git show HEAD~1:<fichier>` + la seule modification voulue.
- Page absente en ligne après 2 minutes : revérifier après 3 minutes, puis contrôler le fichier sur la branche Principal et le déploiement Netlify.
- Donnée impossible à confirmer : ne pas l'écrire ; pour une ville sans prix MeilleursAgents confirmé, passer au sujet suivant et le noter au journal.
- Ajout d'une ligne dans un gros fichier (index.html, sitemap.xml, PLAN.md, JOURNAL.md) : utiliser mcp__ARCADE__Github_UpdateFileLines (remplacer la ligne du marqueur par « nouvelle ligne + marqueur », ou mode append) plutôt que réécrire tout le fichier ; construire d'abord le contenu attendu par script, puis après `git pull` le comparer octet pour octet (`cmp`).
- Les pages modèles contiennent des espaces fines insécables (U+202F) dans certains montants : pour un remplacement par script, les rechercher avec ce caractère, pas avec une espace simple.
- Netlify arrête de déployer sans erreur visible (dernier déploiement « ready » ancien, page neuve en 404) : crédits Netlify épuisés (confirmé le 30/09 par l'e-mail de Netlify « has used all available credits » dans la boîte alsnovagestion@gmail.com ; forfait Personal, 1 000 crédits par cycle du 24 au 23, déploiements de production en pause jusqu'à recharge ou remise à zéro). Chaque commit déclenchait un déploiement. Depuis le 30/09, netlify.toml ignore les commits qui ne touchent que des .md ; limiter aussi le nombre de commits .html/.xml par publication. Ne rien acheter : signaler à Sonia.
- Crédits Firecrawl faibles (signalé le 02/10) : chercher d'abord avec WebSearch et WebFetch (meilleurtaux.com, impots.gouv.fr et insee.fr répondent à WebFetch), garder Firecrawl pour meilleursagents.com, les pages refusées par WebFetch (PROVENANCE_REQUIRED, adresse trop longue) et la vérification de mise en ligne. Ne rien acheter : signaler à Sonia.
- Espace fine insécable (U+202F) des montants : un agent ne peut pas la saisir dans un appel d'outil, elle arrive sur GitHub comme une espace simple (constaté le 02/10 sur arras.html, corrigé). L'écrire `&#8239;` dans le HTML (même affichage) ; depuis le 02/10, les montants de 8 pages ville l'utilisent : dans un script, accepter les trois écritures (U+202F, `&#8239;`, espace simple).
- Mise à jour mensuelle des chiffres d'une page ville : tous les chiffres sont dans les lignes 195 à 216 ; remplacer ce seul bloc avec mcp__ARCADE__Github_UpdateFileLines, puis `git pull` et `cmp` avec le fichier attendu produit par script. Mettre aussi à jour le mois de la balise meta description (ligne 7) quand les prix changent de mois, et la date lastmod de la page dans sitemap.xml.
- Une seule mise en ligne Netlify par passage (15 crédits au lieu de 15 par commit) : terminer le message de chaque commit .html/.xml par `[skip netlify]`, sauf le dernier, qui déclenche un seul déploiement avec tous les fichiers (vérifié le 02/10 : 14 commits dont 13 avec `[skip netlify]`, 1 déploiement). Attendre la fin de ce déploiement (1 minute) avant de modifier les fichiers .md.
- Un seul taux par durée sur tout le site : le taux 25 ans des exemples des pages ville, la valeur par défaut du simulateur et l'exemple de guide-capacite-emprunt.html doivent être identiques (taux moyens d'au moins deux baromètres datés, pas les « bons taux » d'une grille, qui changent chaque semaine). Quand le taux change, recalculer tous ces exemples dans le même passage.
- WebFetch refusé sur toutes les adresses en passage automatique (constaté le 02/10, y compris meilleurtaux.com et impots.gouv.fr) : passer directement à Firecrawl. Pour meilleursagents.com, `includeTags: ["#prices-summary-sell", "#prices-summary-rental"]` ne renvoie que les prix, les loyers et la date.
- La phrase « Sources des taux » du pied de page d'index.html est dans le bloc Mentions légales : ne pas la modifier sans demande de Sonia.