# Fine Tuning Tesseract on Windows

### Install Tesseract OCR and tool

1) Download the setup from this [repo](https://github.com/UB-Mannheim/tesseract/wiki)
and make sure tesseract is in your `PATH`.
2) Install Python version 3.x
3) Install Git
4) Install winget/Windows Package Manager and then run `winget install ezwinports.make` and `winget install wget`.

### Clone repo `tesstrain`

```shell
git clone https://github.com/tesseract-ocr/tesstrain.git
```

and clone the trained model repo 

```shell
git clone https://github.com/tesseract-ocr/tessdata_best.git
```

also clone the `langdata` cause we will need the wordlist, punctation and number files

```shell
git clone https://github.com/tesseract-ocr/langdata
```

or download the trained model you are interested to fine tune in a folder `tessdata_best`

1) `cd tesstrain`

### Create virtual environment

2) Create virtual environment

```shell
python -m venv .venv
```

Activate virtual environment

```shell
.venv\scripts\activate
```

Install requirements 

```shell
python -m pip install -r requirements.txt
```

### Create Folders

Create `data` folder

Open Git Bash and cd into the directory `tesstrain`
and run this:

**>>> DO NOT NEED THIS**
```shell
make tesseract-langdata
```

~~in the `data` a `langdata` folder is create with a bunch of unicharset files~~

### Ground Truth Data

Tesseract 5 using lines of data so we need to provide a image with the line (png or tif)
and a text file with the content of the image.
You can find a ZIP file `ocrd-testset.zip`  with some ground truth data we can use to fine tuning.

1) unzip the file in a folder inside the `data` folder giving the name of the model you are going to create + `ground-truth`

IE: `lft-ground-truth`

### Fine tuning

using this command to start the training process

```shell
make training MODEL_NAME=lft START_MODEL=eng TESSDATA=../tessdata_best LANGDATA_DIR=../langdata LEARNING_RATE=0.001 RATIO_TRAIN=0.80 MAX_ITERATIONS=5000
```

the fine tuning process will start and a folder with your model name will be created inside the `data` folder with the name of your model.

In the ground truth folder `lft-ground-truth` the box files will be created for each image.


At the end of the training process you can 

```shell
make traineddata MODEL_NAME=lft
```

```shell
make plot MODEL_NAME=lft
```