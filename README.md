# Renta & Plán

Interaktivní advisory nástroj pro plánování finanční nezávislosti. Určen pro použití při osobní schůzce bankéře s klientem — jako guided conversation tool, nikoli kalkulačka.

![Renta & Plán – Screenshot](screenshot.png)

---

## Co aplikace dělá

Provede klienta třemi kroky:

1. **Definice cíle** — věk penze, požadovaná měsíční renta, délka čerpání, režim (zachovat / spotřebovat kapitál)
2. **Přehled majetku** — inline editace aktiv s příznakem likvidity, strategie a volatility per aktivum
3. **Strategie** — Monte Carlo simulace přes všechna likvidní aktiva, fan chart, pravděpodobnost úspěchu, what-if slider

---

## Klíčové funkce

- **Per-aktivum modelování** — každé aktivum má vlastní výnos p.a., volatilitu a pravidelnou investici; DPS automaticky počítá státní příspěvek dle pravidel 2025
- **Monte Carlo** — 350 simulací, každé aktivum jede vlastní geometrickou Brownovu cestu, výsledky se agregují
- **Fan chart** — distribuce výsledků v pásmech 5/25/50/75/95. percentil + cílová linie
- **Dosažitelná renta vs. potřebná úložka** — oba výstupy najednou, přepočet při pohybu sliderem
- **Citlivostní tabulka** — automaticky generuje scénáře pro rentu ×0,65 / aktuální / ×1,5
- **Pásma pravděpodobnosti** — orientační zóny < 50 % / 50–70 % / 70–85 % / > 85 % s interpretací a doporučením
- **Likvidita** — nelikvidní aktiva (nemovitosti, podnikání) jsou z výpočtu úložky vyloučena, v grafu ztlumena

---

## Technologie

Čistý HTML + CSS + JavaScript, žádné závislosti na backendu ani build procesu.

| Oblast | Technologie |
|---|---|
| Frontend | Vanilla JS, CSS Custom Properties |
| Grafy | Chart.js 4.4.1 (CDN) |
| Simulace | Monte Carlo, Box-Muller transform |
| Fonty | Google Fonts – Cormorant Garamond, Outfit |

---

## Spuštění

```bash
# Stačí otevřít soubor v prohlížeči
open renta_final.html
```

Nebo nahrát na jakýkoli statický hosting (GitHub Pages, Netlify, vlastní server). Nevyžaduje server, build ani npm.

---

## Přizpůsobení

### Výchozí aktiva klienta
Najdi blok `const S = { ... maj: { akt: [` ve `<script>` sekci. Každé aktivum má tyto parametry:

```js
{
  id: 1,
  type: 'investment',     // investment | pension | cash | real_estate | crypto | business
  name: 'Modelové portfolio CZK',
  val: 3000000,           // aktuální hodnota v Kč
  inst: 'Erste Premier',  // instituce / poznámka
  liquid: true,           // zahrnout do výpočtu úložky
  monthly: 5000,          // pravidelná investice Kč/měs
  employer: 0,            // příspěvek zaměstnavatele (jen pension)
  strat: 1,               // 0 = konzervativní, 1 = vyvážený, 2 = dynamický
  ret: 5,                 // výnos p.a. v %
  vol: 12                 // volatilita p.a. v %
}
```

### Výnosové presety strategií
```js
const ASSET_STRAT = {
  investment:  [[ret,vol], [ret,vol], [ret,vol]],  // konz / vyváž / dyn
  pension:     ...
  real_estate: ...
}
```

### Počet Monte Carlo simulací
```js
mcAggregated(350, rentaVal)  // změň 350 → vyšší číslo pro přesnější výsledky
```

---

## Omezení modelu

- Výnosy jsou modelovány jako **i.i.d. normální rozdělení** — model nepodchycuje fat tails ani volatility clustering
- Aktiva jsou simulována **nezávisle** — nepočítá se s korelací v době krize
- Model nezohledňuje **inflaci ani daně**
- Varianta *Doživotně* používá orientační věk **85 let** jako konec čerpání

Výsledky slouží ke srovnání scénářů a jako podpora rozhovoru — nejsou finanční predikcí.

---

## Licence

MIT — volné použití, úpravy a distribuce s uvedením zdroje.
