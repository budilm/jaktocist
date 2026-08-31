# Paralelní životy

Interaktivní srovnávací časová osa životních drah sedmnácti osobností české politiky.
Statická stránka bez závislostí a bez build kroku — jeden soubor `index.html`.

**Živá verze:** https://budilm.github.io/jaktocist

## Co stránka umí

- výběr jedné až sedmnácti osobností, rozvržení se výběru přizpůsobí
- centrální chronologická osa s ročníky, po stranách dráhy vybraných osob
- ke každé ze 144 událostí rozklikávací článek (celkem 144 článků)
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
.gitignore          nezaverzované složky s podklady z chatu
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

## Datová vrstva

Veškerý obsah je v `<script>` na konci `index.html`:

| struktura | obsah |
|---|---|
| `PERSONS` | osobnosti — `name`, `surname`, `color`, `born`, `arc`, `sum` |
| `ORDER` | pořadí osob ve výběru i ve sloupcích osy |
| `THEMES` | řádky srovnávací matice (pět témat) |
| `FACTS` | hodnoty matice pro každou osobu a téma |
| `EVENTS` | události; `w` je klíč osoby, nebo pole klíčů u průsečíku |
| `ERAS` | historická období, do nichž se události řadí podle roku |
| `PREFIX` | mapa počátečního písmene id události na klíč osoby |

## Přidání další osobnosti

1. `PERSONS` — nový záznam se všemi šesti poli
2. `ORDER` — zařadit klíč do pořadí
3. `FACTS` — vyplnit všech pět témat ze `THEMES`
4. `EVENTS` — události s unikátním `id` ve tvaru `<písmeno>-<rok>`
5. `PREFIX` — namapovat počáteční písmeno id na klíč osoby (jinak se článek otevře
   se špatnou barvou)
6. CSS — proměnná `--klic` v `:root` a pravidlo `.tag.t-klic`
7. Sekce `#archiv` — nový blok `<details class="arch-person" style="--pc:BARVA">`
   s články, každý jako `<article class="artsrc" id="art-ID">`
8. Volitelně doplnit osobu do obsazení `w:[…]` u existujících průsečíků
9. Aktualizovat počty v hlavičce (`description`, `keywords`, JSON-LD), v nadpisu
   archivu, v README a v `og-image.png`

**Kontrolní pravidlo:** počet `id` v `EVENTS` musí přesně odpovídat počtu
`<article id="art-…">` v archivu. Barvy osob musí být unikátní.

## Redakční standard

Články jsou psané investigativně a objektivně. Každý má stejnou stavbu: perex,
dva až tři odstavce faktů a závěrečnou sekci „Jak to číst“, která je autorskou
interpretací a je od faktů výslovně oddělená.

Zásady, které se osvědčily:

- **Neutralita vůči složení osy.** Texty nesmí obsahovat tvrzení závislá na tom,
  kdo je zrovna vybrán — žádné „nejstarší osobnost osy“ ani pevné počty osob.
  Taková formulace se rozbije při prvním dalším přírůstku.
- **Rozlišovat doložené, tvrzené a nedoložené.** U sporných věcí uvádět fakta,
  stanoviska obou stran a explicitně i to, co doložené není. Absence záznamu
  není důkaz.
- **Nepřebírat nálepky.** Charakteristiky typu „proruský“ nebo „dezinformátor“
  patří s atribucí tomu, kdo je vyslovil, nikoli do popisu faktů.

## Obsah

Sestaveno z veřejně dostupných zdrojů, stav k srpnu 2026. Právní stav sporných
kauz a zdrojová východiska jsou uvedeny v patičce stránky.
