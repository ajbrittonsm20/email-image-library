# Email Image Library

Multi-client image library for automated email design. Each client has a folder under `clients/` containing their imagery and a machine-readable `manifest.json` that Claude Design (or any LLM tool) reads to select the right images for an email.

## Structure

```
clients/
  <client-slug>/
    manifest.json      # catalog of this client's images
    heroes/            # 1200x600-ish top banners
    products/          # product shots
    logos/             # brand marks
    lifestyle/         # photography
    banners/           # promos, dividers, CTAs
shared/
  manifest.json        # agency-wide assets (social icons, dividers)
index.json             # list of all clients + manifest paths
```

## Manifest entry schema

```json
{
  "id": "hero-summer-sale-2026",
  "path": "clients/acme/heroes/summer-sale.png",
  "url": "https://cdn.jsdelivr.net/gh/ajbrittonsm20/email-image-library@main/clients/acme/heroes/summer-sale.png",
  "category": "hero",
  "tags": ["summer", "sale", "promo", "beach"],
  "width": 1200,
  "height": 600,
  "alt": "Summer sale banner with beach scene",
  "notes": "",
  "added": "2026-07-09"
}
```

- `url` is the public CDN URL (jsDelivr) — this is what goes into email HTML.
- `category` is one of: `hero`, `product`, `logo`, `lifestyle`, `banner`, `icon`.
- `alt` doubles as accessible alt text in the email HTML.

## Using with Claude Design

Add to the client's design-system prompt:

> Image library: fetch `https://cdn.jsdelivr.net/gh/ajbrittonsm20/email-image-library@main/clients/<client-slug>/manifest.json`. Select images by `category` and `tags` that match the email's theme. Use the `url` field as the `src` of `<img>` tags and the `alt` field as alt text. Only use images from this manifest.

## Adding images

Images are ingested and auto-tagged by the agency's Claude agent (upload images to the agent and say "ingest these for <client>"). The agent analyzes each image, writes tags/alt/category, uploads the file, and updates the manifest.

## Notes

- jsDelivr caches by branch ref; `@main` URLs update within ~24h of a push. For instant-fresh URLs, pin to a commit SHA.
- Keep individual images under ~5MB; repo total should stay under ~2GB. If we outgrow this, split into per-client repos or move binaries to object storage.
