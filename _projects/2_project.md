---
layout: page
title: TITAN
description: Image processing & analysis software for IMC data
img: assets/img/logo-v2.PNG
importance: 3
category: research
---

<style>
th{
    text-align: center;
}
td{
    text-align: center;
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
    <div class="col-sm mt-3">
        {% include figure.html path="assets/img/extensions store v2.png" title="titan store" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    User interface of TITAN. Sections of TITAN's user interface are shown in: a) Visualization tab and b) Analysis tab. c) TITAN can be added directly from the Extensions store in 3D Slicer.
</div>

TITAN uses several well-documented Python libraries including [Python Imaging Library](https://pillow.readthedocs.io/en/stable/), [SimpleITK](https://pypi.org/project/SimpleITK/), [matplotlib](https://matplotlib.org/), and [scikit-learn](https://scikit-learn.org/stable/). It is open-source and publicly available through its [Github repository](https://github.com/SlicerMicro/Slicer-TITAN) along with detailed documentation and tutorials.

<!-- To demonstrate the utility of TITAN’s features and evaluate its segmentation method, we used multiple, publicly available datasets. The [first dataset](https://www.nature.com/articles/s41586-019-1876-x) was collected from breast cancer patients the [second dataset](https://www.nature.com/articles/s41586-021-03475-6), used for further evaluation of the segmentation method, is of lung tissue obtained from COVD-19 patients. -->

<div class="row">
    <div class="col-sm-6 my-3">
        TITAN's functionality is divided into 3 main categories, following the natural order of analysis of IMC data:
    </div>
    <div class="col-sm-6 my-3">
        <ol>
            <li>Visualization</li>
            <li>Segmentation</li>
            <li>Analysis</li>
        </ol>
    </div>
</div>

<!-- TITAN's functionality is divided into 3 main categories, following the natural order of analysis of IMC data:

<ol>
    <li>Visualization</li>
    <li>Segmentation</li>
    <li>Analysis</li>
</ol> -->

<div class="row">
    <div class="col-sm mt-3">
        {% include figure.html path="assets/img/titan_overview.png" title="titan overview" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>


After importing IMC data in text or image format, the user can view individual protein channels as well as an overlay of multiple channels at once. Users can then segment the nucleus channel into a cell mask, which is required for single-cell feature extraction and analysis. Once the cell mask is created, the user can produce a variety of quantitative plots that provide statistical information about the data. 

<h3 size="-6">Data Importation</h3>

TITAN allows the user to import either raw text files, which are generated during acquisition, or TIFF image files of the individual channels. The text file for an ROI contains the pixel information including location and intensity of each protein channel within a given ROI. If importing the raw text file of each ROI, TITAN will convert the file into individual channel images based on the pixel values and use the generated images for downstream functions. This ultimately removes the need of MCD Viewer, which typically requires an MCD file. There is also the option of importing 8-bit TIFF image files of each channel, for situations where the user may not possess the raw text file. The user can import these files directly into TITAN.

<h3 size="-6">Visualization</h3>

TITAN provides a thumbnail overview of all single-channel images within a given acquired IMC ROI, providing users the ability to assess staining easily and qualitatively. TITAN also allows for the visualization and overlay of up to seven false-coloured channels simultaneously for each ROI, which can be used to generate publication quality figures. An advantage of TITAN compared with MCD Viewer is that, with TITAN, the user is able to manually threshold images or perform automated colour scaling – a feature provided through 3D Slicer. These adjustments can be applied globally to the entire ROI, or locally based on a user-selected region of the ROI. This functionality allows users to optimize the brightness/contrast of multi-channel images interactively and conveniently. Furthermore, TITAN enables users to retain the adjustment parameters for a selected ROI and apply it to all other ROIs to achieve standardized and comparable visualizations across a dataset.

<h3 size="-6">Segmentation</h3>

Beyond visualization, IMC enables proteomic imaging at subcellular resolution, thus allowing for single cell-level phenotyping and characterization of tissues. Delineation and segmentation of individual cells within ROIs is a critical step in any IMC data analysis pipeline. In order to segment this nucleus channel, TITAN utilizes a watershed algorithm and morphological image processing to create the initial nucleus mask. When cell segmentation is complete, the total number of cells segmented within each ROI is reported, which is an essential variable for further analysis. Notably, TITAN generates cell masks for multiple ROIs simultaneously and efficiently.

 

<h3 size="6">Analysis</h3>

TITAN contains simple analysis functions including the generation of histograms, scatter plots, and heat maps of cell features, as well as more computationally complex methods such as dimensionality reduction and clustering functions for cell features. 

Following cell segmentation, cell-level mean intensities can be extracted from multi-channel data by calculating the mean intensity of each protein channel within each cell. TITAN is also then capable of exporting these data in spreadsheets, which can be used in external applications as well. This feature provides the user the flexibility to perform downstream analyses of single-cell data in their preferred program or in-house pipeline(s). 

Using the scatter plots produced by TITAN, a manual gating function was implemented. This function allows users to manually gate cell populations based on the hierarchical expression proteins in a two-by-two manner, comparable to how hierarchical gating of cell flow cytometry data is performed. To gate, the user selects two channels for the scatter plot and selects the cells of interest. When the user creates the gated area, a new cell mask containing only the selected cells is displayed and the user has the option to continue gating the images into successive generations. TITAN’s gating feature will be familiar to users experienced with flow cytometry analysis and will enable manual identification and enumeration of cell populations based on marker expression.  


<h2>Summary of Features</h2>

<br>

<table style="width:100%">
    <thead>
        <tr>
            <th rowspan="2" scope="col">Feature</th>
            <th scope="col">My software</th>
            <th colspan="3" scope="colgroup">Manufacturer Software</th>
        </tr>
        <tr>
            <!-- <td></td> -->
            <td scope="col">TITAN</td>
            <td scope="col">MCD Viewer</td>
            <td scope="col">CellProfiler</td>
            <td scope="col">histoCAT</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td style="text-align:left">Thumbnail view</td>
            <td>X</td>
            <td>X</td>
            <td></td>
            <td></td>
        </tr>
        <tr>
            <td style="text-align:left">Image Visualization</td>
            <td>X</td>
            <td>X</td>
            <td></td>
            <td>X</td>
        </tr>
        <tr>
            <td style="text-align:left">Automated colour scaling</td>
            <td>X</td>
            <td></td>
            <td></td>
            <td></td>
        </tr>
        <tr>
            <td style="text-align:left">Reading of multi-channel text files</td>
            <td>X</td>
            <td>X</td>
            <td></td>
            <td></td>
        </tr>
        <tr>
            <td style="text-align:left">Mask creation for all ROI at once</td>
            <td>X</td>
            <td></td>
            <td>X</td>
            <td></td>
        </tr>
        <tr>
            <td style="text-align:left">Basic mean intensity plots</td>
            <td>X</td>
            <td></td>
            <td></td>
            <td>X</td>
        </tr>
        <tr>
            <td style="text-align:left">Gating</td>
            <td>X</td>
            <td></td>
            <td></td>
            <td>X</td>
        </tr>
        <tr>
            <td style="text-align:left">Dimensionality reduction</td>
            <td>X</td>
            <td></td>
            <td></td>
            <td>X</td>
        </tr>
        <tr>
            <td style="text-align:left">Clustering functions</td>
            <td>X</td>
            <td></td>
            <td></td>
            <td></td>
        </tr>
        <tr>
            <td style="text-align:left">Export raw data of cell mean intensities</td>
            <td>X</td>
            <td></td>
            <td></td>
            <td></td>
        </tr>
    </tbody>
</table>

