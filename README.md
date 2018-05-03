### GunDetection


**Simple Gun Detection using tensorflow object_detection API**

The object_detection and research folder map to the real tensorflow directory after you git clone the official repo.
Put them into the official one accordingly.

You are recommanded to use python virtualenv.

Steps:

1. Download the dataset.
Since I want to train a new model to do the gun detection, the first thing is to find a good training dataset.
Ref: [training data](http://sci2s.ugr.es/weapons-detection)

2. Mark and create annotation for your training dataset.
Got the dataset. Nice!  Then we need to mark the place of guns in each images. After that, you can get the .xml files.

Here is a good tool: [labelImg](https://github.com/tzutalin/labelImg)

3. Create a Training dataset and an Evaluation dataset.
See the folder in the object_detection folder. Please carefully follow the style and directories in this folder.

4. Generate TF record file.
Under the "research" directory, run the:
```bash
python tf_record_gun_train.py
python tf_record_gun_train.py
```
They will create two files named: gun_train.record, gun_eval.record

5. Training new model
Now you are ready to train your own model. You can modify the script "train.py"
Under the "research" directory, run the:
```bash
python train.py --logtostderr --train_dir=./GunTraining/models/train --pipeline_config_path=./GunTraining/ssd_mobilenet_v1_coco.config
```
6. Saving a checkpoint model (.ckpt) as a model file .pb
Under “research” folder run this command:
```bash
python export_inference_graph.py --input_type image_tensor --pipeline_config_path ./GunTraining/ssd_mobilenet_v1_coco.config --trained_checkpoint_prefix ./GunTraining/models/train/model.ckpt-200 --output_directory ./GunTraining/fine_tuned_model
```

"frozen_inference_graph.pb" will be generated in the GunTraining/fine_tuned_model folder

7. Copy frozen_inference_graph.pb file to the GunModel folder.

8. Deploy your model to test recoginition
```bash
python wjyTestGunTraining.py
```

#### Simple Test
I training the model using 16 figures with 200 steps. My laptop is slow.
Result:
Successful Recognition Rate:  **53%**


#### Reference:
[Step by Step TensorFlow Object Detection API Tutorial](https://medium.com/@WuStangDan/step-by-step-tensorflow-object-detection-api-tutorial-part-5-saving-and-deploying-a-model-8d51f56dbcf1)