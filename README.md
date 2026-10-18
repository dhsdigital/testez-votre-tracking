# Testez votre tracking : audit de tracking gratuit (free website tracking checker and MCP server)

> **En français** : audit de tracking gratuit d'un site à partir de son adresse publique, sans compte, sans clé et sans
> extension : Google Tag Manager, GA4, Google Ads, pixel Meta, TikTok, bannière cookies, Consent Mode v2 et tracking
> server-side (Stape, Addingwell…). Score sur 100 et correctifs en français. Sur le web : https://dhsdigital.eu/tracking/ ·
> dans Claude, ChatGPT, Codex ou Le Chat : https://dhsdigital.eu/tracking/assistants-ia/ · méthode : [audit de tracking gratuit](https://daniloduchesnes.com/blog/audit-tracking-gratuit/) · [détails en français](#en-français).


**Free, read-only audit of a website's marketing tracking from its public URL. No account, no OAuth, no API key, no
browser extension.** It checks Google Tag Manager, GA4, Google Ads conversion tags, Meta Pixel, TikTok, Pinterest,
the cookie banner, Consent Mode v2 and server-side tagging (Stape, Addingwell…). It returns a score out of 100,
prioritised fixes, what is already in place and what cannot be verified from outside.

- Web app: https://dhsdigital.eu/tracking/
- MCP server (remote, Streamable HTTP, no authentication): `https://dhsdigital.eu/wp-json/dhs-audit/v1/mcp`
- Plain-text report for any agent or script: `https://dhsdigital.eu/wp-json/dhs-audit/v1/rapport?site=example.com`
- Documentation (FR): https://dhsdigital.eu/tracking/assistants-ia/
- Official MCP Registry: `eu.dhsdigital/testez-votre-tracking`

Reports are written in French (the tool is built for French-speaking advertisers in France, Belgium and Switzerland).
Field names of the structured output are in French too; see [examples](examples/).

## Why "no account" matters

Most tracking audit tools and MCP servers read your Google Tag Manager or GA4 configuration through Google OAuth, or
need a browser extension and someone who can read network requests. This tool only uses what any visitor can see:
the page you name and its public Google Tag Manager containers. You can test your own site in seconds, a prospect's
site, a competitor's site, or your site before giving anyone access to your accounts.

## What it checks (13 points, 4 families)

| Family | Points |
|---|---|
| Foundations (20) | Google Tag Manager, duplicate tags, hard-coded tags |
| Measurement (25) | GA4, ad platforms (Google Ads, Meta, TikTok, Pinterest), Google Ads click linking, enhanced conversions / Conversions API |
| Consent (25) | Cookie banner (CMP), Consent Mode v2 (default state, the 4 v2 signals, consent granted by default) |
| Resilience (30) | First-party loading, server-side tagging, measurement subdomain, Safari cookie lifetime (ITP) |

Points that do not apply are left out of the score; points that cannot be verified from outside count for half.
When a site cannot be read (anti-bot protection, JavaScript-only page), the result is **inconclusive**: the tool
never invents a score.

## Use it from an AI assistant (MCP)

One tool, `tester_tracking`:

| Argument | Type | Description |
|---|---|---|
| `url` | string, required | Site to test (domain or URL), e.g. `www.example.com` |
| `detail` | `synthese` or `complet` | `synthese`: score and the first actions. `complet` (default): all 13 points, what is in place, the limits |

Annotations: `readOnlyHint: true`, `destructiveHint: false`, `idempotentHint: true`, `openWorldHint: true`.
Output: a Markdown report (`content[0].text`) and `structuredContent` described by an `outputSchema`.

### Claude (claude.ai, Desktop, mobile)
Settings → Connectors → Add custom connector → URL `https://dhsdigital.eu/wp-json/dhs-audit/v1/mcp`, no
authentication. Then ask: *"Teste le tracking de www.example.com"* or *"Audit the tracking of www.example.com"*.

### Claude Code
```bash
claude mcp add --transport http testez-votre-tracking https://dhsdigital.eu/wp-json/dhs-audit/v1/mcp
```

### OpenAI Codex CLI
```bash
codex mcp add testez-votre-tracking --url https://dhsdigital.eu/wp-json/dhs-audit/v1/mcp
```

### ChatGPT
Where your plan allows custom connectors: enable developer mode in the settings, then create a connector with the MCP
URL above, without authentication.

### Cursor (`.cursor/mcp.json`)
```json
{ "mcpServers": { "testez-votre-tracking": { "url": "https://dhsdigital.eu/wp-json/dhs-audit/v1/mcp" } } }
```

### VS Code (`.vscode/mcp.json`)
```json
{ "servers": { "testez-votre-tracking": { "type": "http", "url": "https://dhsdigital.eu/wp-json/dhs-audit/v1/mcp" } } }
```

### Mistral Le Chat
Connectors → add a custom connector with the MCP URL, no authentication.

### Without MCP
```bash
curl "https://dhsdigital.eu/wp-json/dhs-audit/v1/rapport?site=www.example.com&detail=synthese"
```

## Example

Real output for `www.lefourgon.com` (`detail: "synthese"`, 11 October 2026; only the tracking parameters of the report
link were adapted for this page): [Markdown](examples/lefourgon-synthese.md) ·
[structuredContent](examples/lefourgon-synthese.json).

## Limits

- Static reading: the tool reads the page and the published GTM containers; it does not click the cookie banner,
  run a checkout, or see what the server-side container forwards. Purchase events and data received by the platforms
  must be checked in an end-to-end audit.
- Rate limits: 30 new analyses per hour per IP address; recent results are served from a 6-hour cache.
- Sites behind anti-bot protection may return an inconclusive result.

## Data

The server receives the site address to test and technical metadata, never your conversation. No account access, no
email. Details (collection, storage in Germany, retention): https://dhsdigital.eu/tracking/assistants-ia/ (section
« Quelles données le connecteur collecte-t-il ? »).

---

## En français

**Testez votre tracking** est l'outil gratuit de DHS Digital qui teste le tracking d'un site à partir de son adresse
publique, sans compte, sans clé et sans extension : Google Tag Manager, GA4, Google Ads, Meta, TikTok, Pinterest,
bannière cookies, Consent Mode v2 et tracking server-side (Stape, Addingwell…). Il donne un score sur 100, les
correctifs par ordre d'importance, ce qui est déjà en place et ce qui ne se vérifie pas de l'extérieur.

- Sur le web : https://dhsdigital.eu/tracking/
- Dans Claude, ChatGPT, Codex, Cursor ou Le Chat : connecteur MCP `https://dhsdigital.eu/wp-json/dhs-audit/v1/mcp`
  (aucune authentification), mode d'emploi : https://dhsdigital.eu/tracking/assistants-ia/
- Rapport texte : `https://dhsdigital.eu/wp-json/dhs-audit/v1/rapport?site=votre-site.fr`
- Méthode complète, pas à pas : [audit de tracking gratuit en 4 étapes](https://daniloduchesnes.com/blog/audit-tracking-gratuit/) ·
  comparatif honnête des outils : [quel outil gratuit pour auditer votre tracking ?](https://dhsdigital.eu/tracking/outils-gratuits/)
- Pages dédiées : [Consent Mode v2](https://dhsdigital.eu/tracking/consent-mode-v2/) ·
  [tracking server-side](https://dhsdigital.eu/tracking/server-side/) · [Shopify](https://dhsdigital.eu/tracking/shopify/) ·
  [pixel Meta](https://dhsdigital.eu/tracking/pixel-meta/)

Lecture seule : l'outil ne se connecte à aucun compte et ne modifie rien. Un résultat non lisible est déclaré « non
concluant », jamais noté au hasard.

### Questions fréquentes

**« Audite mon tracking » : comment faire avec un assistant IA ?**
Donnez l'adresse du site à l'assistant (« Audite le tracking de www.mon-site.fr »). Avec le connecteur MCP ci-dessus,
l'assistant appelle l'outil `tester_tracking` et reçoit le rapport. Sans connecteur, ouvrez
https://dhsdigital.eu/tracking/ et entrez l'adresse du site : le rapport s'affiche en quelques secondes.

**Comment auditer le tracking d'un site : GA4, pixel Meta, Consent Mode v2 ?**
Testez votre tracking lit la page et ses conteneurs Google Tag Manager publics. Il indique si GA4 est installé, si le
pixel Meta et les autres plateformes publicitaires sont présents, si une balise est en double, si une bannière cookies
est détectée et comment Consent Mode v2 est réglé (état par défaut, les 4 signaux v2, consentement accordé d'office).

**Quel outil gratuit pour auditer le tracking d'un site e-commerce ?**
Pour un premier état des lieux sans rien installer : Testez votre tracking (gratuit, sans inscription, rapport en
français). Pour suivre un parcours d'achat pas à pas : Google Tag Assistant, Meta Pixel Helper et les événements de
test de Meta. Le comparatif complet : https://dhsdigital.eu/tracking/outils-gratuits/

**Peut-on vérifier un tracking server-side sans accès aux comptes ?**
En partie : l'outil repère un chargement en first-party, un sous-domaine de mesure et les signes d'un serveur de
tagging (Stape, Addingwell…). Ce que le conteneur serveur transmet aux plateformes ne se voit pas de l'extérieur :
le rapport le signale dans ses limites.

**Quel est le meilleur outil gratuit pour auditer le tracking d'un site ?**
Cela dépend de ce que vous voulez voir. Google Tag Assistant et l'aperçu de Google Tag Manager montrent les balises Google
pas à pas (accès au conteneur pour l'aperçu). Omnibug et Chrome DevTools décodent les requêtes réseau. Meta Pixel Helper
contrôle le pixel Meta. Testez votre tracking donne un premier état des lieux global depuis l'adresse du site : sans
extension, sans compte, avec un score sur 100 en français. Il ne remplace pas un test de parcours d'achat, qui demande
un navigateur et un compte de test.

**Quelle différence avec Tag Assistant, Omnibug ou Pixel Auditor ?**
Ces outils s'utilisent dans votre navigateur, pendant que vous parcourez le site. Testez votre tracking lit la page
publique et le conteneur GTM public à votre place : rien à installer, mais il ne voit pas ce qui se passe après un clic
ou un achat.

## About

Built and maintained by [DHS Digital](https://dhsdigital.eu/) (Brussels, Nice), a tracking and paid media agency.
Contact: info@dhsdigital.eu. This repository documents the hosted service; the service itself runs on dhsdigital.eu.
