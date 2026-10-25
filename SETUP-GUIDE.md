# Gaya Sewa: shuru karne ki guide

Do tareeke hain. Pehle **A** se shuru kijiye, jab 20–30 partner jud jaayein tab **B** par jaiye.

## A. WhatsApp wala (bina server, aaj hi)
1. Ek **WhatsApp number** lijiye jo sirf Gaya Sewa ke liye ho. Yeh "dispatcher" number hoga.
2. WhatsApp me ek **Partner group** banaiye aur uska invite link copy kijiye (Group info → Invite via link).
3. Dono files (`gaya-sewa.html` aur `gaya-sewa-live.html`) me aapka number **9229255775** pehle se bhara hua hai:
   `const DISPATCH="9229255775",GROUP="";`
   Partner group ban jaye to uska link `GROUP` me daaliye, jaise: `GROUP="https://chat.whatsapp.com/xxxx"`. Number badalna ho to bhi isi line me badaliye.
4. Ab customer "WhatsApp par bhejiye" dabaye to order seedha aapke number par aayega, aur partner panel me "WhatsApp group se judiye" button dikhega.
5. Aap order group me forward kijiye. Jo pehle jawab de, use deejiye.

Kamzori: sab kuch haath se karna padta hai aur customer ko khud WhatsApp dabana padta hai.

## B. Asli server wala (Firebase)
File `gaya-sewa-live.html` me Firebase ka code pehle se juda hai. Bas config bharna hai.

1. https://console.firebase.google.com par jaiye → **Add project** (naam: gaya-sewa).
2. **Build → Firestore Database → Create database** (production mode, Mumbai `asia-south1` region).
3. **Build → Authentication → Get started → Sign-in method → Anonymous → Enable.**
4. **Project settings (gear) → Your apps → Web (`</>`)** me app register kijiye. Jo `firebaseConfig` mile use copy kijiye.
5. `gaya-sewa-live.html` me yeh line dhundhiye aur config paste kijiye:
   `window.FB_CONFIG=null;` ko badal kar
   `window.FB_CONFIG={apiKey:"...",authDomain:"...",projectId:"...",appId:"..."};`
6. **Firestore → Rules** me `firestore.rules` ki poori file paste karke **Publish** kijiye. (Yeh sabse zaroori step hai: yahi customer ka number surakshit rakhta hai.)
7. File ko internet par daaliye. Teen aasaan raaste hain (teeno me HTTPS link milta hai; HTTPS ke bina GPS aur awaaz theek se nahi chalte):
   - **GitHub Pages (free):** neeche wale hisse me step diye hain.
   - **Netlify:** site ka folder drag-and-drop kijiye.
   - **Firebase Hosting:** `npm i -g firebase-tools`, `firebase login`, `firebase init hosting`, `firebase deploy`.

### GitHub par daalna
Dhyan rakhiye: sirf `.md` file GitHub par daalne se website nahi chalegi. `.md` sirf padhne ki guide hai, wo GitHub par bas likha hua dikhegi. Website ke liye `gaya-sewa-live.html` chahiye.
1. github.com par naya repository banaiye (naam: `gaya-sewa`).
2. **Add file → Upload files** se yeh teen file daaliye: `gaya-sewa-live.html`, `SETUP-GUIDE.md`, `firestore.rules`.
3. `gaya-sewa-live.html` ka naam badal kar **`index.html`** kar dijiye (file kholkar pencil icon se rename). Tabhi link seedha khulega.
4. Repository me **Settings → Pages → Branch: main, folder: / (root) → Save.**
5. 1–2 minute baad link milega, jaise `https://aapkanaam.github.io/gaya-sewa/`. Wahi aapki website hai.
6. Firebase console → **Authentication → Settings → Authorized domains** me `aapkanaam.github.io` jodiye, warna login nahi chalega.

Dhyan: public repository me aapka WhatsApp number aur Firebase config sabko dikhega. Firebase config chhupana zaroori nahi hota (suraksha `firestore.rules` se aati hai), par number ko lekar soch lijiye. Aap private repo bhi bana sakte hain, par free GitHub Pages ke liye aam taur par public repo chahiye hoti hai.

### Partner ko approve karna
1. Partner link kholkar **Partner panel** jaata hai. Wahan "Aapka partner ID: ..." likha dikhta hai.
2. Woh ID aapko WhatsApp par bhejta hai. Aap uske document (Aadhaar, licence, etc.) dekhte hain.
3. Firebase console → Firestore → **Start collection** `partners` → Document ID = uski ID, field `ok` = true.
4. Ab wahi partner "Alert chalu karein" dabaye to orders aur awaaz aane lagenge.

### Test kaise karein
Do phone lijiye. Ek par customer ban kar order dijiye, doosre par approved partner ban kar alert chalu rakhiye. Order dete hi doosre phone par peela alert aur awaaz aani chahiye.

## Aage kya karna hoga
- **Phone OTP:** abhi customer anonymous hai, to koi bhi jhoote order bhej sakta hai. Firebase Authentication me *Phone* login jodiye (SMS ka kharcha aata hai) aur **App Check** chalu kijiye.
- **Push notification (FCM):** abhi awaaz tabhi aati hai jab page khula ho. Phone band hone par bhi alert ke liye Firebase Cloud Messaging + service worker chahiye. Yeh agla bada step hai.
- **Bhugtan:** Razorpay ya UPI se online payment baad me jod sakte hain.
- **Kanooni:** business registration, GST (zaroorat ho to), aur partner ke saath likhit shartein tay kar lijiye. Kisi vakeel ya CA se ek baar salah le lijiye.
- Firebase ke free plan ki seemayen aur daam badalte rehte hain. Shuru karne se pehle unka pricing page dekh lijiye.
