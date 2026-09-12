# Add a new demo station

Governed skill: add a new organ station to KCP Playground — a self-contained
`demos/<slug>/index.html` that runs real decision logic in the browser, prints a
genuinely signed receipt, and is correctly wired into the site's numbering and nav.

## Preconditions

- Know which organ the new station demonstrates (the existing sixteen: the playbook,
  what it does, what it concludes, what it remembers, autonomous, what it spends,
  one adjudicator, the enabler, the trail, the composition, what it spends to know,
  what it spends unattended, what it can prove it spent, your own coding agent) and
  which real `kcp-agent`/`kcp-harness` function it lifts verbatim — or, if the browser genuinely
  cannot run the real code (it needs project signing keys, server-side state, etc.),
  say so explicitly in the station's copy, the way `memory` and `poisoned-playbook`
  already do. Do not silently ship a simulation as if it were the real thing.
- Read one existing station close to what you're building (`demos/shopping/index.html`
  is a good short reference) to copy its shape: header, `.stage`/`.card`/`.printer`
  layout, `nav` footer, script block.

## Steps

1. **(read)** Pick the next station number `N` (one past the current total) and its
   slug. Note the current total `T` — it appears hardcoded in every station's header
   (`NN / T`) and footer (`Station NN of T`), in `index.html`'s `.foot` paragraph, in
   `README.md`, and in `knowledge.yaml`'s `description`. All of these say "twelve"
   today; every one needs to become "thirteen" (etc.) in the same change.
2. **(write)** Create `demos/<slug>/index.html`: self-contained page linking
   `../../assets/shell.css` and `../../assets/shell.js`, header
   `<N as 2 digits> / <T> · <organ>`, and a script block implementing the decision
   logic (verbatim from source where possible — say so in the `.foot` copy) that
   calls `KCP.printReceipt(well, spec)` to render the verdict. Never hand-build
   receipt HTML — `printReceipt` is what actually signs and verifies with
   `crypto.subtle`.
3. **(edit)** Add the `nav` footer to the new page (`← <prev station>`,
   `All stations`, `<next station> →`), *and* fix the neighbors: the previous last
   station's `next →` link and the new page's own position in the sequence.
4. **(edit)** Add a `<a class="st">` card to `index.html`'s `.stations` grid, in
   order, with the new `<span class="n">NN</span>` and organ label.
5. **(edit)** Renumber every other station's `NN / T` header and `Station NN of T`
   footer for the new total `T`, and update the "Twelve stations" (etc.) prose in
   `index.html`'s `.foot`, `README.md`, and `knowledge.yaml`'s `description`.
6. **(edit)** If the new station demonstrates a genuinely new organ label, add it to
   the organ vocabulary consistently (index.html card + station header both use it).

## Verification

`grep -rnE "(/|of) 12\b" demos/ index.html README.md knowledge.yaml` (swap 12 for the
old total) — every hit should be the *new* total; any leftover old-total hit (header
`NN / <old>` or footer `Station NN of <old>`) means a file was missed. This exact miss
has happened before: `demos/auditors-thursday/index.html` still reads "Station 11 of
11" from before station 12 was added — check it isn't hiding a second stale one
elsewhere. Then open
`index.html` in a browser, click through to the new station and its neighbors, and
confirm: the nav links resolve both directions, presets/controls drive a visible
verdict change, and the receipt's `verify(receipt)` / `verify(tampered)` lines show
`✓ valid` / `✓ rejected`. There is no build step and no test suite here — a clean
click-through in a real browser is the verification.

## Rollback

`git restore demos/<slug> index.html README.md knowledge.yaml` (or `git rm -r
demos/<slug>` if the directory is new and unstaged) — nothing here is server state,
so reverting the files fully reverts the change.
