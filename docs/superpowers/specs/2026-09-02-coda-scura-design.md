# La coda scura — le sezioni dopo l'allagamento

Data: 2026-09-02. Stato: **da approvare**. Nessuna riga di `index.html` toccata.

Riguarda la sezione `.after` e la sua chiusura. **Sostituisce per intero** la
versione precedente di questo file, che descriveva il gomito, il cuneo che si
apriva dentro `.after` e un fondo `#554135`: tre cose che non esistono più.

---

## 1. Dove siamo davvero

Quel che è già in `main` e **non si tocca**:

- **L'allagamento** (`2836c9d`). La barra di «Software Designer» non gira più:
  resta orizzontale, si allunga oltre i due bordi, tiene, poi ingrossa da filo a
  pillola a schermo intero. Costanti `TL_OUT`, `TL_FLOOD`, `TL_SPREAD`,
  `TL_FADE`, `TL_HOLD`. **Approvata dall'utente.** Non va rimessa in
  discussione né «migliorata» di iniziativa.
- `.after-rail` **è stato cancellato**, non nascosto. Il fondo scuro della coda
  è `.after` stessa, `background:var(--ink)`, scura da subito: quando lo sticky
  della linea del tempo si sgancia, la forma cresciuta a schermo pieno esce
  dall'alto e questa carta entra dal basso, stessa tinta, giuntura invisibile.
- `--spine` è **rimasta solo una misura di impaginazione**: `30vw` da CSS,
  `24px` da `@media`. Nessun JS la scrive più.
- Le cinque fotografie sono convertite in `img/ph-*.webp` (`27e6145`) e **non
  sono ancora usate in pagina**.

Quel che resta da fare è il contenuto della coda: oggi sono tre movimenti con la
stessa griglia, lo stesso ingresso e lo stesso ritmo. Si legge come un documento
impaginato bene, non come un racconto.

---

## 2. La tesi: come guardo

**Scelta dell'utente.** La coda arriva dopo i progetti e dopo la linea del
tempo. Quelle due sezioni hanno già detto *cosa ha fatto*. La coda non lo ripete:
dice **come guarda**.

Il che ribalta il peso di quello che c'è oggi. `doc/bio.md` lo dice già, e il
sito finora non l'ha ascoltato:

> Si citano solo quando agganciati a una competenza professionale: un hobby che
> spiega *come si pensa* è segnale, uno fine a sé stesso è rumore.

Fotografia e pallacanestro smettono di essere due righe in fondo all'ultimo
movimento e diventano **i due movimenti**. Le competenze non spariscono: scendono
di volume, non di significato.

E la frase che il sito non ha mai detto, dalla bio lunga:

> Prima del software, la fotografia è stata la mia professione.

**Calibrazione che resta valida dalla versione precedente:** la fotografia è un
dettaglio forte, non il titolo. Il sito non deve sembrare il portfolio di un
fotografo.

---

## 3. La struttura

Restano **tre `.mv`**, con la stessa classe `.on`, lo stesso `mvs.forEach` nel
ciclo, la stessa costruzione `[...document.querySelectorAll('.mv')]`
(`index.html:2036`). Cambia solo cosa contengono.

| | oggi | qui |
|---|---|---|
| `mv1` | posizionamento, due paragrafi | **il campo** — quattro foto in parallasse, tesi ferma al centro |
| `mv2` | competenze, cinque `.sk` | **il basket** — una foto sola, ferma |
| `mv3` | lo sguardo, due `.lens` | **cosa so fare** — posizionamento + competenze |
| `.after-end` | stellina + mail | invariata |

Nessuna struttura nuova nel JS. `.lens` sparisce del tutto: i due sguardi non
sono più due paragrafi affiancati, sono le due sezioni.

---

## 4. `mv1` — il campo

Un blocco alto **`140vh`** che apre la coda. Dentro: quattro foto in posizione
assoluta e un blocco di testo `sticky` che si inchioda a metà schermo mentre le
foto gli passano intorno. Lo sticky è nativo — nessuna riga di JS per tenerlo
fermo.

```
   ┌────────────────────────────────────────────────┐
   │ ▓▓▓▓▓                                          │
   │ ▓ b/n▓                        ┌──────────────┐ │
   │ ▓▓▓▓▓                         │    vetri    ▓▓│ │
   │                               └──────────────▓▓│
   │              Prima del software                │
   │              la fotografia era                 │ ← sticky
   │              il mestiere.                      │
   │      ┌─────┐                                   │
   │      │lamp.│  Ha educato l'occhio:             │
   │      └─────┘  equilibrio, luce, dettaglio.     │
   │                                 ┌─────────┐    │
   │                                 │ mensole │    │
   │                                 └─────────┘    │
   └────────────────────────────────────────────────┘
```

### Un numero solo per foto

Ogni foto ha **`--s`**, unitario, quanto è grande sullo schermo. Da `--s` si
ricavano **due** cose:

```
width:      calc(var(--s) * 30vw)
translateY: calc((1 - var(--p)) * var(--s) * 26vh)
```

Più è grande più è vicina, più è vicina più si muove: è la parallasse, e non è
una seconda tabella da tenere allineata alla prima. **La profondità non si tara,
si legge dalla taglia.**

| foto | `--s` | larghezza | corsa | dimensioni reali |
|---|---|---|---|---|
| `ph-architettura-vetri` | `1` | 30vw | 26vh | 1600×1280 |
| `ph-architettura-bn` | `.72` | 21.6vw | 18.7vh | 1280×1600 |
| `ph-mensole` | `.62` | 18.6vw | 16.1vh | 1216×1600 |
| `ph-lampione` | `.5` | 15vw | 13vh | 1280×1600 |

### Il movimento

Il ciclo scrive **una sola** custom property sul campo: `--p`, quanto il campo ha
attraversato lo schermo, da 0 a 1. Una scrittura di stile per frame per tutta la
sezione, non quattro.

```
grezzo:  (innerHeight - (fieldTop - over)) / (innerHeight + fieldH)
--p:     1 - (1 - grezzo)³
```

`fieldTop` è già misurato: è `m.top` di `mv1`, che `measure()` calcola per tutti
i movimenti. `fieldH` è `offsetHeight` dello stesso elemento, misurato lì
accanto. **L'attraversamento si ricava dalle misure vere**: se cambi `140vh` in
CSS non devi toccare il JS.

**Perché la cubica.** L'utente ha chiesto che le foto salgano *velocemente*. Con
`--p` lineare la salita durerebbe tutto l'attraversamento e si leggerebbe come
un lento galleggiamento. La cubica carica il movimento all'inizio — la maggior
parte della corsa succede nel primo terzo — e lascia una deriva lenta per il
resto, così il campo non si congela mentre leggi il testo appeso.

**L'esponente 3 è l'unico numero scelto a occhio di tutto il pezzo**, e va
commentato come tale. Da tarare con l'utente a giri, come è stato fatto con
l'allagamento.

### Il segnale è `over`, non `scrollY`

Trappola trovata leggendo, **non documentata prima**. `.after` **non** sta dentro
`.projects`: i progetti su desktop sono `position:fixed` e li muove solo
`translateY(-over)` (`index.html:2817`), la coda invece è nel flusso e la scorre
il browser. Quindi nella coda ci sono due orologi possibili: lo scroll nativo
(`scrollTarget`) e l'inseguitore a molla (`scrollCur`, da cui `over`).

Finora non se n'era accorto nessuno perché `.on` è un interruttore, e un
interruttore in ritardo di mezzo frame non si vede. Per una parallasse si vede.

**Si usa `over`**, cioè l'inseguitore. Ragione: è la disciplina dichiarata del
file — *un unico segnale* — e la conseguenza è che le foto ereditano la stessa
inerzia della stellina, del nastro e dei progetti invece di combatterla. Il
prezzo è che durante uno scatto veloce le foto restano indietro rispetto alla
loro cornice di `lag × --s`; con corse così corte sono pochi pixel, e leggono
come morbidezza.

*Se un giorno quel ritardo desse fastidio, l'alternativa è `scrollY - max` al
posto di `over` — una riga. Ma allora il campo si muove con un orologio diverso
dal resto del sito, e va scritto nel commento.*

### Il ritaglio e la composizione

| foto | posizione |
|---|---|
| `ph-architettura-bn` | `top:6vh; left:-4vw` — **esce dal bordo sinistro** |
| `ph-architettura-vetri` | `top:14vh; right:-5vw` — **esce dal bordo destro** |
| `ph-lampione` | `top:62vh; left:8vw` |
| `ph-mensole` | `top:74vh; right:12vw` |

Due foto escono dal bordo: il campo non ha margini, ha un fuori. **Nessuna passa
sotto il testo** — la colonna centrale (`max-width:30ch`, `sticky top:38vh`)
resta libera, così non serve né velo né sfocatura per leggere. È un vincolo di
composizione, non un'ottimizzazione: un velo su una fotografia è un modo di
ammettere che la si è messa nel posto sbagliato.

---

## 5. `mv2` — il basket

Una foto sola, **ferma**. Dopo un campo che si muove, una foto che non si muove.

È il contrasto a fare l'argomento: la fotografia è come guarda il mondo, la
pallacanestro è come sta dentro una squadra — una cosa si osserva, l'altra si
abita. Se anche questa galleggiasse, le due sezioni direbbero la stessa cosa con
due contenuti diversi.

Tiene la griglia `.mv` che c'è già (`var(--spine) 1fr`): etichetta nella colonna
stretta, corpo nella larga. Nel corpo: la foto a piena larghezza della colonna,
il testo sotto. Entrata con `.on`, stessa scaletta di tutto il resto. **Nessuna
riga di JS.**

`ph-basket.webp` è 1322×1190 — l'unica quasi quadrata, e l'unica delle cinque in
cui compare l'utente.

---

## 6. `mv3` — cosa so fare

Il paragrafo di posizionamento (Zucchetti, ESG, la laurea) **apre** il blocco.
Non sparisce: trova il posto giusto. È una nota tecnica, non l'apertura del
racconto — ed è esattamente il ruolo che gli spetta una volta che la tesi della
coda è lo sguardo.

Sotto, le cinque aree, una riga ciascuna. Markup `.sk` e `.sk-tags` invariati:
funzionano e sono già lì.

### Muore `--k`

Oggi `--k` moltiplica `font-size` di `.sk h3` per dire il peso con la taglia
(`index.html:1191`, `1364`, e i cinque `style` inline alle righe 1899-1903).

Con la coda ormai portata dalle immagini, cinque titoli di taglie diverse
**competono col campo** invece di aiutarlo: sono cinque gesti tipografici forti
subito dopo cinque gesti fotografici forti.

Il peso lo dicono **l'ordine e un accento**: Design primo e in `#BFCFFF`, le
altre quattro in `--tlbg`. Stessa informazione, un terzo dello spazio, una
variabile in meno da mantenere in tre posti.

L'ordine resta quello del documento, e ci sono **tutte e cinque** le aree, per
intero: il taglio non è mai stato il modo di dire il peso.

---

## 7. La chiusura

`.after-end` — stellina e «contattami» — **resta com'è**. Funziona, è già sulla
palette scura, e nella nuova struttura arriva dopo un blocco tecnico quieto:
esattamente il posto in cui una firma sta bene.

**Ma i tre link social sono `href="#"`** (`index.html:1508-1509`). LinkedIn,
Instagram e Github non portano da nessuna parte, e sono in una barra fissa che
sta sopra tutta la coda. Servono gli URL. **Due link veri battono tre finti**
(punto aperto §12).

---

## 8. La palette e il testo

**Nessun colore nuovo.** La palette scura della coda è già in pagina
(`index.html:1103-1111`) e resta:

| ruolo | valore |
|---|---|
| fondo | `--ink` `#2B1A0F` |
| testo | `--tlbg` `#FFFEF8` |
| accento, micro-etichette, filetti | `#BFCFFF` |

`--blue` `#174FFE` su `--ink` fa ~1.05:1: illeggibile, ed è il motivo per cui
nella sola coda l'azzurro acceso lascia il posto a `#BFCFFF`. Altrove resta.
`.mv-lead` resta a peso **400**: il 300 di DM Sans si sfilaccia sul fondo scuro.

**Le foto arrivano intere, senza filtri.** Ma dai livelli campionati ci sono due
cose vere, **da guardare a schermo e non da decidere qui**:

| foto | nero (5° perc.) | mediana | conseguenza |
|---|---|---|---|
| architettura B/N | `(34,34,34)` | `(38,38,38)` | più scura **e più fredda** del fondo: quella foto *rientra* invece di staccare |
| mensole | `(28,24,25)` | `(230,185,188)` | la **più chiara** delle quattro |

Se la B/N sparisce nel fondo prende un filo di bordo `#BFCFFF` a bassa opacità,
non una cornice.

### Il testo

Bozze. **Le corregge l'utente**, come per le altre sezioni.

Campo:

> Prima del software, la fotografia era il mestiere. Ha educato l'occhio
> all'equilibrio, alla luce, al dettaglio: la sensibilità visiva del design è
> allenata, non innata.

Basket:

> Da sempre gioco a pallacanestro. Visione d'insieme, decisioni rapide sotto
> pressione, e capire il proprio ruolo dentro una squadra — che è poi come si
> lavora su un prodotto.

Posizionamento (`mv3`): il primo paragrafo della bio Media di `doc/bio.md`,
invariato.

**Da sistemare passando di lì:** la bio Media in `doc/bio.md` ripete due volte,
identico, il paragrafo «Ho maturato esperienza specifica nel software B2B…». È
un errore del documento, non del sito.

---

## 9. I tre rami

Invariante del progetto: desktop con puntatore, touch/mobile (`mq`, `canHover`,
`@media (max-width:640px)`), `prefers-reduced-motion`. Ogni cosa nuova va pensata
per tutti e tre o si rompe in silenzio.

**Desktop con puntatore.** Campo intero, parallasse dal ciclo, testo `sticky`.

**Touch.** **Niente transform per frame.** Sul dito il contenuto deve restare
incollato, e scrivere trasformazioni a ogni frame è esattamente il tremolio che
il file ha già combattuto sui progetti (`index.html:2806-2817`, e il `@media`
a `314-316`) — lì la soluzione
è stata togliere la sezione dal flusso *solo* con `hover:hover`. Qui la
soluzione è più semplice: il campo si incolonna. Foto alternate a sinistra e a
destra, più piccole, che entrano con l'ingresso scalettato di `.on`, e il testo
prima di loro, non sticky. `--p` non viene scritto.

**`prefers-reduced-motion`.** Lì **non c'è ciclo rAF**: tutto ciò che dipende da
una scrittura per frame va consegnato già fatto dal CSS. Le foto stanno al loro
posto finale, `--p:1` di default, nessuna entrata. È lo stesso patto già scritto
a `index.html:1419-1423`.

**`--p:1` è il valore di partenza in CSS, non `0`.** Così ogni ramo che non
scrive `--p` — reduced-motion, touch, e il frame prima che il ciclo parta — vede
il campo finito invece che il campo vuoto. Un default sbagliato qui si manifesta
come «le foto non ci sono», che è il sintomo più costoso da inseguire.

**Trappola nota, ancora valida:** il guardiano `tlLive` esce dalla funzione per
**tutto** il ramo incolonnato, non solo quando la timeline è lontana. Qualunque
cosa debba girare anche su mobile va scritta **prima** di quel guardiano — è dove
sta già la riga di `.after` (`index.html:3003`).

---

## 10. Cosa muore, cosa si ricava, cosa si accoppia

**Muore:**

- `.lens` — CSS (`1197-1209`), markup (`1912-1913`) e la regola mobile (`1365`)
- `--k` — CSS (`1191`, `1364`) e i cinque `style` inline (`1899-1903`)
- i due paragrafi `.mv-lead` di `mv1`: uno si sposta in `mv3`, l'altro è la bozza
  nuova del campo
- il filetto `.mv::before` **sul solo `mv1`**: un filetto che cresce dalla guida
  verso l'etichetta non ha senso su un campo che non ha colonne
- il paragrafo duplicato nella bio Media di `doc/bio.md`

**Si ricava, non si tara.** Se stai per scrivere lo stesso numero due volte,
fermati:

- `--p` dall'attraversamento vero: `m.top` e `offsetHeight` di `mv1`, misurati in
  `measure()`. Cambiare `140vh` in CSS non tocca il JS
- larghezza **e** corsa di ogni foto dal solo `--s`
- l'altezza riservata di ogni immagine da `aspect-ratio` con le dimensioni reali
  della tabella §4, non a occhio

**Accoppiati, si cambiano insieme:**

| se tocchi | devi toccare anche |
|---|---|
| l'altezza del campo (`140vh`) | niente: `measure()` la rilegge |
| `--s` di una foto | niente: larghezza e corsa escono da lì |
| le posizioni delle foto | il vincolo «la colonna centrale resta libera» |
| il numero di `.mv` nel markup | niente: `mvs` è una query |
| `#BFCFFF` | tutta la palette della coda, che è costruita su quel solo accento |

---

## 11. Gli asset

Cinque webp già in `img/`, convertite e committate in `27e6145`:

| file | dimensioni | peso |
|---|---|---|
| `ph-architettura-bn.webp` | 1280×1600 | 26 KB |
| `ph-architettura-vetri.webp` | 1600×1280 | 48 KB |
| `ph-lampione.webp` | 1280×1600 | 197 KB |
| `ph-mensole.webp` | 1216×1600 | 43 KB |
| `ph-basket.webp` | 1322×1190 | 76 KB |

Circa 390 KB in tutto, di cui metà il solo lampione (è a qualità 90 perché il
cielo è una sfumatura larga e a 82 bandava).

**Tutte `loading="lazy"`**: sono sotto la piega per definizione.

**Tutte con `width` e `height` dichiarati.** Non è ottimizzazione, è la
condizione perché la coda non rompa le sezioni prima: tutta la geometria di
questo sito è **misurata**, `measure()` legge altezze in pixel e ci costruisce
sopra scroll, pin e corsa della timeline. Un'immagine che arriva dopo il `load`
cambia l'altezza del documento a misure già prese, e il sintomo non sembra
affatto colpa delle foto.

**`alt` veri, non decorativi.** Le foto sono l'argomento della sezione. L'alt
descrive il fotogramma — cosa si vede — non «fotografia di Oliviero Petrucci».
Un `alt=""` qui toglierebbe a chi non vede *tutta* la tesi della sezione.

---

## 12. Punti aperti

1. **I tre URL social.** LinkedIn, Instagram, Github sono `href="#"` da sempre.
   Li dà l'utente. Due link veri battono tre finti.
2. **Il testo definitivo** delle due bozze §8 e dell'`alt` di ogni foto.
3. **Le mensole**: dalla versione precedente di questa spec risultano un lavoro
   di **Alex Pinna**. Se è una sua fotografia di un'opera altrui, l'`alt` e
   l'eventuale didascalia lo devono dire.
4. **I due nodi cromatici** del §8, da guardare sul rendering vero.
5. **L'esponente della cubica** (§4), da tarare a giri con l'utente.

---

## 13. Come si verifica

Non c'è test runner e **non va aggiunto**. Le sonde stanno in
`C:\Users\Oliviero\AppData\Local\Temp\lp-portfolio-probe\`, fuori dal repo.

**Gli script CDP della sessione precedente sono spariti col job.** Vanno
riscritti: non c'è né Playwright né Puppeteer installato (`npm i` fallisce in
questo ambiente), ma c'è il binario di Chromium in
`~/AppData/Local/ms-playwright/chromium-1228/chrome-win64/chrome.exe`, e Node 25
ha `WebSocket` e `fetch` globali — bastano ~50 righe senza dipendenze.
`--headless=new --remote-debugging-port=<porta>`, target da `/json/list`, poi
`Page.navigate`, `Runtime.evaluate`, `Page.captureScreenshot`.

1. **Sintassi** — estrai lo script inline, `node --check`, dopo ogni gruppo di
   modifiche.
2. **Struttura** — annidamento con `html.parser`, confronto classi CSS ↔ classi
   nel markup nelle due direzioni, id duplicati.
3. **Geometria** — chiamare il **codice di produzione**, non riscriverlo:
   `scrollTo(0,y); scrollCur = scrollTarget = scrollY; sync(); applyProjects();`
   e poi leggere `--p` e le transform delle quattro foto.
4. **Convergenza.** Per posizionarsi a un punto preciso **non** basta un
   `scrollTo`: la pagina ha uno scroll inseguito. Si converge — leggi `overCur`,
   scrolla della differenza, aspetta ~450 ms, ripeti. `lockHero()` cambia
   l'altezza dello scroller a metà strada e un conto fatto una volta sola salta.
5. **Contrasto** — misurato sul rendering, non ricalcolato a mano.
6. **Aspetto** — in headless lo screenshot parte dall'origine della pagina. Per
   fotografare una sezione va nascosto il resto dei figli di `body` e portata in
   cima con un margine negativo.
7. **Altezza del documento** — prima e dopo il `load`, per confermare che le
   cinque immagini non spostano niente.
8. **Almeno due proporzioni di finestra** (1920×1000 e 1280×1024): diversi conti
   di questa pagina hanno rami che scattano solo sulle finestre più alte che
   larghe.
9. **Giudizio percettivo** — solo l'utente, ricaricando. **Dichiarare sempre cosa
   è stato misurato e cosa no.**

---

## 14. Un difetto preesistente, non introdotto qui

In fondo a `index.html`:

```js
requestAnimationFrame(function loop(now){ ... })(introStart)
```

Il risultato di `requestAnimationFrame` è un numero, e viene chiamato: solleva un
`TypeError` a ogni caricamento. **Non rompe niente** — il ciclo è già registrato
ed è l'ultima istruzione del `.then()` — ma sporca la console e fa perdere tempo
a chi lo trova. È preesistente e non è mai stato segnalato come da correggere.
Passando di lì, si propone.
