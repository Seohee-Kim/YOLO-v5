# YOLO v5 Object Detection  
* 전동 킥보드 대여 앱(Gxxxxx) 연계 아이디어 - 헬멧 미착용자 감지 시스템
* Overview  
<img width="1169" alt="스크린샷 2023-10-09 오후 1 44 14" src="https://github.com/Seohee-Kim/Yolov5-ObjectDetection/assets/62201733/5372e630-504f-44cf-9798-8fa66f569c07"> </br></br>

## Summary  
* 👩‍💻 **기여** : 100% (1명)  
* 👩‍💻 **역할** : AI Engineer (PM)
* 👩‍💻 **기간** : 약 3주
  * 1차 모델링 (2021.11.01 ~ 11.12)
  * 2차 모델링 (2021.11.15, 16, 17, 25, 26) </br></br>

## 작업환경
* **💻 학습** : AWS EC2, Colab GPU
* **💻 코드** :  [전체 코드 바로가기](https://github.com/Seohee-Kim/Yolov5-ObjectDetection/blob/main/YOLOv5_Custom_Training.ipynb) </br></br>

## v5 빌드사유
* FPS 성능을 최적화할 필요가 없기 때문에 v5의 빠른 동작성 선호
* 파이토치 기반의 쉽고 빠른 환경 구성 </br></br>

## 데이터 처리
#### 1st 어노테이션 (labelimg)  
* Tool : Anaconda prompt, labelimg 1.8.1
* Label : person-with-helmet, person-without-helmet  
  * 라벨명을 명시하면 VOC Dataset 클래스가 이를 받아 albumentations을 수행하고, 정규화된  x, y, w, h가 이미지 크기에 맞게 변경
  * 바운딩 박스를 지정하여 라벨링 한 후, txt 파일 함께 저장  
  
<img width="960" alt="1" src="https://github.com/Seohee-Kim/Yolov5-ObjectDetection/assets/62201733/5c4fc81c-4cbf-4ada-92c1-d93c8bb2ca50">  

> 💡 TIP  
> * 실행 중 [AssertionError: Missing string id : useDefaultLabel] 에러 - pyrcc5 resources.qrc -o resources.py 실행 후 재실행 
> * 후에 이미지 파일 이름과 확장자, txt 파일 이름 통일을 위해 DarkNamer 사용  
</br>

#### 2nd 어노테이션 (roboflow)
* Tool : roboflow  
  * 장점 : YOLO 버전에 맞는 yaml 파일을 자동 생성, API 키 연동을 통한 구성  
* Label : 위와 동일  
> 💡 전처리 & 아규먼테이션
> * Auto-Orient : Applied
> * Resize : Stretch to 416x416
> * Rotation : Between -22° and +22°
> * Bounding Box : Shear: ±15° Horizontal, ±15° Vertical

</br></br>
## 모델 학습 (Train)
```python
!python train.py --img 416 --batch 16 --epochs 300 --data /content/datasets/Helmet-detection-3/data.yaml --weights yolov5s.pt --cache
```
* img : 사이즈
* batch: 배치
* epochs: 학습 에폭 (3000+이 평균이지만 테스트 빌드를 위해)
* data: 데이터셋 위치
* weights: yolov5s.pt ​</br></br>

##  모델 평가 (Valid)
* 전체 mAP 0.75
  * helmet : Recall 1, mAP 0.9
  * no-helmet : Recall 0.6, mAP 0.6
 
> 💡 상대적으로 no-helmet의 평가가 낮은 이유
> * 짧은 머리의 음영을 헬멧으로 인식하는 경우
> * 캡 모자를 헬멧으로 인식하는 경우
    
<img width="958" alt="스크린샷 2023-10-09 오후 1 11 56" src="https://github.com/Seohee-Kim/Yolov5-ObjectDetection/assets/62201733/89674e4d-c512-4110-a19d-aed4623648ce">   
   
<img width="1046" alt="스크린샷 2023-10-08 오후 10 26 36" src="https://github.com/Seohee-Kim/Yolov5-ObjectDetection/assets/62201733/0a456bdd-914f-4c3e-ab4d-3200842355e3">  

</br></br>
## 추론 (Test)  
* Good Case  
![YOLO v5 발표문서-11](https://github.com/Seohee-Kim/Yolov5-ObjectDetection/assets/62201733/9c97569e-a26c-4966-ab1d-3a50b976eeb0)  

* Bad Case  
![YOLO v5 발표문서-12](https://github.com/Seohee-Kim/Yolov5-ObjectDetection/assets/62201733/4672295c-0e9f-4f37-9ed8-02f89e5b277e)  

* 향상된 점  
  * mAP 개선  
  * 객체 사이 공백을 사물로 인식하는 비중 개선  
  * 객체가 아닌 물체를 인식하는 비중 개선  
![YOLO v5 발표문서-13](https://github.com/Seohee-Kim/Yolov5-ObjectDetection/assets/62201733/30b1ade0-90ac-4e66-9011-0f5d06b71cd7)  

</br></br>
## 피드백
* 헬멧을 AWS에서 지원하는 PPE (개인 보호 장비) 디텍션을 1차적으로 거치고, 이후에 개별 모델링으로 보완하여 mAP를 향상시키는 방안


</br></br>
## 참고한 레퍼런스

* https://deep-learning-study.tistory.com/568  
* https://csm-kr.tistory.com/11  
* https://bitcodic.tistory.com/104?category=746696  
* https://github.com/sjchoi86/dl_tutorials_10weeks/blob/master/papers/You%20Only%20Look%20Once-%20Unified%2C%20Real-Time%20Object%20Detection.pdf  
* https://junyoung-jamong.github.io/deep/learning/2019/01/22/Darkflow%EB%A5%BC-%ED%99%9C%EC%9A%A9%ED%95%B4-YOLO%EB%AA%A8%EB%8D%B8-%EC%9D%B4%EB%AF%B8%EC%A7%80-%EB%94%94%ED%85%8D%EC%85%98-%EA%B5%AC%ED%98%84-in-windows.html  
