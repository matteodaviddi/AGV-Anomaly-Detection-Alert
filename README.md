# 🤖 AI Anomaly Detection & Alert — AGV Industriali
### Make + Google Sheets + OpenAI + Gmail

Sistema di monitoraggio AI per AGV (Automated Guided Vehicles) che analizza automaticamente i dati operativi dei veicoli, rileva anomalie e situazioni critiche, e invia alert via email con report HTML professionale — progettato come demo per **E80 Group**.

---

## 🚨 Il problema che risolve

Gli AGV operano 24/7 nei plant industriali. Un guasto imprevisto può fermare l'intera linea produttiva. Monitorare manualmente decine di veicoli è impossibile. Questo sistema rileva automaticamente anomalie prima che diventino guasti critici.

---

## 🚀 Come funziona

1. **Ogni ora** lo scenario si avvia automaticamente
2. **Legge** i dati operativi di tutti gli AGV da Google Sheets
3. **Aggrega** tutte le misurazioni in un testo strutturato
4. **Analizza** con OpenAI GPT-4o rispetto alle soglie operative
5. **Invia** un alert HTML professionale con dettaglio anomalie per ogni AGV

---

## 🛠️ Tech Stack

| Tool | Purpose |
|------|---------|
| [Make](https://make.com) | Workflow automation |
| Google Sheets | Dati operativi AGV + soglie |
| OpenAI API (GPT-4o) | Analisi anomalie AI |
| Gmail | Alert email HTML |

---

## 📐 Workflow Architecture

```
Schedule (Every Hour)
        ↓
Google Sheets — Search Rows (legge metriche AGV)
        ↓
Tools — Text Aggregator (aggrega tutte le righe)
        ↓
OpenAI — Generate Completion (analizza anomalie)
        ↓
Gmail — Send HTML Alert
```

---

## 📊 Metriche Monitorate

| Metrica | Range Normale | Livello CRITICO |
|---------|--------------|-----------------|
| Velocità | 1.4 – 2.0 m/s | < 0.5 m/s |
| Temperatura Batteria | 25 – 45°C | > 65°C |
| Errori/ora | 0 – 1 | > 5 |
| Cicli Completati | 10 – 20/ora | < 5 |
| Carico | 500 – 1000 kg | > 1100 kg |

---

## 📋 Google Sheets Structure

### Tab: `Metriche` (dati operativi)

| Timestamp | AGV_ID | Velocita | Temp_Batteria | Carico | Errori | Cicli | Stato |
|-----------|--------|----------|---------------|--------|--------|-------|-------|
| 2024-04-01 09:00 | AGV-02 | 0.4 | 58 | 920 | 3 | 8 | ANOMALIA |
| 2024-04-01 10:00 | AGV-02 | 0.2 | 71 | 920 | 7 | 4 | CRITICO |

### Tab: `Soglie` (valori di riferimento)

| Metrica | Min | Max | Livello_Critico |
|---------|-----|-----|-----------------|
| Velocita | 1.4 | 2.0 | < 0.5 |
| Temp_Batteria | 25 | 45 | > 65 |

---

## 📧 Report Email

Il report HTML include:
- **Header** rosso (CRITICO) / arancione (ANOMALIA) / verde (OK)
- **KPI cards**: AGV monitorati, anomalie, critici
- **Dettaglio per AGV**: problema, causa, azione immediata
- **Box azioni**: 3 interventi prioritari
- **Footer** automatico con timestamp

---

## ⚙️ Setup Guide

### Prerequisites
- [Make account](https://make.com)
- Google account con Google Sheets
- [OpenAI API key](https://platform.openai.com)
- Gmail account

### Steps

1. **Crea il Google Sheet** con la struttura sopra
2. **Importa il blueprint** su Make (⋮ → Import Blueprint → `blueprint.json`)
3. **Connetti gli account**: Google Sheets, OpenAI, Gmail
4. **Aggiorna il modulo Google Sheets** con il tuo file
5. **Run once** per testare → controlla la tua email
6. **Attiva** il toggle per esecuzione automatica ogni ora

---

## 🔑 Key Design Decisions

**Perché il template HTML è in Gmail e non in OpenAI?**
OpenAI varia leggermente l'output HTML ad ogni chiamata anche con lo stesso prompt. Mettendo il template fisso in Gmail e usando OpenAI solo per il testo delle anomalie, la grafica è garantita sempre identica.

**Perché colonne senza caratteri speciali?**
Make non legge correttamente colonne con nomi come `Velocità (m/s)` o `Temperatura (°C)`. Usando nomi semplici come `Velocita` e `Temp_Batteria` si evitano errori di parsing.

**Perché Text Aggregator?**
Search Rows restituisce un bundle per ogni riga (12 AGV = 12 bundle). Il Text Aggregator li collassa in un unico testo che OpenAI riceve in un singolo messaggio, evitando 12 email separate.

---

## ⚠️ Common Issues

| Problema | Causa | Soluzione |
|----------|-------|-----------|
| Valori vuoti nell'email | Variabili non collegate | Ritrascina variabili dal pannello nel Text Aggregator |
| "Dati non disponibili" | `{{2.text}}` vuoto | Riseleziona variabile text dal pannello OpenAI |
| Troppe email | Text Aggregator mal configurato | Verifica Source Module = Search Rows |
| `Unable to parse range` | Nome foglio scritto a mano | Seleziona sempre dal dropdown |

---

## 🏭 Adattamento per E80

Per collegare il sistema ai dati reali degli AGV E80:

1. Sostituire Google Sheets con il database operativo degli AGV (via API o export automatico)
2. Aggiornare le soglie nel prompt OpenAI con i parametri reali di ogni modello AGV
3. Aggiungere invio alert su Slack/Teams oltre che email
4. Integrare con il sistema di ticketing manutenzione per apertura automatica ordini di intervento

---

## 📄 License

MIT — feel free to use, modify and share.

---

## 👤 Author

**Matteo Daviddi**
AI Automation Developer
[LinkedIn](www.linkedin.com/in/matteodaviddi) · [GitHub](github.com/matteodaviddi)
