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
- import_ipynb

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

There are two notebooks in the repository. **Main.ipynb** is the notebook for the user to make changes and run the script, **CometAssayAlgorithm.ipynb** contains the functions used.

0. Open AutoComet-Main.ipynb

1. **Input Parameters and preprocessing**

- Change input_path to the image directory that you want to analyze
- Change output_path to the directory that you want the output to be generated

Autocomet requires that comets are oriented vertically. Example input image:

<img width="300" alt="Image" src="https://user-images.githubusercontent.com/88739975/140874798-9ccb1221-90e5-47c9-bf62-56f9d99d3252.png">

- Perform `image_fipping` and set to `True` if comet heads are oriented upside down. For AutoComet analysis, comet heads should be on top and tails extending toward the bottom of the image. 
- Perform `image_rotation` if comets are tilted. Change rotation angle in degrees. No changes if it's set to 0 degree. 
- Preview images and modify `preview_images` to your desired number of images to check on the preprocessed images. There will be no changes applied if no flipping/rotating was done.

AutoComet is tested on TIF format images. It converts images to the single channel grayscale images.
- Change `image_extension= 'tif'` to your image type
- Change `crop_dim` (crop dimension, default 273 height x 100 width) to make sure the entire comet is captured. Review in the comet segmentation step and correct the crop dimension if needed. 

2. **Comet segmentation and filtering**

During the comet segmentation process, objects that appear brighter than the background pixels are captured using binary thresholding. Images that have extremely bad intensity are filtered out entirely.  Review your images and check on the crop size. Tails should be included within the red rectanglar box; if not, adjust crop dimension again from previous step. During the segmentation process, these segmented regions are removed:

- Tiny objects (`min_area = 100`) that are usually background noise and debris
- Huge objects (`max_area = 7000`) that are usually clamps or non-comet objects

These tiny and huge filtered objects will appear within gray rectanglar boxes in the "Segmented Mask" image. Example of comet segmentation and filtering:

<img width="1000" alt="Example2" src="https://user-images.githubusercontent.com/88739975/141028577-49818893-bbaf-4789-b421-305749c5649f.png">

Additionally, we will remove regions that might not provide us the best information. Such as:
- Segmented objects that are on the edge of the image or cannot be cropped into the dimension size
- Segmented objects that appear in more than one crop, which might be a tail-only object

3. **Comet Measurement**

AutoComet will then find the head of each comet and the entire body of the comet. AutoComet identifies comet heads by finding the brightest pixel and comet body by finding all bright pixels above background. Examples of head and body detection: 

<p float="middele">
    <img width="32%" alt="Comet detection with no tail" src="https://user-images.githubusercontent.com/88739975/141037269-a844b49b-53fa-474c-8c0a-1f33fa2c3df9.png" />
    <img width="32%" alt="Comet detection with tail" src="https://user-images.githubusercontent.com/88739975/141037187-78c71c49-fc43-44c0-bfc4-c7a7729ab469.png" />
    <img width="32%" alt="Screen Shot 2021-11-09 at 6 22 39 PM" src="https://user-images.githubusercontent.com/88739975/141038031-d6462288-8718-4808-bfbf-6aa2fd165bc3.png"/>

</p>    

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

4. **Outputs**

AutoComet generates a montaged image of all the comets and a cropped image for each comet that went through the measurements. AutoComet also generates a measurement CSV that contains the input file names and all the 14 comet measurements per comet into the specified output folder.

Example montage of comet crops:

<img width="500" alt="montage Example" src="https://user-images.githubusercontent.com/88739975/141199493-c6f54555-2527-4295-bb51-fd54112b5f8c.png">

Example comet measurement CSV:

<img width="1104" alt="Csv example" src="https://user-images.githubusercontent.com/88739975/141056294-b7e08dc2-a0eb-4f43-93db-7a5388de0a63.png">


## Citing
