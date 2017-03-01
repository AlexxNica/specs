## Acceptance Scenario

- [ ] Propose an address scheme that fits with Browser Security Model
- [ ] Provide working code that shows the address scheme working, and how it can be supported in the browser
- [ ] (optional) #4 Document the motivations for the more general fs: namespace and address scheme
- [ ] #6 Tackle identifying origins with (or without?) fs:// paths
- [ ] #7 Reconcile IPFS absolute addresses with standard URL resolution

## Background

The proposed use of `ipfs/` at the root of addresses in the `fs:` namespace like`fs://ipfs/{content hash}` conflicts with the single-domain content origin policy that is central to the browser security model.  This brings into question the whole idea of a generic `fs:` namespace rather than `ipfs:`




-----

This is part of the address scheme work described in #3

The underlying requirement:

Firefox, for example, implements https://url.spec.whatwg.org/ not the URL RFC. That's what we need to use if we want our urls to be Web-browser compatible

## Background

From @gozala:
> At least in Electron there is no way to make origin be anything other than a hostname, which means all the IPFS content will have either “fs://ipfs” or “fs://ipns” origin. I have starting messing with Gecko and I think it maybe possible to make origin different from hostname but even that I won’t be surprised if things won’t quite work out as expected due to implicit assumptions that hostname is an origin. Either way I’d encourage rethinking addressing as I suspect you’ll have a pushback from browser vendors, not only due to implementation difficulties but more due to introducing a new model that would work different from the  established one.

### Option: include CID in the origin domain

@gozala (2017-02-01)
> I have followed a rabbit hole of implementing a protocol handler for firefox that would handle fs://ipfs/${cid}/path/with-in such that origin would be fs://ipfs/${cid} but unfortunately my fear got confirmed and it’s undoable without making fundamental changes to the firefox code base & specifically to the parts that deal with content security policy. That is bad because, I expect to be a very hard sell given the implications it could have on millions of Firefox users.

@jbenet

> - Hopefully it can be done with re-routing the data flow to fit what firefox expects.
> - We can always define schemes like  ipfs://${cid}/path/...  ipns://${cid}/path/...   ipld://${cid}/path/... if that's so much easier, just note it will make other things hard. utlimately it's about tradeoffs and what we can get away with.
> - We've been considering changing fs://   to dweb:// which is clearer and more inclusive of other projects. and avoids repeating "fs" so much.  (eg   dweb://ipfs/${cid}/path/...   ---
> - Ultimately, i'm confident we can find a scheme and setup that works for FF, Chrome, other browsers, IPFS, and everyone. It may require changes on our side, or clever re-routing of info (as you've been exploring).

@gozala (2017-02-06):
> It would be invaluable to have those things listed somewhere.


@gozala (2017-02-01)
>
> Along the way I got some feedback from the people intimately familiar with the relevant code paths in firefox:
>
> Only visible way to implement something like that would be to roll out new C++ implemented component along the lines of nsIStandardURL and patch nsScriptSecurityManager component so that for that type of URL origin will be computed differently. Then also change nsNetUtil component  that is actually responsible for validating if resource is with in the policy (for example you can see that for file: protocol different origin checks are performed.

### Option: include public key in the paths

@gozala (2-17-02-06)
> Dat for instance is free of this problem as they just use dat://{public_key}/path/with/in so origin is what they want it to be.

## origin domains must be case-insensitive

@gozala:
> I attempted to try ipfs://${hash} and ipns://${id} as an alternative solution to make things work in Electron. Issue there is that hostnames are case insensitive & default hashes used by ipfs are by default case sensitive (base58 encoded). Presumably non all lower case addresses could be transcoded to use base16 encoding to avoid this issue, but even than it is not going to be ideal as user maybe be given an address encoded with base58 and say posting it as a link won’t work as expected. Not sure what is the best solution here but ideally all content addresses will be valid.

@jbenet:

> What if we resolve through to a CIDv1 encoded in the right base (16 or 32) non-transparently? meaning that we actually resolve through from
>
>  fs://ipfs/${CIDv0 or CIDv1 in any base}/path  ->   ipfs://${CIDv1 in base16 or base32}/path
>   fs://ipns/${CIDv0 or CIDv1 in any base}/path  ->   ipns://${CIDv1 in base16 or base32}/path
>   fs://ipld/${CIDv0 or CIDv1 in any base}/path  ->   ipld://${CIDv1 in base16 or base32}/path
>   ipfs://${CIDv0 or CIDv1 in any base}/path  ->   ipfs://${CIDv1 in base16 or base32}/path
>   ipns://${CIDv0 or CIDv1 in any base}/path  ->   ipns://${CIDv1 in base16 or base32}/path
>   ipld://${CIDv0 or CIDv1 in any base}/path  ->   ipld://${CIDv1 in base16 or base32}/path

>
> so that the browser can treat ${CIDv1 in base16 or base32} as the origin hostname?

### a working solution

@gozala (2017-02-01)

> Now good news is with David Diaz’s help and necessary fixes I was able to work out a solution which works as follows:
>
> fs, ipfs and ipns protocol handlers are added added to firefox.
> fs protocol handler essentially just redirects to either ipfs or ipns as follows
>
> fs://ipfs/${cid}/path/with-in/ -> ipfs://${cid_v1_base16}/path/with-in
> fs:ipfs/${cid}/path/with-in/ -> ipfs://${cid_v1_base16}/path/with-in
> fs:///ipfs/${cid}/path/with-in/ -> ipfs://${cid_v1_base16}/path/with-in
> fs:/ipfs/${cid}/path/with-in/ -> ipfs://${cid_v1_base16}/path/with-in
> fs://ipns/${cid}/path/with-in/ -> ipns://${cid_v1_base16}/path/with-in
> fs:ipns/${cid}/path/with-in/ -> ipns://${cid_v1_base16}/path/with-in
> fs:///ipns/${cid}/path/with-in/ -> ipns://${cid_v1_base16}/path/with-in
> fs:/ipns/${cid}/path/with-in/ -> ipns://${cid_v1_base16}/path/with-in
>
> both ipfs and ipns protocol handlers redirect to corresponding base16 encoded CID path
>
> ipfs://${cid_v0_base58}/path/with-in -> ipfs://${cid_v1_base16}/path/with-in
> ipfs:/${cid_v0_base58}/path/with-in -> ipfs://${cid_v1_base16}/path/with-in
> ipfs:///${cid_v0_base58}/path/with-in -> ipfs://${cid_v1_base16}/path/with-in
> ipfs:${cid_v0_base58}/path/with-in -> ipfs://${cid_v1_base16}/path/with-in
>
> ipfs://${cid_v1}/path/with-in -> ipfs://${cid_v1_base16}/path/with-in
> ipfs:/${cid_v1}/path/with-in -> ipfs://${cid_v1_base16}/path/with-in
> ipfs:///${cid_v1}/path/with-in -> ipfs://${cid_v1_base16}/path/with-in
> ipfs:${cid_v1}/path/with-in -> ipfs://${cid_v1_base16}/path/with-in
>
> same with ipns
>
> both ipfs and ipns protocol handlers serve content from local node (that is assumed to be running), meaning that firefox will show URLs on the left but will serve content from URLs on the right.
>
> ipfs://${cid_v1_base16}/path/with-in => localhost:8080/ipfs/${cid_v0_base58}/path/with-in
> ipns://${cid_v1_base16}/path/with-in => localhost:8080/ipns/${cid_v0_base58}/path/with-in
>
> In a consequence to all the redirects everything works under (what I assume to be) desired origin policy where it’s either ipfs://${cid_v1_base16}/ or ipfs://${cid_v1_base16}/ respectively.
