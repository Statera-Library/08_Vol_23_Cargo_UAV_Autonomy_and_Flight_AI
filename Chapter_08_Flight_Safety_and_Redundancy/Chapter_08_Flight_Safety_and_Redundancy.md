**Volume 23. Cargo UAV Autonomy and Flight AI**


# Chapter 08. Flight Safety and Redundancy

##  

## 08.01. UAV Safety Architecture FHA FMEA FTA

![](images/image1.png){width="7.268055555555556in" height="7.268055555555556in"}

UAV safety architecture begins with the principle that safety must be engineered into the aircraft rather than added after the flight-control software is complete. The architecture therefore connects aircraft functions, hardware, software, communication links, propulsion, energy systems, payload interfaces, and ground operations to explicit safety objectives. Each function is evaluated according to how its failure could affect the aircraft, people, property, and surrounding airspace.

A Functional Hazard Assessment (FHA) provides the initial structured analysis of aircraft-level and system-level functions. Engineers identify functions such as attitude stabilization, navigation, propulsion control, power distribution, command and control, obstacle avoidance, payload management, and emergency landing. For each function, the analysis considers loss, unintended activation, incorrect output, delayed operation, and misleading information that could create hazardous aircraft behavior.

Failure conditions identified by the FHA are classified according to the severity of their potential consequences. Depending on the applicable assurance framework, categories may range from minor operational effects to hazardous or catastrophic outcomes. A catastrophic condition may involve loss of controlled flight with unacceptable risk to people, while a less severe condition may increase crew workload or reduce mission capability without immediately threatening safe flight. These classifications drive safety requirements.

The FHA should consider combinations of operational conditions rather than examining failures only during nominal flight. A navigation degradation that is manageable at high altitude may become critical during low-altitude operations near structures. Similarly, degraded propulsion may have different consequences during cruise, hover, takeoff, or landing. Mission phase, aircraft mass, weather, traffic density, communication coverage, and available emergency landing areas therefore influence hazard severity and mitigation strategy.

Failure Mode and Effects Analysis (FMEA) moves from functional hazards toward detailed component and subsystem behavior. Each relevant item is examined for credible failure modes, local effects, propagated effects, detection mechanisms, and available responses. A power converter, for example, may fail open, fail short, produce unstable voltage, overheat, or provide incorrect health information. The analysis determines whether these failures remain isolated or propagate into flight-critical equipment.

FMEA is particularly valuable for revealing dependencies that are easily overlooked in block-level architecture. Two redundant flight computers may appear independent while sharing the same power rail, clock source, communication switch, cooling path, or sensor interface. Failure of that shared resource can defeat both channels simultaneously. Effective redundancy therefore requires independence analysis in addition to simply duplicating processors, sensors, communication links, or actuators.

Critical FMEA findings are translated into architectural controls such as physical separation, electrical isolation, independent power sources, dissimilar sensing, watchdog mechanisms, communication monitoring, fault containment, and graceful degradation. The objective is not necessarily to preserve full mission capability after every failure. Instead, the system should maintain an acceptable safe state, which may involve reduced speed, restricted maneuvering, mission termination, return-to-home operation, controlled descent, or emergency landing.

Fault Tree Analysis (FTA) approaches safety from the opposite direction. Instead of beginning with individual components, it starts with an undesirable top-level event and decomposes the combinations of failures capable of producing it. Typical top events include loss of controlled flight, unintended propulsion shutdown, loss of navigation integrity, uncontrolled descent, collision risk, or inability to execute emergency recovery. Logical relationships expose both single-point and combined failure paths.

FTA helps distinguish failures that independently cause a hazardous event from failures that become dangerous only when they occur together. An AND relationship can represent multiple protections that must fail before a top event occurs, while an OR relationship identifies alternative paths capable of producing the same result. This reasoning supports architectural decisions concerning redundancy level, monitoring coverage, fault isolation, backup modes, and independence between primary and secondary safety mechanisms.

FHA, FMEA, and FTA are most effective when treated as complementary and iterative processes. FHA establishes what aircraft-level failures matter and how severe they are. FMEA investigates how lower-level items can create or contribute to those conditions. FTA verifies whether combinations of faults can reach critical top events despite proposed mitigations. Findings from one analysis frequently modify assumptions, requirements, interfaces, or architecture considered by the others.

A robust UAV architecture separates safety-critical functions according to their required availability and integrity. Flight stabilization, actuator control, essential navigation, power management, and emergency logic may require stronger isolation than payload processing or nonessential mission applications. Partitioning prevents failures in high-complexity perception, communication, or payload software from consuming resources or corrupting data needed by deterministic flight-control and recovery functions.

Redundancy must be designed around failure domains rather than component counts. Dual inertial sensors provide limited protection if both depend on the same connector, thermal environment, synchronization source, or software driver. Triple sensors can support voting, but voting is useful only when faults are sufficiently independent and erroneous measurements can be identified. Diversity in sensor technology, placement, algorithms, power paths, and communication routes can reduce common-mode vulnerability.

Flight computers commonly combine monitoring, cross-checking, heartbeat supervision, and state comparison to detect abnormal channels. A redundant architecture must define authority transfer carefully because an uncontrolled switchover can itself become hazardous. The system therefore needs deterministic criteria for fault declaration, channel isolation, command arbitration, synchronization, and recovery. Transient disturbances must be distinguished from persistent failures without allowing detection latency to exceed safety margins.

Actuator and propulsion redundancy require similar reasoning. Multiple motors on a cargo UAV do not automatically guarantee continued controlled flight after a motor failure. Vehicle geometry, thrust margin, center of gravity, aerodynamic coupling, motor-controller independence, electrical distribution, and control allocation determine whether remaining actuators can maintain stability. Safety analysis must therefore connect component redundancy with actual controllability across the certified operating envelope.

Power architecture is another major safety domain because electrical failures can simultaneously disable otherwise independent systems. Battery packs, contactors, distribution buses, converters, wiring, and protection devices should be evaluated for short circuits, open circuits, thermal events, undervoltage, overcurrent, and erroneous state estimation. Segregated buses and controlled cross-ties can preserve essential loads while preventing a failed branch from collapsing the complete electrical system.

Communication loss must be treated as an expected operational condition rather than an exceptional software error. Command-and-control links can experience interference, obstruction, network congestion, equipment failure, or ground-station loss. The UAV therefore requires deterministic lost-link behavior based on mission context. Depending on available navigation integrity and remaining energy, the aircraft may continue briefly, hold position, follow a predefined route, return, divert, or initiate a controlled landing.

Safety architecture also depends on reliable fault detection and health management. Built-in tests, range checks, temporal consistency checks, model-based residuals, cross-sensor comparisons, and communication integrity monitoring can provide evidence that a subsystem is degraded. Detection alone is insufficient; the architecture must associate each detected condition with a defined response, including fault isolation, reconfiguration, capability reduction, operator notification, and transition toward an appropriate safe state.

Common-cause and common-mode failures require dedicated attention because they can invalidate assumptions behind redundancy. Environmental exposure, electromagnetic interference, software defects, incorrect maintenance, shared configuration data, manufacturing defects, icing, overheating, or a common timing error may affect several redundant channels simultaneously. Independence claims should therefore be supported by physical, electrical, functional, and software separation appropriate to the identified hazards.

Safety requirements derived from FHA, FMEA, and FTA should remain traceable through system requirements, subsystem specifications, software requirements, hardware design, verification procedures, and test evidence. Each mitigation must have an identifiable implementation and verification method. Traceability prevents safety controls from becoming informal design intentions and enables engineering teams to determine whether architecture changes introduce new hazards or invalidate previously accepted assumptions.

Verification combines analysis with simulation, software-in-the-loop testing, hardware-in-the-loop testing, fault injection, ground testing, and progressively constrained flight testing. Fault injection is especially important because many redundancy mechanisms remain dormant during normal operation. Engineers intentionally introduce sensor dropouts, communication delays, processor resets, corrupted measurements, actuator degradation, or power disturbances to confirm that detection, isolation, reconfiguration, and recovery occur within required timing limits.

The safety case ultimately connects hazards, requirements, architecture, implementation, verification evidence, and operational constraints into a coherent argument. FHA identifies the consequences that must be controlled, FMEA exposes detailed failure propagation, and FTA examines combinations leading to unacceptable events. Together they provide a disciplined basis for designing UAVs that tolerate credible faults while maintaining controlled behavior and predictable transitions toward safe operating states.

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

##  

## 08.02. Dual Triple Redundant FCC Design and Voting [w/Code]

![](images/image2.png){width="7.268055555555556in" height="7.268055555555556in"}

A redundant Flight Control Computer (FCC) architecture is designed to prevent a single computing failure from causing loss of aircraft control. In a UAV, the FCC executes stabilization, guidance, navigation processing, actuator commands, mode management, and safety logic. Redundancy distributes these functions across independent computing channels so that failures can be detected, isolated, and accommodated while maintaining predictable flight behavior.

Dual-redundant FCC architecture uses two computing channels that receive equivalent sensor information and independently calculate flight-control outputs. Each channel normally executes identical or functionally equivalent control algorithms and continuously monitors the other channel. The architecture can operate as active-standby, active-active, or command-monitor, depending on required availability, computational complexity, fault containment strategy, and aircraft safety objectives.

In an active-standby configuration, the primary FCC controls the aircraft while the secondary FCC remains synchronized and monitors primary operation. When the primary channel is declared faulty, command authority transfers to the standby channel. This architecture is conceptually simple, but safe transfer requires accurate state synchronization, deterministic fault detection, and rapid switching so that actuator commands do not experience unacceptable discontinuities during the transition.

Active-active dual architectures allow both FCCs to calculate control commands simultaneously. Their outputs are compared continuously using state variables, sensor estimates, control modes, and actuator commands. Agreement provides confidence that both channels are operating consistently, while excessive disagreement indicates a fault. The fundamental limitation is that two disagreeing channels cannot inherently determine which channel is correct without additional independent information.

A dual architecture therefore requires an arbitration mechanism capable of resolving ambiguous disagreement. Independent monitors, dissimilar processors, external sensor validation, actuator feedback, or a separate safety controller may provide the additional evidence needed to identify the faulty channel. Without such evidence, a two-channel system can detect disagreement but may be unable to isolate the failed computer reliably, particularly when both outputs remain individually plausible.

Triple-redundant FCC architecture introduces a third independent channel and enables majority voting. When all three channels operate correctly, their computed states and commands should remain within defined agreement thresholds. If one channel deviates significantly while the other two agree, the majority pair can identify the outlier. The failed or suspect channel can then be isolated while the remaining two channels continue controlling the aircraft under a degraded redundancy state.

Triple Modular Redundancy (TMR) is commonly associated with a two-out-of-three voting principle. The voter compares equivalent outputs from three channels and selects the value supported by the majority. For discrete states such as mode selection or validity flags, logical majority voting can be applied directly. For continuous variables such as attitude estimates, thrust commands, or actuator positions, median selection, bounded averaging, or tolerance-based comparison is often more appropriate.

Continuous-value voting requires carefully selected thresholds because perfectly identical numerical outputs are unrealistic. Sensor noise, asynchronous sampling, floating-point behavior, estimator differences, and scheduling jitter can produce small variations between healthy channels. Agreement windows must therefore distinguish normal computational dispersion from genuine faults. Thresholds that are too narrow create nuisance fault declarations, while excessively wide thresholds can allow hazardous erroneous commands to remain accepted.

Voting can be performed at several architectural levels. Raw sensor measurements may be voted before entering the control algorithm, estimated states may be compared after navigation processing, and final actuator commands may be voted before transmission. Each approach detects different failure classes. A robust architecture often combines several layers so that sensor faults, estimation faults, processing faults, and command-generation faults can be distinguished rather than represented by a single generic disagreement signal.

FCC redundancy depends strongly on input independence. Three computers receiving data from one inertial measurement unit do not provide protection against failure of that shared sensor. Safety-critical architectures may therefore use multiple IMUs, air-data sources, GNSS receivers, altitude sensors, or propulsion feedback channels. Sensor-to-FCC mapping should minimize common dependencies while preserving enough cross-channel information for consistency checking and fault identification.

Time synchronization is equally important because voting values generated at different physical times can appear inconsistent even when every channel is healthy. FCCs may use synchronized clocks, hardware timestamps, deterministic communication schedules, and bounded execution periods to maintain temporal alignment. Comparison logic should consider measurement age, processing latency, transport delay, and command validity intervals rather than comparing numerical values without temporal context.

Cross-channel communication enables FCCs to exchange health states, estimated aircraft states, mode information, execution counters, and fault declarations. This communication must itself be treated as a potential failure source. A failed network switch or corrupted shared bus should not simultaneously disable all redundancy. Independent communication paths, bus guardians, message integrity checks, sequence counters, timeouts, and source authentication can strengthen fault containment.

The voter is itself a safety-critical element because an incorrectly implemented voter can defeat otherwise independent FCC channels. Centralized voting simplifies architecture but can create a single point of failure unless the voter is highly assured or replicated. Distributed voting allows each channel to perform independent comparisons, but it introduces challenges involving consistent fault decisions and command authority. The selected architecture must prevent conflicting channels from simultaneously commanding actuators.

Command authority management defines which FCC output is allowed to influence the physical aircraft. Arbitration logic should establish deterministic priority, ownership, and transition rules under normal and degraded conditions. During a channel switchover, integrator states, navigation estimates, control references, and actuator command histories should remain sufficiently synchronized to avoid abrupt control transients. Bumpless transfer is particularly important for large cargo UAVs with substantial inertia.

Fault detection combines voting disagreement with internal FCC health monitoring. Watchdog timers, memory protection, processor lockstep checks, execution-time monitoring, power supervision, temperature monitoring, built-in tests, and software sanity checks can detect failures before command disagreement becomes visible. Combining internal diagnostics with external cross-channel comparison improves fault isolation and reduces dependence on any single detection mechanism.

Redundancy must also address Byzantine-like behavior in which a faulty FCC continues operating but produces inconsistent or misleading information rather than simply stopping. A channel may generate plausible commands while corrupting selected state variables, transmit different data to different peers, or intermittently violate timing constraints. Message consistency checking, independent observation, bounded command validation, and actuator-level protection help prevent such faults from gaining uncontrolled authority.

Common-mode failure is one of the greatest limitations of redundant FCC design. Three identical computers running identical software can simultaneously fail because of the same software defect, erroneous configuration, environmental condition, or corrupted shared input. Physical separation, independent power supplies, separate communication interfaces, dissimilar software implementations, diverse processors, and independent monitoring can reduce the probability that one initiating cause defeats every channel.

Dissimilar redundancy can be particularly valuable for high-consequence functions. A primary FCC may execute a sophisticated flight-control and navigation stack while an independent safety controller uses simpler verified logic to monitor attitude, altitude, velocity, geofencing, and control-command limits. The safety controller does not need full mission capability; its purpose may be to reject unsafe commands, trigger recovery, stabilize the aircraft, or initiate controlled landing when the primary architecture becomes unreliable.

Degraded-mode management must be explicitly defined as redundancy is lost. A triple system may transition from three healthy channels to two-channel operation after isolating one FCC. A subsequent disagreement between the remaining channels creates a more difficult decision because majority voting is no longer available. The aircraft may then restrict maneuvering, terminate the mission, return to a recovery location, reduce flight duration, or initiate landing depending on hazard severity and available independent monitoring.

Redundancy management should avoid unnecessary channel removal caused by temporary disturbances. Fault declaration may require persistence counters, multiple consecutive disagreements, rate-of-change checks, or confirmation from independent health monitors. At the same time, hazardous failures must be isolated quickly enough to prevent incorrect commands from affecting aircraft dynamics. Detection persistence and response latency are therefore balanced against the physical time available before a failure becomes uncontrollable.

Actuator interfaces must preserve the protection established by redundant computation. If multiple FCCs ultimately feed a single unprotected actuator controller through one shared communication path, much of the upstream redundancy can be lost. Smart actuators or redundant actuator-control electronics can perform command validation, source arbitration, position feedback monitoring, and local limit enforcement, creating an additional containment boundary between computing faults and physical motion.

Verification of dual and triple FCC architectures requires more than nominal functional testing. Software-in-the-loop and hardware-in-the-loop environments should inject processor resets, frozen outputs, biased estimates, delayed messages, communication partitions, corrupted commands, clock drift, sensor disagreement, power interruptions, and intermittent faults. Testing should verify not only fault detection but also correct isolation, voting, authority transfer, degraded operation, and eventual recovery behavior.

Latency measurements are essential during verification because redundancy mechanisms consume finite time. Sensor acquisition, channel computation, cross-channel exchange, voting, fault confirmation, arbitration, and actuator transmission all contribute to the end-to-end response. Safety analysis must demonstrate that the total delay remains compatible with aircraft dynamics, especially for unstable platforms, high-speed flight, heavy cargo vehicles, and operations close to terrain or infrastructure.

A successful redundant FCC design therefore combines computational duplication with independence, synchronized information, reliable voting, deterministic arbitration, fault containment, and controlled degradation. Dual architectures can provide valuable availability when supported by independent arbitration, while triple architectures enable stronger fault isolation through majority voting. The ultimate objective is not maximum channel count, but continued safe control when credible hardware, software, communication, sensor, or power failures occur.

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

##  

## 08.03. Engine and Motor Failure Fault Tolerant Control [w/Code]

![](images/image3.png){width="7.268055555555556in" height="7.268055555555556in"}

Engine and motor failure represents one of the most safety-critical disturbances for a UAV because propulsion directly determines lift, thrust, attitude authority, and trajectory control. Fault-Tolerant Control (FTC) is designed to preserve controllability after partial or complete propulsion loss by detecting the failure, estimating remaining control authority, reallocating commands, and transitioning the aircraft toward a safe operating condition.

Propulsion faults can appear as complete motor shutdown, partial thrust degradation, intermittent torque loss, excessive rotor drag, speed-control error, overheating, inverter malfunction, or delayed response. Internal-combustion or turbine-powered UAVs may additionally experience fuel-flow interruption, ignition problems, compressor or mechanical degradation, and governor faults. Each failure produces different dynamic effects and therefore requires different detection and accommodation strategies.

Failure consequences depend strongly on aircraft configuration. In a conventional fixed-wing UAV, engine loss primarily removes forward thrust while aerodynamic control surfaces may remain available for gliding and landing. In multirotor and distributed-electric-propulsion aircraft, individual motors contribute directly to both lift and moments. Losing one propulsion unit can therefore create simultaneous vertical-force deficiency and severe roll, pitch, or yaw disturbances.

Heavy-lift cargo UAVs present additional challenges because high gross mass, large inertia, payload displacement, and limited excess thrust reduce the margin available after a propulsion failure. A vehicle capable of hovering comfortably under nominal conditions may become incapable of maintaining altitude after losing a motor at maximum payload. Fault-tolerant design must consequently evaluate controllability throughout the operational mass, center-of-gravity, altitude, temperature, and battery-state envelope.

Fault detection begins by comparing commanded propulsion behavior with observed response. Motor rotational speed, phase current, voltage, torque estimates, electronic speed controller status, temperature, vibration, and actuator feedback can provide direct evidence of propulsion health. Aircraft-level measurements from inertial sensors can provide additional evidence when unexpected angular acceleration or vertical acceleration appears inconsistent with commanded forces and moments.

Model-based detection can calculate the thrust or torque expected from each propulsion unit and compare it with measured aircraft motion. Residuals that exceed statistically or physically defined limits indicate possible degradation. Because aerodynamic disturbances, gusts, payload motion, sensor noise, and modeling errors can produce similar residuals, fault diagnosis should combine multiple information sources rather than declaring motor failure from a single abnormal measurement.

Detection speed is critical because propulsion faults can produce rapid attitude divergence. A multirotor may have only a short interval between motor thrust loss and excessive roll or yaw rate. The detection architecture must therefore balance rapid response against false alarms. Persistence logic, adaptive thresholds, redundant measurements, and dynamic consistency checks can distinguish brief disturbances from failures while maintaining response times compatible with vehicle dynamics.

Once a failure is confirmed, Fault Detection, Isolation, and Identification (FDII) determines which propulsion unit is affected and estimates the magnitude of lost effectiveness. Isolation identifies the failed motor or engine, while identification determines whether the failure represents complete loss, partial thrust reduction, response lag, or another degraded condition. This information enables the controller to select an appropriate reconfiguration rather than applying one generic emergency response.

Control allocation is central to propulsion fault tolerance in vehicles with multiple actuators. Under nominal operation, the flight controller converts desired total thrust and body moments into individual motor commands. After a failure, the allocation problem is reformulated using only healthy or partially effective actuators. The controller attempts to reproduce the required force and moment vector while respecting motor speed, thrust, power, thermal, and structural constraints.

The post-failure vehicle may no longer be capable of independently producing every commanded force and moment. Control allocation must therefore prioritize objectives according to safety importance. Maintaining roll and pitch stability may take precedence over precise yaw tracking, position holding, or mission trajectory. In some multirotor configurations, controlled yaw rotation may be accepted if it allows the aircraft to preserve vertical support and prevent catastrophic attitude divergence.

Overactuated UAV configurations provide greater opportunities for fault accommodation. Hexacopters, octocopters, distributed-propulsion wings, and aircraft with redundant lift units can retain sufficient actuator authority after one propulsion unit is lost. However, the existence of additional motors does not automatically guarantee fault tolerance. Their placement, thrust direction, maximum capacity, rotational direction, and available power determine the remaining controllable force and moment set.

The flight controller should estimate the post-failure control envelope rather than assuming nominal maneuver capability remains available. Maximum climb rate, lateral acceleration, yaw authority, bank angle, payload capability, and disturbance rejection may all decrease. A reconfigured guidance system can constrain future commands to this reduced envelope, preventing higher-level autonomy or mission planning software from requesting maneuvers that the degraded propulsion system cannot safely execute.

Adaptive or reconfigurable controllers can modify control gains and internal models after propulsion effectiveness changes. A controller designed only around nominal dynamics may become unstable or excessively aggressive when actuator authority is reduced. Gain scheduling, model predictive control, nonlinear control, incremental control methods, or online effectiveness estimation can adapt the control response while preserving stability margins under degraded propulsion conditions.

Energy management becomes especially important after a motor failure. Remaining motors may need to operate at substantially higher thrust, increasing electrical current, inverter temperature, battery discharge rate, and thermal stress. Continuing flight for an extended period can therefore create secondary failures. The emergency controller should estimate remaining energy and thermal margins continuously and determine whether immediate landing is safer than attempting a distant return-to-home maneuver.

Electrical architecture must prevent a local propulsion fault from propagating into healthy channels. A shorted motor winding, failed inverter, damaged power cable, or thermal event can affect a shared power bus if isolation is inadequate. Independent protection devices, contactors, fuses, current limiting, segregated distribution paths, and fault-containment zones can disconnect the failed branch while preserving electrical power for the remaining propulsion units and flight-critical electronics.

Mechanical consequences must also be considered. A failed rotor may stop, windmill, seize, shed material, or generate abnormal vibration. Windmilling can create aerodynamic drag and unwanted torque, while a seized or damaged rotor can alter vehicle dynamics differently from a simple zero-thrust assumption. Health monitoring should therefore distinguish electrical shutdown from mechanical failure whenever possible, and control models should represent the resulting asymmetric aerodynamic effects.

For hybrid or engine-driven UAVs, propulsion recovery may include restart logic in addition to control reconfiguration. The system can evaluate engine speed, fuel pressure, ignition status, temperature, altitude, and flight condition before attempting restart. Repeated restart attempts should be limited when they consume critical energy or create additional risk. If restart is unsuccessful, guidance should transition decisively toward glide, diversion, or emergency landing behavior.

Emergency trajectory generation connects fault-tolerant control with mission-level safety. Once propulsion capability is degraded, the vehicle should evaluate reachable landing locations using altitude, airspeed, wind, terrain, obstacles, population exposure, remaining thrust, and energy. The safest response may differ from the nominal return route. A nearby emergency landing zone can be preferable to returning to the launch point when continued flight increases exposure to secondary failure.

Payload state has a direct influence on propulsion-failure recovery for cargo UAVs. Heavy or externally suspended loads can reduce thrust margin and introduce pendulum dynamics that complicate stabilization. If the aircraft architecture permits controlled payload release, such an action may improve survivability in carefully defined emergency conditions. Any release strategy requires strict safety logic because dropping cargo can transfer aircraft risk to people or infrastructure on the ground.

Fault-tolerant propulsion control should coordinate with navigation and perception systems. A degraded aircraft may require larger turning radii, lower acceleration, reduced obstacle-clearance capability, or a more direct landing trajectory. Navigation software must understand these limitations rather than continuing to generate nominal commands. This creates a closed safety relationship between fault diagnosis, control allocation, guidance, perception, and emergency mission management.

Common-cause failures remain a major concern even when many motors are installed. Multiple propulsion units may share the same battery, cooling system, communication bus, software command path, manufacturing defect, or environmental exposure. Battery failure, icing, overheating, electromagnetic disturbance, or erroneous control software can therefore disable several motors simultaneously. Propulsion redundancy must be supported by independence and diversity at the system level.

Verification requires systematic injection of propulsion faults in simulation before risky physical testing begins. Software-in-the-loop environments can evaluate thousands of failure combinations across different flight conditions, while hardware-in-the-loop testing can introduce realistic controller, inverter, sensor, and communication faults. High-fidelity simulation should represent actuator saturation, motor dynamics, aerodynamic asymmetry, payload effects, battery limitations, and detection delays.

Physical testing can then progress from restrained propulsion tests to controlled flight experiments with carefully bounded fault scenarios. Engineers measure detection latency, attitude excursion, altitude loss, actuator saturation, recovery time, thermal loading, and landing performance. Tests should include partial degradation and intermittent faults as well as complete motor shutdown because subtle failures may be more difficult to identify and can remain active for longer periods.

Safety validation must demonstrate that no credible single propulsion failure produces an uncontrolled transition before mitigation becomes effective, within the intended operational envelope. For configurations that cannot sustain flight after particular failures, the objective changes from continued mission operation to controlled termination. The architecture should explicitly identify which failures permit continued flight, which require immediate diversion, and which demand emergency landing.

Effective engine and motor Fault-Tolerant Control therefore combines propulsion health monitoring, rapid fault isolation, effectiveness estimation, constrained control allocation, adaptive stabilization, energy management, and emergency guidance. The essential objective is not to make propulsion failures invisible, but to ensure that the UAV recognizes its reduced physical capability and rapidly converts that knowledge into stable, predictable, and safety-prioritized behavior.

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

##  

## 08.04. GNSS Spoofing Jamming Detection and Fallback [w/Code]

![](images/image4.png){width="7.268055555555556in" height="7.268055555555556in"}

GNSS is a major navigation source for UAV position, velocity, timing, route tracking, geofencing, and return-to-home functions, but satellite signals arriving at the receiver are weak and vulnerable to interference. A safety-oriented UAV must therefore treat GNSS as a potentially degraded or deceptive information source rather than an unquestioned reference, especially during autonomous or beyond-visual-line-of-sight operations.

GNSS jamming occurs when interference reduces the receiver's ability to acquire or track legitimate satellite signals. It may appear as abrupt loss of satellites, reduced carrier-to-noise density, increased measurement uncertainty, repeated tracking resets, or complete navigation outage. Interference may be intentional or unintentional, and the safety architecture should respond according to observed signal integrity rather than depending on assumptions about its origin.

Spoofing is fundamentally different because the receiver may continue producing apparently valid navigation solutions while processing counterfeit or manipulated signals. A spoofing attack can potentially create false position, velocity, altitude, or timing information without immediately generating a conventional loss-of-signal warning. This makes consistency monitoring and independent navigation references essential for detecting deceptive GNSS behavior.

Detection should combine receiver-level indicators with aircraft-level navigation consistency checks. Useful receiver observations include satellite count, signal strength, carrier-to-noise ratio, automatic gain control behavior, residuals, tracking status, clock behavior, and integrity flags. No single metric provides reliable protection in every environment, so detection confidence should be formed from multiple indicators and their temporal evolution.

Signal-strength anomalies can provide evidence of interference or spoofing when several satellite channels change simultaneously in an unexpected manner. Genuine satellite signals normally arrive with predictable power characteristics, while artificial interference may cause abrupt or unusually correlated changes. However, antenna orientation, aircraft attitude, buildings, terrain, multipath, and atmospheric effects can also change received power, requiring context-aware thresholds.

Navigation innovation monitoring compares GNSS measurements with predictions from an inertial navigation system. An Extended Kalman Filter or similar estimator predicts position and velocity using IMU measurements, then evaluates the difference when GNSS updates arrive. Large or persistent innovations can indicate GNSS corruption, although inertial drift and modeling uncertainty must be represented correctly to prevent legitimate estimation errors from being mistaken for attacks.

Velocity consistency provides another strong cross-check because GNSS-derived velocity can be compared with inertial acceleration, airspeed, optical flow, visual odometry, wheel or ground-contact information where applicable, and the vehicle's commanded motion. A position solution that moves rapidly while independent sensors indicate stable hovering is physically inconsistent and should cause the navigation system to reduce trust in GNSS data.

Temporal consistency is particularly important against gradually manipulated navigation solutions. A sophisticated false signal may move the reported position slowly enough to remain within simple instantaneous thresholds. The detector should therefore monitor accumulated displacement, trajectory curvature, clock drift, innovation trends, map constraints, and the relationship between commanded motion and measured motion over extended windows rather than relying only on single-sample checks.

Multi-constellation and multi-frequency receivers can improve robustness by providing observations from GPS, Galileo, GLONASS, BeiDou, or other supported services and multiple frequency bands. Disagreement between constellations or frequencies can provide useful diagnostic evidence. However, multi-constellation reception should not automatically be considered independent redundancy because common antenna, RF front-end, interference environment, or receiver software can affect all observations.

Antenna-based techniques can add another layer of protection. Dual-antenna or multi-antenna systems can compare carrier phase, heading, direction-of-arrival characteristics, or spatial consistency. Authentic satellites are distributed across the sky, whereas counterfeit signals originating from one transmitter may exhibit suspiciously similar spatial characteristics. These techniques require appropriate antenna geometry, calibration, receiver support, and installation quality.

Map and mission constraints can also support GNSS integrity monitoring. A UAV reported far outside its reachable region, suddenly crossing a geofence without corresponding inertial motion, or moving through an impossible terrain boundary provides evidence of navigation inconsistency. Such checks should remain secondary evidence because maps may be incomplete and emergency maneuvers can legitimately violate expected mission trajectories.

The navigation system should maintain explicit GNSS health states rather than using a simple available-or-unavailable flag. States may represent nominal, suspicious, degraded, rejected, and recovering conditions. Confidence can decrease progressively as anomalies accumulate, allowing the estimator and guidance system to reduce GNSS weighting before complete rejection. This approach avoids abrupt navigation transitions when evidence is uncertain.

Once GNSS is judged unreliable, the estimator should prevent corrupted measurements from continuing to influence navigation states. Merely displaying a warning while still fusing deceptive measurements can allow the position estimate to drift toward a false solution. Measurement gating, adaptive covariance inflation, source isolation, or complete GNSS rejection can be used according to confidence level and the severity of detected inconsistencies.

Fallback navigation begins with inertial navigation because the IMU remains locally available and does not depend on external radio signals. High-rate accelerometer and gyroscope measurements can propagate attitude, velocity, and position after GNSS rejection. However, inertial errors accumulate with time, so pure inertial navigation is generally a bridging capability rather than an indefinite substitute, particularly for small UAVs using low-cost MEMS sensors.

Visual-inertial odometry can constrain inertial drift by tracking visual features across camera frames and combining them with IMU motion estimates. It can provide useful relative position and velocity information in GNSS-denied environments, but performance depends on lighting, texture, motion blur, camera calibration, and scene geometry. Safety architecture should therefore understand when visual navigation itself becomes unreliable.

LiDAR odometry, radar odometry, optical flow, terrain-relative navigation, and map matching can provide additional fallback sources depending on aircraft configuration. Radar may remain useful under lighting or visibility conditions that challenge cameras, while LiDAR can provide accurate geometric constraints in structured environments. Diversity is valuable because environmental conditions that degrade one navigation modality may leave another usable.

Barometric altitude, radar altitude, laser range measurements, magnetometers, and air-data sensors can constrain individual navigation dimensions even when they cannot independently provide full three-dimensional position. A fault-tolerant estimator can combine these partial observations to limit drift. The objective is to preserve enough state accuracy for stabilization, obstacle avoidance, containment, and safe recovery rather than reproducing nominal GNSS performance.

Fallback behavior should depend on the quality of the remaining navigation solution. If visual-inertial or terrain-relative navigation remains highly reliable, the UAV may continue toward a predefined recovery location. If only short-term inertial navigation is available, the safer response may be to hold briefly, climb or descend to a predefined safety profile, follow a limited dead-reckoning route, or initiate landing before position uncertainty becomes excessive.

Position uncertainty must be propagated explicitly during GNSS-denied operation. As uncertainty grows, guidance and geofencing algorithms should increase safety margins around obstacles, restricted airspace, terrain, and landing areas. A UAV should not continue behaving as if its estimated coordinates were exact after external position references have disappeared. Navigation confidence must directly influence permissible mission behavior.

Return-to-home logic requires special protection because spoofed GNSS can transform a safety feature into a hazardous command. The aircraft should not automatically execute return-to-home using a location solution already classified as suspicious. Recovery logic should evaluate home-position integrity, current navigation confidence, reachable alternatives, obstacle information, and available fallback sensors before selecting a return, hold, diversion, or landing strategy.

Recovery of GNSS trust should be gradual. When apparently normal signals return after interference, the receiver and navigation system should perform consistency checks before restoring full weighting. Reacquired position, velocity, timing, satellite geometry, and signal characteristics can be compared with the independently propagated state. Sudden acceptance of a recovered but incorrect solution could recreate the original hazard.

Logging is important for both safety analysis and post-flight diagnosis. The UAV should record receiver metrics, satellite observations when available, navigation innovations, estimator health, detection events, source-selection decisions, and fallback transitions. Accurate timestamps allow engineers to reconstruct whether an anomaly originated in RF reception, receiver processing, sensor fusion, flight-control software, or the external environment.

Verification should expose the navigation system to realistic GNSS degradation without relying solely on nominal flight testing. Simulation and hardware-in-the-loop environments can inject satellite loss, measurement bias, slowly drifting false positions, velocity errors, timing anomalies, multipath-like disturbances, and complete outages. Testing should verify detection latency, false-alarm behavior, estimator isolation, fallback accuracy, uncertainty growth, and recovery transitions.

Field validation should be performed within controlled and legally compliant environments using approved test methods, because intentional radio-frequency interference can affect systems beyond the UAV under test. The purpose of validation is to demonstrate resilient detection and safe fallback behavior, not to reproduce uncontrolled interference. Recorded datasets and RF-safe laboratory equipment can support repeatable evaluation across many threat and degradation scenarios.

A resilient GNSS safety architecture therefore combines signal monitoring, navigation consistency checking, multi-sensor fusion, explicit integrity states, measurement isolation, and diversified fallback navigation. The objective is not to guarantee that satellite navigation will always remain available, but to ensure that loss or corruption of GNSS cannot silently become loss of aircraft control, containment, or safe recovery capability.

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

##  

## 08.05. Battery Failure Emergency Landing Trigger [w/Code]

![](images/image5.png){width="7.268055555555556in" height="7.268055555555556in"}

Battery failure is a critical hazard for electrically powered UAVs because stored electrical energy supports propulsion, flight-control computers, sensors, communications, and safety equipment. An emergency landing trigger must therefore detect not only low remaining charge but also abnormal electrical, thermal, and structural battery conditions that can rapidly reduce available power or create an immediate safety threat.

Battery safety logic begins with continuous monitoring by the Battery Management System (BMS) and aircraft energy-management functions. Important observations include pack voltage, individual cell voltage, current, temperature, state of charge, state of health, internal resistance, power capability, and communication status. These measurements should be interpreted together because a single value rarely describes the complete condition of a heavily loaded UAV battery.

State of Charge (SoC) estimates the remaining usable energy, but SoC alone is insufficient for emergency decisions. Estimation error, aging, temperature, discharge rate, and cell imbalance can cause actual available energy to differ significantly from the displayed percentage. The flight system should therefore combine SoC with voltage behavior, consumed charge, predicted mission energy, load demand, and historical battery characteristics when determining remaining endurance.

State of Health (SoH) describes longer-term degradation in capacity and power capability. An aged battery may report substantial remaining charge but experience excessive voltage sag when high thrust is commanded. For heavy cargo UAVs, this distinction is especially important because takeoff, climb, gust rejection, and emergency maneuvering can impose large transient loads. Energy safety must consider whether the battery can deliver required power, not merely whether energy remains.

Cell-level monitoring is essential because pack-level voltage can hide a weak cell. One deteriorated cell may fall below its safe voltage limit while the total pack voltage still appears acceptable. The BMS should detect cell imbalance, abnormal voltage deviation, excessive internal resistance, and rapid cell-voltage collapse. A weak cell under high load can become the limiting factor that determines whether continued flight remains safe.

Voltage sag should be interpreted dynamically. During high-current operation, terminal voltage decreases because of internal resistance and electrochemical behavior, then partially recovers when load decreases. A simple fixed voltage threshold can therefore cause either premature landing or dangerously late intervention. Better logic considers current, temperature, expected sag, recovery behavior, and the voltage margin required for the remaining mission and landing sequence.

Temperature monitoring provides another major safety channel. Batteries operating outside their acceptable temperature range may suffer reduced power capability, accelerated degradation, or thermal failure. Rapid temperature rise can be more significant than absolute temperature alone. The controller should monitor temperature gradients and differences between modules or cells because localized heating may indicate internal damage that is not visible from average pack temperature.

Thermal runaway requires a response fundamentally different from ordinary low-energy management. Evidence of uncontrolled heating, severe cell abnormality, smoke detection where available, or rapidly escalating electrical anomalies should trigger immediate safety action. Continuing toward a distant home location may be inappropriate because the battery condition can deteriorate faster than predicted. The priority shifts from mission preservation toward minimizing airborne exposure and landing promptly.

Battery fault classification can separate advisory, caution, critical, and emergency conditions. An advisory condition may indicate gradual degradation while sufficient reserve remains. A caution can initiate mission replanning or return-to-home. A critical state can restrict maneuvering and select a nearby landing site. An emergency state should trigger immediate landing or another predefined containment response when continued powered flight can no longer be assured.

Emergency landing decisions should use predicted energy at landing rather than only current battery percentage. The aircraft can estimate energy required to reach candidate sites, compensate for wind, climb or descent, expected hover time, payload mass, temperature, and propulsion efficiency, then add a defined safety reserve. If predicted remaining energy falls below the required landing reserve, recovery action should begin before physical battery limits are reached.

Reserve policy should reflect uncertainty. Energy prediction contains errors from wind estimates, battery models, trajectory changes, aging, and unexpected maneuvering. A fixed reserve percentage may be inadequate across different missions. Dynamic reserve calculation can increase the margin during high payload, strong wind, cold temperature, degraded battery health, uncertain navigation, or operations far from suitable landing locations.

The landing trigger should distinguish return-to-home from immediate landing. Return-to-home is appropriate only when sufficient energy and power margin exist to complete the route safely. If voltage is collapsing, thermal conditions are worsening, or the required return energy exceeds the predicted reserve, attempting to reach the launch point can increase risk. The system should instead select a closer reachable landing location.

Reachable landing-site evaluation connects energy management with navigation and perception. The UAV can assess distance, required altitude changes, wind, terrain, obstacles, surface characteristics, population exposure, and available energy. A physically close location is not necessarily the safest if reaching it requires climbing, hovering, or complex maneuvering. Candidate selection should minimize total risk rather than simply minimizing geographic distance.

For cargo UAVs, payload mass strongly affects emergency energy calculations. A heavily loaded aircraft may require much greater hover power and may have little thrust margin as voltage decreases. The energy-management system should use current mass and center-of-gravity information whenever available. Predictions based on an unloaded vehicle model can significantly overestimate remaining endurance and produce dangerously late landing decisions.

Multiple battery packs can improve fault tolerance when their electrical architecture supports genuine isolation. Parallel packs connected without appropriate protection may allow a failed pack to disturb healthy packs through fault currents or bus collapse. Contactors, current sensors, isolation devices, independent monitoring, and controlled cross-connection can permit a damaged pack to be disconnected while preserving power from healthy energy sources.

After isolating a battery module, the aircraft must immediately recalculate available energy and power. A vehicle that remains airborne after losing one of two packs may have only half its nominal capacity and reduced peak power. Flight-control and mission-management systems should therefore receive updated capability limits so that aggressive acceleration, high climb rates, or unnecessary hovering are avoided during degraded operation.

Power prioritization can extend safe operation when energy becomes limited. Propulsion, flight-control computing, essential navigation, communication, and emergency sensors should receive priority over nonessential payload processing, high-power mission equipment, or convenience functions. Controlled load shedding reduces electrical demand and preserves the energy required to maintain stabilization, reach a landing site, and complete the final descent.

Emergency landing logic should remain coordinated with propulsion capability. As battery voltage falls, maximum motor speed and available thrust can decrease even before total energy is exhausted. The aircraft may therefore lose climb or hover capability while still reporting remaining capacity. Power-aware flight control should estimate available thrust continuously and prevent guidance commands from demanding forces that the battery and propulsion system can no longer provide.

The final landing phase requires protected energy because hover, obstacle avoidance, flare, or precision descent can consume substantial power. Landing reserve should therefore include energy for approach uncertainty and a limited go-around or repositioning capability when the aircraft design supports it. Consuming nearly all available energy merely to reach the landing area can leave insufficient control authority during the most safety-critical portion of recovery.

Communication with the operator should provide meaningful state information without making safe recovery dependent on human response. Alerts can report battery condition, predicted endurance, selected recovery action, landing location, and degraded capabilities. However, when a critical threshold is reached and communication is unavailable or delayed, onboard autonomy should execute the predefined safety response rather than waiting indefinitely for operator confirmation.

Trigger hysteresis and persistence logic can prevent unstable switching between normal, return, and emergency modes. Small voltage fluctuations near a threshold should not repeatedly change the aircraft's mission state. At the same time, rapidly developing faults require immediate escalation. The trigger design can therefore combine persistence for slowly varying conditions with rate-sensitive overrides for fast voltage collapse, overcurrent, or thermal escalation.

Sensor and BMS failures must themselves be considered. Lost battery communication, frozen measurements, implausible SoC values, inconsistent current integration, or disagreement between redundant voltage measurements can make energy status uncertain. A safety-oriented controller should treat severe loss of battery observability as a degradation condition and increase reserve margins or initiate recovery rather than assuming that the last valid value remains correct.

Emergency landing triggers should be deterministic enough for verification while remaining adaptive to operational context. Thresholds, prediction models, uncertainty margins, state transitions, and override conditions should be explicitly defined and traceable to safety requirements. The architecture must make clear which conditions generate warnings, which initiate return-to-home, which force diversion, and which command immediate landing.

Software-in-the-loop and hardware-in-the-loop testing can evaluate battery safety logic across large combinations of charge level, temperature, payload, wind, aging, and flight phase. Fault injection should include weak cells, sudden voltage collapse, sensor bias, BMS communication loss, overtemperature, pack disconnection, and incorrect SoC estimates. Verification should measure trigger timing, trajectory feasibility, reserve at touchdown, and response to conflicting indicators.

Physical validation can use controlled discharge tests, propulsion benches, environmental chambers, and carefully bounded flight tests to characterize real battery behavior. Measured voltage sag, thermal response, usable capacity, and power limits can improve onboard models. Testing across new and aged packs is important because safety logic calibrated only with healthy batteries may underestimate the degradation encountered during operational service.

Post-flight data analysis should preserve battery voltage, cell measurements, current, temperature, BMS status, estimated SoC and SoH, predicted energy, propulsion demand, trigger transitions, and landing decisions. These records allow engineers to compare predictions with actual behavior, refine thresholds, identify aging trends, and determine whether emergency responses were initiated with appropriate safety margins.

A robust battery-failure emergency landing architecture therefore combines cell-level monitoring, energy and power prediction, thermal protection, fault isolation, dynamic reserves, load shedding, reachable-site assessment, and autonomous recovery. The objective is to trigger action early enough that landing remains a controlled choice, rather than waiting until electrical energy or propulsion capability deteriorates to the point where descent becomes unavoidable and uncontrolled.

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

##  

## 08.06. Comm Link Loss Contingency Behavior C2 Link [w/Code]

![](images/image6.png){width="7.268055555555556in" height="7.268055555555556in"}

The Command and Control (C2) link connects a UAV with its remote pilot station, fleet manager, or supervisory control system and carries commands, telemetry, health information, mission updates, and safety status. Loss or severe degradation of this link must be treated as an expected operational contingency rather than an exceptional software event, particularly for autonomous and beyond-visual-line-of-sight operations.

C2 link failure can result from terrain masking, excessive range, antenna blockage, interference, network congestion, equipment malfunction, ground-station failure, power loss, or communication infrastructure outage. Degradation may occur gradually through increasing latency and packet loss or suddenly through complete disconnection. The UAV should therefore monitor communication quality continuously instead of relying only on a binary connected-or-disconnected indication.

Link-health monitoring can include received signal strength, signal-to-noise ratio, packet error rate, packet loss, latency, jitter, heartbeat reception, sequence counters, retransmission activity, and message age. These indicators should be evaluated over suitable time windows because short disturbances may be harmless while persistent degradation can indicate an approaching loss of command authority or telemetry availability.

Communication integrity is as important as availability. A link may remain technically connected while delayed, duplicated, corrupted, or out-of-sequence messages make remote control unsafe. Commands should therefore carry timestamps, sequence information, validity periods, and integrity protection where appropriate. The flight system should reject stale or invalid commands rather than executing them merely because they arrived through an authenticated communication channel.

C2 monitoring should distinguish uplink and downlink failures. The UAV may continue receiving commands while telemetry cannot reach the operator, or the operator may receive aircraft data while commands fail to reach the vehicle. These asymmetric conditions require different responses because the onboard system and ground station can have different understandings of link status. Explicit link-state estimation should account for both communication directions.

A contingency state machine can represent nominal, degraded, temporarily lost, confirmed lost, recovery, and restored conditions. Transition thresholds should reflect aircraft dynamics, mission phase, network characteristics, and operational risk. Brief packet gaps should not trigger unnecessary emergency maneuvers, while prolonged command loss near obstacles or controlled airspace may require much faster autonomous intervention.

The first response to degradation can be to stabilize communication demand rather than immediately terminate the mission. Nonessential data streams may be reduced, telemetry rates lowered, video transmission limited, or alternative communication paths activated. Safety-critical command and status messages should receive priority so that limited bandwidth remains available for essential control, health reporting, and contingency coordination.

Redundant communication links can improve availability when they do not share the same failure domain. A UAV may combine cellular, dedicated radio, satellite, mesh, or other approved communication technologies according to mission requirements. Redundancy is meaningful only when antenna placement, power supply, network infrastructure, software routing, and environmental vulnerabilities are sufficiently independent to prevent one fault from disabling every path.

Automatic link switching should preserve command authority and message ordering. When the primary communication path becomes unreliable, the system can transfer traffic to a secondary path, but duplicate or delayed commands from the previous link must not create conflicting aircraft behavior. Session state, command sequence numbers, source identity, and control ownership should remain consistent throughout the transition.

If all usable C2 paths are lost, onboard autonomy becomes responsible for maintaining safe aircraft behavior. The UAV should not simply continue indefinitely with the last received command unless that behavior has been explicitly demonstrated to be safe. The contingency manager should consider aircraft state, mission phase, navigation integrity, remaining energy, weather constraints, airspace boundaries, and available recovery locations.

A short-duration link loss may justify a controlled hold behavior. A multirotor can maintain position or follow a small containment pattern when navigation confidence is high, while a fixed-wing aircraft may require a predefined loiter pattern because it cannot hover. The hold duration should be bounded by energy reserve, airspace constraints, navigation uncertainty, and the probability that communication can be restored.

Return-to-home is a common contingency action, but it should not be treated as universally safe. The home route may cross terrain, obstacles, restricted airspace, poor weather, or areas with weak communication coverage. GNSS or navigation integrity may also be degraded at the same time as C2 loss. Recovery logic should therefore verify that the home location and route remain valid before committing to an autonomous return.

Diversion to an alternate recovery site can be safer than returning to the launch point. The UAV can evaluate reachable landing locations based on energy, terrain, population exposure, airspace constraints, weather, and navigation confidence. Cargo UAV operations may predefine several contingency sites along the route so that loss of C2 does not force a long return flight when a nearby safe recovery location is available.

Mission continuation after C2 loss should be permitted only when explicitly supported by the operational concept and safety analysis. Highly autonomous UAVs may safely complete a limited segment of a mission without continuous human commands, but this requires reliable navigation, obstacle avoidance, geofencing, health monitoring, and contingency management. Continued flight should remain bounded by predefined time, distance, or geographic constraints.

Geofencing becomes particularly important during lost-link operation because remote intervention is unavailable. The onboard system should independently enforce altitude, geographic, and airspace limits even when the ground station cannot provide updates. If position uncertainty increases, containment margins should expand accordingly, and the vehicle may need to transition toward landing before uncertainty threatens restricted boundaries.

Energy management should influence every lost-link decision. Holding for communication recovery consumes energy that may later be needed for return or landing. The contingency manager should continuously compare the probability and benefit of link recovery against the shrinking energy reserve. A fixed waiting period can be inappropriate when wind, payload, battery degradation, or distance to a safe landing site changes the available margin.

Flight phase strongly affects contingency behavior. C2 loss during cruise may allow time for link recovery or route diversion, while loss during takeoff, final approach, cargo release, or operations near structures can demand an immediate predefined response. The contingency architecture should therefore associate each mission phase with safe actions rather than applying one identical lost-link procedure throughout the entire flight.

For cargo UAVs, payload condition can modify the contingency response. An externally suspended load may make holding in turbulent conditions undesirable, while a heavy payload can reduce return range and landing options. If payload release capability exists, it should not be automatically used merely because communication is lost. Any release requires separate safety criteria and assurance that people and property below are not exposed to unacceptable risk.

The onboard flight-control system should retain authority over basic stabilization regardless of C2 availability. Remote commands should normally represent desired mission behavior rather than bypassing fundamental attitude, rate, actuator, and envelope protections. This separation ensures that loss of the supervisory communication path does not remove the local control loops required to keep the aircraft dynamically stable.

Command authority management is important when multiple ground stations, remote pilots, or supervisory systems are available. During communication recovery, the aircraft must know which source is authorized to resume control. Conflicting commands from an old session and a newly established session should not both be accepted. Control transfer should use deterministic ownership, authentication, session validation, and explicit state synchronization.

Reconnection should not immediately restore full remote authority without checking system consistency. The ground station may have outdated aircraft state, while the UAV may have changed route, altitude, mode, or landing destination during the outage. The aircraft and operator system should exchange current state, mission version, contingency mode, navigation health, and command sequence information before normal supervisory control resumes.

Human-machine interface design should make lost-link behavior understandable to the operator. Before flight, the configured contingency policy should be visible and verifiable. During degradation, the ground station should indicate link quality, last valid communication time, aircraft contingency state, and expected autonomous action. After reconnection, it should clearly report actions the UAV performed while communication was unavailable.

Logging should preserve both communication and flight context. Relevant records include link-quality metrics, packet loss, latency, heartbeat events, command timestamps, mode transitions, alternative-link activation, autonomous decisions, navigation confidence, energy state, and recovery actions. Time-aligned records allow engineers to determine whether an event originated from radio performance, network infrastructure, ground software, onboard communication, or contingency logic.

Verification should include realistic communication degradation rather than only complete link disconnection. Software-in-the-loop and hardware-in-the-loop testing can introduce latency, jitter, packet loss, bandwidth restriction, asymmetric uplink and downlink failure, stale commands, network switching, repeated reconnection, and total outage. Tests should confirm deterministic state transitions and safe behavior under combinations of communication and navigation faults.

Field testing can progressively extend from short controlled interruptions to representative operational scenarios while maintaining independent safety supervision. Engineers should measure detection time, mode-transition latency, containment performance, energy consumption, link-switching behavior, and recovery success. Testing should also confirm that temporary communication disturbances do not create excessive nuisance returns or unnecessary emergency landings.

Safety analysis must address common-cause failures between C2 links and other systems. A shared power source, common antenna location, onboard network switch, software router, or ground infrastructure can defeat apparently redundant communication channels. Independence analysis should therefore extend beyond radio hardware to power, networking, timing, software, ground systems, and environmental exposure.

A robust C2 lost-link architecture combines continuous link-health monitoring, communication diversity, deterministic state management, autonomous stabilization, navigation-aware recovery, energy-aware decision making, containment, and controlled reconnection. The objective is not to guarantee uninterrupted communication, but to ensure that temporary or permanent loss of remote connectivity never leaves the UAV without a predictable and safety-prioritized course of action.

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

##  

## 08.07. Geofence and Volume Constraint Enforcement [w/Code]

![](images/image7.png){width="7.268055555555556in" height="7.268055555555556in"}

Geofence and volume-constraint enforcement provides a safety boundary that prevents a UAV from entering prohibited regions or leaving an approved operating volume. Unlike simple two-dimensional map boundaries, a robust system represents horizontal limits, altitude floors and ceilings, terrain relationships, temporary restrictions, and mission-specific containment areas as constraints that remain active throughout autonomous and remotely supervised flight.

A geofence can be classified as keep-in or keep-out. A keep-in boundary defines the volume within which the aircraft is authorized to remain, while a keep-out boundary represents airspace, infrastructure, terrain, population areas, or other regions that must not be entered. Complex missions can use both simultaneously, creating a permitted corridor surrounded by multiple exclusion volumes with different safety priorities.

Three-dimensional volume representation is important because UAV operations are inherently spatial. A horizontal polygon alone cannot distinguish an authorized route above one structure from a prohibited altitude layer or terrain hazard. Operational volumes should therefore include latitude and longitude or local coordinates, lower and upper altitude limits, reference datums, and where necessary time validity so that each constraint has an unambiguous physical meaning.

Altitude references require particular care. Barometric altitude, GNSS altitude, height above ground level, and height relative to a local reference can differ substantially. A constraint defined using one reference should not be compared directly with a state estimate using another without conversion. The geofence architecture should explicitly maintain altitude datum information and account for terrain elevation and sensor uncertainty when evaluating vertical boundaries.

Static geofences are loaded before flight and normally remain unchanged during the mission. Dynamic geofences can represent temporary airspace restrictions, emergency zones, moving operational boundaries, or updated traffic-management constraints. Dynamic updates require validation because a newly received boundary could conflict with the current aircraft position or make the planned route infeasible. The UAV must always retain a safe response when an update cannot be followed immediately.

Boundary enforcement should occur onboard rather than depending exclusively on a ground station. A C2 communication outage can remove remote supervision at exactly the time containment is most important. The aircraft should therefore store safety-critical constraints locally and continue enforcing them when external communication is unavailable. Ground systems can provide updates and monitoring, but basic containment should remain an independent onboard capability.

Geofence monitoring begins with a trusted estimate of aircraft position and uncertainty. The navigation system provides position, altitude, velocity, and associated covariance or integrity information. The containment function then evaluates not only the estimated point location but also the uncertainty surrounding that estimate. A vehicle close to a boundary with large position uncertainty can present greater risk than a vehicle with the same estimated position and high navigation confidence.

Safety margins should therefore expand when navigation uncertainty increases. A nominal geofence boundary can be surrounded by an internal protection buffer that accounts for localization error, control tracking error, wind disturbance, communication delay, vehicle dimensions, and stopping or turning distance. The aircraft reacts to the protective boundary early enough that normal control errors do not become actual violations of the authorized volume.

Predictive boundary monitoring improves protection beyond simple position checks. The system can project the UAV trajectory using current velocity, acceleration, commanded motion, wind estimates, and vehicle dynamics. If the predicted path intersects a constraint within a defined look-ahead horizon, corrective action can begin before the aircraft physically approaches the boundary. This is especially important for fast or heavy UAVs with significant momentum.

Constraint enforcement can operate at several levels of intervention. At a large distance from the boundary, mission planning can generate routes that naturally avoid restricted regions. Closer to the boundary, guidance can modify velocity or trajectory commands. If the risk becomes immediate, flight-control protection can limit commands directly. Layered enforcement reduces dependence on any single planning or control function.

Command limiting prevents a remote pilot, autonomous planner, or higher-level agent from requesting motion that would knowingly violate a safety boundary. Desired velocity, position, acceleration, or waypoint commands can be checked against the active constraint set before acceptance. Unsafe commands can be clipped, projected onto an admissible direction, rejected, or replaced with a predefined recovery command depending on system design.

Trajectory feasibility should consider vehicle dynamics rather than treating the UAV as a point that can stop instantly. Maximum braking acceleration, turn radius, climb and descent rates, actuator limits, payload mass, wind, and propulsion condition determine how much distance is required to avoid a boundary. Heavy cargo UAVs may require substantially larger containment margins than small multirotors because their kinetic energy and response times are greater.

Wind can create significant containment risk even when commanded motion points away from a boundary. The system should estimate whether available thrust and control authority are sufficient to resist the expected disturbance. A strong crosswind near the edge of an operational corridor may make the nominal route unsafe. Guidance can shift the planned path inward or reduce speed to preserve sufficient disturbance-rejection margin.

Geofence enforcement must remain coordinated with fault-tolerant control. A propulsion failure, battery degradation, sensor fault, or actuator limitation can reduce the vehicle's ability to respect previously feasible boundaries. When control capability changes, the containment system should update reachable sets and margins. A boundary that was easily avoidable under nominal conditions may become critical after loss of thrust or maneuver authority.

GNSS degradation presents a special challenge because many geofences are defined in geographic coordinates. If GNSS becomes unreliable, the system should not simply disable containment. Visual-inertial navigation, terrain-relative localization, map matching, or other fallback sources can maintain a local estimate. At the same time, increasing position uncertainty should produce more conservative margins and may eventually trigger landing before containment can no longer be assured.

Geofence behavior should distinguish an approaching boundary from an actual violation. An approach condition may initiate warning, trajectory correction, speed reduction, or return toward the center of the authorized volume. A confirmed violation can require stronger actions such as immediate re-entry, mission termination, diversion, or landing. The selected response should avoid aggressive maneuvers that create a greater hazard than the boundary excursion itself.

Multiple constraints can overlap or conflict. A return-to-home trajectory may intersect a keep-out zone, while an emergency landing site may lie outside the normal keep-in volume. Constraint management therefore requires priorities and exception rules that are explicitly defined by safety analysis. Emergency behavior should not improvise which restriction to violate; the architecture should determine how competing safety objectives are resolved.

Time-dependent constraints add another dimension to volume enforcement. A corridor may be authorized only during a specific mission window, or temporary restricted airspace may become active at a scheduled time. The UAV should consider future activation when planning its trajectory so that it does not enter a region from which it cannot exit before the constraint changes. Reliable onboard time is therefore part of containment integrity.

Dynamic updates should include version information, timestamps, validity intervals, source identification, and integrity checks. The aircraft should reject malformed, stale, unauthorized, or internally inconsistent constraint data. If an update is lost during communication interruption, the onboard system should continue using the last validated safety dataset according to predefined validity rules rather than silently removing containment protection.

Terrain and obstacle constraints can complement regulatory geofences. A minimum terrain-clearance surface can prevent trajectories from descending into rising ground, while three-dimensional exclusion volumes can protect towers, buildings, cranes, power infrastructure, or sensitive facilities. These constraints can be incorporated into the same evaluation framework while retaining information about their different origins, priorities, and uncertainty.

For cargo UAVs, landing and loading zones may require specialized local volumes. Approach corridors, hover boxes, touchdown areas, and payload-transfer regions can each have different speed, altitude, and lateral-position limits. Enforcement can become progressively tighter as the aircraft approaches people, equipment, or infrastructure, creating a controlled transition from en-route navigation to precision terminal operations.

Containment should also consider uncertainty in the boundaries themselves. Terrain databases, surveyed infrastructure coordinates, temporary construction information, and externally supplied airspace data may contain errors. Safety margins should therefore reflect both aircraft localization uncertainty and constraint-data uncertainty. Treating every boundary coordinate as mathematically exact can produce an unrealistic assessment of actual separation.

Logging is essential for demonstrating compliance and diagnosing near-boundary events. Records should include active geofence versions, aircraft position, navigation uncertainty, predicted trajectories, margin calculations, constraint warnings, command modifications, intervention levels, and any boundary violations. Time synchronization allows investigators to reconstruct whether an event originated from navigation error, planning behavior, control performance, or incorrect constraint data.

Verification should exercise the system across normal and abnormal conditions. Software-in-the-loop testing can generate thousands of approaches to boundaries with different speeds, angles, winds, navigation errors, and vehicle failures. Hardware-in-the-loop testing can introduce delayed navigation, corrupted geofence updates, communication loss, sensor degradation, and actuator limitations while confirming that onboard containment remains deterministic.

Flight testing should begin with large safety buffers and progressively evaluate representative operational boundaries under controlled supervision. Tests can measure minimum separation, prediction accuracy, intervention timing, braking distance, trajectory smoothness, and behavior during link loss or navigation degradation. Validation should demonstrate that protective actions occur before physical boundary violations across the intended operating envelope.

A robust geofence and volume-constraint architecture therefore combines validated spatial data, three-dimensional boundaries, navigation integrity, uncertainty-aware margins, predictive trajectory checking, command limiting, dynamic feasibility analysis, and layered intervention. The objective is not merely to detect that a UAV has crossed a virtual line, but to maintain sufficient awareness and control authority to prevent unsafe boundary violations before they occur.

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

##  

## 08.08. Safety Monitor Watchdog and Heartbeat Design [w/Code]

![](images/image8.png){width="7.268055555555556in" height="7.268055555555556in"}

A UAV safety monitor provides an independent supervisory layer that observes flight-critical computers, sensors, communication interfaces, power systems, and control functions without relying entirely on the software being monitored. Watchdogs and heartbeat mechanisms form core elements of this layer by detecting stalled execution, missed deadlines, communication loss, abnormal state transitions, and other failures that may not be visible through normal functional outputs.

A watchdog is fundamentally a timing-based protection mechanism. A monitored processor or software task must periodically demonstrate that it is executing correctly within an expected interval. If the required service signal does not arrive before a timeout expires, the watchdog assumes that execution has stalled or become unreliable. The resulting response may include fault declaration, processor reset, channel isolation, redundancy switchover, or transition to a predefined safe mode.

Simple watchdog designs can detect complete software freezes, but they cannot prove that an application is functioning correctly. A defective program may continue servicing the watchdog while producing invalid control outputs. Safety-oriented designs therefore combine execution timing supervision with state plausibility checks, control-command validation, sensor consistency monitoring, and cross-channel comparison so that both inactive and actively erroneous failures can be detected.

Independent hardware watchdogs provide stronger protection than watchdog functions implemented only within the same processor being supervised. If a processor suffers scheduler failure, memory corruption, clock malfunction, or software deadlock, an internal software watchdog may fail together with the monitored application. An external device with independent power, timing, and reset authority can preserve the ability to detect and respond to such common failures.

Watchdog timeout selection must reflect the dynamics of the monitored function. A timeout that is too short can generate nuisance resets because of normal scheduling jitter or temporary computational load, while an excessively long timeout allows hazardous failures to persist. Flight-control loops, navigation estimators, mission managers, communication processes, and payload applications can therefore require different supervision intervals according to their safety significance and execution rates.

Windowed watchdogs strengthen timing supervision by requiring a service event within a defined time window rather than merely before a maximum timeout. A task that responds too early, too frequently, or too late can then be identified as abnormal. This helps detect corrupted control flow in which software repeatedly reaches the watchdog service instruction without completing the intended processing sequence.

A heartbeat mechanism extends supervision across processors, software services, sensors, network nodes, and ground communication systems. Each monitored component periodically transmits a compact message indicating that it remains active. Heartbeats can contain more than a simple alive flag, including sequence counters, operating mode, health status, execution cycle number, software version, timestamp, and selected diagnostic information.

Sequence counters are important because repeatedly receiving the same heartbeat should not be interpreted as evidence of continued operation. A frozen communication buffer or repeated network packet could otherwise make a failed component appear healthy. Monitors should verify that sequence values progress correctly and that messages arrive within expected timing limits while also detecting duplicates, missing messages, and unexpected resets.

Timestamps provide additional evidence about data freshness. A heartbeat can arrive through a functioning network even though the originating application stopped updating its internal state. Comparing source timestamps with local reception time allows the monitor to detect stale information and excessive transport latency. Reliable time synchronization improves this capability, particularly in distributed UAV architectures with several computing and sensor nodes.

Heartbeat monitoring should distinguish component failure from communication-path failure. If messages from several unrelated devices disappear simultaneously, a network switch, bus, power domain, or communication gateway may be the common cause. The safety monitor should therefore understand system topology and failure domains rather than independently declaring every missing node defective without considering shared infrastructure.

Hierarchical monitoring can improve scalability in large UAV systems. Local supervisors can monitor high-rate tasks and hardware interfaces within individual computing nodes, while a higher-level safety manager observes node health, communication status, navigation integrity, propulsion capability, and mission state. This arrangement reduces communication overhead while preserving rapid local fault detection and coordinated aircraft-level response.

The safety monitor should be sufficiently independent from primary mission software. A complex autonomous stack may contain perception, planning, mapping, AI inference, and mission logic with high computational complexity. The safety monitor can use simpler, bounded, and highly verifiable logic to supervise critical variables such as attitude, altitude, speed, position, geofence status, battery condition, control commands, and processor health.

Command monitoring allows the safety layer to detect outputs that are computationally valid but physically unsafe. A flight-control or autonomy process may remain alive and continue sending heartbeats while requesting excessive bank angle, thrust, descent rate, or velocity. Independent limit checking can reject, constrain, or override commands that exceed verified aircraft envelopes, providing protection against systematic software failures.

State plausibility monitoring applies physical and temporal constraints to measured and estimated aircraft states. Position cannot normally change by kilometers within one control cycle, battery charge should not increase unexpectedly during high-power flight, and actuator feedback should remain compatible with commanded motion. Such rules can identify corrupted memory, sensor failures, communication errors, or estimator divergence even when all tasks continue executing.

Cross-monitoring between redundant flight-control computers provides another detection layer. Each FCC can compare the other channel's mode, estimated state, control output, execution counter, and health declaration. A separate safety monitor can arbitrate when channels disagree, reducing the risk that two computers detecting disagreement cannot determine which channel is faulty. Independence of the arbiter remains essential to avoid creating a new single point of failure.

Fault responses should be proportional to both severity and confidence. One missed heartbeat may justify a warning or temporary degraded state, whereas repeated missed messages or an external watchdog timeout can justify channel isolation. A physically impossible control command may require immediate intervention without persistence delay. The safety architecture should therefore distinguish transient anomalies from failures requiring rapid protective action.

Persistence counters and hysteresis help prevent unstable fault declarations. Temporary processor overload, network congestion, or scheduling jitter can occasionally delay a heartbeat without indicating permanent failure. Requiring a defined number of consecutive misses can reduce nuisance transitions. However, high-consequence functions may require shorter persistence or immediate response, so timing policies should be assigned according to hazard analysis.

Recovery after a watchdog event must be controlled carefully. Automatically rebooting a failed processor can restore functionality, but repeated resets can create unstable aircraft behavior or conceal a persistent hardware defect. The system should limit restart attempts, record reset causes, verify initialization, synchronize state with healthy channels, and confirm readiness before restoring control authority to the recovered component.

A restarted flight-control computer should not immediately issue actuator commands using uninitialized or stale state. Navigation estimates, control integrators, mission modes, reference trajectories, and actuator histories may need synchronization before reintegration. A warm-start strategy can obtain validated state from healthy channels, while a cold-start strategy may require a longer period of observation before the recovered computer becomes eligible for command authority.

Watchdog architecture must also consider power failures. A monitor powered from the same failed rail as the processor it supervises cannot report or recover from that power loss. Safety-critical supervision may therefore use independent or protected power domains and monitor voltage, reset lines, clock activity, and power-good signals. This enables the system to distinguish software failure from electrical loss and select an appropriate response.

Network watchdogs supervise communication infrastructure rather than individual applications. Bus utilization, message timing, error counters, gateway health, switch status, and communication partitions can be monitored to detect network-level degradation. Safety-critical messages may have bounded transmission deadlines, and failure to meet those deadlines can trigger reconfiguration to redundant buses or simplified local-control modes.

Sensor heartbeat supervision should include data validity, not merely message presence. An IMU can continue transmitting packets while producing frozen, saturated, or biased measurements. Monitors should evaluate update counters, measurement variance, physical ranges, cross-sensor consistency, calibration status, and built-in-test results. A sensor should be considered healthy only when both communication and measurement behavior remain credible.

Safety-monitor outputs should feed a centralized or distributed fault-management function that maintains the current health configuration of the aircraft. This configuration identifies which computers, sensors, communication links, power sources, and actuators remain trustworthy. Guidance and control functions can then adapt their behavior to the available resources instead of repeatedly rediscovering system health independently.

Logging of watchdog and heartbeat events is essential for verification and maintenance. Records should include timeout values, missed sequence numbers, reset causes, processor states, network conditions, mode transitions, fault declarations, recovery attempts, and synchronization results. Accurate timestamps allow engineers to reconstruct the sequence of events and distinguish the initiating failure from secondary effects.

Software-in-the-loop testing can inject task stalls, deadlocks, timing overruns, invalid states, frozen counters, and incorrect commands to verify monitoring logic. Hardware-in-the-loop testing can add processor resets, network interruption, clock faults, power disturbances, sensor freezes, and delayed messages. Verification should confirm detection latency, fault classification, protective response, and successful or rejected recovery.

Stress testing is especially important because watchdog behavior can change when processors approach computational limits. High perception workloads, logging bursts, communication congestion, and simultaneous fault processing may increase scheduling delays. Testing should demonstrate that safety-critical supervision retains timing guarantees under worst-case credible loads and that nonessential workloads cannot starve watchdog or heartbeat functions.

A robust safety-monitor architecture therefore combines independent watchdogs, information-rich heartbeats, timing supervision, sequence and freshness checks, physical plausibility monitoring, command validation, topology-aware fault isolation, and controlled recovery. Its purpose is not simply to determine whether software is running, but to establish continuous evidence that critical UAV functions remain timely, credible, coordinated, and safe enough to retain control authority.

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

##  

## 08.09. Software Safety Case DO 178C DAL A B [w/Code]

![](images/image9.png){width="7.268055555555556in" height="7.268055555555556in"}

A software safety case for a safety-critical UAV provides structured evidence that airborne software performs its intended functions with an acceptable level of assurance and that software failures have been systematically addressed. DO-178C provides the principal framework for software life-cycle assurance, while the assigned Design Assurance Level determines the rigor of planning, development, verification, configuration management, and certification evidence.

The software assurance process begins with aircraft and system safety assessment rather than with source code. Functional Hazard Assessment identifies the consequences of function loss, malfunction, misleading output, or unintended activation. System-level analyses then allocate safety requirements to hardware and software components. The resulting failure classification provides the basis for determining the required software Design Assurance Level.

DAL A applies when anomalous software behavior could contribute to a catastrophic failure condition, while DAL B is associated with hazardous or severe-major consequences under the applicable certification framework. The distinction is important because DAL A requires the highest level of software assurance. Both levels demand disciplined engineering, but DAL A introduces additional verification objectives and stronger independence expectations.

For a cargo UAV, DAL allocation should follow the actual safety consequences of each function rather than the complexity or size of its software. Flight stabilization, propulsion command, flight-envelope protection, or critical navigation functions may require high assurance when their failure can cause loss of the aircraft or unacceptable ground risk. Less critical mission-management or payload functions may receive lower assurance when adequately isolated.

A safety case should establish traceability from identified hazards to system safety requirements and then to software requirements. Each software requirement should have a clear origin and verification method, while every safety-relevant higher-level requirement should be implemented completely. Bidirectional traceability makes it possible to detect missing implementation, unintended functionality, incomplete testing, and requirements that no longer have a valid system justification.

Planning establishes how compliance will be achieved before development evidence is produced. Typical life-cycle planning defines software development, verification, configuration management, quality assurance, certification liaison, standards, tools, environments, and transition criteria. The plans should describe how DAL-specific objectives are satisfied and how independence is maintained where required rather than treating certification as documentation added after implementation.

High-level software requirements describe externally meaningful behavior, interfaces, modes, timing constraints, failure responses, and safety protections. They should be accurate, consistent, verifiable, and compatible with system requirements. For a flight-control function, requirements may define control modes, sensor validity conditions, actuator command limits, degraded-state transitions, and timing deadlines without prematurely embedding unnecessary implementation details.

Low-level requirements and software architecture refine high-level behavior into implementable logic. They can define algorithms, state machines, data transformations, scheduling behavior, interface handling, monitoring logic, and internal control paths. For DAL A and DAL B software, verification must demonstrate that these detailed requirements correctly satisfy their higher-level sources and do not introduce unintended safety behavior.

Software architecture deserves particular attention in redundant UAV systems. Partitioning between flight-critical control, autonomy, communication, payload, and maintenance functions should prevent lower-assurance software from corrupting higher-assurance functions. Memory protection, processor partitioning, communication gateways, scheduling separation, and controlled interfaces can support independence, but their effectiveness must be demonstrated rather than assumed.

Source code should conform to approved coding standards and accurately implement the low-level requirements and architecture. Standards can restrict ambiguous language features, uncontrolled dynamic behavior, unsafe memory usage, recursion, hidden side effects, and other constructs that complicate verification. Compliance should be supported by review and analysis, with deviations explicitly justified and controlled rather than informally accepted.

Verification is not limited to executing tests. Reviews, analyses, requirements-based testing, interface testing, robustness testing, structural coverage analysis, and traceability assessment collectively provide assurance. The objective is to demonstrate that software satisfies specified behavior and to expose unintended implementation. Verification evidence should therefore address both what the software is required to do and what the executable structure actually contains.

Requirements-based testing derives test cases from software requirements rather than from knowledge of the implementation alone. Tests should exercise normal operating conditions, boundary values, invalid inputs, mode transitions, timing conditions, and failure responses. Safety-critical UAV software also benefits from fault-injection scenarios involving sensor failures, communication loss, processor faults, actuator limitations, and inconsistent system states.

Robustness testing evaluates behavior outside nominal input conditions. Software should respond predictably to out-of-range sensor values, malformed messages, stale data, unavailable resources, unexpected mode requests, and timing anomalies. A robust implementation does not necessarily continue the mission under every abnormal condition; instead, it transitions to a defined degraded or safe state without creating uncontrolled behavior.

Structural coverage analysis determines whether requirements-based tests have exercised the implemented software structure to the extent required by the assurance level. Statement and decision coverage provide evidence for lower structural levels, while DAL A requires Modified Condition/Decision Coverage, commonly known as MC/DC. MC/DC demonstrates that individual Boolean conditions can independently affect a decision outcome under appropriate test combinations.

Structural coverage is not a substitute for requirements-based testing. Its purpose is partly to reveal code that existing requirement-derived tests failed to exercise. Uncovered code may indicate inadequate tests, incomplete requirements, defensive logic, deactivated code, or unintended functionality. Each coverage gap should therefore be analyzed and resolved through additional tests, requirement clarification, justified deactivation, or removal of unnecessary code.

DAL A software requires particularly strong evidence because catastrophic failure conditions demand the highest assurance. Verification independence is applied to designated objectives so that the person or process verifying critical artifacts is appropriately independent from their development. Independence reduces the chance that the same misunderstanding, assumption, or implementation error will pass through development and verification without challenge.

DAL B also requires rigorous verification and controlled life-cycle processes, although its objective set and independence requirements differ from DAL A. The engineering approach should not treat DAL B as ordinary commercial software with additional testing. Requirements quality, traceability, configuration control, verification evidence, problem reporting, and reproducibility remain fundamental because hazardous failure conditions still demand substantial assurance.

Data coupling and control coupling analyses help verify interactions between software components. Data coupling examines information exchanged through interfaces, while control coupling examines how one component influences another component's execution or behavior. In distributed UAV architectures, these analyses can reveal unexpected dependencies through shared data, mode commands, network services, or scheduling relationships that could undermine intended safety separation.

Timing behavior is especially important for flight software because correct results delivered too late can still be hazardous. Verification should consider execution deadlines, scheduling margins, communication latency, sensor-to-actuator delay, worst-case execution behavior, and overload conditions. High computational loads from perception or AI functions should not prevent flight-critical DAL A or DAL B software from meeting its required timing guarantees.

Configuration management ensures that requirements, source code, executable objects, test procedures, test results, tool versions, compiler settings, and supporting data remain under controlled identification. Certification evidence must correspond to the exact software configuration installed on the aircraft. Reproducible builds and controlled baselines help prevent differences between the verified software and the operational binary.

Problem reporting and change control continue throughout the software life cycle. An anomaly should be recorded, evaluated for safety impact, corrected when required, and subjected to appropriate regression analysis. A seemingly small code change can affect requirements, timing, structural coverage, interfaces, or previously verified behavior. Change impact analysis determines which certification evidence must be regenerated or reverified.

Tool qualification becomes relevant when a development or verification tool can introduce an error or fail to detect an error and its output is used without subsequent verification that would expose the problem. Compilers, code generators, model-based development environments, static analyzers, coverage tools, and automated verification systems may therefore require qualification consideration according to their role and certification credit.

Model-based development can be integrated into a DO-178C assurance process when models, generated code, verification artifacts, and tool usage are governed by appropriate objectives. Related guidance such as DO-331 addresses model-based development and verification considerations. The model does not eliminate assurance obligations; instead, its role, traceability, verification, and generated artifacts must be clearly controlled.

Formal methods may supplement or replace selected conventional verification activities when properly applied under relevant guidance such as DO-333. Mathematical proof can provide strong evidence for properties including range limits, state invariants, data consistency, and control logic. However, formal verification still depends on correct assumptions and requirements, so it must remain connected to the overall safety argument and configuration baseline.

Object-oriented technologies and related techniques may require consideration of guidance such as DO-332 when used in airborne software. Features including inheritance, dynamic dispatch, polymorphism, and complex object relationships can affect verification and structural analysis. The assurance strategy should demonstrate that these mechanisms remain understandable, bounded, and verifiable at the assigned DAL.

A software safety case should also address partitioning and coexistence with complex autonomy or artificial-intelligence functions. A high-performance planner or learned model may not be developed to the same assurance level as a conventional flight-control kernel. Safety can be strengthened by constraining such components through verified monitors, command envelopes, runtime assurance, independent fallback controllers, and architectural isolation.

Certification evidence should form a coherent argument rather than a collection of disconnected documents. Safety assessments explain why assurance is required, plans explain how it will be achieved, requirements and design define intended behavior, implementation realizes that behavior, verification demonstrates compliance, and configuration records establish exactly what was evaluated. Traceability connects these elements into an auditable chain.

Final software accomplishment evidence summarizes the completed life cycle, resolved anomalies, achieved objectives, configuration identification, verification results, and remaining limitations. Before release, the organization should confirm that required reviews, analyses, tests, structural coverage activities, configuration audits, quality assurance records, and certification artifacts are complete for the applicable DAL and software baseline.

For a safety-critical UAV, DO-178C DAL A/B assurance is therefore not simply a testing requirement applied near the end of development. It is a disciplined engineering framework connecting hazard analysis, requirements, architecture, implementation, verification, independence, configuration management, and certification evidence. The objective is to build a defensible body of evidence that critical software behavior is understood, controlled, verified, and appropriate for the severity of the failures it could cause.

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

##  

## 08.10. Safety Validation Flight Test Protocol

![](images/image10.png){width="7.268055555555556in" height="7.268055555555556in"}

Safety validation flight testing provides the final operational evidence that a UAV's safety architecture behaves correctly when exposed to realistic aircraft dynamics, environmental disturbances, communication conditions, navigation uncertainty, and subsystem degradation. It complements simulation, software-in-the-loop, hardware-in-the-loop, and ground testing by evaluating integrated behavior in the physical flight environment.

A flight-test protocol should originate from system safety requirements rather than from demonstrations of nominal mission capability. Functional Hazard Assessment, FMEA, FTA, software assurance activities, subsystem qualification, and integration testing identify the conditions that require flight evidence. Each test objective should trace to a safety requirement, identified hazard, mitigation mechanism, operational limitation, or certification objective.

Testing should follow a progressive risk-reduction strategy. Initial flights establish basic controllability, propulsion performance, navigation stability, communication reliability, and nominal automation. Later phases introduce controlled disturbances, degraded sensors, communication anomalies, constrained energy margins, and selected fault conditions. Hazardous scenarios should never be introduced before prerequisite containment and recovery functions have been demonstrated.

A formal test card defines the configuration and execution of each flight-test case. It should identify the aircraft hardware and software baseline, payload condition, battery configuration, environmental limits, test area, required personnel, initial conditions, maneuver sequence, expected behavior, termination criteria, and required measurements. Configuration control ensures that results can be associated with the exact system that was tested.

Preflight readiness review verifies that the aircraft and test organization are prepared for the planned risk level. Engineers should confirm maintenance status, software version, parameter files, sensor calibration, battery condition, structural integrity, propulsion health, communication links, geofence data, emergency equipment, weather limits, and data-recording capability. Open anomalies should be reviewed before authorization to fly.

The test area should provide sufficient containment for both nominal trajectories and credible off-nominal motion. Boundaries should account for aircraft speed, altitude, stopping distance, navigation uncertainty, wind, possible control degradation, and emergency landing requirements. Higher-risk tests may require additional separation from people, property, infrastructure, and unrelated air traffic, together with independent safety supervision.

Test roles and command authority must be unambiguous. The flight-test team can include a remote pilot, test conductor, safety pilot, telemetry engineer, subsystem specialists, and observers according to program scale. One designated authority should control progression through the test card, while another authorized safety function should retain the ability to terminate testing when predefined limits are exceeded.

Instrumentation should capture enough information to reconstruct aircraft behavior without relying solely on operator observations. Relevant data include position, velocity, attitude, angular rates, control commands, actuator feedback, propulsion status, battery voltage and current, navigation health, C2 link metrics, mode transitions, fault flags, watchdog events, geofence status, and safety-monitor outputs. Common timing enables accurate event correlation.

Time synchronization is especially important when measurements originate from several onboard computers, sensors, ground stations, and independent test instruments. Unsynchronized logs can make a correct response appear late or obscure the sequence that initiated a failure. Timestamp provenance should therefore be maintained across the acquisition chain so that sensor input, detection, decision, command, and aircraft response can be compared accurately.

Nominal-envelope testing establishes a reference before abnormal scenarios are introduced. The UAV should demonstrate stable takeoff, climb, cruise or hover, maneuvering, descent, approach, and landing across representative payload and environmental conditions. Reference data characterize control margins, tracking error, energy consumption, communication performance, navigation accuracy, and expected variability for later comparison with degraded operation.

Envelope expansion should proceed incrementally in speed, altitude, payload, wind, maneuver severity, and automation authority. Test points near operational limits should be approached only after lower-risk conditions confirm model predictions and adequate margins. Expansion criteria should be quantitative so that progression is based on measured performance rather than subjective confidence in the aircraft.

Fault-injection flight testing should focus on conditions that can be introduced safely and reversibly. Examples include simulated sensor invalidity, selected communication interruption, commanded redundancy switchover, software-generated fault flags, or controlled removal of nonessential information sources. Physical failures that could create uncontrollable behavior should normally be evaluated through simulation, laboratory, or HIL methods instead of deliberately created in flight.

Redundant flight-control architecture should be validated by demonstrating detection, isolation, and reconfiguration under controlled conditions. A fault in one computing channel can be simulated while healthy channels continue operation. Measurements should confirm fault-detection latency, voting behavior, channel isolation, control transients, state synchronization, and continued stability without violating aircraft limits or producing unacceptable trajectory deviations.

Propulsion fault-tolerant behavior requires carefully bounded testing. Rather than introducing destructive motor or engine failures, test systems can emulate loss of command authority or restrict selected propulsion outputs within preapproved limits. The objective is to verify detection, control reallocation, attitude stabilization, trajectory management, and recovery decisions while preserving sufficient control authority for immediate test termination.

GNSS degradation testing should evaluate the transition from satellite-based navigation to available fallback sources without creating uncontrolled radio-frequency interference. Approved simulation, shielding, receiver test techniques, or onboard fault injection can represent signal loss or corrupted navigation inputs. The aircraft response should demonstrate detection, measurement rejection, uncertainty management, fallback navigation, and appropriate recovery behavior.

C2 lost-link validation should examine both gradual degradation and complete loss where permitted by the test configuration. The UAV should demonstrate the configured sequence of degraded-link handling, hold or loiter behavior, alternate-link activation, return, diversion, or landing. Tests should verify that reconnection does not create conflicting commands and that onboard autonomy remains within approved containment boundaries.

Geofence validation should test predictive enforcement rather than merely demonstrating a warning after crossing a boundary. Approaches can be flown at different speeds, angles, altitudes, winds, and navigation uncertainty levels. Measurements should establish when the system predicts a potential violation, modifies guidance commands, limits motion, and restores separation while maintaining stable and physically achievable aircraft behavior.

Battery and energy contingency testing should verify that recovery actions begin before usable propulsion capability becomes critical. Tests can use controlled initial state of charge, representative payload, and predefined reserve thresholds rather than intentionally damaging batteries. The system should correctly distinguish mission continuation, return, diversion, and emergency landing based on predicted energy, power margin, and landing requirements.

Watchdog and safety-monitor validation should demonstrate that critical supervision remains functional during abnormal computational conditions. Controlled software mechanisms can simulate missed heartbeats, task stalls, stale data, invalid commands, or processor-channel loss. The test should confirm detection timing, fault classification, control-authority transfer, recovery behavior, and continued operation of independent safety protections.

Combined-failure testing is necessary because real safety risk often emerges from interactions between degraded systems. A communication loss combined with low battery reserve, or navigation degradation near a geofence boundary, can require different behavior than either condition alone. Combinations should be selected from safety analysis and introduced progressively, with high-risk combinations retained in simulation when safe flight demonstration is impractical.

Abort criteria should be defined before every test. Limits can include excessive attitude deviation, unexpected altitude loss, geofence margin reduction, abnormal vibration, propulsion degradation, battery thresholds, navigation uncertainty, communication conditions, or failure to follow the expected state sequence. The test crew should not need to debate whether to continue after a predefined safety limit has been crossed.

Test termination should transition the UAV into a known recovery state. Depending on aircraft configuration and remaining capability, this can involve manual takeover, autonomous hold, return, diversion, controlled descent, or landing. The termination path itself should be validated because a safe test requires not only recognition of unacceptable conditions but also a reliable means of exiting the experimental condition.

Independent safety monitoring can provide protection beyond the system under test. A separate tracking source, independent communication path, external observer, chase platform where appropriate, or ground-based monitoring system can verify containment and support termination decisions. Independence is particularly valuable when the test intentionally challenges onboard navigation, communications, or flight-control functions.

Environmental conditions should be measured rather than described only qualitatively. Wind speed and direction, gusts, temperature, pressure, visibility, precipitation, and other relevant variables influence control performance, propulsion demand, navigation sensors, and communication links. Environmental data should accompany flight logs so that unexpected behavior can be distinguished from changes in external conditions.

Pass and fail criteria should be quantitative wherever possible. Examples include maximum detection latency, allowable trajectory deviation, minimum geofence separation, maximum control transient, required reserve energy at landing, permitted communication outage duration, or acceptable navigation uncertainty. Quantitative criteria improve repeatability and prevent post-test interpretation from being adjusted to match observed results.

Post-flight data review should begin before the next test point when safety-critical behavior has been exercised. Quick-look analysis can confirm that commanded faults occurred as intended, protective logic activated correctly, limits were respected, and instrumentation remained valid. Unexpected behavior should suspend further envelope expansion until its cause and safety significance are understood.

Detailed analysis should compare measured behavior with models, requirements, and predicted margins. Engineers can reconstruct event timelines from synchronized logs, calculate detection and response delays, evaluate control transients, examine estimator performance, and determine whether safety reserves were maintained. Differences between predicted and observed behavior should feed back into models, requirements, software, or operating limitations.

Regression testing is required when safety-related software, hardware, parameters, or configuration data change. A modification to navigation filtering, control gains, battery thresholds, communication logic, or geofence handling can influence previously validated behavior. Impact analysis should determine which ground, simulation, HIL, and flight tests must be repeated to preserve confidence in the integrated safety case.

Flight-test evidence should remain under configuration and data management. Test cards, approvals, aircraft configuration records, environmental data, telemetry, onboard logs, video where used, anomaly reports, analysis results, and final conclusions should be traceable to specific requirements and software or hardware baselines. This evidence supports engineering decisions, certification activities, operational approval, and future maintenance.

Safety validation is complete only when evidence from multiple verification levels forms a consistent argument. Simulation provides broad scenario coverage, SIL evaluates software behavior, HIL introduces realistic hardware and timing, ground testing validates integrated subsystems, and flight testing confirms selected safety behavior under physical dynamics. No single level should be expected to demonstrate every hazardous condition.

A mature UAV safety flight-test protocol therefore combines requirements traceability, progressive envelope expansion, controlled fault injection, independent monitoring, quantitative limits, synchronized instrumentation, predefined abort logic, and rigorous post-flight analysis. Its objective is not to prove that failures never occur, but to demonstrate with controlled evidence that credible failures are detected, contained, and managed before they develop into unacceptable aircraft or ground risk.

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
