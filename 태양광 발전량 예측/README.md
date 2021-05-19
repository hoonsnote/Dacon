## 태양광 발전량 예측 AI 경진대회
### Pinball loss : 2.01536 / Private rank : 7위 (상위 2%)

[대회 설명]
> 7일(Day 0~ Day6) 동안의 데이터를 인풋으로 활용하여, 향후 2일(Day7 ~ Day8) 동안의 30분 간격의 발전량(TARGET)을 예측 (1일당 48개씩 총 96개 타임스텝에 대한 예측)

[데이터 구성]
> Hour - 시간      Minute - 분   DHI - 수평면 산란일사량(Diffuse Horizontal Irradiance (W/m2))   DNI - 직달일사량(Direct Normal Irradiance (W/m2))
> WS - 풍속(Wind Speed (m/s))
> RH - 상대습도(Relative Humidity (%))
> T - 기온(Temperature (Degree C))
> Target - 태양광 발전량 (kW)

[모델링 과정]
본 대회에서 Light GBM을 활용한 예측과 CNN을 활용한 예측을 진행하였습니다. 본 대회의 평가지표인 pinball_loss(quantile)를 최소화하는 모델 구축을 위해 두 가지 방법 모두 quantile_loss를 loss_function으로 설정하였습니다. 또한, 예측해야하는 2일 중 각각의 날짜를 Target값으로 설정하여 결론적으로는, 일자별(2) * 0.1~0.9quantile별(9)별로 총 18번의 학습을 통해 예측값을 구하였습니다.

1) Light GBM 활용 모델링
본 대회의 데이터는 Target값인 태양광 발전량이 0인 시간대(해가 떠있지 않은 시간)가 높은 비중을 차지하고 있습니다. 해가 떠있는 시간대에서의 Target값에 대한 정밀한 예측값 산출을 위해 Binary Classification(0 : 발전량 X / 1 : 발전량 O) 진행 후(Validation 정확도 99% 이상), 1로 예측된 값에 대해서만 추가적인 Regression 모델링 작업을 진행하였습니다. 
파생 변수로는 일출/일몰 시간, 일정 날짜 동안의 동시간대 기본 변수들의 평균값 등을 추가적으로 생성하였습니다. 

2) CNN 활용 모델링
시간대별로 명확한 Target 값 차이가 존재하기 때문에, 동일 시간대의 값을 위주로 학습하는 것이 중요하다고 생각되었습니다. 이를 위해 Dilated_rate를 하루 간격인 48로 설정하여(24h/30m), 여러개의 Conv Layer를 거쳐 임베딩 값의 time_step이 7일치에서 최종적으로는 1일치로 줄어들도록 모델을 구성하였습니다. 또한, 특정 시간대의 바로 앞, 뒤 시점의 값을 고려하여 더욱 복잡한 시계열성을 학습할 수 있도록, 가장 첫번째 Conv Layer window_size 3의 Conv Layer로 구성하였습니다.
![image](https://user-images.githubusercontent.com/53526441/118768637-36568600-b8ba-11eb-8e87-017e0a283f0b.png)
