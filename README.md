## The processing procedure
1. When searching for barcode, using the fist the first 22 and the last 11 bp in the 50bp-long read as filter with **2** bp of mismatch allowed.  

2. In each barcode, any bases with quality score < **20** was labeled as 'N'. Any barcode with > **13** 'N' is _**discarded**_ as we can't be sure of its origin by mapping to other reads. (each barcode is 17 bp long)

3. Within each cell, the pair-wise Levenshtein distance of all the barcodes were calculated, only considering all the positions that is not 'N' for either barcodes.

4. In each cell, cluster the barcodes so that any pair-wise distance within one cluster is **<=2**.  After clustering, calculate the "true" barcode of each cluster in following manner: for each position in the barcode, pick the most commonly seen non-'N' nucleotide. Therefore, each cell is hence represented by this list of "true" barcode, one from each cluster.

5. For each cell, further _**trim**_ this list of "true" barcode by following two criteria to increase the mapping confidence in the next steps. 
	* with **<33%** of the 17bp as 'N'
	* with **>= 2** reads 

6. Calculate the pair-wise distance of all the valid cells (i.e. cells with at least one valid true barcode).  For any pair of cells, if at least one barcode in cell A and one barcode in cell B has a distance smaller than 0, which is calculated in the same way in (2),  the distance between these two cell is 0. Otherwise, it is 1.

7. Perform hierarchical clustering on the cell using the distance matrix obtain in (5) so that the closest distance between two cluster >= 1.  The closest distance of two cluster is calculated as the smallest distance between any two cell in these two cluster.

## Visualization procedure
1. Pick only the cells of interest (lineages with >=2 cells) 

2. Normalize the expression data
	* For each cell, divide the counts for each gene by the total counts in this cell
	* For each gene, normalize the counts across cells to be with mean of 0 and standard deviation of 1

3. Perform PCA / t-SNE / Isomap visualization 
