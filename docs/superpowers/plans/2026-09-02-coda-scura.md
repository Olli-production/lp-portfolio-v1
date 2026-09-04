# La coda che guarda — Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

> **PIANO ESEGUITO, E IN PARTE SUPERATO IN CORSA.** Le fasi 1, 3, 4 e 5 sono
> andate come sono scritte. **La fase 2 no:** la parallasse è stata sostituita da
> una diapositiva a tre stati, sui cinque fotogrammi (905-909) che l'utente ha
> disegnato dopo. Anche l'ordine dei tre movimenti è cambiato — la nota tecnica
> apre, il campo sta in mezzo. Questo documento resta com'era, perché è il
> racconto di quello che si era deciso di fare; **quello che c'è a schermo lo
> descrive la spec**, `docs/superpowers/specs/2026-09-02-coda-scura-design.md`,
> che è stata riallineata. In caso di disaccordo fra i due, vince la spec.
>
> I riferimenti a riga di questo piano sono quelli di prima della riscrittura e
> non puntano più dove dicono.

**Goal:** Le sezioni della coda smettono di elencare e cominciano a guardare: quattro fotografie salgono in parallasse dietro una tesi ferma, il basket resta immobile accanto, le competenze scendono a nota tecnica.

**Architecture:** Un solo file, `index.html` (CSS e JS in linea, ~3600 righe). Nessuna build, nessuna dipendenza, nessun file nuovo. Restano le tre `.mv` che ci sono già, con la stessa `.on` e lo stesso `mvs.forEach`: cambia cosa contengono. L'unica aggiunta al ciclo è **una** scrittura di stile per frame — `--p` sul campo — e il CSS la distribuisce alle quattro foto con un solo numero per foto.

**Tech Stack:** HTML/CSS/JS vanilla. Node per `node --check`. Chrome headless via CDP per le sonde. Python per le misure sulle immagini.

**Spec:** `docs/superpowers/specs/2026-09-02-coda-scura-design.md` — va letta insieme a questo piano.

## Global Constraints

- **Nessun file nuovo nel repo, nessuna dipendenza, nessun test runner.** Le sonde stanno in `C:\Users\Oliviero\AppData\Local\Temp\lp-portfolio-probe\`, fuori dal repo.
- **Tre rami, sempre:** desktop con puntatore, touch/mobile (`mq`, `canHover`, `@media (max-width:640px)`), `prefers-reduced-motion`. In reduced-motion **non c'è ciclo rAF**: quel che dipende da una scrittura per frame va consegnato da CSS o da `measure()`.
- **Si ricava, non si tara.** Se stai per scrivere lo stesso numero due volte, fermati.
- **Nessun colore nuovo.** La palette della coda è già in pagina: fondo `--ink` `#2B1A0F`, testo `--tlbg` `#FFFEF8`, accento `#BFCFFF`.
- **`--p` vale `1` di default in CSS, mai `0`.** Ogni ramo che non lo scrive deve vedere il campo finito, non il campo vuoto.
- **L'allagamento è approvato e non si tocca.** `TL_OUT`, `TL_FLOOD`, `TL_SPREAD`, `TL_FADE`, `TL_HOLD` e tutto il pin della linea del tempo restano come sono.
- **Commenti in italiano, estesi**, con la motivazione e cosa NON fare. Messaggi di commit in inglese, argomentati, in prosa — non `feat:`/`fix:`.
- **Non inventare dati.** Dove un dato non esiste va una frase, mai un numero verosimile.
- **Il testo lo corregge l'utente.** Ogni bozza qui dentro è una bozza: si mostra, si aspetta l'ok, poi si committa.

---

## File Structure

| file | cosa cambia |
|---|---|
| `index.html` | markup di `.after` (1885-1924), CSS della coda (1063-1245 e i due `@media`), due blocchi nel JS (`measure()` ~2547, ciclo ~3003) |
| `doc/bio.md` | il paragrafo duplicato nella bio Media |
| `docs/superpowers/specs/2026-09-02-coda-scura-design.md` | aggiornata a fine lavoro con quel che è stato deciso a schermo |

Nessun file creato. Nessun file cancellato.

---

## Task 1: Il campo, fermo

Prima la composizione, senza movimento. Se il campo non regge da fermo, non lo salva nessuna parallasse.

**Files:**
- Modify: `index.html:1886-1892` (markup `mv1`), `1063-1112` (CSS della coda, dove aggiungere il blocco `.gaze`), `1352-1369` (`@media max-width:640px`)

**Interfaces:**
- Produces: la classe `.gaze` (il campo), `.gaze-ph` (una foto), `.gaze-say` (la tesi ferma), e la custom property `--s` — un numero per foto, da cui si ricavano **larghezza e corsa**. Le fasi 2 e 3 leggono questi nomi.

- [ ] **Step 1: Il CSS del campo**

Da aggiungere subito dopo il blocco della palette della coda (dopo `index.html:1111`, la riga `.after .mv::before{background:#BFCFFF}`):

```css
/* ---------- IL CAMPO ----------
   Quattro fotografie che salgono da sotto mentre la tesi resta ferma in mezzo.
   E' il primo movimento, e apre la coda: la prima cosa dopo l'allagamento sono
   le sue foto, non un paragrafo.

   --p E' 1 DI DEFAULT, NON 0. Ci sono tre rami che questa property non la
   scrivono mai — reduced-motion (li' non c'e' ciclo), touch (li' il contenuto
   sta incollato al dito e non si scrivono transform per frame) e il frame prima
   che il ciclo parta. Tutti e tre devono vedere il campo FINITO. Con lo zero di
   default il sintomo sarebbe "le foto non ci sono", che e' il piu' costoso da
   inseguire perche' somiglia a un errore di percorso.

   UN NUMERO SOLO PER FOTO. --s dice quanto e' grande, e da li' si ricava anche
   di quanto sale: piu' e' grande piu' e' vicina, piu' e' vicina piu' si muove.
   NON aggiungere una seconda property per la profondita' — sarebbe una seconda
   tabella da tenere allineata alla prima, ed e' esattamente la deriva che
   questo file evita da sempre. */
.gaze{
  --p:1;
  display:block;position:relative;height:140vh;
}
/* il padding-top NON si riscrive qui: .mv ce l'ha gia' (var(--pt)), e
   .after .mv:first-of-type lo porta a 40vh. box-sizing e' border-box in tutto
   il file, quindi i 140vh COMPRENDONO quel padding e offsetHeight vale
   esattamente 140vh — che e' il numero su cui la fase 2 costruisce --p. */
/* il filetto che aggancia il movimento alla guida qui non ha senso: non ci sono
   colonne, e crescerebbe dal nulla verso il nulla. */
.gaze::before{content:none}
/* l'etichetta perde l'allineamento a destra: fuori dalla griglia justify-self
   non fa niente e text-align:right la spedirebbe contro il bordo destro dello
   schermo, dove si leggerebbe come la didascalia della foto dei vetri. */
.gaze .mv-tag{justify-self:auto;text-align:left;padding:0 0 0 2.6vw}
.gaze-ph{
  position:absolute;width:calc(var(--s) * 30vw);
  transform:translateY(calc((1 - var(--p)) * var(--s) * 26vh));
}
/* height:auto con width e height dichiarati sull'<img>: il rapporto lo tiene il
   browser dalle dimensioni vere, e lo spazio e' riservato PRIMA che il file
   arrivi. Non e' ottimizzazione — measure() ha gia' preso le sue altezze in
   pixel quando le immagini finiscono di caricare. */
.gaze-ph img{display:block;width:100%;height:auto}
/* LA TESI, FERMA. sticky nativo: nessuna riga di JS per tenerla in mezzo.
   Il contenitore e' alto 140vh, quindi resta appesa per tutto l'attraversamento
   e se ne va con lui. 30ch e non piu': deve restare una colonna stretta al
   centro, perche' il vincolo di composizione e' che NESSUNA foto le passi
   sotto. Un velo o una sfocatura dietro il testo sarebbero il modo di ammettere
   che una foto e' nel posto sbagliato. */
.gaze-say{
  position:sticky;top:38vh;
  padding:0;margin:0 auto;max-width:30ch;
}
```

- [ ] **Step 2: Il markup del campo**

Sostituisce `index.html:1886-1892` per intero (l'`<article id="mv1">` di oggi).

`.gaze-say` sta **prima** delle foto: su desktop il posizionamento assoluto ignora l'ordine del DOM, ma su mobile e per chi legge con uno screen reader la tesi deve venire prima delle immagini che la dimostrano.

```html
  <article class="mv gaze" id="mv1">
    <h2 class="mv-tag">01 &mdash; come guardo</h2>
    <div class="mv-body gaze-say">
      <p class="mv-lead">Prima del software, la fotografia era il mestiere. Ha educato l&rsquo;occhio all&rsquo;equilibrio, alla luce, al dettaglio: <i>la sensibilit&agrave; visiva del design &egrave; allenata, non innata</i>.</p>
    </div>
    <!-- un numero solo per foto (--s): da li' escono larghezza e corsa. La
         posizione e' inline perche' e' composizione, non regola: sono quattro
         casi unici, e quattro selettori in fondo al foglio direbbero la stessa
         cosa piu' lontano da dove la si legge. -->
    <div class="gaze-ph" style="--s:.72;top:6vh;left:-4vw">
      <img src="img/ph-architettura-bn.webp" width="1280" height="1600" loading="lazy"
           alt="Facciata in cemento in bianco e nero: pilastri e solette tagliati dalla luce diretta, senza mezzi toni.">
    </div>
    <div class="gaze-ph" style="--s:1;top:14vh;right:-5vw">
      <img src="img/ph-architettura-vetri.webp" width="1600" height="1280" loading="lazy"
           alt="Due facciate che si toccano ad angolo: una griglia di vetri turchesi e una parete bianca a finestre strette.">
    </div>
    <div class="gaze-ph" style="--s:.5;top:62vh;left:8vw">
      <img src="img/ph-lampione.webp" width="1280" height="1600" loading="lazy"
           alt="Un lampione a sei bracci acceso contro un cielo al tramonto, fra nuvole rosa e blu.">
    </div>
    <div class="gaze-ph" style="--s:.62;top:74vh;right:12vw">
      <img src="img/ph-mensole.webp" width="1216" height="1600" loading="lazy"
           alt="Due mensole triangolari nere a parete sopra un divano rosa; sul bordo di una cammina una figurina.">
    </div>
  </article>
```

- [ ] **Step 3: Il ramo incolonnato**

Nel `@media (max-width:640px)`, subito dopo `.mv-lead{font-size:19px;max-width:none}` (`index.html:1363`):

```css
  /* IL CAMPO SU TELEFONO. Le foto si incolonnano e la tesi torna in cima, nel
     suo ordine di DOM. L'altezza fissa se ne va: qui il campo e' alto quanto il
     suo contenuto.
     PERCHE' NIENTE PARALLASSE QUI: sul dito il contenuto deve restare incollato,
     e scrivere una transform a ogni frame e' esattamente il tremolio che questo
     file ha gia' combattuto sui progetti (vedi il @media hover a 314-316 e il
     commento in applyProjects). Le foto entrano con l'ingresso scalettato che
     c'e' gia', e basta. */
  .gaze{height:auto;padding-bottom:8vh}
  .gaze-ph{
    position:static;width:74vw;margin:18px 0 0;transform:none;
  }
  .gaze-ph:nth-of-type(even){margin-left:26vw}
  .gaze-say{position:static;top:auto;max-width:none;margin:0}
```

- [ ] **Step 4: Sonda di struttura**

Controlla, con `html.parser`: che `#mv1` esista e abbia entrambe le classi `mv` e `gaze`; che contenga esattamente **quattro** `.gaze-ph`; che ognuna abbia un `<img>` con `src`, `width`, `height`, `loading="lazy"` e un `alt` **non vuoto**; che `.gaze-say` venga **prima** della prima `.gaze-ph` nell'ordine del documento.

Expected: quattro foto, quattro alt pieni, testo prima.

- [ ] **Step 5: L'altezza del documento non si muove al `load`**

Sonda: leggi `document.body.scrollHeight` prima che le immagini siano caricate e dopo l'evento `load`.

Expected: **lo stesso numero**. Se cambia, un `width`/`height` manca o è sbagliato, e ogni misura presa da `measure()` prima del `load` è da buttare. È la trappola che rompe le sezioni *precedenti*, non questa.

- [ ] **Step 6: Screenshot del campo fermo**

Tre schermate, a 1920×1000 e a 1280×1024: il campo appena entrato, il campo a metà, il campo che esce. In headless lo screenshot parte dall'origine della pagina: per fotografare la sezione vanno nascosti gli altri figli di `body` e portata in cima con un margine negativo.

Guarda due cose, che sono le sole già note dai livelli campionati:
1. **La B/N rientra nel fondo?** Il suo nero è `(34,34,34)` contro un fondo `(43,26,15)`: più scuro e più freddo. Se sparisce, un filo di bordo `#BFCFFF` a bassa opacità — **non** una cornice.
2. **Le mensole sono la foto più chiara** (mediana `(230,185,188)`). In basso a destra, a fine campo, potrebbe tirare l'occhio fuori dalla lettura.

- [ ] **Step 7: Verifica che nessuna foto passi sotto il testo**

Sonda: prendi il rettangolo di `.gaze-say` e i quattro di `.gaze-ph` con `getBoundingClientRect()`, a `--p` 0 e a `--p` 1, su entrambe le finestre.

Expected: **nessuna intersezione**, in nessuno dei quattro casi. È il vincolo di composizione della spec, ed è l'unica cosa in questa fase che si può verificare senza guardare.

- [ ] **Step 8: Fallo giudicare**

Mostra gli screenshot. Dichiara cosa è stato misurato (intersezioni, altezza del documento, struttura) e cosa no (se il campo è bello). Aspettati due o tre passate sulle posizioni: sono composizione, si tarano a occhio ed è giusto così.

- [ ] **Step 9: Commit (solo dopo il suo ok)**

```bash
git add index.html
git commit
```

Messaggio in inglese, in prosa, che dica **perché** il campo apre la coda e perché `--p` parte da 1.

---

## Task 2: La parallasse

> **SUPERATA.** Quello che segue non è stato spedito. La parallasse continua —
> `--p` cubico scritto a ogni frame, tesi `sticky`, campo alto 140vh — è arrivata
> a schermo e non ha convinto: l'utente ha disegnato cinque fotogrammi in cui la
> pagina si ferma e avanza a scatti, e quella è diventata la specifica. Al posto
> di `--p` c'è `--t`, che vale 0, 1 o 2; al posto della corsa c'è il blocco dello
> scroll. Il meccanismo sta tutto in un blocco solo di `index.html`, ed è
> descritto nel §5 della spec.
>
> Resta qui per intero perché la fase 1 ne dipendeva e perché dice *perché* la
> composizione veniva prima del movimento — che è rimasto vero.


**Files:**
- Modify: `index.html` — `measure()` intorno a `2547`, il ciclo intorno a `3003`, e la dichiarazione delle variabili vicino a `2256`

**Interfaces:**
- Consumes: `.gaze` e `--s` dalla fase 1; `mvs` (`index.html:2036`), `af0`, `over`, `clamp()`, `mq`, `reduce`.
- Produces: la scrittura di `--p` sul campo. Nessuna fase successiva ne dipende.

- [ ] **Step 1: La variabile di stato**

Accanto a `let af0 = 0;` (`index.html:2256`):

```js
let gazeH = 0;                 // altezza del campo, misurata: NON scritta a mano
let gazeP = '';                // ultimo --p scritto, per non riscrivere lo stesso
```

`gazeP` è una **stringa**, non un numero, e si confronta con la stringa che sta per essere scritta: è lo stesso patto di `tlFo` poco sopra nel ciclo. Confrontare i numeri e poi formattare due volte costa una formattazione in più per frame e non salta niente.

- [ ] **Step 2: La misura**

In `measure()`, sulla riga dopo `mvs.forEach(m => m.top = af0 + m.el.offsetTop);` (~`2547`):

```js
/* L'ATTRAVERSAMENTO DEL CAMPO SI RICAVA, non si scrive. L'altezza sta in CSS
   (140vh) e qui si LEGGE: cambiarla nel foglio non deve obbligare a toccare il
   JS, o i due numeri divergono alla prima modifica e nessuno se ne accorge
   finche' la parallasse non finisce mezzo schermo prima. */
gazeH = mvs[0].el.offsetHeight;
```

`mvs[0]` è il campo perché è il primo `.mv` del documento. Se un giorno l'ordine cambia, questa riga sceglie l'elemento sbagliato **in silenzio**: il commento deve dirlo.

- [ ] **Step 3: La scrittura per frame**

Nel ciclo, subito **dopo** `mvs.forEach(m => m.el.classList.toggle('on', ...))` (~`3003`) e **prima** del guardiano `tlLive`. La posizione non è un dettaglio: quel guardiano esce dalla funzione per tutto il ramo incolonnato.

```js
  /* IL CAMPO. Una sola scrittura di stile per frame per tutta la sezione: il
     CSS la distribuisce alle quattro foto con --s. Quattro scritture (una per
     foto) direbbero la stessa cosa costando quattro volte tanto.

     IL SEGNALE E' `over`, cioe' l'INSEGUITORE, e non lo scroll grezzo. Qui la
     scelta e' vera e va capita: .after non sta dentro .projects — i progetti
     sono position:fixed e li muove solo translateY(-over), la coda invece e' nel
     flusso e la scorre il browser — quindi in questa sezione ci sono DUE
     orologi possibili. Finora non se n'era accorto nessuno perche' .on e' un
     interruttore, e un interruttore in ritardo di mezzo frame non si vede.
     Con `over` le foto ereditano la stessa inerzia della stellina, del nastro e
     dei progetti: un unico segnale, che e' la disciplina di questo file. Il
     prezzo e' che in uno scatto veloce restano indietro dalla loro cornice di
     (ritardo x --s) — pochi pixel con corse cosi' corte, e si legge come
     morbidezza. Volendo l'altro orologio si scrive `scrollY - max` al posto di
     `over`: UNA riga, ma allora il campo si muove con un tempo diverso dal
     resto del sito e va detto qui.

     LA CUBICA. Con --p lineare la salita durerebbe tutto l'attraversamento e
     si leggerebbe come un galleggiamento lento: l'utente ha chiesto che le foto
     salgano VELOCEMENTE. La cubica carica la corsa all'inizio — la maggior
     parte succede nel primo terzo — e lascia una deriva lenta per il resto,
     cosi' il campo non si congela mentre si legge la tesi appesa.
     L'ESPONENTE 3 E' L'UNICO NUMERO SCELTO A OCCHIO DI TUTTO IL PEZZO. Tutto il
     resto qui e' misurato. Se va cambiato si cambia qui e in nessun altro posto.

     Il ramo touch e reduced-motion non entrano mai: la' --p resta al suo default
     di 1 e il campo e' gia' finito. Vedi il commento di .gaze nel foglio. */
  if (!mq.matches && !reduce){
    const gz = clamp((innerHeight - (mvs[0].top - over)) / (innerHeight + gazeH));
    const gp = (1 - (1 - gz) ** 3).toFixed(3);
    if (gp !== gazeP){ mvs[0].el.style.setProperty('--p', gp); gazeP = gp; }
  }
```

- [ ] **Step 4: `node --check`**

Estrai lo script in linea fra l'ultimo `<script>` e `</script>` in un file temporaneo fuori dal repo, poi:

```bash
node --check "%TEMP%\lp-portfolio-probe\inline.js"
```

Expected: nessun output, exit 0.

- [ ] **Step 5: Sonda — `--p` si ricava e arriva agli estremi**

Chiama il **codice di produzione**, non riscriverlo. Per ogni punto: `scrollTo(0,y); scrollCur = scrollTarget = scrollY; sync(); applyProjects();` e poi leggi.

Verifica tre cose:
1. `gazeH` è **uguale** a `1.4 * innerHeight`, a meno di un pixel, su entrambe le finestre. Se non lo è, `140vh` e la misura non parlano dello stesso elemento.
2. Con il bordo alto del campo al bordo basso del viewport, `--p` è **0.000**; con il bordo basso del campo uscito dal bordo alto, `--p` è **1.000**. Sono i due estremi della formula: se uno dei due non ci arriva, il denominatore è sbagliato.
3. A metà attraversamento `--p` è **maggiore di .8**, non intorno a .5. È la cubica che fa il suo mestiere; se è .5 l'esponente non è stato applicato.

- [ ] **Step 6: Sonda — le quattro corse stanno nel rapporto giusto**

A `--p` = 0, leggi la `transform` calcolata delle quattro `.gaze-ph`.

Expected, con `innerHeight` = 1000: vetri `260px`, B/N `187.2px`, mensole `161.2px`, lampione `130px`. Cioè **esattamente `--s × 26vh`**.

Se una non torna, `--s` è finito in un posto sbagliato — ed è il modo in cui «la profondità si ricava dalla taglia» smette di essere vero senza che si veda.

- [ ] **Step 7: Convergenza, non `scrollTo`**

Per gli screenshot in movimento serve arrivare a una posizione precisa, e la pagina ha uno scroll inseguito: **non basta un `scrollTo`**. Si converge — leggi `overCur`, scrolla della differenza, aspetta ~450 ms, ripeti. `lockHero()` cambia l'altezza dello scroller a metà strada, quindi un conto fatto una volta sola salta.

- [ ] **Step 8: Fagli vedere il movimento**

Non si giudica da uno screenshot. Chiedigli di ricaricare e guardare. La domanda è una sola: **è abbastanza veloce?**

Se dice che è lenta, sale l'esponente (4, 5). Se dice che scatta, scende (2). **Un numero, un posto.** Aspettati tre o quattro passate: è come è andata con l'allagamento.

- [ ] **Step 9: Commit (solo dopo il suo ok)**

Il messaggio deve dire perché il segnale è `over` e non `scrollY` — è la scelta che un domani qualcuno rifarà al contrario senza sapere che era stata fatta.

---

## Task 3: Il basket

**Files:**
- Modify: `index.html:1894-1905` (markup `mv2`), il CSS della coda, il `@media (max-width:640px)`

**Interfaces:**
- Consumes: la griglia `.mv` che c'è già (`var(--spine) 1fr`), l'ingresso `.on`.
- Produces: la classe `.shot`. Nessuna fase successiva ne dipende.

- [ ] **Step 1: Il markup**

Sostituisce `index.html:1894-1905` per intero (l'`<article id="mv2">` delle competenze).

```html
  <article class="mv" id="mv2">
    <h2 class="mv-tag">02 &mdash; come sto in squadra</h2>
    <div class="mv-body">
      <figure class="shot">
        <img src="img/ph-basket.webp" width="1322" height="1190" loading="lazy"
             alt="In campo, in maglia numero 23, nel caricamento di un tiro.">
      </figure>
      <p class="mv-lead">Da sempre gioco a pallacanestro. Visione d&rsquo;insieme, decisioni rapide sotto pressione, e <i>capire il proprio ruolo dentro una squadra</i> &mdash; che &egrave; poi come si lavora su un prodotto.</p>
    </div>
  </article>
```

`<figure>` e non `<div>`: è un'immagine di contenuto con una relazione col testo che la segue, ed è il tag che lo dice.

- [ ] **Step 2: Il CSS**

Dopo il blocco `.gaze-say`:

```css
/* IL BASKET. Una foto sola, FERMA. E' il contrasto a fare l'argomento: la
   fotografia e' come guarda il mondo, la pallacanestro e' come sta dentro una
   squadra — una cosa si osserva, l'altra si abita. Se anche questa
   galleggiasse, le due sezioni direbbero la stessa cosa con due contenuti
   diversi, e il campo perderebbe l'unica cosa che lo rende un gesto: essere
   l'unico che si muove.
   Nessuna riga di JS: entra con .on come tutto il resto della coda. */
.shot{margin:0 0 2.4em}
.shot img{display:block;width:100%;height:auto}
```

- [ ] **Step 3: Il ramo incolonnato**

Nel `@media (max-width:640px)`, accanto alle altre regole della coda:

```css
  /* a piena larghezza della colonna: a 24px dal bordo non c'e' niente da
     riservare a sinistra, e una foto rientrata sembrerebbe un errore. */
  .shot{margin-bottom:1.6em}
```

- [ ] **Step 4: Sonda di struttura + `node --check`**

Nessun JS è cambiato, ma `node --check` va rifatto lo stesso: costa un secondo e la modifica precedente potrebbe non essere stata committata.

Struttura: `#mv2` contiene una `figure.shot`, una sola `<img>`, `alt` non vuoto, `width` e `height` presenti. Nessun residuo di `.sk` dentro `#mv2`.

- [ ] **Step 5: Altezza del documento, di nuovo**

Prima e dopo il `load`. Cinque immagini su cinque adesso sono in pagina: è l'ultimo momento in cui questa verifica può ancora salvare le sezioni precedenti.

- [ ] **Step 6: Screenshot**

Su entrambe le finestre. Guarda una cosa sola oltre alla composizione: **il verde della palestra contro il fondo `--ink`**. È l'unica delle cinque foto con una dominante fredda larga, e il fondo è marrone caldo.

- [ ] **Step 7: Fagli correggere il testo, poi commit**

La bozza è una bozza. Mostragliela, aspetta l'ok, poi committa.

---

## Task 4: Cosa so fare

**Files:**
- Modify: `index.html:1907-1915` (markup `mv3`), `1183-1195` (CSS `.sk`), `1196-1209` (CSS `.lens`, da cancellare), `1364-1365` (le due regole mobile), `1409-1410` circa (reduced-motion, se cita `.lens`)
- Modify: `doc/bio.md`

**Interfaces:**
- Consumes: `.sk` e `.sk-tags`, che restano.
- Produces: niente. È l'ultima sezione di contenuto.

- [ ] **Step 1: Il markup**

Sostituisce `index.html:1907-1915` per intero (l'`<article id="mv3">` dello sguardo). Il paragrafo di posizionamento è il primo della bio Media di `doc/bio.md`, invariato: **non riscriverlo**.

```html
  <article class="mv" id="mv3">
    <h2 class="mv-tag">03 &mdash; cosa so fare</h2>
    <div class="mv-body">
      <p class="mv-lead">Sono Oliviero Petrucci, Software Designer in Zucchetti. <i>Il mio lavoro parte dal design</i>: scompongo processi complessi in flussi chiari, disegno interfacce ad alta fedelt&agrave; e scelgo la soluzione tecnica pi&ugrave; adatta a farle diventare prodotti reali, performanti e coerenti.</p>
      <!-- l'ordine e' quello del documento e ci sono TUTTE E CINQUE le aree, per
           intero: il taglio non e' mai stato il modo di dire il peso. A dirlo
           adesso sono l'ordine e UN accento — vedi il commento di .sk. -->
      <div class="sk"><h3>Design</h3><ul class="sk-tags"><li>Figma</li><li>Wireframe e Prototipazione UI/UX</li><li>User Research</li><li>Design System</li></ul></div>
      <div class="sk"><h3>Web Tech</h3><ul class="sk-tags"><li>HTML</li><li>CSS</li><li>JS</li></ul></div>
      <div class="sk"><h3>Sviluppo</h3><ul class="sk-tags"><li>React</li><li>Python</li><li>Go</li><li>Java</li><li>Kotlin</li><li>SQL</li></ul></div>
      <div class="sk"><h3>AI / Tooling</h3><ul class="sk-tags"><li>Figma Maker &amp; Agent</li><li>Claude Code</li><li>Antigravity</li></ul></div>
      <div class="sk"><h3>Project Management</h3><ul class="sk-tags"><li>YouTrack</li><li>Planner</li></ul></div>
    </div>
  </article>
```

- [ ] **Step 2: Muore `--k`**

In `index.html:1189-1193`, il titolo di `.sk` perde il moltiplicatore. Riscrivi il commento sopra il blocco (`1181-1186`), che oggi spiega `--k`: se resta, il file mente.

```css
/* LE COMPETENZE. La gerarchia E' l'argomento: il documento chiede di mettere in
   evidenza la progettazione e di tenere i linguaggi di back-end come profondita'
   tecnica, senza far prevalere lo sviluppo sul ruolo.
   PRIMA lo diceva la TAGLIA: --k moltiplicava la misura del titolo, un numero
   per riga. Non va piu' bene da quando la coda e' portata dalle immagini —
   cinque titoli di taglie diverse subito dopo cinque gesti fotografici forti
   COMPETONO col campo invece di aiutarlo, e vince il rumore.
   Adesso lo dicono L'ORDINE E UN ACCENTO: Design e' primo ed e' l'unico in
   #BFCFFF. Stessa informazione, un terzo dello spazio, e una variabile in meno
   da tenere allineata in tre posti (il foglio, il @media, i cinque style in
   linea nel markup).
   Filetto sopra, e sotto solo all'ultima: e' la stessa costruzione di .pd-facts
   e di .proj-list — un elenco di righe separate da un capello, non schede. */
.sk{padding:20px 0;border-top:1px solid #BFCFFF}
.sk:last-child{border-bottom:1px solid #BFCFFF}
.sk h3{
  font-family:var(--grotesk);font-weight:700;color:#554135;
  font-size:clamp(22px,2.6vw,44px);
  line-height:1.05;letter-spacing:-.02em;margin-bottom:.5em;
}
```

E nella palette della coda (accanto a `index.html:1106`), l'accento sul primo:

```css
/* l'unico accento dell'elenco, ed e' l'argomento: il design viene prima. */
.after .sk:first-of-type h3{color:#BFCFFF}
```

Nel `@media (max-width:640px)`, `index.html:1364` diventa:

```css
  .sk h3{font-size:30px}
```

- [ ] **Step 3: Muore `.lens`**

Cancella:
- il blocco CSS `index.html:1196-1209` (`.lens`, `.lens em`, `.lens p`) e il commento sopra
- `index.html:1365` (`.lens p{font-size:19px;max-width:none}`)
- da `index.html:1103` togli `,.after .lens em`; da `1106` togli `,.after .lens p`; da `1109-1110` togli le due righe di `.lens`

**Cancellare, non commentare.** Un blocco commentato in un file di 3600 righe è una regola che qualcuno rimetterà.

- [ ] **Step 4: Il paragrafo duplicato**

In `doc/bio.md`, la bio **Media** ripete due volte, identico, il capoverso «Ho maturato esperienza specifica nel software B2B per la sostenibilità (ESG)…». Togline uno.

Controlla anche `doc/04-bio-esperienze.md`: se ha lo stesso difetto, stessa correzione.

- [ ] **Step 5: `node --check` + sonda di struttura + classi orfane**

Oltre alle solite: confronta le classi del CSS con quelle del markup **nelle due direzioni**. Expected: **zero occorrenze di `lens` e zero di `--k`** in tutto il file.

È la verifica che dice se la cancellazione è finita o se è rimasto un selettore che non seleziona più niente.

- [ ] **Step 6: Screenshot + ok sul testo, poi commit**

---

## Task 5: I tre rami, la pagina intera, la consegna

**Files:**
- Modify: `index.html` (solo correzioni emerse qui), `docs/superpowers/specs/2026-09-02-coda-scura-design.md`

- [ ] **Step 1: Reduced-motion**

Con `prefers-reduced-motion:reduce`, a 1920×1000. Verifica:
- le quattro foto sono al loro posto **finale** (`--p` non è mai stato scritto, il default di 1 regge)
- non c'è nessuna transizione in corso
- il fondo scuro c'è

Se le foto sono spostate in basso, il default in CSS è `0` e va corretto: è il sintomo esatto che il commento di `.gaze` descrive.

- [ ] **Step 2: Il ramo incolonnato**

A 390×844. Verifica: campo incolonnato, tesi **prima** delle foto, nessuna `transform` scritta sulle `.gaze-ph`, il basket a piena larghezza, le competenze leggibili.

Sonda: `mvs[0].el.style.getPropertyValue('--p')` deve essere **vuoto**. Se ha un valore, la scrittura è finita fuori dal suo guardiano.

- [ ] **Step 3: La pagina intera, dall'inizio**

Scorri tutto il racconto dall'intro alla chiusura, su entrambe le finestre desktop. Cerchi **regressioni**, non la coda: le sezioni prima leggono altezze che le cinque immagini potrebbero aver spostato.

Controlla in particolare che l'allagamento arrivi ancora esattamente dove arrivava — è approvato e non deve essersi mosso di un pixel.

- [ ] **Step 4: I punti aperti**

Chiedigli le tre cose che la spec lascia aperte:
1. **Gli URL social** (LinkedIn, Instagram, Github: `index.html:1508-1509` sono `href="#"`). Due link veri battono tre finti: se ne ha due, si mettono quei due.
2. Se le **mensole** sono una sua fotografia di un'opera di Alex Pinna, l'`alt` lo deve dire.
3. Conferma dei due nodi cromatici guardati alla fase 1.
4. **Il `TypeError` preesistente.** In fondo a `index.html` c'è
   `requestAnimationFrame(function loop(now){ ... })(introStart)`: il risultato di
   `requestAnimationFrame` è un numero e viene chiamato, quindi ogni caricamento
   solleva un `TypeError`. **Non rompe niente** — il ciclo è già registrato ed è
   l'ultima istruzione del `.then()` — ma sporca la console. È preesistente, non
   è mai stato segnalato, e **non va corretto di iniziativa**: si propone, e si
   tocca solo se dice di sì.

   *Fatto: proposto, e poi corretto su sua richiesta. La chiamata non c'è più.*

- [ ] **Step 5: Aggiorna la spec**

Riporta in `docs/superpowers/specs/2026-09-02-coda-scura-design.md` quel che è stato **deciso a schermo** e che la spec dava per aperto: l'esponente finale della cubica, le posizioni finali delle foto, l'esito dei due nodi cromatici, gli URL social. Una spec che resta ferma alle ipotesi è una spec che mente al prossimo.

- [ ] **Step 6: Il resoconto**

Dichiara, separandoli: **cosa è stato misurato** (intersezioni, altezza del documento, estremi di `--p`, rapporti delle corse, classi orfane) e **cosa no** (ogni giudizio di composizione). È la regola di questo repo e non è una formalità: l'unica cosa che rende utili le sonde è non spacciarle per gusto.

- [ ] **Step 7: Commit finale e push**

---

## Rollback

Ogni fase è un commit. Per tornare indietro di una fase: `git revert <sha>`.

Le fasi 1 e 2 sono separate apposta — se la parallasse non convince, si revoca la 2 e resta un campo fermo che regge da solo. È il motivo per cui la composizione viene prima del movimento e non insieme.

*Ed è servito davvero: la parallasse non ha convinto. Non è stata revocata con un `revert` ma riscritta in diapositiva, e il campo fermo della fase 1 è quello che è rimasto sotto — il 907.*

Le fasi 3 e 4 sono indipendenti fra loro e dalla 2: si possono revocare da sole.
