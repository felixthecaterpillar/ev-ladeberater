# EV Ladeberater

Persönlicher Ladeberater für Elektroautos — optimiert Ladezeiten anhand von EPEX AT Spotpreisen.

**Tesla Model Y 2026 · AAE Naturstrom Dynamiktarif · Bad Hofgastein**

## Features

- Live EPEX AT Spotpreise via aWATTar API (kostenlos, kein Account)
- AAE Naturstrom Tarif-Kalkulation (Spot + Aufschlag + MwSt.)
- Zweiphasige Ladeoptimierung: Pflicht (immer) + Bonus (Durchschnittspreisfilter)
- Kostengrenze: €/Ladung oder €/100km (schlagend, jeweils das andere abgeleitet)
- Preischart mit SoC-Verlauf, linker ct/kWh-Achse, rechter €/100km-Achse
- Kostenvergleich EV vs. Verbrenner
- Alle Einstellungen lokal im Browser gespeichert (localStorage)

## Dateien

```
index.html          ← Haupt-App
callback.html       ← Tesla OAuth Callback (für später)
.well-known/
  appspecific/
    com.tesla.3p.public-key.pem   ← Tesla Public Key (für später)
```

## Einrichtung

### GitHub Pages aktivieren

Settings → Pages → Deploy from branch → main / (root)

App dann erreichbar unter: `https://USERNAME.github.io/REPO/`

### Tesla API (später)

1. Public Key generieren (Terminal/Git Bash):
```bash
openssl ecparam -name prime256v1 -genkey -noout -out private-key.pem
openssl ec -in private-key.pem -pubout -out public-key.pem
```
2. Inhalt von `public-key.pem` in `.well-known/appspecific/com.tesla.3p.public-key.pem` einfügen
3. Auf developer.tesla.com App registrieren mit GitHub Pages URLs
4. Client ID + Secret in App-Einstellungen eintragen

⚠️ `private-key.pem` niemals ins Repository hochladen!

## Tarif-Kalkulation

```
Endpreis = (EPEX Spot €/MWh ÷ 1000 + Aufschlag ct/kWh ÷ 100) × (1 + MwSt./100)
```

Standard AAE Naturstrom: Spot + 1,30 ct/kWh + 20% MwSt.

## Ladeoptimierung

```
Pflichtladung  = minSoC-kWh + Fahrt-kWh − aktuelle kWh   (immer, günstigste Slots)
Bonusladung    = bis maxSoC-Deckel, wenn Ø-Preis ≤ Limit  (optional)
```
