## 데이터 접근 방식별 Latency 비교

| 방식 | 가정 | 대표 Latency | RAM 대비 상대 속도 |
|---|---|---:|---:|
| **Local RAM** | 프로세스 메모리에서 직접 접근 | **~80–100 ns** (`0.00008–0.0001 ms`) | **1×** |
| **Disk — NVMe SSD** | 4 KB random read, page cache miss | **~20–130 µs** (`0.02–0.13 ms`) | **~200–1,300×** |
| **SQL `SELECT`** | PostgreSQL, indexed/simple query, connection reuse | **~0.07–0.6 ms** | **~700–6,000×** |
| **Redis `GET`** | 동일 host 또는 LAN, connection reuse | **~0.1–0.6 ms** | **~1,000–6,000×** |
| **HTTP Request + JSON** | 같은 DC 내 서비스, Keep-Alive, 작은 JSON | **~0.5–5+ ms** | **~5,000–50,000×** |
| **Disk — HDD** | Random read + seek | **~8–10 ms** | **~80,000–100,000×** |

**대략적인 순서**

`Local RAM → NVMe SSD → SQL / Redis → HTTP + JSON → HDD`
