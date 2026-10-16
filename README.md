# Nano Banana API (`gemini-2.5-flash-image-preview`)

<!-- conv-kit:v1 -->

<p align="center">
  <img src="assets/badges/price.svg" alt="observed unit price"> <img src="assets/badges/billing.svg" alt="billing model"> <img src="assets/badges/compat.svg" alt="OpenAI-compatible endpoint">
</p>

<p align="center">
  <img src="assets/01-product-hero-perfume.jpg" width="820" alt="Sample render via the APIMart image API">
</p>

> **from $0.0125 per image** — un seul endpoint compatible OpenAI sur `https://api.apimart.ai/v1`, sans abonnement mensuel. *(relevé le 2026-10-12)*

**[Obtenir une clé API](https://go.apimart.ai/k-17be45)** · **[Tarifs en direct](https://go.apimart.ai/k-c4cf16)** · **[Page du modèle](https://go.apimart.ai/k-00a9b2)** · [⚡ Démarrage en 60 secondes](#quickstart)

**Pourquoi les équipes passent par APIMart pour Nano Banana (`gemini-2.5-flash-image-preview`)**

- **Une clé, tout le catalogue.** La même base URL `https://api.apimart.ai/v1` et le même en-tête `Authorization` donnent accès à Nano Banana (`gemini-2.5-flash-image-preview`) et à plus de 300 autres modèles d'image, de vidéo et de langage — changez le champ `model`, pas votre client.
- **$1 minimum, paiement à l'usage.** Ni abonnement ni forfait prépayé à dimensionner à l'avance : rechargez à partir de $1 et consommez-le en appels.
- **Le montant revient dans la réponse.** Chaque tâche terminée indique le montant facturé (`cost` / `credits_cost`) ; une tâche marquée `failed` est remboursée.
- **Asynchrone par conception.** Soumettez, récupérez le `task_id`, interrogez `GET /v1/tasks/{id}` — le traitement par lots et les retries relèvent d'une file d'attente ordinaire, pas d'une intégration sur mesure.

<!-- /conv-kit:v1 -->

**Aussi appelé:** Nano Banane · la Banane

Nano Banana is Google's Gemini 2.5 Flash Image route on APIMart: fast text-to-image and reference-image editing through one OpenAI-compatible endpoint.

## Model id and routes

| Route | `model` value | Billing | Notes |
| --- | --- | --- | --- |
| Per-image | `gemini-2.5-flash-image-preview` | per delivered image, by resolution | see the [model page](https://go.apimart.ai/k-00a9b2) for the exact tiers |

Endpoint: `POST https://api.apimart.ai/v1/images/generations`, then poll `GET /v1/tasks/{task_id}`.

## Pricing

<!-- pricing:model:start -->
| Output | List price | Effective price |
| --- | --- | --- |
| default | 0.015625 | 0.0125 |


<!-- pricing:model:end -->

Prices are a snapshot; the [pricing page](https://go.apimart.ai/k-c4cf16) and [`data/model.json`](data/model.json) are refreshed by CI, and a completed task reports the exact amount in its `cost` field.

## Request parameters

| Field | Type | Default | Notes |
| --- | --- | --- | --- |
| `model` | string | required | the model id shown in the route table above |
| `prompt` | string | required | what to generate, or what to change when editing |
| `size` | string | `auto` / `1:1` | aspect ratio, or `auto` where the route supports it |
| `resolution` | string | `1K` | output resolution tier — cost scales with it |
| `n` | integer | `1` | number of images per task, where the route allows more than one |
| `image_urls` | string[] | — | reference images as public HTTP(S) URLs or base64 data URLs |
| `nsfw_check` | boolean | `false` | runs `omni-moderation-latest` before submitting |

The table lists the fields accepted by the APIMart `images/generations` endpoint. Check the [model page](https://go.apimart.ai/k-00a9b2) for anything this model adds on top.

## Quickstart

```bash
curl --request POST --url https://api.apimart.ai/v1/images/generations \
  --header "Authorization: Bearer $APIMART_API_KEY" --header 'Content-Type: application/json' \
  -d '{"model":"gemini-2.5-flash-image-preview","prompt":"A bamboo forest at sunrise, slow push-in","size":"1:1","resolution":"1K"}'
```

```python
import os, time, requests

BASE = "https://api.apimart.ai/v1"
HEADERS = {"Authorization": f"Bearer {os.environ['APIMART_API_KEY']}", "Content-Type": "application/json"}

created = requests.post(f"{BASE}/images/generations", headers=HEADERS, timeout=60, json={
    "model": "gemini-2.5-flash-image-preview", "prompt": "A bamboo forest at sunrise, slow push-in",
}).json()
task_id = created["data"]["id"]

while True:
    task = requests.get(f"{BASE}/tasks/{task_id}", headers=HEADERS, timeout=60).json()["data"]
    if task["status"] in ("completed", "failed"):
        break
    time.sleep(5)
print(task.get("cost"), task["result"]["images"][0]["url"])
```

Runnable versions: [`examples/`](examples). The task lifecycle is `pending → processing → completed | failed`.

## FAQ

**Quel est l'id du modèle Nano Banana ?**

`gemini-2.5-flash-image-preview`. Voir le tableau des routes ci-dessus.

**Comment la facturation fonctionne-t-elle ?**

**from $0.0125 per image**. La tâche terminée indique le montant exact dans son champ `cost` ; une tâche échouée est remboursée.

**How do I get a key?**

Use the [Get an API key](https://go.apimart.ai/k-17be45) link — $1 minimum top-up, no plan required.

<!-- related-repos:auto -->
## Related repositories

- **Same model, this language** — the other half of this pair: [nano-banana-prompts-fr](https://github.com/f84q06w110/nano-banana-prompts-fr)
- **Same model, other languages** — 9 more language builds: 中文 [nano-banana-api-cn](https://github.com/mpflv/nano-banana-api-cn) · Deutsch [nano-banana-api-de](https://github.com/uh6cob8op/nano-banana-api-de) · Español [nano-banana-api-es](https://github.com/gbm5mfl1/nano-banana-api-es) · Relay/gateway [nano-banana-api-gateway](https://github.com/gix2kust/nano-banana-api-gateway) · Bahasa [nano-banana-api-id](https://github.com/vsa185x9ecrd/nano-banana-api-id) · 日本語 [nano-banana-api-ja](https://github.com/e4ylndl3g-v/nano-banana-api-ja) · 한국어 [nano-banana-api-ko](https://github.com/l-2jb09sq/nano-banana-api-ko) · Português [nano-banana-api-pt](https://github.com/erujt71i1lx/nano-banana-api-pt) · Русский [nano-banana-api-ru](https://github.com/ggpbi40ss/nano-banana-api-ru)
- **More models in this language** — [midjourney-api-fr](https://github.com/apimartt1q/midjourney-api-fr) · [gemini-2-5-flash-image-preview-api-fr](https://github.com/haftslut7ha/gemini-2-5-flash-image-preview-api-fr) · [gemini-3-1-flash-image-preview-api-fr](https://github.com/ok71ckyc/gemini-3-1-flash-image-preview-api-fr) · [nano-banana-2-api-fr](https://github.com/ms0p12t/nano-banana-2-api-fr) · [gemini-3-pro-image-preview-api-fr](https://github.com/w-44vba/gemini-3-pro-image-preview-api-fr) · [nano-banana-pro-api-fr](https://github.com/sgta2m/nano-banana-pro-api-fr)
- **Topic hubs** — every build of this model, all languages: [nano-banana](https://github.com/topics/nano-banana) · [nano-banana-api](https://github.com/topics/nano-banana-api)

This repository is one build in a cross-linked set of API guides for the same model family; each language build links back to the others.
<!-- /related-repos:auto -->

## Attributed links (how this repository is measured)

| Purpose | Attributed link | Target |
| --- | --- | --- |
| Model page | <https://go.apimart.ai/k-00a9b2> | `apimart.ai/fr/model/nano-banana-api` |
| Live pricing | <https://go.apimart.ai/k-c4cf16> | `apimart.ai/fr/pricing` |
| Get an API key | <https://go.apimart.ai/k-17be45> | `apimart.ai/fr/keys` |

Outbound APIMart links are minted through the promo link API; hand-made tracking parameters are rejected by
`tools/check_links.py` in CI.

## Disclosure

Nano Banana is a third-party model served through APIMart; this repository documents how to call it and publishes
model ids and prices, and does not claim official status. Model names, prices and documentation belong to their
respective owners.

## License

MIT — see [LICENSE](LICENSE).
