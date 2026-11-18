# QResto – digitales Bestell- und Speisekartensystem für Restaurants

> 🇬🇧 [English version below](#english)
>
> Der Quellcode liegt in einem privaten Repository. Diese Seite beschreibt das Projekt;
> die Live-Version ist oben rechts unter „About“ verlinkt.

QResto ist eine Web-Anwendung, mit der Restaurants **ohne App-Installation** Bestellungen am
Tisch (per QR-Code), online zur Abholung oder Lieferung annehmen und ihre Speisekarte digital
verwalten. Gäste bestellen im Browser am Handy; Service, Küche und Verwaltung arbeiten auf
eigenen Bildschirmen. Ein Restaurant kann einzelne Bausteine nutzen (z. B. nur die digitale
Speisekarte) oder alles zusammen.

**Status:** in aktiver Entwicklung, Pilotbetrieb in Vorbereitung.

---

## Für wen?

- **Gäste:** Speisekarte in 8 Sprachen lesen, am Tisch per QR-Code bestellen, online zur Abholung
  oder Lieferung bestellen und den Bestellstatus live verfolgen.
- **Restaurant-Personal:** eigene Ansichten für Service (Kellner), Küche und Online-Bestellungen.
- **Inhaber:** Speisekarte, Personal, Öffnungszeiten, Liefergebiet und Zahlungsarten selbst pflegen.
- **Plattform-Betreiber:** mehrere Restaurants in einer Installation verwalten (Mandantenfähigkeit).

## Funktionen

**Gäste**
- QR-Code am Tisch → Speisekarte → Bestellung, ohne Registrierung
- Online-Bestellung: Abholung oder Lieferung mit Adressprüfung (Liefergebiet per Radius),
  Mindestbestellwert, Wunschzeit
- Live-Bestellstatus (bestätigt, fertig, unterwegs) und Stornierung durch den Gast
- 8 Sprachen (DE, EN, FR, IT, ES, TR, RU, PL), Allergene und Ernährungshinweise
- Verschiedene Designs pro Restaurant (Farbpaletten und Layouts)

**Personal**
- Kellner-Ansicht pro Tisch, Küchen-Ansicht, Online-Bestellpanel mit Bestätigen/Ablehnen/Zeit zusagen
- Küchenbon (Bildschirm, PDF und Bondrucker-Format) mit Navigations-QR-Code für die Lieferung
- Bestellverlauf

**Verwaltung**
- Speisekarte mit Kategorien, Optionsgruppen (z. B. Größe, Beilagen), Bildern, Drag & Drop
- Verkaufskanal pro Artikel (vor Ort / online / beides)
- Öffnungszeiten, Feiertage, Pausen, Liefergebiet, akzeptierte Zahlungsarten
- Personalverwaltung mit Rollen (Inhaber, Kellner, Küche, Online)

## Technik

| Bereich | Eingesetzt |
|---|---|
| Frontend | Next.js 16 (App Router), React 19, TypeScript, Tailwind CSS 4 |
| Backend | Supabase: PostgreSQL, Auth, Realtime, Storage, Vault, pg_cron |
| Serverlogik | PL/pgSQL-Funktionen (Bestellung, Preisberechnung, Statuswechsel) |
| Tests | Vitest, Testing Library (rund 2.500 automatisierte Tests), SQL-Tests für Migrationen |
| Weitere | QR-Codes, PDF-Erzeugung (jsPDF), Drag & Drop (dnd-kit), Geokodierung |
| Hosting | Vercel |

## Sicherheit und Datenschutz (DSGVO)

- **Row Level Security** auf allen Tabellen; jede Rolle sieht nur die Daten ihres Restaurants
- **Preise werden auf dem Server berechnet** – der Browser schickt nur Artikel und Mengen,
  ein manipulierter Preis wird abgelehnt
- **Signierte Lieferadresse** (HMAC): nur eine vom Server geprüfte Adresse wird angenommen
- **Datensparsamkeit:** Gäste bestellen ohne Konto; personenbezogene Bestelldaten werden nach
  30 Tagen automatisch anonymisiert
- Rechtliche Vorgaben für den Online-Verkauf berücksichtigt (u. a. Bestellbutton nach § 312j BGB,
  Hinweise zu Datenschutz und Widerrufsrecht)
- Regelmäßige Sicherheitsüberprüfungen; Datenbankrechte nach dem Prinzip der geringsten Rechte

## Arbeitsweise

- **Testgetriebene Entwicklung (TDD):** erst der fehlschlagende Test, dann der Code
- Jede größere Änderung beginnt mit einem schriftlichen Konzept und Plan, wird auf einem eigenen
  Branch umgesetzt und per Pull Request übernommen
- Datenbankänderungen als versionierte Migrationen (über 60), jeweils mit eigenem SQL-Test
- Entscheidungsprotokoll: jede wichtige Entscheidung mit Begründung und verworfenen Alternativen
- Entwicklung mit KI-Unterstützung (Claude Code, OpenAI Codex): Konzept, Entscheidungen,
  Prüfung und Tests liegen bei mir

---

<a id="english"></a>

# QResto – digital ordering and menu system for restaurants

> The source code lives in a private repository. This page describes the project;
> the live version is linked under "About" at the top right.

QResto is a web application that lets restaurants take orders **without any app installation**:
at the table via QR code, online for pickup or delivery, and manage their menu digitally.
Guests order in the browser on their phone; waiters, kitchen and management each work on their
own screens. A restaurant can use single modules (e.g. only the digital menu) or everything together.

**Status:** in active development, pilot operation in preparation.

## Who is it for?

- **Guests:** read the menu in 8 languages, order at the table via QR code, order online for pickup
  or delivery and follow the order status live.
- **Restaurant staff:** dedicated views for waiters, kitchen and online orders.
- **Owners:** manage menu, staff, opening hours, delivery area and payment methods themselves.
- **Platform operator:** manage several restaurants in one installation (multi-tenant).

## Features

**Guests**
- QR code at the table → menu → order, no sign-up
- Online ordering: pickup or delivery with address check (delivery radius), minimum order value,
  preferred time
- Live order status (confirmed, ready, on the way) and cancellation by the guest
- 8 languages (DE, EN, FR, IT, ES, TR, RU, PL), allergens and dietary information
- Different designs per restaurant (colour palettes and layouts)

**Staff**
- Waiter view per table, kitchen view, online order panel to confirm, reject and promise a time
- Kitchen ticket (screen, PDF and receipt-printer format) with a navigation QR code for delivery
- Order history

**Management**
- Menu with categories, option groups (e.g. size, sides), images, drag & drop
- Sales channel per item (in-house / online / both)
- Opening hours, holidays, pauses, delivery area, accepted payment methods
- Staff management with roles (owner, waiter, kitchen, online)

## Tech stack

| Area | Used |
|---|---|
| Frontend | Next.js 16 (App Router), React 19, TypeScript, Tailwind CSS 4 |
| Backend | Supabase: PostgreSQL, Auth, Realtime, Storage, Vault, pg_cron |
| Server logic | PL/pgSQL functions (ordering, price calculation, status changes) |
| Testing | Vitest, Testing Library (around 2,500 automated tests), SQL tests for migrations |
| Other | QR codes, PDF generation (jsPDF), drag & drop (dnd-kit), geocoding |
| Hosting | Vercel |

## Security and privacy (GDPR)

- **Row Level Security** on every table; each role only sees its own restaurant's data
- **Prices are calculated on the server** – the browser only sends items and quantities,
  a manipulated price is rejected
- **Signed delivery address** (HMAC): only an address checked by the server is accepted
- **Data minimisation:** guests order without an account; personal order data is anonymised
  automatically after 30 days
- German online-sales rules taken into account (e.g. order button under § 312j BGB, privacy and
  withdrawal notices)
- Regular security reviews; database permissions follow the principle of least privilege

## How I work

- **Test-driven development (TDD):** failing test first, then the code
- Every larger change starts with a written design and plan, is built on its own branch and merged
  via pull request
- Database changes as versioned migrations (more than 60), each with its own SQL test
- Decision log: every important decision with its reasoning and the rejected alternatives
- Developed with AI assistance (Claude Code, OpenAI Codex): concept, decisions, review and tests
  are my responsibility
