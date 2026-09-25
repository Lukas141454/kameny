# Kameny – úkoly podle Zen a hotovo

Webová aplikace (PWA) na úkoly podle knihy *Zen a hotovo* (Leo Babauta).

| Záložka | Návyk ZTD | Co tam děláš |
|---|---|---|
| **+** (tlačítko) | Shromažďuj | Rychle zapíšeš cokoli do inboxu |
| **Inbox** | Třiď | U každé položky hned rozhodneš: do úkolů, velký kámen, hotovo hned (do 2 min), delegovat, smazat |
| **Týden** | Plánuj | Roční cíl, 4–6 velkých kamenů týdne rozložených do dnů, zpětné zrcátko |
| **Dnes** | Vykonávej | 1–3 NÚ, první z nich velký nahoře, režim soustředění na jednu věc |

## 1. Nahrání na GitHub Pages

1. Na GitHubu vytvoř nový repozitář, např. `kameny`.
2. Nahraj do něj všechny soubory z této složky (Add file → Upload files).
3. Settings → Pages → Source: *Deploy from a branch*, větev `main`, složka `/ (root)` → Save.
4. Za minutu poběží na `https://TVOJE-JMENO.github.io/kameny/`.

**Instalace do mobilu:** otevři adresu v Chrome → menu ⋮ → *Přidat na plochu / Nainstalovat aplikaci*.
**Na počítači:** v Chrome nebo Edge ikona instalace v adresním řádku.

Bez dalšího nastavení aplikace funguje, ale úkoly jsou jen v tom zařízení.

## 2. Synchronizace mobil ↔ počítač (Firebase, zdarma)

1. Jdi na https://console.firebase.google.com → *Vytvořit projekt* (Google Analytics můžeš vypnout).
2. **Přihlášení:** Build → Authentication → Get started → Sign-in method → **Google** → Povolit → Uložit.
3. Tamtéž záložka **Settings → Authorized domains** → *Add domain* → `TVOJE-JMENO.github.io`.
4. **Databáze:** Build → Firestore Database → Create database → umístění `eur3 (europe-west)` → *Start in production mode*.
5. V záložce **Rules** nahraď obsah tímto a dej *Publish*:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /users/{userId} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
    }
  }
}
```

6. ⚙ Project settings → dole *Your apps* → ikona `</>` (Web) → zadej jméno, Hosting nezaškrtávej → Register.
7. Zobrazí se `const firebaseConfig = { ... }`. Zkopíruj obsah složených závorek.
8. V `index.html` najdi řádek `const FIREBASE_CONFIG = null;` a nahraď ho:

```js
const FIREBASE_CONFIG = {
  apiKey: "...",
  authDomain: "...",
  projectId: "...",
  appId: "..."
};
```

9. Nahraj upravený `index.html` na GitHub. V appce: ikona nastavení vpravo nahoře → *Přihlásit přes Google*. Na obou zařízeních stejný účet.

> `apiKey` z Firebase není tajný, může být ve veřejném repozitáři. Data chrání pravidla z kroku 5 – každý vidí jen svoje úkoly.

Barevná tečka u ikony nastavení: zelená = synchronizováno, oranžová = odesílá se / offline, červená = chyba.

## Aktualizace

Po nahrání nové verze na GitHub se aplikace aktualizuje při dalším spuštění s internetem. Kdyby ne: Nastavení → *Načíst novou verzi*.

## Záloha

Nastavení → *Stáhnout zálohu* uloží všechny úkoly do JSON souboru. *Nahrát zálohu* je sloučí zpět.
