# plugin-serpdive

Web search for [elizaOS](https://elizaos.ai) agents through [SERPdive](https://serpdive.com). One call returns the extracted, answer-ready content of each source page instead of a list of links, cleaned and sized for a context window, so your agent can quote and cite facts straight from the response.

Measured, not asserted: on a [public, replayable 1,000-question benchmark](https://github.com/edendalexis/serpdive-benchmark) judged blind by an independent model, SERPdive runs at the same speed as Tavily, feeds the LLM **20.2% fewer tokens**, and wins **60.7% of decided quality duels** against Tavily's default search.

## Install

```bash
bun add plugin-serpdive
```

Set your API key — free at [serpdive.com](https://serpdive.com/dashboard/keys), 1,000 credits every month, no card:

```bash
SERPDIVE_API_KEY=sd_live_...
```

Then add the plugin to your character:

```ts
import { serpdivePlugin } from "plugin-serpdive";

export const character = {
  name: "Researcher",
  plugins: [serpdivePlugin],
};
```

## What it does

Registers a `ServiceType.WEB_SEARCH` service and the `"web"` search category, so web, news, and current-information queries route to SERPdive. It is opt-in and registers no actions, providers, evaluators, or routes.

**Only one web-search provider is active per agent** — pick this one *or* another provider, not both.

### Retrieval depth

| elizaOS `searchDepth` | SERPdive model | Behaviour |
| --- | --- | --- |
| `basic` (default) | `mako` | The fact-carrying sentences of each source. Fast, 1 credit. |
| `advanced` | `moby` | The full readable content of every page. Slower, 1.5 credits. |

A native `model` option takes precedence when provided.

### Honest mapping notes

- **News** is plain search: SERPdive infers freshness and locale from the query itself, so `days` and `freshness` are accepted for compatibility but not forwarded.
- **Images and videos** reuse web search and the `images` array stays empty — SERPdive returns no images, and results are never fabricated.
- **`getPageInfo`** is a direct fetch, not a SERPdive call, matching the behaviour of `plugin-web-search`.
- Without `SERPDIVE_API_KEY` the service boots inert and throws a descriptive error on first use rather than crashing agent startup.

## Development

```bash
bun install
bun run build       # tsup, ESM + types
bun run test        # vitest
bun run typecheck
```

## Links

- [SERPdive docs](https://serpdive.com/docs)
- [Public benchmark](https://github.com/edendalexis/serpdive-benchmark)
- Contact: contact@serpdive.com

## License

MIT
