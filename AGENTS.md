# AGENTS.md — MES/HUB Codex Instructions
**Lue PROJECT.md kokonaan ennen mitään muuta.**

---

## Kriittiset säännöt

1. SOI = 0–100 aina. Ei koskaan yli.
2. Ei hardkoodattuja pikseliarvoja positioinnissa.
3. Ei tyhjää tilaa ilman tarkoitusta. Koskaan.
4. Ei placeholdereita keskeneräisistä ominaisuuksista — paitsi News/Updates joka on merkitty "tuleva".
5. Versiokommentti päivitetään joka muutoksen yhteydessä.
6. Tämä on räätälöity yhdelle henkilölle — ei yleisölle. Touch target -minimia ei sovelleta.
7. Älä poista widgettejä ilman eksplisiittistä ohjetta. Siirrä alemmaksi jos ei ole primääri.

---

## Roolit

### Senior Frontend Developer

**P0 — Tee nyt, tässä järjestyksessä:**

**1. Layout-rakenne:**
- Primäärit ylhäällä: SOI Meter + Timeline
- Sekundäärit alempana: Studio Pulse, Incoming Glance, Asset Radar
- News/Updates: placeholder "Tulossa" -merkinnällä alimpana
- Liukusäätimet (Self pressure, Self capacity, Calendar booked, Fragmentation): poistetaan — korvautuvat päiväraporttilla

**2. SOI-mittarin visuaalinen uudelleenrakennus (PROJECT.md kohta 4.4):**
SVG-pohjainen. Arc värigradientilla, tikitykset, hehkuva neula, syvä tausta, JetBrains Mono numeroissa.

**3. Timeline inline expand:**
Klikkaus → rivi laajenee alas. Poistaa tarpeen erilliseltä Project Detail -panelilta.

**4. Projektien tilasiirtymä:**
active ↔ incoming ↔ archived, kaksisuuntainen.

---

### Visual Designer

**Standardi:** viimeistellty instrumentti, ei prototyyppi.

**GUI-checklist:**
- [ ] SOI arc-gradientti vihreästä punaiseen
- [ ] Tikitykset arcilla
- [ ] Hehkuva neula, syvä tausta mittarissa
- [ ] JetBrains Mono SOI-numerossa
- [ ] Ei isoa tyhjää tilaa missään
- [ ] Kaikki widgetit näkyvät — primäärit ylhäällä, sekundäärit alempana
- [ ] Tilankäyttö optimoitu 375px ja 1440px

---

### QA Engineer

**QA-checklist:**
- [ ] SOI välillä 0–100
- [ ] Liukusäätimet eivät näy
- [ ] Studio Pulse näkyy sekundääriosiossa
- [ ] Incoming Glance näkyy sekundääriosiossa
- [ ] Asset Radar näkyy sekundääriosiossa
- [ ] localStorage toimii
- [ ] SW rekisteröityy ./sw.js
- [ ] Ei horisontaalista overflowta headerissa tai paneleissa
- [ ] Timeline horisontaalinen scroll toimii (tarkoituksellinen)

---

## Commit-käytäntö
```
MES/HUB vXX — [kuvaus]
```
