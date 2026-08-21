---
title: On your tailnet
order: 55
description: Run Hanji on a tailnet and let the tailnet sign people in.
---

# On your tailnet

The recommended way to share a Hanji instance with a team is not a domain, a
certificate, and a public endpoint. It is a [tailnet](https://tailscale.com):
the instance runs on one box, `tailscale serve` fronts it, and it never
touches the public internet. That sentence is the whole security posture, and
it is a sentence you can say to an administrator with a straight face.

Tailscale mode adds the part that makes it feel finished: the tailnet already
knows who is knocking, so Hanji stops asking.

## Turn it on

One variable, plus the serve command:

```bash
export HANJI_TAILSCALE_OWNER=you@github   # your own tailnet login
node_modules/.bin/next start -H 127.0.0.1 -p 4100
tailscale serve --bg 4100
```

Setting `HANJI_TAILSCALE_OWNER` does two things at once. It tells Hanji to
trust the identity headers `tailscale serve` attaches, and it names the
tailnet login that is the owner - you. Open the instance from any device on
your tailnet and you are simply in, as the owner, no password asked.

## Let people in

Everyone else maps through Settings → People. Open a person's sheet and link
their tailnet login in the Tailscale field. From then on, that identity signs
them in with no password - and that is all it does. What they can see and
propose stays exactly the grants you gave them; the tailnet answers who they
are, never what they may read. See [[Permissions]].

<figure><img src="assets/person-sheet.png" alt="A person's sheet: their grants, and the Tailscale login field linking their tailnet identity" class="w-50"><figcaption>One person, one sheet: what they hold, and the tailnet identity that signs them in.</figcaption></figure>

An unlinked visitor is told plainly: the login page names the tailnet
identity it saw and says the owner has not linked it yet. The password form
still works underneath, always - a linked identity is a convenience on top of
sessions, not a replacement for them.

This is also how you invite someone from outside your team: share your node
with them in Tailscale, add them as a person, link their login. Share access,
not code.

## The contract

The headers are trustworthy for exactly one reason: `tailscale serve` strips
any inbound `Tailscale-User-*` headers and sets its own. So the mode is safe
only while the port is reachable exclusively through `tailscale serve` - bind
`127.0.0.1`, as above. If the app is reachable any other way, anyone on that
path can type the header themselves, and `HANJI_TAILSCALE_OWNER` must stay
unset. Hanji will never turn this on by itself.

Two edges worth knowing. Signing out on a tailnet is a shrug: clearing the
session lands you right back in through the header, because the tailnet still
knows you. And agents are untouched by all of this - the MCP surface stays
bearer-token only, and a tailnet identity never grants an agent anything.

## Belt and suspenders: ask tailscaled itself

The contract above is a deployment shape. If you want the instance to verify
it rather than assume it, set one more variable:

```bash
export HANJI_TAILSCALE_WHOIS=1
```

On every header sign-in, Hanji now asks the local tailscaled who owns the
connecting address (`tailscale whois`, one lookup per address per minute) and
refuses the header unless the answers match. A forged header alone no longer
signs anyone in; it would take tailscaled itself vouching for the forger's
address, which the tailnet does not do. The check needs the `tailscale` CLI
on the PATH of the user running Hanji; point `HANJI_TAILSCALE_BIN` at the
binary when it lives elsewhere (on a Mac,
`/Applications/Tailscale.app/Contents/MacOS/Tailscale`). When in doubt, turn
it on: the cost is one process spawn per new visitor address, and the failure
mode is a login page instead of a wrongly trusted header.

## Why this is not SSO

It is better, for the case it covers. On a tailnet you do not integrate an
identity provider; you already have one, and it is the network itself.
Single sign-on for organizations that live behind Okta or Google Workspace
remains where [[Where we are]] puts it: the paid seam, later. This mode is
for the team that wants the writing surface working today, with nobody
typing passwords, on infrastructure they already trust.
