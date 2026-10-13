# IDS variant spectrum, and what it means for editing in MPS II

I am a biologist, and over the past weeks I have been learning to work with
variant data. I used it to ask a question about mucopolysaccharidosis type II:
if genome editing is going to treat MPS II one day, how much of the *IDS*
mutation spectrum could a base editor actually reach?

There are two parts to this. The first asks what base-editing chemistry allows
in principle. The second asks what is left once guide RNAs, PAM sites and
neighbouring bases are taken into account.

## What this is, and what it is not

This is not supervised research and it has not been reviewed by anyone. I am a
biologist, currently doing a master's in bioinformatics, and the code here is
plain — written to be checked rather than to be clever. I used an AI assistant
while writing this up and while working through errors in the code; the
analysis, the decisions about what to count and what to exclude, and any
mistakes in it are my own.

I wanted to know, in numbers rather than in impressions, what the current
generation of editors can and cannot do for the people who carry mutations in
this gene. The answer turned out to be narrower than I expected, and I have
tried to report it exactly as I found it, without rounding it toward the result
I would have preferred.

Everything here is built from public data, and every figure can be reproduced
by running the notebooks from top to bottom. Where I was uncertain, I have said
so. Any errors are mine, and I would rather be corrected than left comfortable.

## What I did

I pulled every pathogenic and likely pathogenic *IDS* variant from ClinVar
(395 of them), sorted them by what kind of change they are, and asked, for each
one, whether a base editor could in principle restore the reference sequence. I
was deliberately generous: I ignored PAM availability, the editing window and
bystander bases. So the number I get is a ceiling, not a realistic rate.

## What I found

**85 of the 395 variants — 21.5% — could in principle be corrected by a
conventional ABE or CBE**, judged on the nucleotide change alone. 54 would need
an ABE, 31 a CBE.

The rest are out of reach for two separate reasons. 166 (42%) are insertions or
deletions: base editors change what a base *is*, not how many there are, so no
base editor will ever touch them. Another 144 are single-nucleotide
substitutions, but transversions — outside the scope of conventional ABE and
CBE chemistry.

**And there is no common mutation to target.** 347 variants fall across 167
different codons. The most frequently hit codon accounts for 7 of them — two
per cent. Half the codons carry a single variant each.

So a per-mutation base-editing strategy does not scale here: you would need a
new reagent for almost every variant, and four fifths of the reported spectrum
lies outside conventional ABE and CBE chemistry altogether.

## The second question: how far below the ceiling the real number sits

21.5% is a ceiling, not a rate. I said so in the first report, and then went
back to find out how far below it reality sits.

To correct a variant, a guide RNA has to place the target base inside the
editing window of the editor, and a PAM has to sit in the right place on the
same strand. I took the *IDS* reference sequence from GRCh38, checked that
every one of the 229 single-nucleotide variants sat where ClinVar said it did —
all 229 agreed — and then asked, for each of the 85 correctable variants,
whether such a guide exists.

**Twenty-two do.** That is 5.6% of the spectrum, not 21.5%. Sixty-three have no
usable guide at all.

Then there is bystander editing. If the window holds a second base of the same
kind, it changes too. Only **five** of the 22 have a guide that would edit the
target base and nothing else — **1.3%** of the spectrum.

Relaxing the PAM requirement helps a great deal and then stops: even an editor
with no PAM requirement whatsoever reaches only 13.2% cleanly. Narrowing the
editing window from five positions to three lifts that to 18.0%. Neither
reaches 21.5%, because that ceiling belongs to the chemistry and not to any
reagent.

So the first report was generous. The distance between what base editing
reaches and what a whole-gene approach would reach is wider than it looked.

## What I am not claiming

ClinVar tells you what has been *reported*, not how common a variant is in
patients. These are counts of variants, not of people — estimating how many
patients a given approach would reach would need allele frequencies from a
registry, which I do not have.

There is also a stretch of the gene, c.419 to c.1180, with no variants in my
filtered set at all — I do not think that is biology, more likely
under-reporting or the *IDS2* pseudogene making assignment hard nearby. If
anything, that makes the spectrum look less diverse than it really is.

The second analysis models geometry, not kinetics. Editing windows have soft
edges in reality, I did not look at off-target binding, and I counted any
bystander as disqualifying without checking whether it would change the
protein. All of this is set out in the second report.

And I did not model prime editing, which would extend the range of correctable
variants, including transversions and small indels. It remains
variant-specific: the edit is encoded in the pegRNA, so a different variant
needs a different pegRNA, and efficiency varies enough with sequence context
that each one has to be tested. The scaling problem is reduced, not removed.

## Two things that cost me time

The ClinVar export has a trailing tab on every row, so pandas reads it one
column off — every field silently shifted. I only caught it because the `Name`
column was showing `IDS`. It needs `index_col=False`.

ClinVar also lists protein positions for two *IDS* isoforms, in inconsistent
order (`G451fs, G541fs` in one row, `Q531*, Q441*` in the next). Taking the
first match mixes two numbering systems and invents a gap in the data. I take
the larger value, which matches the `p.` notation ClinVar gives against
`NM_000202.8` in the `Name` field. I checked this: it agrees for all 347 rows
where both are available, with no exceptions.

## Contents

| | |
|---|---|
| `REPORT.pdf` | What base-editing chemistry allows in principle |
| `REPORT2.pdf` | What is left once guides, PAM sites and bystanders are counted |
| `01_explore_variants.ipynb` | The first analysis |
| `02_pam_accessibility.ipynb` | The second analysis |
| `data/` | ClinVar export, and the *IDS* reference sequence from GRCh38 |
| `results/` | Figures, and the guide list for the 22 reachable variants |

Python 3.11+, pandas and matplotlib. Both notebooks run top to bottom.

---

Pantelis Mastakoulis\
National and Kapodistrian University of Athens