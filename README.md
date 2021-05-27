# OpenVINO-object-Detection
## OpenVINO

https://docs.openvinotoolkit.org/latest/openvino_docs_install_guides_installing_openvino_raspbian.html

Raspbian * OS 용 OpenVINO ™ 툴킷에는 추론 엔진과 MYRIAD 플러그인이 포함되어 있어 USB 포트 중 하나에 연결된 Intel® Neural Compute Stick 2와 함께 사용할 수 있습니다.

openVINO version
>>> 2021.03

## Model zoo

최적화 된 딥 러닝 모델과 고성능 딥 러닝 추론 애플리케이션의 개발을 촉진하기위한 데모 세트가 저장된 github저장소 

```
git clone --depth 1 https://github.com/openvinotoolkit/open_model_zoo
cd open_model_zoo / tools / downloader
python3 -m pip install -r requirements.in
```
- `--name`의 모델 이름을 바꿔주면 원하는 파일을 다운받을 수 있다.
```
python3 downloader.py --name face-detection-adas-0001 
```

- 코드를 실행해 준다. 
```
./armv7l/Release/object_detection_sample_ssd -m face-detection-adas-0001/FP16/face-detection-adas-0001.xml -d MYRIAD -i <path_to_image>
```

## face-detection 결과
people-1979261_1280.jpg![image](https://user-images.githubusercontent.com/78781222/119826013-8eccf980-bf32-11eb-9c7e-6d9531b2ffc7.png)
out_0.bmp![image](https://user-images.githubusercontent.com/78781222/119826054-98eef800-bf32-11eb-8604-3b7b8f4dba2e.png)

## 궁금증이 생김 
- NCS2가 제거 되었을 때 실행이 되는가?
>>> can not init myriad device nc_error 발생

myriad플러그인이 세팅이 되어 있어 NCS2가 빠졌을 시 호환이 되지 않았다. 

## 오류 발생 
- [ERROR] Unsupported layer type : FakeQuantize
>>> 코드를 실행할때 FP32와 FP16으로 파일이 나눠는데, NCS2스틱은 FP16을 지원하므로 나는 에러 
Seems to MYRIAD can not handle INT models (FP32-INT1, FP32-INT8)

### FP32와 FP16의 차이( Floating Point )
FP32 - Single Precision(보통의 float) 

FP16 - Half Precision(float 절반의 정밀도) 

32 비트 단 정밀도 바이너리 형식에 비해 장점 은 저장소와 대역폭의 절반이 필요하다는 것 입니다 (정밀도와 범위를 희생).
산술 계산을 수행하는 데 더 높은 정밀도가 필수적이지 않은 응용 프로그램에서 부동 소수점 값을 저장하기위한 것

![image](https://user-images.githubusercontent.com/78781222/119828347-0b60d780-bf35-11eb-80bc-9ec115513422.png)

- [ERROR]Could Not Find a Package Configuration File Provided by “InferenceEngine”
  "InferenceEngine"에서 제공하는 패키지 구성 파일을 찾을 수 없습니다.
  
![image](https://user-images.githubusercontent.com/78781222/119830684-7ca18a00-bf37-11eb-9269-5f8be01d2063.png)
>>> Cmake를 활용하기 위해선 반드시 독자적인 build파일용 디렉토리를 생성해야하므로 원래 생성되어 있던 파일에 다시 Cmake를 쓰면 안됬다..


# 다른 모델 사용하기 

[참고] 
https://docs.openvinotoolkit.org/latest/omz_tools_downloader.html

1. 물체 감지 모델
2. 객체 인식 모델
3. 재 식별 모델
4. 시맨틱 분할 모델
5. 인스턴스 분할 모델
6. 인간 포즈 추정 모델
7. 이미지 처리
8. 텍스트 감지
9. 텍스트 인식
10. 텍스트 스팟 팅
11. 행동 인식 모델
12. 이미지 검색
13. 압축 모델
14. 질문 답변
15. 기계 번역
16. 텍스트 음성 변환

### 인간 포즈 추정 모델
라즈베리 파이에서 모델을 수행하는 도중 리부팅이 되는 상황이 발생된다. 
심지어 NCS2스틱을 꽂는 순간 리부팅이 되는 경우도 많이 생겼다..
전력부족으로 인해 무한 리부팅의 순서를 걷고 있다...

