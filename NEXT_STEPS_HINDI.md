# Anshu Digital — अगला कदम (आसान हिंदी)

## अभी वेबसाइट में क्या तैयार है?
- Responsive Home page और mobile menu
- फोटो/वीडियो गैलरी और filters
- ब्लॉग/न्यूज़, डाउनलोड सेक्शन और साइट search
- हिंदी/English toggle
- Login, signup और password reset का UI
- Admin panel का UI और Supabase database/RLS schema
- ChatGPT-जैसे chat interface और Netlify server function
- Contact/WhatsApp/social links के placeholders
- SEO के लिए `robots.txt`, `sitemap.xml`, metadata और web manifest

**ध्यान दें:** Login/admin/cloud content और AI के वास्तविक रूप से चलने के लिए सेवाएँ configure करनी होंगी। यह OpenAI का आधिकारिक ChatGPT नहीं, बल्कि वेबसाइट के लिए बनाया गया Anshu AI interface है।

## काम करने का सही क्रम

### 1. पहले वेबसाइट का preview/deploy
1. ZIP को Android के Files ऐप से Extract करें।
2. पूरे `anshu_digital_website` folder को GitHub repository में upload करें।
3. Netlify में `Add new site` → `Import an existing project` → GitHub repository चुनें।
4. Publish directory `.` और Functions directory `netlify/functions` रखें (आमतौर पर `netlify.toml` इन्हें सेट करता है)।
5. Deploy के बाद मिले `*.netlify.app` URL पर साइट जाँचें।

### 2. नाम, संपर्क और सोशल लिंक
`config.js` में `whatsappNumber`, `contactEmail` और `socials` के URLs बदलें। WhatsApp नंबर देश कोड सहित केवल अंकों में लिखें, जैसे `91XXXXXXXXXX`। सार्वजनिक वेबसाइट पर निजी नंबर डालने से पहले सोचें।

### 3. Login / Signup / Admin
1. Supabase पर project बनाएँ: https://supabase.com/
2. Supabase Project URL और publishable/anon key को `config.js` में भरें। **Secret/service_role key कभी वेबसाइट में न रखें।**
3. Supabase Dashboard → SQL Editor में `supabase_schema.sql` चलाएँ।
4. Authentication → URL Configuration में अपनी Netlify site URL जोड़ें।
5. वेबसाइट पर अपना अकाउंट बनाएँ और ईमेल पुष्टि पूरी करें।
6. SQL फ़ाइल के अंत में दिए admin SQL statement में अपने login email को बदलकर चलाएँ। किसी दूसरे व्यक्ति को admin न बनाएँ। फिर logout/login करें।

### 4. Anshu AI चालू करना
1. Netlify में Site configuration → Environment variables खोलें।
2. `OPENAI_API_KEY` नाम से अपनी API key सेट करें। चाहें तो `OPENAI_MODEL` भी सेट करें।
3. साइट को redeploy करें और AI chat जाँचें।
4. API इस्तेमाल पर शुल्क लग सकता है। API key केवल Netlify environment variable में रखें—`config.js`, HTML या GitHub में नहीं। सार्वजनिक साइट पर rate limits/abuse controls भी लगाएँ।

### 5. अपना domain और Google Search
1. Domain registrar से domain खरीदें (यदि आप नाबालिग हैं तो भुगतान/रजिस्ट्रेशन में अभिभावक की मदद लें)।
2. Netlify → Domain management में domain जोड़ें और registrar में Netlify द्वारा बताए DNS records सेट करें।
3. `robots.txt` और `sitemap.xml` में `https://YOUR-DOMAIN.example` को अपने domain से बदलें।
4. Google Search Console में domain verify करके sitemap submit करें। Google में दिखने में समय लग सकता है; इसकी गारंटी नहीं होती।

## सुरक्षा
- सार्वजनिक gallery/download storage में निजी दस्तावेज़ या संवेदनशील जानकारी न डालें।
- Chat में पासवर्ड, API keys या निजी जानकारी न लिखें।
- AI गलत उत्तर दे सकता है; ज़रूरी जानकारी जाँचें।
- Deploy करने से पहले सभी placeholders और links जाँच लें।
