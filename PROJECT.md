# MES/HUB — Project Specification
**Version:** v11+  
**Owner:** Tomi Forsblom, ME Studio  
**Last updated:** 2026-05-13

---

## 1. Visio ja tarkoitus

MES/HUB on Tomi Forsblomille räätälöity henkilökohtainen hallintadashboard. Se pitää hänet kartalla — ennakointi, kokonaiskuvan hahmottaminen ja työkuorman hallinta reaaliajassa.

**Nimi:** MES/HUB  
**Selite:** SOI RADAR & OVERVIEW MONITOR  
**Käyttäjä:** Tomi Forsblom, ME Studio — vain hän, ei tiimi, ei asiakkaat

**Kolme ydintehtävää:**
1. Näytä työkuorma rehellisesti ja läpinäkyvästi (SOI)
2. Anna kokonaiskuva kaikesta käynnissä olevasta yhdellä silmäyksellä (Timeline)
3. Ei manuaalista syöttämistä — data tulee integraatioista automaattisesti

---

## 2. Widgetit ja prioriteetti

### PRIMÄÄRIT — koko dashboardin ydin

**SOI Meter (Orbit Pressure Meter)**
Tomi Forsblomia henkilökohtaisesti mittaava työkuormaindeksi. NASA-TLX -pohjainen. Tämä on dashboardin sydän — sen on oltava visuaalisesti täydellinen, toiminnallisesti tarkka ja välittömästi luettavissa.

**Production Timeline**
Gantt-näkymä kaikista aktiivisista projekteista. Klikkaamalla riviä se laajenee alas — brief, specs, materiaalit, faasipalkit, toimintanapit. Tämä on dashboardin tärkein toiminnallinen elementti.

### SEKUNDÄÄRIT — tärkeät mutta eivät ydin

**Incoming Glance** — tuleva putki, pidetään, siirretään alemmaksi

**Asset Radar** (aiemmin Asset Orbit) — kuvapankkihaut, pidetään, siirretään alemmaksi

**News/Updates** — uusi widget, karuselli, RSS + YouTube. EI RAKENNETA VIELÄ.

### POISTETAAN KOKONAAN

- Liukusäätimet (Self pressure, Self capacity, Calendar booked, Fragmentation) — korvautuvat päiväraporttilla
- Dock / Prompt Corner — epäselvä tarkoitus, poistetaan toistaiseksi

### SIIRRETÄÄN SEKUNDÄÄREIKSI — alempaan osioon

- Studio Pulse — epävarma tarve kun Timeline integroituu Outlookiin, pidetään toistaiseksi
- Incoming Glance — pidetään, siirretään alemmaksi
- Asset Radar (aiemmin Asset Orbit) — pidetään, siirretään alemmaksi
- News/Updates — uusi tuleva widget, karuselli RSS + YouTube, EI RAKENNETA VIELÄ — näytetään "Tulossa" -placeholder

---

## 3. Layout

### Periaatteet
- Ei tyhjää tilaa ilman tarkoitusta. Koskaan.
- Mobiilissa 4×1 (elementit allekkain) on hyväksyttävää — ei pakko tehdä gridia
- Tilankäyttö optimoidaan jokaisella breakpointilla erikseen
- Tämä on räätälöity yhdelle henkilölle — ei yleisölle, ei saavutettavuussääntöjä
- Touch target -minimia (44px) ei sovelleta

### Desktop (≥1260px)
```
[Header: MES/HUB | ticker | NOW | FORECAST | Theme]
[SOI Meter 40%] [SOI Detail 60%: numero + status + dimensiot]
[Production Timeline — full width]
[Incoming Glance | Asset Radar]
```

### Tabletti (760–1260px)
```
[Header]
[SOI Meter — full width, mittari vasemmalla, data oikealla]
[Timeline — full width]
[Incoming | Asset Radar]
```

### Mobiili (<760px)
```
[Header — kompakti]
[SOI Meter — kompakti, mittari ylhäällä]
[Dimension cards]
[Timeline — full width, horisontaalinen scroll OK]
[Incoming Glance]
[Asset Radar]
```

---

## 4. SOI — Studio Overload Index

### 4.1 Perusta
NASA Task Load Index (Hart & Staveland 1988). Mittaa Tomi Forsblomia henkilökohtaisesti. Physical Demand poistettu. 4 dimensiota käytössä.

### 4.2 Dimensiot

| Dimensio | Lähde | Päivitys |
|----------|-------|---------|
| Mental Demand | Aktiiviset projektit × kompleksisuus | Automaattinen |
| Temporal Demand | Deadline-läheisyys + kalenterin täyttö | Automaattinen |
| Effort | Suunniteltu vs toteutunut | Osittain auto + päiväraportti |
| Frustration | Puuttuvat materiaalit + keskeytykset | Osittain auto + päiväraportti |

### 4.3 Laskentakaava

SOI = 0–100 aina. Ei koskaan yli 100.

```
mentalDemand = clamp(activeProjects × avgEffort × 5, 0, 100)
temporalDemand = clamp(urgentDeadlines × 20 + calendarLoad × 0.4, 0, 100)
effort = taskRatio × 0.5 + effortReport × 0.5
frustration = autoFrustration × 0.6 + frustrationReport × 0.4
SOI = mental×0.25 + temporal×0.30 + effort×0.20 + frustration×0.15 + performance×0.10
```

Jos päiväraporttia ei ole: käytetään 7 päivän keskiarvoa. Ei koskaan tyhjää.

### 4.4 SOI-mittarin visuaalinen standardi — KRIITTINEN

Mittarin on oltava viimeistellyn precision-instrumentin tasolla. Tämä on koko dashboardin visuaalinen ankkuri.

**Vaadittu:**
- Arc värigradientilla: vihreä (0–40) → amber (40–70) → oranssi (70–85) → punainen (85–100)
- Tasaväliset tikitykset: major joka 10, minor joka 5
- Neula: tarkka, ohut viiva, hehkuva kärki, pivot-piste pieni ympyrä jossa glow-efekti
- Forecast-neula: erillinen, läpinäkyvämpi, punaisempi
- Tausta: syvä tumma, hillitty radiaaligradietti pivotista ulospäin — ei tasainen musta
- SOI-numero: 60–72px, JetBrains Mono Bold, ei system-ui
- Status-chip: integroitu, ei irrallinen
- Ei isoa tyhjää tilaa mittarin yläpuolella

**Ei hyväksytä:**
- Tasainen musta tausta
- Tikitykset puuttuvat
- System-ui fontti numeroissa
- Iso tyhjä tila yläpuolella
- Neula ilman tarkkuutta ja hehkua

### 4.5 Päiväraportti

Pulssi-indikaattori headerissa klo 12:00 jos eilistä ei ole täytetty.

Kysymykset:
1. Effort slider 0–100: "Meniko päivä niin kuin arvioit?"
2. Frustration slider 0–100: "Kuinka paljon keskeytyksiä?"
3. Performance slider 0–100: "Miten tyytyväinen olet päivään?"
4. Ad-hoc count: montako suunnittelematonta tehtävää
5. Hour variance: arvio aamulla vs toteutunut

---

## 5. Production Timeline

- Gantt, vain `status: 'active'` projektit
- Klikkaus → inline expand alas (ei erillinen paneeli)
- Expand näyttää: brief, specs, assets, faasipalkit, action-napit
- NOW-viiva dynaamisesti laskettu JS:llä — ei hardkoodattu CSS
- Incoming-signaalit palloina, värikoodattu riskin mukaan
- Horisontaalinen scroll tarkoituksellinen — älä poista

### Tilasiirtymät
- incoming → active → archived
- active → incoming (takaisin)
- archived → active (restore)

---

## 6. Incoming Glance

- Risk-score automaattinen: puuttuvat materiaalit + deadline
- Add slide-in panel
- Peek through airlock avaa listan
- Sijainti: sekundääri, dashboardin alaosa

---

## 7. Asset Radar *(aiemmin Asset Orbit)*

- Kategoriat: Kuvat | Video | 3D | Muut
- Filtterit: CC0 | Subscription | Vinkit
- Haku avaa linkin hakusanalla oikeaan palveluun
- Sijainti: sekundääri, dashboardin alaosa

---

## 8. Microsoft Graph -integraatio

**Vaatii:** Azure AD App Registration (ks. AZURE_BRIEF.md)

| Data | Lähde | Endpoint |
|------|-------|----------|
| Kalenteri | Outlook | `GET /me/calendarView` |
| Tehtävät | Planner | `GET /me/planner/tasks` |
| Myyntiputki | Excel/SharePoint | workbook API |
| SOI-historia | SharePoint List | lists API |

Auth: MSAL.js, PKCE, ei client secretiä.

---

## 9. Prioriteetit

### P0 — Tee nyt
1. SOI-mittarin visuaalinen uudelleenrakennus (kohta 4.4)
2. Layout: SOI + Timeline primääreinä, Studio Pulse + Dock + liukusäätimet poistetaan
3. Sekundäärit (Incoming, Asset Radar) siirretään alas

### P1 — Seuraavaksi
4. Timeline inline expand
5. Projektien tilasiirtymä
6. Päiväraportti-komponentti
7. SOI-historia + trendi

### P2 — Myöhemmin
8. Microsoft Graph -autentikaatio
9. Asset Radar kategoriat + filtterit
10. News/Updates -widget

---

## 10. Tekninen rakenne

- Standalone HTML, React 18 UMD, Lucide esm.sh, Babel standalone
- GitHub Pages, Branch: main / root
- localStorage → SharePoint (tuleva)
- Versiokommentti joka muutoksen yhteydessä: `<!-- MES/HUB vXX YYYY-MM-DD -->`
- Ei hardkoodattuja pikseliarvoja positioinnissa
- SW: `./sw.js` (suhteellinen polku)
