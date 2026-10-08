**Volume 23. Cargo UAV Autonomy and Flight AI**


# Chapter 02. UAV System Architecture

##  

## 02.01. Cargo UAV Avionics Architecture FCC FMS GCS

![](images/image1.png){width="7.268055555555556in" height="7.268055555555556in"}

A cargo UAV avionics architecture provides the computational, communication, sensing, and supervisory foundation required to operate a heavy unmanned aircraft safely throughout its mission. Within the broader UAV system architecture, the Flight Control Computer (FCC), Flight Management System (FMS), and Ground Control Station (GCS) form three major functional domains that connect real-time aircraft stabilization, mission-level automation, and ground-based supervision.

The FCC occupies the most time-critical layer of the airborne architecture. It receives aircraft-state information from inertial sensors, navigation systems, air-data sources, propulsion controllers, and actuator feedback interfaces, then executes deterministic control functions. Attitude, angular-rate, altitude, velocity, and position commands are ultimately translated into actuator or propulsion commands through tightly bounded real-time control cycles.

For a cargo UAV, the FCC must accommodate operating conditions that vary significantly with payload mass, center of gravity, fuel or battery state, aerodynamic configuration, and environmental disturbance. These variations make the control architecture more demanding than that of many small UAVs. The FCC therefore acts not merely as an autopilot processor but as a safety-critical execution platform responsible for maintaining the commanded flight state within validated aircraft limits.

The FMS operates above the inner flight-control functions and manages the aircraft from a mission and trajectory perspective. It interprets mission objectives, waypoints, flight plans, navigation constraints, vehicle performance information, and contingency rules. Instead of directly stabilizing the aircraft, the FMS determines where the UAV should travel, how the mission should progress, and which high-level commands should be delivered to lower-level flight-control functions.

This separation between FCC and FMS establishes an important architectural boundary. The FMS may request a climb, descent, course change, waypoint transition, holding pattern, return-to-home maneuver, or landing sequence, while the FCC determines how the aircraft can safely execute that request. Such separation prevents mission-management complexity from becoming inseparably coupled to high-frequency stabilization and supports independent verification of functions with different timing and safety requirements.

Navigation and state-estimation services provide essential information to both domains. GNSS, IMU, barometric sensing, radar or laser altitude measurements, and other navigation sources can be fused into estimates of position, velocity, attitude, and environmental state. The architecture must preserve data validity, timestamps, source identity, and health status because an apparently valid but incorrect navigation solution can be more hazardous than an explicitly failed sensor.

The GCS forms the principal human-machine and operational interface outside the aircraft. It allows operators to prepare missions, monitor vehicle state, observe flight progress, receive warnings, supervise cargo operations, and issue authorized commands. Telemetry from the aircraft may include position, attitude, propulsion status, energy state, navigation integrity, communication quality, payload condition, fault information, and mission progress, giving operators a consolidated view of system health.

The command-and-control link connects the GCS with airborne avionics but should not make safe flight dependent on continuous human communication. A cargo UAV intended for autonomous operation requires predefined behavior for degraded or lost links. Depending on mission phase and aircraft condition, the onboard system may continue an approved mission, enter a holding state, return to a designated location, divert to an alternate site, or initiate another validated contingency procedure.

Communication among avionics computers must distinguish deterministic control traffic from less time-critical mission and payload data. Sensor measurements, actuator commands, synchronization messages, and safety signals may require bounded latency and predictable delivery, whereas map updates, maintenance records, or bulk mission information can tolerate different communication characteristics. This motivates a partitioned communication architecture with explicit priorities, interface contracts, and failure-containment boundaries.

Redundancy becomes increasingly important as cargo UAV size, kinetic energy, payload value, and operational exposure increase. Critical FCC functions may therefore be implemented through dual or triple computing channels, independent power paths, redundant sensor sources, and monitored communication links. Cross-channel comparison, voting, watchdog supervision, and fault isolation allow the avionics system to identify inconsistent behavior and preserve control when a component fails.

The FMS also participates in contingency management by maintaining awareness of route constraints, remaining energy, destination suitability, weather information, airspace restrictions, and alternate landing opportunities. When the nominal mission can no longer be completed safely, it can generate or select an alternative mission state. The FCC then evaluates and executes the corresponding flight commands while retaining authority over immediate aircraft stability and envelope protection.

A dedicated safety-monitoring layer can supervise both FCC and FMS behavior without duplicating every function. Independent monitors may check command plausibility, flight-envelope violations, sensor disagreement, processor heartbeat, actuator response, energy reserves, and communication health. If a hazardous condition is detected, the safety architecture can inhibit invalid commands, switch redundant resources, downgrade autonomy, or transition the aircraft into a predefined safe operating mode.

Cargo-specific systems introduce another architectural domain. A Cargo Management System may monitor payload identity, locking mechanisms, cargo-bay status, load distribution, center-of-gravity estimates, temperature-sensitive freight, or release mechanisms. Cargo information that affects aircraft dynamics must be communicated to flight and mission systems through controlled interfaces so that loading changes cannot silently invalidate assumptions used by navigation or control functions.

Power management is similarly integrated with avionics decision making. Battery-management, generator, hybrid-power, and distribution systems provide information about available energy, component temperature, voltage, current, and predicted endurance. The FMS can use this information for mission feasibility and diversion decisions, while the FCC and safety functions can react immediately to power faults that threaten propulsion, sensing, computation, or actuation.

Time synchronization is a fundamental system-level requirement because distributed sensors and computers observe a rapidly changing physical system. Sensor measurements must be associated with reliable timestamps so that state estimation and control algorithms can reconstruct the correct temporal relationship among IMU, GNSS, radar, camera, LiDAR, actuator, and propulsion information. Poor synchronization can introduce estimation errors even when every individual sensor is functioning correctly.

The architecture should also separate flight-critical functions from computationally intensive autonomy workloads. Perception, AI-based planning, obstacle assessment, or optimization modules may run on high-performance mission computers, but their outputs should pass through validated interfaces before influencing flight-critical control. This arrangement permits advanced autonomy to evolve while maintaining a stable safety boundary around deterministic control, vehicle protection, and emergency behavior.

Operational data flows in the opposite direction as well. The aircraft continuously produces health information, event records, flight parameters, fault histories, and mission logs that can be transmitted to the GCS or retained onboard. These records support maintenance, post-flight analysis, software verification, anomaly investigation, and fleet-level improvement. Logging should therefore preserve sufficient timing and configuration context to reconstruct important events after a mission.

Cybersecurity must be considered across GCS, communication links, maintenance interfaces, and airborne networks. Authentication, authorization, integrity protection, secure software loading, configuration control, and protected command paths reduce the possibility that unauthorized or corrupted information can influence aircraft behavior. Security mechanisms must nevertheless respect real-time and availability requirements so that protection measures do not create new flight-critical failure modes.

System integration ultimately depends on clearly defined responsibility boundaries. The FCC owns deterministic vehicle stabilization and immediate control execution; the FMS manages trajectory, navigation objectives, mission sequencing, and higher-level contingencies; and the GCS provides operator supervision, mission preparation, telemetry visualization, and authorized command interaction. Supporting systems supply sensing, communication, power, payload, safety, and health-management services across these domains.

This FCC-FMS-GCS decomposition establishes the architectural foundation for the remainder of the cargo UAV software stack. The chapter structure subsequently expands the FCC hardware/software design, FMS architecture, redundant computing, avionics buses, power management, sensor integration, cargo management, GCS software, and system integration testing as distinct engineering subjects. Together, these layers transform the UAV from a collection of flight components into an integrated autonomous cargo aircraft.

화물 무인항공기(Cargo UAV)의 항공전자 아키텍처(Avionics Architecture)는 대형 무인항공기를 전체 임무 과정에서 안전하게 운용하기 위해 필요한 연산, 통신, 센싱 및 감독 기능의 기반을 제공한다. 전체 무인항공기 시스템 아키텍처(UAV System Architecture)에서 비행제어컴퓨터(Flight Control Computer, FCC), 비행관리시스템(Flight Management System, FMS), 지상통제소(Ground Control Station, GCS)는 실시간 항공기 안정화, 임무 수준 자동화, 지상 기반 감독을 연결하는 세 가지 핵심 기능 영역을 구성한다.

비행제어컴퓨터(FCC)는 탑재 아키텍처에서 가장 높은 실시간성(Time-Critical)을 요구하는 계층에 위치한다. 관성 센서(Inertial Sensor), 항법 시스템(Navigation System), 대기자료 센서(Air Data Source), 추진 제어기(Propulsion Controller), 구동기 피드백 인터페이스(Actuator Feedback Interface)로부터 항공기 상태 정보를 받아 결정론적 제어(Deterministic Control)를 수행한다. 자세, 각속도, 고도, 속도 및 위치 명령은 엄격하게 제한된 실시간 제어 주기를 통해 최종적으로 구동기 또는 추진 시스템 명령으로 변환된다.

화물 무인항공기(Cargo UAV)의 비행제어컴퓨터(FCC)는 탑재화물 질량(Payload Mass), 무게중심(Center of Gravity), 연료 또는 배터리 상태, 공력 형상(Aerodynamic Configuration), 환경 외란(Environmental Disturbance)에 따라 크게 달라지는 운용 조건을 수용해야 한다. 이러한 변화는 많은 소형 무인항공기보다 제어 아키텍처를 복잡하게 만든다. 따라서 FCC는 단순한 자동조종장치(Autopilot Processor)가 아니라 검증된 항공기 한계 내에서 명령된 비행 상태를 유지하는 안전 필수 실행 플랫폼(Safety-Critical Execution Platform)으로 기능한다.

비행관리시스템(FMS)은 내부 비행제어 기능보다 상위에서 동작하며 임무 및 궤적 관점에서 항공기를 관리한다. 임무 목표, 웨이포인트(Waypoint), 비행계획(Flight Plan), 항법 제약조건, 기체 성능 정보 및 비상대응 규칙(Contingency Rule)을 해석한다. FMS는 항공기를 직접 안정화하는 대신 UAV가 어디로 이동해야 하는지, 임무를 어떤 순서로 진행해야 하는지, 하위 비행제어 기능에 어떤 상위 수준 명령을 전달해야 하는지를 결정한다.

FCC와 FMS의 이러한 분리는 중요한 아키텍처 경계(Architectural Boundary)를 형성한다. FMS는 상승, 하강, 항로 변경, 웨이포인트 전환, 체공 패턴(Holding Pattern), 자동복귀(Return-to-Home), 착륙 절차 등을 요청할 수 있으며, FCC는 해당 요청을 항공기가 어떻게 안전하게 수행할 것인지를 결정한다. 이러한 분리를 통해 임무관리의 복잡성이 고주파 안정화 제어와 강하게 결합되는 것을 방지하고 서로 다른 시간 및 안전 요구사항을 갖는 기능들을 독립적으로 검증할 수 있다.

항법 및 상태추정 서비스(Navigation and State Estimation Service)는 FCC와 FMS 모두에 필수 정보를 제공한다. 위성항법시스템(GNSS), 관성측정장치(IMU), 기압 센서(Barometric Sensor), 레이더 또는 레이저 고도 측정값과 기타 항법 정보원을 융합하여 위치, 속도, 자세 및 환경 상태를 추정할 수 있다. 정상적으로 보이지만 잘못된 항법 정보가 명시적으로 고장 난 센서보다 더 위험할 수 있으므로 데이터 유효성, 타임스탬프(Timestamp), 정보원 식별 및 상태 정보를 보존해야 한다.

지상통제소(GCS)는 항공기 외부에서 주요 인간-기계 및 운용 인터페이스(Human-Machine and Operational Interface)를 형성한다. 운용자는 GCS를 통해 임무를 준비하고, 기체 상태와 비행 진행 상황을 모니터링하며, 경고를 확인하고, 화물 작업을 감독하며, 승인된 명령을 전달한다. 항공기 원격측정(Telemetry)에는 위치, 자세, 추진 상태, 에너지 상태, 항법 무결성, 통신 품질, 탑재화물 상태, 고장 정보 및 임무 진행 상황 등이 포함되어 운용자에게 통합된 시스템 상태 정보를 제공할 수 있다.

명령통제 링크(Command and Control Link, C2 Link)는 GCS와 탑재 항공전자 시스템을 연결하지만, 안전한 비행이 지속적인 인간과의 통신에 의존하도록 설계해서는 안 된다. 자율 운용을 목적으로 하는 화물 UAV에는 통신 링크 성능 저하 또는 두절 상황에 대한 사전 정의 동작이 필요하다. 임무 단계와 항공기 상태에 따라 승인된 임무를 계속하거나, 체공 상태에 진입하거나, 지정 위치로 복귀하거나, 대체 착륙지로 전환하거나, 검증된 다른 비상대응 절차를 실행할 수 있어야 한다.

항공전자 컴퓨터 간 통신은 결정론적 제어 트래픽(Deterministic Control Traffic)과 시간 민감도가 상대적으로 낮은 임무 및 탑재화물 데이터를 구분해야 한다. 센서 측정값, 구동기 명령, 시간 동기화 메시지 및 안전 신호에는 제한된 지연시간과 예측 가능한 전달 특성이 요구될 수 있지만, 지도 업데이트, 정비 기록 또는 대용량 임무 정보는 다른 통신 특성을 허용할 수 있다. 따라서 명확한 우선순위, 인터페이스 계약(Interface Contract), 고장 격리 경계(Failure-Containment Boundary)를 갖는 분할형 통신 아키텍처가 필요하다.

화물 UAV의 크기, 운동에너지, 화물 가치 및 운용 노출도가 증가할수록 이중화(Redundancy)의 중요성도 커진다. 핵심 FCC 기능은 이중 또는 삼중 연산 채널(Dual or Triple Computing Channel), 독립 전원 경로, 중복 센서 정보원 및 감시되는 통신 링크를 통해 구현할 수 있다. 채널 간 비교(Cross-Channel Comparison), 투표(Voting), 감시 타이머(Watchdog), 고장 격리(Fault Isolation)를 적용하면 항공전자 시스템이 비정상 동작을 식별하고 일부 구성요소가 고장 난 상황에서도 제어 기능을 유지할 수 있다.

FMS 역시 항로 제약조건, 잔여 에너지, 목적지 적합성, 기상 정보, 공역 제한 및 대체 착륙 가능 지점을 파악함으로써 비상대응 관리(Contingency Management)에 참여한다. 정상 임무를 더 이상 안전하게 완료할 수 없는 경우 FMS는 대체 임무 상태를 생성하거나 선택할 수 있다. 이후 FCC는 해당 비행 명령을 평가하고 실행하면서 즉각적인 항공기 안정성과 비행영역 보호(Flight Envelope Protection)에 대한 권한을 유지한다.

전용 안전감시 계층(Safety Monitoring Layer)은 모든 기능을 중복 구현하지 않으면서 FCC와 FMS의 동작을 감독할 수 있다. 독립적인 감시 기능은 명령 타당성(Command Plausibility), 비행영역 위반, 센서 불일치, 프로세서 하트비트(Processor Heartbeat), 구동기 응답, 에너지 잔량 및 통신 상태를 검사할 수 있다. 위험 조건이 감지되면 안전 아키텍처는 잘못된 명령을 차단하고, 중복 자원을 전환하며, 자율화 수준을 낮추거나 항공기를 사전에 정의된 안전 운용 모드(Safe Operating Mode)로 전환할 수 있다.

화물 특화 시스템은 또 하나의 아키텍처 영역을 구성한다. 화물관리시스템(Cargo Management System, CMS)은 화물 식별정보, 잠금 장치, 화물칸 상태, 하중 분포, 무게중심 추정, 온도 민감 화물 또는 화물 투하 장치 등을 감시할 수 있다. 항공기 동역학에 영향을 주는 화물 정보는 통제된 인터페이스를 통해 비행 및 임무 시스템으로 전달되어야 하며, 이를 통해 적재 상태 변화가 항법 또는 제어 기능이 사용하는 가정을 예고 없이 무효화하는 것을 방지할 수 있다.

전력관리(Power Management) 역시 항공전자 의사결정과 통합된다. 배터리관리시스템(Battery Management System), 발전기, 하이브리드 전력 시스템 및 전력분배시스템(Power Distribution System)은 사용 가능한 에너지, 구성요소 온도, 전압, 전류 및 예상 운용시간 정보를 제공한다. FMS는 이러한 정보를 임무 수행 가능성 및 회항 판단에 활용하며, FCC와 안전 기능은 추진, 센싱, 연산 또는 구동 기능을 위협하는 전력 고장에 즉각 대응할 수 있다.

시간 동기화(Time Synchronization)는 분산된 센서와 컴퓨터가 빠르게 변화하는 물리 시스템을 관측하기 때문에 기본적인 시스템 수준 요구사항이다. 상태추정 및 제어 알고리즘이 IMU, GNSS, 레이더, 카메라, 라이다(LiDAR), 구동기 및 추진 정보 사이의 올바른 시간 관계를 복원할 수 있도록 센서 측정값에는 신뢰할 수 있는 타임스탬프가 연결되어야 한다. 개별 센서가 모두 정상적으로 동작하더라도 시간 동기화가 불량하면 상태추정 오차가 발생할 수 있다.

아키텍처는 비행 필수 기능(Flight-Critical Function)과 연산 집약적인 자율화 워크로드(Autonomy Workload)를 분리해야 한다. 인지(Perception), 인공지능 기반 계획(AI-Based Planning), 장애물 평가 또는 최적화 모듈은 고성능 임무 컴퓨터에서 실행될 수 있지만, 이들의 출력은 비행 필수 제어에 영향을 주기 전에 검증된 인터페이스를 통과해야 한다. 이를 통해 결정론적 제어, 기체 보호 및 비상 동작을 둘러싼 안정적인 안전 경계를 유지하면서 고급 자율화 기능을 발전시킬 수 있다.

운용 데이터는 반대 방향으로도 지속적으로 흐른다. 항공기는 상태 정보, 이벤트 기록, 비행 파라미터, 고장 이력 및 임무 로그를 지속적으로 생성하며, 이러한 데이터는 GCS로 전송되거나 기체 내부에 저장될 수 있다. 기록 데이터는 정비, 비행 후 분석(Post-Flight Analysis), 소프트웨어 검증, 이상현상 조사 및 함대 수준 개선(Fleet-Level Improvement)을 지원한다. 따라서 중요한 임무 이벤트를 사후에 재구성할 수 있도록 충분한 시간 및 구성정보를 로그에 보존해야 한다.

사이버보안(Cybersecurity)은 GCS, 통신 링크, 정비 인터페이스 및 탑재 네트워크 전반에서 고려되어야 한다. 인증(Authentication), 권한부여(Authorization), 무결성 보호(Integrity Protection), 안전한 소프트웨어 로딩(Secure Software Loading), 구성관리(Configuration Control), 보호된 명령 경로를 적용하면 승인되지 않았거나 손상된 정보가 항공기 동작에 영향을 미칠 가능성을 줄일 수 있다. 동시에 보안 메커니즘은 실시간성과 가용성 요구사항을 충족하여 새로운 비행 필수 고장 형태를 유발하지 않아야 한다.

시스템 통합(System Integration)은 궁극적으로 명확하게 정의된 책임 경계에 의존한다. FCC는 결정론적 기체 안정화와 즉각적인 제어 실행을 담당하고, FMS는 궤적, 항법 목표, 임무 순서 및 상위 수준 비상대응을 관리하며, GCS는 운용자 감독, 임무 준비, 원격측정 시각화 및 승인된 명령 상호작용을 제공한다. 이를 지원하는 시스템들은 이러한 영역 전반에 걸쳐 센싱, 통신, 전력, 화물, 안전 및 상태관리(Health Management) 서비스를 제공한다.

이러한 FCC-FMS-GCS 분할 구조는 화물 UAV 소프트웨어 스택(Cargo UAV Software Stack)의 아키텍처 기반을 형성한다. 이후 시스템 아키텍처에서는 FCC 하드웨어·소프트웨어 설계, FMS 아키텍처, 이중화 연산, 항공전자 통신 버스, 전력관리, 센서 통합, 화물관리, GCS 소프트웨어 및 시스템 통합시험(System Integration Test)을 각각 독립적인 엔지니어링 영역으로 확장한다. 이들 계층은 함께 동작함으로써 개별 비행 구성요소의 집합을 통합된 자율 화물 항공기(Autonomous Cargo Aircraft)로 전환한다.

##  

## 02.02. Flight Control Computer FCC HW SW Design [w/Code]

![](images/image2.png){width="7.268055555555556in" height="7.268055555555556in"}

The Flight Control Computer (FCC) is the primary real-time computing platform responsible for executing the safety-critical control functions of a cargo UAV. Its hardware and software must operate as an integrated deterministic system capable of acquiring sensor data, estimating vehicle state, executing control laws, monitoring system health, and producing actuator commands within tightly bounded timing constraints. The FCC therefore forms the computational core of the flight-control architecture.

FCC hardware is designed around predictable execution, fault containment, environmental robustness, and sufficient computational margin rather than maximum general-purpose performance. A typical platform combines one or more processing devices with volatile and nonvolatile memory, sensor interfaces, communication controllers, discrete I/O, watchdog circuitry, power supervision, and hardware timing resources. Each component must support the reliability requirements associated with the aircraft\'s intended operational and safety objectives.

The processor architecture must provide enough performance for high-frequency control loops while maintaining deterministic behavior under worst-case operating conditions. General-purpose CPUs, microcontrollers, DSPs, or heterogeneous processing devices can be selected according to system requirements. For flight-critical execution, predictable interrupt handling, bounded memory access, hardware protection mechanisms, and reliable timing behavior are often more important than peak computational throughput.

Memory architecture directly affects FCC reliability. Program code, configuration parameters, calibration data, runtime state, and flight logs have different persistence and integrity requirements. Flash or other nonvolatile storage can retain executable software and validated configuration data, while RAM supports real-time calculations. Error detection and correction, memory protection, range checking, and controlled initialization help prevent corrupted data from propagating into flight-control calculations.

The FCC receives data from sensors such as the inertial measurement unit (IMU), GNSS receiver, air-data sensors, radar or laser altimeters, propulsion systems, and actuator feedback channels. Hardware interfaces may include serial links, CAN-based networks, Ethernet-class communication, discrete signals, and dedicated sensor buses. Input processing must associate measurements with timestamps, validity information, source identifiers, and diagnostic status before the data is consumed by control software.

Output interfaces connect the FCC to motors, engine controllers, servos, flight-control surfaces, or other actuation devices. Command generation must respect actuator position, rate, torque, current, and thermal limits. The FCC should also verify whether commanded motion produces the expected physical response. This command-and-feedback relationship enables detection of jammed actuators, disconnected interfaces, degraded propulsion channels, and other failures that cannot be identified from command generation alone.

At the software level, the FCC is commonly organized into layered components that separate hardware access from application-level flight functions. A board support package and device-driver layer manage processor peripherals and physical interfaces. Operating-system or scheduling services provide task execution and timing, while middleware and data services distribute validated information. Flight-control applications then implement estimation, guidance interfaces, control laws, mode logic, monitoring, and actuator command generation.

A real-time operating system (RTOS) can provide deterministic scheduling for FCC software by assigning execution periods, priorities, deadlines, and communication mechanisms to individual tasks. High-rate sensor acquisition and control loops may execute much more frequently than navigation management, diagnostics, logging, or communication tasks. The schedule must ensure that lower-priority activities cannot delay flight-critical computation beyond its allowable deadline.

The primary execution pipeline begins with sensor acquisition and validation. Raw measurements are checked for range, freshness, consistency, and communication integrity before being delivered to state-estimation functions. Estimators combine available measurements to determine attitude, angular velocity, position, velocity, altitude, and other required states. These estimated states then become feedback inputs to the flight-control algorithms responsible for stabilizing and maneuvering the aircraft.

Control-law software transforms desired flight states into physically achievable commands. Inner loops typically regulate rapidly changing variables such as angular rate and attitude, while outer loops can regulate altitude, velocity, and position. The FCC coordinates these loops so that high-level commands received from the Flight Management System (FMS) are converted progressively into control targets and finally into propulsion or actuator commands appropriate for the aircraft configuration.

Cargo UAV operation introduces significant variation in mass and center of gravity. FCC software must therefore account for changes caused by different payload configurations, fuel consumption, cargo movement, or release events. Validated scheduling, parameter adaptation, or model-based compensation can modify controller behavior according to the current vehicle configuration. Any adaptation must remain within defined safety boundaries rather than allowing unrestricted modification of flight-critical control behavior.

Flight-mode management coordinates manual, assisted, autonomous, and contingency states. The FCC interprets mode requests while verifying whether transition conditions are satisfied. A requested autonomous landing mode, for example, should not be entered if required navigation information is unavailable. Explicit transition guards and fallback states prevent invalid combinations of control functions and make system behavior more predictable during abnormal conditions.

Health monitoring operates continuously alongside normal control execution. Software monitors processor status, task timing, sensor freshness, communication links, actuator feedback, power conditions, and internal data consistency. Hardware watchdogs can independently detect stalled execution, while software watchdogs and heartbeat mechanisms supervise individual tasks or computing channels. Detected faults are classified so that the system can isolate, tolerate, or respond to them according to their operational significance.

Redundant FCC architectures can use dual or triple computing channels when a single failure cannot be allowed to cause loss of control. Each channel may independently acquire critical sensor information and calculate control outputs. Cross-channel monitoring compares results, while voting or selection logic determines which output remains authoritative. Physical and electrical separation can further reduce the possibility that one power, communication, or hardware failure disables every control channel simultaneously.

Fault containment requires more than redundant processors. Independent power supplies, separated communication paths, redundant clock or timing resources, and appropriately isolated I/O can prevent common failures from propagating across channels. Software partitioning complements hardware separation by limiting memory access and communication between functions. A failure in diagnostics, logging, or a noncritical service should not be able to corrupt the execution state of the primary flight-control loop.

Timing analysis is essential because correct control output produced too late may be functionally equivalent to incorrect output. Each task has a worst-case execution time, activation period, deadline, and scheduling relationship with other tasks. Engineers analyze end-to-end latency from sensor sampling through state estimation and control computation to actuator output. Sufficient timing margin must remain under maximum processor load and credible fault conditions.

The FCC also requires controlled interfaces with the FMS, Ground Control Station (GCS), navigation computers, propulsion controllers, power-management systems, and Cargo Management System (CMS). Interface definitions specify message content, units, coordinate frames, update rates, timeout behavior, validity states, and failure responses. Strict interface contracts reduce ambiguity and allow individual subsystems to be developed and verified without uncontrolled dependencies.

Boot and initialization behavior must be deterministic because the FCC cannot begin control with unknown configuration or partially initialized data. Startup logic verifies executable software, configuration integrity, memory state, connected devices, sensor readiness, and communication availability before enabling control outputs. Critical parameters should be version controlled and checked against the installed aircraft configuration so that incompatible software or calibration data cannot silently enter operation.

Software updates require similarly strict configuration control. Flight software, bootloaders, control parameters, and hardware definitions should be uniquely identifiable so that the exact aircraft configuration can be reconstructed. Secure loading and integrity verification protect executable software from accidental corruption or unauthorized modification. Rollback or recovery mechanisms may be incorporated where they can be implemented without compromising certification and safety requirements.

Verification of FCC hardware and software progresses from component testing to integrated system testing. Unit tests evaluate individual software functions, while software-in-the-loop (SIL) environments exercise control logic against simulated vehicle dynamics. Hardware-in-the-loop (HIL) testing connects production-representative FCC hardware to real-time aircraft models, allowing sensor inputs, actuator responses, timing behavior, communication failures, and abnormal scenarios to be evaluated before flight.

The final FCC design is therefore not simply a powerful embedded computer. It is a tightly controlled hardware-software platform in which processing, memory, I/O, scheduling, control algorithms, redundancy, monitoring, configuration management, and verification are engineered together. Within the cargo UAV architecture, this integrated design provides the deterministic and fault-tolerant execution foundation required for subsequent flight-control, navigation, mission-management, and safety functions.

비행제어컴퓨터(Flight Control Computer, FCC)는 화물 무인항공기(Cargo UAV)의 안전 필수 제어 기능(Safety-Critical Control Function)을 실행하는 핵심 실시간 연산 플랫폼(Real-Time Computing Platform)이다. 하드웨어와 소프트웨어는 센서 데이터를 획득하고, 기체 상태를 추정하며, 제어 법칙을 실행하고, 시스템 상태를 감시하며, 엄격하게 제한된 시간 조건 내에서 구동기 명령을 생성할 수 있는 통합 결정론적 시스템(Deterministic System)으로 동작해야 한다. 따라서 FCC는 비행제어 아키텍처(Flight-Control Architecture)의 연산 핵심을 형성한다.

FCC 하드웨어는 범용 연산 성능의 극대화보다는 예측 가능한 실행, 고장 격리(Fault Containment), 환경적 견고성(Environmental Robustness), 충분한 연산 여유도(Computational Margin)를 중심으로 설계된다. 일반적인 플랫폼은 하나 이상의 처리장치와 휘발성 및 비휘발성 메모리, 센서 인터페이스, 통신 제어기, 이산 입출력(Discrete I/O), 감시회로(Watchdog Circuitry), 전원 감시 기능 및 하드웨어 타이밍 자원으로 구성된다. 각 구성요소는 항공기의 운용 및 안전 목표와 연계된 신뢰성 요구사항을 충족해야 한다.

프로세서 아키텍처(Processor Architecture)는 최악 조건에서도 결정론적 동작을 유지하면서 고주파 제어 루프(High-Frequency Control Loop)를 실행할 수 있는 충분한 성능을 제공해야 한다. 시스템 요구사항에 따라 범용 중앙처리장치(CPU), 마이크로컨트롤러(Microcontroller), 디지털신호처리기(DSP) 또는 이기종 처리장치(Heterogeneous Processing Device)를 선택할 수 있다. 비행 필수 실행에서는 최고 연산 처리량보다 예측 가능한 인터럽트 처리, 제한된 메모리 접근시간, 하드웨어 보호 기능 및 신뢰할 수 있는 타이밍 동작이 더욱 중요하다.

메모리 아키텍처(Memory Architecture)는 FCC의 신뢰성에 직접적인 영향을 미친다. 프로그램 코드, 구성 파라미터, 보정 데이터(Calibration Data), 런타임 상태 및 비행 로그는 서로 다른 데이터 유지성과 무결성 요구사항을 갖는다. 플래시 메모리(Flash Memory) 등의 비휘발성 저장장치는 실행 소프트웨어와 검증된 구성 데이터를 보존하고, 램(RAM)은 실시간 계산을 지원한다. 오류 검출 및 정정, 메모리 보호, 범위 검사, 통제된 초기화를 통해 손상된 데이터가 비행제어 계산으로 전파되는 것을 방지할 수 있다.

FCC는 관성측정장치(Inertial Measurement Unit, IMU), 위성항법시스템(GNSS) 수신기, 대기자료 센서(Air-Data Sensor), 레이더 또는 레이저 고도계, 추진 시스템 및 구동기 피드백 채널에서 데이터를 수신한다. 하드웨어 인터페이스에는 직렬 링크, CAN 기반 네트워크, 이더넷 계열 통신, 이산 신호 및 전용 센서 버스가 포함될 수 있다. 입력 데이터는 제어 소프트웨어가 사용하기 전에 타임스탬프(Timestamp), 유효성 정보, 정보원 식별자 및 진단 상태와 연결되어 처리되어야 한다.

출력 인터페이스(Output Interface)는 FCC를 모터, 엔진 제어기, 서보(Servo), 비행조종면(Flight-Control Surface) 또는 기타 구동장치와 연결한다. 명령 생성 과정에서는 구동기의 위치, 속도, 토크, 전류 및 열적 한계를 준수해야 한다. 또한 FCC는 명령된 움직임에 대응하여 예상된 물리적 반응이 발생하는지 확인해야 한다. 이러한 명령-피드백 관계(Command-and-Feedback Relationship)를 통해 구동기 고착, 인터페이스 단절, 추진 채널 성능 저하 등 명령 생성만으로는 발견할 수 없는 고장을 식별할 수 있다.

소프트웨어 수준에서 FCC는 일반적으로 하드웨어 접근 기능과 응용 수준의 비행 기능을 분리하는 계층형 구성요소(Layered Component)로 구성된다. 보드지원패키지(Board Support Package, BSP)와 장치 드라이버 계층(Device-Driver Layer)은 프로세서 주변장치와 물리적 인터페이스를 관리한다. 운영체제 또는 스케줄링 서비스는 태스크 실행과 타이밍을 제공하고, 미들웨어(Middleware)와 데이터 서비스는 검증된 정보를 전달한다. 그 위에서 비행제어 응용 소프트웨어가 상태추정, 유도 인터페이스, 제어 법칙, 모드 로직, 감시 및 구동기 명령 생성을 수행한다.

실시간운영체제(Real-Time Operating System, RTOS)는 개별 태스크에 실행 주기, 우선순위, 마감시간(Deadline), 통신 메커니즘을 할당하여 FCC 소프트웨어에 결정론적 스케줄링(Deterministic Scheduling)을 제공할 수 있다. 고속 센서 획득 및 제어 루프는 항법관리, 진단, 로깅 또는 통신 태스크보다 훨씬 높은 주파수로 실행될 수 있다. 스케줄은 낮은 우선순위의 작업이 비행 필수 계산을 허용된 마감시간 이상으로 지연시키지 않도록 설계되어야 한다.

주요 실행 파이프라인(Execution Pipeline)은 센서 데이터 획득 및 검증에서 시작된다. 원시 측정값은 상태추정 기능으로 전달되기 전에 범위, 최신성(Freshness), 일관성 및 통신 무결성을 검사받는다. 상태추정기(Estimator)는 이용 가능한 측정값을 결합하여 자세, 각속도, 위치, 속도, 고도 및 기타 필요한 상태를 결정한다. 이렇게 추정된 상태는 항공기의 안정화와 기동을 담당하는 비행제어 알고리즘의 피드백 입력으로 사용된다.

제어 법칙 소프트웨어(Control-Law Software)는 원하는 비행 상태를 물리적으로 실현 가능한 명령으로 변환한다. 내부 루프(Inner Loop)는 일반적으로 각속도와 자세처럼 빠르게 변화하는 변수를 제어하며, 외부 루프(Outer Loop)는 고도, 속도 및 위치를 제어할 수 있다. FCC는 이러한 루프를 조정하여 비행관리시스템(Flight Management System, FMS)으로부터 받은 상위 수준 명령을 단계적으로 제어 목표로 변환하고, 최종적으로 항공기 구성에 적합한 추진 또는 구동기 명령으로 변환한다.

화물 UAV 운용에서는 질량과 무게중심(Center of Gravity)의 상당한 변화가 발생한다. 따라서 FCC 소프트웨어는 서로 다른 탑재화물 구성, 연료 소비, 화물 이동 또는 화물 투하로 인한 변화를 고려해야 한다. 검증된 스케줄링, 파라미터 적응(Parameter Adaptation) 또는 모델 기반 보상(Model-Based Compensation)을 통해 현재 기체 구성에 따라 제어기의 동작을 조정할 수 있다. 모든 적응 기능은 비행 필수 제어 동작을 제한 없이 변경하는 것이 아니라 정의된 안전 경계 내에서 수행되어야 한다.

비행모드 관리(Flight-Mode Management)는 수동, 보조, 자율 및 비상대응 상태를 조정한다. FCC는 모드 요청을 해석하는 동시에 전환 조건이 충족되었는지 검증한다. 예를 들어 필요한 항법 정보가 제공되지 않는다면 요청된 자율착륙 모드(Autonomous Landing Mode)로 전환해서는 안 된다. 명확한 전환 가드(Transition Guard)와 대체 상태(Fallback State)를 적용하면 잘못된 제어 기능 조합을 방지하고 비정상 상황에서 시스템 동작을 더욱 예측 가능하게 만들 수 있다.

상태감시(Health Monitoring)는 정상적인 제어 실행과 동시에 지속적으로 수행된다. 소프트웨어는 프로세서 상태, 태스크 타이밍, 센서 데이터 최신성, 통신 링크, 구동기 피드백, 전원 상태 및 내부 데이터 일관성을 감시한다. 하드웨어 감시 타이머(Hardware Watchdog)는 실행 정지를 독립적으로 탐지할 수 있으며, 소프트웨어 감시 기능과 하트비트(Heartbeat) 메커니즘은 개별 태스크 또는 연산 채널을 감독한다. 감지된 고장은 운용 중요도에 따라 격리, 허용 또는 대응할 수 있도록 분류된다.

단일 고장으로 인해 제어 기능이 상실되는 것을 허용할 수 없는 경우 이중 또는 삼중 연산 채널(Dual or Triple Computing Channel)을 갖는 중복 FCC 아키텍처(Redundant FCC Architecture)를 사용할 수 있다. 각 채널은 핵심 센서 정보를 독립적으로 획득하고 제어 출력을 계산할 수 있다. 채널 간 감시(Cross-Channel Monitoring)는 계산 결과를 비교하며, 투표 또는 선택 로직(Voting or Selection Logic)은 어떤 출력을 최종 권한 출력으로 사용할지 결정한다. 물리적·전기적 분리를 적용하면 하나의 전원, 통신 또는 하드웨어 고장이 모든 제어 채널을 동시에 정지시킬 가능성을 더욱 줄일 수 있다.

고장 격리(Fault Containment)는 단순히 프로세서를 중복하는 것만으로 달성되지 않는다. 독립 전원공급장치, 분리된 통신 경로, 중복 클록 또는 타이밍 자원, 적절하게 격리된 입출력(I/O)을 사용하여 공통 고장이 여러 채널로 전파되는 것을 방지할 수 있다. 소프트웨어 파티셔닝(Software Partitioning)은 기능 사이의 메모리 접근과 통신을 제한하여 하드웨어 분리를 보완한다. 진단, 로깅 또는 비필수 서비스의 고장이 주 비행제어 루프의 실행 상태를 손상시켜서는 안 된다.

정확한 제어 출력이라도 지나치게 늦게 생성되면 기능적으로 잘못된 출력과 동일한 결과를 초래할 수 있으므로 타이밍 분석(Timing Analysis)은 필수적이다. 각 태스크에는 최악실행시간(Worst-Case Execution Time), 활성화 주기, 마감시간 및 다른 태스크와의 스케줄링 관계가 존재한다. 엔지니어는 센서 샘플링에서 상태추정과 제어 계산을 거쳐 구동기 출력에 이르는 종단 간 지연시간(End-to-End Latency)을 분석한다. 최대 프로세서 부하 및 신뢰 가능한 고장 조건에서도 충분한 타이밍 여유를 유지해야 한다.

FCC에는 FMS, 지상통제소(Ground Control Station, GCS), 항법 컴퓨터, 추진 제어기, 전력관리시스템 및 화물관리시스템(Cargo Management System, CMS)과 연결되는 통제된 인터페이스가 필요하다. 인터페이스 정의에는 메시지 내용, 단위, 좌표계(Coordinate Frame), 업데이트 주기, 타임아웃 동작, 유효성 상태 및 고장 대응 방법이 포함된다. 엄격한 인터페이스 계약(Interface Contract)은 모호성을 줄이고 각 하위 시스템을 통제되지 않은 종속성 없이 독립적으로 개발하고 검증할 수 있도록 한다.

FCC는 알려지지 않은 구성이나 부분적으로 초기화된 데이터로 제어를 시작해서는 안 되므로 부팅 및 초기화(Boot and Initialization) 동작 역시 결정론적이어야 한다. 시작 로직은 제어 출력을 활성화하기 전에 실행 소프트웨어, 구성 무결성, 메모리 상태, 연결 장치, 센서 준비 상태 및 통신 가용성을 검증한다. 핵심 파라미터는 버전 관리(Version Control)되어야 하며 설치된 항공기 구성과 비교하여 호환되지 않는 소프트웨어 또는 보정 데이터가 운용에 사용되는 것을 방지해야 한다.

소프트웨어 업데이트(Software Update)에도 동일하게 엄격한 구성관리(Configuration Control)가 요구된다. 비행 소프트웨어, 부트로더(Bootloader), 제어 파라미터 및 하드웨어 정의에는 고유한 식별정보가 부여되어 정확한 항공기 구성을 재구성할 수 있어야 한다. 안전한 로딩(Secure Loading)과 무결성 검증(Integrity Verification)은 실행 소프트웨어를 우발적인 손상이나 승인되지 않은 변경으로부터 보호한다. 인증 및 안전 요구사항을 훼손하지 않는 범위에서는 롤백(Rollback) 또는 복구 메커니즘을 적용할 수도 있다.

FCC 하드웨어와 소프트웨어의 검증(Verification)은 구성요소 시험에서 통합 시스템 시험으로 단계적으로 확장된다. 단위시험(Unit Test)은 개별 소프트웨어 기능을 평가하며, 소프트웨어 인더루프(Software-in-the-Loop, SIL) 환경은 시뮬레이션된 기체 동역학을 이용해 제어 로직을 시험한다. 하드웨어 인더루프(Hardware-in-the-Loop, HIL) 시험은 실제 또는 양산 대표 FCC 하드웨어를 실시간 항공기 모델과 연결하여 센서 입력, 구동기 응답, 타이밍 동작, 통신 고장 및 비정상 시나리오를 실제 비행 전에 평가할 수 있도록 한다.

최종적인 FCC 설계는 단순히 강력한 임베디드 컴퓨터(Embedded Computer)를 구축하는 것이 아니다. 연산, 메모리, 입출력, 스케줄링, 제어 알고리즘, 이중화, 감시, 구성관리 및 검증이 하나의 체계로 함께 설계되는 엄격하게 통제된 하드웨어-소프트웨어 플랫폼(Hardware-Software Platform)이다. 화물 UAV 아키텍처에서 이러한 통합 설계는 이후의 비행제어, 항법, 임무관리 및 안전 기능을 안정적으로 실행하기 위해 필요한 결정론적이며 고장 허용적인 실행 기반(Deterministic and Fault-Tolerant Execution Foundation)을 제공한다.

##  

## 02.03. Flight Management System FMS Architecture [w/Code]

![](images/image3.png){width="7.268055555555556in" height="7.268055555555556in"}

The Flight Management System (FMS) provides the mission-level planning, navigation, trajectory management, and supervisory logic that connects operational objectives with the real-time flight-control functions of a cargo UAV. Positioned above the Flight Control Computer (FCC), the FMS determines what the aircraft should accomplish and where it should fly, while the FCC determines how commanded flight states are safely achieved through control and actuation.

The FMS architecture receives information from multiple onboard and external systems. Navigation state, aircraft configuration, payload status, energy reserves, weather information, airspace constraints, mission plans, and Ground Control Station (GCS) commands are combined into a coherent operational picture. This information allows the FMS to continuously evaluate whether the planned mission remains feasible and whether the aircraft is progressing within approved operational boundaries.

Mission planning begins with a structured representation of departure points, destinations, intermediate waypoints, altitude constraints, speed requirements, arrival conditions, and alternate landing locations. For cargo operations, the plan may additionally contain loading and unloading events, delivery priorities, ground-handling states, and payload-specific restrictions. The FMS converts these operational requirements into an executable sequence of flight and mission phases.

A mission-state manager coordinates progression through states such as initialization, preflight verification, takeoff, climb, cruise, approach, landing, cargo handling, and mission completion. Each transition is controlled by explicit conditions rather than simple elapsed time. Vehicle health, navigation validity, airspace authorization, energy availability, payload status, and destination readiness can therefore determine whether the next mission phase is permitted to begin.

Trajectory management converts the mission plan into spatial and temporal flight objectives. The FMS can maintain waypoint sequences, route segments, altitude profiles, speed schedules, and time constraints while continuously comparing the planned trajectory with the aircraft\'s estimated state. The resulting guidance objectives are transferred to the FCC as bounded commands rather than direct actuator instructions, preserving the architectural separation between mission guidance and vehicle stabilization.

Navigation management provides the FMS with a consistent representation of aircraft position, velocity, altitude, heading, and route progress. The FMS should not treat navigation data as valid merely because values are available. Integrity indicators, sensor health, uncertainty estimates, update age, and source availability must be considered when deciding whether autonomous navigation functions can continue or whether degraded operating modes are required.

Route management must account for more than geometric distance. Cargo UAV trajectories can be constrained by controlled airspace, geofences, terrain, population exposure, weather, communication coverage, aircraft performance, and available landing locations. The FMS evaluates these constraints against the active flight plan and can identify conditions in which a nominal route is no longer operationally acceptable even though the aircraft remains physically capable of following it.

Energy management is particularly important for heavy cargo UAVs because payload mass, wind, temperature, flight altitude, and mission profile can significantly alter energy consumption. The FMS tracks available battery energy, fuel, or hybrid-system reserves and compares them with predicted requirements for the remaining mission. Reserve thresholds must include sufficient margin for diversion, holding, landing, and credible off-nominal conditions rather than representing only nominal destination energy.

The FMS can periodically recompute mission feasibility as actual performance diverges from preflight assumptions. Strong headwinds, unexpected holding, propulsion degradation, or higher-than-expected energy consumption may reduce the reachable mission envelope. Instead of waiting until reserves become critical, the system can identify deteriorating margins early and recommend or automatically select an alternate route, destination, or contingency strategy according to predefined authority.

Cargo information also influences mission-level decision making. Payload mass and center of gravity affect aircraft performance, while cargo temperature, securing status, release state, or delivery priority may affect mission constraints. Interfaces between the FMS and Cargo Management System (CMS) allow relevant payload conditions to become part of mission logic without placing detailed cargo-device control inside the flight-management function.

The FMS maintains a controlled interface with the FCC. Commands may represent desired position, velocity, altitude, heading, trajectory segment, or flight-mode request, depending on the aircraft design. Every command should include defined validity and timing semantics. The FCC remains responsible for rejecting or limiting requests that violate immediate control, configuration, or flight-envelope constraints, preventing mission logic from directly overriding fundamental vehicle protection.

Communication with the GCS provides mission upload, modification, supervision, and operational authorization functions. Operators can inspect route progress, vehicle health, predicted arrival information, energy margins, active constraints, and contingency status. Changes received from the ground should be authenticated, checked for consistency, and evaluated against the current aircraft state before acceptance so that an inappropriate command cannot immediately destabilize mission execution.

Loss of the command-and-control link must not leave the FMS without a defined operational objective. Preconfigured lost-link logic can specify whether the aircraft should continue the route, hold at a safe location, return to base, divert, or land at an alternate site. The selected behavior may depend on mission phase, energy reserve, airspace conditions, vehicle health, and the availability of validated landing locations at the time communication is lost.

Contingency management is therefore a central FMS responsibility. The system continuously evaluates events such as navigation degradation, propulsion faults, low energy, adverse weather, airspace closure, destination unavailability, communication loss, or cargo anomalies. Rather than treating every fault identically, contingency logic evaluates operational consequences and selects a response appropriate to the severity, remaining aircraft capability, and available recovery options.

The FMS requires explicit priority rules when multiple objectives conflict. Continuing a delivery, minimizing energy consumption, meeting arrival time, avoiding restricted airspace, maintaining communication coverage, and preserving reserve margins cannot always be optimized simultaneously. Safety constraints must remain dominant, followed by operational requirements according to defined policy. This hierarchy prevents optimization functions from trading mandatory safety margins for secondary mission performance.

Software architecture can separate flight-plan management, trajectory generation, navigation management, performance prediction, energy estimation, contingency logic, communication interfaces, and mission-state control into independently testable components. Shared data should be exchanged through controlled interfaces with defined units, coordinate frames, timestamps, validity states, and update rates. This modular structure limits unintended coupling and improves verification of complex mission behavior.

Deterministic execution remains important even though many FMS functions operate more slowly than FCC control loops. Navigation updates, trajectory calculations, health evaluations, and contingency decisions must complete within bounded periods appropriate to their operational function. Computationally expensive route optimization should not prevent urgent safety events from being processed, so scheduling and resource allocation must distinguish critical supervisory functions from background optimization activities.

Persistent data management supports mission continuity and post-flight reconstruction. The FMS can retain the active flight plan, configuration version, waypoint progression, operator modifications, contingency transitions, significant warnings, and relevant performance estimates. Carefully controlled persistence can also support recovery after selected resets, provided that restored information is validated against the current physical state rather than blindly resuming a previously stored mission state.

Cybersecurity is essential because the FMS processes information capable of changing aircraft destination and mission behavior. Flight-plan uploads, GCS commands, database updates, configuration changes, and external airspace information require authentication and integrity protection. Access control should distinguish operational authority from maintenance or diagnostic privileges, while secure logging provides traceability for important mission changes and command transactions.

Verification begins with individual FMS software components and expands toward complete mission scenarios. Software-in-the-loop (SIL) testing can evaluate route logic, state transitions, energy prediction, and contingency behavior across large numbers of simulated missions. Hardware-in-the-loop (HIL) testing then integrates representative FMS and FCC hardware with aircraft dynamics, communication links, navigation sensors, and fault injection to evaluate end-to-end behavior before flight testing.

The resulting FMS architecture acts as the operational intelligence layer between mission intent and deterministic flight execution. By integrating flight plans, navigation state, aircraft performance, energy reserves, cargo information, airspace constraints, GCS interaction, and contingency logic, it enables the cargo UAV to manage complex missions without compromising the FCC\'s authority over immediate vehicle control. This separation provides a scalable foundation for increasingly autonomous cargo operations.

비행관리시스템(Flight Management System, FMS)은 화물 무인항공기(Cargo UAV)의 운용 목표를 실시간 비행제어 기능과 연결하는 임무 수준 계획, 항법, 궤적 관리 및 감독 로직을 제공한다. 비행제어컴퓨터(Flight Control Computer, FCC)의 상위 계층에 위치한 FMS는 항공기가 무엇을 수행하고 어디로 비행해야 하는지를 결정하며, FCC는 명령된 비행 상태를 제어와 구동을 통해 어떻게 안전하게 달성할 것인지를 결정한다.

FMS 아키텍처는 여러 탑재 시스템과 외부 시스템으로부터 정보를 수신한다. 항법 상태, 항공기 구성, 탑재화물 상태, 에너지 잔량, 기상 정보, 공역 제약조건, 임무계획 및 지상통제소(Ground Control Station, GCS) 명령을 결합하여 일관된 운용 상황정보(Operational Picture)를 구성한다. 이를 통해 FMS는 계획된 임무가 계속 수행 가능한지와 항공기가 승인된 운용 경계 내에서 진행되고 있는지를 지속적으로 평가할 수 있다.

임무계획(Mission Planning)은 출발지, 목적지, 중간 웨이포인트(Waypoint), 고도 제약조건, 속도 요구사항, 도착 조건 및 대체 착륙지에 대한 구조화된 표현에서 시작한다. 화물 운송에서는 적재 및 하역 이벤트, 배송 우선순위, 지상조업 상태 및 화물별 제한조건이 추가될 수 있다. FMS는 이러한 운용 요구사항을 실행 가능한 비행 및 임무 단계의 연속적인 시퀀스로 변환한다.

임무상태 관리자(Mission-State Manager)는 초기화, 비행 전 검증, 이륙, 상승, 순항, 접근, 착륙, 화물 처리 및 임무 완료와 같은 상태의 진행을 조정한다. 각 상태 전환은 단순한 경과시간이 아니라 명시적인 조건에 의해 제어된다. 따라서 기체 상태, 항법 유효성, 공역 승인, 에너지 가용성, 화물 상태 및 목적지 준비 여부를 바탕으로 다음 임무 단계의 시작 가능 여부를 결정할 수 있다.

궤적관리(Trajectory Management)는 임무계획을 공간적·시간적 비행 목표로 변환한다. FMS는 웨이포인트 시퀀스, 항로 구간, 고도 프로파일, 속도 계획 및 시간 제약조건을 유지하면서 계획된 궤적과 항공기의 추정 상태를 지속적으로 비교할 수 있다. 생성된 유도 목표(Guidance Objective)는 직접적인 구동기 명령이 아니라 제한된 명령 형태로 FCC에 전달되어 임무 유도와 기체 안정화 사이의 아키텍처 분리를 유지한다.

항법관리(Navigation Management)는 항공기 위치, 속도, 고도, 기수방향 및 항로 진행 상태를 일관된 형태로 FMS에 제공한다. FMS는 단순히 값이 존재한다는 이유만으로 항법 데이터를 유효하다고 판단해서는 안 된다. 자율항법 기능의 지속 여부 또는 성능저하 운용모드(Degraded Operating Mode)로의 전환 여부를 결정할 때 무결성 지표, 센서 상태, 불확실성 추정값, 데이터 갱신 경과시간 및 정보원 가용성을 함께 고려해야 한다.

항로관리(Route Management)는 단순한 기하학적 거리 이상의 요소를 고려해야 한다. 화물 UAV의 궤적은 관제 공역, 지오펜스(Geofence), 지형, 인구 노출도, 기상, 통신 범위, 항공기 성능 및 이용 가능한 착륙지에 의해 제한될 수 있다. FMS는 이러한 제약조건을 활성 비행계획과 비교하여 항공기가 물리적으로 해당 경로를 비행할 수 있더라도 정상 항로가 더 이상 운용상 적합하지 않은 상황을 식별할 수 있다.

에너지관리(Energy Management)는 탑재화물 질량, 바람, 온도, 비행고도 및 임무 프로파일이 에너지 소비량을 크게 변화시킬 수 있기 때문에 대형 화물 UAV에서 특히 중요하다. FMS는 사용 가능한 배터리 에너지, 연료 또는 하이브리드 시스템의 잔량을 추적하고 이를 잔여 임무의 예상 요구량과 비교한다. 예비 에너지 기준(Reserve Threshold)은 정상 목적지 도달에 필요한 에너지만이 아니라 회항, 체공, 착륙 및 신뢰 가능한 비정상 조건을 위한 충분한 여유를 포함해야 한다.

FMS는 실제 성능이 비행 전 가정에서 벗어남에 따라 임무 수행 가능성(Mission Feasibility)을 주기적으로 다시 계산할 수 있다. 강한 역풍, 예상하지 못한 체공, 추진 성능 저하 또는 예상보다 높은 에너지 소비는 도달 가능한 임무 범위를 축소시킬 수 있다. 시스템은 에너지 잔량이 위험 수준에 도달할 때까지 기다리지 않고 여유도가 악화되는 상황을 조기에 식별하여 사전에 정의된 권한에 따라 대체 항로, 목적지 또는 비상대응 전략을 권고하거나 자동으로 선택할 수 있다.

화물 정보 역시 임무 수준 의사결정에 영향을 준다. 탑재화물 질량과 무게중심(Center of Gravity)은 항공기 성능에 영향을 미치며, 화물 온도, 고정 상태, 투하 상태 또는 배송 우선순위는 임무 제약조건에 영향을 줄 수 있다. FMS와 화물관리시스템(Cargo Management System, CMS) 사이의 인터페이스를 통해 상세한 화물 장치 제어 기능을 비행관리 기능 내부에 포함하지 않으면서 관련 화물 상태를 임무 로직에 반영할 수 있다.

FMS는 FCC와 통제된 인터페이스(Controlled Interface)를 유지한다. 항공기 설계에 따라 명령은 원하는 위치, 속도, 고도, 기수방향, 궤적 구간 또는 비행모드 요청을 나타낼 수 있다. 모든 명령에는 정의된 유효성 및 타이밍 의미가 포함되어야 한다. FCC는 즉각적인 제어, 기체 구성 또는 비행영역 제약을 위반하는 요청을 거부하거나 제한할 책임을 유지함으로써 임무 로직이 기본적인 기체 보호 기능을 직접 무시하지 못하도록 한다.

GCS와의 통신은 임무 업로드, 수정, 감독 및 운용 승인 기능을 제공한다. 운용자는 항로 진행 상황, 기체 상태, 예상 도착 정보, 에너지 여유도, 활성 제약조건 및 비상대응 상태를 확인할 수 있다. 지상에서 수신되는 변경사항은 부적절한 명령이 임무 실행에 즉각적인 영향을 미치지 않도록 인증되고, 일관성을 검사하며, 현재 항공기 상태와 비교하여 평가한 후 승인되어야 한다.

명령통제 링크(Command and Control Link)의 상실로 인해 FMS의 운용 목표가 정의되지 않은 상태가 되어서는 안 된다. 사전에 구성된 통신두절 로직(Lost-Link Logic)은 항공기가 현재 항로를 계속 비행할지, 안전한 위치에서 체공할지, 기지로 복귀할지, 대체 목적지로 회항할지 또는 대체 착륙지에 착륙할지를 규정할 수 있다. 선택되는 동작은 통신이 상실된 시점의 임무 단계, 에너지 잔량, 공역 조건, 기체 상태 및 검증된 착륙지의 가용성에 따라 달라질 수 있다.

따라서 비상대응 관리(Contingency Management)는 FMS의 핵심 책임이다. 시스템은 항법 성능 저하, 추진 시스템 고장, 에너지 부족, 악천후, 공역 폐쇄, 목적지 이용 불가, 통신 상실 또는 화물 이상과 같은 이벤트를 지속적으로 평가한다. 모든 고장을 동일하게 처리하는 대신 비상대응 로직은 운용상의 영향을 평가하고 고장의 심각도, 잔여 항공기 성능 및 이용 가능한 복구 방법에 적합한 대응을 선택한다.

여러 목표가 충돌하는 경우 FMS에는 명확한 우선순위 규칙(Priority Rule)이 필요하다. 배송 지속, 에너지 소비 최소화, 도착시간 준수, 제한 공역 회피, 통신 범위 유지 및 예비 에너지 확보를 항상 동시에 최적화할 수 있는 것은 아니다. 안전 제약조건(Safety Constraint)이 가장 높은 우선순위를 유지하고, 이후 정의된 정책에 따라 운용 요구사항이 적용되어야 한다. 이러한 계층구조는 최적화 기능이 부차적인 임무 성능을 위해 필수 안전 여유도를 희생하는 것을 방지한다.

소프트웨어 아키텍처(Software Architecture)는 비행계획 관리, 궤적 생성, 항법관리, 성능 예측, 에너지 추정, 비상대응 로직, 통신 인터페이스 및 임무상태 제어를 독립적으로 시험 가능한 구성요소로 분리할 수 있다. 공유 데이터는 정의된 단위, 좌표계(Coordinate Frame), 타임스탬프(Timestamp), 유효성 상태 및 업데이트 주기를 갖는 통제된 인터페이스를 통해 교환되어야 한다. 이러한 모듈형 구조(Modular Structure)는 의도하지 않은 결합을 제한하고 복잡한 임무 동작의 검증성을 향상시킨다.

많은 FMS 기능이 FCC 제어 루프보다 낮은 주파수로 동작하더라도 결정론적 실행(Deterministic Execution)은 여전히 중요하다. 항법 업데이트, 궤적 계산, 상태 평가 및 비상대응 결정은 각 운용 기능에 적합한 제한된 시간 내에 완료되어야 한다. 계산량이 많은 항로 최적화가 긴급 안전 이벤트의 처리를 방해해서는 안 되므로 스케줄링과 자원 할당은 핵심 감독 기능과 백그라운드 최적화 작업을 구분해야 한다.

영구 데이터 관리(Persistent Data Management)는 임무 연속성과 비행 후 상황 재구성을 지원한다. FMS는 활성 비행계획, 구성 버전, 웨이포인트 진행 상태, 운용자 수정사항, 비상대응 상태 전환, 주요 경고 및 관련 성능 추정값을 저장할 수 있다. 통제된 데이터 영속성은 특정 시스템 재시작 이후 복구를 지원할 수도 있지만, 저장된 이전 임무 상태를 그대로 재개하는 것이 아니라 현재의 물리적 기체 상태와 비교하여 복구 정보를 검증해야 한다.

FMS는 항공기의 목적지와 임무 동작을 변경할 수 있는 정보를 처리하므로 사이버보안(Cybersecurity)이 필수적이다. 비행계획 업로드, GCS 명령, 데이터베이스 업데이트, 구성 변경 및 외부 공역 정보에는 인증(Authentication)과 무결성 보호(Integrity Protection)가 필요하다. 접근제어(Access Control)는 운용 권한과 정비 또는 진단 권한을 구분해야 하며, 안전한 로깅(Secure Logging)은 중요한 임무 변경과 명령 처리에 대한 추적성을 제공한다.

검증(Verification)은 개별 FMS 소프트웨어 구성요소에서 시작하여 전체 임무 시나리오로 확장된다. 소프트웨어 인더루프(Software-in-the-Loop, SIL) 시험은 많은 수의 시뮬레이션 임무를 통해 항로 로직, 상태 전환, 에너지 예측 및 비상대응 동작을 평가할 수 있다. 이후 하드웨어 인더루프(Hardware-in-the-Loop, HIL) 시험에서는 대표적인 FMS 및 FCC 하드웨어를 항공기 동역학, 통신 링크, 항법 센서 및 고장 주입(Fault Injection) 환경과 통합하여 비행시험 이전에 종단 간 시스템 동작을 평가한다.

최종적인 FMS 아키텍처는 임무 의도(Mission Intent)와 결정론적 비행 실행(Deterministic Flight Execution) 사이에서 운용 지능 계층(Operational Intelligence Layer)으로 기능한다. 비행계획, 항법 상태, 항공기 성능, 에너지 잔량, 화물 정보, 공역 제약조건, GCS 상호작용 및 비상대응 로직을 통합함으로써 FCC가 즉각적인 기체 제어 권한을 유지하는 동시에 화물 UAV가 복잡한 임무를 자율적으로 관리할 수 있도록 한다. 이러한 기능 분리는 더욱 높은 수준의 자율 화물 운송을 구현하기 위한 확장 가능한 기반을 제공한다.

##  

## 02.04. Redundant Computing Architecture Dual Triple [w/Code]

![](images/image4.png){width="7.268055555555556in" height="7.268055555555556in"}

Redundant computing architecture is a fundamental design mechanism for cargo UAVs whose flight-critical functions cannot tolerate the loss or corruption of a single computing channel. Dual and triple architectures replicate processors, interfaces, power paths, or complete Flight Control Computer functions so that faults can be detected, isolated, and tolerated while the aircraft remains controllable. The objective is not duplication itself, but continued safe operation after credible failures.

A computing channel normally contains the processing resources, memory, timing services, communication interfaces, and input/output functions required to execute its assigned flight-control software. In a redundant system, channels should be sufficiently independent that a fault in one channel does not automatically propagate into the others. Independence therefore includes electrical, logical, timing, communication, and software considerations rather than simply installing multiple processors.

A dual-redundant architecture uses two computing channels that execute equivalent or complementary safety-critical functions. Both channels can receive common aircraft-state information and independently calculate control outputs. Their results are compared continuously to detect disagreement. If both channels agree, operation proceeds normally; if they disagree, additional monitoring or external information is required to determine which channel should remain authoritative.

The principal limitation of simple dual redundancy is ambiguity after disagreement. When Channel A and Channel B produce different results, comparison alone cannot prove which channel is correct. The architecture therefore requires additional fault-detection mechanisms such as processor self-tests, watchdogs, sensor consistency checks, timing supervision, communication diagnostics, or an independent monitor capable of identifying the failed channel with sufficient confidence.

Dual architectures can operate in active-active or active-standby configurations. In an active-active design, both channels execute the control function simultaneously and their outputs are monitored continuously. In an active-standby design, one channel controls the aircraft while the second remains synchronized and prepared to assume control. The standby approach simplifies output authority but requires reliable failure detection and sufficiently fast transfer without unsafe command discontinuities.

Triple-redundant computing introduces a third independent channel and enables majority voting when all three channels calculate comparable outputs. If two channels agree within defined tolerances while one differs significantly, the disagreeing channel can be identified as the likely faulty member. Triple Modular Redundancy (TMR) therefore provides stronger fault discrimination than a simple dual arrangement and is attractive for highly critical control functions.

Voting logic is central to triple-channel operation. The voter may compare discrete states, numerical control values, mode selections, health indicators, or other safety-relevant outputs. Numerical values normally require tolerance-based comparison because independently executing channels may exhibit small differences. The voting thresholds must distinguish acceptable computational variation from meaningful divergence without creating unnecessary channel rejection during normal operation.

The voter itself becomes a critical architectural element because a faulty centralized voter could compromise otherwise healthy channels. Designs can therefore distribute voting functions, replicate voters, or place comparison logic at multiple stages of the signal path. The objective is to prevent redundancy management from introducing a new single point of failure. Voter health and communication integrity must consequently be included in system-level fault analysis.

Redundant processing provides limited benefit if every channel depends on the same power supply, clock, communication switch, or sensor interface. Cargo UAV architectures must examine common-cause failures that can defeat multiple channels simultaneously. Independent power feeds, separated communication paths, redundant timing references, isolated I/O circuitry, and physical separation can prevent a localized electrical or hardware fault from disabling the entire flight-control capability.

Sensor redundancy must be coordinated with computing redundancy. Multiple FCC channels receiving information from only one IMU or navigation source remain vulnerable to that common sensor failure. Critical state variables can therefore be supported by multiple inertial, GNSS, air-data, altitude, or propulsion feedback sources. Sensor selection and voting logic should preserve source identity so that erroneous measurements can be isolated rather than blindly propagated to every computer.

Communication architecture also influences fault containment. Redundant channels may use independent avionics buses or separated network paths to exchange health information and cross-channel data. Messages should include source identification, sequence information, timestamps, validity state, and integrity checks. A corrupted or delayed communication channel must be distinguishable from an actual disagreement in the underlying control computation.

Cross-channel monitoring compares more than final actuator commands. Channels can compare estimated aircraft state, active flight mode, control-law outputs, configuration data, timing status, and internal health indicators. Detecting divergence early in the processing chain helps identify the origin of a fault before it reaches actuators. However, excessive cross-coupling should be avoided because independence can be weakened if channels rely too heavily on one another.

Redundant channels also require synchronization. Equivalent computations should operate on sensor information representing sufficiently consistent physical times, particularly for rapidly changing attitude and angular-rate data. Time synchronization and timestamp management prevent normal timing offsets from appearing as computational disagreement. At the same time, channels should avoid dependence on a single timing component whose failure could simultaneously corrupt all synchronized computers.

Software design must address the possibility of common-mode software faults. Running identical software on three identical processors protects effectively against many random hardware failures but may not protect against a systematic algorithm or implementation defect shared by every channel. Independent monitoring, diversified implementations, dissimilar hardware, or separately developed safety functions can be considered when system safety analysis identifies common software behavior as an unacceptable risk.

Redundancy management must define what happens after a channel is declared faulty. The failed channel may be isolated from actuator outputs, excluded from voting, reset, or retained only for diagnostic purposes. Remaining channels must update their redundancy state so that subsequent failures are interpreted correctly. A triple system degraded to two healthy channels, for example, no longer possesses the same majority-voting capability that existed before the first failure.

Graceful degradation allows the aircraft to continue operating with reduced functionality rather than immediately losing control. Following a redundancy loss, mission-level functions may command return-to-home, diversion, reduced flight-envelope operation, or landing at a suitable site. The response should depend on remaining computing capability, aircraft condition, mission phase, environmental conditions, and the safety consequences of continued flight.

Actuator command authority requires special attention during channel transitions. A newly selected computer should not introduce abrupt command changes merely because its internal state differs slightly from that of the previously active channel. State synchronization, command limiting, bumpless transfer techniques, and actuator arbitration can maintain continuity when control authority changes. Transition behavior must be validated under both normal and faulted conditions.

Startup behavior is also part of redundant architecture. Each channel must verify memory integrity, executable software, configuration data, sensor connectivity, communication interfaces, timing resources, and internal diagnostics before becoming eligible for control authority. Cross-channel checks can confirm compatible software and configuration versions. A channel that fails initialization should remain isolated rather than reducing the integrity of healthy channels.

Fault detection must balance sensitivity and availability. Thresholds that are too permissive can allow a faulty computer to remain active, while thresholds that are too sensitive can incorrectly reject healthy channels because of temporary timing differences or sensor noise. Debouncing, persistence checks, confidence measures, and fault classification can help distinguish transient anomalies from persistent failures requiring isolation or reconfiguration.

Redundant architecture is verified through systematic fault injection and integration testing. Software-in-the-loop environments can evaluate voting, disagreement detection, and degraded-state logic across many simulated failures. Hardware-in-the-loop testing can introduce processor resets, communication loss, corrupted sensor inputs, timing faults, power interruptions, and actuator anomalies while real FCC hardware interacts with simulated aircraft dynamics.

Testing must also address combinations and sequences of failures rather than only isolated faults. A triple-channel system may behave correctly after one channel failure but respond differently if a communication fault occurs after degradation to dual operation. Verification therefore examines transitions among fully redundant, degraded, fail-operational, and fail-safe states and confirms that the aircraft maintains defined behavior throughout each transition.

The choice between dual and triple computing is ultimately driven by safety objectives, aircraft scale, operational exposure, failure probability, weight, power, cost, and certification requirements. Dual redundancy can provide efficient fault detection and backup capability when sufficient independent monitoring exists, while triple redundancy offers stronger fault isolation through majority voting. For cargo UAVs, the appropriate architecture results from system safety analysis rather than a universal channel count.

A well-designed redundant computing architecture integrates processors, sensors, power, communication, timing, software, voting, monitoring, and reconfiguration into one fault-tolerant system. Dual and triple channels become valuable only when their independence and failure behavior are explicitly engineered. This architecture provides the computational resilience required for cargo UAV flight control to remain predictable as individual hardware, communication, or software elements degrade or fail.

중복 컴퓨팅 아키텍처(Redundant Computing Architecture)는 비행 필수 기능(Flight-Critical Function)이 단일 연산 채널의 상실이나 오류를 허용할 수 없는 화물 무인항공기(Cargo UAV)의 핵심 설계 메커니즘이다. 이중 및 삼중 아키텍처(Dual and Triple Architecture)는 프로세서, 인터페이스, 전원 경로 또는 전체 비행제어컴퓨터(Flight Control Computer, FCC) 기능을 중복 구성하여 항공기가 제어 가능한 상태를 유지하면서 고장을 탐지하고 격리하며 허용할 수 있도록 한다. 목적은 단순한 복제가 아니라 신뢰 가능한 고장이 발생한 이후에도 안전한 운용을 지속하는 것이다.

연산 채널(Computing Channel)은 일반적으로 할당된 비행제어 소프트웨어를 실행하는 데 필요한 처리 자원, 메모리, 타이밍 서비스, 통신 인터페이스 및 입출력 기능으로 구성된다. 중복 시스템에서는 하나의 채널에서 발생한 고장이 다른 채널로 자동 전파되지 않도록 각 채널이 충분한 독립성(Independence)을 확보해야 한다. 따라서 독립성은 단순히 여러 프로세서를 설치하는 것이 아니라 전기적, 논리적, 시간적, 통신 및 소프트웨어 측면까지 포함한다.

이중 중복 아키텍처(Dual-Redundant Architecture)는 동일하거나 상호 보완적인 안전 필수 기능을 실행하는 두 개의 연산 채널을 사용한다. 두 채널 모두 공통된 항공기 상태 정보를 수신하고 독립적으로 제어 출력을 계산할 수 있다. 계산 결과는 불일치를 탐지하기 위해 지속적으로 비교된다. 두 채널이 일치하면 정상적으로 운용되지만 서로 다른 결과를 생성하면 어떤 채널이 최종 권한을 유지해야 하는지 결정하기 위해 추가적인 감시 또는 외부 정보가 필요하다.

단순한 이중 중복 구조의 주요 한계는 불일치 발생 이후의 모호성(Ambiguity)이다. 채널 A(Channel A)와 채널 B(Channel B)가 서로 다른 결과를 생성하면 비교 기능만으로는 어느 채널이 정상인지 판단할 수 없다. 따라서 프로세서 자체시험, 감시 타이머(Watchdog), 센서 일관성 검사, 타이밍 감시, 통신 진단 또는 고장 채널을 충분한 신뢰도로 식별할 수 있는 독립 감시기(Independent Monitor)와 같은 추가적인 고장탐지 메커니즘이 필요하다.

이중 아키텍처는 활성-활성(Active-Active) 또는 활성-대기(Active-Standby) 구성으로 운용할 수 있다. 활성-활성 설계에서는 두 채널이 동시에 제어 기능을 실행하고 출력이 지속적으로 감시된다. 활성-대기 설계에서는 하나의 채널이 항공기를 제어하고 두 번째 채널은 동기화된 상태를 유지하면서 제어권 인수를 준비한다. 대기 방식은 출력 권한을 단순화하지만 신뢰할 수 있는 고장탐지와 위험한 명령 불연속 없이 충분히 빠른 제어권 전환을 요구한다.

삼중 중복 컴퓨팅(Triple-Redundant Computing)은 세 번째 독립 채널을 추가하고 세 채널이 비교 가능한 출력을 계산할 때 다수결 투표(Majority Voting)를 가능하게 한다. 두 채널이 정의된 허용범위 내에서 일치하고 하나의 채널이 크게 벗어난다면 불일치 채널을 고장 가능성이 높은 구성요소로 식별할 수 있다. 따라서 삼중 모듈 중복(Triple Modular Redundancy, TMR)은 단순한 이중 구성보다 강력한 고장 식별 능력을 제공하며 높은 중요도를 갖는 제어 기능에 적합하다.

투표 로직(Voting Logic)은 삼중 채널 운용의 핵심이다. 투표기는 이산 상태, 수치형 제어값, 모드 선택, 상태 지표 또는 기타 안전 관련 출력을 비교할 수 있다. 독립적으로 실행되는 채널에서는 작은 계산 차이가 발생할 수 있으므로 수치값에는 일반적으로 허용오차 기반 비교(Tolerance-Based Comparison)가 필요하다. 투표 임계값은 정상 운용 중 불필요한 채널 배제를 발생시키지 않으면서 허용 가능한 계산 편차와 의미 있는 불일치를 구분해야 한다.

투표기(Voter) 자체도 핵심 아키텍처 구성요소가 된다. 중앙집중형 투표기의 고장은 정상적인 중복 채널 전체를 손상시킬 수 있기 때문이다. 따라서 투표 기능을 분산하거나 투표기를 중복 구성하고 신호 경로의 여러 단계에 비교 로직을 배치할 수 있다. 목적은 중복성 관리(Redundancy Management)가 새로운 단일고장점(Single Point of Failure)을 발생시키지 않도록 하는 것이다. 이에 따라 투표기의 상태와 통신 무결성도 시스템 수준 고장 분석에 포함되어야 한다.

모든 채널이 동일한 전원공급장치, 클록, 통신 스위치 또는 센서 인터페이스에 의존한다면 중복 프로세싱의 효과는 제한적이다. 화물 UAV 아키텍처에서는 여러 채널을 동시에 무력화할 수 있는 공통원인고장(Common-Cause Failure)을 분석해야 한다. 독립적인 전원 공급, 분리된 통신 경로, 중복 타이밍 기준, 격리된 입출력 회로 및 물리적 분리를 적용하면 국부적인 전기적 또는 하드웨어 고장이 전체 비행제어 기능을 정지시키는 것을 방지할 수 있다.

센서 중복성(Sensor Redundancy)은 컴퓨팅 중복성과 함께 조정되어야 한다. 여러 FCC 채널이 하나의 관성측정장치(IMU) 또는 항법 정보원만 사용한다면 해당 공통 센서의 고장에 여전히 취약하다. 따라서 핵심 상태변수는 여러 관성 센서, 위성항법시스템(GNSS), 대기자료 센서, 고도 센서 또는 추진 피드백 정보원을 통해 지원할 수 있다. 센서 선택 및 투표 로직은 잘못된 측정값이 모든 컴퓨터에 무분별하게 전달되지 않고 격리될 수 있도록 정보원의 식별정보를 보존해야 한다.

통신 아키텍처(Communication Architecture) 역시 고장 격리(Fault Containment)에 영향을 준다. 중복 채널은 독립적인 항공전자 버스(Avionics Bus) 또는 분리된 네트워크 경로를 사용하여 상태 정보와 채널 간 데이터를 교환할 수 있다. 메시지에는 정보원 식별자, 순서 정보, 타임스탬프(Timestamp), 유효성 상태 및 무결성 검사가 포함되어야 한다. 손상되거나 지연된 통신 채널을 실제 제어 계산의 불일치와 구별할 수 있어야 한다.

채널 간 감시(Cross-Channel Monitoring)는 최종 구동기 명령만을 비교하는 것이 아니다. 각 채널은 추정된 항공기 상태, 활성 비행모드, 제어 법칙 출력, 구성 데이터, 타이밍 상태 및 내부 상태 지표를 비교할 수 있다. 처리 과정의 초기 단계에서 불일치를 탐지하면 고장이 구동기에 도달하기 전에 발생 원인을 식별하는 데 도움이 된다. 그러나 채널들이 서로 지나치게 의존하면 독립성이 약화될 수 있으므로 과도한 채널 간 결합은 피해야 한다.

중복 채널에는 동기화(Synchronization)도 필요하다. 특히 빠르게 변화하는 자세 및 각속도 데이터의 경우 동일한 계산은 충분히 일관된 물리적 시점을 나타내는 센서 정보를 사용해야 한다. 시간 동기화(Time Synchronization)와 타임스탬프 관리는 정상적인 시간 차이가 계산 불일치로 오인되는 것을 방지한다. 동시에 모든 동기화된 컴퓨터를 동시에 손상시킬 수 있는 하나의 타이밍 구성요소에 모든 채널이 의존하지 않도록 해야 한다.

소프트웨어 설계에서는 공통모드 소프트웨어 고장(Common-Mode Software Fault)의 가능성을 고려해야 한다. 동일한 세 개의 프로세서에서 동일한 소프트웨어를 실행하는 방식은 많은 무작위 하드웨어 고장(Random Hardware Failure)에 효과적으로 대응할 수 있지만, 모든 채널이 공유하는 체계적인 알고리즘 또는 구현 결함에는 대응하지 못할 수 있다. 시스템 안전 분석에서 공통 소프트웨어 동작이 허용할 수 없는 위험으로 식별되는 경우 독립 감시, 다양한 구현, 이종 하드웨어 또는 별도로 개발된 안전 기능을 고려할 수 있다.

중복성 관리(Redundancy Management)는 특정 채널이 고장으로 판정된 이후의 동작을 정의해야 한다. 고장 채널은 구동기 출력에서 격리하거나 투표에서 제외하고, 재설정하거나 진단 목적으로만 유지할 수 있다. 남아 있는 채널은 이후 발생하는 고장을 정확하게 해석할 수 있도록 현재의 중복 상태를 갱신해야 한다. 예를 들어 삼중 시스템이 두 개의 정상 채널만 남은 상태로 성능 저하되면 첫 번째 고장 이전에 제공하던 다수결 투표 능력을 더 이상 유지할 수 없다.

점진적 성능 저하(Graceful Degradation)는 시스템이 즉시 제어 능력을 상실하는 대신 감소된 기능으로 운용을 계속할 수 있도록 한다. 중복성이 감소한 이후 임무 수준 기능은 자동복귀(Return-to-Home), 회항(Diversion), 축소된 비행영역 운용 또는 적절한 지점으로의 착륙을 명령할 수 있다. 대응 방법은 잔여 연산 능력, 항공기 상태, 임무 단계, 환경 조건 및 비행 지속에 따른 안전 영향을 고려하여 결정되어야 한다.

채널 전환 과정에서는 구동기 명령 권한(Actuator Command Authority)에 특별한 주의가 필요하다. 새롭게 선택된 컴퓨터의 내부 상태가 기존 활성 채널과 조금 다르다는 이유만으로 급격한 명령 변화가 발생해서는 안 된다. 상태 동기화, 명령 제한, 무충격 전환(Bumpless Transfer) 기법 및 구동기 중재(Actuator Arbitration)를 통해 제어 권한이 변경되는 동안 연속성을 유지할 수 있다. 이러한 전환 동작은 정상 조건과 고장 조건 모두에서 검증되어야 한다.

시작 동작(Startup Behavior) 역시 중복 아키텍처의 일부이다. 각 채널은 제어 권한을 획득할 수 있는 상태가 되기 전에 메모리 무결성, 실행 소프트웨어, 구성 데이터, 센서 연결성, 통신 인터페이스, 타이밍 자원 및 내부 진단 기능을 검증해야 한다. 채널 간 검사를 통해 소프트웨어와 구성 버전의 호환성을 확인할 수 있다. 초기화에 실패한 채널은 정상 채널의 무결성을 저하시키지 않도록 격리된 상태를 유지해야 한다.

고장탐지(Fault Detection)는 민감도와 가용성(Availability) 사이에서 균형을 유지해야 한다. 임계값이 지나치게 느슨하면 고장 난 컴퓨터가 활성 상태를 유지할 수 있고, 지나치게 민감하면 일시적인 타이밍 차이나 센서 잡음으로 정상 채널을 잘못 배제할 수 있다. 디바운싱(Debouncing), 지속성 검사(Persistence Check), 신뢰도 측정 및 고장 분류를 활용하면 일시적 이상과 격리 또는 재구성이 필요한 지속적 고장을 구분하는 데 도움이 된다.

중복 아키텍처는 체계적인 고장 주입(Fault Injection)과 통합시험을 통해 검증된다. 소프트웨어 인더루프(Software-in-the-Loop, SIL) 환경에서는 다양한 시뮬레이션 고장에 대해 투표, 불일치 탐지 및 성능저하 상태 로직을 평가할 수 있다. 하드웨어 인더루프(Hardware-in-the-Loop, HIL) 시험에서는 실제 FCC 하드웨어가 시뮬레이션된 항공기 동역학과 상호작용하는 동안 프로세서 재설정, 통신 상실, 손상된 센서 입력, 타이밍 고장, 전원 중단 및 구동기 이상을 주입할 수 있다.

시험은 개별 고장뿐만 아니라 고장의 조합과 발생 순서도 다루어야 한다. 삼중 채널 시스템은 하나의 채널이 고장 난 이후에는 정상적으로 동작할 수 있지만, 이중 운용 상태로 성능이 저하된 이후 통신 고장이 추가로 발생하면 다른 방식으로 대응할 수 있다. 따라서 검증에서는 완전 중복 상태, 성능저하 상태, 고장 후 운용 지속 상태(Fail-Operational), 고장안전 상태(Fail-Safe) 사이의 전환을 평가하고 각 전환 과정에서 항공기가 정의된 동작을 유지하는지 확인해야 한다.

이중 또는 삼중 컴퓨팅 구조의 선택은 궁극적으로 안전 목표, 항공기 규모, 운용 노출도, 고장 확률, 중량, 전력, 비용 및 인증 요구사항에 의해 결정된다. 충분한 독립 감시 기능이 제공된다면 이중 중복은 효율적인 고장탐지와 백업 능력을 제공할 수 있으며, 삼중 중복은 다수결 투표를 통해 더욱 강력한 고장 격리를 제공한다. 화물 UAV에서 적절한 아키텍처는 모든 시스템에 동일한 채널 수를 적용하는 것이 아니라 시스템 안전 분석(System Safety Analysis)의 결과에 따라 결정된다.

잘 설계된 중복 컴퓨팅 아키텍처는 프로세서, 센서, 전원, 통신, 타이밍, 소프트웨어, 투표, 감시 및 재구성(Reconfiguration)을 하나의 고장 허용 시스템(Fault-Tolerant System)으로 통합한다. 이중 및 삼중 채널은 독립성과 고장 발생 시 동작이 명확하게 설계될 때 비로소 의미 있는 중복성을 제공한다. 이러한 아키텍처는 개별 하드웨어, 통신 또는 소프트웨어 요소의 성능이 저하되거나 고장이 발생하더라도 화물 UAV의 비행제어가 예측 가능한 상태를 유지하는 데 필요한 연산 복원력(Computational Resilience)을 제공한다.

##  

## 02.05. Avionics Communication Bus ARINC 629 CAN [w/Code]

![](images/image5.png){width="7.268055555555556in" height="7.268055555555556in"}

The avionics communication bus provides the data backbone that connects flight-control computers, navigation systems, sensors, propulsion controllers, power-management units, cargo systems, and other electronic equipment within a cargo UAV. Its architecture must deliver predictable communication while maintaining data integrity, fault containment, and sufficient bandwidth. ARINC 629 and Controller Area Network (CAN) represent different approaches to reliable distributed avionics communication.

Communication requirements vary substantially across the UAV architecture. Flight-control measurements and actuator-related information may require deterministic latency and high update rates, while equipment status, cargo information, maintenance data, and configuration messages can tolerate slower delivery. A well-structured avionics network therefore classifies traffic according to timing criticality, safety significance, bandwidth demand, and required behavior when communication becomes unavailable.

ARINC 629 was developed as a multi-transmitter digital data bus for integrated aircraft systems. Unlike architectures that depend on a single centralized bus controller, multiple terminals can transmit information over the shared medium according to defined access rules. This distributed communication concept reduces dependence on one controlling node and permits numerous avionics units to exchange information while maintaining coordinated access to the communication channel.

A major architectural characteristic of ARINC 629 is its use of deterministic terminal access mechanisms designed to prevent uncontrolled simultaneous transmissions. Each terminal observes timing rules before gaining access to the bus, allowing multiple devices to communicate without relying on a conventional master controller. For safety-oriented aircraft architectures, this approach demonstrates how distributed bus access can be engineered around bounded and predictable communication behavior.

ARINC 629 communication can support redundant physical buses so that failure of one communication path does not necessarily isolate critical avionics equipment. Redundant interfaces can transmit or receive information through independent paths, while system-level logic determines how degraded communication is handled. The effectiveness of such redundancy depends on physical separation, independent transceivers, wiring integrity, power independence, and appropriate failure-detection mechanisms.

Controller Area Network (CAN) provides a different distributed communication model and is widely suited to embedded control networks. Multiple electronic control units can share the same bus, with messages identified primarily by their communication meaning rather than by a fixed destination address. This message-oriented structure allows several nodes to consume the same information and supports flexible integration of sensors, controllers, actuators, power systems, and auxiliary equipment.

CAN resolves simultaneous transmission attempts through message arbitration. Message identifiers participate in a bitwise arbitration process in which higher-priority traffic can continue without destructive collision, while lower-priority transmitters retry later. This behavior is particularly useful when safety-relevant or time-sensitive messages must receive preferential access. Identifier assignment must therefore be treated as part of the real-time architecture rather than merely as a software naming convention.

CAN also includes mechanisms for detecting communication errors and limiting the influence of malfunctioning nodes. Frame checking, acknowledgment behavior, transmission monitoring, and node error states help identify corrupted traffic and prevent a persistently faulty transmitter from indefinitely disrupting the network. These mechanisms provide an important foundation for fault containment, although higher-level avionics software must still evaluate message freshness, plausibility, source health, and operational context.

For cargo UAVs, CAN can connect propulsion controllers, battery-management systems, power-distribution units, landing systems, environmental sensors, cargo mechanisms, and other distributed devices. Flight-critical use requires careful analysis of bus loading, update periods, priority assignment, failure behavior, and electrical topology. A network that performs adequately under nominal traffic must also retain required timing margins when diagnostic messages, retransmissions, or abnormal events increase communication demand.

The avionics architecture does not need to use one bus technology for every subsystem. A heterogeneous communication architecture can assign different networks according to functional criticality and data characteristics. Deterministic flight-control traffic may be separated from propulsion, power, payload, maintenance, or high-bandwidth perception traffic. Such segmentation reduces interference and prevents a traffic surge in one functional domain from consuming communication resources required by another.

Communication gateways can connect these network domains while preserving controlled information flow. A gateway may translate message formats, enforce routing policies, perform integrity checks, manage rate limits, and isolate faults between buses. However, gateways must not become uncontrolled single points of failure. Their latency, buffering behavior, overload response, restart behavior, and failure modes must be included in the system architecture and verification process.

Message definitions are as important as the physical bus. Each avionics message should specify its source, meaning, units, numerical range, update frequency, validity conditions, timeout behavior, and safety significance. Position, velocity, actuator state, battery voltage, cargo status, or propulsion information becomes dangerous if different subsystems interpret its representation differently. Interface control therefore requires precise and version-managed data definitions.

Time information must accompany data when the age of a measurement affects its meaning. A flight-control computer receiving sensor or propulsion information needs to distinguish a recent measurement from a delayed packet that remains syntactically valid. Timestamps, sequence counters, freshness monitoring, and timeout logic allow receiving systems to detect stale or missing information. These mechanisms become especially important when data crosses gateways or redundant communication paths.

Redundant avionics buses require explicit rules for duplicate and inconsistent information. The same message may arrive through two independent paths at slightly different times, or one bus may continue operating while another becomes corrupted. Receiving software must determine whether to accept the first valid message, compare redundant copies, select a preferred channel, or declare a disagreement. The strategy depends on the criticality and timing characteristics of the information.

Physical network topology strongly influences reliability. Bus length, termination, connector quality, electromagnetic interference, grounding, shielding, and cable routing affect signal integrity. Cargo UAVs can expose avionics wiring to propulsion-generated electrical noise, vibration, temperature variation, and high-current power systems. Communication wiring must therefore be designed together with mechanical installation and electromagnetic compatibility rather than treated purely as a software interface.

Network capacity must be analyzed under worst-case conditions. Average utilization alone is insufficient because simultaneous sensor updates, fault reports, retransmissions, or mode transitions can create temporary traffic peaks. Engineers evaluate message periods, frame lengths, arbitration delays, gateway latency, and scheduling margins to determine worst-case response time. Safety-critical messages must remain within their deadlines even when the network experiences credible abnormal loading.

Fault management extends beyond detecting a disconnected bus. Systems should recognize missing messages, excessive latency, corrupted frames, inconsistent redundant data, bus-off nodes, gateway failures, and unexpected transmission rates. Communication faults can then be mapped to system responses such as sensor fallback, channel isolation, degraded control, mission reconfiguration, return-to-home, or landing depending on the affected function and remaining communication capability.

Network partitioning also supports safety assurance. Flight-critical control communication can be isolated from maintenance equipment, payload computers, or computationally intensive autonomy systems so that faults in less critical domains cannot freely propagate into control networks. Gateways and interface monitors enforce the permitted data paths. This separation becomes increasingly important as cargo UAVs integrate high-performance AI computers and numerous networked sensors alongside conventional avionics.

Cybersecurity introduces another communication requirement. Unauthorized messages, modified commands, replayed data, or compromised maintenance interfaces can create effects similar to physical avionics failures. Where appropriate, authentication, integrity checking, access control, secure configuration, and network monitoring can protect communication paths. Security functions must be designed so that their computational or timing overhead does not violate real-time flight requirements.

Verification begins with individual bus interfaces and expands toward complete network behavior. Bench testing can evaluate electrical signaling, message encoding, timing, error detection, and device interoperability. Software-in-the-loop environments can test communication logic and failure handling, while hardware-in-the-loop systems reproduce realistic traffic loads and aircraft behavior. Fault injection can deliberately create message loss, delay, corruption, node resets, or bus failures.

Integration testing should verify not only nominal data exchange but also transitions between normal and degraded network states. Redundant-path switching, gateway restart, bus saturation, intermittent connections, and recovery after node faults must produce controlled system behavior. Logs from communication controllers and receiving applications should provide enough timing and diagnostic information to reconstruct the sequence of events during anomalous conditions.

ARINC 629 and CAN illustrate complementary principles relevant to cargo UAV avionics design: distributed communication, controlled medium access, message integrity, redundancy, priority management, and fault containment. The appropriate implementation depends on aircraft size, subsystem requirements, certification strategy, bandwidth, and integration constraints. The communication bus must ultimately be engineered as part of the flight system rather than as a passive connection between computers.

A robust avionics communication architecture therefore combines physical-network design, deterministic traffic management, precise message contracts, timing control, redundancy, diagnostics, cybersecurity, and systematic verification. By ensuring that FCC, FMS, propulsion, power, sensor, cargo, and supporting systems exchange trustworthy information within defined timing bounds, the communication infrastructure becomes a critical foundation for safe and scalable cargo UAV operation.

항공전자 통신 버스(Avionics Communication Bus)는 화물 무인항공기(Cargo UAV) 내부의 비행제어컴퓨터(Flight Control Computer, FCC), 항법 시스템, 센서, 추진 제어기, 전력관리 장치, 화물 시스템 및 기타 전자장비를 연결하는 데이터 백본(Data Backbone)을 제공한다. 통신 아키텍처는 데이터 무결성(Data Integrity), 고장 격리(Fault Containment), 충분한 대역폭을 유지하면서 예측 가능한 통신을 제공해야 한다. ARINC 629와 제어기 영역 네트워크(Controller Area Network, CAN)는 신뢰성 높은 분산 항공전자 통신을 구현하는 서로 다른 접근방식을 대표한다.

통신 요구사항은 UAV 아키텍처의 기능에 따라 크게 달라진다. 비행제어 측정값과 구동기 관련 정보에는 결정론적 지연시간(Deterministic Latency)과 높은 업데이트 주기가 필요할 수 있지만, 장비 상태, 화물 정보, 정비 데이터 및 구성 메시지는 상대적으로 느린 전달을 허용할 수 있다. 따라서 체계적으로 설계된 항공전자 네트워크는 타이밍 중요도, 안전 중요도, 대역폭 요구량 및 통신 불가 상황에서 요구되는 동작에 따라 트래픽을 분류한다.

ARINC 629는 통합 항공기 시스템을 위한 다중 송신기 디지털 데이터 버스(Multi-Transmitter Digital Data Bus)로 개발되었다. 하나의 중앙집중식 버스 제어기에 의존하는 아키텍처와 달리 여러 터미널이 정의된 접근 규칙에 따라 공유 매체를 통해 정보를 전송할 수 있다. 이러한 분산 통신(Distributed Communication) 개념은 단일 제어 노드에 대한 의존성을 낮추고 다수의 항공전자 장치가 통신 채널에 대한 조정된 접근을 유지하면서 정보를 교환할 수 있도록 한다.

ARINC 629의 주요 아키텍처 특징은 통제되지 않은 동시 전송을 방지하도록 설계된 결정론적 터미널 접근 메커니즘(Deterministic Terminal Access Mechanism)을 사용한다는 점이다. 각 터미널은 버스 접근 권한을 획득하기 전에 정의된 타이밍 규칙을 준수하여 기존의 마스터 제어기에 의존하지 않고 여러 장치가 통신할 수 있도록 한다. 안전 중심의 항공기 아키텍처에서 이러한 방식은 제한되고 예측 가능한 통신 동작을 중심으로 분산 버스 접근을 설계하는 방법을 보여준다.

ARINC 629 통신은 하나의 통신 경로가 고장 나더라도 핵심 항공전자 장비가 반드시 격리되지 않도록 중복 물리 버스(Redundant Physical Bus)를 지원할 수 있다. 중복 인터페이스는 독립된 경로를 통해 정보를 송수신할 수 있으며 시스템 수준 로직은 성능이 저하된 통신 상태를 어떻게 처리할지 결정한다. 이러한 중복성의 효과는 물리적 분리, 독립 트랜시버(Independent Transceiver), 배선 무결성, 전원 독립성 및 적절한 고장탐지 메커니즘에 의해 결정된다.

제어기 영역 네트워크(Controller Area Network, CAN)는 이와 다른 형태의 분산 통신 모델을 제공하며 임베디드 제어 네트워크(Embedded Control Network)에 폭넓게 적용할 수 있다. 여러 전자제어장치(Electronic Control Unit)는 동일한 버스를 공유할 수 있으며 메시지는 고정된 목적지 주소보다는 주로 통신 정보의 의미에 따라 식별된다. 이러한 메시지 중심 구조(Message-Oriented Structure)를 통해 여러 노드가 동일한 정보를 사용할 수 있으며 센서, 제어기, 구동기, 전력 시스템 및 보조장비를 유연하게 통합할 수 있다.

CAN은 메시지 중재(Message Arbitration)를 통해 동시에 발생하는 전송 시도를 해결한다. 메시지 식별자는 비트 단위 중재 과정(Bitwise Arbitration Process)에 참여하며 우선순위가 높은 트래픽은 충돌로 손상되지 않고 전송을 계속하는 반면 낮은 우선순위의 송신기는 이후 다시 전송을 시도한다. 이러한 동작은 안전 관련 또는 시간 민감 메시지가 우선적으로 버스에 접근해야 할 때 특히 유용하다. 따라서 식별자 할당은 단순한 소프트웨어 명명 규칙이 아니라 실시간 아키텍처(Real-Time Architecture)의 일부로 관리해야 한다.

CAN은 통신 오류를 탐지하고 오동작 노드의 영향을 제한하기 위한 메커니즘도 포함한다. 프레임 검사(Frame Checking), 승인 응답 동작(Acknowledgment Behavior), 전송 감시 및 노드 오류 상태를 통해 손상된 트래픽을 식별하고 지속적으로 오동작하는 송신기가 네트워크를 무기한 방해하는 것을 방지할 수 있다. 이러한 메커니즘은 고장 격리의 중요한 기반을 제공하지만 상위 항공전자 소프트웨어에서는 메시지 최신성, 타당성, 정보원 상태 및 운용 상황을 추가로 평가해야 한다.

화물 UAV에서 CAN은 추진 제어기, 배터리관리시스템(Battery Management System), 전력분배장치(Power Distribution Unit), 착륙 시스템, 환경 센서, 화물 장치 및 기타 분산 장비를 연결하는 데 사용할 수 있다. 비행 필수 기능에 적용하려면 버스 부하, 업데이트 주기, 우선순위 할당, 고장 시 동작 및 전기적 토폴로지를 면밀히 분석해야 한다. 정상 트래픽에서 충분한 성능을 제공하는 네트워크도 진단 메시지, 재전송 또는 비정상 이벤트로 통신 요구량이 증가할 때 필요한 타이밍 여유를 유지해야 한다.

항공전자 아키텍처는 모든 하위 시스템에 하나의 버스 기술만을 사용할 필요가 없다. 이기종 통신 아키텍처(Heterogeneous Communication Architecture)는 기능 중요도와 데이터 특성에 따라 서로 다른 네트워크를 할당할 수 있다. 결정론적 비행제어 트래픽을 추진, 전력, 탑재화물, 정비 또는 고대역폭 인지 트래픽과 분리할 수 있다. 이러한 세분화는 간섭을 줄이고 한 기능 영역의 트래픽 급증이 다른 영역에 필요한 통신 자원을 소모하는 것을 방지한다.

통신 게이트웨이(Communication Gateway)는 통제된 정보 흐름을 유지하면서 이러한 네트워크 영역을 연결할 수 있다. 게이트웨이는 메시지 형식을 변환하고, 라우팅 정책을 적용하며, 무결성을 검사하고, 전송률을 제한하며, 버스 간 고장을 격리할 수 있다. 그러나 게이트웨이가 통제되지 않은 단일고장점(Single Point of Failure)이 되어서는 안 된다. 게이트웨이의 지연시간, 버퍼링 동작, 과부하 대응, 재시작 동작 및 고장 형태는 시스템 아키텍처와 검증 과정에 포함되어야 한다.

메시지 정의(Message Definition)는 물리적 버스만큼 중요하다. 각 항공전자 메시지에는 정보원, 의미, 단위, 수치 범위, 업데이트 주기, 유효 조건, 타임아웃 동작 및 안전 중요도가 정의되어야 한다. 위치, 속도, 구동기 상태, 배터리 전압, 화물 상태 또는 추진 정보는 서로 다른 하위 시스템이 그 표현을 다르게 해석하면 위험해질 수 있다. 따라서 인터페이스 제어(Interface Control)에는 정확하고 버전 관리되는 데이터 정의가 필요하다.

측정 데이터의 경과시간이 의미에 영향을 주는 경우 데이터와 함께 시간 정보(Time Information)를 제공해야 한다. 센서 또는 추진 정보를 수신하는 비행제어컴퓨터는 최신 측정값과 문법적으로는 유효하지만 지연된 패킷을 구별할 수 있어야 한다. 타임스탬프(Timestamp), 순서 카운터(Sequence Counter), 최신성 감시(Freshness Monitoring) 및 타임아웃 로직을 사용하면 수신 시스템이 오래되었거나 누락된 정보를 탐지할 수 있다. 이러한 메커니즘은 데이터가 게이트웨이나 중복 통신 경로를 통과할 때 특히 중요하다.

중복 항공전자 버스(Redundant Avionics Bus)에는 중복되거나 일관되지 않은 정보에 대한 명확한 처리 규칙이 필요하다. 동일한 메시지가 서로 독립적인 두 경로를 통해 약간 다른 시간에 도착하거나 하나의 버스는 정상적으로 동작하지만 다른 버스는 손상될 수 있다. 수신 소프트웨어는 최초의 유효 메시지를 사용할지, 중복 데이터를 비교할지, 우선 채널을 선택할지 또는 불일치를 선언할지를 결정해야 한다. 이러한 전략은 해당 정보의 중요도와 타이밍 특성에 따라 달라진다.

물리적 네트워크 토폴로지(Physical Network Topology)는 신뢰성에 큰 영향을 준다. 버스 길이, 종단처리(Termination), 커넥터 품질, 전자기 간섭(Electromagnetic Interference), 접지, 차폐 및 케이블 배선은 신호 무결성에 영향을 미친다. 화물 UAV의 항공전자 배선은 추진 시스템에서 발생하는 전기적 잡음, 진동, 온도 변화 및 대전류 전력 시스템에 노출될 수 있다. 따라서 통신 배선은 단순한 소프트웨어 인터페이스가 아니라 기계적 설치 및 전자기 적합성(Electromagnetic Compatibility)과 함께 설계되어야 한다.

네트워크 용량(Network Capacity)은 최악 조건을 기준으로 분석해야 한다. 평균 이용률만으로는 충분하지 않으며 센서 동시 업데이트, 고장 보고, 재전송 또는 모드 전환으로 일시적인 트래픽 피크가 발생할 수 있다. 엔지니어는 메시지 주기, 프레임 길이, 중재 지연, 게이트웨이 지연시간 및 스케줄링 여유를 평가하여 최악 응답시간(Worst-Case Response Time)을 결정한다. 안전 필수 메시지는 신뢰 가능한 비정상 네트워크 부하에서도 마감시간을 충족해야 한다.

고장관리(Fault Management)는 단순히 버스 단절을 탐지하는 것 이상으로 확장된다. 시스템은 메시지 누락, 과도한 지연, 손상된 프레임, 일관되지 않은 중복 데이터, 버스 오프 노드(Bus-Off Node), 게이트웨이 고장 및 예상하지 못한 전송률을 인식해야 한다. 이후 영향을 받은 기능과 남아 있는 통신 능력에 따라 센서 대체, 채널 격리, 성능저하 제어, 임무 재구성, 자동복귀(Return-to-Home) 또는 착륙과 같은 시스템 대응으로 통신 고장을 연결할 수 있다.

네트워크 파티셔닝(Network Partitioning)은 안전 보증(Safety Assurance)도 지원한다. 비행 필수 제어 통신을 정비 장비, 탑재화물 컴퓨터 또는 연산 집약적인 자율화 시스템과 격리하여 중요도가 낮은 영역의 고장이 제어 네트워크로 자유롭게 전파되는 것을 방지할 수 있다. 게이트웨이와 인터페이스 감시기는 허용된 데이터 경로를 강제한다. 화물 UAV가 기존 항공전자 장비와 함께 고성능 인공지능 컴퓨터와 다수의 네트워크 센서를 통합할수록 이러한 분리는 더욱 중요해진다.

사이버보안(Cybersecurity)은 또 다른 통신 요구사항을 제시한다. 승인되지 않은 메시지, 변조된 명령, 재전송된 데이터 또는 침해된 정비 인터페이스는 물리적인 항공전자 고장과 유사한 영향을 발생시킬 수 있다. 필요한 경우 인증(Authentication), 무결성 검사, 접근제어(Access Control), 안전한 구성관리 및 네트워크 감시를 통해 통신 경로를 보호할 수 있다. 보안 기능은 연산 또는 타이밍 오버헤드로 인해 실시간 비행 요구사항을 위반하지 않도록 설계되어야 한다.

검증(Verification)은 개별 버스 인터페이스에서 시작하여 전체 네트워크 동작으로 확장된다. 벤치시험(Bench Testing)은 전기적 신호, 메시지 인코딩, 타이밍, 오류탐지 및 장치 상호운용성을 평가할 수 있다. 소프트웨어 인더루프(Software-in-the-Loop, SIL) 환경에서는 통신 로직과 고장 대응을 시험하며, 하드웨어 인더루프(Hardware-in-the-Loop, HIL) 시스템에서는 실제에 가까운 트래픽 부하와 항공기 동작을 재현한다. 고장 주입(Fault Injection)을 통해 메시지 손실, 지연, 손상, 노드 재설정 또는 버스 고장을 의도적으로 발생시킬 수도 있다.

통합시험(Integration Testing)은 정상적인 데이터 교환뿐만 아니라 정상 네트워크 상태와 성능저하 네트워크 상태 사이의 전환도 검증해야 한다. 중복 경로 전환, 게이트웨이 재시작, 버스 포화(Bus Saturation), 간헐적 연결 및 노드 고장 이후 복구 과정에서 시스템은 통제된 동작을 보여야 한다. 통신 제어기와 수신 응용프로그램의 로그에는 비정상 조건에서 발생한 이벤트 순서를 재구성할 수 있는 충분한 타이밍 및 진단 정보가 포함되어야 한다.

ARINC 629와 CAN은 화물 UAV 항공전자 설계에 필요한 분산 통신, 통제된 매체 접근, 메시지 무결성, 중복성, 우선순위 관리 및 고장 격리라는 상호 보완적인 원칙을 보여준다. 적절한 구현 방식은 항공기 규모, 하위 시스템 요구사항, 인증 전략, 대역폭 및 통합 제약조건에 따라 결정된다. 궁극적으로 통신 버스는 컴퓨터를 단순히 연결하는 수동적인 수단이 아니라 비행 시스템(Flight System)의 일부로 설계되어야 한다.

견고한 항공전자 통신 아키텍처(Robust Avionics Communication Architecture)는 물리적 네트워크 설계, 결정론적 트래픽 관리, 정확한 메시지 계약(Message Contract), 타이밍 제어, 중복성, 진단, 사이버보안 및 체계적인 검증을 하나의 구조로 통합한다. FCC, FMS, 추진, 전력, 센서, 화물 및 지원 시스템이 정의된 시간 범위 내에서 신뢰할 수 있는 정보를 교환하도록 보장함으로써 통신 인프라는 안전하고 확장 가능한 화물 UAV 운용을 위한 핵심 기반이 된다.

##  

## 02.06. Power Distribution and Battery Management SW [w/Code]

![](images/image6.png){width="7.268055555555556in" height="7.268055555555556in"}

Power distribution and battery management software provides the energy-control foundation that allows a cargo UAV to operate propulsion, avionics, sensors, communications, payload equipment, and safety systems within defined electrical limits. The software coordinates power sources and loads while continuously evaluating energy availability, electrical health, and thermal conditions. For heavy cargo UAVs, these functions directly influence flight endurance, redundancy, mission feasibility, and emergency behavior.

The power architecture may contain high-voltage propulsion buses, lower-voltage avionics rails, battery packs, generators, DC-DC converters, contactors, protection devices, and redundant distribution paths. Software maintains an operational representation of these components and their connectivity. Rather than treating electrical power as a passive resource, the system manages it as a dynamic aircraft subsystem whose configuration can change during startup, flight, fault isolation, and shutdown.

A Battery Management System (BMS) monitors individual cells, modules, and complete battery packs. Typical measurements include cell voltage, pack voltage, current, temperature, insulation condition, and contactor state. These measurements are converted into higher-level health information used by the flight system. Reliable acquisition is essential because inaccurate electrical data can produce incorrect endurance estimates or delay detection of hazardous battery conditions.

State of Charge (SoC) represents the estimated amount of usable energy remaining in a battery. Direct voltage measurement alone is generally insufficient because battery voltage varies with current, temperature, chemistry, aging, and transient loading. BMS software can combine current integration, voltage characteristics, battery models, and historical behavior to maintain a more stable estimate. The uncertainty of the estimate should also be considered when determining operational energy margins.

State of Health (SoH) describes the battery\'s capability relative to its expected or initial performance. Capacity loss, increased internal resistance, cell imbalance, repeated high-current operation, temperature exposure, and aging can gradually reduce usable performance. Tracking SoH allows maintenance and mission-planning systems to distinguish a fully charged but degraded battery from a healthy battery capable of delivering its nominal energy and peak power.

Cell balancing is required because individual cells do not age or charge identically. Small differences can accumulate until one cell reaches an upper or lower voltage limit earlier than the rest of the pack. Battery-management software monitors cell-level imbalance and coordinates balancing functions when supported by the hardware. Maintaining balanced cells improves usable pack capacity and prevents individual cells from becoming the limiting element during demanding flight phases.

Thermal management is closely coupled with battery control. High discharge rates during takeoff, climb, heavy-payload operation, or aggressive maneuvering can generate substantial heat, while low temperatures can reduce available power and usable capacity. The BMS monitors temperature distribution and can coordinate cooling, heating, current limits, or operational restrictions. Thermal decisions must account for both instantaneous temperature and the rate at which conditions are changing.

Power-distribution software manages contactors, relays, solid-state switches, and power converters that connect energy sources to aircraft loads. Startup sequencing is particularly important because high-power equipment cannot necessarily be energized simultaneously. Controlled precharge and staged activation can limit inrush current and prevent bus-voltage collapse. Shutdown sequencing similarly ensures that critical computers and safety functions remain powered long enough to complete required actions.

Electrical loads can be classified according to operational criticality. Flight-control computers, essential navigation sensors, communication equipment, and critical actuators may require protected power availability, while payload processors, nonessential sensors, cabin equipment, or maintenance functions may be disconnected when energy becomes limited. Load-shedding logic therefore preserves essential flight capability by selectively removing lower-priority electrical demand.

Redundant power distribution is necessary when a single electrical failure must not disable flight-critical avionics. Independent battery strings, distribution buses, converters, wiring paths, or power controllers can supply critical equipment through separated channels. Software monitors the availability of each path and coordinates transfer when a source or distribution segment fails. Redundancy must include sufficient electrical isolation so that one short circuit cannot collapse every supposedly independent supply.

Fault detection includes overvoltage, undervoltage, overcurrent, excessive temperature, cell imbalance, insulation degradation, contactor faults, converter failures, and abnormal power consumption. Thresholds alone may not be sufficient because transient conditions can briefly exceed nominal values without representing persistent failure. Software can combine magnitude, duration, rate of change, component state, and operating mode to distinguish temporary disturbances from conditions requiring protective action.

Fault isolation determines which component or electrical segment is responsible for an abnormal condition. If a branch develops excessive current, the system may disconnect that branch while preserving the remaining power network. If one battery pack becomes unavailable, healthy packs may continue supporting essential loads within recalculated limits. Isolation logic must avoid unnecessary cascading shutdowns while ensuring that hazardous faults are separated quickly enough to protect the aircraft.

The Flight Control Computer (FCC) requires reliable power-health information because available electrical capability can affect vehicle control. Reduced bus voltage or propulsion power may require thrust limitation, altered control allocation, or changes to the permitted flight envelope. Power information delivered to the FCC must therefore include validity and fault status rather than only raw voltage and current measurements, allowing control functions to respond appropriately to degraded electrical conditions.

The Flight Management System (FMS) uses energy information at a longer mission timescale. Remaining energy, predicted consumption, battery health, reserve requirements, wind, payload mass, and route characteristics can be combined to estimate whether the destination remains reachable. If energy margins deteriorate, the FMS can initiate replanning, diversion, return-to-home, or landing decisions before the battery reaches a critical condition.

Energy prediction should account for the strong relationship between cargo mass and propulsion demand. A route that is feasible for an unloaded UAV may provide insufficient reserve when the aircraft carries a heavy payload or encounters sustained headwind. Software can continuously compare measured consumption with predicted consumption and update the remaining flight-time or range estimate. Persistent deviation becomes an early indicator that the original mission assumptions are no longer valid.

Hybrid-electric cargo UAVs introduce additional energy-management complexity. Batteries may operate together with turbine generators, internal-combustion generators, fuel cells, or other power sources. Supervisory software determines how power demand is shared, when batteries should charge or discharge, and how reserve capability is preserved. Source coordination must prevent unstable transitions while ensuring that sufficient transient power remains available for demanding flight conditions.

Communication between the BMS, power-distribution controllers, FCC, FMS, and Ground Control Station (GCS) requires precise interface definitions. Messages can contain voltage, current, temperature, SoC, SoH, available power, fault state, contactor status, and predicted endurance. Each value should include defined units, update rates, validity information, and timeout behavior so that stale or corrupted electrical information cannot silently influence flight decisions.

The GCS presents electrical and battery information to operators without requiring them to interpret every cell measurement continuously. Higher-level indications can summarize available energy, reserve margin, battery health, thermal state, active faults, and degraded power configurations. Significant transitions should generate clear alerts, while detailed diagnostic information remains available for maintenance and post-flight analysis. Operator commands affecting power must be controlled by appropriate authority and safety checks.

Data logging supports both safety analysis and predictive maintenance. Cell voltage histories, current profiles, temperatures, charge cycles, high-power events, fault transitions, and energy consumption can be retained for later analysis. Across a fleet, these records reveal degradation trends and abnormal packs before they create operational failures. Battery history can also improve future SoC, SoH, endurance, and maintenance predictions when configuration and environmental context are preserved.

Cybersecurity applies to power-management functions because unauthorized control of contactors, charging equipment, or configuration parameters could affect aircraft availability or safety. Software should protect critical commands, firmware updates, calibration data, and maintenance interfaces through appropriate authentication, integrity checks, and access control. Security mechanisms must not prevent essential protective actions from executing within their required real-time deadlines.

Verification combines software testing with electrical and aircraft-level simulation. Software-in-the-loop testing can evaluate SoC estimation, load shedding, fault logic, and mission-energy calculations. Hardware-in-the-loop testing can reproduce changing voltage, current, temperature, contactor states, and communication failures using representative controllers. Fault injection allows overcurrent, sensor errors, pack loss, converter failure, and abnormal thermal conditions to be tested without exposing an aircraft to unnecessary risk.

System-level testing must verify transitions between normal, degraded, and emergency power states. Loss of one source, sudden propulsion demand, battery overheating, bus undervoltage, or failure of a distribution branch should produce predictable reconfiguration and appropriate flight-system responses. Testing also confirms that redundant power paths remain genuinely independent and that load shedding preserves the equipment required for controlled flight and landing.

Power distribution and battery management software therefore connects electrical engineering directly with flight autonomy. By integrating energy estimation, battery protection, thermal management, load prioritization, redundant distribution, fault isolation, mission prediction, and system monitoring, it transforms raw electrical power into a managed flight resource. This capability is essential for cargo UAVs whose safety, payload performance, range, and contingency options depend strongly on available energy.

전력분배 및 배터리관리 소프트웨어(Power Distribution and Battery Management Software)는 화물 무인항공기(Cargo UAV)가 정의된 전기적 한계 내에서 추진 시스템, 항공전자 장비, 센서, 통신장비, 탑재장비 및 안전 시스템을 운용할 수 있도록 하는 에너지 제어 기반을 제공한다. 소프트웨어는 전원과 부하를 조정하면서 에너지 가용성, 전기적 상태 및 열적 조건을 지속적으로 평가한다. 대형 화물 UAV에서 이러한 기능은 비행시간, 중복성, 임무 수행 가능성 및 비상 동작에 직접적인 영향을 미친다.

전력 아키텍처(Power Architecture)는 고전압 추진 버스, 저전압 항공전자 전원 레일, 배터리 팩, 발전기, 직류-직류 변환기(DC-DC Converter), 접촉기(Contactor), 보호장치 및 중복 전력분배 경로로 구성될 수 있다. 소프트웨어는 이러한 구성요소와 연결 관계에 대한 운용 상태를 유지한다. 전력을 단순한 수동적 자원으로 취급하는 것이 아니라 시동, 비행, 고장 격리 및 종료 과정에서 구성이 변경될 수 있는 동적 항공기 하위 시스템(Dynamic Aircraft Subsystem)으로 관리한다.

배터리관리시스템(Battery Management System, BMS)은 개별 셀, 모듈 및 전체 배터리 팩을 감시한다. 일반적인 측정값에는 셀 전압, 팩 전압, 전류, 온도, 절연 상태 및 접촉기 상태가 포함된다. 이러한 측정값은 비행 시스템에서 사용하는 상위 수준의 상태 정보로 변환된다. 부정확한 전기 데이터는 잘못된 항속시간 추정을 발생시키거나 위험한 배터리 상태의 탐지를 지연시킬 수 있으므로 신뢰할 수 있는 데이터 획득이 필수적이다.

충전상태(State of Charge, SoC)는 배터리에 남아 있는 사용 가능한 에너지의 추정량을 나타낸다. 배터리 전압은 전류, 온도, 화학적 특성, 노화 및 과도 부하에 따라 변하기 때문에 단순한 전압 측정만으로는 일반적으로 충분하지 않다. BMS 소프트웨어는 전류 적산(Current Integration), 전압 특성, 배터리 모델 및 과거 동작을 결합하여 더욱 안정적인 추정값을 유지할 수 있다. 운용 에너지 여유도를 결정할 때는 추정값의 불확실성도 함께 고려해야 한다.

건강상태(State of Health, SoH)는 예상 성능 또는 초기 성능과 비교한 배터리의 현재 성능 능력을 나타낸다. 용량 감소, 내부저항 증가, 셀 불균형, 반복적인 고전류 운용, 온도 노출 및 노화는 사용 가능한 성능을 점진적으로 감소시킬 수 있다. SoH를 추적하면 정비 및 임무계획 시스템이 완전히 충전되었지만 성능이 저하된 배터리와 정격 에너지 및 최대 출력을 제공할 수 있는 정상 배터리를 구분할 수 있다.

셀 밸런싱(Cell Balancing)은 개별 셀이 동일한 방식으로 노화하거나 충전되지 않기 때문에 필요하다. 작은 차이가 누적되면 하나의 셀이 배터리 팩의 다른 셀보다 먼저 상한 또는 하한 전압에 도달할 수 있다. 배터리관리 소프트웨어는 셀 수준의 불균형을 감시하고 하드웨어가 지원하는 경우 밸런싱 기능을 조정한다. 셀 균형을 유지하면 사용 가능한 배터리 팩 용량을 향상시키고 고부하 비행 단계에서 특정 셀이 전체 팩 성능의 제한 요소가 되는 것을 방지할 수 있다.

열관리(Thermal Management)는 배터리 제어와 밀접하게 결합된다. 이륙, 상승, 중량 화물 운송 또는 급격한 기동 과정에서 높은 방전율은 상당한 열을 발생시킬 수 있으며, 낮은 온도에서는 사용 가능한 출력과 용량이 감소할 수 있다. BMS는 온도 분포를 감시하고 냉각, 가열, 전류 제한 또는 운용 제한을 조정할 수 있다. 열적 의사결정에서는 순간적인 온도뿐만 아니라 상태가 변화하는 속도도 함께 고려해야 한다.

전력분배 소프트웨어(Power-Distribution Software)는 에너지원과 항공기 부하를 연결하는 접촉기, 릴레이, 반도체 스위치(Solid-State Switch) 및 전력변환기를 관리한다. 고출력 장비를 반드시 동시에 활성화할 수 있는 것은 아니므로 시동 순서 제어가 특히 중요하다. 제어된 사전충전(Precharge)과 단계적 활성화를 통해 돌입전류(Inrush Current)를 제한하고 버스 전압 붕괴를 방지할 수 있다. 종료 순서 역시 핵심 컴퓨터와 안전 기능이 필요한 작업을 완료할 때까지 전력을 유지하도록 설계되어야 한다.

전기 부하(Electrical Load)는 운용 중요도에 따라 분류할 수 있다. 비행제어컴퓨터, 핵심 항법 센서, 통신장비 및 중요 구동기는 보호된 전력 공급이 필요할 수 있지만 탑재 컴퓨터, 비필수 센서, 객실 장비 또는 정비 기능은 에너지가 부족해질 경우 차단할 수 있다. 따라서 부하 차단 로직(Load-Shedding Logic)은 중요도가 낮은 전력 수요를 선택적으로 제거하여 핵심 비행 능력을 유지한다.

단일 전기적 고장으로 인해 비행 필수 항공전자 장비가 정지해서는 안 되는 경우 중복 전력분배(Redundant Power Distribution)가 필요하다. 독립 배터리 스트링, 분배 버스, 변환기, 배선 경로 또는 전력 제어기는 분리된 채널을 통해 핵심 장비에 전력을 공급할 수 있다. 소프트웨어는 각 경로의 가용성을 감시하고 전원 또는 분배 구간에 고장이 발생하면 전환을 조정한다. 하나의 단락이 모든 독립 전원을 동시에 붕괴시키지 않도록 충분한 전기적 격리가 포함되어야 한다.

고장탐지(Fault Detection)는 과전압, 저전압, 과전류, 과도한 온도, 셀 불균형, 절연 성능 저하, 접촉기 고장, 변환기 고장 및 비정상적인 전력 소비를 포함한다. 일시적인 조건은 지속적인 고장을 의미하지 않으면서도 정상 범위를 잠시 벗어날 수 있으므로 단순한 임계값만으로는 충분하지 않을 수 있다. 소프트웨어는 크기, 지속시간, 변화율, 구성요소 상태 및 운용모드를 결합하여 일시적 외란과 보호 동작이 필요한 상태를 구분할 수 있다.

고장 격리(Fault Isolation)는 비정상 상태를 발생시킨 구성요소 또는 전기적 구간을 식별한다. 특정 분기에서 과도한 전류가 발생하면 시스템은 나머지 전력망을 유지하면서 해당 분기만 차단할 수 있다. 하나의 배터리 팩을 사용할 수 없게 되면 정상 배터리 팩이 재계산된 한계 내에서 핵심 부하에 계속 전력을 공급할 수 있다. 격리 로직은 불필요한 연쇄적 종료를 방지하면서 위험한 고장을 항공기 보호에 충분한 속도로 분리해야 한다.

비행제어컴퓨터(Flight Control Computer, FCC)는 사용 가능한 전기적 능력이 기체 제어에 영향을 미칠 수 있기 때문에 신뢰할 수 있는 전력 상태 정보를 필요로 한다. 버스 전압 또는 추진 출력이 감소하면 추력 제한, 제어 할당 변경 또는 허용 비행영역(Flight Envelope)의 변경이 필요할 수 있다. 따라서 FCC에 전달되는 전력 정보에는 단순한 전압과 전류 측정값뿐만 아니라 유효성 및 고장 상태가 포함되어야 하며, 이를 통해 제어 기능이 성능 저하된 전기적 조건에 적절히 대응할 수 있다.

비행관리시스템(Flight Management System, FMS)은 보다 긴 임무 시간 규모에서 에너지 정보를 활용한다. 잔여 에너지, 예상 소비량, 배터리 상태, 예비 에너지 요구량, 바람, 탑재화물 질량 및 항로 특성을 결합하여 목적지 도달 가능성을 추정할 수 있다. 에너지 여유도가 감소하면 FMS는 배터리가 위험한 상태에 도달하기 전에 임무 재계획, 회항(Diversion), 자동복귀(Return-to-Home) 또는 착륙 결정을 시작할 수 있다.

에너지 예측(Energy Prediction)은 화물 질량과 추진력 요구량 사이의 강한 연관성을 고려해야 한다. 무화물 상태에서는 수행 가능한 항로도 항공기가 중량 화물을 탑재하거나 지속적인 역풍을 만나는 경우 충분한 예비 에너지를 확보하지 못할 수 있다. 소프트웨어는 실제 측정 소비량과 예상 소비량을 지속적으로 비교하고 잔여 비행시간 또는 항속거리 추정값을 갱신할 수 있다. 지속적인 편차는 초기 임무 가정이 더 이상 유효하지 않다는 조기 지표가 된다.

하이브리드 전기 화물 UAV(Hybrid-Electric Cargo UAV)는 추가적인 에너지관리 복잡성을 가진다. 배터리는 터빈 발전기, 내연기관 발전기, 연료전지 또는 기타 전원과 함께 동작할 수 있다. 감독 소프트웨어(Supervisory Software)는 전력 수요를 어떻게 분담할지, 배터리를 언제 충전하거나 방전할지, 예비 전력 능력을 어떻게 유지할지를 결정한다. 에너지원 조정은 불안정한 전환을 방지하는 동시에 높은 출력이 필요한 비행 조건에서 충분한 과도 전력을 확보해야 한다.

BMS, 전력분배 제어기, FCC, FMS 및 지상통제소(Ground Control Station, GCS) 사이의 통신에는 정확한 인터페이스 정의가 필요하다. 메시지에는 전압, 전류, 온도, 충전상태(SoC), 건강상태(SoH), 가용 전력, 고장 상태, 접촉기 상태 및 예상 항속시간이 포함될 수 있다. 오래되거나 손상된 전기 정보가 비행 의사결정에 영향을 미치지 않도록 각 값에는 정의된 단위, 업데이트 주기, 유효성 정보 및 타임아웃 동작이 포함되어야 한다.

GCS는 운용자가 모든 셀 측정값을 지속적으로 해석하지 않아도 되도록 전기 및 배터리 정보를 제공한다. 상위 수준의 표시정보는 사용 가능한 에너지, 예비 에너지 여유도, 배터리 상태, 열적 상태, 활성 고장 및 성능 저하된 전력 구성을 요약할 수 있다. 중요한 상태 전환은 명확한 경고를 발생시키고 상세 진단 정보는 정비 및 비행 후 분석(Post-Flight Analysis)에 활용할 수 있어야 한다. 전력에 영향을 주는 운용자 명령에는 적절한 권한관리와 안전 검사가 적용되어야 한다.

데이터 로깅(Data Logging)은 안전 분석과 예측정비(Predictive Maintenance)를 모두 지원한다. 셀 전압 이력, 전류 프로파일, 온도, 충방전 주기, 고출력 이벤트, 고장 상태 전환 및 에너지 소비량을 저장하여 이후 분석에 사용할 수 있다. 함대(Fleet) 전체에서 이러한 기록을 분석하면 운용 고장을 발생시키기 전에 성능 저하 추세와 비정상 배터리 팩을 식별할 수 있다. 구성 및 환경 정보를 함께 보존하면 배터리 이력을 활용하여 향후 SoC, SoH, 항속시간 및 정비 예측의 정확성을 높일 수도 있다.

접촉기, 충전장비 또는 구성 파라미터에 대한 승인되지 않은 제어는 항공기의 가용성이나 안전에 영향을 미칠 수 있으므로 사이버보안(Cybersecurity)은 전력관리 기능에도 적용된다. 소프트웨어는 적절한 인증(Authentication), 무결성 검사 및 접근제어(Access Control)를 통해 핵심 명령, 펌웨어 업데이트, 보정 데이터 및 정비 인터페이스를 보호해야 한다. 동시에 보안 메커니즘이 필수적인 보호 동작을 요구된 실시간 마감시간 내에 실행하지 못하게 해서는 안 된다.

검증(Verification)은 소프트웨어 시험과 전기 및 항공기 수준 시뮬레이션을 결합하여 수행한다. 소프트웨어 인더루프(Software-in-the-Loop, SIL) 시험에서는 SoC 추정, 부하 차단, 고장 로직 및 임무 에너지 계산을 평가할 수 있다. 하드웨어 인더루프(Hardware-in-the-Loop, HIL) 시험에서는 대표적인 제어기를 사용하여 변화하는 전압, 전류, 온도, 접촉기 상태 및 통신 고장을 재현할 수 있다. 고장 주입(Fault Injection)을 통해 과전류, 센서 오류, 배터리 팩 상실, 변환기 고장 및 비정상적인 열 조건을 실제 항공기에 불필요한 위험을 가하지 않고 시험할 수 있다.

시스템 수준 시험(System-Level Testing)은 정상, 성능저하 및 비상 전력 상태 사이의 전환을 검증해야 한다. 하나의 전원 상실, 갑작스러운 추진 전력 요구, 배터리 과열, 버스 저전압 또는 분배 분기 고장은 예측 가능한 재구성과 적절한 비행 시스템 대응으로 이어져야 한다. 또한 중복 전력 경로가 실제로 독립적인지와 부하 차단이 제어된 비행 및 착륙에 필요한 장비의 전력을 유지하는지도 시험을 통해 확인해야 한다.

따라서 전력분배 및 배터리관리 소프트웨어는 전기공학(Electrical Engineering)을 비행 자율화(Flight Autonomy)와 직접 연결한다. 에너지 추정, 배터리 보호, 열관리, 부하 우선순위, 중복 전력분배, 고장 격리, 임무 예측 및 시스템 감시를 통합함으로써 원시 전기에너지를 관리 가능한 비행 자원(Managed Flight Resource)으로 전환한다. 이러한 능력은 안전성, 탑재화물 운송 성능, 항속거리 및 비상대응 선택지가 사용 가능한 에너지에 크게 의존하는 화물 UAV에서 필수적이다.

##  

## 02.07. Sensor Suite Integration LiDAR Camera Radar IMU [w/Code]

![](images/image7.png){width="7.268055555555556in" height="7.268055555555556in"}

The sensor suite of a cargo UAV provides the physical observations required for flight control, navigation, obstacle detection, landing, and autonomous mission execution. LiDAR, cameras, radar, and inertial measurement units (IMUs) offer complementary information about vehicle motion and the surrounding environment. Their integration requires more than connecting individual devices; the system must establish common timing, coordinate frames, calibration, health monitoring, and reliable data interfaces.

The IMU forms the high-rate motion-sensing foundation of the aircraft. Accelerometers measure specific force while gyroscopes measure angular velocity, allowing the flight-control system to estimate rapid changes in attitude and motion. Because inertial measurements are available at high frequency and do not depend on external infrastructure, they remain essential during aggressive maneuvering or temporary loss of other navigation sources, although accumulated bias causes inertial estimates to drift over time.

LiDAR provides direct geometric measurements of the environment by estimating distance from emitted and returned light. Three-dimensional LiDAR can generate point clouds representing terrain, buildings, obstacles, landing areas, and other structures around the aircraft. For cargo UAVs, LiDAR supports terrain-relative navigation, obstacle detection, altitude estimation, mapping, and landing-zone assessment, particularly where precise geometric understanding is more important than visual appearance.

Camera systems provide dense visual information containing texture, color, edges, semantic features, and object appearance. Monocular, stereo, or multiple distributed cameras can support visual odometry, object detection, landing-marker recognition, terrain classification, and situational awareness. Cameras provide rich information at relatively low sensor weight, but their performance can degrade because of darkness, glare, fog, precipitation, motion blur, or insufficient visual texture.

Radar complements optical sensors by measuring range and, depending on the sensor architecture, relative velocity and direction under environmental conditions where cameras or LiDAR may become degraded. Radar can support obstacle detection, terrain awareness, altitude measurement, weather-related sensing, and detection of moving objects. Its robustness to lighting variation makes it valuable as part of a heterogeneous sensing architecture rather than as a replacement for all other perception sensors.

Sensor integration begins with a clearly defined coordinate-frame architecture. Measurements may originate in individual sensor frames mounted at different locations and orientations on the airframe. Extrinsic calibration defines the transformation between these sensor frames and the aircraft body frame. Accurate transformations are necessary because even small mounting errors can create significant position or orientation errors when measurements are projected over long distances.

Intrinsic calibration characterizes properties internal to each sensing device. Camera calibration includes parameters such as focal length, principal point, and lens distortion, while LiDAR and radar devices may require correction of range, angle, channel, or alignment characteristics. IMU calibration addresses accelerometer and gyroscope bias, scale factors, and axis misalignment. Calibration parameters must be configuration-controlled because they directly influence navigation and perception accuracy.

Time synchronization is equally important because the aircraft and its environment can change rapidly between measurements. A camera frame, LiDAR scan, radar detection, and IMU sample captured at different physical times cannot be treated as simultaneous without compensation. Hardware timestamps, synchronized clocks, trigger signals, or carefully managed software timing allow the integration pipeline to associate each measurement with the physical instant at which it was acquired.

High-rate IMU measurements can help compensate for motion occurring during slower sensor acquisition. A rotating or translating UAV may move significantly while a LiDAR scan or camera exposure sequence is being produced. Motion compensation uses estimated vehicle motion to transform observations into a consistent temporal reference. Without this correction, point clouds may become distorted and visual or geometric features can be incorrectly aligned.

The sensor-processing architecture normally separates device acquisition from perception and fusion functions. Device drivers communicate with physical sensors, decode measurements, apply basic validation, and attach timestamps and metadata. Preprocessing modules then perform operations such as image correction, point-cloud filtering, radar processing, or IMU calibration. Higher-level modules consume these standardized outputs without depending directly on vendor-specific device protocols.

Sensor data rates can differ by orders of magnitude. IMUs may generate compact measurements at very high frequency, cameras produce large image streams, and LiDAR generates substantial three-dimensional point-cloud traffic. Radar has its own detection and signal-processing characteristics. The computing and network architecture must therefore provide sufficient bandwidth, buffering, memory, and scheduling while ensuring that high-volume perception traffic cannot delay flight-critical inertial or control information.

Fusion combines complementary sensor information into estimates that are more useful than individual measurements alone. IMU data provides rapid motion updates, while cameras and LiDAR can constrain accumulated drift through environmental observations. Radar can maintain useful detection capability when optical sensing becomes unreliable. Fusion may occur at raw-data, feature, object, state, or decision level depending on computational cost, timing requirements, and the function being supported.

State-estimation functions require explicit uncertainty handling. Every measurement contains noise and may occasionally contain systematic error or invalid observations. Fusion algorithms should therefore consider measurement covariance, confidence, sensor status, and innovation consistency rather than assuming equal reliability. Measurements inconsistent with the predicted vehicle state can be rejected, down-weighted, or flagged for additional monitoring before they influence navigation or control.

Sensor-health monitoring operates continuously throughout the mission. The system can detect missing frames, frozen values, excessive noise, temperature anomalies, communication errors, timing irregularities, calibration inconsistencies, or implausible measurements. A sensor that continues transmitting syntactically correct but physically incorrect data can be particularly hazardous, so health evaluation must include behavioral plausibility rather than relying only on communication status.

Cross-sensor consistency provides an additional mechanism for detecting degraded sensing. Motion estimated from visual or LiDAR observations can be compared with inertial motion, while radar or LiDAR range can be checked against other altitude or obstacle estimates. Persistent disagreement does not automatically identify the failed sensor, but it provides evidence that the fusion system should reduce confidence, isolate a source, or transition to a degraded sensing configuration.

Graceful degradation is essential because autonomous cargo UAVs should not depend on every sensor remaining available throughout the mission. Loss of a camera may allow navigation to continue using LiDAR, radar, IMU, and GNSS, while temporary LiDAR degradation may be tolerated through visual and inertial sensing. The available combination determines which autonomy functions remain trustworthy and whether the aircraft should continue, reduce its operating envelope, divert, or land.

Environmental conditions strongly influence sensor selection and fusion policy. Bright sunlight, darkness, rain, fog, dust, snow, vibration, electromagnetic interference, and temperature can affect sensors differently. A heterogeneous suite provides resilience because common environmental conditions are less likely to degrade every sensing modality in exactly the same manner. The system can adjust confidence dynamically as environmental and sensor-health conditions change.

Sensor placement is part of the integration problem. Cameras and LiDAR require appropriate fields of view, radar requires suitable coverage and electromagnetic installation, and IMUs should be positioned where structural vibration and thermal effects remain manageable. Airframe structures, landing gear, cargo pods, rotors, propellers, or other components can create occlusion and interference. Installation geometry must therefore be evaluated together with perception requirements.

The Flight Control Computer (FCC) typically consumes compact, validated state information rather than unrestricted raw perception streams. High-performance perception computers can process camera, LiDAR, and radar data and provide navigation states, obstacle information, or landing guidance through controlled interfaces. This separation protects deterministic control execution from variable perception workloads while still allowing advanced sensing to influence autonomous flight through validated outputs.

The Flight Management System (FMS) can use sensor-derived information at the mission level. Detected weather hazards, blocked routes, unsuitable landing zones, navigation degradation, or obstacle conditions may trigger route modification or contingency decisions. The FMS does not need to process raw sensor data directly; instead, perception services provide higher-level information with confidence, validity, location, timestamp, and operational significance.

Data recording is important for validation and post-flight analysis. Selected camera frames, LiDAR data, radar detections, IMU measurements, calibration versions, timestamps, navigation estimates, and sensor-health events can be logged according to available storage and safety requirements. These records allow engineers to reconstruct perception failures, evaluate fusion performance, reproduce difficult scenarios, and improve future algorithms using representative operational data.

Verification progresses from individual sensor characterization to integrated multi-sensor testing. Bench tests evaluate interfaces, calibration, timing, and failure detection, while software-in-the-loop environments exercise perception and fusion algorithms with simulated sensor streams. Hardware-in-the-loop and vehicle-level testing introduce realistic timing, processing loads, vibration, communication behavior, and fault conditions before full autonomous flight testing.

Fault injection can deliberately introduce delayed measurements, dropped frames, biased IMU signals, corrupted point clouds, camera loss, radar outages, or timestamp errors. The objective is to verify that sensor fusion does not simply continue producing apparently valid results after its assumptions have been violated. Detection, isolation, fallback, confidence reduction, and recovery behavior must all be evaluated as part of the integrated sensing architecture.

A well-integrated LiDAR, camera, radar, and IMU suite therefore creates a resilient perception and navigation foundation for cargo UAV autonomy. Each sensor contributes different spatial, visual, dynamic, or inertial information, while synchronization, calibration, fusion, health monitoring, and graceful degradation convert those measurements into trustworthy system knowledge. This integrated sensing layer supports the subsequent navigation, obstacle avoidance, landing, mission management, and safety functions of the cargo UAV.

화물 무인항공기(Cargo UAV)의 센서 제품군(Sensor Suite)은 비행제어, 항법, 장애물 탐지, 착륙 및 자율 임무 수행에 필요한 물리적 관측 정보를 제공한다. 라이다(LiDAR), 카메라(Camera), 레이더(Radar), 관성측정장치(Inertial Measurement Unit, IMU)는 기체 움직임과 주변 환경에 대해 상호 보완적인 정보를 제공한다. 이러한 센서의 통합은 단순히 개별 장치를 연결하는 것이 아니라 공통 시간체계, 좌표계, 보정, 상태감시 및 신뢰할 수 있는 데이터 인터페이스를 구축하는 과정이다.

관성측정장치(IMU)는 항공기의 고속 운동 센싱 기반을 형성한다. 가속도계(Accelerometer)는 비력(Specific Force)을 측정하고 자이로스코프(Gyroscope)는 각속도를 측정하여 비행제어 시스템이 자세와 운동의 빠른 변화를 추정할 수 있도록 한다. 관성 측정값은 높은 주파수로 제공되고 외부 인프라에 의존하지 않으므로 급격한 기동이나 다른 항법 정보원이 일시적으로 상실된 상황에서도 중요하지만, 누적되는 바이어스(Bias)로 인해 관성 추정값은 시간이 지나면서 드리프트(Drift)한다.

라이다(LiDAR)는 방출된 빛과 반사되어 돌아온 빛을 이용하여 거리를 추정함으로써 환경에 대한 직접적인 기하학적 측정값을 제공한다. 3차원 라이다(3D LiDAR)는 항공기 주변의 지형, 건물, 장애물, 착륙구역 및 기타 구조물을 표현하는 포인트 클라우드(Point Cloud)를 생성할 수 있다. 화물 UAV에서 라이다는 시각적 외형보다 정밀한 기하학적 이해가 중요한 환경에서 지형 상대 항법, 장애물 탐지, 고도 추정, 매핑 및 착륙구역 평가를 지원한다.

카메라 시스템(Camera System)은 질감, 색상, 경계선, 의미론적 특징 및 객체의 외형을 포함하는 밀도 높은 시각 정보를 제공한다. 단안 카메라(Monocular Camera), 스테레오 카메라(Stereo Camera) 또는 여러 대의 분산 카메라는 시각 주행거리계(Visual Odometry), 객체 탐지, 착륙 마커 인식, 지형 분류 및 상황인식을 지원할 수 있다. 카메라는 비교적 낮은 센서 중량으로 풍부한 정보를 제공하지만 어둠, 눈부심, 안개, 강수, 모션 블러(Motion Blur) 또는 부족한 시각적 질감으로 인해 성능이 저하될 수 있다.

레이더(Radar)는 카메라나 라이다의 성능이 저하될 수 있는 환경 조건에서도 거리와 센서 아키텍처에 따라 상대속도 및 방향을 측정함으로써 광학 센서를 보완한다. 레이더는 장애물 탐지, 지형 인식, 고도 측정, 기상 관련 센싱 및 이동 객체 탐지를 지원할 수 있다. 조명 변화에 대한 높은 견고성은 레이더를 다른 모든 인지 센서를 대체하는 수단이 아니라 이기종 센싱 아키텍처(Heterogeneous Sensing Architecture)의 중요한 구성요소로 만든다.

센서 통합(Sensor Integration)은 명확하게 정의된 좌표계 아키텍처(Coordinate-Frame Architecture)에서 시작한다. 측정값은 기체의 서로 다른 위치와 방향에 장착된 개별 센서 좌표계에서 생성될 수 있다. 외부 보정(Extrinsic Calibration)은 이러한 센서 좌표계와 항공기 동체 좌표계(Body Frame) 사이의 변환 관계를 정의한다. 작은 장착 오차도 측정값을 먼 거리까지 투영할 경우 상당한 위치 또는 방향 오차를 발생시킬 수 있으므로 정확한 좌표 변환이 필요하다.

내부 보정(Intrinsic Calibration)은 각 센싱 장치 내부의 특성을 정의한다. 카메라 보정에는 초점거리, 주점(Principal Point), 렌즈 왜곡과 같은 파라미터가 포함되며, 라이다와 레이더는 거리, 각도, 채널 또는 정렬 특성에 대한 보정이 필요할 수 있다. IMU 보정은 가속도계 및 자이로스코프의 바이어스, 스케일 계수(Scale Factor), 축 정렬 오차를 다룬다. 보정 파라미터는 항법 및 인지 정확도에 직접 영향을 주기 때문에 구성관리(Configuration Control)가 이루어져야 한다.

항공기와 주변 환경은 측정 사이에도 빠르게 변화할 수 있으므로 시간 동기화(Time Synchronization) 역시 중요하다. 서로 다른 물리적 시점에 획득된 카메라 프레임, 라이다 스캔, 레이더 탐지값 및 IMU 샘플을 보상 없이 동시에 측정된 정보로 취급할 수 없다. 하드웨어 타임스탬프(Hardware Timestamp), 동기화된 클록, 트리거 신호(Trigger Signal) 또는 체계적으로 관리되는 소프트웨어 타이밍을 통해 통합 파이프라인은 각 측정값을 실제 획득된 물리적 시점과 연결할 수 있다.

고주파 IMU 측정값은 상대적으로 느린 센서 데이터 획득 과정에서 발생하는 움직임을 보상하는 데 사용할 수 있다. 회전하거나 이동하는 UAV는 하나의 라이다 스캔 또는 카메라 노출 시퀀스가 생성되는 동안에도 상당한 거리를 움직일 수 있다. 운동 보상(Motion Compensation)은 추정된 기체 움직임을 사용하여 관측값을 일관된 시간 기준으로 변환한다. 이러한 보정이 없으면 포인트 클라우드가 왜곡되고 시각적 또는 기하학적 특징이 잘못 정렬될 수 있다.

센서 처리 아키텍처(Sensor-Processing Architecture)는 일반적으로 장치 데이터 획득 기능과 인지 및 융합 기능을 분리한다. 장치 드라이버(Device Driver)는 물리적 센서와 통신하고 측정값을 디코딩하며 기본적인 유효성 검사를 수행하고 타임스탬프와 메타데이터를 추가한다. 전처리 모듈(Preprocessing Module)은 영상 보정, 포인트 클라우드 필터링, 레이더 처리 또는 IMU 보정 등을 수행한다. 상위 모듈은 특정 제조사의 장치 프로토콜에 직접 의존하지 않고 이러한 표준화된 출력을 사용한다.

센서 데이터 전송률(Data Rate)은 센서 종류에 따라 몇 자릿수 이상의 차이가 발생할 수 있다. IMU는 매우 높은 주파수로 비교적 작은 측정 데이터를 생성하지만 카메라는 대용량 영상 스트림을 생성하고 라이다는 상당한 규모의 3차원 포인트 클라우드 데이터를 생성한다. 레이더 역시 고유한 탐지 및 신호처리 특성을 갖는다. 따라서 연산 및 네트워크 아키텍처는 충분한 대역폭, 버퍼링, 메모리 및 스케줄링을 제공하면서 대용량 인지 트래픽이 비행 필수 관성 또는 제어 정보의 처리를 지연시키지 않도록 해야 한다.

센서 융합(Sensor Fusion)은 상호 보완적인 센서 정보를 결합하여 개별 측정값만으로 얻을 수 있는 것보다 유용한 추정 결과를 생성한다. IMU 데이터는 빠른 운동 업데이트를 제공하고 카메라와 라이다는 환경 관측을 통해 누적 드리프트를 제한할 수 있다. 레이더는 광학 센싱의 신뢰성이 감소하는 상황에서도 유용한 탐지 능력을 유지할 수 있다. 융합은 연산 비용, 타이밍 요구사항 및 지원 기능에 따라 원시 데이터, 특징, 객체, 상태 또는 의사결정 수준에서 수행될 수 있다.

상태추정 기능(State-Estimation Function)은 불확실성을 명시적으로 처리해야 한다. 모든 측정값에는 잡음이 존재하며 때로는 체계적인 오류나 유효하지 않은 관측값이 포함될 수 있다. 따라서 융합 알고리즘은 모든 측정값을 동일하게 신뢰하는 대신 측정 공분산(Measurement Covariance), 신뢰도, 센서 상태 및 혁신값 일관성(Innovation Consistency)을 고려해야 한다. 예측된 기체 상태와 일치하지 않는 측정값은 항법이나 제어에 영향을 주기 전에 제거하거나 가중치를 낮추거나 추가 감시 대상으로 지정할 수 있다.

센서 상태감시(Sensor-Health Monitoring)는 전체 임무 동안 지속적으로 수행된다. 시스템은 누락된 프레임, 고정된 값, 과도한 잡음, 온도 이상, 통신 오류, 타이밍 이상, 보정 불일치 또는 물리적으로 타당하지 않은 측정값을 탐지할 수 있다. 문법적으로 정상적인 데이터를 계속 전송하면서 실제 물리 상태와 다른 값을 제공하는 센서는 특히 위험할 수 있으므로 상태평가는 단순한 통신 상태뿐만 아니라 동작의 물리적 타당성까지 포함해야 한다.

센서 간 일관성(Cross-Sensor Consistency)은 성능이 저하된 센서를 탐지하는 추가적인 메커니즘을 제공한다. 시각 또는 라이다 관측으로 추정된 움직임을 관성 운동 정보와 비교할 수 있으며 레이더 또는 라이다의 거리값을 다른 고도 또는 장애물 추정값과 비교할 수 있다. 지속적인 불일치가 자동으로 고장 센서를 특정하는 것은 아니지만 융합 시스템이 신뢰도를 낮추거나 정보원을 격리하거나 성능저하 센싱 구성으로 전환해야 한다는 근거를 제공한다.

자율 화물 UAV는 임무 전체에서 모든 센서가 항상 사용 가능한 상태에 의존해서는 안 되므로 점진적 성능 저하(Graceful Degradation)가 필수적이다. 카메라가 상실되더라도 라이다, 레이더, IMU 및 위성항법시스템(GNSS)을 이용하여 항법을 지속할 수 있으며, 일시적인 라이다 성능 저하는 시각 및 관성 센싱으로 보완할 수 있다. 사용 가능한 센서 조합에 따라 어떤 자율 기능을 신뢰할 수 있는지와 항공기가 임무를 계속하거나 운용영역을 축소하거나 회항하거나 착륙해야 하는지가 결정된다.

환경 조건(Environmental Condition)은 센서 선택 및 융합 정책에 큰 영향을 미친다. 강한 햇빛, 어둠, 비, 안개, 먼지, 눈, 진동, 전자기 간섭 및 온도는 각 센서에 서로 다른 영향을 줄 수 있다. 이기종 센서 제품군은 일반적인 환경 조건이 모든 센싱 모달리티(Sensing Modality)를 동일한 방식으로 저하시킬 가능성을 줄여 복원력을 제공한다. 시스템은 환경 및 센서 상태가 변화함에 따라 각 정보원에 대한 신뢰도를 동적으로 조정할 수 있다.

센서 배치(Sensor Placement) 역시 통합 문제의 일부이다. 카메라와 라이다에는 적절한 시야각(Field of View)이 필요하고, 레이더에는 적합한 탐지 범위와 전자기적 설치 조건이 요구되며, IMU는 구조 진동과 열적 영향이 관리 가능한 위치에 배치해야 한다. 기체 구조물, 착륙장치, 화물 포드, 로터, 프로펠러 또는 기타 구성요소는 가림(Occlusion)과 간섭을 발생시킬 수 있다. 따라서 센서 설치 형상은 인지 요구사항과 함께 평가되어야 한다.

비행제어컴퓨터(Flight Control Computer, FCC)는 일반적으로 제한 없이 제공되는 원시 인지 데이터 스트림보다 압축되고 검증된 상태 정보를 사용한다. 고성능 인지 컴퓨터(Perception Computer)는 카메라, 라이다 및 레이더 데이터를 처리하고 통제된 인터페이스를 통해 항법 상태, 장애물 정보 또는 착륙 유도 정보를 제공할 수 있다. 이러한 분리는 가변적인 인지 연산 부하로부터 결정론적 제어 실행을 보호하면서도 검증된 출력을 통해 고급 센싱 기능이 자율비행에 영향을 줄 수 있도록 한다.

비행관리시스템(Flight Management System, FMS)은 센서로부터 생성된 정보를 임무 수준에서 활용할 수 있다. 탐지된 기상 위험, 차단된 항로, 부적합한 착륙구역, 항법 성능 저하 또는 장애물 상태는 항로 수정이나 비상대응 결정을 유발할 수 있다. FMS가 원시 센서 데이터를 직접 처리할 필요는 없으며 인지 서비스가 신뢰도, 유효성, 위치, 타임스탬프 및 운용 중요도를 포함하는 상위 수준 정보를 제공한다.

데이터 기록(Data Recording)은 검증과 비행 후 분석(Post-Flight Analysis)에 중요하다. 선택된 카메라 프레임, 라이다 데이터, 레이더 탐지값, IMU 측정값, 보정 버전, 타임스탬프, 항법 추정값 및 센서 상태 이벤트를 사용 가능한 저장공간과 안전 요구사항에 따라 기록할 수 있다. 이러한 기록을 통해 엔지니어는 인지 고장을 재구성하고, 융합 성능을 평가하며, 어려운 시나리오를 재현하고, 실제 운용을 대표하는 데이터를 활용하여 향후 알고리즘을 개선할 수 있다.

검증(Verification)은 개별 센서 특성평가에서 통합 다중센서 시험으로 확장된다. 벤치시험(Bench Test)은 인터페이스, 보정, 타이밍 및 고장탐지를 평가하며, 소프트웨어 인더루프(Software-in-the-Loop, SIL) 환경은 시뮬레이션된 센서 스트림을 이용하여 인지 및 융합 알고리즘을 시험한다. 하드웨어 인더루프(Hardware-in-the-Loop, HIL) 및 기체 수준 시험에서는 완전한 자율비행시험에 앞서 실제와 유사한 타이밍, 연산 부하, 진동, 통신 동작 및 고장 조건을 적용한다.

고장 주입(Fault Injection)을 통해 지연된 측정값, 프레임 손실, 바이어스가 포함된 IMU 신호, 손상된 포인트 클라우드, 카메라 상실, 레이더 중단 또는 타임스탬프 오류를 의도적으로 발생시킬 수 있다. 목적은 센서 융합의 기본 가정이 위반된 이후에도 시스템이 단순히 정상적으로 보이는 결과를 계속 생성하지 않는지 검증하는 것이다. 탐지, 격리, 대체 동작(Fallback), 신뢰도 감소 및 복구 동작을 통합 센싱 아키텍처의 일부로 모두 평가해야 한다.

잘 통합된 라이다, 카메라, 레이더 및 IMU 센서 제품군은 화물 UAV 자율화를 위한 복원력 있는 인지 및 항법 기반을 형성한다. 각 센서는 서로 다른 공간적, 시각적, 동적 또는 관성 정보를 제공하며, 시간 동기화, 보정, 융합, 상태감시 및 점진적 성능 저하 기능은 이러한 측정값을 신뢰할 수 있는 시스템 지식(System Knowledge)으로 변환한다. 이러한 통합 센싱 계층은 이후의 항법, 장애물 회피, 착륙, 임무관리 및 안전 기능을 지원하는 핵심 기반이 된다.

##  

## 02.08. Cargo Management System CMS SW Architecture [w/Code]

![](images/image8.png){width="7.268055555555556in" height="7.268055555555556in"}

The Cargo Management System (CMS) provides the software architecture responsible for supervising payload configuration, loading status, restraint mechanisms, cargo environmental conditions, and delivery operations within a cargo UAV. It connects cargo-specific equipment with the broader aircraft architecture while maintaining separation from flight-critical control. The CMS transforms payload information into validated operational states that can be consumed by the FMS, FCC, and Ground Control Station.

Cargo configuration begins with a structured representation of the payload carried by the aircraft. The CMS can maintain cargo identifiers, mass, dimensions, mounting position, handling requirements, environmental limits, destination information, and operational priority. These parameters allow the aircraft to determine whether the installed payload is compatible with vehicle limits and whether the declared configuration matches the physical loading condition before flight.

Mass and center-of-gravity information is particularly important because cargo directly influences aircraft dynamics and propulsion requirements. The CMS can receive measured or entered payload mass and determine its contribution to the overall weight and balance configuration. Cargo position information can be combined with aircraft geometry to estimate loading distribution, allowing other systems to verify that the center of gravity remains within an approved operating envelope.

The CMS supervises cargo restraint and locking mechanisms used to secure payloads during flight. Sensors can indicate whether latches, locks, clamps, doors, or attachment devices have reached their commanded positions. Software compares commanded and observed states before declaring cargo secure. A closed door indication alone may be insufficient if the locking mechanism is not engaged, so multiple conditions can be combined into a validated cargo-ready state.

Loading and unloading operations are managed through explicit operational states. The software can distinguish between empty, loading, loaded-unverified, secured, flight-ready, unloading, and completed conditions. State transitions occur only when required sensor inputs, operator confirmations, and interlocks are satisfied. This state-machine approach prevents flight preparation from progressing while cargo equipment remains in an uncertain or mechanically unsafe configuration.

Interlocks protect the aircraft from inappropriate cargo actions during flight. Cargo doors, release devices, hoists, ramps, or automated handling equipment should not activate simply because a command is received. The CMS evaluates aircraft state, mission phase, altitude, ground status, actuator health, and authorization before enabling an operation. Safety-critical inhibitions should remain enforceable even when higher-level mission software requests an invalid action.

Some cargo missions require controlled release, lowering, or aerial delivery rather than conventional landing and unloading. In these configurations, the CMS coordinates the sequence associated with preparing and releasing the payload while the flight system retains authority over aircraft stability. Release logic can verify location, altitude, speed, vehicle attitude, cargo readiness, and mission authorization before permitting the mechanical release mechanism to operate.

Cargo release can create an abrupt change in vehicle mass and center of gravity. The CMS therefore communicates release status and relevant payload properties to the FCC and FMS with tightly defined timing semantics. The flight-control system can prepare for the expected dynamic change, while mission management can update aircraft performance and energy predictions after release. Confirmation of actual release should be distinguished from the issuance of a release command.

Environmental monitoring is required when cargo must remain within defined temperature, humidity, vibration, pressure, or other storage limits. The CMS acquires information from payload-compartment sensors and evaluates it against cargo-specific constraints. Warning thresholds can identify deteriorating conditions before hard limits are exceeded. Environmental status can then influence mission decisions when continued transportation may damage sensitive or high-value cargo.

Active environmental control can extend the CMS beyond passive monitoring. Refrigeration, heating, ventilation, or other conditioning equipment may be controlled according to cargo requirements and available aircraft power. Because these systems consume energy, the CMS coordinates with power-management and mission-management functions. Environmental control may be reduced or prioritized according to cargo criticality when aircraft energy margins become constrained.

The CMS maintains a controlled interface with the Flight Management System (FMS). It can provide cargo readiness, payload mass, delivery status, environmental warnings, and handling constraints while receiving mission-phase and delivery authorization information. The FMS uses these states when determining whether departure, continuation, delivery, diversion, or mission completion is permitted, without directly controlling detailed cargo hardware.

The Flight Control Computer (FCC) requires only cargo information that can affect immediate vehicle dynamics or flight safety. Payload mass, estimated center of gravity, cargo-shift detection, door status, or confirmed release events may therefore be communicated through validated interfaces. Detailed logistics information should remain outside the FCC so that deterministic flight-control execution is not unnecessarily coupled to complex cargo-management software.

Cargo-shift detection is especially important for large or heavy payloads. Movement during acceleration, turbulence, or maneuvering can alter the center of gravity and potentially affect controllability. The CMS can use restraint sensors, position sensors, load measurements, or other available information to detect unexpected movement. A confirmed shift can generate warnings and provide the flight system with updated payload-state information for appropriate response.

The Ground Control Station (GCS) provides operators with cargo status without requiring direct interaction with individual devices. The interface can display payload identity, loading state, restraint condition, door status, environmental conditions, delivery progress, and active faults. Operator commands for loading, securing, release, or unloading should be authenticated and evaluated against local CMS interlocks before they are accepted for execution.

Communication loss must not cause cargo equipment to enter an undefined state. If the GCS connection is interrupted, the CMS should preserve the last safe configuration and apply predefined behavior according to the current mission phase. In-flight cargo mechanisms normally remain inhibited unless an autonomous mission procedure explicitly authorizes their operation. Restoration of communication should not automatically repeat commands whose execution state is uncertain.

The software architecture can separate device control, cargo-state management, safety interlocks, environmental management, configuration services, diagnostics, communications, and data logging into modular components. Hardware abstraction prevents higher-level software from depending on a specific latch, sensor, or actuator implementation. This modularity allows different cargo modules to be integrated while preserving consistent aircraft-level interfaces and safety behavior.

Cargo modules may require a standardized interface contract so that different payload systems can be exchanged without redesigning the entire aircraft software stack. The contract can define power requirements, communication messages, status semantics, command authorization, fault reporting, timing behavior, and configuration data. A modular interface is particularly useful for cargo UAV fleets expected to transport containers, pallets, specialized equipment, or mission-specific payload modules.

Fault management identifies failures such as unlocked restraints, actuator jams, sensor disagreement, open doors, communication loss, environmental excursions, unexpected cargo movement, or release-mechanism faults. The CMS evaluates whether a fault prevents departure, requires mission restriction, permits continued transport, or demands diversion or landing. Fault severity must reflect both the cargo consequence and the possible effect on aircraft safety.

Redundancy can be applied selectively to cargo functions whose failure could threaten the aircraft. Critical door-closed indications, locking confirmation, release inhibition, or cargo-position information may require independent sensing or monitoring. Not every logistics function requires the same redundancy level. The architecture should distinguish safety-related cargo functions from mission convenience functions so that complexity is concentrated where failure consequences justify it.

Configuration management ensures that the CMS knows which cargo hardware, software, calibration data, and payload definition are installed for a mission. Incorrect configuration information can invalidate weight calculations, environmental limits, or actuator commands. Version-controlled configuration data and startup compatibility checks reduce the risk of operating a cargo module with incompatible software or applying parameters intended for a different payload type.

Data logging provides traceability across the complete cargo mission. The CMS can record loading confirmation, mass information, lock transitions, environmental history, operator commands, release events, faults, and unloading completion. These records support maintenance, delivery verification, safety investigation, and fleet analysis. Accurate timestamps allow cargo events to be correlated with aircraft motion, power consumption, and mission-state changes.

Cybersecurity is relevant because unauthorized cargo commands can affect both payload integrity and aircraft safety. Interfaces for release, door operation, configuration updates, and maintenance require appropriate authentication, access control, and command validation. Security boundaries should prevent a compromised payload device from obtaining unrestricted access to flight-critical networks while still allowing the validated information required by aircraft systems to cross the interface.

Verification begins with individual cargo devices and software components before progressing to integrated mission scenarios. Software-in-the-loop testing can evaluate state machines, interlocks, configuration logic, and fault responses. Hardware-in-the-loop testing can connect representative locks, doors, sensors, actuators, and controllers to simulated aircraft states, allowing loading, flight, release, unloading, communication loss, and equipment failures to be tested systematically.

System-level validation must include abnormal sequences rather than only nominal cargo handling. Attempts to open a door during flight, release cargo outside an authorized zone, depart with an incomplete lock state, or continue after unexpected cargo movement should produce defined responses. Testing also verifies that CMS faults cannot improperly override FCC authority and that critical cargo information reaches the flight and mission systems within required timing bounds.

The Cargo Management System therefore forms the operational bridge between payload logistics and aircraft autonomy. By integrating cargo configuration, weight and balance information, restraint supervision, environmental management, delivery control, fault handling, and standardized interfaces, the CMS enables diverse payload missions without embedding logistics complexity inside flight-control software. This separation provides a scalable foundation for safe and increasingly autonomous cargo UAV operations.

화물관리시스템(Cargo Management System, CMS)은 화물 무인항공기(Cargo UAV) 내부에서 탑재화물 구성, 적재 상태, 화물 고정 장치, 화물 환경 조건 및 배송 작업을 감독하는 소프트웨어 아키텍처(Software Architecture)를 제공한다. CMS는 화물 전용 장비를 전체 항공기 아키텍처와 연결하면서 비행 필수 제어(Flight-Critical Control) 기능과의 분리를 유지한다. CMS는 탑재화물 정보를 검증된 운용 상태로 변환하여 비행관리시스템(FMS), 비행제어컴퓨터(FCC) 및 지상통제소(GCS)가 사용할 수 있도록 한다.

화물 구성(Cargo Configuration)은 항공기에 탑재된 화물에 대한 구조화된 표현에서 시작한다. CMS는 화물 식별정보, 질량, 크기, 장착 위치, 취급 요구사항, 환경 제한조건, 목적지 정보 및 운용 우선순위를 관리할 수 있다. 이러한 파라미터를 통해 항공기는 설치된 탑재화물이 기체 제한조건에 적합한지 판단하고 비행 전에 선언된 화물 구성이 실제 물리적 적재 상태와 일치하는지 확인할 수 있다.

화물은 항공기 동역학과 추진 요구량에 직접적인 영향을 주기 때문에 질량 및 무게중심(Center of Gravity) 정보가 특히 중요하다. CMS는 측정되거나 입력된 탑재화물 질량을 수신하고 전체 중량 및 균형 구성(Weight and Balance Configuration)에 대한 화물의 영향을 계산할 수 있다. 화물 위치 정보를 항공기 형상과 결합하여 하중 분포를 추정함으로써 다른 시스템이 무게중심이 승인된 운용영역 내에 유지되는지 검증할 수 있도록 한다.

CMS는 비행 중 탑재화물을 고정하는 데 사용되는 화물 구속 및 잠금 장치(Cargo Restraint and Locking Mechanism)를 감독한다. 센서는 래치(Latch), 잠금장치, 클램프, 도어 또는 부착장치가 명령된 위치에 도달했는지를 표시할 수 있다. 소프트웨어는 화물이 안전하게 고정되었다고 판단하기 전에 명령 상태와 실제 관측 상태를 비교한다. 잠금장치가 체결되지 않았다면 도어가 닫혔다는 정보만으로는 충분하지 않을 수 있으므로 여러 조건을 결합하여 검증된 화물 준비 상태(Cargo-Ready State)를 생성할 수 있다.

적재 및 하역 작업(Loading and Unloading Operation)은 명확하게 정의된 운용 상태를 통해 관리된다. 소프트웨어는 빈 상태, 적재 중, 적재 완료-미검증 상태, 고정 완료, 비행 준비, 하역 중 및 완료 상태를 구분할 수 있다. 필요한 센서 입력, 운용자 확인 및 인터록(Interlock)이 충족된 경우에만 상태 전환이 이루어진다. 이러한 상태기계(State Machine) 방식은 화물 장비가 불확실하거나 기계적으로 안전하지 않은 상태에서 비행 준비 절차가 진행되는 것을 방지한다.

인터록(Interlock)은 비행 중 부적절한 화물 관련 동작으로부터 항공기를 보호한다. 화물 도어, 투하장치, 호이스트(Hoist), 램프 또는 자동화된 화물 취급장비는 단순히 명령을 수신했다는 이유만으로 작동해서는 안 된다. CMS는 동작을 허용하기 전에 항공기 상태, 임무 단계, 고도, 지상 상태, 구동기 상태 및 명령 권한을 평가한다. 안전 필수 억제 기능(Safety-Critical Inhibition)은 상위 임무 소프트웨어가 잘못된 동작을 요청하더라도 계속 강제될 수 있어야 한다.

일부 화물 임무에서는 일반적인 착륙 후 하역 대신 제어된 투하, 하강 또는 공중 배송(Aerial Delivery)이 필요할 수 있다. 이러한 구성에서 CMS는 탑재화물 준비 및 투하와 관련된 순서를 조정하며 비행 시스템은 항공기 안정성에 대한 권한을 계속 유지한다. 투하 로직(Release Logic)은 기계적 투하장치의 작동을 허용하기 전에 위치, 고도, 속도, 기체 자세, 화물 준비 상태 및 임무 승인을 검증할 수 있다.

화물 투하(Cargo Release)는 항공기 질량과 무게중심을 급격하게 변화시킬 수 있다. 따라서 CMS는 투하 상태와 관련 탑재화물 특성을 명확하게 정의된 타이밍 조건과 함께 FCC 및 FMS에 전달한다. 비행제어 시스템은 예상되는 동역학적 변화에 대비할 수 있으며 임무관리 시스템은 투하 이후 항공기 성능 및 에너지 예측을 갱신할 수 있다. 실제 화물이 투하되었다는 확인 상태는 단순히 투하 명령이 발행된 상태와 명확하게 구분되어야 한다.

화물이 정의된 온도, 습도, 진동, 압력 또는 기타 보관 한계 내에서 유지되어야 하는 경우 환경감시(Environmental Monitoring)가 필요하다. CMS는 화물칸 센서로부터 정보를 획득하고 이를 화물별 제한조건과 비교하여 평가한다. 경고 임계값(Warning Threshold)을 통해 절대적인 제한조건을 초과하기 전에 상태 악화를 식별할 수 있다. 이후 민감하거나 고가의 화물이 지속적인 운송으로 손상될 가능성이 있다면 환경 상태를 임무 의사결정에 반영할 수 있다.

능동 환경제어(Active Environmental Control)는 CMS의 역할을 수동적인 감시 이상으로 확장할 수 있다. 냉각, 가열, 환기 또는 기타 환경조절 장비는 화물 요구사항과 사용 가능한 항공기 전력에 따라 제어될 수 있다. 이러한 시스템은 에너지를 소비하므로 CMS는 전력관리 및 임무관리 기능과 협조해야 한다. 항공기 에너지 여유가 제한되는 경우 화물 중요도에 따라 환경제어 기능을 축소하거나 우선순위를 조정할 수 있다.

CMS는 비행관리시스템(Flight Management System, FMS)과 통제된 인터페이스(Controlled Interface)를 유지한다. 화물 준비 상태, 탑재화물 질량, 배송 상태, 환경 경고 및 취급 제한조건을 제공하면서 임무 단계와 배송 승인 정보를 수신할 수 있다. FMS는 세부적인 화물 하드웨어를 직접 제어하지 않고 이러한 상태정보를 이용하여 출발, 임무 지속, 배송, 회항 또는 임무 완료의 허용 여부를 결정한다.

비행제어컴퓨터(Flight Control Computer, FCC)는 즉각적인 기체 동역학 또는 비행안전에 영향을 줄 수 있는 화물 정보만 필요로 한다. 따라서 탑재화물 질량, 추정 무게중심, 화물 이동 탐지, 도어 상태 또는 확인된 투하 이벤트를 검증된 인터페이스를 통해 전달할 수 있다. 복잡한 화물관리 소프트웨어와 결정론적 비행제어 실행(Deterministic Flight-Control Execution)이 불필요하게 결합되지 않도록 상세한 물류 정보는 FCC 외부에 유지해야 한다.

대형 또는 중량 화물에서는 화물 이동 탐지(Cargo-Shift Detection)가 특히 중요하다. 가속, 난기류 또는 기동 과정에서 발생하는 화물 이동은 무게중심을 변화시키고 잠재적으로 항공기의 제어 가능성에 영향을 줄 수 있다. CMS는 구속장치 센서, 위치 센서, 하중 측정값 또는 기타 이용 가능한 정보를 사용하여 예상하지 못한 이동을 탐지할 수 있다. 화물 이동이 확인되면 경고를 발생시키고 비행 시스템에 갱신된 탑재화물 상태정보를 제공하여 적절한 대응이 가능하도록 한다.

지상통제소(Ground Control Station, GCS)는 운용자가 개별 장치와 직접 상호작용하지 않고도 화물 상태를 확인할 수 있도록 한다. 인터페이스는 탑재화물 식별정보, 적재 상태, 고정 상태, 도어 상태, 환경 조건, 배송 진행상황 및 활성 고장을 표시할 수 있다. 적재, 고정, 투하 또는 하역과 관련된 운용자 명령은 실행을 승인하기 전에 인증(Authentication)되고 CMS의 로컬 인터록에 따라 평가되어야 한다.

통신 상실(Communication Loss)이 화물 장비를 정의되지 않은 상태로 전환시켜서는 안 된다. GCS 연결이 중단되면 CMS는 마지막 안전 구성을 유지하고 현재 임무 단계에 따라 사전에 정의된 동작을 적용해야 한다. 비행 중 화물 장치는 자율 임무 절차가 명시적으로 동작을 승인하지 않는 한 일반적으로 억제 상태를 유지해야 한다. 통신이 복구되더라도 실행 상태가 불확실한 명령을 자동으로 다시 수행해서는 안 된다.

소프트웨어 아키텍처는 장치 제어, 화물 상태관리, 안전 인터록, 환경관리, 구성 서비스, 진단, 통신 및 데이터 로깅(Data Logging)을 모듈형 구성요소(Modular Component)로 분리할 수 있다. 하드웨어 추상화(Hardware Abstraction)를 통해 상위 소프트웨어가 특정 래치, 센서 또는 구동기 구현에 직접 의존하지 않도록 한다. 이러한 모듈성은 일관된 항공기 수준 인터페이스와 안전 동작을 유지하면서 서로 다른 화물 모듈을 통합할 수 있도록 한다.

서로 다른 탑재화물 시스템을 전체 항공기 소프트웨어 스택을 재설계하지 않고 교체할 수 있도록 화물 모듈에는 표준화된 인터페이스 계약(Standardized Interface Contract)이 필요할 수 있다. 이 계약은 전력 요구사항, 통신 메시지, 상태 의미, 명령 권한, 고장 보고, 타이밍 동작 및 구성 데이터를 정의할 수 있다. 이러한 모듈형 인터페이스는 컨테이너, 팔레트, 특수장비 또는 임무별 탑재 모듈을 운송해야 하는 화물 UAV 함대에서 특히 유용하다.

고장관리(Fault Management)는 잠금되지 않은 구속장치, 구동기 고착, 센서 불일치, 열린 도어, 통신 상실, 환경 제한 초과, 예상하지 못한 화물 이동 또는 투하장치 고장과 같은 문제를 식별한다. CMS는 고장이 출발을 금지해야 하는지, 임무 제한이 필요한지, 운송을 계속할 수 있는지 또는 회항이나 착륙이 필요한지를 평가한다. 고장 심각도(Fault Severity)는 화물에 대한 영향뿐만 아니라 항공기 안전에 미칠 수 있는 영향도 함께 반영해야 한다.

중복성(Redundancy)은 고장으로 인해 항공기의 안전이 위협받을 수 있는 화물 기능에 선택적으로 적용할 수 있다. 핵심 도어 닫힘 상태, 잠금 확인, 투하 억제 또는 화물 위치 정보에는 독립적인 센싱이나 감시가 필요할 수 있다. 모든 물류 기능이 동일한 수준의 중복성을 요구하는 것은 아니다. 아키텍처는 안전 관련 화물 기능과 임무 편의 기능을 구분하여 고장 결과가 충분히 심각한 영역에 복잡성을 집중해야 한다.

구성관리(Configuration Management)는 CMS가 특정 임무에 어떤 화물 하드웨어, 소프트웨어, 보정 데이터 및 탑재화물 정의가 설치되어 있는지를 정확하게 파악하도록 한다. 잘못된 구성정보는 중량 계산, 환경 제한조건 또는 구동기 명령을 무효화할 수 있다. 버전 관리된 구성 데이터와 시작 시 호환성 검사(Startup Compatibility Check)를 적용하면 호환되지 않는 소프트웨어로 화물 모듈을 운용하거나 다른 화물 유형을 위한 파라미터를 잘못 적용할 위험을 줄일 수 있다.

데이터 로깅(Data Logging)은 전체 화물 임무에 대한 추적성(Traceability)을 제공한다. CMS는 적재 확인, 질량 정보, 잠금 상태 전환, 환경 이력, 운용자 명령, 투하 이벤트, 고장 및 하역 완료를 기록할 수 있다. 이러한 기록은 정비, 배송 검증, 안전 조사 및 함대 분석(Fleet Analysis)을 지원한다. 정확한 타임스탬프(Timestamp)를 사용하면 화물 이벤트를 항공기 움직임, 전력 소비 및 임무 상태 변화와 연계하여 분석할 수 있다.

승인되지 않은 화물 명령은 탑재화물 무결성과 항공기 안전 모두에 영향을 미칠 수 있으므로 사이버보안(Cybersecurity)이 중요하다. 투하, 도어 작동, 구성 업데이트 및 정비를 위한 인터페이스에는 적절한 인증, 접근제어(Access Control) 및 명령 검증이 필요하다. 보안 경계(Security Boundary)는 침해된 탑재장치가 비행 필수 네트워크에 제한 없이 접근하는 것을 방지하면서 항공기 시스템에 필요한 검증된 정보는 인터페이스를 통해 전달할 수 있도록 해야 한다.

검증(Verification)은 개별 화물 장치와 소프트웨어 구성요소에서 시작하여 통합 임무 시나리오로 확장된다. 소프트웨어 인더루프(Software-in-the-Loop, SIL) 시험은 상태기계, 인터록, 구성 로직 및 고장 대응을 평가할 수 있다. 하드웨어 인더루프(Hardware-in-the-Loop, HIL) 시험에서는 실제 또는 대표적인 잠금장치, 도어, 센서, 구동기 및 제어기를 시뮬레이션된 항공기 상태와 연결하여 적재, 비행, 투하, 하역, 통신 상실 및 장비 고장을 체계적으로 시험할 수 있다.

시스템 수준 검증(System-Level Validation)은 정상적인 화물 취급뿐만 아니라 비정상적인 동작 순서도 포함해야 한다. 비행 중 도어 개방 시도, 승인된 구역 밖에서의 화물 투하, 잠금 상태가 완료되지 않은 상태에서의 출발 또는 예상하지 못한 화물 이동 이후의 임무 지속 시도는 모두 정의된 대응을 발생시켜야 한다. 또한 CMS 고장이 FCC의 제어 권한을 부적절하게 무시할 수 없는지와 핵심 화물 정보가 요구된 시간 범위 내에서 비행 및 임무 시스템으로 전달되는지도 검증해야 한다.

따라서 화물관리시스템(Cargo Management System, CMS)은 탑재화물 물류(Payload Logistics)와 항공기 자율화(Aircraft Autonomy)를 연결하는 운용적 가교 역할을 한다. 화물 구성, 중량 및 균형 정보, 구속장치 감시, 환경관리, 배송 제어, 고장 처리 및 표준화된 인터페이스를 통합함으로써 비행제어 소프트웨어 내부에 물류 복잡성을 포함시키지 않고 다양한 탑재화물 임무를 수행할 수 있도록 한다. 이러한 기능 분리는 안전하고 점차 고도화되는 자율 화물 UAV 운용을 위한 확장 가능한 기반을 제공한다.

##  

## 02.09. Ground Control Station GCS SW Design [w/Code]

![](images/image9.png){width="7.268055555555556in" height="7.268055555555556in"}

The Ground Control Station (GCS) provides the primary operational interface between human operators and the cargo UAV mission system. Its software integrates mission planning, command transmission, telemetry monitoring, vehicle health visualization, cargo supervision, contingency management, and operational logging. The GCS must present complex aircraft information in a form that supports rapid and reliable decisions without transferring direct flight-stabilization responsibilities from the onboard control system to the operator.

GCS software is commonly organized as a modular architecture separating user-interface functions, mission services, communication management, vehicle-state processing, alerting, configuration, and data recording. This separation prevents changes in one function from unnecessarily affecting unrelated capabilities. It also allows different aircraft types, payload modules, communication links, or operator interfaces to be integrated through defined software interfaces rather than extensive redesign.

Mission planning allows the operator to define departure locations, destinations, waypoints, altitude profiles, speed constraints, delivery locations, alternate landing sites, and operational boundaries. For cargo missions, planning can also incorporate payload mass, delivery priority, energy requirements, loading status, and cargo-specific restrictions. The GCS validates the mission structure before transmission so that incomplete or internally inconsistent plans are identified before aircraft execution.

Geospatial information forms an important part of the operator interface. Route geometry, aircraft position, planned waypoints, geofences, landing locations, terrain constraints, and relevant operational regions can be represented within a common map-based environment. The interface should distinguish planned information from actual aircraft state and clearly indicate when navigation uncertainty or communication delay reduces confidence in the displayed vehicle position.

Command management controls how operator intentions are converted into aircraft requests. Commands may include mission upload, mission start, hold, route modification, return-to-home, diversion, landing, or cargo-related actions. Safety-relevant commands should pass through validation and authorization logic before transmission. The interface can require confirmation for consequential actions while avoiding unnecessary interaction steps for routine monitoring tasks.

The GCS communicates primarily with the onboard Flight Management System (FMS) rather than directly controlling low-level actuators. The FMS interprets mission-level requests and determines whether they are compatible with aircraft state, operational constraints, and onboard safety logic. This architectural boundary ensures that loss, delay, or corruption of a ground command does not directly bypass the deterministic protections implemented by the Flight Control Computer (FCC).

Telemetry processing converts incoming aircraft messages into a coherent operational state. Information may include position, velocity, altitude, heading, flight mode, propulsion status, energy reserves, navigation quality, communication health, sensor status, and cargo condition. The GCS should associate telemetry with timestamps and validity information so that operators can distinguish current aircraft state from delayed, stale, or incomplete information.

Vehicle-health monitoring summarizes large quantities of subsystem information without overwhelming the operator. Flight control, navigation, propulsion, power, communication, sensors, and cargo systems can provide detailed diagnostic data, but normal operation requires a prioritized presentation. The interface should emphasize conditions requiring attention while allowing deeper diagnostic information to be accessed when troubleshooting or maintenance analysis is necessary.

Alert management is critical because excessive alarms can be as problematic as missing warnings. GCS software should classify alerts according to severity, urgency, persistence, and required operator response. Related events can be grouped to prevent a single underlying fault from producing numerous redundant messages. Acknowledgment should confirm operator awareness without hiding an unresolved condition or altering the onboard system\'s independent protective behavior.

Communication-link management monitors command-and-control connectivity between the GCS and aircraft. Link quality, latency, packet loss, message age, available bandwidth, and communication-path status can be evaluated continuously. When redundant radio, cellular, satellite, or other communication channels are available, the GCS may supervise path selection or display which path is active while onboard logic maintains defined behavior during link transitions.

The interface must explicitly represent communication degradation. A vehicle icon continuing to move based only on prediction can create false confidence if telemetry has stopped. The GCS should therefore indicate the age and validity of displayed information and clearly distinguish live telemetry from extrapolated or previously received data. Lost-link conditions should generate defined indications while the aircraft executes its onboard contingency procedure independently.

Cargo supervision extends the GCS beyond conventional flight monitoring. Operators can observe payload identity, mass, loading state, restraint status, environmental conditions, cargo-door state, and delivery progress through information supplied by the Cargo Management System (CMS). Commands related to securing, releasing, or unloading cargo remain subject to onboard interlocks so that ground authority cannot directly bypass aircraft or payload safety conditions.

Energy monitoring is especially important for cargo UAV operations. The GCS can display State of Charge (SoC), available energy, State of Health (SoH), predicted endurance, destination reserve, and abnormal battery or power conditions. Rather than showing only instantaneous battery percentage, the interface should communicate whether the current mission remains energetically feasible and whether reserve margins are improving or deteriorating.

The GCS can support contingency management by presenting available recovery options when abnormal conditions occur. Navigation degradation, propulsion faults, low energy, adverse weather, destination unavailability, or cargo anomalies may require holding, diversion, return, or landing. The software can present the aircraft\'s current contingency state and permitted alternatives while onboard systems retain authority for immediate safety-critical responses.

Human-machine interface design must consider operator workload and situational awareness. Critical flight state, mission progress, energy margin, communication status, and significant faults should be understandable without searching through multiple displays. Consistent symbols, terminology, units, alert behavior, and information hierarchy reduce interpretation errors. Detailed engineering data should remain accessible without dominating the primary operational view.

Role-based access control can separate operational functions from maintenance, engineering, and administrative privileges. A mission operator may be authorized to upload routes or initiate approved contingency actions, while software updates and calibration changes require different credentials. This separation reduces the possibility that routine operations accidentally modify configuration data or that unauthorized users gain access to safety-relevant functions.

Cybersecurity is essential because the GCS is an external entry point into the aircraft system. User authentication, command authorization, encrypted communication where appropriate, integrity checking, secure software updates, and protected credential storage can reduce exposure to unauthorized control or data modification. Security events should be logged and monitored without preventing legitimate safety actions from being executed within required time limits.

Configuration management ensures that the GCS uses aircraft-compatible mission definitions, message formats, maps, payload descriptions, and operational parameters. Software and configuration versions should be identifiable and checked before mission execution. When multiple cargo UAV variants are supported, the station must avoid applying a configuration intended for one aircraft type to another vehicle with different performance, interfaces, or payload capabilities.

Operational logging creates a synchronized record of commands, telemetry, alerts, operator actions, communication events, mission modifications, and configuration changes. Accurate timestamps allow ground records to be correlated with onboard FCC, FMS, CMS, and power-system logs. These records support incident reconstruction, maintenance analysis, delivery verification, operator training, and evaluation of fleet-level operational performance.

The GCS can also support multi-aircraft operations when cargo fleets expand beyond one operator-to-one-aircraft supervision. Fleet interfaces must summarize each aircraft\'s mission state, energy margin, communication health, and active alerts while directing operator attention toward vehicles requiring intervention. Scalability should not reduce visibility of critical events, and authority boundaries must remain clear when responsibility transfers between operators or control stations.

Software resilience is necessary because the GCS itself can experience application crashes, network interruptions, hardware failures, or power loss. Aircraft safety must not depend on uninterrupted execution of the ground application. GCS software can preserve mission records, recover communication sessions, and reconstruct current operational state after restart, while onboard autonomy continues according to previously validated mission and contingency logic.

Verification begins with software components such as mission planning, command validation, telemetry decoding, alert processing, and user-interface logic. Simulated aircraft can exercise the GCS across normal and abnormal mission scenarios without flight risk. Communication delay, packet loss, stale telemetry, conflicting commands, low energy, navigation faults, and cargo-system anomalies can be introduced systematically to evaluate operator indications and software responses.

Hardware-in-the-loop and integrated system testing connect the GCS with representative onboard computers, communication equipment, and simulated aircraft dynamics. These tests verify end-to-end command paths, telemetry timing, mission updates, lost-link transitions, cargo interactions, and recovery behavior. Human-in-the-loop evaluation additionally examines whether operators correctly recognize abnormal conditions and select appropriate actions under realistic workload.

A well-designed Ground Control Station therefore acts as the supervisory human interface to cargo UAV autonomy rather than as a remote low-level flight controller. By integrating mission planning, command management, telemetry, health monitoring, cargo supervision, energy awareness, contingency support, cybersecurity, and operational logging, the GCS allows operators to manage complex missions while preserving the authority and safety boundaries of onboard autonomous systems.

지상통제소(Ground Control Station, GCS)는 인간 운용자와 화물 무인항공기(Cargo UAV) 임무 시스템 사이의 핵심 운용 인터페이스를 제공한다. GCS 소프트웨어는 임무계획, 명령 전송, 텔레메트리 감시, 기체 상태 시각화, 화물 감독, 비상대응 관리 및 운용 로깅을 통합한다. GCS는 복잡한 항공기 정보를 신속하고 신뢰할 수 있는 의사결정을 지원하는 형태로 제공하면서도 탑재 제어 시스템의 직접적인 비행 안정화 책임을 운용자에게 이전하지 않아야 한다.

GCS 소프트웨어는 일반적으로 사용자 인터페이스(User Interface), 임무 서비스, 통신관리, 기체 상태 처리, 경보, 구성관리 및 데이터 기록 기능을 분리하는 모듈형 아키텍처(Modular Architecture)로 구성된다. 이러한 분리는 하나의 기능 변경이 관련 없는 다른 기능에 불필요한 영향을 미치는 것을 방지한다. 또한 서로 다른 항공기 유형, 탑재화물 모듈, 통신 링크 또는 운용자 인터페이스를 대규모 재설계 없이 정의된 소프트웨어 인터페이스를 통해 통합할 수 있도록 한다.

임무계획(Mission Planning)을 통해 운용자는 출발 위치, 목적지, 웨이포인트(Waypoint), 고도 프로파일, 속도 제약조건, 배송 위치, 대체 착륙지 및 운용 경계를 정의할 수 있다. 화물 임무에서는 탑재화물 질량, 배송 우선순위, 에너지 요구량, 적재 상태 및 화물별 제한조건도 계획에 포함할 수 있다. GCS는 임무를 전송하기 전에 임무 구조를 검증하여 불완전하거나 내부적으로 일관되지 않은 계획을 항공기가 실행하기 전에 식별한다.

지리공간 정보(Geospatial Information)는 운용자 인터페이스의 중요한 부분을 구성한다. 항로 형상, 항공기 위치, 계획된 웨이포인트, 지오펜스(Geofence), 착륙 위치, 지형 제약조건 및 관련 운용 영역을 공통 지도 기반 환경(Map-Based Environment)에 표시할 수 있다. 인터페이스는 계획된 정보와 실제 항공기 상태를 구분해야 하며, 항법 불확실성이나 통신 지연으로 표시된 기체 위치에 대한 신뢰도가 감소하는 경우 이를 명확하게 나타내야 한다.

명령관리(Command Management)는 운용자의 의도를 항공기 요청으로 변환하는 방법을 제어한다. 명령에는 임무 업로드, 임무 시작, 체공(Hold), 항로 수정, 자동복귀(Return-to-Home), 회항(Diversion), 착륙 또는 화물 관련 동작이 포함될 수 있다. 안전 관련 명령은 전송 전에 검증 및 권한확인 로직을 통과해야 한다. 인터페이스는 중대한 결과를 초래할 수 있는 동작에는 확인 절차를 요구하면서 일상적인 감시 작업에는 불필요한 상호작용 단계를 최소화할 수 있다.

GCS는 저수준 구동기를 직접 제어하기보다 주로 탑재 비행관리시스템(Flight Management System, FMS)과 통신한다. FMS는 임무 수준 요청을 해석하고 해당 요청이 항공기 상태, 운용 제약조건 및 탑재 안전 로직과 호환되는지를 판단한다. 이러한 아키텍처 경계(Architectural Boundary)는 지상 명령이 손실되거나 지연 또는 손상되더라도 비행제어컴퓨터(Flight Control Computer, FCC)에 구현된 결정론적 보호 기능(Deterministic Protection)을 직접 우회하지 못하도록 한다.

텔레메트리 처리(Telemetry Processing)는 항공기로부터 수신되는 메시지를 일관된 운용 상태로 변환한다. 정보에는 위치, 속도, 고도, 기수방향, 비행모드, 추진 상태, 에너지 잔량, 항법 품질, 통신 상태, 센서 상태 및 화물 상태가 포함될 수 있다. GCS는 텔레메트리를 타임스탬프(Timestamp) 및 유효성 정보와 연결하여 운용자가 현재 항공기 상태를 지연되거나 오래되었거나 불완전한 정보와 구분할 수 있도록 해야 한다.

기체 상태감시(Vehicle-Health Monitoring)는 운용자에게 과도한 정보를 제공하지 않으면서 많은 하위 시스템의 정보를 요약한다. 비행제어, 항법, 추진, 전력, 통신, 센서 및 화물 시스템은 상세한 진단 데이터를 제공할 수 있지만 정상 운용에서는 우선순위에 따라 정보를 표시해야 한다. 인터페이스는 주의가 필요한 상태를 강조하면서 문제 해결이나 정비 분석이 필요한 경우 보다 상세한 진단 정보에 접근할 수 있도록 해야 한다.

과도한 경보는 경고 누락만큼 문제가 될 수 있으므로 경보관리(Alert Management)는 매우 중요하다. GCS 소프트웨어는 심각도, 긴급성, 지속성 및 필요한 운용자 대응에 따라 경보를 분류해야 한다. 하나의 근본적인 고장으로 다수의 중복 메시지가 생성되지 않도록 관련 이벤트를 그룹화할 수 있다. 경보 확인(Acknowledgment)은 운용자가 상황을 인식했음을 나타내야 하지만 해결되지 않은 상태를 숨기거나 탑재 시스템의 독립적인 보호 동작을 변경해서는 안 된다.

통신 링크 관리(Communication-Link Management)는 GCS와 항공기 사이의 명령통제(Command and Control) 연결 상태를 감시한다. 링크 품질, 지연시간, 패킷 손실, 메시지 경과시간, 사용 가능한 대역폭 및 통신 경로 상태를 지속적으로 평가할 수 있다. 중복 무선, 셀룰러, 위성 또는 기타 통신 채널을 사용할 수 있는 경우 GCS는 경로 선택을 감독하거나 현재 활성 경로를 표시할 수 있으며, 탑재 로직은 링크 전환 과정에서도 정의된 동작을 유지한다.

인터페이스는 통신 성능 저하(Communication Degradation)를 명확하게 표시해야 한다. 텔레메트리가 중단되었는데 예측값만을 기반으로 기체 아이콘이 계속 이동하면 운용자에게 잘못된 확신을 줄 수 있다. 따라서 GCS는 표시된 정보의 경과시간과 유효성을 나타내고 실시간 텔레메트리와 외삽되거나 이전에 수신된 데이터를 명확하게 구분해야 한다. 통신두절(Lost-Link) 상태에서는 항공기가 탑재 비상대응 절차를 독립적으로 수행하는 동안 정의된 경고를 제공해야 한다.

화물 감독(Cargo Supervision)은 GCS의 역할을 일반적인 비행감시 이상으로 확장한다. 운용자는 화물관리시스템(Cargo Management System, CMS)이 제공하는 정보를 통해 탑재화물 식별정보, 질량, 적재 상태, 구속장치 상태, 환경 조건, 화물 도어 상태 및 배송 진행상황을 확인할 수 있다. 화물 고정, 투하 또는 하역 관련 명령은 지상 권한이 항공기나 탑재화물의 안전조건을 직접 우회할 수 없도록 탑재 인터록(Interlock)의 적용을 계속 받아야 한다.

에너지 감시(Energy Monitoring)는 화물 UAV 운용에서 특히 중요하다. GCS는 충전상태(State of Charge, SoC), 사용 가능한 에너지, 건강상태(State of Health, SoH), 예상 비행시간, 목적지 도착 시 예비 에너지 및 비정상적인 배터리 또는 전력 상태를 표시할 수 있다. 인터페이스는 단순히 순간적인 배터리 비율만 보여주는 것이 아니라 현재 임무의 에너지 측면 수행 가능성과 예비 에너지 여유도가 개선되고 있는지 또는 악화되고 있는지를 전달해야 한다.

GCS는 비정상 상태가 발생했을 때 이용 가능한 복구 선택지를 제시하여 비상대응 관리(Contingency Management)를 지원할 수 있다. 항법 성능 저하, 추진 시스템 고장, 에너지 부족, 악천후, 목적지 이용 불가 또는 화물 이상이 발생하면 체공, 회항, 복귀 또는 착륙이 필요할 수 있다. 소프트웨어는 항공기의 현재 비상대응 상태와 허용 가능한 대안을 표시할 수 있으며, 탑재 시스템은 즉각적인 안전 필수 대응에 대한 권한을 계속 유지한다.

인간-기계 인터페이스(Human-Machine Interface, HMI) 설계에서는 운용자 작업부하와 상황인식(Situational Awareness)을 고려해야 한다. 핵심 비행 상태, 임무 진행상황, 에너지 여유도, 통신 상태 및 중요한 고장은 여러 화면을 검색하지 않고도 이해할 수 있어야 한다. 일관된 기호, 용어, 단위, 경보 동작 및 정보 계층구조는 해석 오류를 줄인다. 상세 엔지니어링 데이터는 기본 운용 화면을 방해하지 않으면서 필요할 때 접근할 수 있어야 한다.

역할 기반 접근제어(Role-Based Access Control)는 운용 기능을 정비, 엔지니어링 및 관리 권한과 분리할 수 있다. 임무 운용자는 항로를 업로드하거나 승인된 비상대응 동작을 시작할 권한을 가질 수 있지만 소프트웨어 업데이트 및 보정 변경에는 다른 인증정보가 필요할 수 있다. 이러한 분리는 일상적인 운용 과정에서 구성 데이터가 실수로 변경되거나 승인되지 않은 사용자가 안전 관련 기능에 접근할 가능성을 줄인다.

GCS는 항공기 시스템으로 연결되는 외부 진입점이므로 사이버보안(Cybersecurity)이 필수적이다. 사용자 인증, 명령 권한확인, 필요한 경우 암호화 통신, 무결성 검사, 안전한 소프트웨어 업데이트 및 보호된 인증정보 저장을 통해 승인되지 않은 제어나 데이터 변조 위험을 줄일 수 있다. 보안 이벤트는 기록되고 감시되어야 하지만 정당한 안전 동작이 요구된 시간 내에 수행되는 것을 방해해서는 안 된다.

구성관리(Configuration Management)는 GCS가 항공기와 호환되는 임무 정의, 메시지 형식, 지도, 탑재화물 설명 및 운용 파라미터를 사용하도록 보장한다. 소프트웨어 및 구성 버전은 식별 가능해야 하며 임무 수행 전에 검증되어야 한다. 여러 종류의 화물 UAV를 지원하는 경우 GCS는 특정 항공기 유형을 위한 구성이 서로 다른 성능, 인터페이스 또는 탑재화물 능력을 가진 다른 기체에 잘못 적용되지 않도록 해야 한다.

운용 로깅(Operational Logging)은 명령, 텔레메트리, 경보, 운용자 동작, 통신 이벤트, 임무 변경 및 구성 변경에 대한 동기화된 기록을 생성한다. 정확한 타임스탬프를 사용하면 지상 기록을 탑재 FCC, FMS, CMS 및 전력 시스템 로그와 연계할 수 있다. 이러한 기록은 사고 상황 재구성, 정비 분석, 배송 검증, 운용자 교육 및 함대 수준 운용 성능(Fleet-Level Operational Performance) 평가를 지원한다.

화물 UAV 함대가 한 명의 운용자가 한 대의 항공기를 감독하는 구조를 넘어 확장될 경우 GCS는 다중 항공기 운용(Multi-Aircraft Operation)도 지원할 수 있다. 함대 인터페이스는 각 항공기의 임무 상태, 에너지 여유도, 통신 상태 및 활성 경보를 요약하면서 개입이 필요한 기체로 운용자의 주의를 유도해야 한다. 확장성이 핵심 이벤트의 가시성을 감소시켜서는 안 되며 운용자 또는 통제소 사이에서 책임이 이전될 때도 권한 경계가 명확하게 유지되어야 한다.

GCS 자체에서도 애플리케이션 충돌, 네트워크 중단, 하드웨어 고장 또는 전원 상실이 발생할 수 있으므로 소프트웨어 복원력(Software Resilience)이 필요하다. 항공기 안전이 지상 애플리케이션의 중단 없는 실행에 의존해서는 안 된다. GCS 소프트웨어는 임무 기록을 보존하고 통신 세션을 복구하며 재시작 이후 현재 운용 상태를 재구성할 수 있고, 탑재 자율 시스템은 기존에 검증된 임무 및 비상대응 로직에 따라 계속 동작한다.

검증(Verification)은 임무계획, 명령 검증, 텔레메트리 디코딩, 경보 처리 및 사용자 인터페이스 로직과 같은 소프트웨어 구성요소에서 시작한다. 시뮬레이션된 항공기를 이용하면 실제 비행 위험 없이 정상 및 비정상 임무 시나리오에서 GCS를 시험할 수 있다. 통신 지연, 패킷 손실, 오래된 텔레메트리, 충돌하는 명령, 에너지 부족, 항법 고장 및 화물 시스템 이상을 체계적으로 발생시켜 운용자 표시와 소프트웨어 대응을 평가할 수 있다.

하드웨어 인더루프(Hardware-in-the-Loop, HIL) 및 통합 시스템 시험은 GCS를 대표적인 탑재 컴퓨터, 통신장비 및 시뮬레이션된 항공기 동역학과 연결한다. 이러한 시험을 통해 종단 간 명령 경로, 텔레메트리 타이밍, 임무 업데이트, 통신두절 상태 전환, 화물 시스템 상호작용 및 복구 동작을 검증한다. 인간 인더루프(Human-in-the-Loop) 평가에서는 실제와 유사한 작업부하 조건에서 운용자가 비정상 상태를 올바르게 인식하고 적절한 조치를 선택하는지도 추가로 평가한다.

잘 설계된 지상통제소(Ground Control Station)는 원격 저수준 비행제어기가 아니라 화물 UAV 자율 시스템을 감독하는 인간 인터페이스(Supervisory Human Interface)로 기능한다. 임무계획, 명령관리, 텔레메트리, 상태감시, 화물 감독, 에너지 상황인식, 비상대응 지원, 사이버보안 및 운용 로깅을 통합함으로써 GCS는 운용자가 복잡한 임무를 관리하면서도 탑재 자율 시스템의 제어 권한과 안전 경계를 유지할 수 있도록 한다.

##  

## 02.10. System Integration Test Architecture UAV

![](images/image10.png){width="7.268055555555556in" height="7.268055555555556in"}

System integration testing verifies that the cargo UAV operates as a coordinated aircraft rather than as a collection of independently validated subsystems. The integration test architecture connects flight control, mission management, navigation, perception, propulsion, power, cargo management, communication, and ground-control functions within representative operational environments. Its objective is to expose interface, timing, configuration, and interaction failures before they can appear during unrestricted flight.

Integration testing follows the operational interfaces defined by the aircraft architecture. The Flight Control Computer (FCC), Flight Management System (FMS), Cargo Management System (CMS), Ground Control Station (GCS), sensor suite, propulsion controllers, and power-management system exchange commands, states, health information, and timing data. Testing verifies not only individual messages but also whether the complete information flow produces the intended aircraft behavior.

A layered test architecture allows software to mature progressively from isolated execution toward physical aircraft operation. Software-in-the-Loop (SIL) testing executes flight and mission software against simulated aircraft dynamics and environmental models. Processor-in-the-Loop and Hardware-in-the-Loop (HIL) stages introduce representative computing hardware, interfaces, and timing. Ground integration and flight testing then validate behavior with increasingly complete physical systems.

SIL provides a scalable environment for exercising large numbers of scenarios before hardware becomes available. Simulated vehicle dynamics can respond to FCC commands while models generate navigation, sensor, propulsion, power, and cargo states. Engineers can repeat identical conditions, accelerate simulation time, and introduce precisely controlled failures. This repeatability makes SIL particularly useful for regression testing after software or configuration changes.

HIL testing introduces actual avionics computers and communication interfaces while maintaining simulated vehicle dynamics. The FCC can receive realistic sensor signals and issue actuator commands that are returned to the simulation, creating a closed control loop. FMS, CMS, power controllers, and communication equipment can be added progressively. This environment exposes execution-time, bus-loading, driver, synchronization, and hardware-interface issues that pure software simulation may not reveal.

Real-time simulation is essential when testing flight-control functions. The simulated aircraft model must produce sensor and dynamic responses within timing bounds consistent with physical flight. Excessive simulator jitter or unrealistic latency can either create false failures or hide actual timing weaknesses. Test infrastructure therefore requires synchronized clocks, controlled execution rates, measurable end-to-end latency, and traceable timestamps across simulated and physical components.

The sensor integration environment reproduces the information expected from IMUs, GNSS, LiDAR, cameras, radar, altitude sensors, and other navigation sources. Depending on the test level, sensors may be entirely simulated, electrically stimulated, or physically operated in controlled environments. The objective is to verify acquisition, timestamping, calibration, fusion, validity handling, and degraded-sensor behavior across the complete navigation and perception chain.

Communication-network testing evaluates avionics buses and network links under both nominal and stressed conditions. CAN, Ethernet-based networks, redundant avionics channels, and command-and-control links can be exercised with realistic message rates and traffic patterns. Tests measure latency, jitter, packet loss, arbitration effects, gateway behavior, and bus utilization while verifying that flight-critical information continues to satisfy its timing requirements.

The power-system test environment represents batteries, converters, distribution buses, contactors, and electrical loads. Battery State of Charge (SoC), voltage, current, temperature, and fault conditions can be varied while the aircraft software executes representative missions. Integration tests verify that low-energy conditions, source loss, bus undervoltage, and load shedding propagate correctly through the power-management system to the FMS, FCC, and GCS.

Cargo-system integration testing verifies loading, restraint, door, environmental-control, and delivery functions together with aircraft mission logic. The CMS can be connected to representative locks, sensors, and actuators while flight state is simulated. Tests ensure that cargo operations are inhibited during unsafe conditions, that payload mass and release information reach relevant aircraft systems, and that cargo faults produce appropriate mission restrictions or contingency actions.

The GCS is included in end-to-end testing because operator interaction forms part of the operational system. Mission uploads, route changes, hold commands, diversions, return-to-home requests, cargo commands, telemetry displays, and alerts are exercised through representative communication links. The test architecture verifies that operator requests pass through required authorization and onboard validation rather than bypassing aircraft safety boundaries.

Scenario-based testing evaluates complete operational sequences rather than isolated interfaces. A representative test can begin with power-up and built-in tests, continue through mission loading, cargo verification, takeoff, climb, cruise, delivery, return, landing, and shutdown. Each phase creates different interactions among subsystems, allowing integration defects that occur only during transitions or specific combinations of aircraft states to be detected.

Fault injection is a central capability of the integration architecture. Tests can introduce sensor bias, dropped messages, delayed telemetry, processor resets, communication loss, battery degradation, propulsion faults, cargo-lock disagreement, or invalid configuration data. Controlled injection makes it possible to verify fault detection, isolation, annunciation, reconfiguration, and recovery without intentionally creating hazardous conditions on a flying aircraft.

Single faults are not sufficient to characterize a fault-tolerant UAV. Testing must also consider sequential and combined failures that occur after the system has already entered a degraded state. A redundant computer may fail after a sensor channel has been isolated, or communication may be lost after an energy fault triggers diversion. These combinations verify whether degraded configurations retain internally consistent safety and mission behavior.

Interface verification examines more than message syntax. Data units, coordinate frames, sign conventions, scaling, valid ranges, timestamps, sequence counters, update rates, timeout behavior, and fault semantics must agree between transmitting and receiving systems. Many integration failures occur when individually correct components interpret the same data differently, making interface-contract verification a fundamental part of system testing.

Configuration control ensures that test results correspond to a precisely defined aircraft state. Software builds, firmware versions, calibration files, network definitions, aircraft parameters, payload configurations, and simulation models should be uniquely identifiable. Automated recording of the test configuration allows failures to be reproduced and prevents results from different hardware or software baselines from being incorrectly compared.

Test instrumentation must observe system behavior without significantly changing it. Logs can capture control states, sensor estimates, communication traffic, power conditions, mission transitions, cargo events, fault flags, and operator actions. A common time reference allows events from distributed computers to be reconstructed in sequence. High-rate data should be selected carefully so that instrumentation itself does not overload processors or communication networks.

Automated test execution improves coverage and repeatability. Test scripts can configure initial conditions, launch simulations, issue commands, inject faults, monitor expected responses, and compare measured results with acceptance criteria. Automation is especially valuable for regression testing, where hundreds or thousands of previously validated scenarios may need to be rerun after modifications to flight software, interfaces, or configuration data.

Acceptance criteria should be defined before test execution. Requirements can specify allowable tracking error, response time, communication latency, fault-detection time, energy reserve, transition behavior, or recovery performance. Pass or fail decisions should therefore be based on measurable evidence rather than subjective observation. Where tolerances are required, their relationship to system-level safety and performance requirements should remain traceable.

Requirements traceability connects each integration test to the behavior it is intended to verify. A test case identifies applicable requirements, initial conditions, equipment configuration, execution procedure, expected results, and collected evidence. When a failure occurs, engineers can determine which requirement is affected and whether the problem originates in software, hardware, an interface, configuration data, the simulation environment, or the requirement itself.

Regression testing is necessary whenever integrated software or hardware changes. A modification to navigation, communication, power management, or cargo logic can influence functions outside the modified subsystem through shared interfaces. A controlled regression suite therefore includes both tests directly related to the change and broader end-to-end scenarios. This approach reduces the risk that a local correction introduces an unexpected aircraft-level behavior elsewhere.

Ground integration testing moves beyond laboratory simulation by connecting installed aircraft equipment before flight. Power distribution, avionics networks, sensors, actuators, propulsion interfaces, cargo equipment, antennas, and GCS links can be evaluated in the actual vehicle configuration. Ground tests reveal installation-specific effects such as wiring errors, electromagnetic interference, vibration sensitivity, thermal behavior, and physical interface problems.

Flight testing is introduced only after lower-level environments provide sufficient evidence that the integrated system behaves predictably. Early flights typically use constrained envelopes and carefully selected scenarios before progressing toward broader autonomous operation. Test expansion should preserve the ability to monitor critical variables, terminate unsafe scenarios, and compare measured flight behavior with results previously obtained from simulation and HIL environments.

Post-test analysis combines onboard logs, GCS records, simulator data, network captures, and instrumentation measurements into a synchronized record. Engineers compare actual behavior with expected transitions and quantitative acceptance criteria. Unexpected results are classified, investigated, corrected, and retested. Maintaining this evidence creates a verification history showing how the UAV architecture matured from component testing to integrated flight operation.

A mature system integration test architecture therefore creates a continuous verification path from software models to the complete cargo UAV. SIL, HIL, network and power simulation, cargo integration, fault injection, ground testing, and flight testing provide complementary evidence at increasing levels of physical realism. Together they verify that autonomous flight, mission, energy, sensing, cargo, communication, and safety functions cooperate predictably across normal, degraded, and emergency conditions.

시스템 통합시험(System Integration Testing)은 화물 무인항공기(Cargo UAV)가 독립적으로 검증된 하위 시스템의 단순한 집합이 아니라 하나의 조정된 항공기로 동작하는지를 검증한다. 통합시험 아키텍처(Integration Test Architecture)는 비행제어, 임무관리, 항법, 인지, 추진, 전력, 화물관리, 통신 및 지상통제 기능을 대표적인 운용 환경에서 연결한다. 목적은 제한 없는 실제 비행에서 문제가 발생하기 전에 인터페이스, 타이밍, 구성 및 상호작용 관련 고장을 발견하는 것이다.

통합시험은 항공기 아키텍처에 정의된 운용 인터페이스(Operational Interface)를 기반으로 수행된다. 비행제어컴퓨터(Flight Control Computer, FCC), 비행관리시스템(Flight Management System, FMS), 화물관리시스템(Cargo Management System, CMS), 지상통제소(Ground Control Station, GCS), 센서 제품군, 추진 제어기 및 전력관리 시스템은 명령, 상태, 건전성 정보 및 타이밍 데이터를 교환한다. 시험에서는 개별 메시지뿐만 아니라 전체 정보 흐름이 의도한 항공기 동작을 생성하는지도 검증한다.

계층형 시험 아키텍처(Layered Test Architecture)는 소프트웨어가 독립 실행 단계에서 실제 항공기 운용 단계까지 점진적으로 성숙할 수 있도록 한다. 소프트웨어 인더루프(Software-in-the-Loop, SIL) 시험은 시뮬레이션된 항공기 동역학과 환경 모델을 대상으로 비행 및 임무 소프트웨어를 실행한다. 프로세서 인더루프(Processor-in-the-Loop)와 하드웨어 인더루프(Hardware-in-the-Loop, HIL) 단계에서는 실제와 유사한 연산 하드웨어, 인터페이스 및 타이밍을 도입한다. 이후 지상 통합시험과 비행시험을 통해 점차 완전한 물리적 시스템에서 동작을 검증한다.

SIL은 실제 하드웨어가 준비되기 전에 많은 시나리오를 실행할 수 있는 확장 가능한 시험환경을 제공한다. 시뮬레이션된 기체 동역학은 FCC 명령에 반응하고 모델은 항법, 센서, 추진, 전력 및 화물 상태를 생성할 수 있다. 엔지니어는 동일한 조건을 반복하고 시뮬레이션 시간을 가속하며 정밀하게 제어된 고장을 주입할 수 있다. 이러한 반복성은 소프트웨어나 구성 변경 이후의 회귀시험(Regression Testing)에 SIL을 특히 유용하게 만든다.

HIL 시험은 기체 동역학은 시뮬레이션 상태로 유지하면서 실제 항공전자 컴퓨터와 통신 인터페이스를 도입한다. FCC는 실제와 유사한 센서 신호를 수신하고 구동기 명령을 출력하며, 이 명령은 다시 시뮬레이션으로 전달되어 폐루프 제어(Closed Control Loop)를 형성한다. FMS, CMS, 전력 제어기 및 통신장비도 점진적으로 추가할 수 있다. 이러한 환경은 순수 소프트웨어 시뮬레이션에서 발견하기 어려운 실행시간, 버스 부하, 드라이버, 동기화 및 하드웨어 인터페이스 문제를 드러낸다.

비행제어 기능을 시험할 때는 실시간 시뮬레이션(Real-Time Simulation)이 필수적이다. 시뮬레이션된 항공기 모델은 실제 비행과 일관된 타이밍 범위 내에서 센서 및 동역학 응답을 생성해야 한다. 과도한 시뮬레이터 지터(Jitter)나 비현실적인 지연시간은 잘못된 고장을 발생시키거나 실제 타이밍 취약점을 숨길 수 있다. 따라서 시험 인프라에는 동기화된 클록, 제어된 실행주기, 측정 가능한 종단 간 지연시간(End-to-End Latency) 및 추적 가능한 타임스탬프가 필요하다.

센서 통합 환경(Sensor Integration Environment)은 관성측정장치(IMU), 위성항법시스템(GNSS), 라이다(LiDAR), 카메라, 레이더, 고도 센서 및 기타 항법 정보원에서 예상되는 정보를 재현한다. 시험 수준에 따라 센서는 완전히 시뮬레이션하거나 전기적 신호로 자극하거나 통제된 환경에서 실제 장치를 운용할 수 있다. 목적은 전체 항법 및 인지 체인에서 데이터 획득, 타임스탬프 부여, 보정, 융합, 유효성 처리 및 센서 성능저하 상황의 동작을 검증하는 것이다.

통신 네트워크 시험(Communication-Network Testing)은 정상 조건과 스트레스 조건 모두에서 항공전자 버스와 네트워크 링크를 평가한다. 제어기 영역 네트워크(Controller Area Network, CAN), 이더넷 기반 네트워크, 중복 항공전자 채널 및 명령통제 링크를 실제와 유사한 메시지 전송률과 트래픽 패턴으로 시험할 수 있다. 시험에서는 지연시간, 지터, 패킷 손실, 중재 효과, 게이트웨이 동작 및 버스 이용률을 측정하면서 비행 필수 정보가 요구된 타이밍 조건을 계속 충족하는지 검증한다.

전력 시스템 시험환경(Power-System Test Environment)은 배터리, 변환기, 분배 버스, 접촉기 및 전기 부하를 표현한다. 배터리 충전상태(State of Charge, SoC), 전압, 전류, 온도 및 고장 조건을 변화시키면서 항공기 소프트웨어가 대표적인 임무를 수행하도록 할 수 있다. 통합시험은 저에너지 상태, 전원 상실, 버스 저전압 및 부하 차단이 전력관리 시스템을 통해 FMS, FCC 및 GCS에 올바르게 전달되는지를 검증한다.

화물 시스템 통합시험(Cargo-System Integration Testing)은 적재, 구속장치, 도어, 환경제어 및 배송 기능을 항공기 임무 로직과 함께 검증한다. CMS를 실제 또는 대표적인 잠금장치, 센서 및 구동기에 연결하면서 비행 상태를 시뮬레이션할 수 있다. 시험에서는 안전하지 않은 조건에서 화물 관련 동작이 억제되는지, 탑재화물 질량과 투하 정보가 관련 항공기 시스템에 전달되는지, 화물 고장이 적절한 임무 제한 또는 비상대응 동작을 발생시키는지를 확인한다.

운용자 상호작용(Operator Interaction)은 전체 운용 시스템의 일부이므로 GCS도 종단 간 시험(End-to-End Testing)에 포함된다. 임무 업로드, 항로 변경, 체공 명령, 회항, 자동복귀(Return-to-Home) 요청, 화물 명령, 텔레메트리 표시 및 경보를 대표적인 통신 링크를 통해 시험한다. 시험 아키텍처는 운용자 요청이 항공기의 안전 경계를 우회하지 않고 필요한 권한확인과 탑재 검증 과정을 통과하는지 확인한다.

시나리오 기반 시험(Scenario-Based Testing)은 독립적인 인터페이스보다 완전한 운용 절차를 평가한다. 대표적인 시험은 전원 인가와 내장시험(Built-In Test)으로 시작하여 임무 입력, 화물 검증, 이륙, 상승, 순항, 배송, 복귀, 착륙 및 시스템 종료까지 이어질 수 있다. 각 비행 단계에서는 하위 시스템 사이에 서로 다른 상호작용이 발생하므로 특정 상태 전환이나 항공기 상태 조합에서만 나타나는 통합 결함을 탐지할 수 있다.

고장 주입(Fault Injection)은 통합시험 아키텍처의 핵심 기능이다. 시험에서는 센서 바이어스, 메시지 손실, 지연된 텔레메트리, 프로세서 재설정, 통신 상실, 배터리 성능 저하, 추진 시스템 고장, 화물 잠금상태 불일치 또는 잘못된 구성 데이터를 의도적으로 발생시킬 수 있다. 제어된 고장 주입을 통해 실제 비행 항공기에서 위험한 조건을 의도적으로 발생시키지 않고도 고장탐지, 격리, 경고, 재구성 및 복구 동작을 검증할 수 있다.

단일 고장(Single Fault)만으로는 고장 허용 UAV의 특성을 충분히 평가할 수 없다. 시스템이 이미 성능저하 상태(Degraded State)에 진입한 이후 발생하는 순차적 또는 복합 고장도 시험해야 한다. 센서 채널이 격리된 이후 중복 컴퓨터가 고장날 수 있으며, 에너지 고장으로 회항이 시작된 이후 통신이 상실될 수도 있다. 이러한 조합을 통해 성능이 저하된 구성에서도 내부적으로 일관된 안전 및 임무 동작을 유지하는지 검증한다.

인터페이스 검증(Interface Verification)은 단순한 메시지 문법 이상의 요소를 평가한다. 데이터 단위, 좌표계, 부호 규칙, 스케일링, 유효 범위, 타임스탬프, 순서 카운터, 업데이트 주기, 타임아웃 동작 및 고장 의미가 송신 시스템과 수신 시스템 사이에서 일치해야 한다. 개별적으로는 정상인 구성요소들이 동일한 데이터를 서로 다르게 해석할 때 많은 통합 고장이 발생하므로 인터페이스 계약(Interface Contract)의 검증은 시스템 시험의 핵심 요소가 된다.

구성관리(Configuration Control)는 시험 결과가 정확하게 정의된 항공기 상태에 대응하도록 보장한다. 소프트웨어 빌드, 펌웨어 버전, 보정 파일, 네트워크 정의, 항공기 파라미터, 탑재화물 구성 및 시뮬레이션 모델은 고유하게 식별할 수 있어야 한다. 시험 구성을 자동으로 기록하면 고장을 재현할 수 있으며 서로 다른 하드웨어 또는 소프트웨어 기준선(Baseline)에서 생성된 시험 결과가 잘못 비교되는 것을 방지할 수 있다.

시험 계측(Test Instrumentation)은 시스템 동작을 크게 변화시키지 않으면서 이를 관측할 수 있어야 한다. 로그는 제어 상태, 센서 추정값, 통신 트래픽, 전력 상태, 임무 상태 전환, 화물 이벤트, 고장 플래그 및 운용자 동작을 기록할 수 있다. 공통 시간 기준(Common Time Reference)을 사용하면 분산된 컴퓨터에서 발생한 이벤트를 시간 순서대로 재구성할 수 있다. 고속 데이터는 계측 자체가 프로세서나 통신 네트워크를 과부하시키지 않도록 신중하게 선택해야 한다.

자동화된 시험 실행(Automated Test Execution)은 시험 범위와 반복성을 향상시킨다. 시험 스크립트는 초기 조건을 구성하고 시뮬레이션을 시작하며 명령을 발행하고 고장을 주입하고 예상 응답을 감시하며 측정 결과를 합격 기준과 비교할 수 있다. 자동화는 비행 소프트웨어, 인터페이스 또는 구성 데이터 변경 이후 수백 또는 수천 개의 기존 검증 시나리오를 다시 실행해야 하는 회귀시험에서 특히 중요하다.

합격 기준(Acceptance Criteria)은 시험 실행 전에 정의되어야 한다. 요구사항에는 허용 추종오차, 응답시간, 통신 지연시간, 고장탐지 시간, 에너지 예비량, 상태 전환 동작 또는 복구 성능이 포함될 수 있다. 따라서 합격 또는 불합격 판단은 주관적인 관찰이 아니라 측정 가능한 증거를 기반으로 해야 한다. 허용오차가 필요한 경우 해당 허용범위와 시스템 수준 안전 및 성능 요구사항 사이의 추적성을 유지해야 한다.

요구사항 추적성(Requirements Traceability)은 각 통합시험을 검증하려는 시스템 동작과 연결한다. 시험 사례(Test Case)는 적용되는 요구사항, 초기 조건, 장비 구성, 실행 절차, 예상 결과 및 수집된 증거를 식별한다. 고장이 발생하면 엔지니어는 어떤 요구사항이 영향을 받았는지와 문제가 소프트웨어, 하드웨어, 인터페이스, 구성 데이터, 시뮬레이션 환경 또는 요구사항 자체에서 발생했는지를 판단할 수 있다.

통합된 소프트웨어나 하드웨어가 변경될 때마다 회귀시험(Regression Testing)이 필요하다. 항법, 통신, 전력관리 또는 화물 로직의 변경은 공유 인터페이스를 통해 수정된 하위 시스템 외부의 기능에도 영향을 줄 수 있다. 따라서 통제된 회귀시험 제품군은 변경사항과 직접 관련된 시험뿐만 아니라 보다 광범위한 종단 간 시나리오도 포함한다. 이를 통해 국부적인 수정이 다른 영역에서 예상하지 못한 항공기 수준 동작을 발생시키는 위험을 줄인다.

지상 통합시험(Ground Integration Testing)은 실제 비행에 앞서 항공기에 설치된 장비를 연결함으로써 실험실 시뮬레이션을 넘어선 검증을 수행한다. 전력분배, 항공전자 네트워크, 센서, 구동기, 추진 인터페이스, 화물장비, 안테나 및 GCS 링크를 실제 기체 구성에서 평가할 수 있다. 지상시험은 배선 오류, 전자기 간섭, 진동 민감성, 열적 동작 및 물리적 인터페이스 문제와 같은 실제 설치환경 특유의 영향을 발견할 수 있다.

비행시험(Flight Testing)은 하위 수준 시험환경에서 통합 시스템이 예측 가능하게 동작한다는 충분한 근거가 확보된 이후에만 도입된다. 초기 비행은 일반적으로 제한된 비행영역(Constrained Envelope)과 신중하게 선택된 시나리오에서 수행한 후 점차 더 넓은 자율 운용으로 확장한다. 시험 범위를 확대하는 과정에서도 핵심 변수를 감시하고 안전하지 않은 시나리오를 종료하며 실제 비행 동작을 이전의 시뮬레이션 및 HIL 결과와 비교할 수 있는 능력을 유지해야 한다.

시험 후 분석(Post-Test Analysis)은 탑재 로그, GCS 기록, 시뮬레이터 데이터, 네트워크 캡처 및 계측 측정값을 하나의 동기화된 기록으로 결합한다. 엔지니어는 실제 동작을 예상된 상태 전환 및 정량적인 합격 기준과 비교한다. 예상하지 못한 결과는 분류, 조사, 수정 및 재시험 과정을 거친다. 이러한 증거를 지속적으로 관리하면 UAV 아키텍처가 구성요소 시험에서 통합 비행 운용까지 어떻게 성숙했는지를 보여주는 검증 이력(Verification History)을 구축할 수 있다.

성숙한 시스템 통합시험 아키텍처(System Integration Test Architecture)는 소프트웨어 모델에서 완전한 화물 UAV까지 이어지는 연속적인 검증 경로를 구축한다. SIL, HIL, 네트워크 및 전력 시뮬레이션, 화물 시스템 통합, 고장 주입, 지상시험 및 비행시험은 물리적 현실성이 점진적으로 증가하는 단계에서 상호 보완적인 검증 근거를 제공한다. 이를 통해 자율비행, 임무, 에너지, 센싱, 화물, 통신 및 안전 기능이 정상, 성능저하 및 비상 조건에서 예측 가능하게 협력하는지를 종합적으로 검증할 수 있다.
