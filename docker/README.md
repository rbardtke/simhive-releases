# Simhive in Docker

Two prebuilt images, for people who would rather run a container than the
desktop app — a home server, a NAS, a spare Linux box. Nothing to clone,
nothing to compile: SimulationCraft comes from the release channel at the
exact commit the pool runs, and follows the pool from then on.

| Image | What it is | Compose file |
|---|---|---|
| `ghcr.io/rbardtke/simhive` | the interface on port 4747 **and** this machine's cores lent to the pool | `compose.member.yml` |
| `ghcr.io/rbardtke/simhive-worker` | cores only, no interface (a settings page on localhost:4801) | `compose.worker.yml` |

Both need a member token — one per person, the same on every machine you own.
Get one at https://simhive.sidian.app/join (or in the app: Settings → Pool →
Get a token).

## Interface + lending

```bash
mkdir simhive && cd simhive
curl -fsSLO https://raw.githubusercontent.com/rbardtke/simhive-releases/main/docker/compose.member.yml
echo "LOCALBOTS_POOL_TOKEN=paste-your-token-here" > .env
docker compose -f compose.member.yml up -d
```

Open http://localhost:4747. First start downloads simc (35 MB) and the game
tables (60 MB); until then the page says so. Your sims go to the pool, your
cores are lent to it; Settings → Pool sets the thread count, a schedule,
pause. The interface has no login: keep the port on your LAN.

## Worker only

```bash
mkdir simhive-worker && cd simhive-worker
curl -fsSLO https://raw.githubusercontent.com/rbardtke/simhive-releases/main/docker/compose.worker.yml
printf 'WORKER_TOKEN=paste-your-token-here\nWORKER_NAME=my-server\n' > .env
docker compose -f compose.worker.yml up -d
```

The settings page at http://localhost:4801 shows what it is doing and lets
you set threads, a schedule and pause.

## Updating

```bash
docker compose -f compose.member.yml pull && docker compose -f compose.member.yml up -d
```

(same with `compose.worker.yml`). The images are rebuilt on every engine
change; the pool's version column says when a machine is behind. simc moves
by itself — when the pool pins a new build, the running container fetches it
and reconnects, no restart needed.

## What the machine needs

- Docker with the Compose plugin (`curl -fsSL https://get.docker.com | sh` on a fresh Debian/Ubuntu)
- x86-64 Linux — the images and the channel builds are linux-x64 only
- 2 GB RAM for the member image, 6 GB cap in the worker file (simc with many threads)

## Where things live

Named volumes: `simhive-simc` (the current and previous simc build),
`simhive-cache` (game tables), `simhive-history` (your saved sims),
`simhive-config` (this machine's pool settings). Delete the container and
they survive; delete the volumes and everything is fetched again.

## Where the images come from

They are built from the engine repository on every change by a GitHub
workflow, tagged `latest` and with the engine commit; the pool's version
column shows the commit each machine runs.
