# La coda scura — riprogetto delle sezioni dopo la curva

Data: 2026-09-02. Stato: **da approvare**. Nessuna riga di `index.html` toccata.

Riguarda tutto quello che sta dopo la curva a 90°: la sezione `.after` (i tre
movimenti) e la chiusura. Sostituisce il disegno consegnato in `df7497f`.

---

## 1. Perché si rifà

I quattro difetti dichiarati dall'utente, tutti e quattro:

- i tre movimenti hanno la stessa griglia, lo stesso ingresso, lo stesso ritmo:
  si legge come un documento impaginato bene, non come tre momenti
- non succede niente — la guida scende dritta, il contenuto compare e basta
- la chiusura è piatta: stellina e mail, e sotto tre link social morti
- il contenuto è debole: bio lunga, competenze da CV, hobby che sembrano
  riempitivo

E una cosa che il sito **non dice** e che sta in `doc/bio.md`, versione lunga:

> Prima del software, la fotografia è stata la mia professione.

È il tratto che lo distingue da qualsiasi altro software designer. Oggi il sito
lo degrada a hobby, ultima riga dell'ultimo movimento.

**Calibrazione decisa dall'utente:** la fotografia è un **dettaglio forte, non
il titolo**. Il sito non deve sembrare il portfolio di un fotografo.

---

## 2. L'idea che regge tutto: la linea si apre

Idea dell'utente. La barra marrone che scende dopo la curva, scorrendo, si
allarga fino a diventare **il fondo scuro delle sezioni successive**.

**Cosa sostituisce.** L'invariante argomentata in `df7497f` era *«la linea deve
morire sulla mail»*: la carriera arriva a oggi e si ferma sul recapito. Muore
qui. Al suo posto: **la linea non finisce, si apre e diventa la stanza in cui
stai.** La timeline arriva a oggi, gira, e poi oggi è tutto lo schermo.

Va cancellata la logica che la implementa, non aggirata, e il commento va
riscritto: altrimenti fra un mese quel codice mente.

**Dove si apre** (scelta dell'utente): **subito dopo la curva**. Tutta la coda
è su fondo scuro, una palette sola, nessun confine chiaro/scuro sotto un blocco
di testo.

**Dove si vede crescere**: nel vuoto fra l'atterraggio della curva e il primo
movimento — quello schermo scarso in cui non c'è ancora niente da leggere. Il
gesto non compete con nessun testo.

**Come si apre** (scelta dell'utente): **dai due lati**, come una linea che
ingrassa. Sta al 30% della larghezza, quindi il bordo sinistro raggiunge il
margine molto prima del destro. È la fisica onesta di una linea che si ispessisce.

**A cuneo, non a rettangolo.** Un rettangolo che si allarga ha un bordo alto
orizzontale, e un bordo alto orizzontale si legge come *«è entrato un
pannello»*, non come *«la linea ingrassa»*. Si apre a cuneo: sottile in alto
dove la linea arriva, pieno in basso. `clip-path`, costo nullo.

**Il colore del fondo è `#554135`** — il colore esatto della guida, già nel
file. Il fondo *è* la linea, quindi ha il colore della linea. Nessun colore
nuovo, e il marrone caldo ha continuità con la carta; il nero non l'avrebbe.

**Implementazione.** `.after-rail` è già un elemento assoluto con una
`transform`. Diventa `scaleX(k) scaleY(d)` invece della sola `scaleY`: colore
pieno, solo compositor, nessun elemento aggiunto, nessun ridisegno. `k` lo
scrive lo stesso ciclo che oggi scrive `--d`.

**La sua altezza cambia mestiere.** Oggi è `--railH`, ricavata dal centro della
stellina di chiusura, perché la linea doveva morire lì. Diventa **l'altezza
piena di `.after`**: il fondo deve arrivare in fondo alla sezione. Quel che
muore è la *derivazione dalla stellina*, non l'altezza. `.after` conserva il suo
`background:var(--tlbg)`, che è la carta ancora visibile mentre il cuneo si apre.

---

## 3. La palette scura

Vincolo che ci si dà: **nessun valore nuovo.** La palette scura è fatta di
colori già nel file, con i ruoli scambiati.

| ruolo | oggi | nella coda |
|---|---|---|
| fondo | `--tlbg` `#FFFEF8` | `#554135` (il colore della guida) |
| testo | `#484848` | `--tlbg` `#FFFEF8` |
| accento, micro-etichette, corsivi, stellina | `--blue` `#174FFE` | `#BFCFFF` |
| filetti | `#BFCFFF` / `#B2998C` | `#BFCFFF` |

**Perché l'azzurro cambia.** `#174FFE` su `#554135` sono due colori scuri:
contrasto intorno a 1.1:1, illeggibile. `#BFCFFF` è già nel file (è il colore
dei filetti), stessa famiglia di tinta, ~8:1 sul marrone.

*Contrasti calcolati, non misurati a schermo. Vanno verificati sul rendering
vero prima di dire che tornano.*

**Peso tipografico.** `.mv-lead` è DM Sans **300**. Su fondo scuro un peso 300
chiaro si sfilaccia. Nella coda va a **400**: stesso carattere, stessa scala,
solo il peso.

---

## 4. Sezione 1 — chi sono

**La colonna di sinistra prende il numero, grande.** Oggi `01 — posizionamento`
sono 12px di mono azzurro dentro un terzo di schermo vuoto — il commento del CSS
attuale confessa già il problema. Diventa un `01` in Space Grotesk alto quanto
un paragrafo, allineato a destra contro la spina, in `#BFCFFF` tenuto basso
(~20% di opacità: architettura, non contenuto). Sotto, piccola, la parola
`posizionamento` in mono dov'è adesso.

I tre numeri che scendono danno alla coda una spina dorsale visiva.
*Alternativa scartabile a vista: a filo vuoto (`-webkit-text-stroke`), più
freddo.*

**Il testo dimezza.** Oggi ci sono i due paragrafi della bio "Media" (~100
parole). Su fondo scuro 100 parole sono fatica. Passa alla bio **"Breve"** di
`doc/bio.md` (~50 parole).

**Una riga a sé, unico acceso della schermata**, in `#BFCFFF` corsivo:

> *Prima del software, la fotografia è stata la mia professione.*

Apre il credito che la sezione delle foto incassa. Senza, gli scatti dopo sono
un vezzo.

**La riga sulla pallacanestro non entra qui.** Era stata spostata in questa
sezione quando le foto non c'erano; con la foto del basket in chiusura la riga
sparisce del tutto (vedi §7).

**Non si tocca** l'ingresso: opacità + 8px, scalettato per figli. Il gesto
grosso è stato speso venti centimetri più su, col fondo che si apre.

---

## 5. Sezione 2 — cosa so fare

**Il difetto è nel contenuto.** Cinque righe con dentro elenchi separati da
filetti *è* un CV: impaginato bene, ma è una tabella, e una tabella non dice
**perché** sai quelle cose.

**Diventa un paragrafo solo**, in cui gli strumenti sono grandi e le parole di
raccordo piccole: si legge come una frase, si scansiona come una lista. Bozza —
il testo definitivo lo corregge l'utente:

> Disegno in **Figma** — wireframe, prototipi, design system — e verifico con la
> **user research**. Quando serve costruisco: **React**, **HTML**, **CSS**,
> **JS**. Sotto ci sono **Python**, **Go**, **Java**, **Kotlin**, **SQL**: non è
> il mio mestiere, *ma è quello che mi permette di sapere cosa sto chiedendo*. E
> lavoro con **Claude Code**, **Figma Maker**, **Antigravity**.

Quel corsivo è la tesi che `doc/bio.md` §4 chiede e che oggi è affidata a una
gerarchia di corpi che il lettore deve indovinare: i linguaggi back-end sono
profondità tecnica al servizio del ruolo, non un vanto. Detto, funziona;
soltanto disegnato, no.

**Come si vede.** Nomi degli strumenti in `--tlbg` pieno e pesante, raccordi
nello stesso colore spento (~45%). Un colore, due pesi. I filetti `#BFCFFF` fra
le righe **spariscono**: non ci sono più righe da separare.

**Il movimento si sposta, non si aggiunge.** Oggi ci sono `transition-delay`
scalettati sui cinque figli di `.mv-body`; con un paragrafo solo non hanno più
su cosa applicarsi. Gli stessi ritardi vanno sui `<b>` degli strumenti, che si
accendono in sequenza dentro la frase. Nessun JS nuovo.

**Il prezzo, dichiarato:** un recruiter di fretta scansiona una tabella meglio
di una frase. Mitigazione: i nomi degli strumenti sono la cosa più grande della
riga. È un compromesso vero. Se pesa, la versione a righe torna e cambia solo la
palette.

---

## 6. Sezione 3 — le foto

Selezione decisa dall'utente: **quattro scatti**, mensole comprese.

**La sezione esce dalla griglia.** Fin qui tutto sta nel `--spine 1fr`. Qui no:
una fotografia non vuole una colonna, vuole la pagina — ed è il momento in cui
la coda smette di essere un documento. Il `03` e l'etichetta restano nella
griglia, in cima; da lì in giù le foto vanno per conto loro.

**Una frase sola in cima**, grande — la tesi: l'occhio non è innato, è allenato.
Testo definitivo da correggere con l'utente. Poi **niente più prosa**.

### L'ordine: le foto si scaldano scendendo

Le quattro non sono un corpo di lavoro solo — architettura, architettura, cielo,
interni sono quattro generi. **A tenerle insieme non è il soggetto, è la
temperatura:**

| # | file | mondo | temperatura |
|---|---|---|---|
| 1 | `_DSC0248-Modifica.jpg` | architettura, bianco e nero | freddo assoluto |
| 2 | `_DSC0304.jpg` | spigolo, vetri turchesi | colore freddo |
| 3 | `_DSC7957.jpg` | lampione, cielo rosa e blu | il caldo entra |
| 4 | `mensoleAlexPinna_#1.4.png` | still life, rosa e beige | caldo pieno |

L'ultima **appartiene già alla stanza**: il suo rosa-beige sta sul `#554135` del
fondo senza litigare. La sequenza ha una direzione, e la direzione non è il
genere. Giustifica anche il bianco e nero: non è "una in B/N fra le altre", è
**l'inizio della scala**.

### Le didascalie sono i dati di scatto

Non «Fotografia — educa l'occhio all'equilibrio compositivo». Sotto ogni
immagine, in mono `#BFCFFF` piccolo. I dati di scatto sono la lingua di chi il
mestiere l'ha fatto: dicono *«questo era il mio lavoro»* senza dichiararlo, e
sono l'unica cosa nella coda che un altro software designer non può copiare.

Letti dall'EXIF dei file, non inventati:

```
architettura · 26mm · f/10 · 1/125 · ISO 100 · 2019      (D5300, 17 apr 2019)
architettura · 62mm · f/10 · 1/125 · ISO 100 · 2019      (D5300, 17 apr 2019)
               32mm · f/2.8 · 1/800 · ISO 125 · 2020      (D7200, 31 ott 2020)
still life · Alex Pinna
```

**Due file non hanno EXIF** (`mensoleAlexPinna`, PNG esportato, e la foto del
basket). Regola: **la didascalia è quello che il file sa davvero.** Dove i
numeri ci sono, ci sono; dove non ci sono, resta il soggetto — non si inventa un
numero verosimile. Formato identico, lunghezza no: una riga corta in fondo alla
sequenza chiude invece di ripetere.

Le due di architettura sono **lo stesso giorno con le stesse impostazioni**:
le loro didascalie differiscono solo per la focale. È vero, e va lasciato vero.

### In pagina

Alternate, nessuna griglia: 1 verticale a destra → 2 orizzontale quasi piena →
3 verticale a sinistra → 4 verticale a destra. Ognuna **~70vh, non una
schermata piena a testa**: quattro foto pesano circa tre schermate su una coda
che deve restare calma. Nessun pin, nessuna corsa orizzontale.

### Da verificare a schermo, non deciso qui

Due nodi cromatici reali, e la manopola in caso di fallimento è il colore del
fondo:

1. il nero quasi puro del `_DSC0248` sul marrone caldo può diventare **un buco
   freddo**
2. il turchese del `_DSC0304` è quasi complementare del marrone e **può
   litigarci**

Possono anche essere esattamente lo stacco che serve. Si guarda, non si decide
a priori.

---

## 7. La chiusura

**Il ritratto è la foto del basket.** L'utente non ha dato un ritratto; la foto
del basket è **l'unica immagine in cui c'è lui**. Va nella colonna che ha
portato `01`, `02`, `03`: la sequenza dei numeri finisce e **nello stesso slot
compare una faccia**. Non serve annunciarlo, si vede. Lui che tira, concentrato,
accanto a «scrivimi» — più forte di una posa.

Verticale, allineata a destra contro la spina, come i numeri. **Senza didascalia
di dati**: non avendo EXIF non si afferma niente che non si possa reggere.

*Provenienza, dichiarata dall'utente: scatto reale, passato da ChatGPT solo per
ritagliare/pulire. Il soggetto e la scena sono veri. Se il file sorgente
originale salta fuori, si usa quello.*

**La riga di testo sulla pallacanestro sparisce.** C'è l'immagine; il testo
sarebbe la didascalia di una cosa che si vede.

**La mail diventa la cosa più grande della pagina.** Oggi è
`clamp(22px, 2.2vw, 34px)` — più piccola dei titoli delle competenze, ed è
l'ultima cosa che il sito dice. Sale di due o tre gradini. Testo **`scrivimi`**
invece di «contattami», che è già nell'header e lì fa il suo mestiere. Sotto,
l'indirizzo in mono `#BFCFFF`.

**I social smettono di essere una barra e diventano la chiusura.** `.social` è
`position:fixed`, `#212121`, in fondo: sul marrone **sparirebbe comunque**,
scuro su scuro. Quindi invece di ricolorarla:

- quando arriva la chiusura la barra fissa **svanisce** — stessa opacità che il
  ciclo già scrive su `#ui-social` durante l'intro, un termine in più nella
  formula che c'è, nessun macchinario nuovo
- **i tre link ricompaiono dentro la chiusura**, grandi, sotto la mail

L'interfaccia passa la mano al contenuto, e i link smettono di essere una
striscia sotto la fine: sono la seconda riga della fine.

**La stellina chiude.** L'ordine verticale della chiusura è: la foto nella
colonna con accanto mail, indirizzo e i tre social; **poi**, sotto tutto,
la stellina — piccola, centrata sulla spina, in `#BFCFFF`. Non è più accanto
alla mail come oggi: è il punto in fondo alla pagina, il marchio del sito che
chiude il documento.

---

## 8. I tre rami

Invariante del progetto: desktop con puntatore, touch/mobile (`mq`, `canHover`),
`prefers-reduced-motion`. Ogni cosa nuova va pensata per tutti e tre o si rompe
in silenzio.

- **Desktop.** Come sopra. `k` (l'apertura) e `d` (la discesa) li scrive il
  ciclo che c'è già.
- **Mobile** (`max-width:640px`). `--spine` è `24px`: la fascia si apre dalla
  stessa x, molto più a sinistra, e riempie lo schermo prima. Il numero grande e
  la foto della chiusura non stanno in 24px: nel ramo incolonnato il numero
  torna piccolo sopra il testo e la foto va a piena larghezza. Le quattro foto
  restano in colonna.
- **`prefers-reduced-motion`.** Lì **non c'è ciclo rAF**: tutto ciò che dipende
  da una scrittura per frame va consegnato già fatto dal CSS. Quindi **il fondo
  è scuro dall'inizio della coda**, senza apertura. È un ramo che vede una
  pagina diversa, ed è accettabile — ma va scritto nel commento, non scoperto.

**Trappola nota:** il guardiano `tlLive` esce dalla funzione per **tutto** il
ramo incolonnato, non solo quando la timeline è lontana. Qualunque cosa debba
girare anche su mobile va scritta **prima** di quel guardiano — è dove sta già
la guida di `.after`.

---

## 9. Cosa muore, cosa si ricava, cosa si accoppia

**Muore:**

- l'invariante *«la linea muore sulla mail»* e la misura `--railH` presa sul
  centro della stellina (`measure()`): la linea non finisce più da nessuna parte
- i filetti `#BFCFFF` fra le righe delle competenze (`.sk`) e dello sguardo
  (`.lens`): non ci sono più righe
- `--blue` come accento **nella sola coda**; resta ovunque altrove
- il paragrafo lungo della bio "Media", i due blocchi `.lens`, la riga sulla
  pallacanestro

**Si ricava, non si tara** — la disciplina del file, che ha già evitato quattro
derive. Se stai per scrivere lo stesso numero due volte, fermati:

- `--spine` resta scritto in pixel da `measure()` a partire da dove atterra la
  curva, e `AF_SPINE` (.30) resta lui a dire alla coda della traccia quanto
  allungarsi
- l'apertura `k` si ricava dalla larghezza del viewport e da `--spine`, non è un
  numero scritto a mano: deve bastare a coprire il lato **più lontano**
- l'altezza delle foto si riserva con `aspect-ratio` dalle dimensioni reali dei
  file, non a occhio

**Accoppiati, si cambiano insieme:**

| se tocchi | devi toccare anche |
|---|---|
| `AF_SPINE` (JS) | `--spine:30vw` in `.after`, e sappi che cambia la **lunghezza della timeline** |
| lo spessore della guida | `.after-rail` **e** `.tl-elbow path`, o la giuntura si vede |
| il colore del fondo `#554135` | il colore della guida e del gomito: sono lo stesso colore per costruzione |
| `.after` fondo scuro | `.social` (`#212121` su scuro sparisce) e il `padding-bottom` che le fa spazio |

---

## 10. Gli asset

Sorgenti copiate in `doc/immagini/` (erano collegamenti Windows in `img/`,
risolti):

| file | dimensioni | destinazione |
|---|---|---|
| `_DSC0248-Modifica.jpg` | 2450×3062 | `img/ph-architettura-bn.webp` |
| `_DSC0304.jpg` | 3086×2469 | `img/ph-architettura-vetri.webp` |
| `_DSC7957.jpg` | 3159×3949 | `img/ph-lampione.webp` |
| `mensoleAlexPinna_#1.4.png` | 2752×3621 | `img/ph-mensole.webp` |
| `basket-chatgpt.png` | 1322×1190 | `img/ph-basket.webp` |

**Il `#` nel nome va tolto**: in un URL apre un frammento. È il motivo per cui i
nomi di destinazione sono tutti nuovi.

Conversione: webp, lato lungo ~1600px, qualità ~82 — la stessa scala delle webp
di progetto già in `img/`.

**Trappola da disinnescare.** Tutta la geometria di questo sito è **misurata**:
`measure()` legge altezze in pixel e ci costruisce sopra scroll, pin e corsa
della timeline. Immagini che arrivano dopo il `load` **cambiano l'altezza del
documento a misure già prese**, e il sintomo non sembra affatto colpa delle
foto. Quindi ogni immagine nasce con `aspect-ratio` e dimensioni dichiarate: lo
spazio è riservato prima che il file arrivi. Non è ottimizzazione, è la
condizione perché la coda non rompa le sezioni prima.

**Da ripulire:** i cinque file in `img/` non tracciati (quattro `.lnk` e il PNG
originale del basket) vanno tolti a conversione fatta.

---

## 11. Punti aperti

1. **I tre URL social** — LinkedIn, Instagram, Github sono `href="#"` da sempre.
   In una chiusura costruita attorno a loro, tre link morti sono peggio che non
   averli. L'utente li darà. **Due link veri battono tre finti.**
2. **Dati e anno delle mensole** — se l'utente ricorda macchina e anno, la riga
   si completa. Altrimenti resta `still life · Alex Pinna`.
3. **I due nodi cromatici** del §6, da guardare a schermo.
4. **Il testo definitivo** della bio breve, del paragrafo competenze e della
   frase delle foto: bozze qui, li corregge l'utente.

---

## 12. Come si verifica

Non c'è test runner e **non va aggiunto**. Gli script stanno in
`C:\Users\Oliviero\AppData\Local\Temp\lp-portfolio-probe\`, fuori dal repo.

1. **Sintassi** — estrai lo script inline fra l'ultimo `<script>` e `</script>`,
   `node --check`, dopo ogni gruppo di modifiche.
2. **Struttura** — `html.parser` per l'annidamento, confronto classi CSS ↔
   classi nel markup nelle due direzioni, id duplicati.
3. **Geometria** — `probe.py` con un `p-*.js` in `--dump-dom`. Il modo giusto è
   **chiamare il codice di produzione**: `scrollTo(0,y); scrollCur =
   scrollTarget = scrollY; sync(); applyProjects();` e poi leggere.
   Da aggiornare: `p-final.js` (che oggi verifica `--spine`, la guida che si
   disegna e i movimenti che si accendono) e `p-end.js`.
4. **Contrasto** — i rapporti del §3 vanno **misurati** sul rendering, non
   ricalcolati a mano.
5. **Aspetto** — in headless lo screenshot parte dall'origine della pagina: lo
   scroll della finestra non si vede. Per fotografare una sezione bisogna
   nascondere gli altri figli di `body` e portarla in cima con un margine
   negativo (`shot-af.py` lo fa già).
6. **Altezza del documento** — prima e dopo, per confermare che le immagini con
   `aspect-ratio` non spostano niente a `load` avvenuto.
7. **Giudizio percettivo** — solo l'utente, ricaricando. **Dichiarare sempre
   cosa è stato misurato e cosa no.**
