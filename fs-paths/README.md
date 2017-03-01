# FS paths and fs: URIs

> Addressing data on the distributed web.

Authors: Juan Benet \<juan@protocol.ai>, Lars Gierth \<lars@protocol.ai>

This document attempts to describe FS paths as currently used in IPFS and IPLD,
their motiviation, and ways of integrating them with modern web browsers.

- [Motivation](#motivation)
- [Definitions](#definitions)
- [Namespaces](#namespaces)
  - [/ipfs namespace](#ipfs-namespace)
  - [/ipns namespace](#ipns-namespace)
  - [/ipld namespace](#ipld-namespace)
  - [/dns namespace](#dns-namespace)
  - [Other namespaces](#other-namespaces)
- [Web browser integration](#web-browser-integration)
  - [FS path vs. URI vs. URL](#fs-path-vs-uri-vs-url)
  - [fs: URI](#fs-uri)
  - [Origins and Content Security Policy](#origins-and-content-security-policy)
  - [Suborigins](#suborigins)
  - [Mapping fs: URI to standard URL](#mapping-fs-uri-to-standard-url)
  - [Mapping standard URL to fs: URI](#mapping-standard-url-to-fs-uri)
- [FS path vs. multicodec vs. multiaddr](#fs-path-vs-multiaddr)
- [Changelog](#changelog)
- [Reading list](#reading-list)

*Note: the name "FS paths" is a working title for the sake of this document.
Another option is "DWeb paths" and dweb: URIs.*


## Motivation

quote juan, to be edited:

The major reason has to do with unifying FSes, Databases, and the Web
with a singular way of addressing all data. It's about undoing the harm
that URLs brought unto computing systems by fragmenting the ecosystem.
To this day the rift between both worlds prevents simple tooling from
working with both, and has much to do with the nasty complexity of
working with networked data all the modern target platforms. Sorry, this
may sound vague, but it's very specific: addressing of data broke when
URLs and URIs were defined as a space OUTSIDE unix/posix paths, instead
of INSIDE unix/posix paths (unlike say plan9's 9p transparent
addressing). This made sense at the time, but it created a division that
to this day force "the web" and "the OS" to be very distinct platforms.
Things can be much better. Mobile platforms, for one, have done away
with the abstractions in the user facing parts, hiding away the rift
from users, and only forcing developers to deal with it (clearly a
better UX), but problems still exist, and many apps are hard to write
because of it. we'd like to improve things, particularly since "a whole
new world" of things is joining the internet (blockchains, ipfs, other
decentralized web things). It would be nice if there's a nice compatible
way to bridge with the web's expectations (dweb://...) but work towards
fixing things more broadly.

A minor reason is not having to force people to swallow N shemes
(ipfs:// ipns:// ipld:// and counting), and instead use one that muxes.

(important note) These goals are secondary in time to getting browser
adoption. Meaning that we CAN do things like recommend ipfs:// ipns://
ipld:// IF browser vendors think that it's unlikely to get adoption this
way now. We can work on unifying the fs-db-web rift later. We're not
dogmatic, we're pragmatic. But we want to make sure we push in the right
places and try to make as much as we can better.

- the concepts of strictly self-described and upgrade path
- church of context vs. church of self-described

- examples of bad URI
- examples of good FS path
- examples of upgrade path

## Definitions


## Namespaces
- a few general namespace rules

### /ipfs namespace

### /ipns namespace

### /ipld namespace

### /dns namespace
- dweb:/dns/ipfs.io
- semantics of this? conflict with /dns multiaddr?
- /dns could be just dnslink+dnsaddr
  - in multiaddrs: dnsaddr
  - in paths: dnslink

### Other namespaces


## Web browser integration
- the concepts of strictly self-described and upgrade path

### FS path vs. URI vs. URL

### fs: URI

### Origins and Content Security Policy

### Suborigins

### Mapping fs: URI to standard URL
- can we intercept addresses starting in a slash, or are these hardcoded to file://?

### Mapping standard URL to fs: URI
- dweb:// XXX what was the mapping again?
- using dnslink lookup parallel to happy eyeballs
  - more pragmatic for now: on first available request event
- need: dns api in browser, happy eyeballs events
- less related: dnsaddr lookup too. gain: completely A/AAAA-less
  - e.g. TXT _dnsaddr.ipfs.io dnsaddr=/ip4/1.2.3.4/tcp/443/p2p/Qmfoo/www
  - with /www being hector's http-over-swarm


## FS path vs. multiaddr
- addressing data vs. addressing networked process


## Changelog

No noteworthy changes yet.


## Reading list

- WHATWG URL: https://url.spec.whatwg.org/
- NURI: https://github.com/ipfs/go-ipfs/issues/1678#issuecomment-157478515
- Path characters: https://github.com/ipfs/go-ipfs/issues/1710
