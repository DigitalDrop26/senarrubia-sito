# S'Ena Arrubia Camping Village — sito (consegna per lo sviluppo)

Prototipo navigabile del nuovo senarrubia.com, a cura di DROP. È il riferimento per layout, componenti,
contenuti e comportamento: va portato sulla piattaforma definitiva (CMS / tema) mantenendo il risultato visivo.

Anteprima online: https://alessiogranella.github.io/senarrubia-sito-bozza/v2/
Design system (token, componenti, icone, regole del marchio): https://alessiogranella.github.io/senarrubia-design-system/
— il sorgente completo è nella cartella `design-system/` (vedi sotto).

## Come aprirlo

HTML, CSS e JavaScript statici, senza build. Serve un server locale (le icone in `assets/icone/sistemazione.svg`
sono richiamate con `<use href>` e non funzionano aprendo i file con doppio clic):

```
cd senarrubia-sito
python3 -m http.server 8000
# poi http://localhost:8000
```

Librerie da CDN (jsDelivr): GSAP 3.13 (con ScrollTrigger, SplitText, Flip) e Lenis 1.1.13.
Tutte le animazioni rispettano `prefers-reduced-motion`.

## Pagine

| File | Contenuto |
|---|---|
| `index.html` | Home: hero con stagione e offerte a rotazione, barra di prenotazione, sistemazioni, piazzole, «Prima di partire» |
| `sistemazioni.html` | Elenco con filtro (tipologia, ospiti, dotazioni), ordine, vista confronto, scheda in breve («i»), cassetto date → motore |
| `casa-mobile-mirto.html` | Scheda alloggio completa con pianta interattiva (fatta a mano) |
| `casa-mobile-sunset-comfort.html`, `casa-mobile-sunset-comfort-plus.html`, `casa-mobile-fenicottero.html`, `casa-mobile-fenicottero-plus.html`, `tenda-lodge-safari.html`, `tenda-lodge-oasi.html` | Schede alloggio generate da `genera-schede.py` |
| `piazzole.html` | Piazzole S/M/L, «Con cosa viaggi?», zone sulla mappa |
| `mappa.html` | Mappa A3 ufficiale con zone e servizi attivabili |
| `info-utili.html` | Regolamenti per tema, testo completo apribile, PDF |
| `offerte.html` | Offerte con la barra della stagione |
| `lavora-con-noi.html` | Candidatura spontanea con invio CV |

Le voci di menu Servizi e Pet non hanno ancora una pagina nel prototipo; Dove siamo porta alla sezione in home.

## Struttura

```
assets/css/tokens.css   token del design system (--se-*), font Raleway
assets/css/sito.css     componenti base (testata, bottoni, chip, card, form, piede…)
css/v2.css              componenti aggiuntivi delle pagine (stagione, mappa, compreso, cassetti…)
assets/font/            Raleway variabile (licenza SIL OFL)
assets/brand/           logo, pittogramma, sole (SVG ufficiali)
assets/icone/           icone servizi della mappa A3 + sprite dotazioni `sistemazione.svg`
assets/foto/            foto ad alta risoluzione
img/alloggi/<id>/       foto aggiuntive delle schede (850 px, prese dal motore: da sostituire con gli originali)
img/mappa.jpg           ritaglio della mappa A3 ufficiale
js/dati.js              TUTTI i contenuti variabili (vedi sotto)
js/sito.js              testata, piede, animazioni comuni a tutte le pagine
js/filtro.js            filtro della pagina Sistemazioni
js/scheda.js            comportamento delle schede alloggio
js/v2.js                blocchi ripetuti (compreso/non compreso, condizioni, documenti)
js/mappa.js             coordinate di zone e servizi sulla mappa (in % dell'immagine)
genera-schede.py        genera le 6 schede alloggio: testi e foto sono lì dentro
documentazione/         integrazione col motore di prenotazione
design-system/          design system completo (sorgente), vedi la sezione dedicata
```

Testata e piede non sono nell'HTML: li scrive `js/sito.js` su ogni pagina (array `VOCI` per il menu).

## Contenuti: `js/dati.js`

Ogni contenuto che cambia nel tempo sta in un solo file, letto da tutte le pagine. Sulla piattaforma definitiva
conviene trasformarlo in campi gestibili dal cliente (custom post type / campi personalizzati o simili):

- `SISTEMAZIONI` — i 7 alloggi: nome, tipo, posti, m², camere, letti, bagno, dotazioni (`tratti`), foto, pagina, ID sul motore
- `STAGIONE` — date di apertura e chiusura
- `COMPRESO` / `ESCLUSO` — cosa è incluso nella tariffa
- `CONDIZIONI` — condizioni di prenotazione
- `PIAZZOLE`, `ZONE` — misure e zone del campeggio
- `OFFERTE` — offerte con periodo di validità
- `DOCUMENTI` — PDF scaricabili

I valori con `daConfermare: true` e gli elementi `.nota-bozza` / `.vuoto-dato` (bordo tratteggiato azzurro)
sono segnaposto in attesa dei dati della struttura: vanno sostituiti e i due stili eliminati prima della pubblicazione.

## Prenotazioni: motore MyGuestCare (Mycomp)

Il motore resta quello attuale su `booking.senarrubia.com`; il sito ci si collega solo con un link (GET), costruito
da `MOTORE.link()` in `js/dati.js`. Parametri principali:

```
https://booking.senarrubia.com/.eshop?idcliente=senaarrubia&idvendor=1&orderBy=2&fast=1&invcart=1
  &arrivo=gg/mm/aaaa&partenza=gg/mm/aaaa&stanze=1&adulti1=N&bambini1=N&etabambino1{k}=ETÀ
  &lang=1|2|3|4|5   (it, en, fr, de, es)
```

Per aprire il motore con un alloggio in cima ai risultati si aggiungono
`&act=result-strutture&alltratt=1&noFilterUnit=1&productsel=ID` (funziona solo con le date).
L'alloggio viene messo per primo ma gli altri restano visibili: il motore non ha un filtro esclusivo.

ID sul motore: Mirto 71 · Fenicottero 91 · Fenicottero Plus 94 · Sunset Comfort 50 · Sunset Comfort Plus 56 ·
Tenda Lodge Oasi 5 · Tenda Lodge Safari 105 · ACSI 39 · Tenda S/M/L 119/121/123 · Camper S/M/L 125/127/129 ·
Roulotte S/M/L 131/133/135.

Nelle pagine i moduli di prenotazione usano `data-prenota="<id>"`. Il link «Richiedi un preventivo» porta a
`https://www.senarrubia.com/richiesta-disponibilita/`, da mantenere o rifare sul nuovo sito.

In `documentazione/integrazione-myguestcare/`: specifiche in PDF (`DROP_Integrazione_MyGuestCare_specifiche.pdf`,
stesso contenuto di `report.html`) e un componente barra di prenotazione autonomo (`booking-bar.html`).

## Schede alloggio

Le 6 schede diverse dalla Mirto si rigenerano con `python3 genera-schede.py` (Python 3, nessuna dipendenza).
Non modificare a mano gli HTML generati: ogni modifica va nello script. Sulla piattaforma definitiva le 7 schede
diventano un unico modello alimentato dai dati.

## Prima della pubblicazione

- Togliere `<meta name="robots" content="noindex">` da tutte le pagine (c'è perché l'anteprima è pubblica)
- Collegare il modulo di «Lavora con noi» a una casella email: oggi controlla i campi e mostra il messaggio di conferma, ma non invia nulla
- Selettore lingua: c'è solo il pulsante IT, le traduzioni non sono ancora fatte (il motore accetta già `lang`)
- Privacy e Cookie nel piede: link vuoti
- Sostituire i segnaposto in attesa della struttura:
  - date di apertura 2027 (ora 14 maggio – 4 ottobre, dalle offerte 2026) e offerte 2027
  - misure delle piazzole S, M, L e soglia di lunghezza dei camper
  - zona e distanze di ogni zona e di ogni alloggio da spiaggia, piscina, servizi, ristorante (la Mirto ora è d'esempio in zona 300)
  - piante degli alloggi dal produttore (quella della Mirto è indicativa)
  - PDF: regolamento pet, regolamento e orari piscina, calendario raccolta differenziata
  - foto: cameretta di Sunset Comfort, soggiorno di Sunset Comfort Plus, bagno della Tenda Safari; originali al posto delle foto da 850 px in `img/alloggi/`

## Design system (`design-system/`)

Il sorgente della libreria del marchio, la stessa pubblicata online. Il sito usa gli stessi componenti:
nel design system hanno il prefisso `se-` (es. `.se-btn`, `.se-card`), nel prototipo no (es. `.btn`, `.card`).

- `DESIGN.md` — il manuale: colori, tipografia, spazi, uso del logo, fotografia, componenti
- `src/styles/colors_and_type.css` — token CSS (stessi di `assets/css/tokens.css`)
- `src/styles/components.css` — componenti CSS con prefisso `se-`
- `src/tokens.ts` — token in TypeScript
- `src/components/` — componenti React (Bottone, Etichetta, Logo, Prenota, SchedaSistemazione, icone…)
- `src/motore.ts` — `linkMotore()`, la stessa costruzione del link al motore di `js/dati.js`
- `public/brand/` — loghi, icone, font, foto in tutti i formati

Per vederlo in locale (Node 18+):

```
cd design-system
npm install
npm run dev      # http://localhost:5173
npm run build    # sito statico in dist/
```

## Regole di marchio da rispettare

- Font: Raleway per tutto il sito (Signika solo dentro il logo, non va usata nei testi)
- Colori: solo i token `--se-*` di `tokens.css`; unica eccezione i 4 colori delle zone, presi dalla legenda della mappa A3
- Il pretitolo (`.occhiello`) ha davanti il sole del logo
- Logo e pittogramma: solo i file in `assets/brand/`, senza modificarli
