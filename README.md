# Pairwise Graph Team

## Overview

Our team was tasked with two projects:

* Overhauling Geneweaver's GeneSet Graph tool
* Implementing novel ways of ranking genes both globally (among the entire database) and locally (within genesets)

---

### GeneSet Graph

GeneSet Graph allows the user to display a visual respresentation of the relationship between genes in interconnected GeneSets. MOAR HERRE

#### The Current Tool

The current iteration of the tool produces a graph as shown in figure 1. 

![](https://danielholt.github.io/FinalGroupMarkdown/GeneSet_Graph_1.png)
> *Figure 1*: A sample graph produced by the GeneSet Graph tool


The graph is composed of two different types of nodes. A GeneSet node which is represented by a rectangle and a Gene node which is represented by a green circle. 

#### Limitations of the current tool

* Tool takes a while
* Image processesing is server side
* No ability to modify graph without rerunning the tool
* Too much information at once with larger GeneSets

### Our tool



#### Running

We tried to have as little friction for the user as possible between the new and old systems, so the process for running the tool is the same as before.

Select at least two gene sets
![](https://danielholt.github.io/FinalGroupMarkdown/selectGeneSets.png)


Choose the desired options
![](https://danielholt.github.io/FinalGroupMarkdown/chooseOptions.png)


Once the user has selected the desired gene sets, and chosen the options they want, click run and the user will be redirected to the results page.


##### Results

The results of the tool is where a majority of the changes are visible. The user is presented with the list of gene sets (represented by rectangles), and genes (represented by circles) listed by their degree. Genes that have been emphasized are shown in yellow, otherwise they are shown in green. All genes in the result start selected.
![](https://danielholt.github.io/FinalGroupMarkdown/results.png)

The user can choose which genes they want to remove from the graph by clicking them. Selected genes are denoted by a small circle in the bottom right of the gene.
![](https://danielholt.github.io/FinalGroupMarkdown/someSelected.png)

There is a floating green button in the bottom right of the page that will scroll the graph into view.

Some shortcuts that are useful for selecting many genes at a time include clicking the number that represents the degree to select all genes with that degree, or clicking a gene set to select all genes in that set.

The user can use the mouse to drag the graph within the viewer, and use the scroll wheel to zoom in and out. There are links below the graph to export as a PDF file which will download the graph to the user's computer, and to open the graph in a new window.

Note: Removing and adding genes to the graph from this page does not re-run the tool. In the Tool Options menu, you can change options and re-run the tool.

---

## Gene Ranking

#### Current Systems
Currently, the only sortable columns on the View Geneset Details page are Default, Score, and Priority. "Default" contains either the Gene Symbol or some other abbreviations whose origins appear unclear. "Score" contains normalized data specific to the experiment itself that is described in the header (e.g. "Binary"). "Priority" is currently a blank placeholder column.

#### Our Tool
Two global rankings were implemented, Degree and Degree Rank. Additionally, three local rankings were implemented which include Frequency, Information Content and Uniqueness.

### **Local Rankings**

There are now three different local rankings the user can choose from: Frequency, Information Content (IC) and Uniqueness. Each local ranking provides the user with specific and valuable data regarding the geneset they are viewing, allowing them to extrapolate more information about each gene.

#### **_Frequency_**

Frequency is a value in the geneweaver database that is associated with each gene. The frequency for a gene is updated whenever that gene's associated geneset is added to a project. If a gene is shared between multiple genesets, that gene's frequency will be updated once for each geneset it is located in.

![](https://danielholt.github.io/FinalGroupMarkdown/frequency.jpg)

> _For example, if gene E is a part of geneset 1 and that geneset is added to a project, gene E's frequency will be increased. If gene E is also a part of geneset 2 and that geneset is added to a project, then gene E's frequency will be increased again._

#### Normalization
Frequency values within a geneset on the View Geneset Details page are normalized to see the relations between the data. The normalization also accounts for outliers that occur when certain genesets are used for demonstrations or testing purposes. These genes would have very high frequency values, therefore the normalization accounts for outliers and brings the data back to values between 0 and 1.

Normalization is calculated using the min max normalization formula:

> ![](https://danielholt.github.io/FinalGroupMarkdown/min max.jpg)

X min is the smallest frequency value in the geneset and X max is the largest frequency value. This scales data down to a range between 0 and 1 (0 being the least frequent and 1 the most frequent).

For example, if a geneset had four frequency values that equaled 8, 10, 15 and 20, then after normalization the values would be 0, 0.16, 0.25 and 1, respectively.

![](https://danielholt.github.io/FinalGroupMarkdown/min max example.png)

#### **_Information Content_**

Information content was calculated using Shannon Entropy. Entropy quantifies the amount of 'information' a variable contains.
> Lower entropy = lower probability of the gene appearing at random

Genes that do not appear frequently in many other genesets may provide specific or essential functions within that geneset. Information content allows the user to see which genes have a lower probability of appearing at random, therefore potentially making them genes of high interest within the geneset.

![](https://danielholt.github.io/FinalGroupMarkdown/Screen Shot 2017-12-05 at 2.57.20 PM.png)

Shannon entropy is calculated using the equation above, where _pi_ equals the probability of each gene.

_Pi_ is calculated as follows:

> (Number of genesets gene i appears in) / (Total number of genes in those genesets)


For example, Geneset 1 contains five genes:

![](https://danielholt.github.io/FinalGroupMarkdown/gene2.jpg)

These genes could be a part of other genesets

![](https://danielholt.github.io/FinalGroupMarkdown/genesets of genes.jpg)

> _Genes B and C are also a part of geneset 2, gene D is a part of genesets 2 and 3, and E is also a part of geneset 3_

In order to calculate a genes probability, all genes in both genesets 2 and 3 must be gathered.

![](https://danielholt.github.io/FinalGroupMarkdown/gene of genesets.jpg)

> _Genes A through K are the total number of genes in all associated genesets_

Therefore, in this example _pi_ = 2 / 11 or 0.18
