# Paralelní životy

Interaktivní srovnávací časová osa životních drah osmnácti osobností české politiky.
Statická stránka bez závislostí a bez build kroku — jeden soubor `index.html`.

**Živá verze:** https://jaktocist.cz

## Co stránka umí

- výběr libovolného počtu osobností, rozvržení se výběru přizpůsobí; tlačítko
  „Zrušit výběr“ odznačí všechny najednou a osa pak vyzve k novému výběru
- paralelní sloupce zůstávají zachovány při libovolném počtu osob; sloupec nikdy
  neklesne pod čitelnou šířku a při velkém počtu se osa posouvá vodorovně
- centrální chronologická osa s ročníky, po stranách dráhy vybraných osob
- ke každé ze 151 událostí rozklikávací článek (celkem 151 článků)
- průsečíky — události s pevným obsazením, kde se dráhy protínají
- srovnávací matice profilů v pěti tématech
- přepínač Detail / Přehled
- vyhledávání, které filtruje osu a samo vybere odpovídající osobnosti
- plná textová verze v sekci `#archiv` — funguje i bez JavaScriptu

## Struktura

```
index.html          celá aplikace (HTML + CSS + JS + obsah článků)
404.html            chybová stránka
robots.txt          povolení indexace + odkaz na sitemapu
sitemap.xml         mapa webu
CNAME               vlastní doména pro GitHub Pages
favicon.svg         ikona
apple-touch-icon.png
og-image.png        náhled pro sociální sítě (1200×630)
.nojekyll           vypnutí Jekyllu na GitHub Pages
```

## Nasazení na GitHub Pages

1. Nahrát obsah tohoto adresáře do kořene repozitáře.
2. **Settings → Pages → Source:** Deploy from a branch, větev `main`, složka `/ (root)`.
3. **Settings → Pages → Custom domain:** `jaktocist.cz`, zaškrtnout *Enforce HTTPS*.
4. U registrátora domény nastavit DNS:
   - `A` záznamy pro kořen na `185.199.108.153`, `185.199.109.153`, `185.199.110.153`, `185.199.111.153`
   - `CNAME` pro `www` na `<uzivatel>.github.io`

## Změna domény

Adresa je na čtyřech místech: `CNAME`, `robots.txt`, `sitemap.xml` a v hlavičce
`index.html` (canonical, og:url, og:image, JSON-LD). Nahradit `jaktocist.cz` novou doménou.

## Vyhledávání jako filtr osy

Vyhledávací pole v liště legendy nefunguje jako našeptávač, ale **řídí osu**.
Po zadání dotazu a stisku Enter stránka:

1. najde všechny události, v jejichž článku se výraz vyskytuje,
2. automaticky vybere osobnosti, kterých se tyto události týkají,
3. zobrazí na ose pouze tyto události.

Vedle pole se objeví štítek s dotazem a počty. Kliknutím na něj se filtr zruší,
ale **vybrané osobnosti zůstanou** — osa pak ukáže všechny jejich události,
jako by je uživatel naklikal ručně. Filtr ruší i Escape a ruční změna výběru.

Dokud je filtr aktivní, hledaný výraz se zvýrazní (`<mark class="hl">`) na kartách
osy i v otevřeném článku; článek se k prvnímu výskytu sám posune. Zvýraznění
používá stejná pravidla shody jako filtr, takže je vyznačeno přesně to, co událost
do výsledku dostalo. Zdrojové články v `#archiv` se nemění.

Index se staví za běhu při prvním zaostření pole — z `EVENTS` a z článků v sekci
`#archiv`. Nepoužívá knihovnu ani předgenerovaný soubor, takže **přidání osobnosti
nevyžaduje ve vyhledávání žádný zásah**.

Diakritika se normalizuje způsobem, který zachovává délku řetězce. Porovnává se
po slovech, nikoli přes celý text — hledání uvnitř slov dávalo nesmysly
(„OKD“ se našlo v „málokdo“). Skloňování řeší dvojí pravidlo: dotaz se shoduje se
slovem, je-li jeho předponou, nebo sdílejí-li předponu delší než tři čtvrtiny
délky delšího z nich. Volnější práh spojoval „dluhopisy“ s „dluhu“.
Víceslovný dotaz funguje jako AND přes celou událost.

## Patička

Patička je sbalená (`<details class="about">`) a obsahuje čtyři bloky: **Co to je**,
**Zdroje**, **Metoda** a **Opravy a revize**. Je to meta-informace o dokumentu —
nikoli obsah. Právní stav jednotlivých kauz do ní **nepatří**: ten je vždy uveden
přímo u příslušné události, kde má kontext a kde ho čtenář hledá.

**Zápis nové revize** = přidat jeden `<li>` na začátek `.ab-log`:

```html
<li><time datetime="2026-11-04">4. 11. 2026</time><span>Popis změny.</span></li>
```

Datum je v dokumentu jen na tomto jediném místě. Razítko v hlavičce patičky
(„Ověřeno … · N revizí“) se z nejnovějšího záznamu dopočítá samo, včetně počtu
revizí a správného skloňování. Nikde jinde datum revize neaktualizujte.

## Datová vrstva

Veškerý obsah je v `<script>` na konci `index.html`:

| struktura | obsah |
|---|---|
| `PERSONS` | osobnosti — `name`, `surname`, `color`, `born`, `arc`, `sum` |
| `ORDER` | pořadí osob ve výběru i ve sloupcích osy – vždy abecedně podle příjmení (viz Řazení jmen) |
| `THEMES` | řádky srovnávací matice (pět témat) |
| `FACTS` | hodnoty matice pro každou osobu a téma |
| `EVENTS` | události; `w` je klíč osoby, nebo pole klíčů u průsečíku |
| `ERAS` | historická období, do nichž se události řadí podle roku |
| `PREFIX` | mapa počátečního písmene id události na klíč osoby |

## Přidání další osobnosti

1. `PERSONS` — nový záznam se všemi šesti poli
2. `ORDER` — zařadit klíč na místo podle abecedy příjmení (viz Řazení jmen)
3. `FACTS` — vyplnit všech pět témat ze `THEMES`
4. `EVENTS` — události s unikátním `id` ve tvaru `<písmeno>-<rok>`
5. `PREFIX` — namapovat počáteční písmeno id na klíč osoby (jinak se článek otevře
   se špatnou barvou)
6. CSS — proměnná `--klic` v `:root` a pravidlo `.tag.t-klic`
7. Sekce `#archiv` — nový blok `<details class="arch-person" style="--pc:BARVA">` na místo podle abecedy
   s články, každý jako `<article class="artsrc" id="art-ID">`
8. Volitelně doplnit osobu do obsazení `w:[…]` u existujících průsečíků
9. Aktualizovat počty v hlavičce (`description`, `keywords`, JSON-LD), v nadpisu
   archivu, v README a v `og-image.png`

**Kontrolní pravidlo:** počet `id` v `EVENTS` musí přesně odpovídat počtu
`<article id="art-…">` v archivu. Barvy osob musí být unikátní. Pořadí osob musí
odpovídat abecedě příjmení.

## Řazení jmen

Osobnosti jsou **všude řazeny abecedně podle příjmení**, podle české abecedy:
Č, Ř, Š, Ž a Ch jsou samostatná písmena (Rakušan je před Řehkou, Schillerová
patří pod S). Na křestním jménu ani na funkci nezáleží.

Pořadí určuje pole `ORDER` a z něj se odvozuje výběr jmen, pořadí sloupců na ose,
srovnávací matice i obsazení průsečíků. Ve stejném pořadí musí být ručně udržované:

- bloky osob v sekci `#archiv` (průsečíky zůstávají jako poslední blok),
- jména v `<meta name="keywords">` a v seznamu `about` v JSON-LD,
- barevné pruhy v `og-image.png`.

Kontrola v konzoli prohlížeče – musí vrátit `true`:

```js
ORDER.join()===ORDER.slice().sort((a,b)=>PERSONS[a].surname.localeCompare(PERSONS[b].surname,'cs')).join()
```

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
