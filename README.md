# Paralelní životy

Interaktivní srovnávací časová osa životních drah patnácti osobností české politiky.
Statická stránka bez závislostí a bez build kroku — jeden soubor `index.html`.

**Živá verze:** https://budilm.github.io/jaktocist/

## Co stránka umí

- výběr jedné až patnácti osobností, osa se rozvržení přizpůsobí
- centrální chronologická osa s ročníky, po stranách dráhy vybraných osob
- ke každé události rozklikávací článek (celkem 128 článků)
- průsečíky — události s pevným obsazením, kde se dráhy protínají
- srovnávací matice profilů v pěti tématech
- přepínač Detail / Přehled
- plná textová verze v sekci `#archiv` — funguje i bez JavaScriptu

## Struktura

```
index.html          celá aplikace (HTML + CSS + JS + obsah článků)
404.html            chybová stránka
robots.txt          povolení indexace + odkaz na sitemapu
sitemap.xml         mapa webu
favicon.svg         ikona
apple-touch-icon.png
og-image.png        náhled pro sociální sítě (1200×630)
.nojekyll           vypnutí Jekyllu na GitHub Pages
.gitattributes      normalizace konců řádků (LF), PNG jako binární
```

## Nasazení na GitHub Pages

Stránka běží jako *project page* na adrese `https://budilm.github.io/jaktocist/`.

1. Obsah tohoto adresáře je v kořeni repozitáře `budilm/jaktocist`, větev `main`.
2. **Settings → Pages → Source:** Deploy from a branch, větev `main`, složka `/ (root)`.

## Přechod na vlastní doménu

Stránka je zatím bez vlastní domény. Až doména bude registrovaná:

1. Do kořene přidat soubor `CNAME` s jediným řádkem — názvem domény.
2. **Settings → Pages → Custom domain:** vyplnit doménu, zaškrtnout *Enforce HTTPS*.
3. U registrátora domény nastavit DNS:
   - `A` záznamy pro kořen na `185.199.108.153`, `185.199.109.153`, `185.199.110.153`, `185.199.111.153`
   - `CNAME` pro `www` na `budilm.github.io`
4. Přepsat adresu na těchto místech:
   - `robots.txt` — odkaz na sitemapu
   - `sitemap.xml` — `<loc>`
   - `index.html` — canonical, og:url, og:image, twitter:image, JSON-LD
   - `404.html` — `href="/jaktocist/…"` zkrátit na `href="/…"`

Poslední bod platí jen pro vlastní doménu: na project page je stránka v podadresáři
`/jaktocist/`, na vlastní doméně bude v kořeni.

## Přidání další osobnosti

V `<script>` v `index.html` je datová vrstva. Postup:

1. `PERSONS` — nový záznam (`name`, `surname`, `color`, `born`, `arc`, `sum`)
2. `ORDER` — zařadit klíč do pořadí
3. `FACTS` — vyplnit všech pět témat ze `THEMES`
4. `EVENTS` — události s unikátním `id` ve tvaru `<písmeno>-<rok>`
5. `PREFIX` — namapovat počáteční písmeno id na klíč osoby
6. CSS — proměnná `--klic` a pravidlo `.tag.t-klic`
7. Sekce `#archiv` — `<details>` s články, každý jako `<article id="art-ID">`
8. Volitelně doplnit osobu do obsazení `w:[…]` u existujících průsečíků

Počet událostí a článků musí souhlasit — každé `id` v `EVENTS` potřebuje
odpovídající `<article id="art-ID">` v archivu.

## Obsah

Sestaveno z veřejně dostupných zdrojů, stav k srpnu 2026. Články oddělují popis
událostí od závěrečné interpretační sekce „Jak to číst". Podrobnosti a právní stav
sporných kauz jsou uvedeny v patičce stránky.
