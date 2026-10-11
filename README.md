# 🧪 BeauRo - Cosmetics R&D Automated Dispensing System

<div align="center">

![Doosan Robotics](https://img.shields.io/badge/Doosan-M0609-blue?style=for-the-badge)
![ROS2](https://img.shields.io/badge/ROS2-Humble-blue?style=for-the-badge)
![Python](https://img.shields.io/badge/Python-3.8+-green?style=for-the-badge)
![Firebase](https://img.shields.io/badge/Firebase-Realtime_DB-orange?style=for-the-badge)

**두산 로보틱스 M0609 협동로봇을 활용한 화장품 연구개발용 화학약품 자동배합 시스템**

[Features](#-주요-기능) • [Architecture](#-시스템-아키텍처) • [Installation](#-설치-방법) • [Usage](#-사용-방법) • [Contributing](#-기여하기)

</div>

---

## 📋 목차

- [프로젝트 소개](#-프로젝트-소개)
- [주요 기능](#-주요-기능)
- [시스템 아키텍처](#-시스템-아키텍처)
- [기술 스택](#-기술-스택)
- [설치 방법](#-설치-방법)
- [사용 방법](#-사용-방법)
- [프로젝트 구조](#-프로젝트-구조)
- [워크플로우](#-워크플로우)
- [에러 핸들링](#-에러-핸들링-및-복구)
- [설정 파일](#-설정-파일)
- [트러블슈팅](#-트러블슈팅)
- [기여하기](#-기여하기)
- [팀](#-팀)

---

## 🎯 프로젝트 소개

**BeauRo**는 화장품 연구개발(R&D) 단계에서 필요한 화학약품의 정밀 배합 작업을 자동화하는 시스템입니다. 두산 로보틱스의 협동로봇 M0609 모델을 기반으로 하며, ROS2 프레임워크와 Firebase 실시간 데이터베이스를 활용하여 원격 모니터링 및 제어가 가능합니다.

### 🔬 개발 배경

화장품 R&D 과정에서 다양한 성분의 배합 테스트는 필수적이지만, 다음과 같은 문제점이 존재합니다:
- 반복적인 수작업으로 인한 연구원의 피로도 증가
- 미세한 배합 비율 차이로 인한 실험 재현성 문제
- 유해 화학물질 노출 위험
- 시간과 인력 자원의 비효율적 사용

BeauRo는 이러한 문제를 해결하고 R&D 프로세스를 효율화하기 위해 개발되었습니다.

### 🎓 적용 분야

- 화장품 포뮬러 개발 및 테스트
- DoE (Design of Experiments) 실험 자동화
- 배합 조건에 따른 물성 변화 연구
- 신소재 적용 가능성 테스트
- 품질 관리를 위한 재현성 높은 샘플 제작

---

## ✨ 주요 기능

### 🤖 로봇 자동화 작업

#### 1. **액상 재료 분주 (Liquid Dispensing)**
- 피펫 그리퍼를 활용한 정밀 흡입/분주
- 다단계 속도 제어로 액체 비산 방지
- 멀티웰 플레이트 자동 인식 및 배치

#### 2. **분말 재료 투입 (Powder Scooping)**
- 스푼 그리퍼를 이용한 정량 채취
- 레벨링(평탄화) 동작으로 정확한 양 확보
- 쏟기 동작 최적화로 분말 손실 최소화

#### 3. **자동 교반 (Automated Stirring)**
- 스틱 그리퍼로 각 웰 자동 교반
- 3회 반복 패턴으로 균일한 혼합 보장
- 트레이 좌표 기반 자동 위치 계산

#### 4. **트레이 이송 (Tray Transfer)**
- 완성된 샘플 트레이 자동 출력
- 안전한 그립 및 이송 경로

### 🌐 웹 기반 제어 시스템

- **Firebase Realtime Database 연동**
  - 실시간 작업 진행률 모니터링
  - 원격 명령 전송 (시작/중지/재개)
  - 에러 상태 즉시 알림

- **DoE Matrix 입력**
  - 웹 인터페이스를 통한 배합 레시피 설정
  - Well별 각 성분의 투입 횟수 지정
  - JSON 형식의 유연한 실험 설계

### 🛡️ 안전 및 복구 기능

- **자동 에러 감지**
  - 로봇 상태 실시간 모니터링 (3, 5, 6, 9, 10번 에러 상태)
  - 에러 발생 시 즉시 정지 및 알림

- **체크포인트 기반 복구**
  - 작업 단계별 상태 자동 저장
  - 에러 복구 후 중단 지점부터 재개
  - 웹에서 Resume/Stop 선택 가능

- **다단계 복구 전략**
  1. Safe Stop 해제
  2. Servo-On 재시도 (최대 3회)
  3. 필요 시 전원 재기동 가이드 제공

---

## 🏗️ 시스템 아키텍처

```
┌─────────────────────────────────────────────────────────────┐
│                        Web Interface                        │
│  (Order Management, Real-time Monitoring, Error Control)   │
└──────────────────────┬──────────────────────────────────────┘
                       │
                       │ Firebase Realtime DB
                       │
┌──────────────────────▼──────────────────────────────────────┐
│                    BeauRo Controller                        │
│  ┌──────────────────────────────────────────────────────┐  │
│  │  • Order Processing                                   │  │
│  │  • Task State Management (Firebase Checkpoint)       │  │
│  │  • Error Detection & Recovery                        │  │
│  │  • Progress Reporting                                │  │
│  └──────────────────────────────────────────────────────┘  │
└──────────────────────┬──────────────────────────────────────┘
                       │
                       │ ROS2 Service/Action
                       │
┌──────────────────────▼──────────────────────────────────────┐
│              Doosan Robotics M0609 Controller               │
│  ┌──────────────────────────────────────────────────────┐  │
│  │  DSR_ROBOT2 Library                                   │  │
│  │  • Motion Control (movel, movej)                     │  │
│  │  • Digital I/O Control (Gripper)                     │  │
│  │  • Robot State Monitoring                            │  │
│  └──────────────────────────────────────────────────────┘  │
└──────────────────────┬──────────────────────────────────────┘
                       │
                       │ Hardware Control
                       │
┌──────────────────────▼──────────────────────────────────────┐
│                  Physical Components                        │
│  • M0609 6-axis Collaborative Robot                        │
│  • Pneumatic Grippers (Pipette, Spoon, Stick)             │
│  • Material Containers & Well Plates                       │
│  • Safety Sensors                                          │
└─────────────────────────────────────────────────────────────┘
```

### 데이터 흐름

```
Web → Firebase → BeauRo Controller → ROS2 → Robot → Hardware
                      ↓
                 Checkpoint
                  (Resume)
                      ↑
Web ← Firebase ← Error Handler ← Robot State Monitor
```

---

## 🔧 기술 스택

### 로봇 제어
- **ROS2 Humble**: 로봇 운영체제 프레임워크
- **DSR_ROBOT2**: 두산 로보틱스 공식 파이썬 라이브러리
- **Python 3.8+**: 메인 프로그래밍 언어

### 통신 및 데이터베이스
- **Firebase Realtime Database**: 실시간 데이터 동기화
- **Firebase Admin SDK**: 서버 사이드 데이터베이스 제어

### 하드웨어
- **두산 로보틱스 M0609**: 6축 협동로봇 (가반하중 6kg, 도달거리 900mm)
- **공압 그리퍼**: 피펫/스푼/스틱용 3종
- **디지털 I/O**: 그리퍼 제어 신호

### 설정 관리
- **YAML**: 재료 라이브러리 및 좌표 관리
- **JSON**: Firebase 데이터 구조

---

## 📦 설치 방법

### 시스템 요구사항

- Ubuntu 22.04 LTS
- ROS2 Humble
- Python 3.8 이상
- 두산 로보틱스 M0609 제어기
- 인터넷 연결 (Firebase 통신용)

### 1. ROS2 설치

```bash
# ROS2 Humble 설치
sudo apt update && sudo apt install -y software-properties-common
sudo add-apt-repository universe
sudo apt update && sudo apt install -y curl
sudo curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -o /usr/share/keyrings/ros-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(. /etc/os-release && echo $UBUNTU_CODENAME) main" | sudo tee /etc/apt/sources.list.d/ros2.list > /dev/null
sudo apt update
sudo apt install -y ros-humble-desktop
```

### 2. 두산 로보틱스 라이브러리 설치

```bash
# 작업 공간 생성
mkdir -p ~/doosan_ws/src
cd ~/doosan_ws/src

# 두산 로보틱스 ROS2 패키지 클론
git clone https://github.com/doosan-robotics/doosan-robot2.git

# 의존성 설치
cd ~/doosan_ws
rosdep install --from-paths src --ignore-src -r -y

# 빌드
colcon build --symlink-install
source install/setup.bash
```

### 3. BeauRo 프로젝트 설치

```bash
# 프로젝트 클론
git clone https://github.com/rokey-hyunj/beauro_ws.git
cd beauro_ws

# Python 의존성 설치
pip install -r requirements.txt
```

**requirements.txt**:
```
firebase-admin>=6.0.0
PyYAML>=6.0
rclpy
```

### 4. Firebase 설정

1. [Firebase Console](https://console.firebase.google.com/)에서 프로젝트 생성
2. Realtime Database 활성화
3. 서비스 계정 키 다운로드
4. `serviceAccountKey.json` 파일을 프로젝트 루트에 저장

```bash
# 보안을 위해 .gitignore에 추가
echo "serviceAccountKey.json" >> .gitignore
```

### 5. 환경 설정

**material_library.yaml** 파일 생성:

```yaml
# 액상 재료
liquid_A:
  name: "Hyaluronic Acid Solution"
  type: "liquid"
  position: [x, y, z, rx, ry, rz]  # 로봇 좌표계
  
liquid_B:
  name: "Vitamin C Serum"
  type: "liquid"
  position: [x, y, z, rx, ry, rz]

# 분말 재료
powder_A:
  name: "Titanium Dioxide"
  type: "powder"
  position: [x, y, z, rx, ry, rz]
  
powder_B:
  name: "Mica Powder"
  type: "powder"
  position: [x, y, z, rx, ry, rz]

# 교반 스틱
stick:
  ready_1: [x, y, z, rx, ry, rz]
  ready_2: [x, y, z, rx, ry, rz]
  grab: [x, y, z, rx, ry, rz]
  drop: [x, y, z, rx, ry, rz]
  finished: [x, y, z, rx, ry, rz]

# 트레이 출력
tray_out:
  ready_1: [x, y, z, rx, ry, rz]
  ready_2: [x, y, z, rx, ry, rz]
  grab: [x, y, z, rx, ry, rz]
  drop: [x, y, z, rx, ry, rz]
  finished: [x, y, z, rx, ry, rz]

# 트레이 좌표
tray_base:
  base: [x, y, z, rx, ry, rz]
  pitch_x: 57.0  # mm
  pitch_y: 38.0  # mm
```

---

## 🚀 사용 방법

### 1. ROS2 환경 설정

```bash
# 터미널 1: ROS2 소스
source /opt/ros/humble/setup.bash
source ~/beauro_ws/install/setup.bash
```

### 2. 로봇 연결 및 준비

```bash
# 로봇 제어기 IP 확인 (기본값: 192.168.137.100)
# 네트워크 설정 확인
ping 192.168.137.100

# 로봇 전원 ON → Servo-On 상태 확인
```

### 3. BeauRo 실행

```bash
# 프로젝트 디렉토리로 이동
cd beauro_ws

# 메인 스크립트 실행
python3 beauro.py
```

**실행 로그 예시**:
```
🔑 Initializing Firebase...
✅ Connected.
🤖 Initializing Robot...
   - Robot ID: dsr01
   - Model: m0609
   - Tool: Tool Weight
   - TCP: GripperDA_v1
✅ Robot Ready.

✅ Executor Ready. Waiting for Web Order...
```

### 4. 웹 인터페이스에서 주문 생성

Firebase Realtime Database에 다음 형식으로 주문 생성:

```json
{
  "current_order": {
    "order_id": "order_20240110_001",
    "status": "start",
    "doe_matrix": [
      {
        "well_id": "A1",
        "counts": {
          "l1": 2,
          "l2": 1,
          "p1": 1,
          "p2": 0
        }
      },
      {
        "well_id": "A2",
        "counts": {
          "l1": 1,
          "l2": 2,
          "p1": 0,
          "p2": 1
        }
      }
    ]
  }
}
```

**필드 설명**:
- `order_id`: 고유 주문 식별자
- `status`: `start` (시작 대기) / `processing` (진행 중) / `finished` (완료)
- `doe_matrix`: 웰별 배합 정보
  - `well_id`: 웰 플레이트 위치 (A1, A2, ...)
  - `counts`: 각 재료별 투입 횟수
    - `l1`, `l2`: 액상 재료 1, 2
    - `p1`, `p2`: 분말 재료 1, 2

### 5. 진행 상황 모니터링

Firebase 경로 `/robot_state`에서 실시간 상태 확인:

```json
{
  "robot_state": {
    "state": 2,  // 1: Idle, 2: Working, 3: Error
    "message": "Dispensing l1 to A1 (1/2)",
    "progress": 15,
    "phase": 1,  // 1: Dispensing, 2: Mixing
    "is_error": false,
    "error_message": ""
  }
}
```

---

## 📁 프로젝트 구조

```
beauro/
│
├── beauro.py                    # 메인 실행 파일
├── material_library.yaml        # 재료 및 좌표 라이브러리
├── serviceAccountKey.json       # Firebase 인증 키 (비공개)
├── requirements.txt             # Python 의존성
│
├── docs/                        # 문서
│   ├── installation.md
│   ├── coordinate_teaching.md
│   └── troubleshooting.md
│
├── config/                      # 설정 파일
│   └── robot_config.yaml
│
└── README.md                    # 본 파일
```

---

## 🔄 워크플로우

### 전체 작업 흐름

```
1. 주문 수신 (Firebase: status = 'start')
   ↓
2. DoE Matrix 파싱
   ↓
3. Phase 1: Dispensing (배합)
   ├── L1 액상 분주
   ├── L2 액상 분주
   ├── P1 분말 투입
   └── P2 분말 투입
   ↓
4. Phase 2: Mixing (교반)
   ├── 스틱 교반 (각 웰 3회 반복)
   └── 완성 트레이 이송
   ↓
5. 주문 완료 (Firebase: status = 'finished')
```

### 액상 분주 세부 단계 (Liquid)

```
LIQUID_INIT → LIQUID_PICK → LIQUID_SUCTION → LIQUID_DISPENSE → LIQUID_RETURN
     ↓              ↓              ↓                  ↓                ↓
  홈 위치      피펫 파지      액체 흡입          웰에 분주         피펫 반납
```

### 분말 투입 세부 단계 (Powder)

```
POWDER_INIT → POWDER_PICK → POWDER_SCOOP_MOVE → POWDER_SCOOP → POWDER_FLATTEN
     ↓              ↓              ↓                  ↓                ↓
  홈 위치      스푼 파지      스쿱 위치 이동      분말 채취        레벨링
     ↓
POWDER_POUR_MOVE → POWDER_POUR → POWDER_RETURN
     ↓                  ↓              ↓
  웰 상단 이동       분말 투입       스푼 반납
```

---

## 🛡️ 에러 핸들링 및 복구

### 자동 감지 에러 상태

| 상태 코드 | 의미 | 복구 전략 |
|----------|------|----------|
| 3 | Safe Stop | Safe Stop 해제 시도 |
| 5 | E-Stop 활성화 | 수동 E-Stop 해제 필요 |
| 6 | Protective Stop | Safe Stop 해제 후 재시도 |
| 9 | Servo-Off | Servo-On 재시도 (최대 3회) |
| 10 | 전원 OFF | 전원 재기동 가이드 제공 |

### 복구 프로세스

```python
에러 감지
  ↓
로봇 정지 + Firebase 에러 알림
  ↓
웹에서 복구 명령 대기 (resume/stop)
  ↓
Resume 선택 시
  ↓
Safe Stop 해제 시도
  ↓
Servo-On 재시도 (최대 3회)
  ↓
성공 → 체크포인트부터 재개
실패 → 수동 개입 요청
```

### 체크포인트 시스템

모든 작업 단계마다 Firebase에 상태 저장:

```json
{
  "task_checkpoint": {
    "order_id": "order_20240110_001",
    "task_type": "liquid",  // liquid, powder, stick, tray
    "tray_idx": 0,
    "count_idx": 1,
    "step": "LIQUID_DISPENSE",
    "material_key": "liquid_A",
    "error_count": 0,
    "timestamp": 1704873600
  }
}
```

에러 복구 후 중단된 지점부터 자동으로 재개됩니다.

---

## ⚙️ 설정 파일

### beauro.py 내부 상수

```python
# 로봇 식별
ROBOT_ID = "dsr01"
ROBOT_MODEL = "m0609"

# 속도 프로파일
VEL_MOVE = 2000      # 빠른 이동 속도 (mm/s)
VEL_WORK = 500       # 작업 속도
VEL_SPOON = 250      # 스푼 작업 속도 (정밀)
ACC = 500            # 가속도 (mm/s²)
ACC_SPOON = 200      # 스푼 작업 가속도

# 트레이 간격
TRAY_PITCH_X = 57.0  # X축 웰 간격 (mm)
TRAY_PITCH_Y = 38.0  # Y축 웰 간격 (mm)

# Firebase 경로
FIREBASE_DB_URL = 'https://beauro.firebaseio.com/'
```

### 그리퍼 제어 신호

```python
# Digital Output 매핑
GRIPPER_SIGNALS = {
    "init": [1, 0, 0, 0, 0, 0],     # 초기화
    "hold": [0, 1, 0, 0, 0, 0],     # 파지
    "flatten": [0, 0, 1, 0, 0, 0],  # 레벨링
    "release": [0, 0, 0, 1, 0, 0]   # 해제
}
```

---

## 🔍 트러블슈팅

### 문제 1: Firebase 연결 실패

**증상**: `❌ Init Error: ...`

**해결 방법**:
1. `serviceAccountKey.json` 파일 존재 확인
2. Firebase 프로젝트 URL 확인
3. 인터넷 연결 확인
4. Firebase 프로젝트 권한 확인

### 문제 2: 로봇 응답 없음

**증상**: 로봇이 명령에 반응하지 않음

**해결 방법**:
1. 로봇 제어기 네트워크 연결 확인
2. Servo-On 상태 확인
3. E-Stop 해제 확인
4. ROS2 노드 상태 확인: `ros2 node list`

### 문제 3: 좌표 정확도 문제

**증상**: 로봇이 목표 위치에 정확히 도달하지 못함

**해결 방법**:
1. 티칭 펜던트로 좌표 재교시
2. `material_library.yaml` 업데이트
3. TCP 오프셋 확인
4. 로봇 캘리브레이션 수행

### 문제 4: 복구 실패 반복

**증상**: 에러 복구가 계속 실패함

**해결 방법**:
1. 로봇 제어기 재부팅
2. 수동으로 안전한 위치로 이동
3. 체크포인트 삭제: `db.reference('task_checkpoint').delete()`
4. 주문 재시작

---

## 🤝 기여하기

프로젝트 개선에 참여해주세요!

### 기여 방법

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

### 이슈 제보

버그 발견 시 다음 정보와 함께 이슈 등록:
- 로봇 모델 및 펌웨어 버전
- ROS2 버전
- 에러 로그
- 재현 방법

---

## 👥 팀

- **로봇 제어**: [김현종] - 파우더 스쿠핑 동작 및 안전 시스템 설계
- **로봇 제어**: [정예찬] - 스틱 stirring 동작 및 3D CAD 설계
- **로봇 제어**: [배민혁] - 스포이드 피펫팅 동작 설계
- **웹 개발**: [황익주] - Firebase 통신 및 상태 관리 및 웹 인터페이스
- **하드웨어**: [김남규] - 3D CAD 설계 및 프린팅, 문서 작업

## 📞 문의

- **이메일**: hyunjongkim0524@gmail.com
- **이슈 트래커**: [GitHub Issues](https://github.com/rokey-hyunj/beauro_ws/issues)

---

## 🗺️ 로드맵

### v1.0 (완료)
- [x] 기본 액상/분말 배합 기능
- [x] Firebase 실시간 모니터링
- [x] 자동 에러 복구

### v1.1 (진행 중)
- [ ] 다중 트레이 지원 (96-well plate)
- [ ] 재료 재고 관리 시스템
- [ ] 배합 이력 데이터베이스

### v2.0 (계획)
- [ ] 비전 시스템 통합 (재료 인식)
- [ ] AI 기반 최적 배합 추천
- [ ] 다중 로봇 협업 시스템

---

<div align="center">

**⭐ 이 프로젝트가 유용하다면 Star를 눌러주세요! ⭐**

Made with ❤️ by BeauRo Team

</div>
