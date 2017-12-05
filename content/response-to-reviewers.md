# Response to Reviewers

We submitted [preprint version 2](https://doi.org/10.7287/peerj.preprints.3100v2) of the Sci-Hub Coverage Study to the journal [_eLife_](https://elifesciences.org/).
We have now received the peer reviews and are working on addressing the comments.
When submitting a pull request to revise the Deep Review in response to reviewers, please also comment on the relevant section in this document.
The comments should respond to the reviewer comments and should link to the pull requests or issues where they are addressed.
After each reviewer criticism, there is a TODO.
Replace the TODO with the relevant response.
Feel free to also break up paragraphs as necessary, but remember to keep the reviews formatted as blockquotes.

***

## SUMMARY

> This is an interesting and original piece of work that focusses on one of the most potentially disruptive forces in scholarly communication in the 21st Century.
It outlines, for I think the first time, the shear size and scope of SciHub and the amount of material that is now available through it.
It is perhaps a little early to say with any certainly what the effect of SciHub will be on subscriptions and I am pleased that the authors have highlighted the issue, but not moved too far into the space of prediction.
They have allowed the analysis to speak for itself.

TODO: briefly summarize additional major changes since version 2 that are part of the `preprint-v3` [milestone](https://github.com/greenelab/scihub-manuscript/milestone/2).

We updated our journal information to the October 2017 release of Scopus.
In addition, we created patches to standardize publisher names in Scopus.
As a result, many of the numbers reported in the manuscript have changed slightly.

## ESSENTIAL REVISIONS

> 1. The authors make one claim that seems to me not supported by the evidence.
They claim that their paper shows that toll-based publishing is becoming unsustainable.
But they also point to a recent study that estimates that the ratio of the number of times papers are downloaded from the publisher to downloaded from SciHub is 48:1 for Elsevier and 20:1 for Royal Society of Chemistry.
This suggests that SciHub so far has very little influence on the subscription demand for journal articles.

TODO

> 2. To calculate coverage, DOIs from Crossref and SciHub for over 56M records are compared.
Please give details of the algorithm used to compare the DOIs.
>
> A straight comparison of 56M DOI's with another similarly large number of DOIs would require at least 2.5 X 10*14 comparisons.
Was it done this way?
If so, how long did it take?
Was another clustering method used?
>
> And if such comparisons were made, why are the percentages of coverage only given to 3rd order?
Is this just for convenience?
(Giving the percentages of coverage to 3rd order seems to imply a sample and not a complete comparison).

We thank the reviewers for their interest in the computational methods used for our coverage calculations.
Python [includes](https://docs.python.org/3.6/tutorial/datastructures.html#sets) a set data type to store unordered collections with no duplicate elements.
Set operations [enable](https://wiki.python.org/moin/TimeComplexity#set) efficient membership testing and intersection with other sets.
As a result, we do not need to apply a list intersection algorithm, as described by the reviewers, that would scale quadratically due to nested iteration.

We evaluate each Crossref DOI for membership in the set of Sci-Hub DOIs in [`01.catalog-dois.ipynb`](https://github.com/greenelab/scihub/blob/ca4d523e149f30be7fd3d3ae6551a26d1c625313/01.catalog-dois.ipynb).
The specific implementation we apply passes a set of Sci-Hub DOIs to the [`pandas.Series.isin`](http://pandas.pydata.org/pandas-docs/version/0.20.1/generated/pandas.Series.isin.html) function.
This function [uses](https://github.com/pandas-dev/pandas/blob/v0.20.1/pandas/core/algorithms.py#L399) a hash table, the formative data structure behind sets, to enable an efficient comparison of DOIs.
The `01.catalog-dois.ipynb` notebook does take considerable time (perhaps up to an hour) to execute.
However, much of this time is spend on file input/output, which requires reading/writing to disk and (de)compressing streams, both of which can be runtime intensive.

We create a tabular file [`doi.tsv.xz`](https://github.com/greenelab/scihub/blob/ca4d523e149f30be7fd3d3ae6551a26d1c625313/data/doi.tsv.xz) that contains a binary indicator (coded as 0/1) of whether each Crossref DOI is in Sci-Hub's repository.
The general workflow we followed to compute coverage was to subset the DOI dataset for the relevant DOIs (e.g. all DOIs published in 2016) and take the sum/mean of the binary membership column.
These operations made heavy use of [`pandas.DataFrame.join`](http://pandas.pydata.org/pandas-docs/version/0.20.1/generated/pandas.DataFrame.join.html) to incorporate additional information about DOIs and [`pandas.DataFrame.groupby`](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.groupby.html) to compute coverage for all possible subsets at once.

> 3. I believe if the authors compare Figure 4 with Table 2 in Reference 1, a strong negative correlation can be observed whereby the more likely a paper is to be found on the web, the less likely it is in SciHub.

We believe this comment is comparing [Figure 4](https://peerj.com/preprints/3119v1.pdf#page=16) of the State of OA study (titled "Percentage of different access types … per NSF discipline", also see [Figure A5](https://peerj.com/preprints/3119v1.pdf#page=32)) with [Figure 4](https://peerj.com/preprints/3100v2.pdf#page=10) of our study (titled "Coverage by journal attributes").
For example, among plotted disciplines, Sci-Hub coverage is highest for chemistry, whearas oaDOI's coverage is lowest for chemistry.
On the other hand, oaDOI exceeds 50% coverage of mathematics, whereas mathematics coverage is relatively low in Sci-Hub.

Regarding the hypothesis, "the more likely a paper is to be found on the web, the less likely it is in SciHub", we evaluate this directly in the "Coverage by category of access status" section.
This hypothesis appears true for most types of gratis web access.
Compared to closed articles, bronze, hybrid, and gold OA articles are less likely to be in Sci-Hub.
However, green articles (available without charge, but not from the publisher) do not seem have lower coverage than closed articles in Sci-Hub.
The conclusion from the reviewers' comparison could therefore be rephrased as "the less gratis availability of a discipline's articles on the web (according to oaDOI), the greater the coverage on Sci-Hub."
One possibility is that researchers from disciplines with poor oaDOI coverage more frequently encounter access problems, leading to greater awareness and usage of Sci-Hub.

## MINOR POINTS

> 4. The figure of 81.6 million that is given in the abstract is not mentioned/explained elsewhere in the manuscript.
Please explain where this figure comes from at an appropriate place in the manuscript.

TODO

> 5. Please note that we cannot display URLs as you have done for sci-hub.cc, sci-hub.io etc in the introduction to your article, so please use regular text for these URLs.
Also, we are unable to display text, as you have done for the passage that starts "Sci-Hub technically is by itself . . ", so again please use regular text.

TODO

> 6. Table 1 would benefit from a short caption to explain what is being shown in this table.

TODO

> 7. Figure 3 would benefit from a longer caption to better explain what is being shown in this figure.

TODO

> 8. Figure 4 would benefit from a longer caption to better explain what is being shown in this figure.

TODO

> 9. Figure 8 would benefit from a longer caption to better explain what is being shown in this figure.

TODO

> 10. eLife does not publish Supplementary Information so Figures S1, S2, S3 and S4 either need to become full figures in their own right, or supplements to existing figures.
For example, Figure S2 could become Figure 10-Figure Supplement 1.

TODO
