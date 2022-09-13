---
layout: page
title: TITAN
description: Processing & analysis software for IMC data
img: assets/img/logo-v2.PNG
importance: 3
category: research
---

<style>
cover{
    /* object-fit:cover; */
    width: auto;
}
</style>

<i>This work was published in [Cytometry Part A](https://onlinelibrary.wiley.com/doi/abs/10.1002/cyto.a.24535) and presented at the [2021 Imaging Network Ontario conference](https://imno.ca/).
</i>

<iframe width="750" height="350" src="https://www.youtube.com/embed/5r2q63ghE-0" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

<br>

<div class="row justify-content-sm-center" style="background-color: #fbf2fb">
    <div class="col-sm-4 mt-3">
        <h3 style="color: #b87bd0">Goal</h3>
            <p style="color: #b87bd0">Develop a tool that can provide end-to-end analysis of imaging mass cytometry data and improve upon the current software.</p>
    </div>
    <div class="col-sm-8 mt-3">
        <h3 style="color: #b87bd0">Result</h3>
            <p style="color: #b87bd0">Developed <i>TITAN</i>, which provides a step-by-step, user-centered workflow for visualization, segmentation, and analysis. It mitigates the shortcomings of current analysis pipelines and improves efficiency.</p>
    </div>
</div>

<br>

<h2>Introduction</h2>

Imaging mass cytometry (IMC) is a powerful tissue imaging technology that improves upon traditional imaging methods. It's capable of outputting a large amount of data, however the current analysis methods are fairly preliminary. Currently, the analytical pipeline is as follows:

<ol>
    <li>Export raw data using <i>MCD Viewer</i></li>
    <li>Import images from <i>MCD Viewer</i> into <i>CellProfiler</i> for cell segmentation</li>
    <li>Export generated masks from <i>CellProfiler</i> to the analysis software <i>histoCat</i></li>
    <li>Export resulting data for external analyses if necessary</li>
</ol>

In order to complete these tasks, three separate software - endorsed by the manufacturers of the Hyperion™ - are needed and require the unnecessary exportation of partial data results.

Although these software are the standard for analysis of IMC data, they are lacking in utility and require using multiple software for each of the tasks of visualization, segmentation, and analysis. In addition to issues in efficiency, there are certain pitfalls within the functionality of the software themselves, identified by users of these software.

<h2>Implementation</h2>

TITAN was developed within a free open-source software platform - [3D Slicer](https://www.slicer.org/) - for the purpose of addressing pitfalls and inefficienceis in the existing workflows for IMC analysis. It's compatible with both multi-channel text files and single-channel TIFF images of IMC data as inputs and provides an end-to-end pipeline for exploration and quantification of IMC data in a single environment. 

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/extensions store v2.png" title="titan store" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    User interface of TITAN. Sections of TITAN's user interface are shown in: a) Visualization tab and b) Analysis tab. c) TITAN can be added directly from the Extensions store in 3D Slicer.
</div>

TITAN uses several well-documented Python libraries including [Python Imaging Library](https://pillow.readthedocs.io/en/stable/), [SimpleITK](https://pypi.org/project/SimpleITK/), [matplotlib](https://matplotlib.org/), and [scikit-learn](https://scikit-learn.org/stable/). It is open-source and publicly available through its [Github repository](https://github.com/SlicerMicro/Slicer-TITAN) along with detailed documentation and tutorials.

<h3 size="-1">Overview</h3>

TITAN's functionality is divided into 3 main categories, following the natural order of analysis of IMC data:

<ol>
    <li>Visualization</li>
    <li>Segmentation</li>
    <li>Analysis</li>
</ol>

After importing IMC data in text or image format, the user can view individual protein channels as well as an overlay of multiple channels at once. Users can then segment the nucleus channel into a cell mask, which is required for single-cell feature extraction and analysis. Once the cell mask is created, the user can produce a variety of quantitative plots that provide statistical information about the data. 

<h3 size="-1">Data Importation</h3>

TITAN allows the user to import either raw text files, which are generated during acquisition, or TIFF image files of the individual channels. The text file for an ROI contains the pixel information including location and intensity of each protein channel within a given ROI. If importing the raw text file of each ROI, TITAN will convert the file into individual channel images based on the pixel values and use the generated images for downstream functions. This ultimately removes the need of MCD Viewer, which typically requires an MCD file. There is also the option of importing 8-bit TIFF image files of each channel, for situations where the user may not possess the raw text file. The user can import these files directly into TITAN.

<h3 size="-1">Visualization</h3>

TITAN provides a thumbnail overview of all single-channel images within a given acquired IMC ROI, providing users the ability to assess staining easily and qualitatively. TITAN also allows for the visualization and overlay of up to seven false-coloured channels simultaneously for each ROI, which can be used to generate publication quality figures. An advantage of TITAN compared with MCD Viewer is that, with TITAN, the user is able to manually threshold images or perform automated colour scaling – a feature provided through 3D Slicer. These adjustments can be applied globally to the entire ROI, or locally based on a user-selected region of the ROI. This functionality allows users to optimize the brightness/contrast of multi-channel images interactively and conveniently. Furthermore, TITAN enables users to retain the adjustment parameters for a selected ROI and apply it to all other ROIs to achieve standardized and comparable visualizations across a dataset.

<h3 size="-1">Segmentation</h3>







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
