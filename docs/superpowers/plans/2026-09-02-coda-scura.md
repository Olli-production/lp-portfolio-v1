# La coda scura — Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** La barra marrone che scende dopo la curva si apre fino a diventare il fondo scuro della coda del sito, e le quattro sezioni che ci stanno sopra vengono riscritte per quel fondo.

**Architecture:** Un solo file, `index.html` (CSS e JS inline, ~3300 righe). Nessuna build, nessuna dipendenza runtime, nessun file nuovo. Il fondo è lo stesso elemento `.after-rail` che c'è già, con una `scaleX` in più e un `clip-path` a cuneo. Le sezioni cambiano markup e palette; il ciclo di scroll esistente guadagna una riga.

**Tech Stack:** HTML/CSS/JS vanilla. Python + Pillow per convertire le foto. Node per `--check` e per la matematica. Chrome headless per le sonde.

**Spec:** `docs/superpowers/specs/2026-09-02-coda-scura-design.md` — va letta insieme a questo piano.

## Global Constraints

- **Nessun file nuovo nel repo, nessuna dipendenza, nessun test runner.** Le sonde stanno in `C:\Users\Oliviero\AppData\Local\Temp\lp-portfolio-probe\`, fuori dal repo.
- **Tre rami, sempre:** desktop con puntatore, touch/mobile (`mq`, `canHover`, `@media (max-width:640px)`), `prefers-reduced-motion`. In reduced-motion **non c'è ciclo rAF**: quel che dipende da una scrittura per frame va consegnato da CSS o da `measure()`.
- **Si ricava, non si tara.** Se stai per scrivere lo stesso numero due volte, fermati.
- **Nessun colore nuovo.** La palette scura usa valori già nel file: fondo `#554135`, testo `--tlbg` `#FFFEF8`, accento `#BFCFFF`.
- **Commenti in italiano, estesi**, con la motivazione e cosa NON fare. Messaggi di commit in inglese, argomentati.
- **Non inventare dati.** Dove un dato non esiste va una frase, mai un numero verosimile.
- **Ancoraggi unici:** per ogni sostituzione via script, `assert s.count(anchor) == 1`. I patch si scrivono in un file `.py` con Write e si lanciano da lì — mai virgolette inverse dentro `python -c`, mai heredoc con accenti.
- **Il guardiano `tlLive`** (in `applyProjects`, dopo `const tlp = ...`) esce dalla funzione per tutto il ramo incolonnato: quel che deve girare anche su mobile va scritto **prima**, dov'è già la guida di `.after`.

---

## File Structure

| File | Responsabilità | Fase |
|---|---|---|
| `index.html` | tutto: markup, CSS, JS | 2-7 |
| `img/ph-*.webp` | le cinque immagini nuove | 1 |
| `doc/immagini/*` | i sorgenti, già copiati, non serviti al browser | 1 |
| `doc/04-bio-esperienze.md`, `doc/bio.md` | i testi che il sito copia | 3, 4 |

Non si splitta `index.html`: è la struttura del progetto ed è deliberata (nessuna build). L'ordine delle fasi è scelto perché **ogni fase lascia la pagina giudicabile a schermo**, e la fase 2 fa giudicare il fondo *prima* di riscrivere il contenuto — se il marrone non funziona, si scopre con due ore di lavoro dentro, non con dieci.

---

## Task 1: Le immagini

**Files:**
- Create: `img/ph-architettura-bn.webp`, `img/ph-architettura-vetri.webp`, `img/ph-lampione.webp`, `img/ph-mensole.webp`, `img/ph-basket.webp`
- Delete: i quattro `.lnk` e `ChatGPT Image 2 set 2026, 00_33_44.png` in `img/`
- Script: `%TEMP%\lp-portfolio-probe\conv-foto.py`

**Interfaces:**
- Produces: i cinque percorsi `img/ph-*.webp` e le loro **dimensioni reali in pixel**, che le fasi 5 e 6 usano per scrivere `aspect-ratio`. Vanno annotate qui nel piano a conversione fatta.

- [ ] **Step 1: Scrivi lo script di conversione**

`%TEMP%\lp-portfolio-probe\conv-foto.py`:

```python
from PIL import Image
import os
SRC = r'D:\progetti\design\brand-personale\v5\doc\immagini'
DST = r'D:\progetti\design\brand-personale\v5\img'
# il '#' nel nome delle mensole aprirebbe un frammento in un URL: i nomi di
# destinazione sono tutti nuovi anche per quello
MAP = {
    '_DSC0248-Modifica.jpg':      'ph-architettura-bn.webp',
    '_DSC0304.jpg':               'ph-architettura-vetri.webp',
    '_DSC7957.jpg':               'ph-lampione.webp',
    'mensoleAlexPinna_#1.4.png':  'ph-mensole.webp',
    'basket-chatgpt.png':         'ph-basket.webp',
}
LONG = 1600   # stessa scala delle webp di progetto gia' in img/
for src, dst in MAP.items():
    im = Image.open(os.path.join(SRC, src)).convert('RGB')
    im.thumbnail((LONG, LONG), Image.LANCZOS)
    im.save(os.path.join(DST, dst), 'webp', quality=82, method=6)
    print(f'{dst}  {im.size[0]}x{im.size[1]}  '
          f'{os.path.getsize(os.path.join(DST, dst))//1024} KB')
```

- [ ] **Step 2: Lancialo e annota le dimensioni**

Run: `python "%TEMP%\lp-portfolio-probe\conv-foto.py"`
Expected: cinque righe con nome, dimensioni, peso. **Trascrivi le dimensioni nella tabella qui sotto** — le fasi 5 e 6 le leggono da qui, non le rimisurano.

| file | larghezza × altezza | peso |
|---|---|---|
| `ph-architettura-bn.webp` | _da riempire_ | |
| `ph-architettura-vetri.webp` | _da riempire_ | |
| `ph-lampione.webp` | _da riempire_ | |
| `ph-mensole.webp` | _da riempire_ | |
| `ph-basket.webp` | _da riempire_ | |

- [ ] **Step 3: Verifica che siano leggibili e non stravolte**

Run:
```bash
python -c "from PIL import Image; import glob; [print(f, Image.open(f).size, Image.open(f).mode) for f in sorted(glob.glob('img/ph-*.webp'))]"
```
Expected: cinque file, `RGB`, lato lungo 1600 (tranne il basket, che parte da 1322 e non va ingrandito — `thumbnail` non ingrandisce mai, quindi resta 1322×1190).

- [ ] **Step 4: Guarda le cinque webp**

Aprile e confrontale a occhio con i sorgenti in `doc/immagini/`. Cerchi due cose: **banding nei cieli** del lampione (è il file più a rischio, ha una sfumatura larga) e **artefatti sui bordi netti** del bianco e nero. Se il lampione banda, per quel solo file `quality=90`.

- [ ] **Step 5: Togli i file di appoggio da `img/`**

```bash
cd "D:/progetti/design/brand-personale/v5"
rm img/*.lnk "img/ChatGPT Image 2 set 2026, 00_33_44.png"
git status --short
```
Expected: in `img/` restano solo le webp; niente `.lnk`, niente PNG. I sorgenti in `doc/immagini/` restano.

- [ ] **Step 6: Commit**

```bash
git add img/ph-*.webp doc/immagini/
git commit -m "Photographs: five of his own frames, converted for the page"
```

---

## Task 2: Il fondo che si apre

Questa fase **non tocca il contenuto**: cambia solo il fondo e ricolora quel che c'è già. Serve a fargli giudicare il marrone prima che ci sia costruito sopra qualcosa.

**Files:**
- Modify: `index.html:1046-1050` (`.after-rail`), `1031-1037` (`.after`), `1076-1105` (colori del testo), `1114-1136` (filetti), `1143-1163` (chiusura), `1341-1350` (reduced-motion), `2386-2400` (`measure()`), `2831-2843` (il ciclo)

**Interfaces:**
- Consumes: niente.
- Produces: la variabile CSS `--k` su `#afterRail` (apertura, 1 = linea sottile), `--wedge` su `.after` (dove il cuneo raggiunge la larghezza piena, in %), e in JS `afKMax` (apertura massima, ricavata) e `afterH` (altezza di `.after`). Le fasi 3-6 assumono il fondo scuro già in piedi.

- [ ] **Step 1: Sonda di partenza — fotografa lo stato di adesso**

Prima di toccare: `python "%TEMP%\lp-portfolio-probe\shot-af.py"`. Tieni le immagini da parte. Servono per il confronto e perché se il marrone non piace si torna a queste.

- [ ] **Step 2: `.after-rail` diventa il fondo**

Sostituisci il blocco a `index.html:1046-1050`. Il commento vecchio va riscritto per intero: dice che la linea muore sulla mail, e non è più vero.

```css
/* LA GUIDA, CHE E' ANCHE IL FONDO. Nasce spessa 4px come la barra e il gomito
   — e' la stessa linea, e va letta come tale — e scorrendo si APRE fino a
   coprire lo schermo: da li' in giu' e' lei la carta su cui sta la coda del
   sito. La timeline arriva a oggi, gira, e poi oggi e' tutto lo schermo.
   MUORE QUI L'INVARIANTE DI df7497f («la linea deve morire sulla mail»): non
   finisce piu' da nessuna parte, quindi l'altezza e' quella piena della
   sezione e non piu' --railH ricavata dal centro della stellina.
   NIENTE scaleY: il gesto del disegnarsi scendendo e' SOSTITUITO
   dall'apertura, non affiancato. Due gesti nello stesso mezzo secondo si
   annullavano.
   IL CUNEO NON E' DECORAZIONE: un rettangolo che si allarga ha un bordo alto
   orizzontale, e un bordo alto orizzontale si legge come «e' entrato un
   pannello», non come «la linea ingrassa». Con la punta in alto la giuntura
   col gomito resta una linea sola che si apre scendendo.
   Le percentuali del clip-path stanno nello spazio LOCALE dell'elemento, prima
   della transform: il cuneo si scala insieme a lui e resta un cuneo.
   Niente border-radius: a scaleX 300 un raggio di 2px diventerebbe 600. */
.after-rail{
  position:absolute;left:var(--spine);top:0;height:100%;width:4px;
  margin-left:-2px;background:#554135;
  transform:scaleX(var(--k,1));transform-origin:center top;
  clip-path:polygon(50% 0, 100% var(--wedge,4%), 100% 100%, 0 100%, 0 var(--wedge,4%));
}
```

- [ ] **Step 3: `.after` perde il fondo chiaro solo dove serve, e prende la palette**

A `index.html:1031-1037`, `.after` **conserva** `background:var(--tlbg)`: è la carta che si vede ancora mentre il cuneo si apre. Aggiungi sotto il blocco una fascia di regole che ricolora il contenuto esistente:

```css
/* LA PALETTE DELLA CODA. Nessun valore nuovo: sono i colori che il file ha
   gia', con i ruoli scambiati. #174FFE su #554135 sono due colori scuri —
   contrasto ~1.1:1, illeggibile — quindi nella sola coda l'azzurro acceso
   lascia il posto a #BFCFFF, che e' gia' il colore dei filetti.
   Il peso 300 di DM Sans si sfilaccia sul fondo scuro: qui va 400. Stesso
   carattere, stessa scala, solo il peso — senza, il testo sembra sbiadito e
   dai la colpa allo schermo. */
.after .mv-tag,.after .lens em{color:#BFCFFF}
.after .mv-lead{color:var(--tlbg);font-weight:400}
.after .mv-lead i{color:#BFCFFF}
.after .sk h3,.after .lens p,.after .after-addr{color:var(--tlbg)}
.after .after-mail{color:#BFCFFF}
.after .after-star{fill:#BFCFFF}
.after .sk,.after .sk:last-child,
.after .lens,.after .lens:last-child{border-color:#BFCFFF}
.after .mv::before{background:#BFCFFF}
```

- [ ] **Step 4: `measure()` ricava l'apertura invece di tararla**

A `index.html:2386-2400`, sostituisci il blocco che calcola `afRailH`. Il commento vecchio (quello sulla stellina e su `offsetTop` che non esiste sugli SVG) va **conservato in parte**: la trappola dell'SVG resta vera e va ricordata, anche se questa misura non serve più.

```js
  /* .after: dove comincia, quanto e' alta, e quanto deve aprirsi la guida.
     NON si misura piu' il centro della stellina: la linea non deve piu'
     fermarsi li' (vedi il CSS di .after-rail). La stellina resta il marchio in
     fondo, e basta.
     (Resta valida la trappola che quella misura ci aveva insegnato: offsetTop e
     offsetHeight NON esistono sugli elementi SVG, li' tornano undefined e i
     conti diventano NaN senza sollevare niente. Sugli <svg> si misura con
     getBoundingClientRect.) */
  af0 = after.offsetTop - (scroller.offsetHeight - innerHeight);
  afterH = after.offsetHeight || 1;
  /* L'APERTURA MASSIMA SI RICAVA, NON SI SCEGLIE. La fascia parte larga 4px e
     cresce dal proprio centro, che sta a --spine: ogni lato guadagna 2*k px, e
     deve bastare a coprire il lato PIU' LONTANO. Con la guida al 30% il lato
     lungo e' sempre quello destro, ma scriverlo con un max() lo tiene vero
     anche se AF_SPINE cambia o se il ramo incolonnato la porta a 24px. */
  const spinePx = parseFloat(getComputedStyle(after).getPropertyValue('--spine')) || 0;
  afKMax = Math.max(spinePx, innerWidth - spinePx) / 2;
  /* dove il cuneo raggiunge la larghezza piena: UNO SCHERMO sotto l'inizio
     della sezione, cioe' lo stesso vuoto in cui l'apertura avviene. In % perche'
     il clip-path lavora in coordinate locali. */
  after.style.setProperty('--wedge', (100 * innerHeight / afterH).toFixed(2) + '%');
  /* reduced-motion non ha un ciclo per frame: il fondo va consegnato gia'
     aperto da qui, altrimenti quel ramo vede una linea sottile e basta.
     La variabile e' `reduce` (index.html:1922), non noMotion. */
  if (reduce) afterRail.style.setProperty('--k', afKMax.toFixed(2));
  mvs.forEach(m => m.top = af0 + m.el.offsetTop);
```

Dichiara le due variabili nuove accanto alle altre a `index.html:2133`:

```js
let af0 = 0, afterH = 1, afKMax = 1;   // dove comincia .after, quanto e' alta, quanto si apre
```

E togli `afRailH` da quella riga e da ogni suo uso.

- [ ] **Step 5: Il ciclo scrive l'apertura al posto della discesa**

A `index.html:2838-2839`, sostituisci la scrittura di `--d` con quella di `--k`. **Resta prima del guardiano `tlLive`**, per la ragione che il commento lì già spiega.

```js
  /* ---------- il fondo delle sezioni dopo la curva ----------
     Sta PRIMA del guardiano qui sotto, e non e' un dettaglio: quel guardiano
     esce dalla funzione quando la linea del tempo e' lontana e per tutto il
     ramo incolonnato, dove pero' queste sezioni ci sono e il fondo deve
     aprirsi lo stesso.
     L'apertura si consuma in UNO SCHERMO a partire da quando la sezione entra:
     e' il vuoto fra l'atterraggio della curva e il primo movimento, l'unico
     punto della coda in cui non c'e' niente da leggere. Il gesto non compete
     con nessun testo, ed e' per questo che sta li'.
     out3 e non lineare: la fascia parte decisa e arriva morbida, come ogni
     altra apertura del sito. */
  const afOpen = out3(clamp((over - af0 + innerHeight * AF_READ) / (innerHeight * AF_OPEN)));
  afterRail.style.setProperty('--k', (1 + (afKMax - 1) * afOpen).toFixed(2));
```

E accanto ad `AF_READ`/`AF_ON` a `index.html:2137`:

```js
const AF_OPEN = 1;    // in quanti schermi la fascia passa da linea a fondo
```

- [ ] **Step 6: Il ramo reduced-motion**

A `index.html:1341-1350`, dove c'è già `.after-rail{transform:none}`, sostituisci: `transform:none` rimetterebbe la fascia a 4px. L'apertura la scrive `measure()` (Step 4), quindi qui basta non contraddirla:

```css
  /* la fascia e' gia' aperta: --k lo scrive measure(), che gira anche qui.
     transform:none la richiuderebbe a 4px. */
  .after-rail{transform:scaleX(var(--k,1))}
```

- [ ] **Step 7: `node --check`**

```bash
cd "D:/progetti/design/brand-personale/v5"
python - <<'PY'
import re
s = open('index.html', encoding='utf-8').read()
js = s.rsplit('<script>', 1)[1].rsplit('</script>', 1)[0]
open('/tmp/inline.js', 'w', encoding='utf-8').write(js)
PY
node --check /tmp/inline.js
```
Expected: nessun output. Un errore qui **spegne la pagina**: non andare oltre.

- [ ] **Step 8: Sonda — l'apertura si ricava e arriva a coprire**

Scrivi `%TEMP%\lp-portfolio-probe\p-ground.js` e lancialo con `probe.py --dump-dom`. Chiama il codice di produzione, non una copia:

```js
const out = { errors: [] };
window.onerror = e => out.errors.push(String(e));
const after = document.getElementById('after');
const rail  = document.getElementById('afterRail');
const at = y => { scrollTo(0, y); scrollCur = scrollTarget = scrollY; sync(); applyProjects(); };

// prima che la sezione entri: linea sottile
at(af0 + scroller.offsetHeight - innerHeight - innerHeight);
out.kPrima = getComputedStyle(rail).getPropertyValue('--k').trim();
// uno schermo e mezzo dentro: aperta
at(af0 + scroller.offsetHeight - innerHeight + innerHeight * 1.5);
out.kDopo  = getComputedStyle(rail).getPropertyValue('--k').trim();
out.kMax   = afKMax;
// copre davvero? mezza larghezza scalata contro il lato piu' lontano
const spine = parseFloat(getComputedStyle(after).getPropertyValue('--spine'));
out.copre   = 2 * afKMax >= Math.max(spine, innerWidth - spine);
out.wedge   = getComputedStyle(after).getPropertyValue('--wedge').trim();
out.railRect = rail.getBoundingClientRect().width.toFixed(0);
return out;
```

Expected:
- `errors` vuoto
- `kPrima` ≈ `1`, `kDopo` ≈ `kMax`
- `copre` `true`
- `wedge` una percentuale piccola ma non zero (~3-6% su una coda di qualche schermata)
- `railRect` ≈ `4 * kMax`, cioè almeno la larghezza del viewport

- [ ] **Step 9: Screenshot — il cuneo e il fondo**

`python "%TEMP%\lp-portfolio-probe\shot-elbow.py"` e `shot-af.py`. Guarda:
- la giuntura col gomito è **una linea sola che si apre**, non un pannello con un bordo alto orizzontale
- il testo vecchio è leggibile sul marrone
- da nessuna parte resta una striscia di carta `#FFFEF8` fra il fondo e il bordo dello schermo

- [ ] **Step 10: Verifica i contrasti sul rendering vero**

I rapporti della spec §3 sono **calcolati, non misurati**. Nella stessa sonda, leggi i colori calcolati e verificali:

```js
const px = el => getComputedStyle(el).color;
out.colori = {
  lead: px(document.querySelector('.after .mv-lead')),
  tag:  px(document.querySelector('.after .mv-tag')),
  fondo: getComputedStyle(document.getElementById('afterRail')).backgroundColor,
};
```
Poi il rapporto in Node con la formula WCAG. Expected: **testo ≥ 4.5:1, accenti ≥ 3:1**. Se `#BFCFFF` su `#554135` non arriva a 4.5, il testo grande può stare a 3:1 ma le micro-etichette mono no — in quel caso si schiarisce l'accento, non si scurisce il fondo.

- [ ] **Step 11: Fallo giudicare**

Consegna un resoconto per punti, digli di ricaricare, e **dichiara cosa hai misurato e cosa no**: il ritmo mentre si scorre e gli `:hover` qui non sono verificabili. Se il marrone non funziona la manopola è il colore del fondo — ma è accoppiato al colore della guida e del gomito, che sono lo stesso colore per costruzione.

- [ ] **Step 12: Commit (solo dopo il suo ok)**

```bash
git add index.html
git commit -m "The line stops ending and becomes the ground"
```

---

## Task 3: Sezione 1 — chi sono

**Files:**
- Modify: `index.html:1817-1826` (markup `#mv1`), `1076-1093` (`.mv-tag` e l'ingresso), `1279-1293` (mobile)
- Modify: `doc/bio.md` e `doc/04-bio-esperienze.md` — il paragrafo duplicato nella bio Media

**Interfaces:**
- Consumes: il fondo scuro della fase 2.
- Produces: la classe `.mv-num` (il numero grande) e il pattern `<b class="mv-num">01</b>` dentro `.mv-tag`, che le fasi 4 e 5 riusano per `02` e `03`.

- [ ] **Step 1: Il numero grande**

Aggiungi dopo `.mv-tag` (`index.html:1082`):

```css
/* IL NUMERO. La colonna di sinistra e' larga un terzo di schermo e finora ci
   stavano tre parole di mono: il commento di .after lo confessava gia'. Il
   numero la riempie con ARCHITETTURA, non con contenuto — per questo sta basso
   di opacita' e non compete con niente. I tre numeri che scendono danno alla
   coda una spina dorsale visiva.
   line-height:.8 e non 1: un numerale non ha discendenti, e con l'interlinea
   piena l'etichetta sotto sembrerebbe scollata. */
.mv-num{
  display:block;font-family:var(--grotesk);font-weight:700;
  font-size:clamp(48px,7vw,120px);line-height:.8;letter-spacing:-.04em;
  color:#BFCFFF;opacity:.2;margin-bottom:.25em;
}
```

- [ ] **Step 2: Il markup del movimento 1**

Sostituisci `index.html:1817-1826`. Testo: la bio **Breve** di `doc/bio.md`, non la Media.

```html
  <article class="mv" id="mv1">
    <h2 class="mv-tag"><b class="mv-num">01</b>posizionamento</h2>
    <div class="mv-body">
      <p class="mv-lead">Sono Oliviero Petrucci, Software Designer in Zucchetti. Unisco design dell&rsquo;esperienza e ingegneria del software per costruire prodotti digitali scalabili e intuitivi: progetto le interfacce e, quando serve, ne realizzo il front-end per renderle concrete. Mi sono occupato di software B2B per la sostenibilit&agrave;, dove rendere leggibili dati complessi &egrave; ci&ograve; che guida le decisioni.</p>
      <!-- l'unica cosa accesa della schermata, e non e' un vezzo: e' la riga
           che apre il credito che la sezione delle foto poi incassa. Senza,
           gli scatti dopo sono decorazione. Sta in doc/bio.md, versione lunga,
           e il sito non l'ha mai detta. -->
      <p class="mv-lead mv-turn"><i>Prima del software, la fotografia &egrave; stata la mia professione.</i></p>
    </div>
  </article>
```

```css
/* la riga della svolta: stessa scala del lead, ma staccata — e' un capoverso
   che vale da solo */
.mv-turn{margin-top:1.6em}
```

- [ ] **Step 3: Il mobile**

A `index.html:1285`, `.mv-tag` su mobile è allineata a sinistra e il numero grande non ci sta accanto a 24px di spina. Aggiungi nel blocco `@media (max-width:640px)`:

```css
  /* incolonnato il numero non ha una colonna in cui stare: torna piccolo e
     resta sulla stessa riga dell'etichetta, come una numerazione di paragrafo */
  .mv-num{display:inline;font-size:inherit;opacity:.6;margin:0 .5em 0 0}
```

- [ ] **Step 4: Il paragrafo duplicato nei documenti**

`doc/bio.md` e `doc/04-bio-esperienze.md` §1 ripetono **due volte** il paragrafo che comincia con «Ho maturato esperienza specifica…» dentro la bio Media. Il sito ne copiava uno solo, ma il documento è sbagliato da settimane. Togli la seconda occorrenza in **entrambi** i file.

Run per confermare:
```bash
grep -c "Ho maturato esperienza specifica" doc/bio.md doc/04-bio-esperienze.md
```
Expected: `1` per ciascuno.

- [ ] **Step 5: `node --check` e la sonda di struttura**

`node --check` come al Task 2 Step 7, più il confronto classi CSS ↔ markup nelle due direzioni e gli id duplicati. Expected: `.mv-num` e `.mv-turn` esistono in entrambi, nessun id doppio.

- [ ] **Step 6: Screenshot**

`shot-af.py`. Guarda che il numero **non competa** col testo: se lo leggi prima del paragrafo, l'opacità `.2` è troppo alta. È la manopola.

- [ ] **Step 7: Commit**

```bash
git add index.html doc/bio.md doc/04-bio-esperienze.md
git commit -m "First movement: a number fills the empty column, and the bio finally says the thing"
```

---

## Task 4: Sezione 2 — cosa so fare

**Files:**
- Modify: `index.html:1828-1840` (markup `#mv2`), `1108-1122` (le regole `.sk`), `1090-1093` (i ritardi scalettati), `1288` (mobile), `358` (`.mv.on .sk-tags li`)

**Interfaces:**
- Consumes: `.mv-num` dalla fase 3.
- Produces: `.skl` (il paragrafo) e `.skl b` (i nomi degli strumenti). Le regole `.sk`, `.sk h3`, `.sk-tags` spariscono e nessuna fase successiva le usa.

- [ ] **Step 1: Il markup**

Sostituisci `index.html:1828-1840`. **Il testo è una bozza: fattelo correggere prima di committare.**

```html
  <article class="mv" id="mv2">
    <h2 class="mv-tag"><b class="mv-num">02</b>competenze</h2>
    <div class="mv-body">
      <!-- NON e' piu' un elenco, ed e' il punto. Cinque righe di nomi separate
           da filetti sono un CV impaginato bene: dicono cosa sai, mai perche'.
           doc/bio.md §4 chiede che i linguaggi di back-end restino profondita'
           tecnica al servizio del ruolo — finora quella tesi era affidata a una
           gerarchia di corpi che il lettore doveva indovinare. Detta, funziona.
           Il prezzo e' dichiarato: una frase si scansiona peggio di una
           tabella. I nomi degli strumenti sono la cosa piu' grande della riga
           apposta, cosi' l'occhio li prende comunque. -->
      <p class="skl">Disegno in <b>Figma</b> &mdash; wireframe, prototipi, design system &mdash; e verifico con la <b>user research</b>. Quando serve costruisco: <b>React</b>, <b>HTML</b>, <b>CSS</b>, <b>JS</b>. Sotto ci sono <b>Python</b>, <b>Go</b>, <b>Java</b>, <b>Kotlin</b>, <b>SQL</b>: non &egrave; il mio mestiere, <i>ma &egrave; quello che mi permette di sapere cosa sto chiedendo</i>. E lavoro con <b>Claude Code</b>, <b>Figma Maker</b>, <b>Antigravity</b>.</p>
    </div>
  </article>
```

- [ ] **Step 2: Il CSS — un colore, due pesi**

Sostituisci il blocco `.sk` a `index.html:1108-1122` (commento compreso):

```css
/* LE COMPETENZE, che non sono piu' un elenco. Un colore solo e due pesi: gli
   strumenti pieni, il raccordo spento. Si legge come una frase, si scansiona
   come una lista.
   Il chiaro sta nel COLORE e non in opacity: opacity su un genitore se la
   portano dietro i figli, e qui i <b> devono restare pieni dentro un raccordo
   spento. E' la stessa trappola gia' pagata altrove nel file.
   I filetti #BFCFFF fra le righe spariscono con le righe: non c'e' piu' niente
   da separare. */
.skl{
  font-family:var(--sans);font-variation-settings:"opsz" 40;font-weight:400;
  font-size:clamp(20px,2.05vw,33px);line-height:1.42;letter-spacing:-.5px;
  color:rgba(255,254,248,.45);max-width:40ch;
}
.skl b{font-weight:700;color:var(--tlbg)}
.skl i{font-style:italic;color:#BFCFFF}
/* l'ingresso scalettato non si aggiunge: si SPOSTA. I ritardi stavano sui
   cinque figli di .mv-body, e con un paragrafo solo non avevano piu' su cosa
   applicarsi. Vanno sugli strumenti, che si accendono in sequenza dentro la
   frase. Nessun JS: sono ritardi CSS su .on, come per gli eventi della linea
   del tempo. */
.skl b{opacity:0;transition:opacity .5s cubic-bezier(.16,1,.3,1)}
.mv.on .skl b{opacity:1}
.mv.on .skl b:nth-of-type(n+2){transition-delay:calc(.05s * var(--i,0))}
```

Il `--i` per strumento va scritto inline nel markup dello Step 1: `<b style="--i:1">`, `<b style="--i:2">`… fino a 15. **Aggiungilo mentre scrivi il markup**, non dopo.

- [ ] **Step 3: Togli le regole morte**

Cancella `index.html:358` (`.mv.on .sk-tags li{border-color:#BFCFFF}`), la regola `.sk-tags` ovunque sia, e `index.html:1288` (`.sk h3` nel blocco mobile). Cerca `sk-tags` e `\.sk\b` in tutto il file e verifica che non resti niente.

Run:
```bash
grep -n "sk-tags\|\.sk\b\|\.sk{" index.html
```
Expected: nessun risultato.

- [ ] **Step 4: `node --check` + sonda di struttura**

Come sopra. In più: il confronto classi CSS ↔ markup **nelle due direzioni** è il controllo che qui conta di più — è la fase che cancella più regole.

- [ ] **Step 5: Sonda — gli strumenti si accendono in sequenza**

```js
const at = y => { scrollTo(0, y); scrollCur = scrollTarget = scrollY; sync(); applyProjects(); };
at(document.getElementById('mv2').offsetTop + scroller.offsetHeight - innerHeight - innerHeight * .5);
const bs = [...document.querySelectorAll('.skl b')];
return {
  acceso: document.getElementById('mv2').classList.contains('on'),
  n: bs.length,
  ritardi: bs.map(b => getComputedStyle(b).transitionDelay),
};
```
Expected: `acceso` `true`, `n` = 15, i ritardi crescenti dal secondo in poi.
**Nota:** sotto virtual time le transizioni CSS non avanzano mai, quindi `opacity` letta qui è il valore di **partenza**. Non è un difetto: si verifica il ritardo, non l'opacità.

- [ ] **Step 6: Fagli correggere il testo, poi commit**

```bash
git add index.html
git commit -m "Second movement: the skills stop listing and start arguing"
```

---

## Task 5: Sezione 3 — le foto

**Files:**
- Modify: `index.html:1842-1849` (markup `#mv3`), `1124-1136` (le regole `.lens`), `1289` (mobile)

**Interfaces:**
- Consumes: `.mv-num`, i cinque `img/ph-*.webp` e le dimensioni annotate al Task 1 Step 2.
- Produces: `.ph` (la figura), `.ph img`, `.ph figcaption`. La fase 6 non li usa.

- [ ] **Step 1: Il markup**

Sostituisci `index.html:1842-1849`. Le dimensioni negli `width`/`height` vanno prese dalla tabella del Task 1: **se non l'hai riempita, torna a riempirla** — non stimarle.

```html
  <article class="mv mv-ph" id="mv3">
    <h2 class="mv-tag"><b class="mv-num">03</b>lo sguardo</h2>
    <div class="mv-body">
      <p class="ph-claim">L&rsquo;occhio non &egrave; innato. <i>&Egrave; allenato.</i></p>
    </div>
    <!-- LE FOTO ESCONO DALLA GRIGLIA, ed e' il punto: fin qui tutto sta nel
         --spine 1fr, ma una fotografia non vuole una colonna, vuole la pagina.
         E' il momento in cui la coda smette di essere un documento.
         L'ORDINE NON E' IL SOGGETTO, E' LA TEMPERATURA: architettura B/N,
         architettura fredda, cielo che si scalda, still life caldo. Quattro
         generi diversi non sono un corpo di lavoro, ma una scala di temperatura
         si', e l'ultima foto appartiene gia' alla stanza — il suo rosa-beige sta
         sul #554135 del fondo senza litigare. E' anche quel che giustifica il
         bianco e nero: non e' «una in B/N fra le altre», e' l'inizio della scala.
         LE DIDASCALIE SONO I DATI DI SCATTO, letti dall'EXIF dei file, mai
         inventati. Due file non ne hanno: quelle righe dicono solo cio' che il
         file sa davvero. Non si riempie un buco con un numero verosimile.
         width/height dichiarati NON sono ottimizzazione: tutta la geometria del
         sito e' misurata da measure(), e immagini che arrivano dopo il load
         cambiano l'altezza del documento a misure gia' prese. Il sintomo non
         sembrerebbe affatto colpa delle foto. -->
    <figure class="ph ph-r">
      <img src="img/ph-architettura-bn.webp" width="__W__" height="__H__" loading="lazy" decoding="async" alt="Dettaglio di una facciata: solai bianchi che tagliano l&rsquo;ombra, in bianco e nero.">
      <figcaption>architettura &middot; 26mm &middot; f/10 &middot; 1/125 &middot; ISO 100 &middot; 2019</figcaption>
    </figure>
    <figure class="ph ph-wide">
      <img src="img/ph-architettura-vetri.webp" width="__W__" height="__H__" loading="lazy" decoding="async" alt="Lo spigolo di un edificio: vetrate turchesi da un lato, facciata bianca a lame dall&rsquo;altro.">
      <figcaption>architettura &middot; 62mm &middot; f/10 &middot; 1/125 &middot; ISO 100 &middot; 2019</figcaption>
    </figure>
    <figure class="ph ph-l">
      <img src="img/ph-lampione.webp" width="__W__" height="__H__" loading="lazy" decoding="async" alt="Un lampione a sei bracci acceso contro un cielo al tramonto, rosa e blu.">
      <figcaption>32mm &middot; f/2.8 &middot; 1/800 &middot; ISO 125 &middot; 2020</figcaption>
    </figure>
    <figure class="ph ph-r">
      <img src="img/ph-mensole.webp" width="__W__" height="__H__" loading="lazy" decoding="async" alt="Interno: due mensole nere triangolari a parete sopra un divano rosa.">
      <figcaption>still life &middot; Alex Pinna</figcaption>
    </figure>
  </article>
```

- [ ] **Step 2: Il CSS**

Sostituisci il blocco `.lens` a `index.html:1124-1136`:

```css
/* LA FRASE, e poi silenzio. Una sola, grande: la tesi. Da qui in giu' non c'e'
   piu' prosa, ci sono le fotografie e i loro dati. */
.ph-claim{
  font-family:var(--grotesk);font-weight:500;
  font-size:clamp(24px,2.6vw,44px);line-height:1.18;letter-spacing:-.6px;
  color:var(--tlbg);max-width:20ch;
}
.ph-claim i{font-style:italic;color:#BFCFFF}
/* LE FIGURE. ~70vh e non una schermata piena a testa: quattro foto pesano cosi'
   circa tre schermate invece di quattro, su una coda che deve restare calma.
   Nessun pin, nessuna corsa orizzontale — di la' dalla curva lo scroll e'
   tornato quello del browser, ed era la richiesta.
   aspect-ratio dalle dimensioni vere del file: lo spazio e' riservato prima che
   l'immagine arrivi (vedi il commento nel markup). */
.ph{margin-top:14vh;display:flex;flex-direction:column;gap:12px}
.ph img{display:block;width:100%;height:auto;max-height:70vh;object-fit:contain}
.ph figcaption{
  font-family:var(--mono);font-weight:500;font-size:12px;letter-spacing:.06em;
  color:#BFCFFF;
}
/* alternate, non in griglia: e' una SEQUENZA, come si monta un portfolio */
.ph-r{margin-left:auto;margin-right:10vw;width:min(38vw,520px)}
.ph-l{margin-left:10vw;margin-right:auto;width:min(38vw,520px)}
.ph-wide{margin-left:auto;margin-right:auto;width:min(74vw,1100px)}
/* la sezione delle foto esce dalla griglia a due colonne: le figure sono figlie
   dirette di .mv e non di .mv-body, e non devono incolonnarsi con l'etichetta */
.mv-ph{display:block}
.mv-ph .mv-tag{display:block;text-align:left;padding:0 0 0 30vw}
```

Nota: `.mv-ph` passa a `display:block`, quindi `.mv::before` (il filetto che cresce dalla spina) qui non ha più la griglia sotto. Verifica a schermo se il filetto resta al posto giusto; se no, in questa sola sezione si toglie.

- [ ] **Step 3: Il mobile**

```css
  .ph-r,.ph-l,.ph-wide{width:auto;margin-left:24px;margin-right:24px}
  .ph img{max-height:none}
  .ph-claim{font-size:24px;max-width:none}
  .mv-ph .mv-tag{padding:0 24px 0 calc(24px + 20px)}
```

- [ ] **Step 4: Togli le regole morte**

```bash
grep -n "\.lens" index.html
```
Expected: nessun risultato.

- [ ] **Step 5: `node --check` + struttura + alt text**

Oltre ai soliti: ogni `<img>` ha un `alt` non vuoto, un `width` e un `height` numerici. Nessun `__W__` rimasto.

```bash
grep -n "__W__\|__H__" index.html
```
Expected: nessun risultato. **Se ne trovi, non hai riempito la tabella del Task 1.**

- [ ] **Step 6: Sonda — l'altezza del documento non si muove al `load`**

È la verifica che conta più di tutte in questa fase.

```js
const h1 = document.body.firstElementChild.offsetHeight;
await Promise.all([...document.images].map(i => i.complete ? 0 : i.decode().catch(() => 0)));
const h2 = document.body.firstElementChild.offsetHeight;
return { h1, h2, uguali: h1 === h2 };
```
Expected: `uguali` `true`. Se è `false`, un `aspect-ratio` o un `width/height` è sbagliato e **tutta la geometria del sito si sfasa** — pin, corsa della timeline, curva. Non andare oltre.
**Nota:** `scrollHeight` conta anche le transform dei figli: si misura `firstElementChild.offsetHeight`.

- [ ] **Step 7: Screenshot delle quattro foto**

`shot-af.py`. Qui si guardano i **due nodi cromatici** che la spec §6 lascia aperti apposta:
1. il nero quasi puro del bianco e nero sul marrone caldo — buco freddo o stacco voluto?
2. il turchese, che è quasi complementare del marrone — litiga?

Se falliscono, la manopola è **il colore del fondo**, che è accoppiato al colore della guida e del gomito.

- [ ] **Step 8: Commit**

```bash
git add index.html
git commit -m "Third movement: four of his photographs, ordered by temperature"
```

---

## Task 6: La chiusura

**Files:**
- Modify: `index.html:1849-1856` (markup `.after-end`), `1139-1163` (le regole), `1433-1435` (`.social`), `158-165` (`.social` CSS), `2545` (l'opacità di `#ui-social`), `1290-1293` (mobile)

**Interfaces:**
- Consumes: `img/ph-basket.webp` e le sue dimensioni, il fondo della fase 2.
- Produces: niente — è l'ultima sezione.

- [ ] **Step 1: Il markup**

Sostituisci `index.html:1849-1856`:

```html
  <div class="after-end">
    <!-- IL RITRATTO CHE NON E' UN RITRATTO. Non c'era una foto in posa, e va
         benissimo cosi': questa e' l'unica immagine in cui c'e' lui, e sta
         nella colonna che fin qui ha portato 01, 02, 03. La sequenza dei numeri
         finisce e nello stesso slot compare una faccia. Non serve annunciarlo.
         SENZA DIDASCALIA DI DATI: il file non ha EXIF (e' uno scatto vero
         ripulito con ChatGPT, dichiarato dall'utente), quindi non si afferma
         niente che non si possa reggere. -->
    <img class="after-me" src="img/ph-basket.webp" width="__W__" height="__H__" loading="lazy" decoding="async" alt="Oliviero Petrucci in campo, in procinto di tirare.">
    <div class="after-out">
      <a class="after-mail" href="mailto:olivieropetrucci@gmail.com">scrivimi <span>&#8594;</span></a>
      <p class="after-addr">olivieropetrucci@gmail.com</p>
      <!-- I SOCIAL SMETTONO DI ESSERE UNA BARRA. Quella fissa in fondo e'
           #212121 e sul marrone sparirebbe comunque: invece di ricolorarla,
           svanisce (vedi il JS) e i link tornano QUI, grandi, sotto la mail.
           L'interfaccia passa la mano al contenuto, e i tre link smettono di
           essere una striscia sotto la fine: sono la seconda riga della fine. -->
      <ul class="after-soc">
        <li><a href="#">LinkedIn</a></li>
        <li><a href="#">Instagram</a></li>
        <li><a href="#">Github</a></li>
      </ul>
    </div>
  </div>
  <!-- il marchio come punto finale: dopo tutto, non piu' accanto alla mail -->
  <svg class="after-star" id="afterStar" viewBox="0 0 541 541" aria-hidden="true"><use href="#starPath"/></svg>
```

**Gli `href="#"` restano finché non dà gli URL.** Sono un punto aperto dichiarato nella spec §11. Non inventarli, e **non committare la fase come finita** senza averglieli richiesti: in una chiusura costruita attorno a loro, tre link morti sono peggio che non averli. Due link veri battono tre finti.

- [ ] **Step 2: Il CSS**

Sostituisci `index.html:1139-1163`:

```css
/* LA CHIUSURA. La colonna di sinistra e' larga --spine, la stessa dei numeri:
   la foto ci si appoggia a destra e basta. */
.after-end{
  display:grid;grid-template-columns:var(--spine) 1fr;
  align-items:center;padding-top:26vh;
}
.after-me{
  display:block;justify-self:end;width:min(22vw,300px);height:auto;
  margin-right:2.6vw;
}
.after-out{padding-left:2.6vw;padding-right:10vw;display:flex;flex-direction:column;gap:10px;align-items:flex-start}
/* la cosa piu' grande della pagina: e' l'ultima che il sito dice. Prima era
   clamp(22px,2.2vw,34px), cioe' piu' piccola dei titoli delle competenze. */
.after-mail{
  color:#BFCFFF;text-decoration:none;
  display:inline-flex;align-items:center;gap:.5rem;
  font-family:var(--sans);font-variation-settings:"opsz" 14;font-weight:300;
  font-size:clamp(40px,6vw,110px);line-height:1;letter-spacing:-2px;
}
.after-mail span{transition:transform .35s cubic-bezier(.16,1,.3,1)}
.after-mail:hover span{transform:translateX(.35rem)}
.after-mail:focus-visible{outline:1px solid #BFCFFF;outline-offset:4px}
.after-addr{
  font-family:var(--mono);font-weight:500;font-size:12px;letter-spacing:.06em;
  color:var(--tlbg);
}
.after-soc{list-style:none;display:flex;gap:2.4vw;margin-top:1.4em}
.after-soc a{
  color:var(--tlbg);text-decoration:none;
  font-family:var(--sans);font-variation-settings:"opsz" 14;font-weight:300;
  font-size:clamp(18px,1.8vw,26px);letter-spacing:-.4px;
}
.after-soc a:hover{color:#BFCFFF}
.after-soc a:focus-visible{outline:1px solid #BFCFFF;outline-offset:4px}
/* il punto in fondo alla pagina, centrato sulla spina come tutto il resto */
.after-star{
  display:block;width:clamp(28px,3.4vw,52px);height:auto;overflow:visible;
  margin:18vh 0 0 var(--spine);translate:-50% 0;fill:#BFCFFF;
}
```

- [ ] **Step 3: La barra fissa passa la mano**

A `index.html:2545` c'è `document.getElementById('ui-social').style.opacity = ui;`. `ui` è l'opacità dell'interfaccia durante l'intro.

**Attenzione allo scope, verificato:** quella riga sta dentro `apply(scroll)` (`index.html:2424`), **non** dentro `applyProjects()` (`2646`). Quindi `over` lì **non esiste** — è locale ad `applyProjects` (`const over = overCur`, riga 2647). Quello che si legge da entrambe è `overCur`, che è di modulo (riga 2644), e così `af0` e `afterH`.

```js
  /* la barra fissa dei social passa la mano alla chiusura: li' i tre link
     tornano dentro il contenuto, grandi, sotto la mail (vedi .after-soc). Su
     fondo #554135 questa barra sparirebbe comunque, essendo #212121 — quindi
     invece di ricolorarla si toglie di mezzo, ed e' anche il gesto giusto:
     l'interfaccia si ritira quando la pagina e' arrivata.
     Un termine in piu' nella formula che c'era, nessun macchinario nuovo.
     overCur e non over: questa funzione e' apply(), e `over` e' locale ad
     applyProjects(). */
  const socOut = 1 - clamp((overCur - (af0 + afterH - innerHeight * 1.6)) / (innerHeight * .5));
  document.getElementById('ui-social').style.opacity = (ui * socOut).toFixed(3);
```

**Prima di scrivere, verifica dove gira `apply()`.** Se in `prefers-reduced-motion` non viene chiamata (lì non c'è ciclo rAF), in quel ramo la barra resterebbe accesa e `#212121` su `#554135` — invisibile, ma anche non cliccabile per chi ci arriva. In quel caso la spegne una regola CSS dentro il blocco `@media (prefers-reduced-motion:reduce)` (`index.html:1296`), non altro JS:

```css
  /* apply() non gira qui: la barra la spegne il CSS, e i social veri stanno
     nella chiusura */
  .social{display:none}
```

Run per decidere: `grep -n "apply(" index.html` e guarda i punti di chiamata.

- [ ] **Step 4: Il mobile**

```css
  .after-end{grid-template-columns:1fr;padding-top:16vh;gap:18px}
  .after-me{justify-self:start;width:min(60vw,280px);margin-left:24px;margin-right:0}
  .after-out{padding-left:calc(24px + 20px)}
  .after-mail{font-size:40px;letter-spacing:-1px}
  .after-soc{flex-wrap:wrap;gap:16px}
  .after-star{margin-left:24px}
```

- [ ] **Step 5: `node --check` + struttura**

Come sopra. In più: nessun `__W__`/`__H__` rimasto, e `#afterStar` esiste ancora — `measure()` non lo usa più per l'altezza (fase 2) ma la sonda `p-end.js` potrebbe cercarlo.

- [ ] **Step 6: Sonda — la barra svanisce e la chiusura arriva**

```js
const at = y => { scrollTo(0, y); scrollCur = scrollTarget = scrollY; sync(); applyProjects(); };
const doc = document.body.firstElementChild.offsetHeight;
at(doc - innerHeight);
return {
  barra: getComputedStyle(document.getElementById('ui-social')).opacity,
  mailPx: getComputedStyle(document.querySelector('.after-mail')).fontSize,
  soc: [...document.querySelectorAll('.after-soc a')].map(a => a.getAttribute('href')),
  stella: document.getElementById('afterStar').getBoundingClientRect().top < innerHeight,
};
```
Expected: `barra` ≈ `0`, `mailPx` la più grande della pagina, `stella` `true`.
`soc` dirà `["#","#","#"]` finché non dà gli URL: **è il promemoria, non un successo.**

- [ ] **Step 7: Screenshot + richiedi gli URL**

`shot-af.py` sulla chiusura. Poi chiediglieli.

- [ ] **Step 8: Commit**

```bash
git add index.html
git commit -m "The ending gets a face, and the interface hands off to it"
```

---

## Task 7: I tre rami, e la pagina intera

Le fasi 2-6 hanno già scritto il proprio mobile e il proprio reduced-motion. Questa fase **verifica**, non costruisce.

**Files:**
- Modify: `index.html` solo se la verifica trova qualcosa
- Modify: `docs/superpowers/specs/2026-09-02-coda-scura-design.md` — chiudere i punti aperti risolti

- [ ] **Step 1: Reduced-motion**

Lancia la sonda con `prefers-reduced-motion: reduce` forzato. Expected:
- il fondo è **già aperto** (`--k` ≈ `afKMax`) senza che nessun ciclo abbia girato
- nessun errore in console
- i movimenti sono tutti visibili (`.on` o le regole del ramo li mostrano comunque)

- [ ] **Step 2: Il ramo incolonnato**

La finestra headless ha una **larghezza minima ~500px**: dove puoi, **misura invece di fotografare**. Expected:
- `--spine` vale `24px` e la proprietà inline è stata **rimossa**, non riscritta (c'è già un `after.style.removeProperty('--spine')` a `index.html:2383` che serve a questo)
- `afKMax` ricalcolato su quella spina, e `copre` ancora `true`
- il numero è tornato piccolo (`.mv-num{display:inline}`)
- la barra dei social si spegne lo stesso — cioè il codice dello Step 3 del Task 6 sta **prima** del guardiano `tlLive`
- nessuno scorrimento orizzontale: `document.documentElement.scrollWidth <= innerWidth`

- [ ] **Step 3: La pagina intera, dall'inizio**

`p-final.js` aggiornato: errori in console, scroll orizzontale, `--spine`, l'apertura del fondo, i movimenti che si accendono. Percorri l'intera pagina a passi di mezzo schermo e conferma che **non c'è nessun frame con errori** e che l'altezza del documento non cambia.

**Trappola nota:** dopo uno `scrollTo` a mano il ciclo fa scattare `lockHero()`, il documento si accorcia di ~10 000px e lo scroll viene ricacciato indietro — la scena misurata non è quella chiesta. Rifai il piazzamento a ripetizione finché `scrollY` è quello voluto.

- [ ] **Step 4: `sim.js`**

`sim.js` era già fuori registro prima di questa modifica (coda della traccia e lunghezza dell'asse), e adesso lo è di più: `--railH` non esiste più. **O si riallineano le costanti in cima, o si scrive in testa al file che non è affidabile.** Non lasciarlo com'è: un file che sembra una fonte e mente è peggio di un file assente.

- [ ] **Step 5: Aggiorna la spec**

Nella spec, §11 «Punti aperti»: segna quali sono chiusi (gli URL social, i dati delle mensole, i due nodi cromatici) e con che esito. La spec resta il documento che spiega **perché**, e va lasciata vera.

- [ ] **Step 6: Consegna il resoconto**

Per punti, e **separa quello che hai misurato da quello che hai solo letto nel codice**. Non verificabili qui, e vanno dichiarati: il ritmo mentre si scorre, gli stati `:hover`, e il giudizio se il marrone funziona. Quelli li dà lui, ricaricando.

- [ ] **Step 7: Commit finale e push**

```bash
git add index.html sim.js docs/superpowers/specs/2026-09-02-coda-scura-design.md
git commit -m "Branches verified, and the docs left true"
git push
```

---

## Rollback

Ogni fase è un commit. Se una non funziona, `git revert` di quello. La fase 2 è la sola che cambia il carattere della pagina: se il marrone non piace, si torna al commit prima di lei e le fasi 3-6 vanno ripensate per la carta chiara — il contenuto resta valido, la palette no.
