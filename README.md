# RealTime-Object-Detection
손동작 인식 기반 자율주행 로봇 개발

### Development Environment
- 딥러닝 모델 학습 서버 환경
  - Ubuntu v18.04
  - CUDA 10.2, cuDNN 7.6.5, NVIDIA Driver 470
  - Anaconda, Python 3.7.11, OpenCV 4.2, Jupyter Lab, VSCode
- 로봇
  - NVIDIA Jetson Xavier, JetPack 4.4
  - CUDA 10.2, cuDNN 8.0.0, ROS Melodic
  - Python 3.6.9, Tensorflow 2.3.1

### Technology
<p align="center"> 
  <img src="https://img.shields.io/badge/Tensorflow-FF6F00?style=flat-square&logo=Tensorflow&logoColor=white"/></a> &nbsp     
  <img src="https://img.shields.io/badge/YOLOv4-00FFFF?style=flat-square&logo=YOLO&logoColor=white"/></a> &nbsp   
  <img src="https://img.shields.io/badge/OpenCV-5C3EE8?style=flat-square&logo=OpenCV&logoColor=white"/></a> &nbsp
  <img src="https://img.shields.io/badge/Ubuntu-E95420?style=flat-square&logo=Ubuntu&logoColor=white"/></a> &nbsp 
  <img src="https://img.shields.io/badge/ROS-22314E?style=flat-square&logo=ROS&logoColor=white"/></a> &nbsp 
</p>


### Process
#### 사용자 특정 및 전환하는 Human Following Robot
- 딥러닝 학습 서버 노트북과 구동할 로봇의 개발환경 구축
- 알고리즘 자료 조사
- 사람과 마커를 Detection하는 YOLO 모델 만들기
  1. 처음 계획: Person Dataset(COCO 2017) + ArUco markers Dataset(custom)로 YOLOv5 모델 학습
  2. 계획 수정
      - pre-trained weight를 YOLOv5 에서 YOLOv4로 변경
      - Person Dataset을 COCO 데이터셋 대신 MobilityAids 데이터셋 사용
      - ArUco markers Detection하기 위해, YOLO 모델 대신 OpenCV의 aruco 모듈 사용
  3. 계획 수정: OpenCV의 aruco 모듈 대신, Person Dataset(MobilityAids) + ArUco markers Dataset(custom)로 YOLOv4 모델 학습
  4. 계획 수정: inference 환경에 적합하게 Person Dataset 직접 만들어 YOLOv4 모델 학습
  5. 계획 수정: 로봇 플랫폼에서 추론 시 FPS 향상을 위한, 모델 경량화와 최적화 ➡️ YOLOv4 tiny 사용, TensorRT 적용
- DeepSort Algorithm으로 특정 마커를 가진 사람 Tracking 코드 구현
  - Detection 및 Bounding Box 구현
  - Deepsort Algorithm으로 타깃 마커를 가진 사람에게 Track id 부여 후 Tracking
  - 타깃 마커 스위칭 기능 구현
#### 자연스러운 손동작으로 로봇 제어
- 손동작 명령어: `stop`
- 특정된 사람의 body skeleton과 hand skeleton detection
- 사용한 모델
  - trt_pose (https://github.com/NVIDIA-AI-IOT/trt_pose)
  - trt_pose_hand (https://github.com/NVIDIA-AI-IOT/trt_pose_hand)

