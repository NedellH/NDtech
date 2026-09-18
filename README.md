# NHE tech — ndtechai.com

Statische site. Geen build-stap, geen dependencies. Netlify publiceert de
repo-root.

## Structuur

```
index.html     de website
funnel.html    intake-funnel, doel van de QR-code op het visitekaartje
404.html       niet-gevonden-pagina
og.png         deelafbeelding voor LinkedIn, WhatsApp en Slack
status.json    bedoeld voor de mascotte-status (zie waarschuwing onderaan)
robots.txt
sitemap.xml
netlify.toml   publish directory, headers en redirects
```

## Deploy

Netlify is gekoppeld aan deze repo, branch `main`, **publish directory `.`**
(de repo-root, zoals vastgelegd in `netlify.toml`). Elke push naar `main` gaat
automatisch live. Er is geen `dist/`.

## Let op: index.html en funnel.html zijn gebundelde bestanden

Beide pagina's zijn één bestand waarin de hele pagina als JSON in een
`<script type="__bundler/template">` staat. **Bewerk ze niet met de hand in een
editor.** De bundel escapet `<` en `/` zodat de JSON zijn eigen script-tag niet
kan sluiten. Wie dat mist, krijgt een pagina die `{{ t.heroH1 }}` toont in
plaats van tekst.

Bewerken gaat via een script dat de JSON decodeert, de tekst aanpast en met
diezelfde escaping terugschrijft. Zie `git log` voor de wijzigingen.

## Contactformulier en funnel

Beide POST'en naar `https://ndtechai.app.n8n.cloud/webhook/funnel`.

- De funnel stuurt het volledige intake-antwoord.
- Het contactformulier op de homepage stuurt `{ bron: "site-contactformulier",
  form: "contact", naam, bedrijf, email, vraag, taal, page, ts }`.

**Splits hierop in n8n**, anders belanden contactvragen in de intake-flow.

Lukt de POST niet, dan valt beide terug op een mailto met alles ingevuld. Zet
de **Allowed Origins (CORS)** van de Webhook-node op `https://ndtechai.com`,
anders weigert de browser het verzoek en ziet elke bezoeker de terugvaloptie.

QR-code op het visitekaartje: `https://ndtechai.com/intake?src=card`.

## Nog niet af

`status.json` wordt door geen enkele pagina gelezen. De mascotte volgt op dit
moment alleen het weekschema in de code. Het bestand staat er voor als die
koppeling alsnog gebouwd wordt.
