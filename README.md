# Tallybuy for Hermes Agent

Compare delivered prices for products in Germany and Austria from inside [Hermes Agent](https://hermes-agent.nousresearch.com). The plugin connects Hermes to Tallybuy's public MCP server at `https://tallybuy.com/mcp` and adds one skill, `tallybuy-shopping`, that tells the agent how to use it.

No account, no key and no local code: the package is configuration only (`plugin.json`, `mcp.json`, `skills/`).

## Install

```sh
hermes plugins install tallybuy        # from the Hermes plugin catalog, once listed
# or straight from this repository:
hermes plugins install https://github.com/tallybuy/hermes-plugin
hermes plugins enable tallybuy
```

Without the plugin, add the server by hand: copy `config.yaml.example` into `~/.hermes/config.yaml`, then run `/reload-mcp`.

## What it can do

Six read-only tools:

| Tool                | What it returns                                                                    |
| ------------------- | ---------------------------------------------------------------------------------- |
| `list_markets`      | The markets Tallybuy covers (Germany, Austria) with their currency and language.   |
| `search_products`   | Products matching the words you give, in one market.                               |
| `get_product`       | One product with its variants and content.                                         |
| `list_offers`       | Shop offers for one variant: price, delivery cost, delivered total, seller, time.  |
| `get_price_history` | Past prices for one variant.                                                       |
| `compare_items`     | A few variants side by side.                                                       |

Example prompts:

- "Where is the Sony WH-1000XM5 cheapest in Germany, delivered?"
- "Compare these two monitors for delivery to Austria."
- "Is now a good time to buy this TV, going by its price history?"

## How it answers

The skill sets the rules the agent follows:

- A price or delivery cost Tallybuy does not know is reported as unknown, never as zero.
- Every price comes with its shop and the time it was seen. Prices change.
- No basket totals across shops and no single "winner": the agent shows the offers and you choose.
- The agent sends you only to the offer's own link (`purchase_url`) and does not open it itself.

## Disclosure

- **Network calls.** Every tool call is an HTTPS request to `tallybuy.com`. Nothing else is contacted, and nothing runs on your machine.
- **What the server sees.** Your IP address and the request, as for any web request. The address is used to limit requests per visitor; it is not linked to a person. Search words are used to answer and are not stored in a database. Details: [privacy notice](https://tallybuy.com/privacy).
- **Commission.** Shop links are affiliate links. Tallybuy may earn a commission if you buy through one, at no extra cost to you. The skill tells the agent to say so.
- **Limits.** Requests are rate-limited per visitor. On `429 Too Many Requests` the agent waits for `Retry-After` before trying again.
- **No telemetry** from the plugin. The tool set is served by `tallybuy.com` at runtime and is not fixed by this repository's commit.

## Support

- Developer page: https://tallybuy.com/developers
- Issues: https://github.com/tallybuy/hermes-plugin/issues

## License

MIT, see [LICENSE](LICENSE).
