# bakerish-rails-pg-example

End-to-end validation fixture for [bakeri.sh](https://github.com/pirj/bakeri.sh)
+ [pirj/setup-bakerish@v2](https://github.com/pirj/setup-bakerish).

The point: drive `pirj/setup-bakerish@v2` from a real GH Actions
workflow against a real public project, measure the cold-and-warm
wall-clocks, validate the full stack works outside the maintainer's
M3.

## What's in here

- `Dockerfile` — minimal `ruby:3.2-alpine` + apk build deps. Stand-in
  for "your Rails app's container".
- `docker-compose.yml` — postgres:16-alpine + the app, with
  postgres healthcheck and `depends_on: db: service_healthy`.
- `bakerish.toml` — declares `[memory] size = "4G"` for the
  docker-compose live snapshot + a trivial `[prebuild.pg-ready]`
  layer that pings the warm DB.
- `.github/workflows/ci.yml` — uses `pirj/setup-bakerish@v2`, runs
  `bake run` against the fixture, times the whole thing.

## Expected timings (from the round-6 M3 bench for reference)

| Path | M3 / HVF | GH Linux runner / KVM |
|---|---|---|
| Cold (no cache) | ~250 s | TBD — that's what this fixture measures |
| Warm (cache restored from prior run) | ~2.7 s | TBD |
| `bake run` of `echo from-vm` on a running VM | ~220 ms | TBD |

The GH numbers go into this README's `## Results` section after the
workflow runs a few times.

## Results

(populated after the first few CI runs)

## License

MIT — same as the upstream bakeri.sh stack.
