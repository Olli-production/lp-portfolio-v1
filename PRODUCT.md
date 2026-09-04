# Product

## Register

brand

## Users

Chi assume o ingaggia un Software Designer: recruiter, head of design, fondatori,
studi. Ci arrivano da un link — una candidatura, un profilo social, un messaggio
— quasi sempre da desktop, con pochi minuti e altre dieci schede aperte.

Il lavoro da fare è uno: capire in fretta **che tipo di progettista è**, e se vale
la telefonata. Non cercano una lista di tecnologie: quella la trovano sul CV.

## Product Purpose

Il sito personale di Oliviero Petrucci. Non un portfolio a griglia di progetti: un
racconto che si scorre, in cui l'intro, la sezione progetti, la linea del tempo e
la coda sono un pezzo unico guidato da un solo segnale di scroll.

La coda (`.after`) è l'ultimo movimento: dopo aver mostrato *cosa ha fatto*, dice
**come guarda** e come sta in una squadra. Successo = chi arriva in fondo ha
un'idea precisa della persona e ha il recapito sotto gli occhi.

## Brand Personality

Preciso, fisico, non ammiccante. Il sito non spiega di essere curato: lo è, e si
vede dal comportamento — un solo inseguitore a molla per tutto, ogni gesto
misurato, niente animazioni decorative.

Voce: prima persona, frasi brevi, nessun superlativo. La tipografia grida, il
testo no.

## Anti-references

Tutte e quattro confermate dall'utente:

- **Portfolio da fotografo.** Le fotografie sono la prova di come guarda, non il
  mestiere che vende. Se la coda sembra il sito di un fotografo il messaggio si
  ribalta.
- **Landing SaaS.** Niente griglie di schede uguali, metriche giganti, icone
  tonde sopra ogni titolo.
- **Rivista editoriale.** Niente serif in corsivo, capilettera, colonne separate
  da filetti, monocromia da magazine.
- **CV impaginato.** Niente elenchi puntati, sezioni intitolate, competenze come
  tabella. È il difetto che la coda ha oggi e la ragione di questo redesign.

## Design Principles

1. **Un solo segnale.** Tutto il sito è una funzione di `overCur`, l'inseguitore a
   molla. Ogni pezzo nuovo si aggancia a quello: due orologi diversi sulla stessa
   pagina si vedono subito.
2. **Si ricava, non si tara.** Se un numero sta per essere scritto due volte, si
   misura invece di ripeterlo.
3. **Mostra, non elencare.** Le competenze si dicono con l'ordine, il movimento e
   un accento — mai con la taglia di cinque titoli o una tabella.
4. **Il gesto costa.** Ogni animazione toglie controllo a chi guarda: se ne fa una
   sola per sezione, e deve dire qualcosa che il testo fermo non direbbe.
5. **Tre rami sempre.** Desktop con puntatore, touch, `prefers-reduced-motion`.
   Quel che dipende da una scrittura per frame va consegnato dal CSS negli altri
   due, o si rompe in silenzio.

## Accessibility & Inclusion

WCAG AA come minimo, verificato **sul rendering** e non a calcolo: i nove ruoli di
testo della coda stanno oggi fra 10.8:1 e 16.53:1.

- `prefers-reduced-motion` è un ramo di prima classe, non un ripiego: sparisce il
  movimento, non la composizione — e mai lo scroll sequestrato.
- Le fotografie sono contenuto, non decorazione: `alt` che descrivono il
  fotogramma, mai `alt=""`.
- Ogni testo duplicato per un ciclo di scorrimento va `aria-hidden`.
- La scaletta dei titoli è vera: niente `<p>` usati come intestazione.
