---
layout: page
title: Cancer Prognosis Prediction
description: A machine learning pipeline for the predictive analysis of IMC data, including a novel augmentation approach.
img: assets/img/cell-map.PNG
importance: 1
category: work
---

<h2>Introduction</h2>

Both tumour cells and the tumor microenvironemnt (TME) are important considerations when evaluating various cancers at the tissue level. With time, cancer cells evolve and are able to evade destruction by cells of the immune system, resulting in the formation of a tumor. Because of this, it's important to obtain a better understanding of the various cell types within the TME, since they may contribute to tumor progression. 

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/tme.jpg" title="tme" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Tumour microenvironment - network of blood and lymphatic vessels, extracellular matrix, and various cells surrounding a tumour including stromal cells and immune cells.
</div>

For this project, we do this using the Hyperion<sup>TM</sup> imaging system, which is an imaging mass cytometry (IMC) technology capable of high-throughput imaging of cells and proteins <i>in situ</i>. It produces multi-channel images per tissue sample, where each channel highlights the expression level of a given protein. Based on these expression levels, we can then identify the different cell types in the sample. IMC data have been used in cancer analysis studies in the past, however since it is a relatively new technology, these studies are limited and use regular statistical approaches rather than machine learning (ML) to estimate clinical outcomes. Hence, there's a much needed urgency to leverage the quantitative nature of this technology.

The goal of the task here is to develop an end-to-end pipeline for ML-based analysis of IMC multi-channel data including a novel cell-level augmentation approach to increase data diversity, hence the generalizability of ML models. To demonstrate the utility of the proposed pipeline, we studied the association of the TME with various clinical parameters in a cohort of patients with muscle invasive bladder cancer (MIBC). We designed several experiments with signatures of IMC data and classification approaches and demonstrated that the combination of cell types found within the TME show promise as a foundation for future clinical research in this field.

Note: You can find the code in [this Github repository](https://github.com/sindhurathiru/ML-IMCAnalysis).

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/overview v5.png" title="overview" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Multi-channel images are acquired from tissue samples using IMC. Individual cells within samples are segmented and labelled as cell types. Data is augmented using the proposed sector elimination approach. Feature are extracted based on cell proportion in the samples. The average performance in cancer recurrence estimation is evaluated for different feature subsets and classification models, using 4-fold cross validation. 
</div>

<h2>The Data</h2>

We used 30 tissue samples collected from 15 MIBC patients who underwent a cystectomy (removal of the bladder). For each patient, 10 binary (yes/no) clinical parameters were also collected, including smoking, lymph node spread (LN), and other prognostic features. The tissue samples were stained with 28 protein markers and imaged using the Hyperion<sup>TM</sup>, creating a multi-channel image where each channel represents the expression of a given protein. This expression information is then used by a pathologist to identify the cell types within each sample, explained in detail in <i>Data Pre-Processing</i>. The proportions of each cell type for a given sample are then represented as a vector and form the features that will be used for ML experiments.

<h2>Data Pre-Processing</h2>

In order to get the protein expression of each individual cell, we first need to segment the cells. To do this, we used the IMC processing and analysis software I developed, [TITAN](https://sindhurathiru.github.io/projects/2_project/). Once the cells were segmented and we got a resulting cell mask for each sample, we identified the average protein expression for each protein within each cell. This represents the expression of the given protein for that cell, meaning each cell was now represented by 28 protein expressions.

Once we have the overall protein expression information for each cell, we can use that information to identify the type of each cell. For example, if one cell expresses a large amount of the protein CD8, it likely means that the cell is a CD8 T cell. To do this, we used [PhenoGraph clustering](https://www.sciencedirect.com/science/article/pii/S0092867415006376) to identify groups of cells within a sample that have similar protein expression. With this, millions of cells can be represented within tens of clusters. Then, using the resulting clusters, a pathologist annotates the cell type of each cluster based on the protein expressions. 

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/cell seg.png" title="cell seg" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/phenoclus.jpg" title="phenograph map" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/cellannot.png" title="cell annotation" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    in order to get the protein expression for individual cells, we first segment the data to create the cell mask. From there, we use PhenoGraph to cluster cells that have similar expression levels. Using that, we then annotate the cell types based on the protein expression of each cluster.
</div>


<div class="row justify-content-sm-center">
    <div class="col-sm-4 mt-3 mt-md-0">
        {% include figure.html path="assets/img/phenograph heatmap v2.png" title="phenograph heatmap" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.html path="assets/img/cell-map.PNG" title="cell map" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    PhenoGraph clustering uses the cell masks to group cells with similar protein expressions (left). That information is then used to annotate each of the different cell types in a sample (right).
</div>

<h2>Data Augmentation</h2>

With medical data specifically, it has been a longstanding challenge to acquire data from large numbers of subjects. While ML requires data points in the thousands, most patient cohorts are only in the tens. Therefore, proper data augmentation becomes crucial to enable appropriate learning from the data. In IMC images, the protein expressions are measured for each individual cell in the image. For each cell, the averaged pixel intensities within the cell corresponds to its protein expression. Therefore, normal image augmenting techniques like rotations and flips would not work. Though a rotation or flip would change the spatial location of the pixels in relation to the whole image, the pixel intensities within each cell would remain unchanged. Thus, a proper IMC data augmentation technique is necessary.

We developed a novel cell-level spatial augmentation approach called <i>sector elimination</i>. For each tissue sample, we suggest to exclude all cells within a randomly selected 30 degree spatial pie section of the images. This process was randomly repeated 50 times for each of the tissue samples.

<div class="row justify-content-sm-center">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/augmentation.png" title="augmentation" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Sector elimination augmentation approach, showing the augmented partial images. 
</div>

After augmenting, the proportions of each cell type within a sample was calculated and used as the features for that sample.

<h2>ML Experiments</h2>

We used different classification models to evaluate TME association with various clinical labels, shown in Table 1. The features representing the proportion of different cell types were ranked using an ANOVA F-score statistic. We used logistic regression (LR), random forest (RF), decision tree (DT), k-nearest neighbour (KNN), and an ensemble of all four models, in a 4-fold cross validation configuration, to validate the performance of varying numbers of top ranked features. For each classifier, the default parameters were used. During fold generation, care was taken to ensure that all data samples from a single patient remained in the same fold. We also used Synthetic Minority Over-sampling Technique (SMOTE) to balance the data, such that each fold contained an equivalent amount of the label we were approximating. The cross validation for each classifier was repeated 50 times - each time with different, randomly generated folds - and the average accuracy of the classifiers on the test fold data in all runs was reported. Based on the results of the experiments, we decided on the the optimal number of ranked features, along with the top performing classifier, for each label. Additionally, we repeated the ML analyses using the original data without any augmentation to compare with our proposed augmentation approach.





<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/phenograph heatmap v2.png" title="heatmap" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Once the cell mask is created for each sample, PhenoGraph clustering is used to group cells with similar protein expressions (channels).
</div>




Every project has a beautiful feature showcase page.
It's easy to include images in a flexible 3-column grid format.
Make your photos 1/3, 2/3, or full width.

To give your project a background in the portfolio page, just add the img tag to the front matter like so:

    ---
    layout: page
    title: project
    description: a project with a background image
    img: /assets/img/12.jpg
    ---

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/1.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/3.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/5.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Caption photos easily. On the left, a road goes through a tunnel. Middle, leaves artistically fall in a hipster photoshoot. Right, in another hipster photoshoot, a lumberjack grasps a handful of pine needles.
</div>
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/5.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    This image can also have a caption. It's like magic.
</div>

You can also put regular text between your rows of images.
Say you wanted to write a little bit about your project before you posted the rest of the images.
You describe how you toiled, sweated, *bled* for your project, and then... you reveal it's glory in the next row of images.


<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.html path="assets/img/6.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-4 mt-3 mt-md-0">
        {% include figure.html path="assets/img/11.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    You can also have artistically styled 2/3 + 1/3 images, like these.
</div>


The code is simple.
Just wrap your images with `<div class="col-sm">` and place them inside `<div class="row">` (read more about the <a href="https://getbootstrap.com/docs/4.4/layout/grid/">Bootstrap Grid</a> system).
To make images responsive, add `img-fluid` class to each; for rounded corners and shadows use `rounded` and `z-depth-1` classes.
Here's the code for the last row of images above:

{% raw %}
```html
<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.html path="assets/img/6.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-4 mt-3 mt-md-0">
        {% include figure.html path="assets/img/11.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
```
{% endraw %}
