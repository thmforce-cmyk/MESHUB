# MES/HUB — Codex Promptit

## PROMPT 1 — Layout ja siivous

```
Lue PROJECT.md ja AGENTS.md kokonaan.

Tee nämä muutokset:

1. Poista liukusäätimet kokonaan (Self pressure, Self capacity,
   Calendar booked, Fragmentation). Ne korvautuvat myöhemmin päiväraporttilla.

2. Järjestä layout:
   PRIMÄÄRIT ylhäällä: SOI Meter, Production Timeline
   SEKUNDÄÄRIT alempana omassa osiossaan: Studio Pulse, Incoming Glance, Asset Radar
   TULEVA alimpana: News/Updates — näytä pelkkä "Tulossa" -placeholder kortilla

3. Nimeä "Asset Orbit" → "Asset Radar" kaikkialla.

4. Desktop-layout: SOI Meter vasemmalla 40%, SOI-data (numero + status + dimensiot)
   oikealla 60%. Timeline full width sen alla. Sekundäärit rinnakkain alimpana.

Aja QA-checklist ja GUI-checklist. Päivitä versio: MES/HUB v12 2026-05-13.
```

---

## PROMPT 2 — SOI-mittarin visuaalinen uudelleenrakennus

```
Lue PROJECT.md kohta 4.4 — visuaalinen standardi on kirjattu täsmällisesti.

Rakenna SOI-mittari uudelleen SVG:llä:
- Arc värigradientilla: vihreä (0-40) → amber (40-70) → oranssi (70-85) → punainen (85-100)
- Major ticks joka 10 yksikköä, minor ticks joka 5, arcin kehällä
- NOW-neula: ohut, tarkka, hehkuva kärki, pivot pieni ympyrä jossa glow
- FORECAST-neula: erillinen, läpinäkyvämpi, punaisempi
- Tausta: syvä tumma, radiaaligradietti pivotista — ei tasainen musta
- SOI-numero: JetBrains Mono Bold 64px, oikealle puolelle mittaria desktopilla
- Ei isoa tyhjää tilaa mittarin yläpuolella
- Mobiilissa mittari skaalautuu leveyteen — ei kiinteää pikselikorkeutta

Aja GUI-checklist. Päivitä versio: MES/HUB v13 2026-05-13.
```

---

## PROMPT 3 — Timeline inline expand

```
Kun käyttäjä klikkaa projektin riviä timelinessa, rivi laajenee alas
grid-template-rows animaatiolla (0fr → 1fr).

Laajentuneessa näkymässä:
- Brief (teksti)
- Specs (tagit)
- Assets (linkit)
- Napit: "Move to Incoming" | "Archive"

Vain yksi rivi auki kerrallaan. Klikkaamalla uudelleen sulkeutuu.
Poista erillinen Project Detail -paneeli — tämä korvaa sen.

Aja QA + GUI checklist. Päivitä versio: MES/HUB v14 2026-05-13.
```

---

## PROMPT 4 — Projektien tilasiirtymä

```
Lisää projektien tilasiirtymä. project.status: 'active' | 'incoming' | 'archived'

- active: napit "Move to Incoming" ja "Archive"
- incoming: "Activate" (jo olemassa) ja "Archive"
- archived: "Restore to Active"

Arkistoidut piilotetaan timelinesta ja incoming-listasta, säilyvät localStoragessa.

Aja QA + GUI checklist. Päivitä versio: MES/HUB v15 2026-05-13.
```

---

## PROMPT 5 — Päiväraportti

```
Pulssi-indikaattori headeriin klo 12:00 jos eilistä raporttia ei ole.
Klikkaamalla avautuu slide-in paneeli oikealta.

Kysymykset:
1. Slider 0-100: "Meniko päivä niin kuin arvioit?" (Effort)
2. Slider 0-100: "Kuinka paljon keskeytyksiä?" (Frustration)
3. Slider 0-100: "Miten tyytyväinen olet päivään?" (Performance)
4. Numero: "Suunnittelemattomia tehtäviä tuli:"
5. Kaksi numeroa: "Arvioit X tuntia — toteutui Y tuntia"

Tallenna localStorageen päivämäärän mukaan.
SOI käyttää näitä arvoja automaattisesti.

Aja QA + GUI checklist. Päivitä versio: MES/HUB v16 2026-05-13.
```
