**Volume 23. Cargo UAV Autonomy and Flight AI**


# Chapter 05. Perception and Sensor Fusion for UAV

##  

## 05.01. UAV Sensor Suite Architecture and Integration

![](images/image1.png){width="7.268055555555556in" height="7.268055555555556in"}

A cargo UAV sensor suite must be designed as an integrated perception infrastructure rather than as a collection of independent sensing devices. The architecture combines inertial, satellite navigation, vision, LiDAR, radar, altitude, air-data, and vehicle-health sensors so that flight-control, navigation, obstacle-avoidance, landing, and safety functions receive consistent information about vehicle state and the surrounding environment. Volume_23_Cargo_UAV_Autonomy_an...

The inertial measurement unit forms the highest-rate foundation of the sensing architecture. Three-axis accelerometers and gyroscopes continuously measure specific force and angular velocity, while magnetometers may provide heading references when electromagnetic conditions permit. Because inertial measurements accumulate bias and integration error, the IMU is normally combined with independent absolute or relative measurements rather than being used as a standalone navigation source.

GNSS provides geographically referenced position, velocity, and time information and becomes especially important during long-range cargo operations. Multi-constellation receivers, differential corrections, and RTK-capable configurations can improve availability and positioning accuracy. The sensor architecture should nevertheless assume that satellite navigation can become degraded by terrain, buildings, interference, jamming, spoofing, antenna faults, or unfavorable satellite geometry.

Camera systems extend perception from vehicle-state estimation into semantic understanding of the environment. Forward-facing cameras can support obstacle recognition and navigation, downward-facing cameras can provide visual odometry and landing-zone information, and stereo or multi-camera arrangements can estimate depth. Their placement must consider field of view, vibration, propeller visibility, aerodynamic structures, lighting variation, contamination, and synchronization with other sensors.

LiDAR supplies geometrically precise three-dimensional measurements that complement camera information. Point clouds can represent terrain, buildings, vegetation, wires, vehicles, landing surfaces, and other structures around the aircraft. For cargo UAVs operating close to infrastructure or in GNSS-denied environments, LiDAR can contribute to localization, mapping, obstacle detection, terrain following, and landing-zone assessment while preserving direct geometric information about surrounding objects.

Radar provides another complementary sensing channel because its physical operating characteristics differ significantly from optical sensors. Radar can contribute range and relative-velocity measurements and may remain useful when visibility, illumination, haze, dust, or precipitation reduces camera or LiDAR effectiveness. A robust UAV perception architecture therefore exploits sensor diversity rather than expecting one sensing modality to perform reliably under every operational condition.

Altitude sensing should combine measurements appropriate to different flight regimes. Barometric altitude provides lightweight and continuous vertical information, GNSS supplies globally referenced altitude, radar or laser altimeters can measure distance to terrain, and downward perception sensors can estimate local surface geometry. These measurements have different error characteristics, reference frames, ranges, and update rates, requiring explicit fusion and validity management before they are consumed by control functions.

Sensor integration begins with electrical and physical interfaces but cannot end there. Each device requires defined power, communication bandwidth, initialization behavior, calibration data, diagnostic status, and failure response. Interfaces such as CAN, Ethernet, serial links, or dedicated avionics buses must be selected according to data rate and criticality, while high-bandwidth cameras and LiDAR generally require substantially different transport resources from compact inertial or air-data sensors.

Accurate time synchronization is fundamental to multi-sensor perception on a moving aircraft. A camera frame, LiDAR scan, radar return, GNSS observation, and IMU sample represent different physical states if their timestamps are misaligned. Hardware timestamps, synchronized clocks, trigger signals, or disciplined network timing should therefore preserve measurement acquisition time as closely as possible, allowing fusion algorithms to compensate for transport and processing latency.

Spatial calibration is equally important because every sensor observes the aircraft and environment from a different physical location and orientation. The system must maintain transformations between sensor frames, the vehicle body frame, navigation frame, and relevant world frames. Lever-arm offsets become particularly significant on large cargo UAVs, where antennas, cameras, radars, and inertial units may be separated by several meters and experience measurable differences during rotation.

A practical integration architecture separates raw acquisition, preprocessing, state estimation, environmental perception, and application interfaces. Sensor drivers convert device-specific protocols into standardized measurements, preprocessing modules perform filtering and compensation, and fusion components estimate coherent vehicle or environmental states. This separation prevents flight-control and autonomy applications from becoming tightly coupled to individual sensor models and supports controlled hardware replacement or upgrades.

The sensor-fusion layer must preserve uncertainty rather than simply combining measurements numerically. Every observation has noise, bias, latency, calibration uncertainty, environmental sensitivity, and potential failure modes. Covariance estimates, confidence measures, innovation tests, and consistency checks allow estimators to determine how strongly each measurement should influence the current state estimate and whether a measurement has become inconsistent with the rest of the sensing system.

Cargo UAV architecture places additional emphasis on redundancy because perception failures can directly affect a heavy aircraft carrying substantial payload. Critical state variables should not depend on a single sensor or communication path whenever the safety concept requires continued operation after failure. Multiple IMUs, GNSS receivers, altimeters, or complementary perception modalities can provide independent evidence while monitoring logic identifies disagreement, degradation, and loss of validity.

Redundancy does not mean blindly averaging duplicate measurements. Sensors can experience common-mode failures caused by shared power supplies, mounting structures, electromagnetic interference, software defects, environmental conditions, or identical algorithmic assumptions. Architectural independence therefore requires careful consideration of power domains, communication paths, sensor diversity, physical placement, processing resources, and software partitioning so that one fault does not silently invalidate multiple supposedly redundant channels.

Sensor-health management operates continuously alongside normal perception. Drivers and fusion modules should monitor communication timeouts, stale timestamps, impossible values, excessive noise, bias growth, temperature limits, calibration status, packet loss, and disagreement between independent measurements. Health information must propagate with the sensor data so downstream autonomy and flight-control components can distinguish a valid estimate from one generated under degraded sensing conditions.

Graceful degradation is especially important because the surrounding chapter structure explicitly progresses from individual LiDAR, camera, radar, and inertial functions toward multi-sensor fusion and sensor-failure fallback. Volume_23_Cargo_UAV_Autonomy_an... If one modality becomes unavailable, the architecture should determine which capabilities remain trustworthy, reduce operational authority when necessary, and transition toward alternate navigation, contingency flight, return, holding, or landing behavior instead of allowing uncontrolled estimator degradation.

Computing placement must account for both deterministic flight-critical processing and computationally intensive perception. High-rate inertial estimation may execute close to the flight-control computer, while image processing, point-cloud analysis, object detection, and semantic perception can run on dedicated edge accelerators. Interfaces between these domains should expose bounded, well-defined state and perception products rather than allowing noncritical AI workloads to interfere with deterministic control execution.

Integration must also consider the relationship between perception and the broader avionics architecture. The volume structure places sensor-suite integration within UAV system architecture and later develops perception into navigation, collision avoidance, terrain following, landing assessment, and failure fallback functions. Volume_23_Cargo_UAV_Autonomy_an... This implies that sensor infrastructure should be reusable across multiple consumers while maintaining clear ownership of raw data, fused states, diagnostics, and safety decisions.

Verification begins before flight testing. Individual sensor drivers can be tested with recorded data and simulated faults, calibration procedures can be validated against known references, and synchronization accuracy can be measured under representative processing loads. Software-in-the-loop and hardware-in-the-loop environments can inject delayed, missing, corrupted, biased, or contradictory sensor measurements to verify that estimators and health-management logic respond predictably before airborne operation.

Ultimately, the UAV sensor suite should provide a trustworthy and continuously qualified representation of vehicle motion and surrounding space. Its effectiveness depends not merely on sensor resolution but on synchronization, calibration, uncertainty modeling, redundancy, fault isolation, deterministic data transport, and well-defined interfaces. These architectural foundations enable the later LiDAR, camera, radar, IMU-GNSS fusion, terrain perception, collision detection, landing assessment, and fallback functions to operate as one coherent perception system.

화물 무인항공기(Cargo UAV)의 센서 스위트(Sensor Suite)는 독립적인 센싱 장치(Sensing Device)의 단순한 집합이 아니라 통합된 인지 인프라(Perception Infrastructure)로 설계되어야 한다. 이 아키텍처는 관성 센서(Inertial Sensor), 위성 항법(Satellite Navigation), 비전(Vision), 라이다(LiDAR), 레이더(Radar), 고도 센서(Altitude Sensor), 대기자료 센서(Air-Data Sensor), 기체 상태 센서(Vehicle-Health Sensor)를 결합하여 비행 제어(Flight Control), 항법(Navigation), 장애물 회피(Obstacle Avoidance), 착륙(Landing), 안전(Safety) 기능에 기체 상태와 주변 환경에 관한 일관된 정보를 제공한다.

관성측정장치(Inertial Measurement Unit, IMU)는 센싱 아키텍처에서 가장 높은 주기로 동작하는 기반을 형성한다. 3축 가속도계(Three-Axis Accelerometer)와 자이로스코프(Gyroscope)는 비력(Specific Force)과 각속도(Angular Velocity)를 지속적으로 측정하며, 자기계(Magnetometer)는 전자기 환경이 허용되는 경우 방위 기준(Heading Reference)을 제공할 수 있다. 관성 측정값은 바이어스(Bias)와 적분 오차(Integration Error)가 누적되므로 일반적으로 독립적인 절대 또는 상대 측정값과 결합하여 사용한다.

위성항법시스템(GNSS)은 지리적으로 참조된 위치(Position), 속도(Velocity), 시간(Time) 정보를 제공하며 장거리 화물 운송 임무에서 특히 중요하다. 다중 위성군 수신기(Multi-Constellation Receiver), 차분 보정(Differential Correction), 실시간 이동측위(RTK) 구성을 이용하면 가용성과 측위 정확도를 향상시킬 수 있다. 그러나 센서 아키텍처는 지형, 건물, 전파 간섭, 재밍(Jamming), 스푸핑(Spoofing), 안테나 고장 또는 불리한 위성 배치로 인해 위성 항법 성능이 저하될 가능성을 기본적으로 고려해야 한다.

카메라 시스템(Camera System)은 기체 상태 추정을 넘어 주변 환경의 의미론적 이해(Semantic Understanding)를 가능하게 한다. 전방 카메라(Forward-Facing Camera)는 장애물 인식과 항법을 지원하고, 하향 카메라(Downward-Facing Camera)는 시각 주행거리계(Visual Odometry)와 착륙 구역 정보를 제공할 수 있으며, 스테레오 또는 다중 카메라 구성은 깊이(Depth)를 추정할 수 있다. 센서 배치에서는 시야각(Field of View), 진동, 프로펠러 노출, 공력 구조물, 조명 변화, 오염 및 다른 센서와의 동기화를 고려해야 한다.

라이다(LiDAR)는 카메라 정보를 보완하는 정밀한 3차원 기하 측정(Three-Dimensional Geometric Measurement)을 제공한다. 포인트 클라우드(Point Cloud)는 지형, 건물, 식생, 전선, 차량, 착륙 표면 및 항공기 주변의 다양한 구조물을 표현할 수 있다. 기반 시설 가까이 또는 위성항법시스템(GNSS) 사용이 제한된 환경에서 운용되는 화물 무인항공기의 경우 라이다는 주변 객체에 대한 직접적인 기하 정보를 유지하면서 위치추정(Localization), 매핑(Mapping), 장애물 탐지, 지형 추종(Terrain Following), 착륙 구역 평가(Landing-Zone Assessment)에 기여할 수 있다.

레이더(Radar)는 광학 센서(Optical Sensor)와 물리적 동작 특성이 크게 다르기 때문에 또 하나의 상호보완적인 센싱 채널(Sensing Channel)을 제공한다. 레이더는 거리와 상대 속도(Relative Velocity) 측정에 활용될 수 있으며 가시성, 조명, 연무, 먼지 또는 강수로 인해 카메라나 라이다의 성능이 저하되는 상황에서도 유용할 수 있다. 따라서 강건한 무인항공기 인지 아키텍처(Robust UAV Perception Architecture)는 하나의 센서 방식이 모든 운용 환경에서 안정적으로 작동할 것이라고 가정하지 않고 센서 다양성(Sensor Diversity)을 활용한다.

고도 센싱(Altitude Sensing)은 서로 다른 비행 영역에 적합한 여러 측정값을 결합해야 한다. 기압 고도(Barometric Altitude)는 가볍고 연속적인 수직 정보를 제공하고, 위성항법시스템(GNSS)은 전역 기준 고도를 제공하며, 레이더 또는 레이저 고도계(Radar or Laser Altimeter)는 지면까지의 거리를 측정할 수 있다. 하향 인지 센서(Downward Perception Sensor)는 국부적인 지표면 형상을 추정할 수 있다. 이러한 측정값은 오차 특성, 기준 좌표계, 측정 범위 및 갱신 주기가 서로 다르므로 제어 기능에 사용하기 전에 명시적인 융합(Fusion)과 유효성 관리(Validity Management)가 필요하다.

센서 통합(Sensor Integration)은 전기적·물리적 인터페이스에서 시작하지만 거기에서 끝나서는 안 된다. 각 장치에는 정의된 전원, 통신 대역폭, 초기화 동작, 보정 데이터(Calibration Data), 진단 상태(Diagnostic Status), 고장 대응(Failure Response)이 필요하다. CAN, 이더넷(Ethernet), 직렬 링크(Serial Link), 전용 항공전자 버스(Avionics Bus)와 같은 인터페이스는 데이터 전송률과 중요도에 따라 선택해야 하며, 고대역폭 카메라와 라이다는 소형 관성 센서 또는 대기자료 센서와 상당히 다른 전송 자원을 요구한다.

정확한 시간 동기화(Time Synchronization)는 움직이는 항공기에서 다중 센서 인지(Multi-Sensor Perception)를 구현하기 위한 핵심 요소이다. 카메라 프레임, 라이다 스캔, 레이더 반사 신호, 위성항법 관측값 및 관성측정장치(IMU) 샘플은 타임스탬프(Timestamp)가 일치하지 않으면 서로 다른 물리적 상태를 나타낸다. 따라서 하드웨어 타임스탬프(Hardware Timestamp), 동기화된 클록(Synchronized Clock), 트리거 신호(Trigger Signal), 네트워크 시간 동기화(Network Timing)를 통해 실제 측정 획득 시간을 최대한 정확하게 보존해야 하며, 이를 통해 융합 알고리즘이 전송 및 처리 지연을 보상할 수 있다.

공간 보정(Spatial Calibration) 역시 중요하다. 각 센서는 서로 다른 물리적 위치와 방향에서 항공기와 주변 환경을 관측하기 때문이다. 시스템은 센서 좌표계(Sensor Frame), 기체 좌표계(Vehicle Body Frame), 항법 좌표계(Navigation Frame), 관련 세계 좌표계(World Frame) 사이의 변환 관계를 유지해야 한다. 특히 대형 화물 무인항공기에서는 안테나, 카메라, 레이더 및 관성 장치가 수 미터 떨어져 설치될 수 있으므로 회전 운동 중 발생하는 차이를 고려하면 레버암 오프셋(Lever-Arm Offset)이 상당히 중요해진다.

실용적인 통합 아키텍처(Integration Architecture)는 원시 데이터 획득(Raw Acquisition), 전처리(Preprocessing), 상태 추정(State Estimation), 환경 인지(Environmental Perception), 응용 인터페이스(Application Interface)를 분리한다. 센서 드라이버(Sensor Driver)는 장치별 프로토콜을 표준화된 측정값으로 변환하고, 전처리 모듈은 필터링과 보상을 수행하며, 융합 구성요소(Fusion Component)는 일관된 기체 또는 환경 상태를 추정한다. 이러한 분리는 비행 제어와 자율 기능이 개별 센서 모델에 강하게 결합되는 것을 방지하고 센서 하드웨어의 교체 및 업그레이드를 지원한다.

센서 융합 계층(Sensor-Fusion Layer)은 측정값을 단순히 수치적으로 결합하는 것이 아니라 불확실성(Uncertainty)을 유지해야 한다. 모든 관측값에는 노이즈(Noise), 바이어스, 지연시간(Latency), 보정 불확실성, 환경 민감도 및 잠재적 고장 모드가 존재한다. 공분산(Covariance), 신뢰도(Confidence), 이노베이션 테스트(Innovation Test), 일관성 검사(Consistency Check)를 사용하면 추정기가 각 측정값을 현재 상태 추정에 어느 정도 반영해야 하는지, 그리고 특정 측정값이 다른 센싱 시스템과 불일치하기 시작했는지를 판단할 수 있다.

화물 무인항공기 아키텍처에서는 인지 시스템의 고장이 상당한 화물을 운송하는 대형 항공기의 안전에 직접 영향을 줄 수 있으므로 이중화(Redundancy)가 더욱 중요하다. 안전 개념(Safety Concept)이 고장 이후에도 지속적인 운용을 요구한다면 핵심 상태 변수가 하나의 센서 또는 통신 경로에 의존해서는 안 된다. 다중 관성측정장치(IMU), 위성항법 수신기, 고도계 또는 상호보완적인 인지 센서를 이용하면 독립적인 측정 근거를 확보하면서 감시 로직(Monitoring Logic)을 통해 불일치, 성능 저하 및 유효성 상실을 탐지할 수 있다.

이중화(Redundancy)는 중복된 센서의 측정값을 무조건 평균하는 것을 의미하지 않는다. 센서는 공유 전원, 장착 구조, 전자기 간섭, 소프트웨어 결함, 환경 조건 또는 동일한 알고리즘 가정으로 인해 공통원인고장(Common-Mode Failure)을 경험할 수 있다. 따라서 아키텍처 독립성(Architectural Independence)을 확보하려면 전원 도메인(Power Domain), 통신 경로, 센서 다양성, 물리적 배치, 처리 자원 및 소프트웨어 파티셔닝(Software Partitioning)을 신중하게 설계하여 하나의 고장이 여러 개의 이중화 채널을 동시에 무효화하지 않도록 해야 한다.

센서 상태 관리(Sensor-Health Management)는 정상적인 인지 처리와 동시에 지속적으로 수행된다. 드라이버와 융합 모듈은 통신 타임아웃(Communication Timeout), 오래된 타임스탬프, 비정상 값, 과도한 노이즈, 바이어스 증가, 온도 한계, 보정 상태, 패킷 손실 및 독립적인 측정값 사이의 불일치를 감시해야 한다. 상태 정보(Health Information)는 센서 데이터와 함께 하위 시스템으로 전달되어 자율 기능과 비행 제어 구성요소가 정상적인 추정값과 성능이 저하된 센싱 환경에서 생성된 추정값을 구분할 수 있도록 해야 한다.

점진적 성능 저하(Graceful Degradation)는 특히 중요하다. 개별 라이다, 카메라, 레이더 및 관성 센싱 기능은 궁극적으로 다중 센서 융합(Multi-Sensor Fusion)과 센서 고장 시 대체 운용(Sensor-Failure Fallback)으로 연결되어야 한다. 하나의 센서 방식이 사용 불가능해지면 아키텍처는 어떤 기능이 여전히 신뢰할 수 있는지 판단하고, 필요한 경우 운용 권한을 제한하며, 추정기의 성능 저하를 방치하는 대신 대체 항법, 비상 비행(Contingency Flight), 복귀(Return), 대기(Holding) 또는 착륙 동작으로 전환해야 한다.

컴퓨팅 배치(Computing Placement)는 결정론적인 비행 핵심 처리(Deterministic Flight-Critical Processing)와 계산량이 많은 인지 처리를 모두 고려해야 한다. 고주기 관성 상태 추정은 비행제어컴퓨터(Flight Control Computer) 가까이에서 실행할 수 있는 반면, 영상 처리, 포인트 클라우드 분석, 객체 탐지 및 의미론적 인지는 전용 엣지 가속기(Edge Accelerator)에서 실행할 수 있다. 이들 영역 사이의 인터페이스는 비핵심 인공지능(AI) 워크로드가 결정론적 제어 실행을 방해하지 않도록 명확하게 정의되고 범위가 제한된 상태 및 인지 결과를 제공해야 한다.

통합 과정에서는 인지 시스템과 전체 항공전자 아키텍처(Avionics Architecture)의 관계도 고려해야 한다. 센서 스위트 통합은 무인항공기 시스템 아키텍처의 일부이며 이후 항법, 충돌 회피, 지형 추종, 착륙 평가 및 고장 시 대체 기능으로 확장된다. 따라서 센서 인프라는 여러 기능이 공동으로 사용할 수 있도록 설계하는 동시에 원시 데이터, 융합 상태(Fused State), 진단 정보 및 안전 의사결정(Safety Decision)의 소유권과 책임 경계를 명확하게 유지해야 한다.

검증(Verification)은 실제 비행시험 이전부터 시작되어야 한다. 개별 센서 드라이버는 기록된 데이터와 모의 고장(Simulated Fault)을 이용하여 시험할 수 있고, 보정 절차는 알려진 기준값을 이용하여 검증할 수 있으며, 대표적인 처리 부하에서 동기화 정확도를 측정할 수 있다. 소프트웨어 인 더 루프(Software-in-the-Loop, SIL)와 하드웨어 인 더 루프(Hardware-in-the-Loop, HIL) 환경에서는 지연, 누락, 손상, 바이어스 또는 상호 모순된 센서 데이터를 주입하여 추정기와 상태 관리 로직이 실제 비행 전에 예측 가능한 방식으로 대응하는지 검증할 수 있다.

궁극적으로 무인항공기 센서 스위트(UAV Sensor Suite)는 기체 운동과 주변 공간에 대해 신뢰할 수 있고 지속적으로 적격성이 확인되는 표현을 제공해야 한다. 그 성능은 단순한 센서 해상도뿐만 아니라 동기화, 보정, 불확실성 모델링(Uncertainty Modeling), 이중화, 고장 격리(Fault Isolation), 결정론적 데이터 전송 및 명확하게 정의된 인터페이스에 의해 결정된다. 이러한 아키텍처 기반은 이후의 라이다, 카메라, 레이더, 관성측정장치-위성항법시스템 융합(IMU-GNSS Fusion), 지형 인지, 충돌 탐지, 착륙 평가 및 고장 대체 기능이 하나의 일관된 인지 시스템(Coherent Perception System)으로 동작할 수 있도록 한다.

##  

## 05.02. LiDAR 3D Mapping and Obstacle Detection [w/Code]

![](images/image2.png){width="7.268055555555556in" height="7.268055555555556in"}

LiDAR provides cargo UAVs with direct three-dimensional measurements of surrounding geometry by estimating the distance between the aircraft and reflecting surfaces. Unlike passive vision, the sensor actively emits laser energy and measures its return, producing spatial samples that can be transformed into a point cloud. This geometric representation supports mapping, localization, obstacle detection, terrain perception, and safe autonomous flight.

A UAV LiDAR installation must balance sensing range, angular resolution, field of view, scan rate, weight, electrical power, aerodynamic exposure, and environmental robustness. Long-range sensing is valuable during high-speed flight because the autonomy system needs sufficient time to detect hazards and modify the trajectory. Near-field coverage is equally important during takeoff, landing, hovering, and operations close to infrastructure.

Each LiDAR return is initially represented in the coordinate frame of the sensor. To make these measurements useful for navigation, the system transforms them through calibrated sensor-to-body and body-to-navigation relationships. Accurate extrinsic calibration is therefore essential. Even a small angular mounting error can produce substantial spatial displacement at long range, causing distorted maps, inaccurate obstacle positions, and inconsistent alignment between successive scans.

Motion compensation is particularly important for airborne LiDAR because the sensor is continuously translating and rotating while collecting a scan. Points measured at the beginning and end of one scan may correspond to different vehicle poses. High-rate IMU measurements and estimated vehicle motion can be used to deskew the point cloud, transforming individual returns toward a common temporal reference before mapping or obstacle processing begins.

The raw point cloud normally passes through preprocessing before entering higher-level perception. Invalid returns, isolated noise, extremely weak measurements, and points corresponding to known parts of the aircraft can be removed. Range limits and region-of-interest filters reduce unnecessary computation, while voxel-based downsampling can decrease point density without eliminating the dominant geometric structure required for mapping and collision assessment.

Three-dimensional mapping converts successive LiDAR observations into a persistent spatial representation. The system estimates the UAV pose associated with each scan and transforms observed points into a common map frame. Scan registration can align overlapping geometric structures between observations, while inertial and GNSS information can constrain accumulated motion. The resulting map represents surfaces around the aircraft rather than treating every LiDAR scan as an isolated measurement.

Map representation should be selected according to the autonomy function. Dense point-cloud maps preserve detailed geometry but can require substantial memory and processing bandwidth. Voxel maps divide three-dimensional space into discrete cells, while occupancy representations estimate whether individual regions are free, occupied, or unknown. Local maps can emphasize immediate collision avoidance, whereas larger persistent maps can support localization, route execution, and repeated operations.

Obstacle detection begins by distinguishing potentially hazardous structures from free space and irrelevant measurements. Buildings, poles, cranes, trees, cables, terrain, vehicles, and other aircraft may all become collision hazards depending on the mission. Rather than relying only on object identity, a safety-oriented system can first determine whether measured geometry intersects the protected volume or predicted flight corridor of the cargo UAV.

Point clustering groups spatially related measurements into candidate obstacles. Geometric properties such as cluster dimensions, orientation, density, height, and distance can then support further interpretation. Ground or terrain segmentation is also important because terrain should not always be processed as an isolated obstacle. The perception system must instead determine whether terrain elevation, slope, or protruding structures violate the required flight clearance.

Obstacle representation must include the physical dimensions and dynamic behavior of the aircraft. A large cargo UAV cannot be modeled as a single point because its rotors, wings, fuselage, payload structure, and safety margins occupy substantial space. Detected geometry can therefore be expanded by an appropriate clearance margin, or the aircraft footprint can be incorporated into collision checking so that trajectory planning respects the complete protected volume.

Detection range and stopping or maneuvering distance are tightly coupled. A sensor may technically detect an object at long range, but useful avoidance requires enough time for filtering, classification, trajectory generation, control response, and physical maneuver execution. Aircraft velocity, mass, payload, wind, actuator authority, and regulatory separation requirements therefore influence how far ahead the perception system must reliably detect obstacles.

LiDAR-based obstacle processing should distinguish static and dynamic structures whenever sufficient temporal information is available. Static buildings or terrain can be integrated into persistent maps, while objects whose measured positions change across observations require tracking. Estimated position, velocity, uncertainty, and motion consistency can then be supplied to collision prediction so the autonomy system evaluates where an obstacle is likely to be rather than only where it was measured.

Thin objects create a particularly demanding problem for airborne LiDAR. Power lines, cables, antennas, branches, and narrow structural elements may produce relatively few returns and can disappear depending on range, incidence angle, resolution, and surface reflectivity. Filtering algorithms must therefore avoid aggressively deleting sparse measurements when those measurements could correspond to safety-critical obstacles within the projected flight path.

Environmental conditions also influence LiDAR performance. Rain, fog, dust, snow, direct sunlight, reflective surfaces, dark materials, and unfavorable incidence angles can reduce measurement quality or generate misleading returns. The perception architecture should associate measurements with validity and confidence information rather than assuming every point is equally trustworthy. Complementary camera and radar sensing can provide additional evidence when optical ranging becomes degraded.

Real-time execution requires careful management of computational load. High-resolution LiDAR can generate large point clouds at repeated scan rates, while mapping, registration, segmentation, clustering, and collision analysis must complete within bounded latency. Processing pipelines can use spatial cropping, voxelization, parallel computation, GPU acceleration, and local-map management to limit workload while preserving information important to flight safety.

The mapping subsystem should manage uncertainty and map aging. Position-estimation errors can cause repeated surfaces to appear blurred or duplicated, while dynamic objects can leave obsolete occupied regions if measurements are retained indefinitely. Local occupancy information should therefore be updated according to observation confidence and time, allowing newly observed free space to clear outdated obstacles while preventing transient sensor noise from rapidly changing safety-critical map states.

LiDAR perception must also remain synchronized with the broader UAV state-estimation system. Point clouds processed with an incorrect attitude or position estimate can generate internally consistent but geographically incorrect obstacles. Timestamp integrity, IMU synchronization, GNSS alignment, calibration version, and pose uncertainty should therefore accompany the mapping process so downstream navigation modules understand both the estimated geometry and the confidence of its spatial reference.

Obstacle information should be delivered through a stable interface rather than exposing every autonomy component directly to raw point clouds. The perception layer can publish local occupancy maps, obstacle tracks, free-space regions, terrain surfaces, or collision-relevant geometric primitives. Navigation and avoidance modules can then consume representations appropriate to their timing requirements while the LiDAR pipeline retains responsibility for sensor-specific preprocessing and geometric interpretation.

Failure monitoring must detect both complete sensor loss and gradual degradation. Missing scans, abnormal point counts, excessive invalid returns, timing discontinuities, temperature faults, blocked fields of view, calibration changes, or disagreement with other sensors can indicate degraded LiDAR operation. The autonomy system should respond according to the remaining perception capability, potentially reducing speed, increasing clearance, switching sensor sources, modifying the route, or initiating contingency behavior.

Validation should reproduce the geometric and environmental conditions expected during cargo missions. Recorded point clouds and simulation can test algorithmic behavior, while hardware-in-the-loop configurations can evaluate timing and interface failures. Flight testing should progressively examine open terrain, buildings, vegetation, thin obstacles, changing altitude, dynamic targets, adverse visibility, and representative vehicle speeds while measuring detection probability, false alarms, range accuracy, and processing latency.

LiDAR ultimately acts as both a geometric perception sensor and an important contributor to the UAV safety envelope. Its value depends not only on producing dense three-dimensional points but on transforming those measurements into synchronized, calibrated, uncertainty-aware information that autonomy software can use in real time. Integrated with mapping, state estimation, obstacle tracking, navigation, and complementary sensors, LiDAR enables cargo UAVs to maintain spatial awareness in complex three-dimensional operating environments.

라이다(LiDAR)는 항공기와 반사 표면 사이의 거리를 추정함으로써 화물 무인항공기(Cargo UAV)에 주변 환경의 직접적인 3차원 측정값을 제공한다. 수동형 비전(Passive Vision)과 달리 센서가 능동적으로 레이저 에너지를 방출하고 반사 신호를 측정하여 포인트 클라우드(Point Cloud)로 변환할 수 있는 공간 샘플을 생성한다. 이러한 기하학적 표현(Geometric Representation)은 매핑(Mapping), 위치추정(Localization), 장애물 탐지(Obstacle Detection), 지형 인지(Terrain Perception), 안전한 자율 비행을 지원한다.

무인항공기 라이다(LiDAR) 설치는 감지 거리(Sensing Range), 각도 해상도(Angular Resolution), 시야각(Field of View), 스캔 속도(Scan Rate), 중량, 전력 소비, 공기역학적 노출(Aerodynamic Exposure), 환경 강건성(Environmental Robustness) 사이의 균형을 고려해야 한다. 고속 비행에서는 자율 시스템이 위험 요소를 탐지하고 궤적을 수정할 충분한 시간을 확보해야 하므로 장거리 감지가 중요하다. 이륙, 착륙, 호버링(Hovering), 기반 시설 인접 운용에서는 근거리 감지 범위도 동일하게 중요하다.

각각의 라이다 반사 신호(LiDAR Return)는 처음에는 센서 좌표계(Sensor Coordinate Frame)를 기준으로 표현된다. 이러한 측정값을 항법에 사용하려면 보정된 센서-기체(Sensor-to-Body) 및 기체-항법(Body-to-Navigation) 변환 관계를 통해 좌표를 변환해야 한다. 따라서 정확한 외부 보정(Extrinsic Calibration)이 필수적이다. 작은 장착 각도 오차도 장거리에서는 상당한 공간적 변위를 발생시켜 왜곡된 지도, 부정확한 장애물 위치 및 연속 스캔 사이의 정합 불일치를 초래할 수 있다.

운동 보상(Motion Compensation)은 센서가 하나의 스캔을 수집하는 동안 항공기가 지속적으로 병진 및 회전 운동을 수행하기 때문에 항공 라이다에서 특히 중요하다. 하나의 스캔 시작과 종료 시점에 측정된 포인트는 서로 다른 기체 자세(Vehicle Pose)에 대응할 수 있다. 고주기 관성측정장치(IMU) 데이터와 추정된 기체 운동을 이용하여 포인트 클라우드의 왜곡을 보정(Deskew)하고, 매핑이나 장애물 처리를 시작하기 전에 개별 반사점을 공통 시간 기준(Common Temporal Reference)으로 변환할 수 있다.

원시 포인트 클라우드(Raw Point Cloud)는 일반적으로 상위 수준 인지 처리에 입력되기 전에 전처리(Preprocessing)를 거친다. 유효하지 않은 반사 신호, 고립된 노이즈, 지나치게 약한 측정값 및 항공기 자체의 알려진 구조물에 해당하는 포인트를 제거할 수 있다. 거리 제한과 관심영역 필터(Region-of-Interest Filter)는 불필요한 계산을 줄이며, 복셀 기반 다운샘플링(Voxel-Based Downsampling)은 매핑과 충돌 평가에 필요한 주요 기하 구조를 유지하면서 포인트 밀도를 감소시킬 수 있다.

3차원 매핑(Three-Dimensional Mapping)은 연속적인 라이다 관측값을 지속적으로 유지되는 공간 표현(Spatial Representation)으로 변환한다. 시스템은 각 스캔에 해당하는 무인항공기의 자세를 추정하고 관측된 포인트를 공통 지도 좌표계(Map Frame)로 변환한다. 스캔 정합(Scan Registration)은 관측 사이에서 중첩되는 기하 구조를 정렬할 수 있으며, 관성 정보와 위성항법시스템(GNSS) 정보는 누적되는 운동 오차를 제한할 수 있다. 결과 지도는 각각의 라이다 스캔을 독립적으로 처리하는 대신 항공기 주변의 표면 구조를 지속적으로 표현한다.

지도 표현(Map Representation)은 자율 기능의 목적에 따라 선택해야 한다. 고밀도 포인트 클라우드 지도(Dense Point-Cloud Map)는 세부적인 기하 구조를 유지하지만 상당한 메모리와 처리 대역폭을 요구할 수 있다. 복셀 지도(Voxel Map)는 3차원 공간을 개별 셀로 분할하고, 점유 표현(Occupancy Representation)은 각 영역이 자유 공간(Free), 점유 공간(Occupied), 미확인 공간(Unknown) 중 어느 상태인지를 추정한다. 지역 지도(Local Map)는 즉각적인 충돌 회피에 집중할 수 있으며, 더 큰 지속형 지도(Persistent Map)는 위치추정, 경로 실행 및 반복 운용을 지원할 수 있다.

장애물 탐지(Obstacle Detection)는 잠재적으로 위험한 구조물을 자유 공간 및 중요하지 않은 측정값으로부터 구분하는 과정에서 시작된다. 건물, 기둥, 크레인, 나무, 케이블, 지형, 차량 및 다른 항공기는 임무 조건에 따라 모두 충돌 위험 요소가 될 수 있다. 안전 중심 시스템(Safety-Oriented System)은 객체의 정체성에만 의존하기보다 측정된 기하 구조가 화물 무인항공기의 보호 공간(Protected Volume)이나 예측 비행 회랑(Predicted Flight Corridor)과 교차하는지를 우선 판단할 수 있다.

포인트 클러스터링(Point Clustering)은 공간적으로 연관된 측정값을 잠재적인 장애물 단위로 그룹화한다. 이후 클러스터 크기, 방향, 밀도, 높이, 거리와 같은 기하학적 특성을 이용하여 추가적인 해석을 수행할 수 있다. 지면 또는 지형 분할(Ground or Terrain Segmentation) 역시 중요하다. 지형 자체를 항상 독립적인 장애물로 처리해서는 안 되며, 대신 지형 고도, 경사 또는 돌출 구조물이 요구되는 비행 안전거리(Flight Clearance)를 침범하는지를 판단해야 한다.

장애물 표현(Obstacle Representation)은 항공기의 실제 크기와 동적 거동(Dynamic Behavior)을 포함해야 한다. 대형 화물 무인항공기는 로터, 날개, 동체, 화물 구조 및 안전 여유 공간이 상당한 영역을 차지하기 때문에 단순한 하나의 점으로 모델링할 수 없다. 따라서 탐지된 기하 구조를 적절한 안전거리만큼 확장하거나 항공기 외형(Aircraft Footprint)을 충돌 검사(Collision Checking)에 직접 포함하여 궤적 계획이 전체 보호 공간을 고려하도록 해야 한다.

탐지 거리(Detection Range)는 정지 또는 회피 기동 거리와 밀접하게 연계된다. 센서가 기술적으로 장거리에서 객체를 탐지할 수 있더라도 실제 회피를 위해서는 필터링, 분류(Classification), 궤적 생성(Trajectory Generation), 제어 응답(Control Response), 물리적 기동 수행에 충분한 시간이 필요하다. 따라서 항공기의 속도, 질량, 탑재 화물, 바람, 액추에이터 제어 권한(Actuator Authority), 규정상 분리 요구조건이 인지 시스템에서 요구되는 전방 장애물 탐지 거리를 결정한다.

라이다 기반 장애물 처리(LiDAR-Based Obstacle Processing)는 충분한 시간적 정보가 제공될 경우 정적 구조물과 동적 구조물을 구분해야 한다. 정적인 건물이나 지형은 지속형 지도에 통합할 수 있지만 여러 관측에서 위치가 변화하는 객체에는 추적(Tracking)이 필요하다. 추정된 위치, 속도, 불확실성 및 운동 일관성을 충돌 예측(Collision Prediction)에 전달하면 자율 시스템은 장애물이 측정된 현재 위치뿐만 아니라 향후 존재할 것으로 예상되는 위치까지 평가할 수 있다.

가느다란 객체(Thin Object)는 항공 라이다에 특히 어려운 문제를 발생시킨다. 전력선, 케이블, 안테나, 나뭇가지 및 좁은 구조물은 상대적으로 적은 반사점을 생성할 수 있으며 거리, 입사각(Incidence Angle), 해상도, 표면 반사율에 따라 탐지되지 않을 수도 있다. 따라서 필터링 알고리즘은 희소한 측정값이 예상 비행 경로 내의 안전 핵심 장애물(Safety-Critical Obstacle)을 나타낼 가능성이 있는 경우 이를 과도하게 제거하지 않아야 한다.

환경 조건(Environmental Conditions)도 라이다 성능에 영향을 준다. 비, 안개, 먼지, 눈, 직사광선, 반사 표면, 어두운 재질 및 불리한 입사각은 측정 품질을 저하시키거나 잘못된 반사 신호를 발생시킬 수 있다. 인지 아키텍처는 모든 포인트가 동일한 신뢰성을 가진다고 가정하는 대신 측정값에 유효성(Validity)과 신뢰도(Confidence) 정보를 연결해야 한다. 광학 거리 측정 성능이 저하되는 경우 카메라와 레이더 같은 상호보완적 센서가 추가적인 판단 근거를 제공할 수 있다.

실시간 실행(Real-Time Execution)을 위해서는 계산 부하(Computational Load)를 신중하게 관리해야 한다. 고해상도 라이다는 반복적인 스캔 주기마다 대규모 포인트 클라우드를 생성하며 매핑, 정합, 분할(Segmentation), 클러스터링, 충돌 분석은 제한된 지연시간(Bounded Latency) 내에 완료되어야 한다. 처리 파이프라인은 공간 크로핑(Spatial Cropping), 복셀화(Voxelization), 병렬 연산, 그래픽처리장치 가속(GPU Acceleration), 지역 지도 관리를 이용하여 비행 안전에 중요한 정보를 유지하면서 처리 부하를 제한할 수 있다.

매핑 하위 시스템(Mapping Subsystem)은 불확실성과 지도 노화(Map Aging)를 관리해야 한다. 위치 추정 오차로 인해 반복적으로 관측되는 표면이 흐려지거나 중복되어 나타날 수 있으며, 동적 객체의 측정값을 무기한 유지하면 오래된 점유 영역이 지도에 남을 수 있다. 따라서 지역 점유 정보(Local Occupancy Information)는 관측 신뢰도와 시간에 따라 갱신되어야 하며, 새롭게 관측된 자유 공간이 오래된 장애물을 제거하면서도 일시적인 센서 노이즈가 안전 핵심 지도 상태를 급격하게 변경하지 않도록 해야 한다.

라이다 인지(LiDAR Perception)는 전체 무인항공기 상태 추정 시스템(UAV State-Estimation System)과도 동기화되어야 한다. 잘못된 자세나 위치 추정값으로 포인트 클라우드를 처리하면 내부적으로는 일관되어 보이지만 지리적으로 잘못 배치된 장애물이 생성될 수 있다. 따라서 타임스탬프 무결성(Timestamp Integrity), 관성측정장치 동기화, 위성항법시스템 정렬, 보정 버전(Calibration Version), 자세 불확실성을 매핑 과정과 함께 관리하여 하위 항법 모듈이 추정된 기하 구조와 공간 기준의 신뢰성을 모두 이해할 수 있도록 해야 한다.

장애물 정보는 모든 자율 구성요소가 원시 포인트 클라우드에 직접 접근하도록 하는 대신 안정적인 인터페이스(Stable Interface)를 통해 제공해야 한다. 인지 계층은 지역 점유 지도(Local Occupancy Map), 장애물 추적 정보(Obstacle Track), 자유 공간 영역(Free-Space Region), 지형 표면(Terrain Surface), 충돌 관련 기하 프리미티브(Collision-Relevant Geometric Primitive)를 제공할 수 있다. 항법과 회피 모듈은 시간 요구조건에 적합한 표현을 사용하고, 라이다 파이프라인은 센서별 전처리와 기하학적 해석을 담당한다.

고장 감시(Failure Monitoring)는 센서의 완전한 상실뿐만 아니라 점진적인 성능 저하도 탐지해야 한다. 스캔 누락, 비정상적인 포인트 수, 과도한 무효 반사값, 시간 불연속, 온도 이상, 시야 차단, 보정 변화 또는 다른 센서와의 불일치는 라이다 성능 저하를 나타낼 수 있다. 자율 시스템은 남아 있는 인지 능력에 따라 속도를 줄이거나, 안전거리를 증가시키거나, 다른 센서로 전환하거나, 경로를 변경하거나, 비상 동작(Contingency Behavior)을 시작할 수 있어야 한다.

검증(Validation)은 실제 화물 임무에서 예상되는 기하학적 조건과 환경 조건을 재현해야 한다. 기록된 포인트 클라우드와 시뮬레이션을 이용하여 알고리즘 동작을 시험할 수 있으며, 하드웨어 인 더 루프(Hardware-in-the-Loop, HIL) 구성으로 시간 및 인터페이스 고장을 평가할 수 있다. 비행시험에서는 개방 지형, 건물, 식생, 가느다란 장애물, 고도 변화, 동적 표적, 악화된 가시성 및 대표적인 기체 속도를 단계적으로 시험하면서 탐지 확률, 오경보(False Alarm), 거리 정확도 및 처리 지연시간을 측정해야 한다.

궁극적으로 라이다(LiDAR)는 기하학적 인지 센서(Geometric Perception Sensor)이면서 동시에 무인항공기의 안전 영역(Safety Envelope)을 구성하는 중요한 요소로 작용한다. 그 가치는 단순히 고밀도의 3차원 포인트를 생성하는 데 있는 것이 아니라 이러한 측정값을 자율 소프트웨어가 실시간으로 활용할 수 있는 동기화되고 보정되며 불확실성을 고려한 정보로 변환하는 데 있다. 매핑, 상태 추정, 장애물 추적, 항법 및 상호보완적 센서와 통합된 라이다는 화물 무인항공기가 복잡한 3차원 운용 환경에서 지속적인 공간 인식(Spatial Awareness)을 유지할 수 있도록 한다.

##  

## 05.03. Camera Based Perception for UAV [w/Code]

![](images/image3.png){width="7.268055555555556in" height="7.268055555555556in"}

Camera-based perception gives a cargo UAV rich visual information about terrain, infrastructure, obstacles, landing areas, vehicles, people, and other objects that cannot be described adequately by vehicle-state sensors alone. Cameras provide high spatial resolution with relatively low mass and power consumption, making them useful for navigation, object recognition, collision avoidance, landing assistance, and semantic understanding of the operating environment.

The camera architecture should reflect the direction and purpose of observation. Forward-facing cameras provide information along the flight path, downward-facing cameras support visual localization and landing, and side or panoramic cameras can increase situational awareness around the aircraft. A multi-camera arrangement can reduce blind regions, while stereo cameras provide geometric depth when sufficient texture, baseline, resolution, and image correspondence are available.

Camera selection requires tradeoffs among resolution, frame rate, field of view, dynamic range, sensitivity, shutter type, interface bandwidth, weight, and processing cost. High resolution improves recognition of distant or small objects but increases data volume and inference workload. Wide-angle optics expand coverage but reduce effective pixel density and introduce distortion, requiring the sensing configuration to be optimized around mission speed, altitude, and required detection distance.

Image quality depends strongly on aircraft motion. Cargo UAVs experience vibration, attitude changes, translational motion, rotor-induced disturbances, and potentially rapid maneuvers. Rolling-shutter cameras can introduce geometric distortion when image rows are exposed at different times, while global-shutter sensors capture the frame more consistently during motion. Mechanical isolation, short exposure times, stabilization, and software motion compensation can further improve usable imagery.

Geometric calibration establishes the relationship between image pixels and rays in three-dimensional space. Intrinsic calibration estimates focal length, principal point, and lens distortion, while extrinsic calibration determines camera position and orientation relative to the UAV body frame and other sensors. Calibration accuracy directly affects visual odometry, stereo depth, sensor fusion, obstacle localization, and the projection of mapped objects between camera, LiDAR, and navigation coordinate systems.

Time synchronization is equally important because an image represents the environment at a specific aircraft pose. A timestamp error during fast flight can translate into a substantial spatial error when visual observations are fused with IMU, GNSS, LiDAR, or radar data. Hardware triggering and acquisition timestamps are preferable where available, while processing timestamps should remain distinguishable from actual exposure time so downstream algorithms can compensate for latency correctly.

Visual preprocessing prepares camera data for perception algorithms while controlling computational cost. Typical operations include distortion correction, resizing, exposure normalization, noise reduction, image rectification, and region-of-interest selection. Preprocessing must avoid removing subtle features that may correspond to distant obstacles or landing hazards. Different processing branches may therefore use different image resolutions depending on whether they support localization, detection, or semantic interpretation.

Visual feature extraction supports motion estimation and localization by identifying repeatable structures across consecutive images. Corners, edges, textured regions, or learned visual descriptors can be tracked over time to estimate relative camera motion. When combined with inertial measurements, visual-inertial estimation can provide high-rate pose information and maintain navigation during temporary GNSS degradation, although performance depends on sufficient visual structure and accurate temporal alignment.

Stereo perception estimates depth by finding corresponding image regions observed from cameras separated by a known baseline. Disparity between matched pixels can be converted into distance, producing depth maps or three-dimensional points. Stereo methods are particularly useful in textured environments and moderate ranges, but accuracy decreases with distance and can degrade over repetitive patterns, low-texture surfaces, poor illumination, or incorrect camera synchronization.

Monocular cameras cannot directly determine metric depth from a single conventional image without additional assumptions, but motion, learned depth models, known object dimensions, or fusion with other sensors can provide useful distance estimates. Monocular systems offer low weight and simple installation, making them attractive for distributed visual coverage. Their uncertainty must nevertheless be represented explicitly when distance estimates are used for collision avoidance or trajectory modification.

Object detection transforms image content into operationally meaningful entities. Neural-network detectors can identify aircraft, drones, birds, vehicles, people, buildings, towers, cranes, and other classes relevant to the mission. For autonomous flight, classification alone is insufficient; detections should also include location, confidence, temporal consistency, and preferably depth or three-dimensional position so navigation software can evaluate whether an observed object creates a collision risk.

Object tracking connects detections across multiple frames and estimates how objects move relative to the UAV. Tracking reduces instability caused by intermittent detections and supports estimation of relative direction and velocity. When camera observations are associated with vehicle motion and depth information, the system can distinguish stationary structures from moving targets and provide trajectory prediction with uncertainty to sense-and-avoid or collision-assessment functions.

Semantic segmentation assigns visual categories to image regions rather than producing only discrete object boxes. Terrain, vegetation, buildings, roads, water, sky, landing surfaces, vehicles, and people can be separated into meaningful regions. This dense interpretation is useful when the autonomy system must reason about traversable airspace, terrain boundaries, landing suitability, or environmental context instead of simply detecting predefined objects.

Camera perception is particularly important during landing because visual information can characterize surface conditions that basic altitude sensors cannot describe. Downward imagery can detect landing markers, estimate relative pose, identify surface boundaries, and evaluate hazards such as vehicles, people, debris, vegetation, excessive slope, or insufficient clear area. Visual observations can therefore contribute both to precision landing guidance and to deciding whether a candidate landing zone should be rejected.

Environmental variability remains one of the primary limitations of camera-based perception. Illumination can change from bright sunlight to shadow, twilight, night, glare, or backlighting, while rain, fog, snow, dust, lens contamination, and condensation can reduce image quality. High dynamic range sensors, thermal or infrared cameras, active cleaning, exposure control, and complementary LiDAR or radar can reduce dependence on any single visual condition.

Small and distant airborne objects are especially difficult because they may occupy only a few pixels. Birds, small drones, or approaching aircraft can initially appear as weak visual features with uncertain identity and depth. Detection performance therefore depends not only on neural-network accuracy but also on optics, resolution, frame rate, stabilization, detection range, background contrast, and temporal processing that accumulates evidence across successive observations.

Real-time perception requires balancing model capability against deterministic processing latency. High-resolution images and sophisticated neural networks can improve semantic performance but may overload onboard computing resources. Practical systems use optimized inference engines, reduced-precision computation, hardware accelerators, region-based processing, model compression, or multiple processing rates so safety-relevant outputs remain available within the timing constraints imposed by aircraft velocity.

Confidence and uncertainty should accompany visual perception outputs. Neural-network scores alone do not guarantee that an observation is physically correct, particularly when the environment differs from training data. Temporal consistency, geometric checks, sensor agreement, image-quality indicators, and out-of-distribution monitoring can provide additional evidence. Low-confidence perception should lead to conservative behavior rather than being silently treated as reliable environmental knowledge.

Camera-health monitoring should detect missing frames, frozen images, excessive latency, corrupted data, abnormal exposure, severe blur, blocked lenses, synchronization faults, overheating, and communication failures. Multiple cameras can provide overlapping coverage, but redundancy should consider common causes such as shared power, identical optics, environmental contamination, or common processing software. Health status must propagate to fusion and autonomy components when visual capability becomes degraded.

Validation requires datasets and flight tests that represent the actual operational domain. Testing should include different altitudes, speeds, attitudes, terrain types, seasons, lighting conditions, weather, obstacle sizes, backgrounds, and landing environments. Recorded flight data, simulation, synthetic imagery, software-in-the-loop, and hardware-in-the-loop testing can complement progressively more demanding flight trials while measuring detection probability, localization accuracy, false alarms, and end-to-end latency.

Camera-based perception ultimately converts visual observations into structured information that autonomous flight software can use for decisions. Its strength lies in detailed appearance and semantic information, while its weaknesses arise from lighting, visibility, depth ambiguity, motion, and computational demand. Integrated with IMU, GNSS, LiDAR, radar, mapping, and sensor fusion, camera perception becomes a major component of the cargo UAV\'s three-dimensional situational awareness and autonomous safety architecture.

카메라 기반 인지(Camera-Based Perception)는 화물 무인항공기(Cargo UAV)에 지형, 기반 시설, 장애물, 착륙 구역, 차량, 사람 및 기타 객체에 대한 풍부한 시각 정보를 제공하며, 이러한 정보는 기체 상태 센서(Vehicle-State Sensor)만으로는 충분히 표현하기 어렵다. 카메라는 상대적으로 낮은 중량과 전력 소비로 높은 공간 해상도(Spatial Resolution)를 제공하므로 항법, 객체 인식, 충돌 회피, 착륙 지원 및 운용 환경의 의미론적 이해(Semantic Understanding)에 유용하다.

카메라 아키텍처(Camera Architecture)는 관측 방향과 목적을 반영하여 설계해야 한다. 전방 카메라(Forward-Facing Camera)는 비행 경로 방향의 정보를 제공하고, 하향 카메라(Downward-Facing Camera)는 시각 기반 위치추정(Visual Localization)과 착륙을 지원하며, 측면 또는 파노라마 카메라(Panoramic Camera)는 항공기 주변의 상황 인식(Situational Awareness)을 확대할 수 있다. 다중 카메라 구성(Multi-Camera Configuration)은 사각지대를 줄일 수 있으며, 스테레오 카메라(Stereo Camera)는 충분한 텍스처, 베이스라인(Baseline), 해상도 및 영상 대응 관계가 확보될 경우 기하학적 깊이(Geometric Depth)를 제공한다.

카메라 선택에서는 해상도, 프레임률(Frame Rate), 시야각(Field of View), 동적 범위(Dynamic Range), 감도, 셔터 방식, 인터페이스 대역폭, 중량 및 처리 비용 사이의 절충이 필요하다. 높은 해상도는 멀리 있거나 작은 객체의 인식 성능을 향상시키지만 데이터량과 추론 연산 부하를 증가시킨다. 광각 렌즈(Wide-Angle Optics)는 관측 범위를 확대하지만 유효 픽셀 밀도를 낮추고 왜곡을 발생시키므로 임무 속도, 비행 고도 및 요구 탐지 거리를 기준으로 센싱 구성을 최적화해야 한다.

영상 품질(Image Quality)은 항공기의 운동에 크게 영향을 받는다. 화물 무인항공기는 진동, 자세 변화, 병진 운동, 로터로 인한 교란 및 빠른 기동을 경험할 수 있다. 롤링 셔터 카메라(Rolling-Shutter Camera)는 영상의 각 행이 서로 다른 시간에 노출되기 때문에 기하학적 왜곡을 발생시킬 수 있는 반면, 글로벌 셔터 센서(Global-Shutter Sensor)는 움직임 중에도 전체 프레임을 보다 일관되게 획득한다. 기계적 진동 절연, 짧은 노출시간, 안정화 및 소프트웨어 운동 보상(Motion Compensation)을 통해 사용 가능한 영상 품질을 추가로 향상시킬 수 있다.

기하학적 보정(Geometric Calibration)은 영상 픽셀과 3차원 공간의 광선 사이 관계를 설정한다. 내부 보정(Intrinsic Calibration)은 초점거리, 주점(Principal Point), 렌즈 왜곡을 추정하며, 외부 보정(Extrinsic Calibration)은 카메라의 위치와 방향을 무인항공기 기체 좌표계(UAV Body Frame) 및 다른 센서를 기준으로 결정한다. 보정 정확도는 시각 주행거리계(Visual Odometry), 스테레오 깊이 추정, 센서 융합, 장애물 위치추정 및 카메라·라이다·항법 좌표계 사이의 지도 객체 투영 정확도에 직접적인 영향을 미친다.

시간 동기화(Time Synchronization) 역시 중요하다. 하나의 영상은 특정 항공기 자세에서 관측된 환경을 나타내기 때문이다. 고속 비행 중 타임스탬프(Timestamp)에 오차가 발생하면 시각 관측값을 관성측정장치(IMU), 위성항법시스템(GNSS), 라이다(LiDAR), 레이더(Radar) 데이터와 융합할 때 상당한 공간 오차로 변환될 수 있다. 가능한 경우 하드웨어 트리거링(Hardware Triggering)과 획득 타임스탬프(Acquisition Timestamp)를 사용하는 것이 바람직하며, 하위 알고리즘이 지연시간을 정확하게 보상할 수 있도록 처리 시각과 실제 노출 시각을 구분해야 한다.

시각 전처리(Visual Preprocessing)는 계산 비용을 제어하면서 카메라 데이터를 인지 알고리즘에 적합한 형태로 준비한다. 일반적인 처리에는 왜곡 보정, 크기 조정, 노출 정규화(Exposure Normalization), 노이즈 감소, 영상 정류(Image Rectification), 관심영역(Region of Interest) 선택이 포함된다. 전처리 과정에서는 원거리 장애물이나 착륙 위험 요소에 해당할 수 있는 미세한 특징을 제거하지 않아야 한다. 따라서 위치추정, 탐지 또는 의미론적 해석 등 각 목적에 따라 서로 다른 영상 해상도를 사용하는 처리 분기를 구성할 수 있다.

시각 특징 추출(Visual Feature Extraction)은 연속적인 영상에서 반복적으로 관측할 수 있는 구조를 식별하여 운동 추정(Motion Estimation)과 위치추정을 지원한다. 코너, 에지, 텍스처 영역 또는 학습 기반 시각 기술자(Learned Visual Descriptor)를 시간에 따라 추적하여 카메라의 상대 운동을 추정할 수 있다. 관성 측정값과 결합하면 시각-관성 추정(Visual-Inertial Estimation)을 통해 고주기 자세 정보를 제공하고 일시적인 위성항법 성능 저하 상황에서도 항법을 유지할 수 있지만, 충분한 시각적 구조와 정확한 시간 정렬이 필요하다.

스테레오 인지(Stereo Perception)는 알려진 베이스라인만큼 떨어져 배치된 카메라에서 관측된 대응 영상 영역을 탐색하여 깊이를 추정한다. 대응 픽셀 사이의 시차(Disparity)를 거리로 변환하여 깊이 지도(Depth Map) 또는 3차원 포인트를 생성할 수 있다. 스테레오 방식은 텍스처가 풍부한 환경과 중간 거리에서 특히 유용하지만 거리가 증가할수록 정확도가 감소하며 반복적인 패턴, 낮은 텍스처 표면, 불량한 조명 또는 부정확한 카메라 동기화에서는 성능이 저하될 수 있다.

단안 카메라(Monocular Camera)는 추가적인 가정 없이 하나의 일반적인 영상만으로 절대적인 깊이를 직접 결정할 수 없지만, 카메라 운동, 학습 기반 깊이 모델(Learned Depth Model), 알려진 객체 크기 또는 다른 센서와의 융합을 이용하면 유용한 거리 추정값을 얻을 수 있다. 단안 시스템은 중량이 낮고 설치가 간단하여 분산된 시각 감시 범위를 구성하는 데 유리하지만, 거리 추정값이 충돌 회피 또는 궤적 변경에 사용될 경우 그 불확실성을 명시적으로 표현해야 한다.

객체 탐지(Object Detection)는 영상의 내용을 운용에 의미 있는 객체로 변환한다. 신경망 기반 탐지기(Neural-Network Detector)는 항공기, 드론, 조류, 차량, 사람, 건물, 타워, 크레인 및 임무와 관련된 다양한 객체를 식별할 수 있다. 자율 비행에서는 객체 분류만으로 충분하지 않으며, 탐지 결과에 위치, 신뢰도, 시간적 일관성(Temporal Consistency), 가능하다면 깊이 또는 3차원 위치를 포함하여 항법 소프트웨어가 관측된 객체의 충돌 위험 여부를 평가할 수 있도록 해야 한다.

객체 추적(Object Tracking)은 여러 프레임에 걸쳐 탐지 결과를 연결하고 객체가 무인항공기에 대해 어떻게 움직이는지를 추정한다. 추적은 간헐적인 탐지로 발생하는 불안정성을 줄이고 상대 방향과 속도 추정을 지원한다. 카메라 관측값을 기체 운동 및 깊이 정보와 결합하면 시스템은 정적인 구조물과 움직이는 표적을 구분할 수 있으며, 불확실성이 포함된 궤적 예측(Trajectory Prediction)을 감지 및 회피(Sense-and-Avoid) 또는 충돌 평가 기능에 제공할 수 있다.

의미론적 분할(Semantic Segmentation)은 개별 객체의 경계 상자만 생성하는 대신 영상 영역에 시각적 범주를 할당한다. 지형, 식생, 건물, 도로, 수면, 하늘, 착륙 표면, 차량 및 사람 등을 의미 있는 영역으로 구분할 수 있다. 이러한 밀집 해석(Dense Interpretation)은 자율 시스템이 사전에 정의된 객체를 단순히 탐지하는 수준을 넘어 비행 가능 공간, 지형 경계, 착륙 적합성 또는 주변 환경의 맥락을 판단해야 할 때 유용하다.

카메라 인지(Camera Perception)는 기본적인 고도 센서만으로 파악할 수 없는 지표면 상태를 시각 정보로 평가할 수 있기 때문에 착륙 과정에서 특히 중요하다. 하향 영상은 착륙 마커(Landing Marker)를 탐지하고 상대 자세를 추정하며 표면 경계를 식별할 수 있다. 또한 차량, 사람, 잔해, 식생, 과도한 경사 또는 충분하지 않은 안전 공간과 같은 위험 요소를 평가할 수 있어 정밀 착륙 유도(Precision Landing Guidance)뿐만 아니라 후보 착륙 구역의 사용 가능 여부를 결정하는 데 기여한다.

환경 변화(Environmental Variability)는 카메라 기반 인지의 주요 한계 중 하나이다. 조명은 강한 햇빛에서 그림자, 황혼, 야간, 눈부심 또는 역광까지 크게 변화할 수 있으며, 비, 안개, 눈, 먼지, 렌즈 오염 및 결로는 영상 품질을 저하시킬 수 있다. 높은 동적 범위 센서(High Dynamic Range Sensor), 열화상 또는 적외선 카메라(Thermal or Infrared Camera), 능동 세정, 노출 제어 및 상호보완적인 라이다나 레이더를 활용하면 특정 시각 조건에 대한 의존성을 줄일 수 있다.

작고 멀리 있는 공중 객체(Small and Distant Airborne Object)는 영상에서 몇 개의 픽셀만 차지할 수 있기 때문에 탐지가 특히 어렵다. 조류, 소형 드론 또는 접근하는 항공기는 초기에는 정체성과 깊이가 불확실한 약한 시각 특징으로 나타날 수 있다. 따라서 탐지 성능은 신경망 정확도뿐만 아니라 광학계, 해상도, 프레임률, 안정화, 탐지 거리, 배경 대비(Background Contrast), 연속적인 관측에서 증거를 누적하는 시간적 처리(Temporal Processing)에 의해 결정된다.

실시간 인지(Real-Time Perception)를 위해서는 모델 성능과 결정론적 처리 지연시간(Deterministic Processing Latency) 사이의 균형이 필요하다. 고해상도 영상과 복잡한 신경망은 의미론적 인지 성능을 향상시킬 수 있지만 온보드 컴퓨팅 자원(Onboard Computing Resource)을 과도하게 사용할 수 있다. 실제 시스템에서는 최적화된 추론 엔진(Inference Engine), 저정밀 연산(Reduced-Precision Computation), 하드웨어 가속기, 영역 기반 처리, 모델 압축(Model Compression) 또는 다중 처리 주기를 이용하여 항공기 속도가 요구하는 시간 제약 내에서 안전 관련 출력이 제공되도록 한다.

신뢰도(Confidence)와 불확실성(Uncertainty)은 시각 인지 결과와 함께 제공되어야 한다. 신경망 점수만으로는 관측 결과가 물리적으로 정확하다는 것을 보장할 수 없으며, 특히 실제 환경이 학습 데이터와 다른 경우 문제가 커질 수 있다. 시간적 일관성, 기하학적 검사, 센서 간 일치도, 영상 품질 지표 및 분포 외 데이터 감시(Out-of-Distribution Monitoring)를 이용하여 추가적인 판단 근거를 제공할 수 있다. 신뢰도가 낮은 인지 결과는 신뢰할 수 있는 환경 정보로 그대로 처리하기보다 보수적인 기체 동작으로 연결되어야 한다.

카메라 상태 감시(Camera-Health Monitoring)는 프레임 누락, 영상 정지, 과도한 지연시간, 데이터 손상, 비정상적인 노출, 심각한 영상 흐림, 렌즈 차단, 동기화 오류, 과열 및 통신 장애를 탐지해야 한다. 여러 카메라가 중첩된 감시 영역을 제공할 수 있지만 이중화(Redundancy)를 설계할 때는 공유 전원, 동일한 광학계, 환경 오염 또는 공통 처리 소프트웨어와 같은 공통원인고장(Common-Cause Failure)을 고려해야 한다. 시각 인지 기능이 저하되는 경우 상태 정보를 융합 및 자율 기능 구성요소로 전달해야 한다.

검증(Validation)에는 실제 운용 영역(Operational Domain)을 대표하는 데이터세트와 비행시험이 필요하다. 시험에는 다양한 고도, 속도, 자세, 지형 유형, 계절, 조명 조건, 기상, 장애물 크기, 배경 및 착륙 환경이 포함되어야 한다. 기록된 비행 데이터, 시뮬레이션, 합성 영상(Synthetic Imagery), 소프트웨어 인 더 루프(Software-in-the-Loop, SIL), 하드웨어 인 더 루프(Hardware-in-the-Loop, HIL) 시험을 점진적으로 난도가 높아지는 비행시험과 결합하면서 탐지 확률, 위치 정확도, 오경보(False Alarm), 종단간 지연시간(End-to-End Latency)을 측정할 수 있다.

궁극적으로 카메라 기반 인지(Camera-Based Perception)는 시각 관측을 자율 비행 소프트웨어가 의사결정에 사용할 수 있는 구조화된 정보(Structured Information)로 변환한다. 카메라의 강점은 상세한 외형 정보와 의미론적 정보에 있으며, 약점은 조명, 가시성, 깊이 모호성(Depth Ambiguity), 기체 운동 및 계산 부하에서 발생한다. 관성측정장치(IMU), 위성항법시스템(GNSS), 라이다(LiDAR), 레이더(Radar), 매핑 및 센서 융합과 통합된 카메라 인지는 화물 무인항공기의 3차원 상황 인식(Three-Dimensional Situational Awareness)과 자율 안전 아키텍처(Autonomous Safety Architecture)를 구성하는 핵심 요소가 된다.

##  

## 05.04. Radar Based Obstacle Detection Weather [w/Code]

![](images/image4.png){width="7.268055555555556in" height="7.268055555555556in"}

Radar provides cargo UAVs with an active sensing capability for detecting obstacles and observing atmospheric conditions when optical sensors become unreliable. By transmitting radio-frequency energy and analyzing reflected signals, radar can estimate range, direction, and relative radial velocity. Its ability to operate through darkness, haze, dust, and some precipitation makes it an important complementary sensor within a multi-modal UAV perception architecture.

Radar selection for a cargo UAV requires tradeoffs among operating frequency, detection range, angular resolution, field of view, update rate, antenna size, power consumption, mass, and environmental tolerance. Long-range coverage provides additional reaction time during cruise, while wider angular coverage supports maneuvering and terminal operations. The required configuration depends strongly on aircraft velocity, physical dimensions, mission altitude, and expected obstacle environment.

Frequency-modulated continuous-wave radar can estimate target distance from the frequency difference between transmitted and received signals while Doppler processing measures radial velocity. Multiple transmit and receive antennas can additionally provide angular information. These measurements produce radar detections that may be organized as range-angle maps, range-Doppler representations, target lists, or radar point clouds for downstream perception and tracking.

Radar installation must account for the electromagnetic and mechanical characteristics of the aircraft. Antenna placement should minimize blockage by the fuselage, cargo structure, landing gear, rotors, propulsion components, or other sensors. Radomes and protective covers must preserve acceptable radio-frequency transmission, while vibration, structural deformation, electromagnetic interference, and reflections from the aircraft itself should be considered during integration and calibration.

Accurate radar-to-body calibration establishes the transformation between radar measurements and the UAV coordinate system. Angular misalignment can cause a detected obstacle to appear displaced from its true flight-path relationship, particularly at long range. Calibration parameters must therefore remain associated with sensor configuration and mounting state so obstacle detections can be consistently transformed into navigation, mapping, and collision-assessment coordinate frames.

Time synchronization is essential when radar measurements are combined with IMU, GNSS, camera, or LiDAR information. During high-speed flight, even modest timing errors can shift the apparent location of a target. Measurement acquisition timestamps should therefore be preserved independently of processing or communication time, allowing vehicle motion to be compensated and enabling fusion algorithms to associate observations that represent the same physical instant.

Raw radar measurements contain both useful targets and unwanted reflections known as clutter. Terrain, buildings, vegetation, water surfaces, aircraft structures, and multipath propagation can produce strong returns that complicate obstacle detection. Signal processing therefore applies detection thresholds, clutter suppression, Doppler filtering, and spatial consistency checks to identify candidate targets without allowing persistent background reflections to dominate the perception output.

Constant false alarm rate processing can adapt detection thresholds according to the surrounding signal environment rather than applying a fixed threshold everywhere. This is useful when background energy changes with terrain or weather. Threshold selection nevertheless requires careful tuning because excessive sensitivity increases false detections, while aggressive suppression can remove weak but safety-relevant targets such as small aircraft, drones, cables, or distant structures.

Doppler information is one of radar\'s major advantages for airborne obstacle perception. A measured radial velocity helps distinguish objects with different relative motion and can support separation between static background structures and dynamic targets. Because the UAV itself is moving, however, ego-motion compensation is required. Vehicle velocity and attitude estimates must be incorporated so observed Doppler can be interpreted relative to the environment rather than only relative to the sensor.

Radar detections collected across successive measurement cycles can be associated into tracks. A tracking filter estimates target position, velocity, uncertainty, and temporal continuity while rejecting isolated detections that are inconsistent with persistent motion. Track management must handle target appearance, disappearance, missed detections, crossing trajectories, and ambiguous associations, particularly when several objects occupy similar angular regions or ranges.

Collision assessment requires more than reporting target range. The system must determine whether the relative motion between the UAV and an obstacle creates a future spatial conflict. Estimated closest point of approach, time to closest approach, relative velocity, protected aircraft volume, and uncertainty can be evaluated together. These quantities allow the sense-and-avoid system to prioritize targets that pose genuine collision risk rather than reacting equally to every radar return.

Radar is particularly valuable for detecting moving airborne objects such as other aircraft, helicopters, drones, and potentially birds. Such targets may be difficult for cameras to recognize at long distance and may produce sparse LiDAR returns. Radar velocity measurements provide an additional cue for early detection and tracking, although target size, aspect angle, radar cross section, antenna resolution, and background clutter strongly influence achievable performance.

Static obstacle detection presents different challenges because buildings, towers, cranes, terrain, and other fixed structures may have near-zero environmental velocity. Their apparent Doppler is dominated by UAV motion and viewing geometry. Combining compensated radar range and angle measurements with navigation state allows these structures to be positioned relative to the planned flight corridor, where they can be evaluated against vehicle clearance and maneuvering requirements.

Weather sensing extends radar beyond obstacle detection. Atmospheric particles and precipitation can generate measurable radar returns whose intensity and spatial distribution provide evidence of rain or other weather phenomena. For autonomous cargo operations, this information can contribute to identifying degraded flight regions and supporting route or speed decisions. Weather interpretation should remain consistent with the capabilities and frequency characteristics of the installed radar.

Rain can simultaneously be useful information and a source of perception degradation. Distributed precipitation produces numerous radar returns that can increase background energy and create apparent targets. Processing must therefore distinguish compact obstacle-like detections from spatially distributed weather echoes where possible. The perception system should communicate weather-related confidence degradation rather than silently presenting uncertain radar measurements as high-confidence physical obstacles.

Different environmental conditions affect radar differently from cameras and LiDAR. Darkness has little direct effect on radar, while fog, dust, or visual glare may degrade optical perception much more severely. Heavy precipitation can still attenuate or contaminate radar measurements depending on frequency and range. This complementary behavior is the reason radar is most effective as part of sensor diversity rather than as a universal replacement for other perception modalities.

Multi-sensor fusion can combine radar\'s range and velocity information with camera semantics and LiDAR geometry. A camera may identify what an object is, LiDAR may describe its detailed three-dimensional shape, and radar may provide robust relative velocity and range measurements under degraded visibility. Association algorithms must account for different coordinate frames, fields of view, resolutions, update rates, timestamps, and uncertainty characteristics before observations are treated as the same object.

Real-time radar processing must maintain bounded latency from signal acquisition to obstacle output. Signal transforms, detection, angle estimation, clustering, tracking, ego-motion compensation, and sensor fusion all consume processing resources. Hardware acceleration and optimized pipelines may be required when several radar units provide overlapping coverage. Safety functions should receive timely tracks even when noncritical perception workloads place heavy demand on onboard computers.

Radar health monitoring should detect communication loss, missing frames, abnormal noise floors, antenna faults, excessive interference, temperature problems, timing discontinuities, calibration inconsistencies, and implausible target patterns. Gradual degradation can be more difficult to recognize than complete failure, so consistency with vehicle motion and complementary sensors provides valuable diagnostic evidence. Degraded radar status should propagate to the fusion and autonomy layers.

The obstacle-detection system must support graceful fallback when radar capability becomes unavailable or unreliable. Camera and LiDAR information may maintain environmental perception under favorable visibility, while navigation state and operational constraints can determine whether continued flight remains acceptable. Conversely, radar may receive greater fusion weight when optical sensing deteriorates, provided radar health and measurement consistency remain within validated limits.

Validation should include representative targets, ranges, approach angles, aircraft speeds, terrain backgrounds, electromagnetic environments, and weather conditions. Simulation and recorded radar data can support algorithm development, while hardware-in-the-loop and flight tests evaluate timing, interference, mounting effects, and real target behavior. Metrics should include detection probability, false alarms, range and velocity accuracy, track continuity, processing latency, and performance under degraded weather.

Radar-based perception ultimately strengthens cargo UAV autonomy by providing a sensing channel whose failure characteristics differ from those of cameras and LiDAR. Its combination of direct range measurement, Doppler velocity, long-range detection, and resilience to several visibility limitations makes it valuable for obstacle detection and weather awareness. Integrated with navigation, tracking, sensor fusion, and sense-and-avoid functions, radar contributes to a more robust perception and flight-safety architecture.

레이더(Radar)는 광학 센서(Optical Sensor)의 신뢰성이 저하되는 환경에서도 장애물을 탐지하고 기상 상태를 관측할 수 있는 능동형 센싱 기능(Active Sensing Capability)을 화물 무인항공기(Cargo UAV)에 제공한다. 레이더는 무선주파수 에너지(Radio-Frequency Energy)를 송신하고 반사 신호를 분석하여 거리, 방향 및 상대 방사 속도(Relative Radial Velocity)를 추정한다. 어둠, 연무, 먼지 및 일부 강수 환경에서도 작동할 수 있어 다중 모달 무인항공기 인지 아키텍처(Multi-Modal UAV Perception Architecture)의 중요한 상호보완 센서가 된다.

화물 무인항공기의 레이더 선택에서는 동작 주파수, 탐지 거리, 각도 해상도(Angular Resolution), 시야각(Field of View), 갱신 주기, 안테나 크기, 전력 소비, 중량 및 환경 내성(Environmental Tolerance) 사이의 절충이 필요하다. 장거리 탐지 범위는 순항 비행 중 추가적인 대응 시간을 제공하며, 넓은 각도 범위는 기동과 종말 단계 운용(Terminal Operation)을 지원한다. 필요한 구성은 항공기 속도, 물리적 크기, 임무 고도 및 예상 장애물 환경에 크게 좌우된다.

주파수 변조 연속파 레이더(Frequency-Modulated Continuous-Wave Radar, FMCW Radar)는 송신 신호와 수신 신호 사이의 주파수 차이를 이용하여 표적 거리를 추정하고, 도플러 처리(Doppler Processing)를 통해 방사 속도를 측정할 수 있다. 다중 송수신 안테나를 사용하면 추가적인 각도 정보를 얻을 수 있다. 이러한 측정값은 거리-각도 지도(Range-Angle Map), 거리-도플러 표현(Range-Doppler Representation), 표적 목록(Target List) 또는 레이더 포인트 클라우드(Radar Point Cloud) 형태로 구성되어 후속 인지 및 추적 처리에 사용될 수 있다.

레이더 설치에서는 항공기의 전자기적 특성과 기계적 특성을 모두 고려해야 한다. 안테나 배치는 동체, 화물 구조물, 착륙장치, 로터, 추진 구성요소 또는 다른 센서에 의한 차폐를 최소화해야 한다. 레이돔(Radome)과 보호 커버는 적절한 무선주파수 투과 특성을 유지해야 하며, 통합 및 보정 과정에서는 진동, 구조 변형, 전자기 간섭(Electromagnetic Interference) 및 항공기 자체에서 발생하는 반사 신호를 고려해야 한다.

정확한 레이더-기체 보정(Radar-to-Body Calibration)은 레이더 측정값과 무인항공기 좌표계 사이의 변환 관계를 설정한다. 각도 정렬 오차는 특히 장거리에서 탐지된 장애물이 실제 비행 경로에 대한 위치와 다르게 나타나게 할 수 있다. 따라서 보정 파라미터(Calibration Parameter)는 센서 구성과 장착 상태에 연결하여 관리해야 하며, 장애물 탐지 결과를 항법, 매핑 및 충돌 평가 좌표계로 일관되게 변환할 수 있어야 한다.

레이더 측정값을 관성측정장치(IMU), 위성항법시스템(GNSS), 카메라 또는 라이다(LiDAR) 정보와 결합할 때 시간 동기화(Time Synchronization)는 필수적이다. 고속 비행에서는 비교적 작은 시간 오차도 표적의 관측 위치를 크게 이동시킬 수 있다. 따라서 측정 획득 타임스탬프(Measurement Acquisition Timestamp)를 처리 또는 통신 시간과 독립적으로 보존하여 기체 운동을 보상하고, 융합 알고리즘이 동일한 물리적 시점을 나타내는 관측값을 연계할 수 있도록 해야 한다.

원시 레이더 측정값(Raw Radar Measurement)에는 유용한 표적뿐만 아니라 클러터(Clutter)라고 하는 불필요한 반사 신호도 포함된다. 지형, 건물, 식생, 수면, 항공기 구조 및 다중경로 전파(Multipath Propagation)는 강한 반사 신호를 생성하여 장애물 탐지를 어렵게 할 수 있다. 따라서 신호 처리에서는 탐지 임계값, 클러터 억제(Clutter Suppression), 도플러 필터링 및 공간적 일관성 검사를 적용하여 지속적인 배경 반사가 인지 결과를 지배하지 않도록 하면서 후보 표적을 식별한다.

일정 오경보율 처리(Constant False Alarm Rate Processing, CFAR)는 모든 영역에 고정된 임계값을 적용하는 대신 주변 신호 환경에 따라 탐지 임계값을 조절할 수 있다. 이는 지형이나 기상 상태에 따라 배경 에너지가 변화하는 경우 유용하다. 그러나 지나치게 높은 민감도는 오탐지를 증가시키고, 과도한 억제는 소형 항공기, 드론, 케이블 또는 원거리 구조물과 같은 약하지만 안전에 중요한 표적을 제거할 수 있으므로 임계값을 신중하게 조정해야 한다.

도플러 정보(Doppler Information)는 공중 장애물 인지에서 레이더가 제공하는 주요 장점 중 하나이다. 측정된 방사 속도는 상대 운동이 서로 다른 객체를 구분하고 정적인 배경 구조물과 동적 표적을 분리하는 데 활용할 수 있다. 그러나 무인항공기 자체도 움직이므로 자기 운동 보상(Ego-Motion Compensation)이 필요하다. 관측된 도플러 정보를 센서 자체가 아닌 주변 환경을 기준으로 해석할 수 있도록 기체 속도와 자세 추정값을 반영해야 한다.

연속적인 측정 주기에서 수집된 레이더 탐지 결과는 하나의 추적 정보(Track)로 연계할 수 있다. 추적 필터(Tracking Filter)는 표적의 위치, 속도, 불확실성 및 시간적 연속성을 추정하면서 지속적인 움직임과 일치하지 않는 고립된 탐지를 제거한다. 추적 관리(Track Management)는 특히 여러 객체가 유사한 각도 영역이나 거리에 존재할 때 표적의 출현과 소멸, 탐지 누락, 교차 궤적 및 모호한 데이터 연계(Data Association)를 처리해야 한다.

충돌 평가(Collision Assessment)는 단순히 표적의 거리를 보고하는 것 이상의 처리가 필요하다. 시스템은 무인항공기와 장애물 사이의 상대 운동이 미래의 공간적 충돌을 발생시키는지 판단해야 한다. 최근접점(Closest Point of Approach), 최근접점까지의 시간(Time to Closest Approach), 상대 속도, 항공기 보호 공간(Protected Aircraft Volume) 및 불확실성을 함께 평가할 수 있다. 이를 통해 감지 및 회피 시스템(Sense-and-Avoid System)은 모든 레이더 반사에 동일하게 대응하지 않고 실제 충돌 위험이 높은 표적을 우선적으로 처리할 수 있다.

레이더는 다른 항공기, 헬리콥터, 드론 및 잠재적으로 조류와 같은 움직이는 공중 객체를 탐지하는 데 특히 유용하다. 이러한 표적은 장거리에서 카메라로 인식하기 어려울 수 있으며 라이다에서는 희소한 반사점만 생성할 수 있다. 레이더 속도 측정은 조기 탐지와 추적을 위한 추가적인 정보를 제공하지만 표적 크기, 관측 각도, 레이더 단면적(Radar Cross Section), 안테나 해상도 및 배경 클러터가 실제 성능에 큰 영향을 미친다.

정적 장애물 탐지(Static Obstacle Detection)는 다른 종류의 문제를 가진다. 건물, 타워, 크레인, 지형 및 기타 고정 구조물은 주변 환경 기준으로 거의 0에 가까운 속도를 가지므로 관측되는 도플러 성분은 주로 무인항공기의 운동과 관측 기하에 의해 결정된다. 보상된 레이더 거리 및 각도 측정값을 항법 상태와 결합하면 이러한 구조물을 계획된 비행 회랑(Planned Flight Corridor)에 상대적으로 배치하고 기체 안전거리와 기동 요구조건을 기준으로 평가할 수 있다.

기상 감지(Weather Sensing)는 레이더의 역할을 장애물 탐지 이상으로 확장한다. 대기 입자와 강수는 측정 가능한 레이더 반사 신호를 생성할 수 있으며, 그 강도와 공간적 분포는 비 또는 기타 기상 현상에 대한 정보를 제공한다. 자율 화물 운송에서는 이러한 정보가 비행 환경이 악화된 영역을 식별하고 경로나 속도 결정을 지원하는 데 활용될 수 있다. 기상 해석은 설치된 레이더의 성능과 주파수 특성 범위 내에서 수행되어야 한다.

강우(Rain)는 유용한 기상 정보가 되는 동시에 인지 성능 저하의 원인이 될 수 있다. 공간적으로 분산된 강수는 많은 레이더 반사를 생성하여 배경 에너지를 증가시키고 실제 표적처럼 보이는 신호를 만들 수 있다. 따라서 가능한 경우 처리 시스템은 집중된 장애물 형태의 탐지와 공간적으로 분산된 기상 에코(Weather Echo)를 구분해야 한다. 인지 시스템은 불확실한 레이더 측정값을 높은 신뢰도의 물리적 장애물로 처리하기보다 기상으로 인한 신뢰도 저하를 명시적으로 전달해야 한다.

환경 조건은 카메라 및 라이다와 다른 방식으로 레이더 성능에 영향을 미친다. 어둠은 레이더에 직접적인 영향을 거의 주지 않으며, 안개, 먼지 또는 시각적 눈부심은 레이더보다 광학 인지(Optical Perception)의 성능을 훨씬 크게 저하시킬 수 있다. 그러나 강한 강수는 주파수와 거리에 따라 레이더 신호를 감쇠시키거나 측정값을 오염시킬 수 있다. 이러한 상호보완적 특성 때문에 레이더는 다른 인지 방식을 완전히 대체하기보다 센서 다양성(Sensor Diversity)을 구성하는 요소로 활용할 때 가장 효과적이다.

다중 센서 융합(Multi-Sensor Fusion)은 레이더의 거리 및 속도 정보와 카메라의 의미론적 정보, 라이다의 기하학적 정보를 결합할 수 있다. 카메라는 객체의 종류를 식별하고, 라이다는 상세한 3차원 형상을 표현하며, 레이더는 가시성이 저하된 환경에서도 강건한 상대 속도와 거리 측정값을 제공할 수 있다. 관측값을 동일한 객체로 처리하기 전에 데이터 연계 알고리즘은 서로 다른 좌표계, 시야각, 해상도, 갱신 주기, 타임스탬프 및 불확실성 특성을 고려해야 한다.

실시간 레이더 처리(Real-Time Radar Processing)는 신호 획득에서 장애물 정보 출력까지 제한된 지연시간(Bounded Latency)을 유지해야 한다. 신호 변환, 탐지, 각도 추정, 클러스터링, 추적, 자기 운동 보상 및 센서 융합은 모두 처리 자원을 소비한다. 여러 레이더 장치가 중첩된 감시 범위를 제공하는 경우 하드웨어 가속과 최적화된 처리 파이프라인이 필요할 수 있으며, 비핵심 인지 작업으로 온보드 컴퓨터 부하가 증가하더라도 안전 기능에는 적시에 추적 정보가 제공되어야 한다.

레이더 상태 감시(Radar Health Monitoring)는 통신 손실, 프레임 누락, 비정상적인 노이즈 플로어(Noise Floor), 안테나 고장, 과도한 간섭, 온도 이상, 시간 불연속, 보정 불일치 및 비현실적인 표적 패턴을 탐지해야 한다. 점진적인 성능 저하는 완전한 고장보다 탐지하기 어려울 수 있으므로 기체 운동 및 상호보완 센서와의 일관성 검사가 중요한 진단 근거를 제공한다. 레이더 성능 저하 상태는 센서 융합 및 자율 기능 계층으로 전달되어야 한다.

장애물 탐지 시스템은 레이더 기능이 사용 불가능하거나 신뢰할 수 없게 되는 경우 점진적 대체 운용(Graceful Fallback)을 지원해야 한다. 가시성이 양호하다면 카메라와 라이다 정보가 환경 인지 기능을 유지할 수 있으며, 항법 상태와 운용 제약조건을 이용하여 비행 지속 가능 여부를 판단할 수 있다. 반대로 광학 센싱 성능이 저하되는 경우에는 레이더 상태와 측정 일관성이 검증된 범위에 있는 한 센서 융합에서 레이더의 가중치를 높일 수 있다.

검증(Validation)에는 실제 운용을 대표하는 표적, 거리, 접근 각도, 항공기 속도, 지형 배경, 전자기 환경 및 기상 조건이 포함되어야 한다. 시뮬레이션과 기록된 레이더 데이터를 알고리즘 개발에 활용할 수 있으며, 하드웨어 인 더 루프(Hardware-in-the-Loop, HIL)와 비행시험을 통해 시간 특성, 간섭, 장착 영향 및 실제 표적의 동작을 평가할 수 있다. 평가 지표에는 탐지 확률, 오경보(False Alarm), 거리 및 속도 정확도, 추적 연속성(Track Continuity), 처리 지연시간 및 악천후 환경에서의 성능이 포함되어야 한다.

궁극적으로 레이더 기반 인지(Radar-Based Perception)는 카메라 및 라이다와 서로 다른 고장 특성을 가진 센싱 채널을 제공함으로써 화물 무인항공기의 자율성을 강화한다. 직접적인 거리 측정, 도플러 속도, 장거리 탐지 및 다양한 가시성 제한에 대한 강건성을 결합한 레이더는 장애물 탐지와 기상 인식(Weather Awareness)에 높은 가치를 제공한다. 항법, 추적, 센서 융합 및 감지 및 회피(Sense-and-Avoid) 기능과 통합된 레이더는 더욱 강건한 인지 및 비행 안전 아키텍처(Flight-Safety Architecture)를 구성하는 핵심 요소가 된다.

##  

## 05.05. IMU GNSS Tight Coupling for UAV [w/Code]

![](images/image5.png){width="7.268055555555556in" height="7.268055555555556in"}

Inertial Measurement Unit and Global Navigation Satellite System tight coupling provides a cargo UAV with continuous, high-rate navigation by combining complementary sensor characteristics. The IMU measures angular velocity and specific force at high frequency but accumulates drift when integrated over time, while GNSS provides globally referenced position and velocity observations whose availability and accuracy can vary with satellite geometry, interference, and signal blockage.

A tightly coupled architecture differs from a loosely coupled system in the level at which GNSS information enters the navigation estimator. Rather than waiting for the GNSS receiver to generate an independent position and velocity solution, the estimator directly processes measurements associated with individual satellites. Pseudorange, Doppler, carrier-phase, or related receiver observations can therefore contribute even when the receiver cannot maintain a complete standalone navigation fix.

The inertial navigation system propagates the UAV state between external observations. Gyroscope measurements update attitude, while accelerometer measurements are transformed from the body frame into the navigation frame and integrated to estimate velocity and position. Gravity, Earth rotation, coordinate-frame effects, and sensor calibration parameters must be modeled consistently because small systematic errors can grow into substantial navigation errors during extended flight.

A practical state vector normally contains more than position, velocity, and attitude. Gyroscope bias and accelerometer bias are commonly estimated because their errors directly drive inertial drift. Depending on system requirements, the estimator may also include scale factors, GNSS receiver clock bias and drift, antenna lever-arm parameters, atmospheric terms, or other calibration states whose uncertainty significantly influences the navigation solution.

The Extended Kalman Filter is a common framework for tightly coupled IMU-GNSS estimation. The prediction stage propagates the navigation state and covariance using the inertial measurements and system dynamics. When GNSS observations arrive, the filter predicts what each satellite measurement should be, compares that prediction with the actual receiver measurement, and uses the resulting innovation to correct the navigation state and associated sensor errors.

Measurement modeling is fundamental to tight coupling because GNSS observations are related directly to satellite geometry. Satellite ephemeris and timing information are used to determine satellite position and motion, from which expected pseudorange and range-rate measurements can be calculated. Receiver clock effects, atmospheric delays, Earth rotation, antenna location, and measurement uncertainty must be treated consistently to avoid introducing systematic errors into the fusion process.

Doppler or pseudorange-rate measurements are especially useful for constraining velocity. The IMU can provide smooth short-term velocity propagation, but accelerometer bias causes gradual drift. GNSS range-rate information supplies an independent reference that limits this growth. Combining the two allows the estimator to retain the high update rate of inertial navigation while periodically correcting errors using measurements tied to the external satellite constellation.

Precise time synchronization between IMU and GNSS is critical. A GNSS observation and an inertial sample must be associated with the correct physical state of the moving aircraft. At cargo UAV speeds, even relatively small timestamp errors can become meaningful position and velocity inconsistencies. Hardware timestamps, pulse-per-second references, disciplined clocks, and deterministic acquisition pipelines can reduce temporal uncertainty before measurements reach the estimator.

Spatial alignment also requires careful treatment because the IMU and GNSS antenna are rarely located at exactly the same physical point. The antenna lever arm describes the displacement between the inertial reference point and GNSS measurement location. During rotational motion this offset produces measurable differences in velocity and position, particularly on large cargo UAVs where antennas and avionics equipment may be separated by substantial distances.

Initialization establishes attitude, position, velocity, sensor biases, and covariance before normal navigation begins. GNSS can provide an initial geographic reference, while stationary or controlled-motion periods can support inertial alignment. Heading initialization may require vehicle motion, magnetometer information, multiple GNSS antennas, or another independent reference because accelerometers alone primarily constrain the gravity direction rather than absolute yaw.

Tight coupling becomes particularly valuable when satellite visibility deteriorates. A loosely coupled estimator may lose GNSS updates when the receiver cannot generate a full navigation solution, whereas a tightly coupled estimator can continue using valid measurements from the satellites that remain visible. This capability can improve navigation continuity near structures, terrain, partial antenna masking, or other environments where satellite geometry becomes temporarily unfavorable.

GNSS measurement quality must nevertheless be monitored continuously. Low carrier-to-noise ratio, abnormal residuals, inconsistent Doppler, multipath, cycle slips, satellite faults, jamming, or spoofing can make individual observations unreliable. Measurement screening and innovation testing can reject or down-weight suspicious inputs so corrupted satellite data do not force an otherwise stable inertial solution toward an incorrect navigation state.

Integrity monitoring is especially important for large autonomous cargo aircraft because navigation errors propagate into flight control, route tracking, geofence enforcement, collision avoidance, and landing decisions. The estimator should expose not only its best state estimate but also covariance, measurement validity, sensor health, and integrity indicators. Downstream autonomy functions can then adapt operational authority according to the confidence available in the navigation solution.

GNSS outages illustrate the complementary relationship between the two sensing systems. When satellite measurements disappear, the inertial system can continue propagating position, velocity, and attitude without interruption, but uncertainty grows with time because sensor biases are no longer externally corrected. The estimator should therefore increase covariance realistically rather than presenting a continuously propagated inertial solution with an unjustifiably high level of confidence.

Recovery after a GNSS outage must also be controlled carefully. Newly restored measurements may disagree significantly with the inertially propagated state, especially after a long interruption. Immediately applying a large correction can create discontinuities that disturb flight-control or guidance functions. Innovation gating, staged measurement acceptance, covariance-aware correction, and navigation-state smoothing can support stable convergence when reliable satellite observations return.

High-precision cargo UAV operations may incorporate differential GNSS or Real-Time Kinematic corrections. These techniques can substantially improve positioning accuracy when correction data and carrier-phase observations remain valid. Tight integration with inertial sensing helps bridge short interruptions and maintain smooth motion estimates, but the system must separately represent whether it currently has conventional GNSS, differential, float, or fixed high-precision positioning capability.

Redundant navigation architectures can include multiple IMUs, GNSS receivers, antennas, or independent estimator instances. Redundancy supports fault detection and continued operation, but common-mode failures remain possible. Shared antennas, common power supplies, identical receiver software, electromagnetic interference, or satellite-system disturbances can affect several channels simultaneously, so independence must be evaluated at the architectural level rather than assumed from device count alone.

Real-time implementation requires deterministic handling of sensors operating at very different rates. IMU data may arrive hundreds or thousands of times per second, while GNSS measurements typically update much more slowly. The estimator must propagate states efficiently between GNSS epochs, preserve precise timing, handle delayed observations, and publish navigation outputs at a rate appropriate for flight control without allowing communication or processing jitter to create inconsistent states.

Delayed measurements require particular attention because GNSS receiver processing and communication can introduce latency. If an observation is applied to the estimator\'s current state even though it represents an earlier instant, the correction becomes physically inconsistent. Navigation software can maintain state history, propagate measurements to a common time, or perform delayed-state updates so measurement timing is respected without unnecessarily delaying the real-time flight-control output.

The tightly coupled navigation solution should interface cleanly with the broader perception and autonomy architecture. Position, velocity, attitude, angular rates, uncertainty, reference-frame information, timestamps, and sensor-health status can be published as standardized state products. LiDAR mapping, camera perception, radar tracking, route planning, and flight control can then consume a common navigation reference instead of implementing separate interpretations of raw inertial and satellite measurements.

Validation should include nominal satellite coverage as well as deliberately degraded conditions. Simulation, recorded sensor data, software-in-the-loop, hardware-in-the-loop, and flight testing can introduce satellite loss, measurement noise, bias changes, timing errors, multipath, receiver resets, jamming-like interference, and extended GNSS outages. Position, velocity, attitude, drift rate, recovery behavior, integrity response, and end-to-end latency should be evaluated throughout these scenarios.

Ultimately, tightly coupled IMU-GNSS fusion provides the cargo UAV with a navigation backbone that combines the short-term continuity of inertial sensing with the long-term geographic stability of satellite navigation. Its effectiveness depends on accurate sensor models, synchronization, calibration, uncertainty propagation, measurement integrity, and fault management. Properly integrated, it supplies a continuous and confidence-aware state estimate for flight control, perception, autonomous navigation, safety monitoring, and mission execution.

관성측정장치-위성항법시스템 긴밀 결합(IMU-GNSS Tight Coupling)은 상호보완적인 센서 특성을 결합하여 화물 무인항공기(Cargo UAV)에 연속적이고 높은 주기의 항법 정보를 제공한다. 관성측정장치(IMU)는 높은 주파수로 각속도와 비력(Specific Force)을 측정하지만 시간에 따라 적분하면 드리프트(Drift)가 누적되는 반면, 위성항법시스템(GNSS)은 전역 기준 위치와 속도 관측값을 제공하지만 위성 배치, 간섭 및 신호 차단에 따라 가용성과 정확도가 달라질 수 있다.

긴밀 결합 아키텍처(Tightly Coupled Architecture)는 위성항법 정보가 항법 추정기(Navigation Estimator)에 입력되는 수준에서 느슨한 결합 시스템(Loosely Coupled System)과 차이가 있다. 위성항법 수신기가 독립적인 위치 및 속도 해를 생성할 때까지 기다리는 대신 추정기가 개별 위성과 관련된 측정값을 직접 처리한다. 따라서 의사거리(Pseudorange), 도플러(Doppler), 반송파 위상(Carrier Phase) 또는 관련 수신기 관측값은 수신기가 완전한 독립 항법 해를 유지하지 못하는 상황에서도 활용될 수 있다.

관성항법시스템(Inertial Navigation System)은 외부 관측값이 입력되는 사이에 무인항공기의 상태를 전파한다. 자이로스코프 측정값은 자세(Attitude)를 갱신하고, 가속도계 측정값은 기체 좌표계(Body Frame)에서 항법 좌표계(Navigation Frame)로 변환된 후 적분되어 속도와 위치를 추정한다. 작은 체계적 오차도 장시간 비행에서는 상당한 항법 오차로 증가할 수 있으므로 중력, 지구 회전, 좌표계 효과 및 센서 보정 파라미터를 일관되게 모델링해야 한다.

실용적인 상태 벡터(State Vector)는 일반적으로 위치, 속도 및 자세 이상의 정보를 포함한다. 자이로스코프 바이어스(Gyroscope Bias)와 가속도계 바이어스(Accelerometer Bias)는 관성 드리프트를 직접적으로 발생시키므로 일반적으로 추정 대상에 포함된다. 시스템 요구조건에 따라 스케일 팩터(Scale Factor), 위성항법 수신기 클록 바이어스와 드리프트, 안테나 레버암(Lever Arm) 파라미터, 대기 관련 항 또는 항법 해에 상당한 영향을 미치는 기타 보정 상태도 추정기에 포함할 수 있다.

확장 칼만 필터(Extended Kalman Filter, EKF)는 긴밀 결합 관성측정장치-위성항법 추정에 일반적으로 사용되는 프레임워크이다. 예측 단계(Prediction Stage)에서는 관성 측정값과 시스템 동역학을 이용하여 항법 상태와 공분산(Covariance)을 전파한다. 위성항법 관측값이 입력되면 필터는 각 위성 측정값의 예상값을 계산하고 이를 실제 수신기 측정값과 비교한 후 발생한 이노베이션(Innovation)을 이용하여 항법 상태와 관련 센서 오차를 보정한다.

측정 모델링(Measurement Modeling)은 위성항법 관측값이 위성의 기하학적 배치와 직접 연결되므로 긴밀 결합의 핵심 요소이다. 위성 궤도력(Ephemeris)과 시간 정보를 이용하여 위성의 위치와 운동을 결정하고, 이를 기반으로 예상 의사거리와 거리 변화율(Range Rate)을 계산할 수 있다. 수신기 클록 효과, 대기 지연, 지구 회전, 안테나 위치 및 측정 불확실성을 일관되게 처리하여 센서 융합 과정에 체계적인 오차가 유입되지 않도록 해야 한다.

도플러 또는 의사거리 변화율(Pseudorange-Rate) 측정값은 속도를 제한하는 데 특히 유용하다. 관성측정장치는 부드러운 단기 속도 전파를 제공하지만 가속도계 바이어스로 인해 점진적인 드리프트가 발생한다. 위성항법 거리 변화율 정보는 이러한 오차 증가를 제한하는 독립적인 기준을 제공한다. 두 정보를 결합하면 관성항법의 높은 갱신 주기를 유지하면서 외부 위성군에 연결된 측정값을 이용하여 오차를 주기적으로 보정할 수 있다.

관성측정장치와 위성항법시스템 사이의 정밀한 시간 동기화(Time Synchronization)는 매우 중요하다. 위성항법 관측값과 관성 샘플은 이동하는 항공기의 정확한 물리적 상태와 대응되어야 한다. 화물 무인항공기의 비행 속도에서는 비교적 작은 타임스탬프 오차도 의미 있는 위치 및 속도 불일치로 이어질 수 있다. 하드웨어 타임스탬프(Hardware Timestamp), 초당 펄스(Pulse Per Second, PPS) 기준, 동기화된 클록 및 결정론적 데이터 획득 파이프라인을 통해 측정값이 추정기에 도달하기 전에 시간적 불확실성을 줄일 수 있다.

관성측정장치와 위성항법 안테나는 일반적으로 정확히 동일한 물리적 위치에 설치되지 않으므로 공간 정렬(Spatial Alignment)도 신중하게 처리해야 한다. 안테나 레버암(Antenna Lever Arm)은 관성 기준점과 위성항법 측정 위치 사이의 변위를 나타낸다. 회전 운동이 발생하면 이러한 오프셋으로 인해 측정 가능한 위치 및 속도 차이가 발생하며, 안테나와 항공전자 장비가 상당한 거리만큼 떨어질 수 있는 대형 화물 무인항공기에서는 특히 중요하다.

초기화(Initialization)는 정상적인 항법이 시작되기 전에 자세, 위치, 속도, 센서 바이어스 및 공분산을 설정하는 과정이다. 위성항법시스템은 초기 지리적 기준을 제공할 수 있으며, 정지 상태 또는 제어된 운동 구간은 관성 정렬(Inertial Alignment)을 지원할 수 있다. 가속도계만으로는 주로 중력 방향을 결정할 수 있고 절대 요(Yaw)를 직접 결정하기 어려우므로 방위 초기화에는 기체 운동, 자기계(Magnetometer), 다중 위성항법 안테나 또는 다른 독립적인 기준이 필요할 수 있다.

긴밀 결합은 위성 가시성(Satellite Visibility)이 저하될 때 특히 유용하다. 느슨한 결합 추정기는 수신기가 완전한 항법 해를 생성하지 못하면 위성항법 갱신 자체를 상실할 수 있지만, 긴밀 결합 추정기는 여전히 관측 가능한 위성의 유효한 측정값을 계속 사용할 수 있다. 이러한 기능은 구조물이나 지형 주변, 부분적인 안테나 차폐 또는 위성 기하 구조가 일시적으로 불리해지는 환경에서 항법 연속성(Navigation Continuity)을 향상시킬 수 있다.

그러나 위성항법 측정 품질은 지속적으로 감시해야 한다. 낮은 반송파 대 잡음비(Carrier-to-Noise Ratio), 비정상적인 잔차(Residual), 일관되지 않은 도플러, 다중경로(Multipath), 사이클 슬립(Cycle Slip), 위성 고장, 재밍(Jamming) 또는 스푸핑(Spoofing)은 개별 관측값의 신뢰성을 저하시킬 수 있다. 측정값 선별(Measurement Screening)과 이노베이션 검사(Innovation Testing)를 이용하여 의심스러운 입력을 제거하거나 가중치를 낮춤으로써 손상된 위성 데이터가 안정적인 관성항법 해를 잘못된 상태로 이동시키는 것을 방지할 수 있다.

무거운 화물을 운송하는 대형 자율 항공기에서는 항법 오차가 비행 제어, 경로 추종, 지오펜스 적용(Geofence Enforcement), 충돌 회피 및 착륙 의사결정으로 전파되므로 무결성 감시(Integrity Monitoring)가 특히 중요하다. 추정기는 최적 상태 추정값뿐만 아니라 공분산, 측정 유효성, 센서 상태 및 무결성 지표(Integrity Indicator)를 함께 제공해야 한다. 이를 통해 하위 자율 기능은 항법 해에 대한 현재 신뢰도에 따라 운용 권한을 조정할 수 있다.

위성항법 중단(GNSS Outage)은 두 센싱 시스템의 상호보완적인 관계를 명확하게 보여준다. 위성 측정값이 사라지면 관성 시스템은 위치, 속도 및 자세를 중단 없이 계속 전파할 수 있지만 센서 바이어스를 외부 기준으로 더 이상 보정할 수 없기 때문에 시간이 지날수록 불확실성이 증가한다. 따라서 추정기는 관성항법 해를 계속 출력하면서 부당하게 높은 신뢰도를 유지하는 대신 실제 오차 증가에 맞추어 공분산을 현실적으로 증가시켜야 한다.

위성항법 중단 이후의 복구(Recovery) 역시 신중하게 제어해야 한다. 특히 장시간 신호가 중단된 경우 새롭게 복구된 측정값은 관성항법으로 전파된 상태와 상당한 차이를 보일 수 있다. 큰 보정값을 즉시 적용하면 비행 제어나 유도 기능을 방해하는 불연속성이 발생할 수 있다. 이노베이션 게이팅(Innovation Gating), 단계적 측정 수용, 공분산 기반 보정 및 항법 상태 평활화(Navigation-State Smoothing)를 이용하여 신뢰할 수 있는 위성 관측값이 복구될 때 안정적인 수렴을 지원할 수 있다.

고정밀 화물 무인항공기 운용에서는 차분 위성항법(Differential GNSS) 또는 실시간 이동측위(Real-Time Kinematic, RTK)를 적용할 수 있다. 이러한 기술은 보정 데이터와 반송파 위상 관측값이 유효하게 유지될 경우 측위 정확도를 크게 향상시킬 수 있다. 관성 센싱과의 긴밀한 통합은 짧은 신호 중단 구간을 연결하고 부드러운 운동 추정을 유지하는 데 도움이 되지만, 시스템은 현재 일반 위성항법, 차분 방식, 플로트(Float) 또는 고정밀 고정해(Fixed Solution) 중 어떤 측위 기능을 제공하고 있는지를 구분하여 표현해야 한다.

이중화 항법 아키텍처(Redundant Navigation Architecture)는 다중 관성측정장치, 위성항법 수신기, 안테나 또는 독립적인 추정기 인스턴스를 포함할 수 있다. 이중화는 고장 탐지와 지속적인 운용을 지원하지만 공통원인고장(Common-Mode Failure)은 여전히 발생할 수 있다. 공유 안테나, 공통 전원, 동일한 수신기 소프트웨어, 전자기 간섭 또는 위성 시스템 자체의 이상은 여러 채널에 동시에 영향을 미칠 수 있으므로 단순한 장치 수가 아니라 전체 아키텍처 수준에서 독립성을 평가해야 한다.

실시간 구현(Real-Time Implementation)에서는 매우 다른 주기로 동작하는 센서를 결정론적으로 처리해야 한다. 관성측정장치 데이터는 초당 수백 회 또는 수천 회 입력될 수 있지만 위성항법 측정값은 일반적으로 훨씬 낮은 주기로 갱신된다. 추정기는 위성항법 갱신 사이에서 상태를 효율적으로 전파하고 정확한 시간을 유지하며 지연된 관측값을 처리하는 동시에 통신 또는 처리 지터(Jitter)로 인해 상태 불일치가 발생하지 않도록 비행 제어에 적합한 주기로 항법 결과를 제공해야 한다.

지연된 측정값(Delayed Measurement)은 위성항법 수신기의 처리와 통신 과정에서 지연시간이 발생할 수 있기 때문에 특별한 주의가 필요하다. 이전 시점을 나타내는 관측값을 현재 추정 상태에 직접 적용하면 물리적으로 일관되지 않은 보정이 발생한다. 항법 소프트웨어는 상태 이력(State History)을 유지하거나 측정값을 공통 시간으로 전파하거나 지연 상태 갱신(Delayed-State Update)을 수행하여 실시간 비행 제어 출력을 불필요하게 지연시키지 않으면서 측정 시점을 정확하게 반영할 수 있다.

긴밀 결합 항법 해(Tightly Coupled Navigation Solution)는 전체 인지 및 자율 아키텍처와 명확한 인터페이스를 구성해야 한다. 위치, 속도, 자세, 각속도, 불확실성, 기준 좌표계 정보, 타임스탬프 및 센서 상태를 표준화된 상태 정보(Standardized State Product)로 제공할 수 있다. 이를 통해 라이다 매핑, 카메라 인지, 레이더 추적, 경로 계획 및 비행 제어 기능은 원시 관성 및 위성 측정값을 각각 별도로 해석하지 않고 공통된 항법 기준을 사용할 수 있다.

검증(Validation)에는 정상적인 위성 가시 환경뿐만 아니라 의도적으로 성능을 저하시킨 조건도 포함해야 한다. 시뮬레이션, 기록된 센서 데이터, 소프트웨어 인 더 루프(Software-in-the-Loop, SIL), 하드웨어 인 더 루프(Hardware-in-the-Loop, HIL), 비행시험을 통해 위성 손실, 측정 노이즈, 바이어스 변화, 시간 오차, 다중경로, 수신기 재시작, 재밍과 유사한 간섭 및 장시간 위성항법 중단을 주입할 수 있다. 이러한 시나리오에서 위치, 속도, 자세, 드리프트율, 복구 동작, 무결성 대응 및 종단간 지연시간을 평가해야 한다.

궁극적으로 긴밀 결합 관성측정장치-위성항법 융합(Tightly Coupled IMU-GNSS Fusion)은 관성 센싱의 단기적인 연속성과 위성항법의 장기적인 지리적 안정성을 결합하여 화물 무인항공기의 핵심 항법 기반(Navigation Backbone)을 제공한다. 그 효과는 정확한 센서 모델, 동기화, 보정, 불확실성 전파(Uncertainty Propagation), 측정 무결성 및 고장 관리에 의해 결정된다. 적절하게 통합된 시스템은 비행 제어, 인지, 자율 항법, 안전 감시 및 임무 수행을 위한 연속적이고 신뢰도를 인식하는 상태 추정값(Confidence-Aware State Estimate)을 제공한다.

##  

## 05.06. Multi Sensor Fusion EKF for UAV State [w/Code]

![](images/image6.png){width="7.268055555555556in" height="7.268055555555556in"}

Multi-sensor fusion using an Extended Kalman Filter provides a cargo UAV with a unified estimate of position, velocity, attitude, angular motion, and sensor errors from measurements that differ in rate, accuracy, availability, and physical principle. The estimator combines high-rate inertial propagation with corrections from GNSS, cameras, LiDAR, radar, altimeters, magnetometers, and other sensors so downstream systems can operate from one coherent vehicle state.

The Extended Kalman Filter is well suited to UAV state estimation because aircraft motion and sensor observation models are nonlinear. Instead of applying a purely linear estimator, the EKF propagates a nonlinear state model while approximating uncertainty evolution through local linearization. This allows the system to represent both the estimated vehicle state and the covariance describing how uncertain each state and the relationships between states currently are.

A typical UAV state vector contains three-dimensional position, velocity, and attitude together with gyroscope and accelerometer biases. Additional states may represent magnetometer bias, wind, barometric offset, terrain-relative altitude, GNSS clock terms, or calibration parameters when required by the architecture. State selection should remain observable from available measurements because unnecessary or weakly observable states can reduce estimator stability and complicate tuning.

The IMU normally drives the EKF prediction stage because it provides measurements at the highest rate. Gyroscope data propagate orientation, while accelerometer measurements are rotated into the navigation frame and used to propagate velocity and position. Process noise represents uncertainty introduced by sensor noise, bias instability, imperfect dynamics, and unmodeled effects, allowing covariance to grow appropriately between external measurement updates.

Attitude can be represented using quaternions to avoid the singularities associated with Euler-angle propagation. Many practical estimators use an error-state formulation in which the nominal orientation is maintained as a quaternion while small attitude errors are represented locally in the filter state. This approach supports stable high-rate inertial integration while allowing measurement updates to correct orientation without treating the quaternion components as independent unconstrained variables.

The measurement-update stage incorporates observations that constrain selected parts of the predicted state. GNSS may provide position and velocity, a barometer may constrain altitude, a magnetometer may contribute heading information, and radar or laser altimeters may measure terrain-relative height. Each measurement requires an observation model that predicts what the sensor should report from the current state and expresses the expected measurement uncertainty.

Camera and LiDAR measurements can contribute through visual odometry, visual-inertial estimation, scan matching, map localization, or relative pose observations. These sensors do not necessarily produce direct global position measurements. Their outputs may instead constrain relative motion between states or provide pose with respect to a local map. The fusion interface must therefore define coordinate frames, timestamps, covariance, and measurement semantics precisely before an update is applied.

Radar can contribute range, velocity, altitude, or tracked-object information depending on the sensor configuration and navigation architecture. For vehicle-state estimation, measurements should only be fused when their relationship to the UAV state is sufficiently defined. Environmental target tracks, for example, are normally perception products rather than direct vehicle-state observations unless known landmarks or terrain references provide a valid geometric constraint.

Innovation is the difference between an actual sensor measurement and the value predicted from the current state estimate. The EKF uses this residual together with predicted state uncertainty and measurement covariance to calculate how strongly the state should be corrected. A precise, consistent measurement receives greater influence, whereas a noisy measurement contributes less. This probabilistic weighting is central to combining heterogeneous UAV sensors without treating them as equally reliable.

Innovation monitoring also provides an important mechanism for detecting abnormal measurements. A residual that is unexpectedly large relative to its predicted covariance can indicate sensor failure, incorrect association, multipath, calibration error, environmental interference, or a temporary model mismatch. Statistical gating can reject such observations or reduce their influence before they destabilize the fused state, while repeated rejection can trigger higher-level sensor-health logic.

Asynchronous sensor operation is a fundamental design issue because the sensors do not update simultaneously. IMU measurements may arrive at hundreds or thousands of hertz, while GNSS, cameras, LiDAR, radar, and altimeters operate at different rates. The estimator must associate every observation with its acquisition time and propagate the state to the appropriate measurement epoch rather than assuming that all received data describe the aircraft at the current processing instant.

Delayed measurements require state-history management when perception pipelines introduce significant processing latency. A camera pose or LiDAR localization result may represent an aircraft state tens or hundreds of milliseconds earlier. Applying it directly to the latest state can introduce false corrections. The estimator can retain previous states and covariances, apply the delayed update at the correct time, and then repropagate the solution toward the present using buffered inertial measurements.

Coordinate-frame consistency is equally critical. Measurements may be expressed in sensor frames, the UAV body frame, a local navigation frame, an Earth-fixed frame, or a map frame. Known rigid transformations and navigation-frame definitions must be applied consistently. Frame errors can appear deceptively similar to sensor bias, so explicit frame identifiers and validated transformations should accompany every fusion interface rather than relying on implicit conventions.

Sensor lever arms must be modeled when measurements originate from physically separated locations. A GNSS antenna, camera, LiDAR, radar, and IMU mounted at different points on a large cargo UAV experience different instantaneous velocities during rotational motion. Transforming every measurement as though it originated at the vehicle center can create systematic errors, particularly during aggressive attitude changes or when sensor separation is several meters.

Fusion does not require every sensor to be active continuously. The EKF should support changing measurement availability while preserving a valid state and realistic uncertainty. During normal flight, GNSS may dominate long-term position correction; near structures, visual or LiDAR localization may become more important; during landing, radar or laser altitude measurements may receive greater relevance. The prediction model provides continuity while measurement sources enter and leave according to validity.

Sensor failure must cause uncertainty redistribution rather than an immediate collapse of the estimator. If GNSS becomes unavailable, inertial propagation can maintain continuity while covariance grows. If a camera becomes unreliable because of darkness or glare, its updates can be suspended while LiDAR or radar remains available. This graceful degradation depends on each sensor supplying explicit health and confidence information instead of silently generating measurements under invalid conditions.

Correlated measurements require careful handling because two apparently different observations may depend on the same underlying information. Fusing a navigation solution and another estimate derived from that same navigation solution as though they were independent can make covariance unrealistically small. The architecture should identify shared information paths and avoid double counting, especially when combining outputs from nested estimators, visual-inertial systems, or externally fused navigation devices.

Estimator tuning determines how process uncertainty and measurement uncertainty are balanced. Process-noise parameters that are too small can make the filter overconfident in its motion model, while excessively large values can produce noisy state estimates. Similarly, unrealistic measurement covariance can cause sensors to dominate or be ignored incorrectly. Tuning should therefore be supported by sensor characterization, recorded flight data, residual statistics, and representative operating conditions.

The fused state should expose more than a single best estimate. Position, velocity, attitude, angular motion, estimated biases, timestamps, coordinate frames, covariance, sensor contribution status, and integrity indicators provide downstream systems with information about both state and confidence. Flight control may require high-rate attitude and velocity, while navigation, mapping, obstacle avoidance, and mission management may consume lower-rate products with explicit uncertainty.

Real-time implementation must preserve deterministic propagation and bounded update latency even when perception workloads fluctuate. High-rate inertial processing should not be blocked by expensive camera, LiDAR, or radar algorithms. Queue management, timestamp ordering, bounded buffers, prioritized execution, and separate processing domains can ensure that delayed perception observations are incorporated without interrupting the continuous state stream required by flight-control software.

Initialization and reset behavior require explicit design because the EKF cannot assume that all states are accurately known at startup. Initial position and velocity may come from GNSS, gravity can constrain roll and pitch, and heading may require motion, magnetometer data, multiple GNSS antennas, or another reference. Covariance should represent the actual uncertainty of initialization, allowing subsequent measurements to converge the estimator rather than forcing unjustified initial certainty.

Validation must test nominal operation together with sensor faults, dropouts, timing errors, calibration errors, delayed measurements, inconsistent coordinate frames, GNSS outages, visual degradation, and rapidly changing flight dynamics. Simulation, recorded-data replay, software-in-the-loop, hardware-in-the-loop, and flight testing can measure estimation error, covariance consistency, innovation behavior, convergence, drift, recovery time, and computational latency across these conditions.

A multi-sensor EKF ultimately forms the state-estimation core connecting UAV sensing to autonomous flight. Its purpose is not merely to average sensor outputs but to combine motion models, measurement physics, timing, calibration, uncertainty, and sensor health into a continuously qualified estimate of aircraft state. Properly engineered, this fused state provides the common reference required by flight control, mapping, perception, obstacle avoidance, route execution, landing, and safety monitoring.

확장 칼만 필터(Extended Kalman Filter, EKF)를 이용한 다중 센서 융합(Multi-Sensor Fusion)은 측정 주기, 정확도, 가용성 및 물리적 원리가 서로 다른 센서 정보를 결합하여 화물 무인항공기(Cargo UAV)의 위치, 속도, 자세, 각운동 및 센서 오차에 대한 통합된 추정값을 제공한다. 추정기는 고주기 관성 전파(Inertial Propagation)를 위성항법시스템(GNSS), 카메라, 라이다(LiDAR), 레이더(Radar), 고도계, 자기계 및 기타 센서의 보정 정보와 결합하여 하위 시스템이 하나의 일관된 기체 상태(Coherent Vehicle State)를 기반으로 동작하도록 한다.

확장 칼만 필터(EKF)는 항공기 운동과 센서 관측 모델이 비선형(Nonlinear)이기 때문에 무인항공기 상태 추정에 적합하다. 순수한 선형 추정기(Linear Estimator)를 적용하는 대신 EKF는 비선형 상태 모델을 전파하면서 국부적인 선형화(Local Linearization)를 통해 불확실성의 변화를 근사한다. 이를 통해 시스템은 추정된 기체 상태뿐만 아니라 각 상태의 현재 불확실성과 상태 사이의 상관관계를 나타내는 공분산(Covariance)을 함께 표현할 수 있다.

일반적인 무인항공기 상태 벡터(State Vector)는 3차원 위치, 속도 및 자세와 함께 자이로스코프 바이어스(Gyroscope Bias)와 가속도계 바이어스(Accelerometer Bias)를 포함한다. 아키텍처 요구조건에 따라 자기계 바이어스, 바람, 기압 고도 오프셋(Barometric Offset), 지형 상대 고도, 위성항법 클록 관련 항 또는 보정 파라미터를 추가적인 상태로 포함할 수 있다. 불필요하거나 관측 가능성(Observability)이 낮은 상태는 추정기의 안정성을 저하시키고 튜닝을 어렵게 할 수 있으므로 상태 선택은 사용 가능한 측정값으로 충분히 관측 가능한 범위에서 이루어져야 한다.

관성측정장치(IMU)는 가장 높은 주기로 측정값을 제공하므로 일반적으로 EKF의 예측 단계(Prediction Stage)를 구동한다. 자이로스코프 데이터는 자세를 전파하고, 가속도계 측정값은 항법 좌표계(Navigation Frame)로 회전 변환된 후 속도와 위치를 전파하는 데 사용된다. 프로세스 노이즈(Process Noise)는 센서 노이즈, 바이어스 불안정성, 불완전한 동역학 및 모델링되지 않은 효과에서 발생하는 불확실성을 표현하여 외부 측정값 갱신 사이에서 공분산이 적절하게 증가하도록 한다.

자세(Attitude)는 오일러 각(Euler Angle) 전파에서 발생하는 특이점(Singularity)을 피하기 위해 쿼터니언(Quaternion)을 이용하여 표현할 수 있다. 많은 실제 추정기는 명목 자세(Nominal Orientation)를 쿼터니언으로 유지하면서 작은 자세 오차를 필터 상태에서 국부적으로 표현하는 오차 상태 방식(Error-State Formulation)을 사용한다. 이 방식은 안정적인 고주기 관성 적분을 지원하면서 쿼터니언 성분을 서로 독립적인 비제약 변수로 처리하지 않고 측정값 갱신을 통해 자세를 보정할 수 있게 한다.

측정 갱신 단계(Measurement-Update Stage)는 예측된 상태의 특정 부분을 제한하는 관측값을 통합한다. 위성항법시스템은 위치와 속도를 제공할 수 있고, 기압계(Barometer)는 고도를 제한하며, 자기계(Magnetometer)는 방위 정보를 제공할 수 있다. 또한 레이더 또는 레이저 고도계(Radar or Laser Altimeter)는 지형 상대 높이를 측정할 수 있다. 각각의 측정값에는 현재 상태를 기반으로 센서가 어떤 값을 출력해야 하는지를 예측하고 예상 측정 불확실성을 표현하는 관측 모델(Observation Model)이 필요하다.

카메라와 라이다 측정값은 시각 주행거리계(Visual Odometry), 시각-관성 추정(Visual-Inertial Estimation), 스캔 정합(Scan Matching), 지도 기반 위치추정(Map Localization) 또는 상대 자세 관측(Relative Pose Observation)을 통해 상태 추정에 기여할 수 있다. 이러한 센서는 반드시 직접적인 전역 위치 측정값을 생성하는 것은 아니다. 대신 상태 사이의 상대 운동을 제한하거나 지역 지도에 대한 자세를 제공할 수 있으므로 측정값 갱신을 적용하기 전에 융합 인터페이스에서 좌표계, 타임스탬프, 공분산 및 측정값의 의미를 정확하게 정의해야 한다.

레이더(Radar)는 센서 구성과 항법 아키텍처에 따라 거리, 속도, 고도 또는 추적 객체 정보를 제공할 수 있다. 기체 상태 추정에서는 해당 측정값과 무인항공기 상태 사이의 관계가 충분히 정의된 경우에만 측정값을 융합해야 한다. 예를 들어 환경 표적 추적(Environmental Target Track)은 일반적으로 직접적인 기체 상태 관측값이 아니라 인지 결과이므로 알려진 랜드마크나 지형 기준이 유효한 기하학적 제약을 제공하지 않는 한 기체 상태 추정에 직접 사용하지 않는다.

이노베이션(Innovation)은 실제 센서 측정값과 현재 상태 추정값에서 예측된 측정값 사이의 차이를 의미한다. EKF는 이 잔차(Residual)를 예측 상태 불확실성과 측정 공분산과 함께 사용하여 상태를 어느 정도 보정해야 하는지를 계산한다. 정확하고 일관된 측정값은 더 큰 영향을 주고 노이즈가 많은 측정값은 상대적으로 적은 영향을 준다. 이러한 확률적 가중(Probabilistic Weighting)은 서로 다른 종류의 무인항공기 센서를 동일한 신뢰도로 취급하지 않고 결합하기 위한 핵심 원리이다.

이노베이션 감시(Innovation Monitoring)는 비정상적인 측정값을 탐지하는 중요한 메커니즘도 제공한다. 예측 공분산과 비교하여 예상보다 지나치게 큰 잔차는 센서 고장, 잘못된 데이터 연계(Data Association), 다중경로(Multipath), 보정 오차, 환경 간섭 또는 일시적인 모델 불일치를 나타낼 수 있다. 통계적 게이팅(Statistical Gating)을 통해 이러한 관측값을 제거하거나 영향력을 줄여 융합 상태가 불안정해지는 것을 방지할 수 있으며, 반복적인 측정 거부는 상위 수준의 센서 상태 관리 로직을 작동시키는 근거가 될 수 있다.

비동기 센서 동작(Asynchronous Sensor Operation)은 센서가 동시에 갱신되지 않기 때문에 핵심적인 설계 문제이다. 관성측정장치 데이터는 초당 수백 회 또는 수천 회 입력될 수 있지만 위성항법시스템, 카메라, 라이다, 레이더 및 고도계는 서로 다른 주기로 동작한다. 추정기는 수신된 모든 데이터가 현재 처리 시점의 항공기 상태를 나타낸다고 가정하지 않고 각각의 관측값을 실제 획득 시점과 연결하여 해당 측정 시점까지 상태를 정확하게 전파해야 한다.

인지 파이프라인(Perception Pipeline)에서 상당한 처리 지연이 발생하는 경우 지연 측정값(Delayed Measurement)을 처리하기 위한 상태 이력 관리(State-History Management)가 필요하다. 카메라 자세 또는 라이다 위치추정 결과는 수십 또는 수백 밀리초 이전의 항공기 상태를 나타낼 수 있다. 이를 최신 상태에 직접 적용하면 잘못된 보정이 발생할 수 있다. 추정기는 이전 상태와 공분산을 유지하고 정확한 시점에서 지연 측정값을 적용한 후 버퍼링된 관성 데이터를 이용하여 현재 시점까지 상태를 다시 전파할 수 있다.

좌표계 일관성(Coordinate-Frame Consistency) 역시 매우 중요하다. 측정값은 센서 좌표계(Sensor Frame), 무인항공기 기체 좌표계(Body Frame), 지역 항법 좌표계(Local Navigation Frame), 지구 고정 좌표계(Earth-Fixed Frame) 또는 지도 좌표계(Map Frame)로 표현될 수 있다. 알려진 강체 변환(Rigid Transformation)과 항법 좌표계 정의를 일관되게 적용해야 한다. 좌표계 오류는 센서 바이어스와 유사하게 나타날 수 있으므로 암묵적인 규칙에 의존하지 않고 모든 융합 인터페이스에 명시적인 좌표계 식별자와 검증된 변환 관계를 포함해야 한다.

센서가 물리적으로 서로 다른 위치에서 측정값을 생성하는 경우 센서 레버암(Sensor Lever Arm)을 모델링해야 한다. 대형 화물 무인항공기의 서로 다른 위치에 장착된 위성항법 안테나, 카메라, 라이다, 레이더 및 관성측정장치는 회전 운동 중 서로 다른 순간 속도를 경험한다. 모든 측정값이 기체 중심에서 생성된 것으로 변환하면 특히 급격한 자세 변화가 발생하거나 센서 사이의 거리가 수 미터에 이르는 경우 체계적인 오차가 발생할 수 있다.

센서 융합은 모든 센서가 항상 활성화되어 있어야 한다는 것을 의미하지 않는다. EKF는 측정값의 가용성이 변화하더라도 유효한 상태와 현실적인 불확실성을 유지해야 한다. 정상 비행에서는 위성항법시스템이 장기적인 위치 보정을 주도할 수 있고, 구조물 주변에서는 시각 또는 라이다 위치추정의 중요성이 높아질 수 있으며, 착륙 과정에서는 레이더 또는 레이저 고도 측정값의 중요성이 증가할 수 있다. 예측 모델은 연속성을 제공하고 측정 소스는 유효성에 따라 융합 과정에 참여하거나 제외된다.

센서 고장(Sensor Failure)은 추정기를 즉시 붕괴시키는 것이 아니라 불확실성의 재분배(Uncertainty Redistribution)를 발생시켜야 한다. 위성항법시스템을 사용할 수 없으면 관성 전파가 연속성을 유지하면서 공분산이 증가할 수 있다. 어둠이나 눈부심으로 카메라의 신뢰성이 저하되면 카메라 갱신을 중단하고 라이다 또는 레이더 정보를 계속 사용할 수 있다. 이러한 점진적 성능 저하(Graceful Degradation)는 각 센서가 유효하지 않은 조건에서 측정값을 계속 생성하는 대신 명확한 상태 및 신뢰도 정보를 제공할 때 가능하다.

상관된 측정값(Correlated Measurement)은 서로 다른 두 관측값이 실제로 동일한 기반 정보를 사용하고 있을 수 있기 때문에 신중하게 처리해야 한다. 동일한 항법 해에서 파생된 두 추정값을 서로 독립적인 정보로 간주하여 융합하면 공분산이 비현실적으로 작아질 수 있다. 특히 중첩된 추정기(Nested Estimator), 시각-관성 시스템 또는 이미 외부에서 융합된 항법 장치의 출력을 결합할 때 아키텍처는 공유되는 정보 경로를 식별하고 동일한 정보를 중복 계산(Double Counting)하지 않아야 한다.

추정기 튜닝(Estimator Tuning)은 프로세스 불확실성과 측정 불확실성 사이의 균형을 결정한다. 프로세스 노이즈 파라미터가 지나치게 작으면 필터가 운동 모델을 과도하게 신뢰하게 되고, 지나치게 크면 상태 추정값에 많은 노이즈가 발생할 수 있다. 마찬가지로 비현실적인 측정 공분산은 특정 센서가 지나치게 지배적이 되거나 반대로 거의 무시되게 할 수 있다. 따라서 센서 특성 분석, 기록된 비행 데이터, 잔차 통계 및 대표적인 운용 조건을 기반으로 튜닝을 수행해야 한다.

융합 상태(Fused State)는 하나의 최적 추정값만 제공해서는 안 된다. 위치, 속도, 자세, 각운동, 추정된 바이어스, 타임스탬프, 좌표계, 공분산, 센서 기여 상태 및 무결성 지표(Integrity Indicator)를 제공하여 하위 시스템이 상태 정보와 신뢰도 정보를 모두 사용할 수 있도록 해야 한다. 비행 제어는 고주기의 자세와 속도를 요구할 수 있는 반면, 항법, 매핑, 장애물 회피 및 임무 관리는 명시적인 불확실성이 포함된 상대적으로 낮은 주기의 상태 정보를 사용할 수 있다.

실시간 구현(Real-Time Implementation)은 인지 처리 부하가 변화하더라도 결정론적인 상태 전파와 제한된 갱신 지연시간(Bounded Update Latency)을 유지해야 한다. 고주기 관성 처리는 계산량이 많은 카메라, 라이다 또는 레이더 알고리즘에 의해 차단되어서는 안 된다. 큐 관리(Queue Management), 타임스탬프 순서 관리, 제한된 버퍼(Bounded Buffer), 우선순위 기반 실행 및 독립적인 처리 영역을 이용하면 비행 제어 소프트웨어가 요구하는 연속적인 상태 출력을 중단하지 않으면서 지연된 인지 관측값을 융합할 수 있다.

초기화(Initialization)와 재설정 동작(Reset Behavior)은 EKF가 시작 시 모든 상태를 정확하게 알고 있다고 가정할 수 없으므로 명시적으로 설계해야 한다. 초기 위치와 속도는 위성항법시스템에서 얻을 수 있고, 중력은 롤(Roll)과 피치(Pitch)를 제한할 수 있으며, 방위는 기체 운동, 자기계 데이터, 다중 위성항법 안테나 또는 다른 기준을 필요로 할 수 있다. 공분산은 초기화 과정의 실제 불확실성을 반영하여 이후의 측정값이 부당하게 높은 초기 확신을 강제하지 않고 추정기를 점진적으로 수렴시킬 수 있도록 해야 한다.

검증(Validation)에서는 정상 운용뿐만 아니라 센서 고장, 데이터 누락, 시간 오차, 보정 오차, 지연된 측정값, 일관되지 않은 좌표계, 위성항법 중단, 시각 인지 성능 저하 및 급격하게 변화하는 비행 동역학을 시험해야 한다. 시뮬레이션, 기록 데이터 재생(Recorded-Data Replay), 소프트웨어 인 더 루프(Software-in-the-Loop, SIL), 하드웨어 인 더 루프(Hardware-in-the-Loop, HIL), 비행시험을 이용하여 이러한 조건에서 추정 오차, 공분산 일관성, 이노베이션 동작, 수렴, 드리프트, 복구 시간 및 계산 지연시간을 측정할 수 있다.

궁극적으로 다중 센서 확장 칼만 필터(Multi-Sensor EKF)는 무인항공기의 센싱 시스템과 자율 비행을 연결하는 상태 추정 핵심부(State-Estimation Core)를 구성한다. 그 목적은 단순히 여러 센서 출력을 평균하는 것이 아니라 운동 모델, 측정 물리, 시간, 보정, 불확실성 및 센서 상태를 결합하여 지속적으로 신뢰성이 평가되는 항공기 상태를 생성하는 것이다. 적절하게 설계된 융합 상태는 비행 제어, 매핑, 인지, 장애물 회피, 경로 실행, 착륙 및 안전 감시가 공유하는 공통 기준(Common Reference)을 제공한다.

##  

## 05.07. Terrain Following Perception and Altitude [w/Code]

![](images/image7.png){width="7.268055555555556in" height="7.268055555555556in"}

Terrain-following perception enables a cargo UAV to maintain a commanded clearance above changing ground surfaces while flying through three-dimensional environments. Unlike altitude control referenced only to mean sea level, terrain following requires continuous estimation of the local surface beneath and ahead of the aircraft. The perception system must therefore combine terrain-relative sensing, vehicle state, map information, and uncertainty into a reliable representation for guidance and control.

Altitude has several meanings within a UAV navigation system. GNSS provides a geographically referenced altitude, barometric sensors estimate pressure altitude, and radar or laser altimeters measure distance relative to the surface below the aircraft. These quantities are not interchangeable. A terrain-following system must maintain explicit reference definitions so the controller does not confuse absolute altitude with height above local terrain.

Radar altimeters are valuable because they directly measure terrain-relative distance and can operate under lighting conditions that challenge cameras. Their useful range, beam geometry, surface reflectivity, aircraft attitude, and terrain slope influence measurement quality. During banked or pitched flight, the shortest measured range may not correspond directly to vertical ground clearance, so sensor geometry and vehicle attitude must be considered before using the measurement for control.

Laser altimeters and downward-looking LiDAR provide high-resolution geometric measurements of the surface beneath the UAV. A scanning LiDAR can extend this capability by observing terrain over a broader region and estimating local elevation, slope, roughness, and protruding structures. These measurements are especially useful when the aircraft must distinguish a smooth ground surface from trees, buildings, rocks, or infrastructure that reduce the available clearance.

Forward-looking LiDAR or radar can provide terrain information before the aircraft reaches a particular location. This look-ahead capability is essential for high-speed cargo UAVs because a purely downward-looking sensor detects terrain changes only when the aircraft is already above them. By estimating upcoming terrain elevation, the autonomy system can generate vertical trajectory adjustments early enough to respect climb rate, descent rate, acceleration, energy, and passenger-independent cargo constraints.

Camera perception can complement active ranging by providing dense visual information about terrain boundaries and semantic content. Stereo cameras can estimate local depth, while monocular or learned models may provide relative terrain structure when properly validated. Visual perception can also distinguish vegetation, water, roads, buildings, and other surface categories, helping the autonomy system interpret whether measured geometry represents terrain, an obstacle, or an unsuitable emergency landing region.

Terrain maps provide another source of elevation information. A digital elevation model can supply expected ground height along the planned route and extend perception beyond onboard sensor range. However, map resolution, age, coordinate accuracy, vegetation representation, construction changes, and temporary structures can create discrepancies between stored terrain and current reality. Map information should therefore support onboard sensing rather than automatically override more recent validated observations.

A local terrain model can combine onboard measurements and prior map information into a common spatial representation. Grid-based elevation maps, voxel structures, point clouds, or surface models can represent the height and shape of nearby terrain. Each map element should retain confidence or uncertainty so the planning system can distinguish accurately observed surfaces from interpolated, outdated, or completely unknown regions.

Terrain segmentation separates the underlying surface from objects located above it. Trees, utility structures, buildings, cranes, vehicles, and other elevated features must not be incorrectly absorbed into a smooth ground model if they create collision hazards. Conversely, irregular terrain should not generate excessive false obstacles. Geometric slope, height discontinuity, neighborhood consistency, temporal observations, and semantic information can contribute to this classification.

The terrain-following reference is normally derived from desired clearance rather than from terrain elevation alone. If the estimated terrain height at a horizontal location is known, the guidance system can construct a reference altitude by adding a required safety margin. That margin may vary according to aircraft size, speed, navigation uncertainty, terrain uncertainty, weather, regulations, sensor performance, and available vertical maneuvering capability.

Look-ahead terrain processing must account for the UAV\'s predicted trajectory rather than examining only the surface directly in front of the sensor. Terrain elevation can be sampled along a future flight corridor whose width includes aircraft dimensions, navigation error, wind uncertainty, and maneuvering margins. The highest relevant terrain or obstacle within that corridor can then influence the commanded vertical profile before the aircraft reaches the hazardous region.

Vertical trajectory generation should respect aircraft dynamics. A sudden rise in terrain cannot simply produce an equally sudden altitude command because a heavy cargo UAV has finite climb performance, acceleration limits, propulsion reserve, and payload-dependent dynamics. Terrain perception should therefore provide sufficiently early and stable information so the planner can generate a smooth climb or route deviation rather than forcing the flight controller to react to impossible commands.

Terrain following during descent introduces additional constraints because excessive descent rate can rapidly consume ground clearance. The system should compare current terrain-relative altitude, vertical velocity, predicted terrain elevation, and stopping or flare capability. If future clearance falls below a validated threshold, the autonomy system may reduce descent, command a climb, modify the lateral route, or transition to a contingency behavior before minimum safe altitude is violated.

Sensor fusion improves altitude estimation because no single measurement source is reliable in every condition. GNSS provides long-term geographic reference, barometric altitude offers smooth relative changes, inertial sensing supports high-rate vertical motion estimation, and radar or laser measurements directly constrain terrain-relative height. An EKF or related estimator can combine these sources while accounting for different update rates, biases, latency, reference frames, and uncertainty.

Barometric altitude requires careful treatment because atmospheric pressure changes with weather and local conditions. Pressure-based altitude can drift relative to true geometric height even when the sensor itself operates correctly. It remains useful for smooth short-term altitude estimation, but terrain-following control should not assume that barometric altitude alone represents actual ground clearance. GNSS, terrain maps, and direct ranging provide complementary references for correcting or monitoring this drift.

Terrain-relative measurements can become unreliable over certain surfaces. Water, highly reflective or absorptive materials, steep slopes, vegetation, snow, or irregular geometry may reduce return quality or cause the sensor to observe a surface that does not correspond to the desired terrain reference. Measurement confidence should therefore incorporate return strength, geometric consistency, incidence angle, temporal stability, and agreement with complementary sensing sources.

Aircraft attitude influences downward sensor interpretation. During pitch and roll, the sensor beam may intersect the ground away from the point vertically below the UAV. A raw slant-range measurement therefore cannot always be treated as vertical altitude. Attitude compensation and known sensor mounting geometry are required to transform the measured range into a meaningful terrain-relative quantity, particularly during aggressive maneuvering or operation above sloped terrain.

Time synchronization is important because terrain measurements, attitude, position, and vertical velocity must describe compatible physical states. Applying a delayed range measurement to a newer aircraft pose can introduce errors in terrain height or clearance. Acquisition timestamps should be preserved, and delayed observations should be compensated or fused at the appropriate measurement epoch when processing latency becomes significant.

Terrain uncertainty must propagate into flight decisions. If the local surface is poorly observed, the system should not present an artificially precise clearance estimate. The planner can enlarge safety margins, reduce speed, increase altitude, or select a different route when terrain confidence decreases. This behavior converts perception uncertainty into operational conservatism rather than allowing unknown space to be interpreted automatically as safe space.

Fault detection should identify missing measurements, frozen altitude values, implausible range jumps, excessive noise, blocked sensors, timing faults, map disagreement, and inconsistent altitude sources. Comparing radar or laser height with GNSS altitude, barometric trends, inertial vertical motion, and mapped terrain can reveal failures that are difficult to detect from one sensor alone. Persistent disagreement should trigger isolation or reduced weighting of the suspected source.

Graceful degradation is essential when terrain-following capability becomes partially unavailable. If a direct ranging sensor fails, the UAV may temporarily rely on map-based terrain information and other altitude sources, but the permitted flight envelope should reflect the resulting uncertainty. Depending on mission and safety requirements, the system may increase clearance, reduce speed, leave terrain-following mode, climb to a predefined safe altitude, reroute, or initiate contingency procedures.

Real-time implementation requires bounded latency from terrain observation to guidance output. Point-cloud processing, surface fitting, map fusion, filtering, trajectory prediction, and clearance evaluation must execute quickly enough for the aircraft\'s speed and maneuverability. Local maps and bounded look-ahead regions can reduce computational demand while preserving the terrain information that directly affects the near-term flight trajectory.

Validation should include flat terrain, hills, steep slopes, ridges, valleys, vegetation, buildings, water, abrupt elevation changes, and sensor-degraded environments. Simulation and recorded data can test perception algorithms before hardware-in-the-loop and progressive flight trials. Evaluation should measure altitude error, terrain-model accuracy, minimum achieved clearance, look-ahead detection range, false terrain classification, processing latency, and recovery from sensor faults.

Terrain-following perception ultimately links environmental geometry with vertical flight guidance. Its objective is not simply to measure altitude but to understand how the terrain beneath and ahead of the UAV changes relative to the aircraft\'s future motion. By combining direct ranging, inertial state, GNSS, barometric altitude, visual perception, terrain maps, uncertainty, and fault monitoring, the system enables cargo UAVs to maintain safe and efficient clearance across complex terrain.

지형 추종 인지(Terrain-Following Perception)는 화물 무인항공기(Cargo UAV)가 3차원 환경을 비행하면서 변화하는 지표면 위에서 설정된 안전거리(Commanded Clearance)를 유지할 수 있도록 한다. 평균 해수면(Mean Sea Level)만을 기준으로 하는 고도 제어와 달리 지형 추종은 항공기 아래와 전방의 국부 지표면(Local Surface)을 지속적으로 추정해야 한다. 따라서 인지 시스템은 지형 상대 센싱(Terrain-Relative Sensing), 기체 상태, 지도 정보 및 불확실성을 결합하여 유도 및 제어에 사용할 수 있는 신뢰성 높은 표현을 생성해야 한다.

고도(Altitude)는 무인항공기 항법 시스템에서 여러 가지 의미를 가진다. 위성항법시스템(GNSS)은 지리적 기준의 고도를 제공하고, 기압 센서(Barometric Sensor)는 기압 고도(Pressure Altitude)를 추정하며, 레이더 또는 레이저 고도계(Radar or Laser Altimeter)는 항공기 아래 지표면까지의 거리를 측정한다. 이러한 값들은 서로 동일하게 사용할 수 없으므로 지형 추종 시스템은 절대 고도(Absolute Altitude)와 국부 지형 기준 높이(Height Above Local Terrain)가 혼동되지 않도록 기준 정의를 명확하게 유지해야 한다.

레이더 고도계(Radar Altimeter)는 지형 상대 거리를 직접 측정하고 카메라가 어려움을 겪는 조명 조건에서도 작동할 수 있기 때문에 유용하다. 유효 측정거리, 빔 형상(Beam Geometry), 지표면 반사 특성, 항공기 자세 및 지형 경사가 측정 품질에 영향을 준다. 기체가 뱅크(Bank)하거나 피치(Pitch)된 상태에서는 가장 짧게 측정된 거리가 수직 지상고(Vertical Ground Clearance)와 직접 일치하지 않을 수 있으므로 제어에 사용하기 전에 센서 기하와 기체 자세를 고려해야 한다.

레이저 고도계(Laser Altimeter)와 하향 라이다(Downward-Looking LiDAR)는 무인항공기 아래의 지표면에 대한 고해상도 기하학적 측정값을 제공한다. 스캐닝 라이다(Scanning LiDAR)는 더 넓은 영역에서 지형을 관측하여 국부 고도, 경사, 거칠기 및 돌출 구조물을 추정함으로써 이러한 기능을 확장할 수 있다. 이는 항공기가 평탄한 지표면과 나무, 건물, 암석 또는 기반 시설처럼 실제 비행 안전거리를 감소시키는 구조물을 구별해야 할 때 특히 유용하다.

전방 지향 라이다(Forward-Looking LiDAR) 또는 레이더(Radar)는 항공기가 특정 위치에 도달하기 전에 지형 정보를 제공할 수 있다. 이러한 전방 예측 기능(Look-Ahead Capability)은 순수한 하향 센서가 항공기가 이미 해당 지형 위에 도달한 이후에야 지형 변화를 감지하기 때문에 고속 화물 무인항공기에서 필수적이다. 자율 시스템은 전방 지형 고도를 추정하여 상승률, 하강률, 가속도, 에너지 및 화물 운송 조건을 만족하도록 충분히 이른 시점에 수직 궤적을 조정할 수 있다.

카메라 인지(Camera Perception)는 지형 경계와 의미론적 내용에 대한 밀집 시각 정보(Dense Visual Information)를 제공하여 능동형 거리 센싱을 보완할 수 있다. 스테레오 카메라(Stereo Camera)는 국부 깊이를 추정할 수 있으며, 단안 또는 학습 기반 모델(Monocular or Learned Model)은 적절히 검증된 경우 상대적인 지형 구조를 제공할 수 있다. 또한 시각 인지는 식생, 수면, 도로, 건물 및 기타 지표면 범주를 구별하여 측정된 기하 구조가 지형인지, 장애물인지 또는 부적합한 비상 착륙 구역인지를 자율 시스템이 판단하도록 지원한다.

지형 지도(Terrain Map)는 고도 정보를 제공하는 또 다른 정보원이다. 수치표고모델(Digital Elevation Model, DEM)은 계획된 경로를 따라 예상 지표면 높이를 제공하고 온보드 센서 범위를 넘어서는 영역까지 인지 범위를 확장할 수 있다. 그러나 지도 해상도, 제작 시점, 좌표 정확도, 식생 표현, 건설로 인한 변화 및 임시 구조물 때문에 저장된 지형과 현재 실제 환경 사이에 차이가 발생할 수 있다. 따라서 지도 정보는 최신의 검증된 온보드 관측값을 자동으로 대체하기보다 이를 지원하는 정보로 사용해야 한다.

지역 지형 모델(Local Terrain Model)은 온보드 측정값과 사전 지도 정보를 하나의 공통 공간 표현(Common Spatial Representation)으로 결합할 수 있다. 격자 기반 고도 지도(Grid-Based Elevation Map), 복셀 구조(Voxel Structure), 포인트 클라우드(Point Cloud) 또는 표면 모델(Surface Model)을 이용하여 주변 지형의 높이와 형상을 표현할 수 있다. 각각의 지도 요소에는 신뢰도 또는 불확실성을 유지하여 계획 시스템이 정확하게 관측된 지표면과 보간된 영역, 오래된 정보 또는 완전히 알려지지 않은 영역을 구분할 수 있도록 해야 한다.

지형 분할(Terrain Segmentation)은 기본 지표면과 그 위에 존재하는 객체를 구분한다. 나무, 전력 시설, 건물, 크레인, 차량 및 기타 돌출 구조물은 충돌 위험을 발생시키는 경우 매끄러운 지면 모델에 잘못 포함되어서는 안 된다. 반대로 불규칙한 지형이 과도한 오탐 장애물을 발생시켜서도 안 된다. 기하학적 경사, 높이 불연속성, 주변 영역과의 일관성, 시간에 따른 관측 및 의미론적 정보를 이러한 분류에 활용할 수 있다.

지형 추종 기준(Terrain-Following Reference)은 일반적으로 지형 고도 자체가 아니라 요구되는 안전거리(Desired Clearance)를 기준으로 생성된다. 특정 수평 위치에서 추정된 지형 높이를 알고 있다면 유도 시스템은 여기에 필요한 안전 여유(Safety Margin)를 추가하여 기준 고도를 생성할 수 있다. 이 안전 여유는 항공기 크기, 속도, 항법 불확실성, 지형 불확실성, 기상, 규정, 센서 성능 및 사용 가능한 수직 기동 능력에 따라 달라질 수 있다.

전방 지형 처리(Look-Ahead Terrain Processing)는 단순히 센서 바로 앞의 지표면만 검사하는 것이 아니라 무인항공기의 예측 궤적(Predicted Trajectory)을 고려해야 한다. 항공기 크기, 항법 오차, 바람 불확실성 및 기동 여유를 포함하는 미래 비행 회랑(Future Flight Corridor)을 따라 지형 고도를 샘플링할 수 있다. 해당 회랑 내부에서 가장 높은 관련 지형이나 장애물을 이용하여 항공기가 위험 지역에 도달하기 전에 수직 비행 프로파일을 조정할 수 있다.

수직 궤적 생성(Vertical Trajectory Generation)은 항공기 동역학을 고려해야 한다. 지형이 갑자기 높아진다고 해서 동일하게 급격한 고도 명령을 생성할 수는 없다. 무거운 화물 무인항공기는 제한된 상승 성능, 가속도 한계, 추진 여유(Propulsion Reserve) 및 탑재 화물에 따른 동역학적 제약을 갖기 때문이다. 따라서 지형 인지는 계획기가 비현실적인 명령에 비행 제어기가 반응하도록 하기보다 충분히 조기에 안정적인 정보를 제공하여 부드러운 상승 또는 경로 변경을 생성하도록 해야 한다.

하강 중 지형 추종은 과도한 하강률이 지상 안전거리를 빠르게 감소시킬 수 있으므로 추가적인 제약조건을 가진다. 시스템은 현재 지형 상대 고도, 수직 속도, 예측 지형 고도 및 정지 또는 플레어 능력(Flare Capability)을 함께 비교해야 한다. 미래 안전거리가 검증된 임계값 이하로 감소할 것으로 예상되면 최소 안전고도가 침해되기 전에 자율 시스템이 하강률을 줄이거나 상승을 명령하고, 수평 경로를 변경하거나 비상 동작(Contingency Behavior)으로 전환할 수 있다.

단일 측정원이 모든 조건에서 신뢰할 수 있는 것은 아니므로 센서 융합(Sensor Fusion)은 고도 추정 성능을 향상시킨다. 위성항법시스템은 장기적인 지리적 기준을 제공하고, 기압 고도는 부드러운 상대 변화를 제공하며, 관성 센싱은 고주기 수직 운동 추정을 지원하고, 레이더 또는 레이저 측정은 지형 상대 높이를 직접 제한한다. 확장 칼만 필터(Extended Kalman Filter, EKF) 또는 관련 추정기를 이용하여 서로 다른 갱신 주기, 바이어스, 지연시간, 기준 좌표계 및 불확실성을 고려하면서 이러한 정보원을 결합할 수 있다.

기압 고도(Barometric Altitude)는 대기압이 기상과 지역 조건에 따라 변화하기 때문에 신중하게 처리해야 한다. 압력 기반 고도는 센서 자체가 정상적으로 작동하더라도 실제 기하학적 높이(True Geometric Height)에 대해 드리프트할 수 있다. 기압 고도는 부드러운 단기 고도 추정에는 유용하지만 지형 추종 제어에서는 기압 고도만으로 실제 지상 안전거리를 나타낸다고 가정해서는 안 된다. 위성항법, 지형 지도 및 직접 거리 측정이 이러한 드리프트를 보정하거나 감시하기 위한 상호보완적 기준을 제공한다.

지형 상대 측정(Terrain-Relative Measurement)은 특정 지표면에서 신뢰성이 저하될 수 있다. 수면, 반사율이 지나치게 높거나 낮은 재질, 급경사, 식생, 눈 또는 불규칙한 기하 구조는 반사 신호의 품질을 저하시키거나 센서가 원하는 지형 기준과 다른 표면을 관측하도록 만들 수 있다. 따라서 측정 신뢰도는 반사 신호 강도, 기하학적 일관성, 입사각(Incidence Angle), 시간적 안정성 및 상호보완 센서와의 일치도를 고려해야 한다.

항공기 자세(Attitude)는 하향 센서 측정값의 해석에 영향을 준다. 피치와 롤(Roll)이 발생하면 센서 빔이 무인항공기의 수직 아래 지점이 아닌 다른 위치의 지면과 교차할 수 있다. 따라서 원시 경사거리 측정값(Raw Slant-Range Measurement)을 항상 수직 고도로 간주할 수는 없다. 특히 급격한 기동이나 경사진 지형 위를 비행하는 경우 측정된 거리를 의미 있는 지형 상대 값으로 변환하기 위해 자세 보상(Attitude Compensation)과 알려진 센서 장착 기하가 필요하다.

지형 측정값, 자세, 위치 및 수직 속도는 서로 일치하는 물리적 시점을 나타내야 하므로 시간 동기화(Time Synchronization)가 중요하다. 지연된 거리 측정값을 더 최신의 항공기 자세에 적용하면 지형 높이나 안전거리 추정에 오류가 발생할 수 있다. 따라서 획득 타임스탬프(Acquisition Timestamp)를 보존해야 하며 처리 지연이 상당한 경우 지연된 관측값을 보상하거나 적절한 측정 시점에서 융합해야 한다.

지형 불확실성(Terrain Uncertainty)은 비행 의사결정에 반영되어야 한다. 국부 지표면이 충분히 관측되지 않은 경우 시스템은 인위적으로 정밀한 안전거리 추정값을 제공해서는 안 된다. 지형 정보의 신뢰도가 감소하면 계획기는 안전 여유를 확대하고, 속도를 줄이며, 고도를 높이거나 다른 경로를 선택할 수 있다. 이러한 동작은 미확인 공간(Unknown Space)을 자동으로 안전한 공간으로 해석하지 않고 인지 불확실성을 보수적인 운용 판단으로 변환한다.

고장 탐지(Fault Detection)는 측정값 누락, 고정된 고도값, 비현실적인 거리 급변, 과도한 노이즈, 센서 차단, 시간 오류, 지도 정보와의 불일치 및 서로 다른 고도 정보원 사이의 불일치를 식별해야 한다. 레이더 또는 레이저 높이를 위성항법 고도, 기압 변화 추세, 관성 기반 수직 운동 및 지도 지형과 비교하면 하나의 센서만으로 탐지하기 어려운 고장을 발견할 수 있다. 지속적인 불일치가 발생하면 의심되는 센서를 격리하거나 융합 가중치를 낮춰야 한다.

지형 추종 기능을 부분적으로 사용할 수 없게 되는 경우 점진적 성능 저하(Graceful Degradation)가 필수적이다. 직접 거리 측정 센서가 고장 나면 무인항공기는 일시적으로 지도 기반 지형 정보와 다른 고도 정보원에 의존할 수 있지만 허용되는 비행 영역은 증가한 불확실성을 반영해야 한다. 임무와 안전 요구조건에 따라 시스템은 안전거리를 확대하거나 속도를 줄이고, 지형 추종 모드를 종료하거나 사전에 정의된 안전고도로 상승하며, 경로를 변경하거나 비상 절차를 시작할 수 있다.

실시간 구현(Real-Time Implementation)은 지형 관측에서 유도 명령 출력까지 제한된 지연시간(Bounded Latency)을 유지해야 한다. 포인트 클라우드 처리, 표면 피팅(Surface Fitting), 지도 융합, 필터링, 궤적 예측 및 안전거리 평가는 항공기의 속도와 기동 성능에 충분히 대응할 수 있도록 신속하게 실행되어야 한다. 지역 지도(Local Map)와 제한된 전방 예측 영역(Bounded Look-Ahead Region)을 사용하면 가까운 미래의 비행 궤적에 직접적인 영향을 주는 지형 정보를 유지하면서 계산 부하를 줄일 수 있다.

검증(Validation)에는 평탄한 지형, 언덕, 급경사, 능선, 계곡, 식생, 건물, 수면, 급격한 고도 변화 및 센서 성능이 저하된 환경이 포함되어야 한다. 시뮬레이션과 기록 데이터를 이용하여 하드웨어 인 더 루프(Hardware-in-the-Loop, HIL) 및 단계적 비행시험 전에 인지 알고리즘을 시험할 수 있다. 평가에서는 고도 오차, 지형 모델 정확도, 실제 최소 안전거리, 전방 탐지 거리, 지형 오분류(False Terrain Classification), 처리 지연시간 및 센서 고장 이후의 복구 성능을 측정해야 한다.

궁극적으로 지형 추종 인지(Terrain-Following Perception)는 환경의 기하학적 구조와 수직 비행 유도(Vertical Flight Guidance)를 연결한다. 그 목적은 단순히 고도를 측정하는 것이 아니라 무인항공기의 미래 운동에 상대적으로 항공기 아래와 전방의 지형이 어떻게 변화하는지를 이해하는 것이다. 직접 거리 측정, 관성 상태, 위성항법시스템, 기압 고도, 시각 인지, 지형 지도, 불확실성 및 고장 감시를 결합함으로써 화물 무인항공기가 복잡한 지형에서도 안전하고 효율적인 지상 안전거리를 유지할 수 있도록 한다.

##  

## 05.08. Bird and Drone Detection Mid Air Collision [w/Code]

![](images/image8.png){width="7.268055555555556in" height="7.268055555555556in"}

Bird and drone detection is a critical perception function for cargo UAVs operating in shared or uncontrolled airspace. Birds and small unmanned aircraft can appear suddenly, move unpredictably, and present limited visual or radar signatures at long range. The perception system must therefore detect, classify, track, and evaluate airborne objects early enough for the autonomy system to determine whether a mid-air collision risk exists and initiate an appropriate avoidance response.

Detecting small airborne targets is fundamentally difficult because their apparent size decreases rapidly with distance. A bird or compact drone may occupy only a few image pixels during initial detection, while its LiDAR return can be sparse and its radar cross section relatively small. Reliable detection therefore benefits from combining complementary sensors rather than assuming that one sensing modality can provide sufficient performance across every distance, background, weather, and lighting condition.

Cameras provide high-resolution appearance information that can support classification of birds, multirotor drones, fixed-wing UAVs, and other aircraft. Neural-network detectors can search wide-field imagery for candidate airborne objects, while higher-resolution regions can support subsequent classification. Detection performance depends on optics, image resolution, frame rate, exposure, stabilization, background contrast, and the availability of representative training data covering realistic target appearances and viewing angles.

Visual detection becomes particularly challenging when a target appears against clouds, terrain, buildings, vegetation, or direct sunlight. Small objects may temporarily disappear because of low contrast, motion blur, glare, or occlusion. Temporal processing can improve robustness by accumulating evidence across successive frames rather than relying on a single image. Object tracking can also maintain a predicted target state during short periods when the visual detector fails to produce a confident observation.

Radar provides an important complementary sensing channel because it directly measures range and radial velocity and does not depend on visible illumination. Doppler information can help distinguish moving airborne objects from parts of the static environment. Radar performance nevertheless depends on target radar cross section, range, aspect angle, antenna resolution, clutter, and operating frequency, so small birds and drones may still produce intermittent or ambiguous measurements.

LiDAR can provide accurate three-dimensional range measurements when sufficient returns are obtained from an airborne object. Its geometric information is useful for confirming target position and separating an object from a complex visual background. However, narrow structures, small drones, birds, long distances, rapid relative motion, and limited scan density can reduce detection probability. LiDAR is therefore most effective as one component of a multi-sensor airborne-target detection architecture.

Sensor fusion combines the strengths of camera, radar, and LiDAR observations. Radar may provide early range and velocity evidence, a camera may contribute semantic classification, and LiDAR may refine three-dimensional geometry when the target enters an effective sensing range. Fusion algorithms must account for different fields of view, update rates, coordinate frames, measurement uncertainties, timestamps, and detection characteristics before observations from separate sensors are associated with the same physical target.

Accurate time synchronization is especially important because airborne targets and the cargo UAV can both move rapidly. Measurements collected even a fraction of a second apart may correspond to significantly different relative positions. Sensor acquisition timestamps should therefore be preserved, and target observations should be transformed using the UAV state associated with the correct measurement time before multi-sensor association, tracking, or collision prediction is performed.

Target tracking converts intermittent detections into a continuous estimate of relative motion. A tracking filter can estimate three-dimensional position, velocity, acceleration, uncertainty, and track confidence while predicting the target state between observations. Track management must determine when to initialize a new target, associate later detections, tolerate temporary missed observations, separate crossing targets, and terminate tracks that no longer have sufficient supporting evidence.

Bird motion can differ substantially from drone motion. Birds may change direction rapidly, alter speed, flap, glide, climb, descend, or move as groups, while drones may hover, follow structured trajectories, or perform abrupt commanded maneuvers. Classification can therefore use not only appearance but also temporal motion characteristics. Nevertheless, collision avoidance should not depend entirely on correct semantic classification when an unidentified airborne object already presents a credible physical conflict.

False detections must be controlled because clouds, insects, distant ground objects, moving vegetation, reflections, sensor artifacts, and radar clutter can resemble small airborne targets. Excessive false alarms can produce unnecessary avoidance maneuvers and reduce mission efficiency. Conversely, overly aggressive filtering can suppress genuine hazards. Detection thresholds should therefore be linked to uncertainty, persistence, multi-sensor agreement, target motion, and proximity to the predicted flight corridor.

Mid-air collision assessment begins after a target track has sufficient spatial and velocity information. Relative position and relative velocity can be propagated forward to estimate whether the UAV and target trajectories approach dangerously close. Common quantities include time to closest point of approach(Time to CPA), predicted separation at closest approach, closure rate, and uncertainty around the predicted encounter geometry.

Collision prediction must account for uncertainty rather than treating estimated trajectories as exact lines. Target maneuverability, measurement noise, UAV state uncertainty, wind, tracking errors, and processing latency create a region of possible future positions. The protected volume around the cargo UAV can therefore be expanded according to uncertainty and vehicle dimensions so the system begins avoidance before the predicted trajectories reach an unacceptable collision probability.

Large cargo UAVs require particular attention because their physical dimensions, mass, momentum, and maneuvering limits may be substantially greater than those of small drones. A collision-avoidance decision must consider achievable turn rate, climb and descent performance, acceleration limits, propulsion reserve, payload condition, and response delay. Detection range must therefore be derived from the time and distance required to perform a validated avoidance maneuver rather than from sensor capability alone.

Threat prioritization becomes necessary when multiple airborne objects are present. The nearest target is not always the most dangerous because another object may have a much higher closure rate or a trajectory that intersects the UAV\'s protected volume. A threat evaluator can combine predicted separation, time to conflict, track confidence, maneuver uncertainty, target behavior, and available escape directions to determine which encounter requires immediate action.

Avoidance planning should generate a maneuver that increases separation while remaining compatible with aircraft dynamics, terrain, obstacles, geofences, weather, and mission constraints. Depending on encounter geometry, the system may command a lateral deviation, climb, descent, speed adjustment, hover, or coordinated combination. The selected maneuver should also be checked against other tracked objects so avoiding one airborne hazard does not create a new conflict.

Perception and avoidance should operate as a closed feedback process rather than a single detection followed by an open-loop maneuver. During avoidance, sensors continue observing the target and the tracker updates its estimated trajectory. The collision assessor can then determine whether separation is improving as expected. If the target maneuvers unexpectedly, the avoidance trajectory can be revised while preserving aircraft stability and validated safety limits.

Uncooperative drones create additional challenges because they may not transmit identification, position, or intent information. Cooperative traffic information can improve situational awareness when available, but onboard perception remains necessary for objects that are silent, incorrectly reporting, or completely unknown. The collision-avoidance architecture should therefore treat communication-based traffic data as complementary evidence rather than as a replacement for independent physical sensing.

Bird encounters require additional conservatism because individual birds or flocks may react to the UAV itself. A predicted trajectory based on constant velocity may become invalid when birds turn or scatter. Tracking uncertainty should increase when motion becomes highly variable, and avoidance logic should preserve sufficient separation from groups rather than attempting to navigate precisely through gaps that may disappear as flock geometry changes.

Sensor-health monitoring is essential because degradation can directly reduce airborne-target detection range. Blocked camera lenses, image blur, radar interference, LiDAR contamination, missing frames, synchronization faults, calibration errors, or excessive processing latency should be detected and communicated to the autonomy system. Reduced sensing capability may require lower flight speed, greater separation margins, route modification, or transition to a more conservative operating mode.

Real-time implementation must guarantee bounded latency across detection, fusion, tracking, threat assessment, and avoidance planning. A long-range detection provides little safety benefit if processing delays consume the available reaction time. Computational resources should prioritize safety-critical airborne targets, while optimized inference, hardware acceleration, asynchronous processing, and bounded queues can prevent less critical perception workloads from delaying collision-related outputs.

Validation should reproduce representative bird and drone encounters across different ranges, relative speeds, approach angles, target sizes, backgrounds, lighting, weather, and sensor-degradation conditions. Simulation can generate large numbers of controlled collision geometries, while recorded data, hardware-in-the-loop testing, and progressive flight trials can evaluate actual sensing and timing behavior. Detection range, tracking continuity, false alarms, collision prediction accuracy, avoidance success, and minimum separation should be measured.

Bird and drone detection ultimately forms part of a broader sense-and-avoid capability rather than an isolated object-recognition function. The system must transform weak and intermittent observations into reliable tracks, predict future conflicts under uncertainty, and provide enough time for a dynamically feasible response. By integrating cameras, radar, LiDAR, vehicle-state estimation, tracking, threat assessment, and closed-loop avoidance, cargo UAVs can reduce mid-air collision risk in complex operating environments.

조류 및 드론 탐지(Bird and Drone Detection)는 공유 공역 또는 비통제 공역에서 운용되는 화물 무인항공기(Cargo UAV)의 핵심 인지 기능이다. 조류와 소형 무인항공기는 갑작스럽게 출현하고 예측하기 어렵게 움직일 수 있으며, 장거리에서는 시각 또는 레이더 신호가 제한적일 수 있다. 따라서 인지 시스템은 자율 시스템이 공중 충돌(Mid-Air Collision) 위험의 존재 여부를 판단하고 적절한 회피 대응을 시작할 수 있도록 충분히 이른 시점에 공중 객체를 탐지, 분류, 추적 및 평가해야 한다.

소형 공중 표적(Small Airborne Target)의 탐지는 거리가 증가할수록 겉보기 크기가 급격히 감소하기 때문에 본질적으로 어렵다. 조류 또는 소형 드론은 초기 탐지 시 영상에서 불과 몇 개의 픽셀만 차지할 수 있으며, 라이다(LiDAR) 반사 신호는 희소하고 레이더 단면적(Radar Cross Section)은 상대적으로 작을 수 있다. 따라서 모든 거리, 배경, 기상 및 조명 조건에서 하나의 센싱 방식만으로 충분한 성능을 제공한다고 가정하기보다 상호보완적인 센서를 결합하는 것이 신뢰성 높은 탐지에 유리하다.

카메라(Camera)는 조류, 멀티로터 드론(Multirotor Drone), 고정익 무인항공기(Fixed-Wing UAV) 및 기타 항공기의 분류를 지원할 수 있는 고해상도 외형 정보를 제공한다. 신경망 기반 탐지기(Neural-Network Detector)는 광시야 영상에서 후보 공중 객체를 검색하고, 고해상도 관심영역을 이용하여 후속 분류를 수행할 수 있다. 탐지 성능은 광학계, 영상 해상도, 프레임률, 노출, 안정화, 배경 대비 및 실제 표적의 다양한 외형과 관측 각도를 포함하는 대표적인 학습 데이터의 가용성에 영향을 받는다.

표적이 구름, 지형, 건물, 식생 또는 직사광선을 배경으로 나타나는 경우 시각 탐지는 특히 어려워진다. 작은 객체는 낮은 대비, 모션 블러(Motion Blur), 눈부심 또는 가림(Occlusion)으로 인해 일시적으로 사라질 수 있다. 시간적 처리(Temporal Processing)는 단일 영상에만 의존하지 않고 연속 프레임에 걸쳐 증거를 누적함으로써 강건성을 향상시킬 수 있다. 또한 객체 추적(Object Tracking)은 시각 탐지기가 짧은 시간 동안 신뢰할 수 있는 관측값을 생성하지 못하더라도 예측된 표적 상태를 유지할 수 있다.

레이더(Radar)는 거리를 직접 측정하고 방사 속도(Radial Velocity)를 제공하며 가시광 조명에 의존하지 않기 때문에 중요한 상호보완 센싱 채널을 제공한다. 도플러 정보(Doppler Information)는 움직이는 공중 객체를 정적인 주변 환경으로부터 구별하는 데 도움을 줄 수 있다. 그러나 레이더 성능 역시 표적의 레이더 단면적, 거리, 관측 각도, 안테나 해상도, 클러터(Clutter) 및 동작 주파수에 영향을 받으므로 소형 조류와 드론은 간헐적이거나 모호한 측정값을 생성할 수 있다.

라이다(LiDAR)는 공중 객체에서 충분한 반사 신호를 확보할 수 있는 경우 정확한 3차원 거리 측정값을 제공할 수 있다. 라이다의 기하학적 정보는 표적 위치를 확인하고 복잡한 시각적 배경으로부터 객체를 분리하는 데 유용하다. 그러나 가느다란 구조, 소형 드론, 조류, 장거리, 빠른 상대 운동 및 제한된 스캔 밀도는 탐지 확률을 낮출 수 있다. 따라서 라이다는 다중 센서 공중 표적 탐지 아키텍처(Multi-Sensor Airborne-Target Detection Architecture)의 한 구성요소로 활용할 때 가장 효과적이다.

센서 융합(Sensor Fusion)은 카메라, 레이더 및 라이다 관측의 장점을 결합한다. 레이더는 초기 거리와 속도 정보를 제공하고, 카메라는 의미론적 분류(Semantic Classification)에 기여하며, 라이다는 표적이 유효 센싱 범위에 진입했을 때 3차원 기하 정보를 정밀화할 수 있다. 서로 다른 센서의 관측값을 동일한 물리적 표적으로 연계하기 전에 융합 알고리즘은 서로 다른 시야각, 갱신 주기, 좌표계, 측정 불확실성, 타임스탬프 및 탐지 특성을 고려해야 한다.

공중 표적과 화물 무인항공기가 모두 빠르게 이동할 수 있으므로 정확한 시간 동기화(Time Synchronization)가 특히 중요하다. 불과 몇 분의 1초 간격으로 획득된 측정값도 상당히 다른 상대 위치를 나타낼 수 있다. 따라서 센서 획득 타임스탬프(Sensor Acquisition Timestamp)를 보존하고, 다중 센서 연계, 추적 또는 충돌 예측을 수행하기 전에 정확한 측정 시점에 해당하는 무인항공기 상태를 이용하여 표적 관측값을 변환해야 한다.

표적 추적(Target Tracking)은 간헐적인 탐지 결과를 연속적인 상대 운동 추정값으로 변환한다. 추적 필터(Tracking Filter)는 관측 사이에서 표적 상태를 예측하면서 3차원 위치, 속도, 가속도, 불확실성 및 추적 신뢰도(Track Confidence)를 추정할 수 있다. 추적 관리(Track Management)는 새로운 표적을 언제 초기화할지, 후속 탐지 결과를 어떻게 연계할지, 일시적인 탐지 누락을 어떻게 허용할지, 교차하는 표적을 어떻게 분리할지, 충분한 근거가 사라진 추적을 언제 종료할지를 결정해야 한다.

조류의 운동은 드론의 운동과 상당히 다를 수 있다. 조류는 빠르게 방향을 변경하고 속도를 바꾸며 날갯짓, 활공, 상승, 하강 또는 군집 이동을 수행할 수 있는 반면, 드론은 호버링(Hovering), 구조화된 궤적 추종 또는 급격한 명령 기동을 수행할 수 있다. 따라서 분류에는 외형뿐만 아니라 시간에 따른 운동 특성도 활용할 수 있다. 그러나 식별되지 않은 공중 객체가 이미 신뢰할 수 있는 물리적 충돌 위험을 나타내는 경우 충돌 회피가 정확한 의미론적 분류에 전적으로 의존해서는 안 된다.

구름, 곤충, 원거리 지상 객체, 움직이는 식생, 반사 신호, 센서 아티팩트(Sensor Artifact) 및 레이더 클러터가 소형 공중 표적과 유사하게 나타날 수 있으므로 오탐지(False Detection)를 제어해야 한다. 과도한 오경보는 불필요한 회피 기동을 발생시키고 임무 효율을 저하시킬 수 있다. 반대로 지나치게 공격적인 필터링은 실제 위험 요소를 제거할 수 있다. 따라서 탐지 임계값은 불확실성, 지속성, 다중 센서 일치도, 표적 운동 및 예측 비행 회랑과의 근접성을 고려하여 결정해야 한다.

공중 충돌 평가(Mid-Air Collision Assessment)는 표적 추적 정보가 충분한 공간 및 속도 정보를 확보한 이후 시작된다. 상대 위치와 상대 속도를 미래로 전파하여 무인항공기와 표적의 궤적이 위험할 정도로 가까워지는지를 추정할 수 있다. 주요 평가 요소에는 최근접점까지의 시간(Time to Closest Point of Approach, Time to CPA), 최근접점에서의 예측 분리거리, 접근 속도(Closure Rate) 및 예측 조우 기하에 대한 불확실성이 포함된다.

충돌 예측(Collision Prediction)은 추정된 궤적을 정확한 하나의 선으로 간주하지 않고 불확실성을 고려해야 한다. 표적의 기동성, 측정 노이즈, 무인항공기 상태 불확실성, 바람, 추적 오차 및 처리 지연시간은 미래 위치의 가능 영역을 형성한다. 따라서 화물 무인항공기 주변의 보호 공간(Protected Volume)을 불확실성과 기체 크기에 따라 확장하여 예측 궤적이 허용할 수 없는 충돌 확률에 도달하기 전에 시스템이 회피를 시작하도록 할 수 있다.

대형 화물 무인항공기(Large Cargo UAV)는 물리적 크기, 질량, 운동량 및 기동 한계가 소형 드론보다 상당히 클 수 있으므로 특별한 고려가 필요하다. 충돌 회피 결정에서는 실현 가능한 선회율, 상승 및 하강 성능, 가속도 한계, 추진 여유(Propulsion Reserve), 탑재 화물 상태 및 응답 지연을 고려해야 한다. 따라서 요구 탐지 거리는 센서의 최대 성능만으로 결정하는 것이 아니라 검증된 회피 기동을 수행하는 데 필요한 시간과 거리를 기준으로 산정해야 한다.

여러 공중 객체가 존재하는 경우 위협 우선순위 결정(Threat Prioritization)이 필요하다. 가장 가까운 표적이 항상 가장 위험한 것은 아니며, 더 멀리 있는 객체가 훨씬 높은 접근 속도를 가지거나 무인항공기의 보호 공간과 교차하는 궤적을 가질 수 있다. 위협 평가기(Threat Evaluator)는 예측 분리거리, 충돌까지의 시간, 추적 신뢰도, 기동 불확실성, 표적 행동 및 사용 가능한 회피 방향을 결합하여 어떤 조우 상황에 즉각적인 대응이 필요한지를 결정할 수 있다.

회피 계획(Avoidance Planning)은 항공기 동역학, 지형, 장애물, 지오펜스(Geofence), 기상 및 임무 제약조건을 만족하면서 분리거리를 증가시키는 기동을 생성해야 한다. 조우 기하에 따라 시스템은 수평 방향 변경, 상승, 하강, 속도 조정, 호버링 또는 이러한 동작의 조합을 명령할 수 있다. 또한 하나의 공중 위험 요소를 회피하는 과정에서 다른 추적 객체와 새로운 충돌 위험이 발생하지 않도록 선택된 기동을 다른 표적 정보와 함께 검사해야 한다.

인지와 회피는 한 번의 탐지 이후 개방 루프(Open-Loop) 방식으로 기동하는 것이 아니라 폐루프 피드백 과정(Closed-Loop Feedback Process)으로 동작해야 한다. 회피 중에도 센서는 지속적으로 표적을 관측하고 추적기는 추정 궤적을 갱신한다. 충돌 평가기는 분리거리가 예상대로 증가하고 있는지를 판단할 수 있으며, 표적이 예상하지 못한 기동을 수행하면 항공기의 안정성과 검증된 안전 한계를 유지하면서 회피 궤적을 수정할 수 있다.

비협조적 드론(Uncooperative Drone)은 식별 정보, 위치 또는 비행 의도를 송신하지 않을 수 있기 때문에 추가적인 문제를 발생시킨다. 협조적 교통 정보(Cooperative Traffic Information)를 사용할 수 있다면 상황 인식을 향상시킬 수 있지만, 통신하지 않거나 잘못된 정보를 송신하거나 완전히 알려지지 않은 객체에 대응하기 위해서는 온보드 인지(Onboard Perception)가 여전히 필요하다. 따라서 충돌 회피 아키텍처는 통신 기반 교통 데이터를 독립적인 물리적 센싱을 대체하는 수단이 아니라 상호보완적인 정보로 취급해야 한다.

조류와의 조우(Bird Encounter)는 개별 조류 또는 무리가 무인항공기 자체에 반응할 수 있으므로 추가적인 보수성이 필요하다. 일정 속도를 가정한 예측 궤적은 조류가 갑자기 방향을 바꾸거나 흩어지는 경우 더 이상 유효하지 않을 수 있다. 운동 변화가 커지면 추적 불확실성을 증가시켜야 하며, 회피 로직은 조류 무리의 형태가 변화하면서 사라질 수 있는 좁은 간격을 정밀하게 통과하려 하기보다 충분한 분리거리를 유지해야 한다.

센서 상태 감시(Sensor-Health Monitoring)는 성능 저하가 공중 표적 탐지 거리를 직접 감소시킬 수 있기 때문에 필수적이다. 카메라 렌즈 차단, 영상 흐림, 레이더 간섭, 라이다 오염, 프레임 누락, 동기화 오류, 보정 오차 또는 과도한 처리 지연시간을 탐지하고 자율 시스템에 전달해야 한다. 센싱 능력이 저하되면 비행 속도 감소, 분리거리 확대, 경로 변경 또는 더욱 보수적인 운용 모드로의 전환이 필요할 수 있다.

실시간 구현(Real-Time Implementation)은 탐지, 융합, 추적, 위협 평가 및 회피 계획 전체 과정에서 제한된 지연시간(Bounded Latency)을 보장해야 한다. 장거리에서 표적을 탐지하더라도 처리 지연으로 사용 가능한 대응 시간이 소모된다면 안전상의 이점은 크게 감소한다. 계산 자원은 안전에 중요한 공중 표적을 우선적으로 처리해야 하며, 최적화된 추론, 하드웨어 가속, 비동기 처리 및 제한된 큐(Bounded Queue)를 이용하여 중요도가 낮은 인지 작업이 충돌 관련 정보 출력을 지연시키지 않도록 해야 한다.

검증(Validation)은 다양한 거리, 상대 속도, 접근 각도, 표적 크기, 배경, 조명, 기상 및 센서 성능 저하 조건에서 대표적인 조류 및 드론 조우 상황을 재현해야 한다. 시뮬레이션을 통해 다양한 충돌 기하를 대규모로 생성할 수 있으며, 기록 데이터, 하드웨어 인 더 루프(Hardware-in-the-Loop, HIL) 시험 및 단계적 비행시험을 통해 실제 센싱과 시간 특성을 평가할 수 있다. 탐지 거리, 추적 연속성, 오경보, 충돌 예측 정확도, 회피 성공률 및 최소 분리거리를 주요 성능 지표로 측정해야 한다.

궁극적으로 조류 및 드론 탐지(Bird and Drone Detection)는 독립적인 객체 인식 기능이 아니라 보다 광범위한 감지 및 회피(Sense-and-Avoid) 능력의 일부를 구성한다. 시스템은 약하고 간헐적인 관측값을 신뢰할 수 있는 추적 정보로 변환하고, 불확실성을 고려하여 미래의 충돌 가능성을 예측하며, 동역학적으로 실행 가능한 대응을 수행할 충분한 시간을 확보해야 한다. 카메라, 레이더, 라이다, 기체 상태 추정, 추적, 위협 평가 및 폐루프 회피를 통합함으로써 화물 무인항공기는 복잡한 운용 환경에서 공중 충돌 위험을 감소시킬 수 있다.

##  

## 05.09. Landing Zone Assessment Vision Based [w/Code]

![](images/image9.png){width="7.268055555555556in" height="7.268055555555556in"}

Vision-based landing-zone assessment enables a cargo UAV to determine whether a candidate surface provides sufficient geometric, environmental, and operational safety for autonomous landing. Unlike navigation to a predefined coordinate, safe landing requires understanding the local scene immediately before touchdown. The perception system must evaluate surface boundaries, slope, roughness, obstacles, people, vehicles, vegetation, and available clearance while accounting for uncertainty and changing conditions.

The assessment process normally begins with detection of candidate landing regions within downward-looking or oblique camera imagery. A predefined landing pad may be recognized through visual markers, known geometry, or learned features, while an unprepared landing area requires broader scene interpretation. The system should identify contiguous regions large enough to accommodate the aircraft footprint, rotor clearance, positioning error, and additional safety margins required for cargo operations.

Camera configuration strongly influences landing-zone perception. Downward-facing cameras provide direct observations of the touchdown area, while forward or oblique cameras can inspect the approach region before the aircraft is directly overhead. Wide-angle optics increase coverage, whereas higher-resolution cameras improve detection of small hazards. Multi-camera arrangements can combine broad situational awareness with detailed inspection while reducing blind regions caused by the airframe or payload.

Geometric calibration is required to transform image observations into measurements relevant to landing. Camera intrinsic parameters define the relationship between pixels and viewing rays, while extrinsic calibration establishes camera position and orientation relative to the UAV body frame. When camera observations are combined with aircraft pose and altitude, detected image features can be projected into local three-dimensional coordinates for evaluating landing-zone dimensions and relative position.

Landing-zone detection can use semantic segmentation to classify image regions according to surface type and operational meaning. Pavement, concrete, grass, soil, gravel, rooftops, water, vegetation, vehicles, people, and structural obstacles can be assigned different categories. Semantic information helps the autonomy system distinguish surfaces that may appear geometrically similar but have very different suitability for supporting a heavy cargo UAV.

Surface geometry is as important as semantic classification. A region identified as ground may still be unsafe because of excessive slope, steps, holes, rocks, debris, or uneven terrain. Stereo vision, structure from motion, monocular depth estimation, or fusion with LiDAR can provide three-dimensional surface information. Local plane fitting and elevation analysis can then estimate slope, roughness, discontinuities, and other geometric properties relevant to landing stability.

Slope estimation should consider the complete aircraft support and clearance region rather than a single local measurement. Even when the center of a candidate zone appears level, one side may contain a significant height change that affects landing gear contact or rotor clearance. The system can therefore evaluate multiple surface samples across the expected footprint and reject areas whose orientation or elevation variation exceeds validated aircraft limits.

Surface roughness represents smaller-scale height variation that may destabilize touchdown or damage landing gear. Rocks, potholes, curbs, debris, vegetation, or construction materials may create unacceptable local discontinuities even when the average surface is nearly horizontal. Vision-derived depth or complementary ranging measurements can estimate these variations, while uncertainty should increase when image texture or viewing geometry does not support reliable reconstruction.

Obstacle detection must include both permanent structures and temporary hazards. Poles, fences, walls, trees, cables, parked vehicles, containers, equipment, and other objects may interfere with the aircraft during descent or touchdown. The protected region should include not only the landing gear footprint but also fuselage, wings, rotors, propellers, payload structures, and safety clearance required for aerodynamic and control disturbances.

People and moving vehicles require special treatment because a landing zone that was safe several seconds earlier may become unsafe during final approach. Visual object detection and tracking can identify dynamic hazards and estimate whether they are entering the protected landing area. The landing decision should therefore remain continuously revisable until touchdown rather than becoming permanently committed when the candidate zone is first accepted.

Landing-zone assessment must also evaluate the approach and departure volume above the surface. A clear touchdown point is insufficient if trees, buildings, cranes, cables, walls, or other structures obstruct the descent corridor. The perception system should examine a three-dimensional safety volume extending above and around the candidate region so trajectory planning can verify that the UAV can approach, descend, hover, abort, and climb away without collision.

Visual markers can improve precision when landing on prepared infrastructure. Fiducial markers, high-contrast patterns, known pad geometry, or other validated visual references can provide relative position, orientation, and scale. As the UAV descends, marker observations can support increasingly precise alignment. The system should nevertheless maintain independent environmental hazard detection because successful marker recognition does not guarantee that the surrounding landing area remains clear.

Unprepared landing zones require greater reliance on general scene understanding because no dedicated marker or known geometry may be available. The perception system must search for regions that satisfy minimum size, slope, roughness, obstacle-clearance, and accessibility requirements. Candidate zones can be scored according to these criteria, allowing the autonomy system to compare alternatives rather than simply selecting the first apparently open surface.

Candidate scoring should include uncertainty as well as estimated suitability. A visually smooth region may receive a lower score if depth reconstruction is unreliable, shadows hide portions of the surface, or vegetation prevents confirmation of ground geometry. Conversely, a slightly less convenient area with strong visual evidence and clear boundaries may be safer. The scoring process should therefore favor validated knowledge over optimistic interpretation of poorly observed space.

Lighting conditions can significantly affect visual landing assessment. Strong shadows, low sun angles, glare, night operation, rapidly changing exposure, and backlighting can hide obstacles or alter apparent surface texture. High dynamic range imaging, controlled exposure, infrared or thermal sensing, and complementary active sensors can improve robustness. The system should explicitly reduce confidence when illumination falls outside conditions for which visual performance has been validated.

Weather and environmental contamination create additional difficulties. Rain, fog, snow, dust, rotor-induced debris, lens contamination, and condensation can degrade image quality during the most safety-critical phase of flight. Rotor downwash may also move vegetation, dust, lightweight objects, or loose materials after the descent begins. Continuous perception is therefore necessary because the landing environment itself can change in response to the approaching aircraft.

Altitude influences both image scale and the type of assessment that is possible. At higher altitude, the camera can evaluate broad candidate regions and approach corridors but may not resolve small hazards. During descent, increasing ground resolution allows detailed inspection of debris, surface discontinuities, people, and landing markers. A hierarchical perception strategy can therefore transition from coarse site selection to progressively finer hazard assessment as the UAV approaches the surface.

Aircraft position and attitude must be synchronized with camera observations to map detected hazards correctly. During descent, horizontal motion, yaw, pitch, roll, and vibration can change the observed scene rapidly. Accurate timestamps and pose estimates allow image measurements to be transformed into a stable local landing frame. Motion compensation and image stabilization can further improve consistency when the aircraft is hovering or maneuvering above the candidate zone.

The final landing-zone decision should use explicit safety criteria rather than a single perception confidence score. Required dimensions, maximum slope, roughness limits, obstacle clearance, dynamic-object exclusion zones, approach-volume clearance, and minimum perception confidence can be evaluated separately. A candidate should be accepted only when the necessary criteria are satisfied within their validated uncertainty bounds.

Abort logic is an essential component of autonomous landing perception. If a person enters the zone, an obstacle appears, visual tracking is lost, surface confidence decreases, or vehicle-state uncertainty becomes excessive, the UAV must be able to stop descent and execute a safe response. Depending on altitude and aircraft dynamics, this may involve hovering, climbing, shifting to another candidate zone, or returning to an earlier approach state.

Real-time processing must maintain bounded latency because landing conditions can change quickly near the ground. Image acquisition, neural-network inference, depth estimation, segmentation, tracking, geometric analysis, candidate scoring, and hazard updates must complete within the timing requirements of the descent profile. Safety-critical processing should receive priority over nonessential perception tasks as the UAV transitions from approach to final landing.

Validation should cover prepared pads and unprepared surfaces across different slopes, textures, obstacle configurations, lighting conditions, weather, and dynamic hazards. Simulation and recorded imagery can support broad scenario coverage, while hardware-in-the-loop and progressive flight tests evaluate real camera behavior and timing. Metrics should include landing-zone detection accuracy, hazard detection probability, false acceptance, false rejection, pose accuracy, processing latency, and abort performance.

Vision-based landing-zone assessment ultimately transforms camera observations into an operational decision about whether and where a cargo UAV can safely land. Its purpose extends beyond recognizing an open surface to evaluating geometry, semantics, dynamic hazards, approach clearance, uncertainty, and changing environmental conditions. Integrated with vehicle-state estimation, depth sensing, trajectory planning, and abort logic, it enables autonomous landing decisions that remain continuously aware of the actual touchdown environment.

비전 기반 착륙 구역 평가(Vision-Based Landing-Zone Assessment)는 화물 무인항공기(Cargo UAV)가 후보 지표면이 자율 착륙에 필요한 충분한 기하학적, 환경적 및 운용적 안전성을 제공하는지를 판단할 수 있도록 한다. 사전에 정의된 좌표로 항법하는 것과 달리 안전한 착륙을 위해서는 실제 접지 직전의 국부 환경을 이해해야 한다. 따라서 인지 시스템은 불확실성과 변화하는 조건을 고려하면서 지표면 경계, 경사, 거칠기, 장애물, 사람, 차량, 식생 및 사용 가능한 안전공간을 평가해야 한다.

평가 과정은 일반적으로 하향 또는 경사 방향 카메라 영상에서 후보 착륙 영역(Candidate Landing Region)을 탐지하는 것으로 시작한다. 사전에 정의된 착륙 패드는 시각 마커(Visual Marker), 알려진 기하 구조 또는 학습된 특징을 통해 인식할 수 있지만, 비정형 착륙 구역(Unprepared Landing Area)은 보다 광범위한 장면 해석이 필요하다. 시스템은 항공기 외형, 로터 안전거리, 위치 오차 및 화물 운용에 필요한 추가 안전 여유를 수용할 만큼 충분히 큰 연속 영역을 식별해야 한다.

카메라 구성(Camera Configuration)은 착륙 구역 인지에 큰 영향을 미친다. 하향 카메라(Downward-Facing Camera)는 접지 영역을 직접 관측하며, 전방 또는 경사 카메라(Forward or Oblique Camera)는 항공기가 해당 영역의 바로 위에 도달하기 전에 접근 구역을 검사할 수 있다. 광각 광학계(Wide-Angle Optics)는 관측 범위를 확대하고 고해상도 카메라는 작은 위험 요소의 탐지 성능을 향상시킨다. 다중 카메라 구성은 넓은 상황 인식과 상세 검사를 결합하면서 기체 또는 화물로 인해 발생하는 사각지대를 줄일 수 있다.

착륙과 관련된 영상 관측값을 실제 측정값으로 변환하려면 기하학적 보정(Geometric Calibration)이 필요하다. 카메라 내부 파라미터(Camera Intrinsic Parameter)는 픽셀과 관측 광선 사이의 관계를 정의하고, 외부 보정(Extrinsic Calibration)은 무인항공기 기체 좌표계를 기준으로 카메라의 위치와 방향을 설정한다. 카메라 관측값을 항공기 자세 및 고도와 결합하면 탐지된 영상 특징을 국부 3차원 좌표로 투영하여 착륙 구역의 크기와 상대 위치를 평가할 수 있다.

착륙 구역 탐지(Landing-Zone Detection)는 의미론적 분할(Semantic Segmentation)을 이용하여 영상 영역을 지표면 유형과 운용적 의미에 따라 분류할 수 있다. 포장면, 콘크리트, 잔디, 토양, 자갈, 옥상, 수면, 식생, 차량, 사람 및 구조적 장애물을 서로 다른 범주로 구분할 수 있다. 의미론적 정보는 기하학적으로 유사하게 보이지만 무거운 화물 무인항공기를 지지하는 적합성이 크게 다른 지표면을 자율 시스템이 구별하도록 지원한다.

지표면 기하 구조(Surface Geometry)는 의미론적 분류만큼 중요하다. 지면으로 식별된 영역이라도 과도한 경사, 계단, 구멍, 암석, 잔해 또는 불규칙한 지형으로 인해 안전하지 않을 수 있다. 스테레오 비전(Stereo Vision), 운동 기반 구조 복원(Structure from Motion), 단안 깊이 추정(Monocular Depth Estimation) 또는 라이다(LiDAR)와의 융합을 통해 3차원 지표면 정보를 획득할 수 있다. 이후 국부 평면 피팅(Local Plane Fitting)과 고도 분석을 통해 착륙 안정성과 관련된 경사, 거칠기, 불연속성 및 기타 기하학적 특성을 추정할 수 있다.

경사 추정(Slope Estimation)은 하나의 국부 측정값이 아니라 항공기 전체의 지지 및 안전 영역을 고려해야 한다. 후보 구역의 중심이 평탄하게 보이더라도 한쪽 영역에 상당한 높이 변화가 존재하면 착륙장치 접촉이나 로터 안전거리에 영향을 줄 수 있다. 따라서 시스템은 예상 항공기 점유 영역 전체에서 여러 지표면 샘플을 평가하고 방향 또는 고도 변화가 검증된 항공기 허용 한계를 초과하는 영역을 제외할 수 있다.

지표면 거칠기(Surface Roughness)는 접지 안정성을 저하시거나 착륙장치를 손상시킬 수 있는 소규모 높이 변화를 나타낸다. 암석, 포트홀(Pothole), 연석, 잔해, 식생 또는 건설 자재는 평균 지표면이 거의 수평이더라도 허용할 수 없는 국부 불연속성을 만들 수 있다. 비전 기반 깊이 또는 상호보완적 거리 측정값을 이용하여 이러한 변화를 추정할 수 있으며, 영상 텍스처나 관측 기하가 신뢰할 수 있는 복원을 지원하지 못할 때는 불확실성을 증가시켜야 한다.

장애물 탐지(Obstacle Detection)는 영구적인 구조물뿐만 아니라 일시적인 위험 요소도 포함해야 한다. 기둥, 울타리, 벽, 나무, 케이블, 주차된 차량, 컨테이너, 장비 및 기타 객체는 하강이나 접지 과정에서 항공기와 간섭할 수 있다. 보호 영역(Protected Region)은 착륙장치의 점유 영역뿐만 아니라 동체, 날개, 로터, 프로펠러, 화물 구조물 및 공기역학적·제어적 교란을 고려한 안전 여유 공간까지 포함해야 한다.

사람과 이동 차량은 몇 초 전에 안전했던 착륙 구역도 최종 접근 과정에서 위험하게 만들 수 있으므로 특별하게 처리해야 한다. 시각 객체 탐지 및 추적(Visual Object Detection and Tracking)을 통해 동적 위험 요소를 식별하고 이들이 보호 착륙 영역으로 진입하는지를 추정할 수 있다. 따라서 착륙 결정은 후보 구역이 처음 승인되었을 때 영구적으로 확정되는 것이 아니라 실제 접지가 이루어질 때까지 지속적으로 재평가할 수 있어야 한다.

착륙 구역 평가는 지표면 위의 접근 및 이탈 공간(Approach and Departure Volume)도 평가해야 한다. 나무, 건물, 크레인, 케이블, 벽 또는 기타 구조물이 하강 회랑을 방해한다면 접지 지점 자체가 깨끗하더라도 충분하지 않다. 인지 시스템은 후보 영역의 상부와 주변으로 확장되는 3차원 안전 공간을 검사하여 궤적 계획 시스템이 무인항공기의 접근, 하강, 호버링(Hovering), 착륙 중단 및 재상승을 충돌 없이 수행할 수 있는지를 검증하도록 해야 한다.

시각 마커(Visual Marker)는 준비된 기반 시설에 착륙할 때 정밀도를 향상시킬 수 있다. 기준 마커(Fiducial Marker), 고대비 패턴, 알려진 착륙 패드 기하 또는 검증된 기타 시각 기준은 상대 위치, 방향 및 스케일 정보를 제공할 수 있다. 무인항공기가 하강함에 따라 마커 관측값을 이용하여 더욱 정밀한 정렬을 수행할 수 있다. 그러나 마커가 성공적으로 인식되었다고 해서 주변 착륙 영역이 계속 안전하다는 것을 보장하지 않으므로 독립적인 환경 위험 탐지 기능을 유지해야 한다.

비정형 착륙 구역(Unprepared Landing Zone)은 전용 마커나 알려진 기하 구조가 존재하지 않을 수 있으므로 일반적인 장면 이해(General Scene Understanding)에 더 크게 의존한다. 인지 시스템은 최소 크기, 경사, 거칠기, 장애물 안전거리 및 접근성 요구조건을 만족하는 영역을 탐색해야 한다. 이러한 기준에 따라 후보 구역에 점수(Candidate Score)를 부여하면 자율 시스템은 처음 발견한 개방된 지표면을 즉시 선택하는 대신 여러 대안을 비교할 수 있다.

후보 점수 평가(Candidate Scoring)는 추정된 적합성뿐만 아니라 불확실성도 포함해야 한다. 시각적으로 매끄러운 영역이라도 깊이 복원의 신뢰성이 낮거나 그림자가 지표면 일부를 가리거나 식생 때문에 실제 지면 기하를 확인할 수 없다면 낮은 점수를 받을 수 있다. 반대로 다소 불편하더라도 강한 시각적 증거와 명확한 경계를 가진 영역이 더 안전할 수 있다. 따라서 평가 과정은 제대로 관측되지 않은 공간에 대한 낙관적 해석보다 검증된 정보를 우선해야 한다.

조명 조건(Lighting Condition)은 시각 기반 착륙 평가에 상당한 영향을 미칠 수 있다. 강한 그림자, 낮은 태양 고도, 눈부심, 야간 운용, 급격한 노출 변화 및 역광은 장애물을 숨기거나 지표면의 외관을 변화시킬 수 있다. 높은 동적 범위 영상(High Dynamic Range Imaging), 제어된 노출, 적외선 또는 열화상 센싱(Infrared or Thermal Sensing) 및 상호보완적 능동 센서를 이용하여 강건성을 향상시킬 수 있다. 시각 성능이 검증된 조건을 벗어난 조명 환경에서는 시스템이 명시적으로 신뢰도를 낮춰야 한다.

기상과 환경 오염(Environmental Contamination)은 추가적인 어려움을 발생시킨다. 비, 안개, 눈, 먼지, 로터로 인해 발생하는 비산 잔해, 렌즈 오염 및 결로는 비행에서 가장 안전이 중요한 단계에 영상 품질을 저하시킬 수 있다. 또한 로터 다운워시(Rotor Downwash)는 하강이 시작된 이후 식생, 먼지, 가벼운 물체 또는 느슨한 물질을 움직일 수 있다. 따라서 접근하는 항공기 자체로 인해 착륙 환경이 변화할 수 있으므로 지속적인 인지가 필요하다.

고도(Altitude)는 영상 스케일과 수행 가능한 평가 유형 모두에 영향을 준다. 높은 고도에서는 카메라가 넓은 후보 영역과 접근 회랑을 평가할 수 있지만 작은 위험 요소를 충분히 식별하지 못할 수 있다. 하강하면서 지상 해상도(Ground Resolution)가 향상되면 잔해, 지표면 불연속성, 사람 및 착륙 마커를 더욱 상세하게 검사할 수 있다. 따라서 계층적 인지 전략(Hierarchical Perception Strategy)은 무인항공기가 지표면에 접근함에 따라 개략적인 착륙지 선택에서 점진적으로 정밀한 위험 평가로 전환할 수 있다.

탐지된 위험 요소를 정확하게 지도화하기 위해서는 항공기 위치 및 자세가 카메라 관측값과 동기화되어야 한다. 하강 과정에서 수평 운동, 요(Yaw), 피치(Pitch), 롤(Roll) 및 진동으로 인해 관측 장면이 빠르게 변화할 수 있다. 정확한 타임스탬프와 자세 추정값을 이용하면 영상 측정값을 안정적인 지역 착륙 좌표계(Local Landing Frame)로 변환할 수 있다. 운동 보상(Motion Compensation)과 영상 안정화(Image Stabilization)는 후보 구역 상공에서 호버링하거나 기동할 때 일관성을 더욱 향상시킬 수 있다.

최종 착륙 구역 결정(Final Landing-Zone Decision)은 하나의 인지 신뢰도 점수에 의존하기보다 명시적인 안전 기준을 사용해야 한다. 요구되는 구역 크기, 최대 경사, 거칠기 한계, 장애물 안전거리, 동적 객체 제외 영역(Dynamic-Object Exclusion Zone), 접근 공간의 안전성 및 최소 인지 신뢰도를 각각 평가할 수 있다. 후보 구역은 필요한 모든 기준이 검증된 불확실성 범위 내에서 만족되는 경우에만 착륙 가능 영역으로 승인되어야 한다.

착륙 중단 로직(Abort Logic)은 자율 착륙 인지의 필수적인 구성요소이다. 사람이 구역에 진입하거나 장애물이 새롭게 나타나고, 시각 추적이 상실되거나 지표면에 대한 신뢰도가 감소하며, 기체 상태 불확실성이 지나치게 증가하면 무인항공기는 하강을 중단하고 안전한 대응을 수행할 수 있어야 한다. 고도와 항공기 동역학에 따라 호버링, 상승, 다른 후보 구역으로 이동 또는 이전 접근 상태로 복귀하는 동작을 수행할 수 있다.

실시간 처리(Real-Time Processing)는 지면 가까이에서 착륙 조건이 빠르게 변할 수 있으므로 제한된 지연시간(Bounded Latency)을 유지해야 한다. 영상 획득, 신경망 추론, 깊이 추정, 분할, 추적, 기하학적 분석, 후보 점수 계산 및 위험 요소 갱신은 하강 프로파일(Descent Profile)의 시간 요구조건 내에서 완료되어야 한다. 무인항공기가 접근 단계에서 최종 착륙 단계로 전환함에 따라 안전에 중요한 처리 기능이 비필수적인 인지 작업보다 높은 우선순위를 가져야 한다.

검증(Validation)은 서로 다른 경사, 텍스처, 장애물 구성, 조명 조건, 기상 및 동적 위험 요소를 포함하는 준비된 착륙 패드와 비정형 지표면을 모두 대상으로 수행해야 한다. 시뮬레이션과 기록 영상은 광범위한 시나리오를 시험하는 데 활용할 수 있으며, 하드웨어 인 더 루프(Hardware-in-the-Loop, HIL)와 단계적 비행시험을 통해 실제 카메라 동작과 시간 특성을 평가할 수 있다. 평가 지표에는 착륙 구역 탐지 정확도, 위험 요소 탐지 확률, 오수락(False Acceptance), 오거부(False Rejection), 자세 정확도, 처리 지연시간 및 착륙 중단 성능이 포함되어야 한다.

궁극적으로 비전 기반 착륙 구역 평가(Vision-Based Landing-Zone Assessment)는 카메라 관측값을 화물 무인항공기가 어디에, 그리고 안전하게 착륙할 수 있는지를 판단하는 운용 의사결정으로 변환한다. 그 목적은 단순히 개방된 지표면을 인식하는 것을 넘어 기하 구조, 의미론적 정보, 동적 위험 요소, 접근 안전공간, 불확실성 및 변화하는 환경 조건을 종합적으로 평가하는 것이다. 기체 상태 추정, 깊이 센싱, 궤적 계획 및 착륙 중단 로직과 통합함으로써 실제 접지 환경을 지속적으로 인식하는 자율 착륙 의사결정을 가능하게 한다.

##  

## 05.10. Sensor Failure Detection and Fusion Fallback [w/Code]

![](images/image10.png){width="7.268055555555556in" height="7.268055555555556in"}

Sensor failure detection and fusion fallback provide the fault-tolerant perception layer required for cargo UAV autonomy. The aircraft depends on IMU, GNSS, cameras, LiDAR, radar, barometers, altimeters, and other sensors whose outputs can degrade independently or simultaneously. The fusion architecture must identify abnormal behavior, estimate the remaining information quality, isolate unreliable measurements, and continue producing a qualified vehicle state whenever sufficient sensing capability remains.

Sensor failures are not limited to complete loss of data. A device may continue transmitting while producing biased, frozen, delayed, noisy, saturated, or physically inconsistent measurements. Calibration parameters can change because of vibration or mechanical displacement, while environmental conditions can reduce sensing quality without creating an explicit hardware fault. Failure detection must therefore monitor both communication health and the statistical consistency of the measurements themselves.

Hard failures are generally easier to detect because they produce missing frames, communication timeouts, invalid status flags, power loss, or complete signal disappearance. Soft failures are more difficult because the data remain numerically plausible. Slowly drifting IMU bias, GNSS multipath, camera degradation, LiDAR contamination, radar interference, or incorrect barometric pressure can influence the fused estimate gradually before conventional device-health checks report a fault.

A fault-detection architecture should combine sensor self-diagnostics with independent consistency monitoring. Built-in status information can report internal temperature, communication errors, saturation, synchronization state, or hardware alarms. The fusion system should independently compare measurements with predicted vehicle behavior and complementary sensors, preventing a nominal device-health flag from being interpreted automatically as proof that its measurements are suitable for navigation or perception.

Innovation monitoring within an Extended Kalman Filter provides one important mechanism for detecting inconsistent measurements. Each incoming observation can be compared with the value predicted from the current state. The residual is evaluated relative to its expected covariance, allowing statistically abnormal observations to be identified. A single large residual may be rejected temporarily, while persistent or correlated residuals can indicate a sensor fault, calibration problem, or incorrect uncertainty model.

Residual monitoring should distinguish transient outliers from persistent degradation. A GNSS measurement disturbed briefly by multipath or a camera observation corrupted by glare should not necessarily cause permanent sensor isolation. Fault logic can therefore use persistence counters, sliding statistics, confidence trends, and recovery conditions. This prevents rapid switching between healthy and failed states while still allowing genuinely degraded sensors to be removed before they significantly corrupt the fused solution.

Cross-sensor consistency checks provide another layer of protection. GNSS velocity can be compared with inertial propagation, barometric altitude with GNSS and terrain-relative altitude, visual motion with IMU rotation, and LiDAR geometry with expected vehicle movement. Agreement does not prove that every sensor is correct, but significant disagreement can identify candidates for further diagnosis, particularly when several independent sources support one interpretation and a single source diverges.

Redundant sensors enable stronger fault isolation when their failure modes are sufficiently independent. Multiple IMUs, GNSS receivers, altimeters, or perception sensors can provide overlapping observations that support voting or analytical redundancy. However, identical sensors may share vulnerabilities to temperature, electromagnetic interference, vibration, software defects, or environmental conditions. Redundancy should therefore be evaluated according to failure independence rather than simply counting the number of installed devices.

Fault isolation determines which sensor or measurement channel is responsible after an inconsistency is detected. Removing the wrong sensor can make the navigation solution worse, especially when the apparently inconsistent measurement is actually correct and another sensor has drifted. Isolation logic should consider residual patterns, sensor history, independent references, hardware diagnostics, environmental context, and the observability of the remaining estimator before changing fusion configuration.

Once a measurement source is considered unreliable, the fusion system can reject it, reduce its statistical weight, or restrict the types of information accepted from it. A GNSS receiver might retain reliable Doppler while position measurements are rejected, or a camera may continue providing semantic detections after visual odometry becomes unreliable. Channel-level degradation allows the architecture to preserve useful information rather than treating every partial failure as a complete sensor loss.

Fusion fallback should be designed as a set of validated operating configurations rather than improvised behavior after a failure. A nominal configuration may combine IMU, GNSS, barometer, camera, LiDAR, and radar information, while degraded configurations use smaller subsets. Each fallback mode should define which states remain observable, how quickly uncertainty grows, which autonomy functions remain permitted, and what operational limits must be imposed.

GNSS loss provides a representative fallback case. The estimator can continue propagating position, velocity, and attitude using inertial measurements while visual or LiDAR odometry provides relative motion constraints. Barometric altitude and terrain-relative ranging may preserve vertical information. Because global position uncertainty generally increases without satellite reference, the system must propagate that uncertainty and eventually restrict operations if required navigation integrity can no longer be maintained.

IMU degradation presents a different challenge because inertial sensing normally supports the highest-rate state propagation required by flight control. A redundant IMU can be selected when available, but switching must account for different biases, alignment, scale factors, and timestamps. If no suitable inertial source remains, many high-dynamic autonomous functions may become unavailable even when slower external sensors continue operating, requiring transition to a predefined safety response.

Camera failure can result from darkness, glare, fog, contamination, excessive vibration, exposure errors, or computing faults rather than physical camera loss. The fusion system can suspend visual odometry or vision-based hazard measurements while retaining LiDAR, radar, GNSS, and inertial sensing. If the camera is essential for landing-zone assessment or semantic classification, the aircraft may continue navigation while prohibiting autonomous landing until the required visual capability is restored.

LiDAR degradation may appear as reduced point density, blocked sectors, abnormal intensity distributions, missing scans, or inconsistent geometric registration. Radar can preserve obstacle range and velocity information in some conditions, while cameras may continue semantic and visual-geometric perception. The fallback architecture must recognize that these modalities are not equivalent replacements, so reduced geometric resolution or obstacle-detection confidence should be reflected in speed, clearance, and mission restrictions.

Radar failure similarly removes a sensing modality with valuable range, Doppler, and adverse-visibility characteristics. Camera and LiDAR perception may provide sufficient obstacle information in favorable conditions, but the operational envelope may need to exclude weather or visibility states for which radar provided critical diversity. Fallback logic should therefore consider the environment and mission phase rather than assuming that a fixed sensor subset always provides the same safety capability.

Estimator covariance is central to graceful degradation. When a measurement source disappears, the corresponding uncertainty should grow according to the remaining process model and available observations. A fallback solution that continues publishing smooth position or attitude values without increasing uncertainty can be more dangerous than an obvious failure because downstream systems may incorrectly trust an increasingly inaccurate state.

Observability should be evaluated whenever the fusion configuration changes. Removing one sensor may leave some states well constrained while others become weakly observable or completely unobservable. For example, relative visual-inertial navigation may maintain local motion while absolute geographic position drifts. The autonomy system should understand which components of the state remain trustworthy instead of interpreting a single overall estimator-health flag as evidence that every navigation quantity remains valid.

Fallback transitions should avoid abrupt discontinuities in the state supplied to flight control and planning. Switching between sensors or estimator modes can introduce position, velocity, attitude, or bias offsets. State alignment, covariance adjustment, blending, and bounded correction can maintain continuity while the estimator adopts the replacement information source. Safety-critical control loops should not receive large artificial transients merely because the sensing configuration changed.

Sensor recovery requires the same discipline as sensor isolation. A previously failed device should not immediately regain full fusion authority after producing one valid measurement. Recovery logic can require stable diagnostics, statistically consistent residuals, correct synchronization, and agreement over a defined observation period. The sensor can then be reintroduced gradually so an intermittent fault does not repeatedly destabilize the estimator through rapid removal and reinsertion.

The fault-management state should be visible to downstream autonomy functions. Flight control, obstacle avoidance, route planning, landing, and mission management need to know which sensors are unavailable, which estimates are degraded, and how much uncertainty remains. A perception system should therefore publish sensor-health states, active fusion configuration, covariance, integrity status, and applicable operational limitations together with its primary state and environment estimates.

Mission phase influences the severity of a particular sensor failure. Loss of a landing camera during cruise may permit continued flight, while the same failure during final approach may require an immediate abort. GNSS degradation may be manageable when strong local localization is available but critical during long-range navigation. Fault response should therefore combine sensor condition with flight phase, environmental context, available redundancy, and the requirements of the active autonomy function.

Real-time fault management must operate independently enough that overload in a perception algorithm does not prevent detection of its own failure. Watchdogs, heartbeat monitoring, timestamp checks, execution-time limits, queue-depth monitoring, and data-freshness tests can identify computational failures in addition to physical sensor faults. Safety-critical health monitoring should remain deterministic and computationally bounded even when the main perception pipeline experiences abnormal load.

Validation must intentionally inject failures rather than test only nominal sensor performance. Simulation, recorded-data replay, software-in-the-loop, hardware-in-the-loop, and flight testing can introduce dropouts, frozen values, bias ramps, timestamp offsets, excessive noise, calibration errors, corrupted measurements, blocked sensors, and processing delays. Detection time, false alarms, isolation accuracy, estimator error, covariance consistency, fallback transition quality, and recovery behavior should be measured.

Sensor failure detection and fusion fallback ultimately determine whether a multi-sensor cargo UAV remains trustworthy when its perception system is no longer operating nominally. Robust autonomy requires more than redundancy: it requires continuous health assessment, fault isolation, uncertainty-aware reconfiguration, validated degraded modes, and safe recovery. By coordinating these mechanisms, the UAV can preserve essential navigation and perception functions when possible and transition to conservative contingency behavior when the remaining sensing capability is no longer sufficient.

센서 고장 탐지 및 융합 폴백(Sensor Failure Detection and Fusion Fallback)은 화물 무인항공기(Cargo UAV)의 자율 운용에 필요한 고장 허용 인지 계층(Fault-Tolerant Perception Layer)을 제공한다. 항공기는 관성측정장치(IMU), 위성항법시스템(GNSS), 카메라, 라이다(LiDAR), 레이더(Radar), 기압계, 고도계 및 기타 센서에 의존하며, 이들의 출력은 개별적으로 또는 동시에 성능이 저하될 수 있다. 융합 아키텍처는 비정상 동작을 식별하고, 남아 있는 정보의 품질을 평가하며, 신뢰할 수 없는 측정값을 격리하고, 충분한 센싱 능력이 유지되는 경우 검증된 기체 상태를 지속적으로 생성해야 한다.

센서 고장(Sensor Failure)은 데이터가 완전히 사라지는 경우에만 국한되지 않는다. 장치는 데이터를 계속 전송하면서도 바이어스가 발생하거나 값이 고정되고, 지연되거나 노이즈가 증가하며, 포화되거나 물리적으로 일관되지 않은 측정값을 생성할 수 있다. 진동이나 기계적 변위로 인해 보정 파라미터가 변할 수 있으며, 환경 조건은 명시적인 하드웨어 고장을 발생시키지 않으면서 센싱 품질을 저하시킬 수 있다. 따라서 고장 탐지는 통신 상태뿐만 아니라 측정값 자체의 통계적 일관성도 감시해야 한다.

하드 고장(Hard Failure)은 일반적으로 프레임 누락, 통신 타임아웃, 유효하지 않은 상태 플래그, 전원 손실 또는 완전한 신호 소실을 발생시키므로 비교적 탐지하기 쉽다. 소프트 고장(Soft Failure)은 데이터가 수치적으로 정상처럼 보이기 때문에 탐지가 더 어렵다. 서서히 증가하는 관성측정장치 바이어스, 위성항법 다중경로(GNSS Multipath), 카메라 성능 저하, 라이다 오염, 레이더 간섭 또는 잘못된 기압값은 일반적인 장치 상태 점검에서 고장을 보고하기 전에 융합 추정값에 점진적으로 영향을 줄 수 있다.

고장 탐지 아키텍처(Fault-Detection Architecture)는 센서 자체 진단과 독립적인 일관성 감시를 결합해야 한다. 내장 상태 정보는 내부 온도, 통신 오류, 포화, 동기화 상태 또는 하드웨어 경보를 보고할 수 있다. 융합 시스템은 측정값을 예측된 기체 거동 및 상호보완 센서와 독립적으로 비교하여 장치 상태 플래그가 정상이라는 이유만으로 해당 측정값이 항법이나 인지에 적합하다고 자동 판단하는 것을 방지해야 한다.

확장 칼만 필터(Extended Kalman Filter, EKF)의 이노베이션 감시(Innovation Monitoring)는 일관되지 않은 측정값을 탐지하는 중요한 방법 중 하나이다. 각각의 입력 관측값을 현재 상태로부터 예측된 값과 비교할 수 있으며, 그 잔차(Residual)를 예상 공분산(Expected Covariance)에 상대적으로 평가하여 통계적으로 비정상적인 관측값을 식별할 수 있다. 하나의 큰 잔차는 일시적으로 거부할 수 있지만, 지속적이거나 상관된 잔차는 센서 고장, 보정 문제 또는 잘못된 불확실성 모델을 나타낼 수 있다.

잔차 감시(Residual Monitoring)는 일시적인 이상값과 지속적인 성능 저하를 구분해야 한다. 다중경로로 인해 잠시 교란된 위성항법 측정값이나 눈부심으로 손상된 카메라 관측값이 반드시 센서의 영구적인 격리를 요구하는 것은 아니다. 따라서 고장 로직은 지속성 카운터, 이동 통계(Sliding Statistics), 신뢰도 추세 및 복구 조건을 사용할 수 있다. 이를 통해 정상 상태와 고장 상태 사이의 빠른 전환을 방지하면서 실제로 성능이 저하된 센서가 융합 결과를 심각하게 오염시키기 전에 제거할 수 있다.

센서 간 일관성 검사(Cross-Sensor Consistency Check)는 추가적인 보호 계층을 제공한다. 위성항법 속도는 관성 전파(Inertial Propagation)와 비교할 수 있고, 기압 고도는 위성항법 및 지형 상대 고도와 비교할 수 있으며, 시각적 운동은 관성측정장치 회전 정보와, 라이다 기하 정보는 예상 기체 운동과 비교할 수 있다. 센서 간 일치가 모든 센서의 정확성을 보장하지는 않지만, 여러 독립 정보원이 하나의 해석을 지지하고 특정 정보원만 크게 벗어나는 경우 추가 진단이 필요한 센서를 식별할 수 있다.

중복 센서(Redundant Sensor)는 고장 모드가 충분히 독립적일 경우 더욱 강력한 고장 격리를 가능하게 한다. 여러 관성측정장치, 위성항법 수신기, 고도계 또는 인지 센서는 중첩된 관측 정보를 제공하여 투표(Voting) 또는 분석적 중복성(Analytical Redundancy)을 지원할 수 있다. 그러나 동일한 센서들은 온도, 전자기 간섭, 진동, 소프트웨어 결함 또는 환경 조건에 대한 공통 취약성을 가질 수 있다. 따라서 중복성은 단순히 설치된 장치 수가 아니라 고장 독립성(Failure Independence)을 기준으로 평가해야 한다.

고장 격리(Fault Isolation)는 불일치가 탐지된 이후 어떤 센서 또는 측정 채널이 원인인지를 판단한다. 잘못된 센서를 제거하면 항법 해가 오히려 악화될 수 있으며, 특히 비정상적으로 보이는 측정값이 실제로는 정확하고 다른 센서가 드리프트하고 있는 경우 문제가 심각해진다. 따라서 격리 로직은 융합 구성을 변경하기 전에 잔차 패턴, 센서 이력, 독립적인 기준 정보, 하드웨어 진단, 환경적 상황 및 남아 있는 추정기의 관측 가능성(Observability)을 고려해야 한다.

측정 정보원이 신뢰할 수 없는 것으로 판단되면 융합 시스템은 해당 정보를 거부하거나 통계적 가중치를 낮추거나 특정 유형의 정보만 제한적으로 사용할 수 있다. 위성항법 수신기는 위치 측정값이 거부된 상태에서도 신뢰할 수 있는 도플러(Doppler) 정보를 유지할 수 있으며, 카메라는 시각 오도메트리(Visual Odometry)가 신뢰할 수 없어진 이후에도 의미론적 탐지 정보를 제공할 수 있다. 이러한 채널 수준 성능 저하(Channel-Level Degradation)는 부분적인 고장을 전체 센서 손실로 처리하지 않고 유용한 정보를 보존하도록 한다.

융합 폴백(Fusion Fallback)은 고장이 발생한 이후 즉흥적으로 동작하는 방식이 아니라 검증된 운용 구성(Validated Operating Configuration)의 집합으로 설계되어야 한다. 정상 구성에서는 관성측정장치, 위성항법시스템, 기압계, 카메라, 라이다 및 레이더 정보를 결합할 수 있으며, 성능 저하 구성에서는 더 작은 센서 부분집합을 사용한다. 각각의 폴백 모드는 어떤 상태가 계속 관측 가능한지, 불확실성이 얼마나 빠르게 증가하는지, 어떤 자율 기능을 계속 허용할 수 있는지, 어떤 운용 제한을 적용해야 하는지를 정의해야 한다.

위성항법 손실(GNSS Loss)은 대표적인 폴백 사례이다. 추정기는 관성 측정값을 이용하여 위치, 속도 및 자세를 계속 전파할 수 있으며, 시각 또는 라이다 오도메트리가 상대 운동 제약을 제공할 수 있다. 기압 고도와 지형 상대 거리 측정은 수직 정보를 유지하는 데 활용할 수 있다. 위성 기준이 없으면 일반적으로 전역 위치 불확실성이 증가하므로 시스템은 이러한 불확실성을 지속적으로 전파하고 필요한 항법 무결성(Navigation Integrity)을 더 이상 유지할 수 없을 경우 운용을 제한해야 한다.

관성측정장치 성능 저하(IMU Degradation)는 관성 센싱이 일반적으로 비행 제어에 필요한 가장 높은 주기의 상태 전파를 지원하기 때문에 다른 문제를 발생시킨다. 사용 가능한 경우 중복 관성측정장치로 전환할 수 있지만, 전환 과정에서는 서로 다른 바이어스, 정렬, 스케일 계수 및 타임스탬프를 고려해야 한다. 적절한 관성 정보원이 남아 있지 않다면 느린 외부 센서가 계속 동작하더라도 많은 고동적 자율 기능을 사용할 수 없게 되므로 사전에 정의된 안전 대응으로 전환해야 할 수 있다.

카메라 고장(Camera Failure)은 물리적인 카메라 손실뿐만 아니라 어둠, 눈부심, 안개, 오염, 과도한 진동, 노출 오류 또는 연산 장치의 문제로 발생할 수 있다. 융합 시스템은 시각 오도메트리 또는 비전 기반 위험 측정을 중단하면서 라이다, 레이더, 위성항법 및 관성 센싱을 계속 사용할 수 있다. 카메라가 착륙 구역 평가(Landing-Zone Assessment) 또는 의미론적 분류에 필수적이라면 항공기는 항법을 계속 수행하더라도 필요한 시각 기능이 복구될 때까지 자율 착륙을 금지할 수 있다.

라이다 성능 저하(LiDAR Degradation)는 포인트 밀도 감소, 차단된 스캔 영역, 비정상적인 반사 강도 분포, 스캔 누락 또는 일관되지 않은 기하학적 정합 형태로 나타날 수 있다. 일부 조건에서는 레이더가 장애물 거리와 속도 정보를 유지할 수 있으며, 카메라는 의미론적 및 시각 기하 인지를 계속 제공할 수 있다. 그러나 이러한 센싱 방식은 서로 완전히 동일한 대체 수단이 아니므로 감소한 기하학적 해상도나 장애물 탐지 신뢰도를 속도, 안전거리 및 임무 제한에 반영해야 한다.

레이더 고장(Radar Failure)은 유용한 거리, 도플러 및 악천후 가시성 특성을 가진 센싱 방식을 제거한다. 양호한 환경에서는 카메라와 라이다 인지가 충분한 장애물 정보를 제공할 수 있지만, 레이더가 핵심적인 센서 다양성(Sensor Diversity)을 제공하던 기상 또는 가시성 조건은 운용 영역에서 제외해야 할 수 있다. 따라서 폴백 로직은 고정된 센서 부분집합이 항상 동일한 안전 능력을 제공한다고 가정하지 않고 환경과 임무 단계를 함께 고려해야 한다.

추정기 공분산(Estimator Covariance)은 점진적 성능 저하(Graceful Degradation)의 핵심 요소이다. 하나의 측정 정보원이 사라지면 해당 상태의 불확실성은 남아 있는 프로세스 모델과 이용 가능한 관측 정보에 따라 증가해야 한다. 위치나 자세 값이 계속 부드럽게 출력되더라도 불확실성이 증가하지 않는 폴백 해는 명백한 고장보다 더 위험할 수 있다. 하위 시스템이 점점 부정확해지는 상태값을 여전히 신뢰할 수 있기 때문이다.

융합 구성이 변경될 때마다 관측 가능성(Observability)을 평가해야 한다. 하나의 센서를 제거하면 일부 상태는 계속 충분히 제약되지만 다른 상태는 약하게 관측되거나 완전히 관측 불가능해질 수 있다. 예를 들어 상대적인 시각-관성 항법(Visual-Inertial Navigation)은 국부 운동을 유지할 수 있지만 절대 지리적 위치는 점차 드리프트할 수 있다. 따라서 자율 시스템은 하나의 전체적인 추정기 상태 플래그만으로 모든 항법 값이 유효하다고 판단하지 않고 어떤 상태 요소가 계속 신뢰할 수 있는지를 이해해야 한다.

폴백 전환(Fallback Transition)은 비행 제어와 계획 시스템에 제공되는 상태값에서 급격한 불연속이 발생하지 않도록 해야 한다. 센서 또는 추정기 모드를 전환하면 위치, 속도, 자세 또는 바이어스 오프셋이 발생할 수 있다. 상태 정렬(State Alignment), 공분산 조정, 블렌딩(Blending) 및 제한된 보정(Bounded Correction)을 이용하면 추정기가 대체 정보원을 적용하는 동안 연속성을 유지할 수 있다. 안전 필수 제어 루프는 단순히 센싱 구성이 변경되었다는 이유로 큰 인위적 과도응답을 받아서는 안 된다.

센서 복구(Sensor Recovery)는 센서 격리와 동일한 수준의 엄격한 절차가 필요하다. 이전에 고장으로 판정된 장치가 하나의 정상 측정값을 생성했다는 이유만으로 즉시 전체 융합 권한을 회복해서는 안 된다. 복구 로직은 안정적인 진단 상태, 통계적으로 일관된 잔차, 정확한 시간 동기화 및 일정 관측 기간 동안 다른 센서와의 일치를 요구할 수 있다. 이후 센서를 점진적으로 다시 융합하여 간헐적인 고장이 반복적인 제거와 재투입을 통해 추정기를 불안정하게 만드는 것을 방지할 수 있다.

고장 관리 상태(Fault-Management State)는 하위 자율 기능에서도 확인할 수 있어야 한다. 비행 제어, 장애물 회피, 경로 계획, 착륙 및 임무 관리 시스템은 어떤 센서를 사용할 수 없고 어떤 추정값의 성능이 저하되었으며 어느 정도의 불확실성이 남아 있는지를 알아야 한다. 따라서 인지 시스템은 주요 상태 및 환경 추정값과 함께 센서 상태, 활성 융합 구성, 공분산, 무결성 상태(Integrity Status) 및 적용되는 운용 제한을 제공해야 한다.

임무 단계(Mission Phase)는 특정 센서 고장의 심각성에 영향을 준다. 순항 중 착륙 카메라가 손실되면 비행을 계속할 수 있지만 최종 접근 중 동일한 고장이 발생하면 즉각적인 착륙 중단이 필요할 수 있다. 강력한 국부 위치추정 기능을 사용할 수 있는 경우 위성항법 성능 저하는 관리할 수 있지만 장거리 항법에서는 치명적일 수 있다. 따라서 고장 대응은 센서 상태뿐만 아니라 비행 단계, 환경적 상황, 사용 가능한 중복성 및 현재 활성화된 자율 기능의 요구조건을 함께 고려해야 한다.

실시간 고장 관리(Real-Time Fault Management)는 인지 알고리즘의 과부하로 인해 해당 알고리즘 자체의 고장을 탐지하지 못하는 상황이 발생하지 않도록 충분히 독립적으로 동작해야 한다. 워치독(Watchdog), 하트비트 감시(Heartbeat Monitoring), 타임스탬프 검사, 실행시간 제한, 큐 깊이 감시 및 데이터 최신성 검사(Data-Freshness Test)를 이용하여 물리적 센서 고장뿐만 아니라 계산 시스템의 고장도 식별할 수 있다. 안전 필수 상태 감시는 주요 인지 파이프라인에 비정상적인 부하가 발생하더라도 결정적이고 제한된 계산시간을 유지해야 한다.

검증(Validation)은 정상적인 센서 성능만 시험하는 것이 아니라 의도적으로 고장을 주입해야 한다. 시뮬레이션, 기록 데이터 재생, 소프트웨어 인 더 루프(Software-in-the-Loop, SIL), 하드웨어 인 더 루프(Hardware-in-the-Loop, HIL) 및 비행시험을 이용하여 데이터 손실, 고정값, 바이어스 증가, 타임스탬프 오프셋, 과도한 노이즈, 보정 오류, 손상된 측정값, 센서 차단 및 처리 지연을 주입할 수 있다. 탐지 시간, 오경보, 격리 정확도, 추정기 오차, 공분산 일관성, 폴백 전환 품질 및 복구 동작을 측정해야 한다.

궁극적으로 센서 고장 탐지 및 융합 폴백(Sensor Failure Detection and Fusion Fallback)은 다중 센서 화물 무인항공기의 인지 시스템이 정상 상태에서 벗어났을 때에도 계속 신뢰할 수 있는지를 결정한다. 강건한 자율성(Robust Autonomy)은 단순한 중복성을 넘어 지속적인 상태 평가, 고장 격리, 불확실성을 고려한 재구성, 검증된 성능 저하 모드 및 안전한 복구를 요구한다. 이러한 메커니즘을 통합적으로 조정함으로써 무인항공기는 가능한 경우 필수 항법 및 인지 기능을 유지하고, 남아 있는 센싱 능력이 더 이상 충분하지 않은 경우 보수적인 비상 대응(Contingency Behavior)으로 안전하게 전환할 수 있다.
