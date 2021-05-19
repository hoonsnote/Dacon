## 운동 동작 분류 AI 경진대회
### Logloss : 0.65244 / Private rank : 26위 (상위 8%)


[주제 및 배경]   
운동 동작 인식 알고리즘 개발   
스마트 헬스케어 산업에 적용 가능한 데이터 분석 방법   



[대회 설명]   
3축 가속도계(accelerometer)와 3축 자이로스코프(gyroscope)를 활용해 측정된 센서 데이터에 머신러닝 알고리즘을 적용해 운동 동작 인식 알고리즘 개발   


[모델링 과정]
1. Data augmentation : 
총 3,125개의 Label 데이터 중 Label 26(Non-exercise)데이터의 수는 1,518개로 약 50%를 차지하여, 이 외 Label만 Augmentation하여 Class Imbalance 문제를 해소하고자 하였습니다. Sensor data augmentation을 위해서, 다양한 방법 중, 1)Shift, 2) Time Warp, 3) Magnitude Warp 방식을 조합하여 활용하였습니다.(참고 : https://github.com/terryum/Data-Augmentation-For-Wearable-Sensor-Data/blob/master/Example_DataAugmentation_TimeseriesData.ipynb)

2. Multi-Sensor Data multimodal learning : 
서로 다른 acc, gy 센서 데이터를 분리한 후 Conv 1D(3 layers) + LSTM(1 layer)을 통해 추출된 각각의 임베딩 값을 Concatenate하여 Classification Layer로 Input되는 구조로 모델링 하였습니다.
