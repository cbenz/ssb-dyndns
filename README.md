# DynDNS-like service with Scuttlebutt

Very experimental. It's more a tutorial on how to use [Scuttlebutt](https://www.scuttlebutt.nz/) via [Scuttlebot](http://scuttlebot.io/).

## Install

Notes:
- *ssb* is a shortcut for Scuttlebutt
- Scuttlebot is the CLI tool (`sbot` command) for Scuttlebutt
- We install `sbot` command locally via npm

```sh
mkdir ssb-dyndns
cd ssb-dyndns
npm install scuttlebot
```

## Start server

Notes:
- `ssb_appname=ssb-dyndns` env variable stores content in `~/.ssb-dyndns` (by default it's `~/.ssb`)

From a first tab:

```sh
export ssb_appname=ssb-dyndns
export PATH=$PATH:$(npm bin)
sbot server
```

## Publish an IP change

Notes:
- We introduce a new `ssb-dyndns-ip-change` message type

From another tab:

```sh
export ssb_appname=ssb-dyndns
export PATH=$PATH:$(npm bin)
sbot publish --type ssb-dyndns-ip-change --ip $(curl http://ident.me/)
```

## Query the latest IP

```sh
sbot logt ssb-dyndns-ip-change | jq --slurp --raw-output ".[-1].value.content.ip"
```

Should display an IP like `121.121.212.212`.

## Replicate the log

If you want to share the log between many machines, each of them should [join](http://scuttlebot.io/docs/social/join-a-pub.html) the same pub.

If you want to keep data private (the IP addresses) or don't want to pollute the log with the data of other people on the pub, then you might want to [create your own pub](https://scuttlebot.io/docs/config/create-a-pub.html).

## Systemd service

After the machine gets an internet connection, systemd can trigger a script which published the new IP address in the Scuttlebutt log.

TODO

## See also

- http://scuttlebot.io/docs/message-types/custom-types.html
- https://www.scuttlebutt.nz/modules.html
