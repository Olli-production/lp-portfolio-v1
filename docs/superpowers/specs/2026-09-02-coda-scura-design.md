# La coda scura — le sezioni dopo l'allagamento

Data: 2026-09-02. Stato: **implementata** sul ramo `worktree-coda-sguardo`.
I paragrafi marcati «corretto a schermo» sono quello che l'implementazione ha
cambiato rispetto al disegno, con la ragione.

Riguarda la sezione `.after` e la sua chiusura. **Sostituisce per intero** la
versione precedente di questo file, che descriveva il gomito, il cuneo che si
apriva dentro `.after` e un fondo `#554135`: tre cose che non esistono più.

**Riallineata tre volte.** La prima stesura descriveva il campo come una
parallasse continua; è diventato una diapositiva sui cinque fotogrammi
dell'utente. Poi è arrivato un **redesign dell'intera coda** — parola di fondo,
nastro al posto dell'elenco, movimenti senza etichetta, la sezione agganciata
all'inseguitore. Poi un terzo giro, che è quello che questo documento descrive
adesso: **le competenze passano dal nastro a tre colonne, il basket entra nel
palco come seconda diapositiva, e la giunta fra la linea del tempo e la coda
smette di aprirsi.** Dove una decisione è cambiata è riscritta, non cancellata in
silenzio: si dice anche perché.

La direzione l'ha data l'utente, e i suoi punti sono l'indice di quello che
segue. Primo giro: lo scroll era tornato secco; c'era troppo vuoto prima della
prima sezione; via numeri ed etichette; la prima sezione era una lista piatta; il
campo si vedeva montato prima di partire; il basket doveva entrare da solo.
Secondo giro: *«la scritta sale insieme al suo sfondo ma si distaccano dallo
sfondo precedente»* (§3bis); *«non mi piace il layout a scorrimento orizzontale
delle competenze: vai su un più classico a colonne»* (§4); *«unisci le sezioni
fotografia e basket come se fossero due slide»* (§5 e §6).
Terzo giro, **su quattro fotogrammi disegnati in Figma** (913-916) invece che a
parole: il primo movimento diventa una sequenza che lo scroll srotola — parola in
contorno, ritratto, bio, striscia azzurra, competenze (§3ter e §4).

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
- Le cinque fotografie sono convertite in `img/ph-*.webp` (`27e6145`) e adesso
  sono **tutte in pagina**: quattro nel campo, il basket nell'ultimo movimento.

Quel che c'era da fare era il contenuto della coda: erano tre movimenti con la
stessa griglia, lo stesso ingresso e lo stesso ritmo, e si leggevano come un
documento impaginato bene, non come un racconto. Adesso il racconto c'è, e da qui
in avanti questo documento descrive **quello che sta a schermo**.

**Una cosa in più non c'entra col contenuto e viene prima di tutto: lo scroll.**
Il resto del sito non lo scorre il browser — c'è un inseguitore a molla
(`scrollCur`), e le sezioni sopra sono `position:fixed` mosse da lui. `.after`
stava nel flusso, quindi la molla non la toccava e la coda tornava secca proprio
dove tutto il resto è morbido. Vedi §3bis.

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

Restano **due `.mv`**, con la stessa classe `.on`, lo stesso `mvs.forEach` nel
ciclo, la stessa costruzione `[...document.querySelectorAll('.mv')]`. Cambia cosa
contengono, e cambia la cornice.

| | prima | adesso |
|---|---|---|
| — | niente | **`CHI SONO`** in contorno: l'`<h2>` della coda, dentro il palco di `mv1` |
| `mv1` | posizionamento, due paragrafi | **la sequenza** — quattro fotogrammi che lo scroll srotola: parola, ritratto, bio, striscia, competenze |
| `mv2` | competenze, cinque `.sk` | **il palco** — due diapositive: il campo e il basket |
| `mv3` | lo sguardo, due paragrafi | *non esiste più*: il basket è la seconda diapositiva di `mv2` |
| `.after-end` | stellina + mail | uno schermo esatto, per far atterrare il salto d'uscita |

**Erano tre, sono due.** Il basket era un movimento a sé, dopo il campo; l'utente
ha chiesto di unirli — *«unisci le sezioni fotografia e basket come se fossero
due slide»* — e adesso il basket sta **dentro** `.gaze-stage`. Non viene dopo le
fotografie: **prende il loro posto.**

**Via numeri ed etichette.** `01 — cosa so fare` e le sue due sorelle non ci sono
più: erano l'impalcatura tipica di una landing, e su tre sezioni di fila si
leggevano come una griglia da CV. Al loro posto c'è **una sola** intestazione, la
parola di fondo, che è anche l'unico `<h2>` della sezione — la scaletta dei
titoli resta vera e chi legge con la voce sente «Chi sono» una volta, non tre
numeri.

**Con le etichette cade anche la colonna guida.** `.mv` era una griglia
`var(--spine) 1fr`: 30vw di colonna stretta esistevano per appoggiarci
l'etichetta, e senza di lei restavano un terzo di pagina vuoto. Adesso `.mv` è un
blocco e il margine di lettura è `--gut`, 10vw — è il «non sfrutta lo spazio»
segnalato dall'utente, e si risolve togliendo, non aggiungendo.

**Il campo si cerca per classe e non per posizione**
(`gazeM = mvs.find(m => …contains('gaze'))`): con `mvs[0]` ogni riordino
prenderebbe il movimento sbagliato **in silenzio**.

---

## 3bis. Lo scroll: la coda esce dal flusso

> **La giunta con la sezione precedente.** Fuori dal flusso, la coda si muove con
> l'inseguitore mentre `.tl-stage` sopra è `sticky` e la muove il compositore:
> **sono due orologi**, e scorrendo la coda resta indietro di qualche centinaio di
> pixel — misurati, fino a 734 a 1920×1000 e 800 a 1280×1024. In mezzo si scopriva
> `.after-space`, che era **trasparente**: sotto c'è il `body`, crema più
> `bg.webp`. Era la striscia chiara segnalata dall'utente, *«la scritta sale
> insieme al suo sfondo ma si distaccano dallo sfondo precedente»*.
>
> Il rimedio è una riga: `.after-space{background:var(--ink)}`. Il buco geometrico
> resta — i due orologi sono due, e la molla è quel che rende morbido tutto il
> sito — ma non si **vede** più, perché i due lati e quel che sta in mezzo sono lo
> stesso `--ink`. È lo stesso trucco dell'allagamento, che apre il marrone *prima*
> della sezione perché i bordi combacino da soli. E non è un cerotto sul sintomo:
> lo spaziatore **è** la coda vista dal documento, ed è giusto che ne abbia il
> colore. Sincronizzare i due orologi vorrebbe dire togliere la molla o portare la
> linea del tempo fuori dal flusso: due riscritture per un difetto che costa una
> riga. Verificato: la striscia scoperta è `#2B1A0F` in 23/23 e 26/26 frame di una
> corsa decisa, sulle due proporzioni.

Il resto del sito non lo scorre il browser. C'è un inseguitore a molla — `sync()`
integra `scrollCur` verso `scrollTarget` — e `over` è quanto si è andati oltre
l'intro **filtrato da quella molla**. La sezione progetti è `position:fixed` e la
muove una riga sola, `translateY(-over)`.

`.after` invece stava nel flusso: la scorreva il compositore, alla velocità vera
della rotella. Il risultato è quello che l'utente ha descritto — *«lo scroll in
questa sezione è tornato normale, non è più rallentato e fluido come nel resto
della pagina»* — e non era una sensazione: sono due orologi diversi sulla stessa
pagina.

Adesso la coda è trattata come i progetti:

```css
@media (hover: hover){
  .after{position:fixed;left:0;right:0;top:0}
}
```
```js
if (canHover) after.style.transform = `translateY(${(af0 - over).toFixed(1)}px)`;
```

- **`top:0` e non `100svh`.** La coda non comincia sotto la piega: ci arriva
  quando `over` raggiunge `af0`, e la traslazione `af0 - over` vale esattamente
  zero in quel momento. Prima è positiva e la tiene sotto lo schermo.
- **Serve uno spaziatore.** Fuori dal flusso la coda non occupa più spazio nel
  documento, e senza `.after-space` la pagina finirebbe un'intera coda prima: non
  ci sarebbe scroll per vederla. L'altezza gliela mette `measure()`, che è
  l'unico posto che sa se la sezione è fuori dal flusso.
- **Lo spaziatore è anche il righello.** `af0` si legge dal suo `offsetTop`, non
  da quello di `.after`: da fissa, `offsetTop` è misurato dal blocco contenitore
  iniziale e vale zero — un numero che sembra giusto e non lo è.
- **Solo col puntatore.** Al dito lo scroll nativo del compositore è già la cosa
  giusta, e la sezione resta nel flusso con lo spaziatore a zero. Idem in
  `prefers-reduced-motion`, dove nessuno scriverebbe la traslazione e la coda
  resterebbe inchiodata a schermo.

Misurato: `af0` identico a prima del cambio (20409 su 1920×1000), e dopo uno
scatto di rotella la coda **resta indietro e converge** invece di arrivare nello
stesso frame — che è la prova che a muoverla è la molla e non il browser.

---

## 3ter. La parola, in contorno

`CHI SONO` a piena larghezza, in cima. Riempie lo spazio che c'era prima della
prima sezione — *«uno spazio troppo vuoto»* — e non lo riempie con un titolo che
grida: è il fondo su cui la coda è scritta.

**Era marrone su marrone e faceva parallasse.** Adesso è il **tracciato in
contorno disegnato nel Figma** (fotogrammi 913-916), esportato in SVG, e **sta
ferma**: `--w` e i suoi due comportamenti — translate e fade — sono caduti,
perché nei quattro fotogrammi la parola è identica in tutti e quattro. È il
fondo della sequenza, non una cosa che scorre via.

**Il tratto è `#B2998C`, che il foglio chiama già `--line`** ed è il filetto di
THINKER / BUILDER: nessun colore nuovo. Sul marrone misura **1.26:1**, ed è
voluto — deve leggersi come una texture, non come un testo. *La sonda dei
contrasti lo tratta a parte, con una banda dichiarata (1.2–1.8) invece della
soglia AA: un'eccezione scritta è una decisione, un'eccezione silenziosa è un
difetto.*

**Perché un'immagine e non più testo.** Il contorno ha un filtro di disturbo per
glifo; `-webkit-text-stroke` darebbe una linea geometrica, che è un'altra cosa.
Resta un `<h2>` con `aria-label="Chi sono"` e l'immagine `aria-hidden`: la
scaletta dei titoli non perde niente, chi legge con la voce sente la parola una
volta sola, e il `19vw` con il conto sui glifi non serve più — la proporzione la
tiene il viewBox del file.

---

## 4. `mv1` — i quattro fotogrammi, e li srotola lo scroll

Questa sezione **sostituisce due stesure precedenti**: le cinque righe con le
pillole (respinte: *«troppo piatta, non sfrutta lo spazio»*) e il nastro a tre
fasce (respinto: *«non mi piace il layout a scorrimento orizzontale»*). Ora è
una sequenza disegnata in Figma, quattro fotogrammi:

| | cosa c'è a schermo |
|---|---|
| **913** | la parola in contorno, e nient'altro |
| **914** | il ritratto e la bio centrata sopra di lui |
| **915** | ritratto e bio spariti, la striscia azzurra a metà schermo |
| **916** | la striscia salita sulla parola, il titolo e le competenze |

### La meccanica: un nastro, non un filmato

Scelta dell'utente fra tre proposte — a tempo, legata allo scroll, a
diapositiva. **Legata allo scroll**: la sequenza non ha una durata, ha una
lunghezza. Avanza se scorri, si ferma se ti fermi, torna indietro se risali.

- La sezione è alta **300vh**; il palco ne occupa 100 e i restanti **200 sono la
  corsa**. Cambiando quel numero cambia solo *quanto scroll serve*: le finestre
  restano dove sono, perché sono frazioni della corsa e non distanze.
- **Il palco è pinnato a mano**, con una riga: lo si trasla in giù esattamente
  dei pixel di cui la coda è salita. **Niente `position:sticky`** — la coda è
  fissa e la muove una transform, e uno sticky lì dentro non avrebbe un
  contenitore di scorrimento a cui agganciarsi.
- **Perché non a diapositiva**, che pure era il meccanismo già costruito: sarebbero
  diventati due blocchi di rotella nella stessa coda, a due schermi di distanza, e
  il palco del campo resta *l'unico* punto del sito in cui si toglie lo scroll.

### Tutto pende da `--q`

Un numero solo, 0..1, che il ciclo scrive sulla sezione. Ogni pezzo ha la sua
finestra dentro quel numero, e **le finestre si accavallano dove il disegno dice
«nel mentre»**:

| pezzo | finestra | cosa fa |
|---|---|---|
| parola | `0 → .06` | compare |
| ritratto | `.10 → .34` | sale **dal centro dello schermo** e sfuma dentro |
| bio | `.16 → .40` | si forma dalla polvere (`dust()`, la stessa del claim) |
| ritratto + bio | `.48 → .62` | se ne vanno insieme, in dissolvenza |
| striscia | `.48 → .70` | si disegna da sinistra — **nel mentre** del fade out |
| striscia | `.72 → .88` | sale sulla parola |
| competenze | `.74 → .92` | entrano — **nel mentre** della salita |

**L'ultimo 8% non ha nessuna finestra, ed è voluto:** la composizione finale
dev'essere ferma e leggibile per un pezzo di scroll prima che la sezione se ne
vada, o l'ultima cosa che si vede è un'animazione a metà.

### Il ritratto

Ritaglio con canale alfa, **la stessa forma organica del basket** — stesso
trattamento, stesso `conv-foto.py`. La corsa è **lunga**, scelta dell'utente
fra due: parte davvero da metà schermo e sale fino al suo posto.

```css
.m1-me{
  --m1-top:9.2vh;
  transform:translateY(calc((1 - <entrata>) * (50vh - var(--m1-top))));
}
```

**La corsa si ricava dalla posizione a riposo** (`50vh - --m1-top`): scrivendo i
due numeri separati se ne sarebbe rotto uno solo, e in silenzio.

*Trappola pagata:* l'export del nodo a `scale 3` con `download_assets` torna col
fondo del frame (`#FFFEF8`) **cotto dentro** — rende il nodo nel suo contesto,
non isolato. Quello che `get_design_context` restituisce per lo stesso nodo è
ritagliato davvero. Il sintomo era un rettangolo bianco attorno alla foto.

### La bio

Stesso testo di sempre — il primo capoverso della bio Media di `doc/bio.md` —
ma **centrato e sopra il ritratto**, come nel fotogramma. L'ombra
(`0 3px 14px --ink`) non è un vezzo: il testo passa sulla parte chiara della
fotografia. È lo stesso mestiere del terzo centrale nel campo, risolto con
l'ombra invece che con lo spazio, perché qui la composizione è una sola cosa al
centro. Peso **500** e non 300: sopra una fotografia i tratti sottili
spariscono.

**L'ingresso lo scrive `dust()` sui due paragrafi, l'uscita il foglio sul
contenitore.** Due padroni, ma su due elementi diversi, e le opacità si
moltiplicano da sole.

### La striscia

Un path solo, tratto 20, gradiente da `--blue` a `#BFCFFF`: i due azzurri che il
sito ha già.

- **Si disegna con `stroke-dashoffset`**, e il path porta **`pathLength="1"`**:
  dash e offset diventano frazioni, e nessuno deve misurare la curva. È la stessa
  idea del nastro dell'atto 2, senza il pezzo di JS che lì serviva.
- **Inline e non un `<img>`**: quel che si anima è il path, e dentro
  un'immagine non ci si arriva.
- La salita è la differenza fra le due quote dei fotogrammi (`50vh → 6.5vh`), non
  un terzo numero.

### Il blocco delle competenze

Cinque blocchi in **tre colonne tipografiche** (`columns:3`, non una `grid`: a
griglia i cinque finivano su due righe e sotto i blocchi corti restava un buco
alto mezzo schermo). Sopra, il titolo «competenze *tecniche*» — la stessa coppia
tondo + corsivo azzurro della bio e del claim, che è la lingua dell'enfasi in
tutto il sito.

**La scaletta d'ingresso è uno sfalsamento su `--q`**, non più un
`transition-delay`: ogni colonna apre la sua finestra un ventesimo di corsa dopo
la precedente. Scorrendo veloce entrano quasi insieme, ed è giusto — è la natura
di una cosa che si srotola invece di andare a tempo.

### Cosa se n'è andato

- **`--w`**, la parallasse della parola, e **`--r`**, la corsa del nastro:
  nessuno li scriveva più e nessuno li leggeva.
- **Il nastro**: `.rib3`, `.rb`, `.rb-run`, `.rb-in`, le tre `--v`, l'offset `--o`
  e le copie `aria-hidden`.
- **La classe `.on` e la lista `mvs`**, con il pezzo di ciclo che la dava e la
  costante `AF_ON`. Non era rimasto niente che la usasse: il primo movimento è
  governato da `--q`, il campo da `--t`, e le due frasi dentro il palco le
  disegna `dust()`. *Una classe che nessuno legge è peggio di niente: la si cerca
  per ore quando qualcosa non compare.*

---

## 5. `mv2` — il campo, una diapositiva

**Questa sezione sostituisce la parallasse** che la prima stesura descriveva:
quattro foto che salivano dietro una tesi `sticky` mentre la pagina scorreva, con
un `--p` cubico scritto a ogni frame. Non è quello che c'è. L'utente ha disegnato
**cinque fotogrammi in Figma** (905-909), e quelli sono la specifica:

```
  905  schermo tutto marrone, vuoto
  906  primo scroll: la frase compare e le foto ENTRANO DA SOTTO
  907  punto statico: foto grandi al loro posto, frase su marrone pulito
  908  secondo scroll: le foto ESCONO DALL'ALTO
  909  vuoto di nuovo
```

**I fotogrammi descrivevano il solo campo, e adesso il palco ne ospita due.** Col
basket dentro, il 908 cambia di significato: non è più un'uscita verso il vuoto,
è lo **scambio** fra la prima diapositiva e la seconda. Gli stati fermi sono
quattro e i gesti tre.

Non è una parallasse — non c'è più niente che si muova in funzione della
posizione della pagina — è **una diapositiva**: la pagina si ferma, un gesto
avanza di uno stato, e quando la sequenza è finita la pagina riparte.

### Tre vincoli che non vanno reinterpretati

Sono stati sbagliati almeno una volta ciascuno, e stanno commentati nel foglio
(`index.html:1197`) perché non succeda di nuovo.

1. **Le foto non si scuriscono mai.** Nei fotogrammi sono sempre a piena luce e
   la frase sta su marrone pulito. Le foto stanno sui due terzi laterali, il
   terzo centrale è della frase: nessuna sovrapposizione, nessun velo, nessun
   `brightness`. Una versione intermedia le faceva passare dietro al testo
   scurendole — non è quello.
2. **La frase esce dopo le foto.** In 908 le fotografie se ne sono quasi andate e
   la frase è ancora intera; sparisce solo fra 908 e 909.
3. **Una passata, un passo.** Vedi «il gesto», più sotto.

### Un numero solo, e si chiama `--t`

`--t` è l'unica proprietà che il JS scrive sul palco: vale 0, 1, 2 o 3, e i
valori in mezzo mentre si muove. Tutto il resto lo ricava il foglio.

| `--t` | cosa c'è a schermo |
|---|---|
| `0` | 905, vuoto |
| `1` | **la prima diapositiva**: le quattro fotografie e la loro frase (907) |
| `2` | **la seconda diapositiva**: il basket e il suo testo |
| `3` | 909, vuoto di nuovo |

**Erano tre, sono quattro**, e il cambiamento non è aritmetico: il `2` non è più
un'uscita verso il vuoto, è **lo scambio**. Le finestre sono una staffetta, e i
numeri sono gli stessi visti da due parti — `1 + .8 = 1.8` e `1.3 + .7 = 2`:

| chi | entra | esce |
|---|---|---|
| le foto | `0 → 1` | `1 → 1.8` |
| la frase del campo | `0 → 1` | `1 → 1.8`, **con le foto** |
| il basket | `1.3 → 2` | `2 → 2.8` |
| il ritaglio del basket (opacità) | `1.5 → 2` | — |

Il mezzo passaggio di sovrapposizione fra `1.3` e `1.8` è quel che impedisce allo
schermo di restare vuoto in mezzo allo scambio senza accavallare due composizioni
intere. **Toccando uno dei due numeri va ricontrollato l'altro.**

Ogni foto ha **`--s`**, quanto è grande sullo schermo, e da lì escono anche le
sue due corse:

```
width:      calc(var(--s) * 35vw)
translateY: clamp(0, 1 - t,        1) * --s * 200vh   (entrata, da sotto)
          - clamp(0, (t - 1) / .8, 1) * --s * 180vh   (uscita, verso l'alto)
```

Più è grande più è vicina, più è vicina più si muove: **la profondità non si
tara, si legge dalla taglia.**

| foto | `--s` | posizione | dimensioni reali |
|---|---|---|---|
| `ph-architettura-vetri` | `.80` | `top:-14vh; right:-14vw` | 1600×1280 |
| `ph-lampione` | `.72` | `top:14vh; right:-4vw` | 1280×1600 |
| `ph-architettura-bn` | `.62` | `top:-18vh; left:-12vw` | 1280×1600 |
| `ph-mensole` | `.54` | `top:36vh; left:-2vw` | 1216×1600 |

**Le foto sono cresciute** — il fattore è passato da `35vw` a `44vw`, e in campo
misurano ora 475/600/760/401 px di altezza visibile contro 381/477/605/330 — **e
sbordano di più**, non entrano di più. È così che si fanno grandi senza invadere
il terzo centrale: il campo non ha margini, ha un fuori.

La posizione sta **in linea nel markup** perché è composizione, non regola. Due
foto sbordano da sinistra e da destra, tutte e quattro dall'alto: il campo non ha
margini, ha un fuori.

**L'uscita si compie in `.8` di passaggio e non in uno intero**: sono i due
decimi che restano al basket che sta arrivando, così le foto sono già fuori
quando lui si ferma.

**Le corse sono generose — 200vh e 180vh — e non è spreco.** A `--t` 0 e a `--t`
3 lo schermo dev'essere completamente vuoto, e la foto più piccola è anche quella
che si muove meno. Il caso peggiore è la B/N, che parte da `top:-14vh`: con 200vh
di corsa il suo bordo alto finisce a 110vh, sotto la piega. Meno di così e nel
905 si vedrebbe spuntare un angolo.

**`--t` vale `1` di default nel foglio**, non `0` né `3`. I rami che non lo
scrivono — touch e reduced-motion — devono vedere il 907: gli altri due sono uno
schermo vuoto, e uno schermo vuoto sembra un errore di caricamento. Il `2` non
andrebbe bene per un'altra ragione: là il palco mostrerebbe il basket e terrebbe
le fotografie fuori, e **in quei rami non c'è nessun gesto che le riporti**. Come
il palco si srotola senza movimento è scritto in §9.

### Il difetto: il campo si vedeva montato prima di partire

Segnalato così: *«si bugga, vedo già gli elementi quando scrollo e poi si resetta
e parte l'animazione»*. Era esatto. Fuori dalla diapositiva il campo mostrava il
907 **da tutte e due le parti**: scendendo si vedevano le fotografie al loro
posto, poi all'aggancio la diapositiva scattava a `--t 0` e le faceva sparire e
rientrare.

Lo stato a riposo adesso dipende **da che parte lo si guarda**, ed è una riga:

```js
const fermo = top > 0 ? 0 : 1;
```

Sopra il campo — bordo alto ancora sotto la piega — è il **905**, vuoto: chi
scende trova lo schermo pulito e il primo gesto ha qualcosa da far entrare.
Sotto, e mentre lo si risale, resta il 907: lì uno schermo vuoto sembrerebbe una
sezione sparita.

*Due asserzioni delle sonde si sono capovolte con questa correzione, e sono state
riscritte dicendolo: prima chiedevano il 907 sopra il campo.*

### Il palco

`.gaze` è alto **uno schermo esatto** e non contiene niente di suo: sta tutto
dentro `.gaze-stage`, alto quanto lui, `overflow:clip`, ed è lui a tagliare le
fotografie sui quattro bordi come nei fotogrammi.

**Niente `sticky`.** Il campo è alto quanto lo schermo, quindi uno sticky avrebbe
zero corsa e non farebbe niente; a tenere ferma la pagina è il blocco dello
scroll. Un pezzo che non fa niente non è gratis: è una cosa in più da capire
quando qualcosa non torna.

**`position:relative` sul palco è quello che fa funzionare il ritaglio**, e non è
ovvio: `overflow` non taglia un discendente assoluto il cui blocco contenitore
sia un *antenato* della scatola che taglia. Senza quella riga le foto si
posizionano su `.gaze`, escono dal palco come se il clip non ci fosse, e lo
sbordo a destra dei vetri allarga il documento di 6vw — la pagina prende una
barra orizzontale che `main` non ha. Misurato in tutta la corsa: documento a 1920
su finestra da 1920.

**`clip` e non `hidden`**: `hidden` farebbe del palco un contenitore di
scorrimento.

### La frase

Sta nel terzo centrale, larga `min(30vw, 560px)`, centrata sul palco.

**La larghezza è in vw e non in `ch`**, ed è una trappola pagata: `ch` si risolve
nel font dell'elemento su cui è scritto — qui il genitore, 16px — non in quello
del testo dentro, che è tre volte più grande. `30ch` davano nove caratteri per
riga.

**E `padding:0`, che è la seconda metà della stessa trappola.** `.gaze-say` è un
`.mv-body`, e i due `--gut` da 10vw si mangiavano metà della colonna: la frase
tornava a sette righe su una colonna larga il doppio. La sua misura è la
larghezza, non il padding.

La comparsa è **`dust()`**, la stessa funzione del claim e dei titoli delle
sezioni precedenti: sfocatura più opacità, scalettata riga per riga. Non è una
dissolvenza scritta per la coda — era già nel file, ed era la richiesta esplicita
dell'utente. Le unità sono l'etichetta e i tre `<span>` della frase.

Sono `<span>` **inline** e non blocchi: a scatola ognuno diventerebbe indivisibile
e la riga andrebbe a capo dove capita. È anche il motivo per cui la comparsa è
sfocatura e opacità e non una traslazione — sugli inline le `transform` non si
applicano. Lo spazio fra i tag è testo vero, quindi gli a capo cadono come se i
tag non ci fossero.

La finestra della frase è `--t ≤ 1` per entrare e l'ultimo quinto (`1.8..2`) per
uscire: è il vincolo 2 scritto in un'espressione. Sembra stretta e non lo è —
`out3` carica il movimento all'inizio, quindi in `--t` si arriva a 1.8 nel primo
quarto del tempo, e gli altri tre quarti se li prende tutti la frase.

### Il gesto

Tutto il meccanismo sta in **un blocco solo** (`index.html:2355`): stato,
disegno, aggancio, gesti e passo per frame. Il ciclo lo chiama con una riga
(`index.html:3426`). Era sparso in tre punti lontani del file — le funzioni
stavano perfino sopra le variabili che leggevano — e ogni correzione ne toccava
tre. **Non va risparpagliato.**

| costante | valore | cosa dice |
|---|---|---|
| `GZ_DUR` | 900 ms | durata di un passaggio |
| `GZ_SOGLIA` | 55 | quanta rotella serve perché un gesto conti |
| `GZ_QUIETE` | 220 ms | silenzio che chiude una passata |

**UNA PASSATA, UN PASSO.** È la regola, ed è più forte di una soglia. Un colpo di
trackpad manda decine di eventi per un secondo abbondante, con l'inerzia che
continua dopo che il dito si è alzato: contarli a delta accumulato non basta,
perché appena il passaggio finisce la coda della stessa passata ne fa scattare un
altro e la sequenza si attraversa tutta con un gesto solo — l'esatto contrario
della richiesta. Quindi **non si guarda quanta rotella arriva, si guarda quando
tace**. La soglia resta, ma solo per non far scattare niente con uno sfioramento.

**Si arma sul passaggio, non sulla presenza.** Con uno scorrimento veloce il campo
può essere attraversato in un frame solo: chiedere «il bordo alto è dentro?» lo
mancherebbe, chiedere «l'ha appena superato?» no.

**I gesti sono tre**, non più due: `0 → 1` fa entrare le fotografie, `1 → 2` le
scambia col basket, `2 → 3` svuota lo schermo e restituisce la pagina.

**Gli estremi sono due, `0` e `3`, e in mezzo ci sono due composizioni.** Finché
la posa era una sola bastava negarla (`gzStep !== 1`); adesso servono i due
estremi per nome, o si uscirebbe dal palco appena arrivati sul basket.

**Si esce saltando al bordo opposto**, e il salto è gratis perché ai due estremi
il campo è vuoto. Senza il salto resterebbe da scorrere uno schermo di marrone
vuoto — che è esattamente la «sezione scomparsa» segnalata dall'utente.

**Lo stato di riposo dipende da che parte si è usciti**, e non è più uno solo:
uscendo di sotto resta il **basket** (`2`), l'ultima composizione vista, perché
risalendo ci si aspetta di ritrovare quella; uscendo di sopra resta il **vuoto**
(`0`), perché chi ridiscende deve avere qualcosa da far entrare col primo gesto.
La stessa regola è scritta due volte — in `gzEsci()` si legge dal verso, nel ramo
sbloccato di `gzTick()` dalla posizione.

**Il salto deve avere dove atterrare.** `gzEsci()` sposta la pagina di una
schermata intera: se dopo il campo la coda finisce prima, quello `scrollTo` lo
tronca il browser e si resta col basket **tagliato a metà in cima allo schermo**.
È successo nel momento esatto in cui il basket è uscito dal flusso — la coda si è
accorciata di uno schermo e il salto atterrava a `-606` invece che a `-1000`. Il
rimedio sta in §7: la chiusura si prende uno schermo esatto.

`preventDefault` copre la rotella e i tasti; una riga per frame copre tutto il
resto — barra di scorrimento trascinata, inerzia che arriva dopo il rilascio, un
ancoraggio del browser — e riporta la pagina dove deve stare, con mezzo pixel di
tolleranza per non litigare con l'arrotondamento. Serve `passive:false`: è il solo
modo di togliere lo scroll senza toccare `overflow` del documento, che sposterebbe
la posizione e con lei tutte le misure già prese.

### Perché qui si può sequestrare la rotella

È **l'unico punto del sito** in cui succede, ed è una richiesta esplicita
dell'utente, non una libertà presa. Vale la pena dire perché è lecito proprio
qui: il campo è alto esattamente uno schermo e ai due estremi è vuoto, quindi
entrare, uscire e saltare da un bordo all'altro non sposta niente di visibile.
Tutto il resto del sito resta una funzione della posizione; questo pezzo no, ed è
l'unico che porta stato.

### Corretto a schermo

Cose che il disegno non aveva previsto, trovate implementando e verificate con
una sonda.

- **L'ingresso scalettato dei movimenti non vale per il campo.** `.mv.on .mv-tag`
  pesa più di `.gaze-say .mv-tag`, quindi vincerebbe lui e l'etichetta resterebbe
  accesa anche nel 905, che deve essere uno schermo vuoto. Escluso con
  `:not(.gaze)`: è più onesto che rincorrere la specificità dall'altra parte.
- **Il paragrafo del campo è sempre acceso.** È un `.mv-body > *`, quindi la
  regola generale lo terrebbe a opacità 0 finché non arriva `.on` — e un genitore
  trasparente rende invisibili anche i figli, `dust()` o non `dust()`. Le unità
  della polvere sono l'etichetta e gli `<span>` dentro, non lui.
- **Niente transizioni CSS sulle unità della polvere.** Sarebbero un secondo
  padrone sulla stessa proprietà, e vincerebbe l'ultimo a scrivere.
- **Il filetto `.mv::before` sul campo non ha senso**: non ci sono colonne, e
  crescerebbe dal nulla verso il nulla. `content:none`.
- **Si dipinge subito all'armamento.** `--t` in linea non esiste finché
  `gzPaint()` non lo scrive, e senza vale il default del foglio, che è 1: chi
  arriva vedrebbe il 907 già montato e il primo scroll non avrebbe niente da far
  entrare.
- **Il `return` dopo l'uscita non è facoltativo.** L'uscita sposta la pagina di
  una schermata, e la riga che la tiene ferma mentre è bloccata la riporterebbe
  indietro nello stesso giro, annullando il salto.

---

## 6. `.gz-team` — il basket, seconda diapositiva

Era `mv3`, un movimento a sé dopo il campo. Adesso sta **dentro `.gaze-stage`**,
ultimo fra i figli e con lo `z-index` più alto: entrando passa sopra la frase che
se ne sta andando. Non è più un `.mv` — non ha nessun `.on` da ricevere né una
posizione da cui farsi accendere, lo governa `--t` come tutto quel che sta lì
dentro.

```css
.gz-team{
  position:absolute;inset:0;z-index:3;
  display:flex;align-items:center;
  transform:translateY(calc(
      clamp(0, calc((2 - var(--t)) / .7), 1) * 100vh
    - clamp(0, calc((var(--t) - 2) / .8), 1) * 100vh));
}
.gz-team .mv-body > *{opacity:1;transform:none;transition:none}
.gz-team .shot{opacity:clamp(0, calc((var(--t) - 1.5) / .5), 1)}
```

**`inset:0` e non un'altezza**: la slide è esattamente il palco, quindi 100vh di
traslazione la portano fuori di preciso, sopra o sotto. Nessun numero da
azzeccare, e cambiando l'altezza del palco il conto si rifà da solo.

**Stesso verso per tutti.** Il basket sale da sotto mentre le fotografie salgono
via in alto: è quel che fa leggere il secondo gesto come **uno scambio** e non
come due animazioni che capitano insieme. È la scelta fatta sui tre schizzi
proposti — l'alternativa quieta (dissolvenza sul posto) toglieva la direzione, e
quella laterale sarebbe stata l'unico movimento orizzontale della coda.

**La riga dell'opacità non è cosmetica**: senza, i figli resterebbero
all'opacità zero di `.mv-body > *` per sempre e il palco scambierebbe due schermi
vuoti. Lì l'ingresso scalettato lo dà `.on`, e qui `.on` non arriva mai.

**Il ritaglio arriva dopo il testo**, ed era già così quando il basket era una
sezione ferma: là erano `0.28s` di `transition-delay`, qui è una dissolvenza
agganciata a `--t` da `1.5` a `2`, perché dentro il palco il tempo non esiste —
esiste il passaggio. Misurato: `0.3` a metà scambio, `1` a fine.

**Perché non galleggia come le foto.** Prima questa sezione stava ferma apposta,
e l'argomento era che una cosa si osserva e l'altra si abita. L'argomento regge
ancora, ed è per questo che il basket non si sparpaglia: arriva intero, in un
blocco solo, e si ferma. È il campo a essere sparpagliato.

### Il ritaglio

La fotografia è **un ritaglio con canale alfa**, quadrata, 1248×1248: il contorno
è una forma organica e il fondo non c'è. Sul marrone della coda il verde del
campo stacca da solo — nessuna cornice, nessun filo di bordo. È anche il motivo
per cui `conv-foto.py` la converte in `RGBA` mentre le altre quattro vanno in
`RGB`: un `convert('RGB')` qui appiattirebbe la trasparenza su nero.

È un `<figure>` e non un `<div>`: è un'immagine di contenuto con una relazione
col testo che l'accompagna, ed è il tag che lo dice.

### Testo e ritaglio, affiancati

```css
.gz-team .mv-body{
  display:grid;grid-template-columns:minmax(0,1fr) minmax(0,1.15fr);
  gap:4vw;align-items:center;
}
```

Il testo sta a sinistra ed è **primo nel markup**: entra per primo, e il ritaglio
lo raggiunge dopo.

- **`minmax(0,1fr)` e non `1fr`**: un `1fr` ha `min-width:auto` e la colonna del
  testo non scenderebbe mai sotto la sua parola più lunga.
- **Centrati e non allineati in cima**: il ritaglio è quadrato e il testo fa
  quattro righe; appeso in alto lascerebbe sotto un buco alto mezza foto.
- **Su mobile e in reduced-motion torna una colonna**, e la slide torna nel
  flusso: affiancate a 390px darebbero 170px per parte.

**Il `transition-delay` a quattro classi non serve più.** La nota sulla
specificità (`.mv.on .mv-body > :nth-child(2)` vale quattro classi contando la
pseudo-classe) resta vera e vale ancora per `mv1`: qui non serve perché il
ritardo non è più un `transition-delay`.

---

## 7. La chiusura

`.after-end` — stellina e «contattami» — non cambia disegno, ma **si prende uno
schermo esatto**:

```css
.after-end{min-height:100svh;align-content:center}
```

**Non è aria decorativa, è la corsa che serve al salto d'uscita del palco.**
`gzEsci()` sposta la pagina di una schermata intera per non lasciar scorrere il
marrone vuoto; se dopo il campo la coda finisce prima, quello `scrollTo` lo
tronca il browser e si resta col basket tagliato a metà in cima. È successo nel
momento in cui il basket è uscito dal flusso — la coda si è accorciata di uno
schermo e il salto atterrava a `-606` invece che a `-1000`.

**`min-height` e non un `padding`**: così è l'altezza a essere garantita, non una
distanza che il contenuto può mangiarsi. **`svh` e non `vh`**: è la stessa unità
di `.sticky` e `.tl-stage`, e su mobile la barra dell'indirizzo non deve
aggiungere una striscia vuota — dove infatti torna a `min-height:0`, perché lì la
coda è nel flusso e non c'è nessun salto da far atterrare.

E la mail ci guadagna: è l'ultima cosa che si legge, e adesso ha il suo schermo.

**Ma i tre link social sono `href="#"`** (`index.html:1679-1680`). LinkedIn,
Instagram e Github non portano da nessuna parte, e sono in una barra fissa che
sta sopra tutta la coda. Servono gli URL. **Due link veri battono tre finti**
(punto aperto §12).

---

## 8. La palette e il testo

**Nessun colore nuovo.** La palette scura della coda è già in pagina
(`index.html:1103-1122`) e resta:

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

**Esito, guardato a schermo:** la B/N **non** sparisce. I numeri dicevano che il
suo nero è più scuro e più freddo del fondo, ed è vero, ma la fotografia è
portata dai *bianchi*, non dai neri: stacca da sola. **Il filo di bordo non
serve e non è stato messo.** Le mensole erano davvero la più forte, e si è
risolto spostandole, non attenuandole.

**Un difetto trovato misurando i contrasti.** Le tag delle competenze nascono
`#554135` — il colore della carta chiara — e su `--ink` fanno circa **1.4:1**: si
vedeva il bordo della pillola e non la parola dentro. Il difetto **precede questa
riscrittura** (le `.sk` erano già sul fondo scuro), ed è corretto qui perché
questa è la sezione che le contiene. Vanno in `--tlbg`, non in `#BFCFFF`:
l'accento resta la lingua di una cosa sola per volta — il titolo Design — e le
tag sono contenuto, non enfasi.

Contrasti **misurati sul rendering**, non ricalcolati a mano: i nove ruoli di
testo della coda vanno da **10.8:1** a **16.53:1**, tutti oltre AA.

### Il testo

Le bozze qui sotto sono **in pagina**, approvate. Restano di lui, come per le
altre sezioni.

Campo:

> Prima del software, la fotografia era il mestiere. Ha educato l'occhio
> all'equilibrio, alla luce, al dettaglio: la sensibilità visiva del design è
> allenata, non innata.

Basket:

> Da sempre gioco a pallacanestro. Visione d'insieme, decisioni rapide sotto
> pressione, e capire il proprio ruolo dentro una squadra — che è poi come si
> lavora su un prodotto.

Posizionamento (`mv1`): il primo paragrafo della bio Media di `doc/bio.md`,
invariato.

**Un punto aperto sul testo del campo.** Nei fotogrammi il segnaposto è di tre
righe, in pagina la frase ne fa sette: è lunga il doppio di quello che l'utente
ha disegnato, e la proporzione fra testo e marrone pulito non è più quella del
907. Accorciandola torna. È una scelta sua, non un difetto da correggere di
iniziativa (§12).

Il paragrafo che la bio Media ripeteva due volte identico — «Ho maturato
esperienza specifica nel software B2B…» — **è stato tolto**: era un errore del
documento, non del sito.

---

## 9. I tre rami

Invariante del progetto: desktop con puntatore, touch/mobile (`mq`, `canHover`,
`@media (max-width:640px)`), `prefers-reduced-motion`. Ogni cosa nuova va pensata
per tutti e tre o si rompe in silenzio.

**Desktop con puntatore.** La coda è fuori dal flusso e la muove l'inseguitore
(§3bis); il campo è intero — palco alto uno schermo, diapositiva, scroll
bloccato — ed è il solo ramo in cui `gzTick()` viene chiamata. Qui e solo qui il
ciclo scrive `--q`, il progresso della sequenza, e trasla il palco che le fa da
pin.

**Touch. Niente diapositiva sul dito.** Il blocco dello scroll è la cosa più
ostile che si possa fare a un telefono: si toglie del tutto e la sezione torna una
colonna che si legge scorrendo. Il palco perde l'altezza fissa e il
posizionamento, le foto tornano nel flusso a 74vw con uno sfalsamento alternato
su `nth-of-type(even)` — una traccia della sparpagliatura del campo che non
chiede nessun conto — e la frase torna prima delle foto con `order:-1`, che è il
suo ordine di lettura naturale. **Nessuna `transform` scritta per frame:** sul
dito il contenuto deve restare incollato, ed è esattamente il tremolio che questo
file ha già combattuto sui progetti.

*L'alternanza funziona perché le foto sono `<figure>` e la frase è un `<div>`:
`nth-of-type` conta per tipo di elemento. Se un giorno la frase diventasse un
`<figure>`, l'alternanza si sposterebbe di uno in silenzio.*

**Touch, la seconda diapositiva.** Il basket torna un blocco in colonna sotto le
foto, dov'è nel DOM: `position:static`, `transform:none`, e la sua griglia a due
colonne torna un blocco solo. Se restasse assoluto starebbe *sopra* le
fotografie, e con `--t` a 1 sarebbe cento viewport più in basso — cioè invisibile.

**`prefers-reduced-motion`.** Lì **non c'è ciclo rAF**: tutto ciò che dipende da
una scrittura per frame va consegnato già fatto dal CSS. `--t` resta al default di
1 — il 907 — e le unità della polvere si riaccendono a mano
(`opacity:1; filter:none`), altrimenti lo stato «prima del primo frame»
resterebbe per sempre e la frase sarebbe invisibile. **Sparisce il movimento, non
la composizione:** chi chiede meno animazione vuole vedere la stessa pagina
ferma, non una pagina diversa — e soprattutto non vuole trovarsi lo scroll
sequestrato da una sequenza che non parte.

**Ma due diapositive senza un gesto che le scambi sono una composizione sola più
una perduta.** Con `--t` a 1 il palco mostra le fotografie e tiene il basket cento
viewport più in basso, e lì nessuno lo riporterà mai. Quindi in reduced-motion il
palco **si srotola**:

```css
.gaze{height:auto}
.gaze-stage{height:auto;padding-top:100vh}
.gaze-say{top:50vh}
.gz-team{position:static;transform:none;padding-top:12vh}
```

Il `padding-top:100vh` spinge in giù il solo contenuto rimasto nel flusso — il
basket — mentre le fotografie, che sono assolute, restano attaccate al bordo alto
del palco: il 907 non cambia di un pixel e il basket gli va sotto. `.gaze-say`
passa da `top:50%` a `50vh` per la stessa ragione: la percentuale si risolve su
uno stage che adesso è alto due schermi, e la frase scenderebbe in mezzo al
basket. **`overflow:clip` resta**: le fotografie sbordano di 14vw a destra, e
senza il ritaglio la pagina prenderebbe una barra orizzontale.

**Il default di `--t` è `1` in CSS, non `0`.** Un default sbagliato qui si
manifesta come «le foto non ci sono», che è il sintomo più costoso da inseguire.

**LA SEQUENZA SI SROTOLA, in tutti e due i rami senza ciclo.** Nessuno scrive
`--q`, e il suo default è `1` — l'ultimo fotogramma, il 916. Lì dentro il
ritratto e la bio sono spenti, e **nessun gesto li riaccenderà mai**: due terzi
della sezione sarebbero contenuto perduto, e uno dei due è la bio, il testo più
importante della coda.

Quindi il palco smette di essere un palco e torna quello che è sotto: una
colonna. Ognuno dei quattro fotogrammi diventa un blocco, nel loro ordine di DOM,
che è già l'ordine del racconto — parola, ritratto, bio, striscia, competenze.
L'altezza fissa se ne va da `.m1` e dal palco: 300vh di corsa senza niente che le
corra dentro sono tre schermi di marrone da scorrere. La striscia resta, ma con
`stroke-dashoffset:0`: qui è un segno, non un gesto.

**Perché il default di `--q` è 1 e non 0.** Zero è uno schermo con la sola parola
in contorno, che sembra una sezione non caricata. Uno è la composizione in cui la
sezione ha detto tutto — ed è da lì che le regole dei due rami riaprono quel che
manca, invece di doverlo costruire da capo.

**Trappola nota, ancora valida:** il guardiano `tlLive` esce dalla funzione per
**tutto** il ramo incolonnato, non solo quando la timeline è lontana. Qualunque
cosa debba girare anche su mobile va scritta **prima** di quel guardiano — è dove
sta già la riga di `.after`.

Verificato con sonda su tutti e tre i rami: in reduced-motion e a 390×844 il
ciclo non scrive `--t` (inline vuoto), `gzLock` resta `false`, le foto sono al
loro posto, la frase è leggibile, **il basket è tornato nel flusso — `static`,
traslazione zero, ritaglio a piena opacità — e sta sotto le fotografie**, e le
competenze sono una colonna sola.

---

## 10. Cosa muore, cosa si ricava, cosa si accoppia

**Morto:**

- le tre etichette numerate `01/02/03` e tutto il CSS che ci girava intorno
- la colonna guida dentro i movimenti, e il filetto `.mv::before` che la
  agganciava: senza etichetta cresceva dal nulla verso il nulla
- l'elenco delle competenze — `.sk`, `.sk h3`, `.sk-tags` e le regole di palette
  che le servivano
- **il nastro**: `.rib3`, `.rb`, `.rb-run`, `.rb-in`, le tre `--v` in linea,
  l'offset `--o` e le due copie `aria-hidden` di ogni fascia
- **`mv3` come sezione**: il basket non è più un `.mv` ma una slide dentro il
  palco, e con lui se ne vanno i due `transition-delay` sfalsati
- `.lens` e `--k`, morti nel giro precedente
- `--p` e la parallasse continua: non sono mai arrivati in `main`
- il paragrafo duplicato nella bio Media di `doc/bio.md`

Cancellati e non commentati: una regola commentata in un file di questa lunghezza
è una regola che qualcuno rimette.

**Si ricava, non si tara.** Se stai per scrivere lo stesso numero due volte,
fermati:

- `af0` dallo **spaziatore**, non da `.after`, che da fissa non ha più un
  `offsetTop` che voglia dire qualcosa
- l'altezza dello spaziatore da `after.offsetHeight`, riletta a ogni `measure()`
- `gazeH` da `offsetHeight` del campo: l'altezza sta in CSS e il JS la legge
- il punto in cui la pagina si ferma, da `scroller.offsetHeight` e `gazeM.top`
- larghezza **e** le due corse di ogni foto dal solo `--s`
- la corsa d'uscita del basket dal palco stesso (`inset:0`, quindi 100vh esatti)
- l'altezza riservata di ogni immagine da `width` e `height` veri sull'`<img>`

**Accoppiati, si cambiano insieme:**

| se tocchi | devi toccare anche |
|---|---|
| la posizione di `.after` (flusso / fisso) | lo spaziatore in `measure()` **e** la riga che la trasla: sono la stessa decisione detta in tre posti |
| l'altezza del campo | niente nel JS: `measure()` la rilegge. Ma dev'essere **uno schermo esatto**, o il salto d'uscita non atterra sul vuoto |
| `--s` di una foto | niente: larghezza e corse escono da lì |
| le posizioni delle foto | il vincolo «il terzo centrale resta alla frase», che con la colonna a 40vw significa stare entro il 30% e oltre il 70% |
| la parola in contorno | il file `chi-sono.svg`: la parola è un tracciato, non più testo. Il vecchio conto sui glifi (`19vw` = otto glifi ≈ 4.9 em) non serve più — ma cambiando parola va rifatto il disegno, non una riga di CSS |
| l'altezza di `.m1` (i 300vh) | niente: la corsa è `offsetHeight` meno il palco, e le finestre sono frazioni di quella. È il numero che decide *quanto scroll* serve a srotolare la sequenza |
| una finestra di `--q` | quella accanto, se le due si accavallano per disegno: `.48` apre il fade out **e** il disegno della striscia, `.74` apre le competenze mentre la striscia sale. I due «nel mentre» sono queste sovrapposizioni |
| la posizione a riposo del ritratto (`--m1-top`) | niente: la corsa d'entrata è `50vh` meno quella, e si ricalcola da sé |
| la fine dell'uscita delle foto (`/.8`) | l'inizio dell'entrata del basket (`/.7` da 2, cioè 1.3): sono lo stesso numero visto da due parti, `1 + .8 = 1.8` e `1.3 + .7 = 2` |
| gli stati del palco (quanti sono) | `gzRotella` (`next > 3`), gli estremi in `gzTick` (`0` e `3`), il riposo in `gzEsci` e nel ramo sbloccato (`2` sotto, `0` sopra), e la finestra della polvere in `gzPaint` |
| l'altezza della chiusura | niente, ma **non può scendere sotto uno schermo**: è la corsa su cui atterra il salto d'uscita del palco |
| l'ordine dei `.mv` nel markup | niente: il campo si cerca per classe, e `mvs` non esiste più |
| `#BFCFFF` | tutta la palette della coda, costruita su quel solo accento |

---

## 11. Gli asset

Cinque webp in `img/`. Le prime quattro sono di `27e6145`; il basket è stato
rifatto dopo, come ritaglio, e sostituisce la versione orizzontale:

| file | dimensioni | peso | |
|---|---|---|---|
| `ph-architettura-bn.webp` | 1280×1600 | 26 KB | |
| `ph-architettura-vetri.webp` | 1600×1280 | 48 KB | |
| `ph-lampione.webp` | 1280×1600 | 197 KB | |
| `ph-mensole.webp` | 1216×1600 | 43 KB | |
| `ph-basket.webp` | 1248×1248 | 54 KB | **con canale alfa**: è un ritaglio, il contorno *è* l'immagine |

Circa 370 KB in tutto, di cui più della metà il solo lampione (è a qualità 90
perché il cielo è una sfumatura larga e a 82 bandava).

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

**Tre webp in `img/` non sono in pagina** — `cfm-controllo-dati`,
`cfm-dettaglio-indagine`, `cfm-simulazione`. Vengono da `main`, sono le altre
schermate del caso CFM elencate in `doc/05-progetti-riconoscimenti.md`, e non
c'entrano con la coda: la riga dei progetti ne usa una sola. Sono materiale di
quel caso, non residui di questa riscrittura, e restano dove sono.

---

## 12. Punti aperti

Chiusi in questo giro:

- ~~Lo scroll della coda tornava secco.~~ La sezione è fuori dal flusso e la
  muove l'inseguitore, come i progetti (§3bis).
- ~~Il campo si vedeva montato prima di partire.~~ Lo stato a riposo dipende da
  che parte lo si guarda (§5).
- ~~Il vuoto prima della prima sezione.~~ Lo occupa la parola di fondo.
- ~~La prima sezione era una lista piatta.~~ Poi era un nastro che scorreva, e
  non andava bene nemmeno quello: adesso sono tre colonne (§4).
- ~~La coda si distaccava dallo sfondo precedente.~~ Lo spaziatore ha il colore
  della coda (§3bis).
- ~~Fotografia e basket erano due sezioni.~~ Sono due diapositive dello stesso
  palco (§5, §6).
- ~~La frase del campo era lunga il doppio del segnaposto.~~ La colonna a 40vw e
  `padding:0` la portano a tre righe.
- ~~I due nodi cromatici, l'esponente della cubica, `.tl-now`, il `TypeError`.~~
  Chiusi nei giri precedenti.

Ancora aperti, e servono all'utente:

1. **I tre URL social.** LinkedIn, Instagram e Github sono `href="#"` da sempre.
   Li dà lui. Due link veri battono tre finti.
2. **La cornice fissa finisce sopra le fotografie.** Nel 907 il «contattami»
   dell'header cade sulla facciata bianca dei vetri e la barra dei social passa
   sopra le foto. **È una scelta di composizione, non un bug**, e non si prende
   di iniziativa: ritagliare il palco sopra e sotto (diventa una banda
   letterbox), un fondo dietro le etichette fisse, oppure lasciar stare.
3. **Le mensole** risultano un'opera di **Alex Pinna**. Se è una sua fotografia
   di un'opera altrui, l'`alt` e l'eventuale didascalia lo devono dire.

---

## 13. Come si verifica

Non c'è test runner e **non va aggiunto**. Le sonde stanno in
`C:\Users\Oliviero\AppData\Local\Temp\lp-portfolio-probe\`, fuori dal repo, e
adesso ci sono davvero: sono state riscritte in questa sessione e non vanno
rifatte da capo.

| file | cosa chiede |
|---|---|
| `cdp.js` | l'imbracatura: Chrome headless via CDP, zero dipendenze. **Raccoglie le eccezioni** |
| `syntax.js` | estrae lo script in linea e ci passa `node --check` |
| `p-frames.js` | la **resa**: le sei pose del palco — vuoto, entrata, fotografie, scambio, basket, vuoto — e le fotografa |
| `p-slide.js` | il **comportamento**: blocco, gesti, rigiocata, moderazione |
| `p-rami.js` | i tre rami e la pagina intera |
| `p-struct.py` | annidamento, id, alt, classi orfane nelle due direzioni |
| `p-contrast.js` | contrasti **misurati sul rendering** |
| `p-coda.js` | il redesign: la coda che insegue, la parola in parallasse, le cinque colonne e la loro scaletta, lo stato del campo, il ritardo del ritaglio |
| `p-giunta.js` | la **giunta**: che fra la linea del tempo e la coda non si scopra mai il body |
| `p-seq.js` | la **sequenza** dei fotogrammi 913-916: le pose, i due «nel mentre», e che a srotolarla sia davvero lo scroll |
| `p-regress.js` | confronta con `main`: l'allagamento non si deve muovere |
| `shot-coda.js`, `shot-mobile.js`, `shot-parola.js` | le pose, per guardarle |

Resa e comportamento sono **due sonde separate apposta**: mescolate rendevano
ambigua ogni riga rossa.

Non c'è né Playwright né Puppeteer (`npm i` fallisce in questo ambiente), ma c'è
il binario di Chromium in
`~/AppData/Local/ms-playwright/chromium-1228/chrome-win64/chrome.exe`, e Node 25
ha `WebSocket` e `fetch` globali: bastano ~50 righe senza dipendenze.

### Quattro trappole pagate care

1. **Convergere, non calcolare.** Per posizionarsi a un punto preciso non basta
   uno `scrollTo`: la pagina ha uno scroll inseguito e `lockHero()` cambia
   l'altezza dello scroller a metà strada. Si legge dove si è, si corregge, si
   rilegge.
2. **Le eccezioni nel ciclo rAF non si vedono da nessuna misura.** Un
   `ReferenceError` dentro `applyProjects()` uccideva in silenzio tutto quello
   che veniva dopo, header scuro compreso, e il sintomo sembrava «quella cosa non
   si muove». Per questo `cdp.js` ascolta `Runtime.exceptionThrown`.
3. **`node --check` non vede la zona morta.** Una `const` usata prima della
   propria dichiarazione è sintatticamente valida e rompe solo al caricamento.
4. **`getComputedStyle` su una custom property restituisce i token non risolti**,
   non il numero. Si verifica l'effetto — luminosità, opacità, spostamento — non
   la stringa.

**Le sonde delle pose rimettono la coda nel flusso** prima di fotografarla: da
fissa la trasla il ciclo di ventimila pixel, e il ritaglio cadrebbe su un punto
che non contiene niente.

**Una sonda che scatta troppo presto mente.** La prima stesura di `p-coda.js`
leggeva `--w 0.117` dove doveva essere zero e dava per rotto quel che era solo in
viaggio: mezzo secondo non basta a far arrivare la molla. Si converge, sempre.

**`cdp.js` non filtra più niente.** Filtrava `/is not a function/` per non urlare
a ogni giro sul `TypeError` del §14; adesso che è corretto il filtro se n'è
andato, e con lui il rischio di nascondere un'intera classe di errori veri. Non
va rimesso.

Regole che restano:

1. **Sintassi** dopo ogni gruppo di modifiche.
2. **Struttura**: annidamento, id duplicati, classi CSS ↔ markup **nelle due
   direzioni**.
3. **Chiamare il codice di produzione**, non riscriverlo.
4. **Contrasto misurato sul rendering**, non ricalcolato a mano.
5. **Altezza del documento** prima e dopo il `load`, per confermare che le
   immagini non spostano niente.
6. **Almeno due proporzioni di finestra** (1920×1000 e 1280×1024): diversi conti
   di questa pagina hanno rami che scattano solo sulle finestre più alte che
   larghe.
7. **Giudizio percettivo: solo l'utente**, ricaricando. **Dichiarare sempre cosa
   è stato misurato e cosa no.**

---

## 14. Un difetto preesistente, corretto

In fondo a `index.html` c'era:

```js
requestAnimationFrame(function loop(now){ ... })(introStart)
```

Il risultato di `requestAnimationFrame` è un numero, e veniva chiamato: un
`TypeError` a ogni caricamento. Non rompeva niente — il ciclo era già registrato
ed è l'ultima istruzione del `.then()` — ma sporcava la console, e `introStart`
non era mai servito come argomento: la funzione riceve `now` da rAF e il
cronometro se lo scrive da sé al primo frame.

Era preesistente a questo lavoro, identico su `main`, ed è stato segnalato due
volte senza correggerlo. **Adesso è corretto**: la chiamata è sparita, la riga è
`});`. Verificato con la sonda, a filtro rimosso: nessuna eccezione in tutta la
corsa della pagina.
