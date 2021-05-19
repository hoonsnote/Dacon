# 운동 동작 분류 AI 


본 대회의 작업 과정은 다음과 같습니다.

1. Data augmentation : 
총 3,125개의 Label 데이터 중 Label 26(Non-exercise)데이터의 수는 1,518개로 약 50%를 차지하여, 이 외 Label만 Augmentation하여 Class Imbalance 문제를 해소하고자 하였습니다. Sensor data augmentation을 위해서, 다양한 방법 중, 1)Shift, 2) Time Warp, 3) Magnitude Warp 방식을 조합하여 활용하였습니다.(참고 : https://github.com/terryum/Data-Augmentation-For-Wearable-Sensor-Data/blob/master/Example_DataAugmentation_TimeseriesData.ipynb)

2. Multi-Sensor Data multimodal learning : 
서로 다른 acc, gy 센서 데이터를 분리한 후 1DCNN+LSTM Layer를 통해 추출된 각각의 임베딩 값을 Concatenate하여 Classification Layer로 Input되는 구조로 모델링 하였습니다.
