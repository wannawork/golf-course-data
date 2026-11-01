# 골프장 코스 좌표 데이터 (schemaVersion 2)

한국 골프장의 **홀별 티박스·그린 좌표** 공개 데이터입니다. 골프 거리측정 앱, 지도, 분석 등에 자유롭게 쓸 수 있습니다.

- `index.json` — 골프장 목록 (이름, 별칭, 대표 위치, 홀 수, 수정 시각, 파일 경로)
- `courses/<id>.json` — 골프장 1곳의 홀별 좌표
- `dist/courses.geojson` — 모든 지점을 GeoJSON Point 로 (QGIS, geojson.io, Mapbox 등에서 바로 열림)

## 코스 파일 형식

```json
{
  "schemaVersion": 2,
  "id": "bomun-gc",
  "name": "보문GC",
  "aliases": ["보문gc"],
  "region": "경북 경주시",
  "location": { "lat": 35.85976, "lon": 129.273522 },
  "nines": ["밸리", "레이크"],
  "holes": {
    "1": {
      "par": 4,
      "tees":   [{ "lat": 35.857, "lon": 129.277, "label": "white" }],
      "greens": [{ "lat": 35.858754, "lon": 129.274857, "alt": 134.8, "label": "A" },
                 { "lat": 35.858294, "lon": 129.274744, "alt": 134.8, "label": "B" }]
    }
  },
  "updatedAt": "2026-10-30T08:07:56Z",
  "source": "field"
}
```

| 필드 | 설명 |
|---|---|
| `id` | 파일명·URL 용 영문 슬러그 (한글 이름을 로마자로) |
| `holes` 키 | **실제 홀 번호** 1~54. 9홀 묶음 고정: 1~9, 10~18, 19~27, 28~36, 37~45, 46~54 (27홀 골프장은 1~27, 45홀은 1~45) |
| `tees` | 티박스 위치 (여러 개 가능, `label` 선택: blue/white/red 등) |
| `greens` | 그린 중심. 투그린이면 2개(A/B). 원그린이면 1개 |
| `alt` | 선택 (m). 없으면 앱은 남은거리를 평면(2D)으로 계산 |
| `nines` | 선택. 9홀 묶음 이름 |
| `source` | `field`(현장 기록), `satellite`(항공사진), `field+satellite` |

좌표는 WGS84, 소수점 6자리(약 0.1 m). 모르는 필드는 무시하도록 읽어 주세요(앞으로 필드가 늘 수 있음).

## 라이선스

좌표 데이터: **CC BY 4.0** — 출처만 밝히면 상업적 이용 포함 자유롭게 사용 가능.
좌표는 현장 GPS 기록과 Esri World Imagery / VWorld 항공사진을 보며 입력했습니다.

## 데이터 추가·수정 (관리자)

1. `tools/course-editor.html` 을 브라우저로 열어 좌표를 찍고 **코스 파일 저장** → `courses/` 에 넣기
2. `python3 tools/course_data.py validate` — 같은 좌표 두 홀, 비정상 티→그린 거리 등 점검
3. `python3 tools/course_data.py index` — `index.json` 재생성 + 앱 내장본(app/src/main/assets/course-data) 동기화
4. `python3 tools/course_data.py geojson` — `dist/courses.geojson` 갱신
5. 이 폴더 내용을 공개 저장소(github.com/wannawork/golf-course-data)에 올리면 앱이 6시간 안에 받아갑니다.
