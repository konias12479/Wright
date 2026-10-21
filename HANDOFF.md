# Předávací balíček – stav k 21. 10. 2026

## Co je hotové
- 14 kapitol: obnova A1/A3, pravidla, DroneMap a zóny, cvičný test (57 otázek), dron a RC 2, trenažér (virtuální RC 2 s 3D kamerou a minimapou), 3D hra, checklist, první lety a nouze, chování a rizika, tipy, pojištění, odkazy.
- Responzivní design (desktop až 2560 px, mobil), tmavý režim, ukládání postupu v prohlížeči.

## Jak to převzít v Claude Code
```bash
git clone https://github.com/konias12479/Wright.git
cd Wright
git checkout claude/dji-mini5pro-github-pages-o4wrob
cp /cesta/k/balicku/{index.html,README.md,CLAUDE.md} .
git add index.html README.md CLAUDE.md
git commit -m "Letová příprava: virtuální RC 2 s 3D kamerou, DroneMap, chování a rizika, předávací CLAUDE.md"
git push
```
Pak v Claude Code: „Přečti CLAUDE.md a HANDOFF.md a pokračuj podle sekce Další kroky.“

## Otevřené body / další kroky
1. ~~Ověřit 3D hru~~ Hotovo 21. 10.: vykreslení, kamera za dronem i FPV a otáčení ověřeny v Chromiu (desktop i 390 px). Opraveno: pravá páčka do stran letěla obráceně, náklon modelu dronu při jiném kurzu než sever. Opraveno i vodorovné přetékání stránky na mobilu (Pravidla, DroneMap, Trenažér).
2. **Ověřit, že GitHub Pages servíruje novou verzi** (štítek „verze 2026-10-21d“ v trenažéru, menu se na trenažéru sbalí nahoru).
3. ~~Rozdělení `index.html`~~ Uživatel nechce, zůstává jeden soubor.
4. ~~Vendorovat three.js~~ Hotovo: `vendor/three.min.js` (r128), CDN jen jako záloha.
5. Volitelné nápady, které uživatel zatím nechtěl: reset úkolů po nárazu, vítr v simulaci, další scénáře (orbit kolem domu, odhalovací záběr jako úkol s vyhodnocením kompozice).
6. Pravidelně **aktualizovat fakta** (DroneMap, pojistné podmínky, DJI firmware) – jsou datována 10/2026.

## Kontrolní seznam před každým pushem
- [ ] JS bez syntaktických chyb
- [ ] všechny kapitoly se otevřou, žádná chyba v konzoli
- [ ] mobil ~390 px a desktop
- [ ] počty otázek / úkolů v textech sedí
- [ ] štítek verze v trenažéru zvýšen
