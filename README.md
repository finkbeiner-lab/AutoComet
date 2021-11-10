# Auto Comet: a fully automated algorithm to analyze comet assays more accurately

Comet assays are a biochemical tool to analyze DNA damage based on the migration of broken DNA strands towards a positive electrode, which creates a ‘tail’ behind the cell that is used for DNA damage measurements. Our project AutoComet implements a high-throughput comet assay analysis algorithm to address the low throughput and inaccuracy of the open source and commercially available tools. This algorithm segments most comets in an image and effectively filters out overlapping comets and comets on the edge of an image, without the need of manual pre- or post-processing. The algorithm automatically calculates multiple comet measurements such as tail area and percentage of DNA in the tail.

### Highlights:

- Autocomet is a new open source program to analyze alkaline and neutral comet assays
- Autocomet results are highly comparable to manual curation
- Autocomet is more accurate than open source tools CellProfiler and OpenComet
- Autocomet generates results in <3 min and reduces processing time by a tenfold
- AutoComet is fully automated and eliminates all curator bias


# Getting Started 

The project can be either cloned or downloaded to your device from [AutoComet github](https://github.com/finkbeiner-lab/AutoComet). The code is implemented in Python 3.8 with Jupyter Notebook, and with the requirements of the following open-source libraries:

- opencv-python==4.5.3
- matplotlib==3.3.4
- numpy==1.20.1
- pandas==1.2.4
- scipy==1.6.2
- skimage==0.18.1
- Pillow==8.2.0

## Installation
You can find the [Jupyter Notebook installation](https://jupyter.readthedocs.io/en/latest/install.html) documentation on ReadTheDocs. For a local installation, make sure you have
[pip installed](https://pip.readthedocs.io/en/stable/installing/) and run:

    $ pip install notebook

You may install dependencies using:

    $ pip install -r requirements.txt

Or install each depenency with the command:

    $ pip install 'dependency'
    
## Usage - Running the notebook

Launch with:

    $ jupyter notebook
    
### Steps:

1. **Input Parameters and preprocessing**

AutoComet is tested on TIF format images. AutoComet converts images to the single channel grayscale images. 
- Change input_path to the image directory that you want to analyze.
- AutoComet requires that comets are oriented vertically. Flip and/or rotate images if needed so that comet head are on top and tail are extending toward the bottom of the image. Example input image:
<img width="300" alt="Image" src="https://user-images.githubusercontent.com/88739975/140874798-9ccb1221-90e5-47c9-bf62-56f9d99d3252.png">

- Change crop_dim (crop dimension, default 273 height x 143 width) to make sure the entire comet is captured. Review in comet segmentation step and correct crop dimension if needed. 

2. **Comet segmentation and filtering**

During comet segmentation process, objects appear brighter than the background pixels are captured using binary thresholding. Images that have extremely bad intensity are filtered.  Review Images and check on crop size. Tails should be included within the red rectangle box; if not, adjust crop dimension again. During the segmentation process, these segmented regions are removed:

- Tiny objects (min_area = 100) that are usually background noise and debris
- Huge objects (max_area = 7000) that are usually clamps or non-comet objects

These tiny and huge filtered objects will appear within gray rectanglar box under Segmented Mask image. Example comet segmentation and filtering:

<img width="1000" alt="Example2" src="https://user-images.githubusercontent.com/88739975/141028577-49818893-bbaf-4789-b421-305749c5649f.png">

Additionally, by going through each comet crop, we want to remove regions that might not provide us the best information. Such as:
- Segmented objects that are on the edge of the image or cannot be cropped into the dimension size
- Segmented objects that appears in more than one crop, which might be a tail object

3. **Comet Measurement**

AutoComet will then try to find the head of each comet and the entire body of the comet. AutoComet identify comet heads by finding the brightest pixel and comet body by finding all bright pixels above background. During the process, erosion and dilation is performed on the head to add extra pixels in an attempt to obtain a similar size as the head size within body segment. Example head and body detection: 

<img width="249" alt="Comet detection with no tail" src="https://user-images.githubusercontent.com/88739975/141037269-a844b49b-53fa-474c-8c0a-1f33fa2c3df9.png">
<img width="249" alt="Comet detection with tail" src="https://user-images.githubusercontent.com/88739975/141037187-78c71c49-fc43-44c0-bfc4-c7a7729ab469.png">
<img width="249" alt="Comet detection with long tail" src="https://user-images.githubusercontent.com/88739975/141037371-835c3a10-2eb8-412c-ae2a-0c185f15aa37.png">

AutoComet calculates several measurements:

 | Measurement  | Description |
| ------------- | ------------- |
| Head length  | Head length in pixels |
| Tail length  | Tail length in pixels  |
| Body length  | Entire comet length (head + tail) in pixels |
| Comet area  | Sum of comet pixels  |
| Comet DNA content  | Total comet pixel intensity  |
| Comet average intensity  | Avergae comet pixel intensity  |
| Head area  | Sum of head pixels |
| Head DNA content  | Total head pixel intensity |
| Head average intensity  | Average head pixel intensity |
| Head DNA %  | Percentage of head pixel intensity in comet |
| Tail area  | Sum of tail pixels |
| Tail DNA content  | Total tail pixel intensity |
| Tail average intensity  | Average tail pixel intensity |
| Tail DNA %  | Percentage of tail pixel intensity in comet |

## Citing
