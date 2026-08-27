---
name: blog-article
description: Créer ou modifier un article du blog My AI Atlas (site Jekyll). À utiliser à chaque fois qu'on ajoute, restructure ou dépanne un article dans _posts/ — capture les erreurs déjà commises et corrigées sur ce projet, pour ne pas les refaire.
---

# Créer un article pour My AI Atlas

Ce skill condense les leçons d'une longue session de mise au point (plusieurs
allers-retours évitables). Le suivre du début à la fin évite de refaire les
mêmes erreurs.

## 1. Format de départ : Markdown simple par défaut

Sauf demande explicite d'une page interactive sur-mesure, créer un fichier
`.md` classique dans `_posts/`, sur le modèle des articles existants
(`2026-03-12-human-in-the-loop-ai-agents.md`,
`2026-08-19-one-skills-folder-for-all-ai-assistants.md`).

```yaml
---
title: "Titre de l'article"
date: AAAA-MM-JJ
excerpt: "Résumé en une phrase, affiché sur la page d'accueil."
---
```

**Ne jamais mettre `layout: bare`** sauf si l'utilisateur demande
explicitement une page 100 % autonome, très visuelle et interactive (comme
`2026-08-20-how-does-a-model-read-a-photo-or-a-voice-clip.html`). Un `bare`
mal justifié prive l'article de tout l'habillage du site (voir section 4).

## 1 bis. Étiquettes de sujet — obligatoires sur tout article

**Tout article du blog porte des étiquettes de sujet**, sans exception et sans
avoir à le demander. Règle fixée par l'utilisatrice : c'est un élément
d'identité du blog, pas une option.

- **Trois étiquettes**, courtes, en rapport direct avec le contenu réel de
  l'article (ex. `LLM` · `Fondamentaux` · `Transformer`).
- **Placées tout en haut du corps de l'article**, avant le chapô — jamais en
  bas de page.
- **Style** : pastilles arrondies (`border-radius:999px`), police mono, ~11 px,
  majuscules, `letter-spacing:0.08em`, couleur `$teal #00353F` sur fond
  `rgba(0,53,63,0.08)` avec bordure `rgba(0,53,63,0.20)`.

Pour un article **Markdown**, la classe partagée `.post-tags` / `.post-tag`
existe déjà dans `assets/main.scss` — il suffit de l'utiliser :

```html
<div class="post-tags"><span class="post-tag">Agents IA</span><span class="post-tag">Sécurité</span><span class="post-tag">Gouvernance</span></div>
```

Pour une page **HTML sur-mesure** au CSS scopé, redéfinir les mêmes pastilles
sous le préfixe de l'article (ex. `.llm-tags` / `.llm-topic`), en reprenant
les valeurs ci-dessus pour rester identique aux autres articles.

## 1 ter. Mention de prudence en fin d'article — obligatoire aussi

**Tout article se termine par une courte mention** qui délimite ce que le
contenu prétend être. Objectif : protéger l'autrice (pas de revendication
d'officialité, pas d'affirmation sur des systèmes propriétaires).

- **Une phrase**, en pied d'article, après le dernier bloc de contenu.
- **Même style que la note de fin** : filet de séparation au-dessus
  (`border-top`), police mono, ~11 px, couleur `--muted`.
- **Toujours adaptée au contenu réel** — jamais une formule générique
  recopiée. Le point à couvrir dépend du type d'article :

| Type d'article | Ce que la mention doit écarter | Exemple en place |
|---|---|---|
| Résumé d'un cours / d'un contenu tiers | Toute impression de matériel officiel | « Personal recap of LangChain Academy's free course — not official course material. » |
| Schéma d'architecture technique | Toute prétention sur les internes d'un modèle propriétaire | « General multimodal architecture overview — not a confirmed diagram of any specific proprietary model's internals. » |
| Démonstration avec chiffres illustratifs | Que les valeurs passent pour de vraies sorties de modèle | « Les identifiants de tokens, vecteurs et probabilités affichés sont illustratifs, et ne sont pas les sorties réelles d'un modèle en particulier. » |
| Retour d'expérience / montage personnel | Que ce soit lu comme une recommandation universelle | à formuler selon le cas |

## 2. Structure du contenu : jamais un mur de texte qui défile

- Découper avec des `##`/`###` clairs : ils alimentent automatiquement le
  sommaire latéral du site (`_includes/toc.html`), qui surligne la section en
  cours de lecture — c'est le mécanisme natif à utiliser pour qu'un lecteur
  sache toujours où il en est. Pas besoin d'onglets JS ni de menu maison.
- Si le contenu est long et répétitif (ex : plusieurs modules d'un cours),
  proposer une structure avec des sections **repliables** (`<details>` natif,
  ou accordéon simple) plutôt qu'un défilement continu — mais toujours
  **empilées les unes sous les autres**, jamais en colonnes côte à côte
  (retour explicite de l'utilisateur : les colonnes cassent la lecture).
- Ne jamais ajouter de ton publicitaire : un lien source cité une fois, en
  texte simple et souligné, jamais un bouton répété façon « Take the course
  now! » en haut ET en bas de page.
- Images : taille modeste (~400 px de large maximum), jamais pleine largeur.
  Utiliser une balise `<img>` HTML brute avec `width="..."` si besoin de
  contrôler la taille finement (kramdown laisse passer le HTML brut).
- **Largeurs : tout aligner sur la même colonne.** Paragraphes, encadrés,
  grilles et visualisations doivent partager exactement le même bord gauche
  ET le même bord droit. Ne jamais poser de `max-width` en `ch` ou en `px`
  sur un paragraphe ou un encadré « pour la lisibilité » : l'utilisatrice l'a
  signalé plusieurs fois comme un défaut visuel, pas comme un confort. Si un
  bloc paraît trop étroit, chercher le `max-width` en dur qui le contraint
  (y compris sur un élément **parent**, qui plafonne toujours son enfant).

## 3. Diagrammes Mermaid — piège vérifié, solution qui marche

Si l'article a besoin de vrais diagrammes visuels (pas juste du texte de
syntaxe Mermaid affiché tel quel) :

- **Adresse CDN exacte** : `https://cdn.jsdelivr.net/npm/mermaid@11/dist/mermaid.esm.min.mjs`
  — l'extension est `.mjs`, pas `.js`. Une mauvaise extension donne une 404
  silencieuse : Mermaid ne s'initialise jamais, et les diagrammes restent
  affichés en texte brut sans aucune erreur visible dans la page.
- **Ne jamais utiliser `mermaid.initialize({startOnLoad:true})` ou
  `mermaid.run()`** dès qu'il y a plus d'un diagramme de type
  flowchart/graph sur la même page. Bug vérifié : les diagrammes se
  mélangent entre eux (les nœuds d'un diagramme B apparaissent dans le
  rendu du diagramme A, un troisième reste vide) — un souci d'état partagé
  interne à Mermaid pour cette famille de diagrammes, qui persiste même
  avec des identifiants de nœuds différents et un rendu séquentiel via
  `run()`.
- **Solution qui fonctionne à coup sûr** : rendre chaque diagramme
  manuellement, un par un, avec un id explicite et unique :

  ```html
  <script type="module">
    import mermaid from "https://cdn.jsdelivr.net/npm/mermaid@11/dist/mermaid.esm.min.mjs";
    mermaid.initialize({ startOnLoad: false, securityLevel: "loose" });

    async function renderDiagrams(){
      var blocks = document.querySelectorAll("pre.mermaid");
      for (var i = 0; i < blocks.length; i++){
        var el = blocks[i];
        var src = el.textContent;
        try {
          var result = await mermaid.render("mmd-diagram-" + i, src);
          el.innerHTML = result.svg;
        } catch (err){
          el.textContent = "Diagram error: " + err.message;
        }
      }
    }
    renderDiagrams();
  </script>
  ```

- Ne pas utiliser `<br/>` dans les libellés de nœuds (ambiguïté avec le HTML
  qui entoure le bloc) — préférer un libellé court sur une seule ligne.
- Sur un article Markdown classique, un bloc ```` ```mermaid ```` est rendu
  par kramdown dans un conteneur `<div class="language-mermaid ...">`, pas
  `<pre class="mermaid">` — adapter le sélecteur JS en conséquence si on
  choisit ce format plutôt qu'une page HTML dédiée.

## 4. Si une page HTML sur-mesure est vraiment nécessaire

- Laisser le layout par défaut (`post`, hérité de `_config.yml`) pour que
  l'article ait le même habillage que les autres : lien retour + date,
  titre, sommaire latéral, pied de page du site. Un `layout: bare` retire
  tout ça — à ne choisir que pour une page vraiment autonome et assumée
  comme telle.
- **Scoper tout le CSS custom sous une classe wrapper unique**
  (ex. `<div class="lc-recap">...</div>` + toutes les règles préfixées
  `.lc-recap ...`). Ne jamais redéfinir des sélecteurs génériques utilisés
  par le thème du site : `.wrap` (largeur du site entier), `h1`–`h4`, `p`,
  `a`, `pre`, `code`. Une redéfinition non scopée de `.wrap` a cassé la
  largeur de l'en-tête et du pied de page sur toute la page lors d'un essai
  précédent.
- Avant de créer une classe, vérifier avec `grep` si `assets/main.scss` en a
  déjà une : si l'article est un `<pre>` (bloc de code ou diagramme), le
  thème applique `.post-content pre{background:$teal;...}` — neutraliser
  explicitement avec `!important` sur les propriétés qui doivent rester
  celles de l'article (fond, marge, arrondi), sinon le bloc de code ou le
  diagramme récupère un double fond superposé au sien.
- Ne pas dupliquer le `<h1>` du titre ni afficher une date manuellement : le
  layout `post` les affiche déjà à partir du front matter.
- Charger les polices Google Fonts via `@import url(...)` en toute première
  ligne du `<style>`, jamais via une balise `<link>` (il n'y a plus de
  `<head>` séparé disponible pour ce fragment de page).

## 5. Date de publication

Un article daté du jour même peut être exclu silencieusement du build si son
horodatage (minuit UTC) n'est pas encore atteint au moment exact où GitHub
Pages construit le site — le build réussit, mais l'article n'apparaît nulle
part, sans message d'erreur. `_config.yml` a déjà `future: true` pour
corriger ça de façon permanente ; vérifier que ce réglage est toujours présent
si un article publié le jour même n'apparaît pas.

## 6. Checklist avant de montrer le résultat à l'utilisateur

1. Valider l'équilibre des balises HTML (un script `HTMLParser` rapide
   suffit) — ne jamais annoncer un rendu terminé sans ce contrôle.
2. Prévisualiser avec `python tools/preview.py _posts/<fichier>` : ce script
   reconstruit le vrai rendu du site (thème, sommaire, feuille de style)
   sans avoir besoin de Jekyll/Ruby. Dépendances : `markdown`,
   `pymdown-extensions`, `pyyaml`, `libsass`.
3. S'il y a des diagrammes Mermaid ou du JS non trivial, les vérifier
   réellement dans un navigateur avant de les présenter comme fonctionnels —
   un rendu qui « a l'air bon » dans le code ne garantit pas qu'il s'affiche
   correctement. Un aperçu ouvert simplement (fichier ou `preview.py`)
   suffit largement la plupart du temps ; ne pas mettre en place de serveur
   local + navigateur headless sauf véritable doute technique (ce genre
   d'outillage sert à l'auto-vérification, pas à chaque itération).
4. Ne pas complexifier au-delà du nécessaire : si l'utilisateur demande de
   revenir à quelque chose de plus simple, le faire sans réintroduire de
   design superflu.

## 7. Publication

- Ajouter le lien de l'article dans la section « Articles » du `README.md`
  public (voir `NOTES-PERSO.md` pour le format).
- Ne committer et pousser sur GitHub que sur demande explicite de
  l'utilisateur.
- Ne jamais committer `tools/` (script de prévisualisation local, déjà
  exclu par `.gitignore`) : c'est un outil personnel, pas du contenu du
  blog.
