# 🧩 Google Tag Manager (GTM) in die Testseite einbauen

Diese Anleitung zeigt dir Schritt für Schritt, wie du den GTM  
in die bereitgestellte HTML-Testseite integrierst und nutzt.

---

# 🟢 0. Testseite lokal starten (ganz einfach)

Du brauchst **keine Programmierkenntnisse** dafür.

## ✅ Variante A (super einfach – empfohlen)

1. Öffne einen Texteditor (z. B. Notepad)
2. Kopiere den HTML-Code hinein
3. Speichere die Datei als:
index.html

4. Doppelklicke auf die Datei

👉 Die Seite öffnet sich direkt im Browser

---

## ⚠️ Wichtig

Für den Google Tag Manager funktioniert das manchmal **nicht zuverlässig** mit Doppelklick.

---

## ✅ Variante B (funktioniert immer)

👉 Nutze einen einfachen lokalen Server:

### Option 1: Mit Visual Studio

1. Installiere Visual Studio Code https://code.visualstudio.com/
2. Öffne den Ordner mit deiner `index.html`
3. Installiere die Erweiterung **Live Server**
4. Rechtsklick auf `index.html` → **„Open with Live Server“**

👉 Die Seite öffnet sich automatisch im Browser

---

### Option 2: Ohne Installation (wenn Python vorhanden ist)

1. Öffne die Kommandozeile im Ordner
2. Tippe:

```bash
python -m http.server
```
3. Öffne im Browser:
http://localhost:8000

---


# 🧩 Google Tag Manager (GTM) in die Testseite einbauen

Diese Anleitung zeigt Schritt für Schritt, wie du den GTM 
in die bereitgestellte HTML-Testseite integrierst und nutzt.

---

## 1. GTM-Container erstellen

1. Öffne den https://tagmanager.google.com/ 
2. Klicke auf **„Konto erstellen“**
3. Gib folgende Daten ein:
   - **Kontoname** (z. B. „Tracking Kurs“)
   - **Land**
4. Container konfigurieren:
   - Name: z. B. „Testseite“
   - Plattform: **Web**
5. Klicke auf **Erstellen** und akzeptiere die Bedingungen

👉 Danach erhältst du eine **Container-ID** (z. B. `GTM-XXXXXXX`) und zwei Code-Snippets.

---

## 2. GTM in die HTML-Seite einbauen

### 🔹 In den `<head>` einfügen:

```html
<!-- Google Tag Manager -->
<script>
(function(w,d,s,l,i){w[l]=w[l]||[];w[l].push({'gtm.start':
new Date().getTime(),event:'gtm.js'});var f=d.getElementsByTagName(s)[0],
j=d.createElement(s),dl=l!='dataLayer'?'&l='+l:'';j.async=true;j.src=
'https://www.googletagmanager.com/gtm.js?id=GTM-XXXXXXX'+dl;f.parentNode.insertBefore(j,f);
})(window,document,'script','dataLayer','GTM-XXXXXXX');
</script>
<!-- End Google Tag Manager -->
```
direkt danach in `<body>` einfügen:
```<!-- Google Tag Manager (noscript) -->
<noscript>
<iframe src="https://www.googletagmanager.com/ns.html?id=GTM-XXXXXXX"
height="0" width="0" style="display:none;visibility:hidden"></iframe>
</noscript>
<!-- End Google Tag Manager -->
```



---




# 🧩 UTM-Parameter Script in die index.html einbauen
Diese Anleitung zeigt dir Schritt für Schritt, wie du nach dem Google Tag Manager zusätzlich das UTM-/Referrer-Script in deine Testseite integrierst.
Das Script sorgt dafür, dass Besucherquellen wie:


Google Organic


Bing


Social Media


ChatGPT / Perplexity


Direct Traffic


Referral Webseiten


automatisch erkannt und als UTM-Parameter in der URL gespeichert werden.

✅ Voraussetzung
Deine index.html enthält bereits den Google Tag Manager.
Wenn nicht, zuerst GTM einbauen.

📍 Wo kommt das Script hin?
Das Script kommt in den <head> Bereich, idealerweise:
✅ nach dem GTM Script
✅ vor </head>

## 🧱 Beispiel Aufbau
So sieht deine index.html dann ungefähr aus:
```<html>
<head>

<!-- Google Tag Manager -->
<script>
... dein GTM Code ...
</script>

<!-- UTM Script hier einfügen -->
<script>
...
</script>

</head>

<body>

<!-- Google Tag Manager (noscript) -->
<noscript>...</noscript>

</body>
</html>
```

## 🛠 Schritt für Schritt

## 1. index.html öffnen

Öffne deine Datei in:

Visual Studio Code
Notepad++
Editor
Editor

## 2. Im <head> diesen Bereich suchen

```
</head>
```

## 3. Direkt davor folgendes Script einfügen (oder von der Repo laden und dann dort kopieren und einfügen)

```
<script>
(function () {
const params = new URLSearchParams(window.location.search);
const ref = document.referrer;

const isInitialized = sessionStorage.getItem("utm_initialized");

const SEARCH_ENGINES = {
"google.com": "google",
"bing.com": "bing",
"yahoo.com": "yahoo",
"duckduckgo.com": "duckduckgo",
"search.brave.com": "brave",
"ecosia.org": "ecosia"
};

const AI_SOURCES = {
"chat.openai.com": "chatgpt",
"chatgpt.com": "chatgpt",
"perplexity.ai": "perplexity",
"claude.ai": "claude"
};

const SOCIAL_SOURCES = {
"facebook.com": "facebook",
"instagram.com": "instagram",
"x.com": "twitter",
"linkedin.com": "linkedin"
};

function match(ref, map) {
for (const d in map) {
if (ref.includes(d)) return map[d];
}
return null;
}

let source = params.get("utm_source");
let medium = params.get("utm_medium");
let campaign = params.get("utm_campaign");

if (source) {
sessionStorage.setItem("utm_source", source);
sessionStorage.setItem("utm_medium", medium || "");
sessionStorage.setItem("utm_campaign", campaign || "");
sessionStorage.setItem("utm_initialized", "true");
}

else if (!isInitialized) {

if (ref) {
const search = match(ref, SEARCH_ENGINES);
const ai = match(ref, AI_SOURCES);
const social = match(ref, SOCIAL_SOURCES);

if (search) {
source = search;
medium = "organic";
}
else if (ai) {
source = ai;
medium = "ai_referral";
}
else if (social) {
source = social;
medium = "social";
}
else {
source = new URL(ref).hostname;
medium = "referral";
}
}
else {
source = "direct";
medium = "";
}

sessionStorage.setItem("utm_source", source);
sessionStorage.setItem("utm_medium", medium);
sessionStorage.setItem("utm_campaign", "");
sessionStorage.setItem("utm_initialized", "true");
}

const storedSource = sessionStorage.getItem("utm_source");
const storedMedium = sessionStorage.getItem("utm_medium");
const storedCampaign = sessionStorage.getItem("utm_campaign");

const urlParams = new URLSearchParams(window.location.search);

if (!urlParams.get("utm_source") && storedSource) {
urlParams.set("utm_source", storedSource);
if (storedMedium) urlParams.set("utm_medium", storedMedium);
if (storedCampaign) urlParams.set("utm_campaign", storedCampaign);

window.history.replaceState(
{},
"",
window.location.pathname + "?" + urlParams.toString()
);
}

})();
</script>
```

## 💾 4. Datei speichern

Dann speichern:

index.html


## ▶️ 5. Testseite starten

Mit Live Server oder lokalem Webserver öffnen.
