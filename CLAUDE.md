# CLAUDE.md – Letová příprava: DJI Mini 5 Pro

Kontext projektu pro Claude Code. Přečti celý, než začneš měnit kód.

## O projektu
Soukromá učební aplikace v **jednom souboru `index.html`** (HTML + CSS + JS, bez buildu).
Uživatel (Petr) si kupuje **DJI Mini 5 Pro Fly More Combo (standardní, ne Plus) s ovladačem DJI RC 2**,
dron je v EU **třída C0**, létá v podkategorii **A1**. Má registraci provozovatele a A1/A3,
chystá se na **prodloužení A1/A3** u ÚCL. Aplikace ho na to připravuje a učí ovládat dron.

- Repo: `github.com/konias12479/Wright`, větev `claude/dji-mini5pro-github-pages-o4wrob` (jediná, z ní se nasazuje)
- Web: https://konias12479.github.io/Wright/ (GitHub Pages přes workflow v `.github/workflows/`, ten neměň bez důvodu)
- **Repo je veřejné.** Uživatel původně chtěl soukromé; na free účtu by pak nefungovaly Pages. Nevkládej nic osobního (jména rodiny, adresy, čísla provozovatele, pojistky).

## Komunikace s uživatelem
- Česky, stručně, prakticky. Na začátku upozorni, pokud model/effort nesedí na úlohu.
- Bezpečnost a soukromí jeho a rodiny ber v úvahu a aktivně upozorňuj na rizika.
- Nemá rád zbytečné formátování; změny shrň krátce a navrhni commit message.

## Architektura `index.html`
- Router přes `location.hash`; každá kapitola je `<section id="…">`, aktivní má třídu `active`.
- Kapitoly (pořadí = menu): `start, obnova, pravidla, zony, test, dron, trenazer, hra, checklist, lety, chovani, tipy, pojisteni, odkazy`.
  Nová kapitola = položka v `<ol id="nav">` + `<section>` + dlaždice v `#cards` na úvodu (čísla dlaždic se počítají ručně v `data-n`).
- Design tokeny v `:root` (+ dark mode přes `prefers-color-scheme`). Fonty Barlow / Barlow Semi Condensed z Google Fonts s fallbackem.
- `localStorage` klíče (vše v try/catch): `m5p.done` (prostudované kapitoly), `m5p.ck` (checklist), `m5p.wrong` (chybné otázky), `m5p.hist` (historie testů).
- Kvíz: pole `Q` v JS, řádek = `[otázka, [4 možnosti], indexSprávné, vysvětlení, tag]`, tag `"pravidla"` nebo `"dron"`. Aktuálně **57 otázek**. Při změně počtu uprav texty „57 otázek“ (úvod, README).
- Na stránce trenažéru se boční menu sbalí nahoru přes `.app:has(#trenazer.active)`.

### Trenažér (`#trenazer`) – virtuální RC 2
- Stav v objektu `D` (closure `sim()`): `x,y` normalizované 0–1 na mapě 100 × 100 m (y roste na jih), `hdg` stupně (0 = sever, roste po směru hodinových ručiček), `alt` m, `tilt` gimbal −90…+60, `zoom` 1–4, `fm` režim C/N/S, `mode` manual/pause/rth.
- Světové souřadnice pro 3D: `wx(x)=(x-.5)*100`, `wz(y)=(y-.5)*100`; kamera `rotation.order="YXZ"`, `rotation.y=-hdg`, `rotation.x=tilt`, `fov=82/zoom`.
- Objekty scény v poli `OBJ` (rybník, 3 domy, 8 stromů), kreslí se do 3D i na minimapu `#map-objs`. Kolize se stromy/domy (s výškou) → overlay `#crash` a po 1,6 s reset na H, rozpracovaný úkol zůstane.
- 9 úkolů v `TASKS`, posun přes `next(i)`. Klávesy jen když je kapitola aktivní.
- Baterie se vybíjí, při 15 % automatický RTH (vystoupá na 30 m, letí domů, přistane).

### 3D hra (`#hra`)
- Samostatná closure `game()`, vlastní stav `G`, 10 bran, 60 stromů, 12 budov, setrvačnost, kamera za dronem / FPV.
- three.js **r128** z `cdnjs.cloudflare.com` (UMD), sdílený loader `window.__load3`. Offline nepoběží (zobrazí se hláška).
- **Ověřeno uživatelem:** 3D obraz v trenažéru se vykresluje a orientace sedí. **Neověřeno:** 3D hra (vykreslení, směr kamery, otáčení) – první věc ke kontrole.

## Obsahová fakta (stav 10/2026) – neměň bez ověření
- Combo standard = C0 (36 min), Combo Plus = C1 (Plus baterie, 52 min). Plus baterie do C0 dronu = ztráta C0.
- A1/A3 platí 5 let; prodloužení nejdříve 180 dní před koncem v Profilu pilota na dron.caa.gov.cz; po vypršení znovu jako napoprvé (40 otázek, 75 %, 60 min).
- Zóny: od 1. 9. 2025 DroneMap (dronemap.gov.cz) místo DronView; LKR10 nahrazen LKR310–LKR318.
- Pojištění: v ČR v A1 dobrovolné, A2 povinné min. 1 mil. Kč (§ 54d z. 49/1997 Sb.); Itálie a Německo povinné pro všechny drony.
- Pojišťovny (občanská odpovědnost, 10/2026): ČSOB kryje dron do 0,91 kg; Generali (VPP-BH-05/2024 a novější) kryje Open kategorii při platném oprávnění; Allianz (od 25. 6. 2026) a Direct drony nekryjí.
- Obrázky z manuálu DJI **nepřebírat** (manuál zakazuje reprodukci) – jen vlastní SVG schémata a odkazy po kapitolách.

## Konvence
- Typografie: české uvozovky „ “, nedělitelné mezery kde dává smysl, žádné pomlčky jako oddělovač ve větách.
- Každá kapitola končí `<div class="mark"><button class="btn" data-done="ID">`.
- Zdroje uváděj řádkem `<p class="src">Zdroj: …</p>`.
- Před commitem: zkontroluj JS syntaxi (`node -e` s `new Function(...)` nad obsahem `<script>`) a projdi kapitoly v prohlížeči (desktop i šířka ~390 px, světlý i tmavý režim).
- Po úpravě trenažéru zvyš štítek verze v `#trenazer .lead` (`verze 2026-10-21c`), uživatel podle něj pozná, že nemá cache.
