# S'Ena Arrubia Camping Village — Design System

Traduzione operativa del **Brand Book 2024 — Linee guida di utilizzo** (DROP, 28 pagine,
`Sito/Brand/S_Ena Arrubia - Brand Book 2024.pdf`) in token, componenti e file pronti.

```bash
cd design-system && npm install && npm run dev   # http://localhost:5173
```

- **◆** = dal brandbook: normativo.
- **★** = indicazione DROP successiva al brandbook.
- senza segno = derivato, per ciò che il brandbook non copre (web, UI, fotografia).

## 1. Il logo ◆ p.4–15
Tre parti (◆ p.11): **A** pittogramma (l'oasi e il sole, con tre stelle), **B** logotipo S'ENA ARRUBIA,
**C** payoff CAMPING VILLAGE. A può stare da sola; B e C no.
Logotipo da Signika personalizzato (◆ p.12): A senza asta, apostrofo a goccia, RR legate, P aperta,
M con vertice sulla linea di base; ARRUBIA sempre rosso nelle versioni a colori.

| Versione | File | Fondo ◆ p.19-20 |
|---|---|---|
| Ufficiale (sabbia + rosso) | `logo-ufficiale` | bianco |
| Bianco + rosso | `logo-bianco-rosso` | sabbia, cotto, cielo, grigio 25% e 75% |
| Cotto + rosso | `logo-cotto-rosso` | sabbia |
| Negativo (bianco al tratto) | `logo-bianco` | minio, nero, grigio 50% |
| Positivo (nero al tratto) | `logo-nero` | bianco, grigio 10% |

Area di rispetto ◆ p.13: l'altezza della parola CAMPING su ogni lato (≈ 5,6% della larghezza).
Dimensione minima ◆ p.14: **2 cm** stampa, **60 px** schermo.
Vietato (◆ p.15, 21): deformare, ruotare, spostare le parti, fondi fuori palette.

## 2. Icone ◆ p.14, 25
- **Sole** — semplificazione ufficiale del logo per social e segnaletica. 7 colori.
- **Pittogramma** — a colori (sole sabbia / sole bianco) e al tratto (nero, bianco, cotto, minio).
- **Tre stelle** — ritagliate dal pittogramma, 7 colori (derivato; ◆ p.27 le usa nell'insegna).
- **Immagini profilo** — 5 versioni 1080×1080.
- **Servizi** ◇ — 24 icone dalla legenda della mappa 2025 (parcheggio, piscina, casa mobile, area cani…).
- **Raccolta differenziata** ◇ — 6 icone dal flyer 2025.
- **Dotazioni** — 37 icone al tratto (persone, m², letti, aria condizionata, bagno, veranda…) + 8 d'interfaccia,
  disegnate per il sito nel nov 2026 (griglia 24, tratto 1,6, currentColor). Sprite `icone/dotazioni.svg`,
  file singoli da `scripts/dotazioni.py`. Solo per le dotazioni: per i servizi restano le icone multi-tono.
Tutte SVG + PNG; sole/pittogramma/stelle anche `-mono` in currentColor. Generate da `scripts/marchio.py` e
`scripts/icone_mappa.py`, tracciati intatti. Le icone servizi sono multi-tono e non si ritingono.
◇ = dagli stampati 2025 (flyer, mappa A3 e 100×70), composti in Open Sans: i nuovi materiali usano Raleway.

## 3. Colore ◆ p.16–18
| Colore | HEX | Pantone | CMYK | Ruolo |
|---|---|---|---|---|
| Rosso Minio | #D85844 | 2027 C | 10 76 72 2 | primario |
| Sabbia | #EBC6A4 | 719 C | 8 25 38 1 | primario |
| Rosso Cotto | #A4382E | 7627 C | 25 86 80 20 | secondario |
| Bianco | #FFFFFF | — | 0 0 0 0 | secondario |
| Acqua | #D9EEF6 | P 115-2 C | 18 0 4 0 | terziario |
| Cielo | #79CADB | 2197 C | 54 0 15 0 | terziario |

Derivato: fondo bianco, testo nero, titoli cotto, titoli grandi/etichette minio, testo bianco su minio/cotto.
Minio non per testo piccolo (3,9:1); mai testo bianco su cielo (1,9:1) o sabbia (1,6:1).

## 4. Tipografia ★
**Raleway** (OFL), unico carattere. Titoli in frase Bold/ExtraBold, etichette maiuscolo spaziato
(come CAMPING VILLAGE), corpo Regular 17 px / 1,6. Il brandbook indica Signika (◆ p.23): resta solo nel logo.

## 5. Grafica
Archi ◆ p.1, 4, 10, 16, 28: anelli bianchi spessi tagliati dall'angolo, su minio e sabbia (`.se-archi`).
Pretitoli (`.se-etichetta`): maiuscolo spaziato preceduto dal sole del logo, tinto col colore del testo.

## 5b. Componenti (derivato, dal sito — nov 2026)
Presi dalla bozza del sito (`Sito/sito-bozza`, online su alessiogranella.github.io/senarrubia-sito-bozza) e portati
in `components.css` con prefisso `se-`; in React in `src/components/`. I componenti non decidono il proprio margine esterno.
- **Base**: pretitolo con sole, titoli `se-titolo-xl/-l` (seconda parte in corsivo minio), bottoni `se-btn`
  (pieno minio = solo prenotare · scuro · linea · bianco su fondi pieni · vetro su foto), link con freccia.
- **Testata**: fissa, 88 px, trasparente sulla hero (logo bianco), solida dopo (logo a colori), nascosta scorrendo giù.
- **Prenotazione**: barra (sotto la hero) e riquadro (laterale nella scheda), collegati al motore MyGuestCare
  con `linkMotore()` (`src/motore.ts`). Il motore non si cambia.
- **Filtro**: segmento (scelta unica), contatore, chip (scelta multipla, attivi in cielo), pillole numeriche.
- **Sistemazioni**: scheda dell'elenco (foto che cambia in hover, tre dati, dotazioni), card foto 4:5, tabella di confronto.
- **Scheda alloggio**: mosaico foto, in breve, ambienti, griglia di icone, da sapere, orari come linea del tempo, inclusi.
- **Sezioni**: hero, pannello a tutto schermo, voce in card, promo divisa, fascia (sabbia velata = fondo caldo del sito), piede.
- **Movimento**: una curva (`--se-ease`), tre durate. Nel sito GSAP + Lenis; con «riduci movimento» niente animazioni.

## 6. Fotografia (derivato)
Luce piena, pineta, mare, oasi e fenicotteri, persone vere. Niente filtri, stock, AI spacciata per foto.
Originali in `Sito/1. CHECK FOTO 3/`; selezione 1600 px in `design-system/public/brand/foto/`.

## 7. Download
Sezione Download della libreria: kit completo, logo, icone, Raleway, foto. Gli originali di stampa (AI/EPS/PDF del cliente) non sono in questa consegna: si chiedono a DROP.
