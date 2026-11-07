# Differential Gene Expression Analysis of Breast Cancer (GSE42568) in R

Identification of transcripts differing between normal breast tissue and breast cancer samples, using R and Bioconductor.

---

## Dataset

| | |
|---|---|
| Repository | NCBI Gene Expression Omnibus (GEO) |
| Accession | GSE42568 |
| Species | *Homo sapiens* |
| Condition | Breast cancer vs normal breast tissue |
| Data type | Gene expression microarray |
| Platform | Affymetrix Human Genome U133 Plus 2.0 Array |
| Samples | 121 (104 breast tumour, 17 normal breast tissue) |
| Note | Tumour biopsies taken before any tamoxifen or chemotherapy |

## Biological question

Which transcripts are differentially expressed between normal breast tissue and breast cancer samples?

---

## Methods

Standard Bioconductor differential expression workflow for microarray data.

```
GSE42568 (GEO)
    │
    ▼
Download expression + phenotype data (GEOquery)
    │
    ▼
Define groups: normal vs tumour
    │
    ▼
Build design matrix
    │
    ▼
Linear model fit (limma)
    │
    ▼
Empirical Bayes moderation (eBayes)
    │
    ▼
Benjamini–Hochberg FDR correction
    │
    ▼
Significant transcripts (FDR < 0.05)
    │
    ▼
Visualisation: volcano plot, PCA, hierarchical clustering heatmap
```

**Packages:** GEOquery, limma, EnhancedVolcano, pheatmap, org.Hs.eg.db

*(edgeR and clusterProfiler were installed but not used in the final analysis.)*

---

## Results

| Metric | Value |
|---|---|
| Total probes analysed | 54,675 |
| Significant at FDR < 0.05 | 16,803 (30.7%) |
| Upregulated | 8,240 |
| Downregulated | 8,563 |

PCA separated tumour and normal samples along the first principal component, and hierarchical clustering grouped samples by condition.

**Top 10 transcripts by adjusted p-value**

| Rank | Probe ID | logFC | Adjusted p (FDR) |
|---|---|---|---|
| 1 | 237351_at | −4.16 | 7.46 × 10⁻³⁸ |
| 2 | 214505_s_at | −5.37 | 6.08 × 10⁻³⁷ |
| 3 | 89977_at | −4.37 | 1.33 × 10⁻³⁶ |
| 4 | 230595_at | −3.58 | 1.43 × 10⁻³⁶ |
| 5 | 214582_at | −4.41 | 1.43 × 10⁻³⁶ |
| 6 | 223764_x_at | −3.51 | 4.86 × 10⁻³⁶ |
| 7 | 219059_s_at | −4.44 | 1.06 × 10⁻³⁵ |
| 8 | 235670_at | −3.59 | 1.93 × 10⁻³⁵ |
| 9 | 206930_at | −4.49 | 2.00 × 10⁻³⁵ |
| 10 | 221295_at | −3.93 | 2.98 × 10⁻³⁵ |

All ten are downregulated in tumour tissue, with log2 fold changes between −3.5 and −5.4 (roughly 11- to 41-fold lower expression).

## Figures

**Volcano plot**
![Enhanced Volcano Plot] <img width="800" height="600" alt="Enhanced_Volcano_Plot" src="https://github.com/user-attachments/assets/1dd4c0db-57f6-42a2-8108-7ed6e222faee" />

![Volcano Plot] <img width="306" height="523" alt="Volcano_Plot" src="https://github.com/user-attachments/assets/abda24a6-5790-4b21-b389-4a2831fe8dc5" />


**Principal component analysis**
![PCA] <img width="534" height="523" alt="PCA_Plot" src="https://github.com/user-attachments/assets/28c835fd-37e9-44db-8239-856ea48077c7" />


**Hierarchical clustering heatmap**
![Heatmap] <img width="306" height="523" alt="Heat_Map" src="https://github.com/user-attachments/assets/78835fcb-a13d-4a69-b609-c959803010cc" />

---

## Interpretation and limitations

- **The significant list is too large to act on.** 16,803 of 54,675 probes (31%) passed FDR < 0.05. This reflects genuine widespread transcriptomic change between tumour and normal tissue, but it also reflects a filter on adjusted p-value alone, with no fold-change threshold and no removal of low-expression probes. A combined filter, such as FDR < 0.05 and |log2FC| > 1, would give a list that can actually be interpreted.
- **Probes are not annotated.** Results are reported as Affymetrix probe IDs, not gene symbols. Until these are mapped, the output cannot be read biologically or compared with published breast cancer signatures.
- **The dataset is unbalanced**, with 104 tumour samples against 17 normal samples. This limits power for the normal group and makes it more sensitive to individual outliers.
- **No functional analysis** has been performed, so the results point to differences without indicating which pathways they affect.
- **Single dataset, no validation.** These transcripts are candidates generated from one cohort; replication in an independent dataset would be needed before treating any of them as biomarkers.

## Next steps

- Annotate probes with gene symbols (org.Hs.eg.db) and collapse multiple probes per gene
- Apply a fold-change threshold alongside FDR and report the reduced list
- Run GO and KEGG enrichment (clusterProfiler) on the filtered set
- Replicate the comparison in an independent RNA-seq breast cancer cohort

## Author

Shuneed Zulfiqar
Biotechnology graduate | Bioinformatics | Computational biology | Cancer genomics
