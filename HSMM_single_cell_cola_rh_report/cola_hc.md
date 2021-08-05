cola Report for Hierarchical Partitioning - 'HSMM_single_cell'
==================

**Date**: 2021-07-22 16:12:18 CEST, **cola version**: 1.9.4

----------------------------------------------------------------

<style type='text/css'>

body, td, th {
   font-family: Arial,Helvetica,sans-serif;
   background-color: white;
   font-size: 13px;
  max-width: 800px;
  margin: auto;
  margin-left:210px;
  padding: 0px 10px 0px 10px;
  border-left: 1px solid #EEEEEE;
  line-height: 150%;
}

tt, code, pre {
   font-family: 'DejaVu Sans Mono', 'Droid Sans Mono', 'Lucida Console', Consolas, Monaco, 

monospace;
}

h1 {
   font-size:2.2em;
}

h2 {
   font-size:1.8em;
}

h3 {
   font-size:1.4em;
}

h4 {
   font-size:1.0em;
}

h5 {
   font-size:0.9em;
}

h6 {
   font-size:0.8em;
}

a {
  text-decoration: none;
  color: #0366d6;
}

a:hover {
  text-decoration: underline;
}

a:visited {
   color: #0366d6;
}

pre, img {
  max-width: 100%;
}
pre {
  overflow-x: auto;
}
pre code {
   display: block; padding: 0.5em;
}

code {
  font-size: 92%;
  border: 1px solid #ccc;
}

code[class] {
  background-color: #F8F8F8;
}

table, td, th {
  border: 1px solid #ccc;
}

blockquote {
   color:#666666;
   margin:0;
   padding-left: 1em;
   border-left: 0.5em #EEE solid;
}

hr {
   height: 0px;
   border-bottom: none;
   border-top-width: thin;
   border-top-style: dotted;
   border-top-color: #999999;
}

@media print {
   * {
      background: transparent !important;
      color: black !important;
      filter:none !important;
      -ms-filter: none !important;
   }

   body {
      font-size:12pt;
      max-width:100%;
   }

   a, a:visited {
      text-decoration: underline;
   }

   hr {
      visibility: hidden;
      page-break-before: always;
   }

   pre, blockquote {
      padding-right: 1em;
      page-break-inside: avoid;
   }

   tr, img {
      page-break-inside: avoid;
   }

   img {
      max-width: 100% !important;
   }

   @page :left {
      margin: 15mm 20mm 15mm 10mm;
   }

   @page :right {
      margin: 15mm 10mm 15mm 20mm;
   }

   p, h2, h3 {
      orphans: 3; widows: 3;
   }

   h2, h3 {
      page-break-after: avoid;
   }
}
</style>




## Summary



First the variable is renamed to `res_rh`.


```r
res_rh = rh
```



The partition hierarchy and all available functions which can be applied to `res_rh` object.


```r
res_rh
```

```
#> A 'HierarchicalPartition' object with 'ATC:skmeans' method.
#>   On a matrix with 12005 rows and 271 columns.
#>   Performed in total 2550 partitions.
#>   There are 11 groups under the following parameters:
#>     - min_samples: 6
#>     - mean_silhouette_cutoff: 0.9
#>     - min_n_signatures: 151 (signatures are selected based on:)
#>       - fdr_cutoff: 0.05
#>       - group_diff (scaled values): 0.5
#> 
#> Hierarchy of the partition:
#>   0, 271 cols
#>   |-- 01, 102 cols, 796 signatures
#>   |   |-- 011, 60 cols, 94 signatures (c)
#>   |   `-- 012, 42 cols, 422 signatures
#>   |       |-- 0121, 20 cols (a)
#>   |       |-- 0122, 13 cols, 0 signatures (c)
#>   |       `-- 0123, 9 cols (b)
#>   |-- 02, 100 cols, 1634 signatures
#>   |   |-- 021, 44 cols, 111 signatures (c)
#>   |   |-- 022, 35 cols (a)
#>   |   `-- 023, 21 cols, 30 signatures (c)
#>   `-- 03, 69 cols, 606 signatures
#>       |-- 031, 39 cols, 548 signatures
#>       |   |-- 0311, 12 cols, 4 signatures (c)
#>       |   |-- 0312, 19 cols, 13 signatures (c)
#>       |   `-- 0313, 8 cols (b)
#>       `-- 032, 30 cols, 0 signatures (c)
#> Stop reason:
#>   a) Mean silhouette score was too small
#>   b) Subgroup had too few columns.
#>   c) There were too few signatures.
#> 
#> Following methods can be applied to this 'HierarchicalPartition' object:
#>  [1] "all_leaves"            "all_nodes"             "cola_report"           "collect_classes"      
#>  [5] "colnames"              "compare_signatures"    "dimension_reduction"   "functional_enrichment"
#>  [9] "get_anno_col"          "get_anno"              "get_children_nodes"    "get_classes"          
#> [13] "get_matrix"            "get_signatures"        "is_leaf_node"          "max_depth"            
#> [17] "merge_node"            "ncol"                  "node_info"             "node_level"           
#> [21] "nrow"                  "rownames"              "show"                  "split_node"           
#> [25] "suggest_best_k"        "test_to_known_factors" "top_rows_heatmap"      "top_rows_overlap"     
#> 
#> You can get result for a single node by e.g. object["01"]
```

The call of `hierarchical_partition()` was:


```
#> hierarchical_partition(data = m, anno = anno, anno_col = anno_col, cores = 4)
```

Dimension of the input matrix:


```r
mat = get_matrix(res_rh)
dim(mat)
```

```
#> [1] 12005   271
```

All the methods that were tried:


```r
res_rh@param$combination_method
```

```
#> [[1]]
#> [1] "ATC"     "skmeans"
```

### Density distribution

The density distribution for each sample is visualized as one column in the following heatmap.
The clustering is based on the distance which is the Kolmogorov-Smirnov statistic between two distributions.




```r
library(ComplexHeatmap)
densityHeatmap(mat, top_annotation = HeatmapAnnotation(df = get_anno(res_rh), 
    col = get_anno_col(res_rh)), ylab = "value", cluster_columns = TRUE, show_column_names = FALSE,
    mc.cores = 1)
```

![plot of chunk density-heatmap](figure_cola/density-heatmap-1.png)



Some values about the hierarchy:


```r
all_nodes(res_rh)
```

```
#>  [1] "0"    "01"   "011"  "012"  "0121" "0122" "0123" "02"   "021"  "022"  "023"  "03"   "031" 
#> [14] "0311" "0312" "0313" "032"
```

```r
all_leaves(res_rh)
```

```
#>  [1] "011"  "0121" "0122" "0123" "021"  "022"  "023"  "0311" "0312" "0313" "032"
```

```r
node_info(res_rh)
```

```
#>      id best_method depth best_k n_columns n_signatures p_signatures is_leaf
#> 1     0 ATC:skmeans     1      3       271         3037     0.252978   FALSE
#> 2    01 ATC:skmeans     2      2       102          796     0.066306   FALSE
#> 3   011 ATC:skmeans     3      2        60           94     0.007830    TRUE
#> 4   012 ATC:skmeans     3      3        42          422     0.035152   FALSE
#> 5  0121 ATC:skmeans     4      3        20           NA           NA    TRUE
#> 6  0122 ATC:skmeans     4      2        13            0     0.000000    TRUE
#> 7  0123 not applied     4     NA         9           NA           NA    TRUE
#> 8    02 ATC:skmeans     2      3       100         1634     0.136110   FALSE
#> 9   021 ATC:skmeans     3      2        44          111     0.009246    TRUE
#> 10  022 ATC:skmeans     3      2        35           NA           NA    TRUE
#> 11  023 ATC:skmeans     3      2        21           30     0.002499    TRUE
#> 12   03 ATC:skmeans     2      2        69          606     0.050479   FALSE
#> 13  031 ATC:skmeans     3      3        39          548     0.045648   FALSE
#> 14 0311 ATC:skmeans     4      3        12            4     0.000333    TRUE
#> 15 0312 ATC:skmeans     4      3        19           13     0.001083    TRUE
#> 16 0313 not applied     4     NA         8           NA           NA    TRUE
#> 17  032 ATC:skmeans     3      2        30            0     0.000000    TRUE
```

In the output from `node_info()`, there are the following columns:

- `id`: The node id.
- `best_method`: The best method selected.
- `depth`: Depth of the node in the hierarchy.
- `best_k`: Best number of groups of the partition on that node.
- `n_columns`: Number of columns in the submatrix.
- `n_signatures`: Number of signatures with the `best_k`.
- `p_signatures`: Proportion of hte signatures in total number of rows in the matrix.
- `is_leaf`: Whether the node is a leaf.

Labels of nodes are encoded in a special way. The number of digits
correspond to the depth of the node in the hierarchy and the value of the
digits correspond to the index of the subgroup in the current node, E.g. a label
of “012” means the node is the second subgroup of the partition which is the
first subgroup of the root node.

### Suggest the best k



Following table shows the best `k` (number of partitions) for each node in the
partition hierarchy. Clicking on the node name in the table goes to the
corresponding section for the partitioning on that node.

[The cola vignette](https://jokergoo.github.io/cola_vignettes/cola.html#toc_13)
explains the definition of the metrics used for determining the best
number of partitions.



```r
suggest_best_k(res_rh)
```


|Node                |Best method                                         |Is leaf   |Best k |1-PAC |Mean silhouette |Concordance | #samples|   |
|:-------------------|:---------------------------------------------------|:---------|:------|:-----|:---------------|:-----------|--------:|:--|
|[Node0](#Node0)     |ATC:skmeans                                         |          |3      |1.00  |0.97            |0.99        |      271|** |
|[Node01](#Node01)   |ATC:skmeans                                         |          |2      |1.00  |1.00            |1.00        |      102|** |
|Node011-leaf        |ATC:skmeans                                         |✓ (&#99;) |2      |0.96  |0.95            |0.98        |       60|** |
|[Node012](#Node012) |ATC:skmeans                                         |          |4      |0.99  |0.94            |0.97        |       42|** |
|Node0121-leaf       |ATC:skmeans                                         |✓ (a)     |3      |0.69  |0.87            |0.94        |       20|   |
|Node0122-leaf       |ATC:skmeans                                         |✓ (&#99;) |2      |1.00  |0.99            |1.00        |       13|** |
|Node0123-leaf       |<span style='color:grey;'><i>not applied</i></span> |✓ (b)     |       |      |                |            |        9|   |
|[Node02](#Node02)   |ATC:skmeans                                         |          |3      |1.00  |0.97            |0.99        |      100|** |
|Node021-leaf        |ATC:skmeans                                         |✓ (&#99;) |3      |0.97  |0.92            |0.96        |       44|** |
|Node022-leaf        |ATC:skmeans                                         |✓ (a)     |2      |0.70  |0.89            |0.95        |       35|   |
|Node023-leaf        |ATC:skmeans                                         |✓ (&#99;) |2      |1.00  |0.97            |0.99        |       21|** |
|[Node03](#Node03)   |ATC:skmeans                                         |          |2      |1.00  |1.00            |1.00        |       69|** |
|[Node031](#Node031) |ATC:skmeans                                         |          |3      |1.00  |0.99            |1.00        |       39|** |
|Node0311-leaf       |ATC:skmeans                                         |✓ (&#99;) |3      |1.00  |0.99            |1.00        |       12|** |
|Node0312-leaf       |ATC:skmeans                                         |✓ (&#99;) |3      |0.92  |0.99            |0.99        |       19|*  |
|Node0313-leaf       |<span style='color:grey;'><i>not applied</i></span> |✓ (b)     |       |      |                |            |        8|   |
|Node032-leaf        |ATC:skmeans                                         |✓ (&#99;) |2      |1.00  |0.97            |0.99        |       30|** |


Stop reason: a) Mean silhouette score was too small b) Subgroup had too few columns. c) There were too few signatures. 

\*\*: 1-PAC > 0.95, \*: 1-PAC > 0.9


### Partition hierarchy

The nodes of the hierarchy can be merged by setting the `merge_node` parameters. Here we 
control the hierarchy with the `min_n_signatures` parameter. The value of `min_n_signatures` is
from `node_info()`.





<style type='text/css'>



.ui-helper-hidden {
	display: none;
}
.ui-helper-hidden-accessible {
	border: 0;
	clip: rect(0 0 0 0);
	height: 1px;
	margin: -1px;
	overflow: hidden;
	padding: 0;
	position: absolute;
	width: 1px;
}
.ui-helper-reset {
	margin: 0;
	padding: 0;
	border: 0;
	outline: 0;
	line-height: 1.3;
	text-decoration: none;
	font-size: 100%;
	list-style: none;
}
.ui-helper-clearfix:before,
.ui-helper-clearfix:after {
	content: "";
	display: table;
	border-collapse: collapse;
}
.ui-helper-clearfix:after {
	clear: both;
}
.ui-helper-zfix {
	width: 100%;
	height: 100%;
	top: 0;
	left: 0;
	position: absolute;
	opacity: 0;
	filter:Alpha(Opacity=0); 
}

.ui-front {
	z-index: 100;
}



.ui-state-disabled {
	cursor: default !important;
	pointer-events: none;
}



.ui-icon {
	display: inline-block;
	vertical-align: middle;
	margin-top: -.25em;
	position: relative;
	text-indent: -99999px;
	overflow: hidden;
	background-repeat: no-repeat;
}

.ui-widget-icon-block {
	left: 50%;
	margin-left: -8px;
	display: block;
}




.ui-widget-overlay {
	position: fixed;
	top: 0;
	left: 0;
	width: 100%;
	height: 100%;
}
.ui-accordion .ui-accordion-header {
	display: block;
	cursor: pointer;
	position: relative;
	margin: 2px 0 0 0;
	padding: .5em .5em .5em .7em;
	font-size: 100%;
}
.ui-accordion .ui-accordion-content {
	padding: 1em 2.2em;
	border-top: 0;
	overflow: auto;
}
.ui-autocomplete {
	position: absolute;
	top: 0;
	left: 0;
	cursor: default;
}
.ui-menu {
	list-style: none;
	padding: 0;
	margin: 0;
	display: block;
	outline: 0;
}
.ui-menu .ui-menu {
	position: absolute;
}
.ui-menu .ui-menu-item {
	margin: 0;
	cursor: pointer;
	
	list-style-image: url("data:image/gif;base64,R0lGODlhAQABAIAAAAAAAP///yH5BAEAAAAALAAAAAABAAEAAAIBRAA7");
}
.ui-menu .ui-menu-item-wrapper {
	position: relative;
	padding: 3px 1em 3px .4em;
}
.ui-menu .ui-menu-divider {
	margin: 5px 0;
	height: 0;
	font-size: 0;
	line-height: 0;
	border-width: 1px 0 0 0;
}
.ui-menu .ui-state-focus,
.ui-menu .ui-state-active {
	margin: -1px;
}


.ui-menu-icons {
	position: relative;
}
.ui-menu-icons .ui-menu-item-wrapper {
	padding-left: 2em;
}


.ui-menu .ui-icon {
	position: absolute;
	top: 0;
	bottom: 0;
	left: .2em;
	margin: auto 0;
}


.ui-menu .ui-menu-icon {
	left: auto;
	right: 0;
}
.ui-button {
	padding: .4em 1em;
	display: inline-block;
	position: relative;
	line-height: normal;
	margin-right: .1em;
	cursor: pointer;
	vertical-align: middle;
	text-align: center;
	-webkit-user-select: none;
	-moz-user-select: none;
	-ms-user-select: none;
	user-select: none;

	
	overflow: visible;
}

.ui-button,
.ui-button:link,
.ui-button:visited,
.ui-button:hover,
.ui-button:active {
	text-decoration: none;
}


.ui-button-icon-only {
	width: 2em;
	box-sizing: border-box;
	text-indent: -9999px;
	white-space: nowrap;
}


input.ui-button.ui-button-icon-only {
	text-indent: 0;
}


.ui-button-icon-only .ui-icon {
	position: absolute;
	top: 50%;
	left: 50%;
	margin-top: -8px;
	margin-left: -8px;
}

.ui-button.ui-icon-notext .ui-icon {
	padding: 0;
	width: 2.1em;
	height: 2.1em;
	text-indent: -9999px;
	white-space: nowrap;

}

input.ui-button.ui-icon-notext .ui-icon {
	width: auto;
	height: auto;
	text-indent: 0;
	white-space: normal;
	padding: .4em 1em;
}



input.ui-button::-moz-focus-inner,
button.ui-button::-moz-focus-inner {
	border: 0;
	padding: 0;
}
.ui-controlgroup {
	vertical-align: middle;
	display: inline-block;
}
.ui-controlgroup > .ui-controlgroup-item {
	float: left;
	margin-left: 0;
	margin-right: 0;
}
.ui-controlgroup > .ui-controlgroup-item:focus,
.ui-controlgroup > .ui-controlgroup-item.ui-visual-focus {
	z-index: 9999;
}
.ui-controlgroup-vertical > .ui-controlgroup-item {
	display: block;
	float: none;
	width: 100%;
	margin-top: 0;
	margin-bottom: 0;
	text-align: left;
}
.ui-controlgroup-vertical .ui-controlgroup-item {
	box-sizing: border-box;
}
.ui-controlgroup .ui-controlgroup-label {
	padding: .4em 1em;
}
.ui-controlgroup .ui-controlgroup-label span {
	font-size: 80%;
}
.ui-controlgroup-horizontal .ui-controlgroup-label + .ui-controlgroup-item {
	border-left: none;
}
.ui-controlgroup-vertical .ui-controlgroup-label + .ui-controlgroup-item {
	border-top: none;
}
.ui-controlgroup-horizontal .ui-controlgroup-label.ui-widget-content {
	border-right: none;
}
.ui-controlgroup-vertical .ui-controlgroup-label.ui-widget-content {
	border-bottom: none;
}


.ui-controlgroup-vertical .ui-spinner-input {

	
	width: 75%;
	width: calc( 100% - 2.4em );
}
.ui-controlgroup-vertical .ui-spinner .ui-spinner-up {
	border-top-style: solid;
}

.ui-checkboxradio-label .ui-icon-background {
	box-shadow: inset 1px 1px 1px #ccc;
	border-radius: .12em;
	border: none;
}
.ui-checkboxradio-radio-label .ui-icon-background {
	width: 16px;
	height: 16px;
	border-radius: 1em;
	overflow: visible;
	border: none;
}
.ui-checkboxradio-radio-label.ui-checkboxradio-checked .ui-icon,
.ui-checkboxradio-radio-label.ui-checkboxradio-checked:hover .ui-icon {
	background-image: none;
	width: 8px;
	height: 8px;
	border-width: 4px;
	border-style: solid;
}
.ui-checkboxradio-disabled {
	pointer-events: none;
}
.ui-datepicker {
	width: 17em;
	padding: .2em .2em 0;
	display: none;
}
.ui-datepicker .ui-datepicker-header {
	position: relative;
	padding: .2em 0;
}
.ui-datepicker .ui-datepicker-prev,
.ui-datepicker .ui-datepicker-next {
	position: absolute;
	top: 2px;
	width: 1.8em;
	height: 1.8em;
}
.ui-datepicker .ui-datepicker-prev-hover,
.ui-datepicker .ui-datepicker-next-hover {
	top: 1px;
}
.ui-datepicker .ui-datepicker-prev {
	left: 2px;
}
.ui-datepicker .ui-datepicker-next {
	right: 2px;
}
.ui-datepicker .ui-datepicker-prev-hover {
	left: 1px;
}
.ui-datepicker .ui-datepicker-next-hover {
	right: 1px;
}
.ui-datepicker .ui-datepicker-prev span,
.ui-datepicker .ui-datepicker-next span {
	display: block;
	position: absolute;
	left: 50%;
	margin-left: -8px;
	top: 50%;
	margin-top: -8px;
}
.ui-datepicker .ui-datepicker-title {
	margin: 0 2.3em;
	line-height: 1.8em;
	text-align: center;
}
.ui-datepicker .ui-datepicker-title select {
	font-size: 1em;
	margin: 1px 0;
}
.ui-datepicker select.ui-datepicker-month,
.ui-datepicker select.ui-datepicker-year {
	width: 45%;
}
.ui-datepicker table {
	width: 100%;
	font-size: .9em;
	border-collapse: collapse;
	margin: 0 0 .4em;
}
.ui-datepicker th {
	padding: .7em .3em;
	text-align: center;
	font-weight: bold;
	border: 0;
}
.ui-datepicker td {
	border: 0;
	padding: 1px;
}
.ui-datepicker td span,
.ui-datepicker td a {
	display: block;
	padding: .2em;
	text-align: right;
	text-decoration: none;
}
.ui-datepicker .ui-datepicker-buttonpane {
	background-image: none;
	margin: .7em 0 0 0;
	padding: 0 .2em;
	border-left: 0;
	border-right: 0;
	border-bottom: 0;
}
.ui-datepicker .ui-datepicker-buttonpane button {
	float: right;
	margin: .5em .2em .4em;
	cursor: pointer;
	padding: .2em .6em .3em .6em;
	width: auto;
	overflow: visible;
}
.ui-datepicker .ui-datepicker-buttonpane button.ui-datepicker-current {
	float: left;
}


.ui-datepicker.ui-datepicker-multi {
	width: auto;
}
.ui-datepicker-multi .ui-datepicker-group {
	float: left;
}
.ui-datepicker-multi .ui-datepicker-group table {
	width: 95%;
	margin: 0 auto .4em;
}
.ui-datepicker-multi-2 .ui-datepicker-group {
	width: 50%;
}
.ui-datepicker-multi-3 .ui-datepicker-group {
	width: 33.3%;
}
.ui-datepicker-multi-4 .ui-datepicker-group {
	width: 25%;
}
.ui-datepicker-multi .ui-datepicker-group-last .ui-datepicker-header,
.ui-datepicker-multi .ui-datepicker-group-middle .ui-datepicker-header {
	border-left-width: 0;
}
.ui-datepicker-multi .ui-datepicker-buttonpane {
	clear: left;
}
.ui-datepicker-row-break {
	clear: both;
	width: 100%;
	font-size: 0;
}


.ui-datepicker-rtl {
	direction: rtl;
}
.ui-datepicker-rtl .ui-datepicker-prev {
	right: 2px;
	left: auto;
}
.ui-datepicker-rtl .ui-datepicker-next {
	left: 2px;
	right: auto;
}
.ui-datepicker-rtl .ui-datepicker-prev:hover {
	right: 1px;
	left: auto;
}
.ui-datepicker-rtl .ui-datepicker-next:hover {
	left: 1px;
	right: auto;
}
.ui-datepicker-rtl .ui-datepicker-buttonpane {
	clear: right;
}
.ui-datepicker-rtl .ui-datepicker-buttonpane button {
	float: left;
}
.ui-datepicker-rtl .ui-datepicker-buttonpane button.ui-datepicker-current,
.ui-datepicker-rtl .ui-datepicker-group {
	float: right;
}
.ui-datepicker-rtl .ui-datepicker-group-last .ui-datepicker-header,
.ui-datepicker-rtl .ui-datepicker-group-middle .ui-datepicker-header {
	border-right-width: 0;
	border-left-width: 1px;
}


.ui-datepicker .ui-icon {
	display: block;
	text-indent: -99999px;
	overflow: hidden;
	background-repeat: no-repeat;
	left: .5em;
	top: .3em;
}
.ui-dialog {
	position: absolute;
	top: 0;
	left: 0;
	padding: .2em;
	outline: 0;
}
.ui-dialog .ui-dialog-titlebar {
	padding: .4em 1em;
	position: relative;
}
.ui-dialog .ui-dialog-title {
	float: left;
	margin: .1em 0;
	white-space: nowrap;
	width: 90%;
	overflow: hidden;
	text-overflow: ellipsis;
}
.ui-dialog .ui-dialog-titlebar-close {
	position: absolute;
	right: .3em;
	top: 50%;
	width: 20px;
	margin: -10px 0 0 0;
	padding: 1px;
	height: 20px;
}
.ui-dialog .ui-dialog-content {
	position: relative;
	border: 0;
	padding: .5em 1em;
	background: none;
	overflow: auto;
}
.ui-dialog .ui-dialog-buttonpane {
	text-align: left;
	border-width: 1px 0 0 0;
	background-image: none;
	margin-top: .5em;
	padding: .3em 1em .5em .4em;
}
.ui-dialog .ui-dialog-buttonpane .ui-dialog-buttonset {
	float: right;
}
.ui-dialog .ui-dialog-buttonpane button {
	margin: .5em .4em .5em 0;
	cursor: pointer;
}
.ui-dialog .ui-resizable-n {
	height: 2px;
	top: 0;
}
.ui-dialog .ui-resizable-e {
	width: 2px;
	right: 0;
}
.ui-dialog .ui-resizable-s {
	height: 2px;
	bottom: 0;
}
.ui-dialog .ui-resizable-w {
	width: 2px;
	left: 0;
}
.ui-dialog .ui-resizable-se,
.ui-dialog .ui-resizable-sw,
.ui-dialog .ui-resizable-ne,
.ui-dialog .ui-resizable-nw {
	width: 7px;
	height: 7px;
}
.ui-dialog .ui-resizable-se {
	right: 0;
	bottom: 0;
}
.ui-dialog .ui-resizable-sw {
	left: 0;
	bottom: 0;
}
.ui-dialog .ui-resizable-ne {
	right: 0;
	top: 0;
}
.ui-dialog .ui-resizable-nw {
	left: 0;
	top: 0;
}
.ui-draggable .ui-dialog-titlebar {
	cursor: move;
}
.ui-draggable-handle {
	-ms-touch-action: none;
	touch-action: none;
}
.ui-resizable {
	position: relative;
}
.ui-resizable-handle {
	position: absolute;
	font-size: 0.1px;
	display: block;
	-ms-touch-action: none;
	touch-action: none;
}
.ui-resizable-disabled .ui-resizable-handle,
.ui-resizable-autohide .ui-resizable-handle {
	display: none;
}
.ui-resizable-n {
	cursor: n-resize;
	height: 7px;
	width: 100%;
	top: -5px;
	left: 0;
}
.ui-resizable-s {
	cursor: s-resize;
	height: 7px;
	width: 100%;
	bottom: -5px;
	left: 0;
}
.ui-resizable-e {
	cursor: e-resize;
	width: 7px;
	right: -5px;
	top: 0;
	height: 100%;
}
.ui-resizable-w {
	cursor: w-resize;
	width: 7px;
	left: -5px;
	top: 0;
	height: 100%;
}
.ui-resizable-se {
	cursor: se-resize;
	width: 12px;
	height: 12px;
	right: 1px;
	bottom: 1px;
}
.ui-resizable-sw {
	cursor: sw-resize;
	width: 9px;
	height: 9px;
	left: -5px;
	bottom: -5px;
}
.ui-resizable-nw {
	cursor: nw-resize;
	width: 9px;
	height: 9px;
	left: -5px;
	top: -5px;
}
.ui-resizable-ne {
	cursor: ne-resize;
	width: 9px;
	height: 9px;
	right: -5px;
	top: -5px;
}
.ui-progressbar {
	height: 2em;
	text-align: left;
	overflow: hidden;
}
.ui-progressbar .ui-progressbar-value {
	margin: -1px;
	height: 100%;
}
.ui-progressbar .ui-progressbar-overlay {
	background: url("data:image/gif;base64,R0lGODlhKAAoAIABAAAAAP///yH/C05FVFNDQVBFMi4wAwEAAAAh+QQJAQABACwAAAAAKAAoAAACkYwNqXrdC52DS06a7MFZI+4FHBCKoDeWKXqymPqGqxvJrXZbMx7Ttc+w9XgU2FB3lOyQRWET2IFGiU9m1frDVpxZZc6bfHwv4c1YXP6k1Vdy292Fb6UkuvFtXpvWSzA+HycXJHUXiGYIiMg2R6W459gnWGfHNdjIqDWVqemH2ekpObkpOlppWUqZiqr6edqqWQAAIfkECQEAAQAsAAAAACgAKAAAApSMgZnGfaqcg1E2uuzDmmHUBR8Qil95hiPKqWn3aqtLsS18y7G1SzNeowWBENtQd+T1JktP05nzPTdJZlR6vUxNWWjV+vUWhWNkWFwxl9VpZRedYcflIOLafaa28XdsH/ynlcc1uPVDZxQIR0K25+cICCmoqCe5mGhZOfeYSUh5yJcJyrkZWWpaR8doJ2o4NYq62lAAACH5BAkBAAEALAAAAAAoACgAAAKVDI4Yy22ZnINRNqosw0Bv7i1gyHUkFj7oSaWlu3ovC8GxNso5fluz3qLVhBVeT/Lz7ZTHyxL5dDalQWPVOsQWtRnuwXaFTj9jVVh8pma9JjZ4zYSj5ZOyma7uuolffh+IR5aW97cHuBUXKGKXlKjn+DiHWMcYJah4N0lYCMlJOXipGRr5qdgoSTrqWSq6WFl2ypoaUAAAIfkECQEAAQAsAAAAACgAKAAAApaEb6HLgd/iO7FNWtcFWe+ufODGjRfoiJ2akShbueb0wtI50zm02pbvwfWEMWBQ1zKGlLIhskiEPm9R6vRXxV4ZzWT2yHOGpWMyorblKlNp8HmHEb/lCXjcW7bmtXP8Xt229OVWR1fod2eWqNfHuMjXCPkIGNileOiImVmCOEmoSfn3yXlJWmoHGhqp6ilYuWYpmTqKUgAAIfkECQEAAQAsAAAAACgAKAAAApiEH6kb58biQ3FNWtMFWW3eNVcojuFGfqnZqSebuS06w5V80/X02pKe8zFwP6EFWOT1lDFk8rGERh1TTNOocQ61Hm4Xm2VexUHpzjymViHrFbiELsefVrn6XKfnt2Q9G/+Xdie499XHd2g4h7ioOGhXGJboGAnXSBnoBwKYyfioubZJ2Hn0RuRZaflZOil56Zp6iioKSXpUAAAh+QQJAQABACwAAAAAKAAoAAACkoQRqRvnxuI7kU1a1UU5bd5tnSeOZXhmn5lWK3qNTWvRdQxP8qvaC+/yaYQzXO7BMvaUEmJRd3TsiMAgswmNYrSgZdYrTX6tSHGZO73ezuAw2uxuQ+BbeZfMxsexY35+/Qe4J1inV0g4x3WHuMhIl2jXOKT2Q+VU5fgoSUI52VfZyfkJGkha6jmY+aaYdirq+lQAACH5BAkBAAEALAAAAAAoACgAAAKWBIKpYe0L3YNKToqswUlvznigd4wiR4KhZrKt9Upqip61i9E3vMvxRdHlbEFiEXfk9YARYxOZZD6VQ2pUunBmtRXo1Lf8hMVVcNl8JafV38aM2/Fu5V16Bn63r6xt97j09+MXSFi4BniGFae3hzbH9+hYBzkpuUh5aZmHuanZOZgIuvbGiNeomCnaxxap2upaCZsq+1kAACH5BAkBAAEALAAAAAAoACgAAAKXjI8By5zf4kOxTVrXNVlv1X0d8IGZGKLnNpYtm8Lr9cqVeuOSvfOW79D9aDHizNhDJidFZhNydEahOaDH6nomtJjp1tutKoNWkvA6JqfRVLHU/QUfau9l2x7G54d1fl995xcIGAdXqMfBNadoYrhH+Mg2KBlpVpbluCiXmMnZ2Sh4GBqJ+ckIOqqJ6LmKSllZmsoq6wpQAAAh+QQJAQABACwAAAAAKAAoAAAClYx/oLvoxuJDkU1a1YUZbJ59nSd2ZXhWqbRa2/gF8Gu2DY3iqs7yrq+xBYEkYvFSM8aSSObE+ZgRl1BHFZNr7pRCavZ5BW2142hY3AN/zWtsmf12p9XxxFl2lpLn1rseztfXZjdIWIf2s5dItwjYKBgo9yg5pHgzJXTEeGlZuenpyPmpGQoKOWkYmSpaSnqKileI2FAAACH5BAkBAAEALAAAAAAoACgAAAKVjB+gu+jG4kORTVrVhRlsnn2dJ3ZleFaptFrb+CXmO9OozeL5VfP99HvAWhpiUdcwkpBH3825AwYdU8xTqlLGhtCosArKMpvfa1mMRae9VvWZfeB2XfPkeLmm18lUcBj+p5dnN8jXZ3YIGEhYuOUn45aoCDkp16hl5IjYJvjWKcnoGQpqyPlpOhr3aElaqrq56Bq7VAAAOw==");
	height: 100%;
	filter: alpha(opacity=25); 
	opacity: 0.25;
}
.ui-progressbar-indeterminate .ui-progressbar-value {
	background-image: none;
}
.ui-selectable {
	-ms-touch-action: none;
	touch-action: none;
}
.ui-selectable-helper {
	position: absolute;
	z-index: 100;
	border: 1px dotted black;
}
.ui-selectmenu-menu {
	padding: 0;
	margin: 0;
	position: absolute;
	top: 0;
	left: 0;
	display: none;
}
.ui-selectmenu-menu .ui-menu {
	overflow: auto;
	overflow-x: hidden;
	padding-bottom: 1px;
}
.ui-selectmenu-menu .ui-menu .ui-selectmenu-optgroup {
	font-size: 1em;
	font-weight: bold;
	line-height: 1.5;
	padding: 2px 0.4em;
	margin: 0.5em 0 0 0;
	height: auto;
	border: 0;
}
.ui-selectmenu-open {
	display: block;
}
.ui-selectmenu-text {
	display: block;
	margin-right: 20px;
	overflow: hidden;
	text-overflow: ellipsis;
}
.ui-selectmenu-button.ui-button {
	text-align: left;
	white-space: nowrap;
	width: 14em;
}
.ui-selectmenu-icon.ui-icon {
	float: right;
	margin-top: 0;
}
.ui-slider {
	position: relative;
	text-align: left;
}
.ui-slider .ui-slider-handle {
	position: absolute;
	z-index: 2;
	width: 1.2em;
	height: 1.2em;
	cursor: default;
	-ms-touch-action: none;
	touch-action: none;
}
.ui-slider .ui-slider-range {
	position: absolute;
	z-index: 1;
	font-size: .7em;
	display: block;
	border: 0;
	background-position: 0 0;
}


.ui-slider.ui-state-disabled .ui-slider-handle,
.ui-slider.ui-state-disabled .ui-slider-range {
	filter: inherit;
}

.ui-slider-horizontal {
	height: .8em;
}
.ui-slider-horizontal .ui-slider-handle {
	top: -.3em;
	margin-left: -.6em;
}
.ui-slider-horizontal .ui-slider-range {
	top: 0;
	height: 100%;
}
.ui-slider-horizontal .ui-slider-range-min {
	left: 0;
}
.ui-slider-horizontal .ui-slider-range-max {
	right: 0;
}

.ui-slider-vertical {
	width: .8em;
	height: 100px;
}
.ui-slider-vertical .ui-slider-handle {
	left: -.3em;
	margin-left: 0;
	margin-bottom: -.6em;
}
.ui-slider-vertical .ui-slider-range {
	left: 0;
	width: 100%;
}
.ui-slider-vertical .ui-slider-range-min {
	bottom: 0;
}
.ui-slider-vertical .ui-slider-range-max {
	top: 0;
}
.ui-sortable-handle {
	-ms-touch-action: none;
	touch-action: none;
}
.ui-spinner {
	position: relative;
	display: inline-block;
	overflow: hidden;
	padding: 0;
	vertical-align: middle;
}
.ui-spinner-input {
	border: none;
	background: none;
	color: inherit;
	padding: .222em 0;
	margin: .2em 0;
	vertical-align: middle;
	margin-left: .4em;
	margin-right: 2em;
}
.ui-spinner-button {
	width: 1.6em;
	height: 50%;
	font-size: .5em;
	padding: 0;
	margin: 0;
	text-align: center;
	position: absolute;
	cursor: default;
	display: block;
	overflow: hidden;
	right: 0;
}

.ui-spinner a.ui-spinner-button {
	border-top-style: none;
	border-bottom-style: none;
	border-right-style: none;
}
.ui-spinner-up {
	top: 0;
}
.ui-spinner-down {
	bottom: 0;
}
.ui-tabs {
	position: relative;
	padding: .2em;
}
.ui-tabs .ui-tabs-nav {
	margin: 0;
	padding: .2em .2em 0;
}
.ui-tabs .ui-tabs-nav li {
	list-style: none;
	float: left;
	position: relative;
	top: 0;
	margin: 1px .2em 0 0;
	border-bottom-width: 0;
	padding: 0;
	white-space: nowrap;
}
.ui-tabs .ui-tabs-nav .ui-tabs-anchor {
	float: left;
	padding: .5em 1em;
	text-decoration: none;
}
.ui-tabs .ui-tabs-nav li.ui-tabs-active {
	margin-bottom: -1px;
	padding-bottom: 1px;
}
.ui-tabs .ui-tabs-nav li.ui-tabs-active .ui-tabs-anchor,
.ui-tabs .ui-tabs-nav li.ui-state-disabled .ui-tabs-anchor,
.ui-tabs .ui-tabs-nav li.ui-tabs-loading .ui-tabs-anchor {
	cursor: text;
}
.ui-tabs-collapsible .ui-tabs-nav li.ui-tabs-active .ui-tabs-anchor {
	cursor: pointer;
}
.ui-tabs .ui-tabs-panel {
	display: block;
	border-width: 0;
	padding: 1em 1.4em;
	background: none;
}
.ui-tooltip {
	padding: 8px;
	position: absolute;
	z-index: 9999;
	max-width: 300px;
}
body .ui-tooltip {
	border-width: 2px;
}

.ui-widget {
	font-family: Arial,Helvetica,sans-serif;
	font-size: 1em;
}
.ui-widget .ui-widget {
	font-size: 1em;
}
.ui-widget input,
.ui-widget select,
.ui-widget textarea,
.ui-widget button {
	font-family: Arial,Helvetica,sans-serif;
	font-size: 1em;
}
.ui-widget.ui-widget-content {
	border: 1px solid #c5c5c5;
}
.ui-widget-content {
	border: 1px solid #dddddd;
	background: #ffffff;
	color: #333333;
}
.ui-widget-content a {
	color: #333333;
}
.ui-widget-header {
	border: 1px solid #dddddd;
	background: #e9e9e9;
	color: #333333;
	font-weight: bold;
}
.ui-widget-header a {
	color: #333333;
}


.ui-state-default,
.ui-widget-content .ui-state-default,
.ui-widget-header .ui-state-default,
.ui-button,


html .ui-button.ui-state-disabled:hover,
html .ui-button.ui-state-disabled:active {
	border: 1px solid #c5c5c5;
	background: #f6f6f6;
	font-weight: normal;
	color: #454545;
}
.ui-state-default a,
.ui-state-default a:link,
.ui-state-default a:visited,
a.ui-button,
a:link.ui-button,
a:visited.ui-button,
.ui-button {
	color: #454545;
	text-decoration: none;
}
.ui-state-hover,
.ui-widget-content .ui-state-hover,
.ui-widget-header .ui-state-hover,
.ui-state-focus,
.ui-widget-content .ui-state-focus,
.ui-widget-header .ui-state-focus,
.ui-button:hover,
.ui-button:focus {
	border: 1px solid #cccccc;
	background: #ededed;
	font-weight: normal;
	color: #2b2b2b;
}
.ui-state-hover a,
.ui-state-hover a:hover,
.ui-state-hover a:link,
.ui-state-hover a:visited,
.ui-state-focus a,
.ui-state-focus a:hover,
.ui-state-focus a:link,
.ui-state-focus a:visited,
a.ui-button:hover,
a.ui-button:focus {
	color: #2b2b2b;
	text-decoration: none;
}

.ui-visual-focus {
	box-shadow: 0 0 3px 1px rgb(94, 158, 214);
}
.ui-state-active,
.ui-widget-content .ui-state-active,
.ui-widget-header .ui-state-active,
a.ui-button:active,
.ui-button:active,
.ui-button.ui-state-active:hover {
	border: 1px solid #003eff;
	background: #007fff;
	font-weight: normal;
	color: #ffffff;
}
.ui-icon-background,
.ui-state-active .ui-icon-background {
	border: #003eff;
	background-color: #ffffff;
}
.ui-state-active a,
.ui-state-active a:link,
.ui-state-active a:visited {
	color: #ffffff;
	text-decoration: none;
}


.ui-state-highlight,
.ui-widget-content .ui-state-highlight,
.ui-widget-header .ui-state-highlight {
	border: 1px solid #dad55e;
	background: #fffa90;
	color: #777620;
}
.ui-state-checked {
	border: 1px solid #dad55e;
	background: #fffa90;
}
.ui-state-highlight a,
.ui-widget-content .ui-state-highlight a,
.ui-widget-header .ui-state-highlight a {
	color: #777620;
}
.ui-state-error,
.ui-widget-content .ui-state-error,
.ui-widget-header .ui-state-error {
	border: 1px solid #f1a899;
	background: #fddfdf;
	color: #5f3f3f;
}
.ui-state-error a,
.ui-widget-content .ui-state-error a,
.ui-widget-header .ui-state-error a {
	color: #5f3f3f;
}
.ui-state-error-text,
.ui-widget-content .ui-state-error-text,
.ui-widget-header .ui-state-error-text {
	color: #5f3f3f;
}
.ui-priority-primary,
.ui-widget-content .ui-priority-primary,
.ui-widget-header .ui-priority-primary {
	font-weight: bold;
}
.ui-priority-secondary,
.ui-widget-content .ui-priority-secondary,
.ui-widget-header .ui-priority-secondary {
	opacity: .7;
	filter:Alpha(Opacity=70); 
	font-weight: normal;
}
.ui-state-disabled,
.ui-widget-content .ui-state-disabled,
.ui-widget-header .ui-state-disabled {
	opacity: .35;
	filter:Alpha(Opacity=35); 
	background-image: none;
}
.ui-state-disabled .ui-icon {
	filter:Alpha(Opacity=35); 
}




.ui-icon {
	width: 16px;
	height: 16px;
}
.ui-icon,
.ui-widget-content .ui-icon {
	background-image: url("images/ui-icons_444444_256x240.png");
}
.ui-widget-header .ui-icon {
	background-image: url("images/ui-icons_444444_256x240.png");
}
.ui-state-hover .ui-icon,
.ui-state-focus .ui-icon,
.ui-button:hover .ui-icon,
.ui-button:focus .ui-icon {
	background-image: url("images/ui-icons_555555_256x240.png");
}
.ui-state-active .ui-icon,
.ui-button:active .ui-icon {
	background-image: url("images/ui-icons_ffffff_256x240.png");
}
.ui-state-highlight .ui-icon,
.ui-button .ui-state-highlight.ui-icon {
	background-image: url("images/ui-icons_777620_256x240.png");
}
.ui-state-error .ui-icon,
.ui-state-error-text .ui-icon {
	background-image: url("images/ui-icons_cc0000_256x240.png");
}
.ui-button .ui-icon {
	background-image: url("images/ui-icons_777777_256x240.png");
}


.ui-icon-blank { background-position: 16px 16px; }
.ui-icon-caret-1-n { background-position: 0 0; }
.ui-icon-caret-1-ne { background-position: -16px 0; }
.ui-icon-caret-1-e { background-position: -32px 0; }
.ui-icon-caret-1-se { background-position: -48px 0; }
.ui-icon-caret-1-s { background-position: -65px 0; }
.ui-icon-caret-1-sw { background-position: -80px 0; }
.ui-icon-caret-1-w { background-position: -96px 0; }
.ui-icon-caret-1-nw { background-position: -112px 0; }
.ui-icon-caret-2-n-s { background-position: -128px 0; }
.ui-icon-caret-2-e-w { background-position: -144px 0; }
.ui-icon-triangle-1-n { background-position: 0 -16px; }
.ui-icon-triangle-1-ne { background-position: -16px -16px; }
.ui-icon-triangle-1-e { background-position: -32px -16px; }
.ui-icon-triangle-1-se { background-position: -48px -16px; }
.ui-icon-triangle-1-s { background-position: -65px -16px; }
.ui-icon-triangle-1-sw { background-position: -80px -16px; }
.ui-icon-triangle-1-w { background-position: -96px -16px; }
.ui-icon-triangle-1-nw { background-position: -112px -16px; }
.ui-icon-triangle-2-n-s { background-position: -128px -16px; }
.ui-icon-triangle-2-e-w { background-position: -144px -16px; }
.ui-icon-arrow-1-n { background-position: 0 -32px; }
.ui-icon-arrow-1-ne { background-position: -16px -32px; }
.ui-icon-arrow-1-e { background-position: -32px -32px; }
.ui-icon-arrow-1-se { background-position: -48px -32px; }
.ui-icon-arrow-1-s { background-position: -65px -32px; }
.ui-icon-arrow-1-sw { background-position: -80px -32px; }
.ui-icon-arrow-1-w { background-position: -96px -32px; }
.ui-icon-arrow-1-nw { background-position: -112px -32px; }
.ui-icon-arrow-2-n-s { background-position: -128px -32px; }
.ui-icon-arrow-2-ne-sw { background-position: -144px -32px; }
.ui-icon-arrow-2-e-w { background-position: -160px -32px; }
.ui-icon-arrow-2-se-nw { background-position: -176px -32px; }
.ui-icon-arrowstop-1-n { background-position: -192px -32px; }
.ui-icon-arrowstop-1-e { background-position: -208px -32px; }
.ui-icon-arrowstop-1-s { background-position: -224px -32px; }
.ui-icon-arrowstop-1-w { background-position: -240px -32px; }
.ui-icon-arrowthick-1-n { background-position: 1px -48px; }
.ui-icon-arrowthick-1-ne { background-position: -16px -48px; }
.ui-icon-arrowthick-1-e { background-position: -32px -48px; }
.ui-icon-arrowthick-1-se { background-position: -48px -48px; }
.ui-icon-arrowthick-1-s { background-position: -64px -48px; }
.ui-icon-arrowthick-1-sw { background-position: -80px -48px; }
.ui-icon-arrowthick-1-w { background-position: -96px -48px; }
.ui-icon-arrowthick-1-nw { background-position: -112px -48px; }
.ui-icon-arrowthick-2-n-s { background-position: -128px -48px; }
.ui-icon-arrowthick-2-ne-sw { background-position: -144px -48px; }
.ui-icon-arrowthick-2-e-w { background-position: -160px -48px; }
.ui-icon-arrowthick-2-se-nw { background-position: -176px -48px; }
.ui-icon-arrowthickstop-1-n { background-position: -192px -48px; }
.ui-icon-arrowthickstop-1-e { background-position: -208px -48px; }
.ui-icon-arrowthickstop-1-s { background-position: -224px -48px; }
.ui-icon-arrowthickstop-1-w { background-position: -240px -48px; }
.ui-icon-arrowreturnthick-1-w { background-position: 0 -64px; }
.ui-icon-arrowreturnthick-1-n { background-position: -16px -64px; }
.ui-icon-arrowreturnthick-1-e { background-position: -32px -64px; }
.ui-icon-arrowreturnthick-1-s { background-position: -48px -64px; }
.ui-icon-arrowreturn-1-w { background-position: -64px -64px; }
.ui-icon-arrowreturn-1-n { background-position: -80px -64px; }
.ui-icon-arrowreturn-1-e { background-position: -96px -64px; }
.ui-icon-arrowreturn-1-s { background-position: -112px -64px; }
.ui-icon-arrowrefresh-1-w { background-position: -128px -64px; }
.ui-icon-arrowrefresh-1-n { background-position: -144px -64px; }
.ui-icon-arrowrefresh-1-e { background-position: -160px -64px; }
.ui-icon-arrowrefresh-1-s { background-position: -176px -64px; }
.ui-icon-arrow-4 { background-position: 0 -80px; }
.ui-icon-arrow-4-diag { background-position: -16px -80px; }
.ui-icon-extlink { background-position: -32px -80px; }
.ui-icon-newwin { background-position: -48px -80px; }
.ui-icon-refresh { background-position: -64px -80px; }
.ui-icon-shuffle { background-position: -80px -80px; }
.ui-icon-transfer-e-w { background-position: -96px -80px; }
.ui-icon-transferthick-e-w { background-position: -112px -80px; }
.ui-icon-folder-collapsed { background-position: 0 -96px; }
.ui-icon-folder-open { background-position: -16px -96px; }
.ui-icon-document { background-position: -32px -96px; }
.ui-icon-document-b { background-position: -48px -96px; }
.ui-icon-note { background-position: -64px -96px; }
.ui-icon-mail-closed { background-position: -80px -96px; }
.ui-icon-mail-open { background-position: -96px -96px; }
.ui-icon-suitcase { background-position: -112px -96px; }
.ui-icon-comment { background-position: -128px -96px; }
.ui-icon-person { background-position: -144px -96px; }
.ui-icon-print { background-position: -160px -96px; }
.ui-icon-trash { background-position: -176px -96px; }
.ui-icon-locked { background-position: -192px -96px; }
.ui-icon-unlocked { background-position: -208px -96px; }
.ui-icon-bookmark { background-position: -224px -96px; }
.ui-icon-tag { background-position: -240px -96px; }
.ui-icon-home { background-position: 0 -112px; }
.ui-icon-flag { background-position: -16px -112px; }
.ui-icon-calendar { background-position: -32px -112px; }
.ui-icon-cart { background-position: -48px -112px; }
.ui-icon-pencil { background-position: -64px -112px; }
.ui-icon-clock { background-position: -80px -112px; }
.ui-icon-disk { background-position: -96px -112px; }
.ui-icon-calculator { background-position: -112px -112px; }
.ui-icon-zoomin { background-position: -128px -112px; }
.ui-icon-zoomout { background-position: -144px -112px; }
.ui-icon-search { background-position: -160px -112px; }
.ui-icon-wrench { background-position: -176px -112px; }
.ui-icon-gear { background-position: -192px -112px; }
.ui-icon-heart { background-position: -208px -112px; }
.ui-icon-star { background-position: -224px -112px; }
.ui-icon-link { background-position: -240px -112px; }
.ui-icon-cancel { background-position: 0 -128px; }
.ui-icon-plus { background-position: -16px -128px; }
.ui-icon-plusthick { background-position: -32px -128px; }
.ui-icon-minus { background-position: -48px -128px; }
.ui-icon-minusthick { background-position: -64px -128px; }
.ui-icon-close { background-position: -80px -128px; }
.ui-icon-closethick { background-position: -96px -128px; }
.ui-icon-key { background-position: -112px -128px; }
.ui-icon-lightbulb { background-position: -128px -128px; }
.ui-icon-scissors { background-position: -144px -128px; }
.ui-icon-clipboard { background-position: -160px -128px; }
.ui-icon-copy { background-position: -176px -128px; }
.ui-icon-contact { background-position: -192px -128px; }
.ui-icon-image { background-position: -208px -128px; }
.ui-icon-video { background-position: -224px -128px; }
.ui-icon-script { background-position: -240px -128px; }
.ui-icon-alert { background-position: 0 -144px; }
.ui-icon-info { background-position: -16px -144px; }
.ui-icon-notice { background-position: -32px -144px; }
.ui-icon-help { background-position: -48px -144px; }
.ui-icon-check { background-position: -64px -144px; }
.ui-icon-bullet { background-position: -80px -144px; }
.ui-icon-radio-on { background-position: -96px -144px; }
.ui-icon-radio-off { background-position: -112px -144px; }
.ui-icon-pin-w { background-position: -128px -144px; }
.ui-icon-pin-s { background-position: -144px -144px; }
.ui-icon-play { background-position: 0 -160px; }
.ui-icon-pause { background-position: -16px -160px; }
.ui-icon-seek-next { background-position: -32px -160px; }
.ui-icon-seek-prev { background-position: -48px -160px; }
.ui-icon-seek-end { background-position: -64px -160px; }
.ui-icon-seek-start { background-position: -80px -160px; }

.ui-icon-seek-first { background-position: -80px -160px; }
.ui-icon-stop { background-position: -96px -160px; }
.ui-icon-eject { background-position: -112px -160px; }
.ui-icon-volume-off { background-position: -128px -160px; }
.ui-icon-volume-on { background-position: -144px -160px; }
.ui-icon-power { background-position: 0 -176px; }
.ui-icon-signal-diag { background-position: -16px -176px; }
.ui-icon-signal { background-position: -32px -176px; }
.ui-icon-battery-0 { background-position: -48px -176px; }
.ui-icon-battery-1 { background-position: -64px -176px; }
.ui-icon-battery-2 { background-position: -80px -176px; }
.ui-icon-battery-3 { background-position: -96px -176px; }
.ui-icon-circle-plus { background-position: 0 -192px; }
.ui-icon-circle-minus { background-position: -16px -192px; }
.ui-icon-circle-close { background-position: -32px -192px; }
.ui-icon-circle-triangle-e { background-position: -48px -192px; }
.ui-icon-circle-triangle-s { background-position: -64px -192px; }
.ui-icon-circle-triangle-w { background-position: -80px -192px; }
.ui-icon-circle-triangle-n { background-position: -96px -192px; }
.ui-icon-circle-arrow-e { background-position: -112px -192px; }
.ui-icon-circle-arrow-s { background-position: -128px -192px; }
.ui-icon-circle-arrow-w { background-position: -144px -192px; }
.ui-icon-circle-arrow-n { background-position: -160px -192px; }
.ui-icon-circle-zoomin { background-position: -176px -192px; }
.ui-icon-circle-zoomout { background-position: -192px -192px; }
.ui-icon-circle-check { background-position: -208px -192px; }
.ui-icon-circlesmall-plus { background-position: 0 -208px; }
.ui-icon-circlesmall-minus { background-position: -16px -208px; }
.ui-icon-circlesmall-close { background-position: -32px -208px; }
.ui-icon-squaresmall-plus { background-position: -48px -208px; }
.ui-icon-squaresmall-minus { background-position: -64px -208px; }
.ui-icon-squaresmall-close { background-position: -80px -208px; }
.ui-icon-grip-dotted-vertical { background-position: 0 -224px; }
.ui-icon-grip-dotted-horizontal { background-position: -16px -224px; }
.ui-icon-grip-solid-vertical { background-position: -32px -224px; }
.ui-icon-grip-solid-horizontal { background-position: -48px -224px; }
.ui-icon-gripsmall-diagonal-se { background-position: -64px -224px; }
.ui-icon-grip-diagonal-se { background-position: -80px -224px; }





.ui-corner-all,
.ui-corner-top,
.ui-corner-left,
.ui-corner-tl {
	border-top-left-radius: 3px;
}
.ui-corner-all,
.ui-corner-top,
.ui-corner-right,
.ui-corner-tr {
	border-top-right-radius: 3px;
}
.ui-corner-all,
.ui-corner-bottom,
.ui-corner-left,
.ui-corner-bl {
	border-bottom-left-radius: 3px;
}
.ui-corner-all,
.ui-corner-bottom,
.ui-corner-right,
.ui-corner-br {
	border-bottom-right-radius: 3px;
}


.ui-widget-overlay {
	background: #aaaaaa;
	opacity: .3;
	filter: Alpha(Opacity=30); 
}
.ui-widget-shadow {
	-webkit-box-shadow: 0px 0px 5px #666666;
	box-shadow: 0px 0px 5px #666666;
} 
</style>
<script src='js/jquery-1.12.4.js'></script>
<script src='js/jquery-ui.js'></script>

<script>
$( function() {
	$( '#tabs-collect-classes-from-hierarchical-partition' ).tabs();
} );
</script>
<div id='tabs-collect-classes-from-hierarchical-partition'>
<ul>
<li><a href='#tab-collect-classes-from-hierarchical-partition-1'>n_signatures ≥ 422</a></li>
<li><a href='#tab-collect-classes-from-hierarchical-partition-2'>n_signatures ≥ 548</a></li>
<li><a href='#tab-collect-classes-from-hierarchical-partition-3'>n_signatures ≥ 606</a></li>
<li><a href='#tab-collect-classes-from-hierarchical-partition-4'>n_signatures ≥ 796</a></li>
<li><a href='#tab-collect-classes-from-hierarchical-partition-5'>n_signatures ≥ 1634</a></li>
<li><a href='#tab-collect-classes-from-hierarchical-partition-6'>n_signatures ≥ 3037</a></li>
</ul>
<div id='tab-collect-classes-from-hierarchical-partition-1'>
<pre><code class="r">collect_classes(res_rh, merge_node = merge_node_param(min_n_signatures = 422))
</code></pre>

<p><img src="figure_cola/tab-collect-classes-from-hierarchical-partition-1-1.png" alt="plot of chunk tab-collect-classes-from-hierarchical-partition-1"/></p>

</div>
<div id='tab-collect-classes-from-hierarchical-partition-2'>
<pre><code class="r">collect_classes(res_rh, merge_node = merge_node_param(min_n_signatures = 548))
</code></pre>

<p><img src="figure_cola/tab-collect-classes-from-hierarchical-partition-2-1.png" alt="plot of chunk tab-collect-classes-from-hierarchical-partition-2"/></p>

</div>
<div id='tab-collect-classes-from-hierarchical-partition-3'>
<pre><code class="r">collect_classes(res_rh, merge_node = merge_node_param(min_n_signatures = 606))
</code></pre>

<p><img src="figure_cola/tab-collect-classes-from-hierarchical-partition-3-1.png" alt="plot of chunk tab-collect-classes-from-hierarchical-partition-3"/></p>

</div>
<div id='tab-collect-classes-from-hierarchical-partition-4'>
<pre><code class="r">collect_classes(res_rh, merge_node = merge_node_param(min_n_signatures = 796))
</code></pre>

<p><img src="figure_cola/tab-collect-classes-from-hierarchical-partition-4-1.png" alt="plot of chunk tab-collect-classes-from-hierarchical-partition-4"/></p>

</div>
<div id='tab-collect-classes-from-hierarchical-partition-5'>
<pre><code class="r">collect_classes(res_rh, merge_node = merge_node_param(min_n_signatures = 1634))
</code></pre>

<p><img src="figure_cola/tab-collect-classes-from-hierarchical-partition-5-1.png" alt="plot of chunk tab-collect-classes-from-hierarchical-partition-5"/></p>

</div>
<div id='tab-collect-classes-from-hierarchical-partition-6'>
<pre><code class="r">collect_classes(res_rh, merge_node = merge_node_param(min_n_signatures = 3037))
</code></pre>

<p><img src="figure_cola/tab-collect-classes-from-hierarchical-partition-6-1.png" alt="plot of chunk tab-collect-classes-from-hierarchical-partition-6"/></p>

</div>
</div>

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it).


<script>
$( function() {
	$( '#tabs-get-classes-from-hierarchical-partition' ).tabs();
} );
</script>
<div id='tabs-get-classes-from-hierarchical-partition'>
<ul>
<li><a href='#tab-get-classes-from-hierarchical-partition-1'>n_signatures ≥ 422</a></li>
<li><a href='#tab-get-classes-from-hierarchical-partition-2'>n_signatures ≥ 548</a></li>
<li><a href='#tab-get-classes-from-hierarchical-partition-3'>n_signatures ≥ 606</a></li>
<li><a href='#tab-get-classes-from-hierarchical-partition-4'>n_signatures ≥ 796</a></li>
<li><a href='#tab-get-classes-from-hierarchical-partition-5'>n_signatures ≥ 1634</a></li>
<li><a href='#tab-get-classes-from-hierarchical-partition-6'>n_signatures ≥ 3037</a></li>
</ul>

<div id='tab-get-classes-from-hierarchical-partition-1'>
<p><a id='tab-get-classes-from-hierarchical-partition-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">get_classes(res_rh, merge_node = merge_node_param(min_n_signatures = 422))
</code></pre>

<pre><code>#&gt;  T0_CT_A01  T0_CT_A03  T0_CT_A05  T0_CT_A06  T0_CT_A07  T0_CT_A08  T0_CT_A10  T0_CT_A11  T0_CT_B01 
#&gt;      &quot;032&quot;     &quot;0312&quot;     &quot;0312&quot;      &quot;032&quot;     &quot;0312&quot;      &quot;021&quot;      &quot;032&quot;      &quot;032&quot;      &quot;032&quot; 
#&gt;  T0_CT_B03  T0_CT_B05  T0_CT_B07  T0_CT_B08  T0_CT_B09  T0_CT_C02  T0_CT_C03  T0_CT_C05  T0_CT_C06 
#&gt;     &quot;0311&quot;      &quot;032&quot;      &quot;032&quot;     &quot;0311&quot;     &quot;0312&quot;     &quot;0311&quot;      &quot;021&quot;      &quot;011&quot;      &quot;011&quot; 
#&gt;  T0_CT_C07  T0_CT_C08  T0_CT_C09  T0_CT_C11  T0_CT_C12  T0_CT_D01  T0_CT_D02  T0_CT_D03  T0_CT_D05 
#&gt;      &quot;032&quot;     &quot;0312&quot;      &quot;011&quot;      &quot;011&quot;     &quot;0312&quot;      &quot;032&quot;      &quot;032&quot;     &quot;0311&quot;      &quot;032&quot; 
#&gt;  T0_CT_D06  T0_CT_D07  T0_CT_D08  T0_CT_D09  T0_CT_D11  T0_CT_D12  T0_CT_E01  T0_CT_E03  T0_CT_E04 
#&gt;     &quot;0311&quot;      &quot;032&quot;     &quot;0311&quot;      &quot;011&quot;      &quot;011&quot;     &quot;0312&quot;     &quot;0311&quot;      &quot;032&quot;      &quot;011&quot; 
#&gt;  T0_CT_E05  T0_CT_E06  T0_CT_E07  T0_CT_E08  T0_CT_E09  T0_CT_E10  T0_CT_E11  T0_CT_E12  T0_CT_F01 
#&gt;      &quot;022&quot;      &quot;032&quot;     &quot;0312&quot;     &quot;0311&quot;      &quot;032&quot;      &quot;032&quot;     &quot;0312&quot;      &quot;032&quot;     &quot;0312&quot; 
#&gt;  T0_CT_F02  T0_CT_F03  T0_CT_F04  T0_CT_F05  T0_CT_F06  T0_CT_F07  T0_CT_F09  T0_CT_F11  T0_CT_F12 
#&gt;     &quot;0312&quot;     &quot;0312&quot;     &quot;0312&quot;      &quot;032&quot;     &quot;0311&quot;      &quot;032&quot;      &quot;011&quot;      &quot;032&quot;     &quot;0312&quot; 
#&gt;  T0_CT_G01  T0_CT_G02  T0_CT_G03  T0_CT_G04  T0_CT_G07  T0_CT_G08  T0_CT_G09  T0_CT_G11  T0_CT_H01 
#&gt;     &quot;0312&quot;      &quot;032&quot;      &quot;032&quot;     &quot;0311&quot;     &quot;0311&quot;     &quot;0311&quot;      &quot;022&quot;      &quot;032&quot;      &quot;022&quot; 
#&gt;  T0_CT_H02  T0_CT_H04  T0_CT_H05  T0_CT_H08  T0_CT_H09  T0_CT_H12 T24_CT_A01 T24_CT_A03 T24_CT_A04 
#&gt;      &quot;032&quot;      &quot;032&quot;      &quot;032&quot;      &quot;032&quot;      &quot;032&quot;      &quot;032&quot;      &quot;021&quot;      &quot;011&quot;      &quot;011&quot; 
#&gt; T24_CT_A05 T24_CT_A07 T24_CT_A08 T24_CT_A09 T24_CT_A10 T24_CT_B01 T24_CT_B02 T24_CT_B03 T24_CT_B05 
#&gt;     &quot;0312&quot;      &quot;011&quot;      &quot;021&quot;      &quot;021&quot;      &quot;022&quot;      &quot;022&quot;      &quot;021&quot;     &quot;0123&quot;      &quot;011&quot; 
#&gt; T24_CT_B06 T24_CT_B07 T24_CT_B08 T24_CT_B09 T24_CT_B11 T24_CT_C01 T24_CT_C02 T24_CT_C03 T24_CT_C05 
#&gt;      &quot;011&quot;      &quot;021&quot;      &quot;011&quot;      &quot;021&quot;     &quot;0312&quot;      &quot;021&quot;      &quot;011&quot;      &quot;021&quot;      &quot;011&quot; 
#&gt; T24_CT_C07 T24_CT_C08 T24_CT_C09 T24_CT_C10 T24_CT_C11 T24_CT_C12 T24_CT_D01 T24_CT_D02 T24_CT_D03 
#&gt;      &quot;011&quot;      &quot;011&quot;      &quot;011&quot;      &quot;021&quot;      &quot;022&quot;      &quot;011&quot;      &quot;011&quot;      &quot;011&quot;      &quot;011&quot; 
#&gt; T24_CT_D04 T24_CT_D05 T24_CT_D06 T24_CT_D07 T24_CT_D08 T24_CT_D09 T24_CT_D10 T24_CT_D11 T24_CT_E01 
#&gt;      &quot;021&quot;      &quot;022&quot;      &quot;021&quot;      &quot;022&quot;      &quot;011&quot;      &quot;011&quot;      &quot;011&quot;      &quot;011&quot;      &quot;022&quot; 
#&gt; T24_CT_E02 T24_CT_E04 T24_CT_E05 T24_CT_E07 T24_CT_E09 T24_CT_E11 T24_CT_E12 T24_CT_F01 T24_CT_F02 
#&gt;      &quot;021&quot;     &quot;0122&quot;      &quot;021&quot;     &quot;0123&quot;      &quot;021&quot;      &quot;021&quot;      &quot;011&quot;      &quot;011&quot;      &quot;011&quot; 
#&gt; T24_CT_F03 T24_CT_F04 T24_CT_F05 T24_CT_F07 T24_CT_F08 T24_CT_F09 T24_CT_F10 T24_CT_F11 T24_CT_F12 
#&gt;      &quot;022&quot;      &quot;022&quot;      &quot;022&quot;      &quot;022&quot;      &quot;021&quot;      &quot;011&quot;      &quot;022&quot;      &quot;022&quot;     &quot;0313&quot; 
#&gt; T24_CT_G01 T24_CT_G02 T24_CT_G03 T24_CT_G04 T24_CT_G05 T24_CT_G06 T24_CT_G08 T24_CT_G10 T24_CT_G11 
#&gt;      &quot;011&quot;      &quot;021&quot;      &quot;021&quot;      &quot;011&quot;      &quot;011&quot;      &quot;021&quot;      &quot;021&quot;      &quot;011&quot;      &quot;021&quot; 
#&gt; T24_CT_G12 T24_CT_H01 T24_CT_H02 T24_CT_H03 T24_CT_H05 T24_CT_H07 T24_CT_H09 T24_CT_H12 T48_CT_A01 
#&gt;      &quot;021&quot;     &quot;0312&quot;      &quot;021&quot;      &quot;011&quot;      &quot;022&quot;      &quot;022&quot;      &quot;021&quot;      &quot;021&quot;     &quot;0122&quot; 
#&gt; T48_CT_A02 T48_CT_A03 T48_CT_A04 T48_CT_A05 T48_CT_A06 T48_CT_A07 T48_CT_A08 T48_CT_A09 T48_CT_A10 
#&gt;     &quot;0122&quot;      &quot;021&quot;      &quot;011&quot;     &quot;0123&quot;      &quot;021&quot;     &quot;0313&quot;     &quot;0122&quot;      &quot;022&quot;      &quot;011&quot; 
#&gt; T48_CT_A11 T48_CT_A12 T48_CT_B01 T48_CT_B02 T48_CT_B03 T48_CT_B04 T48_CT_B06 T48_CT_B08 T48_CT_B10 
#&gt;      &quot;011&quot;      &quot;022&quot;      &quot;011&quot;      &quot;011&quot;      &quot;021&quot;      &quot;021&quot;      &quot;011&quot;      &quot;021&quot;      &quot;011&quot; 
#&gt; T48_CT_B11 T48_CT_B12 T48_CT_C01 T48_CT_C02 T48_CT_C03 T48_CT_C04 T48_CT_C05 T48_CT_C06 T48_CT_C07 
#&gt;     &quot;0122&quot;      &quot;022&quot;      &quot;011&quot;      &quot;021&quot;      &quot;011&quot;      &quot;021&quot;     &quot;0123&quot;     &quot;0123&quot;      &quot;022&quot; 
#&gt; T48_CT_C09 T48_CT_C10 T48_CT_C11 T48_CT_D01 T48_CT_D02 T48_CT_D03 T48_CT_D04 T48_CT_D06 T48_CT_D07 
#&gt;     &quot;0122&quot;      &quot;022&quot;     &quot;0122&quot;      &quot;011&quot;      &quot;022&quot;      &quot;021&quot;      &quot;011&quot;     &quot;0122&quot;      &quot;011&quot; 
#&gt; T48_CT_D08 T48_CT_D09 T48_CT_D10 T48_CT_D11 T48_CT_D12 T48_CT_E01 T48_CT_E02 T48_CT_E03 T48_CT_E04 
#&gt;      &quot;021&quot;      &quot;011&quot;      &quot;022&quot;     &quot;0122&quot;      &quot;021&quot;     &quot;0123&quot;      &quot;022&quot;     &quot;0123&quot;      &quot;022&quot; 
#&gt; T48_CT_E05 T48_CT_E06 T48_CT_E07 T48_CT_E08 T48_CT_E10 T48_CT_E11 T48_CT_E12 T48_CT_F01 T48_CT_F02 
#&gt;      &quot;011&quot;      &quot;021&quot;      &quot;021&quot;      &quot;011&quot;      &quot;021&quot;      &quot;022&quot;     &quot;0313&quot;      &quot;011&quot;      &quot;022&quot; 
#&gt; T48_CT_F03 T48_CT_F05 T48_CT_F07 T48_CT_F09 T48_CT_F10 T48_CT_F11 T48_CT_G01 T48_CT_G02 T48_CT_G03 
#&gt;      &quot;022&quot;      &quot;011&quot;      &quot;022&quot;      &quot;011&quot;      &quot;022&quot;      &quot;021&quot;      &quot;021&quot;     &quot;0122&quot;      &quot;011&quot; 
#&gt; T48_CT_G07 T48_CT_G08 T48_CT_G09 T48_CT_G10 T48_CT_G11 T48_CT_G12 T48_CT_H01 T48_CT_H02 T48_CT_H04 
#&gt;      &quot;011&quot;     &quot;0123&quot;     &quot;0123&quot;     &quot;0312&quot;      &quot;022&quot;      &quot;011&quot;      &quot;011&quot;      &quot;022&quot;      &quot;021&quot; 
#&gt; T48_CT_H05 T48_CT_H06 T48_CT_H07 T48_CT_H08 T48_CT_H11 T48_CT_H12 T72_CT_A01 T72_CT_A05 T72_CT_A08 
#&gt;      &quot;011&quot;      &quot;011&quot;      &quot;022&quot;     &quot;0122&quot;      &quot;022&quot;     &quot;0122&quot;     &quot;0121&quot;     &quot;0121&quot;      &quot;023&quot; 
#&gt; T72_CT_A09 T72_CT_A11 T72_CT_B01 T72_CT_B02 T72_CT_B03 T72_CT_B04 T72_CT_B05 T72_CT_B06 T72_CT_B08 
#&gt;     &quot;0121&quot;     &quot;0121&quot;     &quot;0121&quot;      &quot;023&quot;      &quot;023&quot;      &quot;023&quot;      &quot;023&quot;      &quot;023&quot;     &quot;0313&quot; 
#&gt; T72_CT_B09 T72_CT_B11 T72_CT_B12 T72_CT_C04 T72_CT_C06 T72_CT_C07 T72_CT_C09 T72_CT_C11 T72_CT_D01 
#&gt;      &quot;021&quot;      &quot;023&quot;     &quot;0121&quot;     &quot;0121&quot;      &quot;023&quot;     &quot;0122&quot;      &quot;023&quot;     &quot;0313&quot;     &quot;0121&quot; 
#&gt; T72_CT_D03 T72_CT_D04 T72_CT_D05 T72_CT_D07 T72_CT_D10 T72_CT_D11 T72_CT_E04 T72_CT_E05 T72_CT_E07 
#&gt;      &quot;023&quot;     &quot;0121&quot;      &quot;023&quot;      &quot;023&quot;      &quot;023&quot;      &quot;023&quot;      &quot;023&quot;     &quot;0121&quot;     &quot;0121&quot; 
#&gt; T72_CT_F01 T72_CT_F05 T72_CT_F07 T72_CT_F10 T72_CT_F11 T72_CT_G03 T72_CT_G04 T72_CT_G06 T72_CT_G08 
#&gt;     &quot;0121&quot;     &quot;0121&quot;      &quot;023&quot;     &quot;0121&quot;     &quot;0121&quot;      &quot;023&quot;      &quot;023&quot;     &quot;0121&quot;      &quot;032&quot; 
#&gt; T72_CT_G10 T72_CT_G11 T72_CT_H01 T72_CT_H03 T72_CT_H05 T72_CT_H08 T72_CT_H09 T72_CT_H10 T72_CT_H11 
#&gt;     &quot;0121&quot;      &quot;023&quot;     &quot;0121&quot;     &quot;0121&quot;     &quot;0313&quot;     &quot;0313&quot;     &quot;0313&quot;      &quot;023&quot;      &quot;023&quot; 
#&gt; T72_CT_H12 
#&gt;     &quot;0121&quot;
</code></pre>

<script>
$('#tab-get-classes-from-hierarchical-partition-1-a').parent().next().next().hide();
$('#tab-get-classes-from-hierarchical-partition-1-a').click(function(){
  $('#tab-get-classes-from-hierarchical-partition-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-get-classes-from-hierarchical-partition-2'>
<p><a id='tab-get-classes-from-hierarchical-partition-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">get_classes(res_rh, merge_node = merge_node_param(min_n_signatures = 548))
</code></pre>

<pre><code>#&gt;  T0_CT_A01  T0_CT_A03  T0_CT_A05  T0_CT_A06  T0_CT_A07  T0_CT_A08  T0_CT_A10  T0_CT_A11  T0_CT_B01 
#&gt;      &quot;032&quot;     &quot;0312&quot;     &quot;0312&quot;      &quot;032&quot;     &quot;0312&quot;      &quot;021&quot;      &quot;032&quot;      &quot;032&quot;      &quot;032&quot; 
#&gt;  T0_CT_B03  T0_CT_B05  T0_CT_B07  T0_CT_B08  T0_CT_B09  T0_CT_C02  T0_CT_C03  T0_CT_C05  T0_CT_C06 
#&gt;     &quot;0311&quot;      &quot;032&quot;      &quot;032&quot;     &quot;0311&quot;     &quot;0312&quot;     &quot;0311&quot;      &quot;021&quot;      &quot;011&quot;      &quot;011&quot; 
#&gt;  T0_CT_C07  T0_CT_C08  T0_CT_C09  T0_CT_C11  T0_CT_C12  T0_CT_D01  T0_CT_D02  T0_CT_D03  T0_CT_D05 
#&gt;      &quot;032&quot;     &quot;0312&quot;      &quot;011&quot;      &quot;011&quot;     &quot;0312&quot;      &quot;032&quot;      &quot;032&quot;     &quot;0311&quot;      &quot;032&quot; 
#&gt;  T0_CT_D06  T0_CT_D07  T0_CT_D08  T0_CT_D09  T0_CT_D11  T0_CT_D12  T0_CT_E01  T0_CT_E03  T0_CT_E04 
#&gt;     &quot;0311&quot;      &quot;032&quot;     &quot;0311&quot;      &quot;011&quot;      &quot;011&quot;     &quot;0312&quot;     &quot;0311&quot;      &quot;032&quot;      &quot;011&quot; 
#&gt;  T0_CT_E05  T0_CT_E06  T0_CT_E07  T0_CT_E08  T0_CT_E09  T0_CT_E10  T0_CT_E11  T0_CT_E12  T0_CT_F01 
#&gt;      &quot;022&quot;      &quot;032&quot;     &quot;0312&quot;     &quot;0311&quot;      &quot;032&quot;      &quot;032&quot;     &quot;0312&quot;      &quot;032&quot;     &quot;0312&quot; 
#&gt;  T0_CT_F02  T0_CT_F03  T0_CT_F04  T0_CT_F05  T0_CT_F06  T0_CT_F07  T0_CT_F09  T0_CT_F11  T0_CT_F12 
#&gt;     &quot;0312&quot;     &quot;0312&quot;     &quot;0312&quot;      &quot;032&quot;     &quot;0311&quot;      &quot;032&quot;      &quot;011&quot;      &quot;032&quot;     &quot;0312&quot; 
#&gt;  T0_CT_G01  T0_CT_G02  T0_CT_G03  T0_CT_G04  T0_CT_G07  T0_CT_G08  T0_CT_G09  T0_CT_G11  T0_CT_H01 
#&gt;     &quot;0312&quot;      &quot;032&quot;      &quot;032&quot;     &quot;0311&quot;     &quot;0311&quot;     &quot;0311&quot;      &quot;022&quot;      &quot;032&quot;      &quot;022&quot; 
#&gt;  T0_CT_H02  T0_CT_H04  T0_CT_H05  T0_CT_H08  T0_CT_H09  T0_CT_H12 T24_CT_A01 T24_CT_A03 T24_CT_A04 
#&gt;      &quot;032&quot;      &quot;032&quot;      &quot;032&quot;      &quot;032&quot;      &quot;032&quot;      &quot;032&quot;      &quot;021&quot;      &quot;011&quot;      &quot;011&quot; 
#&gt; T24_CT_A05 T24_CT_A07 T24_CT_A08 T24_CT_A09 T24_CT_A10 T24_CT_B01 T24_CT_B02 T24_CT_B03 T24_CT_B05 
#&gt;     &quot;0312&quot;      &quot;011&quot;      &quot;021&quot;      &quot;021&quot;      &quot;022&quot;      &quot;022&quot;      &quot;021&quot;      &quot;012&quot;      &quot;011&quot; 
#&gt; T24_CT_B06 T24_CT_B07 T24_CT_B08 T24_CT_B09 T24_CT_B11 T24_CT_C01 T24_CT_C02 T24_CT_C03 T24_CT_C05 
#&gt;      &quot;011&quot;      &quot;021&quot;      &quot;011&quot;      &quot;021&quot;     &quot;0312&quot;      &quot;021&quot;      &quot;011&quot;      &quot;021&quot;      &quot;011&quot; 
#&gt; T24_CT_C07 T24_CT_C08 T24_CT_C09 T24_CT_C10 T24_CT_C11 T24_CT_C12 T24_CT_D01 T24_CT_D02 T24_CT_D03 
#&gt;      &quot;011&quot;      &quot;011&quot;      &quot;011&quot;      &quot;021&quot;      &quot;022&quot;      &quot;011&quot;      &quot;011&quot;      &quot;011&quot;      &quot;011&quot; 
#&gt; T24_CT_D04 T24_CT_D05 T24_CT_D06 T24_CT_D07 T24_CT_D08 T24_CT_D09 T24_CT_D10 T24_CT_D11 T24_CT_E01 
#&gt;      &quot;021&quot;      &quot;022&quot;      &quot;021&quot;      &quot;022&quot;      &quot;011&quot;      &quot;011&quot;      &quot;011&quot;      &quot;011&quot;      &quot;022&quot; 
#&gt; T24_CT_E02 T24_CT_E04 T24_CT_E05 T24_CT_E07 T24_CT_E09 T24_CT_E11 T24_CT_E12 T24_CT_F01 T24_CT_F02 
#&gt;      &quot;021&quot;      &quot;012&quot;      &quot;021&quot;      &quot;012&quot;      &quot;021&quot;      &quot;021&quot;      &quot;011&quot;      &quot;011&quot;      &quot;011&quot; 
#&gt; T24_CT_F03 T24_CT_F04 T24_CT_F05 T24_CT_F07 T24_CT_F08 T24_CT_F09 T24_CT_F10 T24_CT_F11 T24_CT_F12 
#&gt;      &quot;022&quot;      &quot;022&quot;      &quot;022&quot;      &quot;022&quot;      &quot;021&quot;      &quot;011&quot;      &quot;022&quot;      &quot;022&quot;     &quot;0313&quot; 
#&gt; T24_CT_G01 T24_CT_G02 T24_CT_G03 T24_CT_G04 T24_CT_G05 T24_CT_G06 T24_CT_G08 T24_CT_G10 T24_CT_G11 
#&gt;      &quot;011&quot;      &quot;021&quot;      &quot;021&quot;      &quot;011&quot;      &quot;011&quot;      &quot;021&quot;      &quot;021&quot;      &quot;011&quot;      &quot;021&quot; 
#&gt; T24_CT_G12 T24_CT_H01 T24_CT_H02 T24_CT_H03 T24_CT_H05 T24_CT_H07 T24_CT_H09 T24_CT_H12 T48_CT_A01 
#&gt;      &quot;021&quot;     &quot;0312&quot;      &quot;021&quot;      &quot;011&quot;      &quot;022&quot;      &quot;022&quot;      &quot;021&quot;      &quot;021&quot;      &quot;012&quot; 
#&gt; T48_CT_A02 T48_CT_A03 T48_CT_A04 T48_CT_A05 T48_CT_A06 T48_CT_A07 T48_CT_A08 T48_CT_A09 T48_CT_A10 
#&gt;      &quot;012&quot;      &quot;021&quot;      &quot;011&quot;      &quot;012&quot;      &quot;021&quot;     &quot;0313&quot;      &quot;012&quot;      &quot;022&quot;      &quot;011&quot; 
#&gt; T48_CT_A11 T48_CT_A12 T48_CT_B01 T48_CT_B02 T48_CT_B03 T48_CT_B04 T48_CT_B06 T48_CT_B08 T48_CT_B10 
#&gt;      &quot;011&quot;      &quot;022&quot;      &quot;011&quot;      &quot;011&quot;      &quot;021&quot;      &quot;021&quot;      &quot;011&quot;      &quot;021&quot;      &quot;011&quot; 
#&gt; T48_CT_B11 T48_CT_B12 T48_CT_C01 T48_CT_C02 T48_CT_C03 T48_CT_C04 T48_CT_C05 T48_CT_C06 T48_CT_C07 
#&gt;      &quot;012&quot;      &quot;022&quot;      &quot;011&quot;      &quot;021&quot;      &quot;011&quot;      &quot;021&quot;      &quot;012&quot;      &quot;012&quot;      &quot;022&quot; 
#&gt; T48_CT_C09 T48_CT_C10 T48_CT_C11 T48_CT_D01 T48_CT_D02 T48_CT_D03 T48_CT_D04 T48_CT_D06 T48_CT_D07 
#&gt;      &quot;012&quot;      &quot;022&quot;      &quot;012&quot;      &quot;011&quot;      &quot;022&quot;      &quot;021&quot;      &quot;011&quot;      &quot;012&quot;      &quot;011&quot; 
#&gt; T48_CT_D08 T48_CT_D09 T48_CT_D10 T48_CT_D11 T48_CT_D12 T48_CT_E01 T48_CT_E02 T48_CT_E03 T48_CT_E04 
#&gt;      &quot;021&quot;      &quot;011&quot;      &quot;022&quot;      &quot;012&quot;      &quot;021&quot;      &quot;012&quot;      &quot;022&quot;      &quot;012&quot;      &quot;022&quot; 
#&gt; T48_CT_E05 T48_CT_E06 T48_CT_E07 T48_CT_E08 T48_CT_E10 T48_CT_E11 T48_CT_E12 T48_CT_F01 T48_CT_F02 
#&gt;      &quot;011&quot;      &quot;021&quot;      &quot;021&quot;      &quot;011&quot;      &quot;021&quot;      &quot;022&quot;     &quot;0313&quot;      &quot;011&quot;      &quot;022&quot; 
#&gt; T48_CT_F03 T48_CT_F05 T48_CT_F07 T48_CT_F09 T48_CT_F10 T48_CT_F11 T48_CT_G01 T48_CT_G02 T48_CT_G03 
#&gt;      &quot;022&quot;      &quot;011&quot;      &quot;022&quot;      &quot;011&quot;      &quot;022&quot;      &quot;021&quot;      &quot;021&quot;      &quot;012&quot;      &quot;011&quot; 
#&gt; T48_CT_G07 T48_CT_G08 T48_CT_G09 T48_CT_G10 T48_CT_G11 T48_CT_G12 T48_CT_H01 T48_CT_H02 T48_CT_H04 
#&gt;      &quot;011&quot;      &quot;012&quot;      &quot;012&quot;     &quot;0312&quot;      &quot;022&quot;      &quot;011&quot;      &quot;011&quot;      &quot;022&quot;      &quot;021&quot; 
#&gt; T48_CT_H05 T48_CT_H06 T48_CT_H07 T48_CT_H08 T48_CT_H11 T48_CT_H12 T72_CT_A01 T72_CT_A05 T72_CT_A08 
#&gt;      &quot;011&quot;      &quot;011&quot;      &quot;022&quot;      &quot;012&quot;      &quot;022&quot;      &quot;012&quot;      &quot;012&quot;      &quot;012&quot;      &quot;023&quot; 
#&gt; T72_CT_A09 T72_CT_A11 T72_CT_B01 T72_CT_B02 T72_CT_B03 T72_CT_B04 T72_CT_B05 T72_CT_B06 T72_CT_B08 
#&gt;      &quot;012&quot;      &quot;012&quot;      &quot;012&quot;      &quot;023&quot;      &quot;023&quot;      &quot;023&quot;      &quot;023&quot;      &quot;023&quot;     &quot;0313&quot; 
#&gt; T72_CT_B09 T72_CT_B11 T72_CT_B12 T72_CT_C04 T72_CT_C06 T72_CT_C07 T72_CT_C09 T72_CT_C11 T72_CT_D01 
#&gt;      &quot;021&quot;      &quot;023&quot;      &quot;012&quot;      &quot;012&quot;      &quot;023&quot;      &quot;012&quot;      &quot;023&quot;     &quot;0313&quot;      &quot;012&quot; 
#&gt; T72_CT_D03 T72_CT_D04 T72_CT_D05 T72_CT_D07 T72_CT_D10 T72_CT_D11 T72_CT_E04 T72_CT_E05 T72_CT_E07 
#&gt;      &quot;023&quot;      &quot;012&quot;      &quot;023&quot;      &quot;023&quot;      &quot;023&quot;      &quot;023&quot;      &quot;023&quot;      &quot;012&quot;      &quot;012&quot; 
#&gt; T72_CT_F01 T72_CT_F05 T72_CT_F07 T72_CT_F10 T72_CT_F11 T72_CT_G03 T72_CT_G04 T72_CT_G06 T72_CT_G08 
#&gt;      &quot;012&quot;      &quot;012&quot;      &quot;023&quot;      &quot;012&quot;      &quot;012&quot;      &quot;023&quot;      &quot;023&quot;      &quot;012&quot;      &quot;032&quot; 
#&gt; T72_CT_G10 T72_CT_G11 T72_CT_H01 T72_CT_H03 T72_CT_H05 T72_CT_H08 T72_CT_H09 T72_CT_H10 T72_CT_H11 
#&gt;      &quot;012&quot;      &quot;023&quot;      &quot;012&quot;      &quot;012&quot;     &quot;0313&quot;     &quot;0313&quot;     &quot;0313&quot;      &quot;023&quot;      &quot;023&quot; 
#&gt; T72_CT_H12 
#&gt;      &quot;012&quot;
</code></pre>

<script>
$('#tab-get-classes-from-hierarchical-partition-2-a').parent().next().next().hide();
$('#tab-get-classes-from-hierarchical-partition-2-a').click(function(){
  $('#tab-get-classes-from-hierarchical-partition-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-get-classes-from-hierarchical-partition-3'>
<p><a id='tab-get-classes-from-hierarchical-partition-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">get_classes(res_rh, merge_node = merge_node_param(min_n_signatures = 606))
</code></pre>

<pre><code>#&gt;  T0_CT_A01  T0_CT_A03  T0_CT_A05  T0_CT_A06  T0_CT_A07  T0_CT_A08  T0_CT_A10  T0_CT_A11  T0_CT_B01 
#&gt;      &quot;032&quot;      &quot;031&quot;      &quot;031&quot;      &quot;032&quot;      &quot;031&quot;      &quot;021&quot;      &quot;032&quot;      &quot;032&quot;      &quot;032&quot; 
#&gt;  T0_CT_B03  T0_CT_B05  T0_CT_B07  T0_CT_B08  T0_CT_B09  T0_CT_C02  T0_CT_C03  T0_CT_C05  T0_CT_C06 
#&gt;      &quot;031&quot;      &quot;032&quot;      &quot;032&quot;      &quot;031&quot;      &quot;031&quot;      &quot;031&quot;      &quot;021&quot;      &quot;011&quot;      &quot;011&quot; 
#&gt;  T0_CT_C07  T0_CT_C08  T0_CT_C09  T0_CT_C11  T0_CT_C12  T0_CT_D01  T0_CT_D02  T0_CT_D03  T0_CT_D05 
#&gt;      &quot;032&quot;      &quot;031&quot;      &quot;011&quot;      &quot;011&quot;      &quot;031&quot;      &quot;032&quot;      &quot;032&quot;      &quot;031&quot;      &quot;032&quot; 
#&gt;  T0_CT_D06  T0_CT_D07  T0_CT_D08  T0_CT_D09  T0_CT_D11  T0_CT_D12  T0_CT_E01  T0_CT_E03  T0_CT_E04 
#&gt;      &quot;031&quot;      &quot;032&quot;      &quot;031&quot;      &quot;011&quot;      &quot;011&quot;      &quot;031&quot;      &quot;031&quot;      &quot;032&quot;      &quot;011&quot; 
#&gt;  T0_CT_E05  T0_CT_E06  T0_CT_E07  T0_CT_E08  T0_CT_E09  T0_CT_E10  T0_CT_E11  T0_CT_E12  T0_CT_F01 
#&gt;      &quot;022&quot;      &quot;032&quot;      &quot;031&quot;      &quot;031&quot;      &quot;032&quot;      &quot;032&quot;      &quot;031&quot;      &quot;032&quot;      &quot;031&quot; 
#&gt;  T0_CT_F02  T0_CT_F03  T0_CT_F04  T0_CT_F05  T0_CT_F06  T0_CT_F07  T0_CT_F09  T0_CT_F11  T0_CT_F12 
#&gt;      &quot;031&quot;      &quot;031&quot;      &quot;031&quot;      &quot;032&quot;      &quot;031&quot;      &quot;032&quot;      &quot;011&quot;      &quot;032&quot;      &quot;031&quot; 
#&gt;  T0_CT_G01  T0_CT_G02  T0_CT_G03  T0_CT_G04  T0_CT_G07  T0_CT_G08  T0_CT_G09  T0_CT_G11  T0_CT_H01 
#&gt;      &quot;031&quot;      &quot;032&quot;      &quot;032&quot;      &quot;031&quot;      &quot;031&quot;      &quot;031&quot;      &quot;022&quot;      &quot;032&quot;      &quot;022&quot; 
#&gt;  T0_CT_H02  T0_CT_H04  T0_CT_H05  T0_CT_H08  T0_CT_H09  T0_CT_H12 T24_CT_A01 T24_CT_A03 T24_CT_A04 
#&gt;      &quot;032&quot;      &quot;032&quot;      &quot;032&quot;      &quot;032&quot;      &quot;032&quot;      &quot;032&quot;      &quot;021&quot;      &quot;011&quot;      &quot;011&quot; 
#&gt; T24_CT_A05 T24_CT_A07 T24_CT_A08 T24_CT_A09 T24_CT_A10 T24_CT_B01 T24_CT_B02 T24_CT_B03 T24_CT_B05 
#&gt;      &quot;031&quot;      &quot;011&quot;      &quot;021&quot;      &quot;021&quot;      &quot;022&quot;      &quot;022&quot;      &quot;021&quot;      &quot;012&quot;      &quot;011&quot; 
#&gt; T24_CT_B06 T24_CT_B07 T24_CT_B08 T24_CT_B09 T24_CT_B11 T24_CT_C01 T24_CT_C02 T24_CT_C03 T24_CT_C05 
#&gt;      &quot;011&quot;      &quot;021&quot;      &quot;011&quot;      &quot;021&quot;      &quot;031&quot;      &quot;021&quot;      &quot;011&quot;      &quot;021&quot;      &quot;011&quot; 
#&gt; T24_CT_C07 T24_CT_C08 T24_CT_C09 T24_CT_C10 T24_CT_C11 T24_CT_C12 T24_CT_D01 T24_CT_D02 T24_CT_D03 
#&gt;      &quot;011&quot;      &quot;011&quot;      &quot;011&quot;      &quot;021&quot;      &quot;022&quot;      &quot;011&quot;      &quot;011&quot;      &quot;011&quot;      &quot;011&quot; 
#&gt; T24_CT_D04 T24_CT_D05 T24_CT_D06 T24_CT_D07 T24_CT_D08 T24_CT_D09 T24_CT_D10 T24_CT_D11 T24_CT_E01 
#&gt;      &quot;021&quot;      &quot;022&quot;      &quot;021&quot;      &quot;022&quot;      &quot;011&quot;      &quot;011&quot;      &quot;011&quot;      &quot;011&quot;      &quot;022&quot; 
#&gt; T24_CT_E02 T24_CT_E04 T24_CT_E05 T24_CT_E07 T24_CT_E09 T24_CT_E11 T24_CT_E12 T24_CT_F01 T24_CT_F02 
#&gt;      &quot;021&quot;      &quot;012&quot;      &quot;021&quot;      &quot;012&quot;      &quot;021&quot;      &quot;021&quot;      &quot;011&quot;      &quot;011&quot;      &quot;011&quot; 
#&gt; T24_CT_F03 T24_CT_F04 T24_CT_F05 T24_CT_F07 T24_CT_F08 T24_CT_F09 T24_CT_F10 T24_CT_F11 T24_CT_F12 
#&gt;      &quot;022&quot;      &quot;022&quot;      &quot;022&quot;      &quot;022&quot;      &quot;021&quot;      &quot;011&quot;      &quot;022&quot;      &quot;022&quot;      &quot;031&quot; 
#&gt; T24_CT_G01 T24_CT_G02 T24_CT_G03 T24_CT_G04 T24_CT_G05 T24_CT_G06 T24_CT_G08 T24_CT_G10 T24_CT_G11 
#&gt;      &quot;011&quot;      &quot;021&quot;      &quot;021&quot;      &quot;011&quot;      &quot;011&quot;      &quot;021&quot;      &quot;021&quot;      &quot;011&quot;      &quot;021&quot; 
#&gt; T24_CT_G12 T24_CT_H01 T24_CT_H02 T24_CT_H03 T24_CT_H05 T24_CT_H07 T24_CT_H09 T24_CT_H12 T48_CT_A01 
#&gt;      &quot;021&quot;      &quot;031&quot;      &quot;021&quot;      &quot;011&quot;      &quot;022&quot;      &quot;022&quot;      &quot;021&quot;      &quot;021&quot;      &quot;012&quot; 
#&gt; T48_CT_A02 T48_CT_A03 T48_CT_A04 T48_CT_A05 T48_CT_A06 T48_CT_A07 T48_CT_A08 T48_CT_A09 T48_CT_A10 
#&gt;      &quot;012&quot;      &quot;021&quot;      &quot;011&quot;      &quot;012&quot;      &quot;021&quot;      &quot;031&quot;      &quot;012&quot;      &quot;022&quot;      &quot;011&quot; 
#&gt; T48_CT_A11 T48_CT_A12 T48_CT_B01 T48_CT_B02 T48_CT_B03 T48_CT_B04 T48_CT_B06 T48_CT_B08 T48_CT_B10 
#&gt;      &quot;011&quot;      &quot;022&quot;      &quot;011&quot;      &quot;011&quot;      &quot;021&quot;      &quot;021&quot;      &quot;011&quot;      &quot;021&quot;      &quot;011&quot; 
#&gt; T48_CT_B11 T48_CT_B12 T48_CT_C01 T48_CT_C02 T48_CT_C03 T48_CT_C04 T48_CT_C05 T48_CT_C06 T48_CT_C07 
#&gt;      &quot;012&quot;      &quot;022&quot;      &quot;011&quot;      &quot;021&quot;      &quot;011&quot;      &quot;021&quot;      &quot;012&quot;      &quot;012&quot;      &quot;022&quot; 
#&gt; T48_CT_C09 T48_CT_C10 T48_CT_C11 T48_CT_D01 T48_CT_D02 T48_CT_D03 T48_CT_D04 T48_CT_D06 T48_CT_D07 
#&gt;      &quot;012&quot;      &quot;022&quot;      &quot;012&quot;      &quot;011&quot;      &quot;022&quot;      &quot;021&quot;      &quot;011&quot;      &quot;012&quot;      &quot;011&quot; 
#&gt; T48_CT_D08 T48_CT_D09 T48_CT_D10 T48_CT_D11 T48_CT_D12 T48_CT_E01 T48_CT_E02 T48_CT_E03 T48_CT_E04 
#&gt;      &quot;021&quot;      &quot;011&quot;      &quot;022&quot;      &quot;012&quot;      &quot;021&quot;      &quot;012&quot;      &quot;022&quot;      &quot;012&quot;      &quot;022&quot; 
#&gt; T48_CT_E05 T48_CT_E06 T48_CT_E07 T48_CT_E08 T48_CT_E10 T48_CT_E11 T48_CT_E12 T48_CT_F01 T48_CT_F02 
#&gt;      &quot;011&quot;      &quot;021&quot;      &quot;021&quot;      &quot;011&quot;      &quot;021&quot;      &quot;022&quot;      &quot;031&quot;      &quot;011&quot;      &quot;022&quot; 
#&gt; T48_CT_F03 T48_CT_F05 T48_CT_F07 T48_CT_F09 T48_CT_F10 T48_CT_F11 T48_CT_G01 T48_CT_G02 T48_CT_G03 
#&gt;      &quot;022&quot;      &quot;011&quot;      &quot;022&quot;      &quot;011&quot;      &quot;022&quot;      &quot;021&quot;      &quot;021&quot;      &quot;012&quot;      &quot;011&quot; 
#&gt; T48_CT_G07 T48_CT_G08 T48_CT_G09 T48_CT_G10 T48_CT_G11 T48_CT_G12 T48_CT_H01 T48_CT_H02 T48_CT_H04 
#&gt;      &quot;011&quot;      &quot;012&quot;      &quot;012&quot;      &quot;031&quot;      &quot;022&quot;      &quot;011&quot;      &quot;011&quot;      &quot;022&quot;      &quot;021&quot; 
#&gt; T48_CT_H05 T48_CT_H06 T48_CT_H07 T48_CT_H08 T48_CT_H11 T48_CT_H12 T72_CT_A01 T72_CT_A05 T72_CT_A08 
#&gt;      &quot;011&quot;      &quot;011&quot;      &quot;022&quot;      &quot;012&quot;      &quot;022&quot;      &quot;012&quot;      &quot;012&quot;      &quot;012&quot;      &quot;023&quot; 
#&gt; T72_CT_A09 T72_CT_A11 T72_CT_B01 T72_CT_B02 T72_CT_B03 T72_CT_B04 T72_CT_B05 T72_CT_B06 T72_CT_B08 
#&gt;      &quot;012&quot;      &quot;012&quot;      &quot;012&quot;      &quot;023&quot;      &quot;023&quot;      &quot;023&quot;      &quot;023&quot;      &quot;023&quot;      &quot;031&quot; 
#&gt; T72_CT_B09 T72_CT_B11 T72_CT_B12 T72_CT_C04 T72_CT_C06 T72_CT_C07 T72_CT_C09 T72_CT_C11 T72_CT_D01 
#&gt;      &quot;021&quot;      &quot;023&quot;      &quot;012&quot;      &quot;012&quot;      &quot;023&quot;      &quot;012&quot;      &quot;023&quot;      &quot;031&quot;      &quot;012&quot; 
#&gt; T72_CT_D03 T72_CT_D04 T72_CT_D05 T72_CT_D07 T72_CT_D10 T72_CT_D11 T72_CT_E04 T72_CT_E05 T72_CT_E07 
#&gt;      &quot;023&quot;      &quot;012&quot;      &quot;023&quot;      &quot;023&quot;      &quot;023&quot;      &quot;023&quot;      &quot;023&quot;      &quot;012&quot;      &quot;012&quot; 
#&gt; T72_CT_F01 T72_CT_F05 T72_CT_F07 T72_CT_F10 T72_CT_F11 T72_CT_G03 T72_CT_G04 T72_CT_G06 T72_CT_G08 
#&gt;      &quot;012&quot;      &quot;012&quot;      &quot;023&quot;      &quot;012&quot;      &quot;012&quot;      &quot;023&quot;      &quot;023&quot;      &quot;012&quot;      &quot;032&quot; 
#&gt; T72_CT_G10 T72_CT_G11 T72_CT_H01 T72_CT_H03 T72_CT_H05 T72_CT_H08 T72_CT_H09 T72_CT_H10 T72_CT_H11 
#&gt;      &quot;012&quot;      &quot;023&quot;      &quot;012&quot;      &quot;012&quot;      &quot;031&quot;      &quot;031&quot;      &quot;031&quot;      &quot;023&quot;      &quot;023&quot; 
#&gt; T72_CT_H12 
#&gt;      &quot;012&quot;
</code></pre>

<script>
$('#tab-get-classes-from-hierarchical-partition-3-a').parent().next().next().hide();
$('#tab-get-classes-from-hierarchical-partition-3-a').click(function(){
  $('#tab-get-classes-from-hierarchical-partition-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-get-classes-from-hierarchical-partition-4'>
<p><a id='tab-get-classes-from-hierarchical-partition-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">get_classes(res_rh, merge_node = merge_node_param(min_n_signatures = 796))
</code></pre>

<pre><code>#&gt;  T0_CT_A01  T0_CT_A03  T0_CT_A05  T0_CT_A06  T0_CT_A07  T0_CT_A08  T0_CT_A10  T0_CT_A11  T0_CT_B01 
#&gt;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot;      &quot;021&quot;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot; 
#&gt;  T0_CT_B03  T0_CT_B05  T0_CT_B07  T0_CT_B08  T0_CT_B09  T0_CT_C02  T0_CT_C03  T0_CT_C05  T0_CT_C06 
#&gt;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot;      &quot;021&quot;      &quot;011&quot;      &quot;011&quot; 
#&gt;  T0_CT_C07  T0_CT_C08  T0_CT_C09  T0_CT_C11  T0_CT_C12  T0_CT_D01  T0_CT_D02  T0_CT_D03  T0_CT_D05 
#&gt;       &quot;03&quot;       &quot;03&quot;      &quot;011&quot;      &quot;011&quot;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot; 
#&gt;  T0_CT_D06  T0_CT_D07  T0_CT_D08  T0_CT_D09  T0_CT_D11  T0_CT_D12  T0_CT_E01  T0_CT_E03  T0_CT_E04 
#&gt;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot;      &quot;011&quot;      &quot;011&quot;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot;      &quot;011&quot; 
#&gt;  T0_CT_E05  T0_CT_E06  T0_CT_E07  T0_CT_E08  T0_CT_E09  T0_CT_E10  T0_CT_E11  T0_CT_E12  T0_CT_F01 
#&gt;      &quot;022&quot;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot; 
#&gt;  T0_CT_F02  T0_CT_F03  T0_CT_F04  T0_CT_F05  T0_CT_F06  T0_CT_F07  T0_CT_F09  T0_CT_F11  T0_CT_F12 
#&gt;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot;      &quot;011&quot;       &quot;03&quot;       &quot;03&quot; 
#&gt;  T0_CT_G01  T0_CT_G02  T0_CT_G03  T0_CT_G04  T0_CT_G07  T0_CT_G08  T0_CT_G09  T0_CT_G11  T0_CT_H01 
#&gt;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot;      &quot;022&quot;       &quot;03&quot;      &quot;022&quot; 
#&gt;  T0_CT_H02  T0_CT_H04  T0_CT_H05  T0_CT_H08  T0_CT_H09  T0_CT_H12 T24_CT_A01 T24_CT_A03 T24_CT_A04 
#&gt;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot;      &quot;021&quot;      &quot;011&quot;      &quot;011&quot; 
#&gt; T24_CT_A05 T24_CT_A07 T24_CT_A08 T24_CT_A09 T24_CT_A10 T24_CT_B01 T24_CT_B02 T24_CT_B03 T24_CT_B05 
#&gt;       &quot;03&quot;      &quot;011&quot;      &quot;021&quot;      &quot;021&quot;      &quot;022&quot;      &quot;022&quot;      &quot;021&quot;      &quot;012&quot;      &quot;011&quot; 
#&gt; T24_CT_B06 T24_CT_B07 T24_CT_B08 T24_CT_B09 T24_CT_B11 T24_CT_C01 T24_CT_C02 T24_CT_C03 T24_CT_C05 
#&gt;      &quot;011&quot;      &quot;021&quot;      &quot;011&quot;      &quot;021&quot;       &quot;03&quot;      &quot;021&quot;      &quot;011&quot;      &quot;021&quot;      &quot;011&quot; 
#&gt; T24_CT_C07 T24_CT_C08 T24_CT_C09 T24_CT_C10 T24_CT_C11 T24_CT_C12 T24_CT_D01 T24_CT_D02 T24_CT_D03 
#&gt;      &quot;011&quot;      &quot;011&quot;      &quot;011&quot;      &quot;021&quot;      &quot;022&quot;      &quot;011&quot;      &quot;011&quot;      &quot;011&quot;      &quot;011&quot; 
#&gt; T24_CT_D04 T24_CT_D05 T24_CT_D06 T24_CT_D07 T24_CT_D08 T24_CT_D09 T24_CT_D10 T24_CT_D11 T24_CT_E01 
#&gt;      &quot;021&quot;      &quot;022&quot;      &quot;021&quot;      &quot;022&quot;      &quot;011&quot;      &quot;011&quot;      &quot;011&quot;      &quot;011&quot;      &quot;022&quot; 
#&gt; T24_CT_E02 T24_CT_E04 T24_CT_E05 T24_CT_E07 T24_CT_E09 T24_CT_E11 T24_CT_E12 T24_CT_F01 T24_CT_F02 
#&gt;      &quot;021&quot;      &quot;012&quot;      &quot;021&quot;      &quot;012&quot;      &quot;021&quot;      &quot;021&quot;      &quot;011&quot;      &quot;011&quot;      &quot;011&quot; 
#&gt; T24_CT_F03 T24_CT_F04 T24_CT_F05 T24_CT_F07 T24_CT_F08 T24_CT_F09 T24_CT_F10 T24_CT_F11 T24_CT_F12 
#&gt;      &quot;022&quot;      &quot;022&quot;      &quot;022&quot;      &quot;022&quot;      &quot;021&quot;      &quot;011&quot;      &quot;022&quot;      &quot;022&quot;       &quot;03&quot; 
#&gt; T24_CT_G01 T24_CT_G02 T24_CT_G03 T24_CT_G04 T24_CT_G05 T24_CT_G06 T24_CT_G08 T24_CT_G10 T24_CT_G11 
#&gt;      &quot;011&quot;      &quot;021&quot;      &quot;021&quot;      &quot;011&quot;      &quot;011&quot;      &quot;021&quot;      &quot;021&quot;      &quot;011&quot;      &quot;021&quot; 
#&gt; T24_CT_G12 T24_CT_H01 T24_CT_H02 T24_CT_H03 T24_CT_H05 T24_CT_H07 T24_CT_H09 T24_CT_H12 T48_CT_A01 
#&gt;      &quot;021&quot;       &quot;03&quot;      &quot;021&quot;      &quot;011&quot;      &quot;022&quot;      &quot;022&quot;      &quot;021&quot;      &quot;021&quot;      &quot;012&quot; 
#&gt; T48_CT_A02 T48_CT_A03 T48_CT_A04 T48_CT_A05 T48_CT_A06 T48_CT_A07 T48_CT_A08 T48_CT_A09 T48_CT_A10 
#&gt;      &quot;012&quot;      &quot;021&quot;      &quot;011&quot;      &quot;012&quot;      &quot;021&quot;       &quot;03&quot;      &quot;012&quot;      &quot;022&quot;      &quot;011&quot; 
#&gt; T48_CT_A11 T48_CT_A12 T48_CT_B01 T48_CT_B02 T48_CT_B03 T48_CT_B04 T48_CT_B06 T48_CT_B08 T48_CT_B10 
#&gt;      &quot;011&quot;      &quot;022&quot;      &quot;011&quot;      &quot;011&quot;      &quot;021&quot;      &quot;021&quot;      &quot;011&quot;      &quot;021&quot;      &quot;011&quot; 
#&gt; T48_CT_B11 T48_CT_B12 T48_CT_C01 T48_CT_C02 T48_CT_C03 T48_CT_C04 T48_CT_C05 T48_CT_C06 T48_CT_C07 
#&gt;      &quot;012&quot;      &quot;022&quot;      &quot;011&quot;      &quot;021&quot;      &quot;011&quot;      &quot;021&quot;      &quot;012&quot;      &quot;012&quot;      &quot;022&quot; 
#&gt; T48_CT_C09 T48_CT_C10 T48_CT_C11 T48_CT_D01 T48_CT_D02 T48_CT_D03 T48_CT_D04 T48_CT_D06 T48_CT_D07 
#&gt;      &quot;012&quot;      &quot;022&quot;      &quot;012&quot;      &quot;011&quot;      &quot;022&quot;      &quot;021&quot;      &quot;011&quot;      &quot;012&quot;      &quot;011&quot; 
#&gt; T48_CT_D08 T48_CT_D09 T48_CT_D10 T48_CT_D11 T48_CT_D12 T48_CT_E01 T48_CT_E02 T48_CT_E03 T48_CT_E04 
#&gt;      &quot;021&quot;      &quot;011&quot;      &quot;022&quot;      &quot;012&quot;      &quot;021&quot;      &quot;012&quot;      &quot;022&quot;      &quot;012&quot;      &quot;022&quot; 
#&gt; T48_CT_E05 T48_CT_E06 T48_CT_E07 T48_CT_E08 T48_CT_E10 T48_CT_E11 T48_CT_E12 T48_CT_F01 T48_CT_F02 
#&gt;      &quot;011&quot;      &quot;021&quot;      &quot;021&quot;      &quot;011&quot;      &quot;021&quot;      &quot;022&quot;       &quot;03&quot;      &quot;011&quot;      &quot;022&quot; 
#&gt; T48_CT_F03 T48_CT_F05 T48_CT_F07 T48_CT_F09 T48_CT_F10 T48_CT_F11 T48_CT_G01 T48_CT_G02 T48_CT_G03 
#&gt;      &quot;022&quot;      &quot;011&quot;      &quot;022&quot;      &quot;011&quot;      &quot;022&quot;      &quot;021&quot;      &quot;021&quot;      &quot;012&quot;      &quot;011&quot; 
#&gt; T48_CT_G07 T48_CT_G08 T48_CT_G09 T48_CT_G10 T48_CT_G11 T48_CT_G12 T48_CT_H01 T48_CT_H02 T48_CT_H04 
#&gt;      &quot;011&quot;      &quot;012&quot;      &quot;012&quot;       &quot;03&quot;      &quot;022&quot;      &quot;011&quot;      &quot;011&quot;      &quot;022&quot;      &quot;021&quot; 
#&gt; T48_CT_H05 T48_CT_H06 T48_CT_H07 T48_CT_H08 T48_CT_H11 T48_CT_H12 T72_CT_A01 T72_CT_A05 T72_CT_A08 
#&gt;      &quot;011&quot;      &quot;011&quot;      &quot;022&quot;      &quot;012&quot;      &quot;022&quot;      &quot;012&quot;      &quot;012&quot;      &quot;012&quot;      &quot;023&quot; 
#&gt; T72_CT_A09 T72_CT_A11 T72_CT_B01 T72_CT_B02 T72_CT_B03 T72_CT_B04 T72_CT_B05 T72_CT_B06 T72_CT_B08 
#&gt;      &quot;012&quot;      &quot;012&quot;      &quot;012&quot;      &quot;023&quot;      &quot;023&quot;      &quot;023&quot;      &quot;023&quot;      &quot;023&quot;       &quot;03&quot; 
#&gt; T72_CT_B09 T72_CT_B11 T72_CT_B12 T72_CT_C04 T72_CT_C06 T72_CT_C07 T72_CT_C09 T72_CT_C11 T72_CT_D01 
#&gt;      &quot;021&quot;      &quot;023&quot;      &quot;012&quot;      &quot;012&quot;      &quot;023&quot;      &quot;012&quot;      &quot;023&quot;       &quot;03&quot;      &quot;012&quot; 
#&gt; T72_CT_D03 T72_CT_D04 T72_CT_D05 T72_CT_D07 T72_CT_D10 T72_CT_D11 T72_CT_E04 T72_CT_E05 T72_CT_E07 
#&gt;      &quot;023&quot;      &quot;012&quot;      &quot;023&quot;      &quot;023&quot;      &quot;023&quot;      &quot;023&quot;      &quot;023&quot;      &quot;012&quot;      &quot;012&quot; 
#&gt; T72_CT_F01 T72_CT_F05 T72_CT_F07 T72_CT_F10 T72_CT_F11 T72_CT_G03 T72_CT_G04 T72_CT_G06 T72_CT_G08 
#&gt;      &quot;012&quot;      &quot;012&quot;      &quot;023&quot;      &quot;012&quot;      &quot;012&quot;      &quot;023&quot;      &quot;023&quot;      &quot;012&quot;       &quot;03&quot; 
#&gt; T72_CT_G10 T72_CT_G11 T72_CT_H01 T72_CT_H03 T72_CT_H05 T72_CT_H08 T72_CT_H09 T72_CT_H10 T72_CT_H11 
#&gt;      &quot;012&quot;      &quot;023&quot;      &quot;012&quot;      &quot;012&quot;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot;      &quot;023&quot;      &quot;023&quot; 
#&gt; T72_CT_H12 
#&gt;      &quot;012&quot;
</code></pre>

<script>
$('#tab-get-classes-from-hierarchical-partition-4-a').parent().next().next().hide();
$('#tab-get-classes-from-hierarchical-partition-4-a').click(function(){
  $('#tab-get-classes-from-hierarchical-partition-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-get-classes-from-hierarchical-partition-5'>
<p><a id='tab-get-classes-from-hierarchical-partition-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">get_classes(res_rh, merge_node = merge_node_param(min_n_signatures = 1634))
</code></pre>

<pre><code>#&gt;  T0_CT_A01  T0_CT_A03  T0_CT_A05  T0_CT_A06  T0_CT_A07  T0_CT_A08  T0_CT_A10  T0_CT_A11  T0_CT_B01 
#&gt;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot;      &quot;021&quot;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot; 
#&gt;  T0_CT_B03  T0_CT_B05  T0_CT_B07  T0_CT_B08  T0_CT_B09  T0_CT_C02  T0_CT_C03  T0_CT_C05  T0_CT_C06 
#&gt;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot;      &quot;021&quot;       &quot;01&quot;       &quot;01&quot; 
#&gt;  T0_CT_C07  T0_CT_C08  T0_CT_C09  T0_CT_C11  T0_CT_C12  T0_CT_D01  T0_CT_D02  T0_CT_D03  T0_CT_D05 
#&gt;       &quot;03&quot;       &quot;03&quot;       &quot;01&quot;       &quot;01&quot;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot; 
#&gt;  T0_CT_D06  T0_CT_D07  T0_CT_D08  T0_CT_D09  T0_CT_D11  T0_CT_D12  T0_CT_E01  T0_CT_E03  T0_CT_E04 
#&gt;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot;       &quot;01&quot;       &quot;01&quot;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot;       &quot;01&quot; 
#&gt;  T0_CT_E05  T0_CT_E06  T0_CT_E07  T0_CT_E08  T0_CT_E09  T0_CT_E10  T0_CT_E11  T0_CT_E12  T0_CT_F01 
#&gt;      &quot;022&quot;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot; 
#&gt;  T0_CT_F02  T0_CT_F03  T0_CT_F04  T0_CT_F05  T0_CT_F06  T0_CT_F07  T0_CT_F09  T0_CT_F11  T0_CT_F12 
#&gt;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot;       &quot;01&quot;       &quot;03&quot;       &quot;03&quot; 
#&gt;  T0_CT_G01  T0_CT_G02  T0_CT_G03  T0_CT_G04  T0_CT_G07  T0_CT_G08  T0_CT_G09  T0_CT_G11  T0_CT_H01 
#&gt;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot;      &quot;022&quot;       &quot;03&quot;      &quot;022&quot; 
#&gt;  T0_CT_H02  T0_CT_H04  T0_CT_H05  T0_CT_H08  T0_CT_H09  T0_CT_H12 T24_CT_A01 T24_CT_A03 T24_CT_A04 
#&gt;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot;      &quot;021&quot;       &quot;01&quot;       &quot;01&quot; 
#&gt; T24_CT_A05 T24_CT_A07 T24_CT_A08 T24_CT_A09 T24_CT_A10 T24_CT_B01 T24_CT_B02 T24_CT_B03 T24_CT_B05 
#&gt;       &quot;03&quot;       &quot;01&quot;      &quot;021&quot;      &quot;021&quot;      &quot;022&quot;      &quot;022&quot;      &quot;021&quot;       &quot;01&quot;       &quot;01&quot; 
#&gt; T24_CT_B06 T24_CT_B07 T24_CT_B08 T24_CT_B09 T24_CT_B11 T24_CT_C01 T24_CT_C02 T24_CT_C03 T24_CT_C05 
#&gt;       &quot;01&quot;      &quot;021&quot;       &quot;01&quot;      &quot;021&quot;       &quot;03&quot;      &quot;021&quot;       &quot;01&quot;      &quot;021&quot;       &quot;01&quot; 
#&gt; T24_CT_C07 T24_CT_C08 T24_CT_C09 T24_CT_C10 T24_CT_C11 T24_CT_C12 T24_CT_D01 T24_CT_D02 T24_CT_D03 
#&gt;       &quot;01&quot;       &quot;01&quot;       &quot;01&quot;      &quot;021&quot;      &quot;022&quot;       &quot;01&quot;       &quot;01&quot;       &quot;01&quot;       &quot;01&quot; 
#&gt; T24_CT_D04 T24_CT_D05 T24_CT_D06 T24_CT_D07 T24_CT_D08 T24_CT_D09 T24_CT_D10 T24_CT_D11 T24_CT_E01 
#&gt;      &quot;021&quot;      &quot;022&quot;      &quot;021&quot;      &quot;022&quot;       &quot;01&quot;       &quot;01&quot;       &quot;01&quot;       &quot;01&quot;      &quot;022&quot; 
#&gt; T24_CT_E02 T24_CT_E04 T24_CT_E05 T24_CT_E07 T24_CT_E09 T24_CT_E11 T24_CT_E12 T24_CT_F01 T24_CT_F02 
#&gt;      &quot;021&quot;       &quot;01&quot;      &quot;021&quot;       &quot;01&quot;      &quot;021&quot;      &quot;021&quot;       &quot;01&quot;       &quot;01&quot;       &quot;01&quot; 
#&gt; T24_CT_F03 T24_CT_F04 T24_CT_F05 T24_CT_F07 T24_CT_F08 T24_CT_F09 T24_CT_F10 T24_CT_F11 T24_CT_F12 
#&gt;      &quot;022&quot;      &quot;022&quot;      &quot;022&quot;      &quot;022&quot;      &quot;021&quot;       &quot;01&quot;      &quot;022&quot;      &quot;022&quot;       &quot;03&quot; 
#&gt; T24_CT_G01 T24_CT_G02 T24_CT_G03 T24_CT_G04 T24_CT_G05 T24_CT_G06 T24_CT_G08 T24_CT_G10 T24_CT_G11 
#&gt;       &quot;01&quot;      &quot;021&quot;      &quot;021&quot;       &quot;01&quot;       &quot;01&quot;      &quot;021&quot;      &quot;021&quot;       &quot;01&quot;      &quot;021&quot; 
#&gt; T24_CT_G12 T24_CT_H01 T24_CT_H02 T24_CT_H03 T24_CT_H05 T24_CT_H07 T24_CT_H09 T24_CT_H12 T48_CT_A01 
#&gt;      &quot;021&quot;       &quot;03&quot;      &quot;021&quot;       &quot;01&quot;      &quot;022&quot;      &quot;022&quot;      &quot;021&quot;      &quot;021&quot;       &quot;01&quot; 
#&gt; T48_CT_A02 T48_CT_A03 T48_CT_A04 T48_CT_A05 T48_CT_A06 T48_CT_A07 T48_CT_A08 T48_CT_A09 T48_CT_A10 
#&gt;       &quot;01&quot;      &quot;021&quot;       &quot;01&quot;       &quot;01&quot;      &quot;021&quot;       &quot;03&quot;       &quot;01&quot;      &quot;022&quot;       &quot;01&quot; 
#&gt; T48_CT_A11 T48_CT_A12 T48_CT_B01 T48_CT_B02 T48_CT_B03 T48_CT_B04 T48_CT_B06 T48_CT_B08 T48_CT_B10 
#&gt;       &quot;01&quot;      &quot;022&quot;       &quot;01&quot;       &quot;01&quot;      &quot;021&quot;      &quot;021&quot;       &quot;01&quot;      &quot;021&quot;       &quot;01&quot; 
#&gt; T48_CT_B11 T48_CT_B12 T48_CT_C01 T48_CT_C02 T48_CT_C03 T48_CT_C04 T48_CT_C05 T48_CT_C06 T48_CT_C07 
#&gt;       &quot;01&quot;      &quot;022&quot;       &quot;01&quot;      &quot;021&quot;       &quot;01&quot;      &quot;021&quot;       &quot;01&quot;       &quot;01&quot;      &quot;022&quot; 
#&gt; T48_CT_C09 T48_CT_C10 T48_CT_C11 T48_CT_D01 T48_CT_D02 T48_CT_D03 T48_CT_D04 T48_CT_D06 T48_CT_D07 
#&gt;       &quot;01&quot;      &quot;022&quot;       &quot;01&quot;       &quot;01&quot;      &quot;022&quot;      &quot;021&quot;       &quot;01&quot;       &quot;01&quot;       &quot;01&quot; 
#&gt; T48_CT_D08 T48_CT_D09 T48_CT_D10 T48_CT_D11 T48_CT_D12 T48_CT_E01 T48_CT_E02 T48_CT_E03 T48_CT_E04 
#&gt;      &quot;021&quot;       &quot;01&quot;      &quot;022&quot;       &quot;01&quot;      &quot;021&quot;       &quot;01&quot;      &quot;022&quot;       &quot;01&quot;      &quot;022&quot; 
#&gt; T48_CT_E05 T48_CT_E06 T48_CT_E07 T48_CT_E08 T48_CT_E10 T48_CT_E11 T48_CT_E12 T48_CT_F01 T48_CT_F02 
#&gt;       &quot;01&quot;      &quot;021&quot;      &quot;021&quot;       &quot;01&quot;      &quot;021&quot;      &quot;022&quot;       &quot;03&quot;       &quot;01&quot;      &quot;022&quot; 
#&gt; T48_CT_F03 T48_CT_F05 T48_CT_F07 T48_CT_F09 T48_CT_F10 T48_CT_F11 T48_CT_G01 T48_CT_G02 T48_CT_G03 
#&gt;      &quot;022&quot;       &quot;01&quot;      &quot;022&quot;       &quot;01&quot;      &quot;022&quot;      &quot;021&quot;      &quot;021&quot;       &quot;01&quot;       &quot;01&quot; 
#&gt; T48_CT_G07 T48_CT_G08 T48_CT_G09 T48_CT_G10 T48_CT_G11 T48_CT_G12 T48_CT_H01 T48_CT_H02 T48_CT_H04 
#&gt;       &quot;01&quot;       &quot;01&quot;       &quot;01&quot;       &quot;03&quot;      &quot;022&quot;       &quot;01&quot;       &quot;01&quot;      &quot;022&quot;      &quot;021&quot; 
#&gt; T48_CT_H05 T48_CT_H06 T48_CT_H07 T48_CT_H08 T48_CT_H11 T48_CT_H12 T72_CT_A01 T72_CT_A05 T72_CT_A08 
#&gt;       &quot;01&quot;       &quot;01&quot;      &quot;022&quot;       &quot;01&quot;      &quot;022&quot;       &quot;01&quot;       &quot;01&quot;       &quot;01&quot;      &quot;023&quot; 
#&gt; T72_CT_A09 T72_CT_A11 T72_CT_B01 T72_CT_B02 T72_CT_B03 T72_CT_B04 T72_CT_B05 T72_CT_B06 T72_CT_B08 
#&gt;       &quot;01&quot;       &quot;01&quot;       &quot;01&quot;      &quot;023&quot;      &quot;023&quot;      &quot;023&quot;      &quot;023&quot;      &quot;023&quot;       &quot;03&quot; 
#&gt; T72_CT_B09 T72_CT_B11 T72_CT_B12 T72_CT_C04 T72_CT_C06 T72_CT_C07 T72_CT_C09 T72_CT_C11 T72_CT_D01 
#&gt;      &quot;021&quot;      &quot;023&quot;       &quot;01&quot;       &quot;01&quot;      &quot;023&quot;       &quot;01&quot;      &quot;023&quot;       &quot;03&quot;       &quot;01&quot; 
#&gt; T72_CT_D03 T72_CT_D04 T72_CT_D05 T72_CT_D07 T72_CT_D10 T72_CT_D11 T72_CT_E04 T72_CT_E05 T72_CT_E07 
#&gt;      &quot;023&quot;       &quot;01&quot;      &quot;023&quot;      &quot;023&quot;      &quot;023&quot;      &quot;023&quot;      &quot;023&quot;       &quot;01&quot;       &quot;01&quot; 
#&gt; T72_CT_F01 T72_CT_F05 T72_CT_F07 T72_CT_F10 T72_CT_F11 T72_CT_G03 T72_CT_G04 T72_CT_G06 T72_CT_G08 
#&gt;       &quot;01&quot;       &quot;01&quot;      &quot;023&quot;       &quot;01&quot;       &quot;01&quot;      &quot;023&quot;      &quot;023&quot;       &quot;01&quot;       &quot;03&quot; 
#&gt; T72_CT_G10 T72_CT_G11 T72_CT_H01 T72_CT_H03 T72_CT_H05 T72_CT_H08 T72_CT_H09 T72_CT_H10 T72_CT_H11 
#&gt;       &quot;01&quot;      &quot;023&quot;       &quot;01&quot;       &quot;01&quot;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot;      &quot;023&quot;      &quot;023&quot; 
#&gt; T72_CT_H12 
#&gt;       &quot;01&quot;
</code></pre>

<script>
$('#tab-get-classes-from-hierarchical-partition-5-a').parent().next().next().hide();
$('#tab-get-classes-from-hierarchical-partition-5-a').click(function(){
  $('#tab-get-classes-from-hierarchical-partition-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-get-classes-from-hierarchical-partition-6'>
<p><a id='tab-get-classes-from-hierarchical-partition-6-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">get_classes(res_rh, merge_node = merge_node_param(min_n_signatures = 3037))
</code></pre>

<pre><code>#&gt;  T0_CT_A01  T0_CT_A03  T0_CT_A05  T0_CT_A06  T0_CT_A07  T0_CT_A08  T0_CT_A10  T0_CT_A11  T0_CT_B01 
#&gt;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot;       &quot;02&quot;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot; 
#&gt;  T0_CT_B03  T0_CT_B05  T0_CT_B07  T0_CT_B08  T0_CT_B09  T0_CT_C02  T0_CT_C03  T0_CT_C05  T0_CT_C06 
#&gt;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot;       &quot;02&quot;       &quot;01&quot;       &quot;01&quot; 
#&gt;  T0_CT_C07  T0_CT_C08  T0_CT_C09  T0_CT_C11  T0_CT_C12  T0_CT_D01  T0_CT_D02  T0_CT_D03  T0_CT_D05 
#&gt;       &quot;03&quot;       &quot;03&quot;       &quot;01&quot;       &quot;01&quot;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot; 
#&gt;  T0_CT_D06  T0_CT_D07  T0_CT_D08  T0_CT_D09  T0_CT_D11  T0_CT_D12  T0_CT_E01  T0_CT_E03  T0_CT_E04 
#&gt;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot;       &quot;01&quot;       &quot;01&quot;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot;       &quot;01&quot; 
#&gt;  T0_CT_E05  T0_CT_E06  T0_CT_E07  T0_CT_E08  T0_CT_E09  T0_CT_E10  T0_CT_E11  T0_CT_E12  T0_CT_F01 
#&gt;       &quot;02&quot;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot; 
#&gt;  T0_CT_F02  T0_CT_F03  T0_CT_F04  T0_CT_F05  T0_CT_F06  T0_CT_F07  T0_CT_F09  T0_CT_F11  T0_CT_F12 
#&gt;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot;       &quot;01&quot;       &quot;03&quot;       &quot;03&quot; 
#&gt;  T0_CT_G01  T0_CT_G02  T0_CT_G03  T0_CT_G04  T0_CT_G07  T0_CT_G08  T0_CT_G09  T0_CT_G11  T0_CT_H01 
#&gt;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot;       &quot;02&quot;       &quot;03&quot;       &quot;02&quot; 
#&gt;  T0_CT_H02  T0_CT_H04  T0_CT_H05  T0_CT_H08  T0_CT_H09  T0_CT_H12 T24_CT_A01 T24_CT_A03 T24_CT_A04 
#&gt;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot;       &quot;02&quot;       &quot;01&quot;       &quot;01&quot; 
#&gt; T24_CT_A05 T24_CT_A07 T24_CT_A08 T24_CT_A09 T24_CT_A10 T24_CT_B01 T24_CT_B02 T24_CT_B03 T24_CT_B05 
#&gt;       &quot;03&quot;       &quot;01&quot;       &quot;02&quot;       &quot;02&quot;       &quot;02&quot;       &quot;02&quot;       &quot;02&quot;       &quot;01&quot;       &quot;01&quot; 
#&gt; T24_CT_B06 T24_CT_B07 T24_CT_B08 T24_CT_B09 T24_CT_B11 T24_CT_C01 T24_CT_C02 T24_CT_C03 T24_CT_C05 
#&gt;       &quot;01&quot;       &quot;02&quot;       &quot;01&quot;       &quot;02&quot;       &quot;03&quot;       &quot;02&quot;       &quot;01&quot;       &quot;02&quot;       &quot;01&quot; 
#&gt; T24_CT_C07 T24_CT_C08 T24_CT_C09 T24_CT_C10 T24_CT_C11 T24_CT_C12 T24_CT_D01 T24_CT_D02 T24_CT_D03 
#&gt;       &quot;01&quot;       &quot;01&quot;       &quot;01&quot;       &quot;02&quot;       &quot;02&quot;       &quot;01&quot;       &quot;01&quot;       &quot;01&quot;       &quot;01&quot; 
#&gt; T24_CT_D04 T24_CT_D05 T24_CT_D06 T24_CT_D07 T24_CT_D08 T24_CT_D09 T24_CT_D10 T24_CT_D11 T24_CT_E01 
#&gt;       &quot;02&quot;       &quot;02&quot;       &quot;02&quot;       &quot;02&quot;       &quot;01&quot;       &quot;01&quot;       &quot;01&quot;       &quot;01&quot;       &quot;02&quot; 
#&gt; T24_CT_E02 T24_CT_E04 T24_CT_E05 T24_CT_E07 T24_CT_E09 T24_CT_E11 T24_CT_E12 T24_CT_F01 T24_CT_F02 
#&gt;       &quot;02&quot;       &quot;01&quot;       &quot;02&quot;       &quot;01&quot;       &quot;02&quot;       &quot;02&quot;       &quot;01&quot;       &quot;01&quot;       &quot;01&quot; 
#&gt; T24_CT_F03 T24_CT_F04 T24_CT_F05 T24_CT_F07 T24_CT_F08 T24_CT_F09 T24_CT_F10 T24_CT_F11 T24_CT_F12 
#&gt;       &quot;02&quot;       &quot;02&quot;       &quot;02&quot;       &quot;02&quot;       &quot;02&quot;       &quot;01&quot;       &quot;02&quot;       &quot;02&quot;       &quot;03&quot; 
#&gt; T24_CT_G01 T24_CT_G02 T24_CT_G03 T24_CT_G04 T24_CT_G05 T24_CT_G06 T24_CT_G08 T24_CT_G10 T24_CT_G11 
#&gt;       &quot;01&quot;       &quot;02&quot;       &quot;02&quot;       &quot;01&quot;       &quot;01&quot;       &quot;02&quot;       &quot;02&quot;       &quot;01&quot;       &quot;02&quot; 
#&gt; T24_CT_G12 T24_CT_H01 T24_CT_H02 T24_CT_H03 T24_CT_H05 T24_CT_H07 T24_CT_H09 T24_CT_H12 T48_CT_A01 
#&gt;       &quot;02&quot;       &quot;03&quot;       &quot;02&quot;       &quot;01&quot;       &quot;02&quot;       &quot;02&quot;       &quot;02&quot;       &quot;02&quot;       &quot;01&quot; 
#&gt; T48_CT_A02 T48_CT_A03 T48_CT_A04 T48_CT_A05 T48_CT_A06 T48_CT_A07 T48_CT_A08 T48_CT_A09 T48_CT_A10 
#&gt;       &quot;01&quot;       &quot;02&quot;       &quot;01&quot;       &quot;01&quot;       &quot;02&quot;       &quot;03&quot;       &quot;01&quot;       &quot;02&quot;       &quot;01&quot; 
#&gt; T48_CT_A11 T48_CT_A12 T48_CT_B01 T48_CT_B02 T48_CT_B03 T48_CT_B04 T48_CT_B06 T48_CT_B08 T48_CT_B10 
#&gt;       &quot;01&quot;       &quot;02&quot;       &quot;01&quot;       &quot;01&quot;       &quot;02&quot;       &quot;02&quot;       &quot;01&quot;       &quot;02&quot;       &quot;01&quot; 
#&gt; T48_CT_B11 T48_CT_B12 T48_CT_C01 T48_CT_C02 T48_CT_C03 T48_CT_C04 T48_CT_C05 T48_CT_C06 T48_CT_C07 
#&gt;       &quot;01&quot;       &quot;02&quot;       &quot;01&quot;       &quot;02&quot;       &quot;01&quot;       &quot;02&quot;       &quot;01&quot;       &quot;01&quot;       &quot;02&quot; 
#&gt; T48_CT_C09 T48_CT_C10 T48_CT_C11 T48_CT_D01 T48_CT_D02 T48_CT_D03 T48_CT_D04 T48_CT_D06 T48_CT_D07 
#&gt;       &quot;01&quot;       &quot;02&quot;       &quot;01&quot;       &quot;01&quot;       &quot;02&quot;       &quot;02&quot;       &quot;01&quot;       &quot;01&quot;       &quot;01&quot; 
#&gt; T48_CT_D08 T48_CT_D09 T48_CT_D10 T48_CT_D11 T48_CT_D12 T48_CT_E01 T48_CT_E02 T48_CT_E03 T48_CT_E04 
#&gt;       &quot;02&quot;       &quot;01&quot;       &quot;02&quot;       &quot;01&quot;       &quot;02&quot;       &quot;01&quot;       &quot;02&quot;       &quot;01&quot;       &quot;02&quot; 
#&gt; T48_CT_E05 T48_CT_E06 T48_CT_E07 T48_CT_E08 T48_CT_E10 T48_CT_E11 T48_CT_E12 T48_CT_F01 T48_CT_F02 
#&gt;       &quot;01&quot;       &quot;02&quot;       &quot;02&quot;       &quot;01&quot;       &quot;02&quot;       &quot;02&quot;       &quot;03&quot;       &quot;01&quot;       &quot;02&quot; 
#&gt; T48_CT_F03 T48_CT_F05 T48_CT_F07 T48_CT_F09 T48_CT_F10 T48_CT_F11 T48_CT_G01 T48_CT_G02 T48_CT_G03 
#&gt;       &quot;02&quot;       &quot;01&quot;       &quot;02&quot;       &quot;01&quot;       &quot;02&quot;       &quot;02&quot;       &quot;02&quot;       &quot;01&quot;       &quot;01&quot; 
#&gt; T48_CT_G07 T48_CT_G08 T48_CT_G09 T48_CT_G10 T48_CT_G11 T48_CT_G12 T48_CT_H01 T48_CT_H02 T48_CT_H04 
#&gt;       &quot;01&quot;       &quot;01&quot;       &quot;01&quot;       &quot;03&quot;       &quot;02&quot;       &quot;01&quot;       &quot;01&quot;       &quot;02&quot;       &quot;02&quot; 
#&gt; T48_CT_H05 T48_CT_H06 T48_CT_H07 T48_CT_H08 T48_CT_H11 T48_CT_H12 T72_CT_A01 T72_CT_A05 T72_CT_A08 
#&gt;       &quot;01&quot;       &quot;01&quot;       &quot;02&quot;       &quot;01&quot;       &quot;02&quot;       &quot;01&quot;       &quot;01&quot;       &quot;01&quot;       &quot;02&quot; 
#&gt; T72_CT_A09 T72_CT_A11 T72_CT_B01 T72_CT_B02 T72_CT_B03 T72_CT_B04 T72_CT_B05 T72_CT_B06 T72_CT_B08 
#&gt;       &quot;01&quot;       &quot;01&quot;       &quot;01&quot;       &quot;02&quot;       &quot;02&quot;       &quot;02&quot;       &quot;02&quot;       &quot;02&quot;       &quot;03&quot; 
#&gt; T72_CT_B09 T72_CT_B11 T72_CT_B12 T72_CT_C04 T72_CT_C06 T72_CT_C07 T72_CT_C09 T72_CT_C11 T72_CT_D01 
#&gt;       &quot;02&quot;       &quot;02&quot;       &quot;01&quot;       &quot;01&quot;       &quot;02&quot;       &quot;01&quot;       &quot;02&quot;       &quot;03&quot;       &quot;01&quot; 
#&gt; T72_CT_D03 T72_CT_D04 T72_CT_D05 T72_CT_D07 T72_CT_D10 T72_CT_D11 T72_CT_E04 T72_CT_E05 T72_CT_E07 
#&gt;       &quot;02&quot;       &quot;01&quot;       &quot;02&quot;       &quot;02&quot;       &quot;02&quot;       &quot;02&quot;       &quot;02&quot;       &quot;01&quot;       &quot;01&quot; 
#&gt; T72_CT_F01 T72_CT_F05 T72_CT_F07 T72_CT_F10 T72_CT_F11 T72_CT_G03 T72_CT_G04 T72_CT_G06 T72_CT_G08 
#&gt;       &quot;01&quot;       &quot;01&quot;       &quot;02&quot;       &quot;01&quot;       &quot;01&quot;       &quot;02&quot;       &quot;02&quot;       &quot;01&quot;       &quot;03&quot; 
#&gt; T72_CT_G10 T72_CT_G11 T72_CT_H01 T72_CT_H03 T72_CT_H05 T72_CT_H08 T72_CT_H09 T72_CT_H10 T72_CT_H11 
#&gt;       &quot;01&quot;       &quot;02&quot;       &quot;01&quot;       &quot;01&quot;       &quot;03&quot;       &quot;03&quot;       &quot;03&quot;       &quot;02&quot;       &quot;02&quot; 
#&gt; T72_CT_H12 
#&gt;       &quot;01&quot;
</code></pre>

<script>
$('#tab-get-classes-from-hierarchical-partition-6-a').parent().next().next().hide();
$('#tab-get-classes-from-hierarchical-partition-6-a').click(function(){
  $('#tab-get-classes-from-hierarchical-partition-6-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>



### Top rows heatmap

Heatmaps of the top rows:





```r
top_rows_heatmap(res_rh)
```

![plot of chunk top-rows-heatmap](figure_cola/top-rows-heatmap-1.png)

Top rows on each node:


```r
top_rows_overlap(res_rh, method = "upset")
```

![plot of chunk top-rows-overlap](figure_cola/top-rows-overlap-1.png)


### UMAP plot

UMAP plot which shows how samples are separated.




<script>
$( function() {
	$( '#tabs-dimension-reduction-by-depth' ).tabs();
} );
</script>
<div id='tabs-dimension-reduction-by-depth'>
<ul>
<li><a href='#tab-dimension-reduction-by-depth-1'>n_signatures ≥ 422</a></li>
<li><a href='#tab-dimension-reduction-by-depth-2'>n_signatures ≥ 548</a></li>
<li><a href='#tab-dimension-reduction-by-depth-3'>n_signatures ≥ 606</a></li>
<li><a href='#tab-dimension-reduction-by-depth-4'>n_signatures ≥ 796</a></li>
<li><a href='#tab-dimension-reduction-by-depth-5'>n_signatures ≥ 1634</a></li>
<li><a href='#tab-dimension-reduction-by-depth-6'>n_signatures ≥ 3037</a></li>
</ul>
<div id='tab-dimension-reduction-by-depth-1'>
<pre><code class="r">par(mfrow = c(1, 2))
dimension_reduction(res_rh, merge_node = merge_node_param(min_n_signatures = 422),
    method = &quot;UMAP&quot;, top_value_method = &quot;SD&quot;, top_n = 1400, scale_rows = FALSE)
dimension_reduction(res_rh, merge_node = merge_node_param(min_n_signatures = 422),
    method = &quot;UMAP&quot;, top_value_method = &quot;ATC&quot;, top_n = 1400, scale_rows = TRUE)
</code></pre>

<p><img src="figure_cola/tab-dimension-reduction-by-depth-1-1.png" title="plot of chunk tab-dimension-reduction-by-depth-1" alt="plot of chunk tab-dimension-reduction-by-depth-1" width="100%" /></p>

</div>
<div id='tab-dimension-reduction-by-depth-2'>
<pre><code class="r">par(mfrow = c(1, 2))
dimension_reduction(res_rh, merge_node = merge_node_param(min_n_signatures = 548),
    method = &quot;UMAP&quot;, top_value_method = &quot;SD&quot;, top_n = 1400, scale_rows = FALSE)
dimension_reduction(res_rh, merge_node = merge_node_param(min_n_signatures = 548),
    method = &quot;UMAP&quot;, top_value_method = &quot;ATC&quot;, top_n = 1400, scale_rows = TRUE)
</code></pre>

<p><img src="figure_cola/tab-dimension-reduction-by-depth-2-1.png" title="plot of chunk tab-dimension-reduction-by-depth-2" alt="plot of chunk tab-dimension-reduction-by-depth-2" width="100%" /></p>

</div>
<div id='tab-dimension-reduction-by-depth-3'>
<pre><code class="r">par(mfrow = c(1, 2))
dimension_reduction(res_rh, merge_node = merge_node_param(min_n_signatures = 606),
    method = &quot;UMAP&quot;, top_value_method = &quot;SD&quot;, top_n = 1400, scale_rows = FALSE)
dimension_reduction(res_rh, merge_node = merge_node_param(min_n_signatures = 606),
    method = &quot;UMAP&quot;, top_value_method = &quot;ATC&quot;, top_n = 1400, scale_rows = TRUE)
</code></pre>

<p><img src="figure_cola/tab-dimension-reduction-by-depth-3-1.png" title="plot of chunk tab-dimension-reduction-by-depth-3" alt="plot of chunk tab-dimension-reduction-by-depth-3" width="100%" /></p>

</div>
<div id='tab-dimension-reduction-by-depth-4'>
<pre><code class="r">par(mfrow = c(1, 2))
dimension_reduction(res_rh, merge_node = merge_node_param(min_n_signatures = 796),
    method = &quot;UMAP&quot;, top_value_method = &quot;SD&quot;, top_n = 1400, scale_rows = FALSE)
dimension_reduction(res_rh, merge_node = merge_node_param(min_n_signatures = 796),
    method = &quot;UMAP&quot;, top_value_method = &quot;ATC&quot;, top_n = 1400, scale_rows = TRUE)
</code></pre>

<p><img src="figure_cola/tab-dimension-reduction-by-depth-4-1.png" title="plot of chunk tab-dimension-reduction-by-depth-4" alt="plot of chunk tab-dimension-reduction-by-depth-4" width="100%" /></p>

</div>
<div id='tab-dimension-reduction-by-depth-5'>
<pre><code class="r">par(mfrow = c(1, 2))
dimension_reduction(res_rh, merge_node = merge_node_param(min_n_signatures = 1634),
    method = &quot;UMAP&quot;, top_value_method = &quot;SD&quot;, top_n = 1400, scale_rows = FALSE)
dimension_reduction(res_rh, merge_node = merge_node_param(min_n_signatures = 1634),
    method = &quot;UMAP&quot;, top_value_method = &quot;ATC&quot;, top_n = 1400, scale_rows = TRUE)
</code></pre>

<p><img src="figure_cola/tab-dimension-reduction-by-depth-5-1.png" title="plot of chunk tab-dimension-reduction-by-depth-5" alt="plot of chunk tab-dimension-reduction-by-depth-5" width="100%" /></p>

</div>
<div id='tab-dimension-reduction-by-depth-6'>
<pre><code class="r">par(mfrow = c(1, 2))
dimension_reduction(res_rh, merge_node = merge_node_param(min_n_signatures = 3037),
    method = &quot;UMAP&quot;, top_value_method = &quot;SD&quot;, top_n = 1400, scale_rows = FALSE)
dimension_reduction(res_rh, merge_node = merge_node_param(min_n_signatures = 3037),
    method = &quot;UMAP&quot;, top_value_method = &quot;ATC&quot;, top_n = 1400, scale_rows = TRUE)
</code></pre>

<p><img src="figure_cola/tab-dimension-reduction-by-depth-6-1.png" title="plot of chunk tab-dimension-reduction-by-depth-6" alt="plot of chunk tab-dimension-reduction-by-depth-6" width="100%" /></p>

</div>
</div>




### Signature heatmap

Signatures on the heatmap are the union of all signatures found on every node
on the hierarchy. The number of k-means on rows are automatically selected by the function.




<script>
$( function() {
	$( '#tabs-get-signatures-from-hierarchical-partition' ).tabs();
} );
</script>
<div id='tabs-get-signatures-from-hierarchical-partition'>
<ul>
<li><a href='#tab-get-signatures-from-hierarchical-partition-1'>n_signatures ≥ 422</a></li>
<li><a href='#tab-get-signatures-from-hierarchical-partition-2'>n_signatures ≥ 548</a></li>
<li><a href='#tab-get-signatures-from-hierarchical-partition-3'>n_signatures ≥ 606</a></li>
<li><a href='#tab-get-signatures-from-hierarchical-partition-4'>n_signatures ≥ 796</a></li>
<li><a href='#tab-get-signatures-from-hierarchical-partition-5'>n_signatures ≥ 1634</a></li>
<li><a href='#tab-get-signatures-from-hierarchical-partition-6'>n_signatures ≥ 3037</a></li>
</ul>
<div id='tab-get-signatures-from-hierarchical-partition-1'>
<pre><code class="r">get_signatures(res_rh, merge_node = merge_node_param(min_n_signatures = 422))
</code></pre>

<p><img src="figure_cola/tab-get-signatures-from-hierarchical-partition-1-1.png" alt="plot of chunk tab-get-signatures-from-hierarchical-partition-1"/></p>

</div>
<div id='tab-get-signatures-from-hierarchical-partition-2'>
<pre><code class="r">get_signatures(res_rh, merge_node = merge_node_param(min_n_signatures = 548))
</code></pre>

<p><img src="figure_cola/tab-get-signatures-from-hierarchical-partition-2-1.png" alt="plot of chunk tab-get-signatures-from-hierarchical-partition-2"/></p>

</div>
<div id='tab-get-signatures-from-hierarchical-partition-3'>
<pre><code class="r">get_signatures(res_rh, merge_node = merge_node_param(min_n_signatures = 606))
</code></pre>

<p><img src="figure_cola/tab-get-signatures-from-hierarchical-partition-3-1.png" alt="plot of chunk tab-get-signatures-from-hierarchical-partition-3"/></p>

</div>
<div id='tab-get-signatures-from-hierarchical-partition-4'>
<pre><code class="r">get_signatures(res_rh, merge_node = merge_node_param(min_n_signatures = 796))
</code></pre>

<p><img src="figure_cola/tab-get-signatures-from-hierarchical-partition-4-1.png" alt="plot of chunk tab-get-signatures-from-hierarchical-partition-4"/></p>

</div>
<div id='tab-get-signatures-from-hierarchical-partition-5'>
<pre><code class="r">get_signatures(res_rh, merge_node = merge_node_param(min_n_signatures = 1634))
</code></pre>

<p><img src="figure_cola/tab-get-signatures-from-hierarchical-partition-5-1.png" alt="plot of chunk tab-get-signatures-from-hierarchical-partition-5"/></p>

</div>
<div id='tab-get-signatures-from-hierarchical-partition-6'>
<pre><code class="r">get_signatures(res_rh, merge_node = merge_node_param(min_n_signatures = 3037))
</code></pre>

<p><img src="figure_cola/tab-get-signatures-from-hierarchical-partition-6-1.png" alt="plot of chunk tab-get-signatures-from-hierarchical-partition-6"/></p>

</div>
</div>




Compare signatures from different nodes:


```r
compare_signatures(res_rh, verbose = FALSE)
```

![plot of chunk unnamed-chunk-24](figure_cola/unnamed-chunk-24-1.png)

If there are too many signatures, `top_signatures = ...` can be set to only show the 
signatures with the highest FDRs. Note it only works on every node and the final signatures
are the union of all signatures of all nodes.


```r
# code only for demonstration
# e.g. to show the top 500 most significant rows on each node.
tb = get_signature(res_rh, top_signatures = 500)
```


### Test to known annotations

Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.




<script>
$( function() {
	$( '#tabs-test-to-known-factors-from-hierarchical-partition' ).tabs();
} );
</script>
<div id='tabs-test-to-known-factors-from-hierarchical-partition'>
<ul>
<li><a href='#tab-test-to-known-factors-from-hierarchical-partition-1'>n_signatures ≥ 422</a></li>
<li><a href='#tab-test-to-known-factors-from-hierarchical-partition-2'>n_signatures ≥ 548</a></li>
<li><a href='#tab-test-to-known-factors-from-hierarchical-partition-3'>n_signatures ≥ 606</a></li>
<li><a href='#tab-test-to-known-factors-from-hierarchical-partition-4'>n_signatures ≥ 796</a></li>
<li><a href='#tab-test-to-known-factors-from-hierarchical-partition-5'>n_signatures ≥ 1634</a></li>
<li><a href='#tab-test-to-known-factors-from-hierarchical-partition-6'>n_signatures ≥ 3037</a></li>
</ul>
<div id='tab-test-to-known-factors-from-hierarchical-partition-1'>
<pre><code class="r">test_to_known_factors(res_rh, merge_node = merge_node_param(min_n_signatures = 422))
</code></pre>

<pre><code>#&gt;         Hours    Media    State
#&gt; class 2.4e-75 4.58e-35 2.27e-55
</code></pre>

</div>
<div id='tab-test-to-known-factors-from-hierarchical-partition-2'>
<pre><code class="r">test_to_known_factors(res_rh, merge_node = merge_node_param(min_n_signatures = 548))
</code></pre>

<pre><code>#&gt;          Hours    Media    State
#&gt; class 5.51e-63 1.93e-36 2.79e-53
</code></pre>

</div>
<div id='tab-test-to-known-factors-from-hierarchical-partition-3'>
<pre><code class="r">test_to_known_factors(res_rh, merge_node = merge_node_param(min_n_signatures = 606))
</code></pre>

<pre><code>#&gt;          Hours    Media    State
#&gt; class 2.24e-59 3.48e-32 5.31e-56
</code></pre>

</div>
<div id='tab-test-to-known-factors-from-hierarchical-partition-4'>
<pre><code class="r">test_to_known_factors(res_rh, merge_node = merge_node_param(min_n_signatures = 796))
</code></pre>

<pre><code>#&gt;          Hours    Media    State
#&gt; class 6.05e-60 1.57e-31 1.87e-57
</code></pre>

</div>
<div id='tab-test-to-known-factors-from-hierarchical-partition-5'>
<pre><code class="r">test_to_known_factors(res_rh, merge_node = merge_node_param(min_n_signatures = 1634))
</code></pre>

<pre><code>#&gt;          Hours    Media    State
#&gt; class 1.48e-51 7.41e-32 1.26e-54
</code></pre>

</div>
<div id='tab-test-to-known-factors-from-hierarchical-partition-6'>
<pre><code class="r">test_to_known_factors(res_rh, merge_node = merge_node_param(min_n_signatures = 3037))
</code></pre>

<pre><code>#&gt;          Hours    Media    State
#&gt; class 6.78e-31 1.25e-33 4.13e-58
</code></pre>

</div>
</div>



## Results for each node


---------------------------------------------------




### Node0


Child nodes: 
                [Node01](#Node01)
        ,
                [Node02](#Node02)
        ,
                [Node03](#Node03)
        .







The object with results only for a single top-value method and a single partitioning method 
can be extracted as:

```r
res = res_rh["0"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4.
#>   On a matrix with 11404 rows and 271 columns.
#>   Top rows (1140) are extracted by 'ATC' method.
#>   Subgroups are detected by 'skmeans' method.
#>   Performed in total 150 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_partitions"     
#>  [7] "compare_signatures"      "consensus_heatmap"       "dimension_reduction"    
#> [10] "functional_enrichment"   "get_anno_col"            "get_anno"               
#> [13] "get_classes"             "get_consensus"           "get_matrix"             
#> [16] "get_membership"          "get_param"               "get_signatures"         
#> [19] "get_stats"               "is_best_k"               "is_stable_k"            
#> [22] "membership_heatmap"      "ncol"                    "nrow"                   
#> [25] "plot_ecdf"               "predict_classes"         "rownames"               
#> [28] "select_partition_number" "show"                    "suggest_best_k"         
#> [31] "test_to_known_factors"   "top_rows_heatmap"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of subgroups)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk node-0-collect-plots](figure_cola/node-0-collect-plots-1.png)

The plots are:

- The first row: a plot of the eCDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- eCDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus subgroup labels in all
  partitions.
- Area increased. Denote $A_k$ as the area under the eCDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](https://jokergoo.github.io/cola_vignettes/cola.html#toc_13).

Generally speaking, higher 1-PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk node-0-select-partition-number](figure_cola/node-0-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.988       0.995          0.487 0.513   0.513
#> 3 3 1.000           0.974       0.990          0.353 0.775   0.582
#> 4 4 0.755           0.734       0.877          0.108 0.921   0.778
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 3
#> attr(,"optional")
#> [1] 2
```

There is also optional best $k$ = 2 that is worth to check.

Following is the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall subgroup
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-node-0-get-classes' ).tabs();
} );
</script>
<div id='tabs-node-0-get-classes'>
<ul>
<li><a href='#tab-node-0-get-classes-1'>k = 2</a></li>
<li><a href='#tab-node-0-get-classes-2'>k = 3</a></li>
<li><a href='#tab-node-0-get-classes-3'>k = 4</a></li>
</ul>

<div id='tab-node-0-get-classes-1'>
<p><a id='tab-node-0-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette   p1   p2
#&gt; T0_CT_A01      1   0.000      0.996 1.00 0.00
#&gt; T0_CT_A03      1   0.000      0.996 1.00 0.00
#&gt; T0_CT_A05      1   0.000      0.996 1.00 0.00
#&gt; T0_CT_A06      1   0.000      0.996 1.00 0.00
#&gt; T0_CT_A07      1   0.000      0.996 1.00 0.00
#&gt; T0_CT_A08      2   0.000      0.993 0.00 1.00
#&gt; T0_CT_A10      1   0.000      0.996 1.00 0.00
#&gt; T0_CT_A11      1   0.242      0.956 0.96 0.04
#&gt; T0_CT_B01      1   0.000      0.996 1.00 0.00
#&gt; T0_CT_B03      1   0.000      0.996 1.00 0.00
#&gt; T0_CT_B05      2   0.000      0.993 0.00 1.00
#&gt; T0_CT_B07      1   0.000      0.996 1.00 0.00
#&gt; T0_CT_B08      1   0.000      0.996 1.00 0.00
#&gt; T0_CT_B09      1   0.000      0.996 1.00 0.00
#&gt; T0_CT_C02      1   0.000      0.996 1.00 0.00
#&gt; T0_CT_C03      2   0.000      0.993 0.00 1.00
#&gt; T0_CT_C05      1   0.000      0.996 1.00 0.00
#&gt; T0_CT_C06      1   0.943      0.435 0.64 0.36
#&gt; T0_CT_C07      1   0.141      0.977 0.98 0.02
#&gt; T0_CT_C08      1   0.000      0.996 1.00 0.00
#&gt; T0_CT_C09      1   0.000      0.996 1.00 0.00
#&gt; T0_CT_C11      1   0.000      0.996 1.00 0.00
#&gt; T0_CT_C12      1   0.000      0.996 1.00 0.00
#&gt; T0_CT_D01      1   0.000      0.996 1.00 0.00
#&gt; T0_CT_D02      1   0.000      0.996 1.00 0.00
#&gt; T0_CT_D03      1   0.000      0.996 1.00 0.00
#&gt; T0_CT_D05      1   0.000      0.996 1.00 0.00
#&gt; T0_CT_D06      1   0.000      0.996 1.00 0.00
#&gt; T0_CT_D07      1   0.000      0.996 1.00 0.00
#&gt; T0_CT_D08      1   0.000      0.996 1.00 0.00
#&gt; T0_CT_D09      1   0.000      0.996 1.00 0.00
#&gt; T0_CT_D11      1   0.000      0.996 1.00 0.00
#&gt; T0_CT_D12      1   0.000      0.996 1.00 0.00
#&gt; T0_CT_E01      1   0.000      0.996 1.00 0.00
#&gt; T0_CT_E03      1   0.000      0.996 1.00 0.00
#&gt; T0_CT_E04      1   0.000      0.996 1.00 0.00
#&gt; T0_CT_E05      2   0.000      0.993 0.00 1.00
#&gt; T0_CT_E06      1   0.000      0.996 1.00 0.00
#&gt; T0_CT_E07      1   0.000      0.996 1.00 0.00
#&gt; T0_CT_E08      1   0.000      0.996 1.00 0.00
#&gt; T0_CT_E09      1   0.000      0.996 1.00 0.00
#&gt; T0_CT_E10      1   0.000      0.996 1.00 0.00
#&gt; T0_CT_E11      1   0.000      0.996 1.00 0.00
#&gt; T0_CT_E12      1   0.000      0.996 1.00 0.00
#&gt; T0_CT_F01      1   0.000      0.996 1.00 0.00
#&gt; T0_CT_F02      1   0.000      0.996 1.00 0.00
#&gt; T0_CT_F03      1   0.000      0.996 1.00 0.00
#&gt; T0_CT_F04      1   0.000      0.996 1.00 0.00
#&gt; T0_CT_F05      1   0.000      0.996 1.00 0.00
#&gt; T0_CT_F06      1   0.000      0.996 1.00 0.00
#&gt; T0_CT_F07      1   0.000      0.996 1.00 0.00
#&gt; T0_CT_F09      1   0.000      0.996 1.00 0.00
#&gt; T0_CT_F11      1   0.000      0.996 1.00 0.00
#&gt; T0_CT_F12      1   0.000      0.996 1.00 0.00
#&gt; T0_CT_G01      1   0.000      0.996 1.00 0.00
#&gt; T0_CT_G02      2   0.943      0.439 0.36 0.64
#&gt; T0_CT_G03      1   0.000      0.996 1.00 0.00
#&gt; T0_CT_G04      1   0.000      0.996 1.00 0.00
#&gt; T0_CT_G07      1   0.000      0.996 1.00 0.00
#&gt; T0_CT_G08      1   0.000      0.996 1.00 0.00
#&gt; T0_CT_G09      2   0.000      0.993 0.00 1.00
#&gt; T0_CT_G11      1   0.469      0.888 0.90 0.10
#&gt; T0_CT_H01      2   0.000      0.993 0.00 1.00
#&gt; T0_CT_H02      1   0.000      0.996 1.00 0.00
#&gt; T0_CT_H04      1   0.000      0.996 1.00 0.00
#&gt; T0_CT_H05      1   0.141      0.977 0.98 0.02
#&gt; T0_CT_H08      1   0.000      0.996 1.00 0.00
#&gt; T0_CT_H09      1   0.000      0.996 1.00 0.00
#&gt; T0_CT_H12      1   0.000      0.996 1.00 0.00
#&gt; T24_CT_A01     2   0.000      0.993 0.00 1.00
#&gt; T24_CT_A03     1   0.000      0.996 1.00 0.00
#&gt; T24_CT_A04     1   0.000      0.996 1.00 0.00
#&gt; T24_CT_A05     2   0.000      0.993 0.00 1.00
#&gt; T24_CT_A07     1   0.000      0.996 1.00 0.00
#&gt; T24_CT_A08     2   0.000      0.993 0.00 1.00
#&gt; T24_CT_A09     2   0.000      0.993 0.00 1.00
#&gt; T24_CT_A10     2   0.000      0.993 0.00 1.00
#&gt; T24_CT_B01     2   0.000      0.993 0.00 1.00
#&gt; T24_CT_B02     2   0.000      0.993 0.00 1.00
#&gt; T24_CT_B03     1   0.242      0.956 0.96 0.04
#&gt; T24_CT_B05     1   0.000      0.996 1.00 0.00
#&gt; T24_CT_B06     1   0.000      0.996 1.00 0.00
#&gt; T24_CT_B07     2   0.000      0.993 0.00 1.00
#&gt; T24_CT_B08     1   0.000      0.996 1.00 0.00
#&gt; T24_CT_B09     2   0.000      0.993 0.00 1.00
#&gt; T24_CT_B11     2   0.634      0.808 0.16 0.84
#&gt; T24_CT_C01     2   0.000      0.993 0.00 1.00
#&gt; T24_CT_C02     1   0.000      0.996 1.00 0.00
#&gt; T24_CT_C03     2   0.000      0.993 0.00 1.00
#&gt; T24_CT_C05     1   0.000      0.996 1.00 0.00
#&gt; T24_CT_C07     1   0.000      0.996 1.00 0.00
#&gt; T24_CT_C08     1   0.000      0.996 1.00 0.00
#&gt; T24_CT_C09     1   0.000      0.996 1.00 0.00
#&gt; T24_CT_C10     2   0.000      0.993 0.00 1.00
#&gt; T24_CT_C11     2   0.000      0.993 0.00 1.00
#&gt; T24_CT_C12     1   0.000      0.996 1.00 0.00
#&gt; T24_CT_D01     1   0.000      0.996 1.00 0.00
#&gt; T24_CT_D02     1   0.000      0.996 1.00 0.00
#&gt; T24_CT_D03     1   0.000      0.996 1.00 0.00
#&gt; T24_CT_D04     2   0.000      0.993 0.00 1.00
#&gt; T24_CT_D05     2   0.000      0.993 0.00 1.00
#&gt; T24_CT_D06     2   0.000      0.993 0.00 1.00
#&gt; T24_CT_D07     2   0.000      0.993 0.00 1.00
#&gt; T24_CT_D08     1   0.000      0.996 1.00 0.00
#&gt; T24_CT_D09     1   0.000      0.996 1.00 0.00
#&gt; T24_CT_D10     1   0.000      0.996 1.00 0.00
#&gt; T24_CT_D11     1   0.000      0.996 1.00 0.00
#&gt; T24_CT_E01     2   0.000      0.993 0.00 1.00
#&gt; T24_CT_E02     2   0.000      0.993 0.00 1.00
#&gt; T24_CT_E04     1   0.000      0.996 1.00 0.00
#&gt; T24_CT_E05     2   0.000      0.993 0.00 1.00
#&gt; T24_CT_E07     2   0.000      0.993 0.00 1.00
#&gt; T24_CT_E09     2   0.000      0.993 0.00 1.00
#&gt; T24_CT_E11     2   0.000      0.993 0.00 1.00
#&gt; T24_CT_E12     1   0.000      0.996 1.00 0.00
#&gt; T24_CT_F01     1   0.000      0.996 1.00 0.00
#&gt; T24_CT_F02     1   0.000      0.996 1.00 0.00
#&gt; T24_CT_F03     2   0.000      0.993 0.00 1.00
#&gt; T24_CT_F04     2   0.000      0.993 0.00 1.00
#&gt; T24_CT_F05     2   0.000      0.993 0.00 1.00
#&gt; T24_CT_F07     2   0.000      0.993 0.00 1.00
#&gt; T24_CT_F08     2   0.000      0.993 0.00 1.00
#&gt; T24_CT_F09     1   0.000      0.996 1.00 0.00
#&gt; T24_CT_F10     2   0.000      0.993 0.00 1.00
#&gt; T24_CT_F11     2   0.000      0.993 0.00 1.00
#&gt; T24_CT_F12     1   0.000      0.996 1.00 0.00
#&gt; T24_CT_G01     1   0.000      0.996 1.00 0.00
#&gt; T24_CT_G02     2   0.000      0.993 0.00 1.00
#&gt; T24_CT_G03     2   0.000      0.993 0.00 1.00
#&gt; T24_CT_G04     1   0.000      0.996 1.00 0.00
#&gt; T24_CT_G05     1   0.000      0.996 1.00 0.00
#&gt; T24_CT_G06     2   0.000      0.993 0.00 1.00
#&gt; T24_CT_G08     2   0.000      0.993 0.00 1.00
#&gt; T24_CT_G10     1   0.000      0.996 1.00 0.00
#&gt; T24_CT_G11     2   0.000      0.993 0.00 1.00
#&gt; T24_CT_G12     2   0.000      0.993 0.00 1.00
#&gt; T24_CT_H01     1   0.000      0.996 1.00 0.00
#&gt; T24_CT_H02     2   0.000      0.993 0.00 1.00
#&gt; T24_CT_H03     1   0.000      0.996 1.00 0.00
#&gt; T24_CT_H05     2   0.000      0.993 0.00 1.00
#&gt; T24_CT_H07     2   0.000      0.993 0.00 1.00
#&gt; T24_CT_H09     2   0.000      0.993 0.00 1.00
#&gt; T24_CT_H12     2   0.000      0.993 0.00 1.00
#&gt; T48_CT_A01     1   0.000      0.996 1.00 0.00
#&gt; T48_CT_A02     1   0.000      0.996 1.00 0.00
#&gt; T48_CT_A03     2   0.000      0.993 0.00 1.00
#&gt; T48_CT_A04     1   0.000      0.996 1.00 0.00
#&gt; T48_CT_A05     2   0.000      0.993 0.00 1.00
#&gt; T48_CT_A06     2   0.000      0.993 0.00 1.00
#&gt; T48_CT_A07     1   0.000      0.996 1.00 0.00
#&gt; T48_CT_A08     1   0.000      0.996 1.00 0.00
#&gt; T48_CT_A09     2   0.000      0.993 0.00 1.00
#&gt; T48_CT_A10     1   0.000      0.996 1.00 0.00
#&gt; T48_CT_A11     1   0.000      0.996 1.00 0.00
#&gt; T48_CT_A12     2   0.000      0.993 0.00 1.00
#&gt; T48_CT_B01     1   0.000      0.996 1.00 0.00
#&gt; T48_CT_B02     1   0.000      0.996 1.00 0.00
#&gt; T48_CT_B03     2   0.000      0.993 0.00 1.00
#&gt; T48_CT_B04     2   0.000      0.993 0.00 1.00
#&gt; T48_CT_B06     1   0.000      0.996 1.00 0.00
#&gt; T48_CT_B08     2   0.000      0.993 0.00 1.00
#&gt; T48_CT_B10     1   0.000      0.996 1.00 0.00
#&gt; T48_CT_B11     1   0.000      0.996 1.00 0.00
#&gt; T48_CT_B12     2   0.000      0.993 0.00 1.00
#&gt; T48_CT_C01     1   0.000      0.996 1.00 0.00
#&gt; T48_CT_C02     2   0.000      0.993 0.00 1.00
#&gt; T48_CT_C03     1   0.000      0.996 1.00 0.00
#&gt; T48_CT_C04     2   0.000      0.993 0.00 1.00
#&gt; T48_CT_C05     1   0.000      0.996 1.00 0.00
#&gt; T48_CT_C06     1   0.000      0.996 1.00 0.00
#&gt; T48_CT_C07     2   0.000      0.993 0.00 1.00
#&gt; T48_CT_C09     1   0.000      0.996 1.00 0.00
#&gt; T48_CT_C10     2   0.000      0.993 0.00 1.00
#&gt; T48_CT_C11     1   0.000      0.996 1.00 0.00
#&gt; T48_CT_D01     2   0.000      0.993 0.00 1.00
#&gt; T48_CT_D02     2   0.000      0.993 0.00 1.00
#&gt; T48_CT_D03     2   0.000      0.993 0.00 1.00
#&gt; T48_CT_D04     1   0.000      0.996 1.00 0.00
#&gt; T48_CT_D06     1   0.000      0.996 1.00 0.00
#&gt; T48_CT_D07     1   0.000      0.996 1.00 0.00
#&gt; T48_CT_D08     2   0.000      0.993 0.00 1.00
#&gt; T48_CT_D09     1   0.000      0.996 1.00 0.00
#&gt; T48_CT_D10     2   0.000      0.993 0.00 1.00
#&gt; T48_CT_D11     1   0.000      0.996 1.00 0.00
#&gt; T48_CT_D12     2   0.000      0.993 0.00 1.00
#&gt; T48_CT_E01     2   0.000      0.993 0.00 1.00
#&gt; T48_CT_E02     2   0.000      0.993 0.00 1.00
#&gt; T48_CT_E03     2   0.000      0.993 0.00 1.00
#&gt; T48_CT_E04     2   0.000      0.993 0.00 1.00
#&gt; T48_CT_E05     1   0.000      0.996 1.00 0.00
#&gt; T48_CT_E06     2   0.000      0.993 0.00 1.00
#&gt; T48_CT_E07     2   0.000      0.993 0.00 1.00
#&gt; T48_CT_E08     1   0.000      0.996 1.00 0.00
#&gt; T48_CT_E10     2   0.000      0.993 0.00 1.00
#&gt; T48_CT_E11     2   0.000      0.993 0.00 1.00
#&gt; T48_CT_E12     1   0.000      0.996 1.00 0.00
#&gt; T48_CT_F01     1   0.000      0.996 1.00 0.00
#&gt; T48_CT_F02     2   0.000      0.993 0.00 1.00
#&gt; T48_CT_F03     2   0.000      0.993 0.00 1.00
#&gt; T48_CT_F05     1   0.000      0.996 1.00 0.00
#&gt; T48_CT_F07     2   0.000      0.993 0.00 1.00
#&gt; T48_CT_F09     1   0.000      0.996 1.00 0.00
#&gt; T48_CT_F10     2   0.000      0.993 0.00 1.00
#&gt; T48_CT_F11     2   0.000      0.993 0.00 1.00
#&gt; T48_CT_G01     2   0.000      0.993 0.00 1.00
#&gt; T48_CT_G02     1   0.000      0.996 1.00 0.00
#&gt; T48_CT_G03     1   0.000      0.996 1.00 0.00
#&gt; T48_CT_G07     1   0.000      0.996 1.00 0.00
#&gt; T48_CT_G08     1   0.000      0.996 1.00 0.00
#&gt; T48_CT_G09     2   0.000      0.993 0.00 1.00
#&gt; T48_CT_G10     2   0.000      0.993 0.00 1.00
#&gt; T48_CT_G11     2   0.000      0.993 0.00 1.00
#&gt; T48_CT_G12     1   0.000      0.996 1.00 0.00
#&gt; T48_CT_H01     1   0.000      0.996 1.00 0.00
#&gt; T48_CT_H02     2   0.000      0.993 0.00 1.00
#&gt; T48_CT_H04     2   0.000      0.993 0.00 1.00
#&gt; T48_CT_H05     1   0.000      0.996 1.00 0.00
#&gt; T48_CT_H06     1   0.000      0.996 1.00 0.00
#&gt; T48_CT_H07     2   0.000      0.993 0.00 1.00
#&gt; T48_CT_H08     1   0.000      0.996 1.00 0.00
#&gt; T48_CT_H11     2   0.000      0.993 0.00 1.00
#&gt; T48_CT_H12     1   0.000      0.996 1.00 0.00
#&gt; T72_CT_A01     1   0.000      0.996 1.00 0.00
#&gt; T72_CT_A05     1   0.000      0.996 1.00 0.00
#&gt; T72_CT_A08     2   0.000      0.993 0.00 1.00
#&gt; T72_CT_A09     1   0.000      0.996 1.00 0.00
#&gt; T72_CT_A11     1   0.000      0.996 1.00 0.00
#&gt; T72_CT_B01     1   0.000      0.996 1.00 0.00
#&gt; T72_CT_B02     2   0.000      0.993 0.00 1.00
#&gt; T72_CT_B03     2   0.000      0.993 0.00 1.00
#&gt; T72_CT_B04     2   0.000      0.993 0.00 1.00
#&gt; T72_CT_B05     2   0.000      0.993 0.00 1.00
#&gt; T72_CT_B06     2   0.000      0.993 0.00 1.00
#&gt; T72_CT_B08     1   0.000      0.996 1.00 0.00
#&gt; T72_CT_B09     2   0.000      0.993 0.00 1.00
#&gt; T72_CT_B11     2   0.000      0.993 0.00 1.00
#&gt; T72_CT_B12     1   0.000      0.996 1.00 0.00
#&gt; T72_CT_C04     1   0.000      0.996 1.00 0.00
#&gt; T72_CT_C06     2   0.000      0.993 0.00 1.00
#&gt; T72_CT_C07     1   0.000      0.996 1.00 0.00
#&gt; T72_CT_C09     2   0.000      0.993 0.00 1.00
#&gt; T72_CT_C11     1   0.000      0.996 1.00 0.00
#&gt; T72_CT_D01     1   0.000      0.996 1.00 0.00
#&gt; T72_CT_D03     2   0.000      0.993 0.00 1.00
#&gt; T72_CT_D04     1   0.000      0.996 1.00 0.00
#&gt; T72_CT_D05     2   0.000      0.993 0.00 1.00
#&gt; T72_CT_D07     2   0.000      0.993 0.00 1.00
#&gt; T72_CT_D10     2   0.000      0.993 0.00 1.00
#&gt; T72_CT_D11     2   0.000      0.993 0.00 1.00
#&gt; T72_CT_E04     2   0.000      0.993 0.00 1.00
#&gt; T72_CT_E05     1   0.000      0.996 1.00 0.00
#&gt; T72_CT_E07     1   0.327      0.935 0.94 0.06
#&gt; T72_CT_F01     1   0.000      0.996 1.00 0.00
#&gt; T72_CT_F05     1   0.000      0.996 1.00 0.00
#&gt; T72_CT_F07     2   0.000      0.993 0.00 1.00
#&gt; T72_CT_F10     1   0.000      0.996 1.00 0.00
#&gt; T72_CT_F11     1   0.000      0.996 1.00 0.00
#&gt; T72_CT_G03     2   0.000      0.993 0.00 1.00
#&gt; T72_CT_G04     2   0.000      0.993 0.00 1.00
#&gt; T72_CT_G06     1   0.000      0.996 1.00 0.00
#&gt; T72_CT_G08     2   0.827      0.650 0.26 0.74
#&gt; T72_CT_G10     1   0.000      0.996 1.00 0.00
#&gt; T72_CT_G11     2   0.000      0.993 0.00 1.00
#&gt; T72_CT_H01     1   0.000      0.996 1.00 0.00
#&gt; T72_CT_H03     1   0.000      0.996 1.00 0.00
#&gt; T72_CT_H05     1   0.000      0.996 1.00 0.00
#&gt; T72_CT_H08     1   0.000      0.996 1.00 0.00
#&gt; T72_CT_H09     1   0.000      0.996 1.00 0.00
#&gt; T72_CT_H10     2   0.000      0.993 0.00 1.00
#&gt; T72_CT_H11     2   0.000      0.993 0.00 1.00
#&gt; T72_CT_H12     1   0.000      0.996 1.00 0.00
</code></pre>

<script>
$('#tab-node-0-get-classes-1-a').parent().next().next().hide();
$('#tab-node-0-get-classes-1-a').click(function(){
  $('#tab-node-0-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-node-0-get-classes-2'>
<p><a id='tab-node-0-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette   p1   p2   p3
#&gt; T0_CT_A01      3  0.0000      0.980 0.00 0.00 1.00
#&gt; T0_CT_A03      3  0.0000      0.980 0.00 0.00 1.00
#&gt; T0_CT_A05      3  0.0000      0.980 0.00 0.00 1.00
#&gt; T0_CT_A06      3  0.0000      0.980 0.00 0.00 1.00
#&gt; T0_CT_A07      3  0.0000      0.980 0.00 0.00 1.00
#&gt; T0_CT_A08      2  0.0000      0.994 0.00 1.00 0.00
#&gt; T0_CT_A10      3  0.0000      0.980 0.00 0.00 1.00
#&gt; T0_CT_A11      3  0.0000      0.980 0.00 0.00 1.00
#&gt; T0_CT_B01      3  0.0000      0.980 0.00 0.00 1.00
#&gt; T0_CT_B03      3  0.0000      0.980 0.00 0.00 1.00
#&gt; T0_CT_B05      3  0.0000      0.980 0.00 0.00 1.00
#&gt; T0_CT_B07      3  0.0000      0.980 0.00 0.00 1.00
#&gt; T0_CT_B08      3  0.0000      0.980 0.00 0.00 1.00
#&gt; T0_CT_B09      3  0.0000      0.980 0.00 0.00 1.00
#&gt; T0_CT_C02      3  0.0000      0.980 0.00 0.00 1.00
#&gt; T0_CT_C03      2  0.0000      0.994 0.00 1.00 0.00
#&gt; T0_CT_C05      1  0.0000      0.990 1.00 0.00 0.00
#&gt; T0_CT_C06      1  0.9456      0.255 0.48 0.32 0.20
#&gt; T0_CT_C07      3  0.0000      0.980 0.00 0.00 1.00
#&gt; T0_CT_C08      3  0.0000      0.980 0.00 0.00 1.00
#&gt; T0_CT_C09      1  0.0000      0.990 1.00 0.00 0.00
#&gt; T0_CT_C11      1  0.0000      0.990 1.00 0.00 0.00
#&gt; T0_CT_C12      3  0.0000      0.980 0.00 0.00 1.00
#&gt; T0_CT_D01      3  0.0000      0.980 0.00 0.00 1.00
#&gt; T0_CT_D02      3  0.0000      0.980 0.00 0.00 1.00
#&gt; T0_CT_D03      3  0.0000      0.980 0.00 0.00 1.00
#&gt; T0_CT_D05      3  0.5560      0.580 0.30 0.00 0.70
#&gt; T0_CT_D06      3  0.0000      0.980 0.00 0.00 1.00
#&gt; T0_CT_D07      3  0.0000      0.980 0.00 0.00 1.00
#&gt; T0_CT_D08      3  0.0000      0.980 0.00 0.00 1.00
#&gt; T0_CT_D09      1  0.0000      0.990 1.00 0.00 0.00
#&gt; T0_CT_D11      1  0.0000      0.990 1.00 0.00 0.00
#&gt; T0_CT_D12      3  0.0000      0.980 0.00 0.00 1.00
#&gt; T0_CT_E01      3  0.0000      0.980 0.00 0.00 1.00
#&gt; T0_CT_E03      3  0.0000      0.980 0.00 0.00 1.00
#&gt; T0_CT_E04      1  0.0000      0.990 1.00 0.00 0.00
#&gt; T0_CT_E05      2  0.0000      0.994 0.00 1.00 0.00
#&gt; T0_CT_E06      3  0.0000      0.980 0.00 0.00 1.00
#&gt; T0_CT_E07      3  0.0000      0.980 0.00 0.00 1.00
#&gt; T0_CT_E08      3  0.0000      0.980 0.00 0.00 1.00
#&gt; T0_CT_E09      3  0.0000      0.980 0.00 0.00 1.00
#&gt; T0_CT_E10      3  0.4291      0.781 0.18 0.00 0.82
#&gt; T0_CT_E11      3  0.0000      0.980 0.00 0.00 1.00
#&gt; T0_CT_E12      3  0.0000      0.980 0.00 0.00 1.00
#&gt; T0_CT_F01      3  0.0000      0.980 0.00 0.00 1.00
#&gt; T0_CT_F02      3  0.0000      0.980 0.00 0.00 1.00
#&gt; T0_CT_F03      3  0.0000      0.980 0.00 0.00 1.00
#&gt; T0_CT_F04      3  0.0000      0.980 0.00 0.00 1.00
#&gt; T0_CT_F05      3  0.0000      0.980 0.00 0.00 1.00
#&gt; T0_CT_F06      3  0.0000      0.980 0.00 0.00 1.00
#&gt; T0_CT_F07      3  0.0000      0.980 0.00 0.00 1.00
#&gt; T0_CT_F09      1  0.0000      0.990 1.00 0.00 0.00
#&gt; T0_CT_F11      3  0.0000      0.980 0.00 0.00 1.00
#&gt; T0_CT_F12      3  0.0000      0.980 0.00 0.00 1.00
#&gt; T0_CT_G01      3  0.0000      0.980 0.00 0.00 1.00
#&gt; T0_CT_G02      3  0.0000      0.980 0.00 0.00 1.00
#&gt; T0_CT_G03      3  0.0000      0.980 0.00 0.00 1.00
#&gt; T0_CT_G04      3  0.0000      0.980 0.00 0.00 1.00
#&gt; T0_CT_G07      3  0.0000      0.980 0.00 0.00 1.00
#&gt; T0_CT_G08      3  0.0000      0.980 0.00 0.00 1.00
#&gt; T0_CT_G09      2  0.0000      0.994 0.00 1.00 0.00
#&gt; T0_CT_G11      3  0.0000      0.980 0.00 0.00 1.00
#&gt; T0_CT_H01      2  0.0000      0.994 0.00 1.00 0.00
#&gt; T0_CT_H02      3  0.4291      0.781 0.18 0.00 0.82
#&gt; T0_CT_H04      3  0.0000      0.980 0.00 0.00 1.00
#&gt; T0_CT_H05      3  0.0000      0.980 0.00 0.00 1.00
#&gt; T0_CT_H08      3  0.0000      0.980 0.00 0.00 1.00
#&gt; T0_CT_H09      3  0.6126      0.352 0.40 0.00 0.60
#&gt; T0_CT_H12      3  0.0000      0.980 0.00 0.00 1.00
#&gt; T24_CT_A01     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T24_CT_A03     1  0.0000      0.990 1.00 0.00 0.00
#&gt; T24_CT_A04     1  0.0000      0.990 1.00 0.00 0.00
#&gt; T24_CT_A05     3  0.0000      0.980 0.00 0.00 1.00
#&gt; T24_CT_A07     1  0.0000      0.990 1.00 0.00 0.00
#&gt; T24_CT_A08     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T24_CT_A09     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T24_CT_A10     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T24_CT_B01     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T24_CT_B02     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T24_CT_B03     1  0.0000      0.990 1.00 0.00 0.00
#&gt; T24_CT_B05     1  0.0000      0.990 1.00 0.00 0.00
#&gt; T24_CT_B06     1  0.0000      0.990 1.00 0.00 0.00
#&gt; T24_CT_B07     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T24_CT_B08     1  0.0000      0.990 1.00 0.00 0.00
#&gt; T24_CT_B09     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T24_CT_B11     3  0.0000      0.980 0.00 0.00 1.00
#&gt; T24_CT_C01     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T24_CT_C02     1  0.0000      0.990 1.00 0.00 0.00
#&gt; T24_CT_C03     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T24_CT_C05     1  0.0000      0.990 1.00 0.00 0.00
#&gt; T24_CT_C07     1  0.0000      0.990 1.00 0.00 0.00
#&gt; T24_CT_C08     1  0.0000      0.990 1.00 0.00 0.00
#&gt; T24_CT_C09     1  0.0000      0.990 1.00 0.00 0.00
#&gt; T24_CT_C10     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T24_CT_C11     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T24_CT_C12     1  0.0000      0.990 1.00 0.00 0.00
#&gt; T24_CT_D01     1  0.0000      0.990 1.00 0.00 0.00
#&gt; T24_CT_D02     1  0.0000      0.990 1.00 0.00 0.00
#&gt; T24_CT_D03     1  0.0000      0.990 1.00 0.00 0.00
#&gt; T24_CT_D04     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T24_CT_D05     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T24_CT_D06     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T24_CT_D07     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T24_CT_D08     1  0.0000      0.990 1.00 0.00 0.00
#&gt; T24_CT_D09     1  0.0000      0.990 1.00 0.00 0.00
#&gt; T24_CT_D10     1  0.0000      0.990 1.00 0.00 0.00
#&gt; T24_CT_D11     1  0.0000      0.990 1.00 0.00 0.00
#&gt; T24_CT_E01     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T24_CT_E02     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T24_CT_E04     1  0.0000      0.990 1.00 0.00 0.00
#&gt; T24_CT_E05     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T24_CT_E07     1  0.5016      0.683 0.76 0.24 0.00
#&gt; T24_CT_E09     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T24_CT_E11     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T24_CT_E12     1  0.0000      0.990 1.00 0.00 0.00
#&gt; T24_CT_F01     1  0.0000      0.990 1.00 0.00 0.00
#&gt; T24_CT_F02     1  0.0000      0.990 1.00 0.00 0.00
#&gt; T24_CT_F03     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T24_CT_F04     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T24_CT_F05     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T24_CT_F07     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T24_CT_F08     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T24_CT_F09     1  0.0000      0.990 1.00 0.00 0.00
#&gt; T24_CT_F10     2  0.6280      0.137 0.46 0.54 0.00
#&gt; T24_CT_F11     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T24_CT_F12     3  0.0000      0.980 0.00 0.00 1.00
#&gt; T24_CT_G01     1  0.0000      0.990 1.00 0.00 0.00
#&gt; T24_CT_G02     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T24_CT_G03     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T24_CT_G04     1  0.0000      0.990 1.00 0.00 0.00
#&gt; T24_CT_G05     1  0.0000      0.990 1.00 0.00 0.00
#&gt; T24_CT_G06     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T24_CT_G08     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T24_CT_G10     1  0.0000      0.990 1.00 0.00 0.00
#&gt; T24_CT_G11     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T24_CT_G12     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T24_CT_H01     3  0.0000      0.980 0.00 0.00 1.00
#&gt; T24_CT_H02     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T24_CT_H03     1  0.0000      0.990 1.00 0.00 0.00
#&gt; T24_CT_H05     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T24_CT_H07     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T24_CT_H09     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T24_CT_H12     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T48_CT_A01     1  0.0000      0.990 1.00 0.00 0.00
#&gt; T48_CT_A02     1  0.0000      0.990 1.00 0.00 0.00
#&gt; T48_CT_A03     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T48_CT_A04     1  0.0000      0.990 1.00 0.00 0.00
#&gt; T48_CT_A05     1  0.0000      0.990 1.00 0.00 0.00
#&gt; T48_CT_A06     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T48_CT_A07     3  0.0000      0.980 0.00 0.00 1.00
#&gt; T48_CT_A08     1  0.0000      0.990 1.00 0.00 0.00
#&gt; T48_CT_A09     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T48_CT_A10     1  0.0000      0.990 1.00 0.00 0.00
#&gt; T48_CT_A11     1  0.0000      0.990 1.00 0.00 0.00
#&gt; T48_CT_A12     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T48_CT_B01     1  0.0000      0.990 1.00 0.00 0.00
#&gt; T48_CT_B02     1  0.0000      0.990 1.00 0.00 0.00
#&gt; T48_CT_B03     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T48_CT_B04     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T48_CT_B06     1  0.0000      0.990 1.00 0.00 0.00
#&gt; T48_CT_B08     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T48_CT_B10     1  0.0000      0.990 1.00 0.00 0.00
#&gt; T48_CT_B11     1  0.0000      0.990 1.00 0.00 0.00
#&gt; T48_CT_B12     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T48_CT_C01     1  0.0000      0.990 1.00 0.00 0.00
#&gt; T48_CT_C02     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T48_CT_C03     1  0.0000      0.990 1.00 0.00 0.00
#&gt; T48_CT_C04     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T48_CT_C05     1  0.0000      0.990 1.00 0.00 0.00
#&gt; T48_CT_C06     1  0.0000      0.990 1.00 0.00 0.00
#&gt; T48_CT_C07     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T48_CT_C09     1  0.0000      0.990 1.00 0.00 0.00
#&gt; T48_CT_C10     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T48_CT_C11     1  0.0000      0.990 1.00 0.00 0.00
#&gt; T48_CT_D01     1  0.0000      0.990 1.00 0.00 0.00
#&gt; T48_CT_D02     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T48_CT_D03     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T48_CT_D04     1  0.0000      0.990 1.00 0.00 0.00
#&gt; T48_CT_D06     1  0.0000      0.990 1.00 0.00 0.00
#&gt; T48_CT_D07     1  0.0000      0.990 1.00 0.00 0.00
#&gt; T48_CT_D08     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T48_CT_D09     1  0.0000      0.990 1.00 0.00 0.00
#&gt; T48_CT_D10     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T48_CT_D11     1  0.0000      0.990 1.00 0.00 0.00
#&gt; T48_CT_D12     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T48_CT_E01     1  0.0000      0.990 1.00 0.00 0.00
#&gt; T48_CT_E02     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T48_CT_E03     1  0.3340      0.854 0.88 0.12 0.00
#&gt; T48_CT_E04     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T48_CT_E05     1  0.0000      0.990 1.00 0.00 0.00
#&gt; T48_CT_E06     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T48_CT_E07     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T48_CT_E08     1  0.0000      0.990 1.00 0.00 0.00
#&gt; T48_CT_E10     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T48_CT_E11     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T48_CT_E12     3  0.0000      0.980 0.00 0.00 1.00
#&gt; T48_CT_F01     1  0.0000      0.990 1.00 0.00 0.00
#&gt; T48_CT_F02     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T48_CT_F03     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T48_CT_F05     1  0.0000      0.990 1.00 0.00 0.00
#&gt; T48_CT_F07     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T48_CT_F09     1  0.0000      0.990 1.00 0.00 0.00
#&gt; T48_CT_F10     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T48_CT_F11     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T48_CT_G01     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T48_CT_G02     1  0.0000      0.990 1.00 0.00 0.00
#&gt; T48_CT_G03     1  0.0000      0.990 1.00 0.00 0.00
#&gt; T48_CT_G07     1  0.0000      0.990 1.00 0.00 0.00
#&gt; T48_CT_G08     1  0.0000      0.990 1.00 0.00 0.00
#&gt; T48_CT_G09     1  0.0000      0.990 1.00 0.00 0.00
#&gt; T48_CT_G10     3  0.0000      0.980 0.00 0.00 1.00
#&gt; T48_CT_G11     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T48_CT_G12     1  0.0000      0.990 1.00 0.00 0.00
#&gt; T48_CT_H01     1  0.0000      0.990 1.00 0.00 0.00
#&gt; T48_CT_H02     2  0.0892      0.973 0.02 0.98 0.00
#&gt; T48_CT_H04     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T48_CT_H05     1  0.0000      0.990 1.00 0.00 0.00
#&gt; T48_CT_H06     1  0.0000      0.990 1.00 0.00 0.00
#&gt; T48_CT_H07     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T48_CT_H08     1  0.0000      0.990 1.00 0.00 0.00
#&gt; T48_CT_H11     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T48_CT_H12     1  0.0000      0.990 1.00 0.00 0.00
#&gt; T72_CT_A01     1  0.0000      0.990 1.00 0.00 0.00
#&gt; T72_CT_A05     1  0.0000      0.990 1.00 0.00 0.00
#&gt; T72_CT_A08     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T72_CT_A09     1  0.0000      0.990 1.00 0.00 0.00
#&gt; T72_CT_A11     1  0.0000      0.990 1.00 0.00 0.00
#&gt; T72_CT_B01     1  0.0000      0.990 1.00 0.00 0.00
#&gt; T72_CT_B02     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T72_CT_B03     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T72_CT_B04     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T72_CT_B05     2  0.1529      0.950 0.04 0.96 0.00
#&gt; T72_CT_B06     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T72_CT_B08     3  0.0000      0.980 0.00 0.00 1.00
#&gt; T72_CT_B09     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T72_CT_B11     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T72_CT_B12     1  0.0000      0.990 1.00 0.00 0.00
#&gt; T72_CT_C04     1  0.0000      0.990 1.00 0.00 0.00
#&gt; T72_CT_C06     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T72_CT_C07     1  0.0000      0.990 1.00 0.00 0.00
#&gt; T72_CT_C09     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T72_CT_C11     3  0.0000      0.980 0.00 0.00 1.00
#&gt; T72_CT_D01     1  0.0000      0.990 1.00 0.00 0.00
#&gt; T72_CT_D03     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T72_CT_D04     1  0.0000      0.990 1.00 0.00 0.00
#&gt; T72_CT_D05     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T72_CT_D07     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T72_CT_D10     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T72_CT_D11     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T72_CT_E04     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T72_CT_E05     1  0.0000      0.990 1.00 0.00 0.00
#&gt; T72_CT_E07     1  0.0000      0.990 1.00 0.00 0.00
#&gt; T72_CT_F01     1  0.0000      0.990 1.00 0.00 0.00
#&gt; T72_CT_F05     1  0.0000      0.990 1.00 0.00 0.00
#&gt; T72_CT_F07     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T72_CT_F10     1  0.0000      0.990 1.00 0.00 0.00
#&gt; T72_CT_F11     1  0.2066      0.929 0.94 0.00 0.06
#&gt; T72_CT_G03     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T72_CT_G04     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T72_CT_G06     1  0.0892      0.971 0.98 0.00 0.02
#&gt; T72_CT_G08     3  0.5560      0.569 0.00 0.30 0.70
#&gt; T72_CT_G10     1  0.0000      0.990 1.00 0.00 0.00
#&gt; T72_CT_G11     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T72_CT_H01     1  0.0000      0.990 1.00 0.00 0.00
#&gt; T72_CT_H03     1  0.0000      0.990 1.00 0.00 0.00
#&gt; T72_CT_H05     3  0.0000      0.980 0.00 0.00 1.00
#&gt; T72_CT_H08     3  0.0000      0.980 0.00 0.00 1.00
#&gt; T72_CT_H09     3  0.0000      0.980 0.00 0.00 1.00
#&gt; T72_CT_H10     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T72_CT_H11     2  0.0000      0.994 0.00 1.00 0.00
#&gt; T72_CT_H12     1  0.0000      0.990 1.00 0.00 0.00
</code></pre>

<script>
$('#tab-node-0-get-classes-2-a').parent().next().next().hide();
$('#tab-node-0-get-classes-2-a').click(function(){
  $('#tab-node-0-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-node-0-get-classes-3'>
<p><a id='tab-node-0-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette   p1   p2   p3   p4
#&gt; T0_CT_A01      3  0.0000     0.9413 0.00 0.00 1.00 0.00
#&gt; T0_CT_A03      3  0.0000     0.9413 0.00 0.00 1.00 0.00
#&gt; T0_CT_A05      3  0.0000     0.9413 0.00 0.00 1.00 0.00
#&gt; T0_CT_A06      3  0.3400     0.8432 0.00 0.00 0.82 0.18
#&gt; T0_CT_A07      3  0.0000     0.9413 0.00 0.00 1.00 0.00
#&gt; T0_CT_A08      2  0.0000     0.7850 0.00 1.00 0.00 0.00
#&gt; T0_CT_A10      3  0.0000     0.9413 0.00 0.00 1.00 0.00
#&gt; T0_CT_A11      2  0.8662     0.1490 0.10 0.52 0.18 0.20
#&gt; T0_CT_B01      3  0.5784     0.7149 0.10 0.00 0.70 0.20
#&gt; T0_CT_B03      3  0.0000     0.9413 0.00 0.00 1.00 0.00
#&gt; T0_CT_B05      3  0.5147     0.7779 0.00 0.06 0.74 0.20
#&gt; T0_CT_B07      3  0.3610     0.8290 0.00 0.00 0.80 0.20
#&gt; T0_CT_B08      3  0.0000     0.9413 0.00 0.00 1.00 0.00
#&gt; T0_CT_B09      3  0.0000     0.9413 0.00 0.00 1.00 0.00
#&gt; T0_CT_C02      3  0.0000     0.9413 0.00 0.00 1.00 0.00
#&gt; T0_CT_C03      2  0.0000     0.7850 0.00 1.00 0.00 0.00
#&gt; T0_CT_C05      1  0.3610     0.7539 0.80 0.00 0.00 0.20
#&gt; T0_CT_C06      4  0.5062     0.3927 0.30 0.02 0.00 0.68
#&gt; T0_CT_C07      3  0.2345     0.8931 0.00 0.00 0.90 0.10
#&gt; T0_CT_C08      3  0.0000     0.9413 0.00 0.00 1.00 0.00
#&gt; T0_CT_C09      1  0.3610     0.7539 0.80 0.00 0.00 0.20
#&gt; T0_CT_C11      1  0.3610     0.7539 0.80 0.00 0.00 0.20
#&gt; T0_CT_C12      3  0.0000     0.9413 0.00 0.00 1.00 0.00
#&gt; T0_CT_D01      3  0.3610     0.8290 0.00 0.00 0.80 0.20
#&gt; T0_CT_D02      3  0.3172     0.8569 0.00 0.00 0.84 0.16
#&gt; T0_CT_D03      3  0.0000     0.9413 0.00 0.00 1.00 0.00
#&gt; T0_CT_D05      1  0.7738     0.2096 0.44 0.00 0.26 0.30
#&gt; T0_CT_D06      3  0.0000     0.9413 0.00 0.00 1.00 0.00
#&gt; T0_CT_D07      3  0.2345     0.8931 0.00 0.00 0.90 0.10
#&gt; T0_CT_D08      3  0.0000     0.9413 0.00 0.00 1.00 0.00
#&gt; T0_CT_D09      1  0.3610     0.7539 0.80 0.00 0.00 0.20
#&gt; T0_CT_D11      1  0.3610     0.7539 0.80 0.00 0.00 0.20
#&gt; T0_CT_D12      3  0.0000     0.9413 0.00 0.00 1.00 0.00
#&gt; T0_CT_E01      3  0.0000     0.9413 0.00 0.00 1.00 0.00
#&gt; T0_CT_E03      3  0.0000     0.9413 0.00 0.00 1.00 0.00
#&gt; T0_CT_E04      1  0.3610     0.7539 0.80 0.00 0.00 0.20
#&gt; T0_CT_E05      2  0.4994     0.1508 0.00 0.52 0.00 0.48
#&gt; T0_CT_E06      3  0.0000     0.9413 0.00 0.00 1.00 0.00
#&gt; T0_CT_E07      3  0.0000     0.9413 0.00 0.00 1.00 0.00
#&gt; T0_CT_E08      3  0.0000     0.9413 0.00 0.00 1.00 0.00
#&gt; T0_CT_E09      3  0.0000     0.9413 0.00 0.00 1.00 0.00
#&gt; T0_CT_E10      1  0.5784     0.6459 0.70 0.00 0.10 0.20
#&gt; T0_CT_E11      3  0.0000     0.9413 0.00 0.00 1.00 0.00
#&gt; T0_CT_E12      3  0.0707     0.9325 0.00 0.00 0.98 0.02
#&gt; T0_CT_F01      3  0.0000     0.9413 0.00 0.00 1.00 0.00
#&gt; T0_CT_F02      3  0.0000     0.9413 0.00 0.00 1.00 0.00
#&gt; T0_CT_F03      3  0.0000     0.9413 0.00 0.00 1.00 0.00
#&gt; T0_CT_F04      3  0.0000     0.9413 0.00 0.00 1.00 0.00
#&gt; T0_CT_F05      3  0.3610     0.8290 0.00 0.00 0.80 0.20
#&gt; T0_CT_F06      3  0.0000     0.9413 0.00 0.00 1.00 0.00
#&gt; T0_CT_F07      3  0.3610     0.8290 0.00 0.00 0.80 0.20
#&gt; T0_CT_F09      1  0.3610     0.7539 0.80 0.00 0.00 0.20
#&gt; T0_CT_F11      3  0.2345     0.8931 0.00 0.00 0.90 0.10
#&gt; T0_CT_F12      3  0.0000     0.9413 0.00 0.00 1.00 0.00
#&gt; T0_CT_G01      3  0.0000     0.9413 0.00 0.00 1.00 0.00
#&gt; T0_CT_G02      3  0.5486     0.7585 0.00 0.08 0.72 0.20
#&gt; T0_CT_G03      3  0.0000     0.9413 0.00 0.00 1.00 0.00
#&gt; T0_CT_G04      3  0.0000     0.9413 0.00 0.00 1.00 0.00
#&gt; T0_CT_G07      3  0.0000     0.9413 0.00 0.00 1.00 0.00
#&gt; T0_CT_G08      3  0.0000     0.9413 0.00 0.00 1.00 0.00
#&gt; T0_CT_G09      2  0.4994     0.1508 0.00 0.52 0.00 0.48
#&gt; T0_CT_G11      3  0.3610     0.8290 0.00 0.00 0.80 0.20
#&gt; T0_CT_H01      4  0.4948     0.0208 0.00 0.44 0.00 0.56
#&gt; T0_CT_H02      1  0.6049     0.6186 0.68 0.00 0.12 0.20
#&gt; T0_CT_H04      3  0.1637     0.9138 0.00 0.00 0.94 0.06
#&gt; T0_CT_H05      3  0.6797     0.5777 0.16 0.00 0.60 0.24
#&gt; T0_CT_H08      3  0.3610     0.8290 0.00 0.00 0.80 0.20
#&gt; T0_CT_H09      1  0.5147     0.6918 0.74 0.00 0.06 0.20
#&gt; T0_CT_H12      3  0.3610     0.8290 0.00 0.00 0.80 0.20
#&gt; T24_CT_A01     2  0.0000     0.7850 0.00 1.00 0.00 0.00
#&gt; T24_CT_A03     1  0.0000     0.8795 1.00 0.00 0.00 0.00
#&gt; T24_CT_A04     1  0.3610     0.7539 0.80 0.00 0.00 0.20
#&gt; T24_CT_A05     3  0.0000     0.9413 0.00 0.00 1.00 0.00
#&gt; T24_CT_A07     1  0.0000     0.8795 1.00 0.00 0.00 0.00
#&gt; T24_CT_A08     2  0.0000     0.7850 0.00 1.00 0.00 0.00
#&gt; T24_CT_A09     2  0.0000     0.7850 0.00 1.00 0.00 0.00
#&gt; T24_CT_A10     2  0.4994     0.1508 0.00 0.52 0.00 0.48
#&gt; T24_CT_B01     2  0.0000     0.7850 0.00 1.00 0.00 0.00
#&gt; T24_CT_B02     2  0.0000     0.7850 0.00 1.00 0.00 0.00
#&gt; T24_CT_B03     1  0.0000     0.8795 1.00 0.00 0.00 0.00
#&gt; T24_CT_B05     1  0.0000     0.8795 1.00 0.00 0.00 0.00
#&gt; T24_CT_B06     1  0.0000     0.8795 1.00 0.00 0.00 0.00
#&gt; T24_CT_B07     2  0.0000     0.7850 0.00 1.00 0.00 0.00
#&gt; T24_CT_B08     1  0.2345     0.8294 0.90 0.00 0.00 0.10
#&gt; T24_CT_B09     2  0.0000     0.7850 0.00 1.00 0.00 0.00
#&gt; T24_CT_B11     3  0.0000     0.9413 0.00 0.00 1.00 0.00
#&gt; T24_CT_C01     2  0.0000     0.7850 0.00 1.00 0.00 0.00
#&gt; T24_CT_C02     1  0.1211     0.8623 0.96 0.00 0.00 0.04
#&gt; T24_CT_C03     2  0.0000     0.7850 0.00 1.00 0.00 0.00
#&gt; T24_CT_C05     1  0.2345     0.8294 0.90 0.00 0.00 0.10
#&gt; T24_CT_C07     1  0.0000     0.8795 1.00 0.00 0.00 0.00
#&gt; T24_CT_C08     1  0.2345     0.8294 0.90 0.00 0.00 0.10
#&gt; T24_CT_C09     1  0.1211     0.8623 0.96 0.00 0.00 0.04
#&gt; T24_CT_C10     2  0.0000     0.7850 0.00 1.00 0.00 0.00
#&gt; T24_CT_C11     2  0.4522     0.4499 0.00 0.68 0.00 0.32
#&gt; T24_CT_C12     1  0.0000     0.8795 1.00 0.00 0.00 0.00
#&gt; T24_CT_D01     1  0.0000     0.8795 1.00 0.00 0.00 0.00
#&gt; T24_CT_D02     1  0.0000     0.8795 1.00 0.00 0.00 0.00
#&gt; T24_CT_D03     1  0.3610     0.7539 0.80 0.00 0.00 0.20
#&gt; T24_CT_D04     2  0.0000     0.7850 0.00 1.00 0.00 0.00
#&gt; T24_CT_D05     2  0.0000     0.7850 0.00 1.00 0.00 0.00
#&gt; T24_CT_D06     2  0.0000     0.7850 0.00 1.00 0.00 0.00
#&gt; T24_CT_D07     2  0.0707     0.7711 0.00 0.98 0.00 0.02
#&gt; T24_CT_D08     1  0.0000     0.8795 1.00 0.00 0.00 0.00
#&gt; T24_CT_D09     1  0.1637     0.8522 0.94 0.00 0.00 0.06
#&gt; T24_CT_D10     1  0.0707     0.8714 0.98 0.00 0.00 0.02
#&gt; T24_CT_D11     1  0.0000     0.8795 1.00 0.00 0.00 0.00
#&gt; T24_CT_E01     2  0.4994     0.1508 0.00 0.52 0.00 0.48
#&gt; T24_CT_E02     2  0.0000     0.7850 0.00 1.00 0.00 0.00
#&gt; T24_CT_E04     1  0.0000     0.8795 1.00 0.00 0.00 0.00
#&gt; T24_CT_E05     2  0.0000     0.7850 0.00 1.00 0.00 0.00
#&gt; T24_CT_E07     4  0.7021     0.4698 0.40 0.12 0.00 0.48
#&gt; T24_CT_E09     2  0.0000     0.7850 0.00 1.00 0.00 0.00
#&gt; T24_CT_E11     2  0.0000     0.7850 0.00 1.00 0.00 0.00
#&gt; T24_CT_E12     1  0.0000     0.8795 1.00 0.00 0.00 0.00
#&gt; T24_CT_F01     1  0.0000     0.8795 1.00 0.00 0.00 0.00
#&gt; T24_CT_F02     1  0.2011     0.8412 0.92 0.00 0.00 0.08
#&gt; T24_CT_F03     2  0.4994     0.1508 0.00 0.52 0.00 0.48
#&gt; T24_CT_F04     2  0.4406     0.4806 0.00 0.70 0.00 0.30
#&gt; T24_CT_F05     2  0.4994     0.1508 0.00 0.52 0.00 0.48
#&gt; T24_CT_F07     2  0.3400     0.6323 0.00 0.82 0.00 0.18
#&gt; T24_CT_F08     2  0.0000     0.7850 0.00 1.00 0.00 0.00
#&gt; T24_CT_F09     1  0.2345     0.8294 0.90 0.00 0.00 0.10
#&gt; T24_CT_F10     4  0.7121     0.5688 0.30 0.16 0.00 0.54
#&gt; T24_CT_F11     2  0.4994     0.1508 0.00 0.52 0.00 0.48
#&gt; T24_CT_F12     3  0.0000     0.9413 0.00 0.00 1.00 0.00
#&gt; T24_CT_G01     1  0.2345     0.8294 0.90 0.00 0.00 0.10
#&gt; T24_CT_G02     2  0.0000     0.7850 0.00 1.00 0.00 0.00
#&gt; T24_CT_G03     2  0.0000     0.7850 0.00 1.00 0.00 0.00
#&gt; T24_CT_G04     1  0.0707     0.8715 0.98 0.00 0.00 0.02
#&gt; T24_CT_G05     1  0.2345     0.8294 0.90 0.00 0.00 0.10
#&gt; T24_CT_G06     2  0.0000     0.7850 0.00 1.00 0.00 0.00
#&gt; T24_CT_G08     2  0.0000     0.7850 0.00 1.00 0.00 0.00
#&gt; T24_CT_G10     1  0.0000     0.8795 1.00 0.00 0.00 0.00
#&gt; T24_CT_G11     2  0.0000     0.7850 0.00 1.00 0.00 0.00
#&gt; T24_CT_G12     2  0.0000     0.7850 0.00 1.00 0.00 0.00
#&gt; T24_CT_H01     3  0.0000     0.9413 0.00 0.00 1.00 0.00
#&gt; T24_CT_H02     2  0.0000     0.7850 0.00 1.00 0.00 0.00
#&gt; T24_CT_H03     1  0.0000     0.8795 1.00 0.00 0.00 0.00
#&gt; T24_CT_H05     2  0.4994     0.1508 0.00 0.52 0.00 0.48
#&gt; T24_CT_H07     2  0.0000     0.7850 0.00 1.00 0.00 0.00
#&gt; T24_CT_H09     2  0.0000     0.7850 0.00 1.00 0.00 0.00
#&gt; T24_CT_H12     2  0.0000     0.7850 0.00 1.00 0.00 0.00
#&gt; T48_CT_A01     1  0.0000     0.8795 1.00 0.00 0.00 0.00
#&gt; T48_CT_A02     1  0.0000     0.8795 1.00 0.00 0.00 0.00
#&gt; T48_CT_A03     2  0.0000     0.7850 0.00 1.00 0.00 0.00
#&gt; T48_CT_A04     1  0.0000     0.8795 1.00 0.00 0.00 0.00
#&gt; T48_CT_A05     4  0.3610     0.6935 0.20 0.00 0.00 0.80
#&gt; T48_CT_A06     2  0.0000     0.7850 0.00 1.00 0.00 0.00
#&gt; T48_CT_A07     3  0.0000     0.9413 0.00 0.00 1.00 0.00
#&gt; T48_CT_A08     1  0.0000     0.8795 1.00 0.00 0.00 0.00
#&gt; T48_CT_A09     2  0.4994     0.1508 0.00 0.52 0.00 0.48
#&gt; T48_CT_A10     1  0.0000     0.8795 1.00 0.00 0.00 0.00
#&gt; T48_CT_A11     1  0.0000     0.8795 1.00 0.00 0.00 0.00
#&gt; T48_CT_A12     2  0.4994     0.1508 0.00 0.52 0.00 0.48
#&gt; T48_CT_B01     1  0.0000     0.8795 1.00 0.00 0.00 0.00
#&gt; T48_CT_B02     1  0.0000     0.8795 1.00 0.00 0.00 0.00
#&gt; T48_CT_B03     2  0.0000     0.7850 0.00 1.00 0.00 0.00
#&gt; T48_CT_B04     2  0.0000     0.7850 0.00 1.00 0.00 0.00
#&gt; T48_CT_B06     1  0.0000     0.8795 1.00 0.00 0.00 0.00
#&gt; T48_CT_B08     2  0.0000     0.7850 0.00 1.00 0.00 0.00
#&gt; T48_CT_B10     1  0.0000     0.8795 1.00 0.00 0.00 0.00
#&gt; T48_CT_B11     1  0.0000     0.8795 1.00 0.00 0.00 0.00
#&gt; T48_CT_B12     2  0.4994     0.1508 0.00 0.52 0.00 0.48
#&gt; T48_CT_C01     1  0.0000     0.8795 1.00 0.00 0.00 0.00
#&gt; T48_CT_C02     2  0.0000     0.7850 0.00 1.00 0.00 0.00
#&gt; T48_CT_C03     1  0.0000     0.8795 1.00 0.00 0.00 0.00
#&gt; T48_CT_C04     2  0.0000     0.7850 0.00 1.00 0.00 0.00
#&gt; T48_CT_C05     1  0.0000     0.8795 1.00 0.00 0.00 0.00
#&gt; T48_CT_C06     1  0.0000     0.8795 1.00 0.00 0.00 0.00
#&gt; T48_CT_C07     2  0.4994     0.1508 0.00 0.52 0.00 0.48
#&gt; T48_CT_C09     1  0.0000     0.8795 1.00 0.00 0.00 0.00
#&gt; T48_CT_C10     2  0.0000     0.7850 0.00 1.00 0.00 0.00
#&gt; T48_CT_C11     1  0.0000     0.8795 1.00 0.00 0.00 0.00
#&gt; T48_CT_D01     1  0.4994    -0.2050 0.52 0.00 0.00 0.48
#&gt; T48_CT_D02     2  0.4994     0.1508 0.00 0.52 0.00 0.48
#&gt; T48_CT_D03     2  0.0000     0.7850 0.00 1.00 0.00 0.00
#&gt; T48_CT_D04     1  0.0000     0.8795 1.00 0.00 0.00 0.00
#&gt; T48_CT_D06     1  0.0000     0.8795 1.00 0.00 0.00 0.00
#&gt; T48_CT_D07     1  0.0000     0.8795 1.00 0.00 0.00 0.00
#&gt; T48_CT_D08     2  0.0000     0.7850 0.00 1.00 0.00 0.00
#&gt; T48_CT_D09     1  0.0000     0.8795 1.00 0.00 0.00 0.00
#&gt; T48_CT_D10     2  0.4994     0.1508 0.00 0.52 0.00 0.48
#&gt; T48_CT_D11     1  0.3610     0.7539 0.80 0.00 0.00 0.20
#&gt; T48_CT_D12     2  0.0000     0.7850 0.00 1.00 0.00 0.00
#&gt; T48_CT_E01     4  0.3610     0.6935 0.20 0.00 0.00 0.80
#&gt; T48_CT_E02     2  0.4994     0.1508 0.00 0.52 0.00 0.48
#&gt; T48_CT_E03     4  0.3610     0.6935 0.20 0.00 0.00 0.80
#&gt; T48_CT_E04     2  0.4994     0.1508 0.00 0.52 0.00 0.48
#&gt; T48_CT_E05     1  0.0000     0.8795 1.00 0.00 0.00 0.00
#&gt; T48_CT_E06     2  0.0000     0.7850 0.00 1.00 0.00 0.00
#&gt; T48_CT_E07     2  0.0000     0.7850 0.00 1.00 0.00 0.00
#&gt; T48_CT_E08     1  0.0000     0.8795 1.00 0.00 0.00 0.00
#&gt; T48_CT_E10     2  0.0000     0.7850 0.00 1.00 0.00 0.00
#&gt; T48_CT_E11     2  0.4624     0.4171 0.00 0.66 0.00 0.34
#&gt; T48_CT_E12     3  0.0000     0.9413 0.00 0.00 1.00 0.00
#&gt; T48_CT_F01     1  0.0000     0.8795 1.00 0.00 0.00 0.00
#&gt; T48_CT_F02     2  0.4994     0.1508 0.00 0.52 0.00 0.48
#&gt; T48_CT_F03     2  0.4994     0.1508 0.00 0.52 0.00 0.48
#&gt; T48_CT_F05     1  0.0000     0.8795 1.00 0.00 0.00 0.00
#&gt; T48_CT_F07     2  0.3610     0.6094 0.00 0.80 0.00 0.20
#&gt; T48_CT_F09     1  0.0000     0.8795 1.00 0.00 0.00 0.00
#&gt; T48_CT_F10     2  0.4994     0.1508 0.00 0.52 0.00 0.48
#&gt; T48_CT_F11     2  0.0000     0.7850 0.00 1.00 0.00 0.00
#&gt; T48_CT_G01     2  0.0000     0.7850 0.00 1.00 0.00 0.00
#&gt; T48_CT_G02     1  0.0000     0.8795 1.00 0.00 0.00 0.00
#&gt; T48_CT_G03     1  0.0000     0.8795 1.00 0.00 0.00 0.00
#&gt; T48_CT_G07     1  0.0000     0.8795 1.00 0.00 0.00 0.00
#&gt; T48_CT_G08     1  0.0000     0.8795 1.00 0.00 0.00 0.00
#&gt; T48_CT_G09     4  0.4406     0.6002 0.30 0.00 0.00 0.70
#&gt; T48_CT_G10     3  0.0000     0.9413 0.00 0.00 1.00 0.00
#&gt; T48_CT_G11     2  0.4994     0.1508 0.00 0.52 0.00 0.48
#&gt; T48_CT_G12     1  0.0000     0.8795 1.00 0.00 0.00 0.00
#&gt; T48_CT_H01     1  0.0000     0.8795 1.00 0.00 0.00 0.00
#&gt; T48_CT_H02     4  0.4581     0.7316 0.12 0.08 0.00 0.80
#&gt; T48_CT_H04     2  0.0000     0.7850 0.00 1.00 0.00 0.00
#&gt; T48_CT_H05     1  0.0000     0.8795 1.00 0.00 0.00 0.00
#&gt; T48_CT_H06     1  0.0000     0.8795 1.00 0.00 0.00 0.00
#&gt; T48_CT_H07     2  0.4994     0.1508 0.00 0.52 0.00 0.48
#&gt; T48_CT_H08     1  0.0000     0.8795 1.00 0.00 0.00 0.00
#&gt; T48_CT_H11     2  0.4994     0.1508 0.00 0.52 0.00 0.48
#&gt; T48_CT_H12     1  0.0000     0.8795 1.00 0.00 0.00 0.00
#&gt; T72_CT_A01     1  0.4277     0.6264 0.72 0.00 0.00 0.28
#&gt; T72_CT_A05     1  0.4522     0.5747 0.68 0.00 0.00 0.32
#&gt; T72_CT_A08     2  0.4977     0.1883 0.00 0.54 0.00 0.46
#&gt; T72_CT_A09     1  0.4522     0.5747 0.68 0.00 0.00 0.32
#&gt; T72_CT_A11     1  0.4522     0.5747 0.68 0.00 0.00 0.32
#&gt; T72_CT_B01     1  0.0707     0.8692 0.98 0.00 0.00 0.02
#&gt; T72_CT_B02     2  0.0000     0.7850 0.00 1.00 0.00 0.00
#&gt; T72_CT_B03     4  0.3610     0.7260 0.00 0.20 0.00 0.80
#&gt; T72_CT_B04     2  0.0000     0.7850 0.00 1.00 0.00 0.00
#&gt; T72_CT_B05     4  0.4332     0.7163 0.16 0.04 0.00 0.80
#&gt; T72_CT_B06     2  0.0000     0.7850 0.00 1.00 0.00 0.00
#&gt; T72_CT_B08     3  0.3975     0.6810 0.00 0.00 0.76 0.24
#&gt; T72_CT_B09     2  0.0000     0.7850 0.00 1.00 0.00 0.00
#&gt; T72_CT_B11     4  0.3610     0.7260 0.00 0.20 0.00 0.80
#&gt; T72_CT_B12     1  0.4522     0.5747 0.68 0.00 0.00 0.32
#&gt; T72_CT_C04     1  0.0000     0.8795 1.00 0.00 0.00 0.00
#&gt; T72_CT_C06     4  0.3610     0.7260 0.00 0.20 0.00 0.80
#&gt; T72_CT_C07     1  0.0000     0.8795 1.00 0.00 0.00 0.00
#&gt; T72_CT_C09     4  0.3610     0.7260 0.00 0.20 0.00 0.80
#&gt; T72_CT_C11     3  0.3801     0.7096 0.00 0.00 0.78 0.22
#&gt; T72_CT_D01     1  0.4522     0.5747 0.68 0.00 0.00 0.32
#&gt; T72_CT_D03     2  0.4522     0.2801 0.00 0.68 0.00 0.32
#&gt; T72_CT_D04     1  0.4522     0.5747 0.68 0.00 0.00 0.32
#&gt; T72_CT_D05     4  0.3610     0.7260 0.00 0.20 0.00 0.80
#&gt; T72_CT_D07     4  0.3610     0.7260 0.00 0.20 0.00 0.80
#&gt; T72_CT_D10     4  0.4581     0.7311 0.12 0.08 0.00 0.80
#&gt; T72_CT_D11     2  0.0000     0.7850 0.00 1.00 0.00 0.00
#&gt; T72_CT_E04     4  0.3610     0.7260 0.00 0.20 0.00 0.80
#&gt; T72_CT_E05     1  0.4522     0.5747 0.68 0.00 0.00 0.32
#&gt; T72_CT_E07     1  0.4522     0.5747 0.68 0.00 0.00 0.32
#&gt; T72_CT_F01     1  0.4522     0.5747 0.68 0.00 0.00 0.32
#&gt; T72_CT_F05     1  0.4522     0.5747 0.68 0.00 0.00 0.32
#&gt; T72_CT_F07     4  0.3610     0.7260 0.00 0.20 0.00 0.80
#&gt; T72_CT_F10     1  0.4522     0.5747 0.68 0.00 0.00 0.32
#&gt; T72_CT_F11     1  0.7198     0.3600 0.52 0.00 0.16 0.32
#&gt; T72_CT_G03     2  0.0000     0.7850 0.00 1.00 0.00 0.00
#&gt; T72_CT_G04     2  0.0000     0.7850 0.00 1.00 0.00 0.00
#&gt; T72_CT_G06     1  0.7198     0.3792 0.52 0.00 0.16 0.32
#&gt; T72_CT_G08     4  0.4332     0.6538 0.00 0.04 0.16 0.80
#&gt; T72_CT_G10     1  0.4522     0.5747 0.68 0.00 0.00 0.32
#&gt; T72_CT_G11     4  0.4977     0.0415 0.00 0.46 0.00 0.54
#&gt; T72_CT_H01     1  0.0000     0.8795 1.00 0.00 0.00 0.00
#&gt; T72_CT_H03     1  0.4522     0.5747 0.68 0.00 0.00 0.32
#&gt; T72_CT_H05     3  0.0000     0.9413 0.00 0.00 1.00 0.00
#&gt; T72_CT_H08     3  0.0000     0.9413 0.00 0.00 1.00 0.00
#&gt; T72_CT_H09     3  0.0000     0.9413 0.00 0.00 1.00 0.00
#&gt; T72_CT_H10     4  0.3610     0.7260 0.00 0.20 0.00 0.80
#&gt; T72_CT_H11     4  0.3610     0.7260 0.00 0.20 0.00 0.80
#&gt; T72_CT_H12     1  0.4522     0.5747 0.68 0.00 0.00 0.32
</code></pre>

<script>
$('#tab-node-0-get-classes-3-a').parent().next().next().hide();
$('#tab-node-0-get-classes-3-a').click(function(){
  $('#tab-node-0-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.




<script>
$( function() {
	$( '#tabs-node-0-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-node-0-consensus-heatmap'>
<ul>
<li><a href='#tab-node-0-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-node-0-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-node-0-consensus-heatmap-3'>k = 4</a></li>
</ul>
<div id='tab-node-0-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-node-0-consensus-heatmap-1-1.png" alt="plot of chunk tab-node-0-consensus-heatmap-1"/></p>

</div>
<div id='tab-node-0-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-node-0-consensus-heatmap-2-1.png" alt="plot of chunk tab-node-0-consensus-heatmap-2"/></p>

</div>
<div id='tab-node-0-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-node-0-consensus-heatmap-3-1.png" alt="plot of chunk tab-node-0-consensus-heatmap-3"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:





<script>
$( function() {
	$( '#tabs-node-0-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-node-0-membership-heatmap'>
<ul>
<li><a href='#tab-node-0-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-node-0-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-node-0-membership-heatmap-3'>k = 4</a></li>
</ul>
<div id='tab-node-0-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-node-0-membership-heatmap-1-1.png" alt="plot of chunk tab-node-0-membership-heatmap-1"/></p>

</div>
<div id='tab-node-0-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-node-0-membership-heatmap-2-1.png" alt="plot of chunk tab-node-0-membership-heatmap-2"/></p>

</div>
<div id='tab-node-0-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-node-0-membership-heatmap-3-1.png" alt="plot of chunk tab-node-0-membership-heatmap-3"/></p>

</div>
</div>

As soon as the classes for columns are determined, the signatures
that are significantly different between subgroups can be looked for. 
Following are the heatmaps for signatures.




Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-node-0-get-signatures' ).tabs();
} );
</script>
<div id='tabs-node-0-get-signatures'>
<ul>
<li><a href='#tab-node-0-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-node-0-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-node-0-get-signatures-3'>k = 4</a></li>
</ul>
<div id='tab-node-0-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-node-0-get-signatures-1-1.png" alt="plot of chunk tab-node-0-get-signatures-1"/></p>

</div>
<div id='tab-node-0-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-node-0-get-signatures-2-1.png" alt="plot of chunk tab-node-0-get-signatures-2"/></p>

</div>
<div id='tab-node-0-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-node-0-get-signatures-3-1.png" alt="plot of chunk tab-node-0-get-signatures-3"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-node-0-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-node-0-get-signatures-no-scale'>
<ul>
<li><a href='#tab-node-0-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-node-0-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-node-0-get-signatures-no-scale-3'>k = 4</a></li>
</ul>
<div id='tab-node-0-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-node-0-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-node-0-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-node-0-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-node-0-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-node-0-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-node-0-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-node-0-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-node-0-get-signatures-no-scale-3"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk node-0-signature_compare](figure_cola/node-0-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. To get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows (which is done by automatically selecting number of clusters).

If there are too many signatures, `top_signatures = ...` can be set to only show the 
signatures with the highest FDRs:

```r
# code only for demonstration
# e.g. to show the top 500 most significant rows
tb = get_signature(res, k = ..., top_signatures = 500)
```

If the signatures are defined as these which are uniquely high in current group, `diff_method` argument
can be set to `"uniquely_high_in_one_group"`:

```r
# code only for demonstration
tb = get_signature(res, k = ..., diff_method = "uniquely_high_in_one_group")
```




UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-node-0-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-node-0-dimension-reduction'>
<ul>
<li><a href='#tab-node-0-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-node-0-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-node-0-dimension-reduction-3'>k = 4</a></li>
</ul>
<div id='tab-node-0-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-node-0-dimension-reduction-1-1.png" alt="plot of chunk tab-node-0-dimension-reduction-1"/></p>

</div>
<div id='tab-node-0-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-node-0-dimension-reduction-2-1.png" alt="plot of chunk tab-node-0-dimension-reduction-2"/></p>

</div>
<div id='tab-node-0-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-node-0-dimension-reduction-3-1.png" alt="plot of chunk tab-node-0-dimension-reduction-3"/></p>

</div>
</div>



Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk node-0-collect-classes](figure_cola/node-0-collect-classes-1.png)




Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n_sample Hours(p-value) Media(p-value) State(p-value) k
#> ATC:skmeans      269       1.24e-08       1.40e-09       5.87e-38 2
#> ATC:skmeans      268       6.57e-31       1.06e-33       1.12e-57 3
#> ATC:skmeans      235       8.32e-32       5.41e-28       2.16e-50 4
```




If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](https://jokergoo.github.io/cola_vignettes/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### Node01


Parent node: [Node0](#Node0).
Child nodes: 
                Node011-leaf
        ,
                [Node012](#Node012)
        ,
                Node021-leaf
        ,
                Node022-leaf
        ,
                Node023-leaf
        ,
                [Node031](#Node031)
        ,
                Node032-leaf
        .







The object with results only for a single top-value method and a single partitioning method 
can be extracted as:

```r
res = res_rh["01"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4.
#>   On a matrix with 11395 rows and 102 columns.
#>   Top rows (1140) are extracted by 'ATC' method.
#>   Subgroups are detected by 'skmeans' method.
#>   Performed in total 150 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_partitions"     
#>  [7] "compare_signatures"      "consensus_heatmap"       "dimension_reduction"    
#> [10] "functional_enrichment"   "get_anno_col"            "get_anno"               
#> [13] "get_classes"             "get_consensus"           "get_matrix"             
#> [16] "get_membership"          "get_param"               "get_signatures"         
#> [19] "get_stats"               "is_best_k"               "is_stable_k"            
#> [22] "membership_heatmap"      "ncol"                    "nrow"                   
#> [25] "plot_ecdf"               "predict_classes"         "rownames"               
#> [28] "select_partition_number" "show"                    "suggest_best_k"         
#> [31] "test_to_known_factors"   "top_rows_heatmap"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of subgroups)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk node-01-collect-plots](figure_cola/node-01-collect-plots-1.png)

The plots are:

- The first row: a plot of the eCDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- eCDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus subgroup labels in all
  partitions.
- Area increased. Denote $A_k$ as the area under the eCDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](https://jokergoo.github.io/cola_vignettes/cola.html#toc_13).

Generally speaking, higher 1-PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk node-01-select-partition-number](figure_cola/node-01-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.996       0.998          0.489 0.511   0.511
#> 3 3 0.786           0.884       0.943          0.342 0.679   0.454
#> 4 4 0.863           0.880       0.943          0.137 0.775   0.456
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following is the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall subgroup
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-node-01-get-classes' ).tabs();
} );
</script>
<div id='tabs-node-01-get-classes'>
<ul>
<li><a href='#tab-node-01-get-classes-1'>k = 2</a></li>
<li><a href='#tab-node-01-get-classes-2'>k = 3</a></li>
<li><a href='#tab-node-01-get-classes-3'>k = 4</a></li>
</ul>

<div id='tab-node-01-get-classes-1'>
<p><a id='tab-node-01-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette   p1   p2
#&gt; T0_CT_C05      1   0.000      1.000 1.00 0.00
#&gt; T0_CT_C06      1   0.000      1.000 1.00 0.00
#&gt; T0_CT_C09      1   0.000      1.000 1.00 0.00
#&gt; T0_CT_C11      1   0.000      1.000 1.00 0.00
#&gt; T0_CT_D09      1   0.000      1.000 1.00 0.00
#&gt; T0_CT_D11      1   0.000      1.000 1.00 0.00
#&gt; T0_CT_E04      1   0.000      1.000 1.00 0.00
#&gt; T0_CT_F09      1   0.000      1.000 1.00 0.00
#&gt; T24_CT_A03     1   0.000      1.000 1.00 0.00
#&gt; T24_CT_A04     1   0.000      1.000 1.00 0.00
#&gt; T24_CT_A07     1   0.000      1.000 1.00 0.00
#&gt; T24_CT_B03     2   0.000      0.996 0.00 1.00
#&gt; T24_CT_B05     1   0.000      1.000 1.00 0.00
#&gt; T24_CT_B06     1   0.000      1.000 1.00 0.00
#&gt; T24_CT_B08     1   0.000      1.000 1.00 0.00
#&gt; T24_CT_C02     1   0.000      1.000 1.00 0.00
#&gt; T24_CT_C05     1   0.000      1.000 1.00 0.00
#&gt; T24_CT_C07     1   0.000      1.000 1.00 0.00
#&gt; T24_CT_C08     1   0.000      1.000 1.00 0.00
#&gt; T24_CT_C09     1   0.000      1.000 1.00 0.00
#&gt; T24_CT_C12     1   0.000      1.000 1.00 0.00
#&gt; T24_CT_D01     1   0.000      1.000 1.00 0.00
#&gt; T24_CT_D02     1   0.000      1.000 1.00 0.00
#&gt; T24_CT_D03     1   0.000      1.000 1.00 0.00
#&gt; T24_CT_D08     1   0.000      1.000 1.00 0.00
#&gt; T24_CT_D09     1   0.000      1.000 1.00 0.00
#&gt; T24_CT_D10     1   0.000      1.000 1.00 0.00
#&gt; T24_CT_D11     1   0.000      1.000 1.00 0.00
#&gt; T24_CT_E04     2   0.242      0.959 0.04 0.96
#&gt; T24_CT_E07     2   0.000      0.996 0.00 1.00
#&gt; T24_CT_E12     1   0.000      1.000 1.00 0.00
#&gt; T24_CT_F01     1   0.000      1.000 1.00 0.00
#&gt; T24_CT_F02     1   0.000      1.000 1.00 0.00
#&gt; T24_CT_F09     1   0.000      1.000 1.00 0.00
#&gt; T24_CT_G01     1   0.000      1.000 1.00 0.00
#&gt; T24_CT_G04     1   0.000      1.000 1.00 0.00
#&gt; T24_CT_G05     1   0.000      1.000 1.00 0.00
#&gt; T24_CT_G10     1   0.000      1.000 1.00 0.00
#&gt; T24_CT_H03     1   0.000      1.000 1.00 0.00
#&gt; T48_CT_A01     2   0.000      0.996 0.00 1.00
#&gt; T48_CT_A02     2   0.000      0.996 0.00 1.00
#&gt; T48_CT_A04     1   0.000      1.000 1.00 0.00
#&gt; T48_CT_A05     2   0.000      0.996 0.00 1.00
#&gt; T48_CT_A08     2   0.000      0.996 0.00 1.00
#&gt; T48_CT_A10     1   0.000      1.000 1.00 0.00
#&gt; T48_CT_A11     1   0.000      1.000 1.00 0.00
#&gt; T48_CT_B01     1   0.000      1.000 1.00 0.00
#&gt; T48_CT_B02     1   0.000      1.000 1.00 0.00
#&gt; T48_CT_B06     1   0.000      1.000 1.00 0.00
#&gt; T48_CT_B10     1   0.000      1.000 1.00 0.00
#&gt; T48_CT_B11     2   0.402      0.916 0.08 0.92
#&gt; T48_CT_C01     1   0.000      1.000 1.00 0.00
#&gt; T48_CT_C03     1   0.000      1.000 1.00 0.00
#&gt; T48_CT_C05     2   0.000      0.996 0.00 1.00
#&gt; T48_CT_C06     2   0.000      0.996 0.00 1.00
#&gt; T48_CT_C09     2   0.000      0.996 0.00 1.00
#&gt; T48_CT_C11     2   0.000      0.996 0.00 1.00
#&gt; T48_CT_D01     1   0.000      1.000 1.00 0.00
#&gt; T48_CT_D04     1   0.000      1.000 1.00 0.00
#&gt; T48_CT_D06     2   0.000      0.996 0.00 1.00
#&gt; T48_CT_D07     1   0.000      1.000 1.00 0.00
#&gt; T48_CT_D09     1   0.000      1.000 1.00 0.00
#&gt; T48_CT_D11     2   0.000      0.996 0.00 1.00
#&gt; T48_CT_E01     2   0.000      0.996 0.00 1.00
#&gt; T48_CT_E03     2   0.000      0.996 0.00 1.00
#&gt; T48_CT_E05     1   0.000      1.000 1.00 0.00
#&gt; T48_CT_E08     1   0.000      1.000 1.00 0.00
#&gt; T48_CT_F01     1   0.000      1.000 1.00 0.00
#&gt; T48_CT_F05     1   0.000      1.000 1.00 0.00
#&gt; T48_CT_F09     1   0.000      1.000 1.00 0.00
#&gt; T48_CT_G02     2   0.000      0.996 0.00 1.00
#&gt; T48_CT_G03     1   0.000      1.000 1.00 0.00
#&gt; T48_CT_G07     1   0.000      1.000 1.00 0.00
#&gt; T48_CT_G08     2   0.000      0.996 0.00 1.00
#&gt; T48_CT_G09     2   0.000      0.996 0.00 1.00
#&gt; T48_CT_G12     1   0.000      1.000 1.00 0.00
#&gt; T48_CT_H01     1   0.000      1.000 1.00 0.00
#&gt; T48_CT_H05     1   0.000      1.000 1.00 0.00
#&gt; T48_CT_H06     1   0.000      1.000 1.00 0.00
#&gt; T48_CT_H08     2   0.000      0.996 0.00 1.00
#&gt; T48_CT_H12     2   0.327      0.938 0.06 0.94
#&gt; T72_CT_A01     2   0.000      0.996 0.00 1.00
#&gt; T72_CT_A05     2   0.000      0.996 0.00 1.00
#&gt; T72_CT_A09     2   0.000      0.996 0.00 1.00
#&gt; T72_CT_A11     2   0.000      0.996 0.00 1.00
#&gt; T72_CT_B01     2   0.000      0.996 0.00 1.00
#&gt; T72_CT_B12     2   0.000      0.996 0.00 1.00
#&gt; T72_CT_C04     2   0.000      0.996 0.00 1.00
#&gt; T72_CT_C07     2   0.000      0.996 0.00 1.00
#&gt; T72_CT_D01     2   0.000      0.996 0.00 1.00
#&gt; T72_CT_D04     2   0.000      0.996 0.00 1.00
#&gt; T72_CT_E05     2   0.000      0.996 0.00 1.00
#&gt; T72_CT_E07     2   0.000      0.996 0.00 1.00
#&gt; T72_CT_F01     2   0.000      0.996 0.00 1.00
#&gt; T72_CT_F05     2   0.000      0.996 0.00 1.00
#&gt; T72_CT_F10     2   0.000      0.996 0.00 1.00
#&gt; T72_CT_F11     2   0.000      0.996 0.00 1.00
#&gt; T72_CT_G06     2   0.000      0.996 0.00 1.00
#&gt; T72_CT_G10     2   0.000      0.996 0.00 1.00
#&gt; T72_CT_H01     2   0.000      0.996 0.00 1.00
#&gt; T72_CT_H03     2   0.000      0.996 0.00 1.00
#&gt; T72_CT_H12     2   0.000      0.996 0.00 1.00
</code></pre>

<script>
$('#tab-node-01-get-classes-1-a').parent().next().next().hide();
$('#tab-node-01-get-classes-1-a').click(function(){
  $('#tab-node-01-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-node-01-get-classes-2'>
<p><a id='tab-node-01-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette   p1   p2   p3
#&gt; T0_CT_C05      1  0.0000      0.965 1.00 0.00 0.00
#&gt; T0_CT_C06      1  0.0000      0.965 1.00 0.00 0.00
#&gt; T0_CT_C09      1  0.0000      0.965 1.00 0.00 0.00
#&gt; T0_CT_C11      1  0.0000      0.965 1.00 0.00 0.00
#&gt; T0_CT_D09      1  0.0000      0.965 1.00 0.00 0.00
#&gt; T0_CT_D11      1  0.0000      0.965 1.00 0.00 0.00
#&gt; T0_CT_E04      1  0.0000      0.965 1.00 0.00 0.00
#&gt; T0_CT_F09      1  0.0000      0.965 1.00 0.00 0.00
#&gt; T24_CT_A03     1  0.0000      0.965 1.00 0.00 0.00
#&gt; T24_CT_A04     1  0.0000      0.965 1.00 0.00 0.00
#&gt; T24_CT_A07     1  0.0000      0.965 1.00 0.00 0.00
#&gt; T24_CT_B03     3  0.2537      0.859 0.00 0.08 0.92
#&gt; T24_CT_B05     3  0.1529      0.871 0.04 0.00 0.96
#&gt; T24_CT_B06     1  0.3686      0.856 0.86 0.00 0.14
#&gt; T24_CT_B08     1  0.0000      0.965 1.00 0.00 0.00
#&gt; T24_CT_C02     1  0.0000      0.965 1.00 0.00 0.00
#&gt; T24_CT_C05     1  0.0000      0.965 1.00 0.00 0.00
#&gt; T24_CT_C07     3  0.0000      0.884 0.00 0.00 1.00
#&gt; T24_CT_C08     1  0.0000      0.965 1.00 0.00 0.00
#&gt; T24_CT_C09     1  0.0000      0.965 1.00 0.00 0.00
#&gt; T24_CT_C12     3  0.0000      0.884 0.00 0.00 1.00
#&gt; T24_CT_D01     3  0.5560      0.579 0.30 0.00 0.70
#&gt; T24_CT_D02     3  0.5948      0.471 0.36 0.00 0.64
#&gt; T24_CT_D03     1  0.0000      0.965 1.00 0.00 0.00
#&gt; T24_CT_D08     1  0.0000      0.965 1.00 0.00 0.00
#&gt; T24_CT_D09     1  0.0000      0.965 1.00 0.00 0.00
#&gt; T24_CT_D10     1  0.0000      0.965 1.00 0.00 0.00
#&gt; T24_CT_D11     1  0.2066      0.932 0.94 0.00 0.06
#&gt; T24_CT_E04     3  0.0000      0.884 0.00 0.00 1.00
#&gt; T24_CT_E07     3  0.0000      0.884 0.00 0.00 1.00
#&gt; T24_CT_E12     1  0.2537      0.914 0.92 0.00 0.08
#&gt; T24_CT_F01     3  0.2959      0.836 0.10 0.00 0.90
#&gt; T24_CT_F02     1  0.0000      0.965 1.00 0.00 0.00
#&gt; T24_CT_F09     1  0.0892      0.951 0.98 0.02 0.00
#&gt; T24_CT_G01     1  0.0000      0.965 1.00 0.00 0.00
#&gt; T24_CT_G04     1  0.5706      0.488 0.68 0.00 0.32
#&gt; T24_CT_G05     1  0.0000      0.965 1.00 0.00 0.00
#&gt; T24_CT_G10     3  0.0000      0.884 0.00 0.00 1.00
#&gt; T24_CT_H03     1  0.0892      0.955 0.98 0.00 0.02
#&gt; T48_CT_A01     3  0.6126      0.393 0.00 0.40 0.60
#&gt; T48_CT_A02     3  0.0000      0.884 0.00 0.00 1.00
#&gt; T48_CT_A04     1  0.2537      0.914 0.92 0.00 0.08
#&gt; T48_CT_A05     3  0.2959      0.849 0.00 0.10 0.90
#&gt; T48_CT_A08     3  0.2537      0.859 0.00 0.08 0.92
#&gt; T48_CT_A10     1  0.2537      0.914 0.92 0.00 0.08
#&gt; T48_CT_A11     3  0.6126      0.348 0.40 0.00 0.60
#&gt; T48_CT_B01     1  0.0000      0.965 1.00 0.00 0.00
#&gt; T48_CT_B02     3  0.4291      0.761 0.18 0.00 0.82
#&gt; T48_CT_B06     1  0.0000      0.965 1.00 0.00 0.00
#&gt; T48_CT_B10     3  0.0000      0.884 0.00 0.00 1.00
#&gt; T48_CT_B11     3  0.0000      0.884 0.00 0.00 1.00
#&gt; T48_CT_C01     3  0.5835      0.478 0.34 0.00 0.66
#&gt; T48_CT_C03     3  0.0000      0.884 0.00 0.00 1.00
#&gt; T48_CT_C05     3  0.3340      0.836 0.00 0.12 0.88
#&gt; T48_CT_C06     3  0.2537      0.859 0.00 0.08 0.92
#&gt; T48_CT_C09     3  0.4555      0.757 0.00 0.20 0.80
#&gt; T48_CT_C11     3  0.0892      0.880 0.00 0.02 0.98
#&gt; T48_CT_D01     3  0.0000      0.884 0.00 0.00 1.00
#&gt; T48_CT_D04     1  0.0000      0.965 1.00 0.00 0.00
#&gt; T48_CT_D06     3  0.3686      0.820 0.00 0.14 0.86
#&gt; T48_CT_D07     3  0.0000      0.884 0.00 0.00 1.00
#&gt; T48_CT_D09     3  0.0892      0.878 0.02 0.00 0.98
#&gt; T48_CT_D11     2  0.5706      0.532 0.32 0.68 0.00
#&gt; T48_CT_E01     3  0.3340      0.836 0.00 0.12 0.88
#&gt; T48_CT_E03     3  0.3340      0.836 0.00 0.12 0.88
#&gt; T48_CT_E05     3  0.0000      0.884 0.00 0.00 1.00
#&gt; T48_CT_E08     1  0.1529      0.942 0.96 0.00 0.04
#&gt; T48_CT_F01     3  0.2959      0.836 0.10 0.00 0.90
#&gt; T48_CT_F05     3  0.5397      0.616 0.28 0.00 0.72
#&gt; T48_CT_F09     3  0.0000      0.884 0.00 0.00 1.00
#&gt; T48_CT_G02     3  0.4002      0.801 0.00 0.16 0.84
#&gt; T48_CT_G03     3  0.0892      0.878 0.02 0.00 0.98
#&gt; T48_CT_G07     1  0.3340      0.876 0.88 0.00 0.12
#&gt; T48_CT_G08     3  0.2537      0.859 0.00 0.08 0.92
#&gt; T48_CT_G09     3  0.2959      0.849 0.00 0.10 0.90
#&gt; T48_CT_G12     1  0.4555      0.772 0.80 0.00 0.20
#&gt; T48_CT_H01     1  0.2959      0.897 0.90 0.00 0.10
#&gt; T48_CT_H05     1  0.0000      0.965 1.00 0.00 0.00
#&gt; T48_CT_H06     3  0.2537      0.850 0.08 0.00 0.92
#&gt; T48_CT_H08     2  0.4796      0.681 0.00 0.78 0.22
#&gt; T48_CT_H12     3  0.0000      0.884 0.00 0.00 1.00
#&gt; T72_CT_A01     2  0.0000      0.973 0.00 1.00 0.00
#&gt; T72_CT_A05     2  0.0000      0.973 0.00 1.00 0.00
#&gt; T72_CT_A09     2  0.0000      0.973 0.00 1.00 0.00
#&gt; T72_CT_A11     2  0.0000      0.973 0.00 1.00 0.00
#&gt; T72_CT_B01     2  0.0000      0.973 0.00 1.00 0.00
#&gt; T72_CT_B12     2  0.0000      0.973 0.00 1.00 0.00
#&gt; T72_CT_C04     2  0.0000      0.973 0.00 1.00 0.00
#&gt; T72_CT_C07     2  0.0000      0.973 0.00 1.00 0.00
#&gt; T72_CT_D01     2  0.0000      0.973 0.00 1.00 0.00
#&gt; T72_CT_D04     2  0.0000      0.973 0.00 1.00 0.00
#&gt; T72_CT_E05     2  0.0000      0.973 0.00 1.00 0.00
#&gt; T72_CT_E07     2  0.0000      0.973 0.00 1.00 0.00
#&gt; T72_CT_F01     2  0.0000      0.973 0.00 1.00 0.00
#&gt; T72_CT_F05     2  0.0000      0.973 0.00 1.00 0.00
#&gt; T72_CT_F10     2  0.0000      0.973 0.00 1.00 0.00
#&gt; T72_CT_F11     2  0.0000      0.973 0.00 1.00 0.00
#&gt; T72_CT_G06     2  0.0000      0.973 0.00 1.00 0.00
#&gt; T72_CT_G10     2  0.0000      0.973 0.00 1.00 0.00
#&gt; T72_CT_H01     2  0.0000      0.973 0.00 1.00 0.00
#&gt; T72_CT_H03     2  0.0000      0.973 0.00 1.00 0.00
#&gt; T72_CT_H12     2  0.0000      0.973 0.00 1.00 0.00
</code></pre>

<script>
$('#tab-node-01-get-classes-2-a').parent().next().next().hide();
$('#tab-node-01-get-classes-2-a').click(function(){
  $('#tab-node-01-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-node-01-get-classes-3'>
<p><a id='tab-node-01-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette   p1   p2   p3   p4
#&gt; T0_CT_C05      1  0.0000     0.9401 1.00 0.00 0.00 0.00
#&gt; T0_CT_C06      1  0.0000     0.9401 1.00 0.00 0.00 0.00
#&gt; T0_CT_C09      3  0.4907     0.3709 0.42 0.00 0.58 0.00
#&gt; T0_CT_C11      1  0.0000     0.9401 1.00 0.00 0.00 0.00
#&gt; T0_CT_D09      1  0.0000     0.9401 1.00 0.00 0.00 0.00
#&gt; T0_CT_D11      1  0.0000     0.9401 1.00 0.00 0.00 0.00
#&gt; T0_CT_E04      1  0.0000     0.9401 1.00 0.00 0.00 0.00
#&gt; T0_CT_F09      1  0.0000     0.9401 1.00 0.00 0.00 0.00
#&gt; T24_CT_A03     1  0.0000     0.9401 1.00 0.00 0.00 0.00
#&gt; T24_CT_A04     1  0.0000     0.9401 1.00 0.00 0.00 0.00
#&gt; T24_CT_A07     3  0.4134     0.6800 0.26 0.00 0.74 0.00
#&gt; T24_CT_B03     4  0.0000     0.9840 0.00 0.00 0.00 1.00
#&gt; T24_CT_B05     3  0.0707     0.8699 0.00 0.00 0.98 0.02
#&gt; T24_CT_B06     3  0.1637     0.8551 0.06 0.00 0.94 0.00
#&gt; T24_CT_B08     1  0.0000     0.9401 1.00 0.00 0.00 0.00
#&gt; T24_CT_C02     3  0.4907     0.3696 0.42 0.00 0.58 0.00
#&gt; T24_CT_C05     1  0.1211     0.9112 0.96 0.00 0.04 0.00
#&gt; T24_CT_C07     3  0.2345     0.8335 0.00 0.00 0.90 0.10
#&gt; T24_CT_C08     1  0.0000     0.9401 1.00 0.00 0.00 0.00
#&gt; T24_CT_C09     3  0.3801     0.7321 0.22 0.00 0.78 0.00
#&gt; T24_CT_C12     3  0.4855     0.3822 0.00 0.00 0.60 0.40
#&gt; T24_CT_D01     3  0.0000     0.8740 0.00 0.00 1.00 0.00
#&gt; T24_CT_D02     3  0.4949     0.7255 0.18 0.00 0.76 0.06
#&gt; T24_CT_D03     1  0.0000     0.9401 1.00 0.00 0.00 0.00
#&gt; T24_CT_D08     1  0.4790     0.2868 0.62 0.00 0.38 0.00
#&gt; T24_CT_D09     1  0.0000     0.9401 1.00 0.00 0.00 0.00
#&gt; T24_CT_D10     1  0.0000     0.9401 1.00 0.00 0.00 0.00
#&gt; T24_CT_D11     3  0.3400     0.7773 0.18 0.00 0.82 0.00
#&gt; T24_CT_E04     4  0.2647     0.8763 0.00 0.00 0.12 0.88
#&gt; T24_CT_E07     4  0.0000     0.9840 0.00 0.00 0.00 1.00
#&gt; T24_CT_E12     3  0.2345     0.8334 0.10 0.00 0.90 0.00
#&gt; T24_CT_F01     3  0.0707     0.8699 0.00 0.00 0.98 0.02
#&gt; T24_CT_F02     1  0.0000     0.9401 1.00 0.00 0.00 0.00
#&gt; T24_CT_F09     1  0.0000     0.9401 1.00 0.00 0.00 0.00
#&gt; T24_CT_G01     1  0.0000     0.9401 1.00 0.00 0.00 0.00
#&gt; T24_CT_G04     1  0.4731     0.7337 0.78 0.00 0.06 0.16
#&gt; T24_CT_G05     1  0.1211     0.9108 0.96 0.00 0.04 0.00
#&gt; T24_CT_G10     3  0.4522     0.5530 0.00 0.00 0.68 0.32
#&gt; T24_CT_H03     3  0.2011     0.8464 0.08 0.00 0.92 0.00
#&gt; T48_CT_A01     4  0.0000     0.9840 0.00 0.00 0.00 1.00
#&gt; T48_CT_A02     4  0.0000     0.9840 0.00 0.00 0.00 1.00
#&gt; T48_CT_A04     3  0.3610     0.7522 0.20 0.00 0.80 0.00
#&gt; T48_CT_A05     4  0.0000     0.9840 0.00 0.00 0.00 1.00
#&gt; T48_CT_A08     4  0.0000     0.9840 0.00 0.00 0.00 1.00
#&gt; T48_CT_A10     3  0.0000     0.8740 0.00 0.00 1.00 0.00
#&gt; T48_CT_A11     3  0.0707     0.8694 0.02 0.00 0.98 0.00
#&gt; T48_CT_B01     3  0.4790     0.4657 0.38 0.00 0.62 0.00
#&gt; T48_CT_B02     3  0.0000     0.8740 0.00 0.00 1.00 0.00
#&gt; T48_CT_B06     1  0.1637     0.8923 0.94 0.00 0.06 0.00
#&gt; T48_CT_B10     3  0.0000     0.8740 0.00 0.00 1.00 0.00
#&gt; T48_CT_B11     4  0.1211     0.9563 0.00 0.00 0.04 0.96
#&gt; T48_CT_C01     3  0.5767     0.5599 0.06 0.00 0.66 0.28
#&gt; T48_CT_C03     3  0.1211     0.8619 0.00 0.00 0.96 0.04
#&gt; T48_CT_C05     4  0.0000     0.9840 0.00 0.00 0.00 1.00
#&gt; T48_CT_C06     4  0.0000     0.9840 0.00 0.00 0.00 1.00
#&gt; T48_CT_C09     4  0.0000     0.9840 0.00 0.00 0.00 1.00
#&gt; T48_CT_C11     4  0.0707     0.9707 0.00 0.00 0.02 0.98
#&gt; T48_CT_D01     3  0.1211     0.8637 0.00 0.00 0.96 0.04
#&gt; T48_CT_D04     1  0.0707     0.9271 0.98 0.00 0.00 0.02
#&gt; T48_CT_D06     4  0.0000     0.9840 0.00 0.00 0.00 1.00
#&gt; T48_CT_D07     3  0.2647     0.8053 0.00 0.00 0.88 0.12
#&gt; T48_CT_D09     3  0.0000     0.8740 0.00 0.00 1.00 0.00
#&gt; T48_CT_D11     1  0.1637     0.8893 0.94 0.06 0.00 0.00
#&gt; T48_CT_E01     4  0.0000     0.9840 0.00 0.00 0.00 1.00
#&gt; T48_CT_E03     4  0.0000     0.9840 0.00 0.00 0.00 1.00
#&gt; T48_CT_E05     3  0.0000     0.8740 0.00 0.00 1.00 0.00
#&gt; T48_CT_E08     1  0.4994    -0.0164 0.52 0.00 0.48 0.00
#&gt; T48_CT_F01     3  0.0000     0.8740 0.00 0.00 1.00 0.00
#&gt; T48_CT_F05     3  0.0000     0.8740 0.00 0.00 1.00 0.00
#&gt; T48_CT_F09     3  0.0000     0.8740 0.00 0.00 1.00 0.00
#&gt; T48_CT_G02     4  0.0000     0.9840 0.00 0.00 0.00 1.00
#&gt; T48_CT_G03     3  0.0000     0.8740 0.00 0.00 1.00 0.00
#&gt; T48_CT_G07     3  0.0000     0.8740 0.00 0.00 1.00 0.00
#&gt; T48_CT_G08     4  0.0000     0.9840 0.00 0.00 0.00 1.00
#&gt; T48_CT_G09     4  0.0000     0.9840 0.00 0.00 0.00 1.00
#&gt; T48_CT_G12     3  0.0000     0.8740 0.00 0.00 1.00 0.00
#&gt; T48_CT_H01     3  0.0000     0.8740 0.00 0.00 1.00 0.00
#&gt; T48_CT_H05     3  0.3801     0.7322 0.22 0.00 0.78 0.00
#&gt; T48_CT_H06     3  0.0000     0.8740 0.00 0.00 1.00 0.00
#&gt; T48_CT_H08     4  0.1637     0.9316 0.00 0.06 0.00 0.94
#&gt; T48_CT_H12     4  0.1637     0.9392 0.00 0.00 0.06 0.94
#&gt; T72_CT_A01     2  0.0000     1.0000 0.00 1.00 0.00 0.00
#&gt; T72_CT_A05     2  0.0000     1.0000 0.00 1.00 0.00 0.00
#&gt; T72_CT_A09     2  0.0000     1.0000 0.00 1.00 0.00 0.00
#&gt; T72_CT_A11     2  0.0000     1.0000 0.00 1.00 0.00 0.00
#&gt; T72_CT_B01     2  0.0000     1.0000 0.00 1.00 0.00 0.00
#&gt; T72_CT_B12     2  0.0000     1.0000 0.00 1.00 0.00 0.00
#&gt; T72_CT_C04     2  0.0000     1.0000 0.00 1.00 0.00 0.00
#&gt; T72_CT_C07     2  0.0000     1.0000 0.00 1.00 0.00 0.00
#&gt; T72_CT_D01     2  0.0000     1.0000 0.00 1.00 0.00 0.00
#&gt; T72_CT_D04     2  0.0000     1.0000 0.00 1.00 0.00 0.00
#&gt; T72_CT_E05     2  0.0000     1.0000 0.00 1.00 0.00 0.00
#&gt; T72_CT_E07     2  0.0000     1.0000 0.00 1.00 0.00 0.00
#&gt; T72_CT_F01     2  0.0000     1.0000 0.00 1.00 0.00 0.00
#&gt; T72_CT_F05     2  0.0000     1.0000 0.00 1.00 0.00 0.00
#&gt; T72_CT_F10     2  0.0000     1.0000 0.00 1.00 0.00 0.00
#&gt; T72_CT_F11     2  0.0000     1.0000 0.00 1.00 0.00 0.00
#&gt; T72_CT_G06     2  0.0000     1.0000 0.00 1.00 0.00 0.00
#&gt; T72_CT_G10     2  0.0000     1.0000 0.00 1.00 0.00 0.00
#&gt; T72_CT_H01     2  0.0000     1.0000 0.00 1.00 0.00 0.00
#&gt; T72_CT_H03     2  0.0000     1.0000 0.00 1.00 0.00 0.00
#&gt; T72_CT_H12     2  0.0000     1.0000 0.00 1.00 0.00 0.00
</code></pre>

<script>
$('#tab-node-01-get-classes-3-a').parent().next().next().hide();
$('#tab-node-01-get-classes-3-a').click(function(){
  $('#tab-node-01-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.




<script>
$( function() {
	$( '#tabs-node-01-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-node-01-consensus-heatmap'>
<ul>
<li><a href='#tab-node-01-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-node-01-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-node-01-consensus-heatmap-3'>k = 4</a></li>
</ul>
<div id='tab-node-01-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-node-01-consensus-heatmap-1-1.png" alt="plot of chunk tab-node-01-consensus-heatmap-1"/></p>

</div>
<div id='tab-node-01-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-node-01-consensus-heatmap-2-1.png" alt="plot of chunk tab-node-01-consensus-heatmap-2"/></p>

</div>
<div id='tab-node-01-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-node-01-consensus-heatmap-3-1.png" alt="plot of chunk tab-node-01-consensus-heatmap-3"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:





<script>
$( function() {
	$( '#tabs-node-01-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-node-01-membership-heatmap'>
<ul>
<li><a href='#tab-node-01-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-node-01-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-node-01-membership-heatmap-3'>k = 4</a></li>
</ul>
<div id='tab-node-01-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-node-01-membership-heatmap-1-1.png" alt="plot of chunk tab-node-01-membership-heatmap-1"/></p>

</div>
<div id='tab-node-01-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-node-01-membership-heatmap-2-1.png" alt="plot of chunk tab-node-01-membership-heatmap-2"/></p>

</div>
<div id='tab-node-01-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-node-01-membership-heatmap-3-1.png" alt="plot of chunk tab-node-01-membership-heatmap-3"/></p>

</div>
</div>

As soon as the classes for columns are determined, the signatures
that are significantly different between subgroups can be looked for. 
Following are the heatmaps for signatures.




Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-node-01-get-signatures' ).tabs();
} );
</script>
<div id='tabs-node-01-get-signatures'>
<ul>
<li><a href='#tab-node-01-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-node-01-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-node-01-get-signatures-3'>k = 4</a></li>
</ul>
<div id='tab-node-01-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-node-01-get-signatures-1-1.png" alt="plot of chunk tab-node-01-get-signatures-1"/></p>

</div>
<div id='tab-node-01-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-node-01-get-signatures-2-1.png" alt="plot of chunk tab-node-01-get-signatures-2"/></p>

</div>
<div id='tab-node-01-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-node-01-get-signatures-3-1.png" alt="plot of chunk tab-node-01-get-signatures-3"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-node-01-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-node-01-get-signatures-no-scale'>
<ul>
<li><a href='#tab-node-01-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-node-01-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-node-01-get-signatures-no-scale-3'>k = 4</a></li>
</ul>
<div id='tab-node-01-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-node-01-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-node-01-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-node-01-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-node-01-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-node-01-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-node-01-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-node-01-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-node-01-get-signatures-no-scale-3"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk node-01-signature_compare](figure_cola/node-01-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. To get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows (which is done by automatically selecting number of clusters).

If there are too many signatures, `top_signatures = ...` can be set to only show the 
signatures with the highest FDRs:

```r
# code only for demonstration
# e.g. to show the top 500 most significant rows
tb = get_signature(res, k = ..., top_signatures = 500)
```

If the signatures are defined as these which are uniquely high in current group, `diff_method` argument
can be set to `"uniquely_high_in_one_group"`:

```r
# code only for demonstration
tb = get_signature(res, k = ..., diff_method = "uniquely_high_in_one_group")
```




UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-node-01-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-node-01-dimension-reduction'>
<ul>
<li><a href='#tab-node-01-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-node-01-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-node-01-dimension-reduction-3'>k = 4</a></li>
</ul>
<div id='tab-node-01-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-node-01-dimension-reduction-1-1.png" alt="plot of chunk tab-node-01-dimension-reduction-1"/></p>

</div>
<div id='tab-node-01-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-node-01-dimension-reduction-2-1.png" alt="plot of chunk tab-node-01-dimension-reduction-2"/></p>

</div>
<div id='tab-node-01-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-node-01-dimension-reduction-3-1.png" alt="plot of chunk tab-node-01-dimension-reduction-3"/></p>

</div>
</div>



Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk node-01-collect-classes](figure_cola/node-01-collect-classes-1.png)




Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n_sample Hours(p-value) Media(p-value) State(p-value) k
#> ATC:skmeans      102       1.80e-10       3.65e-02       5.63e-04 2
#> ATC:skmeans       97       7.53e-22       1.15e-03       3.56e-05 3
#> ATC:skmeans       96       4.29e-25       2.54e-05       8.48e-07 4
```




If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](https://jokergoo.github.io/cola_vignettes/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### Node012


Parent node: [Node01](#Node01).
Child nodes: 
                Node0121-leaf
        ,
                Node0122-leaf
        ,
                Node0123-leaf
        ,
                Node0311-leaf
        ,
                Node0312-leaf
        ,
                Node0313-leaf
        .







The object with results only for a single top-value method and a single partitioning method 
can be extracted as:

```r
res = res_rh["012"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4.
#>   On a matrix with 11362 rows and 42 columns.
#>   Top rows (932) are extracted by 'ATC' method.
#>   Subgroups are detected by 'skmeans' method.
#>   Performed in total 150 partitions by row resampling.
#>   Best k for subgroups seems to be 4.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_partitions"     
#>  [7] "compare_signatures"      "consensus_heatmap"       "dimension_reduction"    
#> [10] "functional_enrichment"   "get_anno_col"            "get_anno"               
#> [13] "get_classes"             "get_consensus"           "get_matrix"             
#> [16] "get_membership"          "get_param"               "get_signatures"         
#> [19] "get_stats"               "is_best_k"               "is_stable_k"            
#> [22] "membership_heatmap"      "ncol"                    "nrow"                   
#> [25] "plot_ecdf"               "predict_classes"         "rownames"               
#> [28] "select_partition_number" "show"                    "suggest_best_k"         
#> [31] "test_to_known_factors"   "top_rows_heatmap"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of subgroups)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk node-012-collect-plots](figure_cola/node-012-collect-plots-1.png)

The plots are:

- The first row: a plot of the eCDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- eCDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus subgroup labels in all
  partitions.
- Area increased. Denote $A_k$ as the area under the eCDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](https://jokergoo.github.io/cola_vignettes/cola.html#toc_13).

Generally speaking, higher 1-PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk node-012-select-partition-number](figure_cola/node-012-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           1.000       1.000          0.512 0.489   0.489
#> 3 3 1.000           0.977       0.990          0.265 0.813   0.637
#> 4 4 0.985           0.943       0.966          0.163 0.863   0.627
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 4
#> attr(,"optional")
#> [1] 2 3
```

There is also optional best $k$ = 2 3 that is worth to check.

Following is the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall subgroup
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-node-012-get-classes' ).tabs();
} );
</script>
<div id='tabs-node-012-get-classes'>
<ul>
<li><a href='#tab-node-012-get-classes-1'>k = 2</a></li>
<li><a href='#tab-node-012-get-classes-2'>k = 3</a></li>
<li><a href='#tab-node-012-get-classes-3'>k = 4</a></li>
</ul>

<div id='tab-node-012-get-classes-1'>
<p><a id='tab-node-012-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1 p2
#&gt; T24_CT_B03     2       0          1  0  1
#&gt; T24_CT_E04     2       0          1  0  1
#&gt; T24_CT_E07     2       0          1  0  1
#&gt; T48_CT_A01     2       0          1  0  1
#&gt; T48_CT_A02     2       0          1  0  1
#&gt; T48_CT_A05     2       0          1  0  1
#&gt; T48_CT_A08     2       0          1  0  1
#&gt; T48_CT_B11     2       0          1  0  1
#&gt; T48_CT_C05     2       0          1  0  1
#&gt; T48_CT_C06     2       0          1  0  1
#&gt; T48_CT_C09     2       0          1  0  1
#&gt; T48_CT_C11     2       0          1  0  1
#&gt; T48_CT_D06     2       0          1  0  1
#&gt; T48_CT_D11     1       0          1  1  0
#&gt; T48_CT_E01     2       0          1  0  1
#&gt; T48_CT_E03     2       0          1  0  1
#&gt; T48_CT_G02     2       0          1  0  1
#&gt; T48_CT_G08     2       0          1  0  1
#&gt; T48_CT_G09     2       0          1  0  1
#&gt; T48_CT_H08     2       0          1  0  1
#&gt; T48_CT_H12     2       0          1  0  1
#&gt; T72_CT_A01     1       0          1  1  0
#&gt; T72_CT_A05     1       0          1  1  0
#&gt; T72_CT_A09     1       0          1  1  0
#&gt; T72_CT_A11     1       0          1  1  0
#&gt; T72_CT_B01     1       0          1  1  0
#&gt; T72_CT_B12     1       0          1  1  0
#&gt; T72_CT_C04     1       0          1  1  0
#&gt; T72_CT_C07     1       0          1  1  0
#&gt; T72_CT_D01     1       0          1  1  0
#&gt; T72_CT_D04     1       0          1  1  0
#&gt; T72_CT_E05     1       0          1  1  0
#&gt; T72_CT_E07     1       0          1  1  0
#&gt; T72_CT_F01     1       0          1  1  0
#&gt; T72_CT_F05     1       0          1  1  0
#&gt; T72_CT_F10     1       0          1  1  0
#&gt; T72_CT_F11     1       0          1  1  0
#&gt; T72_CT_G06     1       0          1  1  0
#&gt; T72_CT_G10     1       0          1  1  0
#&gt; T72_CT_H01     1       0          1  1  0
#&gt; T72_CT_H03     1       0          1  1  0
#&gt; T72_CT_H12     1       0          1  1  0
</code></pre>

<script>
$('#tab-node-012-get-classes-1-a').parent().next().next().hide();
$('#tab-node-012-get-classes-1-a').click(function(){
  $('#tab-node-012-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-node-012-get-classes-2'>
<p><a id='tab-node-012-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette   p1   p2   p3
#&gt; T24_CT_B03     3   0.000      0.992 0.00 0.00 1.00
#&gt; T24_CT_E04     2   0.000      0.966 0.00 1.00 0.00
#&gt; T24_CT_E07     3   0.000      0.992 0.00 0.00 1.00
#&gt; T48_CT_A01     2   0.000      0.966 0.00 1.00 0.00
#&gt; T48_CT_A02     2   0.502      0.695 0.00 0.76 0.24
#&gt; T48_CT_A05     3   0.000      0.992 0.00 0.00 1.00
#&gt; T48_CT_A08     2   0.254      0.902 0.00 0.92 0.08
#&gt; T48_CT_B11     2   0.000      0.966 0.00 1.00 0.00
#&gt; T48_CT_C05     3   0.207      0.934 0.00 0.06 0.94
#&gt; T48_CT_C06     3   0.000      0.992 0.00 0.00 1.00
#&gt; T48_CT_C09     2   0.000      0.966 0.00 1.00 0.00
#&gt; T48_CT_C11     2   0.000      0.966 0.00 1.00 0.00
#&gt; T48_CT_D06     2   0.000      0.966 0.00 1.00 0.00
#&gt; T48_CT_D11     2   0.000      0.966 0.00 1.00 0.00
#&gt; T48_CT_E01     3   0.000      0.992 0.00 0.00 1.00
#&gt; T48_CT_E03     3   0.000      0.992 0.00 0.00 1.00
#&gt; T48_CT_G02     2   0.000      0.966 0.00 1.00 0.00
#&gt; T48_CT_G08     3   0.000      0.992 0.00 0.00 1.00
#&gt; T48_CT_G09     3   0.000      0.992 0.00 0.00 1.00
#&gt; T48_CT_H08     2   0.000      0.966 0.00 1.00 0.00
#&gt; T48_CT_H12     2   0.000      0.966 0.00 1.00 0.00
#&gt; T72_CT_A01     1   0.000      1.000 1.00 0.00 0.00
#&gt; T72_CT_A05     1   0.000      1.000 1.00 0.00 0.00
#&gt; T72_CT_A09     1   0.000      1.000 1.00 0.00 0.00
#&gt; T72_CT_A11     1   0.000      1.000 1.00 0.00 0.00
#&gt; T72_CT_B01     1   0.000      1.000 1.00 0.00 0.00
#&gt; T72_CT_B12     1   0.000      1.000 1.00 0.00 0.00
#&gt; T72_CT_C04     1   0.000      1.000 1.00 0.00 0.00
#&gt; T72_CT_C07     2   0.207      0.906 0.06 0.94 0.00
#&gt; T72_CT_D01     1   0.000      1.000 1.00 0.00 0.00
#&gt; T72_CT_D04     1   0.000      1.000 1.00 0.00 0.00
#&gt; T72_CT_E05     1   0.000      1.000 1.00 0.00 0.00
#&gt; T72_CT_E07     1   0.000      1.000 1.00 0.00 0.00
#&gt; T72_CT_F01     1   0.000      1.000 1.00 0.00 0.00
#&gt; T72_CT_F05     1   0.000      1.000 1.00 0.00 0.00
#&gt; T72_CT_F10     1   0.000      1.000 1.00 0.00 0.00
#&gt; T72_CT_F11     1   0.000      1.000 1.00 0.00 0.00
#&gt; T72_CT_G06     1   0.000      1.000 1.00 0.00 0.00
#&gt; T72_CT_G10     1   0.000      1.000 1.00 0.00 0.00
#&gt; T72_CT_H01     1   0.000      1.000 1.00 0.00 0.00
#&gt; T72_CT_H03     1   0.000      1.000 1.00 0.00 0.00
#&gt; T72_CT_H12     1   0.000      1.000 1.00 0.00 0.00
</code></pre>

<script>
$('#tab-node-012-get-classes-2-a').parent().next().next().hide();
$('#tab-node-012-get-classes-2-a').click(function(){
  $('#tab-node-012-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-node-012-get-classes-3'>
<p><a id='tab-node-012-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette   p1   p2   p3   p4
#&gt; T24_CT_B03     3  0.0000      0.952 0.00 0.00 1.00 0.00
#&gt; T24_CT_E04     2  0.0707      0.978 0.00 0.98 0.00 0.02
#&gt; T24_CT_E07     3  0.0000      0.952 0.00 0.00 1.00 0.00
#&gt; T48_CT_A01     2  0.1211      0.977 0.00 0.96 0.00 0.04
#&gt; T48_CT_A02     2  0.0000      0.975 0.00 1.00 0.00 0.00
#&gt; T48_CT_A05     3  0.0000      0.952 0.00 0.00 1.00 0.00
#&gt; T48_CT_A08     2  0.0000      0.975 0.00 1.00 0.00 0.00
#&gt; T48_CT_B11     2  0.1211      0.977 0.00 0.96 0.00 0.04
#&gt; T48_CT_C05     3  0.4624      0.476 0.00 0.34 0.66 0.00
#&gt; T48_CT_C06     3  0.0000      0.952 0.00 0.00 1.00 0.00
#&gt; T48_CT_C09     2  0.0707      0.964 0.00 0.98 0.02 0.00
#&gt; T48_CT_C11     2  0.1211      0.977 0.00 0.96 0.00 0.04
#&gt; T48_CT_D06     2  0.1211      0.977 0.00 0.96 0.00 0.04
#&gt; T48_CT_D11     4  0.0707      0.912 0.00 0.02 0.00 0.98
#&gt; T48_CT_E01     3  0.0000      0.952 0.00 0.00 1.00 0.00
#&gt; T48_CT_E03     3  0.0000      0.952 0.00 0.00 1.00 0.00
#&gt; T48_CT_G02     2  0.0000      0.975 0.00 1.00 0.00 0.00
#&gt; T48_CT_G08     3  0.0000      0.952 0.00 0.00 1.00 0.00
#&gt; T48_CT_G09     3  0.0000      0.952 0.00 0.00 1.00 0.00
#&gt; T48_CT_H08     2  0.2011      0.928 0.00 0.92 0.00 0.08
#&gt; T48_CT_H12     2  0.0707      0.978 0.00 0.98 0.00 0.02
#&gt; T72_CT_A01     4  0.1211      0.953 0.04 0.00 0.00 0.96
#&gt; T72_CT_A05     4  0.1211      0.953 0.04 0.00 0.00 0.96
#&gt; T72_CT_A09     1  0.0000      0.973 1.00 0.00 0.00 0.00
#&gt; T72_CT_A11     1  0.2011      0.925 0.92 0.00 0.00 0.08
#&gt; T72_CT_B01     4  0.1211      0.953 0.04 0.00 0.00 0.96
#&gt; T72_CT_B12     1  0.0707      0.971 0.98 0.00 0.00 0.02
#&gt; T72_CT_C04     4  0.1211      0.953 0.04 0.00 0.00 0.96
#&gt; T72_CT_C07     4  0.0000      0.924 0.00 0.00 0.00 1.00
#&gt; T72_CT_D01     1  0.0707      0.971 0.98 0.00 0.00 0.02
#&gt; T72_CT_D04     1  0.0000      0.973 1.00 0.00 0.00 0.00
#&gt; T72_CT_E05     1  0.0000      0.973 1.00 0.00 0.00 0.00
#&gt; T72_CT_E07     1  0.2706      0.902 0.90 0.00 0.02 0.08
#&gt; T72_CT_F01     1  0.0707      0.966 0.98 0.00 0.00 0.02
#&gt; T72_CT_F05     1  0.1211      0.955 0.96 0.00 0.00 0.04
#&gt; T72_CT_F10     1  0.1211      0.961 0.96 0.00 0.00 0.04
#&gt; T72_CT_F11     1  0.0707      0.971 0.98 0.00 0.00 0.02
#&gt; T72_CT_G06     4  0.3801      0.750 0.22 0.00 0.00 0.78
#&gt; T72_CT_G10     1  0.0000      0.973 1.00 0.00 0.00 0.00
#&gt; T72_CT_H01     4  0.1211      0.953 0.04 0.00 0.00 0.96
#&gt; T72_CT_H03     1  0.0000      0.973 1.00 0.00 0.00 0.00
#&gt; T72_CT_H12     1  0.0000      0.973 1.00 0.00 0.00 0.00
</code></pre>

<script>
$('#tab-node-012-get-classes-3-a').parent().next().next().hide();
$('#tab-node-012-get-classes-3-a').click(function(){
  $('#tab-node-012-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.




<script>
$( function() {
	$( '#tabs-node-012-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-node-012-consensus-heatmap'>
<ul>
<li><a href='#tab-node-012-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-node-012-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-node-012-consensus-heatmap-3'>k = 4</a></li>
</ul>
<div id='tab-node-012-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-node-012-consensus-heatmap-1-1.png" alt="plot of chunk tab-node-012-consensus-heatmap-1"/></p>

</div>
<div id='tab-node-012-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-node-012-consensus-heatmap-2-1.png" alt="plot of chunk tab-node-012-consensus-heatmap-2"/></p>

</div>
<div id='tab-node-012-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-node-012-consensus-heatmap-3-1.png" alt="plot of chunk tab-node-012-consensus-heatmap-3"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:





<script>
$( function() {
	$( '#tabs-node-012-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-node-012-membership-heatmap'>
<ul>
<li><a href='#tab-node-012-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-node-012-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-node-012-membership-heatmap-3'>k = 4</a></li>
</ul>
<div id='tab-node-012-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-node-012-membership-heatmap-1-1.png" alt="plot of chunk tab-node-012-membership-heatmap-1"/></p>

</div>
<div id='tab-node-012-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-node-012-membership-heatmap-2-1.png" alt="plot of chunk tab-node-012-membership-heatmap-2"/></p>

</div>
<div id='tab-node-012-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-node-012-membership-heatmap-3-1.png" alt="plot of chunk tab-node-012-membership-heatmap-3"/></p>

</div>
</div>

As soon as the classes for columns are determined, the signatures
that are significantly different between subgroups can be looked for. 
Following are the heatmaps for signatures.




Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-node-012-get-signatures' ).tabs();
} );
</script>
<div id='tabs-node-012-get-signatures'>
<ul>
<li><a href='#tab-node-012-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-node-012-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-node-012-get-signatures-3'>k = 4</a></li>
</ul>
<div id='tab-node-012-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-node-012-get-signatures-1-1.png" alt="plot of chunk tab-node-012-get-signatures-1"/></p>

</div>
<div id='tab-node-012-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-node-012-get-signatures-2-1.png" alt="plot of chunk tab-node-012-get-signatures-2"/></p>

</div>
<div id='tab-node-012-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-node-012-get-signatures-3-1.png" alt="plot of chunk tab-node-012-get-signatures-3"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-node-012-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-node-012-get-signatures-no-scale'>
<ul>
<li><a href='#tab-node-012-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-node-012-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-node-012-get-signatures-no-scale-3'>k = 4</a></li>
</ul>
<div id='tab-node-012-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-node-012-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-node-012-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-node-012-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-node-012-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-node-012-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-node-012-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-node-012-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-node-012-get-signatures-no-scale-3"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk node-012-signature_compare](figure_cola/node-012-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. To get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows (which is done by automatically selecting number of clusters).

If there are too many signatures, `top_signatures = ...` can be set to only show the 
signatures with the highest FDRs:

```r
# code only for demonstration
# e.g. to show the top 500 most significant rows
tb = get_signature(res, k = ..., top_signatures = 500)
```

If the signatures are defined as these which are uniquely high in current group, `diff_method` argument
can be set to `"uniquely_high_in_one_group"`:

```r
# code only for demonstration
tb = get_signature(res, k = ..., diff_method = "uniquely_high_in_one_group")
```




UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-node-012-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-node-012-dimension-reduction'>
<ul>
<li><a href='#tab-node-012-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-node-012-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-node-012-dimension-reduction-3'>k = 4</a></li>
</ul>
<div id='tab-node-012-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-node-012-dimension-reduction-1-1.png" alt="plot of chunk tab-node-012-dimension-reduction-1"/></p>

</div>
<div id='tab-node-012-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-node-012-dimension-reduction-2-1.png" alt="plot of chunk tab-node-012-dimension-reduction-2"/></p>

</div>
<div id='tab-node-012-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-node-012-dimension-reduction-3-1.png" alt="plot of chunk tab-node-012-dimension-reduction-3"/></p>

</div>
</div>



Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk node-012-collect-classes](figure_cola/node-012-collect-classes-1.png)




Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n_sample Hours(p-value) Media(p-value) State(p-value) k
#> ATC:skmeans       42       5.04e-09             NA       0.029392 2
#> ATC:skmeans       42       4.59e-08             NA       0.000692 3
#> ATC:skmeans       41       5.85e-07             NA       0.000242 4
```




If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](https://jokergoo.github.io/cola_vignettes/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### Node02


Parent node: [Node0](#Node0).
Child nodes: 
                Node011-leaf
        ,
                [Node012](#Node012)
        ,
                Node021-leaf
        ,
                Node022-leaf
        ,
                Node023-leaf
        ,
                [Node031](#Node031)
        ,
                Node032-leaf
        .







The object with results only for a single top-value method and a single partitioning method 
can be extracted as:

```r
res = res_rh["02"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4.
#>   On a matrix with 11388 rows and 100 columns.
#>   Top rows (1139) are extracted by 'ATC' method.
#>   Subgroups are detected by 'skmeans' method.
#>   Performed in total 150 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_partitions"     
#>  [7] "compare_signatures"      "consensus_heatmap"       "dimension_reduction"    
#> [10] "functional_enrichment"   "get_anno_col"            "get_anno"               
#> [13] "get_classes"             "get_consensus"           "get_matrix"             
#> [16] "get_membership"          "get_param"               "get_signatures"         
#> [19] "get_stats"               "is_best_k"               "is_stable_k"            
#> [22] "membership_heatmap"      "ncol"                    "nrow"                   
#> [25] "plot_ecdf"               "predict_classes"         "rownames"               
#> [28] "select_partition_number" "show"                    "suggest_best_k"         
#> [31] "test_to_known_factors"   "top_rows_heatmap"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of subgroups)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk node-02-collect-plots](figure_cola/node-02-collect-plots-1.png)

The plots are:

- The first row: a plot of the eCDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- eCDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus subgroup labels in all
  partitions.
- Area increased. Denote $A_k$ as the area under the eCDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](https://jokergoo.github.io/cola_vignettes/cola.html#toc_13).

Generally speaking, higher 1-PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk node-02-select-partition-number](figure_cola/node-02-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.958           0.965       0.985          0.504 0.496   0.496
#> 3 3 1.000           0.968       0.988          0.287 0.771   0.575
#> 4 4 0.854           0.889       0.949          0.155 0.857   0.618
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 3
#> attr(,"optional")
#> [1] 2
```

There is also optional best $k$ = 2 that is worth to check.

Following is the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall subgroup
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-node-02-get-classes' ).tabs();
} );
</script>
<div id='tabs-node-02-get-classes'>
<ul>
<li><a href='#tab-node-02-get-classes-1'>k = 2</a></li>
<li><a href='#tab-node-02-get-classes-2'>k = 3</a></li>
<li><a href='#tab-node-02-get-classes-3'>k = 4</a></li>
</ul>

<div id='tab-node-02-get-classes-1'>
<p><a id='tab-node-02-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette   p1   p2
#&gt; T0_CT_A08      1   0.000      0.988 1.00 0.00
#&gt; T0_CT_C03      1   0.000      0.988 1.00 0.00
#&gt; T0_CT_E05      2   0.000      0.979 0.00 1.00
#&gt; T0_CT_G09      2   0.000      0.979 0.00 1.00
#&gt; T0_CT_H01      2   0.000      0.979 0.00 1.00
#&gt; T24_CT_A01     1   0.000      0.988 1.00 0.00
#&gt; T24_CT_A08     1   0.000      0.988 1.00 0.00
#&gt; T24_CT_A09     1   0.000      0.988 1.00 0.00
#&gt; T24_CT_A10     2   0.141      0.963 0.02 0.98
#&gt; T24_CT_B01     1   0.000      0.988 1.00 0.00
#&gt; T24_CT_B02     1   0.000      0.988 1.00 0.00
#&gt; T24_CT_B07     1   0.000      0.988 1.00 0.00
#&gt; T24_CT_B09     1   0.000      0.988 1.00 0.00
#&gt; T24_CT_C01     1   0.000      0.988 1.00 0.00
#&gt; T24_CT_C03     1   0.000      0.988 1.00 0.00
#&gt; T24_CT_C10     1   0.000      0.988 1.00 0.00
#&gt; T24_CT_C11     1   0.584      0.835 0.86 0.14
#&gt; T24_CT_D04     1   0.000      0.988 1.00 0.00
#&gt; T24_CT_D05     1   0.000      0.988 1.00 0.00
#&gt; T24_CT_D06     1   0.000      0.988 1.00 0.00
#&gt; T24_CT_D07     1   0.000      0.988 1.00 0.00
#&gt; T24_CT_E01     2   0.000      0.979 0.00 1.00
#&gt; T24_CT_E02     1   0.000      0.988 1.00 0.00
#&gt; T24_CT_E05     1   0.000      0.988 1.00 0.00
#&gt; T24_CT_E09     1   0.000      0.988 1.00 0.00
#&gt; T24_CT_E11     1   0.000      0.988 1.00 0.00
#&gt; T24_CT_F03     2   0.000      0.979 0.00 1.00
#&gt; T24_CT_F04     1   0.722      0.751 0.80 0.20
#&gt; T24_CT_F05     2   0.000      0.979 0.00 1.00
#&gt; T24_CT_F07     1   0.000      0.988 1.00 0.00
#&gt; T24_CT_F08     1   0.000      0.988 1.00 0.00
#&gt; T24_CT_F10     2   0.000      0.979 0.00 1.00
#&gt; T24_CT_F11     2   0.000      0.979 0.00 1.00
#&gt; T24_CT_G02     1   0.000      0.988 1.00 0.00
#&gt; T24_CT_G03     1   0.000      0.988 1.00 0.00
#&gt; T24_CT_G06     1   0.000      0.988 1.00 0.00
#&gt; T24_CT_G08     1   0.000      0.988 1.00 0.00
#&gt; T24_CT_G11     1   0.000      0.988 1.00 0.00
#&gt; T24_CT_G12     1   0.000      0.988 1.00 0.00
#&gt; T24_CT_H02     1   0.000      0.988 1.00 0.00
#&gt; T24_CT_H05     1   0.760      0.720 0.78 0.22
#&gt; T24_CT_H07     2   0.795      0.689 0.24 0.76
#&gt; T24_CT_H09     1   0.000      0.988 1.00 0.00
#&gt; T24_CT_H12     1   0.000      0.988 1.00 0.00
#&gt; T48_CT_A03     1   0.000      0.988 1.00 0.00
#&gt; T48_CT_A06     1   0.000      0.988 1.00 0.00
#&gt; T48_CT_A09     2   0.000      0.979 0.00 1.00
#&gt; T48_CT_A12     2   0.000      0.979 0.00 1.00
#&gt; T48_CT_B03     1   0.000      0.988 1.00 0.00
#&gt; T48_CT_B04     1   0.000      0.988 1.00 0.00
#&gt; T48_CT_B08     1   0.000      0.988 1.00 0.00
#&gt; T48_CT_B12     2   0.000      0.979 0.00 1.00
#&gt; T48_CT_C02     1   0.000      0.988 1.00 0.00
#&gt; T48_CT_C04     1   0.000      0.988 1.00 0.00
#&gt; T48_CT_C07     2   0.000      0.979 0.00 1.00
#&gt; T48_CT_C10     2   0.141      0.963 0.02 0.98
#&gt; T48_CT_D02     2   0.000      0.979 0.00 1.00
#&gt; T48_CT_D03     1   0.000      0.988 1.00 0.00
#&gt; T48_CT_D08     1   0.000      0.988 1.00 0.00
#&gt; T48_CT_D10     2   0.000      0.979 0.00 1.00
#&gt; T48_CT_D12     1   0.000      0.988 1.00 0.00
#&gt; T48_CT_E02     2   0.000      0.979 0.00 1.00
#&gt; T48_CT_E04     2   0.000      0.979 0.00 1.00
#&gt; T48_CT_E06     1   0.000      0.988 1.00 0.00
#&gt; T48_CT_E07     1   0.000      0.988 1.00 0.00
#&gt; T48_CT_E10     1   0.000      0.988 1.00 0.00
#&gt; T48_CT_E11     2   0.000      0.979 0.00 1.00
#&gt; T48_CT_F02     2   0.000      0.979 0.00 1.00
#&gt; T48_CT_F03     2   0.529      0.859 0.12 0.88
#&gt; T48_CT_F07     2   0.000      0.979 0.00 1.00
#&gt; T48_CT_F10     2   0.925      0.491 0.34 0.66
#&gt; T48_CT_F11     1   0.141      0.969 0.98 0.02
#&gt; T48_CT_G01     1   0.000      0.988 1.00 0.00
#&gt; T48_CT_G11     2   0.000      0.979 0.00 1.00
#&gt; T48_CT_H02     2   0.000      0.979 0.00 1.00
#&gt; T48_CT_H04     1   0.000      0.988 1.00 0.00
#&gt; T48_CT_H07     2   0.000      0.979 0.00 1.00
#&gt; T48_CT_H11     2   0.000      0.979 0.00 1.00
#&gt; T72_CT_A08     2   0.000      0.979 0.00 1.00
#&gt; T72_CT_B02     2   0.000      0.979 0.00 1.00
#&gt; T72_CT_B03     2   0.000      0.979 0.00 1.00
#&gt; T72_CT_B04     2   0.242      0.944 0.04 0.96
#&gt; T72_CT_B05     2   0.000      0.979 0.00 1.00
#&gt; T72_CT_B06     2   0.000      0.979 0.00 1.00
#&gt; T72_CT_B09     1   0.000      0.988 1.00 0.00
#&gt; T72_CT_B11     2   0.000      0.979 0.00 1.00
#&gt; T72_CT_C06     2   0.000      0.979 0.00 1.00
#&gt; T72_CT_C09     2   0.000      0.979 0.00 1.00
#&gt; T72_CT_D03     2   0.000      0.979 0.00 1.00
#&gt; T72_CT_D05     2   0.000      0.979 0.00 1.00
#&gt; T72_CT_D07     2   0.000      0.979 0.00 1.00
#&gt; T72_CT_D10     2   0.000      0.979 0.00 1.00
#&gt; T72_CT_D11     2   0.000      0.979 0.00 1.00
#&gt; T72_CT_E04     2   0.000      0.979 0.00 1.00
#&gt; T72_CT_F07     2   0.000      0.979 0.00 1.00
#&gt; T72_CT_G03     1   0.000      0.988 1.00 0.00
#&gt; T72_CT_G04     2   0.680      0.782 0.18 0.82
#&gt; T72_CT_G11     2   0.000      0.979 0.00 1.00
#&gt; T72_CT_H10     2   0.000      0.979 0.00 1.00
#&gt; T72_CT_H11     2   0.000      0.979 0.00 1.00
</code></pre>

<script>
$('#tab-node-02-get-classes-1-a').parent().next().next().hide();
$('#tab-node-02-get-classes-1-a').click(function(){
  $('#tab-node-02-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-node-02-get-classes-2'>
<p><a id='tab-node-02-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette   p1   p2  p3
#&gt; T0_CT_A08      1  0.0000     0.9806 1.00 0.00 0.0
#&gt; T0_CT_C03      1  0.4002     0.8045 0.84 0.16 0.0
#&gt; T0_CT_E05      2  0.0000     0.9874 0.00 1.00 0.0
#&gt; T0_CT_G09      2  0.0000     0.9874 0.00 1.00 0.0
#&gt; T0_CT_H01      2  0.0000     0.9874 0.00 1.00 0.0
#&gt; T24_CT_A01     1  0.0000     0.9806 1.00 0.00 0.0
#&gt; T24_CT_A08     1  0.0000     0.9806 1.00 0.00 0.0
#&gt; T24_CT_A09     1  0.0000     0.9806 1.00 0.00 0.0
#&gt; T24_CT_A10     2  0.0000     0.9874 0.00 1.00 0.0
#&gt; T24_CT_B01     2  0.0892     0.9650 0.02 0.98 0.0
#&gt; T24_CT_B02     1  0.0000     0.9806 1.00 0.00 0.0
#&gt; T24_CT_B07     1  0.0000     0.9806 1.00 0.00 0.0
#&gt; T24_CT_B09     1  0.0000     0.9806 1.00 0.00 0.0
#&gt; T24_CT_C01     1  0.0000     0.9806 1.00 0.00 0.0
#&gt; T24_CT_C03     1  0.6302     0.0938 0.52 0.48 0.0
#&gt; T24_CT_C10     1  0.2537     0.8992 0.92 0.08 0.0
#&gt; T24_CT_C11     2  0.0000     0.9874 0.00 1.00 0.0
#&gt; T24_CT_D04     1  0.0000     0.9806 1.00 0.00 0.0
#&gt; T24_CT_D05     2  0.0000     0.9874 0.00 1.00 0.0
#&gt; T24_CT_D06     1  0.0000     0.9806 1.00 0.00 0.0
#&gt; T24_CT_D07     2  0.0000     0.9874 0.00 1.00 0.0
#&gt; T24_CT_E01     2  0.0000     0.9874 0.00 1.00 0.0
#&gt; T24_CT_E02     1  0.0000     0.9806 1.00 0.00 0.0
#&gt; T24_CT_E05     1  0.0000     0.9806 1.00 0.00 0.0
#&gt; T24_CT_E09     1  0.0000     0.9806 1.00 0.00 0.0
#&gt; T24_CT_E11     1  0.0000     0.9806 1.00 0.00 0.0
#&gt; T24_CT_F03     2  0.0000     0.9874 0.00 1.00 0.0
#&gt; T24_CT_F04     2  0.0000     0.9874 0.00 1.00 0.0
#&gt; T24_CT_F05     2  0.0000     0.9874 0.00 1.00 0.0
#&gt; T24_CT_F07     2  0.0000     0.9874 0.00 1.00 0.0
#&gt; T24_CT_F08     1  0.0000     0.9806 1.00 0.00 0.0
#&gt; T24_CT_F10     2  0.0000     0.9874 0.00 1.00 0.0
#&gt; T24_CT_F11     2  0.0000     0.9874 0.00 1.00 0.0
#&gt; T24_CT_G02     1  0.0000     0.9806 1.00 0.00 0.0
#&gt; T24_CT_G03     1  0.0000     0.9806 1.00 0.00 0.0
#&gt; T24_CT_G06     1  0.0000     0.9806 1.00 0.00 0.0
#&gt; T24_CT_G08     1  0.0000     0.9806 1.00 0.00 0.0
#&gt; T24_CT_G11     1  0.0000     0.9806 1.00 0.00 0.0
#&gt; T24_CT_G12     1  0.0000     0.9806 1.00 0.00 0.0
#&gt; T24_CT_H02     1  0.0000     0.9806 1.00 0.00 0.0
#&gt; T24_CT_H05     2  0.0000     0.9874 0.00 1.00 0.0
#&gt; T24_CT_H07     2  0.0000     0.9874 0.00 1.00 0.0
#&gt; T24_CT_H09     1  0.0000     0.9806 1.00 0.00 0.0
#&gt; T24_CT_H12     1  0.0000     0.9806 1.00 0.00 0.0
#&gt; T48_CT_A03     1  0.0000     0.9806 1.00 0.00 0.0
#&gt; T48_CT_A06     1  0.0000     0.9806 1.00 0.00 0.0
#&gt; T48_CT_A09     2  0.0000     0.9874 0.00 1.00 0.0
#&gt; T48_CT_A12     2  0.0000     0.9874 0.00 1.00 0.0
#&gt; T48_CT_B03     1  0.0000     0.9806 1.00 0.00 0.0
#&gt; T48_CT_B04     1  0.0000     0.9806 1.00 0.00 0.0
#&gt; T48_CT_B08     1  0.0000     0.9806 1.00 0.00 0.0
#&gt; T48_CT_B12     2  0.0000     0.9874 0.00 1.00 0.0
#&gt; T48_CT_C02     1  0.0000     0.9806 1.00 0.00 0.0
#&gt; T48_CT_C04     1  0.0000     0.9806 1.00 0.00 0.0
#&gt; T48_CT_C07     2  0.0000     0.9874 0.00 1.00 0.0
#&gt; T48_CT_C10     2  0.0000     0.9874 0.00 1.00 0.0
#&gt; T48_CT_D02     2  0.0000     0.9874 0.00 1.00 0.0
#&gt; T48_CT_D03     1  0.0000     0.9806 1.00 0.00 0.0
#&gt; T48_CT_D08     1  0.1529     0.9419 0.96 0.04 0.0
#&gt; T48_CT_D10     2  0.0000     0.9874 0.00 1.00 0.0
#&gt; T48_CT_D12     1  0.0000     0.9806 1.00 0.00 0.0
#&gt; T48_CT_E02     2  0.0000     0.9874 0.00 1.00 0.0
#&gt; T48_CT_E04     2  0.0000     0.9874 0.00 1.00 0.0
#&gt; T48_CT_E06     1  0.0000     0.9806 1.00 0.00 0.0
#&gt; T48_CT_E07     1  0.0000     0.9806 1.00 0.00 0.0
#&gt; T48_CT_E10     1  0.0000     0.9806 1.00 0.00 0.0
#&gt; T48_CT_E11     2  0.0000     0.9874 0.00 1.00 0.0
#&gt; T48_CT_F02     2  0.0000     0.9874 0.00 1.00 0.0
#&gt; T48_CT_F03     2  0.0000     0.9874 0.00 1.00 0.0
#&gt; T48_CT_F07     2  0.0000     0.9874 0.00 1.00 0.0
#&gt; T48_CT_F10     2  0.0000     0.9874 0.00 1.00 0.0
#&gt; T48_CT_F11     1  0.0000     0.9806 1.00 0.00 0.0
#&gt; T48_CT_G01     1  0.0000     0.9806 1.00 0.00 0.0
#&gt; T48_CT_G11     2  0.0000     0.9874 0.00 1.00 0.0
#&gt; T48_CT_H02     2  0.6126     0.3309 0.00 0.60 0.4
#&gt; T48_CT_H04     1  0.0000     0.9806 1.00 0.00 0.0
#&gt; T48_CT_H07     2  0.0000     0.9874 0.00 1.00 0.0
#&gt; T48_CT_H11     2  0.0000     0.9874 0.00 1.00 0.0
#&gt; T72_CT_A08     3  0.0000     1.0000 0.00 0.00 1.0
#&gt; T72_CT_B02     3  0.0000     1.0000 0.00 0.00 1.0
#&gt; T72_CT_B03     3  0.0000     1.0000 0.00 0.00 1.0
#&gt; T72_CT_B04     3  0.0000     1.0000 0.00 0.00 1.0
#&gt; T72_CT_B05     3  0.0000     1.0000 0.00 0.00 1.0
#&gt; T72_CT_B06     3  0.0000     1.0000 0.00 0.00 1.0
#&gt; T72_CT_B09     1  0.0000     0.9806 1.00 0.00 0.0
#&gt; T72_CT_B11     3  0.0000     1.0000 0.00 0.00 1.0
#&gt; T72_CT_C06     3  0.0000     1.0000 0.00 0.00 1.0
#&gt; T72_CT_C09     3  0.0000     1.0000 0.00 0.00 1.0
#&gt; T72_CT_D03     3  0.0000     1.0000 0.00 0.00 1.0
#&gt; T72_CT_D05     3  0.0000     1.0000 0.00 0.00 1.0
#&gt; T72_CT_D07     3  0.0000     1.0000 0.00 0.00 1.0
#&gt; T72_CT_D10     3  0.0000     1.0000 0.00 0.00 1.0
#&gt; T72_CT_D11     3  0.0000     1.0000 0.00 0.00 1.0
#&gt; T72_CT_E04     3  0.0000     1.0000 0.00 0.00 1.0
#&gt; T72_CT_F07     3  0.0000     1.0000 0.00 0.00 1.0
#&gt; T72_CT_G03     3  0.0000     1.0000 0.00 0.00 1.0
#&gt; T72_CT_G04     3  0.0000     1.0000 0.00 0.00 1.0
#&gt; T72_CT_G11     3  0.0000     1.0000 0.00 0.00 1.0
#&gt; T72_CT_H10     3  0.0000     1.0000 0.00 0.00 1.0
#&gt; T72_CT_H11     3  0.0000     1.0000 0.00 0.00 1.0
</code></pre>

<script>
$('#tab-node-02-get-classes-2-a').parent().next().next().hide();
$('#tab-node-02-get-classes-2-a').click(function(){
  $('#tab-node-02-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-node-02-get-classes-3'>
<p><a id='tab-node-02-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette   p1   p2   p3   p4
#&gt; T0_CT_A08      1  0.0000      0.897 1.00 0.00 0.00 0.00
#&gt; T0_CT_C03      4  0.4755      0.692 0.20 0.04 0.00 0.76
#&gt; T0_CT_E05      2  0.0707      0.952 0.00 0.98 0.00 0.02
#&gt; T0_CT_G09      2  0.0000      0.964 0.00 1.00 0.00 0.00
#&gt; T0_CT_H01      2  0.0707      0.953 0.00 0.98 0.00 0.02
#&gt; T24_CT_A01     4  0.0000      0.927 0.00 0.00 0.00 1.00
#&gt; T24_CT_A08     1  0.0000      0.897 1.00 0.00 0.00 0.00
#&gt; T24_CT_A09     4  0.0707      0.917 0.02 0.00 0.00 0.98
#&gt; T24_CT_A10     2  0.1211      0.938 0.00 0.96 0.00 0.04
#&gt; T24_CT_B01     4  0.0000      0.927 0.00 0.00 0.00 1.00
#&gt; T24_CT_B02     4  0.3400      0.752 0.18 0.00 0.00 0.82
#&gt; T24_CT_B07     1  0.0000      0.897 1.00 0.00 0.00 0.00
#&gt; T24_CT_B09     1  0.0000      0.897 1.00 0.00 0.00 0.00
#&gt; T24_CT_C01     1  0.1211      0.877 0.96 0.00 0.00 0.04
#&gt; T24_CT_C03     4  0.0000      0.927 0.00 0.00 0.00 1.00
#&gt; T24_CT_C10     4  0.0000      0.927 0.00 0.00 0.00 1.00
#&gt; T24_CT_C11     2  0.4522      0.547 0.00 0.68 0.00 0.32
#&gt; T24_CT_D04     1  0.3400      0.777 0.82 0.00 0.00 0.18
#&gt; T24_CT_D05     4  0.0000      0.927 0.00 0.00 0.00 1.00
#&gt; T24_CT_D06     4  0.0000      0.927 0.00 0.00 0.00 1.00
#&gt; T24_CT_D07     4  0.0707      0.915 0.00 0.02 0.00 0.98
#&gt; T24_CT_E01     2  0.0000      0.964 0.00 1.00 0.00 0.00
#&gt; T24_CT_E02     4  0.3801      0.694 0.22 0.00 0.00 0.78
#&gt; T24_CT_E05     1  0.0000      0.897 1.00 0.00 0.00 0.00
#&gt; T24_CT_E09     4  0.0000      0.927 0.00 0.00 0.00 1.00
#&gt; T24_CT_E11     1  0.3610      0.754 0.80 0.00 0.00 0.20
#&gt; T24_CT_F03     2  0.0000      0.964 0.00 1.00 0.00 0.00
#&gt; T24_CT_F04     4  0.3975      0.651 0.00 0.24 0.00 0.76
#&gt; T24_CT_F05     2  0.0000      0.964 0.00 1.00 0.00 0.00
#&gt; T24_CT_F07     4  0.2647      0.826 0.00 0.12 0.00 0.88
#&gt; T24_CT_F08     4  0.0000      0.927 0.00 0.00 0.00 1.00
#&gt; T24_CT_F10     2  0.0000      0.964 0.00 1.00 0.00 0.00
#&gt; T24_CT_F11     2  0.0000      0.964 0.00 1.00 0.00 0.00
#&gt; T24_CT_G02     1  0.0000      0.897 1.00 0.00 0.00 0.00
#&gt; T24_CT_G03     1  0.4624      0.502 0.66 0.00 0.00 0.34
#&gt; T24_CT_G06     4  0.0000      0.927 0.00 0.00 0.00 1.00
#&gt; T24_CT_G08     4  0.2345      0.853 0.10 0.00 0.00 0.90
#&gt; T24_CT_G11     1  0.4907      0.282 0.58 0.00 0.00 0.42
#&gt; T24_CT_G12     4  0.0000      0.927 0.00 0.00 0.00 1.00
#&gt; T24_CT_H02     1  0.4948      0.313 0.56 0.00 0.00 0.44
#&gt; T24_CT_H05     2  0.4522      0.557 0.00 0.68 0.00 0.32
#&gt; T24_CT_H07     2  0.1211      0.938 0.00 0.96 0.00 0.04
#&gt; T24_CT_H09     4  0.1211      0.904 0.04 0.00 0.00 0.96
#&gt; T24_CT_H12     1  0.4406      0.623 0.70 0.00 0.00 0.30
#&gt; T48_CT_A03     1  0.0000      0.897 1.00 0.00 0.00 0.00
#&gt; T48_CT_A06     1  0.0000      0.897 1.00 0.00 0.00 0.00
#&gt; T48_CT_A09     2  0.0000      0.964 0.00 1.00 0.00 0.00
#&gt; T48_CT_A12     2  0.0000      0.964 0.00 1.00 0.00 0.00
#&gt; T48_CT_B03     1  0.0000      0.897 1.00 0.00 0.00 0.00
#&gt; T48_CT_B04     1  0.0000      0.897 1.00 0.00 0.00 0.00
#&gt; T48_CT_B08     1  0.1637      0.870 0.94 0.00 0.00 0.06
#&gt; T48_CT_B12     2  0.0000      0.964 0.00 1.00 0.00 0.00
#&gt; T48_CT_C02     1  0.0000      0.897 1.00 0.00 0.00 0.00
#&gt; T48_CT_C04     1  0.0000      0.897 1.00 0.00 0.00 0.00
#&gt; T48_CT_C07     2  0.0000      0.964 0.00 1.00 0.00 0.00
#&gt; T48_CT_C10     2  0.0707      0.949 0.02 0.98 0.00 0.00
#&gt; T48_CT_D02     2  0.0000      0.964 0.00 1.00 0.00 0.00
#&gt; T48_CT_D03     1  0.3801      0.735 0.78 0.00 0.00 0.22
#&gt; T48_CT_D08     1  0.6449      0.577 0.64 0.14 0.00 0.22
#&gt; T48_CT_D10     2  0.0000      0.964 0.00 1.00 0.00 0.00
#&gt; T48_CT_D12     1  0.0000      0.897 1.00 0.00 0.00 0.00
#&gt; T48_CT_E02     2  0.0000      0.964 0.00 1.00 0.00 0.00
#&gt; T48_CT_E04     2  0.0000      0.964 0.00 1.00 0.00 0.00
#&gt; T48_CT_E06     1  0.0000      0.897 1.00 0.00 0.00 0.00
#&gt; T48_CT_E07     1  0.0000      0.897 1.00 0.00 0.00 0.00
#&gt; T48_CT_E10     1  0.2647      0.830 0.88 0.00 0.00 0.12
#&gt; T48_CT_E11     2  0.0000      0.964 0.00 1.00 0.00 0.00
#&gt; T48_CT_F02     2  0.0000      0.964 0.00 1.00 0.00 0.00
#&gt; T48_CT_F03     2  0.0707      0.953 0.00 0.98 0.00 0.02
#&gt; T48_CT_F07     2  0.0000      0.964 0.00 1.00 0.00 0.00
#&gt; T48_CT_F10     2  0.0707      0.952 0.00 0.98 0.00 0.02
#&gt; T48_CT_F11     1  0.0000      0.897 1.00 0.00 0.00 0.00
#&gt; T48_CT_G01     4  0.0000      0.927 0.00 0.00 0.00 1.00
#&gt; T48_CT_G11     2  0.0000      0.964 0.00 1.00 0.00 0.00
#&gt; T48_CT_H02     2  0.3610      0.737 0.00 0.80 0.20 0.00
#&gt; T48_CT_H04     1  0.0000      0.897 1.00 0.00 0.00 0.00
#&gt; T48_CT_H07     2  0.0000      0.964 0.00 1.00 0.00 0.00
#&gt; T48_CT_H11     2  0.0000      0.964 0.00 1.00 0.00 0.00
#&gt; T72_CT_A08     3  0.0000      0.990 0.00 0.00 1.00 0.00
#&gt; T72_CT_B02     3  0.0000      0.990 0.00 0.00 1.00 0.00
#&gt; T72_CT_B03     3  0.0000      0.990 0.00 0.00 1.00 0.00
#&gt; T72_CT_B04     3  0.0000      0.990 0.00 0.00 1.00 0.00
#&gt; T72_CT_B05     3  0.0000      0.990 0.00 0.00 1.00 0.00
#&gt; T72_CT_B06     3  0.0000      0.990 0.00 0.00 1.00 0.00
#&gt; T72_CT_B09     1  0.0000      0.897 1.00 0.00 0.00 0.00
#&gt; T72_CT_B11     3  0.0000      0.990 0.00 0.00 1.00 0.00
#&gt; T72_CT_C06     3  0.0000      0.990 0.00 0.00 1.00 0.00
#&gt; T72_CT_C09     3  0.0000      0.990 0.00 0.00 1.00 0.00
#&gt; T72_CT_D03     3  0.0000      0.990 0.00 0.00 1.00 0.00
#&gt; T72_CT_D05     3  0.0000      0.990 0.00 0.00 1.00 0.00
#&gt; T72_CT_D07     3  0.0000      0.990 0.00 0.00 1.00 0.00
#&gt; T72_CT_D10     3  0.0000      0.990 0.00 0.00 1.00 0.00
#&gt; T72_CT_D11     3  0.0000      0.990 0.00 0.00 1.00 0.00
#&gt; T72_CT_E04     3  0.0000      0.990 0.00 0.00 1.00 0.00
#&gt; T72_CT_F07     3  0.0000      0.990 0.00 0.00 1.00 0.00
#&gt; T72_CT_G03     1  0.2345      0.821 0.90 0.00 0.10 0.00
#&gt; T72_CT_G04     3  0.0000      0.990 0.00 0.00 1.00 0.00
#&gt; T72_CT_G11     3  0.3172      0.805 0.00 0.16 0.84 0.00
#&gt; T72_CT_H10     3  0.0000      0.990 0.00 0.00 1.00 0.00
#&gt; T72_CT_H11     3  0.0000      0.990 0.00 0.00 1.00 0.00
</code></pre>

<script>
$('#tab-node-02-get-classes-3-a').parent().next().next().hide();
$('#tab-node-02-get-classes-3-a').click(function(){
  $('#tab-node-02-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.




<script>
$( function() {
	$( '#tabs-node-02-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-node-02-consensus-heatmap'>
<ul>
<li><a href='#tab-node-02-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-node-02-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-node-02-consensus-heatmap-3'>k = 4</a></li>
</ul>
<div id='tab-node-02-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-node-02-consensus-heatmap-1-1.png" alt="plot of chunk tab-node-02-consensus-heatmap-1"/></p>

</div>
<div id='tab-node-02-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-node-02-consensus-heatmap-2-1.png" alt="plot of chunk tab-node-02-consensus-heatmap-2"/></p>

</div>
<div id='tab-node-02-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-node-02-consensus-heatmap-3-1.png" alt="plot of chunk tab-node-02-consensus-heatmap-3"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:





<script>
$( function() {
	$( '#tabs-node-02-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-node-02-membership-heatmap'>
<ul>
<li><a href='#tab-node-02-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-node-02-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-node-02-membership-heatmap-3'>k = 4</a></li>
</ul>
<div id='tab-node-02-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-node-02-membership-heatmap-1-1.png" alt="plot of chunk tab-node-02-membership-heatmap-1"/></p>

</div>
<div id='tab-node-02-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-node-02-membership-heatmap-2-1.png" alt="plot of chunk tab-node-02-membership-heatmap-2"/></p>

</div>
<div id='tab-node-02-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-node-02-membership-heatmap-3-1.png" alt="plot of chunk tab-node-02-membership-heatmap-3"/></p>

</div>
</div>

As soon as the classes for columns are determined, the signatures
that are significantly different between subgroups can be looked for. 
Following are the heatmaps for signatures.




Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-node-02-get-signatures' ).tabs();
} );
</script>
<div id='tabs-node-02-get-signatures'>
<ul>
<li><a href='#tab-node-02-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-node-02-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-node-02-get-signatures-3'>k = 4</a></li>
</ul>
<div id='tab-node-02-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-node-02-get-signatures-1-1.png" alt="plot of chunk tab-node-02-get-signatures-1"/></p>

</div>
<div id='tab-node-02-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-node-02-get-signatures-2-1.png" alt="plot of chunk tab-node-02-get-signatures-2"/></p>

</div>
<div id='tab-node-02-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-node-02-get-signatures-3-1.png" alt="plot of chunk tab-node-02-get-signatures-3"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-node-02-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-node-02-get-signatures-no-scale'>
<ul>
<li><a href='#tab-node-02-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-node-02-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-node-02-get-signatures-no-scale-3'>k = 4</a></li>
</ul>
<div id='tab-node-02-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-node-02-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-node-02-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-node-02-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-node-02-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-node-02-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-node-02-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-node-02-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-node-02-get-signatures-no-scale-3"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk node-02-signature_compare](figure_cola/node-02-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. To get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows (which is done by automatically selecting number of clusters).

If there are too many signatures, `top_signatures = ...` can be set to only show the 
signatures with the highest FDRs:

```r
# code only for demonstration
# e.g. to show the top 500 most significant rows
tb = get_signature(res, k = ..., top_signatures = 500)
```

If the signatures are defined as these which are uniquely high in current group, `diff_method` argument
can be set to `"uniquely_high_in_one_group"`:

```r
# code only for demonstration
tb = get_signature(res, k = ..., diff_method = "uniquely_high_in_one_group")
```




UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-node-02-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-node-02-dimension-reduction'>
<ul>
<li><a href='#tab-node-02-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-node-02-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-node-02-dimension-reduction-3'>k = 4</a></li>
</ul>
<div id='tab-node-02-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-node-02-dimension-reduction-1-1.png" alt="plot of chunk tab-node-02-dimension-reduction-1"/></p>

</div>
<div id='tab-node-02-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-node-02-dimension-reduction-2-1.png" alt="plot of chunk tab-node-02-dimension-reduction-2"/></p>

</div>
<div id='tab-node-02-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-node-02-dimension-reduction-3-1.png" alt="plot of chunk tab-node-02-dimension-reduction-3"/></p>

</div>
</div>



Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk node-02-collect-classes](figure_cola/node-02-collect-classes-1.png)




Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n_sample Hours(p-value) Media(p-value) State(p-value) k
#> ATC:skmeans       99       9.32e-07          0.908          0.959 2
#> ATC:skmeans       98       2.77e-18          0.347          0.386 3
#> ATC:skmeans       98       3.21e-20          0.441          0.514 4
```




If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](https://jokergoo.github.io/cola_vignettes/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### Node03


Parent node: [Node0](#Node0).
Child nodes: 
                Node011-leaf
        ,
                [Node012](#Node012)
        ,
                Node021-leaf
        ,
                Node022-leaf
        ,
                Node023-leaf
        ,
                [Node031](#Node031)
        ,
                Node032-leaf
        .







The object with results only for a single top-value method and a single partitioning method 
can be extracted as:

```r
res = res_rh["03"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4.
#>   On a matrix with 11388 rows and 69 columns.
#>   Top rows (1139) are extracted by 'ATC' method.
#>   Subgroups are detected by 'skmeans' method.
#>   Performed in total 150 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_partitions"     
#>  [7] "compare_signatures"      "consensus_heatmap"       "dimension_reduction"    
#> [10] "functional_enrichment"   "get_anno_col"            "get_anno"               
#> [13] "get_classes"             "get_consensus"           "get_matrix"             
#> [16] "get_membership"          "get_param"               "get_signatures"         
#> [19] "get_stats"               "is_best_k"               "is_stable_k"            
#> [22] "membership_heatmap"      "ncol"                    "nrow"                   
#> [25] "plot_ecdf"               "predict_classes"         "rownames"               
#> [28] "select_partition_number" "show"                    "suggest_best_k"         
#> [31] "test_to_known_factors"   "top_rows_heatmap"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of subgroups)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk node-03-collect-plots](figure_cola/node-03-collect-plots-1.png)

The plots are:

- The first row: a plot of the eCDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- eCDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus subgroup labels in all
  partitions.
- Area increased. Denote $A_k$ as the area under the eCDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](https://jokergoo.github.io/cola_vignettes/cola.html#toc_13).

Generally speaking, higher 1-PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk node-03-select-partition-number](figure_cola/node-03-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           1.000       1.000         0.4992 0.501   0.501
#> 3 3 0.858           0.946       0.963         0.3259 0.817   0.641
#> 4 4 0.652           0.727       0.810         0.0956 0.945   0.840
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following is the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall subgroup
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-node-03-get-classes' ).tabs();
} );
</script>
<div id='tabs-node-03-get-classes'>
<ul>
<li><a href='#tab-node-03-get-classes-1'>k = 2</a></li>
<li><a href='#tab-node-03-get-classes-2'>k = 3</a></li>
<li><a href='#tab-node-03-get-classes-3'>k = 4</a></li>
</ul>

<div id='tab-node-03-get-classes-1'>
<p><a id='tab-node-03-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1 p2
#&gt; T0_CT_A01      2       0          1  0  1
#&gt; T0_CT_A03      1       0          1  1  0
#&gt; T0_CT_A05      1       0          1  1  0
#&gt; T0_CT_A06      2       0          1  0  1
#&gt; T0_CT_A07      1       0          1  1  0
#&gt; T0_CT_A10      2       0          1  0  1
#&gt; T0_CT_A11      2       0          1  0  1
#&gt; T0_CT_B01      2       0          1  0  1
#&gt; T0_CT_B03      1       0          1  1  0
#&gt; T0_CT_B05      2       0          1  0  1
#&gt; T0_CT_B07      2       0          1  0  1
#&gt; T0_CT_B08      1       0          1  1  0
#&gt; T0_CT_B09      1       0          1  1  0
#&gt; T0_CT_C02      1       0          1  1  0
#&gt; T0_CT_C07      2       0          1  0  1
#&gt; T0_CT_C08      1       0          1  1  0
#&gt; T0_CT_C12      1       0          1  1  0
#&gt; T0_CT_D01      2       0          1  0  1
#&gt; T0_CT_D02      2       0          1  0  1
#&gt; T0_CT_D03      1       0          1  1  0
#&gt; T0_CT_D05      2       0          1  0  1
#&gt; T0_CT_D06      1       0          1  1  0
#&gt; T0_CT_D07      2       0          1  0  1
#&gt; T0_CT_D08      1       0          1  1  0
#&gt; T0_CT_D12      1       0          1  1  0
#&gt; T0_CT_E01      1       0          1  1  0
#&gt; T0_CT_E03      2       0          1  0  1
#&gt; T0_CT_E06      2       0          1  0  1
#&gt; T0_CT_E07      1       0          1  1  0
#&gt; T0_CT_E08      1       0          1  1  0
#&gt; T0_CT_E09      2       0          1  0  1
#&gt; T0_CT_E10      2       0          1  0  1
#&gt; T0_CT_E11      1       0          1  1  0
#&gt; T0_CT_E12      2       0          1  0  1
#&gt; T0_CT_F01      1       0          1  1  0
#&gt; T0_CT_F02      1       0          1  1  0
#&gt; T0_CT_F03      1       0          1  1  0
#&gt; T0_CT_F04      1       0          1  1  0
#&gt; T0_CT_F05      2       0          1  0  1
#&gt; T0_CT_F06      1       0          1  1  0
#&gt; T0_CT_F07      2       0          1  0  1
#&gt; T0_CT_F11      2       0          1  0  1
#&gt; T0_CT_F12      1       0          1  1  0
#&gt; T0_CT_G01      1       0          1  1  0
#&gt; T0_CT_G02      2       0          1  0  1
#&gt; T0_CT_G03      2       0          1  0  1
#&gt; T0_CT_G04      1       0          1  1  0
#&gt; T0_CT_G07      1       0          1  1  0
#&gt; T0_CT_G08      1       0          1  1  0
#&gt; T0_CT_G11      2       0          1  0  1
#&gt; T0_CT_H02      2       0          1  0  1
#&gt; T0_CT_H04      2       0          1  0  1
#&gt; T0_CT_H05      2       0          1  0  1
#&gt; T0_CT_H08      2       0          1  0  1
#&gt; T0_CT_H09      2       0          1  0  1
#&gt; T0_CT_H12      2       0          1  0  1
#&gt; T24_CT_A05     1       0          1  1  0
#&gt; T24_CT_B11     1       0          1  1  0
#&gt; T24_CT_F12     1       0          1  1  0
#&gt; T24_CT_H01     1       0          1  1  0
#&gt; T48_CT_A07     1       0          1  1  0
#&gt; T48_CT_E12     1       0          1  1  0
#&gt; T48_CT_G10     1       0          1  1  0
#&gt; T72_CT_B08     1       0          1  1  0
#&gt; T72_CT_C11     1       0          1  1  0
#&gt; T72_CT_G08     2       0          1  0  1
#&gt; T72_CT_H05     1       0          1  1  0
#&gt; T72_CT_H08     1       0          1  1  0
#&gt; T72_CT_H09     1       0          1  1  0
</code></pre>

<script>
$('#tab-node-03-get-classes-1-a').parent().next().next().hide();
$('#tab-node-03-get-classes-1-a').click(function(){
  $('#tab-node-03-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-node-03-get-classes-2'>
<p><a id='tab-node-03-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette   p1   p2   p3
#&gt; T0_CT_A01      2  0.0000      0.995 0.00 1.00 0.00
#&gt; T0_CT_A03      1  0.0000      0.959 1.00 0.00 0.00
#&gt; T0_CT_A05      1  0.0000      0.959 1.00 0.00 0.00
#&gt; T0_CT_A06      2  0.0000      0.995 0.00 1.00 0.00
#&gt; T0_CT_A07      1  0.0892      0.949 0.98 0.00 0.02
#&gt; T0_CT_A10      1  0.5406      0.722 0.78 0.20 0.02
#&gt; T0_CT_A11      2  0.0000      0.995 0.00 1.00 0.00
#&gt; T0_CT_B01      2  0.0000      0.995 0.00 1.00 0.00
#&gt; T0_CT_B03      3  0.0000      0.913 0.00 0.00 1.00
#&gt; T0_CT_B05      2  0.0000      0.995 0.00 1.00 0.00
#&gt; T0_CT_B07      2  0.0000      0.995 0.00 1.00 0.00
#&gt; T0_CT_B08      3  0.0892      0.914 0.02 0.00 0.98
#&gt; T0_CT_B09      1  0.0000      0.959 1.00 0.00 0.00
#&gt; T0_CT_C02      3  0.0000      0.913 0.00 0.00 1.00
#&gt; T0_CT_C07      2  0.0000      0.995 0.00 1.00 0.00
#&gt; T0_CT_C08      1  0.0000      0.959 1.00 0.00 0.00
#&gt; T0_CT_C12      1  0.0892      0.949 0.98 0.00 0.02
#&gt; T0_CT_D01      2  0.0000      0.995 0.00 1.00 0.00
#&gt; T0_CT_D02      2  0.0000      0.995 0.00 1.00 0.00
#&gt; T0_CT_D03      3  0.2959      0.905 0.10 0.00 0.90
#&gt; T0_CT_D05      2  0.0000      0.995 0.00 1.00 0.00
#&gt; T0_CT_D06      3  0.0000      0.913 0.00 0.00 1.00
#&gt; T0_CT_D07      2  0.0000      0.995 0.00 1.00 0.00
#&gt; T0_CT_D08      3  0.0000      0.913 0.00 0.00 1.00
#&gt; T0_CT_D12      1  0.0000      0.959 1.00 0.00 0.00
#&gt; T0_CT_E01      3  0.2959      0.881 0.10 0.00 0.90
#&gt; T0_CT_E03      2  0.0892      0.977 0.02 0.98 0.00
#&gt; T0_CT_E06      2  0.0000      0.995 0.00 1.00 0.00
#&gt; T0_CT_E07      1  0.2537      0.912 0.92 0.00 0.08
#&gt; T0_CT_E08      3  0.0892      0.914 0.02 0.00 0.98
#&gt; T0_CT_E09      2  0.2537      0.917 0.00 0.92 0.08
#&gt; T0_CT_E10      2  0.0000      0.995 0.00 1.00 0.00
#&gt; T0_CT_E11      1  0.3340      0.879 0.88 0.00 0.12
#&gt; T0_CT_E12      2  0.0000      0.995 0.00 1.00 0.00
#&gt; T0_CT_F01      1  0.0892      0.950 0.98 0.00 0.02
#&gt; T0_CT_F02      1  0.3340      0.880 0.88 0.00 0.12
#&gt; T0_CT_F03      1  0.0000      0.959 1.00 0.00 0.00
#&gt; T0_CT_F04      1  0.0000      0.959 1.00 0.00 0.00
#&gt; T0_CT_F05      2  0.0000      0.995 0.00 1.00 0.00
#&gt; T0_CT_F06      3  0.3686      0.886 0.14 0.00 0.86
#&gt; T0_CT_F07      2  0.0000      0.995 0.00 1.00 0.00
#&gt; T0_CT_F11      2  0.0000      0.995 0.00 1.00 0.00
#&gt; T0_CT_F12      1  0.0000      0.959 1.00 0.00 0.00
#&gt; T0_CT_G01      1  0.2537      0.908 0.92 0.00 0.08
#&gt; T0_CT_G02      2  0.0000      0.995 0.00 1.00 0.00
#&gt; T0_CT_G03      2  0.1529      0.956 0.04 0.96 0.00
#&gt; T0_CT_G04      3  0.0000      0.913 0.00 0.00 1.00
#&gt; T0_CT_G07      3  0.0000      0.913 0.00 0.00 1.00
#&gt; T0_CT_G08      3  0.0000      0.913 0.00 0.00 1.00
#&gt; T0_CT_G11      2  0.0000      0.995 0.00 1.00 0.00
#&gt; T0_CT_H02      2  0.0000      0.995 0.00 1.00 0.00
#&gt; T0_CT_H04      2  0.0000      0.995 0.00 1.00 0.00
#&gt; T0_CT_H05      2  0.0000      0.995 0.00 1.00 0.00
#&gt; T0_CT_H08      2  0.0000      0.995 0.00 1.00 0.00
#&gt; T0_CT_H09      2  0.0000      0.995 0.00 1.00 0.00
#&gt; T0_CT_H12      2  0.0000      0.995 0.00 1.00 0.00
#&gt; T24_CT_A05     1  0.2066      0.926 0.94 0.00 0.06
#&gt; T24_CT_B11     1  0.0000      0.959 1.00 0.00 0.00
#&gt; T24_CT_F12     3  0.4555      0.859 0.20 0.00 0.80
#&gt; T24_CT_H01     1  0.0000      0.959 1.00 0.00 0.00
#&gt; T48_CT_A07     3  0.4002      0.884 0.16 0.00 0.84
#&gt; T48_CT_E12     3  0.4796      0.832 0.22 0.00 0.78
#&gt; T48_CT_G10     1  0.0000      0.959 1.00 0.00 0.00
#&gt; T72_CT_B08     3  0.3686      0.894 0.14 0.00 0.86
#&gt; T72_CT_C11     3  0.4555      0.859 0.20 0.00 0.80
#&gt; T72_CT_G08     2  0.0000      0.995 0.00 1.00 0.00
#&gt; T72_CT_H05     3  0.3686      0.894 0.14 0.00 0.86
#&gt; T72_CT_H08     3  0.4555      0.859 0.20 0.00 0.80
#&gt; T72_CT_H09     1  0.0000      0.959 1.00 0.00 0.00
</code></pre>

<script>
$('#tab-node-03-get-classes-2-a').parent().next().next().hide();
$('#tab-node-03-get-classes-2-a').click(function(){
  $('#tab-node-03-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-node-03-get-classes-3'>
<p><a id='tab-node-03-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette   p1   p2   p3   p4
#&gt; T0_CT_A01      2  0.6855      0.594 0.04 0.62 0.06 0.28
#&gt; T0_CT_A03      1  0.0707      0.820 0.98 0.00 0.02 0.00
#&gt; T0_CT_A05      1  0.0707      0.820 0.98 0.00 0.02 0.00
#&gt; T0_CT_A06      2  0.2345      0.870 0.00 0.90 0.00 0.10
#&gt; T0_CT_A07      1  0.0707      0.820 0.98 0.00 0.02 0.00
#&gt; T0_CT_A10      4  0.7414     -0.276 0.34 0.18 0.00 0.48
#&gt; T0_CT_A11      2  0.2647      0.858 0.00 0.88 0.00 0.12
#&gt; T0_CT_B01      2  0.0707      0.876 0.00 0.98 0.00 0.02
#&gt; T0_CT_B03      3  0.0000      0.756 0.00 0.00 1.00 0.00
#&gt; T0_CT_B05      2  0.2345      0.866 0.00 0.90 0.00 0.10
#&gt; T0_CT_B07      2  0.2345      0.866 0.00 0.90 0.00 0.10
#&gt; T0_CT_B08      3  0.2011      0.733 0.08 0.00 0.92 0.00
#&gt; T0_CT_B09      1  0.0707      0.820 0.98 0.00 0.02 0.00
#&gt; T0_CT_C02      3  0.0000      0.756 0.00 0.00 1.00 0.00
#&gt; T0_CT_C07      2  0.3801      0.833 0.00 0.78 0.00 0.22
#&gt; T0_CT_C08      1  0.4284      0.754 0.78 0.00 0.02 0.20
#&gt; T0_CT_C12      1  0.0707      0.820 0.98 0.00 0.02 0.00
#&gt; T0_CT_D01      2  0.2647      0.861 0.00 0.88 0.00 0.12
#&gt; T0_CT_D02      2  0.1211      0.877 0.00 0.96 0.00 0.04
#&gt; T0_CT_D03      3  0.4522      0.474 0.32 0.00 0.68 0.00
#&gt; T0_CT_D05      2  0.2011      0.868 0.00 0.92 0.00 0.08
#&gt; T0_CT_D06      3  0.1211      0.746 0.00 0.00 0.96 0.04
#&gt; T0_CT_D07      2  0.3400      0.843 0.00 0.82 0.00 0.18
#&gt; T0_CT_D08      3  0.0000      0.756 0.00 0.00 1.00 0.00
#&gt; T0_CT_D12      1  0.4642      0.740 0.74 0.00 0.02 0.24
#&gt; T0_CT_E01      3  0.4134      0.575 0.26 0.00 0.74 0.00
#&gt; T0_CT_E03      2  0.4713      0.727 0.00 0.64 0.00 0.36
#&gt; T0_CT_E06      2  0.4277      0.793 0.00 0.72 0.00 0.28
#&gt; T0_CT_E07      1  0.6110      0.672 0.66 0.00 0.10 0.24
#&gt; T0_CT_E08      3  0.2011      0.732 0.08 0.00 0.92 0.00
#&gt; T0_CT_E09      2  0.7674      0.431 0.00 0.46 0.28 0.26
#&gt; T0_CT_E10      2  0.2647      0.858 0.00 0.88 0.00 0.12
#&gt; T0_CT_E11      1  0.6649      0.572 0.56 0.00 0.10 0.34
#&gt; T0_CT_E12      2  0.3400      0.838 0.00 0.82 0.00 0.18
#&gt; T0_CT_F01      1  0.4227      0.784 0.82 0.00 0.06 0.12
#&gt; T0_CT_F02      1  0.5962      0.690 0.66 0.00 0.08 0.26
#&gt; T0_CT_F03      1  0.2335      0.817 0.92 0.00 0.02 0.06
#&gt; T0_CT_F04      1  0.4284      0.765 0.78 0.00 0.02 0.20
#&gt; T0_CT_F05      2  0.2647      0.858 0.00 0.88 0.00 0.12
#&gt; T0_CT_F06      3  0.5606      0.167 0.48 0.00 0.50 0.02
#&gt; T0_CT_F07      2  0.2011      0.868 0.00 0.92 0.00 0.08
#&gt; T0_CT_F11      2  0.2921      0.853 0.00 0.86 0.00 0.14
#&gt; T0_CT_F12      1  0.1211      0.819 0.96 0.00 0.00 0.04
#&gt; T0_CT_G01      1  0.0707      0.820 0.98 0.00 0.02 0.00
#&gt; T0_CT_G02      2  0.1211      0.874 0.00 0.96 0.00 0.04
#&gt; T0_CT_G03      2  0.5962      0.714 0.08 0.66 0.00 0.26
#&gt; T0_CT_G04      3  0.1211      0.746 0.00 0.00 0.96 0.04
#&gt; T0_CT_G07      3  0.0000      0.756 0.00 0.00 1.00 0.00
#&gt; T0_CT_G08      3  0.1211      0.741 0.00 0.00 0.96 0.04
#&gt; T0_CT_G11      2  0.0000      0.877 0.00 1.00 0.00 0.00
#&gt; T0_CT_H02      2  0.1637      0.871 0.00 0.94 0.00 0.06
#&gt; T0_CT_H04      2  0.3801      0.830 0.00 0.78 0.00 0.22
#&gt; T0_CT_H05      2  0.0707      0.876 0.00 0.98 0.00 0.02
#&gt; T0_CT_H08      2  0.2345      0.866 0.00 0.90 0.00 0.10
#&gt; T0_CT_H09      2  0.1211      0.874 0.00 0.96 0.00 0.04
#&gt; T0_CT_H12      2  0.0707      0.876 0.00 0.98 0.00 0.02
#&gt; T24_CT_A05     1  0.6879      0.486 0.52 0.02 0.06 0.40
#&gt; T24_CT_B11     1  0.0000      0.816 1.00 0.00 0.00 0.00
#&gt; T24_CT_F12     4  0.7544      0.699 0.20 0.00 0.34 0.46
#&gt; T24_CT_H01     1  0.0000      0.816 1.00 0.00 0.00 0.00
#&gt; T48_CT_A07     3  0.7869     -0.459 0.34 0.00 0.38 0.28
#&gt; T48_CT_E12     4  0.7550      0.687 0.22 0.00 0.30 0.48
#&gt; T48_CT_G10     1  0.3172      0.753 0.84 0.00 0.00 0.16
#&gt; T72_CT_B08     4  0.7581      0.687 0.20 0.00 0.36 0.44
#&gt; T72_CT_C11     4  0.7654      0.703 0.22 0.00 0.34 0.44
#&gt; T72_CT_G08     2  0.2647      0.841 0.00 0.88 0.00 0.12
#&gt; T72_CT_H05     4  0.7581      0.687 0.20 0.00 0.36 0.44
#&gt; T72_CT_H08     4  0.7654      0.703 0.22 0.00 0.34 0.44
#&gt; T72_CT_H09     4  0.6831      0.416 0.42 0.00 0.10 0.48
</code></pre>

<script>
$('#tab-node-03-get-classes-3-a').parent().next().next().hide();
$('#tab-node-03-get-classes-3-a').click(function(){
  $('#tab-node-03-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.




<script>
$( function() {
	$( '#tabs-node-03-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-node-03-consensus-heatmap'>
<ul>
<li><a href='#tab-node-03-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-node-03-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-node-03-consensus-heatmap-3'>k = 4</a></li>
</ul>
<div id='tab-node-03-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-node-03-consensus-heatmap-1-1.png" alt="plot of chunk tab-node-03-consensus-heatmap-1"/></p>

</div>
<div id='tab-node-03-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-node-03-consensus-heatmap-2-1.png" alt="plot of chunk tab-node-03-consensus-heatmap-2"/></p>

</div>
<div id='tab-node-03-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-node-03-consensus-heatmap-3-1.png" alt="plot of chunk tab-node-03-consensus-heatmap-3"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:





<script>
$( function() {
	$( '#tabs-node-03-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-node-03-membership-heatmap'>
<ul>
<li><a href='#tab-node-03-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-node-03-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-node-03-membership-heatmap-3'>k = 4</a></li>
</ul>
<div id='tab-node-03-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-node-03-membership-heatmap-1-1.png" alt="plot of chunk tab-node-03-membership-heatmap-1"/></p>

</div>
<div id='tab-node-03-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-node-03-membership-heatmap-2-1.png" alt="plot of chunk tab-node-03-membership-heatmap-2"/></p>

</div>
<div id='tab-node-03-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-node-03-membership-heatmap-3-1.png" alt="plot of chunk tab-node-03-membership-heatmap-3"/></p>

</div>
</div>

As soon as the classes for columns are determined, the signatures
that are significantly different between subgroups can be looked for. 
Following are the heatmaps for signatures.




Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-node-03-get-signatures' ).tabs();
} );
</script>
<div id='tabs-node-03-get-signatures'>
<ul>
<li><a href='#tab-node-03-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-node-03-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-node-03-get-signatures-3'>k = 4</a></li>
</ul>
<div id='tab-node-03-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-node-03-get-signatures-1-1.png" alt="plot of chunk tab-node-03-get-signatures-1"/></p>

</div>
<div id='tab-node-03-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-node-03-get-signatures-2-1.png" alt="plot of chunk tab-node-03-get-signatures-2"/></p>

</div>
<div id='tab-node-03-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-node-03-get-signatures-3-1.png" alt="plot of chunk tab-node-03-get-signatures-3"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-node-03-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-node-03-get-signatures-no-scale'>
<ul>
<li><a href='#tab-node-03-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-node-03-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-node-03-get-signatures-no-scale-3'>k = 4</a></li>
</ul>
<div id='tab-node-03-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-node-03-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-node-03-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-node-03-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-node-03-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-node-03-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-node-03-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-node-03-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-node-03-get-signatures-no-scale-3"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk node-03-signature_compare](figure_cola/node-03-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. To get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows (which is done by automatically selecting number of clusters).

If there are too many signatures, `top_signatures = ...` can be set to only show the 
signatures with the highest FDRs:

```r
# code only for demonstration
# e.g. to show the top 500 most significant rows
tb = get_signature(res, k = ..., top_signatures = 500)
```

If the signatures are defined as these which are uniquely high in current group, `diff_method` argument
can be set to `"uniquely_high_in_one_group"`:

```r
# code only for demonstration
tb = get_signature(res, k = ..., diff_method = "uniquely_high_in_one_group")
```




UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-node-03-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-node-03-dimension-reduction'>
<ul>
<li><a href='#tab-node-03-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-node-03-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-node-03-dimension-reduction-3'>k = 4</a></li>
</ul>
<div id='tab-node-03-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-node-03-dimension-reduction-1-1.png" alt="plot of chunk tab-node-03-dimension-reduction-1"/></p>

</div>
<div id='tab-node-03-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-node-03-dimension-reduction-2-1.png" alt="plot of chunk tab-node-03-dimension-reduction-2"/></p>

</div>
<div id='tab-node-03-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-node-03-dimension-reduction-3-1.png" alt="plot of chunk tab-node-03-dimension-reduction-3"/></p>

</div>
</div>



Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk node-03-collect-classes](figure_cola/node-03-collect-classes-1.png)




Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n_sample Hours(p-value) Media(p-value) State(p-value) k
#> ATC:skmeans       69       3.34e-02       9.92e-03          0.895 2
#> ATC:skmeans       69       3.53e-02       1.19e-02          0.497 3
#> ATC:skmeans       62       1.24e-06       6.19e-08          0.745 4
```




If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](https://jokergoo.github.io/cola_vignettes/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### Node031


Parent node: [Node03](#Node03).
Child nodes: 
                Node0121-leaf
        ,
                Node0122-leaf
        ,
                Node0123-leaf
        ,
                Node0311-leaf
        ,
                Node0312-leaf
        ,
                Node0313-leaf
        .







The object with results only for a single top-value method and a single partitioning method 
can be extracted as:

```r
res = res_rh["031"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4.
#>   On a matrix with 11354 rows and 39 columns.
#>   Top rows (1040) are extracted by 'ATC' method.
#>   Subgroups are detected by 'skmeans' method.
#>   Performed in total 150 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_partitions"     
#>  [7] "compare_signatures"      "consensus_heatmap"       "dimension_reduction"    
#> [10] "functional_enrichment"   "get_anno_col"            "get_anno"               
#> [13] "get_classes"             "get_consensus"           "get_matrix"             
#> [16] "get_membership"          "get_param"               "get_signatures"         
#> [19] "get_stats"               "is_best_k"               "is_stable_k"            
#> [22] "membership_heatmap"      "ncol"                    "nrow"                   
#> [25] "plot_ecdf"               "predict_classes"         "rownames"               
#> [28] "select_partition_number" "show"                    "suggest_best_k"         
#> [31] "test_to_known_factors"   "top_rows_heatmap"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of subgroups)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk node-031-collect-plots](figure_cola/node-031-collect-plots-1.png)

The plots are:

- The first row: a plot of the eCDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- eCDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus subgroup labels in all
  partitions.
- Area increased. Denote $A_k$ as the area under the eCDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](https://jokergoo.github.io/cola_vignettes/cola.html#toc_13).

Generally speaking, higher 1-PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk node-031-select-partition-number](figure_cola/node-031-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.979       0.981          0.512 0.487   0.487
#> 3 3 1.000           0.991       0.996          0.257 0.852   0.701
#> 4 4 0.896           0.908       0.951          0.178 0.887   0.683
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 3
#> attr(,"optional")
#> [1] 2
```

There is also optional best $k$ = 2 that is worth to check.

Following is the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall subgroup
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-node-031-get-classes' ).tabs();
} );
</script>
<div id='tabs-node-031-get-classes'>
<ul>
<li><a href='#tab-node-031-get-classes-1'>k = 2</a></li>
<li><a href='#tab-node-031-get-classes-2'>k = 3</a></li>
<li><a href='#tab-node-031-get-classes-3'>k = 4</a></li>
</ul>

<div id='tab-node-031-get-classes-1'>
<p><a id='tab-node-031-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette   p1   p2
#&gt; T0_CT_A03      2   0.242      0.982 0.04 0.96
#&gt; T0_CT_A05      2   0.000      0.974 0.00 1.00
#&gt; T0_CT_A07      2   0.000      0.974 0.00 1.00
#&gt; T0_CT_B03      1   0.000      0.984 1.00 0.00
#&gt; T0_CT_B08      1   0.000      0.984 1.00 0.00
#&gt; T0_CT_B09      2   0.000      0.974 0.00 1.00
#&gt; T0_CT_C02      1   0.000      0.984 1.00 0.00
#&gt; T0_CT_C08      2   0.242      0.982 0.04 0.96
#&gt; T0_CT_C12      2   0.242      0.982 0.04 0.96
#&gt; T0_CT_D03      1   0.000      0.984 1.00 0.00
#&gt; T0_CT_D06      1   0.000      0.984 1.00 0.00
#&gt; T0_CT_D08      1   0.000      0.984 1.00 0.00
#&gt; T0_CT_D12      2   0.242      0.982 0.04 0.96
#&gt; T0_CT_E01      1   0.000      0.984 1.00 0.00
#&gt; T0_CT_E07      2   0.242      0.982 0.04 0.96
#&gt; T0_CT_E08      1   0.000      0.984 1.00 0.00
#&gt; T0_CT_E11      2   0.242      0.982 0.04 0.96
#&gt; T0_CT_F01      2   0.242      0.982 0.04 0.96
#&gt; T0_CT_F02      2   0.242      0.982 0.04 0.96
#&gt; T0_CT_F03      2   0.242      0.982 0.04 0.96
#&gt; T0_CT_F04      2   0.242      0.982 0.04 0.96
#&gt; T0_CT_F06      1   0.000      0.984 1.00 0.00
#&gt; T0_CT_F12      2   0.242      0.982 0.04 0.96
#&gt; T0_CT_G01      2   0.242      0.982 0.04 0.96
#&gt; T0_CT_G04      1   0.000      0.984 1.00 0.00
#&gt; T0_CT_G07      1   0.000      0.984 1.00 0.00
#&gt; T0_CT_G08      1   0.000      0.984 1.00 0.00
#&gt; T24_CT_A05     2   0.000      0.974 0.00 1.00
#&gt; T24_CT_B11     2   0.000      0.974 0.00 1.00
#&gt; T24_CT_F12     1   0.242      0.972 0.96 0.04
#&gt; T24_CT_H01     2   0.000      0.974 0.00 1.00
#&gt; T48_CT_A07     1   0.242      0.972 0.96 0.04
#&gt; T48_CT_E12     1   0.242      0.972 0.96 0.04
#&gt; T48_CT_G10     2   0.000      0.974 0.00 1.00
#&gt; T72_CT_B08     1   0.242      0.972 0.96 0.04
#&gt; T72_CT_C11     1   0.242      0.972 0.96 0.04
#&gt; T72_CT_H05     1   0.242      0.972 0.96 0.04
#&gt; T72_CT_H08     1   0.242      0.972 0.96 0.04
#&gt; T72_CT_H09     2   0.000      0.974 0.00 1.00
</code></pre>

<script>
$('#tab-node-031-get-classes-1-a').parent().next().next().hide();
$('#tab-node-031-get-classes-1-a').click(function(){
  $('#tab-node-031-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-node-031-get-classes-2'>
<p><a id='tab-node-031-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette   p1   p2 p3
#&gt; T0_CT_A03      2     0.0       0.99 0.00 1.00  0
#&gt; T0_CT_A05      2     0.0       0.99 0.00 1.00  0
#&gt; T0_CT_A07      2     0.0       0.99 0.00 1.00  0
#&gt; T0_CT_B03      1     0.0       1.00 1.00 0.00  0
#&gt; T0_CT_B08      1     0.0       1.00 1.00 0.00  0
#&gt; T0_CT_B09      2     0.0       0.99 0.00 1.00  0
#&gt; T0_CT_C02      1     0.0       1.00 1.00 0.00  0
#&gt; T0_CT_C08      2     0.0       0.99 0.00 1.00  0
#&gt; T0_CT_C12      2     0.0       0.99 0.00 1.00  0
#&gt; T0_CT_D03      1     0.0       1.00 1.00 0.00  0
#&gt; T0_CT_D06      1     0.0       1.00 1.00 0.00  0
#&gt; T0_CT_D08      1     0.0       1.00 1.00 0.00  0
#&gt; T0_CT_D12      2     0.0       0.99 0.00 1.00  0
#&gt; T0_CT_E01      1     0.0       1.00 1.00 0.00  0
#&gt; T0_CT_E07      2     0.0       0.99 0.00 1.00  0
#&gt; T0_CT_E08      1     0.0       1.00 1.00 0.00  0
#&gt; T0_CT_E11      2     0.4       0.81 0.16 0.84  0
#&gt; T0_CT_F01      2     0.0       0.99 0.00 1.00  0
#&gt; T0_CT_F02      2     0.0       0.99 0.00 1.00  0
#&gt; T0_CT_F03      2     0.0       0.99 0.00 1.00  0
#&gt; T0_CT_F04      2     0.0       0.99 0.00 1.00  0
#&gt; T0_CT_F06      1     0.0       1.00 1.00 0.00  0
#&gt; T0_CT_F12      2     0.0       0.99 0.00 1.00  0
#&gt; T0_CT_G01      2     0.0       0.99 0.00 1.00  0
#&gt; T0_CT_G04      1     0.0       1.00 1.00 0.00  0
#&gt; T0_CT_G07      1     0.0       1.00 1.00 0.00  0
#&gt; T0_CT_G08      1     0.0       1.00 1.00 0.00  0
#&gt; T24_CT_A05     2     0.0       0.99 0.00 1.00  0
#&gt; T24_CT_B11     2     0.0       0.99 0.00 1.00  0
#&gt; T24_CT_F12     3     0.0       1.00 0.00 0.00  1
#&gt; T24_CT_H01     2     0.0       0.99 0.00 1.00  0
#&gt; T48_CT_A07     3     0.0       1.00 0.00 0.00  1
#&gt; T48_CT_E12     3     0.0       1.00 0.00 0.00  1
#&gt; T48_CT_G10     2     0.0       0.99 0.00 1.00  0
#&gt; T72_CT_B08     3     0.0       1.00 0.00 0.00  1
#&gt; T72_CT_C11     3     0.0       1.00 0.00 0.00  1
#&gt; T72_CT_H05     3     0.0       1.00 0.00 0.00  1
#&gt; T72_CT_H08     3     0.0       1.00 0.00 0.00  1
#&gt; T72_CT_H09     3     0.0       1.00 0.00 0.00  1
</code></pre>

<script>
$('#tab-node-031-get-classes-2-a').parent().next().next().hide();
$('#tab-node-031-get-classes-2-a').click(function(){
  $('#tab-node-031-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-node-031-get-classes-3'>
<p><a id='tab-node-031-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette   p1   p2 p3   p4
#&gt; T0_CT_A03      2  0.3801      0.777 0.00 0.78  0 0.22
#&gt; T0_CT_A05      4  0.0707      0.916 0.00 0.02  0 0.98
#&gt; T0_CT_A07      4  0.3172      0.767 0.00 0.16  0 0.84
#&gt; T0_CT_B03      1  0.0000      0.990 1.00 0.00  0 0.00
#&gt; T0_CT_B08      1  0.0000      0.990 1.00 0.00  0 0.00
#&gt; T0_CT_B09      4  0.0000      0.923 0.00 0.00  0 1.00
#&gt; T0_CT_C02      1  0.0000      0.990 1.00 0.00  0 0.00
#&gt; T0_CT_C08      2  0.0000      0.862 0.00 1.00  0 0.00
#&gt; T0_CT_C12      2  0.3975      0.755 0.00 0.76  0 0.24
#&gt; T0_CT_D03      1  0.0000      0.990 1.00 0.00  0 0.00
#&gt; T0_CT_D06      1  0.0000      0.990 1.00 0.00  0 0.00
#&gt; T0_CT_D08      1  0.0000      0.990 1.00 0.00  0 0.00
#&gt; T0_CT_D12      2  0.1211      0.848 0.00 0.96  0 0.04
#&gt; T0_CT_E01      1  0.1637      0.937 0.94 0.06  0 0.00
#&gt; T0_CT_E07      2  0.0000      0.862 0.00 1.00  0 0.00
#&gt; T0_CT_E08      1  0.0000      0.990 1.00 0.00  0 0.00
#&gt; T0_CT_E11      2  0.0000      0.862 0.00 1.00  0 0.00
#&gt; T0_CT_F01      2  0.2345      0.851 0.00 0.90  0 0.10
#&gt; T0_CT_F02      2  0.0000      0.862 0.00 1.00  0 0.00
#&gt; T0_CT_F03      2  0.1637      0.858 0.00 0.94  0 0.06
#&gt; T0_CT_F04      2  0.2011      0.830 0.00 0.92  0 0.08
#&gt; T0_CT_F06      1  0.1211      0.957 0.96 0.04  0 0.00
#&gt; T0_CT_F12      2  0.4994      0.274 0.00 0.52  0 0.48
#&gt; T0_CT_G01      2  0.3610      0.790 0.00 0.80  0 0.20
#&gt; T0_CT_G04      1  0.0000      0.990 1.00 0.00  0 0.00
#&gt; T0_CT_G07      1  0.0000      0.990 1.00 0.00  0 0.00
#&gt; T0_CT_G08      1  0.0000      0.990 1.00 0.00  0 0.00
#&gt; T24_CT_A05     4  0.3172      0.790 0.00 0.16  0 0.84
#&gt; T24_CT_B11     4  0.0707      0.923 0.00 0.02  0 0.98
#&gt; T24_CT_F12     3  0.0000      1.000 0.00 0.00  1 0.00
#&gt; T24_CT_H01     4  0.0000      0.923 0.00 0.00  0 1.00
#&gt; T48_CT_A07     3  0.0000      1.000 0.00 0.00  1 0.00
#&gt; T48_CT_E12     3  0.0000      1.000 0.00 0.00  1 0.00
#&gt; T48_CT_G10     4  0.0707      0.923 0.00 0.02  0 0.98
#&gt; T72_CT_B08     3  0.0000      1.000 0.00 0.00  1 0.00
#&gt; T72_CT_C11     3  0.0000      1.000 0.00 0.00  1 0.00
#&gt; T72_CT_H05     3  0.0000      1.000 0.00 0.00  1 0.00
#&gt; T72_CT_H08     3  0.0000      1.000 0.00 0.00  1 0.00
#&gt; T72_CT_H09     3  0.0000      1.000 0.00 0.00  1 0.00
</code></pre>

<script>
$('#tab-node-031-get-classes-3-a').parent().next().next().hide();
$('#tab-node-031-get-classes-3-a').click(function(){
  $('#tab-node-031-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.




<script>
$( function() {
	$( '#tabs-node-031-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-node-031-consensus-heatmap'>
<ul>
<li><a href='#tab-node-031-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-node-031-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-node-031-consensus-heatmap-3'>k = 4</a></li>
</ul>
<div id='tab-node-031-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-node-031-consensus-heatmap-1-1.png" alt="plot of chunk tab-node-031-consensus-heatmap-1"/></p>

</div>
<div id='tab-node-031-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-node-031-consensus-heatmap-2-1.png" alt="plot of chunk tab-node-031-consensus-heatmap-2"/></p>

</div>
<div id='tab-node-031-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-node-031-consensus-heatmap-3-1.png" alt="plot of chunk tab-node-031-consensus-heatmap-3"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:





<script>
$( function() {
	$( '#tabs-node-031-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-node-031-membership-heatmap'>
<ul>
<li><a href='#tab-node-031-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-node-031-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-node-031-membership-heatmap-3'>k = 4</a></li>
</ul>
<div id='tab-node-031-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-node-031-membership-heatmap-1-1.png" alt="plot of chunk tab-node-031-membership-heatmap-1"/></p>

</div>
<div id='tab-node-031-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-node-031-membership-heatmap-2-1.png" alt="plot of chunk tab-node-031-membership-heatmap-2"/></p>

</div>
<div id='tab-node-031-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-node-031-membership-heatmap-3-1.png" alt="plot of chunk tab-node-031-membership-heatmap-3"/></p>

</div>
</div>

As soon as the classes for columns are determined, the signatures
that are significantly different between subgroups can be looked for. 
Following are the heatmaps for signatures.




Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-node-031-get-signatures' ).tabs();
} );
</script>
<div id='tabs-node-031-get-signatures'>
<ul>
<li><a href='#tab-node-031-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-node-031-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-node-031-get-signatures-3'>k = 4</a></li>
</ul>
<div id='tab-node-031-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-node-031-get-signatures-1-1.png" alt="plot of chunk tab-node-031-get-signatures-1"/></p>

</div>
<div id='tab-node-031-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-node-031-get-signatures-2-1.png" alt="plot of chunk tab-node-031-get-signatures-2"/></p>

</div>
<div id='tab-node-031-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-node-031-get-signatures-3-1.png" alt="plot of chunk tab-node-031-get-signatures-3"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-node-031-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-node-031-get-signatures-no-scale'>
<ul>
<li><a href='#tab-node-031-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-node-031-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-node-031-get-signatures-no-scale-3'>k = 4</a></li>
</ul>
<div id='tab-node-031-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-node-031-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-node-031-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-node-031-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-node-031-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-node-031-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-node-031-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-node-031-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-node-031-get-signatures-no-scale-3"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk node-031-signature_compare](figure_cola/node-031-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. To get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows (which is done by automatically selecting number of clusters).

If there are too many signatures, `top_signatures = ...` can be set to only show the 
signatures with the highest FDRs:

```r
# code only for demonstration
# e.g. to show the top 500 most significant rows
tb = get_signature(res, k = ..., top_signatures = 500)
```

If the signatures are defined as these which are uniquely high in current group, `diff_method` argument
can be set to `"uniquely_high_in_one_group"`:

```r
# code only for demonstration
tb = get_signature(res, k = ..., diff_method = "uniquely_high_in_one_group")
```




UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-node-031-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-node-031-dimension-reduction'>
<ul>
<li><a href='#tab-node-031-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-node-031-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-node-031-dimension-reduction-3'>k = 4</a></li>
</ul>
<div id='tab-node-031-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-node-031-dimension-reduction-1-1.png" alt="plot of chunk tab-node-031-dimension-reduction-1"/></p>

</div>
<div id='tab-node-031-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-node-031-dimension-reduction-2-1.png" alt="plot of chunk tab-node-031-dimension-reduction-2"/></p>

</div>
<div id='tab-node-031-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-node-031-dimension-reduction-3-1.png" alt="plot of chunk tab-node-031-dimension-reduction-3"/></p>

</div>
</div>



Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk node-031-collect-classes](figure_cola/node-031-collect-classes-1.png)




Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n_sample Hours(p-value) Media(p-value) State(p-value) k
#> ATC:skmeans       39       3.28e-01       6.50e-01             NA 2
#> ATC:skmeans       39       1.13e-05       5.63e-06             NA 3
#> ATC:skmeans       38       2.36e-06       1.34e-06             NA 4
```




If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](https://jokergoo.github.io/cola_vignettes/functional_enrichment.html) for more detailed explanations.


 

## Session info


```r
sessionInfo()
```

```
#> R version 4.1.0 (2021-05-18)
#> Platform: x86_64-pc-linux-gnu (64-bit)
#> Running under: CentOS Linux 7 (Core)
#> 
#> Matrix products: default
#> BLAS/LAPACK: /usr/lib64/libopenblas-r0.3.3.so
#> 
#> locale:
#>  [1] LC_CTYPE=en_US.UTF-8       LC_NUMERIC=C               LC_TIME=en_US.UTF-8       
#>  [4] LC_COLLATE=en_US.UTF-8     LC_MONETARY=en_US.UTF-8    LC_MESSAGES=en_US.UTF-8   
#>  [7] LC_PAPER=en_US.UTF-8       LC_NAME=C                  LC_ADDRESS=C              
#> [10] LC_TELEPHONE=C             LC_MEASUREMENT=en_US.UTF-8 LC_IDENTIFICATION=C       
#> 
#> attached base packages:
#> [1] grid      stats     graphics  grDevices utils     datasets  methods   base     
#> 
#> other attached packages:
#> [1] genefilter_1.74.0     ComplexHeatmap_2.8.0  markdown_1.1          knitr_1.33           
#> [5] HSMMSingleCell_1.12.0 RColorBrewer_1.1-2    cola_1.9.4           
#> 
#> loaded via a namespace (and not attached):
#>   [1] colorspace_2.0-2       rjson_0.2.20           ellipsis_0.3.2         mclust_5.4.7          
#>   [5] circlize_0.4.13        XVector_0.32.0         GlobalOptions_0.1.2    clue_0.3-59           
#>   [9] rstudioapi_0.13        bit64_4.0.5            AnnotationDbi_1.54.1   Polychrome_1.3.1      
#>  [13] RSpectra_0.16-0        fansi_0.5.0            xml2_1.3.2             codetools_0.2-18      
#>  [17] splines_4.1.0          doParallel_1.0.16      cachem_1.0.5           impute_1.66.0         
#>  [21] polyclip_1.10-0        jsonlite_1.7.2         Cairo_1.5-12.2         umap_0.2.7.0          
#>  [25] annotate_1.70.0        cluster_2.1.2          png_0.1-7              data.tree_1.0.0       
#>  [29] compiler_4.1.0         httr_1.4.2             assertthat_0.2.1       Matrix_1.3-4          
#>  [33] fastmap_1.1.0          tools_4.1.0            gtable_0.3.0           glue_1.4.2            
#>  [37] GenomeInfoDbData_1.2.6 dplyr_1.0.7            Rcpp_1.0.7             slam_0.1-48           
#>  [41] Biobase_2.52.0         eulerr_6.1.0           vctrs_0.3.8            Biostrings_2.60.1     
#>  [45] iterators_1.0.13       polylabelr_0.2.0       xfun_0.24              stringr_1.4.0         
#>  [49] lifecycle_1.0.0        irlba_2.3.3            XML_3.99-0.6           dendextend_1.15.1     
#>  [53] zlibbioc_1.38.0        scales_1.1.1           microbenchmark_1.4-7   parallel_4.1.0        
#>  [57] memoise_2.0.0          reticulate_1.20        gridExtra_2.3          ggplot2_3.3.5         
#>  [61] stringi_1.7.3          RSQLite_2.2.7          highr_0.9              S4Vectors_0.30.0      
#>  [65] foreach_1.5.1          BiocGenerics_0.38.0    shape_1.4.6            GenomeInfoDb_1.28.1   
#>  [69] rlang_0.4.11           pkgconfig_2.0.3        matrixStats_0.59.0     bitops_1.0-7          
#>  [73] evaluate_0.14          lattice_0.20-44        purrr_0.3.4            bit_4.0.4             
#>  [77] tidyselect_1.1.1       magrittr_2.0.1         R6_2.5.0               IRanges_2.26.0        
#>  [81] generics_0.1.0         DBI_1.1.1              pillar_1.6.1           survival_3.2-11       
#>  [85] KEGGREST_1.32.0        scatterplot3d_0.3-41   RCurl_1.98-1.3         tibble_3.1.2          
#>  [89] crayon_1.4.1           utf8_1.2.1             skmeans_0.2-13         viridis_0.6.1         
#>  [93] GetoptLong_1.0.5       blob_1.2.1             digest_0.6.27          xtable_1.8-4          
#>  [97] brew_1.0-6             openssl_1.4.4          stats4_4.1.0           munsell_0.5.0         
#> [101] viridisLite_0.4.0      askpass_1.1
```




