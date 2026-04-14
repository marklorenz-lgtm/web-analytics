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

### Option 1: Mit :contentReference[oaicite:2]{index=2}

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

---


# 🧩 Google Tag Manager (GTM) in die Testseite einbauen

Diese Anleitung zeigt Schritt für Schritt, wie du den  
:contentReference[oaicite:0]{index=0} (GTM)  
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
