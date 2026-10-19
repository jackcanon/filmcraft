# Nested thumbnail reuse and transformed sequence pictures

Sequence item thumbnails use bounded dependency identities as described in
[thumbnail-revisions.md](thumbnail-revisions.md). Reusing an unchanged inner picture removes
background jobs after an outer-only edit. That does not by itself establish faster monitor updates.

For a plain, fully opaque Normal-blend nest with matching dimensions and color settings, the
renderer can keep a transformed nest as a clipped source-sized working image and let the compositor
place it. It avoids transforming that image into a full output-sized intermediate canvas on the
processor. The existing identity-nest path remains direct layers; unsupported effects, masks,
rotation and other guarded cases keep their reference fallback.

The transformed path follows the reference mip-halving choice, then adds a transparent one-texel
border so edge samples do not clamp opaque pixels into transparent space. `PlanLayer::prefiltered`
marks that working image so the graphics processor does not add another sampling pass. Ordinary
source layers retain their existing sampling. Allocation sizes are checked and bounded; a rejected
working image falls back. No serialized project field or decoding behavior changes.

Generated tests use visible, nonuniform pictures and compare the group result with the reference,
including fractional placement, reduced previews, minification, captions and guarded fallbacks.
Thumbnail tests additionally verify cached picture reuse, changed dependencies, edit history, pool
refreshes and stable idle keys when more cards are visible than the memo can retain.

Native measurements must record actual display assignment and refresh rate. Keep the whole window
on the same display with the same size, preview, footage, frame and thumbnail settings for both
builds. Retain source/build fingerprints, exact exported-picture comparisons and source-integrity
checks. A private matched placement scaffold may control the experiment but must be identical on
both sides and disclosed separately from the production patch; no hard-coded display placement is
part of this contribution.

Use repeated baseline/candidate/baseline brackets with acceptance written before measurement. Keep
invalid runs and their rejection reason, and retain valid results that miss the target. Separate
completed-job work from command-to-current-monitor time, which includes control requests and
interface update cadence. Neither measurement establishes physical display presentation latency,
sustained playback capacity or support for every camera mode. Camera integration on a separately
tracked decoder-capable base does not establish decoding support on another source tree. Private
recordings, projects, derived pictures and captures stay outside the source contribution.
