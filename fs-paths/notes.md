- /ipfs/Qmfoo => dweb://Qmfoobase16/ipfs/Qmfoo
- /ipns/Qmbar => dweb://Qmbarbase16/ipns/Qmbar
- this retains the original path, but still has origins
- for the price of duplicating the hash in there, but if it's just an internal mapping anyhow i think that's okay
- the host/authority portion would be transparent except for its use as an origin, and would be defined as MUST be the base16 v1 cid of the thing in the path
- reason: it might be important that path is consistent
- any code in any stage of the upgrade-path sees /ipfs/Qmfoo as the path


browser motivation:
- all the networked data
- fully distruted networking





---




where is CSP applied?
anyhow, ipfs://$CIDv1b32/path/within and same ipns:// probably sgtm
but do keep in mind what jbenet said about absolute paths

---

fs protocol handler redirection
- sooo great!
- HSHCA https://github.com/neocities/hshca
  - for $cid.neocities.org
- v0b58 -> v1b32 mmmh
  - nobody would be aware of the v1 hash
  - hah, but ipfs could transparently convert it back to v0b58
    - or publish both hashes to the dht

---

"In  cases of directories ipfs node generates links that are somewhat
incompatible with"

TODO

---

gateway directory redirect
- yes exactly as jbenet said. workaround for relative linking within dirs

---
