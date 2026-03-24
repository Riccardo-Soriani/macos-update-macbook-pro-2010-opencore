# 🍎 MacBook Pro 2010 → macOS Ventura con OpenCore Legacy Patcher

> Guida pratica per installare macOS moderno su Mac non supportati, partendo da una situazione estrema: macOS 10.8.5 senza accesso a internet né App Store.

---

## 📋 Indice

- [Panoramica](#-panoramica)
- [Requisiti](#-requisiti)
- [Il problema: situazione di partenza](#-il-problema-situazione-di-partenza)
- [Perché non basta usare un Mac moderno?](#-perché-non-basta-usare-un-mac-moderno)
- [Soluzione step by step](#-soluzione-step-by-step)
  - [Step 1 — Recuperare un browser funzionante](#step-1--recuperare-un-browser-funzionante)
  - [Step 2 — Installare macOS El Capitan 10.11](#step-2--installare-macos-el-capitan-1011)
  - [Step 3 — Installare macOS High Sierra 10.13](#step-3--installare-macos-high-sierra-1013)
  - [Step 4 — Installare OpenCore Legacy Patcher](#step-4--installare-opencore-legacy-patcher)
  - [Step 5 — Creare la USB bootabile con OpenCore](#step-5--creare-la-usb-bootabile-con-opencore)
  - [Step 6 — Installazione e Post-Install](#step-6--installazione-e-post-install)
- [Risorse ufficiali](#-risorse-ufficiali)

---

## 🔍 Panoramica

| Campo | Dettaglio |
|---|---|
| **Dispositivo** | MacBook Pro 15" Mid 2010 |
| **OS di partenza** | macOS Mountain Lion 10.8.5 |
| **OS di arrivo** | macOS Ventura |
| **Data** | Marzo 2026 |
| **Strumento principale** | [OpenCore Legacy Patcher](https://github.com/dortania/OpenCore-Legacy-Patcher) |

---

## 🛠 Requisiti

- MacBook Pro Mid 2010 (o altro modello non supportato)
- Una chiavetta USB da almeno **16 GB**
- Connessione a internet (anche lenta)
- Un secondo Mac per trasferire Firefox (solo per lo Step 1)

---

## ⚠️ Il problema: situazione di partenza

Il MacBook Pro di partenza si trovava in una condizione particolarmente critica:

- **Sistema operativo**: macOS Mountain Lion 10.8.5 — obsoleto da anni
- **Browser**: impossibile navigare, le pagine non caricavano
- **App Store**: non funzionante, nessun aggiornamento disponibile
- **Software moderni**: qualsiasi `.dmg` trasferito da un Mac moderno (es. MacBook Air M1) risultava illeggibile con un errore del tipo *"Immagine disco danneggiata o illeggibile"*
- **OpenCore**: richiedeva come prerequisito minimo **macOS 10.13 High Sierra**

In sostanza: il Mac era connesso alla rete ma **completamente isolato** dal punto di vista software.

---

##  Perché non basta usare un Mac moderno?

Potrebbe sembrare logico preparare tutto su un Mac più recente e trasferire il risultato sul MacBook Pro 2010. In realtà questo approccio non funziona per due motivi distinti:

**1. Incompatibilità dei file DMG**

I pacchetti `.dmg` creati o scaricati su macOS moderno (es. un M1) utilizzano strutture filesystem non riconoscibili da macOS 10.8.5. Il sistema li rifiuta come "danneggiati" anche se sono perfettamente integri. Non c'è modo di aprirli.

**2. Mancanza della partizione EFI al momento della creazione della USB**

Quando si crea una USB bootabile con OpenCore su un Mac moderno, il processo non scrive correttamente la parte EFI necessaria per avviare il MacBook Pro 2010. Il Mac riceve l'immagine del sistema operativo, ma non trova il loader OpenCore all'avvio.

Per questi motivi, **OpenCore deve essere installato e la USB deve essere creata direttamente sul Mac di destinazione**, il che richiede di portare prima il sistema a una versione compatibile.

---

## <> Soluzione step by step

### Step 1 — Recuperare un browser funzionante

Il primo ostacolo è che il Mac non riesce a navigare con Safari né con altri browser moderni. La soluzione è installare una versione molto vecchia di Firefox, compatibile con macOS 10.8.5.

**Come fare:**

1. Su un secondo Mac, scarica **Firefox versione 48** dall'archivio ufficiale Mozilla:
    [https://ftp.mozilla.org/pub/firefox/releases/48.0/mac/](https://ftp.mozilla.org/pub/firefox/releases/48.0/mac/)
   Entra nella cartella della tua lingua (`it/` o `en-US/`) e scarica il `.dmg`

2. Copia il file sul MacBook Pro 2010 tramite chiavetta USB formattata **FAT32**
   >  Usa FAT32, non APFS né Mac OS Extended: è l'unico formato leggibile da entrambi i sistemi indipendentemente dalla versione

3. Apri il `.dmg` dal MacBook Pro e installa Firefox normalmente

>  Firefox 48 è abbastanza datato da essere compatibile con macOS 10.8.5, ma abbastanza recente da riuscire a caricare le pagine web necessarie per i passi successivi.

---

### Step 2 — Installare macOS El Capitan 10.11

Con Firefox funzionante, il Mac può finalmente accedere a internet.

**Come fare:**

1. Apri Firefox sul MacBook Pro e vai alla pagina Apple per il download dei sistemi precedenti:
    [https://support.apple.com/it-it/102662](https://support.apple.com/it-it/102662)

2. Individua **OS X El Capitan 10.11** e clicca il link di download. Il file si scarica direttamente nel browser come `.dmg`.

3. Il download richiederà circa **2–3 ore** (il file è circa 6 GB)

4. Terminato il download, apri il `.dmg` e avvia `Installa OS X El Capitan`. Segui le istruzioni a schermo — il Mac si riavvierà più volte, è normale.

>  **Perché El Capitan e non direttamente High Sierra?**
> Saltare troppi aggiornamenti in un unico passaggio non è supportato su questi hardware. El Capitan è il primo step sicuro da 10.8.5: sblocca App Store, browser moderni e la compatibilità software necessaria per il passo successivo.

---

### Step 3 — Installare macOS High Sierra 10.13

Con El Capitan installato, l'App Store funziona normalmente. Questo rende il download più veloce e affidabile rispetto al browser.

**Come fare:**

1. Apri l'**App Store** e cerca **macOS High Sierra**

2. Avvia il download gratuito (in alternativa usa sempre la pagina Apple: [https://support.apple.com/it-it/102662](https://support.apple.com/it-it/102662))

3. Al termine del download, l'installer si apre automaticamente. Segui le istruzioni — il Mac si riavvierà su **macOS High Sierra 10.13**

>  Hai ora soddisfatto il requisito minimo di OpenCore Legacy Patcher. Sei pronto per il passaggio principale.

---

### Step 4 — Installare OpenCore Legacy Patcher

**OpenCore Legacy Patcher** è un progetto open source che permette di installare versioni moderne di macOS su Mac non più supportati ufficialmente da Apple.

**Come fare:**

1. Vai alla pagina delle release su GitHub:
    [https://github.com/dortania/OpenCore-Legacy-Patcher/releases](https://github.com/dortania/OpenCore-Legacy-Patcher/releases)

2. Scarica l'ultima versione stabile (file `.dmg`)

3. Apri il `.dmg` e avvia **OpenCore-Patcher.app**

---

### Step 5 — Creare la USB bootabile con OpenCore

Questa è la fase centrale: la chiavetta USB da cui il Mac avvierà l'installazione del nuovo macOS.

Segui la guida ufficiale:
 [https://dortania.github.io/OpenCore-Legacy-Patcher/INSTALLER.html](https://dortania.github.io/OpenCore-Legacy-Patcher/INSTALLER.html)

**In sintesi:**

1. Inserisci la chiavetta USB (min. 16 GB) nel MacBook Pro
2. Apri OpenCore Legacy Patcher → **"Create macOS Installer"**
3. Scegli la versione di macOS da installare (es. Ventura) — OpenCore la scaricherà automaticamente
4. Seleziona la chiavetta come destinazione
5. Attendi il completamento

>  La chiavetta **deve essere creata sul MacBook Pro stesso**, non su un altro Mac. Vedi la sezione [Perché non basta usare un Mac moderno?](#-perché-non-basta-usare-un-mac-moderno) per i dettagli.

---

### Step 6 — Installazione e Post-Install

Segui le guide ufficiali di OpenCore per i passi finali:

| Fase | Link |
|---|---|
| Avvio e installazione | [dortania.github.io/.../BOOT.html](https://dortania.github.io/OpenCore-Legacy-Patcher/BOOT.html) |
| Post-installazione | [dortania.github.io/.../POST-INSTALL.html](https://dortania.github.io/OpenCore-Legacy-Patcher/POST-INSTALL.html) |

**In sintesi:**

1. Riavvia il Mac tenendo premuto **Option (⌥)** per aprire il menu di avvio
2. Seleziona la chiavetta USB
3. Segui il processo di installazione guidato da OpenCore
4. Al primo avvio del nuovo sistema, apri di nuovo OpenCore Legacy Patcher e applica le **Post Install Root Patches** — questo ripristina i driver hardware: grafica, Wi-Fi, Bluetooth e altro

---

## 📍 Risorse ufficiali

| Risorsa | Link |
|---|---|
| OpenCore Legacy Patcher — Sito ufficiale | [dortania.github.io/OpenCore-Legacy-Patcher](https://dortania.github.io/OpenCore-Legacy-Patcher/) |
| OpenCore Legacy Patcher — GitHub | [github.com/dortania/OpenCore-Legacy-Patcher](https://github.com/dortania/OpenCore-Legacy-Patcher) |
| Apple — Download macOS versioni precedenti | [support.apple.com/it-it/102662](https://support.apple.com/it-it/102662) |
| Archivio versioni Firefox | [ftp.mozilla.org/pub/firefox/releases/](https://ftp.mozilla.org/pub/firefox/releases/) |
| Guida creazione USB | [dortania.github.io/.../INSTALLER.html](https://dortania.github.io/OpenCore-Legacy-Patcher/INSTALLER.html) |
| Guida avvio | [dortania.github.io/.../BOOT.html](https://dortania.github.io/OpenCore-Legacy-Patcher/BOOT.html) |
| Guida post-install | [dortania.github.io/.../POST-INSTALL.html](https://dortania.github.io/OpenCore-Legacy-Patcher/POST-INSTALL.html) |

---

*Guida scritta nel marzo 2026 — Testata su MacBook Pro 15" Mid 2010*
