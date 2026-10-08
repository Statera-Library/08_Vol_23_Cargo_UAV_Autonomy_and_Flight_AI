**Volume 23. Cargo UAV Autonomy and Flight AI**

# Chapter 10. 2.5t UAV SW Architecture

## 10.01. 2.5t Cargo UAV System Overview and Config

![](images/image1.png){width="7.268055555555556in" height="7.268055555555556in"}

2.5톤 화물 무인항공기(Cargo UAV)는 기존의 소형 무인항공기(Unmanned Aircraft)에서 상당한 화물을 지역 간 거리로 운송할 수 있는 자율 항공 물류 플랫폼(Autonomous Aerial Logistics Platform)으로 전환되는 단계를 의미한다. 시스템 아키텍처(System Architecture)는 결정론적 제어 동작(Deterministic Control Behavior)을 유지하면서 비행 필수 항공전자장비(Flight-Critical Avionics), 추진(Propulsion), 항법(Navigation), 화물 처리(Cargo Handling), 통신(Communication), 임무 자율성(Mission Autonomy)을 통합해야 한다. 따라서 소프트웨어 아키텍처(Software Architecture)는 항공기를 하나의 비행제어기(Flight Controller)가 아니라 분산 사이버 물리 시스템(Distributed Cyber-Physical System)으로 취급한다.

항공기 구성(Aircraft Configuration)은 일반적으로 요구되는 탑재중량(Payload Mass), 최대이륙중량(Maximum Takeoff Weight), 운항거리(Operational Range), 순항속도(Cruise Speed), 제자리비행(Hover) 또는 수직비행(Vertical Flight) 능력, 그리고 목표 물류 환경(Logistics Environment)을 중심으로 결정된다. 임무 요구조건에 따라 플랫폼은 멀티로터(Multirotor), 리프트 플러스 크루즈(Lift-Plus-Cruise), 틸트로터(Tilt-Rotor), 틸트윙(Tilt-Wing) 또는 기타 수직이착륙(VTOL) 구성을 사용할 수 있다. 이러한 선택은 구동기(Actuator)의 수, 공력 천이 로직(Aerodynamic Transition Logic), 추진 관리(Propulsion Management), 에너지 소비(Energy Consumption), 이중화 요구조건(Redundancy Requirements), 비행제어 소프트웨어(Flight-Control Software)의 복잡도에 직접적인 영향을 미친다.

시스템 수준(System Level)에서 무인항공기(UAV)는 비행 필수 제어(Flight-Critical Control), 임무 자율성(Mission Autonomy), 인지 및 항법(Perception and Navigation), 추진 및 에너지 관리(Propulsion and Energy Management), 통신(Communication), 화물 관리(Cargo Management), 기체 상태관리(Vehicle Health) 영역으로 구분할 수 있다. 이러한 영역은 제한 없이 내부 상태를 공유하는 대신 명확하게 정의된 인터페이스(Interface)를 통해 정보를 교환한다. 이러한 분리는 고장 전파(Fault Propagation)를 제한하고 전체 항공기 소프트웨어 스택(Software Stack)을 재설계하지 않고도 개별 소프트웨어 구성요소(Component)를 개발, 검증, 업그레이드 및 교체할 수 있도록 한다.

비행제어 시스템(Flight-Control System)은 항공기의 결정론적 핵심(Deterministic Core)을 구성한다. 비행제어 시스템은 상태 추정(State Estimation) 정보와 조종사, 자율 시스템 또는 임무 명령을 수신하여 자세(Attitude), 속도(Velocity), 궤적(Trajectory), 구동기 수준 명령(Actuator-Level Command)으로 변환한다. 내부 제어 루프(Inner Control Loop)는 수백 헤르츠(Hz)에서 동작할 수 있으며, 유도(Guidance) 및 궤적 기능은 상대적으로 낮은 주파수에서 실행된다. 제어 할당(Control Allocation)은 구동기 및 구조적 제약조건을 준수하면서 요구되는 힘과 모멘트(Force and Moment)를 로터(Rotor), 프로펠러(Propeller), 공력 조종면(Aerodynamic Surface) 또는 기타 효과기(Effector)에 분배한다.

항법 소프트웨어(Navigation Software)는 위성항법시스템(GNSS), 관성측정장치(IMU), 대기자료 센서(Air-Data Sensor), 레이더 또는 레이저 고도계(Radar or Laser Altimeter), 자기계(Magnetometer) 및 기타 사용 가능한 센서의 측정값을 결합한다. 센서 융합(Sensor Fusion)을 통해 위치(Position), 속도(Velocity), 자세(Attitude), 각속도(Angular Rate), 고도(Altitude) 및 관련 불확실성(Uncertainty)을 추정한다. 대형 화물 무인항공기의 경우 명목상 정확도(Nominal Accuracy)만큼 항법 무결성(Navigation Integrity)이 중요하다. 성능이 저하된 위치해(Position Solution)에 대해 잘못된 신뢰도를 갖는 것은 항법 성능 저하를 명시적으로 탐지하는 것보다 더 큰 운용 위험을 발생시킬 수 있기 때문이다.

임무관리 소프트웨어(Mission-Management Software)는 물류 목표(Logistics Objective)를 실행 가능한 비행 운용(Flight Operation)으로 변환한다. 임무에는 초기화(Initialization), 자동 시스템 점검(Automated System Check), 화물 검증(Cargo Verification), 이륙(Takeoff), 상승(Climb), 순항(Cruise), 웨이포인트 항법(Waypoint Navigation), 도착 절차(Arrival Sequencing), 하강(Descent), 착륙(Landing), 하역(Unloading), 복귀(Return Operation)가 포함될 수 있다. 임무 상태(Mission State)는 명확한 전환 조건(Transition Condition)과 안전 가드(Safety Guard)에 의해 감독되므로 상위 수준 자율 시스템(High-Level Autonomy)이 비행 필수 구동기에 제한 없는 명령을 직접 전달하거나 항공기 보호 메커니즘(Aircraft Protection Mechanism)을 우회할 수 없다.

화물 구성(Cargo Configuration)은 임무마다 탑재중량(Payload Mass), 무게중심(Center of Gravity), 관성(Inertia), 하중 분포(Load Distribution)가 달라질 수 있으므로 항공기 동작에 큰 영향을 미친다. 따라서 화물관리 소프트웨어(Cargo-Management Software)는 출발 전에 검증된 적재 정보(Loading Information)를 비행제어 및 임무 시스템과 교환한다. 탑재 센서(Onboard Sensor)가 지원되는 경우 항공기는 구성 변화를 추정하고 이를 승인된 운용영역(Approved Envelope)과 비교할 수 있다. 이후 검증된 기체 구성에 따라 제어 파라미터(Control Parameter), 궤적 제한(Trajectory Limit), 에너지 예비량(Energy Reserve), 착륙 제약조건(Landing Constraint)을 조정할 수 있다.

추진 및 에너지 관리(Propulsion and Energy Management)는 비행 소프트웨어(Flight Software)와 통합된다. 2.5톤 항공기는 상당한 연속 출력(Continuous Power)을 요구하며 이륙, 상승, 천이(Transition), 우회(Diversion), 착륙 과정에서 충분한 여유도(Margin)를 유지해야 하기 때문이다. 플랫폼에 따라 추진 시스템은 전기식(Electric), 하이브리드 전기식(Hybrid-Electric), 터빈 기반(Turbine-Based) 또는 여러 추진 장치에 분산된 형태가 될 수 있다. 소프트웨어는 높은 기동 요구 명령을 승인하기 전에 가용 출력(Available Power), 열 한계(Thermal Limit), 구성품 상태(Component Status), 에너지 예비량 및 추진 성능 저하 상태를 지속적으로 평가한다.

항공전자 네트워크(Avionics Network)는 비행컴퓨터(Flight Computer), 센서, 구동기 제어기(Actuator Controller), 추진 제어기(Propulsion Controller), 통신 장치, 탑재 시스템(Payload System), 상태감시 장비(Health-Monitoring Equipment)를 연결한다. 필수 트래픽(Critical Traffic)은 인지 데이터 스트림(Perception Stream)이나 유지보수 로그(Maintenance Log)와 같은 고대역폭 비필수 데이터와 분리되어야 한다. 결정론적 스케줄링(Deterministic Scheduling), 메시지 우선순위(Message Prioritization), 제한된 지연시간(Bounded Latency), 동기화된 타임스탬프(Synchronized Timestamp), 인터페이스 버전 관리(Interface Version Control)를 통해 높은 계산 및 네트워크 부하에서도 분산 소프트웨어 구성요소들이 일관된 항공기 상태 정보를 유지할 수 있다.

시간 동기화(Time Synchronization)는 항법, 인지 및 제어 데이터가 물리적으로 분산된 센서에서 생성되는 경우 특히 중요하다. 각 관측값(Observation)은 소프트웨어 프로세스가 메시지를 수신한 시간에만 의존하지 않고 추적 가능한 획득 타임스탬프(Acquisition Timestamp)를 유지해야 한다. 하드웨어 지원 타임스탬핑(Hardware-Assisted Timestamping), 동기화된 시계(Synchronized Clock), 명확하게 정의된 타이밍 계약(Timing Contract)은 빠른 이동, 진동 또는 급격한 기동 과정에서 공간 추정 오차(Spatial Estimation Error)처럼 나타날 수 있는 시간 정렬 오차(Temporal Alignment Error)를 감소시킨다.

탑재 컴퓨팅 아키텍처(Onboard Computing Architecture)는 일반적으로 안전 필수 실시간 처리(Safety-Critical Real-Time Processing)와 계산 집약적인 자율성 작업(Computationally Intensive Autonomy Workload)을 분리한다. 비행제어 컴퓨터(Flight-Control Computer)는 엄격하게 제어된 타이밍으로 결정론적 기능을 실행하고, 고성능 프로세서(High-Performance Processor)는 인지, 경로 최적화(Route Optimization), 장애물 평가(Obstacle Assessment), 기계학습 추론(Machine-Learning Inference), 고급 임무 추론(Advanced Mission Reasoning)을 수행할 수 있다. 두 계층 사이의 통제된 인터페이스는 지능형 기능이 궤적이나 목표를 제안할 수 있도록 하면서도 안전 필수 안정화 기능(Safety-Critical Stabilization Function)에 대한 무제한 권한을 획득하지 못하도록 한다.

이중화(Redundancy)는 모든 구성요소를 단순히 복제하는 방식이 아니라 개별 고장의 결과에 따라 설계된다. 필수 센서, 통신 경로, 전원공급장치(Power Supply), 컴퓨팅 채널(Computing Channel), 제어 인터페이스는 독립적인 감시 기능과 함께 이중 또는 삼중 이중화(Dual or Triple Redundancy)를 적용할 수 있다. 투표(Voting), 채널 간 비교(Cross-Channel Comparison), 하트비트 감시(Heartbeat Supervision), 타당성 검사(Plausibility Checking), 장애조치 로직(Failover Logic)은 불일치 동작을 탐지하는 데 활용된다. 또한 외형상 이중화된 채널들이 동일한 취약 자원에 의존하지 않도록 공통원인고장(Common-Cause Failure)을 고려해야 한다.

기체 상태관리(Vehicle Health Management)는 항공전자장비, 추진장치, 구동기, 센서, 전력 시스템(Power System), 통신 링크, 열관리 시스템(Thermal System), 구조 상태감시 채널(Structural Monitoring Channel)의 상태를 지속적으로 평가한다. 원시 고장 플래그(Raw Fault Flag)는 임무 의사결정에 활용할 수 있는 운용 상태(Operational Health State)로 변환된다. 모든 이상에 동일하게 대응하는 대신 시스템은 비행 지속, 임무 중단(Mission Abort), 우회, 통제된 착륙(Controlled Landing), 성능 제한 운용(Reduced-Performance Operation), 즉각적인 비상조치 중 어떤 대응이 적절한지를 판단한다.

통신 아키텍처(Communication Architecture)는 지휘통제(Command and Control), 원격측정(Telemetry), 비행대 협조(Fleet Coordination), 유지보수(Maintenance), 그리고 잠재적인 공역관리 서비스(Airspace-Management Service)를 지원한다. 무인항공기는 운항거리에 따라 가시선 무선통신(Line-of-Sight Radio), 셀룰러 네트워크(Cellular Network), 위성통신(Satellite Communication) 또는 기타 데이터 링크(Data Link)를 조합할 수 있다. 비행 안전은 지속적인 고대역폭 연결에 의존해서는 안 된다. 따라서 통신두절(Loss-of-Link) 상황에서는 대기(Holding), 경로 재설정(Rerouting), 복귀(Return), 착륙 또는 다른 통신 채널로의 전환을 수행하는 자율 절차가 사전에 정의된다.

외부 공역 통합(External Airspace Integration)을 위해 기체는 즉각적인 안전 대응에 대한 탑재 시스템의 권한을 유지하면서 지상 통제 인프라(Ground-Control Infrastructure) 및 무인항공교통관리(UTM) 서비스와 필요한 정보를 교환한다. 경로 승인(Route Authorization), 지오펜싱(Geofencing), 동적 제한구역(Dynamic Restriction), 기상 제약조건(Weather Constraint), 교통 정보(Traffic Information), 착륙장 상태(Landing-Site Status)는 임무계획에 영향을 줄 수 있다. 그러나 외부 서비스는 저수준 기체제어 기능을 직접 조작하는 대신 검증된 인터페이스를 통해 제한된 입력(Bounded Input)을 제공해야 한다.

안전 아키텍처(Safety Architecture)는 원격 운용자(Remote Operator), 자율 임무 소프트웨어, 비행제어 기능, 비상 보호 메커니즘(Emergency Protection Mechanism) 사이의 권한 경계(Authority Boundary)를 설정한다. 명령은 실행 전에 기체 상태, 비행영역(Flight Envelope), 지리공간 제약조건(Geospatial Constraint), 추진 가용성(Propulsion Availability), 운용 규칙(Operational Rule)에 대해 검증된다. 독립 감시기(Independent Monitor)는 안전하지 않은 요청을 거부하거나 사전에 정의된 복구 동작(Recovery Behavior)을 시작할 수 있다. 이러한 계층적 권한 모델(Layered Authority Model)은 하나의 소프트웨어 결함, 통신 오류 또는 잘못된 상위 수준 의사결정이 물리적 구동기로 즉시 전파되는 것을 방지한다.

소프트웨어 형상관리(Software Configuration Management)는 항공기의 운용 동작이 소프트웨어 버전, 보정 데이터(Calibration Data), 기체 파라미터(Vehicle Parameter), 지도(Map), 기계학습 모델(Machine-Learning Model), 임무 구성의 정확한 조합에 따라 결정되기 때문에 필수적이다. 따라서 배포 가능한 각 항공기 구성은 고유하게 식별되고 재현 가능해야 한다. 시동 과정의 호환성 검사(Compatibility Check)는 서로 맞지 않는 구성요소가 운용에 투입되는 것을 방지하며, 서명된 소프트웨어 패키지(Signed Software Package)와 통제된 업데이트 메커니즘(Controlled Update Mechanism)은 전체 비행대 수명주기(Fleet Lifecycle) 동안 구성 무결성(Configuration Integrity)을 보호한다.

지상 시스템(Ground System)은 임무계획(Mission Planning), 비행대 감시(Fleet Monitoring), 유지보수, 소프트웨어 배포(Software Deployment), 데이터 분석(Data Analysis), 운용 감독(Operational Supervision)을 지원함으로써 탑재 아키텍처를 보완한다. 지상통제소(Ground-Control Station)는 운용자가 대량의 원시 원격측정 데이터를 직접 해석할 필요 없이 항공기 상태와 경고를 이해할 수 있도록 구성되어야 한다. 경고, 임무 편차(Mission Deviation), 에너지 여유도, 항법 무결성, 구성품 상태를 명확하게 우선순위화하면 운용자는 정상 및 성능 저하 상황에서 개입이 필요한지를 효과적으로 판단할 수 있다.

검증(Verification)은 개별 소프트웨어 구성요소에서 시작하지만 점진적으로 전체 항공기 구성으로 확대된다. 단위시험(Unit Testing), 인터페이스 시험(Interface Testing), 소프트웨어 인더 루프 시뮬레이션(Software-in-the-Loop Simulation), 하드웨어 인더 루프 시험(Hardware-in-the-Loop Testing), 프로세서 인더 루프 평가(Processor-in-the-Loop Evaluation), 고장 주입(Fault Injection), 통합 기체 시험(Integrated Vehicle Testing)은 서로 다른 유형의 결함을 발견한다. 특히 고충실도 시뮬레이션(High-Fidelity Simulation)은 추진장치 고장, 센서 성능 저하, 통신두절, 악천후, 탑재하중 변화, 비상 상태 전환이 복합적으로 발생하는 드문 상황을 실제 비행에서 반복하지 않고 검증하는 데 유용하다.

궁극적으로 완성된 2.5톤 화물 무인항공기는 명확한 안전 제약조건(Safety Constraint) 아래에서 소프트웨어가 물리적 항공기 능력을 조정하는 통합 자율 운송 시스템(Integrated Autonomous Transportation System)으로 이해해야 한다. 견고한 아키텍처(Robust Architecture)는 결정론적 제어, 신뢰할 수 있는 상태 추정, 제한된 자율성(Bounded Autonomy), 이중화된 필수 자원, 구성 인식(Configuration Awareness), 상태감시, 보안 통신(Secure Communication), 체계적인 검증에 기반한다. 이러한 원칙은 화물 무인항공기 운용을 개별 실험 항공기 수준에서 신뢰할 수 있는 비행대 수준 물류 서비스(Fleet-Level Logistics Service)로 확장하기 위한 기반을 제공한다.

## 10.02. 2.5t FCC HW SW Architecture Specifics [w/Code]

![](images/image2.png){width="7.268055555555556in" height="7.268055555555556in"}

2.5톤 화물 무인항공기(2.5-ton Cargo UAV)의 비행제어컴퓨터(Flight Control Computer, FCC)는 정상, 성능 저하 및 비상 조건에서 안정적이고 안전한 비행을 유지하는 결정론적 컴퓨팅 핵심(Deterministic Computing Core)이다. 계산 집약적인 자율 기능을 실행할 수 있는 임무 컴퓨터(Mission Computer)와 달리, FCC는 제한된 실행시간(Bounded Execution Time), 예측 가능한 지연시간(Predictable Latency), 고장 격리(Fault Containment), 지속적인 가용성(Continuous Availability)을 우선한다. 따라서 하드웨어와 소프트웨어는 비행에 필수적인 플랫폼(Flight-Critical Platform)으로 함께 설계된다.

FCC 하드웨어 아키텍처(Hardware Architecture)는 항공기 위험 분석(Hazard Analysis), 제어 루프 타이밍(Control-Loop Timing), 센서 및 구동기 인터페이스(Sensor and Actuator Interface), 이중화 목표(Redundancy Objective), 환경 요구조건(Environmental Requirement)에 따라 결정된다. 실용적인 아키텍처는 각각 독립적인 프로세서(Processor), 메모리(Memory), 전원 조정 장치(Power Conditioning), 클록 소스(Clock Source), 감시 타이머(Watchdog), 통신 인터페이스를 포함하는 이중 또는 삼중 처리 채널(Dual or Triplex Processing Lane)을 사용할 수 있다. 물리적 및 전기적 분리(Physical and Electrical Separation)는 하나의 전원, 열 또는 인터페이스 고장으로 인해 여러 제어 채널이 동시에 비활성화될 가능성을 줄인다.

처리 장치(Processing Device)는 단순히 최대 계산 처리량(Maximum Computational Throughput)을 높이는 것이 아니라 충분한 결정론적 성능(Deterministic Performance)을 제공해야 한다. 실시간 CPU(Real-Time CPU), 안전 지향 마이크로컨트롤러(Safety-Oriented Microcontroller), 자원 분할(Resource Partitioning)이 가능한 멀티코어 프로세서(Multicore Processor), 또는 CPU와 FPGA 로직의 조합을 사용할 수 있다. 하드웨어 가속(Hardware Acceleration)은 결정론적인 입출력(I/O), 타임스탬핑(Timestamping), 신호 전처리(Signal Conditioning), 제어 관련 전처리(Control-Related Preprocessing)를 담당할 수 있으며, 비행제어 알고리즘은 최악 실행시간(Worst-Case Timing)을 분석하고 검증할 수 있는 실행 환경 내부에서 유지된다.

메모리 아키텍처(Memory Architecture)는 실행 코드(Executable Code), 보정 파라미터(Calibration Parameter), 실행 상태(Runtime State), 통신 버퍼(Communication Buffer), 유지보수 정보(Maintenance Information)를 중요도에 따라 분리한다. 오류정정코드 메모리(Error-Correcting Code Memory), 메모리 보호(Memory Protection), 록스텝 메커니즘(Lockstep Mechanism), 보호된 비휘발성 저장장치(Protected Nonvolatile Storage)를 사용하여 데이터 손상을 탐지하거나 허용할 수 있다. 부트 이미지(Boot Image)와 구성 데이터(Configuration Data)는 무결성 검증(Integrity Verification)이 필요하며, 이를 통해 FCC가 손상된 소프트웨어, 호환되지 않는 파라미터 또는 승인되지 않은 실행 이미지로 운용 모드에 진입하는 것을 방지한다.

FCC 전원 아키텍처(Power Architecture)는 일반적으로 독립적인 조정 전원(Regulated Supply), 과도전압 보호(Transient Protection), 저전압 및 과전압 감시(Undervoltage and Overvoltage Monitoring), 제어된 리셋 동작(Controlled Reset Behavior)을 포함한다. 이중화된 FCC 채널을 사용하는 경우 전원 독립성(Power Independence)은 가능한 한 상위 전원 계층까지 확장되어야 한다. 소프트웨어는 전원 정상 신호(Power-Good Signal)와 리셋 원인(Reset Cause)을 지속적으로 감시하여 정상적인 시작과 저전압 상태(Brownout), 감시 타이머 리셋(Watchdog Reset), 비정상적인 전원 중단(Abnormal Power Interruption)을 구분하고 적절한 복구 상태(Recovery State)를 선택할 수 있어야 한다.

센서 인터페이스(Sensor Interface)는 FCC를 관성측정장치(Inertial Measurement Unit, IMU), 위성항법 수신기(GNSS Receiver), 대기자료 시스템(Air-Data System), 고도계(Altimeter), 자기계(Magnetometer), 구동기 피드백 장치(Actuator Feedback Device), 추진 센서(Propulsion Sensor)와 연결한다. 인터페이스에는 결정론적 직렬 버스(Deterministic Serial Bus), CAN 기반 네트워크(CAN-Based Network), 이더넷(Ethernet), 개별 신호(Discrete Signal), 또는 항공기 전용 데이터 버스(Aircraft-Specific Data Bus)가 사용될 수 있다. 각 입력은 안전 필수 상태 추정(Safety-Critical State Estimation) 또는 제어 계산(Control Calculation)에 입력되기 전에 범위(Range), 최신성(Freshness), 타이밍(Timing), 순서(Sequence), 무결성(Integrity), 개연성(Plausibility)에 대해 검증된다.

구동기 인터페이스(Actuator Interface)는 지연되거나 불일치하는 명령이 대형 항공기를 불안정하게 만들 수 있기 때문에 특히 엄격한 타이밍을 요구한다. FCC는 항공기 구성에 따라 추진장치(Propulsion Unit), 조종면(Control Surface), 로터 메커니즘(Rotor Mechanism), 브레이크 또는 기타 효과기(Effector)에 대한 명령을 생성한다. 명령 메시지에는 유효성 정보(Validity Information)가 포함되며 로컬 구동기 제어기(Local Actuator Controller)가 이를 감시할 수 있다. 피드백은 실제 위치(Position), 속도(Speed), 토크(Torque), 전류(Current), 온도(Temperature), 고장 상태(Fault Condition)를 보고하여 감독 루프(Supervisory Loop)를 완성한다.

시간 동기화(Time Synchronization)는 이중화된 컴퓨터와 분산 센서 사이에 공통 시간 기준(Common Temporal Reference)을 제공한다. FCC는 센서 측정값의 의미를 메시지 수신 시간에만 의존하지 않고 센서 데이터의 획득 시간(Acquisition Time)을 보존해야 한다. 하드웨어 타임스탬프(Hardware Timestamp), 동기화된 클록(Synchronized Clock), 초당 펄스(Pulse-Per-Second, PPS) 기준, PTP 또는 gPTP 메커니즘, 결정론적 트리거 신호(Deterministic Trigger Signal)를 적절하게 사용하여 센싱(Sensing), 상태 추정(State Estimation), 제어 계산(Control Computation), 구동(Actuation) 사이의 시간적 일관성을 유지할 수 있다.

FCC 소프트웨어 스택(Software Stack)은 일반적으로 하드웨어 종속 기능을 항공기 제어 애플리케이션으로부터 격리하도록 계층화된다. 보드 지원 패키지(Board Support Package, BSP)와 장치 드라이버(Device Driver)는 프로세서, 타이머, 버스, 인터럽트, 메모리 및 저수준 입출력(Low-Level I/O)을 관리한다. 그 상위 계층에서는 실시간 운영 환경(Real-Time Operating Environment)이 스케줄링(Scheduling), 프로세스 간 통신(Inter-Process Communication), 상태 감시(Health Monitoring), 자원 보호(Resource Protection), 타이밍 서비스를 제공한다. 이후 비행 애플리케이션(Flight Application)은 안정적이고 명시적으로 제어되는 인터페이스를 통해 실행된다.

실시간 스케줄링(Real-Time Scheduling)은 비행 기능에 실행 주기(Execution Period), 우선순위(Priority), 마감시간(Deadline), 계산 예산(Computational Budget)을 할당한다. 고주기 관성 데이터 수집(High-Rate Inertial Acquisition)과 안정화 루프(Stabilization Loop)는 항법(Navigation), 유도(Guidance), 상태관리(Health Management), 통신(Communication) 작업보다 훨씬 빠른 주기로 실행될 수 있다. 아키텍처는 낮은 중요도의 작업이 비행제어 실행을 지연시키지 않도록 해야 한다. 주기 그룹(Rate Group)과 분할 스케줄링(Partitioned Scheduling)은 센서 수집, 추정, 제어법칙 실행, 명령 생성, 출력 전송 사이의 예측 가능한 관계를 제공한다.

상태 추정(State Estimation)은 서로 다른 센서 측정값을 항공기 운동에 대한 일관된 표현으로 변환한다. FCC는 자세 및 방위각 추정(Attitude and Heading Estimation), 위치 및 속도 융합(Position and Velocity Fusion), 대기자료 처리(Air-Data Processing), 고도 추정(Altitude Estimation), 센서 바이어스 보상(Sensor-Bias Compensation)을 실행할 수 있다. 이중화된 측정값은 융합 전 또는 융합 과정에서 비교되며, 혁신 검사(Innovation Test)와 일관성 검증(Consistency Check)은 더 이상 예상되는 항공기 동역학이나 독립 센싱 채널과 일치하지 않는 측정값을 식별한다.

비행제어 법칙(Flight-Control Law)은 원하는 항공기 운동을 명령된 힘과 모멘트(Commanded Force and Moment)로 변환한다. 항공기 구성에 따라 소프트웨어에는 자세 안정화(Attitude Stabilization), 각속도 제어(Angular-Rate Control), 수직 제어(Vertical Control), 속도 제어(Velocity Control), 위치 제어(Position Control), 천이 제어(Transition Control), 고정익 또는 회전익 전용 기능이 포함될 수 있다. 게인 스케줄링(Gain Scheduling)과 구성별 파라미터(Configuration-Dependent Parameter)를 통해 속도, 탑재하중, 무게중심, 추진 상태, 공력 운용 영역의 변화에 대응할 수 있다.

제어 할당(Control Allocation)은 요구되는 힘과 모멘트를 개별 효과기 명령으로 변환하면서 구동기의 한계를 준수한다. 분산 추진 화물 무인항공기(Distributed-Propulsion Cargo UAV)의 경우 여러 모터, 프로펠러, 공력 조종면 및 틸팅 메커니즘(Tilting Mechanism)을 동시에 관리해야 할 수 있다. 할당 알고리즘은 포화(Saturation), 변화율 제한(Rate Limit), 고장난 효과기(Failed Effector), 가용 추진 권한(Available Propulsion Authority), 구성 제약조건(Configuration Constraint)을 고려하여 현재 항공기의 능력과 동적으로 일관된 명령을 생성한다.

이중화된 FCC 채널은 계산된 상태와 명령이 서로 일관된 상태를 유지하는지를 판단해야 한다. 채널 간 데이터 링크(Cross-Channel Data Link)는 센서 유효성, 추정 상태, 모드 상태, 상태 정보, 제어 출력을 비교할 수 있도록 한다. 아키텍처에 따라 투표(Voting), 감시된 명령 선택(Monitored Command Selection), 주-감시 구조(Master-Monitor Arrangement), 분산 합의 메커니즘(Distributed Consensus Mechanism)을 통해 채널 간 일치 여부를 결정할 수 있다. 정의된 임계값을 넘어서는 불일치가 발생하면 통제되지 않은 경쟁 상태를 허용하지 않고 해당 채널을 격리하거나 시스템을 재구성한다.

고장 탐지, 격리 및 복구(Fault Detection, Isolation, and Recovery)는 하나의 진단 작업으로 구현되는 것이 아니라 FCC 전체에 통합된다. 감시 타이머(Watchdog)는 실행 정지를 탐지하고, 마감시간 감시기(Deadline Monitor)는 타이밍 위반을 식별하며, 통신 감시기(Communication Monitor)는 오래된 데이터(Stale Data)를 탐지하고, 수치 검증(Numerical Check)은 유효하지 않은 계산을 식별한다. 센서 및 구동기 감시기는 물리적 개연성(Physical Plausibility)을 평가한다. 고장이 확인되면 복구 로직(Recovery Logic)은 영향을 받은 자원을 격리하고 검증된 성능 저하 구성(Verified Degraded Configuration)으로 전환한다.

모드 관리(Mode Management)는 시동, 초기화, 지상 운용, 이륙, 상승, 순항, 천이, 접근, 착륙 및 비상 상태를 조정한다. 각 전환은 항공기 상태와 하위 시스템 준비 상태에서 도출된 명시적인 보호 조건(Guard Condition)에 의해 보호된다. 임무 소프트웨어는 모드 변경을 요청할 수 있지만 FCC는 안전 조건과 충돌하는 명령을 거부할 권한을 유지한다. 이를 통해 상위 수준 자율 시스템이나 지상 명령이 항공기를 유효하지 않은 동역학적 구성으로 강제로 전환하는 것을 방지한다.

FCC와 임무 또는 자율 컴퓨터 사이의 인터페이스는 의도적으로 제한된다. 상위 수준 컴퓨터는 일반적으로 직접적인 구동기 명령이 아니라 궤적 기준(Trajectory Reference), 속도 명령(Velocity Command), 웨이포인트(Waypoint), 또는 제한된 기동 요청(Bounded Maneuver Request)을 제공한다. FCC는 요청을 수락하기 전에 비행영역 한계(Flight-Envelope Limit), 항법 무결성, 추진 능력, 지오펜싱 제약조건(Geofencing Constraint), 현재 운용 모드에 대해 검증한다. 따라서 자율 컴퓨터의 연결이 끊기거나 재시작되더라도 기본적인 항공기 안정화 기능은 유지된다.

FCC 소프트웨어는 또한 명령이 검증된 항공기 한계를 초과하지 않도록 비행영역 보호(Envelope Protection)를 포함한다. 항공기 구성에 따라 보호 대상 변수에는 자세, 각속도, 대기속도(Airspeed), 하중배수(Load Factor), 수직속도(Vertical Speed), 로터 회전속도(Rotor Speed), 모터 전류, 구조 하중(Structural Load), 가용 제어 여유도(Available Control Margin)가 포함될 수 있다. 보호 로직은 제어법칙과 조정되어야 하며, 2차적인 불안정성이나 원하지 않는 과도응답(Transient Response)을 발생시킬 수 있는 방식으로 명령을 갑작스럽게 제한해서는 안 된다.

시동 소프트웨어(Startup Software)는 비행 기능을 활성화하기 전에 알려진 신뢰 상태(Known Trusted State)를 설정한다. 부트 시 시험(Boot-Time Test)은 프로세서 동작, 메모리 무결성, 클록, 통신 인터페이스, 저장된 구성, 소프트웨어 식별정보, 필수 입출력을 검증한다. 내장 시험(Built-In Test)은 비행 안전과 호환되는 주기로 운용 중에도 계속 수행된다. 초기화 과정에서 발견된 고장은 무장(Arming) 또는 추진 활성화(Propulsion Activation)를 방지하며, 비행 중 발생한 고장은 사전에 정의된 재구성 및 복구 전략에 따라 처리된다.

FCC 내부의 데이터 로깅(Data Logging)은 실시간 제어에 영향을 주지 않으면서 검증, 유지보수, 사고 재구성(Incident Reconstruction), 비행대 신뢰성 분석(Fleet Reliability Analysis)을 지원한다. 선택된 센서 값, 추정 상태, 명령, 모드 전환, 타이밍 통계, 고장, 구성 식별정보를 동기화된 타임스탬프와 함께 기록한다. 로깅 대역폭과 저장장치 활동은 제한되어야 하며, 진단 기능이 안전 필수 계산이나 통신에 필요한 자원을 소비하지 않도록 해야 한다.

소프트웨어 구성(Software Configuration)은 특정 항공기 하드웨어 구성과 밀접하게 연결된다. 각 FCC 소프트웨어는 호환 가능한 프로세서 개정판(Processor Revision), 인터페이스 정의(Interface Definition), 제어법칙 파라미터, 센서 보정값, 구동기 구성, 항공기 형상을 식별해야 한다. 구성 관리는 하나의 추진 또는 공력 구성에서 검증된 실행 파일이 다른 항공기에 실수로 설치되는 것을 방지한다. 보안 부트(Secure Boot)와 인증된 업데이트(Authenticated Update)는 추가적으로 운용 소프트웨어 기준선(Operational Software Baseline)을 보호할 수 있다.

FCC 아키텍처의 검증(Verification)은 요구사항 기반 시험(Requirements-Based Testing)과 구조, 타이밍, 견고성(Robustness), 고장 대응 평가를 결합한다. 소프트웨어 인더 루프 시험(Software-in-the-Loop Testing)은 하드웨어 통합 전에 제어 로직을 검증하며, 프로세서 인더 루프(Processor-in-the-Loop) 및 하드웨어 인더 루프(Hardware-in-the-Loop) 환경은 실제 실행 타이밍과 전기적 인터페이스를 검증할 수 있도록 한다. 고장 주입(Fault Injection)은 실제 항공기를 위험에 노출하지 않고 센서 손실, 프로세서 리셋, 네트워크 지연, 구동기 고장, 메모리 오류, 이중화 채널 간 불일치 등을 평가한다.

완성된 FCC 하드웨어-소프트웨어 아키텍처(Hardware-Software Architecture)는 주변 시스템의 신뢰성이 저하되더라도 예측 가능한 제어 성능을 제공해야 한다. 그 효과성은 정상 비행 성능뿐만 아니라 고장을 탐지하고, 시간적 무결성을 유지하며, 고장을 격리하고, 안전하게 재구성하며, 충분한 제어 권한을 유지하는 능력으로 평가된다. 2.5톤 화물 무인항공기에서 이러한 결정론적이며 독립적으로 보호되는 FCC 기반은 항공기의 기본적인 안전성을 훼손하지 않으면서 고급 자율 기능을 추가할 수 있는 기반을 제공한다.

## 10.03. 2.5t Propulsion Motor ESC Control SW [w/Code]

![](images/image3.png){width="7.268055555555556in" height="7.268055555555556in"}

2.5톤 화물 무인항공기(Cargo UAV)의 추진 제어 소프트웨어(Propulsion Control Software)는 비행제어 명령(Flight-Control Command)과 실제 추력(Thrust) 생성 사이를 연결하는 실시간 인터페이스(Real-Time Interface)를 구성한다. 추진장치는 대형 항공기의 중량을 지지하면서 높은 출력 수준에서 지속적으로 운용될 수 있으므로, 모터(Motor) 및 전자식 속도제어기(Electronic Speed Controller, ESC) 소프트웨어는 결정론적 응답(Deterministic Response), 정확한 토크 또는 속도 제어, 고장 격리(Fault Containment), 열 보호(Thermal Protection), 다중 추진 채널 간 협조 동작(Coordinated Behavior)을 제공해야 한다.

추진 아키텍처(Propulsion Architecture)는 항공기가 분산 전기 추진(Distributed Electric Propulsion), 하이브리드 전기 추진(Hybrid-Electric Propulsion), 또는 별도의 순항 추진장치와 전기 구동식 양력 장치(Electrically Actuated Lift Unit)를 결합하는지에 따라 달라진다. 각각의 모터-ESC 채널(Motor-ESC Channel)은 독립적으로 감시되는 추진 자원(Propulsion Resource)으로 취급된다. 비행제어컴퓨터(Flight Control Computer, FCC)는 추력, 토크, 로터 회전속도 또는 정규화된 구동기 요구량(Normalized Actuator Demand)을 요청하고, 로컬 추진 제어기(Local Propulsion Controller)는 이러한 명령을 전기적으로 구현 가능한 모터 동작으로 변환한다.

계층적 제어 구조(Hierarchical Control Structure)는 항공기 수준의 힘 생성(Aircraft-Level Force Generation)과 모터 수준의 전기적 제어(Motor-Level Electrical Control)를 분리한다. FCC는 안정화(Stabilization) 및 궤적 추종(Trajectory Tracking)에 필요한 전체 힘과 모멘트(Force and Moment)를 결정하고, 제어 할당(Control Allocation)은 이를 사용 가능한 추진장치에 분배한다. 이후 각 ESC는 고주파 전류, 토크, 속도 또는 전압 제어를 수행한다. 이러한 분리는 FCC가 스위칭 수준의 모터 제어를 직접 수행하지 않으면서도 항공기 동역학(Aircraft Dynamics)에 대한 중앙집중식 제어 권한을 유지하도록 한다.

영구자석 동기모터(Permanent-Magnet Synchronous Motor) 또는 이와 유사한 고출력 전동기(High-Power Machine)의 경우 자속기준제어(Field-Oriented Control, FOC)를 통해 로터 자기 위치(Rotor Magnetic Position)에 정렬된 변환 전류 성분을 이용하여 모터 토크를 제어할 수 있다. 전류 루프(Current Loop)는 일반적으로 항공기 제어 루프보다 훨씬 빠르게 동작하므로 ESC는 전기적 외란(Electrical Disturbance)이 기체 동역학에 영향을 미치기 전에 이를 억제할 수 있다. 로터 위치는 신뢰성 요구조건에 따라 엔코더(Encoder), 리졸버(Resolver), 홀 센서(Hall Sensor) 또는 검증된 센서리스 추정(Sensorless Estimation)을 통해 획득할 수 있다.

FCC와 ESC 사이의 명령 인터페이스(Command Interface)는 결정론적이며 명시적으로 제한되어야 한다. 명령에는 요구 토크(Requested Torque), 회전속도(Rotational Speed), 추력 등가 요구량(Thrust-Equivalent Demand), 활성화 상태(Enable State), 회전 방향 정보(Direction Information), 명령 유효성 메타데이터(Command-Validity Metadata)가 포함될 수 있다. 시퀀스 카운터(Sequence Counter), 타임스탬프(Timestamp), 체크섬(Checksum), 타임아웃 감시(Timeout Supervision), 송신원 식별(Source Identification)은 중복, 손상, 지연 또는 오래된 메시지를 탐지하는 데 사용된다. 유효하지 않은 명령은 고출력 스위칭 하드웨어로 직접 전달되지 않고 거부된다.

추진 피드백(Propulsion Feedback)은 실제 제어 권한(Actual Control Authority)을 추정하는 데 필요한 정보를 FCC에 제공한다. 관련 측정값에는 로터 속도(Rotor Speed), 모터 전류(Motor Current), 직류 링크 전압(DC-Link Voltage), 전력(Electrical Power), 추정 토크(Estimated Torque), 권선 온도(Winding Temperature), 인버터 온도(Inverter Temperature), 베어링 상태(Bearing Condition), ESC 고장 상태(Fault Status)가 포함된다. 제어 시스템은 명령된 추진 응답과 실제 추진 응답을 비교하여 전기적으로는 작동하지만 충분한 추력을 생성하지 못하는 모터도 성능 저하 상태로 식별할 수 있다.

순간적인 추력 변화는 과도한 전기적, 열적, 기계적 또는 구조적 하중을 발생시킬 수 있으므로 모터 명령 형상화(Motor Command Shaping)가 필요하다. 소프트웨어는 명령이 내부 모터 제어기(Inner Motor Controller)에 도달하기 전에 변화율 제한(Rate Limit), 가속도 제한(Acceleration Limit), 토크 제한(Torque Limit), 운용영역 제약조건(Operating-Envelope Constraint)을 적용한다. 이러한 제한값은 로터 속도, 버스 전압(Bus Voltage), 온도, 배터리 상태, 항공기 모드, 구조 하중에 따라 변경될 수 있으며, 검증된 하드웨어 한계를 초과하지 않으면서 가용 추진 성능을 활용하도록 한다.

ESC 상태기계(State Machine)는 전원 차단(Power-Off), 초기화(Initialization), 자체시험(Self-Test), 사전충전(Precharge), 준비(Ready), 무장(Armed), 운전(Running), 성능 저하(Degraded), 종료(Shutdown), 고장(Fault) 상태 사이의 진행을 관리한다. 상태 전환은 단순히 명령 수신에 의존하지 않고 명확한 조건을 요구한다. 토크 생성을 활성화하기 전에 소프트웨어는 공급전압, 게이트 드라이버(Gate Driver) 상태, 센서 유효성, 통신 무결성, 온도, 로터 상태, 구성 호환성(Configuration Compatibility)을 검증한다. 예상하지 못한 조건이 발생하면 제어된 억제(Controlled Inhibition) 또는 종료를 수행한다.

사전충전(Precharge)과 고전압 시퀀싱(High-Voltage Sequencing)은 고출력 전기 추진에서 특히 중요하다. 대용량 직류 링크 커패시터(DC-Link Capacitor)를 주 에너지원(Main Energy Source)에 갑작스럽게 연결하면 손상을 유발하는 돌입전류(Inrush Current)가 발생할 수 있다. 추진 소프트웨어는 인버터(Inverter)를 활성화하기 전에 접촉기(Contactor), 사전충전 회로(Precharge Circuit), 전압 측정값, 준비 신호(Readiness Signal)를 조정한다. 부적절한 전압 수렴(Voltage Convergence), 용착된 접촉기(Welded Contactor), 예상하지 못한 버스 전원 인가(Bus Energization)는 추진 채널이 운용 상태에 진입하기 전에 탐지되어야 한다.

열관리 소프트웨어(Thermal Management Software)는 순간적인 전기 측정값만으로 확인하기 어려운 모터와 전력전자장치(Power Electronics)의 누적 발열로부터 시스템을 보호한다. 온도 센서와 열 모델(Thermal Model)을 사용하여 권선, 자석(Magnet), 인버터, 반도체 접합부(Junction)의 온도를 추정할 수 있다. 제어기는 급격한 정지 임계값에만 의존하지 않고 열 여유도(Thermal Margin)가 감소함에 따라 가용 토크 또는 출력을 점진적으로 디레이팅(Derating)하여 추진 능력이 심각하게 제한되기 전에 항공기 수준 제어기가 대응할 수 있도록 한다.

에너지 및 전력 관리(Energy and Power Management)는 배터리, 발전기(Generator), 변환기(Converter) 또는 직류 배전 버스(DC Distribution Bus)를 공유하는 여러 ESC를 조정해야 한다. 여러 추진장치가 동시에 최대 명령을 수행하면 각 모터가 개별 정격 범위 내에 있더라도 전체 가용 전원 출력을 초과할 수 있다. 따라서 추진 전력 감독기(Propulsion Power Supervisor)는 가용 출력 제한(Available Power Limit)을 제어 할당 기능에 전달하여 추력 분배가 버스 전류, 전원 공급 능력, 전압 안정성 및 에너지 예비량(Energy Reserve)을 준수하도록 한다.

갑작스러운 추진 요구는 직류 버스 전압 강하(DC-Bus Sag)를 발생시킬 수 있고, 빠른 회생 동작(Regenerative Behavior)은 과전압을 유발할 수 있으므로 전압 외란(Voltage Disturbance)에 대한 협조된 대응이 필요하다. ESC 소프트웨어는 전원 상태를 감시하고 전기적 한계에 접근하면 토크 요구를 조정한다. 회생 운전(Regenerative Operation)을 지원하는 경우 회수된 에너지는 배터리 수용 능력(Battery Acceptance Capability) 및 버스 아키텍처와 호환되어야 한다. 그렇지 않으면 제동 전략(Braking Strategy)은 에너지를 흡수할 수 없는 전원으로 제어되지 않은 에너지가 역류하지 않도록 해야 한다.

이중화 추진 아키텍처(Redundant Propulsion Architecture)에서는 소프트웨어가 로컬 모터 고장(Local Motor Fault)과 시스템 수준의 제어 권한 손실(System-Level Loss of Control Authority)을 구분해야 한다. 하나의 추진 채널이 고장나더라도 나머지 효과기가 충분한 힘과 모멘트를 생성할 수 있다면 즉각적인 임무 종료가 반드시 필요한 것은 아니다. 영향을 받은 ESC는 자신의 가용 능력 상태(Capability State)를 보고하고 FCC는 남아 있는 추진장치를 이용해 제어 할당을 갱신한다. 이러한 전환은 과도한 자세 또는 고도 편차가 발생하지 않을 만큼 신속하게 수행되어야 한다.

고장 탐지(Fault Detection)는 전기적, 기계적, 계산적, 통신 및 열적 이상 상태를 포괄한다. 일반적인 감시 조건에는 과전류(Overcurrent), 과전압(Overvoltage), 저전압(Undervoltage), 과속(Overspeed), 로터 정지(Stalled Rotor), 상 불균형(Phase Imbalance), 센서 불일치(Sensor Disagreement), 과도한 온도, 게이트 드라이버 고장, 통신 타임아웃(Communication Timeout), 프로세서 감시 타이머 이벤트(Processor Watchdog Event), 명령 추종 오차(Command-Tracking Error)가 포함된다. 고장 로직은 일시적인 측정 잡음으로 정상 추진장치가 불필요하게 비활성화되지 않도록 필요한 경우 지속성(Persistence) 및 개연성 기준(Plausibility Criteria)을 사용해야 한다.

고장 격리(Fault Isolation)는 잘못된 모터를 정지시키면 이미 성능이 저하된 항공기 상태를 더욱 악화시킬 수 있기 때문에 매우 중요하다. 명령 토크, 측정된 상전류(Phase Current), 로터 가속도, 진동(Vibration), 추정 추력을 상호 비교하면 전기 제어기 고장과 프로펠러 손상 또는 기계적 전달계 고장을 구분하는 데 도움이 된다. 진단된 각 상태는 경고(Warning), 디레이팅, 제어된 종료(Controlled Shutdown), 즉각적인 격리(Immediate Isolation), 제한 조건에서의 지속 운전과 같은 정의된 대응으로 연결된다.

멀티로터(Multirotor) 또는 분산 양력(Distributed-Lift) 구성에서는 추진 동기화(Propulsion Synchronization)가 진동, 구조 하중, 음향 특성(Acoustic Behavior), 센서 품질에 영향을 줄 수 있다. 소프트웨어는 로터 속도 운용 영역을 조정하거나 알려진 구조 공진(Structural Resonance) 부근에서 지속적으로 가진되는 것을 방지할 수 있다. 이러한 조정은 비행제어 요구조건보다 우선할 수 없으며, 목표가 충돌할 경우 자세 및 추력 제어 권한을 유지하는 것이 음향 또는 진동 최적화보다 우선한다.

추진 소프트웨어는 운용 영역 간 천이(Transition Between Operating Regimes)도 관리해야 한다. 리프트 플러스 크루즈(Lift-Plus-Cruise) 또는 틸트 기반(Tilt-Based) 화물 무인항공기는 이륙, 전환, 순항, 접근 및 착륙 과정에서 추진 요구량을 크게 재분배할 수 있다. 제자리비행(Hover) 중 주요 양력을 제공하던 모터는 순항 중 부하가 감소하거나 정지할 수 있으며, 순항 추진장치가 더 큰 역할을 담당하게 된다. 상태 의존적 시퀀싱(State-Dependent Sequencing)은 갑작스러운 토크 재분배를 방지하고 천이 전 과정에서 충분한 제어 권한을 보장한다.

FCC와 추진 제어기 사이의 통신은 빠른 명령 및 피드백 트래픽(Fast Command and Feedback Traffic)과 상대적으로 느린 진단 및 유지보수 정보(Slow Diagnostic and Maintenance Information)를 분리해야 한다. 고속 경로(Fast Path)는 시간에 민감한 명령, 측정 응답, 유효성 정보, 고장 상태를 제한된 지연시간으로 전달한다. 저속 경로(Slow Path)는 상세 온도, 수명 카운터(Lifetime Counter), 이벤트 기록(Event Record), 보정 정보, 유지보수 진단 데이터를 전달한다. 이러한 분리는 대용량 진단 데이터 전송이 비행 필수 추진 통신을 방해하는 것을 방지한다.

추진 구성 데이터(Propulsion Configuration Data)에는 모터 전기 파라미터, 극수(Pole Count), 전류 제한, 속도 제한, 열 임계값(Thermal Threshold), 센서 보정값, 인버터 특성, 프로펠러 또는 로터 특성, 항공기별 운용 제한이 포함된다. 초기화 과정에서 구성 식별정보(Configuration Identity)를 실제 추진장치와 비교하여 확인해야 한다. 잘못된 파라미터 세트(Parameter Set)는 실행 소프트웨어 자체가 정상적으로 작동하더라도 안전하지 않은 토크 추정이나 보호 동작을 발생시킬 수 있다.

내장시험 기능(Built-In Test Function)은 비행 전에 추진 시스템의 준비 상태를 검증하고 운용 중에도 필수 기능을 감시한다. 시동 시험(Startup Test)은 메모리, 프로세서 상태, 전류 센서, 전압 센서, 온도 채널, 로터 위치 센싱, 통신, 게이트 드라이버, 접촉기 피드백을 점검할 수 있다. 연속 시험(Continuous Test)은 실시간 실행 예산(Real-Time Execution Budget)을 준수하면서 진행 중인 고장을 탐지한다. 유지보수 목적의 시험은 지상에서 추진 시스템이 안전하게 비활성화된 경우에만 추가적인 진단 기능을 수행할 수 있다.

추진 데이터 로깅(Propulsion Data Logging)은 명령, 측정 속도, 전류, 전압, 온도, 출력, 고장 전환, 디레이팅 이벤트, 제어기 타이밍을 동기화된 타임스탬프와 함께 기록한다. 이러한 기록은 비행 후 분석(Post-Flight Analysis), 예측 유지보수(Predictive Maintenance), 과도 이벤트(Transient Event) 분석을 지원한다. 화물 무인항공기 비행대에서는 누적된 추진 이력을 분석하여 즉각적인 탑재 고장을 발생시키지 않는 점진적인 효율 저하, 반복적인 열 스트레스, 베어링 열화 또는 ESC 이상 동작을 발견할 수 있다.

소프트웨어 검증(Software Verification)은 모터 및 인버터 모델에서 시작하여 제어기 시뮬레이션(Controller Simulation), 프로세서 시험(Processor Testing), 전력 하드웨어 시험대(Power Hardware Bench), 동력계(Dynamometer), 하드웨어 인더 루프(Hardware-in-the-Loop, HIL) 환경, 통합 추진 시험장치(Integrated Propulsion Rig), 실제 항공기 시험으로 발전한다. 고장 주입(Fault Injection)은 센서 고장, 통신 손실, 버스 전압 외란, 열 한계, 로터 구속(Rotor Blockage), 개별 추진 채널 고장을 재현해야 한다. 시험에서는 로컬 ESC 보호 기능과 그에 따른 항공기 수준 재구성 동작을 모두 검증해야 한다.

완성된 모터 및 ESC 제어 소프트웨어는 전체 운용영역(Operational Envelope)에 걸쳐 비행제어 시스템이 예측할 수 있는 추진 동작을 제공해야 한다. 정확한 명령 추종(Command Tracking)도 중요하지만 안전한 제한(Safe Limitation), 명확한 가용 능력 보고(Capability Reporting), 결정론적 고장 대응(Deterministic Fault Response), 점진적 성능 저하(Graceful Degradation) 역시 중요하다. 따라서 2.5톤 화물 무인항공기에서 추진 소프트웨어는 단순한 모터 드라이버(Motor Driver)가 아니라 전력(Electrical Power), 물리적 추력(Physical Thrust), 기체 수준 비행 제어 권한(Vehicle-Level Flight Authority)을 연결하는 분산형 안전 필수 제어 계층(Distributed Safety-Critical Control Layer)으로 기능한다.

## 10.04. 2.5t Flight Envelope and Control Law [w/Code]

![](images/image4.png){width="7.268055555555556in" height="7.268055555555556in"}

2.5톤 화물 무인항공기(Cargo UAV)의 비행영역(Flight Envelope)은 제어된 비행을 유지할 수 있도록 검증된 속도, 고도, 자세, 하중배수(Load Factor), 탑재하중 상태(Payload Condition), 무게중심(Center of Gravity), 추진 능력(Propulsion Capability), 환경 상태(Environmental State)의 조합을 정의한다. 제어법칙 소프트웨어(Control-Law Software)는 이륙, 천이(Transition), 순항, 접근, 착륙 및 비상 운용에 필요한 충분한 기동성을 제공하면서 항공기가 이러한 경계 내부에서 운용되도록 유지해야 한다.

소형 무인항공기와 달리 대형 화물 플랫폼(Heavy Cargo Platform)은 기동 과정에서 상당한 관성, 공력, 구조 및 추진 하중을 받는다. 따라서 허용 가능한 운용영역(Permissible Operating Region)은 하나의 속도 또는 자세 제한만으로 표현할 수 없다. 비행영역은 다차원적(Multidimensional)이며 항공기 구성, 탑재중량(Payload Mass), 무게중심 위치, 에너지 상태(Energy State), 대기 밀도(Atmospheric Density), 구동기 가용성(Actuator Availability), 추진 성능 저하(Propulsion Degradation)에 따라 변화한다.

비행영역 데이터(Flight-Envelope Data)는 공력 해석(Aerodynamic Analysis), 구조 한계(Structural Limit), 추진 성능(Propulsion Performance), 시뮬레이션, 지상시험(Ground Testing), 단계적 비행시험(Progressive Flight Testing)을 통해 도출된다. 각각의 검증된 구성은 대기속도(Airspeed), 받음각(Angle of Attack), 옆미끄럼각(Sideslip), 뱅크각(Bank Angle), 피치 자세(Pitch Attitude), 수직속도(Vertical Speed), 로터 속도(Rotor Speed), 모터 토크(Motor Torque), 하중배수, 조종면 변위(Control-Surface Deflection) 등의 변수에 대한 경계를 설정한다. 소프트웨어는 이러한 공학적 한계를 실제 적용 가능한 운용 제약조건(Operational Constraint)으로 변환한다.

제어법칙 아키텍처(Control-Law Architecture)는 일반적으로 계층적 구조(Hierarchical Structure)로 구성된다. 내부 루프(Inner Loop)는 각속도(Angular Rate)와 자세(Attitude)를 제어하고, 중간 루프(Intermediate Loop)는 속도와 비행경로 변수(Flight-Path Variable)를 제어하며, 외부 루프(Outer Loop)는 위치 또는 궤적 기준(Trajectory Reference)을 추종한다. 이러한 계층 구조는 빠른 안정화(Stabilization) 기능과 상대적으로 느린 유도 목표(Guidance Objective)를 분리한다. 각 계층은 상위 계층에서 제한된 명령(Bounded Command)을 받아 동역학 및 구동기 제약조건을 준수하면서 하위 계층에 기준값을 제공한다.

각속도 제어(Angular-Rate Control)는 가장 빠른 항공기 수준 피드백 루프(Aircraft-Level Feedback Loop) 중 하나를 구성한다. 자이로스코프(Gyroscope) 측정값을 명령된 동체 각속도(Commanded Body Rate)와 비교하고, 그 결과 발생한 오차를 필요한 롤(Roll), 피치(Pitch), 요(Yaw) 모멘트로 변환한다. 제어기 게인(Controller Gain)은 유연 구조 모드(Flexible Structural Mode), 추진 동역학(Propulsion Dynamics), 센서 잡음(Sensor Noise)을 가진하지 않으면서 충분한 외란 제거(Disturbance Rejection) 성능을 제공해야 한다. 따라서 기체 크기가 증가할수록 필터링(Filtering)과 위상여유(Phase Margin) 요구조건이 더욱 중요해진다.

자세 제어(Attitude Control)는 원하는 롤, 피치 및 방위각(Heading) 동작을 각속도 명령으로 변환한다. 쿼터니언 기반(Quaternion-Based) 또는 이에 상응하는 표현 방법을 사용하면 큰 자세 변화 과정에서 발생할 수 있는 수학적 특이점(Mathematical Singularity)을 방지할 수 있다. 명령 형상화(Command Shaping)는 자세 가속도와 변화율을 제한하여 궤적 요구가 갑작스러운 구조 또는 추진 부하를 발생시키지 않도록 한다. 자세 기준값(Attitude Reference)은 대기속도, 고도, 탑재하중 및 현재 비행 모드에 따라 추가적으로 제한된다.

속도 및 위치 제어(Velocity and Position Control)는 자세 제어 계층보다 상위에서 동작하며 명령된 궤적을 추종하는 데 필요한 힘을 결정한다. 병진운동 제어기(Translational Controller)는 중력, 추정된 공력 효과(Aerodynamic Effect), 바람 및 가용 추력을 고려한다. 그 결과 생성된 힘 벡터(Force Vector)는 자세 및 추진 기준값으로 변환된다. 특히 환경 외란(Environmental Disturbance)이 가용 제어 권한(Control Authority)을 초과하는 경우 위치 추종 성능은 안정성 및 비행영역 보호(Envelope Protection)보다 우선해서는 안 된다.

수직이착륙(VTOL) 또는 분산 양력(Distributed-Lift) 구성에서 수직 제어(Vertical Control)는 전체 추력, 기체 질량, 수직속도 및 고도 추정값 사이의 정확한 조정을 요구한다. 탑재하중 변화는 제자리비행(Hover)과 상승에 필요한 추력을 크게 변화시킨다. 적응형 질량 추정(Adaptive Mass Estimation) 또는 구성별 파라미터(Configuration-Specific Parameter)는 제어 성능을 향상시킬 수 있지만, 잘못된 추정으로 인해 검증된 추진 한계를 초과하는 명령이 생성되지 않도록 적응 범위가 제한되어야 한다.

천이비행(Transition Flight)은 리프트 플러스 크루즈(Lift-Plus-Cruise), 틸트로터(Tilt-Rotor) 또는 틸트윙(Tilt-Wing) 항공기에서 제어법칙 측면에서 가장 까다로운 운용영역 중 하나이다. 대기속도가 증가하거나 감소함에 따라 제어 권한은 로터가 생성하는 힘과 공력 조종면(Aerodynamic Surface) 사이에서 이동한다. 소프트웨어는 천이 상태에 따라 게인, 구동기 우선순위(Actuator Priority), 힘 할당(Force Allocation)을 스케줄링하여 하나의 제어 메커니즘이 다른 메커니즘보다 효과적으로 변화하는 과정에서도 불연속이 발생하지 않도록 한다.

게인 스케줄링(Gain Scheduling)은 하나의 공통 제어 아키텍처가 크게 다른 동역학적 조건에서 동작할 수 있도록 한다. 제어기 게인은 대기속도, 동압(Dynamic Pressure), 로터 속도, 항공기 질량, 무게중심, 틸트각(Tilt Angle), 비행 모드에 따라 달라질 수 있다. 스케줄링 값은 검증된 모델과 시험을 기반으로 도출되며 운용점 사이의 보간(Interpolation)은 부드럽게 이루어져야 한다. 검증된 스케줄링 영역을 벗어난 통제되지 않은 외삽(Uncontrolled Extrapolation)은 허용되지 않는다.

제어 할당(Control Allocation)은 요구되는 힘과 모멘트를 사용 가능한 추진장치와 공력 효과기(Aerodynamic Effector)에 대한 명령으로 변환한다. 항공기가 다수의 모터 또는 이중화된 조종면(Redundant Surface)을 포함하는 경우 제어 할당 문제는 더욱 중요해진다. 소프트웨어는 효과도 행렬(Effectiveness Matrix), 구동기 포화(Actuator Saturation), 변화율 제한(Rate Limit), 에너지 제약조건(Energy Constraint), 고장 상태를 고려하면서 가장 높은 우선순위의 제어 목표, 특히 자세 및 수직비행 제어 권한을 유지하도록 한다.

구동기 포화는 제어기가 수학적으로 안정된 상태를 유지하면서도 항공기가 물리적으로 생성할 수 없는 힘을 요구할 수 있기 때문에 명시적으로 고려되어야 한다. 안티와인드업 로직(Anti-Windup Logic), 명령 제한(Command Limiting), 달성 가능한 명령 추정(Achievable-Command Estimation)은 포화 상태에서 제어기 내부 상태가 과도한 오차를 누적하는 것을 방지한다. 시스템은 또한 감소하는 제어 여유도(Control Margin)를 보고하여 제어 권한이 완전히 소진되기 전에 유도 또는 임무 소프트웨어가 기동 요구를 줄일 수 있도록 한다.

비행영역 보호(Envelope Protection)는 명령이 안전하지 않은 항공기 상태로 발전하기 전에 이를 감독한다. 항공기 구성에 따라 보호 기능은 과도한 뱅크, 피치, 각속도, 받음각, 옆미끄럼각, 대기속도, 하강률(Descent Rate), 로터 속도, 구조 하중 또는 추진 요구를 제한할 수 있다. 보호 기능은 가능한 경우 명령을 점진적으로 수정하여 예측 가능한 조종 특성을 유지해야 하며, 기체 자체를 불안정하게 만들 수 있는 갑작스러운 개입을 발생시켜서는 안 된다.

대기속도 보호(Airspeed Protection)는 순항 중 공력 조종면에 의존하는 항공기에서 특히 중요하다. 저속 보호(Low-Speed Protection)는 유도 명령으로 인해 항공기가 불충분한 공력 제어 권한 또는 실속(Stall) 상태에 접근하는 것을 방지하며, 고속 보호(High-Speed Protection)는 과도한 동압과 구조 하중을 방지한다. 적용되는 경계는 탑재하중, 고도, 플랩 구성(Flap Configuration), 추진 상태 및 대기 조건에 따라 변경될 수 있다.

구조 비행영역 보호(Structural-Envelope Protection)는 기체 또는 화물 구속 시스템(Cargo Restraint System)을 손상시킬 수 있는 기동 하중을 제한한다. 무거운 탑재하중은 전체 항공기 질량이 허용 범위 내에 있더라도 굽힘모멘트(Bending Moment)와 동적 응답(Dynamic Response)을 변화시킬 수 있다. 제어 소프트웨어는 검증된 적재 구성에 따라 가속도, 각속도, 뱅크각, 명령 변화율(Command Slew Rate)을 제한할 수 있다. 따라서 화물별 제한조건(Cargo-Specific Restriction)이 활성 비행제어 구성의 일부가 될 수 있다.

무게중심 변화(Center-of-Gravity Variation)는 안정성, 트림 요구량(Trim Requirement), 구동기 제어 권한에 직접적인 영향을 미친다. 비행 전에 탑재하중 정보를 이용하여 추정된 무게중심이 승인된 범위 내에 있는지 검증한다. 운용 중에는 예상된 제어 동작과 관측된 제어 동작 사이의 일관성을 분석하여 구성 오류(Configuration Error) 또는 화물 이동(Cargo Movement)을 식별할 수 있다. 가용 제어 여유도가 예상치 못하게 감소하면 항공기는 더욱 엄격한 기동 제한을 적용하거나 복구 절차(Recovery Procedure)를 시작할 수 있다.

바람 및 대기 외란(Wind and Atmospheric Disturbance)은 항공기 자체의 불안정성과 구분되어야 한다. 상태 추정 및 제어법칙은 관성 센서, 대기자료(Air Data), 위성항법시스템(GNSS) 및 기타 측정값을 사용하여 대기와 지면에 대한 기체 운동을 모두 추정한다. 돌풍 억제(Gust Rejection)는 구동기 대역폭(Actuator Bandwidth) 및 구조적 한계와 균형을 이루어야 한다. 지나치게 공격적인 외란 보상(Disturbance Compensation)은 임무 성능을 실질적으로 향상시키지 못하면서 구조 하중, 에너지 소비 및 구동기 마모를 증가시킬 수 있다.

추진 성능 저하(Propulsion Degradation)는 사용 가능한 비행영역을 동적으로 변화시킨다. 하나의 모터가 고장나거나 디레이팅(Derating)되면 최대 상승률, 요 제어 권한(Yaw Authority), 제자리비행 능력 또는 허용 뱅크각이 감소할 수 있다. 고장관리 시스템(Fault-Management System)은 가용 추진 능력을 제어 할당 및 비행영역 관리 기능에 전달한다. 항공기는 명목상 한계(Nominal Limit)를 계속 사용하는 대신 현재 생성 가능한 힘과 모멘트를 나타내는 성능 저하 비행영역(Degraded Envelope)으로 전환한다.

센서 성능 저하(Sensor Degradation)도 마찬가지로 제어법칙 동작을 변경해야 할 수 있다. 대기자료 센서의 손실, GNSS 성능 저하 또는 관성 센서 간 불일치는 기본적인 안정화가 가능하더라도 특정 제어 모드를 사용할 수 없게 만들 수 있다. 따라서 모드 로직(Mode Logic)은 각각의 비행 모드에 필요한 센서 무결성(Sensor Integrity)을 연계한다. 이러한 요구조건이 더 이상 충족되지 않으면 FCC는 적절하게 제한된 명령을 사용하는 검증된 대체 모드(Fallback Mode)로 자동 전환한다.

비상 제어법칙(Emergency Control Law)은 정상적인 성능 목표를 더 이상 달성할 수 없는 조건을 위해 설계된다. 그 목적은 정밀한 궤적 추종을 유지하는 것이 아니라 필수적인 안정성, 제어 가능성(Controllability), 생존성(Survivability)을 보존하는 것이다. 고장 유형에 따라 시스템은 사전에 정의된 안전 로직(Safety Logic)에 따라 자세 안정화, 하강 억제(Arrest of Descent), 저속 비행, 통제된 우회(Controlled Diversion), 즉각적인 착륙 또는 최소위험 종료(Minimum-Risk Termination)를 우선할 수 있다.

모드 전환(Mode Transition)은 제어기 상태와 구동기 명령에서 불연속이 발생하지 않도록 해야 한다. 적분기 초기화(Integrator Initialization), 기준값 블렌딩(Reference Blending), 무충격 전환(Bumpless Transfer), 게인 보간(Gain Interpolation), 명령 동기화(Command Synchronization)는 제자리비행, 천이, 순항, 접근, 성능 저하 또는 비상 모드 사이를 전환할 때 사용된다. 이러한 메커니즘이 없으면 논리적으로 올바른 모드 전환이라도 비행영역을 위반할 만큼 큰 갑작스러운 제어 과도현상(Control Transient)을 발생시킬 수 있다.

제어법칙 소프트웨어는 고정된 제한 임계값에만 의존하지 않고 제어 여유도를 지속적으로 계산하거나 추정한다. 잔여 추력(Remaining Thrust), 구동기 이동 여유(Actuator Travel), 토크 능력(Torque Capacity), 에너지 가용성(Energy Availability), 공력 제어 권한(Aerodynamic Authority)은 기체가 제어 가능성을 상실하는 상태에 얼마나 가까워졌는지를 나타내는 지표를 제공한다. 이러한 여유도 정보는 유도 및 임무관리 기능에 제공되어 상위 수준 계획이 거의 소진된 물리적 능력을 요구하는 궤적을 피하도록 할 수 있다.

비행영역 및 제어법칙 소프트웨어의 검증(Verification)은 해석 모델(Analytical Model)에서 시작하여 비선형 시뮬레이션(Nonlinear Simulation), 몬테카를로 분석(Monte Carlo Analysis), 소프트웨어 인더 루프(Software-in-the-Loop), 하드웨어 인더 루프(Hardware-in-the-Loop), 아이언 버드(Iron-Bird) 또는 통합 시험장치(Integrated Rig) 시험, 통제된 비행영역 확장(Controlled Flight Expansion)으로 발전한다. 질량, 무게중심, 공력 특성, 센서 오차, 바람, 지연시간 및 추진 성능의 불확실성을 체계적으로 변화시켜 안정성 또는 비행영역 준수를 어렵게 만드는 조건의 조합을 식별한다.

비행영역 확장(Flight-Envelope Expansion)은 시뮬레이션 결과만으로 안전 경계가 확립되었다고 가정하지 않고 단계적으로 수행된다. 초기 비행은 보수적인 영역(Conservative Region) 내부에서 수행되며, 측정된 항공기 응답이 모델 예측을 확인함에 따라 검증된 한계를 점진적으로 확대한다. 추가 운용영역을 승인하기 전에 기록된 제어 여유도, 구조 하중, 구동기 동작 및 추진 성능을 허용 기준(Acceptance Criteria)과 비교한다.

완성된 비행영역 및 제어법칙 아키텍처(Flight-Envelope and Control-Law Architecture)는 정상 운용에서 심각한 성능 저하 상태에 이르기까지 예측 가능한 동작을 제공해야 한다. 그 목적은 조종사 또는 자율 시스템의 명령을 정확하게 추종하는 것에 그치지 않고 어떤 명령이 물리적으로 안전하고 달성 가능한지를 판단하는 것이다. 2.5톤 화물 무인항공기에서 이러한 보호된 제어 계층(Protected Control Layer)은 증가하는 자율성(Autonomy)과 임무 복잡성(Mission Complexity)이 검증된 공력, 구조, 추진 및 제어 가능성 한계 내에서 유지되도록 보장한다.

## 10.05. 2.5t Navigation and Autonomy Stack [w/Code]

![](images/image5.png){width="7.268055555555556in" height="7.268055555555556in"}

2.5톤 화물 무인항공기(Cargo UAV)의 항법 및 자율 스택(Navigation and Autonomy Stack)은 센서 관측값(Sensor Observation), 임무 목표(Mission Objective), 공역 제약조건(Airspace Constraint), 기체 능력(Vehicle Capability)을 안전한 궤적 명령(Trajectory Command)으로 변환한다. 항공기는 상당한 운동에너지(Kinetic Energy)와 높은 가치의 화물을 운반하므로 자율성(Autonomy)을 제한 없는 의사결정 계층으로 취급해서는 안 된다. 항법 무결성(Navigation Integrity), 결정론적 안전 경계(Deterministic Safety Boundary), 고장 인지형 계획(Fault-Aware Planning), 비행제어컴퓨터(Flight Control Computer, FCC)와의 통제된 인터페이스가 핵심 아키텍처 요구조건이다.

항법 아키텍처(Navigation Architecture)는 관성, 위성항법, 대기자료, 고도 및 환경 측정값을 동기화하여 획득하는 과정에서 시작한다. 대표적인 정보원에는 다중 관성측정장치(Inertial Measurement Unit, IMU), 위성항법시스템 수신기(GNSS Receiver), 자기계(Magnetometer), 기압 센서(Barometric Sensor), 레이더 또는 레이저 고도계(Radar or Laser Altimeter), 카메라(Camera), 라이다(LiDAR)가 포함된다. 각각의 관측값에는 획득 시간(Acquisition Time), 유효성(Validity), 보정 상태(Calibration State), 불확실성(Uncertainty)이 연결된 후 추정 또는 자율 처리 과정에 입력된다.

시간 정렬(Time Alignment)은 서로 다른 버스와 프로세서를 통해 도착하는 측정값이 서로 다른 물리적 시점을 나타낼 수 있기 때문에 매우 중요하다. 하드웨어 타임스탬프(Hardware Timestamp), 초당 펄스(Pulse-Per-Second, PPS) 기준, 정밀시간 프로토콜(Precision Time Protocol, PTP) 또는 일반화 정밀시간 프로토콜(gPTP) 동기화, 결정론적 트리거 메커니즘(Deterministic Trigger Mechanism)을 통해 공통 시간 기준(Common Temporal Basis)을 구축할 수 있다. 스택은 측정 데이터의 출처 및 시간 이력(Measurement Provenance)을 보존하여 지연된 카메라 프레임이나 네트워크 패킷이 최신 관성 상태와 동시에 발생한 데이터인 것처럼 잘못 융합되지 않도록 한다.

상태 추정(State Estimation)은 서로 다른 관측값을 위치(Position), 속도(Velocity), 자세(Attitude), 각속도(Angular Rate), 고도(Altitude), 센서 바이어스(Sensor Bias) 및 관련 불확실성에 대한 일관된 추정값으로 변환한다. 계산 및 보증 요구조건에 따라 확장 칼만 필터(Extended Kalman Filter), 오차상태 필터(Error-State Filter), 팩터 그래프(Factor Graph) 또는 기타 검증된 추정기(Validated Estimator)를 사용할 수 있다. 추정기는 최적 상태 추정값뿐만 아니라 해당 추정값이 현재 활성화된 비행 모드에서 사용할 수 있을 만큼 충분히 신뢰할 수 있는지도 보고해야 한다.

항법 무결성 감시(Navigation Integrity Monitoring)는 이중화되고 서로 다른 방식의 센서 사이의 일관성을 평가한다. GNSS 위치해는 관성 전파(Inertial Propagation), 대기자료 동작(Air-Data Behavior), 지형 상대 고도(Terrain-Relative Altitude), 시각 기반 운동(Visual Motion) 또는 기타 독립적인 관측값과 비교할 수 있다. 혁신 잔차(Innovation Residual), 공분산 증가(Covariance Growth), 범위 검사(Range Check), 시간적 일관성(Temporal Consistency), 채널 간 비교(Cross-Channel Comparison)를 이용하여 손상된 측정값을 식별한다. 의심스러운 정보는 비행제어 상태를 크게 왜곡하기 전에 격리된다.

GNSS 의존 운용(GNSS-Dependent Operation)은 신호 차단(Signal Blockage), 다중경로(Multipath), 전파 간섭(Interference), 스푸핑 징후(Spoofing Indicator), 보정 서비스 손실(Loss of Correction Service)을 명시적으로 처리해야 한다. GNSS를 항상 사용할 수 있다고 가정하는 대신 아키텍처는 제한된 지속시간과 운용 제한을 갖는 성능 저하 항법 모드(Degraded-Navigation Mode)를 정의한다. 관성 전파, 시각-관성 오도메트리(Visual-Inertial Odometry), 지형 상대 항법(Terrain-Relative Navigation), 레이더 측정 또는 기타 보조 정보원을 이용하여 신뢰할 수 있는 절대 위치가 복구될 때까지 제한된 항법 능력을 유지할 수 있다.

인지(Perception)는 주변 지형, 장애물, 착륙구역, 기상 관련 위험요소, 동적 교통에 관한 정보를 생성하여 항법 기능을 기체 상태 추정 이상으로 확장한다. 카메라, 라이다, 레이더(Radar), 지도 데이터(Map Data)는 고성능 자율 컴퓨터(High-Performance Autonomy Computer)에서 처리될 수 있다. 인지 결과에는 신뢰도(Confidence)와 타임스탬프 정보가 함께 표현되어 계획 소프트웨어가 확인된 위험요소와 불확실하거나 오래된 환경 관측값을 구분할 수 있도록 한다.

자율 스택(Autonomy Stack)은 비행 필수 안정화 계층(Flight-Critical Stabilization Layer)과 분리된다. 상위 수준 자율 시스템(High-Level Autonomy)은 임무 단계, 경로, 궤적, 착륙 접근 및 비상 대응을 결정하고, FCC는 자세, 속도 및 비행영역 보호(Flight-Envelope Protection)를 유지한다. 따라서 자율 컴퓨터는 제한 없는 구동기 명령이 아니라 제한된 궤적, 웨이포인트(Waypoint), 속도 또는 기동 요청(Bounded Maneuver Request)을 전달한다. 이러한 분리는 자율 소프트웨어 고장이나 프로세서 재시작이 발생했을 때 그 영향을 제한한다.

임무관리(Mission Management)는 물류 목표를 실행 가능한 일련의 운용 상태(Operational State)로 변환한다. 일반적인 임무에는 비행 전 검증(Preflight Validation), 화물 확인(Cargo Confirmation), 추진 준비 상태 확인(Propulsion Readiness), 이륙, 출발, 상승, 항로 항법(En-Route Navigation), 도착, 접근, 착륙, 화물 인계(Cargo Transfer), 복귀 또는 종료가 포함된다. 각 상태 전환은 단순히 경과시간이나 계획된 순서에 따라 진행되는 것이 아니라 명확한 기체, 항법, 공역 및 하위 시스템 조건에 따라 수행된다.

전역 경로 계획(Global Route Planning)은 지형, 통제공역(Controlled Airspace), 지오펜스(Geofence), 인구 노출도(Population Exposure), 기상, 에너지 요구량, 통신 커버리지(Communication Coverage), 비상 착륙 선택지(Emergency Landing Option), 항공기 성능을 고려하여 출발지와 목적지 사이의 실행 가능한 경로를 결정한다. 선택된 경로는 단순히 거리를 최소화하는 것이 아니라 운용 여유도(Operational Margin)를 유지해야 한다. 상황 변화로 주 경로가 유효하지 않게 될 경우 신속하게 대응할 수 있도록 대체 경로(Alternative Route)를 유지할 수 있다.

지역 궤적 계획(Local Trajectory Planning)은 전역 경로를 보다 짧은 시간 범위에서 동역학적으로 달성 가능한 움직임으로 변환한다. 항공기의 속도, 가속도, 선회반경(Turn Radius), 상승 능력, 구동기 한계, 바람, 장애물 형상, 현재 제어 여유도를 고려한다. 대형 화물 무인항공기의 경우 기하학적으로 가능한 회피 기동이라도 물리적으로 과도할 수 있으므로 계획기는 궤적을 선택할 때 기체 동역학과 구조적 한계를 함께 고려해야 한다.

궤적 생성(Trajectory Generation)은 FCC가 가속도, 저크(Jerk), 자세, 추력 또는 대기속도 제약조건을 위반하지 않고 추종할 수 있는 시간 매개변수화 기준(Time-Parameterized Reference)을 생성한다. 대형 항공기에서는 불연속적인 기준값이 큰 구조 및 추진 과도현상(Transient)을 발생시킬 수 있으므로 부드러움(Smoothness)이 특히 중요하다. 따라서 궤적 인터페이스에는 유효기간(Validity Period), 기준 타이밍(Reference Timing), 제약조건이 포함되어 FCC가 검증된 능력을 초과하는 명령을 거부하거나 재형성할 수 있도록 한다.

동적 장애물 회피(Dynamic Obstacle Avoidance)는 현재 장애물 위치에만 반응하는 것이 아니라 인지 및 교통 정보를 사용하여 잠재적인 충돌을 예측한다. 회피 동작을 평가할 때 상대 운동(Relative Motion), 불확실성, 항공기 기동성(Maneuverability), 요구 분리거리(Required Separation)를 고려한다. 늦은 시점의 급격한 회피는 2.5톤 항공기에서 원래의 충돌 위험보다 더 큰 위험을 만들 수 있으므로 시스템은 충분한 제어 여유도를 유지할 수 있을 만큼 조기에 기동을 선택한다.

착륙장 평가(Landing-Site Assessment)는 단순한 지리적 좌표 이상의 정보를 요구한다. 자율 스택은 접근 형상(Approach Geometry), 지면 상태(Surface Condition), 경사(Slope), 장애물 이격거리(Obstacle Clearance), 가용 크기, 바람, 항법 신뢰도, 그리고 필요한 경우 착륙구역 주변의 사람이나 차량을 평가한다. 하강을 결정하기 전에 센서 기반 확인 결과를 저장된 착륙장 데이터와 비교할 수 있다. 요구 기준이 충족되지 않으면 항공기는 대기(Hold), 대체 착륙장 선택 또는 접근 중단(Abort)을 수행할 수 있다.

정밀 착륙(Precision Landing)은 운용 인프라에 따라 GNSS, 관성 센싱(Inertial Sensing), 시각 랜드마크(Visual Landmark), 기준 마커(Fiducial Target), 라이다, 레이더 또는 상대항법(Relative Navigation)을 결합할 수 있다. 항공기가 지면에 접근할수록 항법 요구조건은 더욱 국부적이고 안전 필수적(Safety-Critical)이 된다. 시스템은 실제 착륙 기준과 잘못된 탐지(False Detection)를 구분해야 하며, 하강 중 정밀 유도(Precision Guidance)가 손실되는 경우 안정화, 재상승(Climb-Out), 복행(Go-Around)을 포함하는 명확한 대응 절차를 유지해야 한다.

공역 통합(Airspace Integration)은 사용 가능한 경우 경로 승인(Route Authorization), 지오펜싱(Geofencing), 교통 정보(Traffic Information), 임시 제한구역(Temporary Restriction), 무인항공교통관리(Unmanned Traffic Management, UTM) 서비스를 자율 스택에 제공한다. 외부 정보는 직접적인 비행제어 권한이 아니라 검증된 임무 입력(Validated Mission Input)으로 취급된다. 외부 서비스와의 연결이 끊기더라도 탑재 로직(Onboard Logic)은 비행 지속, 대기, 우회, 복귀 또는 안전한 착륙에 필요한 충분한 정보와 사전에 정의된 규칙을 유지한다.

기상 정보(Weather Information)는 전략적 및 전술적 자율성(Strategic and Tactical Autonomy) 모두에 영향을 미친다. 예측된 바람, 강수, 가시거리(Visibility), 온도, 착빙 위험(Icing Risk), 대류 활동(Convective Activity)은 출발 전 경로 선택에 영향을 줄 수 있으며, 탑재 관측값은 비행 중 국부적인 환경 변화를 식별할 수 있다. 계획기는 환경 조건을 항공기별 운용 한계와 비교한다. 따라서 지리적으로 유효한 경로라도 바람이나 기상으로 인해 과도한 에너지 또는 제어 여유도를 소모하는 경우 거부될 수 있다.

에너지 인지형 계획(Energy-Aware Planning)은 비상 상황을 위한 예비량을 유지하면서 현재 임무를 완료할 수 있는지를 지속적으로 평가한다. 예상 에너지 소비량은 탑재하중, 고도, 바람, 속도, 추진 효율(Propulsion Efficiency), 열 상태(Thermal State), 경로 형상에 따라 달라진다. 잔여 에너지는 목적지, 대체 착륙장, 우회 및 예비량 정책(Reserve Policy)에 필요한 에너지와 비교된다. 여유도가 감소하면 자율 시스템은 에너지가 위험 수준에 도달하기 전에 속도, 경로, 고도 또는 임무 목표를 변경한다.

기체 능력(Vehicle Capability)은 고정된 성능 사양이 아니라 동적으로 표현된다. 추진 고장, 구동기 성능 저하, 센서 손실, 열적 한계 또는 탑재하중 이상은 상승 능력, 최대속도, 항법 가용성 또는 기동 권한을 감소시킬 수 있다. 따라서 상태관리 정보(Health-Management Information)는 자율 스택에 제공되며, 자율 시스템은 명목 성능을 기준으로 계속 궤적을 요구하는 대신 성능 저하 능력 영역(Degraded Capability Envelope)을 사용하여 경로를 재계획한다.

비상상황 관리(Contingency Management)는 항법 손실, 통신 고장, 추진 성능 저하, 악천후, 착륙장 사용 불가, 교통 충돌 및 기타 비정상 조건에 대한 대응을 정의한다. 대응은 안전 목표와 사용 가능한 기체 능력에 따라 우선순위가 설정된다. 상황에 따라 무인항공기는 대기, 경로 재설정(Reroute), 복귀, 우회, 더 안전한 고도로 하강, 통제된 착륙(Controlled Landing), 또는 최소위험 비상 절차(Minimum-Risk Emergency Procedure)로 전환할 수 있다.

통신 손실(Communication Loss)은 정의되지 않은 예외 상황이 아니라 예상 가능한 운용 조건으로 처리된다. 자율 시스템은 지휘통제 링크(Command-and-Control Link)의 품질을 감시하고 오래되거나 누락된 메시지를 탐지한다. 사전에 정의된 통신두절 정책(Loss-of-Link Policy)은 항공기가 임무를 계속할지, 집결 지점(Rally Point)으로 이동할지, 복귀할지, 대기할지 또는 착륙할지를 규정한다. FCC는 지상통제소 또는 자율 인프라와의 연결 여부와 관계없이 기본적인 안정화 기능을 유지할 수 있다.

지오펜싱(Geofencing)은 계획 제약조건과 독립적인 실시간 보호 기능(Runtime Protection)으로 동시에 구현된다. 전략적 계획은 금지 또는 제한구역을 회피하며, 실시간 감시 기능은 예측된 궤적과 활성 경계 사이의 교차 가능성을 평가한다. 항공기가 지오펜스 경계에 지나치게 근접하여 운용되지 않도록 이격거리를 계산할 때 위치 불확실성(Position Uncertainty)을 포함한다. 동적 제한구역(Dynamic Restriction)은 해당 정보의 진위성(Authenticity), 적용 시간 및 적용 가능성을 검증한 후에만 반영할 수 있다.

자율 권한(Autonomy Authority)은 명시적인 명령 계약(Command Contract)을 통해 제한된다. FCC에 전달되는 모든 요청에는 정의된 단위(Unit), 기준좌표계(Reference Frame), 유효시간 구간(Validity Interval), 제한값, 명령 출처(Source Identity)가 포함된다. FCC는 명령을 수락하기 전에 비행 모드, 항법 무결성, 비행영역 한계, 명령 최신성(Command Freshness)을 독립적으로 확인한다. 자율 컴퓨터가 유효하지 않거나 서로 모순되거나 지나치게 공격적인 명령을 생성하면 비행제어 계층은 이를 거부하고 안전한 대체 상태(Safe Fallback State)를 유지할 수 있다.

자율 소프트웨어 플랫폼(Autonomy Software Platform)은 일반적으로 임무 로직, 인지, 계획, 통신, 지도 처리(Mapping), 진단 기능을 분리하기 위해 파티셔닝(Partitioning)을 사용한다. 자원 예산(Resource Budget)은 CPU, 메모리, 네트워크 및 가속기(Accelerator) 사용량을 제한하여 과부하된 하나의 인지 프로세스가 안전 관련 항법 기능의 자원을 고갈시키지 않도록 한다. 감시 타이머(Watchdog), 하트비트 감시(Heartbeat Monitoring), 프로세스 감독(Process Supervision), 제어된 재시작 메커니즘(Controlled Restart Mechanism)을 통해 전체 항공기 시스템을 재부팅하지 않고도 선택된 자율 서비스를 복구할 수 있다.

데이터 기록(Data Recording)은 항법 측정값, 추정기 상태, 불확실성, 인지 결과, 계획된 궤적, 임무 상태 전환, 명령 결정, 기체 상태 및 외부 제약조건을 동기화된 타임스탬프와 함께 저장한다. 이러한 기록은 검증, 사고 분석(Incident Analysis), 지도 개선(Map Improvement), 비행대 학습(Fleet Learning)을 지원한다. 로깅은 비행 후 분석 과정에서 특정 의사결정이 이루어진 시점에 자율 시스템이 어떤 정보를 보유하고 있었는지를 재구성할 수 있도록 구조화된다.

항법 및 자율 스택의 검증(Verification)은 알고리즘 시험에서 시작하여 시뮬레이션, 소프트웨어 인더 루프(Software-in-the-Loop), 하드웨어 인더 루프(Hardware-in-the-Loop), 기록 데이터 재생(Recorded-Data Replay), 시나리오 시험(Scenario Testing), 통합 기체 시험(Integrated Vehicle Trial), 비행시험(Flight Testing)으로 발전한다. 대규모 시나리오 라이브러리(Scenario Library)를 통해 GNSS 손실, 센서 불일치, 장애물 충돌 위험, 기상 변화, 공역 제한, 추진 성능 저하, 통신두절, 착륙장 거부 상황을 검증한다. 몬테카를로 기법(Monte Carlo Method)은 불확실성과 타이밍 변화에 대한 시스템의 민감도를 평가한다.

완성된 항법 및 자율 스택(Navigation and Autonomy Stack)은 지능적인 임무 수행(Intelligent Mission Execution)과 항공기 능력 및 비행 필수 권한 경계(Flight-Critical Authority Boundary)에 대한 엄격한 준수를 결합해야 한다. 성공 여부는 단순히 무인항공기가 목적지에 도착하는지 여부가 아니라 신뢰할 수 있는 위치 추정을 유지하고, 위험을 사전에 예측하며, 에너지와 제어 여유도를 보존하고, 고장에 적응하며, 안전하고 달성 가능한 명령만을 생성하는지에 따라 평가된다. 따라서 2.5톤 화물 무인항공기에서 자율성은 검증된 항법 무결성과 결정론적 비행 보호(Deterministic Flight Protection)를 기반으로 구축되는 감독형 의사결정 시스템(Supervised Decision System)이다.

## 10.06. 2.5t Cargo Interface and Load Management [w/Code]

![](images/image6.png){width="7.268055555555556in" height="7.268055555555556in"}

2.5톤 화물 무인항공기(Cargo UAV)의 화물 인터페이스 및 하중관리 소프트웨어(Cargo Interface and Load-Management Software)는 물류 운용(Logistics Operation)과 비행 필수 기체 구성(Flight-Critical Vehicle Configuration)을 연결한다. 화물의 질량, 위치, 구속 상태(Restraint Condition), 크기 및 이동은 안정성, 구조 하중(Structural Loading), 추진 요구량(Propulsion Demand), 착륙 성능에 직접 영향을 미칠 수 있으므로 화물을 단순한 수동적 물체로 취급하지 않는다. 따라서 소프트웨어는 비행 전에 화물 상태를 검증하고 임무 전 과정에서 관련 상태를 지속적으로 감시한다.

화물 아키텍처(Cargo Architecture)는 물리적 취급 장비(Physical Handling Equipment), 센싱(Sensing), 로컬 화물 제어기(Local Cargo Controller), 임무관리(Mission Management), 비행제어 인터페이스(Flight-Control Interface)를 분리한다. 물리적 메커니즘에는 화물 도어(Cargo Door), 잠금장치(Lock), 클램프(Clamp), 레일(Rail), 팔레트(Pallet), 윈치(Winch), 호이스트(Hoist), 자동 적재장치(Automated Loading Device)가 포함될 수 있다. 로컬 제어기는 이러한 장치를 작동시키며 상위 수준 소프트웨어는 적재 순서를 조정하고 검증된 구성 정보를 비행제어컴퓨터(Flight Control Computer, FCC) 및 임무 컴퓨터(Mission Computer)와 교환한다.

화물 구성(Cargo Configuration)은 탑재화물의 식별과 예상 특성을 정의하는 것에서 시작한다. 임무 데이터에는 화물 식별자(Cargo Identifier), 질량, 크기, 무게중심 위치(Center-of-Gravity Location), 취급 제한조건(Handling Restriction), 환경 요구조건(Environmental Requirement), 목적지가 포함될 수 있다. 이러한 값은 항공기 제한값과 비교되고 가능한 경우 독립적인 측정값과도 비교된다. 구성 소프트웨어(Configuration Software)는 탑재화물 정보가 누락되거나 일관되지 않거나 승인된 운용영역(Approved Operating Envelope)을 벗어나는 경우 임무 승인을 방지한다.

탑재화물 질량(Payload Mass)은 제자리비행 출력(Hover Power), 상승 능력(Climb Capability), 가속도, 에너지 소비, 착륙 하중(Landing Load), 비상 성능에 직접적인 영향을 미친다. 시스템은 지상 물류 데이터(Ground Logistics Data), 로드셀(Load Cell), 착륙장치 측정값(Landing-Gear Measurement), 현수 센서(Suspension Sensor) 또는 추진 기반 추정(Propulsion-Based Estimation)을 통해 질량을 획득할 수 있다. 측정값과 신고된 값은 출발 전에 상호 검증된다. 잘못된 질량 추정은 비행제어 및 에너지 계획의 전제를 무효화할 수 있으므로 상당한 불일치가 존재하면 자동 승인을 허용하지 않는다.

무게중심 관리(Center-of-Gravity Management)는 대형 화물 항공기에서 특히 중요하다. 소프트웨어는 공허중량 상태 항공기(Empty Aircraft)의 특성, 해당되는 경우 연료 또는 에너지 시스템 상태, 개별 탑재화물 위치를 이용하여 전체 무게중심을 계산한다. 계산 결과는 구성에 따른 종방향(Longitudinal), 횡방향(Lateral), 수직방향(Vertical) 한계와 비교된다. 탑재화물이 전체 질량 제한을 만족하더라도 위치 때문에 충분한 제어 또는 구조 여유도(Control or Structural Margin)를 확보할 수 없다면 운송이 거부될 수 있다.

화물 구속 감시(Cargo Restraint Monitoring)는 추진 시스템 활성화 또는 이륙 전에 화물이 기계적으로 안전하게 고정되었는지를 확인한다. 센서는 잠금장치 체결(Lock Engagement), 클램프 위치, 래치 상태(Latch Status), 구속장치 장력(Restraint Tension), 팔레트 존재 여부를 감시할 수 있다. 필수 구속장치에는 이중화 또는 서로 다른 원리의 센싱(Redundant or Diverse Sensing)을 적용할 수 있다. 소프트웨어는 명령된 잠금 상태와 실제로 확인된 물리적 체결 상태를 구분하여 전기적 명령만으로 화물이 안전하게 고정되었다고 판단하지 않도록 한다.

도어 및 해치 관리(Door and Hatch Management)는 화물 안전 로직(Cargo Safety Logic)과 통합된다. 위치 센서는 화물 도어, 램프(Ramp), 접근 패널(Access Panel)이 열려 있는지, 이동 중인지 또는 완전히 잠겼는지를 확인한다. 필수 폐쇄 상태가 확인되지 않으면 인터록(Interlock)이 이륙 승인을 차단한다. 비행 중 예상하지 못한 도어 상태 변화가 발생하면 공력 항력(Aerodynamic Drag), 구조 하중, 화물 구속, 기체 제어 가능성(Controllability)에 영향을 미칠 수 있으므로 즉각적인 고장 정보가 생성된다.

화물 제어기(Cargo Controller)는 상태기계(State Machine)를 사용하여 적재(Loading), 검증(Verification), 고정 완료(Secured), 비행(Flight), 투하 준비(Release-Ready), 하역(Unloading), 고장(Fault) 상태를 관리한다. 상태 전환에는 관련 센서와 임무 로직의 명시적인 확인이 필요하다. 지상 적재 중에는 유효한 명령이라도 추진 시스템 무장(Propulsion Arming) 또는 착륙장치 하중 해제(Weight-Off-Wheels)가 감지된 이후에는 금지될 수 있다. 이러한 상태 의존적 권한(State-Dependent Authority)은 화물 이동이 항공기 안전을 위협할 수 있는 비행 단계에서 화물 장치가 실수로 작동하는 것을 방지한다.

화물 인터페이스는 가능한 한 비행 필수 제어(Flight-Critical Control)로부터 전기적 및 논리적으로 격리되어야 한다. 고장난 적재장치 또는 주변 물류 장치(Peripheral Logistics Device)가 FCC에 필요한 통신 대역폭, 전력 또는 계산 자원을 소비해서는 안 된다. 정의된 인터페이스 계약(Interface Contract)은 메시지 내용, 갱신 주기(Update Frequency), 유효성, 타임아웃 동작(Timeout Behavior), 고장 의미(Fault Semantics)를 규정한다. 항공기 운용에 필요한 검증된 화물 파라미터만 비행 필수 영역으로 전달된다.

동적 하중 감시(Dynamic Load Monitoring)는 출발 이후 화물이 이동할 가능성을 다룬다. 화물 이동은 불충분한 구속, 변형(Deformation), 진동, 충격 또는 탑재화물 자체의 내부 움직임으로 발생할 수 있다. 로드셀, 변형률 센서(Strain Sensor), 관성 측정값 또는 항공기 트림(Trim)의 변화를 이용하여 비정상적인 하중 재분배(Load Redistribution)의 증거를 확인할 수 있다. 탐지된 이동은 단순한 물류 이상으로만 처리하지 않고 가용 제어 및 구조 여유도와 비교하여 평가한다.

비행제어 시스템(Flight-Control System)은 검증된 화물 정보를 사용하여 구성별 파라미터(Configuration-Specific Parameter)를 선택할 수 있다. 항공기 질량과 무게중심은 제어기 게인(Controller Gain), 트림, 추력 요구량, 제어 할당(Control Allocation), 기동 한계(Maneuver Limit), 비행영역 보호(Envelope Protection)에 영향을 미친다. FCC는 제한 없는 화물 제어기 상태가 아니라 범위가 제한된 구성 데이터(Bounded Configuration Data)를 수신해야 한다. 이륙 후 화물 정보를 사용할 수 없게 되면 FCC는 마지막으로 검증된 구성을 유지하고 필요한 경우 보수적인 제한조건(Conservative Limit)을 적용한다.

구조 하중 관리(Structural Load Management)는 화물 구성과 항공기 기동 상태를 결합한다. 무거운 탑재화물은 총중량(Gross Weight)이 허용 범위 내에 있더라도 바닥 하중(Floor Load), 체결부 하중(Attachment Force), 동체 굽힘모멘트(Fuselage Bending Moment), 착륙 충격 하중(Landing Impact Load)을 증가시킬 수 있다. 소프트웨어는 구성에 따라 가속도, 뱅크각(Bank Angle), 상승률, 하강률, 착륙 제한을 적용할 수 있다. 이후 임무계획은 특정 화물 배치에 승인된 하중영역(Load Envelope)을 초과하는 궤적을 회피한다.

에너지 계획(Energy Planning) 역시 탑재화물 상태와 연계된다. 추가 질량은 출력 요구량을 증가시키고 항속거리 또는 예비 에너지 능력(Reserve Capability)을 감소시킬 수 있기 때문이다. 출발 전에 임무 소프트웨어는 검증된 탑재화물 질량과 예상 경로 조건을 이용하여 에너지 소비량을 추정한다. 비행 중에는 실제 전력 사용량을 예측값과 비교한다. 지속적인 편차는 예상보다 강한 바람, 추진 성능 저하, 잘못된 탑재화물 데이터 또는 임무 재계획이 필요한 다른 상태를 의미할 수 있다.

여러 화물 단위(Cargo Unit)를 운송할 수 있는 항공기의 경우 하중관리는 각 화물을 개별적으로 추적하는 동시에 전체 구성을 관리한다. 하나의 화물을 제거하거나 배송하면 이후 비행 구간의 총질량과 무게중심이 변화한다. 소프트웨어는 확인된 모든 적재 또는 하역 이벤트 이후 기체 구성을 다시 계산한다. 새로운 구성이 구조, 제어, 추진 및 에너지 제약조건에 대해 검증되기 전에는 다음 임무 구간(Mission Leg)을 시작할 수 없다.

자동 적재 시스템(Automated Loading System)은 항공기와 외부 물류장비 사이의 협조된 움직임을 요구한다. 화물 로봇(Cargo Robot), 컨베이어(Conveyor), 지게차(Forklift), 팔레트 시스템 또는 지상 스테이션(Ground Station)은 무인항공기와 준비 상태 및 화물 이송 상태(Transfer State) 정보를 교환할 수 있다. 외부 장비가 작업 완료를 보고했다는 이유만으로 안전하다고 판단해서는 안 된다. 항공기는 물류 운용에서 비행 준비 상태로 전환하기 전에 탑재 센싱(Onboard Sensing)을 통해 화물의 존재, 위치 및 구속 상태를 독립적으로 확인한다.

화물 투하 메커니즘(Cargo Release Mechanism)은 의도하지 않은 투하가 항공기 질량, 무게중심 및 안전 상태를 즉시 변화시킬 수 있으므로 일반적인 적재 기능보다 강력한 권한 제어(Authority Control)가 필요하다. 투하 명령에는 지리적 위치, 고도, 대기속도, 항공기 자세, 임무 단계, 메커니즘 준비 상태의 확인이 요구될 수 있다. 적용 가능한 경우 다중 조건 승인(Multi-Condition Authorization) 또는 독립적인 안전 인터록(Independent Safety Interlock)을 사용하여 하나의 소프트웨어 오류만으로 투하가 시작되는 것을 방지한다.

의도적인 화물 투하(Intentional Cargo Release)는 비행제어 시스템과의 협조된 사전 준비도 요구한다. 질량이 급격히 감소하거나 하중 분포가 변화하면 수직 가속도, 자세 외란(Attitude Disturbance), 트림 변화가 발생할 수 있다. FCC는 승인된 투하에 대한 사전 통보를 받아 적절한 제어 기준값(Control Reference) 또는 게인 상태를 준비할 수 있다. 투하가 확인된 후에는 항공기 구성을 신속하게 갱신하여 추력, 궤적 및 비행영역 계산이 새로운 질량 특성(Mass Properties)을 반영하도록 한다.

고장 처리(Fault Handling)는 출발을 금지해야 하는 화물 시스템 고장과 비행 중 대응이 필요한 고장을 구분한다. 지상에서 적재 센서가 고장난 경우 단순히 임무 승인을 차단할 수 있지만, 비행 중 구속장치 고장이 탐지되면 즉각적인 기동 제한, 우회(Diversion) 또는 착륙이 필요할 수 있다. 따라서 고장 심각도(Fault Severity)는 임무 단계, 화물 유형, 잔여 구속 능력(Remaining Restraint Capability), 항공기 동역학, 안전한 착륙 선택지의 가용성에 따라 결정된다.

운용자에게 제공되는 화물 관련 경고(Cargo-Related Warning)는 단순한 원시 센서 고장이 아니라 운용상의 결과를 나타내야 한다. 다수의 개별 스위치 상태를 표시하는 대신 지상통제 시스템(Ground-Control System)은 화물 미고정(Cargo Unsecured), 무게중심 부적합(Center of Gravity Invalid), 하중 이동 감지(Load Shift Detected), 도어 폐쇄 미확인(Door Not Confirmed Closed), 투하 메커니즘 사용 불가(Release Mechanism Unavailable)와 같은 상태를 보고할 수 있다. 세부 진단 정보는 긴급한 의사결정 과정에서 운용자에게 과도한 정보를 제공하지 않으면서 유지보수 목적으로 접근할 수 있어야 한다.

화물 시스템과 상위 수준 컴퓨터 사이의 통신은 빠른 운용 상태(Fast Operational Status)와 상대적으로 느린 물류 정보(Slow Logistics Information)로 구분된다. 고속 경로(Fast Path)는 비행 안전에 영향을 미치는 구속 유효성, 도어 상태, 하중 이동 경고, 구성 식별정보, 투하 상태를 전달한다. 저속 경로(Slow Path)는 화물 적하목록(Cargo Manifest), 취급 이력(Handling History), 환경 기록, 유지보수 카운터(Maintenance Counter), 상세 센서 진단 정보를 전달하여 비행 필수 네트워크 트래픽과 경쟁하지 않도록 한다.

화물 구성 데이터(Cargo Configuration Data)는 통제된 버전 관리(Controlled Versioning)와 추적성(Traceability)을 필요로 한다. 항공기 제한값, 팔레트 정의(Pallet Definition), 구속장치 용량(Restraint Capacity), 적재 구역(Loading Zone), 센서 보정값, 승인된 탑재화물 범주는 기체 수명주기 동안 변경될 수 있다. 소프트웨어는 어떤 구성 데이터베이스(Configuration Database)가 활성화되어 있는지를 식별하고 호환되지 않는 조합을 방지해야 한다. 기록된 임무 데이터에는 각 비행에 사용된 정확한 화물 및 항공기 구성을 보존하여 이후 분석을 재현할 수 있도록 해야 한다.

내장시험(Built-In Test)은 임무 수행 전에 화물 인터페이스의 준비 상태를 검증한다. 시동 점검(Startup Check)은 제어기 메모리, 통신, 위치 센서, 로드셀, 잠금장치, 도어 스위치, 구동기 피드백(Actuator Feedback), 해당되는 경우 비상 투하 회로(Emergency Release Circuit)를 검사할 수 있다. 지상시험에서는 제한된 이동 범위에서 메커니즘을 작동시켜 정상 동작을 확인할 수 있다. 비행 중 시험은 의도하지 않게 화물 장치를 움직이거나 구속 무결성(Restraint Integrity)을 손상시킬 가능성이 없는 감시 기능으로 제한된다.

사이버보안(Cybersecurity)은 화물 시스템이 외부 물류 네트워크(External Logistics Network) 및 자동화된 지상장비와 상호작용할 수 있기 때문에 중요하다. 외부 시스템으로부터 수신된 화물 적하목록과 적재 명령은 항공기 구성에 영향을 미치기 전에 인증(Authentication)되고 검증되어야 한다. 네트워크 분리(Network Separation), 메시지 권한 검증(Message Authorization), 보안 업데이트 메커니즘(Secure Update Mechanism), 엄격한 명령 허용목록(Command Whitelisting)을 통해 침해된 물류장비가 비행 관련 화물 기능에 의도하지 않은 권한을 획득하는 것을 방지한다.

데이터 로깅(Data Logging)은 탑재화물 식별정보, 측정 질량, 무게중심 계산 결과, 구속 상태, 도어 상태 전환, 하중 측정값, 화물 명령, 투하 이벤트, 고장 및 구성 변경을 동기화된 타임스탬프와 함께 기록한다. 이러한 기록은 유지보수, 물류 추적성(Logistics Traceability), 사고 조사(Incident Investigation), 구조 수명 분석(Structural-Life Analysis)을 지원한다. 비행대 운용(Fleet Operation)에서는 누적된 하중 이력을 이용하여 반복적인 과적 경향이나 구속 메커니즘의 성능 저하를 식별할 수도 있다.

검증(Verification)은 소프트웨어 시뮬레이션, 센서 에뮬레이션(Sensor Emulation), 하드웨어 인더 루프(Hardware-in-the-Loop) 시험, 화물 메커니즘 시험장치(Cargo-Mechanism Rig), 구조 하중시험(Structural Loading Test), 통합 지상시험(Integrated Ground Trial), 비행시험을 활용한다. 시험 시나리오에는 잘못된 질량 신고, 부적절한 화물 위치, 잠금장치 고장, 센서 불일치, 도어 고장, 하중 이동, 통신 손실, 승인 또는 거부된 투하 명령이 포함된다. 시험을 통해 화물 관련 이상 상태가 비행 안전 인터록을 우회하거나 통제되지 않은 구성 변화를 발생시키지 않는지를 확인한다.

완성된 화물 인터페이스 및 하중관리 시스템(Cargo Interface and Load-Management System)은 화물을 외부 물류 문제로 취급하는 대신 탑재화물 정보를 검증된 항공기 구성의 일부로 변환한다. 핵심 기능은 무엇을 운송하는지, 어디에 위치하는지, 안전하게 고정되어 있는지, 그리고 상태 변화가 기체 능력에 어떤 영향을 미치는지를 파악하는 것이다. 2.5톤 화물 무인항공기에서 이러한 통합은 자율 물류 운용(Autonomous Logistics Operation)이 검증된 구조, 추진, 제어, 에너지 및 안전 한계 내에서 수행되도록 한다.

## 10.07. 2.5t GCS and Telemetry Link Design [w/Code]

![](images/image7.png){width="7.268055555555556in" height="7.268055555555556in"}

![](images/image8.png){width="7.268055555555556in" height="4.845138888888889in"}

2.5톤 화물 무인항공기(Cargo UAV)의 지상통제소(Ground Control Station, GCS) 및 원격측정 링크 아키텍처(Telemetry-Link Architecture)는 원격 운용자(Remote Personnel), 비행대 서비스(Fleet Service), 비행 중인 항공기 사이의 운용 연결을 제공한다. 항공기는 상당한 탑재하중과 운동에너지(Kinetic Energy)를 가지므로 통신 시스템은 안정적인 지휘통제(Command and Control, C2)를 지원하면서도 지속적인 통신 연결에 안전 비행이 의존하지 않도록 설계되어야 한다. 외부 링크의 성능이 저하되거나 완전히 상실되더라도 탑재 자율 시스템(Onboard Autonomy)과 비행제어컴퓨터(Flight Control Computer, FCC)는 필수적인 제어 권한을 유지한다.

통신 아키텍처(Communication Architecture)는 지휘통제(C2), 안전 필수 원격측정(Safety-Critical Telemetry), 임무 데이터(Mission Data), 탑재화물 정보(Payload Information), 유지보수 트래픽(Maintenance Traffic), 고대역폭 센서 스트림(High-Bandwidth Sensor Stream)을 운용 중요도에 따라 분리한다. 필수 명령과 기체 상태 메시지는 영상, 로그 또는 대용량 데이터 전송보다 결정론적인 우선순위(Deterministic Priority)를 갖는다. 이러한 분리는 비필수 서비스에서 발생한 통신 혼잡이 항공기 감독 또는 비상 대응에 필요한 정보의 전달을 지연시키는 것을 방지한다.

지역 간 거리를 운항하는 화물 무인항공기는 여러 통신 기술을 동시에 사용할 수 있다. 가시선 무선통신(Line-of-Sight Radio)은 운용 기지 인근에서 낮은 지연시간의 연결을 제공할 수 있으며, 셀룰러(Cellular), 사설 광대역 통신(Private Broadband), 위성 링크(Satellite Link)는 지역 통신 인프라를 넘어 통신 범위를 확장할 수 있다. 탑재 통신 관리자(Onboard Communication Manager)는 하나의 영구적인 통신 채널을 가정하지 않고 지연시간, 대역폭, 무결성(Integrity), 비용, 통신 범위, 안전 요구조건에 따라 사용 가능한 링크를 평가하고 트래픽을 경로 설정한다.

GCS는 운용자가 항공기 상태를 신속하게 이해할 수 있는 형태로 기체 상태를 제공한다. 주요 정보에는 항공기 위치, 고도, 속도, 자세, 비행 모드, 경로 진행 상태(Route Progress), 항법 무결성(Navigation Integrity), 추진 능력(Propulsion Capability), 에너지 예비량(Energy Reserve), 화물 상태, 통신 품질 및 활성 고장(Active Fault)이 포함된다. 인터페이스는 운용상의 결과를 우선적으로 표시하여 운용자가 항공기가 정상 상태를 유지하는지, 성능 저하 상태에 진입했는지 또는 개입이 필요한지를 판단할 수 있도록 한다.

여러 정보원이 항공기에 영향을 미치려고 할 수 있으므로 명령 권한(Command Authority)은 명확하게 통제된다. 명령은 운용자, 임무관리 시스템(Mission-Management System), 비행대 감독 시스템(Fleet Supervisor) 또는 승인된 외부 서비스에서 생성될 수 있다. 각각의 명령원은 인증(Authentication)되고 정의된 권한 수준(Authority Level)을 부여받는다. 탑재 시스템은 사전에 정의된 규칙에 따라 명령 충돌을 해결하며, FCC는 비행 모드, 비행영역(Flight Envelope), 항법 무결성 또는 기체 능력 제약조건을 위반하는 명령을 독립적으로 거부한다.

C2 링크는 정상적인 자율 운용 중 제한 없는 저수준 구동기 명령 대신 범위가 제한된 상위 수준 명령(Bounded High-Level Command)을 전달한다. 대표적인 메시지에는 임무 승인(Mission Authorization), 웨이포인트 변경(Waypoint Change), 대기 요청(Hold Request), 경로 수정(Route Modification), 복귀 명령(Return Command), 착륙장 선택(Landing-Site Selection), 비상조치(Emergency Action)가 포함된다. 안정화(Stabilization)는 기체 내부에서 수행되므로 네트워크 지연이 고주기 자세제어 루프(High-Rate Attitude-Control Loop)에 직접 유입되지 않는다. 이러한 아키텍처는 통신 지연시간이 변화하더라도 항공기의 안정성이 손상되지 않도록 한다.

원격측정(Telemetry)은 갱신 주기(Update Rate)와 중요도(Criticality)에 따라 구성된다. 고속 원격측정(Fast Telemetry)은 비행 상태, 제어 모드, 항법 품질, 추진 상태, 에너지 여유도 및 즉각적인 경고를 전달한다. 중간 주기의 정보는 하위 시스템 상태와 임무 진행 상황을 제공하며, 저속 원격측정(Slow Telemetry)은 유지보수 통계, 상세 온도, 구성 정보(Configuration Information), 진단 기록(Diagnostic Record)을 전달한다. 가용 대역폭이 감소하면 적응형 스케줄링(Adaptive Scheduling)을 통해 낮은 우선순위의 트래픽을 감소시킬 수 있다.

메시지 타이밍(Message Timing)은 데이터 유효성(Data Validity)의 일부로 취급된다. 각각의 운용 메시지에는 타임스탬프(Timestamp), 시퀀스 정보(Sequence Information), 송신원 식별정보(Source Identity), 필요한 경우 만료시간(Expiration Interval)이 포함된다. 따라서 수신기는 현재 유효한 명령과 과도한 네트워크 지연 이후 도착한 명령을 구분할 수 있다. 지상 및 탑재 시스템은 동기화된 시간 기준(Synchronized Time Reference)을 유지하여 운용 중 및 비행 후 분석 과정에서 여러 하위 시스템의 원격측정 데이터를 상호 연계할 수 있도록 한다.

링크 감시(Link Monitoring)는 수신 신호 품질, 패킷 손실(Packet Loss), 왕복 지연시간(Round-Trip Delay), 지터(Jitter), 처리량(Throughput), 인증 상태, 메시지 최신성(Message Freshness)을 지속적으로 평가한다. 단일 지표만으로 통신 상태를 판단할 수는 없다. 통신 링크가 기술적으로 연결된 상태를 유지하더라도 지연시간 때문에 운용 제어에 적합하지 않을 수 있다. 따라서 통신 관리자는 링크 상태를 정상(Nominal), 제한(Constrained), 성능 저하(Degraded), 사용 불가(Unavailable)와 같은 능력 상태(Capability State)로 분류하고 이를 임무관리 시스템에 제공한다.

이중화 통신 링크(Redundant Communication Link)는 가용성(Availability)을 향상시키지만 통제된 전환 동작(Controlled Switching Behavior)을 필요로 한다. 항공기는 주 통신 링크와 하나 이상의 보조 링크를 동시에 유지하여 품질이 저하될 경우 신속한 장애조치(Failover)를 수행할 수 있다. 세션 상태(Session State), 명령 순서(Command Sequence), 권한 부여 상태(Authorization Context)는 채널 전환 과정에서도 유지되어야 한다. 그렇지 않으면 기술적으로 성공적인 무선 핸드오버(Radio Handover)가 중복 명령, 누락된 응답 또는 제어 권한에 대한 일시적인 불확실성을 발생시킬 수 있다.

통신두절 동작(Loss-of-Link Behavior)은 통신 장애가 발생한 이후 즉흥적으로 결정하는 것이 아니라 출발 전에 정의한다. 임무, 위치, 에너지 상태, 기상, 공역 제약조건에 따라 항공기는 승인된 경로를 계속 비행하거나 지정된 지점에서 대기하거나 기지로 복귀하거나 우회 또는 사전 정의된 장소에 착륙할 수 있다. 선택된 정책은 이륙 전에 업로드되고 검증되어 지상 통신이 완전히 사라지더라도 항공기가 이를 자율적으로 수행할 수 있도록 한다.

통신두절 이후의 복구(Communication Recovery) 역시 통제되어야 한다. 링크가 복구되더라도 항공기는 대기 중이던 모든 명령을 즉시 수락하지 않는다. 인증을 다시 설정하고 오래된 메시지(Stale Message)를 폐기하며 현재 임무 상태를 동기화하고 명령 권한을 확인한다. 새로운 지시를 전달하기 전에 운용자는 항공기의 실제 자율 운용 상태를 확인한다. 이를 통해 통신두절 이전에 생성된 오래된 명령이 정상적으로 수행 중인 비상 대응 기동(Contingency Maneuver)을 방해하는 것을 방지한다.

GCS 임무계획 기능(Mission-Planning Function)은 운용자가 경로, 고도 제약조건, 웨이포인트, 착륙장, 지오펜스(Geofence), 통신 예상 조건, 비상 대응 지점(Contingency Point)을 정의할 수 있도록 한다. 임무를 업로드하기 전에 계획을 항공기 능력, 탑재화물 구성, 예상 에너지 사용량, 공역 제한 및 사용 가능한 대안과 비교하여 검증한다. 탑재 시스템은 수신된 임무를 독립적으로 다시 검증하여 잘못된 지상 임무계획이 탑재 안전 제약조건을 자동으로 우회하지 못하도록 한다.

지리공간 정보(Geospatial Information)는 안전한 감독에 충분한 맥락과 함께 표시된다. 지도에는 계획 및 실제 궤적, 통제 또는 제한공역, 지형, 장애물, 착륙장, 비상 대응 구역(Contingency Zone), 동적 제한구역(Dynamic Restriction)이 포함될 수 있다. 항법 품질이 저하될 경우 잘못된 정밀도의 항공기 기호를 표시하는 대신 위치 불확실성(Position Uncertainty)을 표현해야 한다. 운용자는 검증된 항공기 위치와 불확실성이 증가하는 추정 위치를 구분할 수 있어야 한다.

경고 관리(Alert Management)는 다수의 하위 시스템 고장을 우선순위가 설정된 운용 메시지로 변환하여 운용자 과부하(Operator Overload)를 방지한다. 관련 고장은 추진 능력 감소(Reduced Propulsion Capability), 항법 성능 저하(Navigation Degraded), 에너지 예비량 부족(Energy Reserve Low), 화물 미고정(Cargo Unsecured), C2 링크 제한(C2 Link Constrained)과 같은 상태로 통합할 수 있다. 경고에는 심각도(Severity), 지속성(Persistence), 영향을 받는 능력 및 권장 운용 정보가 포함된다. 세부 공학 데이터는 기본 비행감시 화면을 방해하지 않으면서 별도로 확인할 수 있다.

인간-기계 인터페이스(Human-Machine Interface, HMI) 설계는 우발적인 명령을 최소화해야 한다. 임무 종료, 비상 착륙, 화물 투하, 추진 정지 또는 주요 경로 변경과 같이 안전에 중요한 동작은 확인 절차 또는 상황에 따른 권한 승인(Context-Dependent Authorization)을 요구할 수 있다. 인터페이스는 명령 준비(Command Preparation)와 실제 실행을 구분하고 명령이 전송, 수신, 수락, 거부 또는 완료되었는지를 명확하게 표시한다. 모호한 명령 상태(Ambiguous Command State)는 안전 문제로 취급된다.

비행대 운용(Fleet Operation)을 위해 GCS 아키텍처는 한 명의 운용자가 한 대의 항공기만 제어하는 구조를 넘어 확장될 수 있어야 한다. 감독 시스템(Supervisory System)은 여러 항공기를 표시하면서 예외 상황과 성능 저하 상태에 운용자의 주의를 집중시킬 수 있다. 각 무인항공기는 독립적인 탑재 안전 기능을 유지하고, 비행대 소프트웨어는 경로, 일정, 착륙 자원 및 통신 용량을 조정한다. 운용자 또는 통제소 사이의 권한 이전(Authority Transfer)은 식별정보, 임무 상태 및 명령 연속성을 보존해야 한다.

무인항공교통관리(Unmanned Traffic Management, UTM), 기상 제공자(Weather Provider), 물류 플랫폼(Logistics Platform), 비행대 서버(Fleet Server)와 같은 외부 서비스는 GCS와 정보를 교환할 수 있다. 이러한 서비스는 제한 없는 직접 제어가 아니라 범위가 제한된 데이터(Bounded Data)를 제공한다. 입력 데이터가 임무계획에 영향을 미치기 전에 송신원 진위성(Source Authenticity), 데이터 생성 시점, 지리적 관련성(Geographic Relevance), 일관성을 확인한다. 외부 서비스가 고장나거나 침해되더라도 항공기는 안전한 비상 대응 절차를 수행할 수 있어야 한다.

사이버보안(Cybersecurity)은 C2 또는 원격측정에 대한 비인가 접근이 직접적인 물리적 결과를 발생시킬 수 있으므로 통신 아키텍처에 통합된다. 상호 인증(Mutual Authentication), 암호화 전송(Encrypted Transport), 보호된 인증정보(Protected Credential), 안전한 키 저장(Secure Key Storage), 메시지 무결성 검사(Message-Integrity Check), 재전송 공격 방지(Replay Protection), 통제된 소프트웨어 업데이트가 통신 채널을 보호한다. 네트워크 분할(Network Segmentation)은 비행 필수 인터페이스를 유지보수, 탑재장비 및 일반 정보 서비스와 분리하여 침해된 엔드포인트(Compromised Endpoint)의 영향이 확산되는 것을 제한한다.

가용성은 보안과 균형을 이루어야 하며 보안 메커니즘 자체가 위험한 통신 장애를 발생시켜서는 안 된다. 인증서 만료(Certificate Expiration), 클록 오류(Clock Error), 키 교체(Key Rotation), 인증 서비스 장애(Authentication-Service Outage)에 대한 명확한 처리 절차가 필요하다. 보안 상태(Security State)는 다른 시스템 상태 정보와 마찬가지로 감시된다. 복구 메커니즘은 시간에 민감한 상황에서도 신원 또는 무결성 검증을 우회하지 않으면서 신뢰 경계(Trust Boundary)를 유지하고 승인된 운용을 재개할 수 있도록 해야 한다.

고용량 센서 또는 탑재장비 데이터가 통신 인프라를 공유하는 경우 대역폭 관리(Bandwidth Management)가 중요하다. 영상, 이미지, 지도 데이터 또는 유지보수 로그는 C2 원격측정보다 훨씬 많은 대역폭을 사용할 수 있다. 서비스 품질 정책(Quality-of-Service Policy)은 필수 메시지를 위한 통신 용량을 확보하고 네트워크 성능이 저하될 경우 낮은 우선순위 스트림을 제한, 압축, 축소 또는 중단할 수 있다. 따라서 심각한 대역폭 경쟁 상황에서도 C2 트래픽을 유지할 수 있다.

저장 후 전달(Store-and-Forward) 메커니즘은 지속적인 광대역 통신을 사용할 수 없는 운용을 지원한다. 비필수 로그, 이미지 또는 유지보수 기록은 기체 내부에 임시 저장한 후 대역폭이 확보되었을 때 전송할 수 있다. 안전 필수 원격측정은 지연된 정보가 더 이상 현재 기체 상태를 나타내지 않을 수 있으므로 동일한 방식으로 처리하지 않는다. 따라서 아키텍처는 지연 후에도 가치가 유지되는 데이터와 엄격한 최신성 요구조건(Freshness Requirement)을 충족해야 하는 정보를 구분한다.

GCS는 명령, 응답(Acknowledgment), 운용자 조작, 원격측정, 임무 변경, 경고, 통신 품질 지표, 권한 이전을 동기화된 타임스탬프와 함께 기록한다. 이러한 기록을 통해 사고 조사 과정에서 비행 중 기체 동작과 지상 의사결정을 모두 재구성할 수 있다. 감사 추적(Audit Trail)은 운용자 또는 자율 시스템이 특정 행동을 수행했을 때 어떤 정보를 사용할 수 있었는지를 보여줌으로써 유지보수, 운용 개선, 사이버보안 분석 및 규제 증빙(Regulatory Evidence)도 지원한다.

운용 위험이 요구하는 경우 이중화 전원(Redundant Power), 네트워크 연결, 컴퓨팅 자원 및 데이터 저장장치를 통해 지상통제소 가용성을 보호한다. 주 통제소를 사용할 수 없게 될 경우 감독을 인수할 수 있도록 백업 통제소(Backup Control Station)를 준비할 수 있다. 권한 이전은 두 통제소가 동시에 자신이 독점적인 명령 권한을 가지고 있다고 판단하지 않도록 명확한 프로토콜을 따른다. 항공기는 이러한 이전 과정에서도 현재의 안전한 임무 상태를 유지한다.

GCS 및 원격측정 시스템의 검증(Verification)은 네트워크 시뮬레이션(Network Simulation), 지연시간 및 패킷 손실 주입(Latency and Packet-Loss Injection), 대역폭 포화(Bandwidth Saturation), 인증 실패, 무선 핸드오버, 지상통제소 장애, 완전한 통신두절 시나리오를 포함한다. 하드웨어 인더 루프(Hardware-in-the-Loop) 환경에서는 실제 비행컴퓨터가 성능이 저하된 통신 네트워크와 상호작용하도록 할 수 있다. 시험을 통해 지연, 중복, 순서 변경, 손상, 비인가 또는 누락된 메시지가 통제되지 않은 항공기 동작을 발생시키지 않는지 확인한다.

현장시험(Field Testing)은 대표적인 경로, 지형, 고도, 통신 인프라 커버리지 및 전자기 환경(Electromagnetic Environment)에서 통신 성능을 단계적으로 평가한다. 측정된 지연시간, 가용성, 장애조치 시간(Failover Time), 패킷 손실 및 유효 대역폭(Effective Bandwidth)을 설계 가정과 비교한다. 평균적인 성능 통계로 통신 음영지역(Coverage Gap)을 감추는 대신 이를 임무계획에 반영하여 통신이 예측 가능하게 약한 구간에서도 자율 비상 대응 동작을 검증할 수 있도록 한다.

완성된 GCS 및 원격측정 링크 설계(GCS and Telemetry-Link Design)는 운용자에게 신뢰할 수 있는 상황인식(Situational Awareness)과 제한된 명령 권한을 제공하면서 즉각적인 비행 안전을 위한 탑재 시스템의 독립성을 유지해야 한다. 시스템의 효과는 지속적인 통신 연결 여부만으로 평가되는 것이 아니라 링크가 느려지거나 간헐적으로 연결되거나 침해되거나 완전히 사용할 수 없게 되었을 때에도 예측 가능한 동작을 유지하는지에 따라 평가된다. 따라서 2.5톤 화물 무인항공기에서 복원력 있는 통신(Resilient Communication)은 자율성을 지원하면서도 안전한 항공기 운용을 위한 단일고장점(Single Point of Failure)이 되지 않도록 설계되어야 한다.

## 10.08. 2.5t Safety System and Parachute Trigger [w/Code]

![](images/image9.png){width="7.268055555555556in" height="7.268055555555556in"}

2.5톤 화물 무인항공기(Cargo UAV)의 안전 시스템(Safety System)은 정상 비행제어(Nominal Flight Control), 이중화(Redundancy) 또는 자율 비상대응 로직(Autonomous Contingency Logic)만으로 적절하게 관리할 수 없는 상황에 대해 독립적인 보호 계층(Independent Protection Layer)을 제공한다. 이러한 질량의 항공기는 상당한 운동에너지(Kinetic Energy)와 위치에너지(Potential Energy)를 가지므로 비상 기능은 명확한 위험 분석(Hazard Analysis)을 기반으로 해야 한다. 기술적으로 적용 가능한 경우 낙하산 시스템(Parachute System)은 모든 상황에 적용되는 보편적인 복구 수단이 아니라 다계층 안전 아키텍처(Layered Safety Architecture)의 하나의 구성요소로 취급된다.

안전 아키텍처(Safety Architecture)는 위험한 항공기 상태를 식별하고 어떤 시스템이 이를 예방, 제어 또는 완화할 수 있는지를 결정하는 것에서 시작한다. 대표적인 상황에는 추진력 상실(Loss of Propulsion), 자세제어 상실(Loss of Attitude Control), 구조 고장(Structural Failure), 복구 불가능한 항법 성능 저하(Unrecoverable Navigation Degradation), 비행컴퓨터 고장(Flight-Computer Failure), 에너지 시스템 고장(Energy-System Fault), 통제 불가능한 하강(Uncontrolled Descent)이 포함된다. 각 위험요소에는 탐지 기준, 요구 대응시간, 사용 가능한 복구 방안, 비상 하강 또는 낙하산 전개가 전체 위험을 감소시킬 수 있는 조건이 연계된다.

비상 안전 제어기(Emergency Safety Controller)는 공통 비행제어 자원이 고장난 상황에서도 기능을 유지할 수 있도록 주 비행제어컴퓨터(Flight Control Computer, FCC)로부터 충분한 독립성을 가져야 한다. 보증 목표(Assurance Objective)에 따라 독립 프로세서, 전원공급장치, 관성 센싱(Inertial Sensing), 고도 정보원(Altitude Source), 감시 타이머(Watchdog), 이산 인터페이스(Discrete Interface)를 사용할 수 있다. 독립성이 항공전자 시스템 전체의 완전한 복제를 의미하는 것은 아니지만, 단일 고장으로 정상 제어와 최종 안전 기능이 동시에 상실되지 않도록 공통원인 의존성(Common-Cause Dependency)을 명확히 파악해야 한다.

안전 감시기(Safety Monitor)는 제한되고 결정론적인 로직(Bounded Deterministic Logic)을 사용하여 항공기 운동 및 상태 정보를 지속적으로 평가한다. 관련 입력에는 자세, 각속도(Angular Rate), 수직속도(Vertical Velocity), 고도, 대기속도(Airspeed), 추진 가용성(Propulsion Availability), FCC 하트비트(Heartbeat), 제어 여유도(Control Margin), 구조 상태(Structural Status), 항법 무결성(Navigation Integrity)이 포함될 수 있다. 하나의 잘못된 센서 값으로 인해 불필요하고 되돌릴 수 없는 비상 동작이 시작되지 않도록 가능한 경우 여러 지표를 함께 사용하여 안전 결정을 수행해야 한다.

비상 상태 탐지(Emergency-State Detection)는 일시적인 외란과 실제로 복구가 불가능한 상태를 구분한다. 지속시간 타이머(Persistence Timer), 센서 보팅(Sensor Voting), 타당성 검사(Plausibility Check), 채널 간 비교(Cross-Channel Comparison), 동적 상태 평가(Dynamic-State Evaluation)를 사용하여 오작동에 의한 불필요한 작동(Nuisance Triggering)을 줄인다. 동시에 복구가 불가능해질 정도로 탐지 로직이 지나치게 오래 기다려서는 안 된다. 따라서 작동 임계값(Trigger Threshold)은 항공기 동역학, 낙하산 전개시간, 최소 복구 고도(Minimum Recovery Altitude), 예상 고장 진행 과정과 함께 설계된다.

대형 무인항공기의 낙하산 복구 시스템(Parachute Recovery System)은 사용 가능한 전개영역(Deployment Envelope)을 신중하게 정의해야 한다. 전개 효과는 지형 상공 고도(Altitude Above Terrain), 수직 및 수평속도, 항공기 자세, 각속도, 구조 상태, 대기환경, 낙하산 팽창 특성(Parachute Inflation Characteristics)에 따라 달라진다. 소프트웨어는 모든 비상상황에서 낙하산 작동이 유리하다고 가정하는 대신 현재 항공기 상태가 검증된 전개영역 내부에 있는지를 판단한다.

최소 전개고도(Minimum Deployment Altitude)는 캐노피 사출(Canopy Extraction), 산줄 전개(Line Extension), 팽창(Inflation), 하강 안정화(Descent Stabilization)에 일정한 시간과 거리가 필요하기 때문에 특히 중요한 제약조건이다. 적용 가능한 최소 고도는 수직속도와 항공기 자세에 따라 달라질 수 있다. 따라서 안전 소프트웨어는 단일 고도 임계값만 사용하는 대신 전개 과정에서 예상되는 고도 손실(Predicted Height Loss)을 평가한다. 효과적인 전개영역보다 낮은 고도에서는 다른 최소위험 대응(Minimum-Risk Response)이 더 나은 결과를 제공할 수 있다.

과도한 각속도 또는 비정상 자세(Abnormal Attitude)는 낙하산 줄의 얽힘(Entanglement), 비대칭 팽창(Asymmetric Inflation), 구조물과의 간섭 가능성을 증가시켜 낙하산 전개를 어렵게 만들 수 있다. 제어 권한(Control Authority)이 남아 있는 경우 항공기는 전개 전에 짧은 안정화 기동(Stabilization Maneuver)을 시도할 수 있다. 이러한 사전 안정화는 엄격한 시간 제한을 가진다. 안정화가 실패하거나 잔여 고도가 부족해지면 안전 로직은 동작을 무기한 지연하지 않고 사전에 정의된 비상 우선순위에 따라 전환한다.

낙하산 작동(Parachute Triggering)은 일반적인 임무 명령과 분리된 보호된 승인 경로(Protected Authorization Path)를 사용한다. 전개 구동기(Deployment Actuator)는 일반적으로 유지보수, 운송, 적재 및 기타 지상 상태에서는 작동이 금지된다. 정의된 비행 조건과 시스템 점검이 충족된 이후에만 무장(Arming)이 이루어진다. 무장 이후 전개에는 안전 아키텍처와 인증 전략(Certification Strategy)에 따라 여러 논리 조건의 일치 또는 독립적인 비상 명령 경로가 요구될 수 있다.

작동 인터페이스(Trigger Interface)는 무장(Armed), 안전(Safe), 명령(Commanded), 작동(Fired), 전개 확인(Deployment-Confirmed) 상태를 명확하게 구분해야 한다. 전기적 연속성(Electrical Continuity)만으로 성공적인 전개를 입증할 수 없으므로 가능한 경우 추가 피드백을 통해 구동기 작동, 사출 메커니즘 위치, 산줄 해제(Line Release), 캐노피 전개 또는 하강 응답을 감시할 수 있다. 안전 시스템은 명령 정보와 확인 정보를 모두 기록하여 전개 실패와 명령 자체가 발생하지 않은 경우를 구분할 수 있도록 한다.

비의도적 전개 방지(Inadvertent Deployment Prevention)는 성공적인 비상 작동만큼 중요하다. 기계적 안전장치(Mechanical Safing Device), 전기적 금지회로(Electrical Inhibit), 소프트웨어 인터록(Software Interlock), 명령 인증(Command Authentication), 비행 상태 검증(Flight-State Validation), 독립적인 무장 로직을 이용하여 우발적인 작동에 대한 다중 방어장벽(Multiple Barrier)을 구성할 수 있다. 일반적인 소프트웨어 업데이트, 통신 고장, 단일 손상 메시지 또는 운용자 인터페이스 오류만으로 모든 전개 보호 기능을 우회할 수 있어서는 안 된다.

원격 운용자에게 수동 비상 권한(Manual Emergency Authority)을 제공할 수 있지만 운용자 명령 역시 신중하게 정의된 안전 로직의 적용을 받는다. 통신 지연과 불완전한 상황인식(Situational Awareness)으로 인해 수동 작동만으로는 신뢰할 수 있는 대응이 어려울 수 있다. 탑재 자동 작동 기능(Onboard Automatic Trigger)은 시간에 민감한 상태가 탐지될 경우 대응할 수 있으며, 원격 명령은 운용자가 인식한 상황에 대한 추가적인 경로를 제공한다. 자동 권한과 수동 권한 사이의 관계는 명확해야 한다.

낙하산 전개 중 추진 시스템 동작(Propulsion Behavior)은 명시적인 협조가 필요하다. 회전 중인 프로펠러 또는 동력이 공급되는 로터는 낙하산 줄을 손상시키거나 캐노피 사출을 방해하거나 위험한 기체 운동을 발생시킬 수 있다. 비상 절차는 항공기 설계에 따라 추진 정지(Propulsion Shutdown), 제동(Braking), 페더링(Feathering) 또는 기타 구성 변경을 명령할 수 있다. 필요한 순서와 타이밍은 추진 상태 전환 자체로 인해 전개가 가용 복구시간을 초과하여 지연되지 않도록 검증되어야 한다.

에너지 격리(Energy Isolation)도 비상 절차의 일부를 구성할 수 있다. 고전압 배터리(High-Voltage Battery), 연료 시스템(Fuel System), 발전기(Generator), 전력변환기(Power Converter)는 충돌 이후 화재 또는 전기적 위험을 발생시킬 수 있다. 필요한 경우 안전 제어기는 필수 전개 동작이 완료된 후 선택된 에너지원의 격리를 명령할 수 있다. 비상 위치표시(Beaconing), 추적(Tracking), 하강 감시에 필요한 안전 필수 전자장치는 독립 비상전원(Independent Emergency Supply)을 통해 계속 작동할 수 있다.

2.5톤 화물 무인항공기는 전체 질량의 상당 부분이 탑재화물로 구성될 수 있으므로 화물 상태(Cargo State)를 반드시 고려해야 한다. 낙하산 시스템과 체결 구조(Attachment Structure)는 승인된 질량 및 무게중심(Center of Gravity) 범위를 기준으로 설계된다. 소프트웨어는 현재 항공기 구성이 비상 복구영역(Emergency Recovery Envelope)과 호환되는지를 검증한다. 예상하지 못한 화물 이동(Cargo Movement)은 하강 자세 또는 구조 하중을 변화시킬 수 있으므로 사용 가능한 비상 대응 전략에도 영향을 미칠 수 있다.

지리적 위험(Geographic Risk)은 즉각적인 항공기 안정화 요구조건보다 우선하지 않는 범위에서 비상 의사결정 로직에 영향을 미칠 수 있다. 충분한 제어 능력이 남아 있다면 항공기는 비상 하강을 시작하기 전에 인구 밀집지역, 위험 기반시설(Hazardous Infrastructure), 부적합한 지형에서 벗어나도록 이동을 시도할 수 있다. 이러한 기동은 잔여 고도, 에너지, 제어 권한 및 시간에 의해 제한된다. 이상적인 착륙 위치를 찾기 위해 최종 안전조치(Final Safety Action)를 불필요하게 지연해서는 안 된다.

낙하산 전개가 성공하면 항공기는 제어 비행(Controlled Flight)에서 관리된 비상 하강(Managed Emergency Descent) 상태로 전환한다. 더 이상 의미가 없는 비행제어 기능은 비활성화되며 하강률, 위치, 에너지 시스템 상태 및 낙하산 상태에 대한 감시는 계속 수행된다. 통신 시스템은 가능한 경우 비상 상태와 위치를 GCS에 전송한다. 운용 개념(Operational Concept)에 따라 항공기는 시각, 음향 또는 전자식 복구 보조장치(Recovery Aid)를 활성화할 수 있다.

낙하산 전개 실패 또는 부분 전개(Partial Deployment)에 대해서는 사전에 정의된 대체 동작(Fallback Behavior)이 필요하다. 유효한 공력 또는 추진 제어 권한이 남아 있는 경우 항공기는 부분적으로 전개된 시스템을 방해하지 않으면서 하강률을 감소시키거나 자세를 안정화할 수 있다. 소프트웨어는 서로 양립할 수 없는 복구 모드 사이를 반복적으로 전환해서는 안 된다. 비상 우선순위(Emergency Priority)는 전개 절차가 특정 상태를 지나간 이후 어떤 기능을 계속 활성화하고 어떤 명령을 영구적으로 금지할지를 정의한다.

안전 아키텍처는 낙하산 전개가 적절하지 않은 비상상황도 처리한다. 통제된 우회(Controlled Diversion), 즉각적인 착륙(Immediate Landing), 제자리비행 종료(Hover Termination), 지원되는 경우 자동회전 또는 활공 동작(Autorotative or Gliding Behavior), 추진 시스템 재구성(Propulsion Reconfiguration), 최소위험 궤적 제어(Minimum-Risk Trajectory Control)가 일부 고장에서는 더 나은 결과를 제공할 수 있다. 비상 관리기(Emergency Manager)는 모든 심각한 고장에 낙하산 작동을 기본적으로 적용하는 대신 현재 가용 능력에 따라 검증된 대응 방법을 선택한다.

낙하산 시스템의 상태 감시(Health Monitoring)는 비행 전에 시작된다. 내장시험(Built-In Test)은 제어기 메모리, 독립 전원, 작동회로 연속성(Trigger Continuity), 무장 인터페이스, 센서, 전개 구동기 상태, 안전 감시기와의 통신을 검증할 수 있다. 정비주기(Maintenance Interval), 포장 유효기간(Packing Life), 환경 노출(Environmental Exposure), 부품 교체 상태는 구성관리(Configuration Management)를 통해 추적한다. 승인된 운용 구성에서 낙하산 복구 시스템이 필수인 경우 유효기간이 만료되었거나 잘못 구성된 시스템은 운항 승인을 차단한다.

전개 임계값은 항공기 질량 특성(Mass Properties), 낙하산 유형, 체결 구성, 구동기 특성 및 소프트웨어 버전에 따라 달라지므로 구성관리(Configuration Control)는 특히 중요하다. 안전 제어기는 초기화 과정에서 이러한 요소 사이의 호환성을 검증한다. 비상 작동에 영향을 미치는 파라미터는 통제되지 않은 변경으로부터 보호되며, 변경 사항에는 다른 안전 필수 비행 기능과 동일한 수준의 체계적인 검증 절차가 적용된다.

데이터 로깅(Data Logging)은 비상 의사결정을 재구성하는 데 필요한 정보를 보존한다. 기록 변수에는 센서 입력, FCC 상태, 제어 여유도, 탐지된 고장, 작동 조건(Trigger Condition), 무장 상태, 운용자 명령, 전개 순서, 확인 피드백, 전개 이후 기체 운동이 포함된다. 고해상도 동기화 타임스탬프(High-Resolution Synchronized Timestamp)를 통해 조사자는 낙하산이 전개되었는지 여부뿐만 아니라 안전 로직이 특정 시점에 왜 전개가 필요하다고 판단했는지도 확인할 수 있다.

많은 치명적 상태(Catastrophic Condition)를 비행시험에서 반복적으로 생성할 수 없으므로 검증(Verification)은 시뮬레이션에 크게 의존한다. 비선형 기체 모델(Nonlinear Vehicle Model), 낙하산 동역학(Parachute Dynamics), 센서 고장, 추진 고장, 프로세서 고장, 통신 장애를 몬테카를로 시나리오(Monte Carlo Scenario)에서 조합한다. 소프트웨어 인더 루프(Software-in-the-Loop)와 하드웨어 인더 루프(Hardware-in-the-Loop) 시험을 통해 반복 가능한 비정상 조건에서 타이밍, 상태 전환, 작동 금지(Trigger Inhibition), 자동 승인 및 FCC와의 상호작용을 검증한다.

지상시험과 통제된 전개시험(Controlled Deployment Testing)은 소프트웨어에서 사용하는 물리적 가정을 검증한다. 시험을 통해 사출 지연(Extraction Delay), 팽창시간(Inflation Time), 개방 충격하중(Opening Load), 하강률(Descent Rate), 구조 응답(Structural Response), 센서 신호 특성, 작동 명령에서 실제 전개까지의 시간(Trigger-to-Deployment Timing)을 대표적인 구성별로 평가한다. 비행시험은 검증된 전개영역을 단계적이고 보수적으로 확장한다. 소프트웨어 임계값은 낙관적인 이론적 성능이 아니라 실제로 입증된 시스템 동작을 기반으로 설정된다.

오작동 전개 시험(False-Trigger Testing)은 성공적인 전개시험과 동일한 수준의 중요성을 가지고 수행된다. 난기류(Turbulence), 허용 범위 내의 공격적인 기동, 센서 과도현상(Sensor Transient), 일시적인 통신 손실, 프로세서 재시작, 추진 외란(Propulsion Disturbance)을 시험하여 정상적으로 복구 가능한 상황에서 비상 시스템이 작동하지 않는지를 검증한다. 목표는 실제 제어 상실에 대한 높은 탐지 민감도(Detection Sensitivity)를 유지하면서 불필요한 낙하산 전개에 대한 강한 내성(Resistance to Nuisance Deployment)을 확보하는 것이다.

완성된 안전 및 낙하산 작동 아키텍처(Safety and Parachute-Trigger Architecture)는 정상 제어와 이중화 시스템이 더 이상 허용 가능한 결과를 보장할 수 없을 때 최종적인 독립 보호 계층을 제공한다. 그 효과는 신뢰할 수 있는 위험 탐지, 검증된 전개 경계, 보호된 작동 절차, 추진 및 에너지 시스템과의 협조된 동작, 결정론적 비상 순서(Deterministic Emergency Sequencing)에 의해 결정된다. 2.5톤 화물 무인항공기에서 이 시스템은 그 자체가 통제되지 않은 비상 메커니즘이 되지 않으면서 지상 및 항공기 전체의 위험을 감소시킬 수 있도록 설계되어야 한다.

## 10.09. 2.5t UAV System Integration Test Protocol

![](images/image10.png){width="7.268055555555556in" height="7.268055555555556in"}

2.5톤 화물 무인항공기(Cargo UAV)의 시스템 통합시험(System Integration Testing)은 독립적으로 개발된 항공전자(Avionics), 추진(Propulsion), 항법(Navigation), 자율 시스템(Autonomy), 통신(Communication), 화물(Cargo), 전력(Power), 안전(Safety) 하위 시스템이 하나의 항공기로서 올바르게 동작하는지를 검증한다. 그 목적은 개별 기능의 정상 동작을 확인하는 수준을 넘어선다. 정상, 성능 저하 및 비상 운용 조건 전반에서 인터페이스, 타이밍, 고장 대응, 권한 경계(Authority Boundary), 구성 의존성(Configuration Dependency)이 예측 가능한 상태로 유지되는지를 입증해야 한다.

통합 프로그램(Integration Program)은 하드웨어 버전, 소프트웨어 빌드(Software Build), 파라미터 세트(Parameter Set), 배선 구성(Wiring Configuration), 네트워크 토폴로지(Network Topology), 센서 보정값(Sensor Calibration), 탑재화물 구성(Payload Configuration), 시험장비를 정의하는 통제된 기준선(Controlled Baseline)에서 시작한다. 모든 시험 결과는 이 기준선과 연계되어 고장을 재현할 수 있어야 한다. 사소한 펌웨어 또는 보정값 차이도 이전 통합시험에서 얻은 결론을 무효화할 수 있으므로 통제되지 않은 구성 변경은 허용하지 않는다.

요구사항 추적성(Requirements Traceability)은 각각의 통합시험을 시스템 요구사항(System Requirement), 인터페이스 요구사항(Interface Requirement), 위험요소(Hazard), 검증 목표(Verification Objective)와 연결한다. 시험 사례(Test Case)는 필요한 초기 조건, 입력 자극(Stimulus), 예상 응답, 허용오차(Tolerance), 시간 제한, 합격 기준(Pass Criteria), 기록해야 할 증거를 정의한다. 이러한 추적성은 시험이 단순한 시연의 집합으로 변하는 것을 방지하고 항공기가 점진적으로 더 높은 위험의 활동에 승인되기 전에 안전 필수 상호작용(Safety-Critical Interaction)이 명확하게 검증되도록 한다.

복잡한 임무 시나리오를 수행하기 전에 인터페이스 검증(Interface Verification)을 실시한다. 연결된 하위 시스템 사이의 전기적 레벨(Electrical Level), 통신 프로토콜, 메시지 식별자(Message Identifier), 단위(Unit), 좌표계(Coordinate Frame), 부호 규약(Sign Convention), 스케일링(Scaling), 갱신 주기(Update Rate), 타임아웃 동작(Timeout Behavior), 초기화 순서(Initialization Sequence)를 확인한다. 특히 자율 시스템에서 FCC로 전달되는 명령, 화물 구성 입력, 추진 상태 및 비상 안전 신호와 같이 서로 다른 보증 영역(Assurance Boundary)을 통과하는 인터페이스를 중점적으로 검증한다.

시간 동기화(Time Synchronization)는 개별 구성품 사양만으로 충족된다고 가정하지 않고 시스템 수준 기능(System-Level Function)으로 검증한다. 센서 타임스탬프, FCC 시간, 자율 컴퓨터 시간, 원격측정 기록(Telemetry Record), 지상통제소 로그를 공통 기준시간(Common Time Reference)과 비교한다. 정밀시간 프로토콜(Precision Time Protocol, PTP), 일반화 정밀시간 프로토콜(gPTP), 초당 펄스(Pulse Per Second, PPS), 하드웨어 타임스탬프(Hardware Timestamp) 또는 이에 상응하는 메커니즘을 시동, 재시작, 네트워크 혼잡, 기준시간원 손실 조건에서 시험하여 제한된 동기화 오차와 예측 가능한 복구 특성을 확인한다.

전력 통합시험(Power Integration Testing)은 시동 순서, 정상상태 소비전력(Steady-State Consumption), 과도 전력 요구(Transient Demand), 브라운아웃(Brownout) 동작, 백업 전원(Backup Supply), 절연장치(Isolation Device), 종료 순서를 평가한다. 고출력 추진 부하는 전기적 분리가 불충분할 경우 항공전자 장비에 영향을 주는 외란을 발생시킬 수 있다. 따라서 대표적인 모터 과도상태(Motor Transient)를 재현하면서 비행컴퓨터, 센서, 통신장비, 화물 제어기 및 독립 안전 전자장치에서 재설정(Reset), 데이터 손상 또는 타이밍 이상이 발생하는지를 감시한다.

네트워크 통합시험(Network Integration Testing)은 실제 운용 및 최악조건의 부하에서 필수 트래픽(Critical Traffic)이 유지되는지를 검증한다. 지휘통제(Command and Control), 항법 데이터, 구동기 명령, 원격측정, 영상, 진단, 유지보수 트래픽을 동시에 동작시킨다. 패킷 손실(Packet Loss), 혼잡(Congestion), 지연 메시지, 중복 패킷, 네트워크 노드 고장을 의도적으로 발생시킨다. 낮은 우선순위의 트래픽이 가용 대역폭 한계에 접근하더라도 우선순위 및 서비스 품질(Quality of Service) 메커니즘은 비행 필수 통신을 유지해야 한다.

센서 통합시험(Sensor Integration Testing)은 센서 정확도뿐만 아니라 장착에 따른 영향(Installation Effect)도 검증한다. 관성측정장치(Inertial Measurement Unit, IMU), 위성항법시스템(GNSS) 수신기, 대기자료 센서(Air-Data Sensor), 자기계(Magnetometer), 고도계(Altimeter), 카메라, 라이다(LiDAR), 레이더(Radar), 하중 센서(Load Sensor)를 정렬, 지연시간, 진동 민감도, 전자기 간섭(Electromagnetic Interference), 가림(Obstruction), 열적 거동(Thermal Behavior)의 관점에서 평가한다. 항법 시스템이 실제 기체 동역학과 장착으로 인해 발생한 측정오차를 구분할 수 있도록 센서 간 일관성도 평가한다.

구동기 및 추진 통합(Actuator and Propulsion Integration)은 실제 힘을 발생시키는 시험으로 진행하기 전에 작동 금지 상태 또는 무부하 상태에서 시작한다. 모터 제어기, 전자식 속도제어기(Electronic Speed Controller, ESC), 서보(Servo), 틸트 메커니즘(Tilt Mechanism), 브레이크(Brake), 기타 효과기(Effector)를 통제된 범위에서 명령하면서 피드백을 감시한다. 폐루프 항공기 제어(Closed-Loop Aircraft Control)를 허용하기 전에 작동 방향, 스케일링, 변화율 제한(Rate Limit), 포화 동작(Saturation Behavior), 명령 타임아웃, 고장 보고, 안전상태 응답(Safe-State Response)을 확인한다.

비행제어컴퓨터(Flight Control Computer)는 시뮬레이션 및 실제 하위 시스템과 단계적으로 통합된다. 초기 시험에서는 결정론적인 센서 입력(Deterministic Sensor Input)과 시뮬레이션 구동기를 사용하여 제어 모드 및 인터페이스 동작을 검증한다. 이후 실제 센서와 효과기를 점진적으로 추가한다. 이러한 단계적 접근방법(Staged Approach)은 고장을 효율적으로 격리하고 새롭게 연결된 하나의 하위 시스템 결함이 항공기의 다른 영역에서 발생하는 무관한 동작과 혼동되는 것을 방지한다.

소프트웨어 인더 루프 시험(Software-in-the-Loop Testing, SIL)은 하드웨어 제약조건이 고장 진단을 복잡하게 만들기 전에 비행제어, 항법, 자율 시스템, 임무 및 고장관리 소프트웨어를 비선형 항공기 모델(Nonlinear Aircraft Model)에 연결하여 시험한다. 자동화된 시나리오는 이륙, 천이(Transition), 순항, 접근, 착륙, 화물 운용, 경로 변경 및 비정상 상황을 포함한다. 반복 가능성(Repeatability)을 통해 안정적인 시나리오 라이브러리를 기준으로 소프트웨어 변경 결과를 비교하고 연속적인 릴리스에 대한 회귀시험(Regression Testing)을 수행할 수 있다.

하드웨어 인더 루프 시험(Hardware-in-the-Loop Testing, HIL)은 양산 대표 수준(Production-Representative)의 비행컴퓨터 및 인터페이스를 실시간 항공기 시뮬레이션(Real-Time Aircraft Simulation)에 연결한다. 시뮬레이터는 실제 제어 출력을 수신하면서 센서 동작과 항공기 동역학을 생성한다. 프로세서 부하, 버스 타이밍, 네트워크 지연, 센서 고장, 추진 고장 및 통신 손실을 실제 항공기를 위험에 노출시키지 않고 재현할 수 있다. HIL 시험은 특히 정밀한 타이밍에 의존하는 상호작용을 검증하는 데 효과적이다.

통합 지상시험장치(Integrated Ground Rig) 또는 아이언 버드(Iron-Bird) 환경은 대표적인 전력분배 시스템, 구동기, 추진 제어기, 통신장비, 화물 메커니즘 및 안전 하드웨어를 포함하여 HIL 시험을 확장한다. 물리적 인터페이스를 통해 순수 시뮬레이션에서는 발견하기 어려운 배선, 접지, 전자기, 기계적 및 타이밍 문제를 식별할 수 있다. 이 시험장치는 고장 주입(Fault Injection), 소프트웨어 회귀시험, 유지보수 검증 및 비행시험 이상현상 조사에 반복적으로 사용할 수 있는 플랫폼이 된다.

모드 전환 시험(Mode-Transition Testing)은 지상(Ground), 무장(Armed), 이륙, 제자리비행(Hover), 천이, 순항, 접근, 착륙, 화물 운용, 성능 저하(Degraded), 비상(Emergency) 상태 사이의 동작을 검증한다. 각각의 전환에 대해 올바른 선행조건(Precondition), 명령 연속성(Command Continuity), 제어기 초기화, 구동기 동작, 상태 표시(Annunciation)를 확인한다. 운용 고장으로 정상 임무에서는 거의 발생하지 않는 전환이 강제될 수 있으므로 빠르거나 예상하지 못한 상태 전환 순서도 시험한다.

고장 주입시험(Fault-Injection Testing)은 탐지, 격리, 재구성(Reconfiguration), 복구 기능을 검증하기 위해 의도적으로 고장을 발생시킨다. 시나리오에는 센서 데이터 손실, 고정된 센서 값(Frozen Value), 편향된 측정값(Biased Measurement), 프로세서 재시작, 모터 손실, ESC 고장, 네트워크 단절, 전력버스 성능 저하, GNSS 손실, 화물 센서 불일치, GCS 연결 손실 등이 포함될 수 있다. 시스템은 정의된 안전 로직에 따라 대응해야 하며 하나의 주입된 고장이 통제되지 않은 동작으로 확산되어서는 안 된다.

단일 고장에 대한 동작을 충분히 이해한 이후에는 다중 고장 시나리오(Multiple-Fault Scenario)를 적용한다. 실제 항공기에서는 전력 외란 이후 센서 재설정이 발생하거나 추진 성능 저하와 통신 손실이 동시에 발생하는 것처럼 종속적 또는 연쇄적인 고장(Dependent or Cascading Failure)이 발생할 수 있다. 모든 임의 조합을 시험하는 대신 위험 분석에서 식별된 조합을 중심으로 시험한다. 목표는 공유 자원 또는 공통 의존성이 스트레스를 받는 상황에서도 이중화 가정(Redundancy Assumption)이 유효한지를 검증하는 것이다.

항법 및 자율 시스템 통합시험(Navigation and Autonomy Integration Testing)은 추정 상태(Estimated State), 인지(Perception), 경로 계획(Route Planning), 궤적 생성(Trajectory Generation), FCC 명령 수락이 일관된 하나의 처리 체계를 구성하는지를 확인한다. 좌표계, 고도 기준(Altitude Reference), 지오펜스(Geofence), 장애물 데이터, 항법 불확실성, 명령 유효성을 함께 시험한다. FCC는 오래되었거나 형식이 잘못되었거나 활성 비행영역을 벗어나거나 성능 저하 상태의 기체 능력과 호환되지 않는 자율 시스템 요청을 거부해야 한다.

화물 통합시험(Cargo Integration Testing)은 탑재화물 질량, 무게중심(Center of Gravity), 구속 상태(Restraint State), 도어 상태, 투하 또는 하역 기능이 항공기 구성에 정확하게 반영되는지를 검증한다. 잘못된 화물 데이터, 잠금장치 고장, 하중 이동, 센서 불일치를 주입하여 운항 차단(Dispatch Inhibition) 또는 비행 중 대응을 확인한다. 화물 메커니즘은 금지된 비행 상태에서 작동해서는 안 되며 화물 시스템의 고장이 비행 필수 계산 또는 통신 자원을 손상시켜서는 안 된다.

GCS 및 원격측정 통합시험(GCS and Telemetry Integration Testing)은 명령 권한, 응답(Acknowledgment), 메시지 최신성(Message Freshness), 경고 표시, 링크 전환(Link Switching), 통신두절 동작(Loss-of-Link Behavior)을 검증한다. 항공기 시스템이 대표적인 임무를 수행하는 동안 네트워크 지연, 패킷 손실, 대역폭 제한 및 완전한 연결 단절을 발생시킨다. 통신 복구도 시험하여 운용자가 다시 제어권을 행사하기 전에 오래된 명령이 폐기되고 현재 자율 상태가 동기화되는지를 확인한다.

비상 시스템 통합(Emergency-System Integration)은 독립 안전 감시(Independent Safety Monitoring), 무장 로직(Arming Logic), 작동 금지(Trigger Inhibition), 비상전원, 추진 시스템 협조, 설치된 경우 낙하산 인터페이스(Parachute Interface)를 검증한다. 시험에서는 복구 가능한 외란이 잘못된 비상 작동(False Activation)을 발생시키지 않으면서 실제 복구 불가능한 상태에서는 가용 시간 내에 필요한 대응이 발생하는지를 확인한다. 비가역 장치(Irreversible Device)는 반복시험 과정에서 계측된 시뮬레이터(Instrumented Simulator)로 대체한 후 통제된 실제 전개시험으로 진행할 수 있다.

전자기 적합성 시험(Electromagnetic Compatibility Testing)은 고전류 추진 스위칭, 무선장치, 전력변환기, 구동기 및 디지털 전자장치가 항법 센서, 비행컴퓨터 또는 통신 링크에 간섭을 발생시키는지를 평가한다. 여러 시스템이 동시에 작동할 때만 간섭이 나타날 수 있으므로 대표적인 운용 조합을 구성하여 시험한다. 내성(Susceptibility) 및 방출(Emission) 시험을 기능 감시와 함께 수행하여 순간적인 데이터 손상이 간과되지 않도록 한다.

열 통합시험(Thermal Integration Testing)은 대표적인 프로세서, 추진 시스템, 배터리, 통신장비 및 환경 부하에서 완성된 항공기를 평가한다. 냉각 시스템(Cooling System)은 제자리비행, 순항, 지상 운용 및 저풍량 조건(Low-Airflow Condition)에서 시험된다. 온도 제한, 성능 제한 동작(Throttling Behavior), 센서 드리프트(Sensor Drift), 종료 임계값을 감시한다. 가능한 경우 열 고장을 주입하여 하드웨어가 위험한 운용온도에 도달하기 전에 성능 저하 모드로 전환되는지를 확인한다.

구조 및 진동 통합시험(Structural and Vibration Integration Testing)은 장착된 전자장치, 커넥터, 센서, 안테나, 화물 인터페이스 및 배선이 대표적인 하중 조건에서 정상 기능을 유지하는지를 검증한다. 개별 구성품이 적격성 시험(Qualification Test)을 통과했더라도 추진 시스템에서 발생하는 진동은 IMU, 커넥터, 카메라 및 기계식 잠금장치에 영향을 줄 수 있다. 통합 측정을 통해 완성된 기체에 장착한 이후에만 나타나는 공진(Resonance), 간헐적 연결(Intermittent Connection), 센서 이상 신호를 식별한다.

지상 가동시험(Ground-Run Testing)은 의도하지 않은 비행을 방지하도록 기체를 물리적으로 구속한 상태에서 조립 완료된 항공기의 시스템을 단계적으로 활성화한다. 추진 시스템을 낮은 출력에서 대표적인 운용조건까지 점진적으로 작동시키면서 제어 명령, 진동, 전력 품질(Power Quality), 온도, 네트워크 동작 및 안전 인터록을 감시한다. 항공기를 계류비행(Tethered Flight) 또는 자유비행(Free Flight) 시험으로 전환하기 전에 비정상 종료, 비상정지(Emergency Stop), 통신 손실 및 전원 전환을 시험한다.

비행시험(Flight Testing)은 보수적인 비행영역(Conservative Envelope)과 단순한 시험 목표에서 시작하며 광범위한 계측을 수행한다. 초기 비행에서는 기본 안정화, 항법, 추진 협조, 원격측정 및 착륙 동작을 검증한다. 이후 이전 단계의 데이터가 합격 기준을 만족한 경우에만 속도, 고도, 탑재하중, 무게중심, 바람, 천이 및 기동 조건을 확대한다. 따라서 비행영역 확장(Envelope Expansion)은 일정 중심이 아니라 검증 증거 중심(Evidence-Driven)으로 수행된다.

검증된 동작에 영향을 줄 가능성이 있는 소프트웨어, 하드웨어, 보정값 또는 구성 변경 이후에는 회귀시험(Regression Testing)을 수행한다. 필요한 회귀시험 범위는 영향 분석(Impact Analysis)을 통해 결정하지만 핵심 인터페이스, 시동, 제어, 고장관리 및 안전시험은 필수 기준선(Mandatory Baseline)의 일부로 유지한다. 자동화된 SIL 및 HIL 시험군(Test Suite)을 통해 수정된 소프트웨어가 지상시험 또는 비행시험으로 이동하기 전에 의도하지 않은 영향을 신속하게 탐지할 수 있다.

시험 데이터 관리(Test Data Management)는 동기화된 센서 스트림, 명령, 구동기 응답, 네트워크 트래픽, 하위 시스템 상태, 구성 식별정보, 운용자 조작 및 환경조건을 보존한다. 자동 분석(Automated Analysis)은 측정된 응답을 예상 제한값과 비교하고 타이밍 위반, 예상하지 못한 모드 변경 또는 성능 추세를 식별한다. 원시 데이터(Raw Data)는 결론을 독립적으로 검토하고 위험한 시험을 반복하지 않고도 새로운 분석을 수행할 수 있도록 보존한다.

통합시험 중 발견된 이상현상(Anomaly)은 기록되고 분류되며 가능한 경우 재현한 후 시정조치(Corrective Action)까지 추적한다. 설명되지 않은 동작이 존재함에도 임무가 완료되었다는 이유만으로 시험을 성공한 것으로 판단해서는 안 된다. 안전에 중요한 이상현상은 근본원인 분석(Root-Cause Analysis)과 적절한 회귀시험을 요구한다. 수용하기로 결정된 알려진 제한사항(Known Limitation)은 시험 기록 내부에 숨겨두는 것이 아니라 명확한 운용 제약조건(Operational Constraint)과 함께 문서화한다.

최종 통합시험 캠페인(Final Integration Test Campaign)은 2.5톤 화물 무인항공기가 승인된 운용영역 전체에서 하나의 일관된 시스템(Coherent System)으로 동작한다는 것을 입증한다. 성공적인 완료를 위해서는 하위 시스템 인터페이스, 타이밍, 전력, 네트워킹, 제어, 자율 시스템, 화물 기능, 통신 및 비상 보호 기능이 현실적인 고장과 환경 스트레스에서도 상호 협조된 상태를 유지한다는 증거가 필요하다. 따라서 시스템 통합시험은 개별 구성품 검증(Component Verification)과 완성된 항공기의 안전한 운용(Safe Operation)을 연결하는 핵심 검증 근거를 제공한다.

## 10.10. 2.5t UAV Flight Test Campaign Case

![](images/image11.png){width="7.268055555555556in" height="7.268055555555556in"}

2.5톤 화물 무인항공기(Cargo UAV)의 비행시험 캠페인(Flight-Test Campaign)은 통합된 항공기가 검증된 안전 경계(Safety Boundary) 내에서 의도된 물류 임무를 수행할 수 있음을 최종적으로 단계별 입증하는 과정이다. 캠페인은 처음부터 전 범위 자율 화물 운용(Full-Range Autonomous Cargo Operation)을 수행하는 방식으로 시작하지 않는다. 공력 특성, 추진 성능, 비행제어, 항법, 자율 시스템, 통신, 탑재화물 영향 및 비상 기능을 함께 평가하는 통제된 시험 단계를 통해 항공기 운용 능력을 점진적으로 확장한다.

첫 비행(First Flight) 전에 시험 조직은 하드웨어, 소프트웨어, 제어 파라미터(Control Parameter), 추진장치, 배터리 또는 에너지 시스템, 센서, 통신장비, 화물 인터페이스 및 안전장치를 포함하는 구성 통제된 항공기 기준선(Configuration-Controlled Aircraft Baseline)을 설정한다. 모든 비행은 고유한 구성 기록(Configuration Record)과 연계된다. 외견상 사소한 변경도 진동, 전자기 적합성(Electromagnetic Compatibility), 질량 특성(Mass Properties), 열적 거동(Thermal Behavior) 또는 제어 응답에 영향을 줄 수 있으므로 비행 사이에 이루어진 변경 사항을 검토한다.

비행 준비성(Flight Readiness)은 분석, 시뮬레이션, 소프트웨어 인더 루프(Software-in-the-Loop, SIL), 하드웨어 인더 루프(Hardware-in-the-Loop, HIL), 통합 지상시험장치(Integrated Ground Rig), 구조시험, 추진시험 및 구속 지상 가동시험(Restrained Ground Run)을 통해 축적된 증거를 기반으로 판단한다. 비행 승인 전에 미해결 이상현상(Open Anomaly)을 검토한다. 핵심 기능이 충분한 성숙도를 입증하고 초기 비행에서 계획된 제한적 비행영역에 대해 신뢰할 수 있는 비상 절차가 확보된 경우에만 첫 비행을 허용한다.

비행시험 구역(Flight-Test Area)은 사람, 재산 및 관련 없는 항공교통에 대한 위험을 줄일 수 있도록 선정한다. 지형, 장애물 회피거리, 통신 커버리지, 비상 착륙구역(Emergency Landing Area), 일반적인 바람 조건 및 회수 접근성(Recovery Access)을 고려한다. 초기 운용에서는 보수적인 지리적 경계와 고도 제한을 적용한다. 항법 또는 계획 오류로 승인된 시험공간이 의도하지 않게 확대되지 않도록 지오펜스(Geofence)와 임무 제약조건을 독립적으로 확인한다.

시험팀은 비행시험 책임자(Flight-Test Director), 원격 조종사 또는 운용자, 안전 담당자, 원격측정 엔지니어(Telemetry Engineer), 추진 전문가, 비행제어 엔지니어 및 회수팀(Recovery Team)의 책임을 명확하게 정의한다. 출발, 시험 지속, 시험점(Test Point) 수행, 중단(Abort), 비상 종료(Emergency Termination)에 대한 의사결정 권한을 비행 전에 설정한다. 이를 통해 빠르게 변화하는 항공기 상태에서 즉각적인 대응이 필요할 때 명령 책임이 모호해지는 것을 방지한다.

초기 비행은 임무 성능보다 기체의 기본 동작(Fundamental Vehicle Behavior)에 초점을 맞춘다. 항공기는 추진 안정성, 기본 자세제어, 고도 제어, 저속 기동, 원격측정, 항법 일관성, 명령 응답 및 통제된 착륙을 입증한다. 시험점은 의도적으로 분리하여 예상하지 못한 동작이 복잡한 자율 임무 내부에 숨겨지는 대신 제한된 변수 집합과 연계하여 원인을 분석할 수 있도록 한다.

수직이착륙(Vertical Takeoff and Landing, VTOL) 화물 기체의 경우 제자리비행 시험(Hover Testing)은 중요한 초기 특성평가 단계가 된다. 엔지니어는 필요한 추력, 모터 부하, 전력 소비, 열적 거동, 진동, 자세제어 여유도(Attitude-Control Margin), 바람 민감도를 측정한다. 서로 다른 이륙질량과 무게중심(Center of Gravity) 위치에서 제자리비행 성능을 반복 측정한다. 이러한 데이터는 추진 모델을 검증하고 더 역동적인 비행을 수행하기 전에 충분한 제어 여유도가 존재하는지를 확인하는 데 사용된다.

저속 기동시험(Low-Speed Maneuver Testing)은 롤(Roll), 피치(Pitch), 요(Yaw), 병진운동(Translation), 상승 및 하강 명령을 점진적으로 확대한다. 명령된 응답과 실제 측정 응답을 시뮬레이션 예측 및 제어법칙(Control-Law) 요구사항과 비교한다. 오버슈트(Overshoot), 감쇠(Damping), 구동기 사용률(Actuator Utilization), 모터 포화(Motor Saturation), 구조 진동을 감시한다. 캠페인 중 제어 게인을 임의로 변경하지 않으며 변경이 필요한 경우 분석, 구성관리, 회귀시험(Regression Testing), 새로운 비행 준비성 검토를 수행한다.

항공기가 리프트-플러스-크루즈(Lift-Plus-Cruise), 틸트로터(Tilt-Rotor), 틸트윙(Tilt-Wing) 또는 제자리비행과 전진비행의 동역학 특성이 크게 다른 다른 형식을 사용하는 경우 천이시험(Transition Testing)을 점진적으로 수행한다. 각각의 시험점에서 대기속도 또는 기체 구성 변경량을 작은 단계로 증가시킨다. 천이 비행영역(Transition Corridor)을 확대하기 전에 제어 할당(Control Allocation), 게인 스케줄링(Gain Scheduling), 추진력 전환(Propulsion Transfer), 공력 조종면 제어 권한(Aerodynamic-Surface Authority), 에너지 소비에서 불연속성이 발생하는지를 평가한다.

전진비행 비행영역 확장(Forward-Flight Envelope Expansion)은 대기속도, 뱅크각(Bank Angle), 상승률, 하강률, 고도 및 기동 요구량을 점진적으로 증가시키며 평가한다. 각각의 새로운 경계조건은 이전에 검증된 상태에서 접근한다. 동압(Dynamic Pressure), 구조 하중, 추진 여유도(Propulsion Reserve), 구동기 여유도, 진동 및 제어 안정성을 실시간으로 감시한다. 측정 추세에서 검증된 여유도가 예측보다 빠르게 감소하는 것으로 나타나면 이론적 한계에 도달하기 전에 시험점을 종료한다.

항법시험(Navigation Testing)은 탑재 추정값을 독립적인 기준값과 비교하고 대표적인 위성 배치, 기체 운동 및 환경조건에서 성능을 평가한다. 위치, 속도, 고도, 자세, 시간 및 불확실성 거동(Uncertainty Behavior)을 분석한다. 정상 성능을 먼저 검증한 후 통제된 위성항법시스템(GNSS) 성능 저하 또는 보조 정보원(Aiding Source) 변경을 적용하여 다른 고위험 기동과 결합하지 않고 대체 항법(Fallback Navigation)을 검증할 수 있다.

자율 시스템 시험(Autonomy Testing)은 단순 경로에 대한 감독 운용(Supervised Execution)에서 시작하여 완전한 임무 순서로 발전한다. 웨이포인트 추종, 궤적 생성(Trajectory Generation), 지오펜스 처리, 대기(Holding), 경로 재설정(Rerouting), 접근, 착륙 및 비상대응 로직(Contingency Logic)을 개별적으로 시험한 후 통합한다. 비행제어컴퓨터(Flight Control Computer, FCC)는 비행영역 보호(Envelope Protection)를 계속 담당하며 오래되거나 형식이 잘못되었거나 현재 항공기 능력과 물리적으로 호환되지 않는 자율 시스템의 요청을 거부할 수 있다.

탑재하중 확장(Payload Expansion)은 무화물 상태에서 최대 화물 상태로 즉시 이동하는 대신 체계적인 질량 및 무게중심 시험 매트릭스(Mass and Center-of-Gravity Matrix)를 따른다. 밸러스트(Ballast) 또는 대표 탑재화물을 통제된 단계로 추가한다. 각 구성에서 제자리비행 출력, 트림(Trim), 기동 응답, 구조 하중, 에너지 소비, 착륙 동작 및 제어 여유도를 측정한다. 정상 범위에서 벗어나지만 승인된 무게중심 위치는 구동기 제어 권한 감소를 드러낼 수 있으므로 별도의 평가를 수행한다.

화물 인터페이스 비행시험(Cargo-Interface Flight Testing)은 구속 센서(Restraint Sensor), 도어, 하중 감시 및 구성 데이터가 진동과 기동 하중에서도 신뢰성을 유지하는지를 확인한다. 화물 투하 또는 자동 하역(Automated Unloading)이 설계에 포함된 경우 기본적인 항공기 안정성을 입증한 이후에만 해당 기능을 시험한다. 승인된 화물 투하 이후 발생하는 질량 특성 변화는 예측된 과도 응답(Transient Response) 및 갱신된 비행제어 구성과 비교한다.

에너지 성능시험(Energy-Performance Testing)은 실제적인 탑재하중, 경로, 고도, 속도, 바람 및 온도 조건에서 임무 지속시간(Mission Endurance)을 특성화한다. 임무 전 과정에서 측정된 전력 요구량을 비행 전 예측값과 비교한다. 통제된 에너지 임계값에서 복귀, 우회 또는 착륙 결정을 시작하여 예비 에너지 정책(Reserve Policy)을 시험한다. 목표는 최대 체공시간만을 검증하는 것이 아니라 자율 임무계획에 사용되는 탑재 에너지 예측의 정확성도 검증하는 것이다.

통신시험(Communication Trial)은 목표 운용지역 전체에서 지휘통제(Command and Control, C2) 및 원격측정 성능을 평가한다. 비행은 안전한 시험조건을 유지하면서 알려진 통신 취약지역에 의도적으로 접근한다. 링크 지연시간, 패킷 손실(Packet Loss), 핸드오버(Handover) 동작, 대역폭 감소 및 이중화 링크 전환(Redundant-Link Switching)을 기록한다. 이후의 비행에서는 통제된 통신두절(Loss-of-Link) 상황을 적용하여 지속적인 지상 명령에 의존하지 않고 자율 대기, 복귀, 우회 또는 착륙 동작을 검증한다.

환경 비행영역 확장(Environmental Envelope Expansion)은 평온한 기상에서의 동작을 검증한 후 점진적으로 더 강한 바람, 온도 변화, 난기류(Turbulence) 및 기타 승인된 환경조건을 적용한다. 환경시험은 단순히 항공기가 공중에 머무를 수 있음을 입증하기 위한 것이 아니다. 엔지니어는 제어 부담(Control Workload), 에너지 소비, 항법 성능, 열적 여유도(Thermal Margin), 구조 응답, 착륙 정확도 및 자율 시스템이 운용 한계에 접근하는 조건을 올바르게 인식하는지를 평가한다.

비행 중 고장시험(Failure Testing)은 허용 가능한 잔여 위험(Residual Risk)으로 주입할 수 있는 고장으로 제한한다. 예를 들어 통제된 센서 제거, 통신 중단, 시뮬레이션된 하위 시스템 고장 또는 충분한 이중성이 존재하는 경우 명령에 의한 추진 성능 저하를 시험할 수 있다. 더 위험한 고장은 주로 시뮬레이션 및 HIL 환경에서 시험한다. 비행시험은 모든 치명적 고장을 물리적으로 재현하기 위한 것이 아니라 모델 가정과 시스템 수준 대응을 검증하는 데 사용한다.

추진 성능 저하 시험(Propulsion-Degradation Trial)은 항공기가 감소된 추진 능력을 인식하고 그에 맞게 동작을 변경하는지를 검증한다. 기체 구성에 따라 모터 출력 감소, 추진 채널 사용 불가 또는 제한된 추력 여유도(Thrust Margin)를 평가할 수 있다. 항공기는 정상 능력이 유지된다는 가정으로 계속 운항하는 대신 기동 한계, 궤적 요구량, 상승 성능 또는 착륙 전략을 조정해야 한다.

비상 및 비정상 대응 시험(Emergency and Contingency Testing)은 시뮬레이션된 상태 표시에서 신중하게 통제된 실제 시연으로 발전한다. 기지복귀(Return-to-Base), 대체 착륙(Alternate Landing), 비상 하강, 비행모드 복귀(Flight-Mode Reversion), 안전 제어기 개입(Safety-Controller Intervention) 및 기타 절차를 개별적으로 평가한다. 낙하산 복구 시스템(Parachute Recovery System)이 설치된 경우 실제 통제 전개시험을 승인하기 전에 시뮬레이션과 계측 대체장치(Instrumented Substitute)를 이용하여 작동 로직과 전개 순서를 충분히 검증한다.

대형 화물 무인항공기는 위치 정확도와 구조 한계를 유지하면서 상당한 에너지를 소산해야 하므로 착륙시험(Landing Testing)에 특별한 주의를 기울인다. 서로 다른 질량, 무게중심, 바람 및 항법 조건에서 접근을 평가한다. 하강률, 접지속도(Touchdown Velocity), 착륙장치 하중, 추진 응답, 위치 오차 및 복행 로직(Go-Around Logic)을 측정한다. 완전 자율 운용을 승인하기 전에 착륙 중단(Rejected Landing)과 실패 접근(Missed Approach)을 시험한다.

통합 화물 임무(Integrated Cargo Mission)는 이전에 검증된 기능을 대표적인 물류 시나리오로 결합한다. 일반적인 시연에는 적재 검증, 자동 비행 전 점검(Automated Preflight Check), 이륙, 출발, 순항, 경로 변경, 도착, 정밀 접근(Precision Approach), 착륙, 하역 및 복귀가 포함된다. 목적은 단순히 경로를 완주하는 것이 아니라 임무 전체에서 상태 전환, 안전 인터록(Safety Interlock), 에너지 여유도, 통신 및 화물 구성이 일관된 상태를 유지하는지를 입증하는 것이다.

원격측정(Telemetry)을 통해 시험팀은 각 비행 중 핵심 파라미터를 감시할 수 있다. 실시간 화면은 비행 모드, 항법 무결성(Navigation Integrity), 제어 여유도, 추진 예비능력, 에너지 상태, 구조 상태 지표, 통신 품질 및 활성 고장을 중점적으로 표시한다. 사전에 정의된 시험 중단 기준(Stop Criteria)은 언제 시험점을 포기해야 하는지를 규정한다. 항공기가 절대적인 한계를 아직 초과하지 않았다는 이유만으로 여유도가 악화되는 추세에서도 기동을 계속해서는 안 된다.

각 비행 이후에는 다음 비행영역 확장 단계로 이동하기 전에 체계적인 데이터 검토(Structured Data Review)를 수행한다. 기록된 센서 스트림, 제어기 상태, 구동기 명령, 추진 데이터, 네트워크 트래픽, 구조 측정값, 자율 시스템 의사결정 및 운용자 조작을 동기화하여 분석한다. 예측 동작과 측정 동작의 차이를 분류한다. 설명되지 않은 중요한 편차가 존재하면 그 원인과 안전 영향을 충분히 이해할 때까지 관련 시험영역의 확대를 중단한다.

이상현상(Anomaly)은 비행 현장에서 비공식적으로 조정하는 대신 정식 절차를 통해 관리한다. 각각의 중요한 사건을 문서화하고 가능한 경우 시뮬레이션 또는 지상시험에서 재현하며 근본원인 조사(Root-Cause Investigation)와 시정조치(Corrective Action)에 연계한다. 소프트웨어 또는 파라미터 변경은 다시 비행시험에 적용하기 전에 회귀시험을 수행한다. 이러한 절차는 즉흥적인 튜닝(Rapid Tuning)이 더 근본적인 통합 또는 모델링 문제를 감추는 것을 방지한다.

비행영역 확장은 요구되는 질량, 무게중심, 속도, 고도, 기동, 환경 및 하위 시스템 가용성의 조합에 대해 충분한 검증 증거가 확보된 경우에만 완료된다. 시험 결과가 추가 영역의 사용을 정당화하지 못하는 경우 승인된 비행영역(Approved Envelope)은 이론적인 설계 비행영역보다 작게 유지될 수 있다. 따라서 운용 제한조건(Operational Limitation)은 최초에 목표로 설정한 사양을 달성하지 못한 실패가 아니라 유효한 공학적 결과로 취급한다.

최종 캠페인 단계(Final Campaign Phase)는 반복성(Repeatability)과 운용 신뢰성(Operational Reliability)에 중점을 둔다. 양산 의도 하드웨어(Production-Intent Hardware), 소프트웨어, 유지보수 절차, 지상장비 및 훈련된 운용자를 사용하여 대표적인 임무를 반복적으로 수행한다. 운항 신뢰도(Dispatch Reliability), 임무 전환시간(Turnaround Time), 고장 재발, 에너지 예측, 착륙 일관성 및 유지보수 결과를 추적한다. 항공기는 단 한 번의 성공적인 시연 비행이 아니라 반복된 임무 전반에서 안정적인 동작을 입증해야 한다.

비행시험 캠페인의 완료는 2.5톤 화물 무인항공기의 운용 구성(Operational Configuration)과 검증된 제한조건(Verified Limitation)을 정의하는 데 필요한 증거를 제공한다. 확보된 데이터는 해석 모델(Analytical Model) 및 실험실 검증과 실제 항공기 동작을 연결한다. 성공적인 캠페인은 항공기가 방어 가능하고 명확하게 검증된 운용영역(Validated Operating Envelope) 내에서 대표적인 임무를 수행하는 동안 추진, 비행제어, 항법, 자율 시스템, 화물 취급, 통신 및 안전 시스템이 지속적으로 상호 협조된 상태를 유지한다는 것을 입증한다.
