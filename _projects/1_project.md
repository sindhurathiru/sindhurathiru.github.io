---
layout: page
title: Cancer Prognosis Prediction
description: A machine learning pipeline for the predictive analysis of IMC data, including a novel augmentation approach.
img: assets/img/cell-map.PNG
importance: 1
category: work
---

<i>The contents of this page are published in 2021 IEEE EMBS International Conference on Biomedical and Health Informatics (BHI).</i>

<p style="text-align: center;"><b>The Hyperion<sup>TM</sup> Imaging System is a novel technology that uses imaging mass cytometry (IMC) to improve upon current methods of tissue imaging, enabling sub-cellular spatial resolution and acquisition of up to 37 proteins on a single tissue slide. The technology is fairly new, and thus we want to explore the types of analysis possible with these data. Here, we introduce an analysis pipeline to utilize machine learning-based analysis for IMC data using data from a muscle invasive bladder cancer patient cohort. We also propose a novel augmentation method to handle the challenge of low number of tissue samples from IMC studies. Our augmentation method was validated and shown to perform better than when only using the original data. Both our pipeline and augmentation method show promise for applications in future research studies and clinical evaluation of this technology. Our results indicate the feasibility of using the proposed framework with a more robust data set to identify prognostic features, which is an important foundation for further clinical research.</b></p>

<h2>Introduction</h2>

Both tumour cells and the tumor microenvironemnt (TME) are important considerations when evaluating various cancers at the tissue level. With time, cancer cells evolve and are able to evade destruction by cells of the immune system, resulting in the formation of a tumor. Because of this, it's important to obtain a better understanding of the various cell types within the TME, since they may contribute to tumor progression. 

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

With medical data specifically, it has been a longstanding challenge to acquire data from large numbers of subjects. While ML requires data points in the thousands, most patient cohorts are only in the tens. Therefore, proper <b>data augmentation</b> becomes crucial to enable appropriate learning from the data. In IMC images, the protein expressions are measured for each individual cell in the image. For each cell, the averaged pixel intensities within the cell corresponds to its protein expression. Therefore, normal image augmenting techniques like rotations and flips would not work. Though a rotation or flip would change the spatial location of the pixels in relation to the whole image, the pixel intensities within each cell would remain unchanged. Thus, a proper IMC data augmentation technique is necessary.





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
