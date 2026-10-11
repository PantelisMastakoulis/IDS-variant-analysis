# Mutation spectrum of *IDS* constrains per-mutation editing strategies in Mucopolysaccharidosis type II

**Pantelis Mastakoulis**\
National and Kapodistrian University of Athens\
October 2026

---

## Summary

Analysis of 395 pathogenic and likely pathogenic *IDS* variants from ClinVar indicates that at most 21.5% are chemically addressable by current base editors, and that the most frequently affected codon accounts for only 2% of annotated variants. These two properties — chemical inaccessibility and the absence of prevalent alleles — are features of the mutational architecture of the gene rather than limitations of any particular reagent, and they place MPS II in a different regime from monogenic diseases where per-mutation correction has succeeded.

---

## Rationale

Genome editing has produced its clearest therapeutic successes in diseases with a favourable genetic architecture. In sickle cell disease every affected patient carries at least one copy of the sickle allele — a single substitution in *HBB* — whether as HbSS, HbSC or HbS/β-thalassaemia, so a single shared target serves the entire population. Notably, the approved editing therapy does not repair that substitution at all: it disrupts an erythroid enhancer of *BCL11A* to re-express fetal haemoglobin, a mechanism that benefits all of these genotypes at once [8]. In Fanconi anaemia, correction efforts have targeted prevalent *FANCA* alleles [9], an approach that is tractable precisely because a subset of alleles recurs across patients.

Mucopolysaccharidosis type II is caused by loss-of-function variants in *IDS*, and more than seven hundred pathogenic variants have been described [2]. Whether this allelic heterogeneity constitutes a practical obstacle to per-mutation correction, or merely an inconvenience, has not been quantified.

This analysis asks a narrow question: of the pathogenic *IDS* variants currently catalogued, what fraction could a base editor address even under maximally permissive assumptions?

---

## Methods

**Data source.** Pathogenic and likely pathogenic *IDS* variants were retrieved from ClinVar [1] (accessed 2 October 2026). The query was `IDS[gene]`, with the ClinVar filters *Germline classification: Pathogenic* and *Likely pathogenic*, and *Variant length: < 1 kb, single gene*. The resulting set comprised 395 variants. The identical query re-run on 11 October 2026 returned the same 395 variants.

**Verification against ClinVar's own counts.** The variant-type composition of the downloaded file was checked against ClinVar's filter counts for the identical query: single nucleotide 229, deletion 100, duplication 37, insertion 16, indel 5, with the remaining 8 annotated as microsatellite. These agree exactly. Molecular-consequence counts differ slightly from ClinVar's facets (missense 155 against 158; splice 31 against 33) because ClinVar counts a variant under every consequence it carries, whereas each variant here is assigned a single primary consequence by the hierarchy described below.

**Exclusion of multi-gene rearrangements.** Large multi-gene rearrangements were deliberately excluded. Inspection of this excluded set (n = 99) showed that it consists almost entirely of chromosome-scale copy number changes spanning hundreds of genes, which correspond to contiguous gene syndromes rather than *IDS*-specific disease.

**Consequence assignment.** Each variant was assigned a single primary molecular consequence using a severity hierarchy (frameshift > nonsense > splice > in-frame indel > start loss > missense > synonymous > UTR > intron), since ClinVar reports multiple consequences per variant across transcripts. The assignment was verified by cross-tabulation against variant type; no frameshift variant was annotated as a single-nucleotide change, and no missense variant as an indel.

**Base-editing accessibility.** Accessibility was assessed under deliberately permissive assumptions. A variant was counted as addressable only if:

1. it is a single-nucleotide substitution, since base editors alter base identity without changing sequence length; and
2. the substitution required to restore the reference sequence is a transition achievable by a current editor — adenine base editors install A→G (T→C on the opposite strand) [5] and cytosine base editors install C→T (G→A on the opposite strand) [4].

Transversions are not achievable by either chemistry.

PAM availability, position within the editing window, and bystander editing were **not** modelled. The resulting figure is therefore an upper bound; the achievable fraction is necessarily lower.

**Implementation.** ClinVar lists protein positions for two *IDS* isoforms in inconsistent order; the larger value was taken as the canonical *NM_000202.8* numbering. This assignment was verified against the `p.` notation in the `Name` field, which ClinVar reports against *NM_000202.8*, and agreed for all 347 variants carrying interpretable protein annotation. Analysis was performed in Python 3.14 using pandas and matplotlib.

---

## Results

### Two fifths of pathogenic *IDS* variants are structurally inaccessible to base editing

Of the 395 variants analysed, **166 (42.0%)** are insertions, deletions, duplications or complex indels. Because base editors alter base identity without changing sequence length, these variants are not directly addressable by conventional base editors. Frameshift variants alone account for 135 (34.2%) of the set — the second largest consequence class after missense.

**Table 1. Molecular consequence of pathogenic *IDS* variants (n = 395)**

| Consequence | n | % |
|---|---:|---:|
| Missense | 155 | 39.2 |
| Frameshift | 135 | 34.2 |
| Nonsense | 52 | 13.2 |
| Splice | 31 | 7.8 |
| In-frame indel | 17 | 4.3 |
| Intron | 5 | 1.3 |

### Among single-nucleotide variants, most require chemistry that does not exist

The remaining 229 variants (58.0%) are single-nucleotide substitutions and therefore candidates in principle. However, correcting a substitution requires installing the reverse change, and current base editors install transitions only. Of the 229 candidates, **144 (62.9%) arose through transversions** — G>C, G>T, C>A, C>G, T>G, T>A, A>T, A>C — for which no base editor is available.

Only 85 variants are chemically correctable: 54 by an adenine base editor and 31 by a cytosine base editor.

> **This corresponds to 21.5% of all pathogenic *IDS* variants, and represents an upper bound before PAM availability, editing-window position and bystander editing are considered.**

**Table 2. Accessibility of *IDS* variants to base editing**

| Category | n | % of total |
|---|---:|---:|
| Correctable by ABE | 54 | 13.7 |
| Correctable by CBE | 31 | 7.8 |
| SNV, transversion — no editor | 144 | 36.5 |
| Indel — structurally out of reach | 166 | 42.0 |
| **Total addressable** | **85** | **21.5** |

![](results/fig1_coverage.png)

**Figure 1.** Accessibility of pathogenic *IDS* variants to base editing. ABE 54, CBE 31, transversions 144, indels 166 (n = 395).

![](results/fig2.png)

**Figure 2.** Spectrum coverage by strategy: per-mutation base editing (upper bound 21.5%) versus mutation-agnostic whole-gene insertion. The 100% shown for insertion is theoretical coverage of the reported variant spectrum by a single reagent — not a claim about efficacy, delivery or safety.

### No single codon dominates the reported *IDS* spectrum

Codon-level analysis of 347 variants with interpretable protein annotation identified **167 distinct affected codons**, a mean of 2.08 variants per codon. The most frequently affected codon accounts for 7 variants (**2.0%** of the 347 annotated, 1.8% of all 395); the five most frequent together account for 9.5%. Just over half of affected codons (87 of 167) carry a single reported variant.

The mutational landscape is therefore dispersed rather than concentrated. Targeting even the ten most common positions would address a small minority of the reported variants, and each would require its own reagent, guide design and validation path.

![](results/fig3_distribution.png)

**Figure 3.** Distribution of pathogenic variants across the *IDS* coding sequence (NM_000202.8). The interval between codons ~140 and ~390 is empty in the filtered ClinVar set; see Limitations.

---

## Limitations

Several constraints should be stated plainly.

**ClinVar reflects reporting practice rather than population frequency.** The absence of a dominant codon in this dataset indicates that no single variant dominates the *reported* spectrum; it does not establish allele frequencies in any patient population. A registry-based cohort would be required for that claim, and the conclusion drawn here is correspondingly limited to reported variant diversity.

**A coverage gap exists between c.419 and c.1180.** No variants are catalogued in this interval in the filtered set. This is a property of the ClinVar query rather than of the download: the identical query re-run on 11 October 2026 returns the same 395 variants, with the same gap. It is unlikely to be biological — pathogenic variants in this interval are reported in the literature, for example c.879G>A (p.Gln293Gln), which has been shown to create an exonic splicing enhancer [3]. The gap more plausibly reflects under-reporting to ClinVar, or difficulty in unambiguously assigning variants in a region adjacent to the *IDS2* pseudogene, where recombination is known to occur. Its quantitative effect on the figures reported here cannot be determined from this dataset; if the unrepresented interval resembles the rest of the gene, the analysis understates rather than overstates the diversity of the spectrum.

**The 21.5% figure is an upper bound** and should not be read as an achievable correction rate. Applying realistic constraints — PAM availability within the editing window, position of the target base within that window, and the presence of bystander bases — would reduce it, in some cases substantially. The direction of this bias is known even though its magnitude is not.

**Prime editing was not modelled.** It is more permissive than base editing [6], accommodating transversions and small indels, and would therefore extend coverage beyond the figure reported here. It remains, however, variant-specific: the edit is encoded in the pegRNA, so a different variant requires a different pegRNA, and efficiency varies enough with sequence context that each must be validated. The scaling problem is reduced rather than removed.

---

## Implication

The two findings reported here are properties of the gene rather than of any reagent. Allelic diversity cannot be engineered away, and the chemical restriction of base editors to transitions is intrinsic to the mechanism. Improvements in editor efficiency, specificity or delivery would not alter either result.

This places MPS II in a different category from the diseases where per-mutation correction has been taken furthest. In sickle cell disease a single shared allele defines the patient population; in Fanconi anaemia, correction efforts have targeted prevalent *FANCA* alleles. For *IDS*, no comparable shortcut exists: the most common codon accounts for seven of 347 annotated variants, and four fifths of the spectrum lies outside base-editing chemistry altogether.

Approaches that are indifferent to the patient's specific variant — insertion of a complete functional coding sequence at a defined locus — would in principle address the entire reported spectrum with a single reagent. That is a statement about variant coverage, not about efficacy, delivery or safety, each of which would have to be established on its own. Recently described RNA-guided bridge recombinases [7, 10] were introduced on precisely this rationale: site-specific insertion of gene-sized fragments, their authors write, would enable "correction of multiallelic genetic disorders that cannot be addressed with small-scale edits" [10]. The present analysis offers a quantitative case for one such disorder.

---

## References

1. Landrum MJ, Lee JM, Benson M, Brown GR, Chao C, Chitipiralla S, et al. ClinVar: improving access to variant interpretations and supporting evidence. *Nucleic Acids Research* 2018;46(D1):D1062–D1067. doi:10.1093/nar/gkx1153

2. Zanetti A, D'Avanzo F, Tomanin R. Molecular basis of mucopolysaccharidosis type II (Hunter syndrome): first review and classification of published IDS gene variants. *Human Genomics* 2024;18(1):1–17. doi:10.1186/s40246-024-00701-w

3. Zhang H, Li J, Zhang X, Wang Y, Qiu W, Ye J, et al. Analysis of the IDS gene in 38 patients with Hunter syndrome: the c.879G>A (p.Gln293Gln) synonymous variation in a female creates exonic splicing. *PLoS ONE* 2011;6(8):e22951. doi:10.1371/journal.pone.0022951

4. Komor AC, Kim YB, Packer MS, Zuris JA, Liu DR. Programmable editing of a target base in genomic DNA without double-stranded DNA cleavage. *Nature* 2016;533(7603):420–424. doi:10.1038/nature17946

5. Gaudelli NM, Komor AC, Rees HA, Packer MS, Badran AH, Bryson DI, Liu DR. Programmable base editing of A•T to G•C in genomic DNA without DNA cleavage. *Nature* 2017;551(7681):464–471. doi:10.1038/nature24644

6. Anzalone AV, Randolph PB, Davis JR, Sousa AA, Koblan LW, Levy JM, et al. Search-and-replace genome editing without double-strand breaks or donor DNA. *Nature* 2019;576(7785):149–157. doi:10.1038/s41586-019-1711-4

7. Durrant MG, Perry NT, Pai JJ, Jangid AR, Athukoralage JS, Hiraizumi M, et al. Bridge RNAs direct programmable recombination of target and donor DNA. *Nature* 2024;630(8018):984–993. doi:10.1038/s41586-024-07552-4

8. Frangoul H, Altshuler D, Cappellini MD, Chen YS, Domm J, Eustace BK, et al. CRISPR-Cas9 gene editing for sickle cell disease and β-thalassemia. *New England Journal of Medicine* 2021;384(3):252–260. doi:10.1056/NEJMoa2031054

9. Siegner SM, Ugalde L, Clemens A, Garcia-Garcia L, Bueren JA, Rio P, Karasu ME, Corn JE. Adenine base editing efficiently restores the function of Fanconi anemia hematopoietic stem and progenitor cells. *Nature Communications* 2022;13:6900. doi:10.1038/s41467-022-34479-z

10. Pelea O, Tálas A, Fernández Carrera J, Mathis N, van de Venn L, Yeh CD, et al. Programmable genome editing in human cells using RNA-guided bridge recombinases. *Science* 2026;391:eadz1884. doi:10.1126/science.adz1884

---

## Data and code availability

Repository: <https://github.com/PantelisMastakoulis/IDS-variant-analysis>\
Source data: ClinVar, accessed 2 October 2026.\
Analysis code: `01_explore_variants.ipynb`\
Figures: `results/`