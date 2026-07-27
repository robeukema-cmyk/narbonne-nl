# Narbonne.nl

Nederlandstalige reisgids-website over Narbonne (Zuid-Frankrijk). Platte, statische
HTML/CSS/JS — geen build-tool, geen framework, geen CMS. Gehost via GitHub Pages,
custom domain `www.narbonne.nl`. De site staat nog in dev/pre-launch fase: de
eigenaar wil wijzigingen gewoon direct gemerged en live hebben, zonder lang PR-overleg.

## Structuur

- `index.html` — homepage
- `bezienswaardigheden.html`, `hotels.html`, `camping.html`, `klimaat.html`,
  `restaurants.html`, `auto-huren.html` — subpagina's, alle met identieke opzet
  (header → page-hero → breadcrumb → TOC → `.prose`-content → footer)
- `blog/index.html`, `blog/*.html` — blogoverzicht en -artikelen (zelfde patroon,
  maar met `../` in elk pad naar css/js/afbeeldingen/andere pagina's)
- `css/style.css` — al het CSS, met design tokens bovenin (`:root`)
- `js/main.js` — mobiel menu, actieve nav-link, header-schaduw, scroll-reveal
- `stijlgids.html` — **interne referentiepagina**, niet gelinkt in het menu, met
  `noindex`. Documenteert elk herbruikbaar component (kleuren, typografie,
  knoppen, hero's, kaarten, TOC, statistiekenbalk, footer) met live demo's,
  kopieerbare codevoorbeelden én twee complete paginasjablonen. **Begin hier**
  bij twijfel over hoe iets hoort te worden opgebouwd.

## Belangrijkste valkuil: geen templating

Elke pagina heeft zijn eigen kopie van header, footer en andere terugkerende
blokken — er is geen include/partial-systeem. Dat betekent: **een wijziging aan
een gedeeld onderdeel (header-nav, footer, TOC-patroon, meta-structuur) moet
sitebreed worden doorgevoerd**, niet alleen op de pagina die toevallig werd
aangepast. Dit is al twee keer misgegaan in deze repo (footer-logo op 7 van de
8 pagina's vergeten; TOC moest apart op 6+1 pagina's worden toegevoegd). Check
dus bij zo'n wijziging altijd alle bestanden: `index.html`,
`bezienswaardigheden.html`, `hotels.html`, `camping.html`, `klimaat.html`,
`restaurants.html`, `auto-huren.html`, `blog/index.html`, `blog/*.html`.

**Houd `stijlgids.html` (en `css/style.css` waar nodig) in sync** wanneer je een
nieuw herbruikbaar component toevoegt of een bestaand patroon wijzigt (nieuwe
knop-variant, ander kaarttype, aangepaste footer, etc.) — dat is de opzet die de
eigenaar expliciet gevraagd heeft.

## Conventies

- **Nieuwe pagina**: start altijd vanuit een van de twee sjablonen in
  `stijlgids.html`, niet vanuit een leeg bestand.
- **TOC**: elke pagina met 2+ `<h2>`'s in de `.prose`-content krijgt een
  `<nav class="toc">` vlak na de breadcrumb, met een `id` op elke kop. Heeft de
  sectie ook `<h3>`'s, dan een geneste `<ul>` in het TOC-item (zie hotels.html,
  camping.html, restaurants.html).
- **SEO-meta**: elke pagina heeft canonical, Open Graph, Twitter Card en een
  JSON-LD BreadcrumbList (blogartikelen ook een BlogPosting-block). Kopieer het
  blok uit een bestaande vergelijkbare pagina.
- **Nieuwe pagina in het menu?** Werk dan ook bij: `<nav class="site-nav">` op
  élke pagina, de footer-linkkolommen op élke pagina, en `sitemap.xml`.
- **Afbeeldingen zonder foto**: gebruik de gradient-placeholders
  (`.card-img.placeholder.placeholder-*`, of laat `<img>` weg in `.blog-card-img`
  voor een automatische gradient) in plaats van een stockfoto erbij te zoeken.

## Git-workflow in deze repo

- Werkbranch: `claude/session-history-other-device-org3k3`. Als de vorige PR
  vanaf deze branch al gemerged is, herstart de branch vanaf `origin/master`
  (`git checkout -B <branch> origin/master`) vóór nieuwe commits — niet
  doorbouwen op al-gemergede geschiedenis.
- De eigenaar wil site-wijzigingen gewoon gemerged zien (squash merge naar
  `master`) zonder er expliciet om te vragen, zolang het om deze site gaat en
  de wijziging is getest/gecontroleerd (lokaal gerenderd, geen kapotte links,
  geen regressies elders). Bij twijfel of iets risicovol/ingrijpend is: wel
  eerst afstemmen.
- GitHub Pages deployt automatisch vanaf `master` — geen aparte build- of
  deploy-stap nodig.
