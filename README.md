# NHE tech — ndtechai.com

Statische site. Geen build-stap, geen dependencies.

## Structuur

```
dist/            wat live gaat (publish directory op Netlify)
  index.html     de website
  funnel.html    intake-funnel, doel van de QR-code op het visitekaartje
  status.json    status van de pixel-mascotte
  _redirects     /intake en /funnel
  robots.txt
  sitemap.xml
netlify.toml     publish directory, headers en redirects
```

De bestanden in `dist/` zijn gecompileerd. Bewerk ze niet met de hand.

## Deploy

Netlify staat gekoppeld aan deze repo, branch `main`, publish directory `dist`.
Elke push naar `main` gaat automatisch live.

## Mascotte-status bijwerken

`dist/status.json` overschrijven verandert de mascotte binnen een minuut:

```json
{ "state": "gym-push", "note": "OHP 5x5", "expires_at": "2026-08-28T20:00:00Z" }
```

States: `idle`, `working`, `deep-work`, `field-work`, `gym-push`, `gym-pull`,
`gym-legs`, `commuting`, `sleeping`, `offline`, `private`. Leeg bestand = de
mascotte volgt het weekschema in de code.

## Intake-funnel

De funnel POST't naar `https://ndtechai.app.n8n.cloud/webhook/funnel`. Zet de
**Allowed Origins (CORS)** van die Webhook-node op `https://ndtechai.com`, anders
weigert de browser het verzoek en valt de funnel terug op een mailto-link.

QR-code op het visitekaartje: `https://ndtechai.com/intake?src=card`.
