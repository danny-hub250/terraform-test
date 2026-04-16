# Azure Terraform 배포 정보 템플릿

## 1. Azure 인증 정보

| 항목 | 값 | 비고 |
|------|-----|------|
| Subscription ID | | `az account show --query id` |
| Tenant ID | | `az account show --query tenantId` |
| 인증 방식 | [ ] Service Principal / [ ] Managed Identity / [ ] Azure CLI | |
| Client ID | | Service Principal 사용 시 |
| Client Secret | | Service Principal 사용 시 (비밀값 — 코드에 하드코딩 금지) |

---

## 2. 배포 환경 기본 설정

| 항목 | 값 | 예시 |
|------|-----|------|
| 리전 (Location) | | `koreacentral`, `eastus`, `japaneast` |
| 환경 구분 | [ ] dev / [ ] staging / [ ] prod | |
| 멀티 환경 방식 | [ ] tfvars 분리 / [ ] Workspace | |
| 리소스 그룹 이름 | | `rg-{project}-{env}` |
| 네이밍 컨벤션 | | `{type}-{project}-{env}-{region}` |

---

## 3. 배포할 리소스 목록

아래에서 필요한 항목에 체크하고 세부 정보를 기입하세요.

### 3-1. 컴퓨팅

#### [ ] Virtual Machine
| 항목 | 값 |
|------|-----|
| VM 이름 | |
| OS 종류 | [ ] Windows / [ ] Linux |
| OS 이미지 | 예: `Ubuntu 22.04 LTS` |
| VM 크기 (SKU) | 예: `Standard_B2s` |
| 관리자 계정 | |
| 인증 방식 | [ ] 패스워드 / [ ] SSH 키 |
| OS 디스크 크기 (GB) | |
| 데이터 디스크 | [ ] 필요 / [ ] 불필요 — 크기: |
| 퍼블릭 IP | [ ] 필요 / [ ] 불필요 |
| 가용성 구성 | [ ] Availability Set / [ ] Availability Zone / [ ] 없음 |

#### [ ] App Service (Web App)
| 항목 | 값 |
|------|-----|
| App 이름 | |
| 런타임 | 예: `Node 20`, `Python 3.11`, `.NET 8`, `Java 17` |
| App Service Plan SKU | 예: `B1`, `P1v3`, `S1` |
| OS | [ ] Linux / [ ] Windows |
| 슬롯 (Slot) | [ ] 필요 (staging) / [ ] 불필요 |

#### [ ] Function App
| 항목 | 값 |
|------|-----|
| Function 이름 | |
| 런타임 | 예: `python`, `node`, `dotnet` |
| 호스팅 플랜 | [ ] Consumption / [ ] Premium / [ ] Dedicated |
| 트리거 종류 | 예: HTTP, Timer, Blob, Service Bus |

#### [ ] AKS (Azure Kubernetes Service)
| 항목 | 값 |
|------|-----|
| 클러스터 이름 | |
| Kubernetes 버전 | |
| 노드 풀 VM 크기 | 예: `Standard_D2s_v3` |
| 노드 수 (min/max) | |
| 네트워크 플러그인 | [ ] kubenet / [ ] Azure CNI |
| RBAC | [ ] 활성화 / [ ] 비활성화 |

---

### 3-2. 스토리지

#### [ ] Storage Account
| 항목 | 값 |
|------|-----|
| 스토리지 이름 | |
| 계층 (tier) | [ ] Standard / [ ] Premium |
| 복제 유형 | [ ] LRS / [ ] GRS / [ ] ZRS |
| 사용 용도 | [ ] Blob / [ ] File Share / [ ] Table / [ ] Queue |
| 퍼블릭 접근 | [ ] 허용 / [ ] 차단 |
| 수명 주기 정책 | [ ] 필요 / [ ] 불필요 |

---

### 3-3. 데이터베이스

#### [ ] Azure SQL Database
| 항목 | 값 |
|------|-----|
| 서버 이름 | |
| DB 이름 | |
| 관리자 계정 | |
| SKU | 예: `S0`, `GP_Gen5_2` |
| 서비스 티어 | [ ] DTU / [ ] vCore |
| 백업 보존 기간 (일) | |
| Geo 복제 | [ ] 필요 / [ ] 불필요 |

#### [ ] Azure Database for PostgreSQL / MySQL
| 항목 | 값 |
|------|-----|
| 서버 이름 | |
| DB 종류 | [ ] PostgreSQL / [ ] MySQL |
| 버전 | |
| SKU | 예: `GP_Standard_D2ds_v4` |
| 스토리지 크기 (GB) | |
| 고가용성 | [ ] Zone Redundant / [ ] Same Zone / [ ] 없음 |

#### [ ] Cosmos DB
| 항목 | 값 |
|------|-----|
| 계정 이름 | |
| API 종류 | [ ] NoSQL / [ ] MongoDB / [ ] Cassandra / [ ] Gremlin / [ ] Table |
| 일관성 수준 | 예: `Session`, `Strong` |
| 멀티 리전 쓰기 | [ ] 필요 / [ ] 불필요 |

---

### 3-4. 네트워킹

#### [ ] Virtual Network (VNet)
| 항목 | 값 |
|------|-----|
| VNet 이름 | |
| 주소 공간 (CIDR) | 예: `10.0.0.0/16` |
| 서브넷 목록 | |
| &nbsp;&nbsp;- 서브넷 이름 | 예: `snet-app`, `snet-db`, `snet-mgmt` |
| &nbsp;&nbsp;- 서브넷 CIDR | 예: `10.0.1.0/24` |
| VNet Peering | [ ] 필요 / [ ] 불필요 — 대상 VNet: |

#### [ ] Network Security Group (NSG)
| 항목 | 값 |
|------|-----|
| 인바운드 허용 포트 | 예: `80, 443, 22` |
| 소스 IP 제한 | 예: 특정 IP 대역만 허용 |
| 아웃바운드 제한 | [ ] 필요 / [ ] 불필요 |

#### [ ] Application Gateway / Load Balancer
| 항목 | 값 |
|------|-----|
| 종류 | [ ] Application Gateway / [ ] Load Balancer / [ ] Front Door |
| SKU | 예: `Standard_v2`, `WAF_v2` |
| WAF 활성화 | [ ] 필요 / [ ] 불필요 |
| 백엔드 풀 | |
| SSL 인증서 | [ ] Key Vault 참조 / [ ] 직접 업로드 |

#### [ ] Private Endpoint
| 항목 | 값 |
|------|-----|
| 대상 리소스 | 예: SQL, Storage, Key Vault |
| 연결 서브넷 | |
| Private DNS Zone | [ ] 자동 생성 / [ ] 기존 사용 |

---

### 3-5. 보안 및 ID

#### [ ] Key Vault
| 항목 | 값 |
|------|-----|
| Key Vault 이름 | |
| SKU | [ ] Standard / [ ] Premium (HSM) |
| 접근 모델 | [ ] Access Policy / [ ] RBAC |
| Soft Delete 보존 기간 | 예: `90일` |
| Purge Protection | [ ] 활성화 / [ ] 비활성화 |
| 저장할 비밀값 목록 | 예: DB 패스워드, API 키 |

#### [ ] Managed Identity
| 항목 | 값 |
|------|-----|
| 종류 | [ ] System-Assigned / [ ] User-Assigned |
| 할당 리소스 | |
| 필요한 RBAC 역할 | 예: `Storage Blob Data Reader`, `Key Vault Secrets User` |

---

### 3-6. 모니터링

#### [ ] Log Analytics Workspace
| 항목 | 값 |
|------|-----|
| Workspace 이름 | |
| SKU | 예: `PerGB2018` |
| 데이터 보존 기간 (일) | 예: `30` |

#### [ ] Application Insights
| 항목 | 값 |
|------|-----|
| 연결 대상 앱 | |
| 종류 | [ ] Web / [ ] Other |
| Log Analytics 연동 | [ ] 필요 / [ ] 불필요 |

---

## 4. Terraform 상태(State) 관리

| 항목 | 값 |
|------|-----|
| 백엔드 종류 | [ ] Azure Blob Storage / [ ] 로컬 |
| Storage Account 이름 | |
| 컨테이너 이름 | |
| State 파일 이름 | 예: `{env}.terraform.tfstate` |
| 상태 잠금 | [ ] 사용 (기본값) / [ ] 미사용 |

---

## 5. 태그 (Tags)

공통 태그로 모든 리소스에 적용됩니다.

| 태그 키 | 값 |
|---------|-----|
| environment | 예: `dev`, `prod` |
| project | |
| owner | |
| cost-center | |
| managed-by | `terraform` |
| 추가 태그 | |

---

## 6. 기타 요구사항

| 항목 | 내용 |
|------|------|
| CI/CD 파이프라인 연동 | [ ] GitHub Actions / [ ] Azure DevOps / [ ] 없음 |
| 모듈화 구조 | [ ] 단일 루트 / [ ] 모듈 분리 |
| 변수 관리 방식 | [ ] tfvars 파일 / [ ] 환경 변수 / [ ] Key Vault |
| Terraform 버전 | 예: `>= 1.7.0` |
| AzureRM Provider 버전 | 예: `~> 3.0` |
| 특별 컴플라이언스 요건 | 예: ISO 27001, 금융권 보안 요건 |
| 기타 요구사항 | |
