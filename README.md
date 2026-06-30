# SO-101 Web Simulator

TheRobotStudio **SO-ARM101(SO-101)** 6DOF 로봇팔의 웹 시뮬레이터입니다. 브라우저에서 관절을 움직이고 키프레임으로 모션을 만들어 재생합니다. (실물 연결 없음, 시각화 전용)

공식 URDF/STL 출처: [TheRobotStudio/SO-ARM100](https://github.com/TheRobotStudio/SO-ARM100) (`Simulation/SO101/so101_new_calib.urdf`).

## 실행

`index.html`을 정적 호스팅(GitHub Pages 등)에 올리면 됩니다. 페이지를 열면 `data/`의 URDF + STL이 자동 로드됩니다. 로컬에서 바로 더블클릭하면 보안정책(fetch) 때문에 모델이 안 뜰 수 있으니, 간단한 로컬 서버로 여세요:

```bash
python3 -m http.server 8000
# http://localhost:8000 접속
```

## 구성

```
so101-sim/
├─ index.html          # 시뮬레이터 (단일 파일)
└─ data/
   ├─ so101.urdf       # 관절/링크 정의 (메시경로 평면화됨)
   └─ *.stl            # 부품 메시 13개
```

## 관절 (6DOF)

| 관절 | 범위 |
|---|---|
| shoulder_pan | ±110° |
| shoulder_lift | ±100° |
| elbow_flex | ±97° |
| wrist_flex | ±95° |
| wrist_roll | −157°~163° |
| gripper | −10°~100° |

모든 관절 0 = URDF 기본 자세.

## 사용법

- **3D 뷰**: 좌클릭 회전 · 우클릭 이동 · 휠 줌
- **관절 패널(우측)**: 슬라이더 또는 −/+ 버튼(1°, 길게 누르면 연속)으로 자세 조절. 상단 검색으로 관절 필터.
- **그리드 / 와이어프레임 / 관절축** 토글 (좌상단)
- **리셋**: 모든 관절 0으로

## 타임라인 (모션 제작/재생)

1. 자세를 만든 뒤 원하는 시점(1초 단위)을 클릭하고 **+ 키프레임**.
2. 시점을 바꿔가며 키프레임 여러 개 추가.
3. **▶** 재생(키프레임 사이 보간), **루프**·**속도** 지원.
4. **저장**하면 `so101_motion.json`으로 내려받고, **불러오기**로 복원.

모션 JSON 형식:
```json
{ "duration": 30,
  "keyframes": [ { "time": 0, "pose": { "shoulder_pan": 0, "elbow_flex": -30, ... } } ] }
```

## 비고

- 메시 경로는 URDF에서 `assets/` 없이 파일명만 참조하도록 평면화했고, STL은 모두 `data/`에 둡니다.
- 실물(Feetech STS3215) 제어는 포함하지 않습니다. 필요 시 별도 추가.
