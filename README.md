# Stratego

**Anatomie d’une stratégie d’évaporation de l’État belge.**

Site statique consacré à la stratégie politique de la N-VA de Bart De Wever :
la doctrine de l’évaporation, l’ingénierie confédérale, la doctrine Maddens,
l’action de la coalition Arizona, et les verrous qui bloquent toute partition.
État des connaissances arrêté au **22 août 2026**.

→ **[ouaisfieu.github.io/stratego](https://ouaisfieu.github.io/stratego/)**

Site jumeau : [Le Dossier Arizona](https://ouaisfieu.github.io/arizona/), consacré
à la coalition fédérale elle-même.

---

## Mise en ligne

Le dépôt se publie tel quel : il n’y a rien à compiler, rien à installer.

### Avec git

```bash
cd stratego
git init && git add -A
git commit -m "Stratego : mise en ligne initiale"
git branch -M main
git remote add origin https://github.com/ouaisfieu/stratego.git
git push -u origin main
```

### Sans git, depuis le navigateur

1. Sur `github.com/ouaisfieu/stratego`, cliquer **Add file → Upload files**.
2. Glisser **le contenu** du dossier décompressé (les fichiers et dossiers,
   pas le dossier qui les contient).
3. **Commit changes**.

Le dossier `.github/` n’est pas toujours repris par le glisser-déposer d’un
navigateur. S’il manque, ce n’est pas grave : voir l’option B ci-dessous.

### Activer GitHub Pages

Dans **Settings → Pages** du dépôt :

- **Option A — GitHub Actions** *(recommandé)* : choisir *Source :
  GitHub Actions*. Le workflow `.github/workflows/deploy.yml` valide le HTML
  et le XML, puis publie.
- **Option B — sans workflow** : choisir *Source : Deploy from a branch*,
  branche `main`, dossier `/ (root)`. Le site est publié directement, sans
  aucune validation préalable. C’est l’option à prendre si le dossier
  `.github/` n’a pas été téléversé.

Le site est en ligne à `https://ouaisfieu.github.io/stratego/` une à deux
minutes plus tard.

## Contenu du dépôt

```
.
├── index.html                 accueil
├── doctrine/index.html        I   · la doctrine de l’évaporation
├── confederalisme/index.html  II  · l’ingénierie de la coquille vide
├── maddens/index.html         III · la doctrine Maddens
├── occupation/index.html      IV  · l’occupation du fort
├── institutions/index.html    V   · Sénat, Codeco, Brussels Airport
├── bruxelles/index.html       VI  · le nœud gordien
├── verrous/index.html         VII · ce qui manque pour scinder
├── contre-analyse/index.html  VIII· le dossier contradictoire
├── scenarios/index.html       IX  · quatre scénarios falsifiables
├── chronologie/index.html     X   · 2001 → 2026
├── donnees/index.html         XI  · chiffres et fiabilité
├── lexique/index.html         XII · 29 définitions
├── methode/index.html         XIII· méthode, biais, corrections
├── sources/index.html         XIV · bibliographie
├── 404.html
├── assets/css/style.css       feuille de style unique
├── assets/img/og.png          aperçu de partage (1200 × 630)
├── assets/img/favicon.svg
├── sitemap.xml  feed.xml  robots.txt  humans.txt
├── .nojekyll                  fichier vide : désactive Jekyll
└── .github/workflows/deploy.yml
```

## Modifier le site

Chaque page est un fichier HTML complet et autonome. On l’ouvre, on modifie le
texte entre les balises, on enregistre. Il n’y a pas d’étape de génération,
pas de dépendance, pas de gestionnaire de paquets.

Trois endroits à connaître :

| Ce qu’on veut changer | Où |
| --- | --- |
| Le texte d’une rubrique | le `<article class="prose">` du fichier concerné |
| Les couleurs, les espacements, la typographie | `assets/css/style.css`, section 1 (jetons de couleur) |
| Le menu, le pied de page | présents dans chaque page — à répercuter partout |

Après modification, vérifier le rendu en ouvrant un serveur local plutôt qu’en
double-cliquant le fichier — sinon la feuille de style et les liens relatifs ne
se chargent pas correctement :

```bash
cd stratego
python3 -m http.server 8000     # puis http://localhost:8000
```

## Validation

Les seize pages passent le **validateur HTML du W3C** (Nu HTML Checker) sans
aucune erreur. Le workflow de déploiement rejoue cette validation à chaque
`push`, et valide en plus `sitemap.xml` et `feed.xml`.

La validation CSS est volontairement désactivée : le validateur CSS du W3C
n’a pas été mis à jour pour les propriétés modernes employées ici
(`color-scheme`, `margin-inline`, `backdrop-filter`, `text-wrap`,
`color-mix`), pourtant standard et largement supportées. L’activer ferait
échouer le déploiement sur des faux positifs.

## Principes techniques

| Choix | Raison |
| --- | --- |
| Aucun JavaScript | Rien sur ce site n’en a besoin. Le contenu s’affiche même si tout échoue sauf le HTML. |
| Aucune dépendance externe | Pas de CDN, pas de police distante, pas de traceur, pas de cookie. Zéro requête tierce, donc zéro fuite de données de lecture. |
| Une seule feuille CSS | Thèmes clair et sombre par jetons de couleur, aucun préprocesseur. |
| Polices système | Chargement instantané, longévité maximale. |
| SVG écrits à la main | Les schémas sont du texte : indexables, accessibles, redimensionnables, lisibles dans un diff. |

Poids d’une page : 20 à 28 Ko, image de partage non comprise.

## SEO et web sémantique

- **Schema.org en JSON-LD**, un `@graph` par page : `WebSite`, `Organization`,
  `Person` (auteur), plus le type propre à la page — `Article`, `Dataset` pour
  les données, `DefinedTermSet` pour le lexique, `ItemList` d’`Event` pour la
  chronologie, `FAQPage` pour l’accueil, `BreadcrumbList` partout.
- **Open Graph** et **Twitter Cards** complets, image 1200 × 630.
- `canonical`, `hreflang` (`fr-be` + `x-default`), `robots` explicite.
- `sitemap.xml` avec `lastmod` et priorités, référencé depuis `robots.txt`.
- Flux **RSS 2.0** avec lien `atom:self`.
- HTML sémantique : `article`, `section`, `nav`, `aside`, `figure`/`figcaption`,
  `time datetime`, `dfn`, `abbr`, `cite`, `blockquote`, tables avec `caption`,
  `thead` et `scope`.
- Accessibilité : lien d’évitement, `aria-current`, `aria-label` sur chaque
  navigation, `<title>` et `<desc>` dans chaque schéma SVG, contrastes
  conformes en clair comme en sombre, respect de `prefers-reduced-motion` et
  `prefers-color-scheme`, feuille d’impression.

## Éditorial

Le site distingue explicitement quatre niveaux de certitude — **établi**,
**contesté**, **interprétation**, **prospectif** — signalés visuellement dans
le texte. La page **Méthode** publie l’origine du corpus, les biais assumés,
les angles morts et le **journal des treize corrections** apportées aux
documents de départ, y compris celles qui affaiblissent la thèse générale.

Signalements d’erreur : ouvrir une *issue*. Les corrections substantielles sont
ajoutées au journal plutôt que substituées silencieusement.

## Auteur et licence

Rédaction et appareil critique : **Claude (Anthropic)**.
Direction éditoriale : anonyme, à sa demande.

- Contenu rédactionnel : [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/deed.fr)
- Code (gabarit, CSS) : [MIT](LICENSE)
