**Volume 23. Cargo UAV Autonomy and Flight AI**

# Chapter 08. Flight Safety and Redundancy

## 08.01. UAV Safety Architecture FHA FMEA FTA

![](images/image1.png){width="7.268055555555556in" height="7.268055555555556in"}

UAV 안전 아키텍처(UAV Safety Architecture)는 비행 제어 소프트웨어(Flight-Control Software)가 완성된 이후 안전 기능을 추가하는 것이 아니라, 항공기 설계 단계부터 안전성을 내재화해야 한다는 원칙에서 시작한다. 따라서 아키텍처는 항공기 기능, 하드웨어(Hardware), 소프트웨어(Software), 통신 링크(Communication Link), 추진 시스템(Propulsion System), 에너지 시스템(Energy System), 페이로드 인터페이스(Payload Interface), 지상 운용(Ground Operation)을 명확한 안전 목표(Safety Objective)와 연결한다. 각 기능은 고장이 항공기, 사람, 재산 및 주변 공역에 미칠 수 있는 영향을 기준으로 평가된다.

기능 위험성 평가(FHA, Functional Hazard Assessment)는 항공기 수준(Aircraft-Level)과 시스템 수준(System-Level)의 기능을 대상으로 초기의 체계적인 분석을 제공한다. 엔지니어는 자세 안정화(Attitude Stabilization), 항법(Navigation), 추진 제어(Propulsion Control), 전력 분배(Power Distribution), 지휘 및 제어(Command and Control), 장애물 회피(Obstacle Avoidance), 페이로드 관리(Payload Management), 비상 착륙(Emergency Landing) 등의 기능을 식별한다. 각 기능에 대해 기능 상실, 의도하지 않은 작동, 잘못된 출력, 작동 지연 및 위험한 항공기 동작을 유발할 수 있는 잘못된 정보 제공을 검토한다.

기능 위험성 평가(FHA)를 통해 식별된 고장 조건(Failure Condition)은 잠재적인 결과의 심각도(Severity)에 따라 분류된다. 적용되는 보증 프레임워크(Assurance Framework)에 따라 경미한 운용 영향부터 위험(Hazardous) 또는 치명적(Catastrophic) 결과까지 여러 범주로 구분할 수 있다. 치명적 조건은 사람에게 허용할 수 없는 위험을 초래하는 제어 비행 상실(Loss of Controlled Flight)을 포함할 수 있으며, 상대적으로 낮은 수준의 고장은 즉각적인 안전 비행 위협 없이 운용자의 작업 부하를 증가시키거나 임무 수행 능력을 저하시킬 수 있다. 이러한 분류는 안전 요구사항(Safety Requirement)을 결정하는 기준이 된다.

기능 위험성 평가(FHA)는 정상 비행(Nominal Flight)에서의 고장만 검토하는 것이 아니라 다양한 운용 조건의 조합을 고려해야 한다. 높은 고도에서는 관리 가능한 항법 성능 저하(Navigation Degradation)가 구조물 주변의 저고도 운용에서는 심각한 문제가 될 수 있다. 마찬가지로 추진 성능 저하(Propulsion Degradation)는 순항(Cruise), 호버링(Hover), 이륙(Takeoff), 착륙(Landing) 단계에 따라 서로 다른 결과를 초래한다. 따라서 임무 단계(Mission Phase), 항공기 중량, 기상, 교통 밀도, 통신 범위 및 이용 가능한 비상 착륙 구역(Emergency Landing Area)은 위험 심각도와 완화 전략(Mitigation Strategy)에 영향을 미친다.

고장 형태 및 영향 분석(FMEA, Failure Mode and Effects Analysis)은 기능적 위험에서 보다 세부적인 구성요소(Component)와 하위 시스템(Subsystem)의 동작으로 분석 범위를 확장한다. 관련 항목마다 발생 가능한 고장 형태(Failure Mode), 국부적 영향(Local Effect), 전파 영향(Propagated Effect), 고장 탐지 메커니즘(Detection Mechanism), 대응 방법을 분석한다. 예를 들어 전력 변환기(Power Converter)는 개방 고장(Fail Open), 단락 고장(Fail Short), 불안정 전압 출력, 과열 또는 잘못된 상태 정보 제공 등의 형태로 고장날 수 있다. 분석을 통해 이러한 고장이 격리되는지 또는 비행 필수 장비(Flight-Critical Equipment)로 전파되는지를 판단한다.

고장 형태 및 영향 분석(FMEA)은 블록 수준 아키텍처(Block-Level Architecture)에서 쉽게 간과될 수 있는 의존성(Dependency)을 발견하는 데 특히 중요하다. 두 개의 중복 비행 컴퓨터(Redundant Flight Computer)가 독립적인 것처럼 보이더라도 동일한 전력 레일(Power Rail), 클록 소스(Clock Source), 통신 스위치(Communication Switch), 냉각 경로(Cooling Path), 센서 인터페이스(Sensor Interface)를 공유할 수 있다. 이러한 공유 자원의 고장은 두 채널을 동시에 무력화할 수 있다. 따라서 효과적인 중복성(Redundancy)을 확보하려면 프로세서, 센서, 통신 링크 또는 액추에이터(Actuator)를 단순히 복제하는 것뿐만 아니라 독립성 분석(Independence Analysis)이 필요하다.

중요한 고장 형태 및 영향 분석(FMEA) 결과는 물리적 분리(Physical Separation), 전기적 절연(Electrical Isolation), 독립 전원(Independent Power Source), 이종 센싱(Dissimilar Sensing), 감시 장치(Watchdog Mechanism), 통신 모니터링(Communication Monitoring), 고장 격리(Fault Containment), 점진적 성능 저하(Graceful Degradation)와 같은 아키텍처 제어 수단으로 변환된다. 모든 고장 이후 전체 임무 수행 능력을 유지하는 것이 반드시 목표는 아니다. 대신 시스템은 허용 가능한 안전 상태(Safe State)를 유지해야 하며, 여기에는 속도 감소, 기동 제한, 임무 종료, 자동 복귀(Return-to-Home), 제어 하강(Controlled Descent), 비상 착륙 등이 포함될 수 있다.

결함 트리 분석(FTA, Fault Tree Analysis)은 안전 문제를 반대 방향에서 접근한다. 개별 구성요소에서 시작하는 대신 바람직하지 않은 최상위 사건(Top-Level Event)을 정의하고, 이를 발생시킬 수 있는 고장 조합을 단계적으로 분해한다. 대표적인 최상위 사건에는 제어 비행 상실(Loss of Controlled Flight), 의도하지 않은 추진 시스템 정지(Unintended Propulsion Shutdown), 항법 무결성 상실(Loss of Navigation Integrity), 비제어 하강(Uncontrolled Descent), 충돌 위험(Collision Risk), 비상 복구 수행 불능(Inability to Execute Emergency Recovery) 등이 있다. 논리적 관계를 분석하면 단일 지점 고장(Single-Point Failure)과 복합 고장 경로(Combined Failure Path)를 모두 식별할 수 있다.

결함 트리 분석(FTA)은 독립적으로 위험 사건을 발생시키는 고장과 여러 고장이 동시에 발생해야 위험해지는 상황을 구분하는 데 도움을 준다. AND 관계(AND Relationship)는 최상위 사건이 발생하기 전에 여러 보호 기능이 동시에 실패해야 하는 경우를 나타내며, OR 관계(OR Relationship)는 동일한 결과를 발생시킬 수 있는 여러 대체 경로를 식별한다. 이러한 분석은 중복성 수준(Redundancy Level), 모니터링 범위(Monitoring Coverage), 고장 격리(Fault Isolation), 백업 모드(Backup Mode), 주 안전 메커니즘과 보조 안전 메커니즘 사이의 독립성을 결정하는 근거가 된다.

기능 위험성 평가(FHA), 고장 형태 및 영향 분석(FMEA), 결함 트리 분석(FTA)은 상호 보완적이고 반복적인 프로세스(Iterative Process)로 운영할 때 가장 효과적이다. FHA는 어떤 항공기 수준 고장이 중요한지와 그 심각도를 결정한다. FMEA는 하위 수준 구성요소가 그러한 조건을 어떻게 발생시키거나 기여할 수 있는지를 분석한다. FTA는 제안된 완화 대책에도 불구하고 여러 결함의 조합이 중요 최상위 사건에 도달할 수 있는지를 검증한다. 한 분석에서 발견된 결과는 다른 분석에서 고려되는 가정, 요구사항, 인터페이스 또는 아키텍처를 수정하는 데 사용된다.

견고한 UAV 아키텍처는 요구되는 가용성(Availability)과 무결성(Integrity)에 따라 안전 필수 기능(Safety-Critical Function)을 분리한다. 비행 안정화, 액추에이터 제어(Actuator Control), 필수 항법(Essential Navigation), 전력 관리(Power Management), 비상 로직(Emergency Logic)은 페이로드 처리 또는 비필수 임무 애플리케이션보다 높은 수준의 격리를 요구할 수 있다. 파티셔닝(Partitioning)은 복잡도가 높은 인지(Perception), 통신 또는 페이로드 소프트웨어의 고장이 결정론적 비행 제어(Deterministic Flight Control)와 복구 기능에 필요한 자원을 소비하거나 데이터를 손상시키는 것을 방지한다.

중복성(Redundancy)은 구성요소의 개수가 아니라 고장 영역(Failure Domain)을 중심으로 설계해야 한다. 두 개의 관성 센서(Inertial Sensor)가 동일한 커넥터, 열 환경(Thermal Environment), 동기화 소스(Synchronization Source), 소프트웨어 드라이버(Software Driver)에 의존한다면 이중화의 보호 효과는 제한적이다. 세 개의 센서는 투표 방식(Voting)을 지원할 수 있지만, 고장이 충분히 독립적이고 잘못된 측정값을 식별할 수 있을 때만 효과적이다. 센서 기술, 설치 위치, 알고리즘, 전력 경로 및 통신 경로의 다양성(Diversity)은 공통 모드 취약성(Common-Mode Vulnerability)을 감소시킬 수 있다.

비행 컴퓨터(Flight Computer)는 일반적으로 모니터링, 상호 검증(Cross-Checking), 하트비트 감시(Heartbeat Supervision), 상태 비교(State Comparison)를 결합하여 비정상 채널을 탐지한다. 중복 아키텍처에서는 제어되지 않은 전환 자체가 위험을 초래할 수 있기 때문에 제어 권한 전환(Authority Transfer)을 신중하게 정의해야 한다. 따라서 시스템에는 고장 선언(Fault Declaration), 채널 격리(Channel Isolation), 명령 중재(Command Arbitration), 동기화(Synchronization), 복구(Recovery)에 대한 결정론적 기준이 필요하다. 일시적 장애(Transient Disturbance)는 지속적인 고장과 구분되어야 하며, 동시에 고장 탐지 지연(Detection Latency)이 안전 한계를 초과해서는 안 된다.

액추에이터와 추진 시스템의 중복성 역시 동일한 관점에서 분석해야 한다. 화물 UAV(Cargo UAV)에 여러 모터가 장착되어 있다고 해서 모터 하나가 고장난 이후에도 반드시 제어 비행을 지속할 수 있는 것은 아니다. 기체 형상(Vehicle Geometry), 추력 여유(Thrust Margin), 무게중심(Center of Gravity), 공기역학적 결합(Aerodynamic Coupling), 모터 제어기 독립성(Motor-Controller Independence), 전력 분배 및 제어 할당(Control Allocation)에 따라 잔여 액추에이터가 안정성을 유지할 수 있는지가 결정된다. 따라서 안전 분석은 구성요소 중복성을 인증 운용 영역(Certified Operating Envelope) 전체에서의 실제 제어 가능성(Controllability)과 연결해야 한다.

전력 아키텍처(Power Architecture)는 서로 독립적인 시스템까지 동시에 비활성화할 수 있는 전기적 고장 때문에 또 다른 핵심 안전 영역이다. 배터리 팩(Battery Pack), 접촉기(Contactor), 배전 버스(Distribution Bus), 변환기(Converter), 배선(Wiring), 보호 장치(Protection Device)는 단락, 개방 회로, 열적 이상(Thermal Event), 저전압, 과전류 및 잘못된 상태 추정에 대해 평가되어야 한다. 분리된 버스(Segregated Bus)와 제어 가능한 교차 연결(Controlled Cross-Tie)은 고장난 분기 회로가 전체 전기 시스템을 붕괴시키는 것을 방지하면서 필수 부하(Essential Load)를 유지할 수 있도록 한다.

통신 상실(Communication Loss)은 예외적인 소프트웨어 오류가 아니라 예상 가능한 운용 조건(Expected Operational Condition)으로 취급해야 한다. 지휘 및 제어 링크(Command-and-Control Link)는 간섭, 장애물 차폐, 네트워크 혼잡, 장비 고장 또는 지상국 상실로 인해 영향을 받을 수 있다. 따라서 UAV에는 임무 상황에 따른 결정론적 링크 상실 동작(Deterministic Lost-Link Behavior)이 필요하다. 사용 가능한 항법 무결성(Navigation Integrity)과 잔여 에너지에 따라 항공기는 일정 시간 비행을 지속하거나, 위치를 유지하거나, 사전 정의된 경로를 따라가거나, 복귀하거나, 우회하거나, 제어 착륙(Controlled Landing)을 시작할 수 있다.

안전 아키텍처는 신뢰성 높은 고장 탐지 및 상태 관리(Fault Detection and Health Management)에도 의존한다. 내장 시험(Built-In Test), 범위 검사(Range Check), 시간적 일관성 검사(Temporal Consistency Check), 모델 기반 잔차(Model-Based Residual), 센서 간 비교(Cross-Sensor Comparison), 통신 무결성 모니터링(Communication Integrity Monitoring)을 통해 하위 시스템의 성능 저하 여부를 판단할 수 있다. 그러나 고장 탐지만으로는 충분하지 않으며, 아키텍처는 탐지된 각 상태를 고장 격리, 재구성(Reconfiguration), 기능 축소(Capability Reduction), 운용자 통보 및 적절한 안전 상태로의 전환과 연결해야 한다.

공통 원인 고장(Common-Cause Failure)과 공통 모드 고장(Common-Mode Failure)은 중복성에 대한 기본 가정을 무효화할 수 있기 때문에 별도의 분석이 필요하다. 환경 노출, 전자기 간섭(Electromagnetic Interference), 소프트웨어 결함, 잘못된 정비, 공유 구성 데이터(Shared Configuration Data), 제조 결함, 결빙(Icing), 과열 또는 공통 타이밍 오류(Common Timing Error)는 여러 중복 채널에 동시에 영향을 줄 수 있다. 따라서 독립성에 대한 주장은 식별된 위험에 적합한 물리적, 전기적, 기능적 및 소프트웨어적 분리(Software Separation)를 통해 뒷받침되어야 한다.

기능 위험성 평가(FHA), 고장 형태 및 영향 분석(FMEA), 결함 트리 분석(FTA)에서 도출된 안전 요구사항은 시스템 요구사항(System Requirement), 하위 시스템 사양(Subsystem Specification), 소프트웨어 요구사항(Software Requirement), 하드웨어 설계(Hardware Design), 검증 절차(Verification Procedure), 시험 증거(Test Evidence)까지 추적 가능해야 한다. 각각의 완화 대책에는 명확하게 식별 가능한 구현 방법과 검증 방법이 존재해야 한다. 이러한 추적성(Traceability)은 안전 제어 수단이 단순한 설계 의도로 남는 것을 방지하며, 아키텍처 변경이 새로운 위험을 발생시키거나 기존에 승인된 가정을 무효화하는지를 판단할 수 있도록 한다.

검증(Verification)은 분석뿐만 아니라 시뮬레이션(Simulation), 소프트웨어 인 더 루프 시험(SIL, Software-in-the-Loop Testing), 하드웨어 인 더 루프 시험(HIL, Hardware-in-the-Loop Testing), 고장 주입(Fault Injection), 지상 시험(Ground Testing), 단계적으로 제약을 완화하는 비행 시험(Flight Testing)을 결합하여 수행한다. 특히 많은 중복 메커니즘이 정상 운용 중에는 활성화되지 않기 때문에 고장 주입 시험이 중요하다. 엔지니어는 센서 데이터 중단, 통신 지연, 프로세서 리셋, 손상된 측정값, 액추에이터 성능 저하 또는 전력 장애를 의도적으로 발생시켜 탐지, 격리, 재구성 및 복구가 요구된 시간 한계 내에서 수행되는지를 확인한다.

최종적으로 안전 사례(Safety Case)는 위험(Hazard), 요구사항, 아키텍처, 구현, 검증 증거(Verification Evidence), 운용 제약(Operational Constraint)을 하나의 일관된 논리 체계로 연결한다. 기능 위험성 평가(FHA)는 제어해야 하는 결과를 식별하고, 고장 형태 및 영향 분석(FMEA)은 세부적인 고장 전파(Failure Propagation)를 분석하며, 결함 트리 분석(FTA)은 허용할 수 없는 사건으로 이어지는 고장 조합을 검토한다. 이러한 분석을 통합함으로써 UAV는 발생 가능한 고장을 허용하면서도 제어 가능한 동작을 유지하고 적절한 안전 운용 상태(Safe Operating State)로 예측 가능하게 전환할 수 있도록 체계적으로 설계될 수 있다.

## 08.02. Dual Triple Redundant FCC Design and Voting [w/Code]

![](images/image2.png){width="7.268055555555556in" height="7.268055555555556in"}

중복 비행 제어 컴퓨터(FCC, Flight Control Computer) 아키텍처는 단일 컴퓨팅 고장으로 인해 항공기 제어를 상실하는 상황을 방지하도록 설계된다. UAV에서 FCC는 안정화(Stabilization), 유도(Guidance), 항법 처리(Navigation Processing), 액추에이터 명령(Actuator Command), 모드 관리(Mode Management), 안전 로직(Safety Logic)을 수행한다. 중복성(Redundancy)은 이러한 기능을 독립적인 컴퓨팅 채널(Computing Channel)에 분산하여 고장을 탐지하고 격리하며, 예측 가능한 비행 동작을 유지하면서 고장을 수용할 수 있도록 한다.

이중 중복 FCC(Dual-Redundant FCC) 아키텍처는 두 개의 컴퓨팅 채널이 동일하거나 동등한 센서 정보를 입력받아 독립적으로 비행 제어 출력을 계산하도록 구성한다. 각 채널은 일반적으로 동일하거나 기능적으로 동등한 제어 알고리즘(Control Algorithm)을 실행하면서 상대 채널을 지속적으로 감시한다. 요구되는 가용성(Availability), 계산 복잡도(Computational Complexity), 고장 격리 전략(Fault Containment Strategy), 항공기 안전 목표(Safety Objective)에 따라 이 아키텍처는 능동-대기(Active-Standby), 능동-능동(Active-Active), 명령-감시(Command-Monitor) 방식으로 운용될 수 있다.

능동-대기(Active-Standby) 구성에서는 주 FCC(Primary FCC)가 항공기를 제어하고, 보조 FCC(Secondary FCC)는 동기화된 상태를 유지하면서 주 FCC의 동작을 감시한다. 주 채널에 고장이 선언되면 제어 권한(Command Authority)은 대기 채널로 전환된다. 이러한 구조는 개념적으로 단순하지만 안전한 전환을 위해서는 정확한 상태 동기화(State Synchronization), 결정론적인 고장 탐지(Deterministic Fault Detection), 신속한 전환이 필요하다. 이를 통해 전환 과정에서 액추에이터 명령에 허용할 수 없는 불연속이 발생하지 않도록 해야 한다.

능동-능동 이중 구조(Active-Active Dual Architecture)에서는 두 FCC가 동시에 제어 명령을 계산할 수 있다. 각 채널의 출력은 상태 변수(State Variable), 센서 추정값(Sensor Estimate), 제어 모드(Control Mode), 액추에이터 명령을 기준으로 지속적으로 비교된다. 두 채널의 출력이 일치하면 양쪽 채널이 정상적으로 동작하고 있다는 신뢰도가 높아지며, 과도한 불일치는 고장을 나타낸다. 그러나 두 채널이 서로 다른 결과를 생성할 경우 어느 채널이 올바른지를 두 채널 자체만으로 결정할 수 없다는 근본적인 한계가 있다.

따라서 이중 구조에는 모호한 불일치를 해결할 수 있는 중재 메커니즘(Arbitration Mechanism)이 필요하다. 독립 모니터(Independent Monitor), 이종 프로세서(Dissimilar Processor), 외부 센서 검증(External Sensor Validation), 액추에이터 피드백(Actuator Feedback) 또는 별도의 안전 제어기(Safety Controller)는 고장 채널을 식별하는 데 필요한 추가 증거를 제공할 수 있다. 이러한 증거가 없다면 이중 채널 시스템은 불일치를 탐지할 수는 있지만, 특히 두 출력이 각각 타당해 보이는 경우 어느 비행 제어 컴퓨터가 고장났는지를 안정적으로 격리하지 못할 수 있다.

삼중 중복 FCC(Triple-Redundant FCC) 아키텍처는 세 번째 독립 채널을 추가하여 다수결 투표(Majority Voting)를 가능하게 한다. 세 채널이 모두 정상적으로 동작하면 계산된 상태와 명령은 정의된 일치 범위(Agreement Threshold) 안에 있어야 한다. 한 채널이 다른 두 채널과 비교하여 크게 벗어나고 나머지 두 채널이 서로 일치한다면, 다수의 두 채널이 이상 채널을 식별할 수 있다. 이후 고장 또는 의심 채널을 격리하고 나머지 두 채널을 사용하여 항공기를 계속 제어할 수 있다.

삼중 모듈 중복성(TMR, Triple Modular Redundancy)은 일반적으로 2-out-of-3 투표 원칙과 연관된다. 투표기(Voter)는 세 채널에서 생성된 동등한 출력을 비교하고 다수 채널이 지지하는 값을 선택한다. 모드 선택(Mode Selection)이나 유효성 플래그(Validity Flag)와 같은 이산 상태(Discrete State)는 직접적인 논리 다수결 투표를 적용할 수 있다. 반면 자세 추정(Attitude Estimate), 추력 명령(Thrust Command), 액추에이터 위치(Actuator Position)와 같은 연속값(Continuous Value)은 중앙값 선택(Median Selection), 제한된 평균(Bounded Averaging), 허용오차 기반 비교(Tolerance-Based Comparison) 등이 더 적절할 수 있다.

연속값 투표(Continuous-Value Voting)는 정상적인 수치 차이를 고려하여 신중하게 임계값을 설정해야 한다. 센서 잡음(Sensor Noise), 비동기 샘플링(Asynchronous Sampling), 부동소수점 연산 차이(Floating-Point Behavior), 스케줄링 지터(Scheduling Jitter) 때문에 정상 채널에서도 수치 출력이 완전히 동일하지 않을 수 있다. 따라서 일치 범위는 정상적인 계산 편차와 실제 고장을 구분할 수 있어야 한다. 임계값이 지나치게 좁으면 불필요한 고장 선언이 증가하고, 반대로 지나치게 넓으면 위험한 잘못된 명령이 허용될 수 있다.

투표는 여러 아키텍처 수준에서 수행할 수 있다. 원시 센서 측정값(Raw Sensor Measurement)을 제어 알고리즘에 입력하기 전에 투표할 수도 있고, 항법 처리 이후 추정 상태(Estimated State)를 비교할 수도 있으며, 최종 액추에이터 명령을 전송하기 전에 투표할 수도 있다. 각각의 방법은 서로 다른 고장 유형을 탐지한다. 견고한 아키텍처는 일반적으로 여러 계층을 결합하여 센서 고장, 추정 고장, 처리 고장 및 명령 생성 고장을 하나의 일반적인 불일치 신호로 처리하지 않고 구분할 수 있도록 한다.

FCC 중복성은 입력의 독립성(Input Independence)에 크게 의존한다. 세 대의 컴퓨터가 동일한 관성 측정 장치(IMU, Inertial Measurement Unit)로부터 데이터를 입력받는다면 공유 센서의 고장에 대해서는 실질적인 보호가 제공되지 않는다. 따라서 안전 필수 아키텍처(Safety-Critical Architecture)는 여러 IMU, 대기 데이터 센서(Air-Data Source), GNSS 수신기, 고도 센서 또는 추진 피드백 채널을 사용할 수 있다. 센서와 FCC 사이의 데이터 연결은 공통 의존성을 최소화하면서도 일관성 검사를 위한 충분한 상호 정보를 유지하도록 설계해야 한다.

시간 동기화(Time Synchronization) 역시 중요하다. 서로 다른 물리적 시점에서 생성된 투표 데이터는 모든 채널이 정상적으로 동작하더라도 서로 일치하지 않는 것처럼 보일 수 있기 때문이다. FCC는 동기화된 클록(Synchronized Clock), 하드웨어 타임스탬프(Hardware Timestamp), 결정론적 통신 스케줄(Deterministic Communication Schedule), 제한된 실행 주기(Bounded Execution Period)를 사용하여 시간적 정렬을 유지할 수 있다. 비교 로직은 단순히 수치값만 비교하는 것이 아니라 측정 데이터의 연령(Measurement Age), 처리 지연(Processing Latency), 전송 지연(Transport Delay), 명령 유효 시간(Command Validity Interval)을 고려해야 한다.

채널 간 통신(Cross-Channel Communication)은 FCC가 상태 정보, 추정 항공기 상태, 모드 정보, 실행 카운터(Execution Counter), 고장 선언을 서로 교환할 수 있도록 한다. 그러나 이러한 통신 자체도 잠재적인 고장 원인으로 취급해야 한다. 고장난 네트워크 스위치나 손상된 공유 버스(Shared Bus)가 전체 중복 시스템을 동시에 비활성화해서는 안 된다. 독립 통신 경로, 버스 보호기(Bus Guardian), 메시지 무결성 검사(Message Integrity Check), 시퀀스 카운터(Sequence Counter), 타임아웃(Timeout), 송신원 인증(Source Authentication)은 고장 격리 능력을 향상시킬 수 있다.

투표기(Voter)는 독립적인 FCC 채널이 확보한 안전성을 무력화할 수 있기 때문에 그 자체가 안전 필수 구성요소(Safety-Critical Element)이다. 중앙 집중형 투표(Centralized Voting)는 아키텍처를 단순화하지만, 투표기가 단일 고장점(Single Point of Failure)이 될 수 있으므로 높은 수준의 보증(Assurance)을 확보하거나 투표기 자체를 중복화해야 한다. 분산 투표(Distributed Voting)는 각 채널이 독립적으로 비교를 수행할 수 있도록 하지만, 고장 판정의 일관성과 명령 권한(Command Authority) 관리라는 문제가 발생한다. 선택된 아키텍처는 서로 충돌하는 채널이 동시에 액추에이터에 명령을 내리지 못하도록 해야 한다.

명령 권한 관리(Command Authority Management)는 어떤 FCC 출력이 실제 항공기에 영향을 줄 수 있는지를 정의한다. 중재 로직(Arbitration Logic)은 정상 및 성능 저하 상태에서 결정론적인 우선순위(Priority), 제어 권한(Ownership), 전환 규칙(Transition Rule)을 설정해야 한다. FCC가 전환되는 동안 적분기 상태(Integrator State), 항법 추정값, 제어 기준(Control Reference), 액추에이터 명령 이력은 제어 과도 현상(Control Transient)을 방지할 수 있을 정도로 동기화되어 있어야 한다. 큰 관성을 갖는 대형 화물 UAV에서는 특히 무충격 전환(Bumpless Transfer)이 중요하다.

고장 탐지(Fault Detection)는 투표 불일치와 FCC 내부 상태 감시를 결합한다. 워치독 타이머(Watchdog Timer), 메모리 보호(Memory Protection), 프로세서 록스텝 검사(Processor Lockstep Check), 실행 시간 감시(Execution-Time Monitoring), 전원 감시(Power Supervision), 온도 감시(Temperature Monitoring), 내장 시험(Built-In Test), 소프트웨어 건전성 검사(Software Sanity Check)를 사용하여 명령 불일치가 발생하기 전에 고장을 탐지할 수 있다. 내부 진단과 외부 채널 간 비교를 결합하면 고장 격리 능력이 향상되고 특정 하나의 탐지 메커니즘에 대한 의존성이 감소한다.

중복성은 단순히 작동을 중단하는 고장이 아니라 비잔틴과 유사한 고장(Byzantine-Like Failure)도 고려해야 한다. 이러한 고장에서는 FCC가 계속 작동하지만 일관되지 않거나 잘못된 정보를 생성할 수 있다. 하나의 채널이 일부 상태 변수에 잘못된 값을 삽입하거나, 서로 다른 상대 채널에 서로 다른 데이터를 전송하거나, 간헐적으로 타이밍 제약을 위반할 수도 있다. 메시지 일관성 검사(Message Consistency Check), 독립적인 관측(Independent Observation), 제한된 명령 검증(Bounded Command Validation), 액추에이터 수준 보호(Actuator-Level Protection)는 이러한 고장이 통제되지 않은 권한을 획득하는 것을 방지하는 데 도움을 준다.

공통 모드 고장(Common-Mode Failure)은 중복 FCC 설계의 가장 중요한 한계 중 하나이다. 동일한 소프트웨어를 실행하는 동일한 컴퓨터 세 대가 동일한 소프트웨어 결함, 잘못된 구성, 환경 조건 또는 손상된 공유 입력으로 인해 동시에 고장날 수 있다. 물리적 분리(Physical Separation), 독립 전원 공급(Independent Power Supply), 별도의 통신 인터페이스, 이종 소프트웨어 구현(Dissimilar Software Implementation), 다양한 프로세서(Diverse Processor), 독립 모니터링(Independent Monitoring)을 적용하면 하나의 원인이 모든 채널을 동시에 무력화할 가능성을 줄일 수 있다.

이종 중복성(Dissimilar Redundancy)은 높은 결과 위험(High-Consequence)을 갖는 기능에서 특히 유용할 수 있다. 주 FCC는 정교한 비행 제어 및 항법 소프트웨어 스택을 실행하는 반면, 독립적인 안전 제어기(Independent Safety Controller)는 자세, 고도, 속도, 지오펜싱(Geofencing), 제어 명령 한계를 감시하는 단순하고 검증된 로직을 사용할 수 있다. 안전 제어기는 전체 임무 수행 능력을 가질 필요가 없으며, 그 목적은 위험한 명령을 거부하거나, 항공기를 안정화하거나, 주 아키텍처의 신뢰성이 떨어질 경우 복구 동작을 시작하거나 제어 착륙을 수행하는 것이다.

중복성이 감소하는 상황에서는 성능 저하 모드(Degraded Mode) 관리가 명확하게 정의되어야 한다. 삼중 시스템은 세 개의 정상 채널에서 한 개의 FCC를 격리한 이후 두 채널 운용 상태로 전환될 수 있다. 이후 남은 두 채널 사이에 불일치가 발생하면 다수결 투표가 더 이상 가능하지 않기 때문에 훨씬 어려운 판단이 필요하다. 항공기는 이때 기동을 제한하거나, 임무를 종료하거나, 복구 위치로 복귀하거나, 비행 시간을 줄이거나, 위험 수준과 독립 모니터링의 가용성에 따라 착륙을 시작할 수 있다.

중복성 관리는 일시적인 장애로 인해 불필요하게 채널이 제거되는 것을 방지해야 한다. 고장 선언은 지속성 카운터(Persistence Counter), 여러 번의 연속적인 불일치, 변화율 검사(Rate-of-Change Check), 독립 상태 감시기의 확인 등을 요구할 수 있다. 동시에 위험한 고장은 잘못된 명령이 항공기 동역학에 영향을 미치기 전에 충분히 빠르게 격리되어야 한다. 따라서 고장 확인에 필요한 지속 시간과 고장 대응 지연 시간은 고장이 제어 불능 상태로 발전하기 전에 이용 가능한 물리적 시간과 균형을 이루어야 한다.

액추에이터 인터페이스(Actuator Interface)는 중복 계산 구조에서 확보한 보호 기능을 유지해야 한다. 여러 FCC의 출력이 최종적으로 하나의 보호되지 않은 액추에이터 제어기(Actuator Controller)로 연결되고 단일 통신 경로를 사용하는 경우 상위 수준에서 확보한 중복성의 상당 부분이 사라질 수 있다. 스마트 액추에이터(Smart Actuator) 또는 중복 액추에이터 제어 전자장치(Redundant Actuator-Control Electronics)는 명령 검증, 송신원 중재, 위치 피드백 감시, 로컬 한계 적용(Local Limit Enforcement)을 수행할 수 있으며, 이를 통해 컴퓨팅 고장과 실제 물리적 움직임 사이에 추가적인 격리 경계를 형성할 수 있다.

이중 및 삼중 FCC 아키텍처의 검증은 정상 기능 시험만으로 충분하지 않다. 소프트웨어 인 더 루프(SIL, Software-in-the-Loop)와 하드웨어 인 더 루프(HIL, Hardware-in-the-Loop) 환경에서는 프로세서 리셋, 출력 고정(Frozen Output), 편향된 추정값(Biased Estimate), 지연된 메시지, 통신 분할(Communication Partition), 손상된 명령, 클록 드리프트(Clock Drift), 센서 불일치, 전원 중단, 간헐적 고장 등을 주입해야 한다. 시험은 고장 탐지뿐 아니라 정확한 격리, 투표, 제어 권한 전환, 성능 저하 운용, 최종 복구 동작까지 검증해야 한다.

지연 시간(Latency) 측정은 검증 과정에서 필수적이다. 중복성 메커니즘 자체가 유한한 시간을 소비하기 때문이다. 센서 취득, 채널 계산, 채널 간 정보 교환, 투표, 고장 확인, 중재, 액추에이터 전송이 모두 종단 간 응답 시간(End-to-End Response Time)에 영향을 준다. 안전 분석은 전체 지연 시간이 항공기 동역학과 양립 가능한지를 입증해야 하며, 특히 불안정 플랫폼(Unstable Platform), 고속 비행, 대형 화물 항공기, 지형 또는 인프라에 가까운 운용에서는 더욱 중요하다.

성공적인 중복 FCC 설계는 단순한 컴퓨팅 복제가 아니라 독립성, 동기화된 정보, 신뢰성 있는 투표, 결정론적 중재, 고장 격리, 제어된 성능 저하를 함께 구현한다. 이중 아키텍처는 독립적인 중재 기능이 지원될 경우 높은 가용성을 제공할 수 있으며, 삼중 아키텍처는 다수결 투표를 통해 더욱 강력한 고장 격리 능력을 제공한다. 궁극적인 목표는 채널의 숫자를 최대화하는 것이 아니라, 신뢰할 수 있는 하드웨어, 소프트웨어, 통신, 센서 또는 전력 시스템에서 고장이 발생하더라도 안전한 제어를 지속하는 것이다.

## 08.03. Engine and Motor Failure Fault Tolerant Control [w/Code]

![](images/image3.png){width="7.268055555555556in" height="7.268055555555556in"}

엔진 및 모터 고장(Engine and Motor Failure)은 추진 시스템(Propulsion System)이 양력, 추력, 자세 제어 권한(Attitude Authority), 궤적 제어(Trajectory Control)를 직접 결정하기 때문에 UAV에서 가장 안전에 중요한 장애 중 하나이다. 고장 허용 제어(FTC, Fault-Tolerant Control)는 추진력의 부분적 또는 완전한 상실 이후에도 제어 가능성(Controllability)을 유지하도록 설계되며, 고장을 탐지하고 잔여 제어 권한(Remaining Control Authority)을 추정하며 명령을 재할당하고 항공기를 안전한 운용 상태(Safe Operating Condition)로 전환한다.

추진 시스템 고장(Propulsion Fault)은 완전한 모터 정지, 부분적인 추력 저하, 간헐적인 토크 손실, 과도한 로터 항력(Rotor Drag), 속도 제어 오류, 과열, 인버터 고장(Inverter Malfunction), 응답 지연 등의 형태로 나타날 수 있다. 내연기관 또는 터빈 기반 UAV에서는 연료 유량 중단(Fuel-Flow Interruption), 점화 문제, 압축기 또는 기계적 성능 저하, 조속기 고장(Governor Fault) 등이 추가로 발생할 수 있다. 각 고장은 서로 다른 동적 영향을 발생시키므로 서로 다른 탐지 및 고장 수용 전략이 필요하다.

고장의 결과는 항공기 구성(Aircraft Configuration)에 크게 의존한다. 기존 고정익 UAV(Conventional Fixed-Wing UAV)에서는 엔진 상실이 주로 전방 추력(Forward Thrust)을 제거하지만 공기역학적 조종면(Aerodynamic Control Surface)은 활공과 착륙을 위해 계속 사용할 수 있다. 반면 멀티로터(Multirotor)와 분산 전기 추진(Distributed Electric Propulsion) 항공기에서는 개별 모터가 양력과 모멘트 생성에 직접 기여한다. 따라서 하나의 추진 장치가 상실되면 수직력 부족과 심각한 롤(Roll), 피치(Pitch), 요(Yaw) 교란이 동시에 발생할 수 있다.

대형 화물 UAV(Heavy-Lift Cargo UAV)는 높은 총중량, 큰 관성, 페이로드 위치 변화(Payload Displacement), 제한된 잉여 추력(Excess Thrust)으로 인해 추진 고장 이후 사용할 수 있는 여유가 감소하므로 추가적인 문제가 발생한다. 정상 조건에서 충분히 호버링할 수 있는 항공기라도 최대 페이로드 상태에서 모터 하나를 상실하면 고도를 유지하지 못할 수 있다. 따라서 고장 허용 설계는 운용 중량, 무게중심(Center of Gravity), 고도, 온도, 배터리 상태(Battery State) 전체 영역에서 제어 가능성을 평가해야 한다.

고장 탐지(Fault Detection)는 명령된 추진 시스템 동작과 실제 관측된 응답을 비교하는 것에서 시작한다. 모터 회전 속도, 상전류(Phase Current), 전압, 토크 추정값(Torque Estimate), 전자식 속도 제어기(ESC, Electronic Speed Controller) 상태, 온도, 진동, 액추에이터 피드백(Actuator Feedback)은 추진 시스템 상태를 직접적으로 판단할 수 있는 정보를 제공한다. 관성 센서(Inertial Sensor)의 항공기 수준 측정값도 명령된 힘과 모멘트에 부합하지 않는 예상치 못한 각가속도 또는 수직 가속도가 발생할 경우 추가적인 고장 증거를 제공한다.

모델 기반 탐지(Model-Based Detection)는 각 추진 장치에서 예상되는 추력 또는 토크를 계산하고 이를 실제 측정된 항공기 운동과 비교할 수 있다. 통계적 또는 물리적으로 정의된 한계를 초과하는 잔차(Residual)는 잠재적인 성능 저하를 나타낸다. 그러나 공기역학적 외란, 돌풍, 페이로드 움직임, 센서 잡음, 모델링 오차도 유사한 잔차를 발생시킬 수 있으므로 하나의 비정상 측정값만으로 모터 고장을 선언하기보다는 여러 정보원을 결합하여 고장을 진단해야 한다.

추진 시스템 고장은 빠른 자세 발산(Attitude Divergence)을 유발할 수 있으므로 탐지 속도(Detection Speed)가 매우 중요하다. 멀티로터는 모터 추력 상실 이후 과도한 롤 또는 요 속도가 발생하기까지 매우 짧은 시간만 허용될 수 있다. 따라서 탐지 아키텍처는 신속한 대응과 오경보(False Alarm) 사이에서 균형을 유지해야 한다. 지속성 로직(Persistence Logic), 적응형 임계값(Adaptive Threshold), 중복 측정값, 동적 일관성 검사(Dynamic Consistency Check)를 사용하면 차량 동역학에 적합한 응답 시간을 유지하면서 일시적 외란과 실제 고장을 구분할 수 있다.

고장이 확인되면 고장 탐지·격리·식별(FDII, Fault Detection, Isolation, and Identification)은 어떤 추진 장치가 영향을 받았는지 판단하고 상실된 추진 효과의 크기를 추정한다. 격리(Isolation)는 고장난 모터 또는 엔진을 식별하며, 식별(Identification)은 고장이 완전한 기능 상실인지, 부분적인 추력 감소인지, 응답 지연인지 또는 다른 성능 저하 상태인지를 판단한다. 이러한 정보를 통해 제어기는 하나의 일반적인 비상 대응을 적용하는 대신 고장 상태에 적합한 재구성(Reconfiguration)을 선택할 수 있다.

제어 할당(Control Allocation)은 여러 액추에이터를 사용하는 항공기의 추진 고장 허용에서 핵심적인 역할을 한다. 정상 운용에서는 비행 제어기(Flight Controller)가 요구되는 총추력과 기체 모멘트(Body Moment)를 개별 모터 명령으로 변환한다. 고장 이후에는 정상 또는 부분적으로 동작 가능한 액추에이터만을 사용하여 제어 할당 문제를 다시 구성한다. 제어기는 모터 속도, 추력, 전력, 열 및 구조적 제약조건을 준수하면서 필요한 힘과 모멘트 벡터를 최대한 재현하려고 한다.

고장 이후의 항공기는 더 이상 요구되는 모든 힘과 모멘트를 독립적으로 생성하지 못할 수 있다. 따라서 제어 할당은 안전 중요도(Safety Importance)에 따라 제어 목표의 우선순위를 결정해야 한다. 정확한 요 추종(Yaw Tracking), 위치 유지(Position Holding), 임무 궤적 유지보다 롤과 피치 안정성을 유지하는 것이 우선될 수 있다. 일부 멀티로터 구성에서는 수직 지지력을 유지하고 치명적인 자세 발산을 방지할 수 있다면 제어된 요 회전(Controlled Yaw Rotation)을 허용할 수도 있다.

과구동 UAV 구성(Overactuated UAV Configuration)은 고장을 수용할 수 있는 더 많은 가능성을 제공한다. 헥사콥터(Hexacopter), 옥토콥터(Octocopter), 분산 추진 날개(Distributed-Propulsion Wing), 중복 양력 장치(Redundant Lift Unit)를 갖는 항공기는 하나의 추진 장치를 상실한 이후에도 충분한 액추에이터 제어 권한을 유지할 수 있다. 그러나 추가적인 모터가 존재한다고 해서 자동으로 고장 허용성이 보장되는 것은 아니다. 모터의 배치, 추력 방향, 최대 출력, 회전 방향, 사용 가능한 전력에 따라 잔여 힘과 모멘트의 제어 가능 영역이 결정된다.

비행 제어기는 정상적인 기동 능력이 그대로 유지된다고 가정하는 대신 고장 이후 제어 영역(Post-Failure Control Envelope)을 추정해야 한다. 최대 상승률, 횡가속도, 요 제어 권한, 뱅크각(Bank Angle), 페이로드 운용 능력, 외란 억제 능력(Disturbance Rejection)은 모두 감소할 수 있다. 재구성된 유도 시스템(Guidance System)은 향후 명령을 이러한 축소된 운용 영역 내로 제한하여 상위 수준 자율 시스템이나 임무 계획 소프트웨어가 성능이 저하된 추진 시스템으로 수행할 수 없는 기동을 요구하지 않도록 해야 한다.

적응형 또는 재구성 가능 제어기(Adaptive or Reconfigurable Controller)는 추진 효과가 변화한 이후 제어 게인(Control Gain)과 내부 모델을 수정할 수 있다. 정상 동역학만을 기준으로 설계된 제어기는 액추에이터 제어 권한이 감소하면 불안정해지거나 지나치게 공격적으로 동작할 수 있다. 게인 스케줄링(Gain Scheduling), 모델 예측 제어(Model Predictive Control), 비선형 제어(Nonlinear Control), 증분 제어(Incremental Control), 온라인 효과 추정(Online Effectiveness Estimation) 등을 통해 추진 시스템 성능 저하 상태에서도 안정성 여유(Stability Margin)를 유지하면서 제어 응답을 조정할 수 있다.

모터 고장 이후에는 에너지 관리(Energy Management)가 특히 중요해진다. 잔여 모터가 훨씬 높은 추력으로 작동해야 할 수 있으며, 이에 따라 전류, 인버터 온도, 배터리 방전율(Battery Discharge Rate), 열적 스트레스(Thermal Stress)가 증가한다. 이러한 상태에서 장시간 비행을 지속하면 2차 고장(Secondary Failure)이 발생할 수 있다. 따라서 비상 제어기는 잔여 에너지와 열적 여유(Thermal Margin)를 지속적으로 추정하여 먼 거리의 자동 복귀(Return-to-Home)를 시도하는 것보다 즉시 착륙하는 것이 더 안전한지를 판단해야 한다.

전기 아키텍처(Electrical Architecture)는 국부적인 추진 시스템 고장이 정상 채널로 전파되는 것을 방지해야 한다. 모터 권선 단락, 인버터 고장, 손상된 전력 케이블, 열적 이상(Thermal Event)은 적절한 격리가 이루어지지 않을 경우 공유 전력 버스(Shared Power Bus)에 영향을 미칠 수 있다. 독립 보호 장치, 접촉기(Contactor), 퓨즈(Fuse), 전류 제한(Current Limiting), 분리된 배전 경로(Segregated Distribution Path), 고장 격리 영역(Fault-Containment Zone)은 고장난 분기를 차단하면서 나머지 추진 장치와 비행 필수 전자장치에 전력을 유지할 수 있도록 한다.

기계적 영향(Mechanical Consequence)도 고려해야 한다. 고장난 로터는 정지하거나, 풍차 회전(Windmilling)을 하거나, 고착되거나, 부품이 이탈하거나, 비정상적인 진동을 발생시킬 수 있다. 풍차 회전은 공기역학적 항력과 원하지 않는 토크를 발생시킬 수 있으며, 고착되거나 손상된 로터는 단순한 무추력(Zero-Thrust) 가정과 다른 차량 동역학을 유발할 수 있다. 따라서 상태 감시 시스템은 가능한 경우 전기적 정지와 기계적 고장을 구분해야 하며, 제어 모델은 그 결과 발생하는 비대칭 공기역학 효과(Asymmetric Aerodynamic Effect)를 반영해야 한다.

하이브리드 또는 엔진 구동 UAV(Hybrid or Engine-Driven UAV)에서는 추진 시스템 복구 과정에 제어 재구성뿐만 아니라 재시동 로직(Restart Logic)이 포함될 수 있다. 시스템은 재시동을 시도하기 전에 엔진 속도, 연료 압력, 점화 상태, 온도, 고도, 비행 조건을 평가할 수 있다. 반복적인 재시동 시도가 중요한 에너지를 소비하거나 추가적인 위험을 발생시키는 경우 이를 제한해야 한다. 재시동에 실패하면 유도 시스템은 명확하게 활공(Glide), 우회(Diversion), 비상 착륙 동작으로 전환해야 한다.

비상 궤적 생성(Emergency Trajectory Generation)은 고장 허용 제어와 임무 수준 안전(Mission-Level Safety)을 연결한다. 추진 능력이 저하되면 항공기는 고도, 대기속도, 바람, 지형, 장애물, 인구 노출도(Population Exposure), 잔여 추력, 에너지를 이용하여 도달 가능한 착륙 지점을 평가해야 한다. 가장 안전한 대응 경로는 정상적인 복귀 경로와 다를 수 있다. 비행 지속으로 2차 고장 위험이 증가한다면 발사 지점으로 복귀하는 것보다 가까운 비상 착륙 구역(Emergency Landing Zone)을 선택하는 것이 더 안전할 수 있다.

페이로드 상태(Payload State)는 화물 UAV의 추진 시스템 고장 복구에 직접적인 영향을 준다. 무거운 화물 또는 외부 현수 하중(Externally Suspended Load)은 추력 여유를 감소시키고 안정화를 어렵게 하는 진자 동역학(Pendulum Dynamics)을 발생시킬 수 있다. 항공기 아키텍처가 제어된 페이로드 분리(Controlled Payload Release)를 지원한다면 엄격하게 정의된 비상 조건에서 생존성을 향상시킬 수 있다. 그러나 화물 투하는 항공기의 위험을 지상의 사람이나 인프라로 이전할 수 있으므로 모든 분리 전략에는 엄격한 안전 로직이 필요하다.

고장 허용 추진 제어(Fault-Tolerant Propulsion Control)는 항법 및 인지 시스템(Navigation and Perception System)과 연계되어야 한다. 성능이 저하된 항공기는 더 큰 선회 반경, 낮은 가속도, 감소된 장애물 회피 여유 또는 더욱 직접적인 착륙 궤적을 필요로 할 수 있다. 항법 소프트웨어는 정상 상태의 명령을 계속 생성하는 대신 이러한 제한을 이해해야 한다. 이를 통해 고장 진단, 제어 할당, 유도, 인지, 비상 임무 관리(Emergency Mission Management) 사이에 폐루프 안전 관계(Closed-Loop Safety Relationship)가 형성된다.

다수의 모터가 설치되어 있더라도 공통 원인 고장(Common-Cause Failure)은 여전히 중요한 문제이다. 여러 추진 장치가 동일한 배터리, 냉각 시스템, 통신 버스, 소프트웨어 명령 경로, 제조 결함 또는 환경 조건을 공유할 수 있다. 따라서 배터리 고장, 결빙(Icing), 과열, 전자기적 교란(Electromagnetic Disturbance), 잘못된 제어 소프트웨어로 인해 여러 모터가 동시에 정지할 수 있다. 추진 시스템 중복성(Propulsion Redundancy)은 시스템 수준의 독립성(Independence)과 다양성(Diversity)을 통해 뒷받침되어야 한다.

검증(Verification)은 위험성이 높은 실제 시험을 시작하기 전에 시뮬레이션 환경에서 추진 시스템 고장을 체계적으로 주입하는 과정이 필요하다. 소프트웨어 인 더 루프(SIL, Software-in-the-Loop) 환경에서는 다양한 비행 조건에 걸쳐 수천 가지 고장 조합을 평가할 수 있으며, 하드웨어 인 더 루프(HIL, Hardware-in-the-Loop) 시험에서는 실제적인 제어기, 인버터, 센서, 통신 고장을 주입할 수 있다. 고충실도 시뮬레이션(High-Fidelity Simulation)은 액추에이터 포화, 모터 동역학, 공기역학적 비대칭, 페이로드 영향, 배터리 제한, 고장 탐지 지연을 반영해야 한다.

이후 실제 시험(Physical Testing)은 구속된 추진 시스템 시험(Restrained Propulsion Test)에서 시작하여 엄격하게 제한된 고장 시나리오를 적용하는 비행 시험으로 단계적으로 확장할 수 있다. 엔지니어는 고장 탐지 지연, 자세 변화, 고도 손실, 액추에이터 포화(Actuator Saturation), 복구 시간, 열 부하(Thermal Loading), 착륙 성능을 측정한다. 미세한 고장은 식별하기 더 어렵고 장시간 지속될 수 있기 때문에 완전한 모터 정지뿐만 아니라 부분적인 성능 저하와 간헐적 고장(Intermittent Fault)도 시험해야 한다.

안전 검증(Safety Validation)은 의도된 운용 영역(Intended Operational Envelope) 내에서 완화 조치가 효과를 발휘하기 전에 신뢰할 수 있는 단일 추진 고장(Credible Single Propulsion Failure)이 제어되지 않은 상태 전이를 발생시키지 않는다는 것을 입증해야 한다. 특정 고장 이후 지속 비행이 불가능한 항공기 구성에서는 목표가 임무 지속에서 제어된 종료(Controlled Termination)로 변경된다. 아키텍처는 어떤 고장에서 지속 비행이 가능한지, 어떤 고장에서 즉각적인 우회가 필요한지, 어떤 고장에서 비상 착륙이 요구되는지를 명확하게 정의해야 한다.

효과적인 엔진 및 모터 고장 허용 제어(Engine and Motor Fault-Tolerant Control)는 추진 시스템 상태 감시, 신속한 고장 격리, 추진 효과 추정(Effectiveness Estimation), 제약조건 기반 제어 할당(Constrained Control Allocation), 적응형 안정화(Adaptive Stabilization), 에너지 관리, 비상 유도(Emergency Guidance)를 통합한다. 핵심 목표는 추진 시스템 고장을 보이지 않게 만드는 것이 아니라, UAV가 감소된 물리적 능력을 정확하게 인식하고 그 정보를 신속하게 안정적이고 예측 가능하며 안전을 우선하는 동작으로 전환하도록 하는 것이다.

## 08.04. GNSS Spoofing Jamming Detection and Fallback [w/Code]

![](images/image4.png){width="7.268055555555556in" height="7.268055555555556in"}

위성항법시스템(GNSS, Global Navigation Satellite System)은 UAV의 위치, 속도, 시간 동기화(Timing), 경로 추종(Route Tracking), 지오펜싱(Geofencing), 자동 복귀(Return-to-Home) 기능을 위한 주요 항법 정보원이다. 그러나 수신기에 도달하는 위성 신호는 매우 약하기 때문에 간섭에 취약하다. 따라서 안전 중심 UAV는 특히 자율 비행이나 가시권 밖 비행(BVLOS, Beyond Visual Line of Sight)에서 GNSS를 절대적인 기준이 아니라 성능이 저하되거나 기만될 가능성이 있는 정보원으로 취급해야 한다.

GNSS 재밍(Jamming)은 간섭 신호가 수신기의 정상적인 위성 신호 획득 또는 추적 능력을 감소시킬 때 발생한다. 이는 갑작스러운 위성 수 감소, 반송파 대 잡음 밀도(Carrier-to-Noise Density) 저하, 측정 불확실성 증가, 반복적인 추적 재설정 또는 완전한 항법 중단으로 나타날 수 있다. 간섭은 의도적일 수도 있고 비의도적일 수도 있으며, 안전 아키텍처는 간섭의 발생 원인을 추정하는 것보다 관측된 신호 무결성(Signal Integrity)을 기준으로 대응해야 한다.

스푸핑(Spoofing)은 수신기가 위조되거나 조작된 신호를 처리하면서도 외관상 정상적인 항법 해(Navigation Solution)를 계속 생성할 수 있다는 점에서 재밍과 근본적으로 다르다. 스푸핑 공격은 기존의 신호 상실 경고를 즉시 발생시키지 않으면서 잘못된 위치, 속도, 고도 또는 시간 정보를 생성할 가능성이 있다. 따라서 기만적인 GNSS 동작을 탐지하려면 일관성 모니터링(Consistency Monitoring)과 독립적인 항법 기준(Independent Navigation Reference)이 필수적이다.

탐지(Detection)는 수신기 수준 지표(Receiver-Level Indicator)와 항공기 수준의 항법 일관성 검사(Navigation Consistency Check)를 결합해야 한다. 유용한 수신기 관측값에는 위성 수, 신호 강도, 반송파 대 잡음비(Carrier-to-Noise Ratio), 자동 이득 제어(Automatic Gain Control) 동작, 잔차(Residual), 추적 상태, 클록 동작(Clock Behavior), 무결성 플래그(Integrity Flag) 등이 포함된다. 어떠한 단일 지표도 모든 환경에서 신뢰성 있는 보호를 제공할 수 없으므로 여러 지표와 시간에 따른 변화 양상을 결합하여 탐지 신뢰도(Detection Confidence)를 형성해야 한다.

여러 위성 채널에서 동시에 예상하지 못한 신호 변화가 발생하는 경우 신호 강도 이상(Signal-Strength Anomaly)은 간섭이나 스푸핑의 증거를 제공할 수 있다. 정상적인 위성 신호는 일반적으로 예측 가능한 전력 특성을 가지지만 인공적인 간섭 신호는 갑작스럽거나 비정상적으로 높은 상관성을 갖는 변화를 발생시킬 수 있다. 그러나 안테나 방향, 항공기 자세, 건물, 지형, 다중경로(Multipath), 대기 영향도 수신 전력을 변화시킬 수 있으므로 운용 상황을 고려한 임계값(Context-Aware Threshold)이 필요하다.

항법 이노베이션 모니터링(Navigation Innovation Monitoring)은 GNSS 측정값을 관성항법시스템(INS, Inertial Navigation System)의 예측값과 비교한다. 확장 칼만 필터(EKF, Extended Kalman Filter) 또는 유사한 추정기는 관성측정장치(IMU, Inertial Measurement Unit)의 측정값을 사용하여 위치와 속도를 예측하고, GNSS 업데이트가 입력될 때 그 차이를 평가한다. 크거나 지속적인 이노베이션(Innovation)은 GNSS 데이터 손상을 나타낼 수 있지만 정상적인 추정 오차를 공격으로 잘못 판단하지 않도록 관성 드리프트(Inertial Drift)와 모델링 불확실성을 정확하게 반영해야 한다.

속도 일관성(Velocity Consistency)도 강력한 교차 검증(Cross-Check) 수단을 제공한다. GNSS에서 계산된 속도를 관성 가속도, 대기속도(Airspeed), 광류(Optical Flow), 시각 주행거리계(Visual Odometry), 적용 가능한 경우 바퀴 또는 지면 접촉 정보, 항공기의 명령된 움직임과 비교할 수 있다. 독립 센서가 안정적인 호버링 상태를 나타내는데 위치 해가 빠르게 이동한다면 물리적으로 일관되지 않는 상황이며, 항법 시스템은 GNSS 데이터에 대한 신뢰도를 낮춰야 한다.

시간적 일관성(Temporal Consistency)은 점진적으로 조작되는 항법 해를 탐지하는 데 특히 중요하다. 정교한 위조 신호는 보고되는 위치를 매우 천천히 이동시켜 단순한 순간 임계값 검사를 통과할 수 있다. 따라서 탐지기는 단일 샘플 검사에만 의존하지 않고 누적 변위(Accumulated Displacement), 궤적 곡률(Trajectory Curvature), 클록 드리프트(Clock Drift), 이노베이션 변화 추세, 지도 제약조건(Map Constraint), 명령된 움직임과 실제 측정된 움직임 사이의 관계를 장시간에 걸쳐 감시해야 한다.

다중 위성군 및 다중 주파수 수신기(Multi-Constellation and Multi-Frequency Receiver)는 GPS, Galileo, GLONASS, BeiDou 또는 기타 지원되는 위성항법 서비스와 여러 주파수 대역의 관측값을 제공하여 강건성(Robustness)을 향상시킬 수 있다. 서로 다른 위성군 또는 주파수 사이의 불일치는 유용한 진단 정보를 제공할 수 있다. 그러나 공통 안테나, RF 프런트엔드(RF Front-End), 간섭 환경 또는 수신기 소프트웨어가 모든 관측값에 영향을 줄 수 있으므로 다중 위성군 수신 자체를 완전히 독립적인 중복성으로 간주해서는 안 된다.

안테나 기반 기법(Antenna-Based Technique)은 추가적인 보호 계층을 제공할 수 있다. 이중 안테나 또는 다중 안테나 시스템은 반송파 위상(Carrier Phase), 헤딩(Heading), 도래 방향(Direction of Arrival) 특성 또는 공간적 일관성을 비교할 수 있다. 정상적인 위성은 하늘의 여러 방향에 분산되어 있지만 하나의 송신원에서 발생한 위조 신호는 의심스러울 정도로 유사한 공간 특성을 나타낼 수 있다. 이러한 기법에는 적절한 안테나 배치 구조, 보정(Calibration), 수신기 지원 및 높은 설치 품질이 필요하다.

지도 및 임무 제약조건(Map and Mission Constraint)도 GNSS 무결성 모니터링을 지원할 수 있다. UAV의 보고 위치가 물리적으로 도달 가능한 영역에서 크게 벗어나거나, 이에 대응하는 관성 움직임 없이 갑자기 지오펜스(Geofence)를 통과하거나, 불가능한 지형 경계를 통과하는 것으로 나타난다면 항법 불일치의 증거가 될 수 있다. 그러나 지도 정보가 불완전할 수 있고 비상 기동이 정상적인 임무 궤적을 벗어날 수도 있으므로 이러한 검사는 보조적인 증거로 사용해야 한다.

항법 시스템은 단순한 사용 가능 또는 사용 불가 플래그 대신 명시적인 GNSS 건전성 상태(GNSS Health State)를 유지해야 한다. 상태는 정상(Nominal), 의심(Suspicious), 성능 저하(Degraded), 거부(Rejected), 복구 중(Recovering) 등으로 표현할 수 있다. 이상 징후가 누적되면 신뢰도를 점진적으로 감소시켜 완전히 GNSS 데이터를 거부하기 전에 추정기(Estimator)와 유도 시스템(Guidance System)이 GNSS의 가중치를 줄일 수 있다. 이러한 방식은 증거가 불확실한 상황에서 갑작스러운 항법 전환을 방지한다.

GNSS가 신뢰할 수 없는 것으로 판단되면 추정기는 손상된 측정값이 항법 상태에 계속 영향을 미치지 못하도록 해야 한다. 기만된 측정값을 계속 융합하면서 단순히 경고만 표시하는 경우 위치 추정값이 잘못된 해로 이동할 수 있다. 탐지 신뢰도와 불일치의 심각도에 따라 측정 게이팅(Measurement Gating), 적응형 공분산 증가(Adaptive Covariance Inflation), 정보원 격리(Source Isolation), 완전한 GNSS 거부(Complete GNSS Rejection)를 적용할 수 있다.

대체 항법(Fallback Navigation)은 외부 무선 신호에 의존하지 않고 관성측정장치(IMU)를 항공기 내부에서 지속적으로 사용할 수 있기 때문에 관성항법(Inertial Navigation)에서 시작한다. 고속으로 측정되는 가속도계와 자이로스코프 데이터를 이용하여 GNSS가 거부된 이후에도 자세, 속도, 위치를 계속 추정할 수 있다. 그러나 관성 오차는 시간에 따라 누적되므로 특히 저비용 MEMS 센서를 사용하는 소형 UAV에서 순수 관성항법은 무기한 사용할 수 있는 대체 수단이라기보다 일정 시간 동안 항법을 연결하는 브리징 기능(Bridging Capability)에 가깝다.

시각-관성 주행거리계(VIO, Visual-Inertial Odometry)는 카메라 프레임 사이에서 시각 특징점을 추적하고 이를 IMU 기반 운동 추정과 결합하여 관성 드리프트를 제한할 수 있다. GNSS 음영 환경(GNSS-Denied Environment)에서 유용한 상대 위치와 속도 정보를 제공할 수 있지만 조명, 영상 텍스처(Texture), 모션 블러(Motion Blur), 카메라 보정(Camera Calibration), 장면 기하 구조(Scene Geometry)에 따라 성능이 달라진다. 따라서 안전 아키텍처는 시각 항법 자체의 신뢰성이 저하되는 조건도 판단할 수 있어야 한다.

라이다 주행거리계(LiDAR Odometry), 레이더 주행거리계(Radar Odometry), 광류, 지형 상대 항법(Terrain-Relative Navigation), 지도 정합(Map Matching)은 항공기 구성에 따라 추가적인 대체 항법 정보원을 제공할 수 있다. 레이더는 카메라의 성능을 저하시키는 조명 또는 가시성 조건에서도 유용할 수 있으며, 라이다(LiDAR)는 구조화된 환경에서 정밀한 기하학적 제약조건을 제공할 수 있다. 하나의 항법 모달리티(Navigation Modality)를 저하시키는 환경에서도 다른 방식은 정상적으로 동작할 수 있기 때문에 센서 다양성(Diversity)이 중요하다.

기압 고도(Barometric Altitude), 레이더 고도(Radar Altitude), 레이저 거리 측정(Laser Range Measurement), 자기계(Magnetometer), 대기 데이터 센서(Air-Data Sensor)는 독립적으로 완전한 3차원 위치를 제공할 수 없더라도 개별 항법 차원의 오차를 제한할 수 있다. 고장 허용 추정기(Fault-Tolerant Estimator)는 이러한 부분적인 관측 정보를 결합하여 드리프트를 제한할 수 있다. 목표는 정상적인 GNSS 성능을 완전히 재현하는 것이 아니라 안정화, 장애물 회피, 운용 영역 유지(Containment), 안전 복구에 충분한 상태 정확도를 확보하는 것이다.

대체 동작(Fallback Behavior)은 잔여 항법 해(Remaining Navigation Solution)의 품질에 따라 결정되어야 한다. 시각-관성 또는 지형 상대 항법이 높은 신뢰도를 유지한다면 UAV는 사전에 정의된 복구 지점으로 계속 비행할 수 있다. 반대로 단기간의 관성항법만 사용할 수 있다면 위치 불확실성이 과도하게 증가하기 전에 잠시 위치를 유지하거나, 사전에 정의된 안전 프로파일에 따라 상승 또는 하강하거나, 제한된 추측항법(Dead Reckoning) 경로를 따르거나, 착륙을 시작하는 것이 더 안전할 수 있다.

GNSS 음영 운용 중에는 위치 불확실성(Position Uncertainty)을 명시적으로 전파해야 한다. 불확실성이 증가함에 따라 유도 및 지오펜싱 알고리즘은 장애물, 제한 공역, 지형, 착륙 구역 주변의 안전 여유(Safety Margin)를 증가시켜야 한다. 외부 위치 기준이 사라진 이후에도 UAV가 추정 좌표를 정확한 값으로 간주하여 정상 상태와 동일하게 동작해서는 안 된다. 항법 신뢰도(Navigation Confidence)는 허용 가능한 임무 동작에 직접적으로 영향을 미쳐야 한다.

자동 복귀 로직(Return-to-Home Logic)은 스푸핑된 GNSS가 안전 기능 자체를 위험한 명령으로 바꿀 수 있기 때문에 특별한 보호가 필요하다. 항공기는 이미 의심스러운 것으로 분류된 위치 해를 이용하여 자동으로 복귀 명령을 실행해서는 안 된다. 복구 로직(Recovery Logic)은 복귀 지점 위치의 무결성(Home-Position Integrity), 현재 항법 신뢰도, 도달 가능한 대체 지점, 장애물 정보, 사용 가능한 대체 센서를 평가한 후 복귀, 대기(Hold), 우회 또는 착륙 전략을 선택해야 한다.

GNSS 신뢰도 복구(Recovery of GNSS Trust)는 점진적으로 수행되어야 한다. 간섭 이후 외관상 정상적인 신호가 다시 수신되더라도 수신기와 항법 시스템은 전체 가중치를 복원하기 전에 일관성 검사를 수행해야 한다. 다시 획득된 위치, 속도, 시간, 위성 기하 구조(Satellite Geometry), 신호 특성을 독립적으로 전파된 항법 상태와 비교할 수 있다. 복구되었지만 잘못된 항법 해를 갑자기 수용하면 기존 위험이 다시 발생할 수 있다.

로깅(Logging)은 안전 분석과 비행 후 진단(Post-Flight Diagnosis) 모두에서 중요하다. UAV는 수신기 지표, 사용 가능한 경우 위성 관측값, 항법 이노베이션, 추정기 상태, 탐지 이벤트, 정보원 선택 결정(Source-Selection Decision), 대체 모드 전환을 기록해야 한다. 정확한 타임스탬프(Timestamp)를 사용하면 엔지니어가 이상 현상의 원인이 RF 수신, 수신기 처리, 센서 융합, 비행 제어 소프트웨어 또는 외부 환경 중 어디에서 발생했는지를 재구성할 수 있다.

검증(Verification)은 정상적인 비행 시험에만 의존하지 않고 현실적인 GNSS 성능 저하 상황을 항법 시스템에 적용해야 한다. 시뮬레이션과 하드웨어 인 더 루프(HIL, Hardware-in-the-Loop) 환경에서는 위성 신호 상실, 측정 편향(Measurement Bias), 서서히 이동하는 잘못된 위치, 속도 오류, 시간 이상(Timing Anomaly), 다중경로와 유사한 교란, 완전한 신호 중단 등을 주입할 수 있다. 시험에서는 탐지 지연, 오경보 동작, 추정기 격리, 대체 항법 정확도, 불확실성 증가, 복구 전환을 검증해야 한다.

현장 검증(Field Validation)은 의도적인 무선 주파수 간섭이 시험 대상 UAV 이외의 시스템에도 영향을 줄 수 있기 때문에 승인된 시험 방법을 사용하여 통제되고 법규를 준수하는 환경에서 수행해야 한다. 검증의 목적은 통제되지 않은 간섭을 재현하는 것이 아니라 강건한 탐지와 안전한 대체 동작을 입증하는 것이다. 기록된 데이터셋(Recorded Dataset)과 RF 안전 실험실 장비(RF-Safe Laboratory Equipment)를 사용하면 다양한 위협 및 성능 저하 시나리오에서 반복 가능한 평가를 수행할 수 있다.

강건한 GNSS 안전 아키텍처(Resilient GNSS Safety Architecture)는 신호 모니터링(Signal Monitoring), 항법 일관성 검사, 다중 센서 융합(Multi-Sensor Fusion), 명시적인 무결성 상태(Integrity State), 측정값 격리(Measurement Isolation), 다양한 대체 항법(Diversified Fallback Navigation)을 통합한다. 핵심 목표는 위성항법이 항상 사용 가능하도록 보장하는 것이 아니라, GNSS의 상실 또는 손상이 항공기 제어, 운용 영역 유지 또는 안전 복구 능력의 상실로 은밀하게 이어지지 않도록 하는 것이다.

## 08.05. Battery Failure Emergency Landing Trigger [w/Code]

![](images/image5.png){width="7.268055555555556in" height="7.268055555555556in"}

배터리 고장(Battery Failure)은 저장된 전기 에너지가 추진 시스템(Propulsion System), 비행 제어 컴퓨터(Flight-Control Computer), 센서, 통신 장치, 안전 장비에 전력을 공급하기 때문에 전기 추진 UAV에서 매우 중요한 위험 요소이다. 따라서 비상 착륙 트리거(Emergency Landing Trigger)는 단순히 낮은 잔여 충전량뿐만 아니라 사용 가능한 전력을 급격하게 감소시키거나 즉각적인 안전 위협을 발생시킬 수 있는 비정상적인 전기적, 열적, 구조적 배터리 상태까지 탐지해야 한다.

배터리 안전 로직(Battery Safety Logic)은 배터리 관리 시스템(BMS, Battery Management System)과 항공기 에너지 관리 기능(Energy-Management Function)의 지속적인 모니터링에서 시작된다. 중요한 관측값에는 팩 전압(Pack Voltage), 개별 셀 전압(Cell Voltage), 전류, 온도, 충전 상태(SoC, State of Charge), 건강 상태(SoH, State of Health), 내부 저항(Internal Resistance), 전력 공급 능력(Power Capability), 통신 상태가 포함된다. 하나의 측정값만으로는 높은 부하에서 동작하는 UAV 배터리의 전체 상태를 정확하게 판단하기 어렵기 때문에 이러한 측정값을 종합적으로 해석해야 한다.

충전 상태(SoC)는 사용 가능한 잔여 에너지를 추정하지만, SoC만으로 비상 상황을 판단하는 것은 충분하지 않다. 추정 오차, 노화(Aging), 온도, 방전율(Discharge Rate), 셀 불균형(Cell Imbalance)으로 인해 실제 사용 가능한 에너지는 표시되는 비율과 크게 달라질 수 있다. 따라서 비행 시스템은 잔여 비행 가능 시간을 판단할 때 SoC와 함께 전압 변화, 누적 소비 전하(Consumed Charge), 예상 임무 에너지, 부하 요구량, 과거 배터리 특성을 함께 고려해야 한다.

건강 상태(SoH)는 배터리 용량과 전력 공급 능력의 장기적인 성능 저하를 나타낸다. 노화된 배터리는 상당한 잔여 충전량을 표시하더라도 높은 추력이 요구될 때 과도한 전압 강하(Voltage Sag)를 경험할 수 있다. 대형 화물 UAV(Heavy Cargo UAV)에서는 이륙, 상승, 돌풍 대응, 비상 기동이 매우 큰 순간 부하(Transient Load)를 발생시킬 수 있기 때문에 이러한 구분이 특히 중요하다. 에너지 안전은 단순히 에너지가 남아 있는지를 판단하는 것이 아니라 배터리가 요구되는 전력을 실제로 공급할 수 있는지를 고려해야 한다.

셀 수준 모니터링(Cell-Level Monitoring)은 팩 수준 전압만으로는 성능이 저하된 셀을 발견하지 못할 수 있기 때문에 필수적이다. 하나의 열화된 셀이 안전 전압 한계 아래로 떨어지더라도 전체 팩 전압은 정상적으로 보일 수 있다. 배터리 관리 시스템(BMS)은 셀 불균형, 비정상적인 전압 편차, 과도한 내부 저항, 급격한 셀 전압 붕괴(Cell-Voltage Collapse)를 탐지해야 한다. 높은 부하에서 약한 셀 하나가 지속 비행의 안전 여부를 결정하는 제한 요소가 될 수 있다.

전압 강하(Voltage Sag)는 동적으로 해석해야 한다. 높은 전류가 흐르는 동안 내부 저항과 전기화학적 특성으로 인해 단자 전압(Terminal Voltage)이 감소하고, 부하가 감소하면 일정 부분 다시 회복된다. 따라서 단순한 고정 전압 임계값(Fixed Voltage Threshold)을 적용하면 지나치게 이른 착륙이나 위험할 정도로 늦은 대응을 발생시킬 수 있다. 보다 효과적인 로직은 전류, 온도, 예상 전압 강하, 전압 회복 특성, 잔여 임무와 착륙 절차에 필요한 전압 여유를 함께 고려한다.

온도 모니터링(Temperature Monitoring)은 또 다른 핵심 안전 채널을 제공한다. 허용 온도 범위를 벗어나 동작하는 배터리는 전력 공급 능력이 감소하거나 열화가 가속되거나 열적 고장(Thermal Failure)이 발생할 수 있다. 절대 온도뿐만 아니라 급격한 온도 상승률도 중요한 이상 징후가 될 수 있다. 평균 팩 온도에서는 나타나지 않는 국부적인 내부 손상을 발견할 수 있도록 제어기는 온도 변화율과 모듈 또는 셀 사이의 온도 차이를 감시해야 한다.

열 폭주(Thermal Runaway)는 일반적인 저에너지 관리와 근본적으로 다른 대응이 필요하다. 제어되지 않는 온도 상승, 심각한 셀 이상, 가능한 경우 연기 감지(Smoke Detection), 급격하게 악화되는 전기적 이상 징후가 나타나면 즉각적인 안전 조치가 실행되어야 한다. 배터리 상태가 예상보다 빠르게 악화될 수 있기 때문에 먼 복귀 지점까지 계속 비행하는 것은 적절하지 않을 수 있다. 이 경우 우선순위는 임무 유지에서 공중 노출 시간(Airborne Exposure)을 최소화하고 신속하게 착륙하는 방향으로 전환된다.

배터리 고장 분류(Battery Fault Classification)는 주의 정보(Advisory), 경고(Caution), 심각(Critical), 비상(Emergency) 상태로 구분할 수 있다. 주의 상태는 충분한 예비 에너지가 남아 있지만 점진적인 성능 저하가 발생하고 있음을 나타낼 수 있다. 경고 상태에서는 임무 재계획(Mission Replanning)이나 자동 복귀(Return-to-Home)를 시작할 수 있다. 심각 상태에서는 기동을 제한하고 가까운 착륙 지점을 선택하며, 비상 상태에서는 지속적인 동력 비행을 더 이상 보장할 수 없을 때 즉시 착륙 또는 사전에 정의된 안전 대응을 실행해야 한다.

비상 착륙 판단(Emergency Landing Decision)은 현재 배터리 잔량 비율만이 아니라 착륙 시점의 예상 에너지(Predicted Energy at Landing)를 기준으로 해야 한다. 항공기는 후보 착륙 지점까지 이동하는 데 필요한 에너지를 계산하고 바람, 상승 또는 하강, 예상 호버링 시간, 페이로드 중량, 온도, 추진 효율을 반영한 후 정의된 안전 예비량(Safety Reserve)을 추가할 수 있다. 예상 잔여 에너지가 필요한 착륙 예비량보다 낮아질 것으로 판단되면 실제 배터리 한계에 도달하기 전에 복구 동작을 시작해야 한다.

예비 에너지 정책(Reserve Policy)은 불확실성(Uncertainty)을 반영해야 한다. 에너지 예측에는 바람 추정, 배터리 모델, 궤적 변경, 노화, 예상하지 못한 기동으로 인한 오차가 포함된다. 따라서 모든 임무에 동일한 고정 예비 비율을 적용하는 것은 충분하지 않을 수 있다. 동적 예비량 계산(Dynamic Reserve Calculation)은 높은 페이로드, 강풍, 저온, 저하된 배터리 건강 상태, 불확실한 항법 또는 적절한 착륙 지점에서 멀리 떨어진 운용 상황에서 안전 여유를 증가시킬 수 있다.

착륙 트리거(Landing Trigger)는 자동 복귀(Return-to-Home)와 즉시 착륙(Immediate Landing)을 구분해야 한다. 충분한 에너지와 전력 여유가 있어 전체 복귀 경로를 안전하게 완료할 수 있는 경우에만 자동 복귀가 적절하다. 전압이 빠르게 붕괴하거나 열 상태가 악화되거나 복귀에 필요한 에너지가 예상 예비량을 초과한다면 발사 지점까지 복귀하려는 시도 자체가 위험을 증가시킬 수 있다. 이 경우 시스템은 더 가까운 도달 가능한 착륙 지점을 선택해야 한다.

도달 가능한 착륙 지점 평가(Reachable Landing-Site Evaluation)는 에너지 관리와 항법 및 인지 시스템(Navigation and Perception System)을 연결한다. UAV는 거리, 필요한 고도 변화, 바람, 지형, 장애물, 지표면 특성, 인구 노출도(Population Exposure), 사용 가능한 에너지를 평가할 수 있다. 물리적으로 가장 가까운 위치라고 해서 반드시 가장 안전한 것은 아니며, 해당 지점에 도달하기 위해 상승, 호버링 또는 복잡한 기동이 필요할 수 있다. 후보 지점은 단순한 지리적 거리보다 전체 위험(Total Risk)을 최소화하도록 선택해야 한다.

화물 UAV에서는 페이로드 중량(Payload Mass)이 비상 에너지 계산에 직접적인 영향을 미친다. 무거운 화물을 운송하는 항공기는 훨씬 높은 호버링 전력을 필요로 할 수 있으며, 전압이 감소하면 추력 여유(Thrust Margin)가 매우 작아질 수 있다. 에너지 관리 시스템은 가능한 경우 현재 중량과 무게중심(Center of Gravity) 정보를 사용해야 한다. 무부하 항공기 모델을 기반으로 한 예측은 잔여 비행 가능 시간을 크게 과대평가하여 위험할 정도로 늦은 착륙 결정을 발생시킬 수 있다.

다중 배터리 팩(Multiple Battery Pack)은 전기 아키텍처가 실제적인 고장 격리를 지원하는 경우 고장 허용성(Fault Tolerance)을 향상시킬 수 있다. 적절한 보호 없이 병렬 연결된 배터리 팩은 고장 전류(Fault Current)나 버스 전압 붕괴(Bus Collapse)를 통해 고장난 팩이 정상 팩에 영향을 줄 수 있다. 접촉기(Contactor), 전류 센서, 절연 장치(Isolation Device), 독립 모니터링, 제어된 교차 연결(Controlled Cross-Connection)을 사용하면 손상된 팩을 분리하면서 정상 에너지원의 전력을 유지할 수 있다.

배터리 모듈을 격리한 이후 항공기는 사용 가능한 에너지와 전력을 즉시 다시 계산해야 한다. 두 개의 배터리 팩 가운데 하나를 상실한 이후에도 비행을 유지할 수 있는 항공기라 하더라도 정상 용량의 절반 정도만 사용할 수 있으며 최대 출력 능력도 감소할 수 있다. 따라서 비행 제어 및 임무 관리 시스템(Mission-Management System)은 갱신된 성능 한계를 전달받아 성능 저하 상태에서 급격한 가속, 높은 상승률 또는 불필요한 호버링을 피해야 한다.

에너지가 제한될 경우 전력 우선순위 관리(Power Prioritization)를 통해 안전 운용 시간을 연장할 수 있다. 추진 시스템, 비행 제어 컴퓨팅(Flight-Control Computing), 필수 항법(Essential Navigation), 통신, 비상 센서는 비필수 페이로드 처리, 고전력 임무 장비 또는 편의 기능보다 높은 우선순위로 전력을 공급받아야 한다. 제어된 부하 차단(Controlled Load Shedding)은 전력 소비를 감소시키고 항공기 안정화, 착륙 지점 도달, 최종 하강에 필요한 에너지를 보존한다.

비상 착륙 로직(Emergency Landing Logic)은 추진 능력(Propulsion Capability)과 지속적으로 연계되어야 한다. 배터리 전압이 감소하면 전체 에너지가 완전히 소진되기 전에도 최대 모터 속도와 사용 가능한 추력이 감소할 수 있다. 따라서 항공기는 잔여 용량을 표시하면서도 상승 또는 호버링 능력을 상실할 수 있다. 전력 인식 비행 제어(Power-Aware Flight Control)는 사용 가능한 추력을 지속적으로 추정하여 유도 명령(Guidance Command)이 배터리와 추진 시스템에서 더 이상 제공할 수 없는 힘을 요구하지 않도록 해야 한다.

최종 착륙 단계(Final Landing Phase)에는 호버링, 장애물 회피, 플레어(Flare), 정밀 하강(Precision Descent)이 상당한 전력을 소비할 수 있기 때문에 보호된 에너지(Protected Energy)가 필요하다. 따라서 착륙 예비량에는 접근 과정의 불확실성과 항공기 설계가 지원하는 경우 제한적인 복행(Go-Around) 또는 위치 재조정 능력을 위한 에너지가 포함되어야 한다. 착륙 구역에 도달하는 데 거의 모든 에너지를 소비하면 안전에 가장 중요한 최종 복구 단계에서 충분한 제어 권한을 확보하지 못할 수 있다.

운용자와의 통신(Operator Communication)은 안전 복구가 사람의 대응에 의존하지 않으면서도 의미 있는 상태 정보를 제공해야 한다. 경고 정보에는 배터리 상태, 예상 비행 가능 시간, 선택된 복구 동작, 착륙 위치, 성능 저하 상태를 포함할 수 있다. 그러나 심각 임계값(Critical Threshold)에 도달하고 통신을 사용할 수 없거나 지연되는 경우 기체 자율 시스템(Onboard Autonomy)은 운용자의 확인을 무기한 기다리는 대신 사전에 정의된 안전 대응을 실행해야 한다.

트리거 히스테리시스(Trigger Hysteresis)와 지속성 로직(Persistence Logic)은 정상, 복귀, 비상 모드 사이에서 불안정하게 반복 전환되는 것을 방지할 수 있다. 임계값 부근에서 발생하는 작은 전압 변동 때문에 항공기의 임무 상태가 반복적으로 변경되어서는 안 된다. 반면 빠르게 진행되는 고장은 즉각적인 대응 단계 상승(Escalation)이 필요하다. 따라서 트리거 설계는 서서히 변화하는 상태에 대해서는 지속성 판단을 적용하고, 빠른 전압 붕괴, 과전류(Overcurrent), 급격한 열적 악화에는 변화율 기반 우선 대응(Rate-Sensitive Override)을 적용할 수 있다.

센서와 배터리 관리 시스템(BMS)의 고장 자체도 고려해야 한다. 배터리 통신 상실, 고정된 측정값(Frozen Measurement), 비현실적인 충전 상태 값, 일관되지 않은 전류 적산(Current Integration), 중복 전압 측정값 사이의 불일치는 에너지 상태를 불확실하게 만들 수 있다. 안전 중심 제어기는 심각한 배터리 관측 가능성(Battery Observability) 상실을 성능 저하 조건으로 처리하고 마지막 정상 측정값이 계속 유효하다고 가정하는 대신 예비 여유를 증가시키거나 복구 동작을 시작해야 한다.

비상 착륙 트리거는 검증이 가능할 정도로 결정론적(Deterministic)이어야 하면서 운용 상황에 적응할 수 있어야 한다. 임계값, 예측 모델, 불확실성 여유(Uncertainty Margin), 상태 전환(State Transition), 우선 개입 조건(Override Condition)은 명확하게 정의되고 안전 요구사항(Safety Requirement)까지 추적 가능해야 한다. 아키텍처는 어떤 조건에서 경고를 생성하고, 어떤 조건에서 자동 복귀를 시작하며, 어떤 조건에서 우회를 강제하고, 어떤 조건에서 즉시 착륙을 명령하는지를 명확하게 규정해야 한다.

소프트웨어 인 더 루프(SIL, Software-in-the-Loop)와 하드웨어 인 더 루프(HIL, Hardware-in-the-Loop) 시험을 통해 충전 수준, 온도, 페이로드, 바람, 노화, 비행 단계의 다양한 조합에서 배터리 안전 로직을 평가할 수 있다. 고장 주입(Fault Injection)에는 약한 셀, 갑작스러운 전압 붕괴, 센서 편향(Sensor Bias), BMS 통신 상실, 과열, 배터리 팩 분리, 잘못된 SoC 추정 등을 포함해야 한다. 검증에서는 트리거 작동 시점, 궤적 실행 가능성, 착륙 시 잔여 예비량, 상충되는 상태 지표에 대한 대응을 측정해야 한다.

실물 검증(Physical Validation)은 제어된 방전 시험(Controlled Discharge Test), 추진 시스템 시험대(Propulsion Bench), 환경 챔버(Environmental Chamber), 엄격하게 제한된 비행 시험을 이용하여 실제 배터리 특성을 분석할 수 있다. 측정된 전압 강하, 열적 응답, 사용 가능 용량, 출력 한계는 기체 내장 모델(Onboard Model)을 개선하는 데 사용할 수 있다. 안전 로직이 정상적인 새 배터리만을 기준으로 보정될 경우 실제 운용 중 발생하는 성능 저하를 과소평가할 수 있으므로 새 배터리와 노화된 배터리를 모두 시험하는 것이 중요하다.

비행 후 데이터 분석(Post-Flight Data Analysis)을 위해 배터리 전압, 셀 측정값, 전류, 온도, BMS 상태, 추정된 SoC와 SoH, 예상 에너지, 추진 시스템 요구량, 트리거 상태 전환, 착륙 결정 정보를 보존해야 한다. 이러한 기록을 이용하면 엔지니어가 예측값과 실제 배터리 동작을 비교하고, 임계값을 개선하며, 배터리 노화 추세(Aging Trend)를 식별하고, 비상 대응이 적절한 안전 여유를 확보한 상태에서 시작되었는지를 판단할 수 있다.

견고한 배터리 고장 비상 착륙 아키텍처(Battery-Failure Emergency Landing Architecture)는 셀 수준 모니터링, 에너지 및 전력 예측, 열 보호(Thermal Protection), 고장 격리, 동적 예비량(Dynamic Reserve), 부하 차단, 도달 가능한 착륙 지점 평가, 자율 복구(Autonomous Recovery)를 통합한다. 핵심 목표는 전기 에너지 또는 추진 능력이 제어 불가능한 수준까지 저하되어 강제적인 비제어 하강이 발생할 때까지 기다리는 것이 아니라, 착륙이 여전히 제어 가능한 선택(Controlled Choice)으로 남아 있을 만큼 충분히 이른 시점에 안전 동작을 시작하는 것이다.

## 08.06. Comm Link Loss Contingency Behavior C2 Link [w/Code]

![](images/image6.png){width="7.268055555555556in" height="7.268055555555556in"}

지휘 및 제어 링크(C2 Link, Command and Control Link)는 UAV를 원격 조종국(Remote Pilot Station), 플릿 관리자(Fleet Manager) 또는 감독 제어 시스템(Supervisory Control System)과 연결하며 명령, 텔레메트리(Telemetry), 상태 정보, 임무 업데이트, 안전 상태를 전달한다. 특히 자율 비행과 가시권 밖 비행(BVLOS, Beyond Visual Line of Sight)에서는 이 링크의 상실 또는 심각한 성능 저하를 예외적인 소프트웨어 사건이 아니라 예상 가능한 운용 비상 상황(Operational Contingency)으로 취급해야 한다.

C2 링크 고장(C2 Link Failure)은 지형 차폐(Terrain Masking), 과도한 통신 거리, 안테나 차단, 간섭, 네트워크 혼잡, 장비 고장, 지상국 고장, 전원 상실 또는 통신 인프라 장애로 인해 발생할 수 있다. 성능 저하는 지연 시간(Latency)과 패킷 손실(Packet Loss)이 점진적으로 증가하는 형태로 나타날 수도 있고 완전한 연결 단절로 갑작스럽게 발생할 수도 있다. 따라서 UAV는 단순한 연결 또는 단절 상태만 사용하는 대신 통신 품질을 지속적으로 감시해야 한다.

링크 상태 모니터링(Link-Health Monitoring)에는 수신 신호 강도(Received Signal Strength), 신호 대 잡음비(Signal-to-Noise Ratio), 패킷 오류율(Packet Error Rate), 패킷 손실, 지연 시간, 지터(Jitter), 하트비트(Heartbeat) 수신, 시퀀스 카운터(Sequence Counter), 재전송 활동(Retransmission Activity), 메시지 경과 시간(Message Age) 등이 포함될 수 있다. 짧은 통신 장애는 문제가 되지 않을 수 있지만 지속적인 성능 저하는 명령 권한 또는 텔레메트리 가용성 상실이 임박했음을 의미할 수 있으므로 이러한 지표를 적절한 시간 구간에 걸쳐 평가해야 한다.

통신 무결성(Communication Integrity)은 가용성(Availability)만큼 중요하다. 링크가 기술적으로 연결된 상태를 유지하더라도 지연되거나, 중복되거나, 손상되거나, 순서가 뒤바뀐 메시지로 인해 원격 제어가 안전하지 않을 수 있다. 따라서 명령에는 적절한 경우 타임스탬프(Timestamp), 시퀀스 정보, 유효 기간(Validity Period), 무결성 보호(Integrity Protection)가 포함되어야 한다. 비행 시스템은 인증된 통신 채널을 통해 도착했다는 이유만으로 오래되거나 유효하지 않은 명령을 실행해서는 안 되며 이를 거부해야 한다.

C2 모니터링은 업링크(Uplink)와 다운링크(Downlink) 고장을 구분해야 한다. UAV가 계속 명령을 수신하지만 텔레메트리를 운용자에게 전송하지 못할 수도 있고, 운용자는 항공기 데이터를 수신하지만 명령이 기체에 도달하지 않을 수도 있다. 이러한 비대칭 통신 상태(Asymmetric Communication Condition)에서는 기체 내장 시스템과 지상국이 서로 다른 링크 상태를 인식할 수 있기 때문에 서로 다른 대응이 필요하다. 명시적인 링크 상태 추정(Link-State Estimation)은 양방향 통신 상태를 모두 고려해야 한다.

비상 상태 머신(Contingency State Machine)은 정상(Nominal), 성능 저하(Degraded), 일시적 상실(Temporarily Lost), 상실 확정(Confirmed Lost), 복구(Recovery), 복원(Restored) 상태를 표현할 수 있다. 상태 전환 임계값(Transition Threshold)은 항공기 동역학, 임무 단계(Mission Phase), 네트워크 특성, 운용 위험을 반영해야 한다. 짧은 패킷 공백으로 불필요한 비상 기동을 실행해서는 안 되지만, 장애물 또는 관제 공역 근처에서 장시간 명령이 상실되는 경우에는 훨씬 빠른 자율 개입(Autonomous Intervention)이 필요할 수 있다.

통신 성능 저하에 대한 첫 번째 대응은 임무를 즉시 종료하는 것이 아니라 통신 요구량(Communication Demand)을 안정화하는 방식이 될 수 있다. 비필수 데이터 스트림(Nonessential Data Stream)을 줄이고, 텔레메트리 전송률을 낮추며, 영상 전송을 제한하거나 대체 통신 경로(Alternative Communication Path)를 활성화할 수 있다. 제한된 대역폭에서도 필수 제어, 상태 보고, 비상 상황 조정을 수행할 수 있도록 안전 필수 명령과 상태 메시지에 높은 우선순위를 부여해야 한다.

중복 통신 링크(Redundant Communication Link)는 동일한 고장 영역(Failure Domain)을 공유하지 않을 경우 통신 가용성을 향상시킬 수 있다. UAV는 임무 요구사항에 따라 셀룰러(Cellular), 전용 무선 통신(Dedicated Radio), 위성 통신(Satellite Communication), 메시 네트워크(Mesh Network) 또는 기타 승인된 통신 기술을 결합할 수 있다. 그러나 하나의 고장이 모든 경로를 동시에 비활성화하지 않도록 안테나 배치, 전원 공급, 네트워크 인프라, 소프트웨어 라우팅(Software Routing), 환경적 취약성이 충분히 독립적인 경우에만 중복성이 실질적인 의미를 갖는다.

자동 링크 전환(Automatic Link Switching)은 명령 권한(Command Authority)과 메시지 순서를 유지해야 한다. 주 통신 경로가 불안정해지면 시스템은 트래픽을 보조 경로로 전환할 수 있지만, 이전 링크에서 지연되어 도착하거나 중복된 명령이 항공기의 상충된 동작을 유발해서는 안 된다. 전환 과정 전체에서 세션 상태(Session State), 명령 시퀀스 번호(Command Sequence Number), 송신원 식별(Source Identity), 제어 권한(Control Ownership)의 일관성이 유지되어야 한다.

사용 가능한 모든 C2 경로가 상실되면 기체 자율 시스템(Onboard Autonomy)이 안전한 항공기 동작을 유지할 책임을 갖는다. 마지막으로 수신된 명령을 계속 수행하는 것이 안전하다는 사실이 명확하게 입증되지 않은 경우 UAV는 해당 명령을 무기한 실행해서는 안 된다. 비상 상황 관리자(Contingency Manager)는 항공기 상태, 임무 단계, 항법 무결성(Navigation Integrity), 잔여 에너지, 기상 제약조건, 공역 경계, 사용 가능한 복구 지점을 고려해야 한다.

단기간의 링크 상실에서는 제어된 대기 동작(Controlled Hold Behavior)이 적절할 수 있다. 항법 신뢰도가 높은 경우 멀티로터(Multirotor)는 위치를 유지하거나 작은 운용 제한 패턴(Containment Pattern)을 따를 수 있으며, 고정익 항공기는 호버링이 불가능하기 때문에 사전에 정의된 선회 대기 패턴(Loiter Pattern)이 필요할 수 있다. 대기 시간은 에너지 예비량(Energy Reserve), 공역 제약, 항법 불확실성, 통신 복구 가능성을 고려하여 제한되어야 한다.

자동 복귀(Return-to-Home)는 일반적인 비상 대응이지만 모든 상황에서 안전하다고 간주해서는 안 된다. 복귀 경로에는 지형, 장애물, 제한 공역, 악천후 또는 통신 음영 지역이 존재할 수 있다. 또한 C2 링크 상실과 동시에 위성항법시스템(GNSS) 또는 항법 무결성이 저하될 수도 있다. 따라서 복구 로직(Recovery Logic)은 자율 복귀를 실행하기 전에 복귀 위치와 경로가 여전히 유효한지를 검증해야 한다.

대체 복구 지점(Alternate Recovery Site)으로 우회하는 것이 발사 지점으로 복귀하는 것보다 안전할 수 있다. UAV는 에너지, 지형, 인구 노출도(Population Exposure), 공역 제약, 기상, 항법 신뢰도를 기반으로 도달 가능한 착륙 지점을 평가할 수 있다. 화물 UAV 운용에서는 경로를 따라 여러 비상 착륙 지점(Contingency Site)을 사전에 정의하여 C2 상실 시 가까운 안전 복구 지점이 있음에도 장거리 복귀 비행을 수행하지 않도록 할 수 있다.

C2 상실 이후의 임무 지속(Mission Continuation)은 운용 개념(Operational Concept)과 안전 분석(Safety Analysis)에서 명시적으로 지원되는 경우에만 허용해야 한다. 높은 수준의 자율성을 갖는 UAV는 지속적인 인간 명령 없이 제한된 임무 구간을 안전하게 완료할 수 있지만, 이를 위해서는 신뢰할 수 있는 항법, 장애물 회피, 지오펜싱(Geofencing), 상태 모니터링, 비상 상황 관리가 필요하다. 지속 비행은 사전에 정의된 시간, 거리 또는 지리적 제약조건 내에서 제한되어야 한다.

지오펜싱은 원격 개입을 사용할 수 없는 링크 상실 운용에서 특히 중요하다. 기체 내장 시스템은 지상국으로부터 업데이트를 수신할 수 없는 경우에도 고도, 지리적 영역, 공역 한계를 독립적으로 강제 적용해야 한다. 위치 불확실성이 증가하면 운용 제한 여유(Containment Margin)도 이에 따라 확대되어야 하며, 불확실성이 제한 경계에 위협이 되기 전에 항공기가 착륙 상태로 전환해야 할 수도 있다.

에너지 관리(Energy Management)는 모든 링크 상실 판단에 영향을 미쳐야 한다. 통신 복구를 기다리며 대기하는 동안에는 이후 복귀 또는 착륙에 필요할 수 있는 에너지가 소비된다. 비상 상황 관리자는 링크가 복구될 가능성과 그 이점을 지속적으로 감소하는 에너지 예비량과 비교해야 한다. 바람, 페이로드, 배터리 성능 저하, 안전 착륙 지점까지의 거리가 가용 여유를 변화시키기 때문에 고정된 대기 시간(Fixed Waiting Period)을 모든 상황에 동일하게 적용하는 것은 적절하지 않을 수 있다.

비행 단계(Flight Phase)는 비상 대응 동작에 큰 영향을 미친다. 순항 중 C2 상실은 링크 복구 또는 경로 우회를 위한 충분한 시간을 제공할 수 있지만, 이륙, 최종 접근(Final Approach), 화물 투하(Cargo Release), 구조물 인근 운용 중 발생하는 링크 상실은 즉각적으로 사전 정의된 대응을 요구할 수 있다. 따라서 비상 아키텍처는 전체 비행에 하나의 동일한 링크 상실 절차를 적용하는 대신 각 임무 단계에 적합한 안전 동작을 연결해야 한다.

화물 UAV에서는 페이로드 상태(Payload Condition)가 비상 대응을 변경할 수 있다. 외부 현수 하중(Externally Suspended Load)은 난류 환경에서 대기 비행을 위험하게 만들 수 있으며, 무거운 페이로드는 복귀 가능 거리와 착륙 선택지를 감소시킬 수 있다. 페이로드 분리 기능(Payload Release Capability)이 존재하더라도 통신이 상실되었다는 이유만으로 자동으로 화물을 분리해서는 안 된다. 모든 화물 분리에는 별도의 안전 기준이 필요하며 지상의 사람과 재산이 허용할 수 없는 위험에 노출되지 않는다는 것을 보장해야 한다.

기체 비행 제어 시스템(Onboard Flight-Control System)은 C2 가용성과 관계없이 기본 안정화(Basic Stabilization)에 대한 제어 권한을 유지해야 한다. 원격 명령은 일반적으로 기본적인 자세, 각속도, 액추에이터, 비행 영역 보호(Envelope Protection)를 우회하는 것이 아니라 요구되는 임무 동작을 지정하는 형태여야 한다. 이러한 분리를 통해 감독 통신 경로(Supervisory Communication Path)가 상실되더라도 항공기를 동적으로 안정하게 유지하는 데 필요한 로컬 제어 루프(Local Control Loop)는 계속 동작할 수 있다.

여러 지상국, 원격 조종사 또는 감독 시스템을 사용할 수 있는 경우 명령 권한 관리(Command Authority Management)가 중요하다. 통신 복구 과정에서 항공기는 어떤 정보원이 제어 권한을 다시 획득할 수 있는지를 판단해야 한다. 이전 세션의 명령과 새롭게 설정된 세션의 상충된 명령을 동시에 수용해서는 안 된다. 제어 권한 전환(Control Transfer)은 결정론적인 권한 소유(Deterministic Ownership), 인증(Authentication), 세션 검증(Session Validation), 명시적인 상태 동기화(State Synchronization)를 사용해야 한다.

재연결(Reconnection)이 이루어졌다고 해서 시스템 일관성을 확인하지 않은 상태에서 전체 원격 제어 권한을 즉시 복원해서는 안 된다. 통신 중단 동안 UAV는 경로, 고도, 모드 또는 착륙 목적지를 변경했을 수 있지만 지상국은 오래된 항공기 상태를 가지고 있을 수 있다. 정상적인 감독 제어가 재개되기 전에 항공기와 운용자 시스템은 현재 상태, 임무 버전(Mission Version), 비상 모드, 항법 상태, 명령 시퀀스 정보를 교환해야 한다.

인간-기계 인터페이스(HMI, Human-Machine Interface)는 링크 상실 동작을 운용자가 쉽게 이해할 수 있도록 설계해야 한다. 비행 전에 설정된 비상 대응 정책(Contingency Policy)을 확인하고 검증할 수 있어야 한다. 통신 성능이 저하되는 동안 지상국은 링크 품질, 마지막 정상 통신 시간, 항공기의 비상 상태, 예상되는 자율 동작을 표시해야 한다. 재연결 이후에는 통신을 사용할 수 없었던 동안 UAV가 수행한 동작을 명확하게 보고해야 한다.

로깅(Logging)은 통신 상태와 비행 상황을 함께 보존해야 한다. 관련 기록에는 링크 품질 지표, 패킷 손실, 지연 시간, 하트비트 이벤트, 명령 타임스탬프, 모드 전환, 대체 링크 활성화, 자율 판단, 항법 신뢰도, 에너지 상태, 복구 동작이 포함된다. 시간적으로 정렬된 기록(Time-Aligned Record)을 이용하면 엔지니어가 사건의 원인이 무선 통신 성능, 네트워크 인프라, 지상 소프트웨어, 기체 통신 시스템 또는 비상 대응 로직 중 어디에서 발생했는지를 판단할 수 있다.

검증(Verification)은 완전한 링크 단절뿐만 아니라 현실적인 통신 성능 저하를 포함해야 한다. 소프트웨어 인 더 루프(SIL, Software-in-the-Loop)와 하드웨어 인 더 루프(HIL, Hardware-in-the-Loop) 시험에서는 지연 시간, 지터, 패킷 손실, 대역폭 제한, 비대칭 업링크 및 다운링크 고장, 오래된 명령, 네트워크 전환, 반복적인 재연결, 완전한 통신 중단을 주입할 수 있다. 시험은 통신 장애와 항법 고장이 결합된 상황에서도 결정론적인 상태 전환과 안전 동작이 수행되는지를 확인해야 한다.

현장 시험(Field Testing)은 독립적인 안전 감독(Independent Safety Supervision)을 유지하면서 짧고 통제된 통신 중단에서 실제 운용을 대표하는 시나리오까지 단계적으로 확장할 수 있다. 엔지니어는 고장 탐지 시간, 모드 전환 지연(Mode-Transition Latency), 운용 영역 유지 성능, 에너지 소비, 링크 전환 동작, 복구 성공 여부를 측정해야 한다. 또한 일시적인 통신 장애가 과도한 불필요 복귀(Nuisance Return)나 불필요한 비상 착륙을 발생시키지 않는지도 검증해야 한다.

안전 분석(Safety Analysis)은 C2 링크와 다른 시스템 사이의 공통 원인 고장(Common-Cause Failure)을 고려해야 한다. 공유 전원, 공통 안테나 위치, 기체 내 네트워크 스위치, 소프트웨어 라우터(Software Router), 지상 인프라는 외관상 중복된 통신 채널을 동시에 무력화할 수 있다. 따라서 독립성 분석(Independence Analysis)은 무선 통신 하드웨어뿐만 아니라 전력, 네트워킹, 시간 동기화, 소프트웨어, 지상 시스템, 환경 노출까지 확장되어야 한다.

견고한 C2 링크 상실 아키텍처(Robust C2 Lost-Link Architecture)는 지속적인 링크 상태 모니터링, 통신 다양성(Communication Diversity), 결정론적 상태 관리(Deterministic State Management), 자율 안정화(Autonomous Stabilization), 항법 상태를 고려한 복구(Navigation-Aware Recovery), 에너지 인식 의사결정(Energy-Aware Decision Making), 운용 영역 유지(Containment), 제어된 재연결(Controlled Reconnection)을 통합한다. 핵심 목표는 통신이 절대로 끊기지 않도록 보장하는 것이 아니라, 일시적 또는 영구적인 원격 연결 상실이 발생하더라도 UAV가 예측 가능하고 안전을 최우선으로 하는 대응 방법을 항상 유지하도록 하는 것이다.

## 08.07. Geofence and Volume Constraint Enforcement [w/Code]

![](images/image7.png){width="7.268055555555556in" height="7.268055555555556in"}

지오펜스 및 비행 공간 제약 강제 적용(Geofence and Volume-Constraint Enforcement)은 UAV가 금지된 영역에 진입하거나 승인된 운용 공간(Operating Volume)을 벗어나는 것을 방지하는 안전 경계를 제공한다. 단순한 2차원 지도 경계와 달리 견고한 시스템은 수평 경계, 고도 하한과 상한, 지형과의 관계, 임시 제한 구역, 임무별 운용 제한 영역(Mission-Specific Containment Area)을 제약조건으로 표현하고 자율 비행 및 원격 감독 비행 전체에서 이를 지속적으로 적용한다.

지오펜스(Geofence)는 내부 유지 경계(Keep-In Boundary)와 진입 금지 경계(Keep-Out Boundary)로 구분할 수 있다. 내부 유지 경계는 항공기가 머물도록 허가된 공간을 정의하며, 진입 금지 경계는 진입해서는 안 되는 공역, 인프라, 지형, 인구 밀집 지역 또는 기타 영역을 나타낸다. 복잡한 임무에서는 두 가지 경계를 동시에 사용하여 서로 다른 안전 우선순위를 갖는 여러 제외 공간(Exclusion Volume)으로 둘러싸인 허용 비행 회랑(Permitted Corridor)을 구성할 수 있다.

UAV 운용은 본질적으로 공간적이기 때문에 3차원 비행 공간 표현(Three-Dimensional Volume Representation)이 중요하다. 수평 다각형(Horizontal Polygon)만으로는 특정 구조물 위의 허가된 경로와 금지된 고도층 또는 지형 위험을 구분할 수 없다. 따라서 운용 공간은 위도와 경도 또는 로컬 좌표(Local Coordinate), 고도 하한과 상한, 기준 좌표계(Reference Datum), 필요한 경우 시간 유효성(Time Validity)을 포함하여 각각의 제약조건이 명확한 물리적 의미를 갖도록 해야 한다.

고도 기준(Altitude Reference)은 특별히 주의해서 관리해야 한다. 기압 고도(Barometric Altitude), GNSS 고도(GNSS Altitude), 지상고(AGL, Height Above Ground Level), 로컬 기준점에 대한 상대 고도는 서로 상당한 차이가 발생할 수 있다. 하나의 기준으로 정의된 제약조건을 변환 없이 다른 기준의 상태 추정값과 직접 비교해서는 안 된다. 지오펜스 아키텍처는 고도 기준면(Altitude Datum) 정보를 명시적으로 유지하고 수직 경계를 평가할 때 지형 고도와 센서 불확실성을 고려해야 한다.

정적 지오펜스(Static Geofence)는 비행 전에 입력되며 일반적으로 임무 수행 중 변경되지 않는다. 동적 지오펜스(Dynamic Geofence)는 임시 공역 제한, 비상 구역, 이동하는 운용 경계 또는 갱신된 교통 관리 제약조건을 표현할 수 있다. 새롭게 수신된 경계가 현재 항공기 위치와 충돌하거나 계획된 경로를 실행 불가능하게 만들 수 있으므로 동적 업데이트에는 검증(Validation)이 필요하다. 업데이트를 즉시 따를 수 없는 상황에서도 UAV는 항상 안전한 대응 방법을 유지해야 한다.

경계 강제 적용(Boundary Enforcement)은 지상국에만 의존하지 않고 기체 내에서 수행되어야 한다. 지휘 및 제어(C2, Command and Control) 통신이 중단되면 운용 영역 유지가 가장 중요한 순간에 원격 감독 기능을 사용할 수 없게 될 수 있다. 따라서 항공기는 안전 필수 제약조건(Safety-Critical Constraint)을 로컬에 저장하고 외부 통신을 사용할 수 없는 상황에서도 계속 적용해야 한다. 지상 시스템은 업데이트와 모니터링을 제공할 수 있지만 기본적인 운용 영역 유지(Containment)는 독립적인 기체 내장 기능으로 유지되어야 한다.

지오펜스 모니터링(Geofence Monitoring)은 신뢰할 수 있는 항공기 위치와 불확실성 추정에서 시작된다. 항법 시스템은 위치, 고도, 속도와 관련 공분산(Covariance) 또는 무결성 정보(Integrity Information)를 제공한다. 운용 영역 유지 기능은 추정된 단일 위치점만 평가하는 것이 아니라 해당 추정값을 둘러싼 불확실성도 함께 평가한다. 경계에 가까우면서 위치 불확실성이 큰 항공기는 동일한 추정 위치에서 높은 항법 신뢰도를 가진 항공기보다 더 큰 위험을 나타낼 수 있다.

따라서 항법 불확실성(Navigation Uncertainty)이 증가하면 안전 여유(Safety Margin)도 확대되어야 한다. 정상 지오펜스 경계 안쪽에는 위치 추정 오차, 제어 추종 오차(Control Tracking Error), 바람 외란, 통신 지연, 기체 크기, 정지 또는 선회 거리를 고려하는 내부 보호 버퍼(Internal Protection Buffer)를 설정할 수 있다. 항공기는 실제 허가 공간 경계에 도달하기 전에 보호 경계에서 대응을 시작하여 정상적인 제어 오차가 실제 경계 위반으로 발전하지 않도록 해야 한다.

예측형 경계 모니터링(Predictive Boundary Monitoring)은 단순한 현재 위치 검사보다 높은 수준의 보호 기능을 제공한다. 시스템은 현재 속도, 가속도, 명령된 움직임, 바람 추정값, 기체 동역학을 이용하여 UAV의 미래 궤적을 예측할 수 있다. 예측된 경로가 정의된 예측 시간 구간(Look-Ahead Horizon) 내에서 제약조건과 교차하는 경우 항공기가 실제 경계에 접근하기 전에 수정 동작을 시작할 수 있다. 이는 상당한 운동량을 갖는 고속 또는 대형 UAV에서 특히 중요하다.

제약조건 강제 적용(Constraint Enforcement)은 여러 수준의 개입 단계로 수행될 수 있다. 경계에서 충분히 멀리 떨어진 경우 임무 계획(Mission Planning)이 제한 구역을 자연스럽게 회피하는 경로를 생성할 수 있다. 경계에 가까워지면 유도 시스템(Guidance System)이 속도 또는 궤적 명령을 수정할 수 있다. 위험이 즉각적인 수준에 도달하면 비행 제어 보호 기능(Flight-Control Protection)이 명령을 직접 제한할 수 있다. 이러한 계층적 강제 적용(Layered Enforcement)은 하나의 계획 또는 제어 기능에 대한 의존성을 감소시킨다.

명령 제한(Command Limiting)은 원격 조종사, 자율 계획기(Autonomous Planner), 상위 수준 에이전트(Higher-Level Agent)가 안전 경계를 위반하는 움직임을 요구하는 것을 방지한다. 요구 속도, 위치, 가속도 또는 웨이포인트(Waypoint) 명령은 승인 전에 활성화된 제약조건 집합(Active Constraint Set)을 기준으로 검사할 수 있다. 시스템 설계에 따라 안전하지 않은 명령은 제한(Clipping)하거나 허용 가능한 방향으로 투영하거나, 거부하거나, 사전에 정의된 복구 명령(Recovery Command)으로 대체할 수 있다.

궤적 실행 가능성(Trajectory Feasibility)은 UAV가 즉시 정지할 수 있는 점 질량(Point Mass)이라고 가정하는 대신 실제 기체 동역학을 고려해야 한다. 최대 제동 가속도(Maximum Braking Acceleration), 선회 반경, 상승 및 하강률, 액추에이터 한계, 페이로드 중량, 바람, 추진 시스템 상태에 따라 경계를 회피하는 데 필요한 거리가 결정된다. 대형 화물 UAV는 운동 에너지와 응답 시간이 더 크기 때문에 소형 멀티로터보다 훨씬 큰 운용 영역 유지 여유가 필요할 수 있다.

명령된 움직임이 경계에서 멀어지는 방향이더라도 바람은 상당한 운용 영역 이탈 위험을 발생시킬 수 있다. 시스템은 예상 외란에 대응하기에 사용 가능한 추력과 제어 권한(Control Authority)이 충분한지를 평가해야 한다. 운용 회랑의 가장자리에서 강한 측풍(Crosswind)이 발생하면 정상 경로 자체가 안전하지 않을 수 있다. 유도 시스템은 충분한 외란 억제 여유(Disturbance-Rejection Margin)를 유지하기 위해 계획된 경로를 내부로 이동시키거나 속도를 감소시킬 수 있다.

지오펜스 강제 적용은 고장 허용 제어(Fault-Tolerant Control)와 지속적으로 연계되어야 한다. 추진 시스템 고장, 배터리 성능 저하, 센서 고장 또는 액추에이터 제한은 기존에 실행 가능했던 경계를 준수할 수 있는 기체 능력을 감소시킬 수 있다. 제어 능력이 변경되면 운용 영역 유지 시스템은 도달 가능 집합(Reachable Set)과 안전 여유를 갱신해야 한다. 정상 상태에서는 쉽게 회피할 수 있었던 경계도 추력이나 기동 제어 권한을 상실한 이후에는 심각한 위험 요소가 될 수 있다.

GNSS 성능 저하(GNSS Degradation)는 많은 지오펜스가 지리 좌표(Geographic Coordinate)로 정의되기 때문에 특별한 문제를 발생시킨다. GNSS를 신뢰할 수 없게 되더라도 시스템은 운용 영역 유지 기능을 단순히 비활성화해서는 안 된다. 시각-관성 항법(Visual-Inertial Navigation), 지형 상대 위치 추정(Terrain-Relative Localization), 지도 정합(Map Matching) 또는 기타 대체 항법 정보원을 이용하여 로컬 위치 추정을 유지할 수 있다. 동시에 위치 불확실성이 증가하면 더 보수적인 안전 여유를 적용하고, 운용 영역 유지를 더 이상 보장할 수 없게 되기 전에 착륙을 시작해야 할 수 있다.

지오펜스 동작은 경계 접근(Approaching Boundary)과 실제 경계 위반(Actual Violation)을 구분해야 한다. 접근 상태에서는 경고, 궤적 수정, 속도 감소 또는 승인된 운용 공간 중심 방향으로의 복귀를 시작할 수 있다. 경계 위반이 확인되면 즉각적인 재진입, 임무 종료, 우회 또는 착륙과 같은 더 강력한 조치가 필요할 수 있다. 선택된 대응은 경계 이탈 자체보다 더 큰 위험을 발생시키는 과도한 기동을 피해야 한다.

여러 제약조건은 서로 중첩되거나 충돌할 수 있다. 자동 복귀(Return-to-Home) 궤적이 진입 금지 구역(Keep-Out Zone)을 통과할 수 있으며, 비상 착륙 지점이 정상적인 내부 유지 공간(Keep-In Volume) 외부에 존재할 수도 있다. 따라서 제약조건 관리(Constraint Management)에는 안전 분석을 통해 명시적으로 정의된 우선순위와 예외 규칙(Exception Rule)이 필요하다. 비상 상황에서 어떤 제한을 위반할 것인지를 즉석에서 결정해서는 안 되며, 아키텍처가 서로 경쟁하는 안전 목표를 해결하는 방법을 사전에 정의해야 한다.

시간 의존 제약조건(Time-Dependent Constraint)은 비행 공간 강제 적용에 또 다른 차원을 추가한다. 특정 비행 회랑은 정해진 임무 시간 동안에만 허가될 수 있으며, 임시 제한 공역(Temporary Restricted Airspace)은 예정된 시각에 활성화될 수 있다. UAV는 궤적을 계획할 때 향후 제약조건 활성화를 고려하여 제약조건이 변경되기 전에 빠져나올 수 없는 영역으로 진입하지 않도록 해야 한다. 따라서 신뢰성 있는 기체 내장 시간(Onboard Time)은 운용 영역 무결성(Containment Integrity)의 일부가 된다.

동적 업데이트(Dynamic Update)에는 버전 정보(Version Information), 타임스탬프, 유효 기간, 송신원 식별(Source Identification), 무결성 검사가 포함되어야 한다. 항공기는 형식이 잘못되거나 오래되었거나 승인되지 않았거나 내부적으로 일관되지 않은 제약조건 데이터를 거부해야 한다. 통신 중단으로 업데이트가 손실되면 기체 내장 시스템은 운용 영역 보호 기능을 임의로 제거하는 대신 사전에 정의된 유효성 규칙에 따라 마지막으로 검증된 안전 데이터셋(Safety Dataset)을 계속 사용해야 한다.

지형 및 장애물 제약조건(Terrain and Obstacle Constraint)은 규제 지오펜스(Regulatory Geofence)를 보완할 수 있다. 최소 지형 이격면(Minimum Terrain-Clearance Surface)은 상승하는 지형으로 궤적이 하강하는 것을 방지할 수 있으며, 3차원 제외 공간은 타워, 건물, 크레인, 전력 인프라 또는 중요 시설을 보호할 수 있다. 이러한 제약조건은 서로 다른 발생 원인, 우선순위, 불확실성 정보를 유지하면서 동일한 평가 프레임워크(Evaluation Framework)에 통합할 수 있다.

화물 UAV의 착륙 및 적재 구역에서는 특수한 로컬 비행 공간(Local Volume)이 필요할 수 있다. 접근 회랑(Approach Corridor), 호버링 영역(Hover Box), 접지 영역(Touchdown Area), 페이로드 전달 영역(Payload-Transfer Region)은 각각 서로 다른 속도, 고도, 횡방향 위치 한계를 가질 수 있다. 항공기가 사람, 장비 또는 인프라에 가까워질수록 제약조건을 점진적으로 강화하여 순항 항법(En-Route Navigation)에서 정밀 종말 운용(Precision Terminal Operation)으로 제어된 전환을 구현할 수 있다.

운용 영역 유지 기능은 경계 데이터 자체의 불확실성(Boundary Uncertainty)도 고려해야 한다. 지형 데이터베이스, 측량된 인프라 좌표, 임시 건설 정보, 외부에서 제공되는 공역 데이터에는 오차가 포함될 수 있다. 따라서 안전 여유는 항공기 위치 추정 불확실성과 제약조건 데이터의 불확실성을 모두 반영해야 한다. 모든 경계 좌표를 수학적으로 완벽하게 정확한 값으로 취급하면 실제 분리 거리(Actual Separation)를 비현실적으로 평가할 수 있다.

로깅(Logging)은 규정 준수를 입증하고 경계 근접 사건을 분석하는 데 필수적이다. 기록에는 활성 지오펜스 버전, 항공기 위치, 항법 불확실성, 예측 궤적(Predicted Trajectory), 안전 여유 계산, 제약조건 경고, 명령 수정, 개입 수준(Intervention Level), 경계 위반 정보가 포함되어야 한다. 시간 동기화(Time Synchronization)를 통해 조사자는 사건이 항법 오류, 계획 동작, 제어 성능 또는 잘못된 제약조건 데이터 중 어디에서 발생했는지를 재구성할 수 있다.

검증(Verification)은 정상 조건과 비정상 조건 전체에서 시스템을 평가해야 한다. 소프트웨어 인 더 루프(SIL, Software-in-the-Loop) 시험은 서로 다른 속도, 접근 각도, 바람, 항법 오차, 기체 고장 조건에서 경계에 접근하는 수천 가지 상황을 생성할 수 있다. 하드웨어 인 더 루프(HIL, Hardware-in-the-Loop) 시험에서는 지연된 항법 정보, 손상된 지오펜스 업데이트, 통신 상실, 센서 성능 저하, 액추에이터 제한 등을 주입하면서 기체 내장 운용 영역 유지 기능이 결정론적으로 유지되는지를 확인할 수 있다.

비행 시험(Flight Testing)은 충분히 큰 안전 버퍼에서 시작하여 통제된 감독 환경에서 실제 운용을 대표하는 경계를 단계적으로 평가해야 한다. 시험에서는 최소 분리 거리(Minimum Separation), 예측 정확도, 개입 시점, 제동 거리, 궤적 평활성(Trajectory Smoothness), 링크 상실 또는 항법 성능 저하 상황에서의 동작을 측정할 수 있다. 검증은 의도된 운용 영역(Intended Operating Envelope) 전체에서 실제 경계 위반이 발생하기 전에 보호 동작이 수행된다는 것을 입증해야 한다.

견고한 지오펜스 및 비행 공간 제약 아키텍처(Robust Geofence and Volume-Constraint Architecture)는 검증된 공간 데이터, 3차원 경계, 항법 무결성, 불확실성을 고려한 안전 여유(Uncertainty-Aware Margin), 예측 궤적 검사, 명령 제한, 동적 실행 가능성 분석(Dynamic Feasibility Analysis), 계층적 개입을 통합한다. 핵심 목표는 UAV가 가상의 경계를 넘어갔다는 사실을 단순히 탐지하는 것이 아니라, 안전하지 않은 경계 위반이 발생하기 전에 이를 예방할 수 있을 만큼 충분한 상황 인식과 제어 권한을 지속적으로 유지하는 것이다.

## 08.08. Safety Monitor Watchdog and Heartbeat Design [w/Code]

![](images/image8.png){width="7.268055555555556in" height="7.268055555555556in"}

UAV 안전 모니터(Safety Monitor)는 감시 대상 소프트웨어에 전적으로 의존하지 않으면서 비행 필수 컴퓨터(Flight-Critical Computer), 센서, 통신 인터페이스, 전력 시스템, 제어 기능을 관찰하는 독립적인 감독 계층(Independent Supervisory Layer)을 제공한다. 워치독(Watchdog)과 하트비트(Heartbeat) 메커니즘은 실행 정지, 데드라인 미준수, 통신 상실, 비정상적인 상태 전환 및 정상적인 기능 출력만으로는 확인하기 어려운 기타 고장을 탐지하는 이 계층의 핵심 요소이다.

워치독(Watchdog)은 기본적으로 시간 기반 보호 메커니즘(Timing-Based Protection Mechanism)이다. 감시 대상 프로세서 또는 소프트웨어 태스크는 예상된 시간 간격 내에서 정상적으로 실행되고 있음을 주기적으로 증명해야 한다. 요구되는 서비스 신호가 타임아웃(Timeout)이 만료되기 전에 도착하지 않으면 워치독은 실행이 정지되었거나 신뢰할 수 없는 상태가 되었다고 판단한다. 그 결과 고장 선언, 프로세서 리셋, 채널 격리(Channel Isolation), 중복 채널 전환(Redundancy Switchover) 또는 사전에 정의된 안전 모드(Safe Mode)로의 전환을 수행할 수 있다.

단순한 워치독 설계는 소프트웨어의 완전한 정지를 탐지할 수 있지만 애플리케이션이 올바르게 동작하고 있다는 사실까지 보장할 수는 없다. 결함이 있는 프로그램이 잘못된 제어 출력을 생성하면서도 계속 워치독 서비스를 수행할 수 있기 때문이다. 따라서 안전 중심 설계에서는 실행 시간 감시(Execution Timing Supervision)를 상태 타당성 검사(State Plausibility Check), 제어 명령 검증(Control-Command Validation), 센서 일관성 모니터링(Sensor Consistency Monitoring), 채널 간 비교(Cross-Channel Comparison)와 결합하여 비활성 고장과 잘못된 출력을 지속적으로 생성하는 능동적 고장을 모두 탐지해야 한다.

독립적인 하드웨어 워치독(Independent Hardware Watchdog)은 감시 대상 프로세서 내부에만 구현된 워치독 기능보다 강력한 보호를 제공한다. 프로세서에서 스케줄러 고장, 메모리 손상, 클록 고장(Clock Malfunction), 소프트웨어 교착 상태(Software Deadlock)가 발생하면 내부 소프트웨어 워치독도 감시 대상 애플리케이션과 함께 실패할 수 있다. 독립적인 전원, 타이밍, 리셋 권한(Reset Authority)을 갖는 외부 장치는 이러한 공통 고장을 탐지하고 대응할 수 있는 능력을 유지할 수 있다.

워치독 타임아웃(Watchdog Timeout)은 감시 대상 기능의 동역학을 반영하여 설정해야 한다. 지나치게 짧은 타임아웃은 정상적인 스케줄링 지터(Scheduling Jitter)나 일시적인 계산 부하 때문에 불필요한 리셋을 발생시킬 수 있으며, 지나치게 긴 타임아웃은 위험한 고장이 장시간 지속되도록 만들 수 있다. 따라서 비행 제어 루프, 항법 추정기(Navigation Estimator), 임무 관리자(Mission Manager), 통신 프로세스, 페이로드 애플리케이션은 안전 중요도와 실행 주기에 따라 서로 다른 감시 시간 간격을 요구할 수 있다.

윈도우형 워치독(Windowed Watchdog)은 단순히 최대 타임아웃 이전에 서비스 신호가 발생하도록 요구하는 대신 정의된 시간 창(Time Window) 내에서 서비스 이벤트가 발생하도록 요구함으로써 시간 감시를 강화한다. 이를 통해 태스크가 지나치게 빠르게, 지나치게 자주 또는 지나치게 늦게 응답하는 비정상 상태도 식별할 수 있다. 이러한 방식은 소프트웨어가 의도된 처리 순서를 완료하지 않은 상태에서 워치독 서비스 명령만 반복적으로 실행하는 손상된 제어 흐름(Corrupted Control Flow)을 탐지하는 데 도움이 된다.

하트비트 메커니즘(Heartbeat Mechanism)은 프로세서, 소프트웨어 서비스, 센서, 네트워크 노드, 지상 통신 시스템까지 감시 범위를 확장한다. 각 감시 대상 구성요소는 자신이 계속 활성 상태임을 나타내는 간결한 메시지를 주기적으로 전송한다. 하트비트에는 단순한 활성 플래그(Alive Flag)뿐만 아니라 시퀀스 카운터(Sequence Counter), 운용 모드, 상태 정보, 실행 주기 번호(Execution Cycle Number), 소프트웨어 버전, 타임스탬프(Timestamp), 선택된 진단 정보를 포함할 수 있다.

시퀀스 카운터는 동일한 하트비트를 반복적으로 수신하는 것을 정상적인 지속 동작의 증거로 잘못 판단하지 않도록 하기 때문에 중요하다. 고정된 통신 버퍼(Frozen Communication Buffer) 또는 반복 전송되는 네트워크 패킷 때문에 고장난 구성요소가 정상적으로 보일 수 있다. 모니터는 시퀀스 값이 올바르게 증가하고 메시지가 예상된 시간 한계 내에서 도착하는지 확인하는 동시에 중복 메시지, 누락 메시지, 예상하지 못한 리셋을 탐지해야 한다.

타임스탬프는 데이터 최신성(Data Freshness)에 대한 추가적인 증거를 제공한다. 하트비트가 정상적인 네트워크를 통해 도착하더라도 해당 메시지를 생성하는 애플리케이션의 내부 상태 업데이트가 중단되었을 수 있다. 송신원 타임스탬프(Source Timestamp)와 로컬 수신 시간을 비교하면 오래된 정보와 과도한 전송 지연(Transport Latency)을 탐지할 수 있다. 여러 컴퓨팅 및 센서 노드를 사용하는 분산형 UAV 아키텍처에서는 신뢰성 있는 시간 동기화(Time Synchronization)가 이러한 기능을 더욱 향상시킨다.

하트비트 모니터링(Heartbeat Monitoring)은 구성요소 고장과 통신 경로 고장을 구분해야 한다. 서로 관련이 없는 여러 장치의 메시지가 동시에 사라지는 경우 개별 장치가 모두 고장난 것이 아니라 네트워크 스위치, 버스, 전력 영역(Power Domain), 통신 게이트웨이(Communication Gateway)가 공통 원인일 수 있다. 따라서 안전 모니터는 공유 인프라를 고려하지 않은 채 누락된 모든 노드를 개별 고장으로 선언하기보다 시스템 토폴로지(System Topology)와 고장 영역(Failure Domain)을 이해해야 한다.

계층적 모니터링(Hierarchical Monitoring)은 대규모 UAV 시스템에서 확장성(Scalability)을 향상시킬 수 있다. 로컬 감독기(Local Supervisor)는 개별 컴퓨팅 노드 내부의 고속 태스크와 하드웨어 인터페이스를 감시하고, 상위 수준 안전 관리자(Higher-Level Safety Manager)는 노드 상태, 통신 상태, 항법 무결성(Navigation Integrity), 추진 능력, 임무 상태를 감시할 수 있다. 이러한 구성은 통신 오버헤드를 감소시키면서 신속한 로컬 고장 탐지와 항공기 수준의 통합 대응을 유지한다.

안전 모니터는 주 임무 소프트웨어(Primary Mission Software)로부터 충분한 독립성을 가져야 한다. 복잡한 자율 시스템 스택(Autonomous Stack)은 인지(Perception), 계획(Planning), 매핑(Mapping), AI 추론(AI Inference), 임무 로직을 포함할 수 있으며 높은 계산 복잡도를 갖는다. 안전 모니터는 보다 단순하고 실행 범위가 제한되며 높은 검증 가능성을 갖는 로직을 이용하여 자세, 고도, 속도, 위치, 지오펜스 상태, 배터리 상태, 제어 명령, 프로세서 상태와 같은 핵심 변수를 감시할 수 있다.

명령 모니터링(Command Monitoring)을 통해 안전 계층은 계산상 유효하지만 물리적으로 안전하지 않은 출력을 탐지할 수 있다. 비행 제어 또는 자율 프로세스가 정상적으로 활성 상태를 유지하고 하트비트를 계속 전송하면서도 과도한 뱅크각(Bank Angle), 추력, 하강률 또는 속도를 요구할 수 있다. 독립적인 한계 검사(Independent Limit Checking)는 검증된 항공기 운용 영역을 초과하는 명령을 거부하거나 제한하거나 재정의(Override)하여 체계적인 소프트웨어 고장(Systematic Software Failure)에 대한 보호 기능을 제공한다.

상태 타당성 모니터링(State Plausibility Monitoring)은 측정되고 추정된 항공기 상태에 물리적 및 시간적 제약조건을 적용한다. 일반적으로 한 번의 제어 주기 내에서 위치가 수 킬로미터 이동할 수 없으며, 고출력 비행 중 배터리 충전량이 예상하지 못하게 증가해서도 안 되고, 액추에이터 피드백은 명령된 움직임과 일관되어야 한다. 이러한 규칙을 통해 모든 태스크가 계속 실행되고 있는 경우에도 메모리 손상, 센서 고장, 통신 오류 또는 추정기 발산(Estimator Divergence)을 식별할 수 있다.

중복 비행 제어 컴퓨터(FCC, Flight Control Computer) 사이의 상호 감시(Cross-Monitoring)는 또 다른 고장 탐지 계층을 제공한다. 각 FCC는 상대 채널의 모드, 추정 상태, 제어 출력, 실행 카운터, 상태 선언을 비교할 수 있다. 별도의 안전 모니터는 채널 사이에 불일치가 발생할 때 이를 중재하여 두 컴퓨터가 불일치를 탐지했지만 어느 채널이 고장났는지 판단하지 못하는 위험을 줄일 수 있다. 새로운 단일 고장점(Single Point of Failure)이 발생하지 않도록 중재기의 독립성을 확보하는 것이 중요하다.

고장 대응(Fault Response)은 심각도(Severity)와 탐지 신뢰도(Detection Confidence)에 비례해야 한다. 한 번의 하트비트 누락은 경고 또는 일시적인 성능 저하 상태를 적용하는 것으로 충분할 수 있지만, 반복적인 메시지 누락이나 외부 워치독 타임아웃은 채널 격리를 정당화할 수 있다. 물리적으로 불가능한 제어 명령은 지속성 지연(Persistence Delay) 없이 즉각적인 개입이 필요할 수 있다. 따라서 안전 아키텍처는 일시적인 이상 상태와 신속한 보호 동작이 필요한 실제 고장을 구분해야 한다.

지속성 카운터(Persistence Counter)와 히스테리시스(Hysteresis)는 불안정한 고장 선언을 방지하는 데 도움이 된다. 일시적인 프로세서 과부하, 네트워크 혼잡, 스케줄링 지터 때문에 영구적인 고장이 없더라도 하트비트가 가끔 지연될 수 있다. 정의된 횟수의 연속 누락이 발생한 이후 고장을 선언하도록 하면 불필요한 상태 전환을 감소시킬 수 있다. 그러나 결과가 심각한 기능은 더 짧은 지속 시간 또는 즉각적인 대응이 필요할 수 있으므로 시간 정책은 위험 분석(Hazard Analysis)에 따라 지정해야 한다.

워치독 이벤트 이후의 복구(Recovery)는 신중하게 제어해야 한다. 고장난 프로세서를 자동으로 재부팅하면 기능을 복원할 수 있지만 반복적인 리셋은 불안정한 항공기 동작을 발생시키거나 지속적인 하드웨어 결함을 숨길 수 있다. 시스템은 재시작 횟수를 제한하고, 리셋 원인(Reset Cause)을 기록하고, 초기화 상태를 검증하며, 정상 채널과 상태를 동기화한 후 복구된 구성요소에 제어 권한을 다시 부여하기 전에 준비 상태를 확인해야 한다.

재시작된 비행 제어 컴퓨터는 초기화되지 않았거나 오래된 상태 정보를 사용하여 즉시 액추에이터 명령을 출력해서는 안 된다. 항법 추정값, 제어 적분기(Control Integrator), 임무 모드, 기준 궤적(Reference Trajectory), 액추에이터 명령 이력은 시스템에 다시 통합되기 전에 동기화가 필요할 수 있다. 웜 스타트 전략(Warm-Start Strategy)은 정상 채널에서 검증된 상태를 전달받을 수 있으며, 콜드 스타트 전략(Cold-Start Strategy)은 복구된 컴퓨터가 제어 권한을 획득하기 전에 더 긴 관찰 시간이 필요할 수 있다.

워치독 아키텍처는 전원 고장(Power Failure)도 고려해야 한다. 감시 대상 프로세서와 동일한 고장 전력 레일에서 전원을 공급받는 모니터는 해당 전원 상실을 보고하거나 복구할 수 없다. 따라서 안전 필수 감시 기능은 독립적이거나 보호된 전력 영역을 사용하고 전압, 리셋 라인(Reset Line), 클록 동작, 전원 정상 신호(Power-Good Signal)를 감시할 수 있다. 이를 통해 시스템은 소프트웨어 고장과 전기적 전원 상실을 구분하고 적절한 대응을 선택할 수 있다.

네트워크 워치독(Network Watchdog)은 개별 애플리케이션이 아니라 통신 인프라를 감시한다. 버스 사용률(Bus Utilization), 메시지 타이밍, 오류 카운터, 게이트웨이 상태, 스위치 상태, 통신 분할(Communication Partition)을 감시하여 네트워크 수준의 성능 저하를 탐지할 수 있다. 안전 필수 메시지는 제한된 전송 데드라인(Bounded Transmission Deadline)을 가질 수 있으며, 이를 충족하지 못하면 중복 버스로 재구성하거나 단순화된 로컬 제어 모드(Local-Control Mode)로 전환할 수 있다.

센서 하트비트 감시(Sensor Heartbeat Supervision)는 단순히 메시지가 존재하는지만 확인하는 것이 아니라 데이터 유효성(Data Validity)까지 포함해야 한다. 관성측정장치(IMU)는 패킷을 계속 전송하면서도 고정되거나 포화되거나 편향된 측정값을 생성할 수 있다. 모니터는 업데이트 카운터, 측정 분산(Measurement Variance), 물리적 범위, 센서 간 일관성, 보정 상태(Calibration Status), 내장 시험(Built-In Test) 결과를 평가해야 한다. 통신과 측정 동작이 모두 신뢰할 수 있는 경우에만 센서를 정상 상태로 판단해야 한다.

안전 모니터 출력은 항공기의 현재 상태 구성(Health Configuration)을 유지하는 중앙집중형 또는 분산형 고장 관리 기능(Fault-Management Function)에 전달되어야 한다. 이러한 구성 정보는 어떤 컴퓨터, 센서, 통신 링크, 전원, 액추에이터가 여전히 신뢰할 수 있는지를 식별한다. 이후 유도 및 제어 기능은 각각 독립적으로 시스템 상태를 반복해서 판단하는 대신 사용 가능한 자원에 맞추어 동작을 조정할 수 있다.

워치독 및 하트비트 이벤트의 로깅(Logging)은 검증과 유지보수에 필수적이다. 기록에는 타임아웃 값, 누락된 시퀀스 번호, 리셋 원인, 프로세서 상태, 네트워크 조건, 모드 전환, 고장 선언, 복구 시도, 동기화 결과가 포함되어야 한다. 정확한 타임스탬프를 사용하면 엔지니어가 사건의 발생 순서를 재구성하고 최초 고장(Initiating Failure)과 이후 발생한 2차 영향을 구분할 수 있다.

소프트웨어 인 더 루프(SIL, Software-in-the-Loop) 시험에서는 태스크 정지, 교착 상태, 실행 시간 초과(Timing Overrun), 잘못된 상태, 고정된 카운터, 잘못된 명령 등을 주입하여 모니터링 로직을 검증할 수 있다. 하드웨어 인 더 루프(HIL, Hardware-in-the-Loop) 시험에서는 프로세서 리셋, 네트워크 중단, 클록 고장, 전원 장애, 센서 데이터 고정, 지연된 메시지를 추가할 수 있다. 검증에서는 탐지 지연(Detection Latency), 고장 분류, 보호 대응, 성공적인 복구 또는 복구 거부가 정확하게 수행되는지를 확인해야 한다.

스트레스 시험(Stress Testing)은 프로세서가 계산 성능 한계에 접근할 때 워치독 동작이 달라질 수 있기 때문에 특히 중요하다. 높은 인지 처리 부하(Perception Workload), 집중적인 로깅, 통신 혼잡, 여러 고장의 동시 처리로 인해 스케줄링 지연이 증가할 수 있다. 시험을 통해 최악의 신뢰 가능한 부하(Worst-Case Credible Load)에서도 안전 필수 감시 기능이 시간 보장을 유지하고, 비필수 작업이 워치독 또는 하트비트 기능의 실행을 방해하지 않는다는 것을 입증해야 한다.

견고한 안전 모니터 아키텍처(Robust Safety-Monitor Architecture)는 독립 워치독, 정보가 풍부한 하트비트(Information-Rich Heartbeat), 시간 감시, 시퀀스 및 최신성 검사(Freshness Check), 물리적 타당성 모니터링, 명령 검증, 시스템 토폴로지를 고려한 고장 격리(Topology-Aware Fault Isolation), 제어된 복구(Controlled Recovery)를 통합한다. 그 목적은 단순히 소프트웨어가 실행되고 있는지를 판단하는 것이 아니라, UAV의 핵심 기능이 적시에 동작하고 신뢰할 수 있으며 서로 조정된 상태를 유지하고 제어 권한을 계속 보유할 만큼 안전하다는 지속적인 증거를 확보하는 것이다.

## 08.09. Software Safety Case DO 178C DAL A B [w/Code]

![](images/image9.png){width="7.268055555555556in" height="7.268055555555556in"}

안전 필수 UAV를 위한 소프트웨어 안전 사례(Software Safety Case)는 항공 소프트웨어가 의도된 기능을 허용 가능한 보증 수준으로 수행하고 소프트웨어 고장이 체계적으로 다루어졌음을 입증하는 구조화된 증거를 제공한다. DO-178C는 소프트웨어 수명주기 보증(Software Life-Cycle Assurance)을 위한 주요 프레임워크를 제공하며, 할당된 설계 보증 수준(DAL, Design Assurance Level)은 계획, 개발, 검증, 형상 관리(Configuration Management), 인증 증거에 적용되는 엄격성의 수준을 결정한다.

소프트웨어 보증 프로세스(Software Assurance Process)는 소스 코드(Source Code)가 아니라 항공기 및 시스템 안전성 평가(Aircraft and System Safety Assessment)에서 시작된다. 기능 위험 평가(FHA, Functional Hazard Assessment)는 기능 상실, 오동작, 오도성 출력(Misleading Output), 의도하지 않은 활성화가 초래하는 결과를 식별한다. 이후 시스템 수준 분석은 안전 요구사항을 하드웨어 및 소프트웨어 구성요소에 할당하며, 그 결과 도출된 고장 분류(Failure Classification)가 필요한 소프트웨어 설계 보증 수준을 결정하는 기준이 된다.

DAL A는 비정상적인 소프트웨어 동작이 치명적 고장 상태(Catastrophic Failure Condition)에 기여할 수 있는 경우 적용되며, DAL B는 해당 인증 프레임워크에서 위험 또는 심각-중대 고장 결과(Hazardous or Severe-Major Consequence)와 연관된다. DAL A는 가장 높은 수준의 소프트웨어 보증을 요구하므로 이러한 구분은 중요하다. 두 수준 모두 엄격한 엔지니어링을 요구하지만 DAL A에는 추가적인 검증 목표와 더욱 강한 독립성 요구사항(Independence Expectation)이 적용된다.

화물 UAV(Cargo UAV)의 DAL 할당은 소프트웨어의 복잡성이나 규모가 아니라 각 기능이 실제로 초래할 수 있는 안전 결과를 기준으로 해야 한다. 비행 안정화(Flight Stabilization), 추진 명령(Propulsion Command), 비행 영역 보호(Flight-Envelope Protection), 핵심 항법 기능은 고장으로 인해 항공기 상실이나 허용할 수 없는 지상 위험이 발생할 수 있다면 높은 보증 수준이 필요할 수 있다. 반면 적절하게 격리된 중요도가 낮은 임무 관리 또는 페이로드 기능은 더 낮은 보증 수준을 적용할 수 있다.

안전 사례는 식별된 위험요소(Hazard)에서 시스템 안전 요구사항(System Safety Requirement), 그리고 소프트웨어 요구사항(Software Requirement)까지 이어지는 추적성(Traceability)을 확립해야 한다. 각 소프트웨어 요구사항은 명확한 출처와 검증 방법을 가져야 하며, 안전과 관련된 모든 상위 수준 요구사항은 완전하게 구현되어야 한다. 양방향 추적성(Bidirectional Traceability)을 통해 누락된 구현, 의도하지 않은 기능, 불완전한 시험, 더 이상 유효한 시스템 근거를 갖지 않는 요구사항을 탐지할 수 있다.

계획 수립(Planning)은 개발 증거가 생성되기 전에 규정 준수를 달성하는 방법을 정의한다. 일반적인 수명주기 계획은 소프트웨어 개발, 검증, 형상 관리, 품질 보증(Quality Assurance), 인증 기관 협의(Certification Liaison), 표준, 도구, 개발 환경, 단계 전환 기준(Transition Criteria)을 정의한다. 계획은 인증을 구현 이후 추가하는 문서 작업으로 취급하는 대신 DAL별 목표를 어떻게 충족하고 필요한 경우 독립성을 어떻게 유지할 것인지를 설명해야 한다.

상위 수준 소프트웨어 요구사항(High-Level Software Requirement)은 외부적으로 의미 있는 동작, 인터페이스, 모드, 시간 제약조건, 고장 대응, 안전 보호 기능을 정의한다. 이러한 요구사항은 정확하고 일관되며 검증 가능하고 시스템 요구사항과 호환되어야 한다. 비행 제어 기능의 경우 불필요한 구현 세부사항을 조기에 포함하지 않으면서 제어 모드, 센서 유효 조건, 액추에이터 명령 한계, 성능 저하 상태 전환(Degraded-State Transition), 시간 데드라인(Timing Deadline)을 정의할 수 있다.

하위 수준 요구사항(Low-Level Requirement)과 소프트웨어 아키텍처(Software Architecture)는 상위 수준 동작을 구현 가능한 로직으로 구체화한다. 알고리즘, 상태 머신(State Machine), 데이터 변환, 스케줄링 동작, 인터페이스 처리, 모니터링 로직, 내부 제어 경로를 정의할 수 있다. DAL A 및 DAL B 소프트웨어에서는 이러한 세부 요구사항이 상위 수준의 원천 요구사항을 정확하게 충족하며 의도하지 않은 안전 관련 동작을 추가하지 않는다는 것을 검증을 통해 입증해야 한다.

중복 UAV 시스템(Redundant UAV System)에서는 소프트웨어 아키텍처에 특별한 주의가 필요하다. 비행 필수 제어, 자율 기능, 통신, 페이로드, 유지보수 기능 사이의 파티셔닝(Partitioning)은 낮은 보증 수준의 소프트웨어가 높은 보증 수준의 기능을 손상시키지 못하도록 해야 한다. 메모리 보호(Memory Protection), 프로세서 파티셔닝, 통신 게이트웨이, 스케줄링 분리, 통제된 인터페이스를 통해 독립성을 지원할 수 있지만 그 효과는 단순히 가정하는 것이 아니라 실제로 입증해야 한다.

소스 코드는 승인된 코딩 표준(Coding Standard)을 준수하고 하위 수준 요구사항과 아키텍처를 정확하게 구현해야 한다. 코딩 표준은 모호한 언어 기능, 통제되지 않는 동적 동작, 안전하지 않은 메모리 사용, 재귀(Recursion), 숨겨진 부작용(Hidden Side Effect), 기타 검증을 어렵게 만드는 구조를 제한할 수 있다. 준수 여부는 검토와 분석을 통해 입증되어야 하며, 예외 사항은 비공식적으로 허용하는 대신 명시적으로 정당화하고 통제해야 한다.

검증(Verification)은 단순한 시험 실행에 한정되지 않는다. 검토(Review), 분석, 요구사항 기반 시험(Requirements-Based Testing), 인터페이스 시험, 강건성 시험(Robustness Testing), 구조적 커버리지 분석(Structural Coverage Analysis), 추적성 평가를 종합적으로 활용하여 보증 근거를 형성한다. 목표는 소프트웨어가 명시된 동작을 충족한다는 것을 입증하는 동시에 의도하지 않은 구현을 발견하는 것이다. 따라서 검증 증거는 소프트웨어가 수행해야 하는 기능과 실제 실행 구조에 포함된 내용을 모두 다루어야 한다.

요구사항 기반 시험은 구현에 대한 지식만을 기준으로 하는 것이 아니라 소프트웨어 요구사항으로부터 시험 사례(Test Case)를 도출한다. 시험은 정상 운용 조건, 경계값, 유효하지 않은 입력, 모드 전환, 시간 조건, 고장 대응을 포함해야 한다. 안전 필수 UAV 소프트웨어에서는 센서 고장, 통신 상실, 프로세서 고장, 액추에이터 제한, 일관되지 않은 시스템 상태와 관련된 고장 주입(Fault Injection) 시나리오도 유용하다.

강건성 시험은 정상 입력 범위를 벗어난 조건에서의 동작을 평가한다. 소프트웨어는 범위를 벗어난 센서 값, 잘못 구성된 메시지(Malformed Message), 오래된 데이터(Stale Data), 사용할 수 없는 자원, 예상하지 못한 모드 요청, 시간 이상에 대해 예측 가능한 방식으로 대응해야 한다. 강건한 구현이 모든 비정상 조건에서 임무를 계속해야 한다는 의미는 아니며, 제어되지 않은 동작을 발생시키지 않고 정의된 성능 저하 또는 안전 상태(Safe State)로 전환해야 한다는 의미이다.

구조적 커버리지 분석은 요구사항 기반 시험이 구현된 소프트웨어 구조를 요구되는 보증 수준까지 실행했는지를 판단한다. 명령문 커버리지(Statement Coverage)와 결정 커버리지(Decision Coverage)는 기본적인 구조적 실행 증거를 제공하며, DAL A에서는 수정 조건/결정 커버리지(MC/DC, Modified Condition/Decision Coverage)가 요구된다. MC/DC는 적절한 시험 조합에서 각각의 개별 불리언 조건(Boolean Condition)이 결정 결과에 독립적으로 영향을 줄 수 있음을 입증한다.

구조적 커버리지는 요구사항 기반 시험을 대체하지 않는다. 그 목적 중 하나는 기존 요구사항 기반 시험에서 실행되지 않은 코드를 발견하는 것이다. 커버되지 않은 코드는 시험 부족, 불완전한 요구사항, 방어적 로직(Defensive Logic), 비활성 코드(Deactivated Code) 또는 의도하지 않은 기능을 의미할 수 있다. 따라서 각 커버리지 공백(Coverage Gap)은 추가 시험, 요구사항 명확화, 정당화된 비활성화 또는 불필요한 코드 제거를 통해 분석하고 해결해야 한다.

DAL A 소프트웨어는 치명적 고장 상태에 대해 가장 높은 보증이 요구되므로 특히 강력한 증거가 필요하다. 지정된 검증 목표에는 검증 독립성(Verification Independence)이 적용되어 핵심 산출물을 검증하는 사람 또는 프로세스가 해당 산출물의 개발로부터 적절하게 독립되도록 한다. 독립성은 동일한 오해, 가정 또는 구현 오류가 개발과 검증 단계를 모두 통과하면서 발견되지 않을 가능성을 감소시킨다.

DAL B도 엄격한 검증과 통제된 수명주기 프로세스를 요구하지만 목표 집합과 독립성 요구사항은 DAL A와 차이가 있다. 엔지니어링 접근에서는 DAL B를 추가적인 시험만 적용하는 일반 상용 소프트웨어로 취급해서는 안 된다. 위험한 고장 상태는 여전히 상당한 수준의 보증을 요구하기 때문에 요구사항 품질, 추적성, 형상 통제(Configuration Control), 검증 증거, 문제 보고, 재현성(Reproducibility)이 핵심 요소로 유지된다.

데이터 결합 및 제어 결합 분석(Data Coupling and Control Coupling Analysis)은 소프트웨어 구성요소 사이의 상호작용을 검증하는 데 도움이 된다. 데이터 결합(Data Coupling)은 인터페이스를 통해 교환되는 정보를 분석하고, 제어 결합(Control Coupling)은 하나의 구성요소가 다른 구성요소의 실행 또는 동작에 미치는 영향을 분석한다. 분산형 UAV 아키텍처에서는 공유 데이터, 모드 명령, 네트워크 서비스 또는 스케줄링 관계를 통한 예상하지 못한 의존성을 발견하여 의도된 안전 분리를 훼손할 가능성을 확인할 수 있다.

비행 소프트웨어에서는 정확한 결과라도 너무 늦게 전달되면 위험할 수 있으므로 시간 동작(Timing Behavior)이 특히 중요하다. 검증에서는 실행 데드라인, 스케줄링 여유(Scheduling Margin), 통신 지연, 센서에서 액추에이터까지의 지연(Sensor-to-Actuator Delay), 최악 조건 실행 동작(Worst-Case Execution Behavior), 과부하 조건을 고려해야 한다. 인지 또는 AI 기능에서 발생하는 높은 계산 부하가 비행 필수 DAL A 또는 DAL B 소프트웨어의 요구 시간 보장을 방해해서는 안 된다.

형상 관리(Configuration Management)는 요구사항, 소스 코드, 실행 객체(Executable Object), 시험 절차, 시험 결과, 도구 버전, 컴파일러 설정, 지원 데이터가 통제된 식별 체계 아래 유지되도록 한다. 인증 증거는 항공기에 실제 설치되는 정확한 소프트웨어 형상과 일치해야 한다. 재현 가능한 빌드(Reproducible Build)와 통제된 베이스라인(Controlled Baseline)은 검증된 소프트웨어와 실제 운용 바이너리 사이의 차이를 방지하는 데 도움이 된다.

문제 보고(Problem Reporting)와 변경 통제(Change Control)는 소프트웨어 수명주기 전체에서 지속된다. 이상 현상(Anomaly)은 기록되고 안전 영향에 대해 평가되며 필요한 경우 수정되고 적절한 회귀 분석(Regression Analysis)을 받아야 한다. 겉보기에 작은 코드 변경도 요구사항, 실행 시간, 구조적 커버리지, 인터페이스 또는 기존에 검증된 동작에 영향을 줄 수 있다. 변경 영향 분석(Change Impact Analysis)을 통해 어떤 인증 증거를 다시 생성하거나 재검증해야 하는지를 결정한다.

도구 적격성(Tool Qualification)은 개발 또는 검증 도구가 오류를 유입하거나 오류 탐지에 실패할 수 있고, 해당 도구의 출력이 문제를 발견할 수 있는 후속 검증 없이 사용되는 경우 중요해진다. 컴파일러, 코드 생성기(Code Generator), 모델 기반 개발 환경(Model-Based Development Environment), 정적 분석기(Static Analyzer), 커버리지 도구, 자동 검증 시스템은 인증 과정에서 담당하는 역할과 인증 크레딧(Certification Credit)에 따라 적격성 검토가 필요할 수 있다.

모델 기반 개발(Model-Based Development)은 모델, 생성 코드, 검증 산출물, 도구 사용이 적절한 목표에 따라 관리되는 경우 DO-178C 보증 프로세스에 통합할 수 있다. DO-331과 같은 관련 지침은 모델 기반 개발 및 검증에 관한 고려사항을 다룬다. 모델을 사용한다고 해서 보증 의무가 제거되는 것은 아니며, 모델의 역할, 추적성, 검증, 생성된 산출물을 명확하게 통제해야 한다.

정형 기법(Formal Methods)은 DO-333과 같은 관련 지침에 따라 적절하게 적용되는 경우 일부 기존 검증 활동을 보완하거나 대체할 수 있다. 수학적 증명(Mathematical Proof)은 범위 제한, 상태 불변조건(State Invariant), 데이터 일관성, 제어 로직과 같은 속성에 대해 강력한 증거를 제공할 수 있다. 그러나 정형 검증도 올바른 가정과 요구사항에 의존하므로 전체 안전 논증(Safety Argument)과 형상 베이스라인에 연결되어야 한다.

객체지향 기술(Object-Oriented Technology) 및 관련 기법이 항공 소프트웨어에 사용되는 경우 DO-332와 같은 지침을 고려해야 할 수 있다. 상속(Inheritance), 동적 디스패치(Dynamic Dispatch), 다형성(Polymorphism), 복잡한 객체 관계는 검증과 구조적 분석에 영향을 줄 수 있다. 보증 전략은 이러한 메커니즘이 할당된 DAL에서 이해 가능하고, 범위가 제한되며, 검증 가능하게 유지된다는 것을 입증해야 한다.

소프트웨어 안전 사례는 복잡한 자율 기능 또는 인공지능 기능과의 파티셔닝 및 공존(Coexistence)도 다루어야 한다. 고성능 계획기(Planner)나 학습 모델(Learned Model)은 기존 비행 제어 커널과 동일한 보증 수준으로 개발되지 않을 수 있다. 검증된 모니터(Verified Monitor), 명령 허용 범위(Command Envelope), 런타임 보증(Runtime Assurance), 독립 대체 제어기(Independent Fallback Controller), 아키텍처 격리(Architectural Isolation)를 통해 이러한 구성요소를 제한함으로써 안전성을 강화할 수 있다.

인증 증거(Certification Evidence)는 서로 단절된 문서들의 집합이 아니라 일관된 논증(Coherent Argument)을 형성해야 한다. 안전성 평가는 왜 해당 보증이 필요한지를 설명하고, 계획은 보증을 어떻게 달성할 것인지를 설명하며, 요구사항과 설계는 의도된 동작을 정의한다. 구현은 그 동작을 실현하고, 검증은 요구사항 준수를 입증하며, 형상 기록은 정확히 어떤 대상이 평가되었는지를 확립한다. 추적성은 이러한 요소를 감사 가능한 연속적인 증거 사슬(Auditable Chain)로 연결한다.

최종 소프트웨어 완료 증거(Final Software Accomplishment Evidence)는 완료된 수명주기, 해결된 이상 현상, 달성된 목표, 형상 식별, 검증 결과, 잔여 제한사항을 종합한다. 릴리스 이전에 조직은 적용되는 DAL과 소프트웨어 베이스라인에 요구되는 검토, 분석, 시험, 구조적 커버리지 활동, 형상 감사(Configuration Audit), 품질 보증 기록, 인증 산출물이 모두 완료되었는지를 확인해야 한다.

따라서 안전 필수 UAV를 위한 DO-178C DAL A/B 보증은 개발 후반부에 단순히 적용하는 시험 요구사항이 아니다. 이는 위험 분석, 요구사항, 아키텍처, 구현, 검증, 독립성, 형상 관리, 인증 증거를 연결하는 체계적인 엔지니어링 프레임워크(Engineering Framework)이다. 핵심 목표는 핵심 소프트웨어의 동작이 충분히 이해되고 통제되며 검증되었고, 해당 소프트웨어가 초래할 수 있는 고장의 심각도에 적합한 수준의 보증을 갖추었다는 방어 가능한 증거 체계(Defensible Body of Evidence)를 구축하는 것이다.

## 08.10. Safety Validation Flight Test Protocol

![](images/image10.png){width="7.268055555555556in" height="7.268055555555556in"}

안전 검증 비행 시험(Safety Validation Flight Testing)은 UAV의 안전 아키텍처(Safety Architecture)가 실제 항공기 동역학, 환경 외란(Environmental Disturbance), 통신 조건, 항법 불확실성(Navigation Uncertainty), 서브시스템 성능 저하에 노출되었을 때 올바르게 동작한다는 최종적인 운용 증거를 제공한다. 이는 시뮬레이션, 소프트웨어 인 더 루프(SIL, Software-in-the-Loop), 하드웨어 인 더 루프(HIL, Hardware-in-the-Loop), 지상 시험을 보완하며 실제 비행 환경에서 통합된 시스템 동작을 평가한다.

비행 시험 프로토콜(Flight-Test Protocol)은 정상적인 임무 수행 능력을 시연하는 것보다 시스템 안전 요구사항(System Safety Requirement)을 기반으로 수립되어야 한다. 기능 위험 평가(FHA, Functional Hazard Assessment), 고장 형태 및 영향 분석(FMEA, Failure Mode and Effects Analysis), 결함 트리 분석(FTA, Fault Tree Analysis), 소프트웨어 보증 활동, 서브시스템 적격성 평가(Subsystem Qualification), 통합 시험을 통해 비행 증거가 필요한 조건을 식별한다. 각 시험 목표는 안전 요구사항, 식별된 위험요소(Hazard), 완화 메커니즘(Mitigation Mechanism), 운용 제한 또는 인증 목표까지 추적 가능해야 한다.

시험은 점진적인 위험 감소 전략(Progressive Risk-Reduction Strategy)을 따라야 한다. 초기 비행에서는 기본적인 조종 가능성, 추진 성능, 항법 안정성, 통신 신뢰성, 정상 자동화 기능을 확인한다. 이후 단계에서 통제된 외란, 성능이 저하된 센서, 통신 이상, 제한된 에너지 여유, 선택된 고장 조건을 단계적으로 도입한다. 사전에 요구되는 운용 영역 유지(Containment)와 복구 기능이 입증되기 전에 위험한 시나리오를 적용해서는 안 된다.

정식 시험 카드(Test Card)는 각 비행 시험 사례의 구성과 실행 방법을 정의한다. 시험 카드에는 항공기 하드웨어 및 소프트웨어 베이스라인(Baseline), 페이로드 상태, 배터리 구성, 환경 한계, 시험 구역, 필요한 인원, 초기 조건, 기동 순서, 예상 동작, 시험 종료 기준, 필요한 측정 항목을 명시해야 한다. 형상 통제(Configuration Control)를 통해 시험 결과를 실제 시험에 사용된 정확한 시스템 구성과 연결할 수 있어야 한다.

비행 전 준비 검토(Preflight Readiness Review)는 항공기와 시험 조직이 계획된 위험 수준에 대응할 준비가 되었는지를 검증한다. 엔지니어는 정비 상태, 소프트웨어 버전, 파라미터 파일(Parameter File), 센서 보정 상태, 배터리 상태, 구조적 건전성, 추진 시스템 상태, 통신 링크, 지오펜스 데이터, 비상 장비, 기상 한계, 데이터 기록 기능을 확인해야 한다. 해결되지 않은 이상 현상(Open Anomaly)은 비행 승인을 내리기 전에 검토되어야 한다.

시험 구역(Test Area)은 정상 궤적뿐만 아니라 현실적으로 발생 가능한 비정상 움직임(Off-Nominal Motion)까지 수용할 수 있는 충분한 운용 영역을 제공해야 한다. 경계 설정에서는 항공기 속도, 고도, 정지 거리, 항법 불확실성, 바람, 발생 가능한 제어 성능 저하, 비상 착륙 요구사항을 고려해야 한다. 위험도가 높은 시험에서는 독립적인 안전 감독(Independent Safety Supervision)과 함께 사람, 재산, 인프라, 관련 없는 항공 교통으로부터 추가적인 분리 거리를 확보해야 할 수 있다.

시험 역할(Test Role)과 명령 권한(Command Authority)은 명확하게 정의되어야 한다. 프로그램 규모에 따라 비행 시험팀은 원격 조종사(Remote Pilot), 시험 책임자(Test Conductor), 안전 조종사(Safety Pilot), 텔레메트리 엔지니어(Telemetry Engineer), 서브시스템 전문가, 관찰자로 구성될 수 있다. 지정된 한 명의 책임자가 시험 카드의 단계 진행을 통제하고, 별도로 권한이 부여된 안전 기능은 사전에 정의된 한계를 초과하는 경우 시험을 종료할 수 있는 권한을 유지해야 한다.

계측 시스템(Instrumentation)은 운용자의 관찰에만 의존하지 않고 항공기의 동작을 재구성할 수 있을 정도로 충분한 정보를 수집해야 한다. 관련 데이터에는 위치, 속도, 자세, 각속도, 제어 명령, 액추에이터 피드백, 추진 시스템 상태, 배터리 전압 및 전류, 항법 상태, C2 링크 지표, 모드 전환, 고장 플래그, 워치독(Watchdog) 이벤트, 지오펜스 상태, 안전 모니터(Safety Monitor) 출력이 포함된다. 공통 시간 기준(Common Timing)을 사용하면 사건을 정확하게 상호 연계할 수 있다.

여러 기체 내장 컴퓨터, 센서, 지상국, 독립 시험 계측 장비에서 측정값이 생성되는 경우 시간 동기화(Time Synchronization)가 특히 중요하다. 동기화되지 않은 로그는 정상적인 대응을 지연된 것처럼 보이게 하거나 고장을 시작한 사건의 순서를 불분명하게 만들 수 있다. 따라서 센서 입력, 탐지, 판단, 명령, 항공기 반응을 정확하게 비교할 수 있도록 전체 데이터 획득 체인에서 타임스탬프 출처 추적성(Timestamp Provenance)을 유지해야 한다.

정상 비행 영역 시험(Nominal-Envelope Testing)은 비정상 시나리오를 도입하기 전에 기준 데이터를 확립한다. UAV는 대표적인 페이로드와 환경 조건에서 안정적인 이륙, 상승, 순항 또는 호버링, 기동, 하강, 접근, 착륙을 입증해야 한다. 기준 데이터는 이후 성능 저하 운용과 비교할 수 있도록 제어 여유(Control Margin), 추종 오차, 에너지 소비, 통신 성능, 항법 정확도, 정상적인 변동 범위를 특성화한다.

비행 영역 확장(Envelope Expansion)은 속도, 고도, 페이로드, 바람, 기동 강도, 자동화 권한(Automation Authority)을 점진적으로 증가시키는 방식으로 수행해야 한다. 운용 한계에 가까운 시험 지점은 위험도가 낮은 조건에서 모델 예측과 충분한 안전 여유가 확인된 이후에만 접근해야 한다. 확장 기준은 주관적인 항공기 신뢰도가 아니라 측정된 성능을 기반으로 시험 진행 여부를 결정할 수 있도록 정량적으로 정의되어야 한다.

고장 주입 비행 시험(Fault-Injection Flight Testing)은 안전하고 가역적으로 적용할 수 있는 조건에 집중해야 한다. 예를 들어 모의 센서 무효 상태, 선택적인 통신 중단, 명령 기반 중복 채널 전환(Redundancy Switchover), 소프트웨어에서 생성한 고장 플래그, 비필수 정보원의 통제된 제거 등을 사용할 수 있다. 제어 불가능한 동작을 발생시킬 수 있는 물리적 고장은 의도적으로 비행 중 발생시키기보다 일반적으로 시뮬레이션, 실험실 또는 HIL 환경에서 평가해야 한다.

중복 비행 제어 아키텍처(Redundant Flight-Control Architecture)는 통제된 조건에서 고장 탐지, 격리, 재구성(Reconfiguration)을 시연하여 검증해야 한다. 하나의 컴퓨팅 채널에서 고장을 모사하고 정상 채널이 계속 운용되도록 할 수 있다. 측정을 통해 고장 탐지 지연(Fault-Detection Latency), 보팅 동작(Voting Behavior), 채널 격리, 제어 과도응답(Control Transient), 상태 동기화, 항공기 한계 위반이나 허용할 수 없는 궤적 편차 없이 안정성이 지속되는지를 확인해야 한다.

추진 시스템 고장 허용 동작(Propulsion Fault-Tolerant Behavior)은 엄격하게 제한된 시험이 필요하다. 파괴적인 모터 또는 엔진 고장을 실제로 발생시키는 대신 시험 시스템을 이용하여 명령 권한 상실을 모사하거나 사전에 승인된 범위 내에서 특정 추진 출력을 제한할 수 있다. 목적은 즉각적인 시험 종료를 위한 충분한 제어 권한을 유지하면서 고장 탐지, 제어 재할당(Control Reallocation), 자세 안정화, 궤적 관리, 복구 판단을 검증하는 것이다.

GNSS 성능 저하 시험(GNSS Degradation Testing)은 통제되지 않은 무선 주파수 간섭(Radio-Frequency Interference)을 발생시키지 않으면서 위성 기반 항법에서 사용 가능한 대체 항법 정보원으로 전환하는 과정을 평가해야 한다. 승인된 시뮬레이션, 차폐(Shielding), 수신기 시험 기법 또는 기체 내 고장 주입을 이용하여 신호 상실이나 손상된 항법 입력을 모사할 수 있다. 항공기는 탐지, 측정값 거부, 불확실성 관리, 대체 항법(Fallback Navigation), 적절한 복구 동작을 입증해야 한다.

C2 링크 상실 검증(C2 Lost-Link Validation)은 시험 구성에서 허용되는 범위 내에서 점진적인 성능 저하와 완전한 통신 상실을 모두 평가해야 한다. UAV는 설정된 성능 저하 링크 처리, 대기 또는 선회 대기(Hold or Loiter), 대체 링크 활성화, 복귀, 우회, 착륙 절차를 시연해야 한다. 재연결 과정에서 상충되는 명령이 발생하지 않고 기체 자율 시스템(Onboard Autonomy)이 승인된 운용 영역 경계 내에 계속 머무르는지를 검증해야 한다.

지오펜스 검증(Geofence Validation)은 경계를 넘어선 이후 경고를 발생시키는 것만 시연하는 것이 아니라 예측 기반 강제 적용(Predictive Enforcement)을 시험해야 한다. 서로 다른 속도, 접근 각도, 고도, 바람, 항법 불확실성 수준으로 경계에 접근할 수 있다. 시스템이 잠재적인 위반을 언제 예측하고, 유도 명령을 수정하고, 움직임을 제한하며, 안정적이고 물리적으로 실행 가능한 항공기 동작을 유지하면서 분리 거리를 회복하는지를 측정해야 한다.

배터리 및 에너지 비상 상황 시험(Battery and Energy Contingency Testing)은 사용 가능한 추진 능력이 심각한 수준으로 저하되기 전에 복구 동작이 시작되는지를 검증해야 한다. 의도적으로 배터리를 손상시키는 대신 통제된 초기 충전 상태(State of Charge), 대표적인 페이로드, 사전에 정의된 예비 에너지 임계값을 사용할 수 있다. 시스템은 예상 에너지, 전력 여유(Power Margin), 착륙 요구사항에 따라 임무 지속, 복귀, 우회, 비상 착륙을 정확하게 구분해야 한다.

워치독 및 안전 모니터 검증(Watchdog and Safety-Monitor Validation)은 비정상적인 계산 조건에서도 핵심 감독 기능이 유지된다는 것을 입증해야 한다. 통제된 소프트웨어 메커니즘을 통해 하트비트 누락, 태스크 정지, 오래된 데이터, 유효하지 않은 명령, 프로세서 채널 상실을 모사할 수 있다. 시험에서는 탐지 시간, 고장 분류, 제어 권한 전환(Control-Authority Transfer), 복구 동작, 독립적인 안전 보호 기능의 지속적인 작동을 확인해야 한다.

복합 고장 시험(Combined-Failure Testing)은 실제 안전 위험이 성능 저하된 여러 시스템 사이의 상호작용에서 발생하는 경우가 많기 때문에 필요하다. 낮은 배터리 예비량과 통신 상실이 동시에 발생하거나 지오펜스 경계 근처에서 항법 성능이 저하되는 상황은 각각의 단일 고장과 다른 대응을 요구할 수 있다. 복합 고장은 안전 분석을 기반으로 선정하고 단계적으로 도입해야 하며, 안전한 비행 시연이 현실적으로 어려운 고위험 조합은 시뮬레이션 환경에서 평가해야 한다.

시험 중단 기준(Abort Criteria)은 모든 시험 전에 정의되어야 한다. 과도한 자세 편차, 예상하지 못한 고도 손실, 지오펜스 안전 여유 감소, 비정상적인 진동, 추진 성능 저하, 배터리 임계값, 항법 불확실성, 통신 상태 또는 예상된 상태 전환 순서 미준수 등을 한계로 설정할 수 있다. 사전에 정의된 안전 한계를 초과한 이후 시험을 계속할 것인지 여부를 시험팀이 현장에서 다시 논의해야 하는 상황이 발생해서는 안 된다.

시험 종료(Test Termination)는 UAV를 알려진 복구 상태(Known Recovery State)로 전환해야 한다. 항공기 구성과 남아 있는 기능에 따라 수동 제어 전환(Manual Takeover), 자율 대기, 복귀, 우회, 제어된 하강 또는 착륙을 사용할 수 있다. 안전한 시험을 위해서는 허용할 수 없는 조건을 인식하는 것뿐만 아니라 실험 조건에서 안정적으로 벗어날 수 있는 수단도 필요하기 때문에 시험 종료 경로 자체도 검증해야 한다.

독립적인 안전 모니터링(Independent Safety Monitoring)은 시험 대상 시스템 이상의 추가적인 보호를 제공할 수 있다. 별도의 추적 정보원, 독립 통신 경로, 외부 관찰자, 적절한 경우 추적 플랫폼(Chase Platform), 지상 기반 모니터링 시스템을 통해 운용 영역 유지를 확인하고 시험 종료 판단을 지원할 수 있다. 시험에서 기체 내장 항법, 통신 또는 비행 제어 기능을 의도적으로 성능 저하시키는 경우 이러한 독립성이 특히 중요하다.

환경 조건(Environmental Condition)은 단순히 정성적으로 설명하는 것이 아니라 실제로 측정해야 한다. 풍속과 풍향, 돌풍, 온도, 압력, 가시성, 강수량 및 기타 관련 환경 변수는 제어 성능, 추진 요구량, 항법 센서, 통신 링크에 영향을 준다. 예상하지 못한 항공기 동작이 외부 환경 변화로 발생한 것인지 시스템 자체의 문제인지를 구분할 수 있도록 환경 데이터를 비행 로그와 함께 보존해야 한다.

합격 및 불합격 기준(Pass and Fail Criteria)은 가능한 경우 정량적으로 정의해야 한다. 최대 탐지 지연, 허용 가능한 궤적 편차, 최소 지오펜스 분리 거리, 최대 제어 과도응답, 착륙 시 요구되는 최소 예비 에너지, 허용 가능한 통신 중단 시간, 허용 가능한 항법 불확실성 등을 사용할 수 있다. 정량적 기준은 시험 반복성을 향상시키고 시험 이후 관측된 결과에 맞추어 해석 기준이 변경되는 것을 방지한다.

안전 필수 동작을 시험한 경우 다음 시험 지점으로 진행하기 전에 비행 후 데이터 검토(Post-Flight Data Review)를 시작해야 한다. 신속 분석(Quick-Look Analysis)을 통해 명령된 고장이 의도한 대로 발생했는지, 보호 로직이 올바르게 활성화되었는지, 시스템 한계가 준수되었는지, 계측 데이터가 유효한지를 확인할 수 있다. 예상하지 못한 동작이 발견되면 그 원인과 안전 영향이 이해될 때까지 추가적인 비행 영역 확장을 중단해야 한다.

상세 분석(Detailed Analysis)은 측정된 동작을 모델, 요구사항, 예측된 안전 여유와 비교해야 한다. 엔지니어는 동기화된 로그를 이용하여 사건 타임라인(Event Timeline)을 재구성하고, 탐지 및 대응 지연을 계산하며, 제어 과도응답을 평가하고, 추정기 성능을 분석하며, 안전 예비량이 유지되었는지를 판단할 수 있다. 예측 결과와 실제 관측 결과의 차이는 모델, 요구사항, 소프트웨어 또는 운용 제한 개선에 다시 반영되어야 한다.

안전 관련 소프트웨어, 하드웨어, 파라미터 또는 형상 데이터가 변경되는 경우 회귀 시험(Regression Testing)이 필요하다. 항법 필터링, 제어 게인(Control Gain), 배터리 임계값, 통신 로직, 지오펜스 처리의 변경은 이전에 검증된 동작에 영향을 미칠 수 있다. 영향 분석(Impact Analysis)을 통해 통합 안전 사례(Integrated Safety Case)에 대한 신뢰를 유지하기 위해 어떤 지상 시험, 시뮬레이션, HIL 시험, 비행 시험을 반복해야 하는지 결정해야 한다.

비행 시험 증거(Flight-Test Evidence)는 형상 및 데이터 관리(Configuration and Data Management) 체계 아래 유지되어야 한다. 시험 카드, 승인 기록, 항공기 형상 기록, 환경 데이터, 텔레메트리, 기체 내장 로그, 사용된 경우 영상 자료, 이상 현상 보고서, 분석 결과, 최종 결론은 특정 요구사항과 소프트웨어 또는 하드웨어 베이스라인까지 추적 가능해야 한다. 이러한 증거는 엔지니어링 의사결정, 인증 활동, 운용 승인, 향후 유지보수를 지원한다.

안전 검증(Safety Validation)은 여러 검증 수준에서 확보한 증거가 일관된 논증을 형성할 때에만 완료된 것으로 볼 수 있다. 시뮬레이션은 광범위한 시나리오 범위를 제공하고, SIL은 소프트웨어 동작을 평가하며, HIL은 실제적인 하드웨어와 시간 특성을 적용하고, 지상 시험은 통합 서브시스템을 검증하며, 비행 시험은 실제 물리적 동역학 환경에서 선택된 안전 동작을 확인한다. 하나의 검증 수준만으로 모든 위험 조건을 입증하려고 해서는 안 된다.

성숙한 UAV 안전 검증 비행 시험 프로토콜(Mature UAV Safety Flight-Test Protocol)은 요구사항 추적성, 점진적인 비행 영역 확장, 통제된 고장 주입, 독립적인 모니터링, 정량적 한계, 동기화된 계측 시스템, 사전에 정의된 시험 중단 로직(Abort Logic), 엄격한 비행 후 분석을 통합한다. 핵심 목표는 고장이 절대로 발생하지 않는다는 것을 입증하는 것이 아니라, 현실적으로 발생 가능한 고장이 허용할 수 없는 항공기 또는 지상 위험으로 발전하기 전에 탐지되고, 운용 영역 내에서 억제되며, 안전하게 관리된다는 것을 통제된 증거를 통해 입증하는 것이다.
