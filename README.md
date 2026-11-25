# IDS variant spectrum, and what it means for editing in MPS II

I am a biologist, and over the past weeks I have been learning to work with
variant data. I used it to ask a question about mucopolysaccharidosis type II:
if genome editing is going to treat MPS II one day, how much of the *IDS*
mutation spectrum could a base editor actually reach?

There are two parts to this. The first asks what base-editing chemistry allows
in principle. The second asks what is left once guide RNAs, PAM sites and
neighbouring bases are taken into account.

## What this is, and what it is not

This is not supervised research and it has not been formally peer-reviewed. I am a biologist, currently doing a master's in bioinformatics, and the code here is plain — written to be checked rather than to be clever. The question is mine, and so are the decisions about what to count and what to exclude.

I developed the analysis step by step, making my own attempts at writing the code and working through the problems I encountered. I used an AI assistant to help identify errors, correct mistakes and refine the code where needed. My aim was to understand the process, not simply to obtain results. I ran every cell myself and examined the outputs. I also wrote the reports in English, using AI to improve their grammar, wording and clarity.

Any mistakes here are mine.

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

But a bystander only disqualifies a guide if it changes the protein. I went
back and checked: I rebuilt the reading frame from each guide, applied the
change the editor would make, and translated it. Eleven of the bystanders leave
the amino acid unchanged, which lifts the count from five variants to **ten** —
**2.5%** of the spectrum. Two variants have intronic targets and are not
assessed this way. The number moved in favour of base editing, against the
argument I had been making.

Whether a bystander matters at all turns out to depend almost entirely on where
it falls inside the codon. In the third position it leaves the amino acid
unchanged 96% of the time; in the second position it did so in none of 115
cases, which is what the genetic code requires. The editing window knows nothing
about codons, so where a bystander lands is blind — but what it does is not.

Relaxing the PAM requirement helps a great deal and then stops: even an editor
with no PAM requirement whatsoever reaches only 13.2% with a strictly
bystander-free guide, or **16.5%** judged at the level of the protein. Narrowing
the editing window from five positions to three lifts that to **19.5%** — just
over ninety per cent of what the chemistry allows, and never past 21.5%, because
that ceiling belongs to the chemistry and not to any reagent.

Which means the bystander problem is mostly a question of how wide the editing
window is. Of the fifteen variants that no PAM reaches cleanly, fourteen are
blocked by a single neighbouring base, and a narrower window recovers ten of
them. **Five** variants in the whole gene carry a protein-changing bystander in
every possible placement, and no amount of guide design reaches those.

So the first report was generous, but not in the way I expected. Two of the three
constraints turned out to be engineering problems that give way. The one that
does not is the chemistry: base editors install transitions, and four fifths of
the reported spectrum needs something else.

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
edges in reality, and I did not look at off-target binding. Where a bystander
leaves the amino acid unchanged I treat the guide as clean, which says nothing
about any effect on splicing or on translation. And since fourteen of the
fifteen hardest variants are decided by a single base, the numbers lean on that
hard-edged window more than I would like. All of this is set out in the second
report.

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
| `How_I_did_this_and_why.pdf` | The reasoning behind every decision, and the two errors I caught |
| `REPORT.pdf` | What base-editing chemistry allows in principle |
| `REPORT2.pdf` | What is left once guides, PAM sites and bystanders are counted |
| `01_explore_variants.ipynb` | The first analysis |
| `02_pam_accessibility.ipynb` | The second analysis |
| `03_bystander_consequences.ipynb` | Whether each bystander edit changes the protein |
| `04_pam_consequences.ipynb` | The same question across every PAM and every window |
| `data/` | ClinVar export, and the *IDS* reference sequence from GRCh38 |
| `results/` | Figures, the guide list for the 22 reachable variants, and the summary tables |

Python 3.11+, pandas and matplotlib. All four notebooks run top to bottom.

---

Pantelis Mastakoulis\
National and Kapodistrian University of Athens