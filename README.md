# object-detection-hoop_and_ball
1. Collect images that contain your object
2. Annotate/label the images, with LabelImg(https://github.com/tzutalin/labelImg). This process is basically drawing boxes around your object(s) in an image. The label program automatically will create an XML file that describes the object(s) in the pictures.
  using 
3. Split this data into train/test samples
use xml_to_csv.py to convert labeled results from xml format to csv format.
4. Generate TF Records from these splits
``` 
protoc object_detection/protos/*.proto --python_out=. $protos folder not included in this repo$
python generate_tfrecord.py --csv_input=data/train_labels.csv --output_path=data/train.record --image_dir=images/
python generate_tfrecord.py --csv_input=data/test_labels.csv --output_path=data/test.record --image_dir=images/
```
5. Train. Setup a .config file for the model of choice (you could train your own from scratch, but we'll be using transfer learning)
```
python legacy/train.py --logtostderr --train_dir=training/ --pipeline_config_path=training/ssd_mobilenet_v1
```
6. Export graph from new trained model
```
python export_inference_graph.py --input_type image_tensor --pipeline_config_path training/ssd_mobilenet_v1_pets.config --trained_checkpoint_prefix training/model.ckpt-791 --output_directory basketball_interface_graph
```
7. Detect custom objects in real time
Use object_detection_tutorial.ipynb!
