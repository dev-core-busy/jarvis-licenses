# Jarvis – Lizenz-Statusdienst

Dieses Repository enthält ausschließlich die Datei **`status.json`**: den
aktuellen Gültigkeits- und Widerrufsstand der ausgegebenen Jarvis-Lizenzen.
Jede Installation ruft sie einmal täglich ab.

## Was drinsteht – und was ausdrücklich nicht

```json
{
  "v": 1,
  "stand": "<Zeitpunkt der Veröffentlichung>",
  "eintraege": {
    "<sha256 der Lizenz-Kennung>": {
      "status": "active | revoked",
      "art": "FREE | BASIC | ENTERPRISE",
      "gueltig_bis": "JJJJ-MM-TT",
      "hwid": "<Hardware-Kennung> | null"
    }
  },
  "zert": { ... },
  "sig": "<Signatur über v, stand und eintraege>"
}
```

**Keine Firmennamen, keine Abteilungen, keine Mailadressen.** Ein Eintrag ist
über den Hash seiner Lizenzkennung auffindbar – jede Installation findet ihren
eigenen, ein Außenstehender kann daraus weder die Kennung noch den Inhaber
ableiten. Die Hardware-Kennung besteht ihrerseits nur aus Hashwerten.

## Warum die Datei signiert ist

Die Datei wird als Ganzes mit einem Ed25519-Schlüssel signiert; das dabei
mitgelieferte Zertifikat stammt vom Root-Schlüssel der Ausgabestelle, den jede
Installation kennt. Ohne Signatur genügte ein Fork dieses Repositories plus
eine manipulierte Namensauflösung, um jede Lizenz auf die höchste Stufe zu
heben.

Der Feldwert `stand` ist zugleich ein Rückspielschutz: Eine Installation
übernimmt keinen Stand, der älter ist als der ihr bereits bekannte. Ein
Widerruf lässt sich damit nicht durch das Zurückspielen einer älteren – korrekt
signierten – Fassung aufheben.

## Bearbeiten

Nicht von Hand. `status.json` wird von der Ausgabestelle erzeugt und
veröffentlicht; jede händische Änderung macht die Signatur ungültig, und alle
Installationen behalten dann ihren letzten bekannten Stand.
