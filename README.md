# 뉴노멀 — ATD Korea 대기오염 시각화

> **기업 협력 프로젝트** · 팀 **무디개** · 2025.03 ~ 2025.11 · 🏆 **인재상 수상**

대기 측정 데이터를 지도 위에 시각화하고 고도화하는, **ATD Korea와의 기업 협력 프로젝트**입니다.

측정 장비가 수집한 대기 데이터를 지도·차트로 표현하고, 악취 확산 범위를 시각화해 현장에서 바로 판단할 수 있게 만드는 것이 목표였습니다.

<p align="center">
  <img width="860" alt="악취 확산 지도" src="https://github.com/user-attachments/assets/554c0255-9c68-4243-9037-dae25b889577" />
  <br/><sub>악취 확산 지도 — 네이버 지도 위에 <code>GroundImageOverlay</code>로 확산 범위를 표현</sub>
</p>

| | |
| --- | --- |
| 협업 기업 | **ATD Korea** |
| 팀 | 무디개 |
| 역할 | **팀장 · 프론트엔드** |
| 기간 | 2025.03 ~ 2025.11 |
| 결과 | 🏆 **인재상 수상** |

---

## 이 프로젝트의 제약

> **기업 요구로 프레임워크 사용이 금지되었습니다. 바닐라 HTML · CSS · JavaScript 사용

React를 쓸 수 없다는 건 컴포넌트·상태관리·라우팅을 전부 직접 만들어야 한다는 트에서 이걸 방치하면 페이지마다 같은 코드가 복사되고, 네비게이션 하나 고치는데 다섯 파일을 열어야 합니다.

그래서 **프레임워크 없이 컴포넌트 구조를 직접 설계**하는 것이 프론트엔드의 첫 과제였습니다.

### HTML 조각 + JS 로직 쌍으로 컴포넌트화

`Components/`에 마크업을, `JS/`에 로직을 두고 **1:1로 대응**시켰습니다. 페이지는 마운트 지점만 선언하고, 컴포넌트가 자기 자신을 주입합니다.

```html
<!-- 페이지는 이것만 선언 -->
<div id="navContainer"></div>
<div id="statusBar"></div>
<div id="noticeBar"></div>
```

| 컴포넌트 | 마크업 | 로직 |
| --- | --- | --- |
| 네비게이션 | `Components/nav.html` | `JS/nav.js` · `JS/navToggle.js` |
| 상태 바 | `Components/statusBar.html` | `JS/status.js` |
| 공지 바 | `Components/noticeBar.html` | `JS/noticeBar.js` |
| 대기 정보 | `Components/atmosphereBox.html` | `JS/atmosphereInfoBox.js` |
| 라인 그래프 | `Components/lineGraphBox.html` | `JS/lineGraph.js` |
| 레이더 그래프 | `Components/radarGraph.html` | `JS/radarGraph.js` |
| 온습도 그래프 | `Components/temperatureHumidityGraph.html` | `JS/temperatur
| 원시 데이터 | `Components/rawDataBox.html` | `JS/rawDataBox.js` |
| 위치 데이터 | `Components/locationDataAPI.html` | `JS/locationDataAPI.js` |

**효과**: 네비게이션 구조를 바꿔도 `nav.html` 하나만 고치면 5개 페이지에 동시 가할 때도 마운트 지점 한 줄이면 됩니다.

---

## 화면

<table>
<tr>
<td width="50%">
  <img alt="대시보드" src="https://github.com/user-attachments/assets/d5744ca0-0017-4a18-bfed-3fe9e95b4405" />
  <p align="center"><b>대시보드</b><br/><sub>게이지 · 라인 · 레이더 · 온습도 </p>
</td>
<td width="50%">
  <img alt="악취 확산 지도" src="https://github.com/user-attachments/assets/554c0255-9c68-4243-9037-dae25b889577" />
  <p align="center"><b>악취 확산 지도</b><br/><sub>지도 위 확산 범위 시각화 ·
</td>
</tr>
<tr>
<td width="50%">
  <img alt="포인트별 데이터" src="https://github.com/user-attachments/assets/ebf12605-28d1-428a-b3cb-a063a8f79779" />
  <p align="center"><b>측정 장비 포인트별 데이터</b><br/><sub>측정 지점 단위 데이터 조 </p>
</td>
<td width="50%">
  <img alt="로우데이터" src="https://github.com/user-attachments/assets/0b4a8524-4717-45e6-84af-b2cf3cad6bd5" />
  <p align="center"><b>로우데이터</b><br/><sub>측정 원본 데이터 테이블 조회</
</td>
</tr>
</table>

| 화면 | 파일 |
| --- | --- |
| 대시보드 | `dashboardIndex.html` |
| 악취 확산 | `odorDispersionIndex.html` |
| 포인트별 데이터 | `devicePointIndex.html` |
| 로우데이터 | `rawDataIndex.html` |
| 로그인 | `loginIndex.html` |

---

## 지도 라이브러리 선정

측정 지점은 전국에 흩어져 있고, 악취는 **점이 아니라 면으로 퍼지는** 데이터입니다. 마커만으로는 표현되지 않아, 후보마다 **실제로 돌아가는 데모를 만들어 비교**했습니다.

| 후보 | 검증 폴더 | 결과 |
| --- | --- | --- |
| Leaflet | [`leaflet/`](leaflet) | ❌ |
| Leaflet.heat | [`leaflet_heat/`](leaflet_heat) | ❌ |
| D3.js | [`D3/`](D3) | ❌ |
| **네이버 지도 API v3** | [`naver_map/`](naver_map) | ✅ **채택** |

**네이버 지도를 선택한 이유**는 국내 서비스이기 때문입니다. 측정 지점이 전부   쓰는 화면인데, OSM 기반 타일은 국내 지명·건물 정보의 밀도와 최신성이떨어집니다. 지도가 익숙하지 않으면 "지금 어디를 보고 있는지"부터 헷갈립니다.

각 검증 폴더는 `*_index.html` + `*.js` + `*.css` 세트로 **단독 실행**됩니다. 브라우저로 바로 열어 비교할 수 있게 만들었습니다.

### 커스텀 오버레이 직접 구현

히트맵 플러그인을 쓸 수 없게 되면서, `naver.maps.OverlayView`를 확장해 필요한 표현을 직접 만들었습니다.

| 오버레이 | 역할 |
| --- | --- |
| `MapDeviceManager` | 측정 장비 마커 관리 |
| `TypeLabelOverlay` | 장비 유형 라벨 |
| `DeviceDetailOverlay` | 장비 상세 정보 팝업 |
| **`GroundImageOverlay`** | **악취 확산 범위를 지도 위 이미지 레이어로 표현*

악취 확산은 결국 `GroundImageOverlay`로 해결했습니다. 히트맵 플러그인의 일반  시뮬레이션 결과 이미지를 지도 좌표에 정합해 얹는** 방식입니다.

---

## 기술 스택

| 영역 | 사용 기술 |
| --- | --- |
| 프론트엔드 | **바닐라 HTML · CSS · JavaScript** (프레임워크 미사용) |
| 지도 | **네이버 지도 API v3** + 커스텀 `OverlayView` |
| UI | jQuery · Moment.js · daterangepicker · Pretendard |
| 백엔드 | **Flask** · PyMySQL · Flask-CORS |
| 데이터베이스 | **MySQL** |
| 배포 | Cloudtype |
| 검증용 | Leaflet · Leaflet.heat · D3.js |

---

## 구조

```
.
├── Frontend/
│   ├── dashboardIndex.html         # 대시보드
│   ├── odorDispersionIndex.html    # 악취 확산
│   ├── devicePointIndex.html       # 포인트별 데이터
│   ├── rawDataIndex.html           # 로우데이터
│   ├── loginIndex.html             # 로그인
│   ├── Components/                 # 컴포넌트 마크업 (9종)
│   ├── JS/                         # 컴포넌트 로직 (15종)
│   ├── CSS/                        # 컴포넌트 스타일
│   ├── globalStyle.css
│   └── api_proxy.php               # API 프록시
│
├── Backend/
│   ├── app.py                      # Flask API
│   ├── requirements.txt
│   └── .cloudtype/
│
├── naver_map/                      # ✅ 채택
├── leaflet/  leaflet_heat/  D3/    # 검증 후 미채택
```

---

## Backend API

측정 지점(위경도) CRUD를 제공합니다. **Base path** `/newnormal-test`

| 메서드 | 엔드포인트 | 설명 |
| --- | --- | --- |
| `GET` | `/api/data` | 전체 측정 지점 조회 |
| `POST` | `/api/data` | 지점 등록 |
| `PUT` | `/api/data/<id>` | 지점 위경도 수정 |
| `DELETE` | `/api/data/<id>` | 지점 삭제 |

**테이블** `newnormal_table` — `location_ID` · `latitude` · `longitude`

---
