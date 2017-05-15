# StairsNet: Mixed Multi-Scale Network for Object Detection


### Contents
1. [Installation](#installation)
2. [Preparation](#preparation)
3. [Train/Eval](#traineval)
4. [Models](#models)

### Installation
1. Get the code. We will call the directory that you cloned Caffe into `$CAFFE_ROOT`
  ```Shell
  git clone https://github.com/gwyve/caffe.git
  cd caffe
  git checkout StairsNet
  ```

2. Build the code. Please follow [Caffe instruction](http://caffe.berkeleyvision.org/installation.html) to install all necessary packages and build it.
  ```Shell
  # Modify Makefile.config according to your Caffe installation.
  cp Makefile.config.example Makefile.config
  make -j8
  # Make sure to include $CAFFE_ROOT/python to your PYTHONPATH.
  make py
  make test -j8
  # (Optional)
  make runtest -j8
  ```

### Preparation
1. Download [ResNet-101 jin](https://iuxblw-bn1305.files.1drv.com/y4mvtb7EBQ_y-A0fatiz6H1en6ffoZoOK6sDXlf9eiCVBDV289lgBEojeaHyNKOwl60H5H5NDfTWHOSDpvOYtUnlWvTr0XYW7L2qguEXAH7Z0z_s17MDUlnZPWRxjLOLdNYHOMVM9PAi8UFp1zuijeK3rN4RunkKai2WUNtCU8UxRYHfFzOpPxHu_7bOB_JkvW_OGs3Oor9hGnHDbHUKunlcQ/ResNet-101-model.caffemodel?download&psid=1). By default, we assume the model is stored in `$CAFFE_ROOT/models/ResNet/`

2. Download VOC2007 and VOC2012 dataset. By default, we assume the data is stored in `$HOME/data/`
  ```Shell
  # Download the data.
  cd $HOME/data
  wget http://host.robots.ox.ac.uk/pascal/VOC/voc2012/VOCtrainval_11-May-2012.tar
  wget http://host.robots.ox.ac.uk/pascal/VOC/voc2007/VOCtrainval_06-Nov-2007.tar
  wget http://host.robots.ox.ac.uk/pascal/VOC/voc2007/VOCtest_06-Nov-2007.tar
  # Extract the data.
  tar -xvf VOCtrainval_11-May-2012.tar
  tar -xvf VOCtrainval_06-Nov-2007.tar
  tar -xvf VOCtest_06-Nov-2007.tar
  ```

3. Create the LMDB file.
  ```Shell
  cd $CAFFE_ROOT
  # Create the trainval.txt, test.txt, and test_name_size.txt in data/VOC0712/
  ./data/VOC0712/create_list.sh
  # You can modify the parameters in create_data.sh if needed.
  # It will create lmdb files for trainval and test with encoded original image:
  #   - $HOME/data/VOCdevkit/VOC0712/lmdb/VOC0712_trainval_lmdb
  #   - $HOME/data/VOCdevkit/VOC0712/lmdb/VOC0712_test_lmdb
  # and make soft links at examples/VOC0712/
  ./data/VOC0712/create_data.sh
  ```

### Train/Eval
1. Traini and Eval the base StairsNet model.
  ```Shell
  # It will create model definition files and save snapshot models in:
  #   - $CAFFE_ROOT/models/ResNet/VOC0712/BSN_321x321/
  # and job file, log file, and the python script in:
  #   - $CAFFE_ROOT/jobs/ResNet/VOC0712/BSN_321x321/
  # It should reach 75.0* mAP at 100k iterations.
  python examples/stairsNet/base_stairsNet_321.py
  ```

2. Train and Eval StairsNet model.
  ```Shell
  # Use the base StairsNet as the pretrained model
  python examples/staisNet/stairsNet_321.py
  # It should reach 77.7* mAP at 100k iterations.


