
Latent Dirichlet Allocation
===========================

This repository contains material for the workshop on
Latent Dirichlet Allocation (LDA) during the 2020 CANDEV Data Challenge in Ottawa.

The __slides__ folder contains the slides presented during the workshop.

In addition, this repository contains a toy (textual) data set and
a demo pipeline that applies LDA (a topic modelling technique) to the data set.

The data set consists of 6,000 abstracts of scientific articles downloaded
from https://arxiv.org (hosted by Cornell University).
The data set can be found in the __data/arXiv/__ folder.

Software reqirements of the pipeline
------------------------------------
*  R v3.6.2
*  R packages: ggplot2, gplots , text2vec, xml2, stopwords, dplyr, tidyr, ComplexHeatmap

How to execute the pipeline
---------------------------
Clone this repository by running the following at the command line:

```
git clone https://github.com/CANDEV-OTTAWA-2020-WORKSHOPS-ORG/Topic-Modelling-Latent-Dirichlet-Allocation-in-R.git
```

Change directory to the folder of this pipeline in the local cloned repository:

```
cd <LOCAL CLONED REPOSITORY>
```

If you are using a Linux or macOS computer, execute the following shell script (in order to run the full pipeline):

```
.\runall.sh
```

If you are using a Windows computer, execute the following batch script at the Command Prompt instead:

```
.\runall.bat
```

This will trigger the creation of the output folder
`<LOCAL CLONED REPOSITORY>/output/`
if it does not already exist, followed by execution of the pipeline.
All output and log files will be saved to the output folder.
See below for information about the contents of the output folder.

What LDA, and hence this pipeline, does
---------------------------------------
This pipeline performs LDA on the input data, namely the 6000 downloaded
scientific abstracts.

The pipeline starts by building a `vocabulary` based on the words that
occur in the abstracts, and it attempts to find ten `topics` to which the
abstracts may pertain, based on patterns of co-occurence of the words in
the vocabulary.

Technically, each of these putative topics is a probability distribution over
the vocabulary, and LDA assigns each document a probability distribution over
the putative topics.

Input files
-----------
The input files are located in
`<LOCAL CLONED REPOSITORY>/data/arXiv/`.
Below are descriptions of the input text files for this pipeline.

* __query-arXiv-cat-cs-LG-1000.txt__

    This XML file was retrieved from the
    __arXiv API__ (https://arxiv.org/help/api)
    on information of the most
    recent (as of September 6, 2018) 1000 arXiv articles on
    __machine learning (computer science)__.

* __query-arXiv-cat-math-AG-1000.txt__

    Similarly, but for __algebraic geometry (mathematics)__.

* __query-arXiv-cat-physics-acc-ph-1000.txt__

    Similarly, but for __accelerator physics (physics)__.

* __query-arXiv-cat-q-bio-GN-1000.txt__

    Similarly, but for __genomics (quantitative biology)__.

* __query-arXiv-cat-quant-ph-1000.txt__

    Similarly, but for __quantum physics (physics)__.

* __query-arXiv-cat-stat-ME-1000.txt__

    Similarly, but for __methodology (statistics)__.

Main Output files
-----------------

* __lda-document-topic-distributions.csv__

    A comma-delimited tabular file, each of whose rows correponds to an input
    abstract. For each document (row), the (non-negative) numbers under the
    columns with headers `Topic1`, ... , `Topic10` form the probability
    distribution over the ten putative topics for that document.
    (Note, in particular, that these non-negative numbers sum to 1 for each row.)
    The figures under the column `entropy` is the (base-2) entropy of the
    probability distribution (over the putative topics) of the corresponding
    document (row).
    The figure under the column `two_entropy` is simply 2 raised to the power
    `entropy` (which can be interpreted as the "effective number of topics"
    contained in the corresponding document).

* __lda-topic-word-distributions.csv__

    A comma-delimited tabular file, where each row correponds to a word in the
    vocabulary, and each column corresponds to a putative topic.
    The non-negative numbers under the column with header `Topic1` form the
    probability distirubtion over the vocabulary for Topic 1.
    Similarly for the other columns with headers `Topic*`.

* __lda-topic-top-words.csv__

    A comma-delimited tabular file, where each column with header `Topic*`
    corresponds to a putative topic.
    The 30 words in the column with header `Topic1` are the 30 words in the
    vocabulary having the largest probabilities with respect to the categorical
    distribution (over the vocabulary) that defines Topic 1.
    Similarly for the other columns with headers `Topic*`.

Auxiliary output files
----------------------
* __lda-correlations-topic.png__

    Correlation heatmap for the columns with headers `Topic1`, ... , `Topic10`
    in the output file __lda-document-topic-distributions.csv__.
    Note that the ten putative topics are largely uncorrelated.

* __lda-correlations-topic-domain.png__

    Correlation heatmap for the columns with headers `Topic1`, ... , `Topic10`
    in the output file __lda-document-topic-distributions.csv__
    against the one-hot encoding variables generated from the `domain` column.

* __lda-correlations-all.png__

    Correlation heatmap for the columns with headers `Topic1`, ... , `Topic10`
    in the output file __lda-document-topic-distributions.csv__
    as well as the one-hot encoding variables generated from the `domain` column.

* __query-arXiv-cat-cs-LG-1000.csv__

    Comma-delimited file containing the ID, domain, title and summary
    (abstract) in tabular format of the articles in the input XML file
    __query-arXiv-cat-cs-LG-1000.txt__.

* __query-arXiv-cat-math-AG-1000.csv__
* __query-arXiv-cat-physics-acc-ph-1000.csv__
* __query-arXiv-cat-q-bio-GN-1000.csv__
* __query-arXiv-cat-quant-ph-1000.csv__
* __query-arXiv-cat-stat-ME-1000.csv__

    Similarly, but for the rest of the input XML files.

* __stdout.R.runall__

    Standard output log files of __runall.sh__ or __runall.bat__.

* __stderr.R.runall__

    Standard error log files of __runall.sh__ or __runall.bat__.

Structure of the pipeline
-------------------------
1)  The script `runall.sh` or `runall.bat` does the following:

* Create the output folder `<LOCAL CLONED REPOSITORY>/output/`.
* Make a replica of the pipeline code in the `code` subfolder of the output
  folder (for reproducibility: each time the code is run, a snapshot is made
  of the exact version of the code executed for that run).
* Execute the `runall.R` program in the code folder.

2)  The `runall.R` program does the following (by invoking supporting R
    functions; see contents of `runall.R` as well as the pipeline's code
    subfolder):

* Extract relevant information from the input XML files and save the extracted
  data in tabular (CSV) format.
* Generate the document-term matrix for the extracted tabular data
  (scientific abstracts).
* Perform LDA on the resulting document-term matrix.
