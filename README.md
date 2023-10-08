# YOLO v5 Object Detection
전동 킥보드 대여 앱(G*****) 내에서 구동할 수 있는 헬멧 미착용자 감지 시스템  
<br/>

> ### Summary  
> * 👩‍💻 Attribute : 100% (1명)
> * 👩‍💻 Key Role : AI Engineer, PM  
> * 👩‍💻  : 3주    
>> * 레퍼런스 학습 ~ 1차 모델링 약 2주 (2021.11.01 ~ 11.12)
>> * 2차 모델링 ~ 피드백 약 1주 (2021.11.15, 16, 17, 25, 26)  
<br/>

## 작업환경
* Colab
* 💻 [전체코드](https://github.com/Seohee-Kim/Yolov5-ObjectDetection/blob/main/YOLOv5_Custom_Training.ipynb)

<br/>  

## v5 빌드 사유
* 1️⃣ FPS 성능을 최적화할 필요가 없기 때문에 v5의 빠른 동작성 선호
* 2️⃣ 파이토치 기반의 쉽고 빠른 환경 구성  
<br/>  

​
## 데이터 준비
### 1st 어노테이션 (labelimg 버전)  
* Tool : Anaconda prompt, labelimg 1.8.1
* Label : person-with-helmet, person-without-helmet  
* 바운딩 박스를 지정하여 라벨링 한 후, txt 파일 함께 저장  
    
<img width="960" alt="1" src="https://github.com/Seohee-Kim/Yolov5-ObjectDetection/assets/62201733/5c4fc81c-4cbf-4ada-92c1-d93c8bb2ca50">  
<br/>

> 💡 TIP  
> * 실행 중 [AssertionError: Missing string id : useDefaultLabel] 에러가 발생한다면, pyrcc5 resources.qrc -o resources.py 실행 후 재실행 
> * 후에 이미지 파일 이름과 확장자, txt 파일 이름 통일을 위해 DarkNamer 사용  

### 2nd 어노테이션 (roboflow 버전)
* Tool : roboflow  
* Label : 위와 동일  
* 장점 : 욜로 버전에 맞는 yaml 파일을 자동 생성, API 키 연동을 통한 구성  

> 💡 전처리
> * Auto-Orient : Applied
> * Resize : Stretch to 416x416

> 💡 아규먼테이션
> * Rotation : Between -22° and +22°
> * Bounding Box : Shear: ±15° Horizontal, ±15° Vertical

## 3. 모델 학습 (Train)
img: define input image size
batch: determine batch size
epochs: define the number of training epochs. (Note: often, 3000+ are common here!)
data: Our dataset locaiton is saved in the dataset.location
weights: specify a path to weights to start transfer learning from. Here we choose the generic COCO pretrained checkpoint.
cache: cache images for faster training

!python train.py --img 416 --batch 16 --epochs 300 --data /content/datasets/Helmet-detection-3/data.yaml --weights yolov5s.pt --cache



라벨명을 명시하면 VOC Dataset 클래스가 이를 받아 albumentations을 수행하고,

정규화된  x, y, w, h가 이미지 크기에 맞게 변경되는 과정을 거친다

# VOC class names
```python
classes = [
    "person-with-helmet",
    "person-without-helmet"
]
​```

cfg는 yolov3-voc.cfg를 사용했고 darknet 깃에서 클론할 수 있다

그러면 config 분석 함수가 이를 받아 block_lists로 반환한다

!git clone https://github.com/pjreddie/darknet.git
path2config = '/content/darknet/cfg/yolov3-voc.cfg'






     
##  모델 평가
* mAP 0.5 기준
<img width="1046" alt="스크린샷 2023-10-08 오후 10 26 36" src="https://github.com/Seohee-Kim/Yolov5-ObjectDetection/assets/62201733/0a456bdd-914f-4c3e-ab4d-3200842355e3">

## 추론 결과


## 참고한 레퍼런스
* https://deep-learning-study.tistory.com/568  
* https://csm-kr.tistory.com/11  
* https://bitcodic.tistory.com/104?category=746696  
* https://github.com/sjchoi86/dl_tutorials_10weeks/blob/master/papers/You%20Only%20Look%20Once-%20Unified%2C%20Real-Time%20Object%20Detection.pdf  
* https://junyoung-jamong.github.io/deep/learning/2019/01/22/Darkflow%EB%A5%BC-%ED%99%9C%EC%9A%A9%ED%95%B4-YOLO%EB%AA%A8%EB%8D%B8-%EC%9D%B4%EB%AF%B8%EC%A7%80-%EB%94%94%ED%85%8D%EC%85%98-%EA%B5%AC%ED%98%84-in-windows.html  
