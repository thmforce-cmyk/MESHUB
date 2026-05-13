# Azure AD App Registration — Tekninen briiffi Promealle

**Projekti:** MES/HUB — ME Studio Dashboard  
**Pyytäjä:** Tomi Forsblom, tomi.forsblom@mestudio.fi  
**Päivämäärä:** 2026-05-13

---

## Tavoite

ME Studion sisäinen hallintadashboard tarvitsee pääsyn Microsoft 365 -dataan ilman että käyttäjän tarvitsee syöttää tietoja manuaalisesti. Tämä vaatii yhden Azure AD -sovelluksen rekisteröinnin.

---

## Toimenpiteet

### 1. Luo sovellus Azure-portaalissa

portal.azure.com → Azure Active Directory → App registrations → **New registration**

| Kenttä | Arvo |
|--------|------|
| Name | MESHUB |
| Supported account types | Accounts in this organizational directory only |
| Platform | Single-page application (SPA) |
| Redirect URI | `https://thmforce-cmyk.github.io/MESHUB/` |

### 2. Lisää API-oikeudet

API permissions → Add a permission → Microsoft Graph → **Delegated permissions**

Lisää seuraavat:

| Permission | Tarkoitus |
|-----------|-----------|
| `User.Read` | Käyttäjän perustiedot kirjautumiseen |
| `Calendars.Read` | Outlook-kalenteri → Studio Pulse |
| `Tasks.ReadWrite` | Planner-tehtävät → SOI Mental Demand |
| `Sites.Read.All` | SharePoint Excel-tiedostot → Incoming |
| `offline_access` | Token-uusinta ilman uutta kirjautumista |

### 3. Anna admin consent

API permissions -sivulla: **Grant admin consent for ME Studio**

Tämä on pakollinen — ilman sitä jokainen käyttäjä joutuisi erikseen hyväksymään oikeudet.

### 4. Toimita tunnisteet

Overview-sivulta kopioi ja toimita Tomi Forsblomin käyttöön:

- **Application (client) ID** — esim. `a1b2c3d4-...`
- **Directory (tenant) ID** — esim. `e5f6g7h8-...`

**Ei salasanoja, ei client secretejä.** SPA-sovellukset käyttävät PKCE-autentikaatiota joka ei vaadi salaisuuksia.

---

## Huomiot

- Sovellus on Single-Page Application — ei palvelinta, ei backendiä
- Kaikki data käsitellään käyttäjän selaimessa
- Microsoft ei lähetä dataa kolmansille osapuolille
- Rekisteröinti ei vaikuta muihin ME Studion Microsoft-palveluihin
