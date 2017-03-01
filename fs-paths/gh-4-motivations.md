This is related to the work of #3: Sorting out an Address Scheme that fits with Browser Security Model.

Also see discussion in #5

## Acceptance Criteria

- [ ] Write a summary of how we want the namespace and address scheme to work, what actual problems it solves, what get’s better etc.. And followup with a compromise like this that could be employed in the meantime.

## Background

From @jbenet:

> ** (important note) These goals are secondary in time to getting browser adoption. Meaning that we CAN do things like recommend ipfs:// ipns:// ipld:// IF browser vendors think that it's unlikely to get adoption this way now. We can work on unifying the fs-db-web rift later. We're not dogmatic, we're pragmatic. But we want to make sure we push in the right places and try to make as much as we can better.**

and

> Note also that we expect dns naming (and similar-- eg blockstack, namecoin, ens) to be a thing for a while-- meaning that we can endeavor to make "the easy path" things like ipns://${domain-name}/path

His background explanation for the fs: approach:
> The major reason has to do with unifying FSes, Databases, and the Web with a singular way of addressing all data. It's about undoing the harm that URLs brought unto computing systems by fragmenting the ecosystem. To this day the rift between both worlds prevents simple tooling from working with both, and has much to do with the nasty complexity of working with networked data all the modern target platforms. Sorry, this may sound vague, but it's very specific: addressing of data broke when URLs and URIs were defined as a space OUTSIDE unix/posix paths, instead of INSIDE unix/posix paths (unlike say plan9's 9p transparent addressing). This made sense at the time, but it created a division that to this day force "the web" and "the OS" to be very distinct platforms. Things can be much better. Mobile platforms, for one, have done away with the abstractions in the user facing parts, hiding away the rift from users, and only forcing developers to deal with it (clearly a better UX), but problems still exist, and many apps are hard to write because of it. we'd like to improve things, particularly since "a whole new world" of things is joining the internet (blockchains, ipfs, other decentralized web things). It would be nice if there's a nice compatible way to bridge with the web's expectations (dweb://...) but work towards fixing things more broadly.

@jbenet:
> we'd like to improve things, particularly since "a whole new world" of things is joining the internet (blockchains, ipfs, other decentralized web things). It would be nice if there's a nice compatible way to bridge with the web's expectations (dweb://...) but work towards fixing things more broadly.

@gozala:
> I think I’ve seen talk or read about this. While I think that’s a very noble goal, I think it would be hard to sell for a very pragmatic crowd like browser vendors. I frequently see standardization process taking specs into least ambitious and most pragmatic direction, I often disagree, but I think often times that’s only way to make progress. Maybe some version of this goal could be articulated in perfectionistic manner and in a more pragmatic one ?

@jbenet:
> A minor reason is not having to force people to swallow N shemes (ipfs:// ipns:// ipld:// and counting), and instead use one that muxes.

@gozala:
> I think it was in the discussion I’ve quoted, but if not I most definitely got opposite feedback when talking to platform engineers about this. I think part of it is due to the fact it is compatible with existing security model on the web.
>
> To be honest I am much more worried about end users (browser users) perspective on having all this new protocols, regardless if they use separate schemes or a “hostname”. I am afraid either way it would be way too much & more familiar it will look the better off you’ll be.
>
> That being said given that /ipns/${id}/path may be referring to resource under /ipfs/${other-id}/other-path/ it ends up being cross-protocol resource handling that maybe one pragmatic reason to go for single protocol, but I still don’t think it’s going to be very strong one.
>
> My impression more and more has being that ipfs/ipns/ipld are internal details of IPFS and less relevant to the "dweb-apps”. Or to put it differently IPFS already has CIDs to encode several pieces of information about the content in a single ID, have you considered maybe encoding the ipfs/ipns/ipld  bits into it as well ? It may make things slightly less readable on one hand but on the other hand could unite everything under one protocol and keep it open for further extensions and require less coordination with embedders.

...

@gozala
> 👍 I would still encourage to write a summary of how you want it to be, what actual problems it solves, what get’s better etc.. And followup with a compromise like this that could be employed in the meantime.
