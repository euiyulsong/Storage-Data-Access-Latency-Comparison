| 방식 | 대표적인 경로 | 현실적인 Latency 범위 | 주요 비용 | 해석 |
|---|---|---:|---|---|
| **Local RAM** | App → DRAM | **~0.0001 ms 수준** | memory access | 압도적으로 빠름 |
| **Local NVMe SSD** | App → OS → NVMe | **~0.05–0.2 ms** | syscall + storage I/O | 4 KiB random read가 대략 수십~수백 µs 수준 ([Dell Technologies Info Hub][1]) |
| **Remote Redis `GET`** | App → TCP → Redis | **~0.1–1 ms** | network RTT + Redis command | Redis 자체 명령은 매우 빠르고 실제 latency는 네트워크 영향이 큼. Redis 공식 문서는 일반 1 Gbit/s 네트워크 통신을 약 200 µs 수준으로 설명 ([Redis][2]) |
| **Remote SQL `SELECT`** | App → TCP → PostgreSQL → index/buffer lookup | **~0.5–5 ms** | network RTT + DB protocol + parsing/planning + query | 단순 indexed query 기준. PostgreSQL 공식 `pgbench` 예시에서도 개별 `SELECT`가 약 **0.6 ms** 수준이지만 환경에 따라 크게 달라짐 ([PostgreSQL][3]) |
| **HTTP + JSON, cache/RAM 응답** | App → HTTP server → RAM/cache → JSON | **~0.5–5 ms** | network RTT + HTTP routing + app code + JSON encode/decode | SQL을 안 타면 **remote SQL보다 빠를 수도 있음** |
| **HTTP + JSON → SQL** | App → HTTP server → SQL server | **~1–10+ ms** | **HTTP 처리 + SQL 처리 + 추가 network hop** | 같은 SQL을 직접 호출하는 것보다 일반적으로 느림 |
| **Cross-AZ HTTP / SQL / Redis** | AZ A → AZ B | **수 ms 이상 추가 가능** | cross-AZ network | AWS는 같은 AZ를 보통 sub-ms, AZ 간은 single-digit ms RTT로 설명 ([Amazon Web Services, Inc.][4]) |

[1]: https://infohub.delltechnologies.com/en-sg/t/third-party-analysis-8/?utm_source=chatgpt.com "Third-party Analysis | Dell Technologies Info Hub"

[2]: https://redis.io/docs/latest/management/optimization/latency/?utm_source=chatgpt.com "Diagnosing latency issues | Docs"

[3]: https://www.postgresql.org/docs/19/pgbench.html?utm_source=chatgpt.com "PostgreSQL: Documentation: 19: pgbench"

[4]: https://aws.amazon.com/blogs/architecture/improving-performance-and-reducing-cost-using-availability-zone-affinity//?utm_source=chatgpt.com "Improving Performance and Reducing Cost Using Availability Zone Affinity | AWS Architecture Blog"
