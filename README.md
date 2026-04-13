# SmileQuest — Landing + Waitlist + Dashboard

## Stack
- **Next.js 14** (App Router)
- **Tailwind CSS** + custom fonts (Google Fonts)
- **Supabase** (database + auth + edge functions voor emails)
- **Resend** (transactionele emails)

---

## 1. Project aanmaken

```bash
npx create-next-app@latest smilequest --typescript --tailwind --app
cd smilequest
npm install @supabase/supabase-js resend
```

Kopieer alle bestanden uit deze map naar je project.

---

## 2. Supabase instellen

1. Ga naar [supabase.com](https://supabase.com) → nieuw project aanmaken
2. Ga naar **SQL Editor** → plak de inhoud van `supabase/schema.sql` → Run
3. Ga naar **Project Settings → API** en kopieer:
   - `Project URL`
   - `anon/public key`
   - `service_role key` (alleen voor server-side gebruik)

---

## 3. Resend instellen (emails)

1. Ga naar [resend.com](https://resend.com) → gratis account
2. Maak een API key aan
3. Verifieer je domein (of gebruik `onboarding@resend.dev` voor testen)

---

## 4. Environment variables

Maak `.env.local` aan in de root:

```env
NEXT_PUBLIC_SUPABASE_URL=https://jouwproject.supabase.co
NEXT_PUBLIC_SUPABASE_ANON_KEY=jouw_anon_key
SUPABASE_SERVICE_ROLE_KEY=jouw_service_role_key
RESEND_API_KEY=re_jouw_api_key
RESEND_FROM_EMAIL=hallo@smilequest.nl
DASHBOARD_PASSWORD=kiesEenSterkWachtwoord123
```

---

## 5. Draaien

```bash
npm run dev
```

- **Landing + waitlist**: `http://localhost:3000`
- **Leads dashboard**: `http://localhost:3000/dashboard`

---

## 6. Deployen (Vercel)

```bash
npx vercel
```

Voeg dezelfde `.env.local` variabelen toe in Vercel → Project Settings → Environment Variables.

---

## Architectuur

```
/app
  page.tsx              ← Landing page (Nederlands)
  /api/waitlist
    route.ts            ← POST endpoint: Supabase + email
  /dashboard
    page.tsx            ← Leads dashboard (wachtwoord beveiligd)
/components
  WaitlistForm.tsx      ← Email capture component
  CalendarPreview.tsx   ← 24-dagen preview animatie
/lib
  supabase.ts           ← Supabase client
  resend.ts             ← Email helper
/supabase
  schema.sql            ← Database schema
```

---

## Email flow

1. Ouder vult email in op landing page
2. `POST /api/waitlist` → opslaan in Supabase
3. Automatisch welkomstmail via Resend
4. Dashboard toont alle leads met statistieken

---

## Dashboard beveiliging

Het dashboard is beveiligd met een simpel wachtwoord via `DASHBOARD_PASSWORD`. Voor productie: vervang dit door Supabase Auth of NextAuth.
