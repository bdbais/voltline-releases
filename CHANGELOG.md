# Changelog

VOLT_LINE e' gratis, senza pubblicita' e senza tracciamento. Se ti diverte,
puoi offrire un caffe' al progetto: **[paypal.me/bellizia](https://paypal.me/bellizia)**.

Le versioni seguono `MAJOR.MINOR.PATCH`. Il `versionCode` di Android e' il
numero dopo il `+` in `app/pubspec.yaml` e cresce di uno a ogni rilascio: e' il
numero su cui l'app decide se c'e' un aggiornamento.

## 1.2.0 — 10 novembre 2026

### Hall of fame

I punteggi migliori stanno online, su voltline.bais.info. Dal menu si apre la
classifica, a fine partita si manda il proprio risultato: si sceglie un nome
una volta sola e resta.

Una classifica per andatura, perche' un punteggio fatto in facile e uno fatto
in difficile non si confrontano. Si legge anche dal browser, su
[voltline.bais.info/classifica](https://voltline.bais.info/classifica), in
tutte e tredici le lingue.

Non e' a prova di imbroglio, e non puo' esserlo: il punteggio lo manda il
gioco, e un gioco si puo' modificare. Quello che si puo' fare e' alzare il
costo -- campi validati, un tetto di punti al secondo che scarta i valori
impossibili, un limite di invii per indirizzo. Dell'indirizzo IP non resta
niente: serve solo al limite, e viene tenuto come primi sedici caratteri di
uno sha256 con un sale. Il gioco promette di non tracciare nessuno, e questo
non e' un'eccezione.

### Corretto

- **"2st place".** L'ordinale inglese dipende dalle ultime due cifre, non
  dall'ultima: 1st, 2nd, 3rd, ma 11th, 12th, 13th. In francese cambia solo il
  primo. Ora e' giusto, e un test lo tiene giusto.
- **Il pannello dei comandi da tastiera non partiva a premere un tasto.**
  Si chiudeva solo col clic. Visto dal vivo nel browser.

## 1.1.0 — 9 novembre 2026

### Si correva troppo

Tre giri di prove hanno detto sempre la stessa cosa: ingiocabile. La versione
originale partiva a 0.72 schermate al secondo e toccava il tetto di velocita'
in 635 punti, cioe' dentro la prima partita di chiunque.

**Tre andature**, scegli nelle impostazioni: facile, medio, difficile. Non
cambiano le regole -- livelli, power-up, punteggi sono identici -- cambiano
solo quanto tempo hai per reagire. Ognuna tiene il suo record, perche'
confrontare un punteggio fatto in facile con uno fatto in difficile non vuol
dire niente.

| | partenza | tetto |
|---|---|---|
| Facile | 0.28 schermate/s | 1600 punti |
| Medio (predefinito) | 0.34 | 1560 punti |
| Difficile | 0.50 | 875 punti |

Al livello 6, con 700 punti: facile 0.77, medio 1.04, difficile 2.18. Prima
erano 2.88 per tutti.

Anche al massimo della velocita' restano almeno trentatre centesimi di secondo
fra un ostacolo e l'altro: sotto quella soglia non si schiva piu', si muore a
caso. C'e' un test che lo verifica.

### Verificato, non supposto

Il sospetto era che il gioco andasse piu' veloce su telefoni piu' potenti.
Misurato: da 20 a 144 fps la distanza percorsa in quattro secondi e' la stessa
entro il 2%, e su schermi da 360x640 a 1280x800 un ostacolo impiega sempre
2.29 secondi ad arrivare. Era semplicemente tarato troppo veloce. Due test
nuovi lo bloccano.

### Come si installa

Scarica `voltline-arm64.apk` qui sotto (per i telefoni a 32 bit c'e'
`voltline-1.1.0-arm32.apk`). Android chiedera' una volta di consentire
l'installazione da questa fonte.

Fuori dal Play Store l'unica garanzia sul file e' il suo `sha256`, pubblicato
in `latest.json` accanto all'APK: puoi confrontarlo con
`sha256sum voltline-arm64.apk`. L'app fa la stessa verifica da sola prima di
installare un aggiornamento, e rifiuta il file se non corrisponde.

Il gioco controlla da se' se esce una versione nuova (si puo' spegnere nelle
impostazioni) e la scarica solo se glielo chiedi.

### Grazie

Beta test di **Claire**, che ha giocato e ha rifatto la curva di difficolta':
le tre andature di questa versione vengono dalle sue partite, non da una
taratura a tavolino.

## 1.0.0 — mai pubblicata

Prima versione Android, riscritta in Flutter partendo dal prototipo HTML
(`VoltLine.html`, che resta nel repository come riferimento giocabile).

### Il gioco

- Cinque cavi, sei livelli. Dal terzo compaiono blocchi doppi che occupano due
  cavi; dal sesto i cavi stessi iniziano a ondeggiare.
- **Quasi contatto**: passare rasente a un blocco vale un punto e una
  scintilla. Rischiare adesso conviene.
- Tre power-up dal secondo livello: **scudo** (assorbe un colpo), **rallentatore**
  (il mondo rallenta, il punteggio no), **magnete** (le cariche vicine si
  piegano verso l'impulso invece di teletrasportarsi).
- Combo sulle cariche consecutive, con moltiplicatore fino a cinque.
- Morte al rallentatore con scossa dello schermo e il blocco fatale illuminato:
  si capisce che cosa e' andato storto.

### Rispetto al prototipo

- **Fisica a tempo, non a frame.** Il prototipo muoveva il mondo di "pixel per
  frame": su un telefono a 120 Hz girava al doppio della velocita'. Ora la
  velocita' e' in schermate al secondo e le distanze fra gli ostacoli in secondi
  di viaggio, percio' il tempo per reagire e' lo stesso su ogni schermo.
- **Verticale e orizzontale.** In orizzontale la banda dei cavi si allarga, cosi'
  cinque cavi restano leggibili su uno schermo basso. Ruotando, il mondo si
  riscala invece di sparire o di presentarsi tutto insieme.
- **Collisione cerchio-rettangolo** invece del confronto fra indici di cavo: un
  salto a meta' strada schiva davvero.
- Pausa automatica quando l'app va in background, e col tasto indietro.

### Corretto durante il collaudo

- **La demo nel browser si piantava su GIOCA.** Mancava in `web/index.html` il
  tag che istanzia il modulo audio WASM (obbligatorio da flutter_soloud 5.0):
  `init()` lo cercava per cinque secondi e la schermata iniziale lo aspettava.
- La schermata iniziale non aspetta piu' l'audio: se non arriva, si gioca in
  silenzio. `init()` ha una scadenza, e il fallimento si vede nelle
  impostazioni.
- Alone al neon senza sfocature a tutto schermo (erano fino a diciassette per
  frame) e fondale dei menu che si ferma durante la partita.
- `versionCode` uniforme fra APK e manifest: con `--split-per-abi` Flutter ne
  sommava 1000 per architettura e l'aggiornamento in-app non sarebbe scattato.
- Rimossi i permessi di lettura dei media che `open_filex` portava dentro.

### Contorno

- Tredici lingue: it, en, es, fr, de, pt, tr, ru, uk, ar (da destra a sinistra),
  zh, ja, ko.
- Impostazioni: volumi separati per musica ed effetti, vibrazione, palette per
  daltonici (che da' a ogni cavo anche una forma), effetti ridotti, tre schemi di
  comando (tocco, trascinamento, due tasti), lingua.
- Statistiche: record, partite, livello massimo, combo migliore, cariche, quasi
  contatti, distanza.
- Aggiornamento in-app: manifest `latest.json` dal repository pubblico delle
  release, con verifica sha256 dell'APK prima dell'installazione.
- Audio generato in memoria: nessun file sonoro nell'APK. Sequencer a
  sedicesimi, BPM da 90 a 165 in funzione della velocita', quattro livelli di
  strumentazione.
