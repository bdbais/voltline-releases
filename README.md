# VOLT_LINE — release

Questo repository contiene **solo i binari**: gli APK firmati, il manifest degli
aggiornamenti e le note di versione. Il codice del gioco sta in un repository
privato.

- **Scarica l'ultima versione:** [voltline.bais.info](https://voltline.bais.info)
  (il pulsante porta sempre all'ultima release pubblicata)
- **Link diretto stabile:**
  [`voltline-arm64.apk`](https://github.com/bdbais/voltline-releases/releases/latest/download/voltline-arm64.apk)
- **Sostieni il progetto:** [paypal.me/bellizia](https://paypal.me/bellizia)

## Come verificare quello che installi

Ogni release pubblica due copie dello stesso APK — una col numero di versione,
una a nome fisso — e un `latest.json` con lo `sha256` del file. Fuori dal Play
Store quel numero e' l'unica garanzia che il file installato sia quello
compilato dalla build automatica:

```bash
sha256sum voltline-arm64.apk
curl -s https://raw.githubusercontent.com/bdbais/voltline-releases/main/latest.json
```

I due valori devono combaciare. L'app fa la stessa verifica da sola prima di
installare un aggiornamento, e rifiuta il file se non corrisponde.

Tutte le release sono firmate con la stessa chiave: se Android si lamenta della
firma durante un aggiornamento, il file non viene da qui.

## `latest.json`

E' il file che l'app interroga. Vive sul ramo `main` — non fra gli asset —
perche' `raw.githubusercontent.com` non ha limiti di chiamate.

```json
{
  "versionName": "1.0.0",
  "versionCode": 1,
  "releaseDate": "2026-09-13",
  "mandatory": false,
  "notes": "una riga di novita'",
  "android": {
    "url": "https://github.com/bdbais/voltline-releases/releases/download/v1.0.0/voltline-arm64.apk",
    "sha256": "...",
    "sizeBytes": 0
  }
}
```

## Requisiti

Android 6 o successivo, 64 bit (arm64-v8a). Per i telefoni a 32 bit ogni
release pubblica anche un APK `arm32`.
