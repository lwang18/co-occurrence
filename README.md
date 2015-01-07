A Walkthrough of Co-occurrence Analyses
=============

These are R scripts used to perform co-occurrence analysis following the paper,
 [Demonstrating microbial co-occurrence pattern analyses within and between ecosystems](http://journal.frontiersin.org/Journal/10.3389/fmicb.2014.00358/full).

Pulling data from MGRAST
===========

First, we pulled data from [MGRAST](http://metagenomics.anl.gov/) using the script
 [_pulling_data_from_MGRAST_with_matR.R](https://raw.githubusercontent.com/ryanjw/co-occurrence/master/pulling_data_from_MGRAST_with_matR.R).  
This script uses APIs to pull 16S rRNA amplicon datasets from the MGRAST database. See the paper link above for a full description of data used here.  

When pulling data from MGRAST, you are required to have an authentication key that can be received after registering with the database.  The authentication 
key then goes between the ``""`` in the ``msession$setAuth("")`` command within the script.    

If problems occur when trying to pull data from MGRAST, or if you wish to skip this step, .csv's of the data can be downloaded here:  
[Data summarized by bacterial order](https://github.com/ryanjw/co-occurrence/blob/master/data/total_order_info.csv)  
[Data summarized by bacterial family](https://github.com/ryanjw/co-occurrence/blob/master/data/total_family_info.csv)

The data tables are organized as the following:

|reads |MGRASTid |trt |rep |order 1 |order 2 |order 3 |... |
|:-----|:--------|:---|:---|:-------|:-------|:-------|:---|
|1000 |mgm1234567.89  |soil |forest |3|0|50|... |
|1500 |mgm1234567.10  |soil |desert |10|1|0|... |
  

Once the data has been pulled into R using `read.csv()`, the co-occurrence analysis can begin.

Performing the Analysis
=======================

The script, [co_occurrence_pairwise_routine.R](https://raw.githubusercontent.com/ryanjw/co-occurrence/master/co_occurrence_pairwise_routine.R),
is used to perform co-occurrence analysis based on the organization of the data listed above.  It can be customized to work for any dataset where samples are rows
and colummns are sample information (which needs to be considered when creating iterators for `for` loops) along with abundance of each OTU.  

In short, this script uses nested for loops to perform pairwise correlations between each column in the dataset.  Here, a Spearman's correlation is used to avoid data transformation issues that can 
occur with these types of data.  The product of the script is a data frame called 'results' that lists the trt (i.e. the environment that the samples originate from),
the pair of OTUs, correlation coefficient, P value for statistical test, and the abundances of the OTUs.

MGRAST does also include some Eukaryotic taxa that are not part of the analysis here.  They are removed in the script as well.    

Testing for Differences in Network Topology
==========================================

One analysis that can be performed is a test to see if networks have different structures (i.e. topology).  The rationale behind this analysis is the following;
If you assume that two networks have equivalent sets of nodes (microbial taxa) you can test to see if the edges between nodes (correlation from co-occurrence analysis) from 
the two networks are different in a multivariate framework.  Using a permutational multivariate analysis of variance (PERMANOVA), we can use permutations of edges from one network 
are different from another.  The beginning portion of the script [co-occurrence_permanova_sim.R] (https://raw.githubusercontent.com/ryanjw/co-occurrence/master/co-occurrence_permanova_sim.R)







This script contains the code for running the PERMANOVA that tests for differences in community co-occurrence between ecosystems.  At the beginnning of the script there is a si

|permutation_test.R|
--------------------

This is a script that contains a permutation test based on the F-statistic.  This is a function that can be sourced into other scripts for use.  Original credit goes to Dr. Dean Adams who wrote the code for the test, while I wrapped it in a function.


[This is md practice](http://co-occurrence.readthedocs.org/en/latest/practice/)
