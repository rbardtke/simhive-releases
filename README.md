# Simhive releases

Installers for [Simhive](https://github.com/rbardtke/simhive) — your sims on
everyone's machines, your machine for everyone's sims — and `latest.json`,
the manifest installed copies update themselves from. Each release is
signed with the Simhive release key; the app refuses anything else.

The app's source lives elsewhere; this repository only carries builds.

Where things are: the pool and the guide are at https://simhive.app
(`/guide` explains installing and joining), SimulationCraft binaries the app
fetches come from [rbardtke/simc-builds](https://github.com/rbardtke/simc-builds),
and every release here lists the SHA-256 of each file, a VirusTotal report and
the key that signs updates — see "Check what you download" in the release notes.

Running it in Docker instead of the app — on a server, a NAS, a spare Linux
box — is at [rbardtke/simhive-docker](https://github.com/rbardtke/simhive-docker):
one container, with or without the interface.
