---
title: "Un Seul Classeur pour Tous mes Assistants IA : la fin du copier-coller de skills"
date: 2026-06-15
excerpt: "Comment le format ouvert Agent Skills permet de centraliser ses compétences IA dans un seul dossier, partagé entre Claude Code, Codex et GitHub Copilot."
---

<div class="post-tags"><span class="post-tag">Skills</span><span class="post-tag">Productivité</span><span class="post-tag">Automatisation</span></div>

![Un classeur central de skills, synchronisé vers Claude, Codex et Copilot]({{ '/assets/images/one-binder-every-ai.png' | relative_url }})

## 1. Introduction : le Jour où j'ai Écrit la Même Chose pour la Troisième Fois

Voici une situation que tout utilisateur de plusieurs assistants IA finit par connaître : j'avais appris à Claude Code à créer des présentations PowerPoint à la charte de mon entreprise. Quelques semaines plus tard, je voulais la même compétence dans Codex. Puis dans GitHub Copilot. À chaque fois, la même question : vais-je vraiment recopier ces instructions une troisième fois ?

Le vrai problème du copier-coller n'est pas la perte de temps initiale. C'est la **dérive** : trois copies d'une même compétence finissent inévitablement par diverger. On corrige l'une, on oublie les autres, et un jour on ne sait plus laquelle est la bonne. Trois assistants, trois versions de la vérité — c'est-à-dire aucune.

## 2. La Découverte : les Assistants IA Parlent Désormais la Même Langue

Ce qui rend la solution possible aujourd'hui, c'est une convergence discrète mais majeure : le format **Agent Skills**, ouvert par Anthropic fin 2025, a été adopté par les principaux assistants de code. Claude Code, Codex et GitHub Copilot lisent désormais exactement le même format :

> Une skill = un dossier + un fichier `SKILL.md` : quelques lignes d'en-tête (nom, description) suivies d'instructions en langage naturel.

La description joue un rôle clé : c'est elle que l'assistant lit pour décider *quand* mobiliser la skill. Le corps du fichier n'est chargé qu'à ce moment-là. Une skill bien décrite fonctionne donc partout, sans adaptation.

Conséquence stratégique : mes compétences ne sont plus « des réglages de Claude » ou « des réglages de Copilot ». Ce sont **mes** compétences, dans un format standard, et les assistants sont interchangeables en dessous.

## 3. L'Architecture : une Source de Vérité, des Panneaux Indicateurs

L'idée directrice tient en une image : au lieu de photocopier mes recettes pour chaque cuisine, je garde **un seul classeur**, et chaque cuisine reçoit un panneau « les recettes sont là ».

Concrètement, sur mon PC :

| Élément | Rôle |
|---|---|
| `C:\...\mes-skills\` | Le classeur : la seule version « vivante » de chaque skill |
| Jonctions Windows (raccourcis) | Les panneaux : chaque assistant croit avoir les skills chez lui, il ne fait que lire le classeur |
| Dépôt GitHub **privé** | La photocopie de secours : sauvegarde et réinstallation sur un autre PC en une commande |

Chaque assistant cherche ses skills dans son propre dossier (`~\.claude\skills\`, `~\.codex\skills\`, `~\.config\github-copilot\skills\`). Les jonctions font pointer ces trois dossiers vers le classeur central. Je corrige une skill **une fois** → les trois assistants voient la correction **instantanément**, dans tous mes projets.

Le moment le plus parlant de la mise en place : une skill créée à l'origine pour Codex est apparue dans Claude Code à la seconde où la jonction a été posée. Aucune copie, aucune synchronisation — le même fichier, vu de deux endroits.

## 4. Le Processus au Quotidien : Deux Règles, Rien de Plus

**Règle 1 — Toujours travailler dans le classeur.** Une nouvelle skill se crée dans `mes-skills\`, jamais directement dans le dossier d'un assistant. Un petit script (`sync-skills.ps1`) crée ensuite les raccourcis manquants vers les trois assistants — relançable à volonté, sans risque.

**Règle 2 — Sauvegarder n'est pas automatique.** Comme un document Word : modifier le fichier ne met pas à jour la copie de secours. Après un changement, un commit + push vers GitHub (trois clics dans le panneau Source Control de VS Code, ou une simple demande à un assistant).

Et deux garde-fous :

- **Privé ne veut pas dire risqué, mais prudent quand même** : jamais de secret (mot de passe, clé, donnée client) dans une skill, même dans un dépôt privé.
- **Les comptes ne se croisent pas** : le compte GitHub qui héberge la sauvegarde et le compte qui fournit Copilot peuvent être différents — les skills sont lues sur le disque local, aucun assistant ne sait où elles sont sauvegardées.

## 5. Pourquoi c'est Plus qu'une Astuce d'Organisation

Ce montage change le statut de mes instructions : elles deviennent un **actif personnel, portable et versionné**.

- **Portable** : changer d'assistant, ou en adopter un quatrième, ne coûte plus rien — un raccourci suffit.
- **Versionné** : Git garde l'historique de chaque amélioration ; je peux voir comment une skill a évolué, et revenir en arrière.
- **Capitalisé** : chaque leçon apprise avec un assistant profite immédiatement aux autres. Le savoir s'accumule au lieu de se disperser.

> **« Mes skills n'appartiennent plus à un outil. Les outils passent ; le classeur reste. »**

## 6. Conclusion : Écrire pour l'Écosystème, pas pour l'Outil

La leçon dépasse le cas des skills : dès qu'un format ouvert émerge dans l'écosystème IA, il vaut la peine de réorganiser son travail autour de lui plutôt qu'autour d'un produit. Le jour où un nouvel assistant apparaîtra, la question ne sera plus « faut-il tout recopier ? » mais simplement « où est le panneau indicateur ? ».
