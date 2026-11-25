---
title: "Manuale utente — tema Meetup"
type: user-manual
status: active
theme: Meetup
screenshot: assets/front-office-home.png
created: 2026-11-25
updated: 2026-11-25
qmd: "manuale utente tema meetup screenshots"
---

> Screenshot reale del front office (`/it`), verificato il 25 novembre 2026 con `tools/visual/shot.mjs` su `http://127.0.0.1:8000`.

![Home FO](assets/front-office-home.png)

# Tema Meetup — indice manuals

## Scopo

Tema pubblico `pub_theme` del tenant: pagine Folio/Volt localizzate (`/{locale}/...`),
auth, eventi, profilo pubblico, header/footer condivisi. Questa cartella
`docs/manuals` è il contratto uniforme richiesto a ogni modulo/tema; il contenuto
operativo dettagliato per ruolo resta in [`../manuale-utente/`](../manuale-utente/readme.md).

## Screenshot verificati in questa cartella

Catturati il 25 novembre 2026 sull'app locale (database senza eventi seedati: le
pagine eventi mostrano lo stato vuoto reale). Per screenshot con dati demo usa
`docs/manuale-utente/tools/` (solo database con "demo" nel nome).

| Schermata | File | URL |
|---|---|---|
| Home italiano | [home-it.png](assets/home-it.png) | `/it` (full page) |
| Home inglese | [home-en.png](assets/home-en.png) | `/en` (full page) |
| Home mobile 375×812 | [home-mobile.png](assets/home-mobile.png) | `/it` |
| Eventi | [events-it.png](assets/events-it.png) | `/it/events` |
| Eventi passati | [events-past-it.png](assets/events-past-it.png) | `/it/events/past` |
| Contatti | [contact-it.png](assets/contact-it.png) | `/it/contact` |
| Chi siamo | [about-it.png](assets/about-it.png) | `/it/about` |
| Login | [login-it.png](assets/login-it.png) | `/it/auth/login` |
| Registrazione | [register-it.png](assets/register-it.png) | `/it/auth/register` |
| Profilo pubblico | [profile-public.png](assets/profile-public.png) | `/it/profile/{userId}` |

Esiste anche il set `assets/fo-*.webp` (stesse pagine, formato compresso) e
`assets/eventi.jpg`.

## Matrice attori (superficie pubblica del tema)

| Attore | Cosa vede / può fare |
|---|---|
| Ospite | Home, eventi (prossimi/passati, filtri città/data), contatti, chi siamo, privacy/terms/cookie-policy, login, registrazione, profilo pubblico, selettore lingua (6 locale), cookie banner. |
| Utente autenticato | Come l'ospite, più menu utente in header: dashboard, `/events/my`, eventi vicino, `/profile/edit`, logout (POST `/logout`); pagine protette e assistente AI. |
| Organizzatore / Admin | Usano il tema come visitatori; la gestione è nei pannelli Filament dei moduli. |

## Flussi reali

- **Navigazione localizzata**: header/footer da `components/sections/{header,footer}/v1`,
  link con `LaravelLocalization::localizeUrl()`.
- **Pagine CMS**: JSON in `config/local/{tenant}/database/content/pages/` resi dal
  catch-all Folio `pages/[slug].blade.php` e da `pages/[container0]/[slug0]/index.blade.php`.
- **Auth**: `pages/auth/{login,register}.blade.php` montano i widget Filament dei
  moduli User/Gdpr; logout GET in `pages/auth/logout.blade.php` e POST `/logout`.
- **Eventi**: `/events`, `/events/past`, `/events/my` (auth), `/events/{slug}`.

## Prerequisiti

- `php artisan serve` attivo, tenant `laravelpizza` con `xra.pub_theme = Meetup`.
- Tema compilato (`npm run build`) servito via symlink `public_html/themes/Meetup`;
  asset via symlink `public_html/assets/meetup`.

## Limiti

- Database di sviluppo senza eventi: `/events` mostra "Nessun evento trovato".
- Nessun toggle dark mode nel layout pubblico (componenti `ui/theme-toggle` e
  `ui/light-dark-switch` non montati in `layouts/app`).
- Link "Chat Community" rimossi da header/footer: non esiste pagina `/chat`;
  riattivare quando il modulo chat sarà disponibile.
- `/events-near-me` è un alias SEO che 301-redirecta a `/events`
  (`config/local/laravelpizza/seo.php`).
- Le chiavi di traduzione `community_chat` restano definite ma non più referenziate
  dalle sezioni live.

## Link al manuale esteso

- [Indice e guida per ruolo](../manuale-utente/readme.md) (PDF:
  [manuale-completo.pdf](../manuale-utente/pdf/manuale-completo.pdf),
  [edizione inglese](../manuale-utente/en/readme.md)).
- I capitoli `.md` non sono più nel repository (rimossi dal cleanup docs);
  il testo vivente è nelle versioni PDF e in `manuale-utente/img/`.

## Cattura screenshot

- `tools/visual/shot.mjs` (repo root) per pagine pubbliche e pannelli.
- `docs/manuale-utente/tools/capture.cjs` per il set annotato del manuale esteso
  (dati demo, `reset-demo.sh` + `seed-demo.php`, solo DB con "demo" nel nome).
