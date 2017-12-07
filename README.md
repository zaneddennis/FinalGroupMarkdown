Put images here!!

https://drive.google.com/drive/folders/1_bR8KmkzpvKUvn_NnVxZj_g2PNEIQseV

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

> *Figure 1*: A sample graph produced by the GeneSet Graph tool
![](https://lh6.googleusercontent.com/HADJ9I7KWlVkzCVdElOlk9KvLQd4OdrqqaUDOVJBqYocCk0q2HFU4HTrkZQvf4MibdQ8EYhAt09q2M8ebAcH=w1920-h1070-rw)


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
![](https://lh4.googleusercontent.com/26exnTSS4xvLvURRl5Nx7qc70iS_9V2Ou6dewl2WMlKdSeO0wi-UwTfpk-vpcuMuWSyMW_KbOmV9-pdXOhMW=w1920-h1070-rw)


Choose the desired options
![](https://lh5.googleusercontent.com/BoR3ZmkGcVWenQmOvQ7A2L44QDrA61LtV0iDOrRMgQQG3FtvhnLg5AvPxsPo297vVMOem_sHOty9jqwKw9Z1=w1920-h1103-rw)


Once the user has selected the desired gene sets, and chosen the options they want, click run and the user will be redirected to the results page.


##### Results

The results of the tool is where a majority of the changes are visible. The user is presented with the list of gene sets (represented by rectangles), and genes (represented by circles) listed by their degree. Genes that have been emphasized are shown in yellow, otherwise they are shown in green. All genes in the result start selected.
![](https://lh4.googleusercontent.com/Sxqdbk2txOnyKU2AhtJcYEhemAiNy5v10NTtJO6vP8MeK6I8-Q6gijWbL4ab9l0lgH-8omXldmSmFMqvWQZ5=w1920-h1103-rw)

![](https://)

The user can choose which genes they want to remove from the graph by clicking them. Selected genes are denoted by a small circle in the bottom right of the gene.
![](https://lh6.googleusercontent.com/8m0JAOxBblvtxvlepxQ1iTIv1L7UaucAwEjDDQBOUBEQBRWALKEezuS7rVJaIIBthqI3NxNgLEoTLVasHkmH=w1920-h1103-rw)

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

![](https://lh5.googleusercontent.com/WTdPC58R55MA3ULOf4l1555tOkg4McFiSeZpBG1TXCXYBKp-fsE5abuVqIIH6eoHgMQjgaVwZCrYpSqrAYRy=w2315-h1141)

> _For example, if gene E is a part of geneset 1 and that geneset is added to a project, gene E's frequency will be increased. If gene E is also a part of geneset 2 and that geneset is added to a project, then gene E's frequency will be increased again._

#### Normalization
Frequency values within a geneset on the View Geneset Details page are normalized to see the relations between the data. The normalization also accounts for outliers that occur when certain genesets are used for demonstrations or testing purposes. These genes would have very high frequency values, therefore the normalization accounts for outliers and brings the data back to values between 0 and 1.

Normalization is calculated using the min max normalization formula:

> ![](https://lh5.googleusercontent.com/Jbj6gEsddlkFYCTxqVIy4PtPtuHiv0jzN9pLXjVCJJmFYtD7ea2PpKbaOBM6-Ash1L8RuGOOMtPGXEzK2Eo_=w2315-h1141)

X min is the smallest frequency value in the geneset and X max is the largest frequency value. This scales data down to a range between 0 and 1 (0 being the least frequent and 1 the most frequent).

For example, if a geneset had four frequency values that equaled 8, 10, 15 and 20, then after normalization the values would be 0, 0.16, 0.25 and 1, respectively.

![](https://lh6.googleusercontent.com/6Z1SS566Tyf-PaxKcgQyST_Q9p_CNT4UGWAzOiFyeF6iojsZU37ma9dOau1xi6wUenFD2FLPCc70qqOKvDj1=w2315-h1141)

#### **_Information Content_**

Information content was calculated using Shannon Entropy. Entropy quantifies the amount of 'information' a variable contains.
> Lower entropy = lower probability of the gene appearing at random

Genes that do not appear frequently in many other genesets may provide specific or essential functions within that geneset. Information content allows the user to see which genes have a lower probability of appearing at random, therefore potentially making them genes of high interest within the geneset.

![](https://lh4.googleusercontent.com/exfn4DuKHqS1d5OzsEMKv1mrBaU28RjMnLCFJhMPHoxlQXxXrtkvm05NSfdt-RTklv1MmhoQNohRhoXsjfKu=w2315-h1141)

Shannon entropy is calculated using the equation above, where _pi_ equals the probability of each gene.

_Pi_ is calculated as follows:

> (Number of genesets gene i appears in) / (Total number of genes in those genesets)


For example, Geneset 1 contains five genes:

![](https://lh6.googleusercontent.com/4zNbFlnnfSwK7FSaH9zQvIFgAYX6S3hrNchg69bl5FAbU4Zx7A0TnYId1m-Ptsi7anUbAntbqK5Vk1uhJNEa=w2315-h1141)

These genes could be a part of other genesets

![](https://lh6.googleusercontent.com/hDO54KGCJkDr9rpkL4yJFt279lkfuS8Bm7czbsaellV3cYgEhGNFo9rL4nATTjRHhactTqyxuptsSa3iKlfk=w2315-h1141)

> _Genes B and C are also a part of geneset 2, gene D is a part of genesets 2 and 3, and E is also a part of geneset 3_

In order to calculate a genes probability, all genes in both genesets 2 and 3 must be gathered.

![](https://lh3.googleusercontent.com/Z6TjlYLSg1kzSkJj_uScUSkdCosxH1Aauh6QclSnh64Cv-dP_Tv_JpC15BXEHfnmuCNqO2G12f_9qH-Btl-R=w2315-h1141)

> _Genes A through K are the total number of genes in all associated genesets_

Therefore, in this example _pi_ = 2 / 11 or 0.18
