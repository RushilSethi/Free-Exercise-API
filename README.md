# Exercise API (CDN)

Public-domain exercise data republished as a **static, jsDelivr-friendly API**: one URL for the full catalog, optional per-exercise JSON, and image paths that map cleanly under a single CDN base.

**Repository:** [github.com/RushilSethi/Free-Exercise-API](https://github.com/RushilSethi/Free-Exercise-API)

## Source & this repo

This dataset is **not original research** — it is taken from [yuhonas/free-exercise-db](https://github.com/yuhonas/free-exercise-db), which in turn builds on [wrkout/exercises.json](https://github.com/wrkout/exercises.json). Credit and thanks belong to those projects and their contributors.

What changed here:

- **CDN-first layout** — stable paths meant for jsDelivr (`cdn.jsdelivr.net/gh/RushilSethi/Free-Exercise-API/...`) instead of raw GitHub URLs or a demo site repo.
- **`data/exercises.json`** — single request for the full list (876 exercises). The upstream project used a `dist/` folder for generated bundles; this repo uses `data/` so the path reads as published dataset, not a local build artifact.
- **No app or CI baggage** — exercise JSON, images, schema, and the combined file only.

## Licensing

| Part | License |
|------|---------|
| This repo’s docs, layout, and packaging (e.g. README, CDN paths) | [MIT](./LICENSE) — Copyright (c) 2026 RushilSethi |
| Exercise JSON, images, and `schema.json` | Public domain — see [DATA-LICENSE](./DATA-LICENSE) (Unlicense, via upstream) |

If you use this in a product, keep attribution to the upstream projects below.

## Layout

| Path | Purpose |
|------|---------|
| `data/exercises.json` | One JSON array — primary endpoint for apps |
| `exercises/` | Per-exercise `.json` files and image folders |
| `schema.json` | JSON Schema for a single exercise object |

## jsDelivr URLs

Default branch is `@main`. Pin a [release tag](https://github.com/RushilSethi/Free-Exercise-API/releases) or commit SHA in production for stable URLs.

**Full catalog (recommended)**

```
https://cdn.jsdelivr.net/gh/RushilSethi/Free-Exercise-API@main/data/exercises.json
```

**Single exercise**

```
https://cdn.jsdelivr.net/gh/RushilSethi/Free-Exercise-API@main/exercises/Alternate_Incline_Dumbbell_Curl.json
```

**Images** — values in `images` are relative to `exercises/` (e.g. `Air_Bike/0.jpg`):

```
https://cdn.jsdelivr.net/gh/RushilSethi/Free-Exercise-API@main/exercises/Air_Bike/0.jpg
```

## Usage in a project

```ts
const CDN = "https://cdn.jsdelivr.net/gh/RushilSethi/Free-Exercise-API@main";

const res = await fetch(`${CDN}/data/exercises.json`);
const exercises: Exercise[] = await res.json();

function imageUrl(relativePath: string) {
  return `${CDN}/exercises/${relativePath}`;
}
```

jsDelivr caches file contents aggressively; prefer a tagged version in production (e.g. `@v1.0.0` instead of `@main`).

## Exercise shape

See [schema.json](./schema.json). Fields include `id`, `name`, `force`, `level`, `mechanic`, `equipment`, `primaryMuscles`, `secondaryMuscles`, `instructions`, `category`, and `images`.

## Regenerating `data/exercises.json`

After editing `exercises/*.json`, rebuild the combined file (requires [jq](https://jqlang.github.io/jq/)):

```sh
jq -s '.' exercises/*.json > data/exercises.json
```

## Thanks

- [yuhonas/free-exercise-db](https://github.com/yuhonas/free-exercise-db) — structured JSON, schema, and images used here
- [wrkout/exercises.json](https://github.com/wrkout/exercises.json) — earlier open exercise list
