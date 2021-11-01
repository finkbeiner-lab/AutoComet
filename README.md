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
    
You can change the user input under the section **Input Parameters and preprocessing**
 
## Citing
