# WorkTime — Next.js + Supabase

Աշխատակիցների ներկայության (attendance) համակարգ՝ QR կոդով check-in/check-out։

## Ֆունկցիոնալություն

- Բիզնեսի գրանցում և մուտք (Supabase Auth)
- Աշխատակիցների ավելացում/ջնջում
- Յուրաքանչյուր աշխատակցի համար եզակի QR կոդ (դիտում + ներբեռնում)
- Scanner էջ (կամերայով) QR կոդը սկանավորելու համար՝ ավտոմատ check-in/check-out
- Dashboard overview՝ ընդհանուր աշխատակիցներ, հիմա ներկա գտնվողներ, օրվա գրառումներ

## 1. Տեղադրում

```bash
npm install
cp .env.example .env.local
```

`.env.local`-ում ավելացրու Supabase Project URL-ը և anon key-ը (Supabase Dashboard → Project Settings → API)։

## 2. Database

Supabase Dashboard → SQL Editor → գործարկիր `supabase/schema.sql`-ը։

Այն ստեղծում է `businesses`, `employees`, `attendance` աղյուսակները և Row Level Security policy-ները, որոնք ապահովում են, որ յուրաքանչյուր բիզնես տեսնում է միայն իր տվյալները։

## 3. Աշխատեցնել

```bash
npm run dev
```

Բացիր `http://localhost:3000`

## 4. Flow

Register business → Login → Dashboard → Add employee → Scanner → սկանավորիր աշխատակցի QR կոդը։

Առաջին սկանավորումը գրանցում է check-in, հաջորդը (նույն օրվա ընթացքում)՝ check-out։

## Կառուցվածք

```
app/
  login/, register/       — auth pages
  dashboard/
    page.tsx               — overview (stats + today's activity)
    employees/page.tsx     — employee list + add form + QR codes
    scanner/page.tsx       — camera QR scanner + check-in/out logic
components/                — reusable client components
lib/supabase/               — browser/server Supabase clients
proxy.ts                    — session refresh + route protection (Next.js middleware/proxy)
supabase/schema.sql          — tables + RLS policies
```

## Տեխնոլոգիաներ

Next.js (App Router) · TypeScript · Tailwind CSS v4 · Supabase (Auth + Postgres + RLS) · `qrcode` (գեներացիա) · `html5-qrcode` (սկանավորում) · `lucide-react`

### Կարևոր

Այս starter-ը իրական QR check-in/check-out-ն ունի։ Դեմքի biometric verification-ը դիտավորյալ առանձին փուլ է, որովհետև production համակարգում պետք է օգտագործել մասնագիտացված face-recognition SDK/API, consent, liveness detection և անվտանգ biometric storage։

Production-ում երբեք մի օգտագործիր service-role key browser-ում։ Այս starter-ը ամբողջությամբ աշխատում է anon key + RLS-ով, ինչը անվտանգ է browser-ի կողմից օգտագործման համար։
