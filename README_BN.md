# TradeLedger Pro — GitHub + Render + Supabase

এই ভার্সনে Email/Password authentication, Supabase cloud database, private screenshot storage, dark/light theme, calendar, monthly report, strategy performance এবং paper-trading risk calculator আছে।

## খরচ ও Payment Card
- Frontend hosting: Render **Static Site Free** নির্বাচন করো; paid Web Service নয়।
- Database/Auth/Storage: Supabase-এর Free plan দিয়ে শুরু করো।
- Free plan-এর সীমা, নিষ্ক্রিয় project pause, storage/bandwidth limits এবং payment-card policy সময়ের সঙ্গে বদলাতে পারে। Dashboard যদি card চায় বা paid upgrade দেখায়, এগোবে না। কোনো paid plan/credit কিনবে না।
- এই starter-এ payment integration নেই এবং প্রয়োজনও নেই।

## ধাপ ১ — Supabase project তৈরি
1. https://supabase.com/ এ account খোলো এবং Free project তৈরি করো।
2. Project name: `tradeledger-pro` (যেকোনো নাম চলবে)।
3. Database password নিজের কাছে নিরাপদে রাখবে; কাউকে পাঠাবে না।
4. Project তৈরি হলে Dashboard → SQL Editor → New query খোলো।
5. এই ZIP-এর `supabase/schema.sql` ফাইলের সম্পূর্ণ SQL কপি করে SQL Editor-এ চালাও।
6. Authentication → Providers → Email enabled আছে কি দেখো।
7. Authentication → URL Configuration-এ Render URL পরে Site URL এবং Redirect URLs-এ যোগ করবে।

## ধাপ ২ — Supabase key বসানো
1. Supabase → Project Settings → API (কিংবা API Keys) খোলো।
2. Project URL কপি করো।
3. `anon` / `publishable` public key কপি করো। `service_role` বা secret key কখনো ব্যবহার করবে না।
4. `config.js` খুলে:
   - `PASTE_YOUR_SUPABASE_PROJECT_URL`-এর জায়গায় Project URL
   - `PASTE_YOUR_SUPABASE_ANON_KEY`-এর জায়গায় anon/public key
5. Save করো।

**গুরুত্বপূর্ণ:** anon/publishable key frontend-এ থাকা স্বাভাবিক; ডেটা সুরক্ষা RLS policy দিয়ে হয়। Service role key frontend-এ দিলে পুরো database ঝুঁকিতে পড়বে।

## ধাপ ৩ — GitHub repository
1. https://github.com/ → New repository।
2. নাম: `tradeledger-pro`
3. এই ZIP Extract করো।
4. নিচের ফাইলগুলো repository root-এ upload করো: `index.html`, `style.css`, `app.js`, `config.js`, `README_BN.md`।
5. `supabase` folder-টিও upload করো (এর মধ্যে `schema.sql` আছে)।
6. Commit changes করো।
7. `config.js`-এ কোনো secret/service_role key নেই নিশ্চিত হও।

## ধাপ ৪ — Render Free Static Site
1. https://render.com/ → New → **Static Site**।
2. GitHub connect করে `tradeledger-pro` repo নির্বাচন করো।
3. Branch: `main`
4. Build Command: ফাঁকা
5. Publish Directory: `.`
6. Free instance/static site option নির্বাচন করো।
7. Create Static Site চাপো। Render `https://...onrender.com` URL দেবে।
8. Static Site hosting-এর জন্য payment card চাওয়া হলে এগোবে না; paid service নির্বাচন করবে না।

## ধাপ ৫ — Auth URL সেটিং
1. Render URL কপি করো।
2. Supabase → Authentication → URL Configuration।
3. Site URL-এ Render URL বসাও।
4. Redirect URLs-এ Render URL এবং `https://তোমার-render-url/**` যোগ করো।
5. Save করো। এরপর ওয়েবসাইট refresh করে account তৈরি করো।
6. Email confirmation চালু থাকলে inbox-এ confirmation link আসবে। প্রয়োজন হলে Supabase Auth settings-এ email confirmation policy দেখো।

## কীভাবে ব্যবহার করবে
- Sign up: নিজের email/password দিয়ে account তৈরি।
- Trade Log: simulated trade, setup, entry/stop/exit, quantity, fees এবং screenshot যোগ।
- Calendar: প্রতিদিনের aggregate simulated P/L।
- Strategy: strategy নাম অনুযায়ী trade statistics।
- Monthly Report: মাস বেছে summary এবং reflection save।
- Risk Calculator: কেবল hypothetical practice হিসাব।
- Daily Journal: দিনের context, ভালো সিদ্ধান্ত, ভুল ও পরের লক্ষ্য।
- CSV Export: নিজের রেকর্ডের কপি ডাউনলোড।

## Data security / privacy
- Database tables-এ Row Level Security (RLS) চালু; user_id দিয়ে প্রত্যেকে শুধু নিজের row access করতে পারে।
- Screenshot bucket private; signed URL স্বল্প সময়ের জন্য তৈরি হয়।
- Public repository-তে config.js-এ শুধু public anon/publishable key থাকবে।
- Password, database password, service_role key, secret key কখনো GitHub-এ দেবে না।
- নিজের account-এ শক্তিশালী, অন্য কোথাও ব্যবহার না করা password দাও।

## Troubleshooting
- `Invalid API key`: config.js-এ project URL এবং anon/publishable key ঠিক আছে কি দেখো।
- `relation does not exist`: `supabase/schema.sql` পুরোটা SQL Editor-এ চালাও।
- `row-level security` error: logged in আছ কি না দেখো; SQL policy তৈরি হয়েছে কি না যাচাই করো।
- Screenshot error: Storage bucket `trade-screenshots` এবং policies তৈরি হয়েছে কি না দেখো; ছবি 5MB-এর কম JPG/PNG/WEBP হতে হবে।
- Email confirmation না এলে spam folder দেখো। Supabase Free email sending limits থাকতে পারে।
- Deploy-এর পর config.js পরিবর্তন করলে GitHub-এ commit করে redeploy করো।

## সীমাবদ্ধতা
- এটি educational journal, broker connection নয়।
- কোনো live order, broker API, trade signal, payment, subscription বা financial advice নেই।
- P/L হিসাব simple price-difference × quantity; contract multiplier, pip value, spread, tax, funding, slippage বা সব asset-specific rules পুরোপুরি model করে না।
- Calendar/report analytics-কে historical practice review হিসেবে দেখো, ভবিষ্যৎ ফলের পূর্বাভাস হিসেবে নয়।
