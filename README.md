# Elastic Cluster MVU Customer Impact Guide

**Author**: Yujin Cho (choyujin@microsoft.com)

이 저장소는 Elastic Cluster Major Version Upgrade(MVU) 실측 결과를 운영 관점에서 정리한 private 배포용 문서입니다.

## 웹 페이지

- 고객용 페이지: `index.html`
- 시각 자료: `assets/*.svg` (PPTX 내용 기반 재구성)

## 핵심 메시지

1. MVU는 migration 없이 in-place major upgrade를 수행해 운영 복잡도를 낮춘다.
2. Zero downtime 기능은 아니며, reconnect 시점과 서비스 정상화 시점은 다르다.
3. 변경 창은 DB outage가 아니라 요청부터 Ready까지 전체 구간으로 잡아야 한다.
4. Reconnect 이후에도 metadata, consistency, auth, SLO 검증 전에는 write를 재개하면 안 된다.
5. Preview 단계에서는 pre-check/rehearsal/post-validation 절차를 표준화해야 한다.
6. Entra auth, write visibility 등 open item은 제품팀 확인이 필요하다.

## 운영 체크리스트

### 사전

- [ ] Inventory (PG/Citus, node/shard, extension, HA/replica)
- [ ] Blocker pre-check (slot, prepared tx, trigger, extension)
- [ ] Probe 준비 (read/write/auth/latency/error)
- [ ] 업그레이드 요청에 authConfig 포함

### 진행 중

- [ ] Application write 중지
- [ ] Async operation + resource state 동시 추적
- [ ] 요청~Ready 전체를 변경 창으로 관리

### 사후

- [ ] Cluster Ready + node version 확인
- [ ] Metadata/shard + consistency 검증
- [ ] Entra 신규 로그인 검증
- [ ] 대표 query 성능 확인 후 단계적 write 재개

## Sources

- Azure updates: https://azure.microsoft.com/ko-kr/updates?id=571504rs
- Deck: https://microsoftapc-my.sharepoint.com/:p:/g/personal/choyujin_microsoft_com/cQrVXWKssoY0QacADNOc5nnDEgUBTxa49xsKH6tuzAqs0uQtwA
