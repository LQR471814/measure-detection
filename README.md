## measure-detection

***A measure detection model for typeset scores, based on the tensorflow `object_detection` api. Inspired by the more general [MeasureDetector](https://github.com/OMR-Research/MeasureDetector) for handwritten and typeset scores.***


This repository uses the `AudioLabs_v2` dataset you can get it [here](https://github.com/apacha/OMR-Datasets/releases/download/datasets/AudioLabs_v2.zip)


### training

**note: please make sure you use `tensorflow<2.9`, on windows using the latest version of tensorflow will cause it to break JIT compilation**

it is recommended to have `make` installed to build the targets, however you can always run the scripts manually

if you want to change the command the `makefile` uses to run python, you can change the `PYTHON` environment variable.

before training one should make sure that the tensorflow `object_detection` api is setup correctly

```
git clone https://github.com/tensorflow/models.git
cd models/research
protoc object_detection/protos/*.proto --python_out=.
cp object_detection/packages/tf2/setup.py .
python -m pip install .
cd ../..
```

quickly test the installation with

```
make test
```

then download a pretrained model from the tensorflow model zoo like [ssd_resnet50_v1_fpn_640x640_tpu-8](http://download.tensorflow.org/models/object_detection/tf2/20200711/ssd_resnet50_v1_fpn_640x640_coco17_tpu-8.tar.gz), unzip the contents and copy it into the `pretrained` directory

your directory structure should look like this

```
models/
    ...
pretrained/
    ssd_resnet50_v1_fpn_640x640_tpu-8/
        checkpoint/
        saved_model/
        pipeline.config
...
```


after that, configure the path to the dataset with the `DATASET_DIR` environment variable and run

```
make prepare-dataset
```

we are now ready to begin training

```
make train
make evaluate
```

after the training has finished, you can export it to a `saved_model` using

```
make freeze-pb
```

then perform a sanity check with

```
make inference-pb
```

and if you wish to you can convert it to a `tensorflowjs` model using

```
make convert-tfjs
```
