
## Hierarchical consensus partitioning analysis on dataset 'HSMM_single_cell'

Runnable code:

```r
library(cola)
library(RColorBrewer)
library(HSMMSingleCell)
data(HSMM_expr_matrix)
data(HSMM_sample_sheet)

anno = HSMM_sample_sheet[, c("Hours", "Media", "State")]
anno_col = list(
    Hours = structure(brewer.pal(9, "Blues")[c(2, 4, 6, 8)], names = c("0", "24", "48", "72")),
    Media = c("GM" = "orange", "DM" = "purple"),
    State = c("1" = "red", "2" = "blue", "3" = "green"))

m = adjust_matrix(log10(HSMM_expr_matrix + 1))
gt = readRDS(url("https://jokergoo.github.io/cola_examples/HSMM_single_cell/gene_type_gencode_v17.rds"))
m = m[gt[rownames(m)] == "protein_coding", , drop = FALSE]

rh = hierarchical_partition(m, cores = 4,
    anno = anno,
    anno_col = anno_col
)
saveRDS(rh, file = "HSMM_single_cell_cola_rh.rds")


cola_report(rh, output = "HSMM_single_cell_cola_rh_report", title = "cola Report for Hierarchical Partitioning - 'HSMM_single_cell'")
```

[The HTML report is here.](https://cola-rh.github.io/HSMM_single_cell/HSMM_single_cell_cola_rh_report/cola_hc.html) (generated by __cola__ version 2.0.0, R 4.1.0.)
