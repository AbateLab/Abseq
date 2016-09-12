# Abseq

Scripts used in the 2016 Abseq paper

##Abseq_barcode_analysis.R:


##dfsCluster.py:
Tools for filtering barcoding data. A full set of tools for barcoding analysis is available at <https://github.com/AbateLab/Barcoding>.

Run without arguments, or -h or --help for basic usage description.

Intended to cluster pcr output sequences into groups of sequences all originating from a unique pcr input sequence.

As the name suggests, dfsCluster employs a dfs over a hamming space, but can only visit nodes specified by pcr output sequences. This should work because

1. PCR in the overwhelming majority of the time will not make more than 1 error per sequence
2. The hamming space is large enough, and the error rate is low enough, that random walks originating from random points in the space will not ever come closer than 2 hamming distance of another path.

Simulations suggest assumption 1 is safe, as cases of more than 1 error per sequence merely land in already-encountered mutations, and collisions, while rare in our current setup, will be more or less common depending on PCR cycle count, number of starting sequences, and sequence length. dfsCluster provides an option to not include any clusters suspected of being a conglomerate cluster (collided) in final output.

dfsCluster reads a fasta/q file of reads (nonunique), groups reads into barcode groups (unique), removes barcode groups with fewer reads than a specified value (default 1), and groups the barcodes into larger clusters assigned to a "center", or the sequence composed of the majority base at each position in the barcodes in the cluster. dfsCluster then creates a jackpottogram (pie chart with each slice being the number of reads in a single cluster), a histogram and pie chart binned by cluster size, a histogram binned by minimum hamming distance between a center and all other centers, and a .cid file which prints all the id's of reads associated with a cluster below the cluster's center. Ex:

>\>"center1"  
[id1, id2, id3, ...]  
\>"center2"  
[id10, id11, ...]

It is recommended to run with -v (verbose), or at least save the terminal output to a file, because information such as #unique barcodes, #barcodes cut, %reads cut, #clusters formed will be offered through it.
