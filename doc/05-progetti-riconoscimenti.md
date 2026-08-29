# 05 — Progetti e Riconoscimenti

Descrizioni riutilizzabili del lavoro e dei premi. Il lavoro professionale viene
prima: per un brand maturo conta più degli hackathon. I tre progetti Zucchetti
sono scritti come casi studio, con gli screenshot disponibili in
`immagini/`; gli hackathon sono verificati.

Struttura ricorrente di ogni progetto: **Problema → Ruolo → Approccio →
Risultato**, più la sfida progettuale quando c'è.

> **Riservatezza.** I progetti aziendali possono avere vincoli di NDA. Sono
> raccontati al livello consentito (problema, ruolo, tipo di risultato), senza
> dati o dettagli tecnici riservati. Tutti gli screenshot usano dati fittizi o
> di prova. In dubbio, verificare con Zucchetti.

---

## 1. Lavoro in Zucchetti

Da ottobre 2023:
- **Carbon Footprint Mobility** ed **ESG Datastore** — due prodotti costruiti da
  zero, seguiti dalle prime fasi di analisi e progettazione fino all'attuale
  miglioramento continuo.
- **ZTravel Smart** — miglioramento di funzionalità su un prodotto esistente.

Due dei tre progetti sono in ambito ESG/sostenibilità: confermano con i fatti la
verticalità ESG dichiarata nel profilo (`01`, `04`) e il pilastro "complessità
resa chiara". È esperienza dimostrabile, non più solo un'affermazione. Il terzo,
ZTravel Smart, tocca lo stesso mondo dal lato delle note spese — il filo ESG
attraversa tutto.

### 1.1 Carbon Footprint Mobility

*Contesto: Zucchetti, ambito ESG/sostenibilità. Prodotto costruito da zero,
dalle prime fasi di analisi e progettazione; oggi in miglioramento continuo.*

**Problema.** Le normative europee sulla rendicontazione delle emissioni si sono
fatte via via più stringenti per le medie e grandi aziende, mentre la
trasparenza ambientale è diventata un fattore competitivo. Le aziende devono
misurare le emissioni del proprio business — e chi lo faceva, lo faceva a mano,
in Excel. Il team, specializzato in mobilità, ha scelto di partire da un
verticale preciso: le emissioni legate agli spostamenti.

**Ruolo.** Sono entrato in Zucchetti in stage, su questo progetto, in un team che
un designer non l'aveva mai avuto: si passava dall'idea direttamente allo
sviluppo. Ho introdotto una fase di progettazione e validazione *prima* del
codice, in Figma. Mi sono occupato dell'analisi di necessità e requisiti, ho
progettato e prototipato il prodotto, e sono stato il punto di riferimento verso
il team di sviluppo. Alla fine dello stage sono stato assunto.

**Approccio.** È un prodotto data-driven: la priorità è rendere i dati leggibili
a chi deve decidere. Ogni funzionalità segue questo principio — la dashboard dei
risultati con viste personalizzate per scendere nel dettaglio di ciò che emette,
la pagina di Controllo dati per verificare stato e caricamento dei dataset. La
funzione **Simulazione** — l'unica che ho anche sviluppato, in React — lascia
all'utente modificare la propria flotta, gli spostamenti casa-lavoro e i viaggi,
e vedere in tempo reale come variano le emissioni.

**Risultato.** Oggi il prodotto è operativo: raccoglie i dati aziendali e
restituisce un calcolo preciso delle emissioni, esportabile nel formato che
serve. Un inventario che a mano richiedeva settimane di lavoro su più fogli Excel
oggi si chiude in poche ore — e con la Simulazione l'utente vede l'impatto di una
decisione (elettrificare la flotta, aumentare lo smart working) in tempo reale,
cosa impossibile da fare a mano.

**La sfida.** Un software così specifico non ha riferimenti di UI già pronti:
molti componenti non esistevano e ho dovuto progettarli da zero, ad hoc per lo
scopo.

*Tag: ESG · Costruito da zero · Figma · React (Simulazione)*

**Immagini** (`immagini/`):
- `cfm-simulazione.png` — la funzione Simulazione: scenari before/after, impatto
  in tempo reale sulle emissioni. *(La feature sviluppata in React.)*
- `cfm-dettaglio-indagine.png` — dashboard dei risultati: scopes GHG, ripartizioni
  per categoria, viste personalizzate.
- `cfm-controllo-dati.png` — pagina Controllo dati: qualità mese per mese e stato
  dei caricamenti.

**Descrizione breve riutilizzabile:**
> Carbon Footprint Mobility — prodotto ESG costruito da zero in Zucchetti. Ho
> introdotto la progettazione in un team che ne era privo, curato analisi e UI, e
> sviluppato in React la funzione Simulazione. Un calcolo che prima richiedeva
> settimane in Excel oggi si chiude in ore.

### 1.2 ESG Datastore

*Contesto: Zucchetti, ambito ESG/sostenibilità. Prodotto costruito da zero.*

**Problema.** Il quadro europeo sulla rendicontazione di sostenibilità — la
direttiva CSRD con i suoi standard ESRS, e lo standard volontario VSME per le
PMI — obbliga un numero crescente di aziende a redigere un bilancio di
sostenibilità verificabile. Ma il problema pratico non è *calcolare*: è
*raccogliere*. Gli stessi dati vivono sparsi in piattaforme diverse — HR,
energia, mobilità — in formati che non parlano tra loro.

**Ruolo.** Qui il mio ruolo di orchestratore dell'analisi e della progettazione
si è consolidato. Il prodotto ha richiesto una figura dedicata all'analisi
normativa — di cui non mi sono occupato io — mentre io tenevo insieme requisiti,
struttura e interfaccia.

**Approccio.** ESG Datastore è tutto organizzazione di dati grezzi e KPI. Ho
progettato per la chiarezza, non per l'estetica: ogni schermata mette la gestione
del dato al centro, senza fronzoli. I KPI sono strutturati per datapoint ESRS,
così che ciò che l'utente vede corrisponda a ciò che la norma chiede.

**Risultato.** Il prodotto è diventato l'hub unico dei dati di sostenibilità del
business: raccoglie da sorgenti diverse e standardizza il caricamento con dei
template. Il passaggio più fragile — mettere insieme dati eterogenei — è
diventato ripetibile e controllabile.

**La sfida.** Due nodi. Un **layout modulare** capace di reggere KPI molto
diversi senza reinventare la pagina ogni volta. E la **configurazione
dell'ambiente**: quattro tipi di utente con ruoli distinti, e aziende trattate
come gruppi o come insiemi di filiali.

*Tag: ESG · Costruito da zero · Figma · Design AI-assistito*

**Immagini** (`immagini/`):
- `esg-datastore-risultati.png` — risultati dell'analisi: KPI organizzati per
  datapoint ESRS, grafici e tabelle di sintesi.
- `esg-datastore-sessione.png` — sessione di raccolta: framework, sorgenti dati e
  stato di avanzamento per ogni azienda.
- `esg-datastore-home.png` — dashboard di amministrazione: utenti e ruoli,
  connettori, pacchetti di KPI, sessioni.

**Descrizione breve riutilizzabile:**
> ESG Datastore — hub per la rendicontazione di sostenibilità (CSRD/ESRS, VSME),
> costruito da zero. Ho orchestrato analisi e progettazione, standardizzando la
> raccolta di dati eterogenei con template e un layout modulare per KPI diversi.

### 1.3 ZTravel Smart

*Contesto: Zucchetti, prodotto esistente. Intervento di miglioramento su
funzionalità già in produzione. Lavorare su un prodotto già avviato dimostra una
capacità complementare al costruire da zero: inserirsi in un sistema esistente,
capirlo e migliorarlo — un segnale di affidabilità per i recruiter.*

**Problema.** ZTravel Smart è un prodotto già avviato e in uso quotidiano. Qui il
compito non era costruire, ma risolvere necessità che emergevano nel tempo —
funzionalità nuove da aggiungere, esistenti da rifare — senza stravolgere ciò che
gli utenti già conoscevano.

**Ruolo.** Redesign di alcune funzionalità e progettazione di nuove, tenendo
sempre la coerenza con il resto dell'app. Vincolo doppio: migliorare senza
rivoltare il prodotto, e disegnare cose che il team potesse integrare con
facilità nello sviluppo esistente.

**Approccio.** Innestare idee nuove su ciò che già funzionava, non riscrivere.
Ogni intervento pensato per la semplicità di sviluppo e integrazione: la
soluzione migliore *che si poteva davvero mettere in produzione*, non quella più
ambiziosa sulla carta.

**Risultato.** Nel tempo ho consegnato interventi concreti su tutto il prodotto:
**landing page co-brandizzate** per le funzionalità nate con UniCredit/Mastercard
e ZPAY, nuove pagine e banner di comunicazione dentro l'app, una **pagina
dedicata alla generazione di report con l'AI**, e l'**integrazione del calcolo
delle emissioni nelle note spese** — il punto in cui questo prodotto tocca il
mondo ESG del resto del mio lavoro.

**La sfida.** Il nodo vero: portare funzionalità progettate in Figma dentro un
prodotto sviluppato "alla vecchia maniera" — senza progettazione preliminare,
senza design system. Far incontrare due modi di lavorare, non solo due schermate.

*Tag: Prodotto esistente · Redesign · Figma · Prototipi AI · UniCredit/Mastercard · ZPAY*

**Immagini** (`immagini/`):
- `zts-unicredit-mastercard.png` — landing page co-brand UniCredit/Mastercard per
  le note spese digitali. *(Pagina pubblica.)*
- `zts-zpay.png` — feature page ZPAY: note spese con OCR e riepilogo spese.
- `zts-report-ai.png` — pagina "ZTravel Smart AI": generazione di report a partire
  da richieste in linguaggio naturale.

**Descrizione breve riutilizzabile:**
> ZTravel Smart — miglioramento di un prodotto in produzione. Ho progettato nuove
> funzionalità e redesign (landing co-brand UniCredit/Mastercard e ZPAY, report
> con AI, calcolo emissioni nelle note spese), portando la progettazione Figma
> dentro un prodotto legacy senza design system.

---

## 2. SmartHack Milano 2022 🏆

| Campo | Dato |
|-------|------|
| Evento | Smart&Hack Milano 2022 (4ª edizione) — tema Smart City |
| Risultato | Team vincitore (Team GMO) |
| Punteggio | 8,75 / 10 |
| Team | Giacomo Ferretti, Mattia Lucarelli, Oliviero Petrucci |
| Premio | Stage in Zucchetti |

È l'origin story del brand: il premio (stage in Zucchetti) ha portato al ruolo
attuale (vedi `01`, sez. 5).

**Descrizione breve riutilizzabile:**
> Vincitore di Smart&Hack Milano 2022 (tema Smart City) con il Team GMO. Il premio,
> uno stage in Zucchetti, ha dato avvio al mio percorso professionale.

---

## 3. TracerTag 🏆

| Campo | Dato |
|-------|------|
| Evento | NOI Hackathon Summer Edition — 2-3 agosto 2024, Bolzano |
| Risultato | Vincitore della challenge "Gruppo FOS" |
| Team | Oliviero Petrucci, Alessandro Taufer, Giacomo Ferretti, Davide Sbetti |
| Codice | github.com/TracerTag |

Software per il riconoscimento di oggetti e il tracciamento dei bordi nelle
immagini: algoritmi di detection identificano i contorni, che l'utente può
selezionare ed esportare in SVG. Progetto tecnicamente sofisticato (computer
vision) con output orientato al design (export SVG): l'intersezione codice-design
del posizionamento.

**Descrizione breve riutilizzabile:**
> TracerTag — vincitore al NOI Hackathon 2024 (challenge Gruppo FOS). Software di
> object recognition ed edge tracking che esporta i contorni selezionati in SVG.

---

## 4. Bug Squash 🏆

| Campo | Dato |
|-------|------|
| Evento | NOI Hackathon Summer Edition — 5-6 agosto 2022, Bolzano |
| Risultato | Vincitore della challenge "Catch Solve" |
| Team | Marco Keppel, Alessandro Taufer, Oliviero Petrucci, Giacomo Ferretti |
| Codice | github.com/MarcoKeppel/BugSquash |

Strumento per automatizzare il testing di un sito: l'utente esegue i passi una
volta, il sistema li memorizza e li riproduce, fornendo insight sulla pagina.
Mostra attenzione all'usabilità anche in uno strumento tecnico.

**Descrizione breve riutilizzabile:**
> Bug Squash — vincitore al NOI Hackathon 2022 (challenge Catch Solve). Tool che
> registra le azioni dell'utente su un sito e le riproduce per automatizzare il testing.

---

## 5. Il pattern complessivo

Due livelli che insieme raccontano un profilo completo:

- **Il lavoro vero (Zucchetti):** costruzione di prodotti dall'analisi al
  miglioramento continuo, con verticalità ESG concreta. La prova di affidabilità e
  impatto.
- **Gli hackathon:** tre vittorie (2022 ×2, 2024) con un team ricorrente (Giacomo
  Ferretti in tutti e tre, Alessandro Taufer in due): capacità di collaborare e
  consegnare sotto pressione.

Filo comune: prendere un problema tecnico e renderlo usabile.

**Frase sintetica riutilizzabile:**
> Costruisco prodotti digitali dall'analisi al rilascio — oggi in ambito ESG in
> Zucchetti — e ho vinto tre hackathon su temi dal testing automatizzato alla
> computer vision. Il filo comune: trasformare un problema tecnico in qualcosa che
> una persona può davvero usare.

---

## 6. Da completare

- **Metriche aggiuntive** per i progetti Zucchetti, dove disponibili e non
  riservate (numero di sorgenti/KPI, tempi, adozione). Anche una sola metrica
  reale rafforza un caso. Metrica attuale già inserita: CFM, "settimane → ore".
- **Link** a demo, pagine pubbliche o video degli hackathon (i due repo GitHub
  sono già indicati).
- **Repository personali** o contributi open source, se presenti.
