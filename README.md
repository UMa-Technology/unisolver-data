# unisolver-data

Star databases for [unisolver](https://github.com/UMa-Technology/unisolver), an offline plate solver.
Every file is an asset of the [`v3` release](https://github.com/UMa-Technology/unisolver-data/releases/tag/v3);
this repository itself holds no data.

## What the release holds

| Asset | Contents | Field of view | Download | Installed | Platforms |
|---|---|---|---|---|---|
| `manifest-v3.json` | Index of the files below: names, sizes, SHA-256, field of view, licence | | | | |
| `unisolver_10_80-<hash>.db.zst` | Wide tier (also bundled with the Flutter plugin) | 10–80° | 14 MB | 29 MB | all |
| `unisolver_5_10-<hash>.db.zst` | Tier N1 | 5–10° | 23 MB | 41 MB | all |
| `unisolver_2p5_5-<hash>.db.zst` | Tier N2 | 2.5–5° | 108 MB | 165 MB | all |
| `unisolver_1_2p5-<hash>.db.zst` | Tier N3 | 1–2.5° | 287 MB | 400 MB | desktop |
| `unisolver_narrow-<hash>.idx.zst`, `unisolver_narrow-<hash>.stars.zst` | Narrow-field package (index and star file) | 0.18–3.1° | 2.5 GB | 3.2 GB | desktop |
| `unisolver_names-<hash>.bin` | Object names in 13 languages (optional) | | 0.2 MB | | all |
| `unisolver_names-source-<commit>.tar.gz` | Corresponding source of the names file | | | | |

`<hash>` is the last 8 hex digits of the file's SHA-256, so a file never changes under the same name.
The databases need unisolver 0.5.0 or later (`min_engine` in the manifest).

## Using it

With the Flutter plugin, point `DbManager` at the release:

```dart
final manager = DbManager(
  dir: (await getApplicationSupportDirectory()).path,
  baseUrl: 'https://github.com/UMa-Technology/unisolver-data/releases/download/v3/',
  register: (path) => pool.register(dbPath: path),
  registerPackage: (index, stars) =>
      pool.registerNarrow(indexPath: index, starsPath: stars),
);
final manifest = await manager.fetchManifest();
```

Anything else can read `manifest-v3.json` directly: download `base_url + key`, check `sha256`,
decompress the `.zst` files with zstd and check `raw_sha256`.

## Licences

Each entry of `manifest-v3.json` carries its own `license` and `attribution`; see [LICENSE.md](LICENSE.md).
