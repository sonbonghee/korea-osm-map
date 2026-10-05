# Korea OSM Map

대한민국 OpenStreetMap 데이터를 자체 호스팅하는 Coolify용 Docker Compose 프로젝트입니다.

- 지도 데이터: [Geofabrik South Korea](https://download.geofabrik.de/asia/south-korea.html)
- 타일 생성: [Planetiler OpenMapTiles](https://github.com/openmaptiles/planetiler-openmaptiles)
- 지도 서버: [TileServer GL](https://github.com/maptiler/tileserver-gl)
- 지도 스타일: [OSM Bright](https://github.com/openmaptiles/osm-bright-gl-style)
- 서비스 주소: [map.airquiz.kr](https://map.airquiz.kr/)

## 구성

1. `korea-tile-builder`가 최신 `south-korea-latest.osm.pbf`를 내려받아 `/data/korea.mbtiles`를 생성합니다.
2. `map-style-init`이 OSM Bright 스타일과 글꼴, TileServer GL 설정 및 루트 리디렉션 페이지를 준비합니다.
3. `tileserver-gl`이 벡터/래스터 타일과 지도 뷰어를 8080 포트로 제공합니다.

지도 원본과 생성된 MBTiles는 Docker 볼륨에 저장되며 Git 저장소에는 포함되지 않습니다.

## Coolify 배포

1. GitHub 저장소를 소스로 하는 새 Docker Compose 리소스를 만듭니다.
2. Compose 경로를 `/docker-compose.yml`로 지정합니다.
3. `tileserver-gl` 서비스의 8080 포트에 `https://map.airquiz.kr` 도메인을 연결합니다.
4. 배포를 시작합니다. 최초 배포는 한국 전체 타일을 생성하므로 서버 사양과 네트워크에 따라 시간이 걸립니다.

권장 메모리는 최소 20GB이며, Planetiler JVM 최대 힙은 현재 16GB입니다. 메모리가 부족한 서버에서는 `JAVA_TOOL_OPTIONS`를 서버 용량에 맞게 낮추고 최대 확대 수준도 함께 조정해야 합니다.

## 데이터 갱신

재배포 시 builder가 최신 Geofabrik 데이터를 내려받아 MBTiles를 다시 생성합니다. 기존 동작 중인 지도를 갱신할 때는 생성 시간 동안 서비스 영향이 있을 수 있으므로 점검 시간에 수행하는 것을 권장합니다.

## 라이선스

지도 데이터는 © OpenStreetMap contributors이며 [ODbL](https://www.openstreetmap.org/copyright) 조건을 따릅니다. 각 이미지와 소프트웨어의 라이선스는 해당 upstream 프로젝트를 참조하세요.
