**Volume 23. Cargo UAV Autonomy and Flight AI**


# Chapter 11. 5t and 10t UAV SW Architecture

##  

## 11.01. 5t 10t UAV Design Drivers and SW Differences

![](images/image1.png){width="7.268055555555556in" height="7.268055555555556in"}

The software architecture of 5-ton and 10-ton cargo UAVs is driven by substantially different operational assumptions from those used for small unmanned aircraft. Vehicle mass, payload capacity, propulsion energy, flight duration, operating altitude, and kinetic energy increase the consequences of software or hardware failures. The architecture must therefore emphasize deterministic control, fault containment, redundancy, continuous health monitoring, and predictable degraded operation.

A 5-ton cargo UAV can typically be treated as a heavy autonomous aircraft whose software coordinates flight control, navigation, propulsion, payload management, communication, and mission execution through clearly separated functional domains. Although autonomy may reduce dependence on a human pilot, the flight-critical software must remain isolated from computationally intensive perception and AI functions. This separation prevents failures or timing overruns in high-level autonomy from propagating into stabilization and vehicle-control loops.

The 10-ton class introduces a further increase in system complexity because propulsion, power distribution, actuators, communication networks, and flight computers may contain more replicated components. Software must manage these distributed resources as a coordinated aircraft rather than as independent subsystems. Redundancy management consequently becomes a primary architectural function responsible for determining component validity, selecting healthy resources, detecting disagreement, and performing controlled reconfiguration without destabilizing the vehicle.

Vehicle scale strongly influences the timing architecture. Inner-loop stabilization may require deterministic execution at high frequencies, while navigation, trajectory generation, mission planning, and fleet coordination operate at progressively slower rates. The software should therefore use a hierarchical timing model in which each function has defined execution periods, latency budgets, jitter limits, and data-age requirements. Time synchronization across distributed computers becomes increasingly important as the number of sensors and controllers grows.

A major difference between the two vehicle classes appears in computational partitioning. A 5-ton UAV may use several redundant flight computers combined with separate mission and perception computers. A 10-ton platform can require a more explicitly distributed computing architecture containing multiple flight-control channels, propulsion controllers, vehicle-management computers, safety monitors, and autonomy processors. Communication between these nodes must use well-defined interfaces so that individual computing elements can be replaced without redesigning the complete software stack.

Flight-control authority must remain clearly bounded regardless of vehicle size. High-level autonomy may request a trajectory, velocity, destination, or mission action, but certified or safety-critical control functions should validate these commands before they reach actuators. Command envelopes can constrain airspeed, attitude, acceleration, load factor, altitude, thrust, and actuator demand. This architecture allows advanced AI capabilities to contribute to mission performance without granting them unrestricted control over safety-critical vehicle dynamics.

Sensor architecture also changes with increasing aircraft size. Both platforms may integrate GNSS, inertial measurement units, radar altimeters, air-data sensors, cameras, LiDAR, weather sensors, and propulsion feedback, but the 10-ton vehicle may require greater sensor diversity and physical separation. Software must track sensor provenance, synchronization, calibration status, confidence, and failure state so that navigation and control functions understand not only the estimated aircraft state but also the reliability of the observations supporting it.

Fault detection, isolation, and recovery are fundamental design drivers. A single sensor anomaly should not immediately cause mission termination, while correlated failures must not be mistaken for independent events. The software therefore needs consistency checks, analytical redundancy, voting mechanisms, timeout monitoring, range validation, and model-based residuals. When confidence decreases, the vehicle can transition through predefined degraded modes while preserving stabilization, navigation, communication, and safe landing capability.

Propulsion management creates another important architectural distinction. Heavy cargo UAVs may employ distributed electric propulsion, hybrid-electric systems, turbine engines, or other multi-engine configurations. The software must coordinate thrust commands with energy availability, thermal conditions, engine health, and aerodynamic control requirements. On a 10-ton vehicle, propulsion failures can produce larger asymmetric forces, making rapid fault identification and coordinated control allocation particularly important for maintaining controllability.

Power and energy management must also become part of the aircraft-level software architecture. Computing systems, actuators, sensors, communication equipment, payload mechanisms, and propulsion components compete for limited electrical resources. Software should monitor generation, storage, distribution, temperature, and consumption while distinguishing essential from nonessential loads. During abnormal conditions, controlled load shedding can preserve flight-critical functions and provide sufficient energy for diversion, emergency landing, or recovery operations.

The mission-management layer must account for the operational consequences of carrying large payloads. Loading condition, center of gravity, gross mass, fuel or battery state, environmental conditions, and landing-site characteristics can significantly alter vehicle performance. Mission software should therefore evaluate feasibility before departure and continuously reassess margins during flight. Changes in payload state or energy reserves can trigger trajectory replanning while remaining inside limits established by the flight-safety layer.

Cargo handling introduces interfaces that are uncommon in smaller UAV architectures. Door mechanisms, winches, loading systems, locking devices, payload sensors, and ground equipment may interact with mission software. These functions should be governed by interlocks that prevent unsafe actions during flight phases where payload movement could compromise stability. The flight-control system should receive relevant payload state information so that mass-property changes can be incorporated into estimation, control allocation, and performance calculations.

Communication requirements expand as the vehicle becomes more operationally consequential. The architecture may support command-and-control links, telemetry, payload communication, maintenance channels, navigation corrections, and traffic-management interfaces. These channels should be logically separated according to criticality and security requirements. Loss of a noncritical data link must not disturb flight control, while loss of command-and-control communication should activate a deterministic contingency behavior defined before the mission.

Network design becomes especially important for the 10-ton platform. Multiple computers exchanging high-rate state, actuator, propulsion, and health information can create congestion or nondeterministic delays if traffic is unmanaged. Critical messages should therefore receive bounded latency and predictable priority. Network redundancy should avoid common failure paths, and software should detect link degradation before communication is completely lost. Timestamped data allows receiving functions to reject stale information and maintain temporal consistency.

Cybersecurity must be integrated without violating real-time requirements. Authentication, secure boot, signed software, encrypted external communication, access control, and protected maintenance interfaces reduce the possibility of unauthorized modification or command injection. At the same time, security mechanisms must not introduce unpredictable delays into flight-critical paths. The architecture should separate externally exposed services from control networks and enforce carefully defined gateways between operational, maintenance, payload, and safety domains.

Software update strategy is another significant design driver because heavy UAVs can remain in service for many years. Flight-critical software, autonomy models, maps, perception networks, and mission applications evolve at different rates and should not require simultaneous replacement. Modular deployment allows individual components to be updated while preserving verified interfaces. Version compatibility, rollback capability, configuration control, cryptographic verification, and recorded software provenance are necessary for maintaining an auditable operational baseline.

Verification requirements grow with aircraft size and operational risk. Software behavior should be evaluated through unit testing, integration testing, software-in-the-loop simulation, hardware-in-the-loop testing, fault injection, Monte Carlo simulation, and progressively representative flight testing. The 10-ton architecture particularly benefits from digital models capable of reproducing propulsion failures, actuator faults, sensor corruption, communication delays, environmental disturbances, and combinations of failures that would be unsafe to create directly in flight.

Simulation must represent differences between nominal performance and abnormal operation rather than merely demonstrating successful missions. Test environments should evaluate timing overload, stale sensor data, processor resets, network partitions, power interruptions, actuator saturation, navigation degradation, and unexpected payload conditions. Requirements can then be traced to measurable evidence showing that the software reaches a defined safe state. This approach converts redundancy from a hardware feature into verified system behavior.

Maintenance and fleet operation also influence software architecture. Heavy cargo UAVs generate extensive health, event, and performance data that can support condition-based maintenance and reliability analysis. Onboard software should record synchronized diagnostic information without interfering with real-time control. Ground systems can analyze trends in propulsion efficiency, actuator response, sensor drift, thermal behavior, and communication quality, allowing maintenance decisions to be based on measured degradation rather than fixed schedules alone.

Despite their differences, 5-ton and 10-ton UAVs should share a common architectural philosophy wherever practical. Standardized interfaces, reusable safety services, common health-monitoring frameworks, consistent mission abstractions, and portable autonomy components reduce development cost and simplify verification. Vehicle-specific differences can then be concentrated in configuration, control laws, propulsion management, redundancy policies, performance envelopes, and hardware abstraction layers instead of producing entirely independent software ecosystems.

The central design distinction is therefore not simply that a 10-ton UAV needs more computing power than a 5-ton UAV. Increasing scale changes the consequences of failure, the number of interacting subsystems, the complexity of redundancy, and the rigor required for deterministic recovery. A successful architecture treats autonomy, flight control, safety, networking, propulsion, energy, payload, and maintenance as coordinated but fault-contained domains, enabling both aircraft classes to evolve toward dependable large-scale autonomous cargo operations.

5톤 및 10톤 화물 무인항공기(Cargo UAV)의 소프트웨어 아키텍처(Software Architecture)는 소형 무인항공기에 적용되는 설계 가정과 상당히 다른 운용 조건에 의해 결정된다. 기체 중량, 탑재중량(Payload Capacity), 추진 에너지(Propulsion Energy), 비행 지속시간, 운용 고도 및 운동에너지(Kinetic Energy)가 증가할수록 소프트웨어 또는 하드웨어 고장이 초래하는 결과도 커진다. 따라서 아키텍처는 결정론적 제어(Deterministic Control), 고장 격리(Fault Containment), 이중화(Redundancy), 지속적인 상태 감시(Health Monitoring), 예측 가능한 성능 저하 운용(Degraded Operation)을 중점적으로 고려해야 한다.

5톤 화물 무인항공기(Cargo UAV)는 비행 제어(Flight Control), 항법(Navigation), 추진(Propulsion), 탑재물 관리(Payload Management), 통신(Communication), 임무 수행(Mission Execution)을 명확하게 분리된 기능 도메인(Functional Domain)을 통해 조정하는 대형 자율 항공기(Heavy Autonomous Aircraft)로 볼 수 있다. 자율화(Autonomy)를 통해 인간 조종사에 대한 의존도를 줄일 수 있지만, 비행 필수 소프트웨어(Flight-Critical Software)는 계산 집약적인 인지(Perception) 및 인공지능(AI) 기능과 분리되어야 한다. 이러한 분리는 상위 자율 기능의 고장이나 실행시간 초과가 안정화 및 기체 제어 루프(Control Loop)로 전파되는 것을 방지한다.

10톤급에서는 추진 시스템, 전력 분배(Power Distribution), 액추에이터(Actuator), 통신 네트워크 및 비행 컴퓨터에 더욱 많은 복제 구성요소가 포함될 수 있기 때문에 시스템 복잡성이 한 단계 더 증가한다. 소프트웨어는 이러한 분산 자원(Distributed Resources)을 독립적인 하위 시스템이 아니라 하나의 통합된 항공기로 관리해야 한다. 따라서 이중화 관리(Redundancy Management)는 구성요소의 유효성 판단, 정상 자원 선택, 불일치 탐지, 기체의 안정성을 훼손하지 않는 제어된 재구성(Controlled Reconfiguration)을 담당하는 핵심 아키텍처 기능이 된다.

기체 규모는 타이밍 아키텍처(Timing Architecture)에 큰 영향을 미친다. 내부 루프 안정화(Inner-Loop Stabilization)는 높은 주파수에서 결정론적으로 실행되어야 하는 반면, 항법, 궤적 생성(Trajectory Generation), 임무 계획(Mission Planning), 편대 조정(Fleet Coordination)은 점진적으로 낮은 주파수에서 동작한다. 따라서 각 기능별로 실행 주기, 지연시간 예산(Latency Budget), 지터 한계(Jitter Limit), 데이터 유효시간(Data-Age Requirement)을 정의하는 계층형 타이밍 모델(Hierarchical Timing Model)이 필요하다. 센서와 제어기의 수가 증가할수록 분산 컴퓨터 간 시간 동기화(Time Synchronization)의 중요성도 더욱 커진다.

두 기체 등급의 주요 차이 중 하나는 컴퓨팅 파티셔닝(Computational Partitioning)에서 나타난다. 5톤 무인항공기는 여러 개의 이중화 비행 컴퓨터와 별도의 임무 및 인지 컴퓨터를 결합할 수 있다. 반면 10톤 플랫폼은 다중 비행 제어 채널(Flight-Control Channel), 추진 제어기(Propulsion Controller), 기체 관리 컴퓨터(Vehicle-Management Computer), 안전 감시기(Safety Monitor), 자율 컴퓨터(Autonomy Processor)를 포함하는 보다 명확한 분산 컴퓨팅 아키텍처(Distributed Computing Architecture)가 필요할 수 있다. 각 노드 사이에는 전체 소프트웨어 스택을 재설계하지 않고 개별 컴퓨팅 요소를 교체할 수 있도록 명확한 인터페이스가 정의되어야 한다.

기체 크기에 관계없이 비행 제어 권한(Flight-Control Authority)은 명확한 경계 안에서 유지되어야 한다. 상위 자율 시스템은 궤적, 속도, 목적지 또는 임무 행동을 요청할 수 있지만, 인증되었거나 안전 필수적인 제어 기능은 이러한 명령이 액추에이터에 전달되기 전에 검증해야 한다. 명령 엔벌로프(Command Envelope)를 통해 대기속도, 자세, 가속도, 하중계수(Load Factor), 고도, 추력 및 액추에이터 요구량을 제한할 수 있다. 이를 통해 첨단 인공지능 기능이 임무 성능 향상에 기여하면서도 안전 필수 기체 동역학에 무제한적인 제어 권한을 갖지 않도록 할 수 있다.

기체 규모가 증가하면 센서 아키텍처(Sensor Architecture)도 변화한다. 두 플랫폼 모두 위성항법시스템(GNSS), 관성측정장치(IMU), 레이더 고도계(Radar Altimeter), 대기자료 센서(Air-Data Sensor), 카메라, 라이다(LiDAR), 기상 센서, 추진 시스템 피드백을 통합할 수 있지만, 10톤 기체에는 더 높은 센서 다양성과 물리적 분리가 요구될 수 있다. 소프트웨어는 센서 출처(Sensor Provenance), 시간 동기화, 보정 상태(Calibration Status), 신뢰도, 고장 상태를 추적하여 항법 및 제어 기능이 추정된 기체 상태뿐 아니라 이를 뒷받침하는 관측 정보의 신뢰성까지 판단할 수 있도록 해야 한다.

고장 탐지·격리·복구(Fault Detection, Isolation, and Recovery)는 핵심적인 설계 동인(Design Driver)이다. 하나의 센서 이상이 발생했다고 해서 즉시 임무를 종료해서는 안 되며, 동시에 상관된 고장(Correlated Failure)을 서로 독립적인 사건으로 잘못 판단해서도 안 된다. 따라서 소프트웨어에는 일관성 검사(Consistency Check), 분석적 이중화(Analytical Redundancy), 투표 메커니즘(Voting Mechanism), 타임아웃 감시(Timeout Monitoring), 범위 검증(Range Validation), 모델 기반 잔차(Model-Based Residual)가 필요하다. 신뢰도가 감소하면 기체는 사전에 정의된 성능 저하 모드(Degraded Mode)로 전환하면서 안정화, 항법, 통신 및 안전 착륙 기능을 유지할 수 있어야 한다.

추진 관리(Propulsion Management)는 또 다른 중요한 아키텍처 차이를 만든다. 대형 화물 무인항공기는 분산 전기 추진(Distributed Electric Propulsion), 하이브리드 전기 시스템(Hybrid-Electric System), 터빈 엔진 또는 기타 다중 엔진 구성을 사용할 수 있다. 소프트웨어는 추력 명령을 가용 에너지, 열 상태(Thermal Condition), 엔진 건전성 및 공력 제어 요구조건과 연계하여 조정해야 한다. 특히 10톤 기체에서는 추진계 고장이 더 큰 비대칭 힘(Asymmetric Force)을 발생시킬 수 있으므로, 조종성(Controllability)을 유지하기 위한 신속한 고장 식별과 통합 제어 할당(Control Allocation)이 중요하다.

전력 및 에너지 관리(Power and Energy Management) 역시 항공기 수준의 소프트웨어 아키텍처에 포함되어야 한다. 컴퓨팅 시스템, 액추에이터, 센서, 통신 장비, 탑재물 장치 및 추진 구성요소는 제한된 전기 자원을 공유한다. 소프트웨어는 발전, 저장, 분배, 온도 및 소비량을 감시하면서 필수 부하(Essential Load)와 비필수 부하(Nonessential Load)를 구분해야 한다. 비정상 상황에서는 제어된 부하 차단(Load Shedding)을 통해 비행 필수 기능을 유지하고 회항(Diversion), 비상 착륙(Emergency Landing) 또는 복구에 필요한 충분한 에너지를 확보할 수 있다.

임무 관리 계층(Mission-Management Layer)은 대형 탑재물을 운송함으로써 발생하는 운용상의 영향을 고려해야 한다. 적재 상태, 무게중심(Center of Gravity), 총중량(Gross Mass), 연료 또는 배터리 상태, 환경 조건 및 착륙장 특성은 기체 성능을 크게 변화시킬 수 있다. 따라서 임무 소프트웨어는 출발 전에 임무 실행 가능성(Feasibility)을 평가하고 비행 중에도 지속적으로 안전 여유(Margin)를 재평가해야 한다. 탑재 상태 또는 에너지 잔량의 변화가 발생하면 비행 안전 계층(Flight-Safety Layer)이 설정한 제한 범위 안에서 궤적 재계획(Trajectory Replanning)을 수행할 수 있다.

화물 취급(Cargo Handling)은 소형 무인항공기 아키텍처에서는 일반적이지 않은 새로운 인터페이스를 도입한다. 도어 메커니즘, 윈치(Winch), 적재 시스템, 잠금 장치, 탑재물 센서 및 지상 장비가 임무 소프트웨어와 상호작용할 수 있다. 이러한 기능은 탑재물 이동으로 안정성이 손상될 수 있는 비행 단계에서 위험한 동작이 발생하지 않도록 인터록(Interlock)을 통해 제어되어야 한다. 비행 제어 시스템은 탑재물 상태 정보를 전달받아 질량 특성(Mass Property)의 변화를 상태 추정, 제어 할당 및 성능 계산에 반영해야 한다.

기체의 운용 중요도가 증가할수록 통신 요구조건도 확대된다. 아키텍처는 지휘통제 링크(Command-and-Control Link), 텔레메트리(Telemetry), 탑재물 통신, 정비 채널(Maintenance Channel), 항법 보정 정보 및 교통관리 인터페이스를 지원할 수 있다. 이러한 채널은 중요도와 보안 요구사항에 따라 논리적으로 분리되어야 한다. 비필수 데이터 링크가 손실되어도 비행 제어에는 영향을 주지 않아야 하며, 지휘통제 통신이 상실되면 임무 수행 전에 정의된 결정론적 비상 동작(Deterministic Contingency Behavior)이 활성화되어야 한다.

네트워크 설계(Network Design)는 특히 10톤 플랫폼에서 중요하다. 여러 컴퓨터가 높은 전송률로 상태, 액추에이터, 추진 및 건전성 정보를 교환하면 트래픽을 적절하게 관리하지 않을 경우 네트워크 혼잡이나 비결정론적 지연(Non-Deterministic Delay)이 발생할 수 있다. 따라서 중요 메시지는 제한된 지연시간(Bounded Latency)과 예측 가능한 우선순위를 보장받아야 한다. 네트워크 이중화(Network Redundancy)는 공통 고장 경로(Common Failure Path)를 방지해야 하며, 소프트웨어는 통신이 완전히 손실되기 전에 링크 성능 저하를 탐지해야 한다. 타임스탬프 데이터(Timestamped Data)를 사용하면 수신 기능이 오래된 정보를 거부하고 시간적 일관성(Temporal Consistency)을 유지할 수 있다.

사이버보안(Cybersecurity)은 실시간 요구조건을 침해하지 않는 방식으로 통합되어야 한다. 인증(Authentication), 보안 부팅(Secure Boot), 서명된 소프트웨어(Signed Software), 암호화된 외부 통신, 접근 제어(Access Control), 보호된 정비 인터페이스를 통해 무단 변경이나 명령 주입(Command Injection)의 가능성을 줄일 수 있다. 동시에 보안 메커니즘이 비행 필수 경로에 예측할 수 없는 지연을 발생시켜서는 안 된다. 아키텍처는 외부에 노출되는 서비스를 제어 네트워크와 분리하고 운용, 정비, 탑재물 및 안전 도메인 사이에 명확하게 정의된 게이트웨이(Gateway)를 적용해야 한다.

대형 무인항공기는 장기간 운용될 가능성이 높기 때문에 소프트웨어 업데이트 전략(Software Update Strategy)도 중요한 설계 동인이 된다. 비행 필수 소프트웨어, 자율 모델(Autonomy Model), 지도, 인지 신경망(Perception Network), 임무 애플리케이션은 서로 다른 속도로 발전하므로 동시에 교체하도록 설계해서는 안 된다. 모듈형 배포(Modular Deployment)를 사용하면 검증된 인터페이스를 유지하면서 개별 구성요소를 업데이트할 수 있다. 감사 가능한 운용 기준선(Auditable Operational Baseline)을 유지하려면 버전 호환성, 롤백(Rollback), 형상 관리(Configuration Control), 암호학적 검증(Cryptographic Verification), 소프트웨어 출처 기록이 필요하다.

기체 규모와 운용 위험이 증가하면 검증 요구사항(Verification Requirement)도 강화된다. 소프트웨어 동작은 단위 시험(Unit Testing), 통합 시험(Integration Testing), 소프트웨어 인더루프 시뮬레이션(Software-in-the-Loop Simulation), 하드웨어 인더루프 시험(Hardware-in-the-Loop Testing), 고장 주입(Fault Injection), 몬테카를로 시뮬레이션(Monte Carlo Simulation), 단계적으로 실제 환경에 가까워지는 비행 시험을 통해 평가되어야 한다. 특히 10톤 아키텍처에서는 추진 고장, 액추에이터 고장, 센서 데이터 손상, 통신 지연, 환경 외란(Environmental Disturbance) 및 실제 비행에서 직접 구현하기 위험한 복합 고장 상황을 재현할 수 있는 디지털 모델(Digital Model)이 중요하다.

시뮬레이션(Simulation)은 정상적인 임무 성공만 보여주는 것이 아니라 정상 성능과 비정상 운용 사이의 차이를 재현해야 한다. 시험 환경에서는 타이밍 과부하(Timing Overload), 오래된 센서 데이터, 프로세서 재시작, 네트워크 분할(Network Partition), 전원 중단, 액추에이터 포화(Actuator Saturation), 항법 성능 저하 및 예상하지 못한 탑재 상태 등을 평가해야 한다. 이후 각 요구조건을 측정 가능한 검증 증거(Verification Evidence)와 연결하여 소프트웨어가 정의된 안전 상태(Safe State)에 도달하는지를 입증할 수 있다. 이러한 접근은 이중화를 단순한 하드웨어 특성이 아니라 검증된 시스템 동작으로 전환한다.

정비 및 편대 운용(Fleet Operation) 역시 소프트웨어 아키텍처에 영향을 준다. 대형 화물 무인항공기는 상태, 이벤트 및 성능에 관한 방대한 데이터를 생성하며, 이는 상태 기반 정비(Condition-Based Maintenance)와 신뢰성 분석(Reliability Analysis)에 활용할 수 있다. 탑재 소프트웨어는 실시간 제어에 영향을 주지 않으면서 동기화된 진단 정보를 기록해야 한다. 지상 시스템은 추진 효율, 액추에이터 응답, 센서 드리프트(Sensor Drift), 열 거동 및 통신 품질의 추세를 분석하여 고정된 정비 주기 대신 실제 측정된 성능 저하에 기반한 정비 의사결정을 지원할 수 있다.

이러한 차이에도 불구하고 5톤 및 10톤 무인항공기는 가능한 범위에서 공통된 아키텍처 철학을 공유하는 것이 바람직하다. 표준화된 인터페이스, 재사용 가능한 안전 서비스(Safety Service), 공통 상태 감시 프레임워크(Health-Monitoring Framework), 일관된 임무 추상화(Mission Abstraction), 이식 가능한 자율 구성요소(Portable Autonomy Component)는 개발 비용을 줄이고 검증을 단순화한다. 이에 따라 기체별 차이는 완전히 독립적인 소프트웨어 생태계를 구축하는 대신 형상 설정(Configuration), 제어 법칙(Control Law), 추진 관리, 이중화 정책, 성능 엔벌로프(Performance Envelope), 하드웨어 추상화 계층(Hardware Abstraction Layer)에 집중시킬 수 있다.

따라서 핵심적인 설계 차이는 단순히 10톤 무인항공기가 5톤 무인항공기보다 더 높은 컴퓨팅 성능을 필요로 한다는 데 있지 않다. 기체 규모의 증가는 고장의 결과, 상호작용하는 하위 시스템의 수, 이중화 복잡성, 결정론적 복구(Deterministic Recovery)에 요구되는 엄격성을 함께 증가시킨다. 성공적인 아키텍처는 자율화, 비행 제어, 안전, 네트워킹, 추진, 에너지, 탑재물 및 정비 기능을 서로 조정되면서도 고장이 격리되는 도메인으로 구성함으로써 5톤과 10톤 기체 모두가 신뢰성 높은 대형 자율 화물 운송(Large-Scale Autonomous Cargo Operation)으로 발전할 수 있도록 해야 한다.

##  

## 11.02. 5t UAV Flight Control Law Heavy Lift [w/Code]

![](images/image2.png){width="7.268055555555556in" height="7.268055555555556in"}

A 5-ton heavy-lift UAV requires a flight control law designed around high inertia, large payload variation, strong propulsion coupling, and significantly greater consequences of unstable motion than those of small UAVs. The controller must provide predictable handling throughout takeoff, climb, cruise, approach, and landing while maintaining sufficient stability margins under changing mass, center-of-gravity, aerodynamic, and environmental conditions.

The control architecture is typically organized as nested loops with different bandwidths. Fast inner loops regulate angular rates and attitude, while slower outer loops regulate velocity, altitude, position, and trajectory. This hierarchy prevents mission-level commands from directly exciting rapid vehicle dynamics. Each loop requires explicitly allocated timing, latency, and stability margins so that distributed computation and communication delays do not degrade closed-loop performance.

Heavy-lift dynamics create particular challenges because a 5-ton aircraft cannot rapidly correct disturbances using the aggressive maneuvers available to small multirotors. Large inertia reduces angular acceleration for a given control moment, while structural and propulsion constraints limit how quickly thrust can change. The control law must therefore anticipate vehicle response, avoid excessive command transients, and maintain adequate control authority before disturbances develop into large attitude or trajectory errors.

Accurate state estimation is fundamental to control performance. The flight controller depends on synchronized measurements from inertial measurement units, GNSS, air-data sensors, radar or laser altimeters, propulsion sensors, and other navigation sources. Sensor fusion produces estimates of position, velocity, attitude, angular rate, altitude, and acceleration together with confidence information that allows the controller to adapt its behavior when individual measurements become degraded or unavailable.

Payload variation introduces large changes in mass properties. A vehicle may operate nearly empty on one mission and close to maximum takeoff mass on another, producing different acceleration, climb, braking, and maneuvering characteristics. The controller should therefore use validated mass and center-of-gravity information rather than assuming a fixed vehicle model. Gain scheduling or model-based adaptation can maintain comparable handling characteristics across the approved loading envelope.

Center-of-gravity displacement is particularly important for cargo operations because loading errors or payload movement can alter pitch and roll moments. The flight-control system should monitor estimated mass distribution and compare actuator demand against expected behavior. When the center of gravity approaches an approved limit, the controller can reduce maneuver aggressiveness or restrict portions of the flight envelope, preventing control saturation from becoming the first indication of an unsafe loading condition.

For a multirotor or distributed-propulsion heavy UAV, the flight-control law does not directly command individual motors from high-level trajectory requests. Instead, it calculates the total forces and moments required to achieve the desired vehicle motion. A control-allocation function then distributes these demands among available propulsion units and aerodynamic actuators while respecting thrust, rate, thermal, electrical, and mechanical constraints.

Control allocation becomes especially valuable when propulsion units differ in capability. Motor temperature, battery condition, generator output, rotor efficiency, or an emerging fault can reduce available thrust on one channel. Rather than assuming equal actuator performance, the allocator can use current capability estimates to redistribute demand among healthy channels. This allows the aircraft-level controller to preserve required forces and moments without embedding detailed actuator logic inside every control loop.

Actuator saturation must be handled explicitly because a heavy vehicle may encounter conditions where commanded moments cannot physically be generated. Integrator windup, repeated saturation, or competing control objectives can destabilize an otherwise well-tuned controller. Anti-windup mechanisms, command limiting, priority-based allocation, and achievable-set calculations can ensure that stabilization receives higher priority than trajectory tracking when available control authority becomes limited.

Propulsion dynamics also influence controller design. Large rotors, engines, generators, or electric propulsion systems may have meaningful response delays and different rates for increasing and decreasing thrust. The controller should incorporate these dynamics instead of treating thrust as instantaneous. Feedforward terms, dynamic compensation, and carefully shaped commands can reduce altitude deviations and attitude transients during rapid changes in collective thrust or forward-flight demand.

Wind disturbances represent a major external challenge. Gusts, crosswinds, turbulence, and rotor interactions can generate substantial forces over the large projected area of a cargo UAV. Disturbance observers or robust feedback techniques can estimate and reject persistent external forces while avoiding excessive actuator activity. Position-hold performance should be balanced against structural loading and energy consumption rather than attempting to eliminate every small displacement in severe turbulence.

Takeoff control requires careful management of the transition from ground-supported weight to fully airborne flight. Thrust buildup must remain coordinated across propulsion channels while the controller avoids reacting excessively to sensor vibration or ground-contact effects. Ground-state logic, weight-on-support detection, altitude confirmation, and thrust thresholds can prevent premature attitude corrections and provide a deterministic transition into the normal airborne control mode.

Landing introduces another demanding regime because vertical velocity, horizontal drift, attitude, and touchdown loads must be controlled simultaneously. The controller should reduce descent rate as the aircraft approaches the surface while preserving enough authority to reject gusts. Terrain-relative altitude sensing can complement GNSS, and touchdown detection should reliably distinguish actual ground contact from temporary acceleration or sensor anomalies before thrust is reduced.

Emergency control modes must be designed as part of the primary flight-control architecture rather than added after nominal control development. Loss of a propulsion unit, degraded navigation, actuator malfunction, communication failure, or reduced electrical power may require different control objectives. The controller can progressively sacrifice position accuracy, trajectory performance, or mission completion while maintaining attitude stability and controllability as the highest priorities.

Propulsion failure on a heavy multirotor creates both thrust loss and an asymmetric moment. Detection must therefore occur quickly enough for the controller and allocator to reconfigure before attitude errors become excessive. The failed channel should be removed from the available actuator set, remaining thrust should be redistributed, and trajectory demands should be reduced to values achievable with the degraded configuration. Landing may then become the dominant mission objective.

Navigation degradation requires a different form of control reconfiguration. Loss or corruption of GNSS should not affect the basic attitude loops, because stabilization should depend primarily on local inertial measurements. Position-level functions can transition to alternative navigation sources such as visual, LiDAR, radar, or inertial estimates. If absolute position confidence becomes insufficient, the vehicle can abandon precision trajectory tracking while retaining bounded velocity, attitude, and altitude behavior.

Structural flexibility can become relevant at the 5-ton scale because large arms, wings, rotor supports, landing structures, or cargo assemblies may contain vibration modes within the broader control spectrum. A controller that excites these modes can create oscillations even when rigid-body stability appears satisfactory. Notch filtering, command shaping, bandwidth separation, structural-mode identification, and sensor placement should therefore be considered during control-law development and validation.

Flight-envelope protection provides an additional safety layer around normal control laws. Limits on attitude, angular rate, airspeed, descent rate, acceleration, load factor, actuator demand, and energy state can prevent autonomy or operator commands from driving the vehicle into poorly controlled regions. Protection logic should intervene smoothly and predictably, ensuring that constraint enforcement does not itself introduce discontinuities capable of destabilizing the aircraft.

The control law must interact with trajectory generation through a carefully defined contract. The planner provides dynamically reasonable references, while the controller reports tracking capability, available acceleration, actuator margin, and degraded-state restrictions. If the vehicle cannot satisfy a requested trajectory, the correct response is not persistent saturation but negotiation of a feasible reference. This separation improves both stability and modularity across different mission-planning systems.

Deterministic software execution is essential because control performance depends on both mathematical algorithms and implementation timing. Inner-loop tasks should execute at defined rates with bounded jitter, while sensor timestamps must represent the actual measurement time. Delayed or stale information should be detected rather than silently processed as current data. Worst-case execution time and communication latency therefore become part of the practical stability analysis.

Software-in-the-loop and hardware-in-the-loop testing should expose the controller to a broad range of vehicle parameters and disturbances before flight. Tests can vary payload mass, center of gravity, actuator effectiveness, propulsion delay, wind, sensor noise, communication latency, and navigation quality. Monte Carlo campaigns help identify combinations that reduce stability or control margins even when each individual parameter remains within its expected operating range.

Flight-test expansion should proceed from highly constrained conditions toward the full operational envelope. Initial tests verify basic stabilization and actuator direction before progressing through hover, translation, climb, descent, higher speeds, representative payloads, crosswinds, and controlled degraded configurations. Recorded data should be compared with simulation predictions so that model discrepancies are understood before more demanding operating regions are authorized.

Performance assessment should extend beyond simple trajectory error. Relevant measures include stability margins, settling time, overshoot, disturbance rejection, actuator utilization, control-allocation residuals, energy consumption, structural response, and remaining control authority. Monitoring these quantities provides evidence that acceptable tracking performance is not being achieved by operating actuators continuously near saturation or consuming excessive redundancy margins.

Ultimately, the 5-ton heavy-lift flight-control law must convert a physically slow, high-energy, payload-sensitive aircraft into a predictable autonomous platform. Its effectiveness depends on coordinated state estimation, hierarchical feedback, control allocation, envelope protection, fault accommodation, deterministic execution, and rigorous validation. The objective is not maximum maneuverability, but stable and controllable behavior with measurable safety margins across nominal, degraded, and emergency operating conditions.

5톤급 대형 화물 무인항공기(Heavy-Lift UAV)는 높은 관성, 큰 탑재중량 변화, 강한 추진계 결합, 그리고 소형 무인항공기보다 훨씬 큰 불안정 운동의 결과를 고려하여 설계된 비행제어법칙(Flight Control Law)이 필요하다. 제어기는 이륙, 상승, 순항, 접근 및 착륙 전 과정에서 예측 가능한 조종 특성을 제공하면서 변화하는 질량, 무게중심(Center of Gravity), 공력 및 환경 조건에서도 충분한 안정성 여유(Stability Margin)를 유지해야 한다.

제어 아키텍처(Control Architecture)는 일반적으로 서로 다른 대역폭(Bandwidth)을 갖는 중첩 제어 루프(Nested Control Loop)로 구성된다. 빠른 내부 루프(Inner Loop)는 각속도와 자세를 제어하고, 상대적으로 느린 외부 루프(Outer Loop)는 속도, 고도, 위치 및 궤적을 제어한다. 이러한 계층 구조는 임무 수준 명령이 빠른 기체 동역학을 직접 자극하는 것을 방지한다. 각 루프에는 분산 컴퓨팅과 통신 지연이 폐루프 성능을 저하시키지 않도록 명확한 실행 주기, 지연시간 및 안정성 여유가 할당되어야 한다.

대형 화물 기체의 동역학은 소형 멀티로터에서 가능한 공격적인 기동을 5톤급 기체가 빠르게 수행하기 어렵다는 점에서 특별한 문제를 만든다. 큰 관성은 동일한 제어 모멘트에 대해 상대적으로 작은 각가속도를 발생시키며, 구조 및 추진계 제약은 추력을 얼마나 빠르게 변화시킬 수 있는지를 제한한다. 따라서 제어법칙은 기체 응답을 예측하고 과도한 명령 변화를 피하며, 외란이 큰 자세 또는 궤적 오차로 발전하기 전에 충분한 제어 권한(Control Authority)을 유지해야 한다.

정확한 상태 추정(State Estimation)은 제어 성능의 핵심이다. 비행제어기는 관성측정장치(IMU), 위성항법시스템(GNSS), 대기자료 센서(Air-Data Sensor), 레이더 또는 라이다 고도계(Radar or LiDAR Altimeter), 추진계 센서 및 기타 항법 센서로부터 동기화된 측정값을 사용한다. 센서 융합(Sensor Fusion)은 위치, 속도, 자세, 각속도, 고도 및 가속도를 추정하며, 각 측정값의 신뢰도 정보도 함께 제공하여 개별 센서가 성능 저하 또는 고장을 일으켰을 때 제어기가 동작을 조정할 수 있도록 한다.

탑재중량 변화는 질량 특성(Mass Property)에 큰 변화를 발생시킨다. 하나의 임무에서는 거의 빈 상태로 운용되고 다른 임무에서는 최대이륙중량에 가까운 상태로 운용될 수 있으므로 가속, 상승, 감속 및 기동 특성이 달라진다. 따라서 제어기는 고정된 기체 모델을 가정하기보다 검증된 질량 및 무게중심 정보를 사용해야 한다. 이득 스케줄링(Gain Scheduling) 또는 모델 기반 적응(Model-Based Adaptation)을 적용하면 승인된 적재 범위 전체에서 유사한 조종 특성을 유지할 수 있다.

무게중심의 이동은 특히 중요하다. 화물 적재 오류나 탑재물 이동으로 인해 피치 및 롤 모멘트가 변화할 수 있기 때문이다. 비행제어 시스템은 추정된 질량 분포를 감시하고 액추에이터 요구량을 예상되는 동작과 비교해야 한다. 무게중심이 승인된 한계에 가까워지면 제어기는 기동의 공격성을 낮추거나 비행 엔벌로프(Flight Envelope)의 일부를 제한할 수 있으며, 이를 통해 제어 포화(Control Saturation)가 위험한 적재 상태를 처음으로 알려주는 신호가 되는 상황을 방지할 수 있다.

멀티로터 또는 분산 추진(Distributed Propulsion)을 사용하는 대형 무인항공기의 비행제어법칙은 높은 수준의 궤적 명령으로부터 개별 모터를 직접 명령하지 않는다. 대신 원하는 기체 운동을 달성하는 데 필요한 전체 힘과 모멘트를 계산한다. 이후 제어 할당(Control Allocation) 기능이 이러한 요구량을 사용 가능한 추진 장치와 공력 액추에이터에 분배하며, 추력, 변화율, 열, 전력 및 기계적 제약조건을 동시에 고려한다.

제어 할당은 추진 장치별 성능이 서로 다를 때 특히 유용하다. 모터 온도, 배터리 상태, 발전기 출력, 로터 효율 또는 고장 발생에 따라 특정 채널의 가용 추력이 감소할 수 있다. 따라서 모든 액추에이터의 성능이 동일하다고 가정하지 않고 현재의 가용 능력 추정치를 사용하여 요구량을 정상적인 추진 채널에 재분배할 수 있다. 이를 통해 기체 수준 제어기는 각각의 제어 루프에 세부적인 액추에이터 로직을 포함하지 않고도 필요한 힘과 모멘트를 유지할 수 있다.

액추에이터 포화는 대형 기체에서 요구되는 모멘트를 실제로 생성할 수 없는 상황이 발생할 수 있기 때문에 명시적으로 처리되어야 한다. 적분기 와인드업(Integrator Windup), 반복적인 포화 또는 상충하는 제어 목표는 정상적으로 조정된 제어기조차 불안정하게 만들 수 있다. 안티윈드업(Anti-Windup), 명령 제한(Command Limiting), 우선순위 기반 할당(Priority-Based Allocation), 달성 가능 영역 계산(Achievable-Set Calculation)을 적용하면 가용 제어 권한이 제한될 때 궤적 추종보다 안정화 기능에 높은 우선순위를 부여할 수 있다.

추진계 동역학(Propulsion Dynamics)도 제어기 설계에 영향을 미친다. 대형 로터, 엔진, 발전기 또는 전기 추진 시스템은 의미 있는 응답 지연(Response Delay)을 가질 수 있으며 추력을 증가시키는 속도와 감소시키는 속도가 서로 다를 수도 있다. 제어기는 추력이 순간적으로 변화한다고 가정하지 않고 이러한 동역학을 모델에 포함해야 한다. 피드포워드(Feedforward), 동역학 보상(Dynamic Compensation), 적절한 명령 형상화(Command Shaping)를 사용하면 집단 추력(Collective Thrust)이나 전진비행 요구가 급격하게 변화할 때 발생하는 고도 오차와 자세 과도응답을 줄일 수 있다.

바람 외란(Wind Disturbance)은 주요 외부 문제이다. 돌풍, 횡풍, 난류 및 로터 상호작용은 대형 화물 무인항공기의 넓은 투영면적에 상당한 힘을 발생시킬 수 있다. 외란 관측기(Disturbance Observer) 또는 강인 피드백(Robust Feedback) 기법을 사용하면 지속적으로 발생하는 외력을 추정하고 제거하면서도 과도한 액추에이터 동작을 방지할 수 있다. 위치 유지 성능은 심한 난류에서 모든 작은 위치 변위를 제거하려 하기보다는 구조 하중과 에너지 소비를 함께 고려하여 균형을 맞추어야 한다.

이륙 제어는 지상에서 기체의 무게가 지지되는 상태에서 완전히 공중에 떠 있는 상태로 전환되는 과정을 신중하게 관리해야 한다. 추력 증가는 여러 추진 채널 사이에서 조정되어야 하며, 동시에 제어기는 센서 진동이나 지상 접촉 효과에 과도하게 반응해서는 안 된다. 지상 상태 로직(Ground-State Logic), 지상 지지 감지(Weight-on-Support Detection), 고도 확인 및 추력 임계값을 활용하면 정상 비행 제어 모드로 전환하기 전에 불필요한 자세 보정이 발생하는 것을 방지할 수 있다.

착륙은 수직 속도, 수평 표류, 자세 및 착륙 충격을 동시에 제어해야 하기 때문에 또 다른 어려운 운용 영역이다. 기체가 지면에 접근하면 제어기는 하강 속도를 줄이는 동시에 돌풍에 대응할 수 있는 충분한 제어 권한을 유지해야 한다. 지형 기준 고도 감지(Terrain-Relative Altitude Sensing)는 GNSS를 보완할 수 있으며, 착륙 감지는 실제 지면 접촉과 일시적인 가속도 또는 센서 이상을 신뢰성 있게 구분해야 추력을 안전하게 감소시킬 수 있다.

비상 제어 모드(Emergency Control Mode)는 정상 제어 개발 이후 추가하는 기능이 아니라 기본 비행제어 아키텍처의 일부로 설계되어야 한다. 추진 장치 손실, 항법 성능 저하, 액추에이터 고장, 통신 장애 또는 전력 감소에 따라 서로 다른 제어 목표가 필요할 수 있다. 제어기는 위치 정확도, 궤적 성능 또는 임무 완료 가능성을 점진적으로 희생하더라도 자세 안정성과 조종성을 가장 높은 우선순위로 유지할 수 있어야 한다.

대형 멀티로터에서 추진 장치 고장은 추력 손실과 비대칭 모멘트를 동시에 발생시킨다. 따라서 자세 오차가 과도하게 증가하기 전에 제어기와 제어 할당기가 재구성할 수 있을 정도로 빠르게 고장을 탐지해야 한다. 고장 난 채널은 가용 액추에이터 집합에서 제거하고, 남은 추진 장치의 추력을 재분배하며, 성능 저하 구성에서 달성 가능한 수준으로 궤적 요구량을 감소시켜야 한다. 이후에는 착륙이 가장 중요한 임무 목표가 될 수 있다.

항법 성능 저하는 다른 형태의 제어 재구성을 요구한다. GNSS가 손실되거나 손상되더라도 안정화의 기본 루프는 주로 국부 관성 측정값에 의존해야 하므로 영향을 받아서는 안 된다. 위치 수준 기능은 시각, 라이다, 레이더 또는 관성 추정치와 같은 대체 항법 소스로 전환할 수 있다. 절대 위치 신뢰도가 충분하지 않으면 기체는 정밀 궤적 추종을 포기하고 제한된 속도, 자세 및 고도 동작을 유지할 수 있다.

대형 5톤급 기체에서는 구조적 유연성(Structural Flexibility)도 중요해질 수 있다. 대형 암, 날개, 로터 지지 구조, 착륙 구조 또는 화물 구조물에는 전체 제어 주파수 범위 안에 진동 모드(Vibration Mode)가 존재할 수 있다. 이러한 모드를 자극하는 제어기는 강체 동역학(Rigid-Body Dynamics)의 안정성이 양호하더라도 진동을 발생시킬 수 있다. 따라서 노치 필터(Notch Filter), 명령 형상화, 대역폭 분리(Bandwidth Separation), 구조 모드 식별 및 적절한 센서 배치를 제어법칙 개발과 검증 단계에서 고려해야 한다.

비행 엔벌로프 보호(Flight-Envelope Protection)는 정상 제어법칙 주변에 추가적인 안전 계층을 제공한다. 자세, 각속도, 대기속도, 하강률, 가속도, 하중계수, 액추에이터 요구량 및 에너지 상태에 대한 제한을 설정하여 자율 시스템이나 운용자 명령이 기체를 제어가 어려운 영역으로 진입시키는 것을 방지할 수 있다. 보호 로직은 원활하고 예측 가능하게 개입해야 하며, 제약조건 적용 자체가 기체를 불안정하게 만들 수 있는 불연속 명령을 발생시키지 않도록 해야 한다.

비행제어법칙은 명확하게 정의된 인터페이스를 통해 궤적 생성기(Trajectory Generator)와 상호작용해야 한다. 계획기는 동역학적으로 합리적인 기준값(Reference)을 제공하고, 제어기는 추종 가능성, 가용 가속도, 액추에이터 여유 및 성능 저하 상태의 제한조건을 전달한다. 기체가 요청된 궤적을 만족할 수 없다면 올바른 대응은 지속적인 포화가 아니라 실현 가능한 기준값으로의 조정이다. 이러한 분리는 안정성과 모듈성을 향상시키며 서로 다른 임무 계획 시스템에서도 동일한 제어 구조를 활용할 수 있게 한다.

결정론적 소프트웨어 실행(Deterministic Software Execution)은 제어 성능이 수학적 알고리즘뿐 아니라 실제 실행 타이밍에도 의존하기 때문에 필수적이다. 내부 루프 태스크는 제한된 지터(Bounded Jitter)를 갖는 정의된 주기로 실행되어야 하며 센서 타임스탬프는 실제 측정 시점을 나타내야 한다. 지연되거나 오래된 정보는 현재 정보인 것처럼 조용히 처리하지 말고 탐지해야 한다. 따라서 최악 실행시간(Worst-Case Execution Time)과 통신 지연은 실제 안정성 분석의 일부가 되어야 한다.

소프트웨어 인더루프(Software-in-the-Loop) 및 하드웨어 인더루프(Hardware-in-the-Loop) 시험에서는 비행 전에 광범위한 기체 파라미터와 외란을 제어기에 적용해야 한다. 시험에서는 탑재중량, 무게중심, 액추에이터 효율, 추진 지연, 바람, 센서 잡음, 통신 지연 및 항법 품질을 변화시킬 수 있다. 몬테카를로 시험(Monte Carlo Campaign)을 통해 각각의 파라미터가 개별적으로는 예상 운용 범위 안에 있더라도 안정성 또는 제어 여유를 감소시키는 조합을 식별할 수 있다.

비행시험 확대는 매우 제한된 조건에서 시작하여 전체 운용 엔벌로프로 점진적으로 진행되어야 한다. 초기 시험에서는 기본 안정화와 액추에이터 방향을 검증한 후 호버링, 병진 운동, 상승, 하강, 고속 비행, 대표 탑재중량, 횡풍 및 통제된 성능 저하 구성으로 확장한다. 기록된 비행 데이터는 시뮬레이션 예측값과 비교해야 하며, 보다 까다로운 운용 영역을 승인하기 전에 모델과 실제 기체 사이의 차이를 이해해야 한다.

성능 평가는 단순한 궤적 오차를 넘어야 한다. 주요 평가 항목에는 안정성 여유, 정착 시간(Settling Time), 오버슈트(Overshoot), 외란 제거 성능(Disturbance Rejection), 액추에이터 사용률, 제어 할당 잔차(Control-Allocation Residual), 에너지 소비, 구조 응답 및 남아 있는 제어 권한이 포함된다. 이러한 지표를 지속적으로 감시하면 액추에이터를 지속적으로 포화에 가깝게 사용하거나 과도한 이중화 여유를 소비하면서 양호한 추종 성능을 만들어내는 상황을 방지할 수 있다.

궁극적으로 5톤급 대형 화물 무인항공기의 비행제어법칙은 물리적으로 응답이 느리고 높은 에너지를 가지며 탑재중량에 민감한 기체를 예측 가능한 자율 플랫폼으로 전환해야 한다. 이를 위해서는 상태 추정, 계층형 피드백(Hierarchical Feedback), 제어 할당, 엔벌로프 보호, 고장 대응(Fault Accommodation), 결정론적 실행 및 엄격한 검증이 통합적으로 수행되어야 한다. 목표는 최대의 기동성이 아니라 정상, 성능 저하 및 비상 운용 조건에서 측정 가능한 안전 여유를 유지하면서 안정적이고 조종 가능한 동작을 보장하는 것이다.

##  

## 11.03. 5t UAV Redundant Avionics Architecture [w/Code]

![](images/image3.png){width="7.268055555555556in" height="7.268055555555556in"}

A 5-ton cargo UAV requires a redundant avionics architecture capable of maintaining controlled flight after credible failures of computers, sensors, communication links, power channels, or actuators. Because the aircraft carries substantial kinetic energy and payload, redundancy must be designed as a coordinated system property rather than as simple duplication. Fault detection, isolation, voting, reconfiguration, and degraded-mode management are therefore fundamental architectural functions.

The avionics architecture should separate flight-critical control from mission, perception, payload, and maintenance computing. Flight-control computers execute deterministic stabilization, guidance interfaces, actuator commands, and safety functions, while higher-level processors perform trajectory planning, perception, AI inference, and mission optimization. Controlled interfaces between these domains prevent failures or computational overload in noncritical software from propagating into flight-critical execution.

Multiple flight-control computer channels provide the foundation for computational redundancy. A practical architecture may use dual or triple channels depending on the safety objectives and required fault tolerance. Each channel independently processes sensor information and computes control outputs. Cross-channel monitoring compares internal states, timing, and command values so that disagreement can be detected before an erroneous channel gains unacceptable influence over aircraft behavior.

Triple-redundant architectures support majority voting when three valid and sufficiently independent channels are available. If one channel produces outputs inconsistent with the other two, voting logic can identify and isolate the disagreeing channel while maintaining control through the remaining pair. However, redundancy is effective only when common-cause failures are controlled, so software versions, power sources, clock dependencies, communication paths, and environmental exposure must be considered during architecture development.

Dual-redundant systems present a different challenge because disagreement between two channels does not inherently reveal which channel is correct. Additional independent monitors, sensor consistency checks, analytical models, or dissimilar safety computers can provide the information required for fault discrimination. The architecture should define deterministic rules for resolving disagreement rather than relying on ambiguous switching behavior during a time-critical failure.

Sensor redundancy must support both availability and integrity. Multiple inertial measurement units, GNSS receivers, altitude sensors, air-data sources, and propulsion feedback channels can provide independent observations of vehicle state. Sensor-management software evaluates measurement ranges, rates of change, timestamps, noise characteristics, cross-sensor consistency, and model residuals. A sensor should be excluded because evidence indicates degradation, not merely because another sensor reports a different value.

Physical separation is important because several logically redundant devices can still fail simultaneously if they share the same environmental vulnerability. Redundant sensors and computers should, where practical, use separated mounting locations, wiring routes, connectors, network paths, and power feeds. Thermal zones, vibration exposure, electromagnetic interference, moisture ingress, and mechanical damage should be evaluated to prevent a single physical event from disabling multiple supposedly independent channels.

Power architecture forms another layer of redundancy. Flight-critical computers, sensors, communication equipment, and actuators should not depend on a single electrical distribution path. Independent buses, protected converters, batteries, generators, and switching elements can maintain essential loads after a source failure. Avionics software must continuously monitor voltage, current, temperature, source status, and remaining energy so that electrical degradation becomes visible before critical functions are lost.

Load shedding should be coordinated with avionics criticality. When available electrical power decreases, payload processors, high-performance perception computers, nonessential communication equipment, or auxiliary systems can be reduced or disconnected before flight-control functions are affected. The system should use predefined priorities rather than making arbitrary decisions during an emergency. This allows the aircraft to preserve stabilization, navigation, essential communication, and landing capability for as long as possible.

Redundant communication networks connect distributed avionics without creating a single shared failure point. Flight-control data can be transmitted over independent network paths with bounded latency and deterministic scheduling. Critical messages should include sequence information, timestamps, source identification, and integrity checks. Receivers can then identify missing, duplicated, corrupted, or stale data rather than treating every received packet as valid aircraft state information.

Time synchronization is especially important in redundant systems because voting data generated at different times can appear inconsistent even when every channel is operating correctly. A common time base should therefore be distributed with known accuracy, while local clocks retain sufficient stability during temporary synchronization loss. Sensor measurements and control messages should preserve acquisition timestamps so that comparisons are performed on temporally aligned information rather than arrival order alone.

Redundancy management software should maintain an explicit health state for every safety-relevant resource. A component may be classified as healthy, suspected, degraded, failed, isolated, or recovering according to validated transition rules. This approach prevents rapid oscillation between resources when measurements fluctuate near fault thresholds. Hysteresis, persistence timers, confidence metrics, and recovery qualification can provide stable reconfiguration behavior under intermittent faults.

Fault detection should combine several forms of evidence. Built-in tests can identify internal hardware problems, while watchdogs detect missed execution deadlines or processor stalls. Cross-channel comparison reveals disagreement, and analytical monitors compare measured behavior with expected vehicle dynamics. Network supervision identifies communication faults, while power monitoring detects electrical anomalies. Combining these mechanisms reduces dependence on any single diagnostic method and improves fault coverage.

Fault isolation determines whether a detected anomaly belongs to a sensor, computer, network, power source, actuator, or external disturbance. Incorrect isolation can be more dangerous than delayed detection because a healthy resource may be removed while the actual failure remains active. Diagnostic logic should therefore use system-level context, including correlated events and shared dependencies, before commanding irreversible isolation or major control reconfiguration.

Reconfiguration must preserve continuity of control. When a flight computer or communication path fails, switching to another channel should avoid discontinuities in state estimates, integrator values, actuator commands, and mode logic. Standby channels may therefore operate in synchronized hot-standby mode rather than starting from an uninitialized state after failure. State transfer and bumpless switching techniques reduce transients during changes in the active control path.

Actuator redundancy extends avionics fault tolerance into the physical control system. Distributed propulsion, duplicated servo channels, or multiple aerodynamic surfaces may provide alternative means of producing required forces and moments. The avionics system should maintain current estimates of actuator availability and effectiveness. Control allocation can then redistribute commands following a failure instead of continuing to demand performance from an actuator that is unavailable or severely degraded.

Propulsion monitoring is particularly critical for a 5-ton UAV because loss of thrust may rapidly create asymmetric forces and reduced vertical capability. Motor or engine controllers should report rotational speed, commanded and delivered thrust indicators, temperature, electrical state, vibration, and internal fault information. Aircraft-level avionics correlate these measurements with vehicle response to determine whether a propulsion channel remains trustworthy and how much control authority is still available.

Navigation redundancy should support operation through individual sensor failures and temporary loss of external positioning. GNSS can be complemented by inertial navigation, visual odometry, LiDAR, radar, terrain-relative navigation, or other independent sources according to the mission. The navigation system should expose both its estimated state and associated integrity or uncertainty so that flight-control and mission software can distinguish precise navigation from merely available navigation.

A separate safety-monitoring function can provide protection against failures that affect the primary computing architecture. This monitor may use simplified independent logic to observe attitude, altitude, velocity, processor health, communication status, and flight-envelope boundaries. It does not need to reproduce every autonomy capability. Its purpose is to detect unacceptable conditions and initiate bounded protective actions when the primary system can no longer be trusted.

Degraded operating modes should be defined before failures occur. Loss of redundancy does not always require immediate landing, but it should change the operational envelope and mission policy. The aircraft may restrict speed, altitude, maneuvering, weather exposure, or destination options after a channel failure. Further degradation can trigger diversion, return-to-base, precautionary landing, or emergency landing according to remaining navigation, propulsion, power, and control capability.

Common-cause failure analysis is essential because adding more identical components does not automatically increase safety. A software defect, incorrect configuration, shared power transient, network protocol error, electromagnetic event, or environmental condition may affect several redundant channels simultaneously. Independence should therefore be evaluated across hardware, software, data, power, timing, installation, and maintenance processes rather than measured only by the number of installed devices.

Dissimilarity can reduce selected common-cause risks when justified by safety analysis. Independent monitoring software, different sensor technologies, separate power-conversion paths, or diverse navigation principles may prevent one failure mechanism from defeating all channels. However, unnecessary diversity can increase integration and verification complexity. The architecture should therefore apply dissimilarity where it addresses identified hazards rather than treating diversity as an objective by itself.

Maintenance architecture must preserve redundancy after the aircraft returns to service. Configuration errors, incorrect replacement parts, incomplete calibration, or mismatched software versions can silently compromise fault tolerance before takeoff. Automated preflight checks should verify hardware identity, software compatibility, sensor status, power paths, network connectivity, synchronization, and redundancy availability. Maintenance records should preserve the history of faults, replacements, and configuration changes.

Verification must demonstrate not only nominal performance but successful behavior during combinations of failures. Software-in-the-loop and hardware-in-the-loop environments can inject processor resets, sensor bias, frozen data, network delay, packet loss, power-source failures, actuator degradation, and timing faults. Tests should confirm detection latency, isolation correctness, switching transients, remaining control authority, and the final degraded mode reached by the aircraft.

The resulting redundant avionics architecture should provide graceful degradation rather than an unrealistic expectation of uninterrupted full capability after every failure. Its objective is to ensure that individual faults are contained, detected, and managed while essential flight functions remain available. By combining computational, sensor, network, power, actuator, navigation, and safety-monitoring redundancy, the 5-ton UAV can maintain predictable behavior and measurable safety margins throughout demanding cargo operations.

5톤급 화물 무인항공기(Cargo UAV)는 컴퓨터, 센서, 통신 링크, 전력 채널 또는 액추에이터에서 발생할 수 있는 신뢰 가능한 고장(Credible Failure) 이후에도 제어 비행(Controlled Flight)을 유지할 수 있는 이중화 항공전자 아키텍처(Redundant Avionics Architecture)가 필요하다. 기체가 상당한 운동에너지와 탑재물을 가지므로 이중화(Redundancy)는 단순한 장치 복제가 아니라 상호 조정되는 시스템 특성으로 설계되어야 한다. 따라서 고장 탐지(Fault Detection), 격리(Isolation), 투표(Voting), 재구성(Reconfiguration), 성능 저하 모드 관리(Degraded-Mode Management)는 핵심적인 아키텍처 기능이 된다.

항공전자 아키텍처(Avionics Architecture)는 비행 필수 제어(Flight-Critical Control)를 임무, 인지(Perception), 탑재물 및 정비 컴퓨팅과 분리해야 한다. 비행제어 컴퓨터(Flight-Control Computer)는 결정론적 안정화(Deterministic Stabilization), 유도 인터페이스(Guidance Interface), 액추에이터 명령 및 안전 기능을 실행하고, 상위 프로세서는 궤적 계획(Trajectory Planning), 인지, 인공지능 추론(AI Inference), 임무 최적화를 수행한다. 이러한 도메인 사이의 제어된 인터페이스는 비필수 소프트웨어의 고장이나 계산 과부하가 비행 필수 실행 영역으로 전파되는 것을 방지한다.

다중 비행제어 컴퓨터 채널(Multiple Flight-Control Computer Channel)은 계산 이중화(Computational Redundancy)의 기반을 제공한다. 실제 아키텍처에서는 안전 목표와 요구되는 고장 허용성(Fault Tolerance)에 따라 이중(Dual) 또는 삼중(Triple) 채널을 사용할 수 있다. 각 채널은 독립적으로 센서 정보를 처리하고 제어 출력을 계산한다. 채널 간 감시(Cross-Channel Monitoring)는 내부 상태, 타이밍 및 명령값을 비교하여 잘못된 채널이 기체 동작에 허용할 수 없는 영향을 미치기 전에 불일치를 탐지할 수 있도록 한다.

삼중 이중화 아키텍처(Triple-Redundant Architecture)는 유효하고 충분히 독립적인 세 개의 채널을 사용할 수 있을 때 다수결 투표(Majority Voting)를 지원한다. 하나의 채널이 다른 두 채널과 일치하지 않는 출력을 생성하면 투표 로직(Voting Logic)은 불일치 채널을 식별하고 격리하면서 나머지 두 채널을 통해 제어를 유지할 수 있다. 그러나 공통원인 고장(Common-Cause Failure)이 통제되는 경우에만 이중화가 효과적이므로 소프트웨어 버전, 전원, 클록 의존성, 통신 경로 및 환경 노출을 아키텍처 개발 과정에서 함께 고려해야 한다.

이중 이중화 시스템(Dual-Redundant System)은 두 채널 사이에 불일치가 발생해도 어느 채널이 올바른지 본질적으로 판단할 수 없다는 다른 문제를 가진다. 추가적인 독립 감시기(Independent Monitor), 센서 일관성 검사(Sensor Consistency Check), 분석 모델(Analytical Model) 또는 이종 안전 컴퓨터(Dissimilar Safety Computer)를 이용하여 고장 판별에 필요한 정보를 제공할 수 있다. 아키텍처는 시간적으로 긴급한 고장 상황에서 모호한 전환 동작에 의존하지 않고 불일치를 해결하기 위한 결정론적 규칙(Deterministic Rule)을 정의해야 한다.

센서 이중화(Sensor Redundancy)는 가용성(Availability)과 무결성(Integrity)을 모두 지원해야 한다. 다수의 관성측정장치(IMU), 위성항법시스템 수신기(GNSS Receiver), 고도 센서, 대기자료 센서(Air-Data Source), 추진 피드백 채널은 기체 상태에 대한 독립적인 관측 정보를 제공할 수 있다. 센서 관리 소프트웨어는 측정 범위, 변화율, 타임스탬프, 잡음 특성, 센서 간 일관성 및 모델 잔차(Model Residual)를 평가한다. 단순히 다른 센서와 값이 다르다는 이유가 아니라 성능 저하를 나타내는 충분한 근거가 있을 때 해당 센서를 제외해야 한다.

물리적 분리(Physical Separation)도 중요하다. 논리적으로 이중화된 여러 장치가 동일한 환경적 취약성을 공유하면 동시에 고장날 수 있기 때문이다. 가능한 경우 이중화 센서와 컴퓨터는 서로 분리된 장착 위치, 배선 경로, 커넥터, 네트워크 경로 및 전원 공급 경로를 사용해야 한다. 하나의 물리적 사건으로 여러 독립 채널이 동시에 손실되는 것을 방지하기 위해 열 구역(Thermal Zone), 진동 노출, 전자기 간섭(Electromagnetic Interference), 수분 침투 및 기계적 손상 가능성을 평가해야 한다.

전력 아키텍처(Power Architecture)는 또 다른 이중화 계층을 형성한다. 비행 필수 컴퓨터, 센서, 통신 장비 및 액추에이터가 하나의 전력 분배 경로에 의존해서는 안 된다. 독립 버스(Independent Bus), 보호형 변환기(Protected Converter), 배터리, 발전기 및 스위칭 장치를 이용하면 하나의 전원이 고장난 이후에도 필수 부하(Essential Load)를 유지할 수 있다. 항공전자 소프트웨어는 전압, 전류, 온도, 전원 상태 및 잔여 에너지를 지속적으로 감시하여 필수 기능이 손실되기 전에 전력 성능 저하를 식별해야 한다.

부하 차단(Load Shedding)은 항공전자 시스템의 중요도와 연계되어야 한다. 가용 전력이 감소하면 비행제어 기능이 영향을 받기 전에 탑재물 프로세서, 고성능 인지 컴퓨터, 비필수 통신 장비 또는 보조 시스템의 출력을 줄이거나 전원을 차단할 수 있다. 시스템은 비상 상황에서 임의로 판단하는 대신 사전에 정의된 우선순위를 사용해야 한다. 이를 통해 안정화, 항법, 필수 통신 및 착륙 기능을 가능한 한 오랫동안 유지할 수 있다.

이중화 통신 네트워크(Redundant Communication Network)는 하나의 공유 고장점(Single Shared Failure Point)을 만들지 않으면서 분산 항공전자 시스템을 연결한다. 비행제어 데이터는 제한된 지연시간(Bounded Latency)과 결정론적 스케줄링(Deterministic Scheduling)을 갖는 독립 네트워크 경로를 통해 전송할 수 있다. 중요 메시지에는 순서 정보, 타임스탬프, 송신원 식별정보 및 무결성 검사(Integrity Check)가 포함되어야 한다. 이를 통해 수신기는 모든 패킷을 유효한 기체 상태 정보로 간주하지 않고 누락, 중복, 손상 또는 오래된 데이터를 식별할 수 있다.

시간 동기화(Time Synchronization)는 이중화 시스템에서 특히 중요하다. 서로 다른 시점에 생성된 투표 데이터는 모든 채널이 정상적으로 작동하더라도 서로 일치하지 않는 것처럼 보일 수 있기 때문이다. 따라서 알려진 정확도를 갖는 공통 시간 기준(Common Time Base)을 분배해야 하며, 로컬 클록(Local Clock)은 일시적으로 동기화가 손실되더라도 충분한 안정성을 유지해야 한다. 센서 측정값과 제어 메시지는 획득 시점의 타임스탬프를 보존하여 단순한 데이터 도착 순서가 아니라 시간적으로 정렬된 정보를 기준으로 비교할 수 있도록 해야 한다.

이중화 관리 소프트웨어(Redundancy Management Software)는 모든 안전 관련 자원에 대해 명시적인 건전성 상태(Health State)를 유지해야 한다. 구성요소는 검증된 상태 전이 규칙에 따라 정상(Healthy), 의심(Suspected), 성능 저하(Degraded), 고장(Failed), 격리(Isolated) 또는 복구 중(Recovering)으로 분류될 수 있다. 이러한 방식은 측정값이 고장 임계값 부근에서 변동할 때 자원 사이의 빈번한 전환을 방지한다. 히스테리시스(Hysteresis), 지속시간 타이머(Persistence Timer), 신뢰도 지표(Confidence Metric), 복구 적합성 검증(Recovery Qualification)을 통해 간헐적 고장에서도 안정적인 재구성 동작을 구현할 수 있다.

고장 탐지는 여러 형태의 증거를 결합해야 한다. 내장 시험(Built-In Test)은 내부 하드웨어 문제를 식별하고, 워치독(Watchdog)은 실행 마감시간 누락이나 프로세서 정지를 탐지할 수 있다. 채널 간 비교는 불일치를 발견하고, 분석 감시기(Analytical Monitor)는 측정된 동작을 예상되는 기체 동역학과 비교한다. 네트워크 감시는 통신 고장을 식별하며 전력 감시는 전기적 이상을 탐지한다. 이러한 메커니즘을 결합하면 단일 진단 방식에 대한 의존도를 낮추고 고장 탐지 범위(Fault Coverage)를 향상시킬 수 있다.

고장 격리(Fault Isolation)는 탐지된 이상이 센서, 컴퓨터, 네트워크, 전원, 액추에이터 또는 외부 외란 가운데 어디에서 발생했는지를 판단한다. 잘못된 고장 격리는 실제 고장이 계속 활성화된 상태에서 정상 자원을 제거할 수 있기 때문에 늦은 고장 탐지보다 더 위험할 수도 있다. 따라서 진단 로직은 비가역적인 격리 또는 주요 제어 재구성을 명령하기 전에 상관된 이벤트(Correlated Event)와 공유 의존성을 포함한 시스템 수준의 상황 정보를 활용해야 한다.

재구성(Reconfiguration)은 제어 연속성(Control Continuity)을 유지해야 한다. 비행 컴퓨터 또는 통신 경로가 고장났을 때 다른 채널로 전환하는 과정에서 상태 추정값, 적분기 값, 액추에이터 명령 및 모드 로직에 불연속이 발생해서는 안 된다. 따라서 대기 채널은 고장 이후 초기화되지 않은 상태에서 시작하기보다는 동기화된 핫 스탠바이 모드(Hot-Standby Mode)로 운용될 수 있다. 상태 전달(State Transfer)과 무충격 전환(Bumpless Switching) 기법은 활성 제어 경로가 변경될 때 발생하는 과도응답을 감소시킨다.

액추에이터 이중화(Actuator Redundancy)는 항공전자 시스템의 고장 허용성을 실제 물리적 제어 시스템까지 확장한다. 분산 추진(Distributed Propulsion), 이중화 서보 채널 또는 다중 공력 제어면은 필요한 힘과 모멘트를 생성하는 대체 수단을 제공할 수 있다. 항공전자 시스템은 액추에이터의 가용성과 유효성에 대한 최신 추정치를 유지해야 한다. 이를 통해 제어 할당(Control Allocation)은 고장 발생 후 사용할 수 없거나 심각하게 성능이 저하된 액추에이터에 계속 명령하는 대신 정상 액추에이터로 명령을 재분배할 수 있다.

추진계 감시(Propulsion Monitoring)는 5톤급 무인항공기에서 특히 중요하다. 추력 손실은 빠르게 비대칭 힘과 수직 비행 능력 감소를 발생시킬 수 있기 때문이다. 모터 또는 엔진 제어기는 회전속도, 명령된 추력과 실제 추력을 나타내는 지표, 온도, 전기 상태, 진동 및 내부 고장 정보를 제공해야 한다. 기체 수준의 항공전자 시스템은 이러한 측정값을 실제 기체 응답과 연계하여 특정 추진 채널을 계속 신뢰할 수 있는지, 그리고 어느 정도의 제어 권한이 남아 있는지를 판단한다.

항법 이중화(Navigation Redundancy)는 개별 센서 고장과 외부 위치정보의 일시적인 손실 상황에서도 운용을 지원해야 한다. 임무 특성에 따라 GNSS는 관성항법(Inertial Navigation), 시각 주행거리 추정(Visual Odometry), 라이다(LiDAR), 레이더(Radar), 지형 기준 항법(Terrain-Relative Navigation) 또는 기타 독립적인 항법 정보원으로 보완할 수 있다. 항법 시스템은 추정된 상태뿐 아니라 관련 무결성 또는 불확실성 정보도 제공하여 비행제어 및 임무 소프트웨어가 정밀한 항법과 단순히 사용 가능한 항법을 구분할 수 있도록 해야 한다.

별도의 안전 감시 기능(Safety-Monitoring Function)은 주 컴퓨팅 아키텍처에 영향을 주는 고장에 대한 추가 보호를 제공할 수 있다. 이 감시기는 단순화된 독립 로직을 사용하여 자세, 고도, 속도, 프로세서 건전성, 통신 상태 및 비행 엔벌로프 경계(Flight-Envelope Boundary)를 감시할 수 있다. 모든 자율 기능을 동일하게 구현할 필요는 없다. 목적은 주 시스템을 더 이상 신뢰할 수 없을 때 허용할 수 없는 상태를 탐지하고 제한된 보호 동작(Bounded Protective Action)을 실행하는 것이다.

성능 저하 운용 모드(Degraded Operating Mode)는 고장이 발생하기 전에 정의되어야 한다. 이중화 기능의 일부 손실이 항상 즉각적인 착륙을 요구하는 것은 아니지만 운용 엔벌로프와 임무 정책은 변경되어야 한다. 특정 채널이 고장난 이후 기체는 속도, 고도, 기동, 기상 조건 또는 목적지 선택을 제한할 수 있다. 추가적인 성능 저하가 발생하면 남아 있는 항법, 추진, 전력 및 제어 능력에 따라 회항(Diversion), 기지 복귀(Return-to-Base), 예방 착륙(Precautionary Landing) 또는 비상 착륙(Emergency Landing)을 수행할 수 있다.

공통원인 고장 분석(Common-Cause Failure Analysis)은 동일한 구성요소를 추가하는 것만으로 안전성이 자동으로 향상되는 것이 아니기 때문에 필수적이다. 소프트웨어 결함, 잘못된 형상 설정, 공유 전력 과도현상(Shared Power Transient), 네트워크 프로토콜 오류, 전자기적 사건 또는 환경 조건은 여러 이중화 채널에 동시에 영향을 줄 수 있다. 따라서 독립성(Independence)은 설치된 장치의 개수만으로 평가하지 않고 하드웨어, 소프트웨어, 데이터, 전력, 타이밍, 설치 및 정비 프로세스 전체에서 평가해야 한다.

이종성(Dissimilarity)은 안전성 분석에서 필요성이 확인된 경우 특정 공통원인 위험을 감소시킬 수 있다. 독립적인 감시 소프트웨어, 서로 다른 센서 기술, 분리된 전력 변환 경로 또는 서로 다른 항법 원리(Diverse Navigation Principle)를 사용하면 하나의 고장 메커니즘이 모든 채널을 동시에 무력화하는 것을 방지할 수 있다. 그러나 불필요한 다양성은 통합 및 검증 복잡성을 증가시킬 수 있다. 따라서 아키텍처는 다양성 자체를 목표로 하기보다는 식별된 위험을 해결하는 경우에 선택적으로 이종성을 적용해야 한다.

정비 아키텍처(Maintenance Architecture)는 기체가 다시 운용에 투입된 이후에도 이중화 특성을 유지해야 한다. 형상 설정 오류, 잘못된 교체 부품, 불완전한 보정(Calibration) 또는 일치하지 않는 소프트웨어 버전은 이륙 전에 고장 허용성을 보이지 않게 손상시킬 수 있다. 자동 비행 전 점검(Automated Preflight Check)은 하드웨어 식별정보, 소프트웨어 호환성, 센서 상태, 전력 경로, 네트워크 연결성, 시간 동기화 및 이중화 가용성을 검증해야 한다. 정비 기록에는 고장, 부품 교체 및 형상 변경 이력이 보존되어야 한다.

검증(Verification)은 정상 성능뿐 아니라 여러 고장 조합에서 시스템이 성공적으로 동작하는지도 입증해야 한다. 소프트웨어 인더루프(Software-in-the-Loop) 및 하드웨어 인더루프(Hardware-in-the-Loop) 환경에서는 프로세서 재시작, 센서 바이어스(Sensor Bias), 고정된 데이터(Frozen Data), 네트워크 지연, 패킷 손실, 전원 고장, 액추에이터 성능 저하 및 타이밍 고장을 주입할 수 있다. 시험을 통해 탐지 지연시간, 고장 격리의 정확성, 전환 과도응답, 잔여 제어 권한 및 최종적으로 기체가 도달하는 성능 저하 모드를 확인해야 한다.

최종적인 이중화 항공전자 아키텍처(Redundant Avionics Architecture)는 모든 고장 이후에도 완전한 성능이 중단 없이 유지된다는 비현실적인 목표보다는 점진적 성능 저하(Graceful Degradation)를 제공해야 한다. 핵심 목적은 개별 고장을 격리하고 탐지하며 관리하는 동시에 필수 비행 기능을 계속 사용할 수 있도록 하는 것이다. 계산, 센서, 네트워크, 전력, 액추에이터, 항법 및 안전 감시 이중화를 통합함으로써 5톤급 무인항공기는 까다로운 화물 운송 임무 전반에서 예측 가능한 동작과 측정 가능한 안전 여유(Safety Margin)를 유지할 수 있다.

##  

## 11.04. 10t UAV Turbine Hybrid Propulsion SW [w/Code]

![](images/image4.png){width="7.268055555555556in" height="7.268055555555556in"}

A 10-ton cargo UAV using turbine-hybrid propulsion requires software that coordinates turbine engines, generators, electrical storage, power converters, motors, and aircraft-level flight control as one integrated energy and propulsion system. The software must balance thrust demand, electrical generation, fuel consumption, battery state, thermal limits, and component health while preserving deterministic response for flight-critical propulsion commands.

The hybrid architecture separates energy production from distributed thrust generation. A turbine may drive one or more generators that supply electrical buses, while batteries or other storage devices absorb transients and provide reserve power. Electric motors then convert distributed electrical energy into propulsive force. Software becomes responsible for coordinating energy flow across these domains so that temporary differences between generated power and propulsion demand do not destabilize the aircraft.

A hierarchical propulsion-control structure is appropriate for this scale. Aircraft-level software determines required total forces, moments, and power, while propulsion-management software converts these objectives into turbine, generator, battery, and motor operating targets. Local controllers regulate individual machines at faster rates. This separation allows high-level optimization without placing detailed turbine or motor dynamics directly inside the primary flight-control loops.

The turbine controller must manage start, acceleration, steady operation, deceleration, shutdown, and abnormal conditions within validated operating limits. Rotor speed, exhaust temperature, fuel flow, compressor condition, lubrication, vibration, and generator load are monitored continuously. Command scheduling should prevent excessive thermal or mechanical transients while still providing sufficient power response to support changes in aircraft thrust demand.

Turbines generally respond more slowly than electric motors, creating an important control challenge. Rapid thrust changes can produce electrical demand faster than the turbine-generator system can increase output. Energy storage therefore acts as a dynamic buffer. Propulsion software predicts short-term power demand and coordinates battery discharge or charge so that motor commands can respond quickly without forcing the turbine beyond safe acceleration or temperature limits.

Energy-management software should maintain sufficient battery state of charge for more than nominal efficiency optimization. Stored energy represents a safety resource that may be required for takeoff, landing, rejected maneuvers, turbine transients, generator failures, or emergency descent. The control strategy should therefore preserve configurable reserve margins and prevent routine mission optimization from consuming energy needed to manage credible abnormal conditions.

Power-flow management must account for conversion losses and component limitations. Generator output passes through electrical distribution and power electronics before reaching propulsion motors or storage systems. Each stage has current, voltage, temperature, and power limits. The software should calculate available rather than theoretical power, incorporating derating caused by thermal conditions, altitude, component aging, or faults when determining feasible propulsion commands.

Electrical bus regulation is a central function because distributed motors can create large and rapidly changing loads. Bus voltage must remain within acceptable limits during acceleration, regenerative events, load switching, or component isolation. Battery converters, generator controllers, and load-management functions can cooperate to stabilize the bus. Protection logic must distinguish recoverable voltage excursions from faults requiring immediate isolation of a power channel.

A 10-ton platform may use multiple turbine-generator channels to provide propulsion redundancy. Aircraft-level software must know the current capacity, efficiency, temperature, and health of each channel rather than simply assuming equal contribution. Power commands can then be distributed according to available capability. If one generator becomes limited, remaining generators and energy storage can temporarily compensate while the flight system reduces demand or initiates a contingency.

Distributed electric propulsion requires close coordination between energy management and control allocation. The flight controller determines the forces and moments needed for vehicle motion, while the allocator distributes thrust among available motors. The energy manager verifies whether the resulting electrical demand is sustainable. When power is constrained, the architecture must resolve competing objectives by prioritizing stabilization and controllability over trajectory accuracy or mission performance.

Motor and inverter controllers should provide high-rate feedback on rotational speed, current, voltage, torque indicators, temperature, and internal faults. Aircraft-level software compares commanded and observed behavior to estimate propulsion effectiveness. A motor producing less thrust than expected may indicate electrical limitation, mechanical damage, rotor degradation, or aerodynamic interference. Such information allows control allocation to adapt before a complete propulsion failure occurs.

Thermal management is inseparable from propulsion control because turbines, generators, batteries, inverters, motors, and cooling systems all have temperature-dependent capability. The software should predict thermal trends rather than responding only after limits are exceeded. Progressive derating can reduce power before protective shutdown becomes necessary, while mission planning can account for ambient temperature, altitude, hover duration, and expected cooling effectiveness.

Fuel management remains important even when thrust is produced electrically. Turbine fuel consumption depends on operating point, generator loading, atmospheric conditions, and transient behavior. Energy-management algorithms can schedule turbines near efficient operating regions while batteries handle short-duration variations. However, efficiency optimization must remain subordinate to safety, propulsion availability, reserve requirements, thermal constraints, and the need for rapid response during critical flight phases.

The propulsion software should use explicit operating modes such as initialization, start, warm-up, normal generation, high-demand operation, degraded operation, emergency power, cooldown, and shutdown. Transitions between modes require verified entry conditions and bounded actions. Mode logic prevents conflicting commands, such as demanding maximum generator power before a turbine has reached a valid operating condition or disconnecting storage during a critical transient.

Start sequencing becomes more complex when multiple turbines, generators, batteries, and electrical buses interact. Software must verify fuel availability, battery condition, contactor state, cooling readiness, communication integrity, and controller health before initiating start. After ignition, generator connection should occur only after speed, voltage, frequency, temperature, and synchronization requirements are satisfied. Failed starts must lead to controlled recovery rather than repeated uncontrolled attempts.

Fault detection, isolation, and recovery should operate across both mechanical and electrical domains. Turbine overtemperature, generator faults, inverter failures, battery abnormalities, bus faults, cooling degradation, sensor errors, and motor failures can produce similar symptoms at the aircraft level. Diagnostic software therefore correlates measurements across components to identify the likely fault source before isolating equipment or significantly changing propulsion configuration.

A turbine failure requires coordinated energy and flight responses. The affected generator channel should be isolated if necessary, while batteries and remaining generation sources immediately support essential propulsion loads. The energy manager calculates revised sustainable power, and the flight-control system receives updated thrust limits. Mission software can then reduce speed, climb demand, or maneuvering and determine whether continued flight, diversion, or immediate landing is appropriate.

Generator or electrical distribution failures may occur even when the turbine itself remains mechanically healthy. The architecture should distinguish loss of mechanical power production from loss of electrical conversion or transmission. This distinction prevents unnecessary turbine shutdown and supports alternative configurations where power can be rerouted through another bus or converter. Isolation logic must act rapidly enough to prevent a local electrical fault from collapsing healthy power channels.

Battery faults require particularly careful containment because high-energy storage can introduce thermal and electrical hazards. Battery-management software monitors cell voltage, temperature, current, insulation state, state of charge, and state of health. Abnormal modules may require current limitation or isolation. The aircraft must then recalculate available transient and emergency power because removing storage capacity can significantly reduce propulsion response even when turbine generation remains available.

Communication between propulsion controllers should use deterministic interfaces with timestamps, validity information, sequence counters, and integrity checks. Commands and measurements must have defined update rates and timeout behavior. A stale thrust or power command should never remain active indefinitely after communication loss. Local controllers require safe fallback behavior that maintains bounded operation while aircraft-level software determines the appropriate degraded configuration.

Time synchronization supports accurate correlation of turbine, generator, battery, motor, and aircraft dynamics. When a thrust deficit occurs, diagnostic functions must determine whether electrical power changed before motor speed, whether turbine response lagged generator demand, or whether a sensor simply reported late. Preserving measurement timestamps across the propulsion network improves fault isolation, transient analysis, control tuning, and post-flight investigation.

Cybersecurity must protect propulsion configuration and commands without compromising deterministic control. Secure boot, authenticated software, protected calibration data, access-controlled maintenance interfaces, and authenticated external updates reduce the risk of unauthorized changes. Network segmentation should prevent payload or mission applications from directly issuing low-level propulsion commands, while safety gateways expose only the interfaces required for legitimate aircraft-level control.

Propulsion software verification requires high-fidelity models spanning mechanical, electrical, thermal, and flight dynamics. Software-in-the-loop testing can evaluate energy-management algorithms over long missions, while hardware-in-the-loop environments reproduce controller timing, bus behavior, sensors, and communication faults. Fault injection should include turbine loss, generator trips, battery isolation, inverter faults, motor degradation, cooling failures, and combinations of reduced-capability conditions.

Transient testing is especially important because hybrid systems may appear stable during steady operation while failing during rapid power redistribution. Verification should examine takeoff power application, aggressive climb commands, sudden thrust reduction, generator connection and disconnection, turbine failure, battery current limiting, and emergency landing. Voltage excursions, thermal response, control continuity, and remaining propulsion margin should be measured throughout each event.

Mission planning should receive propulsion capability as a dynamic constraint rather than assuming a fixed performance envelope. Available power changes with fuel quantity, battery state, temperature, altitude, component health, and redundancy status. By exposing these limits to trajectory and mission software, the aircraft can avoid requesting climbs, speeds, or hover durations that cannot be sustained under the current energy configuration.

The central objective of 10-ton turbine-hybrid propulsion software is therefore coordinated management of propulsion authority and energy availability. Turbines provide high-energy endurance, electrical storage supplies rapid response and reserve capability, and distributed motors provide controllable thrust. Reliable operation emerges when flight control, energy management, thermal protection, fault handling, and power distribution continuously exchange validated capability information.

A well-designed system does not attempt to maximize turbine efficiency, battery utilization, or motor performance independently. It optimizes the complete aircraft while preserving safety margins and graceful degradation. Through deterministic control, predictive energy management, redundant power paths, fault containment, and rigorous verification, turbine-hybrid propulsion software can support the sustained power, rapid response, and fault tolerance required for autonomous 10-ton cargo UAV operations.

터빈-하이브리드 추진(Turbine-Hybrid Propulsion)을 사용하는 10톤급 화물 무인항공기(Cargo UAV)는 터빈 엔진, 발전기, 전기 에너지 저장장치, 전력 변환기(Power Converter), 모터 및 기체 수준 비행제어 시스템을 하나의 통합된 에너지 및 추진 시스템으로 조정하는 소프트웨어가 필요하다. 소프트웨어는 비행 필수 추진 명령에 대한 결정론적 응답(Deterministic Response)을 유지하면서 추력 요구량, 발전량, 연료 소비, 배터리 상태, 열적 한계 및 구성요소 건전성을 균형 있게 관리해야 한다.

하이브리드 아키텍처(Hybrid Architecture)는 에너지 생산과 분산 추력 생성을 분리한다. 터빈은 하나 이상의 발전기를 구동하여 전력 버스(Electrical Bus)에 전력을 공급하고, 배터리 또는 기타 에너지 저장장치는 과도 상태의 에너지를 흡수하거나 예비 전력을 제공한다. 이후 전기 모터가 분산된 전기 에너지를 추진력으로 변환한다. 소프트웨어는 발전 전력과 추진 요구량 사이의 일시적인 차이가 기체를 불안정하게 만들지 않도록 이러한 영역 전체의 에너지 흐름을 조정해야 한다.

이 규모의 기체에는 계층형 추진 제어 구조(Hierarchical Propulsion-Control Structure)가 적합하다. 기체 수준 소프트웨어는 필요한 전체 힘, 모멘트 및 전력을 결정하고, 추진 관리 소프트웨어(Propulsion-Management Software)는 이러한 목표를 터빈, 발전기, 배터리 및 모터의 운용 목표로 변환한다. 로컬 제어기(Local Controller)는 개별 장치를 더 빠른 주기로 제어한다. 이러한 분리는 세부적인 터빈 또는 모터 동역학을 기본 비행제어 루프에 직접 포함하지 않으면서 상위 수준의 최적화를 가능하게 한다.

터빈 제어기(Turbine Controller)는 검증된 운용 한계 안에서 시동, 가속, 정상 운전, 감속, 정지 및 비정상 상황을 관리해야 한다. 회전자 속도, 배기가스 온도, 연료 유량, 압축기 상태, 윤활 상태, 진동 및 발전기 부하를 지속적으로 감시한다. 명령 스케줄링(Command Scheduling)은 과도한 열적 또는 기계적 과도현상을 방지하면서도 기체의 추력 요구 변화에 대응할 수 있는 충분한 전력 응답을 제공해야 한다.

터빈은 일반적으로 전기 모터보다 느리게 응답하므로 중요한 제어 문제가 발생한다. 급격한 추력 변화가 발생하면 터빈-발전기 시스템이 출력을 증가시키는 속도보다 빠르게 전력 수요가 증가할 수 있다. 따라서 에너지 저장장치가 동적 버퍼(Dynamic Buffer) 역할을 수행한다. 추진 소프트웨어는 단기 전력 수요를 예측하고 배터리의 방전 또는 충전을 조정하여 터빈이 안전한 가속 또는 온도 한계를 초과하지 않으면서 모터 명령에 신속하게 대응할 수 있도록 해야 한다.

에너지 관리 소프트웨어(Energy-Management Software)는 단순한 정상 운용 효율 최적화를 넘어 충분한 배터리 충전상태(State of Charge)를 유지해야 한다. 저장된 에너지는 이륙, 착륙, 기동 중단, 터빈 과도상태, 발전기 고장 또는 비상 하강에 필요할 수 있는 안전 자원(Safety Resource)이다. 따라서 제어 전략은 설정 가능한 예비 에너지 여유(Reserve Margin)를 유지하고, 정상 임무 최적화 과정에서 신뢰 가능한 비정상 상황에 대응하는 데 필요한 에너지가 소모되지 않도록 해야 한다.

전력 흐름 관리(Power-Flow Management)는 변환 손실과 구성요소의 제한조건을 고려해야 한다. 발전기 출력은 추진 모터 또는 에너지 저장 시스템에 도달하기 전에 전력 분배 시스템과 전력전자 장치(Power Electronics)를 통과한다. 각 단계에는 전류, 전압, 온도 및 전력 한계가 존재한다. 소프트웨어는 이론적인 전력이 아니라 실제 가용 전력(Available Power)을 계산해야 하며, 추진 명령의 실행 가능성을 판단할 때 열적 조건, 고도, 구성요소 노화 또는 고장으로 발생하는 출력 저감(Derating)을 반영해야 한다.

분산된 모터가 크고 빠르게 변화하는 부하를 생성할 수 있으므로 전력 버스 조정(Electrical Bus Regulation)은 핵심 기능이다. 가속, 회생 상황(Regenerative Event), 부하 전환 또는 구성요소 격리 과정에서도 버스 전압은 허용 범위 안에서 유지되어야 한다. 배터리 변환기, 발전기 제어기 및 부하 관리 기능은 협력하여 버스를 안정화할 수 있다. 보호 로직은 복구 가능한 전압 변동과 전력 채널의 즉각적인 격리가 필요한 고장을 구분해야 한다.

10톤급 플랫폼은 추진 이중화(Propulsion Redundancy)를 위해 다중 터빈-발전기 채널(Multiple Turbine-Generator Channel)을 사용할 수 있다. 기체 수준 소프트웨어는 각 채널이 동일한 성능을 제공한다고 단순하게 가정하지 않고 현재 용량, 효율, 온도 및 건전성을 파악해야 한다. 이후 가용 능력에 따라 전력 명령을 분배할 수 있다. 하나의 발전기가 제한되면 남아 있는 발전기와 에너지 저장장치가 일시적으로 이를 보상하는 동안 비행 시스템은 요구량을 감소시키거나 비상 절차를 시작할 수 있다.

분산 전기 추진(Distributed Electric Propulsion)은 에너지 관리와 제어 할당(Control Allocation) 사이의 긴밀한 조정을 요구한다. 비행제어기는 기체 운동에 필요한 힘과 모멘트를 결정하고, 제어 할당기는 사용 가능한 모터에 추력을 분배한다. 에너지 관리기는 이에 따른 전력 요구량을 지속적으로 공급할 수 있는지를 검증한다. 전력이 제한될 경우 아키텍처는 궤적 정확도나 임무 성능보다 안정화와 조종성(Controllability)을 우선함으로써 서로 경쟁하는 제어 목표를 조정해야 한다.

모터 및 인버터 제어기(Inverter Controller)는 회전속도, 전류, 전압, 토크 지표, 온도 및 내부 고장에 대한 고속 피드백을 제공해야 한다. 기체 수준 소프트웨어는 명령된 동작과 관측된 동작을 비교하여 추진 유효성(Propulsion Effectiveness)을 추정한다. 예상보다 낮은 추력을 생성하는 모터는 전기적 제한, 기계적 손상, 로터 성능 저하 또는 공기역학적 간섭을 나타낼 수 있다. 이러한 정보를 활용하면 완전한 추진 고장이 발생하기 전에 제어 할당을 조정할 수 있다.

열 관리(Thermal Management)는 추진 제어와 분리할 수 없다. 터빈, 발전기, 배터리, 인버터, 모터 및 냉각 시스템은 모두 온도에 따라 가용 성능이 달라지기 때문이다. 소프트웨어는 한계를 초과한 이후에만 대응하는 것이 아니라 열적 변화 추세를 예측해야 한다. 점진적 출력 저감(Progressive Derating)을 통해 보호 정지가 필요해지기 전에 출력을 낮출 수 있으며, 임무 계획에서는 주변 온도, 고도, 호버링 지속시간 및 예상 냉각 성능을 고려할 수 있다.

추력이 전기적으로 생성되더라도 연료 관리(Fuel Management)는 여전히 중요하다. 터빈의 연료 소비량은 운전점(Operating Point), 발전기 부하, 대기 조건 및 과도응답에 따라 달라진다. 에너지 관리 알고리즘은 터빈을 효율적인 운전 영역에서 동작시키고 배터리가 단시간의 변동을 처리하도록 스케줄링할 수 있다. 그러나 효율 최적화는 항상 안전, 추진 가용성, 예비 에너지 요구조건, 열적 제약 및 중요 비행 단계에서의 신속한 응답보다 낮은 우선순위를 가져야 한다.

추진 소프트웨어는 초기화(Initialization), 시동(Start), 예열(Warm-Up), 정상 발전(Normal Generation), 고출력 운전(High-Demand Operation), 성능 저하 운전(Degraded Operation), 비상 전력(Emergency Power), 냉각(Cooldown), 정지(Shutdown)와 같은 명확한 운용 모드를 사용해야 한다. 각 모드 사이의 전환에는 검증된 진입 조건과 제한된 동작(Bounded Action)이 필요하다. 모드 로직(Mode Logic)은 터빈이 유효한 운전 상태에 도달하기 전에 최대 발전기 출력을 요구하거나 중요한 과도상태에서 에너지 저장장치를 분리하는 것과 같은 상충 명령을 방지한다.

여러 터빈, 발전기, 배터리 및 전력 버스가 상호작용하면 시동 순서(Start Sequencing)는 더욱 복잡해진다. 소프트웨어는 시동을 시작하기 전에 연료 가용성, 배터리 상태, 접촉기(Contactor) 상태, 냉각 준비 상태, 통신 무결성 및 제어기 건전성을 확인해야 한다. 점화 이후에는 회전속도, 전압, 주파수, 온도 및 동기화 요구조건을 충족한 경우에만 발전기를 연결해야 한다. 시동 실패가 발생하면 통제되지 않은 반복 시동이 아니라 제어된 복구 절차로 전환해야 한다.

고장 탐지·격리·복구(Fault Detection, Isolation, and Recovery)는 기계 및 전기 영역 전체에서 수행되어야 한다. 터빈 과열, 발전기 고장, 인버터 고장, 배터리 이상, 버스 고장, 냉각 성능 저하, 센서 오류 및 모터 고장은 기체 수준에서 유사한 증상을 발생시킬 수 있다. 따라서 진단 소프트웨어(Diagnostic Software)는 구성요소 전반의 측정값을 상호 연계하여 장비를 격리하거나 추진 구성을 크게 변경하기 전에 가장 가능성이 높은 고장 원인을 식별해야 한다.

터빈 고장은 에너지 시스템과 비행 시스템의 통합된 대응을 요구한다. 필요한 경우 해당 발전기 채널을 격리하고, 배터리와 남아 있는 발전원이 즉시 필수 추진 부하를 지원해야 한다. 에너지 관리기는 변경된 조건에서 지속 가능한 전력을 계산하고 비행제어 시스템에는 갱신된 추력 한계가 전달된다. 이후 임무 소프트웨어는 속도, 상승 요구량 또는 기동성을 감소시키고 비행 지속, 회항(Diversion), 즉각적인 착륙 가운데 적절한 대응을 결정할 수 있다.

터빈 자체가 기계적으로 정상인 상태에서도 발전기 또는 전력 분배 고장이 발생할 수 있다. 아키텍처는 기계적 동력 생산 손실과 전기적 변환 또는 전달 손실을 구분해야 한다. 이러한 구분은 불필요한 터빈 정지를 방지하고 다른 버스 또는 변환기를 통해 전력을 재분배할 수 있는 대체 구성을 지원한다. 격리 로직(Isolation Logic)은 국부적인 전기 고장이 정상적인 전력 채널까지 붕괴시키기 전에 충분히 빠르게 동작해야 한다.

배터리 고장은 고에너지 저장장치가 열적 및 전기적 위험을 발생시킬 수 있으므로 특히 신중한 격리가 필요하다. 배터리 관리 소프트웨어(Battery-Management Software)는 셀 전압, 온도, 전류, 절연 상태, 충전상태(State of Charge) 및 건전상태(State of Health)를 감시한다. 비정상 모듈에는 전류 제한 또는 격리가 필요할 수 있다. 에너지 저장 용량이 제거되면 터빈 발전이 계속 가능하더라도 추진 응답 능력이 크게 감소할 수 있으므로 기체는 가용 과도 전력과 비상 전력을 다시 계산해야 한다.

추진 제어기 사이의 통신은 타임스탬프, 유효성 정보, 시퀀스 카운터(Sequence Counter) 및 무결성 검사를 포함하는 결정론적 인터페이스(Deterministic Interface)를 사용해야 한다. 명령과 측정값에는 정의된 갱신 주기와 타임아웃 동작이 있어야 한다. 통신 손실 이후 오래된 추력 또는 전력 명령이 무기한 활성 상태로 남아서는 안 된다. 로컬 제어기는 기체 수준 소프트웨어가 적절한 성능 저하 구성을 결정하는 동안 제한된 운용을 유지하는 안전 폴백 동작(Safe Fallback Behavior)을 가져야 한다.

시간 동기화(Time Synchronization)는 터빈, 발전기, 배터리, 모터 및 기체 동역학 사이의 정확한 상관관계 분석을 지원한다. 추력 부족이 발생하면 진단 기능은 모터 속도가 변화하기 전에 전력이 변화했는지, 터빈 응답이 발전기 요구량보다 지연되었는지, 또는 단순히 센서 보고가 늦었는지를 판단해야 한다. 추진 네트워크 전체에서 측정 타임스탬프를 보존하면 고장 격리, 과도상태 분석, 제어 튜닝 및 비행 후 조사(Post-Flight Investigation)의 정확성을 향상시킬 수 있다.

사이버보안(Cybersecurity)은 결정론적 제어를 저해하지 않으면서 추진 구성과 명령을 보호해야 한다. 보안 부팅(Secure Boot), 인증된 소프트웨어, 보호된 보정 데이터(Calibration Data), 접근이 통제된 정비 인터페이스 및 인증된 외부 업데이트를 통해 무단 변경의 위험을 줄일 수 있다. 네트워크 분할(Network Segmentation)을 통해 탑재물 또는 임무 애플리케이션이 저수준 추진 명령을 직접 전달하지 못하도록 해야 하며, 안전 게이트웨이(Safety Gateway)는 합법적인 기체 수준 제어에 필요한 인터페이스만 제공해야 한다.

추진 소프트웨어 검증(Propulsion Software Verification)에는 기계, 전기, 열 및 비행 동역학을 포괄하는 고충실도 모델(High-Fidelity Model)이 필요하다. 소프트웨어 인더루프(Software-in-the-Loop) 시험은 장시간 임무에서 에너지 관리 알고리즘을 평가할 수 있으며, 하드웨어 인더루프(Hardware-in-the-Loop) 환경에서는 제어기 타이밍, 버스 동작, 센서 및 통신 고장을 재현할 수 있다. 고장 주입(Fault Injection)에는 터빈 손실, 발전기 차단, 배터리 격리, 인버터 고장, 모터 성능 저하, 냉각 고장 및 복합적인 성능 저하 조건이 포함되어야 한다.

과도상태 시험(Transient Testing)은 하이브리드 시스템이 정상 상태에서는 안정적으로 보이면서도 급격한 전력 재분배 과정에서 실패할 수 있기 때문에 특히 중요하다. 검증 과정에서는 이륙 출력 적용, 급격한 상승 명령, 갑작스러운 추력 감소, 발전기 연결 및 분리, 터빈 고장, 배터리 전류 제한 및 비상 착륙을 평가해야 한다. 각 사건 전체에서 전압 변동, 열 응답, 제어 연속성 및 잔여 추진 여유(Remaining Propulsion Margin)를 측정해야 한다.

임무 계획(Mission Planning)은 고정된 성능 엔벌로프를 가정하는 대신 추진 능력(Propulsion Capability)을 동적으로 변화하는 제약조건으로 받아들여야 한다. 가용 전력은 연료량, 배터리 상태, 온도, 고도, 구성요소 건전성 및 이중화 상태에 따라 변화한다. 이러한 한계를 궤적 및 임무 소프트웨어에 제공하면 현재 에너지 구성으로 지속할 수 없는 상승, 속도 또는 호버링 시간을 기체가 요구하는 상황을 방지할 수 있다.

따라서 10톤급 터빈-하이브리드 추진 소프트웨어의 핵심 목표는 추진 권한(Propulsion Authority)과 에너지 가용성(Energy Availability)을 통합적으로 관리하는 것이다. 터빈은 높은 에너지 밀도를 기반으로 장시간 운용 능력을 제공하고, 전기 에너지 저장장치는 신속한 응답과 예비 능력을 제공하며, 분산 모터는 제어 가능한 추력을 생성한다. 비행제어, 에너지 관리, 열 보호, 고장 처리 및 전력 분배 시스템이 검증된 가용 능력 정보를 지속적으로 교환할 때 신뢰성 높은 운용이 가능해진다.

잘 설계된 시스템은 터빈 효율, 배터리 사용률 또는 모터 성능을 각각 독립적으로 최대화하려 하지 않는다. 대신 안전 여유(Safety Margin)와 점진적 성능 저하(Graceful Degradation)를 유지하면서 전체 기체 수준에서 최적화한다. 결정론적 제어, 예측형 에너지 관리(Predictive Energy Management), 이중화 전력 경로, 고장 격리 및 엄격한 검증을 통합함으로써 터빈-하이브리드 추진 소프트웨어는 자율 10톤급 화물 무인항공기 운용에 필요한 지속적인 출력, 신속한 응답 및 고장 허용성(Fault Tolerance)을 제공할 수 있다.

##  

## 11.05. 10t UAV Fly By Wire and Actuator Control [w/Code]

![](images/image5.png){width="7.268055555555556in" height="7.268055555555556in"}

A 10-ton cargo UAV requires a fly-by-wire system that converts autonomous guidance and flight-control commands into precise, bounded, and fault-tolerant actuator motion. Mechanical control authority is replaced by electronic sensing, computation, communication, and actuation, making software part of the primary flight-control path. The architecture must therefore preserve deterministic timing, signal integrity, redundancy, and predictable behavior after credible failures.

The fly-by-wire architecture should separate guidance objectives from actuator-level execution. Mission software generates trajectory objectives, the flight-control system calculates required forces and moments, and control allocation converts these demands into surface, rotor, thrust-vector, or other actuator commands. Local actuator controllers then close fast position, velocity, force, or torque loops. This hierarchy prevents high-level autonomy from directly commanding safety-critical hardware.

Command processing begins with validation before any requested motion reaches an actuator. Software should verify command source, freshness, sequence, range, rate, and compatibility with the current flight mode. Invalid, stale, discontinuous, or physically impossible commands are rejected or limited. This boundary protects the physical aircraft from communication faults, software anomalies, and inappropriate commands generated elsewhere in the autonomous system.

A heavy UAV may employ electromechanical, electrohydraulic, or hybrid actuation depending on control force, response, weight, and redundancy requirements. The flight-control software should interact through standardized actuator abstractions rather than embedding device-specific behavior throughout the control laws. Each actuator interface can expose commanded state, measured state, available authority, health, temperature, power consumption, and internal fault information.

Actuator dynamics must be represented explicitly because a 10-ton aircraft can require large forces and substantial mechanical travel. Position response, rate limits, acceleration limits, backlash, friction, compliance, hydraulic dynamics, and motor torque constraints affect achievable control performance. The flight-control law and allocator should therefore request motions that remain within validated dynamic capability rather than assuming that actuator commands are reproduced instantaneously.

Rate limiting is especially important for large control surfaces and high-force mechanisms. An abrupt command may exceed actuator power, produce structural loads, or generate undesirable aircraft transients even when the final position remains within limits. Command shaping, acceleration limiting, and jerk management can produce smoother actuator trajectories. These mechanisms should preserve rapid response for stabilization while restricting unnecessary high-frequency motion from slower guidance functions.

Position and force feedback provide evidence that commanded motion has actually occurred. Redundant sensors may measure actuator position, motor current, hydraulic pressure, torque, or surface displacement. Local software compares command and response to identify tracking errors, excessive resistance, runaway behavior, or loss of mechanical coupling. Aircraft-level software can then determine whether an actuator remains fully effective, degraded, jammed, or unavailable.

Redundancy is required where loss of a single actuator channel could threaten controlled flight. Multiple motors, duplicated electronics, independent power feeds, redundant position sensors, or mechanically separated actuation paths can provide continued authority after a failure. Redundancy management must consider independence, because two nominally separate channels connected to the same power converter, communication bus, or mechanical element may still share a common failure mode.

Fly-by-wire computers should also be redundant and continuously cross-monitored. Each control channel processes synchronized sensor data and produces expected actuator demands. Cross-channel comparison detects disagreement in computed commands, internal states, execution timing, or mode selection. Depending on the architecture, voting or independent safety monitoring can isolate a faulty computational channel while preserving uninterrupted control through healthy channels.

Communication between flight-control computers and remote actuator electronics must be deterministic. Critical command and feedback messages require bounded latency, predictable update periods, sequence counters, timestamps, source identification, and integrity protection. If messages are delayed or lost, the actuator controller must distinguish temporary network disturbance from sustained command loss and enter a predefined fallback state rather than continuing indefinitely with stale information.

Time synchronization is necessary because actuator feedback from different channels must represent comparable physical instants. A timestamped architecture allows control computers to distinguish real actuator disagreement from differences caused by communication delay. Accurate temporal alignment also improves fault diagnosis, control allocation, structural-load analysis, and post-flight reconstruction. Local clocks should maintain bounded drift during temporary loss of network synchronization.

Power availability directly affects actuator authority. Large electromechanical actuators can draw substantial current during rapid motion or high aerodynamic loading, while electrohydraulic systems depend on pump and pressure availability. The avionics architecture should therefore exchange power capability with actuator management. When electrical or hydraulic resources become limited, the controller can reduce nonessential motion and preserve authority for stabilization and landing.

Control allocation connects aircraft-level force and moment requirements to the available actuator set. A 10-ton UAV may combine aerodynamic surfaces, differential propulsion, thrust vectoring, rotor-speed control, or other effectors. The allocator should account for effectiveness, position, rate, saturation, power, and health constraints. When one effector degrades, control demand can be redistributed among remaining devices while maintaining the most important stabilization objectives.

Actuator saturation requires coordinated handling throughout the control hierarchy. When a surface reaches its travel limit or a motor reaches its maximum force, upstream controllers must know that additional command cannot be achieved. Without this feedback, integrators can accumulate error and generate severe transients when authority returns. Anti-windup logic, achievable-command estimation, and allocation residuals help maintain stable behavior near physical limits.

A jammed actuator creates a different problem from an actuator that simply loses power. The fixed surface or mechanism may continue generating an aerodynamic or mechanical effect that must be compensated by other effectors. Fault-management software should estimate the actual jam position and update the control-effectiveness model. Remaining actuators can then counter the persistent moment while the flight envelope is reduced to prevent exhaustion of residual control authority.

Runaway behavior requires rapid detection because an actuator moving without valid command can create large forces on a heavy aircraft. Monitoring logic can compare commanded direction, measured position, velocity, current, and expected dynamic response. When runaway is confirmed, the affected drive channel may need to be electrically isolated or mechanically disengaged. Other channels then compensate while the aircraft transitions to an appropriate degraded or emergency mode.

Sensor disagreement within an actuator must be resolved without unnecessary loss of control. Dual position sensors alone cannot always determine which measurement is correct when they disagree. Additional motor rotation, current, pressure, surface-position, or aircraft-response information can support fault discrimination. Triple sensing can enable voting, but common wiring, excitation, or environmental dependencies must still be considered in the safety analysis.

Flight-envelope protection should constrain actuator commands before mechanical or aerodynamic limits are reached. Surface position, hinge moment, structural load, angular rate, airspeed, acceleration, and load factor may all contribute to allowable command boundaries. These limits can vary with aircraft mass, center of gravity, speed, altitude, and configuration. Dynamic protection is therefore more effective than relying only on fixed actuator travel stops.

Structural-load protection becomes particularly important at the 10-ton scale. Large control surfaces and propulsion effectors can generate forces capable of exceeding local or global structural limits if commanded aggressively. Flight-control software can use estimated loads and measured accelerations to moderate actuator demand. Load alleviation functions may redistribute control effort across several effectors while retaining adequate trajectory and attitude control.

Mode management determines how actuator authority changes through takeoff, cruise, approach, landing, ground operation, and emergency conditions. Certain effectors may be enabled, limited, or assigned different control priorities depending on the flight phase. Mode transitions should be explicit, verified, and bumpless. Unexpected combinations of control modes must be prevented so that two functions do not issue incompatible demands to the same actuator.

Ground operation requires special interlocks because large actuators can present hazards to personnel, cargo equipment, and the aircraft itself. Software should restrict surface movement, rotor-related mechanisms, steering, doors, or thrust-vectoring devices according to maintenance and ground states. Transition to flight-ready configuration requires confirmation that locks, supports, access equipment, and actuator inhibit conditions have been properly cleared.

Built-in test functions should continuously evaluate actuator electronics, sensors, communication, power, and mechanical response. Startup tests can verify wiring, sensor plausibility, controller memory, and limited motion where safe, while continuous monitoring detects faults during operation. Maintenance diagnostics should preserve event histories and measured conditions so intermittent problems can be investigated rather than disappearing when the aircraft is powered down.

Degraded control modes should correspond to remaining physical authority. Loss of redundancy may permit continued flight with reduced maneuver limits, while a jammed primary effector may require lower speed, restricted turns, or immediate diversion. Multiple failures may leave only enough authority for controlled descent and landing. The software should calculate capability from actual surviving resources rather than relying only on fixed failure labels.

Software-in-the-loop testing should verify control laws, allocation, mode logic, saturation handling, and failure responses across the full operating envelope. Models should represent actuator rate limits, backlash, delays, nonlinear friction, structural flexibility, power constraints, and sensor errors. Monte Carlo testing can vary these parameters simultaneously to reveal conditions where acceptable nominal tracking conceals insufficient stability or redundancy margins.

Hardware-in-the-loop testing adds real controllers, communication networks, actuator electronics, and representative loads to the verification environment. Fault injection can reproduce frozen feedback, network interruption, power loss, sensor disagreement, actuator slowdown, runaway commands, and jam conditions without endangering the aircraft. Timing measurements should confirm that detection, isolation, reconfiguration, and fallback actions satisfy allocated safety requirements.

Flight testing should expand actuator authority progressively. Early tests confirm command direction, feedback polarity, rate limits, and basic closed-loop response before proceeding to higher speeds, larger loads, representative payloads, and degraded configurations. Measured actuator demand, tracking error, power consumption, structural response, and remaining authority should be compared with simulation predictions before additional portions of the envelope are released.

Cybersecurity is also relevant because fly-by-wire networks directly influence physical motion. Secure boot, authenticated software, protected calibration, controlled maintenance access, and authenticated command interfaces reduce unauthorized modification risk. Network segmentation should prevent payload or external applications from directly addressing actuator controllers. Security mechanisms must remain compatible with the deterministic timing required by flight-critical control.

The central objective of fly-by-wire and actuator-control software is to ensure that every physical control action is valid, achievable, observable, and fault contained. The architecture must know not only what motion was commanded but whether the actuator produced it and how much authority remains. This continuous capability awareness allows the aircraft to adapt control allocation and operating limits as physical resources change.

For a 10-ton autonomous cargo UAV, fly-by-wire is therefore more than replacement of mechanical linkages with electronics. It forms the safety-critical bridge between digital autonomy and high-energy physical motion. Through redundant computation, deterministic networks, actuator health monitoring, envelope protection, graceful reconfiguration, and rigorous verification, the system can preserve predictable control across nominal, degraded, and emergency flight conditions.

10톤급 화물 무인항공기(Cargo UAV)는 자율 유도(Autonomous Guidance) 및 비행제어 명령을 정밀하고 제한되며 고장 허용성을 갖춘 액추에이터 동작으로 변환하는 플라이바이와이어 시스템(Fly-By-Wire System)이 필요하다. 기계적 제어 권한(Mechanical Control Authority)은 전자식 센싱, 계산, 통신 및 구동으로 대체되므로 소프트웨어가 기본 비행제어 경로의 일부가 된다. 따라서 아키텍처는 결정론적 타이밍(Deterministic Timing), 신호 무결성(Signal Integrity), 이중화(Redundancy), 신뢰 가능한 고장 이후의 예측 가능한 동작을 보장해야 한다.

플라이바이와이어 아키텍처(Fly-By-Wire Architecture)는 유도 목표(Guidance Objective)와 액추에이터 수준 실행(Actuator-Level Execution)을 분리해야 한다. 임무 소프트웨어는 궤적 목표를 생성하고, 비행제어 시스템은 필요한 힘과 모멘트를 계산하며, 제어 할당(Control Allocation)은 이러한 요구량을 조종면, 로터, 추력 벡터 또는 기타 액추에이터 명령으로 변환한다. 이후 로컬 액추에이터 제어기(Local Actuator Controller)가 빠른 위치, 속도, 힘 또는 토크 루프를 폐루프로 제어한다. 이러한 계층 구조는 상위 자율 시스템이 안전 필수 하드웨어를 직접 명령하는 것을 방지한다.

명령 처리는 요청된 동작이 액추에이터에 도달하기 전에 수행되는 검증에서 시작된다. 소프트웨어는 명령의 출처, 최신성(Freshness), 순서, 범위, 변화율 및 현재 비행 모드와의 호환성을 확인해야 한다. 유효하지 않거나 오래되었거나 불연속적이거나 물리적으로 불가능한 명령은 거부하거나 제한한다. 이러한 경계는 통신 고장, 소프트웨어 이상 및 자율 시스템의 다른 영역에서 생성된 부적절한 명령으로부터 실제 기체를 보호한다.

대형 무인항공기는 제어력, 응답성, 중량 및 이중화 요구조건에 따라 전기기계식(Electromechanical), 전기유압식(Electrohydraulic) 또는 하이브리드 구동(Hybrid Actuation)을 사용할 수 있다. 비행제어 소프트웨어는 장치별 동작을 제어법칙 전체에 직접 포함하기보다는 표준화된 액추에이터 추상화(Standardized Actuator Abstraction)를 통해 상호작용해야 한다. 각 액추에이터 인터페이스는 명령 상태, 측정 상태, 가용 제어 권한, 건전성, 온도, 전력 소비 및 내부 고장 정보를 제공할 수 있다.

10톤급 항공기는 큰 힘과 상당한 기계적 이동량을 요구할 수 있으므로 액추에이터 동역학(Actuator Dynamics)을 명시적으로 표현해야 한다. 위치 응답, 변화율 한계, 가속도 한계, 백래시(Backlash), 마찰, 유연성(Compliance), 유압 동역학 및 모터 토크 제약은 달성 가능한 제어 성능에 영향을 준다. 따라서 비행제어법칙과 제어 할당기는 액추에이터 명령이 순간적으로 실현된다고 가정하지 않고 검증된 동적 성능 범위 안에서 동작을 요구해야 한다.

변화율 제한(Rate Limiting)은 대형 조종면과 고출력 구동장치에서 특히 중요하다. 최종 위치가 제한 범위 안에 있더라도 갑작스러운 명령은 액추에이터 전력 한계를 초과하거나 구조 하중을 발생시키고 바람직하지 않은 기체 과도응답을 유발할 수 있다. 명령 형상화(Command Shaping), 가속도 제한 및 저크 관리(Jerk Management)를 통해 보다 부드러운 액추에이터 궤적을 생성할 수 있다. 이러한 메커니즘은 안정화에 필요한 빠른 응답을 유지하면서 상대적으로 느린 유도 기능에서 발생하는 불필요한 고주파 동작을 제한해야 한다.

위치 및 힘 피드백(Position and Force Feedback)은 명령된 동작이 실제로 수행되었는지를 확인하는 근거를 제공한다. 이중화 센서는 액추에이터 위치, 모터 전류, 유압 압력, 토크 또는 조종면 변위를 측정할 수 있다. 로컬 소프트웨어는 명령과 응답을 비교하여 추종 오차, 과도한 저항, 폭주 동작(Runaway Behavior) 또는 기계적 연결 손실을 식별한다. 이후 기체 수준 소프트웨어는 해당 액추에이터가 정상, 성능 저하, 고착(Jammed) 또는 사용 불가능 상태인지를 판단할 수 있다.

하나의 액추에이터 채널 손실이 제어 비행을 위협할 수 있는 경우에는 이중화가 필요하다. 다중 모터, 이중화 전자장치, 독립 전원 공급, 이중화 위치 센서 또는 기계적으로 분리된 구동 경로를 통해 고장 이후에도 제어 권한을 유지할 수 있다. 이중화 관리는 독립성(Independence)을 고려해야 한다. 명목상 분리된 두 채널이라도 동일한 전력 변환기, 통신 버스 또는 기계적 요소에 연결되어 있다면 여전히 공통 고장 모드(Common Failure Mode)를 공유할 수 있기 때문이다.

플라이바이와이어 컴퓨터(Fly-By-Wire Computer) 역시 이중화되고 지속적으로 상호 감시되어야 한다. 각 제어 채널은 동기화된 센서 데이터를 처리하고 예상되는 액추에이터 요구량을 생성한다. 채널 간 비교(Cross-Channel Comparison)는 계산된 명령, 내부 상태, 실행 타이밍 또는 모드 선택의 불일치를 탐지한다. 아키텍처에 따라 투표(Voting) 또는 독립 안전 감시(Independent Safety Monitoring)를 통해 고장 난 계산 채널을 격리하면서 정상 채널을 이용하여 중단 없는 제어를 유지할 수 있다.

비행제어 컴퓨터와 원격 액추에이터 전자장치(Remote Actuator Electronics) 사이의 통신은 결정론적이어야 한다. 중요 명령 및 피드백 메시지는 제한된 지연시간(Bounded Latency), 예측 가능한 갱신 주기, 시퀀스 카운터(Sequence Counter), 타임스탬프, 송신원 식별정보 및 무결성 보호(Integrity Protection)를 가져야 한다. 메시지가 지연되거나 손실되면 액추에이터 제어기는 일시적인 네트워크 장애와 지속적인 명령 손실을 구분하고 오래된 정보를 계속 사용하는 대신 사전에 정의된 폴백 상태(Fallback State)로 전환해야 한다.

시간 동기화(Time Synchronization)는 서로 다른 채널의 액추에이터 피드백이 비교 가능한 물리적 시점을 나타내도록 하기 위해 필요하다. 타임스탬프 기반 아키텍처(Timestamped Architecture)는 제어 컴퓨터가 실제 액추에이터 불일치와 통신 지연으로 인한 차이를 구분할 수 있도록 한다. 정확한 시간 정렬(Temporal Alignment)은 고장 진단, 제어 할당, 구조 하중 분석 및 비행 후 재구성(Post-Flight Reconstruction)의 정확성도 향상시킨다. 로컬 클록은 네트워크 동기화가 일시적으로 손실되더라도 제한된 드리프트(Bounded Drift)를 유지해야 한다.

전력 가용성(Power Availability)은 액추에이터 제어 권한에 직접적인 영향을 준다. 대형 전기기계식 액추에이터는 빠른 움직임이나 높은 공력 하중에서 상당한 전류를 소비할 수 있으며, 전기유압식 시스템은 펌프 및 압력 가용성에 의존한다. 따라서 항공전자 아키텍처는 전력 가용 능력을 액추에이터 관리 시스템과 공유해야 한다. 전기 또는 유압 자원이 제한되면 제어기는 비필수 동작을 줄이고 안정화와 착륙에 필요한 제어 권한을 보존할 수 있다.

제어 할당(Control Allocation)은 기체 수준의 힘과 모멘트 요구량을 사용 가능한 액추에이터 집합에 연결한다. 10톤급 무인항공기는 공력 조종면, 차등 추진(Differential Propulsion), 추력 벡터링(Thrust Vectoring), 로터 속도 제어 또는 기타 효과기(Effector)를 조합할 수 있다. 제어 할당기는 유효성, 위치, 변화율, 포화, 전력 및 건전성 제약조건을 고려해야 한다. 하나의 효과기가 성능 저하를 일으키면 가장 중요한 안정화 목표를 유지하면서 나머지 장치에 제어 요구량을 재분배할 수 있다.

액추에이터 포화(Actuator Saturation)는 전체 제어 계층에서 통합적으로 처리되어야 한다. 조종면이 이동 한계에 도달하거나 모터가 최대 힘에 도달하면 상위 제어기는 추가적인 명령을 실현할 수 없다는 사실을 알아야 한다. 이러한 피드백이 없으면 적분기에 오차가 누적되고 제어 권한이 복원될 때 심각한 과도응답이 발생할 수 있다. 안티윈드업 로직(Anti-Windup Logic), 달성 가능한 명령 추정(Achievable-Command Estimation), 제어 할당 잔차(Control-Allocation Residual)는 물리적 한계 근처에서도 안정적인 동작을 유지하는 데 도움이 된다.

액추에이터 고착(Jammed Actuator)은 단순히 전력을 상실한 액추에이터와 다른 문제를 발생시킨다. 고정된 조종면이나 메커니즘은 계속해서 공기역학적 또는 기계적 영향을 발생시킬 수 있으며, 다른 효과기가 이를 보상해야 한다. 고장 관리 소프트웨어(Fault-Management Software)는 실제 고착 위치를 추정하고 제어 유효성 모델(Control-Effectiveness Model)을 갱신해야 한다. 이후 남은 액추에이터가 지속적인 모멘트를 상쇄하도록 하고 잔여 제어 권한이 소진되지 않도록 비행 엔벌로프를 축소할 수 있다.

폭주 동작(Runaway Behavior)은 유효한 명령 없이 움직이는 액추에이터가 대형 기체에 큰 힘을 발생시킬 수 있으므로 신속하게 탐지해야 한다. 감시 로직은 명령 방향, 측정 위치, 속도, 전류 및 예상되는 동적 응답을 비교할 수 있다. 폭주가 확인되면 해당 구동 채널을 전기적으로 격리하거나 기계적으로 분리해야 할 수 있다. 이후 다른 채널이 이를 보상하는 동안 기체는 적절한 성능 저하 모드(Degraded Mode) 또는 비상 모드(Emergency Mode)로 전환한다.

액추에이터 내부의 센서 불일치는 불필요한 제어 손실 없이 해결되어야 한다. 두 개의 위치 센서만 사용하는 경우 서로 다른 값을 보고할 때 어느 측정값이 정확한지를 항상 판단할 수 있는 것은 아니다. 추가적인 모터 회전량, 전류, 압력, 조종면 위치 또는 기체 응답 정보를 이용하여 고장을 판별할 수 있다. 삼중 센싱(Triple Sensing)을 사용하면 투표가 가능하지만 공통 배선, 여자 전원(Excitation) 또는 환경적 의존성도 안전성 분석에서 고려해야 한다.

비행 엔벌로프 보호(Flight-Envelope Protection)는 기계적 또는 공기역학적 한계에 도달하기 전에 액추에이터 명령을 제한해야 한다. 조종면 위치, 힌지 모멘트(Hinge Moment), 구조 하중, 각속도, 대기속도, 가속도 및 하중계수(Load Factor)는 모두 허용 가능한 명령 경계를 결정하는 요소가 될 수 있다. 이러한 제한은 기체 질량, 무게중심, 속도, 고도 및 구성에 따라 변화할 수 있다. 따라서 고정된 액추에이터 이동 제한에만 의존하는 것보다 동적 보호(Dynamic Protection)를 적용하는 것이 효과적이다.

구조 하중 보호(Structural-Load Protection)는 10톤급 기체에서 특히 중요하다. 대형 조종면과 추진 효과기는 과도하게 명령될 경우 국부 또는 전체 구조 한계를 초과하는 힘을 생성할 수 있다. 비행제어 소프트웨어는 추정된 하중과 측정된 가속도를 이용하여 액추에이터 요구량을 조절할 수 있다. 하중 완화 기능(Load Alleviation Function)은 충분한 궤적 및 자세 제어 능력을 유지하면서 여러 효과기에 제어력을 재분배할 수 있다.

모드 관리(Mode Management)는 이륙, 순항, 접근, 착륙, 지상 운용 및 비상 조건에서 액추에이터 제어 권한이 어떻게 변화하는지를 결정한다. 특정 효과기는 비행 단계에 따라 활성화되거나 제한될 수 있으며 서로 다른 제어 우선순위를 부여받을 수 있다. 모드 전환은 명시적이고 검증 가능하며 무충격(Bumpless)으로 수행되어야 한다. 두 기능이 동일한 액추에이터에 서로 호환되지 않는 요구량을 전달하지 않도록 예상하지 못한 제어 모드 조합을 방지해야 한다.

지상 운용(Ground Operation)에서는 대형 액추에이터가 작업자, 화물 장비 및 기체 자체에 위험을 초래할 수 있으므로 특별한 인터록(Interlock)이 필요하다. 소프트웨어는 정비 및 지상 상태에 따라 조종면 움직임, 로터 관련 메커니즘, 조향 장치, 도어 또는 추력 벡터링 장치의 작동을 제한해야 한다. 비행 준비 상태로 전환하려면 잠금장치, 지지장치, 접근 장비 및 액추에이터 작동 금지 조건이 적절하게 해제되었는지 확인해야 한다.

내장 시험 기능(Built-In Test Function)은 액추에이터 전자장치, 센서, 통신, 전력 및 기계적 응답을 지속적으로 평가해야 한다. 시동 시험에서는 안전이 보장되는 범위에서 배선, 센서 타당성, 제어기 메모리 및 제한된 동작을 검증할 수 있으며, 지속적인 감시는 운용 중 발생하는 고장을 탐지한다. 정비 진단(Maintenance Diagnostics)은 간헐적인 문제가 기체 전원을 차단한 이후 사라져 원인을 찾을 수 없게 되는 것을 방지하도록 이벤트 이력과 측정 조건을 보존해야 한다.

성능 저하 제어 모드(Degraded Control Mode)는 실제로 남아 있는 물리적 제어 권한과 대응되어야 한다. 이중화 일부가 손실되더라도 기동 한계를 낮추어 비행을 계속할 수 있지만, 주요 효과기가 고착되면 속도 감소, 선회 제한 또는 즉각적인 회항이 필요할 수 있다. 여러 고장이 동시에 발생하면 제어된 하강 및 착륙에 필요한 최소한의 제어 권한만 남을 수 있다. 소프트웨어는 고정된 고장 명칭에만 의존하지 않고 실제 생존 자원(Surviving Resource)을 기준으로 가용 능력을 계산해야 한다.

소프트웨어 인더루프 시험(Software-in-the-Loop Testing)은 전체 운용 엔벌로프에서 제어법칙, 제어 할당, 모드 로직, 포화 처리 및 고장 대응을 검증해야 한다. 모델은 액추에이터 변화율 제한, 백래시, 지연, 비선형 마찰(Nonlinear Friction), 구조적 유연성, 전력 제약 및 센서 오류를 표현해야 한다. 몬테카를로 시험(Monte Carlo Testing)을 통해 이러한 파라미터를 동시에 변화시키면 정상적인 추종 성능이 부족한 안정성 또는 이중화 여유를 감추는 조건을 식별할 수 있다.

하드웨어 인더루프 시험(Hardware-in-the-Loop Testing)은 실제 제어기, 통신 네트워크, 액추에이터 전자장치 및 대표 하중(Representative Load)을 검증 환경에 추가한다. 고장 주입(Fault Injection)을 통해 기체를 위험에 노출하지 않고 고정된 피드백(Frozen Feedback), 네트워크 중단, 전원 손실, 센서 불일치, 액추에이터 속도 저하, 폭주 명령 및 고착 조건을 재현할 수 있다. 타이밍 측정을 통해 고장 탐지, 격리, 재구성 및 폴백 동작이 할당된 안전 요구조건을 충족하는지 확인해야 한다.

비행시험(Flight Testing)은 액추에이터 제어 권한을 점진적으로 확대하는 방식으로 수행해야 한다. 초기 시험에서는 명령 방향, 피드백 극성(Feedback Polarity), 변화율 제한 및 기본 폐루프 응답을 확인한 후 더 높은 속도, 더 큰 하중, 대표 탑재중량 및 성능 저하 구성으로 확장한다. 추가적인 비행 엔벌로프를 승인하기 전에 측정된 액추에이터 요구량, 추종 오차, 전력 소비, 구조 응답 및 잔여 제어 권한을 시뮬레이션 예측값과 비교해야 한다.

플라이바이와이어 네트워크가 실제 물리적 움직임에 직접 영향을 주기 때문에 사이버보안(Cybersecurity)도 중요하다. 보안 부팅(Secure Boot), 인증된 소프트웨어, 보호된 보정 데이터(Protected Calibration), 통제된 정비 접근 및 인증된 명령 인터페이스를 통해 무단 변경 위험을 줄일 수 있다. 네트워크 분할(Network Segmentation)은 탑재물 또는 외부 애플리케이션이 액추에이터 제어기에 직접 접근하는 것을 방지해야 한다. 보안 메커니즘은 비행 필수 제어에 요구되는 결정론적 타이밍과 호환되어야 한다.

플라이바이와이어 및 액추에이터 제어 소프트웨어(Fly-By-Wire and Actuator-Control Software)의 핵심 목표는 모든 물리적 제어 동작이 유효하고, 달성 가능하며, 관측 가능하고, 고장 격리될 수 있도록 보장하는 것이다. 아키텍처는 어떠한 동작이 명령되었는지만 아는 것이 아니라 액추에이터가 실제로 해당 동작을 수행했는지, 그리고 어느 정도의 제어 권한이 남아 있는지도 파악해야 한다. 이러한 지속적인 가용 능력 인식(Capability Awareness)을 통해 물리적 자원이 변화함에 따라 제어 할당과 운용 한계를 적응적으로 조정할 수 있다.

따라서 10톤급 자율 화물 무인항공기에서 플라이바이와이어(Fly-By-Wire)는 단순히 기계적 연결장치를 전자장치로 대체하는 기술 이상의 의미를 가진다. 이는 디지털 자율 시스템(Digital Autonomy)과 고에너지 물리적 운동(High-Energy Physical Motion)을 연결하는 안전 필수 인터페이스를 형성한다. 이중화 계산, 결정론적 네트워크, 액추에이터 건전성 감시, 비행 엔벌로프 보호, 점진적 재구성(Graceful Reconfiguration) 및 엄격한 검증을 통합함으로써 정상, 성능 저하 및 비상 비행 조건 전반에서 예측 가능한 제어를 유지할 수 있다.

##  

## 11.06. 5t 10t Cargo Load CoG Adaptive Control [w/Code]

![](images/image6.png){width="7.268055555555556in" height="7.268055555555556in"}

A 5-ton or 10-ton cargo UAV can experience large changes in total mass, center of gravity, inertia, and structural loading between missions. Cargo position may also change slightly during operation because of suspension compliance, fuel consumption, liquid motion, or imperfect restraint. Cargo-load and center-of-gravity adaptive control must therefore preserve stability and predictable handling across a substantially wider mass-property envelope than that of a fixed-configuration aircraft.

The control architecture should begin with an explicit aircraft mass-property model. Before flight, cargo weight, location, mounting geometry, fuel quantity, battery configuration, and other variable masses are combined with the empty-aircraft model to estimate total mass, center of gravity, and inertia tensor. These values become configuration inputs to flight control, trajectory planning, propulsion management, structural protection, and performance prediction.

Preflight information should not be trusted without validation. Cargo manifests and loading-system measurements can contain errors, while actual restraint geometry may differ from nominal configuration. Load cells, landing-gear measurements, suspension sensors, actuator trim, or other observations can provide independent evidence of aircraft mass and balance. Software should compare declared and measured values and prevent departure when disagreement exceeds validated limits.

Center-of-gravity location directly affects the moments required to maintain attitude and trajectory. A longitudinal shift changes pitch trim and available pitch authority, while lateral displacement creates roll demand and unequal actuator loading. Vertical displacement can also influence coupling between translational and rotational motion. The control system should therefore represent center of gravity as a three-dimensional property rather than treating balance only as a longitudinal loading constraint.

Vehicle inertia changes with both cargo mass and its distance from the aircraft reference axes. Two missions with identical gross mass can therefore exhibit different angular acceleration and disturbance response. Adaptive control should account for these differences when scheduling gains, predicting rotational motion, and allocating actuator authority. A controller tuned only for nominal inertia may become unnecessarily sluggish or excessively aggressive at another loading condition.

Gain scheduling provides a practical method for handling known loading variation. Validated controller gains can be defined across mass, center-of-gravity, airspeed, altitude, and configuration regions and interpolated during operation. Scheduling should be bounded within a certified or verified envelope so that uncertain estimates cannot generate arbitrary control parameters. Transition between gain sets must remain smooth to avoid introducing control discontinuities.

Model-based adaptation can complement gain scheduling when actual vehicle behavior differs from the preflight estimate. The controller can observe relationships among commanded forces, measured accelerations, actuator activity, and angular response to refine estimates of mass or inertia. Adaptation should occur slowly enough to distinguish persistent model mismatch from temporary disturbances such as wind gusts, turbulence, or ground effect.

Online center-of-gravity estimation can use persistent trim requirements as an important source of information. If the aircraft repeatedly requires additional pitch or roll moment under otherwise steady conditions, the software can evaluate whether the behavior is consistent with a displaced center of gravity. Such inference should be combined with propulsion, aerodynamic, wind, and actuator-health information because trim bias alone does not uniquely identify cargo displacement.

Cargo movement requires faster recognition than ordinary model adaptation. A sudden shift can create an abrupt moment, change inertia, and alter structural loads simultaneously. Detection logic can monitor unexpected angular acceleration, actuator demand, load-cell changes, restraint sensors, and differences between predicted and measured motion. When several indicators agree, the system can classify the event as probable load shift rather than treating it only as an external disturbance.

After a detected cargo shift, stabilization receives the highest priority. The controller should first arrest excessive attitude and angular-rate motion using available authority, then estimate the revised trim condition and determine whether continued flight is feasible. Mission tracking can temporarily be relaxed while the system evaluates center-of-gravity limits, actuator margins, structural loads, and the possibility of additional cargo movement.

Control allocation must reflect the changed mass properties. Distributed propulsion, aerodynamic surfaces, thrust-vectoring devices, or other effectors may have different effectiveness when the center of gravity moves. The allocator should use an updated control-effectiveness model and preserve sufficient reserve authority in critical axes. A feasible solution with almost no remaining margin may be unacceptable for continued operation in turbulence or during landing.

Actuator trim is a useful indicator of loading condition but also a finite resource. Persistent asymmetric thrust or surface deflection consumes control authority that would otherwise be available for gust rejection and maneuvering. The software should therefore monitor not only whether equilibrium can be maintained but how much actuator margin remains. Excessive trim demand can trigger reduced speed, maneuver restrictions, diversion, or landing.

Propulsion requirements change directly with gross mass. Heavier loading increases hover or climb power and can reduce reserve thrust available after a propulsion failure. The adaptive-control system should exchange current mass estimates with propulsion and energy management so that trajectory commands remain compatible with available power. A mission that is feasible at takeoff may become constrained by temperature, energy state, or degraded propulsion later in flight.

For turbine-hybrid or distributed-electric aircraft, loading information also influences energy strategy. High mass may require greater electrical buffering during takeoff and landing, while an unfavorable center of gravity may create continuous differential-thrust demand. Energy-management software should account for this additional control power rather than budgeting only for symmetric propulsion. Reserve calculations must include the power needed to maintain controllability.

Structural protection is closely coupled with cargo-load adaptation. Increased mass or displaced cargo can change bending moments, attachment loads, landing loads, and allowable maneuver acceleration. Flight-envelope limits should therefore be adjusted according to current mass properties. The same commanded acceleration may be acceptable for a lightly loaded aircraft but exceed structural or cargo-restraint limits near maximum payload.

Cargo restraint should be treated as a monitored safety subsystem when instrumentation is available. Lock status, tension, load distribution, door condition, attachment sensors, or pallet interfaces can provide evidence that the payload remains secured. Flight software does not need to control every mechanical restraint directly, but it should receive enough status information to determine whether the assumed mass configuration remains trustworthy during flight.

Liquid or suspended cargo creates additional dynamic behavior because the payload can move relative to the aircraft. Sloshing, pendulum motion, or flexible suspension introduces modes that may interact with flight control. Filters, command shaping, reduced maneuver rates, or model-based damping can prevent the controller from exciting these dynamics. The allowable envelope may need to depend on cargo type as well as total weight.

Trajectory planning should incorporate mass and center-of-gravity constraints before issuing references to flight control. Acceleration, turn rate, climb rate, descent profile, and landing approach can be adjusted according to current capability. When loading is near a limiting condition, smoother trajectories reduce actuator saturation, structural stress, cargo motion, and energy consumption while increasing the reserve available for disturbance rejection.

Takeoff provides an early opportunity to validate the preflight mass model. Once the aircraft becomes airborne, measured acceleration and thrust can be compared with predicted response. Significant mismatch may indicate incorrect mass data, propulsion underperformance, sensor error, or unexpected aerodynamic effects. The system can restrict envelope expansion until the discrepancy is understood rather than immediately proceeding with the planned mission.

Landing requires accurate knowledge of mass and balance because touchdown energy, flare behavior, descent arrest, and ground-load distribution depend on the current configuration. Cargo may also have changed position during the mission. The controller should use the latest validated estimates rather than relying exclusively on departure data. Approach speed, descent rate, thrust reserve, and touchdown criteria can then be adapted to the actual vehicle state.

Fault diagnosis must distinguish mass-property changes from actuator, propulsion, sensor, and aerodynamic failures. An unexpected roll trim could result from lateral cargo displacement, a weak motor, a damaged control surface, or crosswind. Diagnostic software should correlate load measurements, actuator response, propulsion feedback, navigation data, and estimated external forces before selecting a corrective action. Incorrect diagnosis could unnecessarily remove healthy control resources.

Adaptive functions require strict authority limits because an incorrect estimator must not be allowed to destabilize the aircraft. Estimated mass, center of gravity, inertia, and controller parameters should remain inside validated bounds. Rapid or implausible changes can be rejected, frozen, or referred to a degraded mode. Baseline robust control should retain sufficient stability even when adaptation is unavailable or temporarily inhibited.

Confidence information should accompany every estimated mass property. The controller may have high confidence in measured total mass but lower confidence in lateral center of gravity or inertia. Flight-envelope management can use this uncertainty explicitly by increasing safety margins when confidence decreases. This is preferable to presenting uncertain estimates as exact values and unknowingly operating close to a physical limit.

Redundant sensing improves integrity of load and balance estimation. Multiple load measurements, independent cargo-position sensors, inertial observations, and propulsion-response models can cross-check one another. Common-cause errors must still be considered, particularly when several estimates depend on the same configuration database or calibration. Software should preserve measurement provenance so diagnostic functions understand which estimates are truly independent.

Deterministic timing remains important because adaptive estimates affect flight-critical control. Mass-property updates can occur more slowly than inner-loop stabilization, but their timestamps and validity must be defined. A newly calculated center of gravity should not be applied inconsistently across control channels. Configuration updates should be synchronized, versioned, and introduced through controlled transitions so redundant computers operate with equivalent models.

Software-in-the-loop testing should cover the complete loading envelope, including combinations of maximum mass, extreme center-of-gravity positions, inertia variation, wind, actuator limitations, and propulsion degradation. Cargo shifts can be injected at different flight phases and rates. Monte Carlo testing can expose combinations where individual parameters are acceptable but their interaction leaves insufficient stability, structural, energy, or actuator margin.

Hardware-in-the-loop testing should include representative load sensors, flight computers, actuator interfaces, propulsion controllers, and communication delays. Tests can introduce erroneous cargo data, sensor drift, sudden load shifts, stale configuration messages, and disagreement between redundant estimators. Verification should confirm that adaptation remains bounded and that incorrect estimates lead to safe fallback behavior rather than uncontrolled controller changes.

Flight-test expansion should use carefully defined payload configurations whose mass properties are independently measured. Testing can progress from nominal center of gravity toward approved boundaries while monitoring trim, control margins, structural response, energy use, and estimator accuracy. The objective is not merely to demonstrate that the aircraft can fly at each loading point, but to validate the predicted margins between nominal behavior and control limits.

A 5-ton and a 10-ton UAV can share the same conceptual adaptive-control framework while using different parameter ranges, structural limits, actuator models, and safety margins. Common software services for mass-property estimation, confidence management, configuration validation, and capability reporting can improve reuse. Vehicle-specific control laws then apply these services according to the dynamics and redundancy architecture of each aircraft class.

The fundamental objective is to make cargo configuration an observable and actively managed part of flight control rather than a fixed assumption established before departure. By combining preflight mass modeling, online estimation, bounded adaptation, control allocation, structural protection, propulsion coordination, and fault diagnosis, heavy cargo UAVs can preserve predictable behavior as payload conditions vary.

For 5-ton and 10-ton autonomous cargo aircraft, successful adaptive control does not mean continuously changing the controller without restriction. It means adjusting verified control behavior within known boundaries while retaining robust fallback modes when information becomes uncertain. This approach allows large UAVs to accommodate practical payload variation while maintaining measurable stability, controllability, structural, and energy margins throughout the mission.

5톤 또는 10톤급 화물 무인항공기(Cargo UAV)는 임무에 따라 전체 질량, 무게중심(Center of Gravity), 관성(Inertia), 구조 하중(Structural Loading)이 크게 변화할 수 있다. 또한 서스펜션의 유연성, 연료 소비, 액체 화물의 움직임 또는 불완전한 고정으로 인해 운용 중 화물 위치가 소폭 변화할 수도 있다. 따라서 화물 하중 및 무게중심 적응제어(Cargo-Load and Center-of-Gravity Adaptive Control)는 고정 형상 항공기보다 훨씬 넓은 질량 특성 엔벌로프(Mass-Property Envelope)에서 안정성과 예측 가능한 조종 특성을 유지해야 한다.

제어 아키텍처(Control Architecture)는 명시적인 기체 질량 특성 모델(Aircraft Mass-Property Model)에서 시작해야 한다. 비행 전 화물 중량, 위치, 장착 형상, 연료량, 배터리 구성 및 기타 가변 질량을 공허중량 기체 모델(Empty-Aircraft Model)과 결합하여 전체 질량, 무게중심 및 관성 텐서(Inertia Tensor)를 추정한다. 이러한 값은 비행제어, 궤적 계획, 추진 관리, 구조 보호 및 성능 예측을 위한 형상 입력(Configuration Input)으로 사용된다.

비행 전 정보(Preflight Information)는 검증 없이 신뢰해서는 안 된다. 화물 적하목록(Cargo Manifest)과 적재 시스템의 측정값에는 오류가 포함될 수 있으며, 실제 고정 형상이 명목상의 구성과 다를 수도 있다. 로드셀(Load Cell), 착륙장치 측정값, 서스펜션 센서, 액추에이터 트림(Actuator Trim) 또는 기타 관측값은 기체 질량과 균형에 대한 독립적인 근거를 제공할 수 있다. 소프트웨어는 신고된 값과 측정된 값을 비교하고 그 차이가 검증된 한계를 초과하면 출발을 방지해야 한다.

무게중심 위치는 자세와 궤적을 유지하는 데 필요한 모멘트에 직접적인 영향을 준다. 종방향 이동(Longitudinal Shift)은 피치 트림(Pitch Trim)과 가용 피치 제어 권한을 변화시키며, 횡방향 변위(Lateral Displacement)는 롤 요구량과 불균등한 액추에이터 하중을 발생시킨다. 수직방향 변위도 병진 운동과 회전 운동 사이의 결합에 영향을 줄 수 있다. 따라서 제어 시스템은 균형을 단순한 종방향 적재 제약으로 처리하지 않고 무게중심을 3차원 특성(Three-Dimensional Property)으로 표현해야 한다.

기체 관성은 화물 질량뿐 아니라 기체 기준축으로부터 화물이 위치한 거리에 따라서도 변화한다. 따라서 동일한 총중량을 갖는 두 임무라도 각가속도와 외란 응답 특성이 서로 다를 수 있다. 적응제어(Adaptive Control)는 이득을 스케줄링하고 회전 운동을 예측하며 액추에이터 제어 권한을 할당할 때 이러한 차이를 고려해야 한다. 명목 관성만을 기준으로 조정된 제어기는 다른 적재 조건에서 지나치게 느리거나 과도하게 공격적으로 동작할 수 있다.

이득 스케줄링(Gain Scheduling)은 알려진 적재 변화를 처리하기 위한 실용적인 방법을 제공한다. 검증된 제어기 이득을 질량, 무게중심, 대기속도, 고도 및 기체 구성 영역에 따라 정의하고 운용 중 보간(Interpolation)할 수 있다. 불확실한 추정값으로 임의의 제어 파라미터가 생성되지 않도록 스케줄링은 인증되거나 검증된 엔벌로프 안에서 제한되어야 한다. 제어 불연속이 발생하지 않도록 이득 집합 사이의 전환도 부드럽게 수행되어야 한다.

모델 기반 적응(Model-Based Adaptation)은 실제 기체 동작이 비행 전 추정값과 다를 때 이득 스케줄링을 보완할 수 있다. 제어기는 명령된 힘, 측정된 가속도, 액추에이터 동작 및 각운동 응답 사이의 관계를 관측하여 질량 또는 관성 추정값을 개선할 수 있다. 적응은 지속적인 모델 불일치와 돌풍, 난류 또는 지면 효과(Ground Effect)와 같은 일시적인 외란을 구분할 수 있을 정도로 충분히 느리게 이루어져야 한다.

온라인 무게중심 추정(Online Center-of-Gravity Estimation)은 지속적인 트림 요구량을 중요한 정보원으로 활용할 수 있다. 비교적 안정된 조건에서도 기체가 반복적으로 추가적인 피치 또는 롤 모멘트를 요구한다면 소프트웨어는 이러한 동작이 무게중심 이동과 일치하는지를 평가할 수 있다. 그러나 트림 바이어스(Trim Bias)만으로 화물 변위를 유일하게 식별할 수는 없으므로 이러한 추론은 추진, 공력, 바람 및 액추에이터 건전성 정보와 함께 사용해야 한다.

화물 이동(Cargo Movement)은 일반적인 모델 적응보다 빠르게 인식해야 한다. 갑작스러운 이동은 급격한 모멘트를 발생시키고 관성을 변화시키며 동시에 구조 하중을 변화시킬 수 있다. 탐지 로직은 예상하지 못한 각가속도, 액추에이터 요구량, 로드셀 변화, 고정장치 센서(Restraint Sensor), 예측 운동과 실제 측정 운동 사이의 차이를 감시할 수 있다. 여러 지표가 일치하면 시스템은 이를 단순한 외부 외란이 아니라 화물 이동 가능성(Probable Load Shift)으로 분류할 수 있다.

화물 이동이 탐지된 이후에는 안정화(Stabilization)가 가장 높은 우선순위를 가진다. 제어기는 먼저 가용 제어 권한을 사용하여 과도한 자세 및 각속도 운동을 억제한 다음 변경된 트림 상태를 추정하고 비행을 계속할 수 있는지를 판단해야 한다. 시스템이 무게중심 한계, 액추에이터 여유, 구조 하중 및 추가 화물 이동 가능성을 평가하는 동안 임무 궤적 추종 성능은 일시적으로 완화될 수 있다.

제어 할당(Control Allocation)은 변경된 질량 특성을 반영해야 한다. 분산 추진(Distributed Propulsion), 공력 조종면, 추력 벡터링 장치 또는 기타 효과기(Effector)는 무게중심이 이동하면 서로 다른 제어 유효성을 가질 수 있다. 제어 할당기는 갱신된 제어 유효성 모델(Control-Effectiveness Model)을 사용하고 중요한 축에서 충분한 예비 제어 권한을 유지해야 한다. 거의 모든 제어 여유를 소진하는 실현 가능한 해는 난류 또는 착륙 운용을 계속하기에는 허용되지 않을 수 있다.

액추에이터 트림은 적재 상태를 나타내는 유용한 지표이지만 동시에 유한한 자원이다. 지속적인 비대칭 추력 또는 조종면 변위는 돌풍 대응과 기동에 사용할 수 있는 제어 권한을 소비한다. 따라서 소프트웨어는 단순히 평형 상태를 유지할 수 있는지만 판단하는 것이 아니라 얼마나 많은 액추에이터 여유(Actuator Margin)가 남아 있는지도 감시해야 한다. 과도한 트림 요구량은 속도 감소, 기동 제한, 회항(Diversion) 또는 착륙을 유발할 수 있다.

추진 요구량(Propulsion Requirement)은 총중량에 따라 직접 변화한다. 적재량이 증가하면 호버링 또는 상승에 필요한 출력이 증가하며 추진계 고장 이후 사용할 수 있는 예비 추력이 감소할 수 있다. 적응제어 시스템은 현재 질량 추정값을 추진 및 에너지 관리 시스템과 공유하여 궤적 명령이 가용 출력과 일치하도록 해야 한다. 이륙 시에는 실행 가능한 임무라도 비행 후반부에는 온도, 에너지 상태 또는 추진계 성능 저하로 인해 제한될 수 있다.

터빈-하이브리드(Turbine-Hybrid) 또는 분산 전기 추진(Distributed-Electric Propulsion) 항공기의 경우 적재 정보는 에너지 전략에도 영향을 준다. 높은 질량은 이륙과 착륙 중 더 많은 전기적 버퍼링(Electrical Buffering)을 요구할 수 있으며, 불리한 무게중심은 지속적인 차등 추력(Differential Thrust)을 필요로 할 수 있다. 에너지 관리 소프트웨어는 대칭 추진에 필요한 전력만 계산하지 않고 이러한 추가적인 제어 전력까지 고려해야 한다. 예비 에너지 계산에는 조종성을 유지하는 데 필요한 전력이 포함되어야 한다.

구조 보호(Structural Protection)는 화물 하중 적응과 밀접하게 결합되어 있다. 질량 증가 또는 화물 위치 변화는 굽힘 모멘트(Bending Moment), 장착부 하중, 착륙 하중 및 허용 기동 가속도를 변화시킬 수 있다. 따라서 비행 엔벌로프 한계(Flight-Envelope Limit)는 현재 질량 특성에 따라 조정되어야 한다. 동일한 명령 가속도라도 가벼운 적재 상태에서는 허용될 수 있지만 최대 탑재중량에 가까운 상태에서는 구조 또는 화물 고정 한계를 초과할 수 있다.

계측 장비가 제공되는 경우 화물 고정장치(Cargo Restraint)는 감시되는 안전 하위 시스템(Safety Subsystem)으로 취급해야 한다. 잠금 상태, 장력, 하중 분포, 도어 상태, 부착 센서 또는 팔레트 인터페이스(Pallet Interface)는 탑재물이 계속 안전하게 고정되어 있다는 근거를 제공할 수 있다. 비행 소프트웨어가 모든 기계적 고정장치를 직접 제어할 필요는 없지만, 비행 중 가정된 질량 구성을 계속 신뢰할 수 있는지를 판단할 수 있을 만큼 충분한 상태 정보를 제공받아야 한다.

액체 또는 현수 화물(Suspended Cargo)은 탑재물이 기체에 대해 상대적으로 움직일 수 있으므로 추가적인 동적 거동을 발생시킨다. 슬로싱(Sloshing), 진자 운동(Pendulum Motion) 또는 유연한 서스펜션은 비행제어와 상호작용할 수 있는 동적 모드를 도입한다. 필터, 명령 형상화(Command Shaping), 기동률 감소 또는 모델 기반 감쇠(Model-Based Damping)를 통해 제어기가 이러한 동역학을 자극하는 것을 방지할 수 있다. 허용 운용 엔벌로프는 전체 중량뿐 아니라 화물 유형에 따라서도 달라질 수 있다.

궤적 계획(Trajectory Planning)은 비행제어에 기준 명령을 전달하기 전에 질량과 무게중심 제약조건을 반영해야 한다. 가속도, 선회율, 상승률, 하강 프로파일 및 착륙 접근을 현재 가용 능력에 따라 조정할 수 있다. 적재 상태가 한계에 가까울 경우 더욱 부드러운 궤적을 사용하면 액추에이터 포화, 구조 응력, 화물 움직임 및 에너지 소비를 감소시키면서 외란 대응에 사용할 수 있는 예비 여유를 증가시킬 수 있다.

이륙은 비행 전 질량 모델을 검증할 수 있는 초기 기회를 제공한다. 기체가 공중에 뜬 이후 측정된 가속도와 추력을 예측 응답과 비교할 수 있다. 큰 차이가 발생하면 잘못된 질량 데이터, 추진 성능 저하, 센서 오류 또는 예상하지 못한 공력 효과를 의미할 수 있다. 시스템은 즉시 계획된 임무를 계속 수행하기보다 이러한 차이가 충분히 파악될 때까지 비행 엔벌로프 확대를 제한할 수 있다.

착륙은 접지 에너지(Touchdown Energy), 플레어 동작(Flare Behavior), 하강 정지 및 지상 하중 분포가 현재 기체 구성에 따라 달라지므로 정확한 질량 및 균형 정보가 필요하다. 임무 중 화물 위치가 변경되었을 수도 있다. 따라서 제어기는 출발 시점의 데이터에만 의존하지 않고 가장 최근에 검증된 추정값을 사용해야 한다. 이를 통해 접근 속도, 하강률, 추력 여유 및 접지 기준(Touchdown Criteria)을 실제 기체 상태에 맞게 조정할 수 있다.

고장 진단(Fault Diagnosis)은 질량 특성 변화와 액추에이터, 추진계, 센서 및 공력 고장을 구분해야 한다. 예상하지 못한 롤 트림은 횡방향 화물 이동, 약화된 모터, 손상된 조종면 또는 횡풍에 의해 발생할 수 있다. 진단 소프트웨어는 교정 동작을 선택하기 전에 하중 측정값, 액추에이터 응답, 추진 피드백, 항법 데이터 및 추정된 외력을 상호 연계해야 한다. 잘못된 진단은 정상적인 제어 자원을 불필요하게 제거할 수 있다.

적응 기능(Adaptive Function)은 잘못된 추정기가 기체를 불안정하게 만들지 못하도록 엄격한 권한 제한(Authority Limit)을 가져야 한다. 추정된 질량, 무게중심, 관성 및 제어기 파라미터는 검증된 범위 안에 유지되어야 한다. 급격하거나 물리적으로 타당하지 않은 변화는 거부하거나 고정하고 성능 저하 모드로 전환할 수 있다. 적응 기능을 사용할 수 없거나 일시적으로 비활성화된 경우에도 기본 강인 제어(Baseline Robust Control)는 충분한 안정성을 유지해야 한다.

모든 추정 질량 특성에는 신뢰도 정보(Confidence Information)가 함께 제공되어야 한다. 제어기는 측정된 전체 질량에는 높은 신뢰도를 가지지만 횡방향 무게중심 또는 관성에는 상대적으로 낮은 신뢰도를 가질 수 있다. 비행 엔벌로프 관리는 이러한 불확실성을 명시적으로 활용하여 신뢰도가 감소할수록 안전 여유를 증가시킬 수 있다. 이는 불확실한 추정값을 정확한 값으로 간주하여 물리적 한계에 지나치게 가까운 영역에서 운용하는 것보다 안전하다.

이중화 센싱(Redundant Sensing)은 하중 및 균형 추정의 무결성을 향상시킨다. 다중 하중 측정값, 독립적인 화물 위치 센서, 관성 관측값 및 추진 응답 모델은 서로를 교차 검증할 수 있다. 그러나 여러 추정값이 동일한 형상 데이터베이스(Configuration Database) 또는 보정값에 의존하는 경우에는 공통원인 오류(Common-Cause Error)를 고려해야 한다. 소프트웨어는 측정값의 출처(Provenance)를 보존하여 진단 기능이 어떤 추정값이 실제로 독립적인지를 판단할 수 있도록 해야 한다.

적응 추정값이 비행 필수 제어에 영향을 미치므로 결정론적 타이밍(Deterministic Timing)은 여전히 중요하다. 질량 특성 갱신은 내부 루프 안정화보다 느린 주기로 수행할 수 있지만 타임스탬프와 유효성이 명확하게 정의되어야 한다. 새롭게 계산된 무게중심 값이 여러 제어 채널에 서로 다르게 적용되어서는 안 된다. 형상 갱신은 동기화되고 버전 관리되며 제어된 전환을 통해 적용되어 이중화 컴퓨터가 동일한 모델을 사용하도록 해야 한다.

소프트웨어 인더루프 시험(Software-in-the-Loop Testing)은 최대 질량, 극단적인 무게중심 위치, 관성 변화, 바람, 액추에이터 제한 및 추진 성능 저하의 조합을 포함하여 전체 적재 엔벌로프를 다루어야 한다. 서로 다른 비행 단계와 이동 속도로 화물 이동을 주입할 수 있다. 몬테카를로 시험(Monte Carlo Testing)은 각각의 파라미터가 개별적으로는 허용 범위에 있더라도 상호작용으로 인해 안정성, 구조, 에너지 또는 액추에이터 여유가 부족해지는 조합을 식별할 수 있다.

하드웨어 인더루프 시험(Hardware-in-the-Loop Testing)은 대표적인 하중 센서, 비행 컴퓨터, 액추에이터 인터페이스, 추진 제어기 및 통신 지연을 포함해야 한다. 시험에서는 잘못된 화물 데이터, 센서 드리프트(Sensor Drift), 갑작스러운 하중 이동, 오래된 형상 메시지(Stale Configuration Message), 이중화 추정기 사이의 불일치를 주입할 수 있다. 검증 과정에서는 적응 기능이 제한된 범위 안에서 유지되고 잘못된 추정값이 통제되지 않은 제어기 변화가 아니라 안전한 폴백 동작(Safe Fallback Behavior)으로 이어지는지를 확인해야 한다.

비행시험 확대(Flight-Test Expansion)는 질량 특성이 독립적으로 측정된 신중하게 정의된 탑재 구성에서 수행해야 한다. 시험은 명목 무게중심에서 시작하여 승인된 경계 방향으로 점진적으로 확장하면서 트림, 제어 여유, 구조 응답, 에너지 사용량 및 추정기 정확도를 감시할 수 있다. 목표는 각 적재 조건에서 단순히 기체가 비행할 수 있음을 입증하는 것이 아니라 정상 동작과 제어 한계 사이의 예측된 여유를 검증하는 것이다.

5톤급과 10톤급 무인항공기는 서로 다른 파라미터 범위, 구조 한계, 액추에이터 모델 및 안전 여유를 적용하면서 동일한 개념적 적응제어 프레임워크(Adaptive-Control Framework)를 공유할 수 있다. 질량 특성 추정, 신뢰도 관리, 형상 검증 및 가용 능력 보고(Capability Reporting)를 위한 공통 소프트웨어 서비스를 사용하면 재사용성을 향상시킬 수 있다. 이후 기체별 제어법칙이 각 항공기 등급의 동역학과 이중화 아키텍처에 맞게 이러한 서비스를 적용할 수 있다.

핵심 목표는 화물 구성을 출발 전에 설정되는 고정된 가정으로 취급하지 않고 비행제어에서 관측 가능하고 능동적으로 관리되는 요소로 만드는 것이다. 비행 전 질량 모델링, 온라인 추정, 제한된 적응(Bounded Adaptation), 제어 할당, 구조 보호, 추진 시스템 조정 및 고장 진단을 결합함으로써 대형 화물 무인항공기는 탑재 조건이 변화하더라도 예측 가능한 동작을 유지할 수 있다.

5톤 및 10톤급 자율 화물 항공기에서 성공적인 적응제어는 아무런 제한 없이 제어기를 지속적으로 변경하는 것을 의미하지 않는다. 이는 정보가 불확실해질 경우 강인한 폴백 모드(Robust Fallback Mode)를 유지하면서 알려진 경계 안에서 검증된 제어 동작을 조정하는 것을 의미한다. 이러한 접근을 통해 대형 무인항공기는 실제 운용에서 발생하는 다양한 탑재 조건을 수용하면서 임무 전반에 걸쳐 측정 가능한 안정성, 조종성, 구조 및 에너지 여유를 유지할 수 있다.

##  

## 11.07. 5t 10t UTM and Air Traffic Integration [w/Code]

![](images/image7.png){width="7.268055555555556in" height="7.268055555555556in"}

Integration of 5-ton and 10-ton cargo UAVs with unmanned aircraft system traffic management and conventional air traffic systems requires more than exchanging position reports. These aircraft have substantial mass, kinetic energy, operational range, and infrastructure requirements, so traffic integration must coordinate strategic planning, tactical separation, communication, navigation, surveillance, contingency management, and flight-control constraints as a unified operational process.

The software architecture should separate onboard flight safety from external traffic-management services. UTM or air traffic systems may provide route authorization, constraints, traffic information, weather advisories, or rerouting instructions, but the aircraft must retain independent stabilization and safety functions. Loss of an external service should therefore degrade strategic coordination without directly compromising basic aircraft control or the ability to execute a predefined contingency.

Before departure, mission software should translate logistics objectives into an airspace-compatible flight intent. Departure point, destination, altitude profile, route corridor, estimated timing, vehicle performance, contingency sites, and operational restrictions form part of this intent. The planning system can then compare the proposed mission with airspace constraints and modify the route before the aircraft commits energy and payload resources to flight.

Flight-intent management must support updates because heavy cargo missions can last long enough for airspace conditions to change significantly. Temporary restrictions, weather, emergency activity, airport traffic, or congestion may invalidate an initially approved route. The software should maintain a distinction between the mission objective and the currently authorized trajectory so that external constraints can change the route without corrupting the higher-level logistics task.

UTM interfaces should use well-defined data contracts for identity, position, velocity, planned trajectory, operational status, emergency state, and authorization information. Each message requires clear units, coordinate frames, timestamps, validity intervals, and quality indicators. Ambiguous interpretation of altitude references, time bases, or route geometry can create greater risk for a 10-ton aircraft than for a small drone, making interface consistency a safety-relevant requirement.

Time synchronization is fundamental to traffic integration because trajectory prediction depends on both position and time. Aircraft state reports, route reservations, conflict calculations, and separation decisions should use a consistent time reference with known uncertainty. A position without an accurate timestamp may be unsuitable for tactical conflict assessment. Onboard software should therefore preserve measurement time separately from communication arrival time.

Navigation integrity must accompany navigation availability. The aircraft should report not only its estimated position but also confidence or integrity information that indicates how trustworthy that estimate is. GNSS degradation, multipath, interference, inertial drift, or sensor disagreement may reduce navigation quality before position is completely lost. Traffic-management behavior can then increase separation margins or initiate contingency procedures before uncertainty becomes unacceptable.

Surveillance should combine cooperative and onboard information where appropriate. Cooperative sources can provide identification and state information for participating aircraft, while onboard sensors may detect traffic that is not represented accurately in external services. Radar, electro-optical sensing, or other detect-and-avoid technologies can support local awareness. The aircraft must reconcile these sources without assuming that either external surveillance or onboard perception is always complete.

Strategic deconfliction should occur before two trajectories approach a hazardous condition. UTM services can compare planned routes and reserve time-space volumes or apply other separation policies. Heavy UAV software should treat these constraints as trajectory boundaries rather than simple waypoints. Vehicle dimensions, navigation uncertainty, wind, maneuver capability, communication latency, and contingency margins all influence the volume of airspace required for safe operation.

Tactical conflict management addresses situations that develop after strategic planning. Unexpected aircraft behavior, weather deviations, navigation uncertainty, or emergency traffic may create conflicts that were not predicted earlier. The onboard system should evaluate traffic information against current vehicle capability and generate feasible avoidance trajectories. Commands must remain compatible with flight-envelope, structural, propulsion, energy, and cargo constraints.

A 10-ton UAV cannot necessarily perform the rapid avoidance maneuvers possible for a small drone. Large inertia, limited acceleration, structural loading, and payload stability require earlier conflict detection and longer maneuver horizons. Traffic-management algorithms should therefore use vehicle-specific performance models rather than applying identical separation logic to all UAV classes. Required separation may increase as maneuver capability decreases or uncertainty grows.

Detect-and-avoid software should maintain a clear relationship with flight control. The traffic function identifies conflicts and proposes avoidance objectives, while trajectory generation converts them into dynamically feasible paths. Flight control then tracks those paths within validated limits. This layered architecture prevents a traffic alert from directly generating abrupt actuator commands and allows aircraft constraints to remain authoritative during collision-avoidance behavior.

Priority management becomes important when several constraints compete. Avoiding an imminent collision normally has greater urgency than maintaining a logistics schedule, while terrain, structural limits, and minimum energy reserves remain hard constraints. The decision architecture should explicitly represent these priorities. It should avoid solving one conflict by creating another, such as commanding a climb that exceeds available propulsion power or enters prohibited airspace.

Conventional air traffic integration may require interaction with controlled airspace, airports, instrument procedures, and human air traffic controllers. The UAV system should translate external clearances or instructions into machine-verifiable constraints before execution. Autonomous interpretation should preserve altitude, route, timing, and clearance limits exactly enough that the aircraft behaves predictably alongside crewed aviation rather than relying on informal intent interpretation.

Communication architecture may include command-and-control links, UTM connectivity, air traffic interfaces, cooperative surveillance, and operator communication. These channels should be separated according to function and criticality. Loss of a logistics data service should not have the same consequence as loss of a safety-relevant traffic link. The aircraft should monitor link quality, latency, authentication state, and message age so that communication degradation is recognized explicitly.

Communication loss requires predefined behavior based on flight phase and airspace context. The aircraft may continue along an authorized route, hold within a protected volume, return to a defined location, divert, or land at a contingency site. The correct response depends on remaining navigation confidence, traffic awareness, energy state, weather, and local airspace rules. A single universal lost-link action is unlikely to be appropriate for every heavy-UAV mission.

Contingency planning should be integrated into the original route rather than calculated only after a failure. Suitable diversion airports, emergency landing areas, holding regions, communication recovery points, and energy requirements can be evaluated before departure. During flight, the system should continuously update which contingency options remain reachable as fuel, battery state, weather, traffic, and propulsion capability change.

Emergency status must be communicated consistently across onboard and external systems. A propulsion failure, navigation degradation, cargo problem, or flight-control fault may change the aircraft\'s maneuver capability and required airspace priority. The vehicle should expose a concise operational state and revised performance limits so traffic-management services do not continue predicting motion from nominal assumptions after the aircraft has entered a degraded condition.

Weather integration is particularly significant for large cargo UAVs because wind affects both trajectory accuracy and energy consumption. Convective weather, icing risk, turbulence, visibility, wind shear, and strong crosswinds may require rerouting or operational restrictions. Weather data should be treated with timestamps, spatial resolution, uncertainty, and source quality. Strategic forecasts and onboard observations can be combined to support updated trajectory decisions.

Energy-aware traffic integration prevents external rerouting from creating an infeasible mission. A longer path, extended hold, altitude change, or repeated conflict maneuver consumes additional fuel or electrical energy. The aircraft should expose endurance and reserve constraints to its mission planner, which evaluates traffic-management instructions before accepting them. Safety-critical external restrictions remain authoritative, but the system may need to request an alternative route or diversion.

Cargo characteristics can also affect airspace maneuverability. High mass, sensitive cargo, suspended loads, or unfavorable center of gravity may reduce allowable acceleration, bank angle, climb rate, or descent rate. Traffic-management interfaces should therefore receive capability information derived from the current aircraft configuration rather than assuming a fixed maneuver model. This allows conflict-resolution algorithms to generate trajectories the vehicle can actually execute.

Geofencing should be implemented as a verified constraint service rather than a simple map overlay. Static prohibited areas, temporary restrictions, altitude limits, airport protection zones, and mission-specific boundaries can be represented with effective times and confidence information. Onboard enforcement should account for navigation uncertainty and stopping or turning distance so that action begins before the aircraft physically reaches a prohibited boundary.

Route changes should pass through a controlled acceptance process. The aircraft receives an external constraint or proposed reroute, validates message integrity and authorization, checks geometric and temporal consistency, evaluates vehicle feasibility, and generates a flyable trajectory. Only then should the new route become active. This prevents malformed, stale, or operationally impossible instructions from being forwarded directly into guidance.

Cybersecurity is essential because traffic-management connectivity exposes the aircraft to external networks. Authentication, encryption where appropriate, signed data, secure boot, protected credentials, replay protection, and network segmentation reduce the risk of unauthorized route or identity manipulation. External traffic services should never obtain unrestricted access to flight-control or actuator networks; communication must pass through controlled and monitored gateways.

Identity management supports accountability and traffic correlation. Aircraft identity, mission identity, operator authorization, and communication credentials should remain consistent across planning, surveillance, and operational systems. Software must manage credential validity and prevent confusion between vehicle identity and temporary network sessions. Incorrect association of trajectory information with the wrong aircraft could undermine otherwise correct conflict-management logic.

Data recording should preserve traffic-management decisions for safety analysis and operational improvement. Relevant records include received constraints, transmitted state reports, navigation integrity, traffic tracks, conflict alerts, route revisions, operator actions, and contingency transitions. Accurate timestamps allow investigators to reconstruct whether a conflict resulted from prediction error, delayed communication, navigation uncertainty, or inappropriate vehicle response.

Simulation should test integration with dense and abnormal traffic rather than only nominal route following. Scenarios can include conflicting flight intents, delayed surveillance, communication loss, GNSS degradation, unexpected crewed aircraft, weather rerouting, emergency priority traffic, and simultaneous propulsion limitations. The objective is to verify that traffic functions continue to generate safe and dynamically feasible decisions under uncertainty.

Hardware-in-the-loop testing can connect real flight computers and communication interfaces to simulated UTM and air traffic environments. Network delay, packet loss, duplicated messages, clock errors, invalid clearances, stale restrictions, and service outages can be introduced safely. Verification should confirm that external failures remain contained and that the aircraft transitions predictably to onboard contingency logic when required.

Field trials should progress from segregated airspace toward increasingly representative mixed operations. Initial flights can verify identification, tracking, route conformance, geofence behavior, and communication before introducing traffic conflicts and controlled rerouting. Later trials can evaluate interaction with human controllers, conventional surveillance, airports, and contingency procedures while maintaining carefully defined safety boundaries.

The same integration framework can support both 5-ton and 10-ton UAVs, but performance parameters and separation assumptions must remain vehicle specific. A 5-ton aircraft may have greater maneuver flexibility, while a 10-ton platform may require longer prediction horizons, larger contingency volumes, and more conservative energy margins. Common protocols should therefore exchange capability rather than hide these important physical differences.

Successful UTM and air traffic integration ultimately depends on connecting digital airspace coordination with the real physical limitations of heavy aircraft. External systems provide strategic awareness and shared traffic constraints, while onboard systems preserve independent safety and execute feasible trajectories. By combining synchronized data, navigation integrity, conflict management, contingency planning, secure communication, and capability-aware control, 5-ton and 10-ton cargo UAVs can participate predictably in increasingly integrated airspace.

5톤 및 10톤급 화물 무인항공기(Cargo UAV)를 무인항공기 교통관리(UTM, Unmanned Aircraft System Traffic Management) 및 기존 항공교통 시스템(Air Traffic System)과 통합하는 것은 단순히 위치 정보를 교환하는 것 이상의 기능을 요구한다. 이러한 항공기는 상당한 질량, 운동에너지, 운용 거리 및 인프라 요구조건을 가지므로 교통 통합은 전략적 계획(Strategic Planning), 전술적 분리(Tactical Separation), 통신, 항법, 감시, 비상상황 관리 및 비행제어 제약조건을 하나의 통합된 운용 프로세스로 조정해야 한다.

소프트웨어 아키텍처(Software Architecture)는 탑재 비행 안전(Onboard Flight Safety)을 외부 교통관리 서비스와 분리해야 한다. UTM 또는 항공교통 시스템은 항로 승인, 제약조건, 교통 정보, 기상 주의정보 또는 재경로 설정(Rerouting) 명령을 제공할 수 있지만, 항공기는 독립적인 안정화 및 안전 기능을 유지해야 한다. 따라서 외부 서비스의 손실은 전략적 조정 기능의 성능을 저하시킬 수 있지만 기본적인 기체 제어나 사전에 정의된 비상 절차(Contingency)를 수행하는 능력을 직접적으로 손상시켜서는 안 된다.

출발 전에 임무 소프트웨어(Mission Software)는 물류 목표를 공역과 호환되는 비행 의도(Flight Intent)로 변환해야 한다. 출발지, 목적지, 고도 프로파일, 항로 회랑(Route Corridor), 예상 시간, 기체 성능, 비상 착륙 후보지 및 운용 제한조건이 이러한 비행 의도의 일부를 구성한다. 이후 계획 시스템은 제안된 임무를 공역 제약조건과 비교하고, 기체가 비행에 필요한 에너지와 탑재 자원을 투입하기 전에 항로를 수정할 수 있다.

비행 의도 관리(Flight-Intent Management)는 대형 화물 운송 임무가 공역 조건이 크게 변화할 만큼 장시간 지속될 수 있기 때문에 갱신 기능을 지원해야 한다. 임시 제한구역, 기상, 비상 활동, 공항 교통 또는 혼잡으로 인해 최초 승인된 항로가 더 이상 유효하지 않을 수 있다. 소프트웨어는 임무 목표와 현재 승인된 궤적(Authorized Trajectory)을 구분하여 외부 제약조건이 변경되더라도 상위 물류 임무 자체를 손상시키지 않고 항로를 수정할 수 있어야 한다.

UTM 인터페이스(UTM Interface)는 식별정보, 위치, 속도, 계획 궤적, 운용 상태, 비상 상태 및 승인 정보를 위한 명확하게 정의된 데이터 계약(Data Contract)을 사용해야 한다. 각 메시지는 명확한 단위, 좌표계(Coordinate Frame), 타임스탬프, 유효시간 및 품질 지표를 포함해야 한다. 고도 기준, 시간 기준 또는 항로 형상의 모호한 해석은 소형 드론보다 10톤급 항공기에서 더 큰 위험을 발생시킬 수 있으므로 인터페이스 일관성(Interface Consistency)은 안전과 관련된 중요한 요구조건이 된다.

시간 동기화(Time Synchronization)는 궤적 예측이 위치뿐 아니라 시간에도 의존하기 때문에 교통 통합의 기본 요소이다. 기체 상태 보고, 항로 예약, 충돌 계산 및 분리 결정은 알려진 불확실성을 갖는 일관된 시간 기준을 사용해야 한다. 정확한 타임스탬프가 없는 위치 정보는 전술적 충돌 평가에 적합하지 않을 수 있다. 따라서 탑재 소프트웨어는 통신 도착시간과 실제 측정시간(Measurement Time)을 분리하여 보존해야 한다.

항법 무결성(Navigation Integrity)은 항법 가용성(Navigation Availability)과 함께 제공되어야 한다. 항공기는 추정 위치뿐 아니라 해당 추정값을 어느 정도 신뢰할 수 있는지를 나타내는 신뢰도 또는 무결성 정보도 보고해야 한다. 위성항법시스템(GNSS) 성능 저하, 다중경로(Multipath), 간섭, 관성 드리프트(Inertial Drift) 또는 센서 불일치는 위치 정보가 완전히 손실되기 전에 항법 품질을 저하시킬 수 있다. 교통관리 시스템은 이러한 정보를 이용하여 불확실성이 허용할 수 없는 수준에 도달하기 전에 분리 여유를 확대하거나 비상 절차를 시작할 수 있다.

감시(Surveillance)는 적절한 경우 협력식 정보(Cooperative Information)와 탑재 정보를 결합해야 한다. 협력식 정보원은 시스템에 참여하는 항공기의 식별정보와 상태를 제공할 수 있으며, 탑재 센서는 외부 서비스에 정확하게 표현되지 않는 교통 대상을 탐지할 수 있다. 레이더, 전자광학 센싱(Electro-Optical Sensing) 또는 기타 탐지회피 기술(Detect-and-Avoid Technology)은 국부적인 상황 인식을 지원할 수 있다. 항공기는 외부 감시 또는 탑재 인지 시스템 가운데 어느 하나도 항상 완전하다고 가정하지 않고 이러한 정보원을 통합해야 한다.

전략적 충돌 방지(Strategic Deconfliction)는 두 궤적이 위험한 상태에 접근하기 전에 수행되어야 한다. UTM 서비스는 계획된 항로를 비교하여 시공간 영역(Time-Space Volume)을 예약하거나 다른 분리 정책을 적용할 수 있다. 대형 무인항공기 소프트웨어는 이러한 제약조건을 단순한 웨이포인트(Waypoint)가 아니라 궤적 경계(Trajectory Boundary)로 취급해야 한다. 기체 크기, 항법 불확실성, 바람, 기동 능력, 통신 지연 및 비상 여유가 안전한 운용에 필요한 공역 크기에 영향을 준다.

전술적 충돌 관리(Tactical Conflict Management)는 전략적 계획 이후 발생하는 상황을 처리한다. 예상하지 못한 항공기 동작, 기상 우회, 항법 불확실성 또는 비상 항공교통으로 인해 사전에 예측하지 못한 충돌이 발생할 수 있다. 탑재 시스템은 현재 기체의 가용 능력을 기준으로 교통 정보를 평가하고 실행 가능한 회피 궤적(Avoidance Trajectory)을 생성해야 한다. 이러한 명령은 비행 엔벌로프, 구조, 추진, 에너지 및 화물 제약조건과 호환되어야 한다.

10톤급 무인항공기는 소형 드론에서 가능한 급격한 회피 기동을 반드시 수행할 수 있는 것은 아니다. 높은 관성, 제한된 가속 능력, 구조 하중 및 화물 안정성 때문에 더 이른 충돌 탐지와 더 긴 기동 예측 구간(Maneuver Horizon)이 필요하다. 따라서 교통관리 알고리즘은 모든 무인항공기에 동일한 분리 로직을 적용하는 대신 기체별 성능 모델(Vehicle-Specific Performance Model)을 사용해야 한다. 기동 능력이 감소하거나 불확실성이 증가할수록 요구되는 분리 거리는 증가할 수 있다.

탐지회피 소프트웨어(Detect-and-Avoid Software)는 비행제어 시스템과 명확한 관계를 유지해야 한다. 교통관리 기능은 충돌을 식별하고 회피 목표를 제안하며, 궤적 생성 기능은 이를 동역학적으로 실행 가능한 경로로 변환한다. 이후 비행제어 시스템은 검증된 한계 안에서 해당 경로를 추종한다. 이러한 계층형 아키텍처는 교통 경보가 직접적으로 급격한 액추에이터 명령을 생성하는 것을 방지하고 충돌 회피 중에도 기체 제약조건이 최우선으로 유지되도록 한다.

여러 제약조건이 동시에 경쟁하는 경우 우선순위 관리(Priority Management)가 중요해진다. 임박한 충돌을 회피하는 것은 일반적으로 물류 일정을 유지하는 것보다 높은 긴급성을 가지며, 지형, 구조 한계 및 최소 에너지 예비량은 여전히 반드시 지켜야 하는 강제 제약조건(Hard Constraint)이다. 의사결정 아키텍처는 이러한 우선순위를 명확하게 표현해야 한다. 가용 추진 출력을 초과하는 상승을 명령하거나 금지 공역으로 진입하는 것처럼 하나의 충돌을 해결하면서 또 다른 충돌을 만드는 상황을 방지해야 한다.

기존 항공교통 통합(Conventional Air Traffic Integration)은 관제 공역(Controlled Airspace), 공항, 계기비행 절차(Instrument Procedure) 및 인간 항공교통관제사(Air Traffic Controller)와의 상호작용을 요구할 수 있다. 무인항공기 시스템은 외부 허가 또는 지시를 실행하기 전에 기계적으로 검증 가능한 제약조건(Machine-Verifiable Constraint)으로 변환해야 한다. 자율 해석 기능은 고도, 항로, 시간 및 허가 한계를 정확하게 보존하여 유인항공기와 함께 예측 가능한 방식으로 운용되도록 해야 한다.

통신 아키텍처(Communication Architecture)는 지휘통제 링크(Command-and-Control Link), UTM 연결, 항공교통 인터페이스, 협력식 감시(Cooperative Surveillance) 및 운용자 통신을 포함할 수 있다. 이러한 채널은 기능과 중요도에 따라 분리되어야 한다. 물류 데이터 서비스의 손실과 안전 관련 교통 링크의 손실이 동일한 결과를 발생시켜서는 안 된다. 항공기는 링크 품질, 지연시간, 인증 상태 및 메시지 경과시간을 감시하여 통신 성능 저하를 명확하게 인식해야 한다.

통신 손실(Communication Loss)은 비행 단계와 공역 상황에 따라 사전에 정의된 동작을 요구한다. 항공기는 승인된 항로를 계속 비행하거나, 보호된 영역에서 대기(Hold)하거나, 정의된 위치로 복귀하거나, 다른 장소로 회항하거나, 비상 착륙 지점에 착륙할 수 있다. 올바른 대응은 남아 있는 항법 신뢰도, 교통 상황 인식, 에너지 상태, 기상 및 지역 공역 규칙에 따라 달라진다. 따라서 모든 대형 무인항공기 임무에 하나의 범용 통신 두절 동작(Lost-Link Action)을 적용하는 것은 적절하지 않다.

비상계획(Contingency Planning)은 고장이 발생한 이후에만 계산하는 것이 아니라 최초 항로에 통합되어야 한다. 적절한 회항 공항, 비상 착륙 지역, 대기 구역, 통신 복구 지점 및 필요한 에너지를 출발 전에 평가할 수 있다. 비행 중에는 연료, 배터리 상태, 기상, 교통 및 추진 능력이 변화함에 따라 어떤 비상 대안이 여전히 도달 가능한지를 시스템이 지속적으로 갱신해야 한다.

비상 상태(Emergency Status)는 탑재 시스템과 외부 시스템 전체에서 일관되게 전달되어야 한다. 추진 고장, 항법 성능 저하, 화물 문제 또는 비행제어 고장은 기체의 기동 능력과 필요한 공역 우선순위를 변화시킬 수 있다. 항공기는 간결한 운용 상태와 변경된 성능 한계를 외부에 제공하여 기체가 성능 저하 상태에 진입한 이후에도 교통관리 서비스가 정상 성능을 기준으로 움직임을 계속 예측하는 상황을 방지해야 한다.

기상 통합(Weather Integration)은 바람이 궤적 정확도와 에너지 소비에 모두 영향을 미치기 때문에 대형 화물 무인항공기에서 특히 중요하다. 대류성 기상(Convective Weather), 결빙 위험, 난류, 가시거리, 윈드시어(Wind Shear) 및 강한 횡풍은 항로 변경이나 운용 제한을 요구할 수 있다. 기상 데이터는 타임스탬프, 공간 해상도, 불확실성 및 정보원 품질과 함께 처리되어야 한다. 전략적 예보와 탑재 관측 정보를 결합하여 갱신된 궤적 결정을 지원할 수 있다.

에너지 인식형 교통 통합(Energy-Aware Traffic Integration)은 외부의 항로 변경으로 인해 실행 불가능한 임무가 만들어지는 것을 방지한다. 더 긴 항로, 장시간 대기, 고도 변경 또는 반복적인 충돌 회피 기동은 추가적인 연료 또는 전기 에너지를 소비한다. 항공기는 항속 및 예비 에너지 제약조건을 임무 계획기에 제공하고, 임무 계획기는 교통관리 지시를 수락하기 전에 이를 평가해야 한다. 안전 필수 외부 제한은 우선되어야 하지만 필요할 경우 시스템은 대체 항로나 회항을 요청해야 한다.

화물 특성도 공역에서의 기동성에 영향을 줄 수 있다. 높은 질량, 민감한 화물, 현수 화물(Suspended Load) 또는 불리한 무게중심은 허용 가속도, 뱅크각(Bank Angle), 상승률 또는 하강률을 감소시킬 수 있다. 따라서 교통관리 인터페이스는 고정된 기동 모델을 가정하지 않고 현재 기체 구성에서 도출된 가용 능력 정보를 제공받아야 한다. 이를 통해 충돌 해결 알고리즘은 기체가 실제로 수행할 수 있는 궤적을 생성할 수 있다.

지오펜싱(Geofencing)은 단순한 지도 오버레이가 아니라 검증된 제약조건 서비스(Verified Constraint Service)로 구현되어야 한다. 고정 금지구역, 임시 제한구역, 고도 제한, 공항 보호구역 및 임무별 경계를 적용시간과 신뢰도 정보와 함께 표현할 수 있다. 탑재 시스템의 강제 적용 기능은 항법 불확실성과 정지 또는 선회에 필요한 거리를 고려하여 기체가 실제 금지 경계에 도달하기 전에 대응을 시작해야 한다.

항로 변경(Route Change)은 통제된 수락 프로세스(Controlled Acceptance Process)를 거쳐야 한다. 항공기는 외부 제약조건 또는 제안된 재경로를 수신하고 메시지 무결성과 승인 상태를 검증하며, 기하학적·시간적 일관성을 확인하고, 기체의 실행 가능성을 평가한 다음 실제 비행 가능한 궤적을 생성한다. 이러한 과정이 완료된 이후에만 새로운 항로를 활성화해야 한다. 이를 통해 잘못 구성되거나 오래되었거나 운용상 실행할 수 없는 지시가 유도 시스템으로 직접 전달되는 것을 방지한다.

교통관리 연결은 항공기를 외부 네트워크에 노출시키므로 사이버보안(Cybersecurity)이 필수적이다. 인증(Authentication), 필요한 경우의 암호화, 서명된 데이터(Signed Data), 보안 부팅(Secure Boot), 보호된 자격증명(Protected Credential), 재전송 공격 방지(Replay Protection), 네트워크 분할(Network Segmentation)을 통해 승인되지 않은 항로 또는 식별정보 변경 위험을 줄일 수 있다. 외부 교통 서비스는 비행제어 또는 액추에이터 네트워크에 제한 없이 접근해서는 안 되며, 모든 통신은 통제되고 감시되는 게이트웨이를 통과해야 한다.

식별 관리(Identity Management)는 책임 추적성과 교통 정보의 정확한 연계를 지원한다. 기체 식별정보, 임무 식별정보, 운용자 승인 및 통신 자격증명은 계획, 감시 및 운용 시스템 전체에서 일관성을 유지해야 한다. 소프트웨어는 자격증명의 유효성을 관리하고 기체 식별정보와 임시 네트워크 세션이 혼동되지 않도록 해야 한다. 궤적 정보가 잘못된 항공기와 연결되면 다른 충돌관리 로직이 올바르게 동작하더라도 전체 교통 안전성이 손상될 수 있다.

데이터 기록(Data Recording)은 안전성 분석과 운용 개선을 위해 교통관리 의사결정을 보존해야 한다. 관련 기록에는 수신된 제약조건, 전송된 상태 보고, 항법 무결성, 교통 추적정보, 충돌 경보, 항로 변경, 운용자 동작 및 비상 상태 전환이 포함된다. 정확한 타임스탬프를 이용하면 충돌이 예측 오류, 통신 지연, 항법 불확실성 또는 부적절한 기체 대응 중 어느 원인으로 발생했는지를 재구성할 수 있다.

시뮬레이션(Simulation)은 정상적인 항로 추종뿐 아니라 밀집된 교통 환경과 비정상적인 교통 상황에서의 통합 기능도 시험해야 한다. 시나리오에는 상충하는 비행 의도, 지연된 감시 정보, 통신 손실, GNSS 성능 저하, 예상하지 못한 유인항공기, 기상에 따른 항로 변경, 비상 우선순위 항공교통 및 동시에 발생하는 추진 제한이 포함될 수 있다. 목표는 불확실성이 존재하는 상황에서도 교통관리 기능이 안전하고 동역학적으로 실행 가능한 결정을 지속적으로 생성하는지를 검증하는 것이다.

하드웨어 인더루프 시험(Hardware-in-the-Loop Testing)은 실제 비행 컴퓨터와 통신 인터페이스를 시뮬레이션된 UTM 및 항공교통 환경에 연결할 수 있다. 네트워크 지연, 패킷 손실, 중복 메시지, 클록 오류, 유효하지 않은 허가, 오래된 제한정보 및 서비스 중단을 안전하게 주입할 수 있다. 검증 과정에서는 외부 시스템의 고장이 내부로 전파되지 않고 필요한 경우 항공기가 탑재 비상 로직(Onboard Contingency Logic)으로 예측 가능하게 전환되는지를 확인해야 한다.

현장 시험(Field Trial)은 분리된 공역에서 시작하여 점차 실제와 유사한 혼합 운용 환경으로 확대해야 한다. 초기 비행에서는 교통 충돌과 통제된 항로 변경을 도입하기 전에 식별, 추적, 항로 준수(Route Conformance), 지오펜스 동작 및 통신을 검증할 수 있다. 이후 시험에서는 명확하게 정의된 안전 경계를 유지하면서 인간 관제사, 기존 감시 시스템, 공항 및 비상 절차와의 상호작용을 평가할 수 있다.

동일한 통합 프레임워크(Integration Framework)를 5톤과 10톤급 무인항공기에 공통으로 적용할 수 있지만 성능 파라미터와 분리 가정(Separation Assumption)은 기체별로 유지되어야 한다. 5톤급 기체는 상대적으로 높은 기동 유연성을 가질 수 있지만, 10톤급 플랫폼은 더 긴 예측 구간, 더 넓은 비상 운용 영역 및 더욱 보수적인 에너지 여유를 요구할 수 있다. 따라서 공통 프로토콜은 이러한 중요한 물리적 차이를 숨기는 대신 기체 가용 능력(Capability)을 교환할 수 있어야 한다.

성공적인 UTM 및 항공교통 통합은 궁극적으로 디지털 공역 조정(Digital Airspace Coordination)을 대형 항공기의 실제 물리적 한계와 연결하는 데 달려 있다. 외부 시스템은 전략적 상황 인식과 공유된 교통 제약조건을 제공하고, 탑재 시스템은 독립적인 안전 기능을 유지하면서 실행 가능한 궤적을 수행한다. 동기화된 데이터, 항법 무결성, 충돌 관리, 비상계획, 보안 통신 및 가용 능력 인식형 제어(Capability-Aware Control)를 결합함으로써 5톤 및 10톤급 화물 무인항공기는 점차 통합되는 공역 환경에서 예측 가능하게 운용될 수 있다.

##  

## 11.08. 5t 10t DO 178C Level A B Compliance [w/Code]

![](images/image8.png){width="7.268055555555556in" height="7.268055555555556in"}

For 5-ton and 10-ton cargo UAVs, DO-178C compliance provides a disciplined framework for developing and verifying airborne software whose failure could affect aircraft safety. The applicable software level is not determined by vehicle mass alone, but by the system safety assessment and the severity of the failure condition associated with each software function. Flight-critical functions can consequently require Level A or Level B assurance.

Level A applies when anomalous software behavior could contribute to a catastrophic failure condition, while Level B addresses software whose failure could contribute to a hazardous or severe-major condition. A heavy cargo UAV may therefore contain several assurance levels within one aircraft. Primary flight control, critical stabilization, or selected safety functions may require Level A, while other important but less severe functions may be assigned Level B or lower levels.

Software-level allocation should follow aircraft and system safety processes rather than being selected after implementation. Functional hazard assessment identifies potential failure conditions, and system safety analysis determines how architecture, redundancy, monitoring, and independence mitigate those hazards. The resulting assurance allocation establishes which software components require the most rigorous development, verification, configuration management, and quality-assurance activities.

A robust architecture should minimize the amount of software requiring Level A assurance. Flight-critical kernels, control laws, actuator interfaces, and essential monitoring functions can be isolated from mission planning, perception, payload applications, maintenance tools, and other rapidly evolving software. Effective partitioning reduces certification scope while ensuring that failures in lower-assurance functions cannot corrupt the timing, memory, data, or control authority of higher-assurance software.

Planning is the foundation of a DO-178C lifecycle. The development organization defines how software requirements, design, coding, integration, verification, configuration management, quality assurance, and certification liaison activities will be performed. Plans should establish standards, responsibilities, tools, environments, review criteria, transition conditions, and evidence expectations before large-scale implementation begins rather than attempting to reconstruct compliance after development.

High-level software requirements should be derived from allocated system requirements and describe software behavior in a verifiable form. They should define control functions, modes, interfaces, timing, limits, fault responses, data validity, and safety behavior without unnecessary ambiguity. Each requirement should be traceable to its source so reviewers can determine why the behavior exists and whether all allocated system requirements have been implemented.

Low-level requirements and software design refine high-level behavior into sufficient detail for implementation and verification. They may describe algorithms, state transitions, data flows, scheduling, numerical behavior, interface handling, and fault logic. For complex flight-control or redundancy-management software, precise low-level requirements help separate intended behavior from implementation accidents and provide an independent basis for structural verification.

Bidirectional traceability is essential throughout the lifecycle. System requirements should trace to high-level software requirements, which trace to low-level requirements, source code, and verification cases where applicable. Traceability in the reverse direction demonstrates that implemented code and tests have legitimate requirements. Untraceable functionality can indicate unintended behavior, while missing downstream traces can reveal incomplete implementation or verification.

Source code should comply with defined coding standards intended to improve determinism, analyzability, maintainability, and verification. Restrictions may address dynamic memory, recursion, implicit conversions, numerical behavior, concurrency, defensive programming, and language constructs that are difficult to analyze. The objective is not stylistic uniformity alone, but reduction of implementation ambiguity and prevention of software behavior that cannot be adequately verified.

The executable object code must correctly implement the software requirements in the target environment. Compilation, linking, processor behavior, operating-system services, partitioning mechanisms, and hardware interfaces can affect execution. Verification therefore cannot stop at source-level review. Target-based testing and analysis are necessary where required to demonstrate that the integrated executable behaves as intended on representative airborne computing hardware.

Verification independence becomes increasingly important at Levels A and B. The person or activity verifying selected lifecycle data should have sufficient independence from the person or activity that produced it, according to the applicable objectives. Independent reviews can identify incorrect assumptions that may be overlooked when authors verify their own work. Organizational processes should define independence explicitly rather than treating it as an informal reviewer preference.

Requirements-based testing demonstrates that software satisfies specified behavior under normal conditions and robustness conditions. Tests should cover nominal inputs, boundaries, invalid data, timing conditions, mode transitions, communication loss, sensor failures, actuator limitations, and other relevant abnormal situations. For heavy UAV software, test environments should also reproduce representative mass, propulsion, redundancy, and degraded-flight configurations where they affect software behavior.

Structural coverage analysis determines whether requirements-based testing has exercised the implemented software structure to the degree required by the assigned assurance level. Statement and decision coverage are relevant at lower rigor, while Level A additionally requires Modified Condition/Decision Coverage. MC/DC demonstrates that individual Boolean conditions can independently affect a decision outcome, providing stronger evidence for highly critical decision logic.

Structural coverage should not be treated as a goal for generating arbitrary tests merely to increase percentages. Coverage gaps must be analyzed to determine whether they result from missing requirements-based tests, unintended code, deactivated code, defensive logic, or infeasible execution paths. The correct resolution depends on the cause. Adding tests without understanding why code was not exercised can conceal specification or implementation problems.

Level A software requires particular attention to data coupling and control coupling analysis. Integration behavior must show that software components exchange data and transfer control consistently with the intended architecture. This is especially important in distributed flight-control systems where partitions, tasks, communication channels, and redundant computers interact. Testing and analysis should demonstrate that actual coupling is consistent with verified design assumptions.

Robustness verification evaluates behavior outside ordinary nominal inputs. Flight-critical software should respond predictably to out-of-range sensor values, stale messages, invalid mode requests, arithmetic boundaries, resource limitations, and communication faults. Defensive behavior should itself be requirements-based. Code added solely as undocumented protection can create verification difficulties because it represents behavior without an explicit approved requirement.

Timing and resource usage must be verified because functional correctness alone is insufficient for real-time flight software. Worst-case execution time, task periods, scheduling margins, stack usage, memory consumption, network latency, and processor loading should remain within allocated limits. A correct control algorithm that misses deadlines under peak load can create hazardous behavior equivalent to an incorrect algorithm.

Partitioning and interference analysis are critical when software of different assurance levels shares computing hardware. Memory protection, processor scheduling, communication interfaces, shared resources, and failure containment should ensure that lower-assurance applications cannot adversely affect Level A or Level B functions. Evidence may include architecture analysis, target testing, resource stress testing, and platform assurance activities depending on the selected computing environment.

Tool qualification may be necessary when a software tool can introduce an error into airborne software or certification data and that error is not otherwise detected by the lifecycle processes. Compilers, code generators, verification tools, coverage analyzers, and model-based development environments must therefore be evaluated according to their intended use and error-detection context. Qualification scope should be established early because late discovery can create substantial rework.

Model-based development can support complex flight-control functions when models are treated within an appropriate lifecycle framework. Requirements, executable models, generated code, and verification artifacts must maintain traceability and consistency. The use of automatic code generation does not eliminate assurance obligations. Instead, it changes where evidence is produced and may introduce tool-qualification or model-verification considerations.

Configuration management ensures that certification evidence corresponds to the exact software being evaluated. Source code, requirements, design data, test procedures, test results, tools, libraries, configuration files, and executable binaries require controlled identification and versioning. Baselines should be reproducible so that a released flight-software image can be traced back to the lifecycle data and environment used to produce and verify it.

Problem reporting and change control are equally important because certification is not a one-time snapshot of development. Anomalies should be documented, evaluated for safety and lifecycle impact, corrected under configuration control, and regression-tested as necessary. Changes to flight-control or redundancy logic may require re-verification of affected requirements, structural coverage, timing behavior, interfaces, and previously accepted safety assumptions.

Software quality assurance provides independent confidence that defined lifecycle processes are actually followed. Audits and reviews examine compliance with plans, standards, transition criteria, configuration controls, and verification procedures. Quality assurance should identify process deviations early enough for correction. Evidence produced by an uncontrolled process may be technically correct yet still be difficult to use as credible certification evidence.

Certification liaison activities maintain communication between the applicant, development organization, system-safety activities, and the relevant certification authority. Lifecycle data should be organized so reviewers can understand software boundaries, assurance levels, architecture, development methods, verification strategy, unresolved issues, and compliance evidence. Early agreement on unusual technologies or methods reduces the risk of major interpretation differences late in the program.

Heavy autonomous UAVs introduce additional complexity because advanced autonomy and AI functions may evolve more rapidly than conventional flight-control software. The architecture should prevent experimental or frequently updated algorithms from becoming implicit dependencies of Level A or Level B safety functions unless appropriate assurance evidence exists. Safety monitors and constrained command interfaces can allow advanced functions to contribute while limiting their direct authority.

Redundant architectures do not automatically reduce software assurance obligations. If identical software executes on several redundant flight computers, a common software defect may affect all channels simultaneously. Safety analysis must consider this common-mode possibility. Architectural mitigation may include independent monitoring, functional dissimilarity, bounded command interfaces, or other measures, but each mitigation requires its own verified requirements and evidence.

Hardware-in-the-loop testing is particularly valuable for heavy UAV certification evidence because it can reproduce realistic processor timing, networks, sensors, actuators, and failure responses. Fault injection can exercise processor resets, frozen sensors, corrupted messages, power interruptions, actuator faults, and redundant-channel disagreement. These tests complement lower-level verification by demonstrating integrated behavior under representative abnormal conditions.

Software-in-the-loop and simulation environments remain important for broad parameter exploration. Thousands of combinations of payload, center of gravity, wind, navigation uncertainty, propulsion degradation, and communication delay can be evaluated before selecting representative target-based tests. Simulation results do not automatically replace required verification evidence, but they can expose weaknesses early and improve the completeness of requirements and robustness testing.

Level A and Level B compliance should therefore be viewed as an engineering discipline integrated with system safety rather than as documentation added after software completion. Requirements, architecture, code, tests, coverage, configuration control, quality assurance, and certification evidence must remain mutually consistent throughout development. This lifecycle discipline is especially important when software directly controls a high-energy autonomous aircraft.

For 5-ton and 10-ton cargo UAVs, successful DO-178C compliance ultimately supports confidence that flight-critical software behaves according to explicit requirements, that unintended functionality has been systematically addressed, and that changes remain controlled. Combined with system safety, redundant architecture, rigorous integration testing, and operational limitations, Level A and Level B assurance provides a structured foundation for dependable heavy-UAV flight software.

5톤 및 10톤급 화물 무인항공기(Cargo UAV)에서 DO-178C 준수(DO-178C Compliance)는 소프트웨어 고장이 항공기 안전에 영향을 미칠 수 있는 항공 탑재 소프트웨어(Airborne Software)를 개발하고 검증하기 위한 체계적인 프레임워크를 제공한다. 적용되는 소프트웨어 수준(Software Level)은 기체 질량만으로 결정되는 것이 아니라 시스템 안전성 평가(System Safety Assessment)와 각 소프트웨어 기능에 연관된 고장 조건(Failure Condition)의 심각도에 따라 결정된다. 따라서 비행 필수 기능(Flight-Critical Function)은 레벨 A(Level A) 또는 레벨 B(Level B)의 보증 수준을 요구할 수 있다.

레벨 A(Level A)는 비정상적인 소프트웨어 동작이 치명적 고장 조건(Catastrophic Failure Condition)에 기여할 수 있는 경우 적용되며, 레벨 B(Level B)는 소프트웨어 고장이 위험 또는 중대-심각 고장 조건(Hazardous or Severe-Major Failure Condition)에 기여할 수 있는 경우 적용된다. 따라서 대형 화물 무인항공기는 하나의 기체 안에 여러 보증 수준(Assurance Level)을 포함할 수 있다. 기본 비행제어, 핵심 안정화 또는 일부 안전 기능에는 레벨 A가 요구될 수 있으며, 중요하지만 상대적으로 심각도가 낮은 기능에는 레벨 B 또는 그보다 낮은 수준이 할당될 수 있다.

소프트웨어 수준 할당(Software-Level Allocation)은 구현 이후에 선택하는 것이 아니라 항공기 및 시스템 안전 프로세스에 따라 이루어져야 한다. 기능 위험성 평가(Functional Hazard Assessment)는 잠재적인 고장 조건을 식별하고, 시스템 안전성 분석(System Safety Analysis)은 아키텍처, 이중화, 감시 및 독립성이 이러한 위험을 어떻게 완화하는지를 결정한다. 그 결과로 도출되는 보증 수준 할당은 어떤 소프트웨어 구성요소가 가장 엄격한 개발, 검증, 형상관리(Configuration Management) 및 품질보증 활동을 요구하는지를 결정한다.

강인한 아키텍처(Robust Architecture)는 레벨 A 보증이 필요한 소프트웨어의 범위를 최소화해야 한다. 비행 필수 커널(Flight-Critical Kernel), 제어법칙(Control Law), 액추에이터 인터페이스 및 필수 감시 기능은 임무 계획, 인지(Perception), 탑재물 애플리케이션, 정비 도구 및 빠르게 변화하는 기타 소프트웨어와 분리할 수 있다. 효과적인 파티셔닝(Partitioning)은 인증 범위를 줄이는 동시에 낮은 보증 수준 기능의 고장이 높은 보증 수준 소프트웨어의 타이밍, 메모리, 데이터 또는 제어 권한을 손상시키지 않도록 보장한다.

계획 수립(Planning)은 DO-178C 생명주기(Lifecycle)의 기반이다. 개발 조직은 소프트웨어 요구사항, 설계, 코딩, 통합, 검증, 형상관리, 품질보증 및 인증 협력 활동(Certification Liaison Activity)을 어떻게 수행할 것인지 정의한다. 계획은 대규모 구현이 시작되기 전에 표준, 책임, 도구, 환경, 검토 기준, 전환 조건 및 증거 요구사항을 설정해야 하며, 개발이 완료된 이후 준수성을 역으로 구성하려 해서는 안 된다.

상위 수준 소프트웨어 요구사항(High-Level Software Requirement)은 할당된 시스템 요구사항으로부터 도출되어야 하며 소프트웨어 동작을 검증 가능한 형태로 기술해야 한다. 제어 기능, 모드, 인터페이스, 타이밍, 제한조건, 고장 대응, 데이터 유효성 및 안전 동작을 불필요한 모호성 없이 정의해야 한다. 각 요구사항은 그 출처까지 추적 가능해야 하며, 이를 통해 검토자는 해당 동작이 존재하는 이유와 할당된 모든 시스템 요구사항이 구현되었는지를 확인할 수 있다.

하위 수준 요구사항(Low-Level Requirement)과 소프트웨어 설계(Software Design)는 상위 수준 동작을 구현과 검증에 충분한 세부 수준으로 구체화한다. 여기에는 알고리즘, 상태 전이(State Transition), 데이터 흐름, 스케줄링, 수치적 동작, 인터페이스 처리 및 고장 로직 등이 포함될 수 있다. 복잡한 비행제어 또는 이중화 관리 소프트웨어에서는 정밀한 하위 수준 요구사항이 의도된 동작과 구현 과정에서 우연히 발생한 동작을 구분하고 구조적 검증을 위한 독립적인 기준을 제공한다.

양방향 추적성(Bidirectional Traceability)은 전체 생명주기에서 필수적이다. 시스템 요구사항은 상위 수준 소프트웨어 요구사항으로, 상위 수준 요구사항은 하위 수준 요구사항, 소스 코드 및 해당되는 검증 사례까지 추적되어야 한다. 반대 방향의 추적성은 구현된 코드와 시험이 정당한 요구사항을 기반으로 하고 있음을 입증한다. 추적되지 않는 기능은 의도하지 않은 동작을 의미할 수 있으며, 하위 단계의 추적 연결이 누락되면 불완전한 구현 또는 검증을 나타낼 수 있다.

소스 코드(Source Code)는 결정론적 동작, 분석 가능성, 유지보수성 및 검증성을 향상시키기 위해 정의된 코딩 표준(Coding Standard)을 준수해야 한다. 제한사항에는 동적 메모리(Dynamic Memory), 재귀 호출(Recursion), 암시적 형 변환, 수치적 동작, 동시성(Concurrency), 방어적 프로그래밍(Defensive Programming) 및 분석하기 어려운 언어 구성요소 등이 포함될 수 있다. 목적은 단순한 스타일의 통일이 아니라 구현의 모호성을 줄이고 충분하게 검증할 수 없는 소프트웨어 동작을 방지하는 것이다.

실행 가능한 목적 코드(Executable Object Code)는 대상 환경(Target Environment)에서 소프트웨어 요구사항을 정확하게 구현해야 한다. 컴파일, 링크, 프로세서 동작, 운영체제 서비스, 파티셔닝 메커니즘 및 하드웨어 인터페이스가 실행에 영향을 줄 수 있다. 따라서 검증은 소스 코드 수준의 검토만으로 끝날 수 없다. 통합 실행 파일이 대표적인 항공 탑재 컴퓨팅 하드웨어에서 의도대로 동작한다는 것을 입증하기 위해 필요한 경우 대상 기반 시험(Target-Based Testing)과 분석을 수행해야 한다.

검증 독립성(Verification Independence)은 레벨 A와 레벨 B에서 더욱 중요해진다. 선택된 생명주기 데이터를 검증하는 담당자 또는 활동은 적용되는 목표에 따라 해당 데이터를 작성한 담당자 또는 활동으로부터 충분한 독립성을 가져야 한다. 독립적인 검토는 작성자가 자신의 작업을 검증할 때 놓칠 수 있는 잘못된 가정을 발견할 수 있다. 조직의 프로세스는 독립성을 단순한 검토자의 선호사항으로 취급하지 않고 명확하게 정의해야 한다.

요구사항 기반 시험(Requirements-Based Testing)은 소프트웨어가 정상 조건과 강건성 조건(Robustness Condition)에서 명시된 동작을 충족하는지를 입증한다. 시험은 정상 입력, 경계조건, 유효하지 않은 데이터, 타이밍 조건, 모드 전환, 통신 손실, 센서 고장, 액추에이터 제한 및 기타 관련 비정상 상황을 포함해야 한다. 대형 무인항공기 소프트웨어에서는 소프트웨어 동작에 영향을 미치는 경우 대표적인 질량, 추진, 이중화 및 성능 저하 비행 구성을 시험 환경에서 재현해야 한다.

구조적 커버리지 분석(Structural Coverage Analysis)은 요구사항 기반 시험이 할당된 보증 수준에서 요구하는 정도까지 구현된 소프트웨어 구조를 실행했는지를 판단한다. 낮은 수준의 엄격성에서는 문장 커버리지(Statement Coverage)와 결정 커버리지(Decision Coverage)가 관련되며, 레벨 A에서는 추가로 수정 조건/결정 커버리지(MC/DC, Modified Condition/Decision Coverage)가 요구된다. MC/DC는 개별 불리언 조건(Boolean Condition)이 결정 결과에 독립적으로 영향을 줄 수 있음을 입증하여 매우 중요한 의사결정 로직에 대해 더욱 강력한 검증 근거를 제공한다.

구조적 커버리지(Structural Coverage)는 단순히 비율을 높이기 위해 임의의 시험을 생성하는 목표로 취급해서는 안 된다. 커버리지 공백(Coverage Gap)은 요구사항 기반 시험 누락, 의도하지 않은 코드, 비활성 코드(Deactivated Code), 방어 로직 또는 실행 불가능한 경로 중 무엇에서 발생했는지를 분석해야 한다. 올바른 해결 방법은 원인에 따라 달라진다. 코드가 실행되지 않은 이유를 이해하지 않은 상태에서 시험만 추가하면 명세 또는 구현상의 문제를 숨길 수 있다.

레벨 A 소프트웨어(Level A Software)는 데이터 결합(Data Coupling) 및 제어 결합(Control Coupling) 분석에 특별한 주의가 필요하다. 통합 동작에서는 소프트웨어 구성요소가 의도된 아키텍처와 일치하도록 데이터를 교환하고 제어를 전달한다는 것을 입증해야 한다. 이는 파티션, 태스크, 통신 채널 및 이중화 컴퓨터가 상호작용하는 분산 비행제어 시스템(Distributed Flight-Control System)에서 특히 중요하다. 시험과 분석을 통해 실제 결합 관계가 검증된 설계 가정과 일치함을 입증해야 한다.

강건성 검증(Robustness Verification)은 일반적인 정상 입력 범위를 벗어난 상황에서의 동작을 평가한다. 비행 필수 소프트웨어는 범위를 벗어난 센서 값, 오래된 메시지, 유효하지 않은 모드 요청, 산술 경계조건, 자원 제한 및 통신 고장에 대해 예측 가능하게 대응해야 한다. 방어 동작(Defensive Behavior) 자체도 요구사항을 기반으로 해야 한다. 문서화된 요구사항 없이 단순한 보호 목적으로 추가된 코드는 승인된 명시적 요구사항이 없는 동작을 의미하므로 검증을 어렵게 만들 수 있다.

실시간 비행 소프트웨어(Real-Time Flight Software)에서는 기능적 정확성만으로 충분하지 않기 때문에 타이밍과 자원 사용량도 검증해야 한다. 최악 실행시간(Worst-Case Execution Time), 태스크 주기, 스케줄링 여유, 스택 사용량, 메모리 소비, 네트워크 지연시간 및 프로세서 부하는 할당된 한계 안에 유지되어야 한다. 정확한 제어 알고리즘이라도 최대 부하 상태에서 마감시간(Deadline)을 지키지 못하면 잘못된 알고리즘과 동등한 위험 동작을 발생시킬 수 있다.

서로 다른 보증 수준의 소프트웨어가 동일한 컴퓨팅 하드웨어를 공유하는 경우 파티셔닝 및 간섭 분석(Interference Analysis)이 중요하다. 메모리 보호, 프로세서 스케줄링, 통신 인터페이스, 공유 자원 및 고장 격리(Failure Containment)는 낮은 보증 수준 애플리케이션이 레벨 A 또는 레벨 B 기능에 악영향을 주지 않도록 보장해야 한다. 선택한 컴퓨팅 환경에 따라 아키텍처 분석, 대상 시스템 시험, 자원 스트레스 시험 및 플랫폼 보증 활동이 근거 자료로 사용될 수 있다.

소프트웨어 도구가 항공 탑재 소프트웨어 또는 인증 데이터에 오류를 유입할 수 있고 해당 오류가 다른 생명주기 프로세스에서 검출되지 않는 경우 도구 적격성(Tool Qualification)이 필요할 수 있다. 따라서 컴파일러, 코드 생성기, 검증 도구, 커버리지 분석기 및 모델 기반 개발 환경(Model-Based Development Environment)은 의도된 사용 목적과 오류 검출 환경에 따라 평가되어야 한다. 적격성 범위는 조기에 설정해야 하며, 늦게 발견될 경우 상당한 재작업이 발생할 수 있다.

모델 기반 개발(Model-Based Development)은 모델이 적절한 생명주기 프레임워크 안에서 관리되는 경우 복잡한 비행제어 기능 개발을 지원할 수 있다. 요구사항, 실행 가능한 모델, 생성된 코드 및 검증 산출물 사이에는 추적성과 일관성이 유지되어야 한다. 자동 코드 생성(Automatic Code Generation)을 사용한다고 해서 보증 의무가 사라지는 것은 아니다. 대신 검증 증거가 생성되는 위치가 변화하며 도구 적격성 또는 모델 검증과 관련된 추가 고려사항이 발생할 수 있다.

형상관리(Configuration Management)는 인증 근거가 실제 평가되는 정확한 소프트웨어와 대응하도록 보장한다. 소스 코드, 요구사항, 설계 데이터, 시험 절차, 시험 결과, 도구, 라이브러리, 구성 파일 및 실행 바이너리는 통제된 식별과 버전 관리가 필요하다. 베이스라인(Baseline)은 재현 가능해야 하며, 배포된 비행 소프트웨어 이미지가 이를 생성하고 검증하는 데 사용된 생명주기 데이터 및 환경까지 추적될 수 있어야 한다.

문제 보고(Problem Reporting)와 변경 통제(Change Control)도 중요하다. 인증은 개발 과정의 특정 시점을 한 번 검토하는 작업이 아니기 때문이다. 이상 현상(Anomaly)은 문서화되고 안전성과 생명주기에 미치는 영향을 평가받으며 형상 통제 아래 수정되고 필요에 따라 회귀시험(Regression Testing)을 거쳐야 한다. 비행제어 또는 이중화 로직 변경은 영향을 받는 요구사항, 구조적 커버리지, 타이밍 동작, 인터페이스 및 기존에 승인된 안전 가정에 대한 재검증을 요구할 수 있다.

소프트웨어 품질보증(Software Quality Assurance)은 정의된 생명주기 프로세스가 실제로 준수되고 있다는 독립적인 신뢰를 제공한다. 감사(Audit)와 검토는 계획, 표준, 전환 기준, 형상 통제 및 검증 절차에 대한 준수 여부를 확인한다. 품질보증 활동은 프로세스 이탈(Process Deviation)을 수정할 수 있을 만큼 조기에 식별해야 한다. 통제되지 않은 프로세스에서 생성된 증거는 기술적으로 정확하더라도 신뢰할 수 있는 인증 근거로 사용하기 어려울 수 있다.

인증 협력 활동(Certification Liaison Activity)은 신청자, 개발 조직, 시스템 안전 활동 및 관련 인증 당국(Certification Authority) 사이의 의사소통을 유지한다. 생명주기 데이터는 검토자가 소프트웨어 경계, 보증 수준, 아키텍처, 개발 방법, 검증 전략, 미해결 문제 및 준수 근거를 이해할 수 있도록 구성되어야 한다. 비정상적이거나 새로운 기술 및 방법에 대해 조기에 합의하면 프로그램 후반부에 중대한 해석 차이가 발생할 위험을 줄일 수 있다.

대형 자율 무인항공기(Heavy Autonomous UAV)는 첨단 자율 기능과 인공지능(AI) 기능이 기존 비행제어 소프트웨어보다 빠르게 변화할 수 있기 때문에 추가적인 복잡성을 가진다. 아키텍처는 적절한 보증 근거가 존재하지 않는 한 실험적이거나 자주 갱신되는 알고리즘이 레벨 A 또는 레벨 B 안전 기능의 암묵적인 의존요소가 되지 않도록 해야 한다. 안전 감시기(Safety Monitor)와 제한된 명령 인터페이스(Constrained Command Interface)를 이용하면 첨단 기능이 직접적인 제어 권한을 제한받으면서 시스템에 기여하도록 할 수 있다.

이중화 아키텍처(Redundant Architecture)를 적용한다고 해서 소프트웨어 보증 의무가 자동으로 감소하는 것은 아니다. 동일한 소프트웨어가 여러 이중화 비행 컴퓨터에서 실행되는 경우 공통 소프트웨어 결함(Common Software Defect)이 모든 채널에 동시에 영향을 줄 수 있다. 안전성 분석에서는 이러한 공통모드 가능성(Common-Mode Possibility)을 고려해야 한다. 아키텍처 완화 방법에는 독립 감시, 기능적 비유사성(Functional Dissimilarity), 제한된 명령 인터페이스 또는 기타 방법이 포함될 수 있지만 각각의 완화 수단에는 자체적으로 검증된 요구사항과 근거가 필요하다.

하드웨어 인더루프 시험(Hardware-in-the-Loop Testing)은 실제 프로세서 타이밍, 네트워크, 센서, 액추에이터 및 고장 대응을 재현할 수 있기 때문에 대형 무인항공기의 인증 근거 확보에 특히 유용하다. 고장 주입(Fault Injection)을 통해 프로세서 리셋, 센서 고정(Frozen Sensor), 손상된 메시지, 전원 중단, 액추에이터 고장 및 이중화 채널 불일치를 시험할 수 있다. 이러한 시험은 대표적인 비정상 조건에서 통합 동작을 입증함으로써 하위 수준 검증을 보완한다.

소프트웨어 인더루프(Software-in-the-Loop) 및 시뮬레이션 환경은 광범위한 파라미터 탐색에 여전히 중요하다. 탑재중량, 무게중심(Center of Gravity), 바람, 항법 불확실성, 추진 성능 저하 및 통신 지연에 대한 수천 가지 조합을 평가한 후 대표적인 대상 기반 시험을 선정할 수 있다. 시뮬레이션 결과가 요구되는 검증 근거를 자동으로 대체하는 것은 아니지만 개발 초기 단계에서 취약점을 발견하고 요구사항과 강건성 시험의 완전성을 향상시키는 데 활용할 수 있다.

따라서 레벨 A 및 레벨 B 준수(Level A and Level B Compliance)는 소프트웨어 완성 이후 추가하는 문서 작업이 아니라 시스템 안전과 통합된 공학적 규율(Engineering Discipline)로 이해해야 한다. 요구사항, 아키텍처, 코드, 시험, 커버리지, 형상 통제, 품질보증 및 인증 근거는 개발 전 과정에서 상호 일관성을 유지해야 한다. 이러한 생명주기 규율은 소프트웨어가 고에너지 자율 항공기를 직접 제어하는 경우 특히 중요하다.

5톤 및 10톤급 화물 무인항공기에서 성공적인 DO-178C 준수는 궁극적으로 비행 필수 소프트웨어가 명시적인 요구사항에 따라 동작하고, 의도하지 않은 기능이 체계적으로 처리되며, 모든 변경사항이 통제된다는 신뢰를 뒷받침한다. 시스템 안전성, 이중화 아키텍처, 엄격한 통합 시험 및 운용 제한조건과 결합된 레벨 A 및 레벨 B 보증(Level A and Level B Assurance)은 신뢰성 높은 대형 무인항공기 비행 소프트웨어를 구축하기 위한 체계적인 기반을 제공한다.

##  

## 11.09. 5t 10t Ground Support Equipment SW [w/Code]

![](images/image9.png){width="7.268055555555556in" height="7.268055555555556in"}

Ground support equipment software for 5-ton and 10-ton cargo UAVs coordinates the activities required to prepare, inspect, service, load, configure, launch, recover, and maintain a heavy autonomous aircraft. Unlike simple drone ground applications, this software interacts with high-energy propulsion, large electrical systems, cargo-handling equipment, safety interlocks, maintenance networks, and flight-critical configuration data.

The ground architecture should separate operational control, maintenance functions, logistics services, and safety supervision. A ground control station may manage mission preparation and aircraft status, while dedicated maintenance terminals perform diagnostics and software servicing. Cargo systems, charging or fueling equipment, and launch-area infrastructure can operate through controlled interfaces. Separation limits unintended authority across functions with different safety responsibilities.

Aircraft connection begins with positive identification and configuration verification. Ground software should confirm vehicle identity, hardware configuration, installed software versions, propulsion arrangement, sensor configuration, and maintenance status before allowing mission preparation to proceed. This prevents a valid mission package intended for one aircraft configuration from being loaded into another aircraft whose capabilities or limitations differ.

Preflight configuration management combines mission data with verified aircraft data. Route information, payload characteristics, center-of-gravity estimates, fuel or battery state, weather constraints, communication settings, contingency locations, and operational limitations must form a consistent configuration baseline. Ground software should detect incompatible or incomplete parameters before transferring the mission package to the aircraft.

Cargo handling requires close integration between software and physical loading equipment. Load cells, pallet interfaces, cargo locks, hoists, ramps, doors, and restraint systems may provide digital status to the ground system. The software can compare measured cargo weight and location with the manifest, calculate preliminary mass properties, and verify that loading remains inside structural, center-of-gravity, and restraint limits.

For 5-ton and 10-ton platforms, cargo loading may significantly change landing-gear loads and local structural forces. Ground software should therefore evaluate not only total payload mass but also load distribution. Excessive concentration on a particular attachment point or cargo-zone floor may be unacceptable even when gross aircraft mass remains below its maximum. Loading validation should identify these conditions before flight preparation continues.

Fueling and charging operations require dedicated safety states. Turbine-powered or hybrid aircraft may require fuel quantity, fuel type, temperature, leak status, grounding, and valve configuration to be monitored. Electrified systems require battery state, pack temperature, insulation condition, charger status, connector locking, and permissible current to be verified. Ground software should prevent propulsion activation while incompatible servicing operations remain active.

High-voltage electrical systems require explicit isolation and authorization logic. Maintenance personnel should be able to determine whether propulsion buses, battery contactors, inverters, and stored-energy devices are energized. Ground interfaces should present verified electrical states rather than assumptions based only on commanded switches. Lockout and maintenance procedures should remain consistent with the physical energy state of the aircraft.

Propulsion ground testing requires carefully bounded command authority. Turbine starts, generator tests, motor rotation, actuator motion, or cooling-system operation may be necessary during maintenance, but unrestricted commands can create severe hazards. Ground software should require the correct maintenance mode, authenticated authorization, exclusion-zone confirmation, and predefined command limits before enabling high-energy tests.

Actuator maintenance functions should support inspection without bypassing essential protections. Technicians may command limited control-surface or mechanism motion to verify position sensors, motor currents, hydraulic pressure, travel limits, and mechanical response. Test sequences should restrict velocity, force, and range according to the ground configuration. Unexpected motion or disagreement should cause the sequence to stop in a predictable safe state.

Built-in test data from the aircraft should be collected and organized by the ground system. Flight computers, propulsion controllers, battery systems, sensors, communication units, actuators, and cargo subsystems can report health information and fault codes. Ground software should correlate these reports with configuration and operating history so technicians can distinguish persistent faults from transient events or configuration-dependent behavior.

Maintenance diagnostics should preserve context rather than displaying isolated fault identifiers. A motor overtemperature event is more useful when accompanied by commanded power, ambient conditions, cooling state, flight phase, and related electrical measurements. Timestamped event histories allow maintenance personnel to reconstruct failure sequences and determine whether one component caused secondary faults elsewhere in the aircraft.

Condition-based maintenance can use accumulated operational data to identify degradation before a hard failure occurs. Motor current trends, bearing vibration, actuator response time, battery resistance, generator temperature, communication error rates, and sensor drift can be monitored across flights. Ground software can compare these indicators with established thresholds or models and recommend inspection when deterioration becomes significant.

Software loading is a safety-relevant ground function because incorrect airborne software can invalidate the aircraft configuration. Update systems should verify package identity, version, compatibility, cryptographic authenticity, and installation prerequisites before loading. After installation, the aircraft should report the active software set so ground software can confirm that all required components correspond to an approved configuration baseline.

Rollback and recovery mechanisms are necessary when an update fails or produces an invalid configuration. The system should preserve a known acceptable software image or provide a controlled restoration path. Partial updates across redundant computers can create inconsistent behavior, so installation sequencing and compatibility rules should ensure that channels are either updated coherently or remain in a clearly defined maintenance state.

Parameter and calibration management requires controls similar to executable software management. Flight-control gains, sensor calibrations, actuator limits, mass-property tables, propulsion parameters, and communication settings can directly affect safety even when executable code is unchanged. Ground software should identify, version, validate, authorize, and record parameter changes so unauthorized or accidental modifications cannot silently enter service.

Mission-data loading should remain distinct from maintenance configuration. Operators may need to update routes, schedules, cargo information, weather data, or contingency locations frequently, while certified control parameters should change only under controlled engineering processes. Separating these data classes prevents routine mission preparation from unintentionally modifying safety-critical aircraft configuration.

Preflight health assessment should combine subsystem status into an operational readiness decision. A simple collection of green indicators is insufficient if several individually acceptable degradations combine to reduce safety margin. Ground software can evaluate redundancy status, propulsion reserve, battery condition, navigation integrity, communication availability, actuator health, maintenance deferrals, and payload configuration against mission requirements.

Dispatch logic should distinguish between aircraft airworthiness and mission suitability. An aircraft may remain technically serviceable but lack sufficient capability for a specific payload, route, weather condition, or contingency requirement. Ground software should therefore evaluate the planned mission against the current vehicle state rather than issuing a universal ready/not-ready result independent of operational context.

Ground-to-aircraft data transfer requires integrity and freshness controls. Mission packages, configuration files, navigation databases, and authorization data should include version identifiers, timestamps, checksums or signatures, and applicability information. The aircraft should acknowledge not merely that data were received but that they were validated and activated. Ground systems should retain evidence of the exact dataset accepted for flight.

Communication testing before departure should verify more than basic link connectivity. Command-and-control paths, telemetry, UTM interfaces, cooperative surveillance, backup links, and operator communication may each have different performance requirements. Ground software can measure latency, packet loss, signal quality, authentication state, and redundancy before launch and identify whether the mission depends on a degraded communication path.

Navigation preparation includes verification of databases, reference frames, geofences, departure procedures, destination data, and contingency sites. GNSS, inertial navigation, radar, vision, or other navigation sensors may also require alignment or health checks. The ground system should confirm that the navigation configuration corresponds to the planned operating region and that required data remain within their validity periods.

Weather and environmental information should be incorporated into dispatch preparation with clear timestamps and applicability. Wind, temperature, precipitation, icing risk, visibility, and atmospheric pressure can affect performance and operational limits. For heavy cargo UAVs, ground software should compare environmental conditions with payload mass, propulsion capability, runway or landing-area requirements, and available contingency options.

Launch sequencing should be managed as a state-controlled process. Cargo securement, access panels, doors, servicing connections, maintenance inhibits, landing-area clearance, communication links, navigation readiness, propulsion state, and flight-control health must reach compatible conditions before release. The software should prevent progression when required prerequisites are incomplete rather than relying solely on operator memory.

Human-machine interface design is important because ground crews may work under time pressure around hazardous equipment. Displays should clearly distinguish commanded state from confirmed physical state, warnings from advisory information, and flight configuration from maintenance configuration. Critical actions should require deliberate confirmation without creating excessive routine prompts that encourage operators to acknowledge messages without understanding them.

Role-based access control should restrict functions according to operational responsibility. A cargo operator may need access to load information without permission to modify flight-control parameters, while maintenance personnel may require diagnostic authority but not mission dispatch approval. Engineering functions can require additional authorization. Separating privileges reduces both accidental changes and cybersecurity exposure.

Cybersecurity protections should extend across maintenance laptops, ground stations, wireless links, update servers, removable media, and aircraft gateways. Secure boot, authenticated users, signed software, protected credentials, encrypted communications where appropriate, audit logs, and network segmentation help prevent unauthorized modification. Ground maintenance interfaces should never become an uncontrolled path into flight-critical networks.

Offline operation should be supported because cargo UAVs may operate from remote locations with limited external connectivity. Essential mission preparation, diagnostics, configuration verification, and recovery functions should not depend entirely on cloud availability. Cached databases and credentials require controlled validity periods, while synchronization mechanisms should reconcile records when reliable connectivity becomes available again.

Recovery after landing should include automatic collection of aircraft state and operational records. Flight logs, fault histories, energy usage, propulsion cycles, actuator statistics, navigation performance, and cargo events can be transferred to ground systems. The software should identify events requiring immediate inspection and separate them from information that can be processed later for fleet analytics or engineering analysis.

Fleet-level ground software can compare multiple aircraft and coordinate maintenance resources, software configurations, mission assignments, and spare components. A 5-ton and 10-ton fleet may share common infrastructure while retaining vehicle-specific limits and maintenance requirements. Configuration-aware fleet management prevents assumptions from one aircraft class from being incorrectly applied to another.

Simulation and training modes should be isolated from live-aircraft control. Operators and maintainers need environments for practicing mission preparation, fault diagnosis, cargo loading, and emergency procedures without commanding real hardware. Interfaces can resemble operational systems, but software should make the mode unmistakable and technically prevent simulation commands from reaching connected flight-critical equipment.

Verification of ground support software should include interface errors, stale data, interrupted updates, incorrect configurations, sensor disagreement, communication loss, operator mistakes, and power interruptions. Hardware-in-the-loop environments can connect representative aircraft controllers and ground equipment to test end-to-end behavior. Particular attention should be given to transitions where maintenance authority is transferred back to flight authority.

The most important design principle is that ground support equipment software should make unsafe configurations difficult to create and easy to detect. It must provide reliable evidence that the aircraft, payload, software, energy systems, communications, and mission data form one consistent operational configuration. Automation should reduce workload without concealing the physical state or removing appropriate human oversight.

For 5-ton and 10-ton cargo UAVs, ground support equipment software becomes part of the broader operational safety architecture rather than merely a maintenance utility. By combining configuration control, cargo verification, energy servicing, diagnostics, secure software loading, dispatch assessment, controlled launch sequencing, and post-flight data management, the ground system supports repeatable and traceable operation of heavy autonomous aircraft.

5톤 및 10톤급 화물 무인항공기(Cargo UAV)의 지상지원장비 소프트웨어(Ground Support Equipment Software)는 대형 자율 항공기의 비행 준비, 점검, 정비 지원, 화물 적재, 구성 설정, 출발, 회수 및 유지보수에 필요한 활동을 조정한다. 단순한 드론용 지상 애플리케이션과 달리 이 소프트웨어는 고에너지 추진 시스템, 대규모 전기 시스템, 화물 취급 장비, 안전 인터록(Safety Interlock), 정비 네트워크 및 비행 필수 구성 데이터(Flight-Critical Configuration Data)와 상호작용한다.

지상 아키텍처(Ground Architecture)는 운용 제어, 정비 기능, 물류 서비스 및 안전 감독(Safety Supervision)을 분리해야 한다. 지상통제소(Ground Control Station)는 임무 준비와 기체 상태를 관리할 수 있으며, 전용 정비 단말기(Maintenance Terminal)는 진단과 소프트웨어 정비를 수행한다. 화물 시스템, 충전 또는 급유 장비 및 출발 구역 인프라는 통제된 인터페이스를 통해 운용할 수 있다. 이러한 분리는 서로 다른 안전 책임을 갖는 기능 사이에서 의도하지 않은 제어 권한이 발생하는 것을 제한한다.

기체 연결은 확실한 식별과 구성 검증(Configuration Verification)에서 시작해야 한다. 지상 소프트웨어는 임무 준비를 허용하기 전에 기체 식별정보, 하드웨어 구성, 설치된 소프트웨어 버전, 추진 시스템 구성, 센서 구성 및 정비 상태를 확인해야 한다. 이를 통해 특정 기체 구성을 위해 작성된 유효한 임무 패키지(Mission Package)가 성능이나 제한조건이 다른 다른 기체에 잘못 탑재되는 것을 방지할 수 있다.

비행 전 구성관리(Preflight Configuration Management)는 임무 데이터와 검증된 기체 데이터를 결합한다. 항로 정보, 탑재물 특성, 무게중심(Center of Gravity) 추정값, 연료 또는 배터리 상태, 기상 제약조건, 통신 설정, 비상 대체 지점 및 운용 제한조건은 일관된 구성 기준선(Configuration Baseline)을 형성해야 한다. 지상 소프트웨어는 임무 패키지를 기체로 전송하기 전에 서로 호환되지 않거나 불완전한 파라미터를 탐지해야 한다.

화물 취급(Cargo Handling)은 소프트웨어와 물리적 적재 장비 사이의 긴밀한 통합을 요구한다. 로드셀(Load Cell), 팔레트 인터페이스(Pallet Interface), 화물 잠금장치, 호이스트(Hoist), 램프, 도어 및 고정 시스템(Restraint System)은 지상 시스템에 디지털 상태 정보를 제공할 수 있다. 소프트웨어는 측정된 화물 중량 및 위치를 적하목록(Cargo Manifest)과 비교하고 초기 질량 특성을 계산하며 적재 상태가 구조, 무게중심 및 화물 고정 한계 안에 있는지를 검증할 수 있다.

5톤 및 10톤급 플랫폼에서는 화물 적재가 착륙장치 하중과 국부적인 구조력에 상당한 변화를 발생시킬 수 있다. 따라서 지상 소프트웨어는 전체 탑재중량뿐 아니라 하중 분포(Load Distribution)도 평가해야 한다. 특정 부착 지점이나 화물 구역 바닥에 과도하게 집중된 하중은 기체 총중량이 최대 허용값보다 낮더라도 허용되지 않을 수 있다. 적재 검증(Loading Validation)은 비행 준비가 계속되기 전에 이러한 상태를 식별해야 한다.

급유 및 충전 작업은 전용 안전 상태(Dedicated Safety State)를 필요로 한다. 터빈 또는 하이브리드 항공기에서는 연료량, 연료 종류, 온도, 누출 상태, 접지 및 밸브 구성을 감시해야 할 수 있다. 전동화 시스템에서는 배터리 상태, 팩 온도, 절연 상태, 충전기 상태, 커넥터 잠금 및 허용 전류를 검증해야 한다. 지상 소프트웨어는 서로 양립할 수 없는 정비 작업이 활성화된 상태에서 추진 시스템이 작동하지 못하도록 해야 한다.

고전압 전기 시스템(High-Voltage Electrical System)은 명시적인 격리 및 권한 부여 로직(Isolation and Authorization Logic)을 필요로 한다. 정비 담당자는 추진 전력 버스, 배터리 접촉기(Contactor), 인버터 및 저장 에너지 장치가 실제로 통전 상태인지 확인할 수 있어야 한다. 지상 인터페이스는 단순히 스위치에 전달된 명령을 기반으로 상태를 추정하지 않고 검증된 실제 전기 상태를 표시해야 한다. 잠금 및 정비 절차(Lockout and Maintenance Procedure)는 기체의 실제 물리적 에너지 상태와 일치해야 한다.

추진 시스템 지상시험(Propulsion Ground Testing)은 엄격하게 제한된 명령 권한을 요구한다. 정비 중 터빈 시동, 발전기 시험, 모터 회전, 액추에이터 동작 또는 냉각 시스템 운전이 필요할 수 있지만 제한되지 않은 명령은 심각한 위험을 발생시킬 수 있다. 지상 소프트웨어는 고에너지 시험을 활성화하기 전에 올바른 정비 모드, 인증된 권한, 위험 배제구역(Exclusion Zone) 확인 및 사전에 정의된 명령 제한을 요구해야 한다.

액추에이터 정비 기능(Actuator Maintenance Function)은 필수적인 보호 기능을 우회하지 않으면서 점검을 지원해야 한다. 정비 담당자는 위치 센서, 모터 전류, 유압 압력, 이동 한계 및 기계적 응답을 검증하기 위해 제한된 조종면 또는 메커니즘 움직임을 명령할 수 있다. 시험 절차는 지상 구성에 따라 속도, 힘 및 이동 범위를 제한해야 한다. 예상하지 못한 움직임이나 상태 불일치가 발생하면 시험 절차는 예측 가능한 안전 상태에서 중단되어야 한다.

기체의 내장 시험(Built-In Test) 데이터는 지상 시스템에서 수집하고 체계적으로 관리해야 한다. 비행 컴퓨터, 추진 제어기, 배터리 시스템, 센서, 통신 장치, 액추에이터 및 화물 하위 시스템은 건전성 정보와 고장 코드를 보고할 수 있다. 지상 소프트웨어는 이러한 보고 정보를 기체 구성 및 운용 이력과 연계하여 정비 담당자가 지속적인 고장과 일시적인 이벤트 또는 특정 구성에 의존하는 이상 현상을 구분할 수 있도록 해야 한다.

정비 진단(Maintenance Diagnostics)은 단순히 개별 고장 식별자만 표시하지 않고 관련 상황 정보를 함께 보존해야 한다. 모터 과열 이벤트는 명령 출력, 주변 환경 조건, 냉각 상태, 비행 단계 및 관련 전기 측정값과 함께 제공될 때 더욱 유용하다. 타임스탬프가 포함된 이벤트 이력(Timestamped Event History)을 통해 정비 담당자는 고장 발생 순서를 재구성하고 하나의 구성요소가 기체의 다른 부분에서 2차 고장을 발생시켰는지를 판단할 수 있다.

상태 기반 정비(Condition-Based Maintenance)는 누적된 운용 데이터를 이용하여 심각한 고장이 발생하기 전에 성능 저하를 식별할 수 있다. 모터 전류 추세, 베어링 진동, 액추에이터 응답시간, 배터리 내부 저항, 발전기 온도, 통신 오류율 및 센서 드리프트(Sensor Drift)를 여러 비행에 걸쳐 감시할 수 있다. 지상 소프트웨어는 이러한 지표를 설정된 임계값 또는 모델과 비교하고 성능 저하가 유의미한 수준에 도달하면 점검을 권고할 수 있다.

소프트웨어 탑재(Software Loading)는 잘못된 항공 탑재 소프트웨어가 기체 구성을 무효화할 수 있기 때문에 안전과 관련된 지상 기능이다. 업데이트 시스템은 설치 전에 패키지 식별정보, 버전, 호환성, 암호학적 진위성(Cryptographic Authenticity) 및 설치 선행조건을 검증해야 한다. 설치 후 기체는 활성화된 소프트웨어 집합을 보고하고, 지상 소프트웨어는 모든 필수 구성요소가 승인된 구성 기준선과 일치하는지를 확인해야 한다.

업데이트가 실패하거나 유효하지 않은 구성이 생성될 경우 롤백 및 복구 메커니즘(Rollback and Recovery Mechanism)이 필요하다. 시스템은 정상 동작이 확인된 소프트웨어 이미지(Known Acceptable Software Image)를 보존하거나 통제된 복원 경로를 제공해야 한다. 이중화 컴퓨터에 부분적인 업데이트가 적용되면 서로 일관되지 않은 동작이 발생할 수 있으므로 설치 순서와 호환성 규칙을 통해 각 채널이 일관되게 업데이트되거나 명확하게 정의된 정비 상태로 유지되도록 해야 한다.

파라미터 및 보정 관리(Parameter and Calibration Management)는 실행 소프트웨어 관리와 유사한 수준의 통제를 요구한다. 비행제어 이득, 센서 보정값, 액추에이터 한계, 질량 특성 테이블, 추진 파라미터 및 통신 설정은 실행 코드가 변경되지 않더라도 안전에 직접 영향을 미칠 수 있다. 지상 소프트웨어는 파라미터 변경을 식별하고 버전 관리하며 검증, 승인 및 기록함으로써 승인되지 않았거나 우발적인 변경이 운용 기체에 조용히 적용되는 것을 방지해야 한다.

임무 데이터 탑재(Mission-Data Loading)는 정비 구성(Maintenance Configuration)과 분리되어야 한다. 운용자는 항로, 일정, 화물 정보, 기상 데이터 또는 비상 대체 지점을 자주 갱신해야 할 수 있지만 인증된 제어 파라미터는 통제된 엔지니어링 프로세스를 통해서만 변경되어야 한다. 이러한 데이터 등급을 분리하면 일상적인 임무 준비 과정에서 안전 필수 기체 구성이 의도하지 않게 변경되는 것을 방지할 수 있다.

비행 전 건전성 평가(Preflight Health Assessment)는 여러 하위 시스템의 상태를 통합하여 운용 준비 여부를 결정해야 한다. 여러 개의 개별적인 성능 저하가 각각 허용 가능한 수준이더라도 결합되었을 때 안전 여유를 감소시킬 수 있으므로 단순한 녹색 상태 표시의 집합만으로는 충분하지 않다. 지상 소프트웨어는 이중화 상태, 추진 예비량, 배터리 상태, 항법 무결성, 통신 가용성, 액추에이터 건전성, 정비 유예 항목 및 탑재물 구성을 임무 요구조건과 비교하여 평가할 수 있다.

운항 승인 로직(Dispatch Logic)은 기체의 감항성(Airworthiness)과 특정 임무에 대한 적합성(Mission Suitability)을 구분해야 한다. 항공기가 기술적으로 운용 가능한 상태를 유지하더라도 특정 탑재중량, 항로, 기상 조건 또는 비상 대응 요구조건을 충족할 충분한 능력이 없을 수 있다. 따라서 지상 소프트웨어는 운용 상황과 무관한 단일 준비/비준비 결과를 제공하는 대신 계획된 임무와 현재 기체 상태를 비교하여 평가해야 한다.

지상-기체 데이터 전송(Ground-to-Aircraft Data Transfer)은 무결성 및 최신성 제어(Integrity and Freshness Control)를 필요로 한다. 임무 패키지, 구성 파일, 항법 데이터베이스 및 승인 데이터에는 버전 식별자, 타임스탬프, 체크섬(Checksum) 또는 서명(Signature), 적용 대상 정보가 포함되어야 한다. 기체는 단순히 데이터를 수신했다는 사실만 응답하는 것이 아니라 데이터가 검증되고 활성화되었다는 사실까지 확인해야 한다. 지상 시스템은 비행에 실제 적용된 정확한 데이터 집합에 대한 근거를 보존해야 한다.

출발 전 통신 시험(Communication Testing)은 단순한 링크 연결 여부 이상의 항목을 검증해야 한다. 지휘통제 경로(Command-and-Control Path), 텔레메트리(Telemetry), UTM 인터페이스, 협력식 감시(Cooperative Surveillance), 백업 링크 및 운용자 통신은 각각 서로 다른 성능 요구조건을 가질 수 있다. 지상 소프트웨어는 출발 전에 지연시간, 패킷 손실, 신호 품질, 인증 상태 및 이중화를 측정하고 임무가 성능이 저하된 통신 경로에 의존하고 있는지를 식별할 수 있다.

항법 준비(Navigation Preparation)에는 데이터베이스, 기준 좌표계(Reference Frame), 지오펜스(Geofence), 출발 절차, 목적지 데이터 및 비상 대체 지점의 검증이 포함된다. 위성항법시스템(GNSS), 관성항법(Inertial Navigation), 레이더, 비전 또는 기타 항법 센서에도 정렬(Alignment)이나 건전성 점검이 필요할 수 있다. 지상 시스템은 항법 구성이 계획된 운용 지역과 일치하며 필요한 데이터가 유효기간 안에 있는지를 확인해야 한다.

기상 및 환경 정보는 명확한 타임스탬프와 적용 범위를 포함하여 운항 준비에 반영되어야 한다. 바람, 온도, 강수, 결빙 위험, 가시거리 및 대기압은 성능과 운용 한계에 영향을 미칠 수 있다. 대형 화물 무인항공기의 경우 지상 소프트웨어는 환경 조건을 탑재중량, 추진 능력, 활주로 또는 착륙구역 요구조건 및 이용 가능한 비상 대안과 비교해야 한다.

출발 시퀀싱(Launch Sequencing)은 상태 기반으로 통제되는 프로세스로 관리해야 한다. 화물 고정 상태, 접근 패널, 도어, 정비 연결장치, 정비 작동 금지 상태(Maintenance Inhibit), 착륙구역 안전 확보, 통신 링크, 항법 준비 상태, 추진 상태 및 비행제어 건전성이 서로 호환 가능한 조건에 도달해야 출발을 허용할 수 있다. 소프트웨어는 운용자의 기억에만 의존하지 않고 필요한 선행조건이 충족되지 않은 경우 다음 단계로 진행하지 못하도록 해야 한다.

인간-기계 인터페이스(Human-Machine Interface) 설계는 지상 작업자가 위험한 장비 주변에서 시간 압박을 받으며 작업할 수 있기 때문에 중요하다. 화면은 명령된 상태와 확인된 실제 물리적 상태, 경고(Warning)와 주의 정보(Advisory Information), 비행 구성과 정비 구성을 명확하게 구분해야 한다. 중요 작업에는 의도적인 확인 절차가 필요하지만, 운용자가 내용을 이해하지 않고 반복적으로 확인하도록 만드는 과도한 일상적 알림은 피해야 한다.

역할 기반 접근제어(Role-Based Access Control)는 운용 책임에 따라 기능을 제한해야 한다. 화물 담당자는 비행제어 파라미터를 수정할 권한 없이 적재 정보에 접근할 수 있어야 하며, 정비 담당자는 진단 권한은 필요하지만 임무 운항 승인 권한은 필요하지 않을 수 있다. 엔지니어링 기능에는 추가적인 승인이 요구될 수 있다. 권한 분리는 우발적인 변경과 사이버보안 노출을 동시에 줄일 수 있다.

사이버보안 보호(Cybersecurity Protection)는 정비 노트북, 지상통제소, 무선 링크, 업데이트 서버, 이동식 저장매체 및 기체 게이트웨이까지 확장되어야 한다. 보안 부팅(Secure Boot), 인증된 사용자, 서명된 소프트웨어, 보호된 자격증명, 필요한 경우 암호화 통신, 감사 로그(Audit Log) 및 네트워크 분할(Network Segmentation)은 승인되지 않은 변경을 방지하는 데 도움이 된다. 지상 정비 인터페이스가 비행 필수 네트워크로 연결되는 통제되지 않은 경로가 되어서는 안 된다.

화물 무인항공기가 외부 연결성이 제한된 원격 지역에서 운용될 수 있으므로 오프라인 운용(Offline Operation)을 지원해야 한다. 필수적인 임무 준비, 진단, 구성 검증 및 복구 기능이 클라우드 가용성에 전적으로 의존해서는 안 된다. 캐시된 데이터베이스와 자격증명에는 통제된 유효기간이 필요하며, 안정적인 연결이 다시 확보되면 동기화 메커니즘(Synchronization Mechanism)을 통해 기록의 일관성을 복원해야 한다.

착륙 후 회수(Recovery After Landing) 과정에는 기체 상태와 운용 기록의 자동 수집이 포함되어야 한다. 비행 로그, 고장 이력, 에너지 사용량, 추진 시스템 운전 사이클, 액추에이터 통계, 항법 성능 및 화물 관련 이벤트를 지상 시스템으로 전송할 수 있다. 소프트웨어는 즉각적인 점검이 필요한 이벤트를 식별하고 이후 함대 분석(Fleet Analytics)이나 엔지니어링 분석에서 처리할 수 있는 정보와 구분해야 한다.

함대 수준 지상 소프트웨어(Fleet-Level Ground Software)는 여러 기체를 비교하고 정비 자원, 소프트웨어 구성, 임무 할당 및 예비 부품을 조정할 수 있다. 5톤 및 10톤급 기체로 구성된 함대는 공통 인프라를 공유하면서도 기체별 제한조건과 정비 요구사항을 유지할 수 있다. 구성 인식형 함대 관리(Configuration-Aware Fleet Management)는 한 기체 등급의 가정이 다른 기체 등급에 잘못 적용되는 것을 방지한다.

시뮬레이션 및 훈련 모드(Simulation and Training Mode)는 실제 기체 제어와 격리되어야 한다. 운용자와 정비 담당자는 실제 하드웨어에 명령을 전달하지 않고 임무 준비, 고장 진단, 화물 적재 및 비상 절차를 연습할 수 있는 환경이 필요하다. 인터페이스는 실제 운용 시스템과 유사할 수 있지만 현재 모드를 명확하게 식별할 수 있어야 하며, 시뮬레이션 명령이 연결된 비행 필수 장비로 전달되는 것을 기술적으로 방지해야 한다.

지상지원장비 소프트웨어 검증(Ground Support Equipment Software Verification)은 인터페이스 오류, 오래된 데이터, 중단된 업데이트, 잘못된 구성, 센서 불일치, 통신 손실, 운용자 실수 및 전원 중단을 포함해야 한다. 하드웨어 인더루프 환경(Hardware-in-the-Loop Environment)은 대표적인 기체 제어기와 지상 장비를 연결하여 종단 간 동작(End-to-End Behavior)을 시험할 수 있다. 특히 정비 제어 권한이 다시 비행 제어 권한으로 전환되는 과정에 주의를 기울여야 한다.

가장 중요한 설계 원칙은 지상지원장비 소프트웨어가 안전하지 않은 구성을 생성하기 어렵게 만들고, 그러한 구성이 발생하더라도 쉽게 탐지할 수 있도록 하는 것이다. 기체, 탑재물, 소프트웨어, 에너지 시스템, 통신 및 임무 데이터가 하나의 일관된 운용 구성(Operational Configuration)을 형성한다는 신뢰할 수 있는 근거를 제공해야 한다. 자동화는 물리적 상태를 숨기거나 적절한 인간 감독(Human Oversight)을 제거하지 않으면서 작업 부담을 감소시켜야 한다.

5톤 및 10톤급 화물 무인항공기에서 지상지원장비 소프트웨어(Ground Support Equipment Software)는 단순한 정비 유틸리티가 아니라 광범위한 운용 안전 아키텍처(Operational Safety Architecture)의 일부가 된다. 구성 통제, 화물 검증, 에너지 정비, 진단, 안전한 소프트웨어 탑재, 운항 적합성 평가, 통제된 출발 시퀀싱 및 비행 후 데이터 관리를 통합함으로써 지상 시스템은 대형 자율 항공기의 반복 가능하고 추적 가능한 운용을 지원한다.

##  

## 11.10. 5t 10t UAV Certification Campaign Overview

![](images/image10.png){width="7.268055555555556in" height="7.268055555555556in"}

A certification campaign for 5-ton and 10-ton cargo UAVs is a coordinated engineering and regulatory program that demonstrates the aircraft can perform its intended missions with an acceptable level of safety. Because these vehicles combine heavy payloads, autonomous functions, complex propulsion, digital flight control, and ground infrastructure, certification must integrate aircraft design, software, hardware, operations, maintenance, and continuing airworthiness evidence.

The campaign begins by defining the certification basis and intended operational concept. Aircraft category, maximum takeoff mass, cargo mission, operating altitude, airspace class, takeoff and landing method, autonomy level, command-and-control architecture, and environmental envelope influence the applicable requirements. Early agreement with the certification authority reduces the risk that major design assumptions must be changed after verification has already begun.

The concept of operations should describe how the aircraft is actually intended to function throughout a mission. Ground preparation, cargo loading, dispatch, departure, en-route operation, interaction with air traffic, contingency management, arrival, landing, and maintenance all contribute to safety. Certification evidence must therefore demonstrate not only that individual components operate correctly but that the complete operational system behaves predictably.

A certification plan translates the regulatory basis into specific means of compliance. Analysis, inspection, simulation, laboratory testing, hardware-in-the-loop testing, ground testing, and flight testing can each provide different evidence. The plan should identify which method supports each requirement, what configuration will be tested, which organization is responsible, and what acceptance criteria determine successful compliance.

System safety assessment provides the risk framework for the campaign. Functional hazard assessment identifies failure conditions and classifies their severity, while preliminary and detailed safety analyses allocate safety requirements to aircraft systems. Redundancy, monitoring, independence, fault containment, and operational limitations are then evaluated to determine whether catastrophic, hazardous, and other failure conditions meet the required safety objectives.

The architecture should be stabilized sufficiently early for safety assumptions to remain meaningful. Flight-control computers, communication networks, sensors, actuators, propulsion channels, electrical buses, navigation systems, and ground interfaces have dependencies that influence failure behavior. Late architectural changes can invalidate safety analyses and previously completed tests, creating extensive recertification work even when the individual change appears small.

Requirements management connects certification objectives to engineering implementation. Aircraft-level requirements flow into system, subsystem, software, hardware, interface, and operational requirements. Bidirectional traceability allows each verification result to be connected back to an approved requirement and allows each implemented feature to be justified. Missing or inconsistent traceability is often evidence of incomplete design or verification.

Software assurance is a major element because heavy UAVs depend on digital control for stabilization, navigation, propulsion coordination, redundancy management, and contingency handling. DO-178C processes may be applied according to the safety classification of individual functions. Level A or Level B activities can require rigorous requirements, design, coding, verification independence, structural coverage, configuration management, and quality-assurance evidence.

Airborne electronic hardware requires a corresponding assurance strategy. Complex processing hardware, programmable logic, interface devices, and custom electronics may require structured development and verification evidence appropriate to their safety role. Hardware and software assurance cannot be treated independently when timing, partitioning, memory protection, communication, or device behavior contributes to the safety architecture of the integrated flight computer.

Propulsion certification must demonstrate both nominal performance and safe behavior after failures. For electric, turbine, or turbine-hybrid configurations, evidence should address power availability, thermal limits, energy reserves, control response, isolation, fire or thermal hazards, and degraded operation. A 10-ton aircraft may require substantial demonstration of continued controllability after loss or limitation of a propulsion channel.

Electrical power systems must support flight-critical loads under expected normal and abnormal conditions. Certification testing should examine generator or battery failures, bus faults, converter failures, load shedding, transient voltage behavior, thermal conditions, and emergency power endurance. Analysis must also show that common electrical dependencies do not defeat apparently redundant flight-control, navigation, communication, or propulsion functions.

Flight-control certification progressively demonstrates stability, controllability, handling qualities, envelope protection, and failure response. Simulation establishes initial expectations, ground integration verifies implementation, and flight tests confirm real-aircraft behavior. For autonomous cargo UAVs, control laws must be evaluated across payload mass, center-of-gravity positions, environmental conditions, actuator limits, propulsion states, and degraded configurations.

Cargo integration becomes a certification subject because payload mass and location directly influence structural loads and controllability. Loading limits, cargo restraint, floor or attachment strength, center-of-gravity boundaries, and loading procedures require verification. If cargo can shift, swing, or contain liquids, dynamic effects may need additional analysis and testing to demonstrate that expected cargo behavior does not destabilize the aircraft.

Structural substantiation should cover limit loads, ultimate loads, fatigue, vibration, landing loads, propulsion loads, and cargo-induced stresses. Analytical models can be supported by material, component, and full-scale tests. Software-based load alleviation may contribute to compliance, but dependence on active control creates additional assurance obligations because structural safety then depends partly on correct sensor, computation, and actuator behavior.

Environmental qualification demonstrates that airborne equipment operates under the temperature, vibration, shock, humidity, electromagnetic, power-input, and other environmental conditions expected in service. Qualification levels should correspond to actual installation locations and aircraft environments. Heavy electric and hybrid UAVs may require particular attention to electromagnetic compatibility because high-power converters and motors operate near sensitive avionics and communication systems.

Navigation and surveillance evidence must support the intended airspace operation. Position accuracy alone is insufficient; integrity, continuity, availability, failure detection, and degraded-mode behavior are also relevant. GNSS interference, inertial drift, sensor disagreement, and navigation-source transitions should be evaluated. The required rigor increases when autonomous separation or route conformance depends directly on navigation performance.

Command-and-control communication requires performance and failure analysis across the intended operating range. Coverage, latency, continuity, interference, authentication, backup links, and lost-link behavior should be demonstrated. The certification campaign should show that communication degradation does not lead to uncontrolled aircraft behavior and that predefined contingency procedures remain feasible with the navigation, energy, and traffic information still available.

UTM and conventional air traffic integration must be demonstrated at both interface and operational levels. Route authorization, geofencing, surveillance, flight-intent exchange, conflict management, emergency status, and interaction with human controllers may require staged trials. External services should remain separated from fundamental aircraft stabilization so that traffic-service outages degrade coordination without directly removing onboard flight safety.

Detect-and-avoid capability requires representative encounter scenarios rather than only nominal traffic observations. Testing should include converging traffic, crossing encounters, delayed surveillance, noncooperative targets where applicable, navigation uncertainty, and communication degradation. Avoidance trajectories must be shown to remain within aircraft maneuver, structural, propulsion, energy, cargo, and airspace constraints.

Cybersecurity assessment should address interfaces through which unauthorized actions could affect safety. Aircraft networks, ground stations, maintenance ports, update mechanisms, wireless links, credentials, and external services require protection appropriate to their exposure. Security controls must also be assessed for safety impact, because authentication delays, failed updates, or unavailable security services must not create unacceptable flight-control behavior.

Ground support equipment forms part of the certification campaign when it loads safety-relevant data, performs maintenance, services high-energy systems, or determines dispatch readiness. Mission loading, software updates, calibration management, cargo verification, charging or fueling, diagnostics, and launch sequencing require controlled interfaces. Evidence should demonstrate that foreseeable ground errors are detected before they create an unsafe flight configuration.

Simulation is used extensively before hazardous or expensive flight tests. High-fidelity aircraft models can explore combinations of payload, wind, sensor errors, actuator faults, propulsion degradation, communication delay, and navigation uncertainty. Monte Carlo campaigns help identify boundary cases and support test selection. Simulation credibility depends on model validation, configuration control, assumptions, and comparison with measured aircraft data.

Software-in-the-loop testing verifies algorithms and logic over broad scenario sets, while hardware-in-the-loop testing introduces real flight computers, networks, interfaces, and timing behavior. Fault injection can reproduce sensor freezes, corrupted messages, processor resets, actuator failures, propulsion faults, and communication outages. These environments allow dangerous failure combinations to be investigated before equivalent flight-test exposure is considered.

Ground testing establishes that the integrated aircraft is physically ready for flight. Power-on sequences, propulsion operation, actuator motion, communication, navigation alignment, thermal behavior, electromagnetic compatibility, emergency shutdown, and ground safety interlocks can be evaluated. Taxi, tethered, restrained, or low-energy tests may provide intermediate steps depending on the vehicle\'s takeoff and propulsion configuration.

Flight-test expansion should proceed from low-risk configurations toward the boundaries of the intended operating envelope. Initial flights verify basic stability, command response, navigation, communication, and recovery. Later testing introduces representative payloads, extreme center-of-gravity conditions, higher speeds, stronger winds, system failures, autonomous rerouting, and contingency procedures only after prerequisite evidence demonstrates adequate margin.

Instrumentation and telemetry must provide sufficient evidence to evaluate each test objective. Flight-control states, actuator commands, structural loads, propulsion performance, electrical conditions, navigation estimates, communication quality, environmental data, and safety-monitor status may need synchronized recording. Accurate timestamps allow investigators to reconstruct interactions across systems and distinguish physical response from communication or processing delay.

Test configuration control is essential because evidence is valid only for a known aircraft configuration. Hardware part numbers, software versions, calibration datasets, payload arrangement, sensor installations, structural modifications, and test equipment must be recorded. If the aircraft changes during the campaign, engineering analysis must determine which previous evidence remains applicable and which tests or analyses require repetition.

Certification findings and anomalies should be managed through formal problem-reporting processes. Unexpected flight behavior, test failures, software defects, hardware faults, and documentation inconsistencies require impact assessment and controlled resolution. Corrective changes can affect requirements, safety analyses, software coverage, hardware evidence, simulation models, and previous test results, so regression scope must be determined systematically.

Compliance documentation should form a coherent argument rather than a collection of unrelated reports. Safety analyses explain why particular assurance and redundancy measures are required, development records show how they were implemented, and verification evidence demonstrates that they work. Certification reviewers should be able to follow the relationship from regulatory objective through requirement, implementation, test evidence, and final compliance conclusion.

The 5-ton and 10-ton variants may share a common certification framework, but commonality must be demonstrated rather than assumed. Shared software, avionics, ground systems, and operational procedures can reduce duplicated effort, while differences in mass, propulsion, structural loads, maneuverability, and failure consequences require variant-specific evidence. Reuse is strongest when configuration differences and applicability rules are explicitly controlled.

Operational limitations may be used where design or evidence does not support unrestricted operation. Payload limits, wind limits, approved routes, minimum communication coverage, required diversion sites, maintenance intervals, or prohibited weather conditions can define a safe operating envelope. Such limitations must be measurable, enforceable, documented, and reflected consistently in mission planning and operational procedures.

Continuing airworthiness extends the certification discipline beyond initial approval. In-service faults, software updates, hardware modifications, component aging, maintenance findings, and operational events must be monitored for safety impact. Configuration control and change assessment determine whether modifications require additional analysis, regression testing, authority involvement, or revision of operating limitations.

The certification campaign ultimately integrates evidence from safety engineering, design assurance, simulation, laboratory testing, ground testing, and flight testing into a defensible demonstration of aircraft safety. For heavy autonomous UAVs, no single successful flight or subsystem test is sufficient. Confidence emerges from consistent evidence showing that nominal behavior, foreseeable failures, operational interfaces, and recovery mechanisms all remain within defined safety boundaries.

For 5-ton and 10-ton cargo UAVs, a successful certification campaign therefore requires certification activities to evolve together with the aircraft rather than follow development as a final approval step. Early safety allocation, controlled architecture, traceable requirements, rigorous assurance, progressive testing, configuration discipline, and continuing operational feedback provide the foundation for introducing heavy autonomous cargo aircraft into real-world airspace with predictable and verifiable behavior.

5톤 및 10톤급 화물 무인항공기(Cargo UAV)의 인증 캠페인(Certification Campaign)은 항공기가 의도된 임무를 허용 가능한 수준의 안전성으로 수행할 수 있음을 입증하는 통합 엔지니어링 및 규제 프로그램이다. 이러한 기체는 대형 탑재물, 자율 기능, 복잡한 추진 시스템, 디지털 비행제어 및 지상 인프라를 결합하므로 인증에서는 항공기 설계, 소프트웨어, 하드웨어, 운용, 정비 및 지속감항성(Continuing Airworthiness)에 관한 근거를 통합해야 한다.

인증 캠페인은 인증 기준(Certification Basis)과 의도된 운용 개념(Operational Concept)을 정의하는 것에서 시작한다. 항공기 등급, 최대이륙중량(Maximum Takeoff Mass), 화물 임무, 운용 고도, 공역 등급, 이착륙 방식, 자율화 수준, 지휘통제 아키텍처(Command-and-Control Architecture) 및 환경 운용범위가 적용 요구사항에 영향을 준다. 인증 당국(Certification Authority)과 조기에 합의하면 검증이 이미 진행된 이후 주요 설계 가정을 변경해야 하는 위험을 줄일 수 있다.

운용 개념(Concept of Operations)은 항공기가 전체 임무 과정에서 실제로 어떻게 운용될 것인지를 설명해야 한다. 지상 준비, 화물 적재, 운항 승인(Dispatch), 출발, 항로 운항, 항공교통과의 상호작용, 비상상황 관리, 도착, 착륙 및 정비가 모두 안전성에 기여한다. 따라서 인증 근거는 개별 구성요소가 올바르게 동작한다는 것뿐 아니라 전체 운용 시스템이 예측 가능한 방식으로 동작한다는 것도 입증해야 한다.

인증 계획(Certification Plan)은 규제 기준을 구체적인 적합성 입증 방법(Means of Compliance)으로 변환한다. 분석, 검사, 시뮬레이션, 실험실 시험, 하드웨어 인더루프 시험(Hardware-in-the-Loop Testing), 지상시험 및 비행시험은 각각 서로 다른 근거를 제공할 수 있다. 계획에서는 각 요구사항을 어떤 방법으로 입증할 것인지, 어떤 구성을 시험할 것인지, 어떤 조직이 책임을 담당하는지, 그리고 어떤 합격 기준이 적합성을 결정하는지를 정의해야 한다.

시스템 안전성 평가(System Safety Assessment)는 인증 캠페인의 위험관리 프레임워크를 제공한다. 기능 위험성 평가(Functional Hazard Assessment)는 고장 조건을 식별하고 심각도를 분류하며, 예비 및 상세 안전성 분석은 안전 요구사항을 항공기 시스템에 할당한다. 이후 이중화, 감시, 독립성, 고장 격리(Fault Containment) 및 운용 제한조건을 평가하여 치명적, 위험 및 기타 고장 조건이 요구되는 안전 목표를 충족하는지를 판단한다.

아키텍처는 안전성 가정이 계속 유효할 수 있도록 충분히 이른 단계에서 안정화되어야 한다. 비행제어 컴퓨터, 통신 네트워크, 센서, 액추에이터, 추진 채널, 전기 버스, 항법 시스템 및 지상 인터페이스는 고장 동작에 영향을 미치는 상호 의존성을 가진다. 늦은 단계의 아키텍처 변경은 개별 변경사항이 작아 보이더라도 안전성 분석과 이미 완료된 시험을 무효화하여 광범위한 재인증 작업을 발생시킬 수 있다.

요구사항 관리(Requirements Management)는 인증 목표와 엔지니어링 구현을 연결한다. 항공기 수준 요구사항은 시스템, 하위 시스템, 소프트웨어, 하드웨어, 인터페이스 및 운용 요구사항으로 전개된다. 양방향 추적성(Bidirectional Traceability)을 통해 각각의 검증 결과를 승인된 요구사항까지 연결하고 구현된 각 기능의 근거를 확인할 수 있다. 누락되거나 일관되지 않은 추적성은 불완전한 설계 또는 검증을 나타내는 근거가 될 수 있다.

소프트웨어 보증(Software Assurance)은 대형 무인항공기가 안정화, 항법, 추진 조정, 이중화 관리 및 비상상황 처리에서 디지털 제어에 의존하기 때문에 인증의 핵심 요소이다. 개별 기능의 안전성 분류에 따라 DO-178C 프로세스를 적용할 수 있다. 레벨 A(Level A) 또는 레벨 B(Level B) 활동에는 엄격한 요구사항, 설계, 코딩, 검증 독립성, 구조적 커버리지(Structural Coverage), 형상관리(Configuration Management) 및 품질보증 근거가 요구될 수 있다.

항공 탑재 전자 하드웨어(Airborne Electronic Hardware)에도 이에 대응하는 보증 전략이 필요하다. 복잡한 처리 하드웨어, 프로그래머블 로직(Programmable Logic), 인터페이스 장치 및 주문형 전자장치는 안전성 역할에 적합한 체계적인 개발 및 검증 근거를 요구할 수 있다. 타이밍, 파티셔닝(Partitioning), 메모리 보호, 통신 또는 장치 동작이 통합 비행 컴퓨터의 안전 아키텍처에 기여하는 경우 하드웨어와 소프트웨어 보증을 서로 독립적으로 다루어서는 안 된다.

추진 시스템 인증(Propulsion Certification)은 정상 성능뿐 아니라 고장 발생 이후의 안전한 동작도 입증해야 한다. 전기식, 터빈식 또는 터빈-하이브리드(Turbine-Hybrid) 구성에서는 가용 출력, 열 한계, 에너지 예비량, 제어 응답, 격리, 화재 또는 열적 위험 및 성능 저하 운용을 검증해야 한다. 10톤급 항공기에서는 하나의 추진 채널이 손실되거나 제한된 이후에도 조종성을 유지할 수 있음을 상당한 수준으로 입증해야 할 수 있다.

전력 시스템(Electrical Power System)은 예상되는 정상 및 비정상 조건에서 비행 필수 부하(Flight-Critical Load)를 지원해야 한다. 인증 시험에서는 발전기 또는 배터리 고장, 버스 고장, 변환기 고장, 부하 차단(Load Shedding), 과도 전압 동작, 열적 조건 및 비상 전원 지속시간을 평가해야 한다. 또한 공통 전기 의존성이 외형상 이중화된 비행제어, 항법, 통신 또는 추진 기능을 동시에 무력화하지 않는다는 것을 분석을 통해 입증해야 한다.

비행제어 인증(Flight-Control Certification)은 안정성, 조종성, 조종 특성(Handling Qualities), 비행영역 보호(Envelope Protection) 및 고장 대응을 단계적으로 입증한다. 시뮬레이션은 초기 예상 동작을 설정하고, 지상 통합시험은 구현 상태를 검증하며, 비행시험은 실제 기체 동작을 확인한다. 자율 화물 무인항공기의 제어법칙(Control Law)은 탑재중량, 무게중심(Center of Gravity), 환경 조건, 액추에이터 한계, 추진 상태 및 성능 저하 구성 전체에 걸쳐 평가되어야 한다.

화물 통합(Cargo Integration)은 탑재물의 질량과 위치가 구조 하중 및 조종성에 직접 영향을 미치므로 인증 대상이 된다. 적재 한계, 화물 고정(Cargo Restraint), 바닥 또는 부착부 강도, 무게중심 경계 및 적재 절차를 검증해야 한다. 화물이 이동하거나 흔들릴 수 있거나 액체를 포함하는 경우에는 예상되는 화물 동작이 항공기를 불안정하게 만들지 않는다는 것을 입증하기 위해 추가적인 동적 분석과 시험이 필요할 수 있다.

구조 입증(Structural Substantiation)은 제한 하중(Limit Load), 극한 하중(Ultimate Load), 피로, 진동, 착륙 하중, 추진 하중 및 화물에 의해 발생하는 응력을 포함해야 한다. 해석 모델은 재료, 구성품 및 실기체 규모 시험으로 보완할 수 있다. 소프트웨어 기반 하중 경감(Load Alleviation)이 적합성 입증에 기여할 수도 있지만, 능동제어에 대한 의존은 구조 안전성이 센서, 연산 및 액추에이터의 올바른 동작에도 부분적으로 의존하게 하므로 추가적인 보증 의무를 발생시킨다.

환경 적격성(Environmental Qualification)은 항공 탑재 장비가 실제 운용에서 예상되는 온도, 진동, 충격, 습도, 전자기 환경, 입력 전원 및 기타 환경 조건에서 동작함을 입증한다. 적격성 수준은 실제 장착 위치와 항공기 환경에 대응해야 한다. 대형 전기 및 하이브리드 무인항공기에서는 고출력 변환기와 모터가 민감한 항공전자장비 및 통신 시스템 가까이에서 동작하므로 전자기 적합성(Electromagnetic Compatibility)에 특별한 주의가 필요할 수 있다.

항법 및 감시(Navigation and Surveillance) 근거는 의도된 공역 운용을 지원해야 한다. 위치 정확도만으로는 충분하지 않으며 무결성, 연속성, 가용성, 고장 탐지 및 성능 저하 모드 동작도 중요하다. 위성항법시스템(GNSS) 간섭, 관성 드리프트(Inertial Drift), 센서 불일치 및 항법 정보원 전환을 평가해야 한다. 자율 분리 또는 항로 준수(Route Conformance)가 항법 성능에 직접 의존하는 경우 요구되는 검증 엄격성은 더욱 높아진다.

지휘통제 통신(Command-and-Control Communication)은 의도된 운용 범위 전체에서 성능 및 고장 분석이 필요하다. 통신 범위, 지연시간, 연속성, 간섭, 인증(Authentication), 백업 링크 및 통신 두절 동작(Lost-Link Behavior)을 입증해야 한다. 인증 캠페인은 통신 성능 저하가 통제되지 않은 기체 동작으로 이어지지 않으며, 남아 있는 항법, 에너지 및 교통 정보를 이용하여 사전에 정의된 비상 절차를 계속 수행할 수 있음을 보여주어야 한다.

무인항공기 교통관리(UTM, Unmanned Aircraft System Traffic Management) 및 기존 항공교통 통합은 인터페이스 수준과 실제 운용 수준 모두에서 입증되어야 한다. 항로 승인, 지오펜싱(Geofencing), 감시, 비행 의도 교환, 충돌 관리, 비상 상태 및 인간 관제사와의 상호작용은 단계적인 시험을 요구할 수 있다. 외부 교통 서비스는 기본적인 기체 안정화 기능과 분리되어 교통 서비스 장애가 조정 기능을 저하시킬 수는 있어도 탑재 비행 안전 기능을 직접 제거하지 않도록 해야 한다.

탐지회피 능력(Detect-and-Avoid Capability)은 정상적인 교통 관측만이 아니라 대표적인 조우 시나리오(Encounter Scenario)를 통해 검증해야 한다. 시험에는 접근 교통, 교차 조우, 지연된 감시 정보, 필요한 경우 비협력 표적, 항법 불확실성 및 통신 성능 저하를 포함해야 한다. 회피 궤적은 기체의 기동, 구조, 추진, 에너지, 화물 및 공역 제약조건 안에서 유지될 수 있음을 입증해야 한다.

사이버보안 평가(Cybersecurity Assessment)는 승인되지 않은 행위가 안전에 영향을 줄 수 있는 인터페이스를 다루어야 한다. 항공기 네트워크, 지상통제소, 정비 포트, 업데이트 메커니즘, 무선 링크, 자격증명 및 외부 서비스에는 노출 수준에 적합한 보호가 필요하다. 보안 제어 자체가 안전성에 미치는 영향도 평가해야 하며, 인증 지연, 업데이트 실패 또는 보안 서비스의 비가용성이 허용할 수 없는 비행제어 동작을 발생시켜서는 안 된다.

지상지원장비(Ground Support Equipment)가 안전 관련 데이터를 탑재하거나 정비를 수행하고, 고에너지 시스템을 서비스하거나 운항 준비 상태를 결정하는 경우 인증 캠페인의 일부가 된다. 임무 데이터 탑재, 소프트웨어 업데이트, 보정 관리, 화물 검증, 충전 또는 급유, 진단 및 출발 시퀀싱(Launch Sequencing)은 통제된 인터페이스를 필요로 한다. 예측 가능한 지상 작업 오류가 안전하지 않은 비행 구성으로 이어지기 전에 탐지된다는 것을 근거를 통해 입증해야 한다.

시뮬레이션(Simulation)은 위험하거나 비용이 높은 비행시험을 수행하기 전에 광범위하게 활용된다. 고충실도 항공기 모델(High-Fidelity Aircraft Model)을 이용하여 탑재중량, 바람, 센서 오류, 액추에이터 고장, 추진 성능 저하, 통신 지연 및 항법 불확실성의 조합을 탐색할 수 있다. 몬테카를로 캠페인(Monte Carlo Campaign)은 경계 사례를 식별하고 시험 선정에 도움을 준다. 시뮬레이션의 신뢰성은 모델 검증, 형상 통제, 가정 및 실제 기체 측정 데이터와의 비교에 달려 있다.

소프트웨어 인더루프 시험(Software-in-the-Loop Testing)은 광범위한 시나리오에서 알고리즘과 로직을 검증하며, 하드웨어 인더루프 시험(Hardware-in-the-Loop Testing)은 실제 비행 컴퓨터, 네트워크, 인터페이스 및 타이밍 동작을 추가한다. 고장 주입(Fault Injection)을 통해 센서 고정, 손상된 메시지, 프로세서 리셋, 액추에이터 고장, 추진계 고장 및 통신 두절을 재현할 수 있다. 이러한 환경을 이용하면 동등한 수준의 위험을 비행시험에서 감수하기 전에 위험한 고장 조합을 조사할 수 있다.

지상시험(Ground Testing)은 통합된 항공기가 실제 비행을 수행할 물리적인 준비가 되어 있음을 확인한다. 전원 인가 시퀀스, 추진 시스템 동작, 액추에이터 운동, 통신, 항법 정렬, 열적 동작, 전자기 적합성, 비상 정지 및 지상 안전 인터록을 평가할 수 있다. 기체의 이륙 및 추진 구성에 따라 지상 활주, 계류 시험(Tethered Test), 구속 시험 또는 저에너지 시험을 중간 단계로 사용할 수 있다.

비행시험 확대(Flight-Test Expansion)는 저위험 구성에서 시작하여 의도된 운용 엔벌로프(Operating Envelope)의 경계 방향으로 점진적으로 진행해야 한다. 초기 비행에서는 기본 안정성, 명령 응답, 항법, 통신 및 회수 기능을 검증한다. 이후 선행 근거가 충분한 안전 여유를 입증한 다음에만 대표 탑재물, 극단적인 무게중심 조건, 높은 속도, 강한 바람, 시스템 고장, 자율 재경로 설정 및 비상 절차를 단계적으로 도입해야 한다.

계측 및 텔레메트리(Instrumentation and Telemetry)는 각 시험 목표를 평가할 수 있는 충분한 근거를 제공해야 한다. 비행제어 상태, 액추에이터 명령, 구조 하중, 추진 성능, 전기적 상태, 항법 추정값, 통신 품질, 환경 데이터 및 안전 감시기(Safety Monitor) 상태를 동기화하여 기록해야 할 수 있다. 정확한 타임스탬프를 사용하면 여러 시스템의 상호작용을 재구성하고 물리적 응답과 통신 또는 처리 지연을 구분할 수 있다.

시험 형상관리(Test Configuration Control)는 인증 근거가 알려진 항공기 구성에 대해서만 유효하기 때문에 필수적이다. 하드웨어 부품번호, 소프트웨어 버전, 보정 데이터세트, 탑재물 배치, 센서 설치 상태, 구조 변경사항 및 시험 장비를 기록해야 한다. 인증 캠페인 도중 기체가 변경되면 어떤 기존 근거가 계속 적용될 수 있고 어떤 시험 또는 분석을 반복해야 하는지를 엔지니어링 분석을 통해 결정해야 한다.

인증 과정에서 발견된 사항과 이상 현상(Anomaly)은 공식적인 문제 보고 프로세스(Problem-Reporting Process)를 통해 관리해야 한다. 예상하지 못한 비행 동작, 시험 실패, 소프트웨어 결함, 하드웨어 고장 및 문서 불일치는 영향 평가와 통제된 해결 절차를 필요로 한다. 수정사항은 요구사항, 안전성 분석, 소프트웨어 커버리지, 하드웨어 근거, 시뮬레이션 모델 및 기존 시험 결과에 영향을 줄 수 있으므로 회귀 검증 범위(Regression Scope)를 체계적으로 결정해야 한다.

적합성 문서(Compliance Documentation)는 서로 관련 없는 보고서의 집합이 아니라 일관된 논증(Coherent Argument)을 형성해야 한다. 안전성 분석은 특정 보증 및 이중화 조치가 필요한 이유를 설명하고, 개발 기록은 이러한 조치가 어떻게 구현되었는지를 보여주며, 검증 근거는 실제로 올바르게 동작함을 입증한다. 인증 검토자는 규제 목표에서 요구사항, 구현, 시험 근거 및 최종 적합성 결론까지의 관계를 추적할 수 있어야 한다.

5톤 및 10톤급 파생형(Variant)은 공통 인증 프레임워크를 공유할 수 있지만 공통성(Commonality)은 가정하는 것이 아니라 입증해야 한다. 공통 소프트웨어, 항공전자장비, 지상 시스템 및 운용 절차는 중복 작업을 줄일 수 있지만 질량, 추진 시스템, 구조 하중, 기동성 및 고장 결과의 차이는 파생형별 인증 근거를 요구한다. 구성 차이와 적용성 규칙(Applicability Rule)이 명확하게 통제될수록 기존 인증 근거를 효과적으로 재사용할 수 있다.

설계 또는 검증 근거가 제한 없는 운용을 지원하지 못하는 경우 운용 제한조건(Operational Limitation)을 적용할 수 있다. 탑재중량 한계, 풍속 한계, 승인된 항로, 최소 통신 범위, 필수 회항 지점, 정비 주기 또는 금지 기상 조건 등을 통해 안전한 운용 엔벌로프를 정의할 수 있다. 이러한 제한조건은 측정 가능하고 강제 적용이 가능하며 문서화되어야 하고, 임무 계획 및 운용 절차에도 일관되게 반영되어야 한다.

지속감항성(Continuing Airworthiness)은 최초 승인 이후에도 인증 규율을 계속 적용한다. 실제 운용 중 발생하는 고장, 소프트웨어 업데이트, 하드웨어 변경, 구성품 노화, 정비 결과 및 운용 이벤트를 감시하고 안전성에 미치는 영향을 평가해야 한다. 형상 통제와 변경 영향 평가(Change Assessment)를 통해 수정사항에 추가 분석, 회귀시험, 인증 당국의 검토 또는 운용 제한조건 수정이 필요한지를 결정한다.

인증 캠페인은 궁극적으로 안전공학, 설계보증(Design Assurance), 시뮬레이션, 실험실 시험, 지상시험 및 비행시험에서 확보된 근거를 통합하여 항공기 안전성을 방어 가능한 형태로 입증한다. 대형 자율 무인항공기에서는 단 한 번의 성공적인 비행이나 개별 하위 시스템 시험만으로 충분하지 않다. 정상 동작, 예측 가능한 고장, 운용 인터페이스 및 복구 메커니즘이 모두 정의된 안전 경계 안에 유지된다는 일관된 근거의 축적을 통해 신뢰성을 확보해야 한다.

5톤 및 10톤급 화물 무인항공기의 성공적인 인증 캠페인은 인증 활동을 개발이 완료된 이후 수행하는 최종 승인 단계로 취급하지 않고 항공기 개발과 함께 발전시키는 것을 요구한다. 초기 안전성 할당(Early Safety Allocation), 통제된 아키텍처, 추적 가능한 요구사항, 엄격한 보증 활동, 단계적 시험, 형상관리 규율 및 지속적인 운용 피드백을 결합함으로써 대형 자율 화물 항공기를 실제 공역에 예측 가능하고 검증 가능한 방식으로 도입하기 위한 기반을 구축할 수 있다.
