# Anshu Digital — Full Website Starter

एक responsive Hindi/English personal website starter जिसमें:
- होम पेज और परिचय
- फोटो/वीडियो गैलरी और फ़िल्टर
- ब्लॉग / न्यूज़ सेक्शन
- डाउनलोड सेक्शन
- वेबसाइट के भीतर सर्च
- ईमेल, WhatsApp और सोशल लिंक
- हिंदी / English toggle
- लॉगिन / साइनअप / पासवर्ड रीसेट (Supabase से जोड़ने के बाद)
- Admin panel और persistent posts/media/downloads (Supabase + RLS + Storage के बाद)
- ChatGPT-जैसा chat interface और असली AI response path (Netlify Function + API key के बाद)
- SEO files: `robots.txt`, `sitemap.xml`, Open Graph metadata, `manifest.webmanifest`

> **महत्वपूर्ण:** साइट का UI तैयार है, लेकिन वास्तविक authentication, cloud content और AI responses चलाने के लिए नीचे के सेटअप चरण पूरे करने होंगे। बिना सेटअप के लॉगिन/एडमिन/AI पूरी तरह काम करने का दावा नहीं किया गया है।

## 1. स्थानीय रूप से देखें
1. ZIP extract करें।
2. `index.html` को ब्राउज़र में खोलें।
3. `config.js` में अपनी सार्वजनिक वेबसाइट सेटिंग्स बदलें।
4. AI function को चलाकर देखने के लिए Netlify पर functions सहित deploy करें; केवल `index.html` खोलने से AI API नहीं चलेगी।

## 2. Contact और social links
`config.js` में `contactEmail`, `whatsappNumber` (सिर्फ अंक, country code सहित) और social URLs बदलें। सार्वजनिक वेबसाइट पर अपना निजी फोन/ईमेल डालने से पहले सोचें।

## 3. असली login, signup और Admin panel — Supabase
1. Supabase पर project बनाएँ: https://supabase.com/
2. Project settings से Project URL और publishable/anon key लें। केवल सार्वजनिक client key को `config.js` में रखें; **service_role key कभी नहीं**।
3. Supabase Dashboard → SQL Editor में `supabase_schema.sql` की सामग्री चलाएँ। इससे tables, Row Level Security (RLS) और `media` bucket की policies बनेंगी।
4. `config.js` में `supabaseUrl` और `supabaseAnonKey` भरें।
5. अपनी साइट के URL को Supabase Authentication → URL Configuration में Site URL / redirect allow-list में जोड़ें। Email confirmation चालू रखें।
6. वेबसाइट पर अपना अकाउंट बनाएँ और email confirm करके login करें।
7. SQL editor में schema फ़ाइल के अंत में दिया गया `UPDATE auth.users ...` statement **अपना ईमेल भरकर** चलाएँ; यह आपके अकाउंट को admin role देगा। फिर logout करके दोबारा login करें। यह admin role किसी और को न दें।
8. अब Admin panel से पोस्ट, फोटो/वीडियो और डाउनलोड जोड़ें। Storage upload 50 MB तक सीमित है और MIME प्रकार भी सीमित हैं; वीडियो के लिए अपने Storage plan/limits देखें।

प्रकाशित सामग्री public होगी। निजी दस्तावेज़ या व्यक्तिगत जानकारी इस public media bucket में न डालें। RLS database rows को सुरक्षित करती है; client-side UI अकेले सुरक्षा नहीं है।

## 4. ChatGPT-जैसा AI चैट — Netlify Function
1. प्रोजेक्ट को GitHub repository में अपलोड करें और Netlify में **Git से import** करें ताकि `netlify/functions/chat.mjs` deploy हो सके। केवल static files का drag-and-drop functions deploy करने के लिए पर्याप्त नहीं भी हो सकता।
2. Netlify site → Project configuration / Environment variables में `OPENAI_API_KEY` जोड़ें। `OPENAI_MODEL` optional है; default `gpt-4.1-mini` है।
3. Environment variable को Functions के लिए उपलब्ध रखें और साइट को redeploy करें।
4. AI API usage के लिए provider account/billing की जरूरत हो सकती है। API key को `config.js`, HTML, source control या browser code में कभी न डालें।

`netlify/functions/chat.mjs` पिछले 12 chat messages तक सीमित रखता है और प्रति request input size सीमित करता है, पर public deployment पर अतिरिक्त rate limiting / abuse controls की जरूरत हो सकती है। API use से खर्च हो सकता है।

Official references:
- Supabase signup: https://supabase.com/docs/reference/javascript/auth-signup
- Supabase JS auth overview: https://supabase.com/docs/reference/javascript/auth
- Netlify Functions environment variables: https://docs.netlify.com/build/functions/environment-variables/
- OpenAI API documentation: https://platform.openai.com/docs/api-reference/responses

## 5. Domain और Google Search
1. पहले Netlify पर site deploy करें और काम करता हुआ `*.netlify.app` URL जाँचें।
2. अपने नाम का domain किसी registrar से लें; यदि उपयोगकर्ता minor है तो payment/registration के लिए अभिभावक की मदद लें।
3. Netlify → Domain management में domain जोड़ें और Netlify द्वारा बताए गए DNS records को registrar में सेट करें। DNS बदलाव में कुछ समय लग सकता है।
4. `sitemap.xml` और `robots.txt` में `https://YOUR-DOMAIN.example` को अपने असली domain से बदलें। `index.html` का title/description भी अपने content के अनुसार रखें।
5. Google Search Console में अपना domain/site verify करें और sitemap submit करें। इससे Google में आने की गारंटी नहीं मिलती, लेकिन discovery में मदद मिलती है।

## 6. फ़ाइलें
- `index.html` — साइट संरचना
- `style.css` — responsive design
- `script.js` — search, language, auth, admin और chat UI
- `config.js` — सार्वजनिक settings placeholders
- `supabase_schema.sql` — database, RLS और storage setup
- `netlify/functions/chat.mjs` — server-side AI proxy
- `netlify.toml` — Netlify publish/functions config
- `robots.txt`, `sitemap.xml`, `manifest.webmanifest` — discovery/PWA metadata
- `assets/website-guide.txt` — example download
