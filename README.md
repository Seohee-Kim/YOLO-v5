# Yolov5-ObjectDetection

## Requirements
* matplotlib>=3.2.2
* numpy>=1.18.5
* opencv-python>=4.1.2
* Pillow>=7.1.2
* PyYAML>=5.3.1
* requests>=2.23.0
* scipy>=1.4.1
* torch>=1.7.0
* torchvision>=0.8.1
* tqdm>=4.41.0
* tensorboard>=2.4.1
* pandas>=1.1.4
* seaborn>=0.11.0

## 작업환경
* Colab
* 링크 :  [YOLOv5_Custom_Training.ipynb](https://github.com/Seohee-Kim/Yolov5-ObjectDetection/blob/main/YOLOv5_Custom_Training.ipynb)https://github.com/Seohee-Kim/Yolov5-ObjectDetection/blob/main/YOLOv5_Custom_Training.ipynb

## Roboflow - Bounding box annotations
* Auto-Orient : Applied
* Resize : Stretch to 416x416
* Rotation : Between -22° and +22°
* Bounding Box : Shear: ±15° Horizontal, ±15° Vertical

## 3. 모델 학습 (Train)
img: define input image size
batch: determine batch size
epochs: define the number of training epochs. (Note: often, 3000+ are common here!)
data: Our dataset locaiton is saved in the dataset.location
weights: specify a path to weights to start transfer learning from. Here we choose the generic COCO pretrained checkpoint.
cache: cache images for faster training

!python train.py --img 416 --batch 16 --epochs 300 --data /content/datasets/Helmet-detection-3/data.yaml --weights yolov5s.pt --cache
     
## 모델 평가
* mAP 0.5 기준
<img width="1046" alt="스크린샷 2023-10-08 오후 10 26 36" src="https://github.com/Seohee-Kim/Yolov5-ObjectDetection/assets/62201733/0a456bdd-914f-4c3e-ab4d-3200842355e3">

## 추론 결과
