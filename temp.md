
아래 내용은 보고/공유 문서에 그대로 넣을 수 있는 형태로, AlertCategory 전체 항목을 원문 enum 명칭/주석을 유지하여 나열하고, 각 항목의 의미를 한국어로 종합 정리한 것입니다. (현재 대화에서 제공된 항목 기준)


---

AlertCategory (Network Security Alerts) – 전체 목록 및 의미

AlertCategory는 네트워크 보안(Network Security) 알림을 카테고리로 분류하기 위한 enum이며, 모뎀이 감지 가능한 보안 이벤트를 표준화된 유형 코드로 프레임워크에 전달하기 위해 사용된다.
모뎀은 getSupportedNetworkAlertCategories()를 통해 지원 가능한 AlertCategory 목록을 반환하며, 프레임워크는 이를 기반으로 기능 활성화/정책 적용을 결정할 수 있다.

1) Enum 목록 (원문 유지)

// Default, unspecified.
UNSPECIFIED = 0,

// Forced downgrade, general.
DOWNGRADE = 1,

// Forced downgrade from 3G to 2G.
DOWNGRADE_2G = 2,

// forced downgrade from 4G to 3G.
DOWNGRADE_3G = 3,

// forced downgrade from 5G to 4G.
DOWNGRADE_4G = 4,

// Attempt to lock UE to a cell.
IMPRISONMENT = 5,

// Network Denial of Service.
DOS_NETWORK = 6,

// Suspiciously attractive cell.
ATTRACTIVE_CELL = 7,

// RF Jamming detected
JAMMING = 8,

// Suspicious location tracking attempts
LOCATION_TRACKING = 9,

// Network element passed authentication
AUTH_PASSED = 10,

// Unauthenticated SMS.
UNAUTH_SMS = 11,

// Unauthenticated emergency message
UNAUTH_EMERGENCY_MSG = 12,


---

2) 항목별 의미(한국어 설명)

UNSPECIFIED (0)

카테고리가 명확하지 않거나 기본값으로 처리되는 경우.

분류 불가/미정 상태를 표현하는 fallback 용도.


DOWNGRADE (1)

강제 RAT downgrade가 감지된 일반 카테고리.

구체 RAT 전환 유형이 특정되지 않았거나, 포괄적인 downgrade 이상 징후에 사용.


DOWNGRADE_2G (2)

3G → 2G 강제 다운그레이드.

2G는 보안 수준이 상대적으로 취약할 수 있어(환경/정책에 따라) 보안 경고로 별도 구분 가치가 큼.


DOWNGRADE_3G (3)

4G(LTE) → 3G 강제 다운그레이드.

보안/품질 측면에서 비정상적 fallback, 혹은 특정 공격 시나리오(rogue cell 유도 등)와 연계 가능.


DOWNGRADE_4G (4)

5G → 4G(LTE) 강제 다운그레이드.

5G SA/NSA 운용 환경에서 비정상적인 fallback 또는 정책 위반 상황을 감지하기 위한 카테고리.


IMPRISONMENT (5)

UE를 특정 셀에 고정(lock)시키려는 시도 감지.

이동성 제어(핸드오버/리셀렉션 제한) 이상 징후로, fake/rogue cell 시나리오에서 나타날 수 있는 패턴과 연관 가능.


DOS_NETWORK (6)

Network Denial of Service(DoS) 관련 이상 징후.

과도한 reject, signaling overload, 반복적인 attach/registration 실패 유도 등 서비스 가용성을 저해하는 현상/공격을 포괄.


ATTRACTIVE_CELL (7)

의심스러울 정도로 “매력적인” 셀 감지.

비정상적으로 높은 우선순위/신호 품질을 광고하여 UE를 유인하는 패턴을 의미하며, rogue/fake cell 탐지 목적에 활용 가능.


JAMMING (8)

RF jamming(전파 재밍) 감지.

물리 계층/무선 구간에서 의도적 간섭 또는 강한 외란으로 인해 통신이 저해되는 상황을 나타냄.


LOCATION_TRACKING (9)

의심스러운 위치 추적 시도 감지.

과도한 paging/LAU/TAU 유도, 특정 셀로의 반복 유도 등 UE의 위치를 추적하려는 행위 패턴을 포괄할 수 있음.


AUTH_PASSED (10)

네트워크 요소가 인증(authentication)을 통과했음을 나타내는 상태/이벤트.

경고 상황 이후 정상 상태 전환을 표시하거나, “인증 성공” 이벤트를 명시적으로 로깅하기 위한 용도로 활용 가능(정책에 따라 정보성 이벤트로 취급될 수 있음).


UNAUTH_SMS (11)

인증되지 않은 SMS 감지.

fake base station을 통한 메시지 주입, spoofing, 비정상 경로의 SMS 관련 보안 이벤트를 나타낼 수 있음.


UNAUTH_EMERGENCY_MSG (12)

인증되지 않은 긴급 메시지(Emergency message) 감지.

공공 경보/긴급 알림 위장 등 사회적 영향이 큰 메시지에 대한 보안 위협 가능성을 별도로 구분.



---

3) 종합 요약(보고용)

AlertCategory는 모뎀에서 감지 가능한 네트워크 보안 이벤트를 유형별로 표준화한 분류 체계이다.

주요 커버리지:

1. RAT downgrade 계열: DOWNGRADE, DOWNGRADE_2G, DOWNGRADE_3G, DOWNGRADE_4G


2. 셀/네트워크 기반 공격·이상 징후: IMPRISONMENT, DOS_NETWORK, ATTRACTIVE_CELL


3. 무선 구간 공격: JAMMING


4. 프라이버시 위협: LOCATION_TRACKING


5. 인증/메시지 무결성 관련: AUTH_PASSED, UNAUTH_SMS, UNAUTH_EMERGENCY_MSG


6. 기타/미분류: UNSPECIFIED



프레임워크는 getSupportedNetworkAlertCategories() 응답을 통해 모뎀이 지원하는 카테고리 범위를 확인하고, 카테고리별 정책(표시/로그/조치)을 적용할 수 있다.



---

원하시면, 이 내용을 한 장짜리(One-pager) 보고 포맷으로 더 줄여서:

배경 / 목적 / API 요약 / enum 표(카테고리-의미-위협유형) / 기대효과
형태로 재구성해 드리겠습니다.
