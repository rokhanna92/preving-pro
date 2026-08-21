# Preving Pro

Sajt agencije Preving Pro, Novi Sad. Astro, statički build, bez servera.

## Struktura

    site/            Astro projekat (izvor sajta)
    site/mockup/     originalni dizajn mokapi (.dc.html), referenca
    Biznis plan.docx, *.jpg, *.svg    poslovni materijal, ne ulazi u build

## Lokalni rad

Iz `site/`:

    npm install
    npm run dev      # http://localhost:4321
    npm run build    # -> site/dist/

Dev server u pozadini: `astro dev --background`, pa `astro dev status`,
`astro dev logs`, `astro dev stop`.

Napomena: dodavanje ili brisanje fajla u `src/pages/` obara HMR router
(`Failed to update routes via HMR`) i nova ruta vraća 404 dok se dev server
ne restartuje. Izmene postojećih stranica rade normalno.

## Deploy: Cloudflare Pages

Radi sa privatnim repozitorijumom, besplatno, i servira sa korena domena.

| Podešavanje | Vrednost |
| --- | --- |
| Framework preset | Astro |
| Build command | `npm run build` |
| Build output directory | `dist` |
| Root directory | `site` |
| Node verzija | iz `site/.node-version` (22.12.0) |

`Root directory` mora biti `site`, jer Astro projekat nije u korenu
repozitorijuma. Bez toga build pada odmah.

## Zašto ne GitHub Pages na project repo

Stranice koriste apsolutne putanje (`/usluge`, `/kontakt`,
`/refinery-scene.js`). Astro opcija `base` ne prepisuje ručno napisane
linkove, pa bi na `korisnik.github.io/preving-pro/` sve rute, favicon i 3D
hero vraćali 404. Cloudflare Pages i repo nazvan `korisnik.github.io`
serviraju sa korena, gde ovo nije problem.

## Stranice

| Ruta | Fajl |
| --- | --- |
| `/` | `src/pages/index.astro` |
| `/usluge` | `src/pages/usluge.astro` |
| `/cenovnik` | `src/pages/cenovnik.astro` |
| `/kontakt` | `src/pages/kontakt.astro` |
| `/o-nama` | `src/pages/o-nama.astro` |

Sadržaj (cene, FAQ, usluge, tim) stoji u `site/src/data/*.json`.
Zajedničko zaglavlje i podnožje: `site/src/components/`.

3D hero (`site/public/refinery-scene.js`) učitava three.js sa unpkg-a u
runtime-u. Ako CDN nije dostupan, hero ostaje tamna pozadina, ostatak
stranice radi normalno.
