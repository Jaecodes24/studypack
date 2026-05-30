# Studypack — Setup & Deployment Guide

## Stack
- **Frontend**: Single HTML file, no build step
- **File storage**: Cloudinary (free tier)
- **Database**: Supabase (free tier) — stores resource metadata so all visitors see the same uploads
- **Hosting**: Netlify (free tier, GitHub-connected)

---

## Deploy to Netlify (same as Gold Leaf)
1. Push this folder to a GitHub repo
2. Netlify → New site → Import from Git → select repo
3. Build command: *(leave blank)*
4. Publish directory: `/`
5. Deploy

---

## Connect Cloudinary (file storage)
1. Sign up free at https://cloudinary.com
2. Dashboard → Settings → Upload → Add upload preset
   - Mode: **Unsigned**
   - Name it: `studypack_uploads`
3. Copy your **Cloud Name** from the dashboard
4. In `index.html`, update:
```js
const CLOUDINARY_CLOUD_NAME    = 'your_cloud_name_here';
const CLOUDINARY_UPLOAD_PRESET = 'studypack_uploads';
```

---

## Connect Supabase (shared database)
1. Sign up free at https://supabase.com → create a new project
2. Go to SQL Editor and run this:
```sql
CREATE TABLE resources (
  id uuid DEFAULT gen_random_uuid() PRIMARY KEY,
  title text NOT NULL,
  subject text NOT NULL,
  description text,
  type text,
  url text,
  date date DEFAULT CURRENT_DATE
);
ALTER TABLE resources ENABLE ROW LEVEL SECURITY;
CREATE POLICY "Public read"   ON resources FOR SELECT USING (true);
CREATE POLICY "Public insert" ON resources FOR INSERT WITH CHECK (true);
```
3. Go to Project Settings → API → copy **Project URL** and **anon/public key**
4. In `index.html`, update:
```js
const SUPABASE_URL  = 'https://your-project.supabase.co';
const SUPABASE_ANON = 'your_anon_key_here';
```

---

## Demo mode
Until credentials are set, the site runs in demo mode:
- Uploads save to browser localStorage (device only)
- No Cloudinary or Supabase calls are made
- Safe to test everything locally before going live
