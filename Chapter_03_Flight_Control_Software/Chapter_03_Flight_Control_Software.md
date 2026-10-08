**Volume 23. Cargo UAV Autonomy and Flight AI**


# Chapter 03. Flight Control Software

##  

## 03.01. FCS Architecture Attitude Altitude Position Loop

![](images/image1.png){width="7.268055555555556in" height="7.268055555555556in"}

The flight control system of a cargo UAV is organized as a hierarchy of closed-loop controllers that progressively transform mission-level motion commands into actuator-level control actions. At the core of this hierarchy are the position, altitude, and attitude loops, each operating at a different dynamic timescale. This layered architecture allows the aircraft to maintain stable flight while simultaneously following commanded trajectories and compensating for disturbances.

The position loop forms the outer portion of the flight control hierarchy and regulates the aircraft's horizontal location relative to a commanded reference. Position estimates derived from navigation sensors are compared with desired coordinates, producing position errors that are converted into velocity or acceleration commands. Because translational motion develops more slowly than rotational motion, this loop normally operates at a lower bandwidth than the inner attitude controller.

A velocity control stage is commonly placed between position regulation and attitude control. The controller compares desired horizontal velocity with estimated aircraft velocity and calculates the acceleration required to reduce the error. These acceleration commands are subsequently transformed into desired roll and pitch orientations. The result is a cascaded structure in which horizontal translation is achieved indirectly by controlling the orientation of the vehicle's thrust vector.

Altitude control follows a similar hierarchical principle but operates along the vertical axis. The commanded altitude is compared with the estimated altitude to generate a vertical position error, which is converted into a vertical velocity reference. A vertical-speed controller then determines the thrust adjustment required to climb, descend, or maintain altitude. Separating altitude and vertical-speed regulation improves transient behavior and prevents abrupt thrust changes caused by large altitude errors.

The attitude loop is the fastest major feedback layer and directly stabilizes roll, pitch, and yaw orientation. Desired attitude commands arriving from the outer control loops are compared with estimated orientation, and the resulting errors generate angular-rate references. An inner angular-rate controller then calculates the torque commands necessary to achieve those rates. This cascaded attitude-rate architecture provides rapid disturbance rejection while maintaining predictable aircraft response.

Reliable state information is essential because every feedback loop depends on estimates of the aircraft's actual motion. The flight control software therefore receives attitude, angular velocity, position, velocity, and altitude estimates from the navigation and estimation subsystem. The surrounding chapter structure explicitly separates attitude estimation using EKF, IMU, and GNSS fusion from controller design, establishing estimation and control as closely coupled but independently engineered functions. Volume_23_Cargo_UAV_Autonomy_an...

For a multirotor cargo UAV, the controller outputs cannot be sent directly to individual motors. Desired total thrust and body-axis moments must first pass through a control-allocation or mixing stage that distributes commands across the available propulsion units. The architecture therefore connects attitude, altitude, and position regulation to a multirotor mixing matrix and motor-allocation function, which is treated as a dedicated flight-control function in the volume structure. Volume_23_Cargo_UAV_Autonomy_an...

The different loops must be designed with clear bandwidth separation. Angular-rate regulation is generally the fastest layer, followed by attitude regulation, velocity control, and finally position control. This ordering prevents a slower outer loop from demanding motion that the inner dynamics cannot track. Each outer controller effectively assumes that the controller beneath it can execute its command sufficiently faster than the rate at which the outer reference changes.

Cargo UAVs introduce an additional challenge because aircraft dynamics can vary substantially with payload mass, center-of-gravity location, fuel or battery state, and cargo release. A controller tuned for an unloaded vehicle may therefore respond differently at maximum payload. The FCS architecture should expose vehicle-state and payload-related parameters so that gains, limits, feedforward terms, or control allocation can adapt without compromising the deterministic behavior of the stabilization loops.

Payload changes are especially important for altitude control because the thrust required for hover depends strongly on total vehicle mass. An inaccurate hover-thrust estimate forces the feedback controller to continuously compensate for a systematic error. Feedforward based on estimated mass can provide the nominal thrust requirement, while feedback corrects residual deviations. The chapter structure consequently treats cargo-weight load-change adaptive control as a distinct extension of the basic FCS architecture. Volume_23_Cargo_UAV_Autonomy_an...

External disturbances must also be considered throughout the cascaded loops. Wind can create position drift, velocity error, attitude perturbations, and additional propulsion demand simultaneously. Fast attitude regulation rejects short-duration rotational disturbances, while velocity and position controllers compensate for accumulated translational displacement. Disturbance estimation can further improve performance by providing feedforward compensation instead of requiring every disturbance to appear first as feedback error.

Command limiting is essential between control layers. Position errors should not produce unlimited velocity commands, velocity errors should not demand physically impossible tilt angles, and altitude errors should not generate excessive climb rates or thrust. Rate, acceleration, attitude, thrust, and actuator limits should therefore be enforced at defined interfaces. Anti-windup mechanisms are also required where integral control is used so that saturation does not produce excessive stored controller error.

The FCS must remain coordinated with the flight-mode manager. Manual, semi-automatic, and fully autonomous operation may generate references from different sources, but they should converge through controlled interfaces before reaching safety-critical stabilization functions. Mode transitions require reference synchronization, state initialization, and bumpless transfer so that changing command authority does not suddenly introduce attitude, altitude, velocity, or position discontinuities.

A practical software implementation separates estimation, reference generation, control-law computation, allocation, actuator output, monitoring, and safety supervision while maintaining deterministic timing between them. High-rate attitude and angular-rate functions require predictable execution latency, whereas position and mission-related commands can operate more slowly. This separation also supports independent verification of critical components and simplifies SIL and HIL testing later in the development process.

Fault handling must be integrated without making nominal control logic unnecessarily complex. Invalid navigation estimates, excessive attitude errors, actuator saturation, propulsion degradation, timing overruns, or inconsistent sensor states should be detected by monitoring functions and converted into predefined control responses. Depending on severity, the system may constrain the flight envelope, switch estimation sources, reduce mission authority, initiate return-to-home behavior, or transition toward an emergency landing strategy.

For heavy cargo aircraft, redundancy further affects the architecture because multiple flight-control computers, sensor channels, communication paths, or propulsion controllers may participate in the same control chain. The control-law interfaces should therefore use clearly defined timestamps, validity information, synchronization rules, and command ownership. This complements the volume's broader redundant-computing architecture and later safety functions rather than embedding redundancy policy directly inside every individual controller.

The resulting FCS can be understood as a deterministic conversion chain from desired aircraft motion to physical force and moment generation. Position error produces translational commands, altitude error produces vertical-motion commands, and these outer-loop objectives become attitude and thrust references. Fast attitude and rate loops stabilize the vehicle, while control allocation converts the requested forces and moments into propulsion commands that the aircraft can physically execute.

This hierarchical architecture provides the foundation for the remainder of the flight-control chapter. Attitude estimation supplies the state, PID or LQR methods implement attitude regulation, vertical controllers govern altitude, outer loops regulate position and velocity, and motor allocation converts control effort into actuator commands. Adaptive payload control, wind rejection, flight-mode management, and SIL/HIL verification then extend the same foundation toward a robust cargo-UAV flight-control system.

화물 무인항공기(Cargo UAV)의 비행 제어 시스템(Flight Control System, FCS)은 임무 수준의 운동 명령을 단계적으로 액추에이터 수준의 제어 동작으로 변환하는 계층형 폐루프 제어기(Closed-Loop Controller) 구조로 구성된다. 이 계층의 핵심에는 위치 루프(Position Loop), 고도 루프(Altitude Loop), 자세 루프(Attitude Loop)가 있으며, 각각 서로 다른 동적 시간 척도(Dynamic Timescale)에서 동작한다. 이러한 계층 구조를 통해 항공기는 안정적인 비행을 유지하면서 명령된 궤적을 추종하고 외란(Disturbance)을 동시에 보상할 수 있다.

위치 루프(Position Loop)는 비행 제어 계층의 외부 영역을 구성하며 명령된 기준 위치에 대한 항공기의 수평 위치를 제어한다. 항법 센서(Navigation Sensor)로부터 얻은 위치 추정값(Position Estimate)을 목표 좌표와 비교하여 위치 오차(Position Error)를 계산하고, 이를 속도 또는 가속도 명령으로 변환한다. 병진 운동(Translational Motion)은 회전 운동(Rotational Motion)보다 상대적으로 느리게 발생하므로 이 루프는 일반적으로 내부 자세 제어기(Attitude Controller)보다 낮은 대역폭(Bandwidth)으로 동작한다.

속도 제어 단계(Velocity Control Stage)는 일반적으로 위치 제어와 자세 제어 사이에 배치된다. 제어기는 목표 수평 속도와 추정된 항공기 속도를 비교하여 오차를 감소시키는 데 필요한 가속도를 계산한다. 이러한 가속도 명령은 이후 목표 롤(Roll) 및 피치(Pitch) 자세로 변환된다. 결과적으로 수평 병진 운동은 항공기 추력 벡터(Thrust Vector)의 방향을 제어함으로써 간접적으로 구현되는 직렬 제어 구조(Cascaded Control Structure)를 형성한다.

고도 제어(Altitude Control)는 유사한 계층적 원리를 따르지만 수직축을 중심으로 동작한다. 명령 고도(Commanded Altitude)와 추정 고도(Estimated Altitude)를 비교하여 수직 위치 오차를 생성하고, 이를 수직 속도 기준값(Vertical Velocity Reference)으로 변환한다. 이후 수직 속도 제어기(Vertical-Speed Controller)는 상승, 하강 또는 고도 유지를 위해 필요한 추력 조정량을 결정한다. 고도와 수직 속도 제어를 분리하면 과도 응답(Transient Response)을 개선하고 큰 고도 오차로 인해 급격한 추력 변화가 발생하는 것을 방지할 수 있다.

자세 루프(Attitude Loop)는 주요 피드백 계층 가운데 가장 빠르게 동작하며 롤(Roll), 피치(Pitch), 요(Yaw) 방향을 직접 안정화한다. 외부 제어 루프에서 전달되는 목표 자세 명령을 추정된 자세와 비교하고, 그 오차를 이용하여 각속도 기준값(Angular-Rate Reference)을 생성한다. 내부 각속도 제어기(Angular-Rate Controller)는 다시 해당 각속도를 달성하기 위해 필요한 토크 명령(Torque Command)을 계산한다. 이러한 직렬 자세-각속도 구조(Cascaded Attitude-Rate Architecture)는 예측 가능한 항공기 응답을 유지하면서 빠른 외란 제거 성능을 제공한다.

모든 피드백 루프(Feedback Loop)는 항공기의 실제 운동 상태 추정값에 의존하기 때문에 신뢰할 수 있는 상태 정보(State Information)가 필수적이다. 따라서 비행 제어 소프트웨어(Flight Control Software)는 항법 및 상태 추정 서브시스템(Navigation and Estimation Subsystem)으로부터 자세, 각속도, 위치, 속도 및 고도 추정값을 입력받는다. 전체 구조에서는 확장 칼만 필터(Extended Kalman Filter, EKF), 관성 측정 장치(Inertial Measurement Unit, IMU), 위성항법시스템(Global Navigation Satellite System, GNSS) 융합을 이용한 자세 추정과 제어기 설계를 구분함으로써 상태 추정과 제어를 밀접하게 연동하면서도 독립적으로 설계할 수 있도록 한다.

멀티로터 화물 무인항공기(Multirotor Cargo UAV)의 경우 제어기 출력값을 개별 모터에 직접 전달할 수 없다. 목표 총추력(Total Thrust)과 기체 축 모멘트(Body-Axis Moment)는 먼저 제어 할당(Control Allocation) 또는 믹싱(Mixing) 단계를 거쳐 사용 가능한 추진 장치에 명령을 분배해야 한다. 따라서 비행 제어 구조는 자세, 고도 및 위치 제어 기능을 멀티로터 믹싱 행렬(Multirotor Mixing Matrix)과 모터 할당(Motor Allocation) 기능에 연결하여 최종적인 추진 명령을 생성한다.

각 제어 루프는 명확한 대역폭 분리(Bandwidth Separation)를 기반으로 설계되어야 한다. 각속도 제어(Angular-Rate Control)가 일반적으로 가장 빠른 계층이며, 그다음으로 자세 제어, 속도 제어, 위치 제어가 이어진다. 이러한 순서는 느린 외부 루프가 내부 동역학이 추종할 수 없는 운동을 요구하는 것을 방지한다. 각 외부 제어기는 하위 제어기가 외부 기준값의 변화 속도보다 충분히 빠르게 명령을 실행할 수 있다는 가정을 기반으로 동작한다.

화물 무인항공기는 탑재 화물 질량(Payload Mass), 무게중심(Center of Gravity, CoG) 위치, 연료 또는 배터리 상태, 화물 방출 등에 따라 항공기 동역학(Aircraft Dynamics)이 크게 달라질 수 있다는 추가적인 문제를 가진다. 따라서 무화물 상태에서 조정된 제어기는 최대 화물 탑재 상태에서 서로 다른 응답을 나타낼 수 있다. 비행 제어 시스템은 안정화 루프의 결정론적 동작(Deterministic Behavior)을 훼손하지 않으면서 이득(Gain), 제한값(Limit), 피드포워드(Feedforward) 항 또는 제어 할당을 조정할 수 있도록 기체 상태 및 화물 관련 매개변수를 제공해야 한다.

화물 변화(Payload Change)는 호버링(Hovering)에 필요한 추력이 전체 항공기 질량에 크게 의존하기 때문에 특히 고도 제어에 중요하다. 호버 추력(Hover Thrust)을 부정확하게 추정하면 피드백 제어기가 지속적으로 체계적인 오차를 보상해야 한다. 추정 질량을 기반으로 한 피드포워드(Feedforward)는 기본적인 추력 요구량을 제공할 수 있으며, 피드백(Feedback)은 남아 있는 오차를 보정한다. 따라서 화물 중량 변화에 대응하는 적응 제어(Load-Change Adaptive Control)는 기본 비행 제어 구조를 확장하는 중요한 기능이 된다.

외부 외란(External Disturbance) 역시 전체 직렬 제어 루프에서 고려해야 한다. 바람은 위치 편차(Position Drift), 속도 오차, 자세 교란 및 추가 추진력 요구를 동시에 발생시킬 수 있다. 빠른 자세 제어는 단시간의 회전 외란을 억제하고, 속도 및 위치 제어기는 누적된 병진 위치 편차를 보상한다. 외란 추정(Disturbance Estimation)을 적용하면 모든 외란이 먼저 피드백 오차로 나타난 이후 보상되는 방식 대신 피드포워드 보상(Feedforward Compensation)을 제공하여 제어 성능을 더욱 향상시킬 수 있다.

제어 계층 사이에서는 명령 제한(Command Limiting)이 필수적이다. 위치 오차가 무제한적인 속도 명령으로 변환되어서는 안 되며, 속도 오차가 물리적으로 불가능한 기울기 각도(Tilt Angle)를 요구해서도 안 된다. 또한 고도 오차가 과도한 상승률 또는 추력을 발생시켜서는 안 된다. 따라서 속도, 가속도, 자세, 추력 및 액추에이터 제한값(Actuator Limit)을 정의된 인터페이스에서 적용해야 한다. 적분 제어(Integral Control)를 사용하는 경우 포화(Saturation)로 인해 과도한 누적 제어 오차가 발생하지 않도록 안티와인드업(Anti-Windup) 기능도 필요하다.

비행 제어 시스템은 비행 모드 관리자(Flight-Mode Manager)와 긴밀하게 연동되어야 한다. 수동(Manual), 반자동(Semi-Automatic), 완전 자율(Full Autonomous) 운용에서는 서로 다른 소스로부터 기준 명령이 생성될 수 있지만, 안전 중요 안정화 기능(Safety-Critical Stabilization Function)에 전달되기 전에 제어된 인터페이스를 통해 통합되어야 한다. 모드 전환(Mode Transition) 과정에서는 명령 권한의 변경으로 자세, 고도, 속도 또는 위치에 급격한 불연속이 발생하지 않도록 기준값 동기화(Reference Synchronization), 상태 초기화(State Initialization), 무충격 전환(Bumpless Transfer)이 필요하다.

실제 소프트웨어 구현에서는 상태 추정(Estimation), 기준값 생성(Reference Generation), 제어 법칙 계산(Control-Law Computation), 제어 할당(Control Allocation), 액추에이터 출력(Actuator Output), 모니터링(Monitoring), 안전 감독(Safety Supervision)을 분리하면서 이들 사이의 결정론적 타이밍(Deterministic Timing)을 유지해야 한다. 고속 자세 및 각속도 제어 기능에는 예측 가능한 실행 지연시간(Execution Latency)이 필요하지만, 위치 및 임무 관련 명령은 상대적으로 낮은 주기로 동작할 수 있다. 이러한 분리는 핵심 구성요소의 독립적인 검증을 지원하고 이후 소프트웨어 인 더 루프(Software-in-the-Loop, SIL) 및 하드웨어 인 더 루프(Hardware-in-the-Loop, HIL) 시험을 단순화한다.

고장 처리(Fault Handling)는 정상 제어 로직을 불필요하게 복잡하게 만들지 않으면서 시스템에 통합되어야 한다. 유효하지 않은 항법 추정값, 과도한 자세 오차, 액추에이터 포화, 추진 시스템 성능 저하, 타이밍 초과 또는 센서 상태 불일치는 모니터링 기능에 의해 감지되고 사전에 정의된 제어 대응으로 변환되어야 한다. 심각도에 따라 시스템은 비행 영역(Flight Envelope)을 제한하거나 상태 추정 소스를 전환하고, 임무 제어 권한을 축소하거나 귀환(Return-to-Home) 또는 비상 착륙(Emergency Landing) 절차로 전환할 수 있다.

대형 화물 무인항공기(Heavy Cargo UAV)에서는 여러 비행 제어 컴퓨터(Flight Control Computer), 센서 채널, 통신 경로 또는 추진 제어기가 동일한 제어 체인(Control Chain)에 참여할 수 있기 때문에 이중화(Redundancy)가 전체 구조에 추가적인 영향을 준다. 따라서 제어 법칙 인터페이스(Control-Law Interface)는 명확하게 정의된 타임스탬프(Timestamp), 유효성 정보(Validity Information), 동기화 규칙(Synchronization Rule), 명령 소유권(Command Ownership)을 사용해야 한다. 이러한 설계는 각각의 개별 제어기에 이중화 정책을 직접 포함하지 않으면서 상위의 이중화 컴퓨팅 구조(Redundant Computing Architecture) 및 안전 기능과 연계될 수 있도록 한다.

결과적으로 비행 제어 시스템은 목표 항공기 운동을 실제 힘과 모멘트 생성으로 변환하는 결정론적 변환 체인(Deterministic Conversion Chain)으로 이해할 수 있다. 위치 오차는 병진 운동 명령을 생성하고, 고도 오차는 수직 운동 명령을 생성하며, 이러한 외부 루프의 목표는 자세 및 추력 기준값으로 변환된다. 빠른 자세 및 각속도 루프가 기체를 안정화하고, 제어 할당은 요구된 힘과 모멘트를 항공기가 실제로 실행할 수 있는 추진 명령으로 변환한다.

이러한 계층형 구조(Hierarchical Architecture)는 이후 비행 제어 소프트웨어의 모든 세부 기능을 위한 기반을 제공한다. 자세 추정은 필요한 상태 정보를 제공하고, 비례-적분-미분 제어(Proportional-Integral-Derivative, PID) 또는 선형 이차 조절기(Linear Quadratic Regulator, LQR)는 자세 제어를 구현하며, 수직 제어기는 고도를 조절하고 외부 루프는 위치와 속도를 제어한다. 이후 모터 할당(Motor Allocation)이 제어 출력을 액추에이터 명령으로 변환하며, 화물 적응 제어, 바람 외란 제거, 비행 모드 관리, SIL/HIL 검증이 동일한 기반을 확장하여 견고한 화물 무인항공기 비행 제어 시스템을 구성한다.

##  

## 03.02. Attitude Estimation EKF IMU GNSS Fusion [w/Code]

![](images/image2.png){width="7.268055555555556in" height="7.268055555555556in"}

Attitude estimation provides the flight control system with a continuous representation of the aircraft's orientation and rotational motion. For a cargo UAV, this estimate must remain stable during hover, aggressive maneuvering, payload-induced disturbances, and changing environmental conditions. The estimator combines high-rate inertial measurements with lower-rate navigation observations so that the controller receives an accurate state without relying on any single sensor source.

The inertial measurement unit, or IMU, forms the high-frequency foundation of the attitude estimator. Three-axis gyroscopes measure angular velocity, while accelerometers measure specific force along the aircraft body axes. Gyroscope measurements can be integrated to propagate orientation rapidly between external observations, making them well suited to the fast dynamics required by attitude control. However, even small gyro biases accumulate over time and eventually produce significant orientation drift.

Accelerometers provide an additional reference because the gravity vector can be observed when non-gravitational acceleration is sufficiently limited. The estimator can compare the measured acceleration direction with the expected gravity direction to correct roll and pitch drift. During aggressive maneuvering, strong wind response, or cargo motion, however, accelerometer measurements contain substantial translational acceleration. The estimator must therefore distinguish useful gravity information from vehicle dynamics rather than treating every acceleration sample as a direct attitude reference.

GNSS measurements complement the IMU by providing globally referenced position and velocity observations. GNSS does not normally measure roll and pitch directly, but velocity information can constrain the navigation solution and indirectly improve attitude estimation through the coupled vehicle-state model. Systems equipped with multiple GNSS antennas may additionally derive heading from carrier-phase measurements, providing an absolute yaw reference that does not depend solely on magnetic sensing.

An Extended Kalman Filter, or EKF, provides a practical framework for combining these heterogeneous measurements. The EKF maintains a state vector describing quantities such as orientation, velocity, position, gyroscope bias, and accelerometer bias. Because UAV motion and attitude kinematics are nonlinear, the filter propagates the nonlinear state model while locally linearizing the system for covariance prediction and measurement updates. This allows uncertainty to be explicitly represented throughout the estimation process.

The prediction stage normally executes at the IMU sampling rate. Gyroscope measurements propagate aircraft orientation, while accelerometer measurements are transformed from the body coordinate frame into the navigation frame and used to propagate velocity and position. Bias estimates are incorporated into this process so that corrected inertial measurements drive the state equations. The covariance matrix is propagated simultaneously to represent how uncertainty increases between external measurement updates.

When a GNSS observation becomes available, the EKF performs a measurement update rather than replacing the inertial state directly. Predicted position or velocity is compared with the corresponding GNSS observation to calculate an innovation, or measurement residual. The Kalman gain determines how strongly this innovation should modify the current state according to the relative uncertainty assigned to the prediction and measurement. This probabilistic weighting is central to robust multi-sensor fusion.

Orientation can be represented internally using quaternions because they avoid the singularities associated with Euler-angle representations near certain orientations. Roll, pitch, and yaw remain useful for monitoring, command interfaces, and flight-control interpretation, but quaternion-based propagation provides a more robust mathematical representation for three-dimensional rotation. Careful quaternion normalization and coordinate-frame conventions are essential because small implementation inconsistencies can create large control errors.

Sensor bias estimation is particularly important for long-duration cargo missions. Gyroscope bias directly produces attitude drift, while accelerometer bias creates velocity and position errors after integration. Including these biases as EKF states allows the filter to estimate them gradually from measurement innovations. Bias dynamics are commonly modeled as slowly varying stochastic processes so that the estimator can track thermal changes, aging effects, and other gradual sensor variations during operation.

Timing accuracy is as important as measurement accuracy. IMU and GNSS observations arrive at different frequencies and may experience different transport, processing, and communication delays. Each observation should therefore be associated with a reliable measurement timestamp. If delayed GNSS data are fused as though they represented the current aircraft state, the EKF can introduce artificial innovations and degraded attitude estimates, particularly during high-speed or high-angular-rate maneuvers.

Coordinate-frame management must also remain consistent throughout the estimator. IMU measurements originate in the sensor frame and may require transformation into the aircraft body frame before being used. Navigation states are represented in a defined local or geographic reference frame, while GNSS measurements originate from an Earth-referenced coordinate system. Explicit transformations, axis conventions, rotation directions, and unit definitions prevent frame mismatches from propagating into the flight controller.

Measurement quality should be evaluated before every EKF correction. GNSS accuracy may degrade because of satellite geometry, multipath, interference, obstruction, or atmospheric effects, while IMU measurements may become saturated or temporarily corrupted. Innovation tests can compare measurement residuals against their predicted uncertainty and reject observations that are statistically inconsistent. This prevents a single abnormal measurement from immediately destabilizing the navigation and attitude solution.

GNSS loss must not cause immediate loss of attitude stabilization because the IMU continues to provide high-rate rotational information. During a GNSS outage, the EKF can continue inertial propagation while recognizing that position and velocity uncertainty will increase over time. Attitude may remain sufficiently accurate for stabilization for a longer interval, depending on available aiding sources and sensor quality, but navigation drift eventually requires additional observations or a transition to an appropriate degraded operating mode.

For heavy cargo UAVs, vibration management becomes especially significant because large propulsion systems can introduce structural vibration into inertial measurements. Rotor harmonics, motor imbalance, flexible structures, and payload coupling may contaminate accelerometer and gyroscope signals. Mechanical isolation, sensor placement, digital filtering, and estimator tuning must therefore be coordinated. Excessive filtering should also be avoided because additional phase delay can reduce the usefulness of the state estimate for fast feedback control.

Payload motion can create another estimation challenge. A suspended or partially flexible load can generate oscillatory accelerations and rotational disturbances that do not correspond directly to rigid-body aircraft attitude. The estimator should remain focused on the vehicle state required by the flight controller while avoiding excessive correction from transient load-induced accelerations. Appropriate process noise, measurement covariance, innovation gating, and vehicle modeling help maintain stable estimates under these conditions.

Estimator outputs should include more than nominal attitude values. The flight-control software benefits from angular rates, navigation states, bias estimates, covariance or confidence information, sensor-validity status, innovation statistics, and estimator-health indicators. These outputs allow control and safety functions to distinguish a physically unusual aircraft state from an unreliable state estimate and to select appropriate fallback behavior when estimation quality deteriorates.

The estimator should also support initialization and reinitialization as explicit operating phases. Before flight, the system must establish valid orientation, sensor biases, navigation references, and covariance values. Initialization while stationary is comparatively straightforward, whereas recovery after an in-flight estimator reset requires careful handling of current motion and available measurements. Flight-control authority should only transition to functions that depend on the estimator after the required state-validity criteria have been satisfied.

Verification requires evaluating both nominal accuracy and failure behavior. Recorded sensor data, software-in-the-loop simulation, hardware-in-the-loop testing, injected bias, GNSS dropout, delayed measurements, vibration, and abnormal innovations can be used to assess estimator robustness. The surrounding flight-control structure places EKF-based IMU/GNSS attitude estimation immediately before attitude, altitude, and position controller development, emphasizing its role as the state foundation for the complete control hierarchy. Volume_23_Cargo_UAV_Autonomy_an...

A well-designed EKF-based fusion architecture therefore creates a bridge between noisy physical sensors and deterministic flight-control functions. High-rate IMU propagation provides responsiveness, GNSS observations constrain accumulated inertial drift, and probabilistic fusion determines how each source influences the state according to its uncertainty. The resulting attitude and navigation estimate becomes the common state reference from which stable cargo-UAV flight control, autonomous navigation, safety monitoring, and fault management can operate.

자세 추정(Attitude Estimation)은 비행 제어 시스템(Flight Control System)에 항공기의 방향과 회전 운동을 연속적으로 표현하는 상태 정보를 제공한다. 화물 무인항공기(Cargo UAV)의 경우 이러한 추정값은 호버링(Hover), 급격한 기동, 화물에 의한 외란(Payload-Induced Disturbance), 변화하는 환경 조건에서도 안정적으로 유지되어야 한다. 추정기는 고주파 관성 측정값과 상대적으로 저주파인 항법 관측값을 결합하여 특정 센서에만 의존하지 않고 정확한 상태 정보를 제어기에 제공한다.

관성 측정 장치(Inertial Measurement Unit, IMU)는 자세 추정기의 고주파 처리 기반을 형성한다. 3축 자이로스코프(Three-Axis Gyroscope)는 각속도를 측정하고 가속도계(Accelerometer)는 항공기 기체 축을 따라 비력(Specific Force)을 측정한다. 자이로스코프 측정값은 외부 관측 사이에서 자세를 빠르게 전파하기 위해 적분할 수 있으므로 자세 제어에 필요한 빠른 동역학에 적합하다. 그러나 작은 자이로 바이어스(Gyro Bias)도 시간이 지나면서 누적되어 결국 상당한 자세 드리프트(Orientation Drift)를 발생시킨다.

가속도계(Accelerometer)는 비중력 가속도가 충분히 작은 조건에서 중력 벡터(Gravity Vector)를 관측할 수 있기 때문에 추가적인 기준 정보를 제공한다. 추정기는 측정된 가속도 방향과 예상되는 중력 방향을 비교하여 롤(Roll)과 피치(Pitch)의 드리프트를 보정할 수 있다. 그러나 급격한 기동, 강한 바람에 대한 대응 또는 화물 운동이 발생하는 동안에는 가속도계 측정값에 상당한 병진 가속도(Translational Acceleration)가 포함된다. 따라서 추정기는 모든 가속도 샘플을 직접적인 자세 기준으로 처리하는 대신 유효한 중력 정보와 기체 동역학을 구분해야 한다.

위성항법시스템(Global Navigation Satellite System, GNSS) 측정값은 전역 기준 위치와 속도 관측값을 제공하여 관성 측정 장치를 보완한다. GNSS는 일반적으로 롤과 피치를 직접 측정하지 않지만 속도 정보는 항법 해(Navigation Solution)를 제한하고 결합된 기체 상태 모델을 통해 간접적으로 자세 추정 성능을 향상시킬 수 있다. 다중 GNSS 안테나(Multiple GNSS Antenna)를 사용하는 시스템에서는 반송파 위상 측정(Carrier-Phase Measurement)을 이용하여 헤딩(Heading)을 계산할 수 있으며, 자기 센서에만 의존하지 않는 절대 요(Yaw) 기준을 제공할 수 있다.

확장 칼만 필터(Extended Kalman Filter, EKF)는 이러한 서로 다른 특성의 측정값을 결합하기 위한 실용적인 프레임워크를 제공한다. EKF는 자세, 속도, 위치, 자이로스코프 바이어스, 가속도계 바이어스 등의 상태를 나타내는 상태 벡터(State Vector)를 유지한다. 무인항공기의 운동과 자세 운동학은 비선형(Nonlinear)이므로 필터는 비선형 상태 모델을 이용하여 상태를 전파하면서 공분산 예측(Covariance Prediction)과 측정 업데이트를 위해 시스템을 국부적으로 선형화한다. 이를 통해 전체 추정 과정에서 불확실성을 명시적으로 표현할 수 있다.

예측 단계(Prediction Stage)는 일반적으로 IMU 샘플링 주기로 실행된다. 자이로스코프 측정값을 사용하여 항공기의 자세를 전파하고, 가속도계 측정값은 기체 좌표계(Body Coordinate Frame)에서 항법 좌표계(Navigation Frame)로 변환된 후 속도와 위치를 전파하는 데 사용된다. 바이어스 추정값도 이 과정에 반영되어 보정된 관성 측정값이 상태 방정식을 구동한다. 동시에 공분산 행렬(Covariance Matrix)을 전파하여 외부 측정 업데이트 사이에서 불확실성이 어떻게 증가하는지를 표현한다.

GNSS 관측값이 사용 가능해지면 EKF는 관성 상태를 직접 대체하는 대신 측정 업데이트(Measurement Update)를 수행한다. 예측된 위치 또는 속도를 해당 GNSS 관측값과 비교하여 이노베이션(Innovation) 또는 측정 잔차(Measurement Residual)를 계산한다. 칼만 이득(Kalman Gain)은 예측값과 측정값에 할당된 상대적인 불확실성에 따라 이 이노베이션이 현재 상태를 어느 정도 수정해야 하는지를 결정한다. 이러한 확률적 가중(Probabilistic Weighting)은 견고한 다중 센서 융합(Multi-Sensor Fusion)의 핵심 요소이다.

자세는 특정 방향에서 발생하는 오일러 각(Euler Angle)의 특이점(Singularity)을 피할 수 있기 때문에 내부적으로 쿼터니언(Quaternion)을 이용하여 표현할 수 있다. 롤, 피치, 요는 모니터링, 명령 인터페이스 및 비행 제어 해석에 여전히 유용하지만, 쿼터니언 기반 전파(Quaternion-Based Propagation)는 3차원 회전을 보다 견고하게 표현할 수 있다. 작은 구현상의 불일치도 큰 제어 오차로 이어질 수 있으므로 정확한 쿼터니언 정규화(Quaternion Normalization)와 좌표계 규약(Coordinate-Frame Convention)이 필수적이다.

센서 바이어스 추정(Sensor Bias Estimation)은 장시간 화물 운송 임무에서 특히 중요하다. 자이로스코프 바이어스는 직접적으로 자세 드리프트를 발생시키며, 가속도계 바이어스는 적분 과정을 거쳐 속도와 위치 오차를 생성한다. 이러한 바이어스를 EKF 상태에 포함하면 필터가 측정 이노베이션을 이용하여 점진적으로 바이어스를 추정할 수 있다. 바이어스 동역학(Bias Dynamics)은 일반적으로 천천히 변화하는 확률적 과정(Stochastic Process)으로 모델링하여 운용 중 발생하는 온도 변화, 노화 효과 및 기타 점진적인 센서 변화를 추적할 수 있도록 한다.

타이밍 정확도(Timing Accuracy)는 측정 정확도만큼 중요하다. IMU와 GNSS 관측값은 서로 다른 주기로 입력되며 전송, 처리 및 통신 과정에서 서로 다른 지연시간을 경험할 수 있다. 따라서 각각의 관측값에는 신뢰할 수 있는 측정 타임스탬프(Measurement Timestamp)가 연결되어야 한다. 지연된 GNSS 데이터를 현재 항공기 상태를 나타내는 것처럼 융합하면 EKF에서 인위적인 이노베이션이 발생하고, 특히 고속 비행이나 높은 각속도 기동 중에 자세 추정 성능이 저하될 수 있다.

좌표계 관리(Coordinate-Frame Management) 역시 추정기 전체에서 일관성을 유지해야 한다. IMU 측정값은 센서 좌표계(Sensor Frame)에서 생성되며 사용하기 전에 항공기 기체 좌표계(Body Frame)로 변환해야 할 수 있다. 항법 상태는 정의된 지역 또는 지리적 기준 좌표계에 표현되는 반면 GNSS 측정값은 지구 기준 좌표계(Earth-Referenced Coordinate System)에서 생성된다. 명확한 좌표 변환, 축 규약(Axis Convention), 회전 방향 및 단위 정의를 통해 좌표계 불일치가 비행 제어기로 전파되는 것을 방지해야 한다.

각 EKF 보정 이전에는 측정 품질(Measurement Quality)을 평가해야 한다. GNSS 정확도는 위성 배치, 다중경로(Multipath), 전파 간섭, 신호 차단 또는 대기 영향으로 저하될 수 있으며, IMU 측정값도 포화(Saturation)되거나 일시적으로 손상될 수 있다. 이노베이션 검사(Innovation Test)는 측정 잔차를 예측된 불확실성과 비교하여 통계적으로 일관되지 않은 관측값을 제거할 수 있다. 이를 통해 하나의 비정상적인 측정값이 항법 및 자세 추정 결과를 즉시 불안정하게 만드는 것을 방지할 수 있다.

GNSS 신호 손실(GNSS Loss)이 발생하더라도 IMU는 계속해서 고주파 회전 정보를 제공하므로 자세 안정화 기능이 즉시 상실되어서는 안 된다. GNSS 사용이 불가능한 동안에도 EKF는 관성 전파(Inertial Propagation)를 계속 수행할 수 있지만 위치와 속도의 불확실성이 시간에 따라 증가한다는 사실을 반영해야 한다. 사용 가능한 보조 센서와 센서 품질에 따라 자세는 더 오랫동안 안정화에 충분한 정확도를 유지할 수 있지만, 항법 드리프트가 증가하면 결국 추가적인 관측 정보 또는 적절한 성능 저하 운용 모드(Degraded Operating Mode)로의 전환이 필요하다.

대형 화물 무인항공기(Heavy Cargo UAV)에서는 대형 추진 시스템이 관성 측정값에 구조 진동을 유입할 수 있기 때문에 진동 관리(Vibration Management)가 특히 중요하다. 로터 고조파(Rotor Harmonics), 모터 불균형, 유연한 구조물 및 화물 결합 현상은 가속도계와 자이로스코프 신호를 오염시킬 수 있다. 따라서 기계적 절연(Mechanical Isolation), 센서 배치, 디지털 필터링(Digital Filtering), 추정기 튜닝(Estimator Tuning)을 상호 연계하여 설계해야 한다. 과도한 필터링은 추가적인 위상 지연(Phase Delay)을 발생시켜 고속 피드백 제어에 필요한 상태 추정값의 유효성을 감소시킬 수 있으므로 피해야 한다.

화물 운동(Payload Motion)은 또 다른 상태 추정 문제를 발생시킬 수 있다. 현수형 화물(Suspended Load)이나 부분적으로 유연한 화물은 강체 항공기의 자세와 직접적으로 대응하지 않는 진동성 가속도 및 회전 외란을 생성할 수 있다. 추정기는 일시적인 화물 유발 가속도에 의해 과도하게 보정되는 것을 방지하면서 비행 제어기가 요구하는 기체 상태 추정에 집중해야 한다. 적절한 프로세스 잡음(Process Noise), 측정 공분산(Measurement Covariance), 이노베이션 게이팅(Innovation Gating), 기체 모델링을 통해 이러한 조건에서도 안정적인 상태 추정을 유지할 수 있다.

추정기의 출력은 단순한 자세 값 이상을 포함해야 한다. 비행 제어 소프트웨어는 각속도, 항법 상태, 바이어스 추정값, 공분산 또는 신뢰도 정보, 센서 유효성 상태, 이노베이션 통계 및 추정기 상태 지표(Estimator-Health Indicator)를 활용할 수 있다. 이러한 출력 정보를 이용하면 제어 및 안전 기능이 실제로 비정상적인 항공기 상태와 신뢰할 수 없는 상태 추정 결과를 구분하고, 추정 품질이 저하되는 경우 적절한 폴백 동작(Fallback Behavior)을 선택할 수 있다.

추정기는 초기화(Initialization)와 재초기화(Reinitialization)를 명시적인 운용 단계로 지원해야 한다. 비행 전에 시스템은 유효한 자세, 센서 바이어스, 항법 기준 및 공분산 값을 설정해야 한다. 정지 상태에서의 초기화는 비교적 간단하지만 비행 중 추정기 리셋 이후의 복구 과정에서는 현재의 기체 운동과 사용 가능한 측정값을 신중하게 처리해야 한다. 추정기에 의존하는 비행 제어 기능은 필요한 상태 유효성 기준(State-Validity Criteria)이 충족된 이후에만 제어 권한을 부여받아야 한다.

검증(Verification)에서는 정상 조건의 정확도뿐만 아니라 고장 조건에서의 동작도 평가해야 한다. 기록된 센서 데이터, 소프트웨어 인 더 루프(Software-in-the-Loop, SIL) 시뮬레이션, 하드웨어 인 더 루프(Hardware-in-the-Loop, HIL) 시험, 인위적으로 주입된 바이어스, GNSS 신호 단절, 지연된 측정값, 진동 및 비정상 이노베이션 등을 이용하여 추정기의 견고성을 평가할 수 있다. 전체 비행 제어 구조에서 EKF 기반 IMU/GNSS 자세 추정은 자세, 고도 및 위치 제어기 개발에 앞서 배치되며, 전체 제어 계층을 위한 상태 정보 기반으로서의 역할을 수행한다.

잘 설계된 EKF 기반 융합 구조(EKF-Based Fusion Architecture)는 잡음이 포함된 물리 센서와 결정론적인 비행 제어 기능 사이를 연결하는 역할을 한다. 고주파 IMU 전파는 빠른 응답성을 제공하고, GNSS 관측값은 누적되는 관성 드리프트를 제한하며, 확률적 융합(Probabilistic Fusion)은 각 센서의 불확실성에 따라 해당 센서가 상태 추정에 미치는 영향을 결정한다. 이렇게 생성된 자세 및 항법 추정값은 안정적인 화물 무인항공기 비행 제어, 자율 항법(Autonomous Navigation), 안전 모니터링(Safety Monitoring), 고장 관리(Fault Management)가 공통으로 사용하는 상태 기준(Common State Reference)이 된다.

##  

## 03.03. Attitude Controller Design PID LQR [w/Code]

![](images/image3.png){width="7.268055555555556in" height="7.268055555555556in"}

Attitude control is the core stabilization function that converts commanded aircraft orientation into the body moments required to control roll, pitch, and yaw. In a cargo UAV, the controller must provide rapid and predictable response while accommodating large variations in mass, inertia, propulsion loading, and environmental disturbance. The flight-control structure therefore places attitude-controller design directly after state estimation and before altitude, position, and motor-allocation functions. Volume_23_Cargo_UAV_Autonomy_an...

A practical attitude-control architecture is usually implemented as a cascaded system. The outer attitude loop compares commanded orientation with the estimated aircraft attitude and generates desired angular rates. The inner rate loop compares these references with measured or estimated body rates and calculates roll, pitch, and yaw moment commands. Separating orientation regulation from angular-rate stabilization allows the inner loop to reject disturbances much faster than the outer loop changes aircraft orientation.

The attitude error must be represented carefully because aircraft orientation is inherently three-dimensional. Euler-angle errors can provide intuitive roll, pitch, and yaw quantities for moderate operating envelopes, while quaternion-based or rotation-based errors provide more consistent behavior for larger rotations. Regardless of representation, the controller must use coordinate conventions that exactly match the estimator, vehicle model, and control-allocation subsystem to prevent incorrect moment commands.

PID control provides a widely applicable baseline for attitude and rate regulation. The proportional term generates corrective action according to the current error, the integral term compensates for persistent bias or steady disturbance, and the derivative term introduces damping based on error variation. In UAV implementations, derivative behavior is often obtained from measured angular rate rather than directly differentiating noisy attitude errors, improving robustness against sensor noise.

Proportional gain strongly influences responsiveness. A low value produces slow attitude convergence, whereas excessive proportional gain can create oscillation or excite structural and propulsion dynamics. Integral gain removes persistent errors caused by aerodynamic asymmetry, center-of-gravity offset, propulsion mismatch, or sustained wind disturbance. However, excessive integral action can generate slow oscillations and large accumulated commands when actuators become saturated.

Derivative or rate feedback provides damping and is particularly important for fast rotational dynamics. Because gyroscope measurements contain vibration and high-frequency noise, rate signals may require filtering before they are used by the controller. Filter bandwidth must be selected carefully: insufficient filtering allows noise to reach actuator commands, while excessive filtering introduces phase delay and can reduce stability margins in the high-bandwidth inner control loop.

Anti-windup logic is essential whenever integral control operates near physical actuator limits. During aggressive maneuvers, heavy payload operation, strong winds, or propulsion degradation, requested moments may exceed available authority. If the integral state continues accumulating while the actuator is saturated, substantial overshoot can occur when control authority returns. Integral clamping, conditional integration, or back-calculation can constrain this behavior.

PID controllers can be tuned independently for roll, pitch, and yaw when axis coupling is limited, but heavy cargo UAVs may exhibit significant cross-axis interaction. Large structures, asymmetric payload placement, changing inertia, and propulsion configuration can cause a moment around one axis to influence motion around another. Gain scheduling or model-based compensation can therefore supplement conventional PID control when fixed independent gains no longer provide adequate performance across the complete flight envelope.

Linear Quadratic Regulator, or LQR, control provides a model-based alternative for coupled attitude dynamics. The aircraft dynamics are linearized around an operating condition and represented through state-space equations. LQR determines a feedback gain matrix by minimizing a quadratic cost function that penalizes state deviation and control effort. The resulting controller can coordinate multiple states and control channels simultaneously rather than treating each rotational axis as completely independent.

LQR behavior is shaped primarily through state-weighting and control-weighting matrices. Increasing the penalty on attitude or angular-rate error generally produces stronger corrective action, while increasing the penalty on control effort reduces actuator demand. These matrices therefore express the desired compromise among tracking accuracy, damping, actuator usage, and robustness. Their selection still requires engineering judgment even though the feedback gain itself is calculated mathematically.

The usefulness of LQR depends on the accuracy of the underlying model and the validity of the selected operating point. A controller derived for one payload mass, center-of-gravity position, airspeed, or propulsion condition may become less effective when the vehicle configuration changes substantially. Multiple linear models, gain scheduling, parameter adaptation, or robust design techniques can extend model-based control across a wider cargo-UAV operating envelope.

PID and LQR should not necessarily be viewed as mutually exclusive approaches. A flight-control architecture may use PID-based inner-rate control because of its simplicity, transparency, and mature tuning methodology while applying model-based techniques to selected coupled dynamics. Conversely, LQR can provide the principal state-feedback law while integral augmentation compensates for steady disturbances. Controller selection should follow vehicle dynamics, certification objectives, computational constraints, and verification requirements.

Feedforward control can improve either approach by commanding part of the required response before significant feedback error develops. Desired angular acceleration, estimated inertia, thrust state, or known aerodynamic effects can be used to calculate nominal moment demand. Feedback then corrects modeling errors and external disturbances. This division reduces the amount of corrective action demanded from the feedback controller and can improve tracking during rapid commanded maneuvers.

Control commands must pass through explicit limit management before reaching the allocation layer. Maximum angular rates, attitude angles, moments, and command slew rates should reflect structural limits, propulsion capability, payload constraints, and the approved flight envelope. When saturation occurs, the controller should preserve the most safety-critical stabilization objectives rather than allowing incompatible commands from multiple axes to compete without priority.

Cargo mass and center-of-gravity variation directly affect attitude-control authority because they modify rotational inertia and the relationship between actuator force and body acceleration. A heavily loaded vehicle may respond more slowly to the same commanded moment, while an asymmetric load can introduce trim requirements and axis coupling. The broader flight-control structure therefore extends basic attitude control with dedicated cargo-weight adaptive control and disturbance-rejection functions. Volume_23_Cargo_UAV_Autonomy_an...

Wind gusts and propulsion disturbances provide important tests of controller robustness. The inner rate loop should rapidly suppress unexpected rotational motion, while the attitude loop restores the commanded orientation after the transient disturbance. Performance can be evaluated using rise time, settling time, overshoot, steady-state error, disturbance-rejection time, control effort, and stability margins rather than relying on a single tracking-error metric.

Controller execution must also satisfy deterministic real-time requirements. The attitude and rate loops operate at higher frequencies than altitude and position control, so sampling period, computation time, sensor latency, and actuator-command delay directly affect achievable bandwidth. Timing jitter or delayed state estimates effectively introduce additional phase lag, meaning a controller that is stable in an ideal simulation may perform poorly when implemented on actual flight-control hardware.

Fault conditions require deliberate degradation behavior. If an actuator loses effectiveness, a motor reaches saturation, state-estimation quality deteriorates, or available control authority becomes insufficient, the controller should expose this condition to higher-level safety logic. Remaining authority may be redistributed through control allocation, attitude limits may be reduced, or the aircraft may transition into a contingency mode rather than continuing to demand an unattainable nominal response.

Verification should begin with mathematical analysis and simulation before progressing toward software-in-the-loop and hardware-in-the-loop testing. Parameter sweeps can examine payload mass, inertia, center-of-gravity shift, sensor noise, actuator delay, wind disturbance, and propulsion degradation. Step commands and disturbance injections reveal transient characteristics, while frequency-domain analysis can evaluate gain and phase margins for critical control loops.

Flight testing should expand the operating envelope progressively rather than immediately exercising maximum commands. Initial hover and low-rate maneuvers can confirm axis signs, gain behavior, actuator authority, and estimator-controller consistency. Subsequent tests can introduce larger attitude commands, payload variations, wind disturbances, and aggressive transitions while monitoring stability margins, saturation events, tracking error, structural response, and controller timing.

The final attitude-control design is therefore more than a choice between PID and LQR. It is an integrated stabilization function connecting state estimation, rotational dynamics, command shaping, actuator limits, payload variation, disturbance rejection, real-time execution, and control allocation. When these elements are engineered together, the attitude controller provides the stable inner foundation required by altitude control, position control, autonomous navigation, and safety-critical cargo-UAV operation.

자세 제어(Attitude Control)는 명령된 항공기 자세를 롤(Roll), 피치(Pitch), 요(Yaw)를 제어하는 데 필요한 기체 모멘트(Body Moment)로 변환하는 핵심 안정화 기능이다. 화물 무인항공기(Cargo UAV)의 제어기는 질량, 관성(Inertia), 추진 부하(Propulsion Loading), 환경 외란(Environmental Disturbance)의 큰 변화에 대응하면서 빠르고 예측 가능한 응답을 제공해야 한다. 따라서 비행 제어 구조에서는 자세 제어기 설계를 상태 추정(State Estimation) 이후, 그리고 고도, 위치 및 모터 할당(Motor Allocation) 기능 이전에 배치한다.

실용적인 자세 제어 구조(Attitude-Control Architecture)는 일반적으로 직렬 제어 시스템(Cascaded System)으로 구현된다. 외부 자세 루프(Outer Attitude Loop)는 명령된 자세와 추정된 항공기 자세를 비교하여 목표 각속도(Desired Angular Rate)를 생성한다. 내부 각속도 루프(Inner Rate Loop)는 이 기준값을 측정 또는 추정된 기체 각속도와 비교하여 롤, 피치, 요 모멘트 명령을 계산한다. 자세 조절과 각속도 안정화를 분리하면 외부 루프가 항공기 자세를 변경하는 속도보다 훨씬 빠르게 내부 루프가 외란을 억제할 수 있다.

항공기 자세는 본질적으로 3차원이므로 자세 오차(Attitude Error)를 신중하게 표현해야 한다. 오일러 각 오차(Euler-Angle Error)는 일반적인 운용 영역에서 직관적인 롤, 피치, 요 값을 제공하며, 쿼터니언(Quaternion) 또는 회전 기반 오차(Rotation-Based Error)는 더 큰 회전 범위에서 일관된 동작을 제공할 수 있다. 어떠한 표현 방식을 사용하더라도 잘못된 모멘트 명령을 방지하려면 제어기가 상태 추정기, 기체 모델 및 제어 할당(Control Allocation) 서브시스템과 정확하게 동일한 좌표계 규약(Coordinate Convention)을 사용해야 한다.

비례-적분-미분 제어(Proportional-Integral-Derivative Control, PID)는 자세 및 각속도 제어에 폭넓게 적용할 수 있는 기본적인 제어 방법을 제공한다. 비례 항(Proportional Term)은 현재 오차에 따라 보정 동작을 생성하고, 적분 항(Integral Term)은 지속적인 바이어스 또는 정상 상태 외란을 보상하며, 미분 항(Derivative Term)은 오차 변화에 따라 감쇠(Damping)를 제공한다. 무인항공기에서는 잡음이 포함된 자세 오차를 직접 미분하기보다 측정된 각속도를 이용하여 미분 동작을 구현하는 경우가 많으며, 이를 통해 센서 잡음에 대한 견고성을 향상시킬 수 있다.

비례 이득(Proportional Gain)은 응답성에 큰 영향을 준다. 값이 너무 낮으면 자세 수렴이 느려지고, 지나치게 높은 비례 이득은 진동을 발생시키거나 구조 및 추진 시스템의 동역학을 자극할 수 있다. 적분 이득(Integral Gain)은 공력 비대칭(Aerodynamic Asymmetry), 무게중심(Center of Gravity) 편차, 추진력 불균형 또는 지속적인 바람 외란으로 발생하는 정상 상태 오차를 제거한다. 그러나 과도한 적분 동작은 느린 진동을 발생시키고 액추에이터가 포화될 때 큰 누적 명령을 만들 수 있다.

미분 또는 각속도 피드백(Derivative or Rate Feedback)은 감쇠를 제공하며 특히 빠른 회전 동역학에서 중요하다. 자이로스코프 측정값에는 진동과 고주파 잡음이 포함되므로 제어기에 사용하기 전에 각속도 신호에 필터링이 필요할 수 있다. 필터 대역폭(Filter Bandwidth)은 신중하게 결정해야 한다. 필터링이 부족하면 잡음이 액추에이터 명령까지 전달되며, 과도한 필터링은 위상 지연(Phase Delay)을 발생시켜 고대역폭 내부 제어 루프의 안정 여유(Stability Margin)를 감소시킬 수 있다.

적분 제어가 물리적인 액추에이터 한계 부근에서 동작하는 경우 안티와인드업 로직(Anti-Windup Logic)이 필수적이다. 급격한 기동, 대형 화물 운송, 강풍 또는 추진 시스템 성능 저하 상황에서는 요구되는 모멘트가 사용 가능한 제어 권한(Control Authority)을 초과할 수 있다. 액추에이터가 포화된 상태에서도 적분 상태가 계속 누적되면 제어 권한이 회복되었을 때 큰 오버슈트(Overshoot)가 발생할 수 있다. 적분 제한(Integral Clamping), 조건부 적분(Conditional Integration), 역계산(Back-Calculation) 등을 이용하여 이러한 현상을 제한할 수 있다.

축 간 결합(Axis Coupling)이 제한적인 경우 PID 제어기는 롤, 피치, 요에 대해 독립적으로 튜닝할 수 있지만 대형 화물 무인항공기에서는 상당한 축 간 상호작용(Cross-Axis Interaction)이 나타날 수 있다. 대형 구조물, 비대칭 화물 배치, 변화하는 관성 및 추진 구성으로 인해 한 축에 대한 모멘트가 다른 축의 운동에 영향을 줄 수 있다. 따라서 고정된 독립 이득만으로 전체 비행 영역에서 충분한 성능을 확보하기 어려운 경우 이득 스케줄링(Gain Scheduling) 또는 모델 기반 보상(Model-Based Compensation)을 기존 PID 제어에 추가할 수 있다.

선형 이차 조절기(Linear Quadratic Regulator, LQR)는 결합된 자세 동역학을 제어하기 위한 모델 기반 대안을 제공한다. 항공기 동역학은 특정 운용 조건 주변에서 선형화되고 상태 공간 방정식(State-Space Equation)으로 표현된다. LQR은 상태 편차(State Deviation)와 제어 입력(Control Effort)에 페널티를 부여하는 이차 비용 함수(Quadratic Cost Function)를 최소화하여 피드백 이득 행렬(Feedback Gain Matrix)을 결정한다. 이를 통해 각각의 회전축을 완전히 독립적으로 처리하는 대신 여러 상태와 제어 채널을 동시에 조정할 수 있다.

LQR의 동작 특성은 주로 상태 가중 행렬(State-Weighting Matrix)과 제어 가중 행렬(Control-Weighting Matrix)을 통해 결정된다. 자세 또는 각속도 오차에 대한 페널티를 증가시키면 일반적으로 더 강한 보정 동작이 발생하고, 제어 입력에 대한 페널티를 증가시키면 액추에이터 요구량이 감소한다. 따라서 이러한 행렬은 추종 정확도, 감쇠, 액추에이터 사용량 및 견고성 사이에서 요구되는 절충 관계를 표현한다. 피드백 이득 자체는 수학적으로 계산되지만 가중 행렬의 선택에는 여전히 공학적인 판단이 필요하다.

LQR의 유효성은 기반이 되는 모델의 정확성과 선택된 운용점(Operating Point)의 유효성에 영향을 받는다. 특정 화물 질량, 무게중심 위치, 대기속도(Airspeed) 또는 추진 조건을 기준으로 설계된 제어기는 기체 구성이 크게 변경되면 성능이 저하될 수 있다. 다중 선형 모델(Multiple Linear Model), 이득 스케줄링, 매개변수 적응(Parameter Adaptation) 또는 견고 제어 설계(Robust Design) 기법을 이용하면 모델 기반 제어를 더 넓은 화물 무인항공기 운용 영역으로 확장할 수 있다.

PID와 LQR은 반드시 서로 배타적인 제어 방법으로 볼 필요가 없다. 비행 제어 구조에서는 단순성, 투명성 및 성숙한 튜닝 방법론을 활용하기 위해 PID 기반 내부 각속도 제어를 사용하면서 특정 결합 동역학에 모델 기반 기법을 적용할 수 있다. 반대로 LQR을 주요 상태 피드백 제어 법칙(State-Feedback Control Law)으로 사용하면서 정상 상태 외란을 보상하기 위해 적분 제어를 추가할 수도 있다. 제어기 선택은 기체 동역학, 인증 목표, 계산 제약 및 검증 요구사항을 기반으로 이루어져야 한다.

피드포워드 제어(Feedforward Control)는 상당한 피드백 오차가 발생하기 전에 필요한 응답의 일부를 명령함으로써 PID 또는 LQR 방식 모두의 성능을 향상시킬 수 있다. 목표 각가속도(Desired Angular Acceleration), 추정 관성, 추력 상태 또는 알려진 공력 효과를 이용하여 기본 모멘트 요구량을 계산할 수 있다. 이후 피드백 제어는 모델링 오차와 외부 외란을 보정한다. 이러한 역할 분리는 피드백 제어기에 요구되는 보정량을 감소시키고 빠른 명령 기동에서 추종 성능을 향상시킬 수 있다.

제어 명령은 제어 할당 계층(Control Allocation Layer)에 도달하기 전에 명시적인 제한 관리(Limit Management)를 거쳐야 한다. 최대 각속도, 자세각, 모멘트 및 명령 변화율(Command Slew Rate)은 구조적 한계, 추진 능력, 화물 제약 및 승인된 비행 영역(Flight Envelope)을 반영해야 한다. 포화가 발생하면 여러 축에서 생성된 서로 양립할 수 없는 명령이 우선순위 없이 경쟁하도록 하는 대신 가장 안전에 중요한 안정화 목표를 우선적으로 유지해야 한다.

화물 질량과 무게중심 변화는 회전 관성과 액추에이터 힘과 기체 가속도 사이의 관계를 변화시키므로 자세 제어 권한에 직접적인 영향을 준다. 무거운 화물을 탑재한 기체는 동일한 모멘트 명령에 더 느리게 반응할 수 있으며, 비대칭 화물은 트림 요구량(Trim Requirement)과 축 간 결합을 발생시킬 수 있다. 따라서 전체 비행 제어 구조에서는 기본 자세 제어 기능을 화물 중량 적응 제어(Cargo-Weight Adaptive Control) 및 외란 제거(Disturbance Rejection) 기능으로 확장한다.

돌풍(Wind Gust)과 추진 시스템 외란은 제어기 견고성을 평가하는 중요한 조건을 제공한다. 내부 각속도 루프는 예상하지 못한 회전 운동을 신속하게 억제해야 하며, 자세 루프는 과도 외란이 사라진 이후 항공기를 명령된 자세로 복원해야 한다. 성능은 하나의 추종 오차 지표에만 의존하기보다 상승 시간(Rise Time), 정착 시간(Settling Time), 오버슈트, 정상 상태 오차, 외란 제거 시간, 제어 입력 및 안정 여유 등의 지표를 이용하여 평가할 수 있다.

제어기의 실행은 결정론적 실시간 요구사항(Deterministic Real-Time Requirement)도 충족해야 한다. 자세 및 각속도 루프는 고도와 위치 제어보다 높은 주파수에서 동작하므로 샘플링 주기(Sampling Period), 계산 시간, 센서 지연시간 및 액추에이터 명령 지연이 달성 가능한 대역폭에 직접적인 영향을 준다. 타이밍 지터(Timing Jitter) 또는 지연된 상태 추정값은 추가적인 위상 지연으로 작용하므로 이상적인 시뮬레이션에서는 안정적인 제어기가 실제 비행 제어 하드웨어에서는 성능이 저하될 수 있다.

고장 조건에서는 의도적으로 설계된 성능 저하 동작(Degradation Behavior)이 필요하다. 액추에이터의 효율이 감소하거나 모터가 포화되고, 상태 추정 품질이 저하되거나 사용 가능한 제어 권한이 부족해지는 경우 제어기는 이러한 상태를 상위 안전 로직(Safety Logic)에 전달해야 한다. 남아 있는 제어 권한은 제어 할당을 통해 재분배할 수 있으며, 자세 제한을 축소하거나 달성할 수 없는 정상 응답을 계속 요구하는 대신 항공기를 비상 운용 모드(Contingency Mode)로 전환할 수 있다.

검증(Verification)은 소프트웨어 인 더 루프(Software-in-the-Loop, SIL) 및 하드웨어 인 더 루프(Hardware-in-the-Loop, HIL) 시험으로 진행하기 전에 수학적 분석과 시뮬레이션에서 시작해야 한다. 매개변수 스윕(Parameter Sweep)을 이용하여 화물 질량, 관성, 무게중심 이동, 센서 잡음, 액추에이터 지연, 바람 외란 및 추진 시스템 성능 저하의 영향을 평가할 수 있다. 계단 명령(Step Command)과 외란 주입(Disturbance Injection)을 통해 과도 응답 특성을 확인하고, 주파수 영역 분석(Frequency-Domain Analysis)을 통해 핵심 제어 루프의 이득 여유(Gain Margin)와 위상 여유(Phase Margin)를 평가할 수 있다.

비행 시험(Flight Testing)은 처음부터 최대 명령을 적용하기보다 운용 영역을 점진적으로 확장하는 방식으로 수행해야 한다. 초기 호버링과 낮은 각속도의 기동을 통해 축 방향, 이득 특성, 액추에이터 제어 권한 및 상태 추정기와 제어기 사이의 일관성을 확인할 수 있다. 이후 안정 여유, 포화 발생, 추종 오차, 구조 응답 및 제어기 타이밍을 모니터링하면서 더 큰 자세 명령, 화물 변화, 바람 외란 및 급격한 전환 동작을 단계적으로 시험할 수 있다.

최종적인 자세 제어 설계는 단순히 PID와 LQR 가운데 하나를 선택하는 문제가 아니다. 이는 상태 추정, 회전 동역학, 명령 형상화(Command Shaping), 액추에이터 제한, 화물 변화, 외란 제거, 실시간 실행 및 제어 할당을 연결하는 통합 안정화 기능(Integrated Stabilization Function)이다. 이러한 요소를 하나의 시스템으로 통합하여 설계하면 자세 제어기는 고도 제어, 위치 제어, 자율 항법(Autonomous Navigation) 및 안전 중요 화물 무인항공기 운용(Safety-Critical Cargo-UAV Operation)에 필요한 안정적인 내부 제어 기반을 제공할 수 있다.

##  

## 03.04. Altitude and Vertical Speed Controller [w/Code]

![](images/image4.png){width="7.268055555555556in" height="7.268055555555556in"}

Altitude control regulates the vertical position of a cargo UAV while the vertical-speed controller governs the rate at which the aircraft climbs or descends. These functions form a cascaded control structure between higher-level trajectory commands and the propulsion system. Within the flight-control architecture, altitude and vertical-speed regulation follows attitude control and precedes position and velocity control, establishing a coordinated hierarchy for three-dimensional flight.

The outer altitude loop compares the commanded altitude with the estimated altitude and converts the resulting height error into a desired vertical-speed command. Rather than attempting to control thrust directly from altitude error, this intermediate reference allows vertical motion to be shaped gradually. Maximum climb and descent rates can be imposed at this stage so that large altitude errors do not produce commands outside the aircraft's safe operating envelope.

The inner vertical-speed loop compares the commanded climb or descent rate with the estimated vertical velocity. The resulting error is converted into a vertical acceleration or collective-thrust correction. Because this loop operates faster than the altitude loop, it can reject short-term vertical disturbances while the slower outer loop maintains long-term altitude accuracy. This bandwidth separation is fundamental to predictable cascaded-control behavior.

Vertical state estimation is therefore closely coupled with controller performance. Altitude information may originate from GNSS, barometric pressure sensors, radar or laser altimeters, and fused navigation estimates, while vertical velocity can be derived from inertial and navigation measurements. The controller should consume a consistent fused state rather than independently switching between raw sensors, because discontinuities in altitude or velocity estimates can immediately create unwanted thrust commands.

A basic altitude controller can use proportional or proportional-integral feedback to transform altitude error into vertical-speed demand. Proportional action determines how aggressively the aircraft closes the altitude difference, while integral action can eliminate persistent offsets. The command should normally pass through climb-rate, descent-rate, acceleration, and jerk constraints to produce smooth motion compatible with cargo stability, passenger-free structural limits, and propulsion capability.

The vertical-speed controller commonly applies proportional-integral-derivative or equivalent feedback around vertical velocity. Proportional action responds to instantaneous speed error, integral action compensates for persistent thrust mismatch, and acceleration-related damping can improve transient behavior. For cargo UAVs, the controller must avoid excessive vertical oscillation because repeated acceleration can increase structural loads, disturb suspended cargo, and reduce propulsion efficiency.

Hover-thrust estimation is especially important because gravity produces a continuous force that must be balanced before feedback corrections are considered. If the nominal thrust required to support the vehicle is known, it can be applied as a feedforward term and the feedback controller only needs to correct deviations. Without this compensation, the integral controller may be forced to generate most of the steady hover command, producing slower convergence and greater sensitivity to saturation.

Cargo mass directly changes the required hover thrust. A vehicle carrying a heavy payload requires substantially greater collective propulsion than the same aircraft operating unloaded. The flight-control software should therefore update the nominal thrust model when reliable payload or vehicle-mass information is available. This relationship connects altitude regulation with the chapter's dedicated load-change adaptive-control function for changing cargo weight. Volume_23_Cargo_UAV_Autonomy_an...

Center-of-gravity variation can also affect vertical control indirectly. Although altitude regulation primarily determines collective thrust, an offset center of gravity may require differential actuator forces to maintain attitude. Some propulsion capacity is consequently consumed by attitude stabilization rather than pure vertical force generation. The altitude controller and control-allocation subsystem must respect the remaining thrust authority instead of assuming that maximum theoretical propulsion is always available for climbing.

Vehicle attitude changes the relationship between total thrust and vertical force. During level hover, most collective thrust acts against gravity, whereas during significant roll or pitch angles part of the thrust vector produces horizontal acceleration. Vertical control may therefore require tilt compensation so that commanded collective thrust accounts for the reduced vertical component. Such compensation must remain bounded because aggressive tilt combined with high altitude demand can otherwise drive propulsion into saturation.

Thrust saturation represents one of the most important nonlinear constraints in vertical control. Maximum thrust limits climbing capability, while minimum usable thrust and vehicle dynamics constrain rapid descent. When commanded acceleration exceeds available propulsion authority, integral terms should not continue accumulating error indefinitely. Anti-windup logic, command limiting, and saturation feedback allow the controller to recover smoothly after normal control authority becomes available again.

Descent behavior requires particular attention because multirotor aerodynamics may become unfavorable at high downward velocity. The controller should therefore apply validated descent-rate and acceleration limits rather than treating upward and downward motion as perfectly symmetric. Heavy cargo further changes safe descent characteristics because increased mass alters momentum, required recovery thrust, and the vertical distance necessary to arrest a descent before landing or obstacle clearance is compromised.

Wind disturbances create vertical as well as horizontal control errors. Updrafts and downdrafts can change climb rate even when collective thrust remains constant, while turbulent airflow around large structures may generate rapidly varying vertical forces. The inner vertical-speed loop should suppress these disturbances without reacting excessively to measurement noise. Disturbance estimates or acceleration feedforward can further improve response when reliable information is available.

Command shaping becomes especially important for cargo missions. Sudden changes from hover to maximum climb can produce large acceleration and jerk, causing cargo movement and structural loading even when the aircraft remains controllable. Vertical commands can therefore be passed through rate and acceleration limiters or trajectory generators. Smooth references reduce mechanical stress and improve tracking because the controller is not repeatedly asked to reproduce physically unrealistic step changes.

Takeoff introduces a special vertical-control transition because the vehicle moves from ground support to aerodynamic support. Before liftoff, increasing thrust may not produce corresponding vertical acceleration because the landing gear still carries part of the weight. The controller should avoid interpreting this condition as an ordinary altitude-tracking failure. Controlled thrust ramping, liftoff detection, estimator validity, and transition logic are needed before normal airborne altitude regulation assumes full authority.

Landing presents the opposite transition. The altitude controller must progressively reduce height while maintaining an appropriate descent profile, but near the surface the control objective shifts toward touchdown management. Radar or laser altitude measurements may become more important than global altitude references. Touchdown detection should prevent the controller from increasing thrust merely because the commanded altitude can no longer be reached after the landing gear has established ground contact.

Altitude-reference management must also distinguish among absolute altitude, altitude above ground level, and altitude relative to a mission reference. GNSS-derived height, barometric altitude, terrain-relative altitude, and local navigation-frame height do not represent identical quantities. The flight-control interface must define which reference is being controlled and ensure that reference transitions do not introduce sudden altitude errors or unintended climb and descent commands.

The controller should monitor its own operating condition through variables such as altitude error, vertical-speed error, commanded acceleration, thrust demand, saturation state, integral state, and available thrust margin. These values support flight monitoring and fault detection. Persistent altitude error combined with maximum thrust, for example, can indicate excessive payload, propulsion degradation, strong downdraft, or an incorrect vehicle-mass estimate rather than poor controller tuning alone.

Failure handling should preserve basic vertical stability whenever possible. Degraded altitude sensors, GNSS loss, propulsion faults, or unreliable vertical-velocity estimates may require switching to alternative state sources or restricting autonomous functions. The controller should expose validity and authority information to the flight-mode and safety systems so that they can select hover, controlled descent, return-to-home, or emergency-landing behavior according to the remaining capability.

Verification should evaluate the complete vertical-control chain rather than only nominal altitude tracking. Simulation and SIL/HIL testing can introduce payload variation, thrust-model error, sensor noise, delayed measurements, wind disturbances, actuator saturation, and propulsion degradation. Step and ramp altitude commands reveal transient response, while long hover tests expose steady-state drift, integral behavior, thermal effects, and sensitivity to changes in vehicle mass.

The altitude and vertical-speed controller ultimately acts as the vertical-motion bridge between mission-level trajectory objectives and physical propulsion. The altitude loop determines how vertical position error should become climb or descent motion, the vertical-speed loop converts that motion into acceleration and thrust demand, and feedforward compensation balances predictable forces such as gravity. Together with attitude stabilization and later position control, this structure provides the controlled three-dimensional motion required for reliable cargo-UAV operations.

고도 제어(Altitude Control)는 화물 무인항공기(Cargo UAV)의 수직 위치를 조절하며, 수직 속도 제어기(Vertical-Speed Controller)는 항공기가 상승하거나 하강하는 속도를 제어한다. 이러한 기능은 상위 수준의 궤적 명령(Trajectory Command)과 추진 시스템(Propulsion System) 사이에서 직렬 제어 구조(Cascaded Control Structure)를 형성한다. 비행 제어 구조에서 고도 및 수직 속도 제어는 자세 제어(Attitude Control) 이후, 위치 및 속도 제어 이전에 배치되어 3차원 비행을 위한 조정된 제어 계층을 구성한다.

외부 고도 루프(Outer Altitude Loop)는 명령된 고도와 추정된 고도를 비교하고 그 결과로 발생한 고도 오차(Height Error)를 목표 수직 속도 명령(Desired Vertical-Speed Command)으로 변환한다. 고도 오차로부터 추력을 직접 제어하는 대신 이러한 중간 기준값을 사용하면 수직 운동을 점진적으로 형성할 수 있다. 이 단계에서 최대 상승률과 하강률을 제한하여 큰 고도 오차가 항공기의 안전 운용 영역(Safe Operating Envelope)을 벗어나는 명령을 발생시키지 않도록 할 수 있다.

내부 수직 속도 루프(Inner Vertical-Speed Loop)는 명령된 상승 또는 하강 속도를 추정된 수직 속도와 비교한다. 그 결과로 발생하는 오차는 수직 가속도(Vertical Acceleration) 또는 집단 추력 보정값(Collective-Thrust Correction)으로 변환된다. 이 루프는 고도 루프보다 빠르게 동작하기 때문에 상대적으로 느린 외부 루프가 장기적인 고도 정확도를 유지하는 동안 단기적인 수직 외란을 억제할 수 있다. 이러한 대역폭 분리(Bandwidth Separation)는 예측 가능한 직렬 제어 동작의 기본 조건이다.

따라서 수직 상태 추정(Vertical State Estimation)은 제어기 성능과 밀접하게 연계된다. 고도 정보는 위성항법시스템(Global Navigation Satellite System, GNSS), 기압 센서(Barometric Pressure Sensor), 레이더 또는 레이저 고도계(Radar or Laser Altimeter), 융합 항법 추정값(Fused Navigation Estimate) 등에서 얻을 수 있으며, 수직 속도는 관성 및 항법 측정값으로부터 계산할 수 있다. 고도나 속도 추정값의 불연속은 즉각적으로 원하지 않는 추력 명령을 발생시킬 수 있으므로 제어기는 개별 원시 센서를 독립적으로 전환하기보다 일관된 융합 상태(Fused State)를 사용해야 한다.

기본적인 고도 제어기는 비례 제어(Proportional Control) 또는 비례-적분 제어(Proportional-Integral Control)를 이용하여 고도 오차를 수직 속도 요구값으로 변환할 수 있다. 비례 동작은 항공기가 고도 차이를 얼마나 적극적으로 감소시킬지를 결정하며, 적분 동작은 지속적인 오프셋을 제거할 수 있다. 일반적으로 명령은 상승률, 하강률, 가속도 및 저크(Jerk) 제한을 통과하도록 하여 화물 안정성, 무인 항공기 구조적 한계 및 추진 성능에 적합한 부드러운 운동을 생성해야 한다.

수직 속도 제어기(Vertical-Speed Controller)는 일반적으로 수직 속도를 대상으로 비례-적분-미분 제어(Proportional-Integral-Derivative Control, PID) 또는 이에 상응하는 피드백 제어를 적용한다. 비례 동작은 순간적인 속도 오차에 대응하고, 적분 동작은 지속적인 추력 불일치를 보상하며, 가속도 관련 감쇠(Acceleration-Related Damping)는 과도 응답 특성을 개선할 수 있다. 화물 무인항공기에서는 반복적인 가속이 구조 하중을 증가시키고 현수 화물을 교란하며 추진 효율을 감소시킬 수 있으므로 과도한 수직 진동을 방지해야 한다.

호버 추력 추정(Hover-Thrust Estimation)은 중력이 지속적으로 작용하기 때문에 특히 중요하다. 피드백 보정을 적용하기 전에 중력에 대응하는 힘을 지속적으로 생성해야 한다. 기체를 지지하는 데 필요한 기본 추력을 알고 있다면 이를 피드포워드 항(Feedforward Term)으로 적용하고 피드백 제어기는 편차만을 보정하도록 할 수 있다. 이러한 보상이 없으면 적분 제어기가 정상 호버 명령의 대부분을 생성해야 하므로 수렴이 느려지고 포화(Saturation)에 대한 민감도가 증가할 수 있다.

화물 질량(Cargo Mass)은 필요한 호버 추력을 직접적으로 변화시킨다. 무거운 화물을 탑재한 항공기는 무화물 상태의 동일 기체보다 훨씬 큰 집단 추진력(Collective Propulsion)을 필요로 한다. 따라서 신뢰할 수 있는 화물 또는 기체 질량 정보가 사용 가능하다면 비행 제어 소프트웨어는 기본 추력 모델(Nominal Thrust Model)을 갱신해야 한다. 이러한 관계는 고도 제어를 변화하는 화물 중량에 대응하는 전용 부하 변화 적응 제어(Load-Change Adaptive Control) 기능과 연결한다.

무게중심 변화(Center-of-Gravity Variation)는 수직 제어에도 간접적인 영향을 줄 수 있다. 고도 제어는 주로 집단 추력을 결정하지만 무게중심이 중심에서 벗어나면 자세를 유지하기 위해 차등 액추에이터 힘(Differential Actuator Force)이 필요할 수 있다. 이에 따라 추진 능력의 일부가 순수한 수직력 생성이 아니라 자세 안정화에 사용된다. 따라서 고도 제어기와 제어 할당(Control Allocation) 서브시스템은 이론적인 최대 추진력을 항상 상승에 사용할 수 있다고 가정하지 않고 실제 남아 있는 추력 제어 권한(Thrust Authority)을 고려해야 한다.

기체 자세(Vehicle Attitude)는 전체 추력과 수직력 사이의 관계를 변화시킨다. 수평 호버링에서는 대부분의 집단 추력이 중력에 대응하지만 상당한 롤 또는 피치 각도가 발생하면 추력 벡터의 일부가 수평 가속도를 생성한다. 따라서 수직 제어에서는 감소한 수직 추력 성분을 고려하여 명령된 집단 추력을 조정하는 기울기 보상(Tilt Compensation)이 필요할 수 있다. 그러나 급격한 기울기와 높은 고도 요구가 동시에 발생하면 추진 시스템이 포화될 수 있으므로 이러한 보상에는 적절한 제한이 필요하다.

추력 포화(Thrust Saturation)는 수직 제어에서 가장 중요한 비선형 제약(Nonlinear Constraint) 가운데 하나이다. 최대 추력은 상승 능력을 제한하고, 최소 사용 가능 추력과 기체 동역학은 빠른 하강 능력을 제한한다. 명령된 가속도가 사용 가능한 추진 제어 권한을 초과하는 경우 적분 항이 무한정 오차를 누적해서는 안 된다. 안티와인드업 로직(Anti-Windup Logic), 명령 제한(Command Limiting), 포화 피드백(Saturation Feedback)을 적용하면 정상적인 제어 권한이 다시 확보된 이후 제어기가 부드럽게 복구될 수 있다.

하강 동작(Descent Behavior)은 멀티로터 공기역학(Multirotor Aerodynamics)이 높은 하강 속도에서 불리해질 수 있기 때문에 특별한 주의가 필요하다. 따라서 제어기는 상승과 하강 운동이 완전히 대칭적이라고 가정하는 대신 검증된 하강률 및 가속도 제한을 적용해야 한다. 대형 화물은 질량 증가로 인해 운동량, 회복에 필요한 추력 및 착륙이나 장애물 회피 전에 하강을 정지시키는 데 필요한 수직 거리가 달라지므로 안전한 하강 특성에도 영향을 준다.

바람 외란(Wind Disturbance)은 수평 방향뿐만 아니라 수직 방향의 제어 오차도 발생시킨다. 상승기류(Updraft)와 하강기류(Downdraft)는 집단 추력이 일정하게 유지되는 경우에도 상승률을 변화시킬 수 있으며, 대형 구조물 주변의 난류(Turbulent Airflow)는 빠르게 변화하는 수직력을 발생시킬 수 있다. 내부 수직 속도 루프는 측정 잡음에 과도하게 반응하지 않으면서 이러한 외란을 억제해야 한다. 신뢰할 수 있는 정보가 제공되는 경우 외란 추정(Disturbance Estimation) 또는 가속도 피드포워드(Acceleration Feedforward)를 이용하여 응답 성능을 더욱 향상시킬 수 있다.

명령 형상화(Command Shaping)는 화물 운송 임무에서 특히 중요하다. 호버링 상태에서 최대 상승으로 갑자기 전환하면 항공기가 제어 가능한 상태를 유지하더라도 큰 가속도와 저크가 발생하여 화물 이동 및 구조 하중을 유발할 수 있다. 따라서 수직 명령을 변화율 및 가속도 제한기(Rate and Acceleration Limiter) 또는 궤적 생성기(Trajectory Generator)를 통과시킬 수 있다. 부드러운 기준 명령은 기계적 응력을 감소시키고 제어기에 물리적으로 비현실적인 계단 형태의 변화를 반복적으로 요구하지 않으므로 추종 성능도 향상시킨다.

이륙(Takeoff)은 기체가 지면 지지 상태에서 공력 지지 상태로 이동하기 때문에 특별한 수직 제어 전환 과정이 필요하다. 이륙 전에는 착륙 장치가 여전히 기체 중량의 일부를 지지하므로 추력이 증가해도 이에 대응하는 수직 가속도가 발생하지 않을 수 있다. 제어기는 이러한 상태를 일반적인 고도 추종 실패로 해석해서는 안 된다. 정상적인 공중 고도 제어가 완전한 제어 권한을 갖기 전에 제어된 추력 램핑(Controlled Thrust Ramping), 이륙 감지(Liftoff Detection), 추정기 유효성(Estimator Validity), 전환 로직(Transition Logic)이 필요하다.

착륙(Landing)은 이와 반대되는 전환 과정이다. 고도 제어기는 적절한 하강 프로파일(Descent Profile)을 유지하면서 고도를 점진적으로 낮춰야 하지만 지면에 가까워질수록 제어 목표는 접지 관리(Touchdown Management)로 전환된다. 이 단계에서는 전역 고도 기준보다 레이더 또는 레이저 고도 측정값이 더 중요해질 수 있다. 착륙 장치가 지면과 접촉한 이후 명령된 고도에 도달할 수 없다는 이유만으로 제어기가 추력을 다시 증가시키지 않도록 접지 감지(Touchdown Detection) 기능이 필요하다.

고도 기준 관리(Altitude-Reference Management)는 절대 고도(Absolute Altitude), 지상고(Altitude Above Ground Level), 임무 기준 상대 고도(Altitude Relative to Mission Reference)를 구분해야 한다. GNSS 기반 고도, 기압 고도(Barometric Altitude), 지형 상대 고도(Terrain-Relative Altitude), 지역 항법 좌표계의 높이는 서로 동일한 물리량을 나타내지 않는다. 비행 제어 인터페이스는 어떤 기준을 제어하고 있는지를 명확하게 정의하고 기준 전환 과정에서 갑작스러운 고도 오차 또는 의도하지 않은 상승 및 하강 명령이 발생하지 않도록 해야 한다.

제어기는 고도 오차, 수직 속도 오차, 명령 가속도, 추력 요구량, 포화 상태, 적분 상태 및 사용 가능한 추력 여유(Thrust Margin) 등의 변수를 통해 자체 운용 상태를 모니터링해야 한다. 이러한 값은 비행 모니터링과 고장 감지(Fault Detection)를 지원한다. 예를 들어 최대 추력 상태에서 지속적인 고도 오차가 발생한다면 단순히 제어기 튜닝 문제라기보다 과도한 화물, 추진 시스템 성능 저하, 강한 하강기류 또는 잘못된 기체 질량 추정이 원인일 수 있다.

고장 처리(Failure Handling)는 가능한 경우 기본적인 수직 안정성을 유지해야 한다. 고도 센서의 성능 저하, GNSS 신호 손실, 추진 시스템 고장 또는 신뢰할 수 없는 수직 속도 추정값이 발생하면 대체 상태 정보로 전환하거나 자율 기능을 제한해야 할 수 있다. 제어기는 유효성 및 제어 권한 정보를 비행 모드 시스템(Flight-Mode System)과 안전 시스템(Safety System)에 제공하여 남아 있는 기능에 따라 호버링, 제어된 하강(Controlled Descent), 자동 귀환(Return-to-Home), 비상 착륙(Emergency Landing) 등의 동작을 선택할 수 있도록 해야 한다.

검증(Verification)은 정상적인 고도 추종 성능만이 아니라 전체 수직 제어 체인(Vertical-Control Chain)을 평가해야 한다. 시뮬레이션, 소프트웨어 인 더 루프(Software-in-the-Loop, SIL), 하드웨어 인 더 루프(Hardware-in-the-Loop, HIL) 시험에서 화물 변화, 추력 모델 오차, 센서 잡음, 지연된 측정값, 바람 외란, 액추에이터 포화 및 추진 시스템 성능 저하를 적용할 수 있다. 계단 및 램프 형태의 고도 명령을 통해 과도 응답을 평가하고, 장시간 호버링 시험을 통해 정상 상태 드리프트, 적분 동작, 열적 영향 및 기체 질량 변화에 대한 민감도를 확인할 수 있다.

고도 및 수직 속도 제어기(Altitude and Vertical-Speed Controller)는 궁극적으로 임무 수준의 궤적 목표와 실제 추진 시스템 사이에서 수직 운동을 연결하는 역할을 한다. 고도 루프는 수직 위치 오차를 어떻게 상승 또는 하강 운동으로 변환할지를 결정하고, 수직 속도 루프는 이러한 운동을 가속도와 추력 요구량으로 변환하며, 피드포워드 보상(Feedforward Compensation)은 중력과 같이 예측 가능한 힘을 상쇄한다. 이러한 구조는 자세 안정화 및 이후의 위치 제어와 함께 신뢰성 높은 화물 무인항공기 운용에 필요한 제어된 3차원 운동을 제공한다.

##  

## 03.05. Position and Velocity Controller Outer Loop [w/Code]

![](images/image5.png){width="7.268055555555556in" height="7.268055555555556in"}

The position and velocity controller forms the outer translational layer of the cargo UAV flight-control hierarchy. Its purpose is to convert desired horizontal position or trajectory references into motion commands that the faster attitude controller can execute. Within the chapter structure, this function follows attitude and altitude control and precedes motor allocation, adaptive payload control, wind rejection, and flight-mode management, defining its role as the outer-loop bridge between navigation and stabilization.

The position loop compares the commanded horizontal position with the estimated vehicle position in a defined navigation frame. The resulting position error represents how far the aircraft is displaced from its target in each horizontal direction. Rather than converting this error directly into motor commands, the controller generates a desired velocity reference. This separation allows position convergence to be shaped independently from the fast rotational dynamics of the aircraft.

The velocity loop operates inside the position loop and compares the desired velocity with the estimated horizontal velocity. Velocity error is transformed into a commanded acceleration or equivalent horizontal force demand. The controller therefore creates a sequence from position error to velocity reference, from velocity error to acceleration demand, and finally from acceleration demand to the attitude and thrust commands required to move the vehicle.

For a multirotor aircraft, horizontal acceleration is generated primarily by tilting the total thrust vector. A forward acceleration command requires an appropriate pitch attitude, while lateral acceleration requires roll. The outer-loop controller must therefore translate desired acceleration expressed in the navigation frame into a desired thrust-vector orientation. The attitude controller then tracks this orientation using the faster roll, pitch, and angular-rate stabilization loops.

This architecture depends on clear bandwidth separation. The attitude and angular-rate loops must respond significantly faster than the velocity loop, while the position loop should normally be slower than velocity regulation. If the position controller is tuned too aggressively, it can command velocity or acceleration changes faster than the inner loops can reproduce them. The result may be overshoot, oscillation, actuator saturation, or poor trajectory tracking.

Position feedback normally originates from the navigation estimator rather than directly from a single sensor. GNSS may provide globally referenced position and velocity, while inertial measurements support high-rate propagation between GNSS updates. Other navigation sources can contribute when available. The outer controller should receive a continuous, time-consistent state estimate because sudden position jumps or velocity discontinuities can generate large and unnecessary acceleration commands.

A proportional position controller provides a simple relationship between position error and desired velocity. Larger displacement produces a larger velocity command, while the commanded speed decreases naturally as the aircraft approaches its target. Practical implementations limit this reference according to mission speed, flight envelope, obstacle environment, and vehicle capability. Additional shaping can prevent abrupt changes when waypoints or trajectory segments are switched.

The velocity controller can use proportional-integral or PID-like feedback to generate acceleration demand. Proportional action responds immediately to velocity error, while integral action compensates for persistent effects such as wind, propulsion asymmetry, or small modeling errors. Derivative or acceleration-related feedback may provide additional damping, but noisy velocity estimates require careful filtering so that high-frequency measurement noise is not converted into rapidly varying attitude commands.

Acceleration commands must be constrained before they are passed to the attitude-control layer. Excessive horizontal acceleration requires large tilt angles and consumes thrust that would otherwise support vehicle weight. Maximum acceleration, tilt angle, velocity, and command slew rate should therefore be coordinated. These limits become especially important for heavy cargo UAVs because structural loading, payload motion, propulsion margin, and stopping distance can impose tighter constraints than those of smaller vehicles.

The coupling between horizontal and vertical control cannot be ignored. When the aircraft tilts to accelerate horizontally, only part of its total thrust remains available to oppose gravity. The altitude controller may request additional collective thrust to preserve height, but propulsion capability is finite. The flight-control architecture must therefore coordinate horizontal acceleration demands with vertical thrust margin so that aggressive position tracking does not unintentionally produce altitude loss.

Cargo mass strongly influences translational response. A heavier aircraft requires greater force to achieve the same acceleration, while payload-dependent inertia and propulsion loading can change transient behavior. If the controller assumes an incorrect mass, acceleration response may be weaker or stronger than expected. Feedforward based on estimated vehicle mass can improve force prediction, while feedback compensates for residual uncertainty and the dedicated load-adaptive functions can address larger configuration changes.

Center-of-gravity displacement and cargo motion can further affect position tracking because the attitude controller may require additional control effort to maintain the thrust-vector orientation requested by the outer loop. Suspended cargo can also introduce pendulum-like motion when aggressive acceleration commands are applied. Position trajectories should therefore be shaped with acceleration and jerk limits that reduce excitation of payload dynamics while preserving acceptable mission performance.

Wind is one of the most significant disturbances affecting horizontal position and velocity control. A steady wind may require continuous tilt and thrust to maintain a fixed position, while gusts can create rapid velocity deviations. Integral feedback can compensate for persistent disturbances, but dedicated wind estimation can provide feedforward information that reduces tracking error. The surrounding flight-control structure consequently treats wind-disturbance rejection and estimation as a dedicated function following the basic position controller.

Waypoint flight illustrates the interaction between navigation and the outer-loop controller. A mission manager or trajectory generator defines the desired path, while the position controller determines the local motion required to follow it. Simply commanding each waypoint as an independent position step can produce abrupt velocity changes. Smooth trajectory references containing position, velocity, and potentially acceleration allow the controller to anticipate motion and reduce unnecessary transient error.

Feedforward terms are particularly valuable when the trajectory generator already provides desired velocity or acceleration. Instead of waiting for position error to develop, the controller can apply the expected motion directly and use feedback to correct deviations. This improves tracking during curved paths, coordinated turns, acceleration phases, and deceleration before arrival. Feedback remains essential because wind, mass uncertainty, and propulsion variation prevent purely model-based execution from being sufficiently accurate.

Stopping behavior must be considered as part of position-control design. A heavy cargo UAV traveling at substantial speed cannot instantaneously stop when it reaches a target coordinate. The controller or trajectory generator must account for available deceleration, tilt limits, thrust margin, payload constraints, and environmental conditions. Braking should begin sufficiently early to prevent waypoint overshoot while avoiding aggressive commands that could destabilize cargo or saturate propulsion.

Position-hold mode places a different demand on the same controller. Instead of following a continuously changing trajectory, the aircraft attempts to maintain a fixed horizontal location despite wind and estimation noise. Excessive controller gain can cause constant small attitude corrections and actuator activity, while insufficient gain permits drift. Appropriate deadbands, filtering, integral limits, and disturbance compensation can balance position accuracy against unnecessary control activity.

Command saturation requires coordinated anti-windup behavior. When velocity or acceleration demands exceed permitted limits, the position and velocity integrators should not continue accumulating error as if the requested motion remained achievable. Saturation status can be propagated between control layers so that outer loops understand when inner-loop authority is constrained. This prevents large stored errors from producing overshoot after the vehicle returns to an achievable operating region.

Fault conditions may require degradation from position control to simpler stabilization modes. GNSS loss, inconsistent navigation estimates, excessive position uncertainty, or degraded propulsion can make precise trajectory tracking unreliable. The controller should expose state validity and control-authority information to the flight-mode manager so that the aircraft can transition to velocity control, attitude stabilization, return-to-home, controlled landing, or another predefined contingency behavior.

Real-time implementation requires deterministic exchange of state estimates and commands among navigation, position control, velocity control, attitude control, and actuator functions. Timestamp consistency is particularly important because delayed position or velocity measurements represent an earlier aircraft state. Latency and timing jitter can reduce phase margin and tracking quality, especially when outer-loop bandwidth is increased for demanding trajectory-following applications.

Verification should evaluate both trajectory tracking and disturbance response over the expected cargo-UAV operating envelope. Simulation and SIL/HIL testing can vary payload mass, center of gravity, wind, navigation noise, sensor latency, actuator limits, and propulsion capability. Tests should examine position error, velocity error, acceleration demand, tilt command, settling behavior, saturation frequency, stopping distance, and recovery after disturbances or navigation degradation.

The position and velocity outer loop ultimately transforms geometric mission intent into dynamically achievable aircraft motion. Position error determines the desired translational progression, velocity regulation determines the required acceleration, and acceleration is converted into thrust-vector orientation for the inner attitude controller. Integrated with altitude control, payload adaptation, wind rejection, and control allocation, this outer-loop architecture enables a cargo UAV to follow trajectories accurately while respecting its physical and safety constraints.

위치 및 속도 제어기(Position and Velocity Controller)는 화물 무인항공기(Cargo UAV) 비행 제어 계층에서 외부 병진 제어 계층(Outer Translational Layer)을 구성한다. 이 제어기의 목적은 목표 수평 위치 또는 궤적 기준을 빠르게 동작하는 자세 제어기(Attitude Controller)가 실행할 수 있는 운동 명령으로 변환하는 것이다. 비행 제어 구조에서 이 기능은 자세 및 고도 제어 이후, 모터 할당(Motor Allocation), 화물 적응 제어(Adaptive Payload Control), 바람 외란 제거(Wind Rejection), 비행 모드 관리(Flight-Mode Management) 이전에 위치하여 항법과 안정화 사이의 외부 루프 연결 계층 역할을 수행한다.

위치 루프(Position Loop)는 정의된 항법 좌표계(Navigation Frame)에서 명령된 수평 위치와 추정된 기체 위치를 비교한다. 그 결과로 생성되는 위치 오차(Position Error)는 각 수평 방향에서 항공기가 목표 지점으로부터 얼마나 벗어나 있는지를 나타낸다. 이 오차를 직접 모터 명령으로 변환하는 대신 제어기는 목표 속도 기준(Desired Velocity Reference)을 생성한다. 이러한 분리를 통해 항공기의 빠른 회전 동역학과 독립적으로 위치 수렴 특성을 조정할 수 있다.

속도 루프(Velocity Loop)는 위치 루프 내부에서 동작하며 목표 속도와 추정된 수평 속도를 비교한다. 속도 오차(Velocity Error)는 명령 가속도(Commanded Acceleration) 또는 이에 상응하는 수평 힘 요구량(Horizontal Force Demand)으로 변환된다. 따라서 제어기는 위치 오차에서 속도 기준으로, 속도 오차에서 가속도 요구량으로, 최종적으로 가속도 요구량에서 기체를 이동시키는 데 필요한 자세 및 추력 명령으로 이어지는 제어 흐름을 형성한다.

멀티로터 항공기(Multirotor Aircraft)에서 수평 가속도는 주로 전체 추력 벡터(Total Thrust Vector)를 기울임으로써 생성된다. 전방 가속 명령에는 적절한 피치(Pitch) 자세가 필요하며, 측면 가속에는 롤(Roll)이 필요하다. 따라서 외부 루프 제어기는 항법 좌표계에서 표현된 목표 가속도를 원하는 추력 벡터 방향(Thrust-Vector Orientation)으로 변환해야 한다. 이후 자세 제어기는 더 빠른 롤, 피치 및 각속도 안정화 루프를 이용하여 이 자세를 추종한다.

이러한 구조는 명확한 대역폭 분리(Bandwidth Separation)에 의존한다. 자세 및 각속도 루프는 속도 루프보다 상당히 빠르게 응답해야 하며, 위치 루프는 일반적으로 속도 제어보다 느리게 동작해야 한다. 위치 제어기가 지나치게 공격적으로 튜닝되면 내부 루프가 구현할 수 있는 속도보다 빠른 속도 또는 가속도 변화를 요구할 수 있다. 그 결과 오버슈트(Overshoot), 진동(Oscillation), 액추에이터 포화(Actuator Saturation) 또는 궤적 추종 성능 저하가 발생할 수 있다.

위치 피드백(Position Feedback)은 일반적으로 하나의 센서로부터 직접 획득하기보다 항법 추정기(Navigation Estimator)에서 제공된다. 위성항법시스템(Global Navigation Satellite System, GNSS)은 전역 기준 위치와 속도를 제공할 수 있으며, 관성 측정값(Inertial Measurement)은 GNSS 업데이트 사이에서 고주파 상태 전파를 지원한다. 사용 가능한 경우 다른 항법 정보도 추가될 수 있다. 갑작스러운 위치 변화나 속도 불연속이 큰 불필요한 가속도 명령을 발생시킬 수 있으므로 외부 제어기는 시간적으로 일관된 연속 상태 추정값을 사용해야 한다.

비례 위치 제어기(Proportional Position Controller)는 위치 오차와 목표 속도 사이에 단순한 관계를 제공한다. 목표로부터의 거리가 클수록 더 큰 속도 명령이 생성되고, 항공기가 목표에 접근할수록 명령 속도는 자연스럽게 감소한다. 실제 구현에서는 임무 속도, 비행 영역(Flight Envelope), 장애물 환경 및 기체 성능에 따라 이 기준값을 제한한다. 웨이포인트(Waypoint) 또는 궤적 구간이 전환될 때 급격한 변화가 발생하지 않도록 추가적인 명령 형상화(Command Shaping)를 적용할 수도 있다.

속도 제어기(Velocity Controller)는 비례-적분 제어(Proportional-Integral Control) 또는 PID 형태의 피드백을 이용하여 가속도 요구량을 생성할 수 있다. 비례 동작은 속도 오차에 즉시 대응하고, 적분 동작은 바람, 추진 비대칭 또는 작은 모델링 오차와 같은 지속적인 영향을 보상한다. 미분 또는 가속도 관련 피드백은 추가적인 감쇠(Damping)를 제공할 수 있지만, 잡음이 포함된 속도 추정값은 고주파 측정 잡음이 빠르게 변화하는 자세 명령으로 변환되지 않도록 신중하게 필터링해야 한다.

가속도 명령은 자세 제어 계층에 전달되기 전에 제한되어야 한다. 과도한 수평 가속도는 큰 기울기 각도(Tilt Angle)를 필요로 하며 기체 중량을 지지하는 데 사용될 추력의 일부를 소비한다. 따라서 최대 가속도, 기울기 각도, 속도 및 명령 변화율(Command Slew Rate)을 상호 조정해야 한다. 대형 화물 무인항공기에서는 구조 하중, 화물 운동, 추진 여유(Propulsion Margin), 정지 거리 등이 소형 기체보다 더 엄격한 제약을 부과할 수 있으므로 이러한 제한이 특히 중요하다.

수평 제어와 수직 제어 사이의 결합(Coupling)은 무시할 수 없다. 항공기가 수평으로 가속하기 위해 기울어지면 전체 추력 중 일부만 중력에 대응하는 데 사용된다. 고도 제어기(Altitude Controller)는 높이를 유지하기 위해 추가적인 집단 추력(Collective Thrust)을 요구할 수 있지만 추진 시스템의 능력에는 한계가 있다. 따라서 비행 제어 구조는 공격적인 위치 추종으로 인해 의도하지 않은 고도 손실이 발생하지 않도록 수평 가속도 요구량과 수직 추력 여유(Vertical Thrust Margin)를 조정해야 한다.

화물 질량(Cargo Mass)은 병진 운동 응답에 큰 영향을 준다. 무거운 항공기는 동일한 가속도를 얻기 위해 더 큰 힘이 필요하며, 화물에 따라 변화하는 관성과 추진 부하는 과도 응답 특성을 변화시킬 수 있다. 제어기가 잘못된 질량을 가정하면 실제 가속도 응답이 예상보다 약하거나 강하게 나타날 수 있다. 추정된 기체 질량을 기반으로 한 피드포워드(Feedforward)는 힘 예측을 개선할 수 있으며, 피드백은 남아 있는 불확실성을 보정하고 전용 부하 적응 기능(Load-Adaptive Function)은 더 큰 기체 구성 변화를 처리할 수 있다.

무게중심 변위(Center-of-Gravity Displacement)와 화물 운동도 위치 추종에 영향을 줄 수 있다. 자세 제어기가 외부 루프에서 요구한 추력 벡터 방향을 유지하기 위해 추가적인 제어력을 필요로 할 수 있기 때문이다. 현수 화물(Suspended Cargo)은 공격적인 가속도 명령이 적용될 경우 진자 형태의 운동(Pendulum-Like Motion)을 발생시킬 수도 있다. 따라서 위치 궤적은 허용 가능한 임무 성능을 유지하면서 화물 동역학의 가진을 줄일 수 있도록 가속도와 저크(Jerk) 제한을 적용하여 형성해야 한다.

바람(Wind)은 수평 위치 및 속도 제어에 영향을 미치는 가장 중요한 외란 가운데 하나이다. 일정한 바람에서는 고정된 위치를 유지하기 위해 지속적인 기울기와 추력이 필요할 수 있으며, 돌풍(Gust)은 빠른 속도 편차를 발생시킬 수 있다. 적분 피드백은 지속적인 외란을 보상할 수 있지만 전용 바람 추정(Wind Estimation)을 이용하면 추종 오차를 감소시키는 피드포워드 정보를 제공할 수 있다. 따라서 전체 비행 제어 구조에서는 기본 위치 제어 이후 바람 외란 제거 및 추정을 별도의 전용 기능으로 다룬다.

웨이포인트 비행(Waypoint Flight)은 항법과 외부 루프 제어기의 상호작용을 보여주는 대표적인 사례이다. 임무 관리자(Mission Manager) 또는 궤적 생성기(Trajectory Generator)는 목표 경로를 정의하고, 위치 제어기는 해당 경로를 추종하는 데 필요한 국부적인 운동을 결정한다. 각 웨이포인트를 독립적인 위치 계단 명령(Position Step Command)으로 단순하게 처리하면 급격한 속도 변화가 발생할 수 있다. 위치, 속도 및 필요한 경우 가속도를 포함하는 부드러운 궤적 기준을 사용하면 제어기가 운동을 미리 예측하고 불필요한 과도 오차를 감소시킬 수 있다.

궤적 생성기가 이미 목표 속도 또는 가속도를 제공하는 경우 피드포워드 항(Feedforward Term)이 특히 유용하다. 위치 오차가 발생하기를 기다리는 대신 제어기는 예상되는 운동을 직접 적용하고 피드백을 이용하여 실제 편차를 보정할 수 있다. 이를 통해 곡선 경로, 협조 선회(Coordinated Turn), 가속 구간 및 목표 지점 도착 전 감속 과정에서 추종 성능을 향상시킬 수 있다. 그러나 바람, 질량 불확실성 및 추진력 변화로 인해 순수한 모델 기반 실행만으로는 충분한 정확성을 확보하기 어려우므로 피드백은 여전히 필수적이다.

정지 동작(Stopping Behavior)도 위치 제어 설계의 일부로 고려해야 한다. 상당한 속도로 이동하는 대형 화물 무인항공기는 목표 좌표에 도달하는 순간 즉시 정지할 수 없다. 제어기 또는 궤적 생성기는 사용 가능한 감속도, 기울기 제한, 추력 여유, 화물 제약 및 환경 조건을 고려해야 한다. 화물을 불안정하게 만들거나 추진 시스템을 포화시키는 공격적인 명령을 피하면서 웨이포인트 오버슈트를 방지할 수 있도록 충분히 이른 시점에서 제동(Braking)을 시작해야 한다.

위치 유지 모드(Position-Hold Mode)는 동일한 제어기에 다른 형태의 요구조건을 부여한다. 지속적으로 변화하는 궤적을 추종하는 대신 항공기는 바람과 상태 추정 잡음에도 불구하고 고정된 수평 위치를 유지하려 한다. 제어기 이득이 지나치게 높으면 지속적으로 작은 자세 보정과 액추에이터 동작이 발생하며, 이득이 부족하면 위치 드리프트(Position Drift)가 발생한다. 적절한 데드밴드(Deadband), 필터링, 적분 제한 및 외란 보상을 통해 위치 정확성과 불필요한 제어 동작 사이의 균형을 확보할 수 있다.

명령 포화(Command Saturation)가 발생할 경우 제어 계층 사이에서 조정된 안티와인드업(Anti-Windup) 동작이 필요하다. 속도 또는 가속도 요구량이 허용 한계를 초과하면 위치 및 속도 적분기는 요구된 운동이 계속 달성 가능한 것처럼 오차를 누적해서는 안 된다. 포화 상태 정보를 제어 계층 사이에서 전달하여 외부 루프가 내부 루프의 제어 권한이 제한되고 있음을 인식하도록 할 수 있다. 이를 통해 기체가 다시 달성 가능한 운용 영역으로 복귀한 이후 누적된 큰 오차로 인해 오버슈트가 발생하는 것을 방지한다.

고장 조건(Fault Condition)에서는 위치 제어에서 더 단순한 안정화 모드로 성능을 단계적으로 저하시켜야 할 수 있다. GNSS 손실, 일관되지 않은 항법 추정값, 과도한 위치 불확실성 또는 추진 시스템 성능 저하는 정밀한 궤적 추종의 신뢰성을 떨어뜨릴 수 있다. 제어기는 상태 유효성(State Validity)과 제어 권한 정보를 비행 모드 관리자(Flight-Mode Manager)에 제공하여 항공기가 속도 제어, 자세 안정화, 자동 귀환(Return-to-Home), 제어된 착륙(Controlled Landing) 또는 사전에 정의된 다른 비상 동작(Contingency Behavior)으로 전환할 수 있도록 해야 한다.

실시간 구현(Real-Time Implementation)에서는 항법, 위치 제어, 속도 제어, 자세 제어 및 액추에이터 기능 사이에서 상태 추정값과 명령을 결정론적으로 교환해야 한다. 지연된 위치 또는 속도 측정값은 과거 시점의 항공기 상태를 나타내므로 타임스탬프 일관성(Timestamp Consistency)이 특히 중요하다. 지연시간(Latency)과 타이밍 지터(Timing Jitter)는 위상 여유와 추종 성능을 감소시킬 수 있으며, 특히 높은 성능의 궤적 추종을 위해 외부 루프 대역폭을 증가시키는 경우 그 영향이 커진다.

검증(Verification)에서는 예상되는 화물 무인항공기 운용 영역 전체에 대해 궤적 추종과 외란 대응 성능을 모두 평가해야 한다. 시뮬레이션과 소프트웨어 인 더 루프(Software-in-the-Loop, SIL), 하드웨어 인 더 루프(Hardware-in-the-Loop, HIL) 시험을 통해 화물 질량, 무게중심, 바람, 항법 잡음, 센서 지연, 액추에이터 제한 및 추진 능력을 변화시킬 수 있다. 시험에서는 위치 오차, 속도 오차, 가속도 요구량, 기울기 명령, 정착 특성, 포화 발생 빈도, 정지 거리 및 외란이나 항법 성능 저하 이후의 복구 특성을 평가해야 한다.

위치 및 속도 외부 루프(Position and Velocity Outer Loop)는 궁극적으로 기하학적으로 정의된 임무 의도(Geometric Mission Intent)를 항공기가 동역학적으로 달성할 수 있는 운동으로 변환한다. 위치 오차는 목표 병진 운동을 결정하고, 속도 제어는 필요한 가속도를 결정하며, 가속도는 내부 자세 제어기가 사용할 추력 벡터 방향으로 변환된다. 이러한 외부 루프 구조를 고도 제어, 화물 적응, 바람 외란 제거 및 제어 할당과 통합하면 화물 무인항공기는 물리적 제약과 안전 제약을 준수하면서 정확하게 궤적을 추종할 수 있다.

##  

## 03.06. Multirotor Mixing Matrix and Motor Allocation [w/Code]

![](images/image6.png){width="7.268055555555556in" height="7.268055555555556in"}

The multirotor mixing matrix and motor-allocation function form the final transformation stage between the flight-control laws and the physical propulsion system. Attitude, altitude, and position controllers generate desired forces and body moments, but individual motors cannot directly interpret these abstract commands. The allocation layer converts collective thrust and roll, pitch, and yaw moment demands into coordinated commands for each propulsion unit.

For a conventional multirotor, each rotor contributes an upward or downward thrust component according to the vehicle coordinate convention, while its position relative to the center of gravity creates roll and pitch moments. Rotor aerodynamic drag also produces a reaction torque around the yaw axis. The total aircraft force and moment therefore result from the combined contribution of all rotors rather than from any single propulsion unit operating independently.

The mixing matrix mathematically describes this relationship. Each column represents the contribution of one motor or rotor to collective thrust and the three rotational moments, while each row represents a controlled force or moment axis. Motor geometry, arm length, rotor orientation, rotation direction, and thrust characteristics determine the matrix coefficients. The resulting model provides a compact mapping between actuator forces and the control objectives requested by the flight controller.

For a simple symmetric configuration, the relationship can often be inverted directly to determine individual motor commands from desired total thrust and moments. However, cargo UAVs may use six, eight, or more propulsion units to obtain greater lift capacity and redundancy. Such configurations can become overactuated, meaning that multiple combinations of rotor forces can generate approximately the same requested aircraft force and moment.

In an overactuated system, motor allocation becomes an optimization problem rather than a fixed mixing operation. A pseudoinverse can provide a nominal solution, while constrained optimization can select an actuator combination that satisfies thrust and moment commands while respecting motor limits. Additional objectives may minimize total actuator effort, balance motor loading, preserve control authority, or reduce thermal and electrical stress across the propulsion system.

The allocation model must reflect actual propulsion geometry accurately. Rotor position should be defined relative to the aircraft center of gravity, and thrust direction should account for any rotor cant or nonvertical installation. Heavy-lift cargo UAVs may employ distributed propulsion arrangements that are less symmetric than small quadrotors. In these systems, simplified fixed coefficients can produce systematic force and moment errors if the physical geometry is not represented correctly.

Motor commands also depend on the relationship between requested rotor thrust and actuator input. Rotor thrust is generally nonlinear with respect to rotational speed, while electronic speed controller commands may have additional nonlinearities, dead zones, and dynamic delays. A propulsion model or calibrated thrust mapping can convert desired rotor force into motor-speed or ESC commands so that the mixing layer operates in physically meaningful force units rather than assuming a linear command-to-thrust relationship.

Collective thrust and rotational moments compete for the same actuator authority. When the aircraft is near maximum payload and already using most available thrust to support its weight, only limited propulsion margin remains for roll, pitch, yaw, or vertical acceleration. The allocator must recognize these coupled constraints. Simply clipping individual motor commands after mixing can distort the requested force and moment combination and may degrade attitude stability.

Control prioritization is therefore important during saturation. Maintaining roll and pitch stability may be more critical than perfectly satisfying yaw or collective-thrust commands under some operating conditions. The allocation strategy can assign priorities or weights to different control objectives so that limited actuator authority is used for the most safety-critical functions first. The exact priority policy must remain consistent with vehicle dynamics, flight phase, and safety requirements.

Yaw control is particularly dependent on propulsion configuration. In a conventional multirotor, yaw moment is commonly produced by creating an imbalance between clockwise and counterclockwise rotor reaction torques. Generating yaw therefore changes the distribution of motor loading even when total thrust remains approximately constant. If motors approach saturation, available yaw authority can decrease significantly, requiring the controller to limit yaw-rate commands or accept temporary yaw-tracking error.

Center-of-gravity changes caused by cargo loading modify the effective moment arms between propulsion units and the vehicle mass center. A mixing matrix constructed around a fixed nominal center of gravity may therefore become less accurate when heavy cargo is positioned asymmetrically. The allocator can incorporate updated center-of-gravity information or trim corrections so that the required moments are generated without forcing the attitude controller to continuously compensate for allocation bias.

Payload variation also changes the propulsion operating point. An unloaded aircraft may hover with substantial thrust reserve, whereas a fully loaded vehicle may operate much closer to continuous motor limits. Allocation should therefore consider not only instantaneous command limits but also sustained propulsion capability. Continuous operation near maximum motor, inverter, battery, or thermal limits can reduce reliability even when individual commands remain within short-duration allowable values.

Rate limiting can prevent unrealistic actuator transitions. Flight controllers may generate rapid changes in requested moments during disturbances, but motors and propellers require finite time to accelerate or decelerate. If the allocator assumes instantaneous actuator response, the realized aircraft moment may differ substantially from the commanded value. Incorporating actuator dynamics, command slew limits, or achievable-rate constraints improves consistency between the control model and physical propulsion response.

Motor failure creates a more demanding allocation problem. If one propulsion unit becomes unavailable or loses effectiveness, the nominal mixing matrix no longer represents the actual system. A fault-aware allocator can remove or derate the failed actuator and recompute the achievable force and moment distribution using the remaining motors. Whether full attitude and altitude control can be maintained depends on propulsion redundancy, geometry, available thrust reserve, and the specific failed actuator.

Partial actuator degradation should also be represented because failures are not always binary. A motor, propeller, ESC, or power channel may produce less thrust than commanded while continuing to operate. Effectiveness coefficients can represent this reduced capability within the allocation model. Health-monitoring information can then modify the allocation matrix or actuator bounds, allowing remaining propulsion units to compensate within their available margins.

Allocation residuals provide valuable diagnostic information. After computing the motor commands, the system can estimate the force and moments that are actually achievable and compare them with the requested control vector. A large residual indicates that the requested command cannot be fully realized because of saturation, failure, geometry, or another constraint. This information should be returned to the upstream controllers and safety functions rather than hidden inside the allocation layer.

Anti-windup behavior benefits from this feedback. If the attitude or altitude controller requests control effort that the motor allocator cannot produce, integrators should not continue accumulating error as though the command were being executed. Achieved-force estimates, saturation flags, or allocation residuals can be propagated back through the control architecture. This creates coordinated saturation management across the controller and actuator layers.

Power-system constraints are especially relevant for heavy cargo UAVs. Multiple motors may individually remain within their limits while their combined electrical demand exceeds battery, generator, inverter, or distribution-system capability. Motor allocation can therefore include total power or current constraints in addition to individual actuator limits. Such coordination prevents a valid aerodynamic command from becoming an infeasible electrical command.

Real-time execution places strict requirements on the allocation algorithm. A fixed mixing matrix is computationally inexpensive, while constrained optimization provides greater flexibility but must still complete within the deterministic control period. Solver execution time, convergence behavior, numerical conditioning, and fallback logic must be evaluated under worst-case conditions. A sophisticated allocator is unsuitable if its timing behavior cannot be guaranteed on the flight-control computer.

Numerical robustness is also important when propulsion geometry produces poorly conditioned allocation matrices. Small command or modeling errors can otherwise generate disproportionately large actuator changes. Matrix scaling, condition monitoring, regularization, actuator weighting, and carefully designed geometry can improve robustness. The software should detect invalid configurations rather than issuing uncontrolled commands when the allocation model becomes singular or inconsistent.

Verification should test the complete path from commanded force and moments to realized propulsion response. Simulation and SIL/HIL environments can exercise nominal mixing, saturation, asymmetric loading, center-of-gravity shifts, motor nonlinearities, command delays, partial degradation, and complete motor failures. Tests should examine allocation error, remaining control authority, actuator utilization, timing performance, and transitions between nominal and degraded configurations.

The mixing and motor-allocation layer ultimately converts the mathematical objectives of the flight controller into physically realizable propulsion actions. Its design must combine vehicle geometry, actuator models, saturation handling, control priorities, payload effects, power constraints, and fault tolerance. Within the chapter structure, it provides the actuator-level foundation upon which subsequent load-adaptive control, wind-disturbance rejection, flight-mode management, and FCS verification functions can operate. Volume_23_Cargo_UAV_Autonomy_an...

멀티로터 믹싱 행렬(Multirotor Mixing Matrix)과 모터 할당(Motor Allocation) 기능은 비행 제어 법칙(Flight-Control Law)과 실제 추진 시스템(Physical Propulsion System) 사이의 최종 변환 단계를 구성한다. 자세, 고도 및 위치 제어기는 목표 힘과 기체 모멘트(Body Moment)를 생성하지만 개별 모터는 이러한 추상적인 명령을 직접 해석할 수 없다. 할당 계층(Allocation Layer)은 집단 추력(Collective Thrust)과 롤(Roll), 피치(Pitch), 요(Yaw) 모멘트 요구량을 각 추진 장치에 대한 조정된 명령으로 변환한다.

일반적인 멀티로터(Multirotor)에서는 각 로터가 기체 좌표계 규약(Vehicle Coordinate Convention)에 따라 상향 또는 하향 추력 성분에 기여하며, 무게중심(Center of Gravity)에 대한 로터의 상대적 위치에 의해 롤과 피치 모멘트가 생성된다. 로터의 공기역학적 항력(Rotor Aerodynamic Drag)은 요축 주변의 반작용 토크(Reaction Torque)도 발생시킨다. 따라서 전체 항공기의 힘과 모멘트는 하나의 추진 장치가 독립적으로 생성하는 것이 아니라 모든 로터의 기여가 결합되어 생성된다.

믹싱 행렬(Mixing Matrix)은 이러한 관계를 수학적으로 표현한다. 각 열(Column)은 하나의 모터 또는 로터가 집단 추력과 세 개의 회전 모멘트에 기여하는 정도를 나타내고, 각 행(Row)은 제어되는 힘 또는 모멘트 축을 나타낸다. 모터 형상, 암 길이(Arm Length), 로터 방향, 회전 방향 및 추력 특성이 행렬 계수를 결정한다. 이를 통해 액추에이터 힘과 비행 제어기가 요구하는 제어 목표 사이의 관계를 간결한 수학적 형태로 표현할 수 있다.

단순하고 대칭적인 구성에서는 목표 총추력과 모멘트로부터 개별 모터 명령을 결정하기 위해 이러한 관계를 직접 역변환할 수 있다. 그러나 화물 무인항공기(Cargo UAV)는 더 큰 양력 능력과 이중화(Redundancy)를 확보하기 위해 6개, 8개 또는 그 이상의 추진 장치를 사용할 수 있다. 이러한 구성은 과잉 구동 시스템(Overactuated System)이 될 수 있으며, 이는 여러 가지 로터 힘의 조합이 거의 동일한 항공기 힘과 모멘트를 생성할 수 있음을 의미한다.

과잉 구동 시스템에서 모터 할당은 고정된 믹싱 연산이 아니라 최적화 문제(Optimization Problem)가 된다. 의사역행렬(Pseudoinverse)을 이용하여 기본 해를 구할 수 있으며, 제약 최적화(Constrained Optimization)를 이용하면 모터 한계를 준수하면서 추력과 모멘트 명령을 만족하는 액추에이터 조합을 선택할 수 있다. 추가적인 목표로 전체 액추에이터 사용량 최소화, 모터 부하 균등화, 제어 권한(Control Authority) 보존, 추진 시스템 전체의 열적 및 전기적 스트레스 감소 등을 고려할 수 있다.

할당 모델(Allocation Model)은 실제 추진 시스템의 기하학적 구조를 정확하게 반영해야 한다. 로터 위치는 항공기 무게중심을 기준으로 정의되어야 하며, 추력 방향은 로터의 기울어진 설치(Rotor Cant) 또는 비수직 설치를 고려해야 한다. 대형 화물 무인항공기(Heavy-Lift Cargo UAV)는 소형 쿼드로터보다 비대칭적인 분산 추진 구조(Distributed Propulsion Arrangement)를 사용할 수 있다. 이러한 시스템에서 실제 기하학적 구조를 정확하게 표현하지 않고 단순화된 고정 계수를 사용하면 체계적인 힘과 모멘트 오차가 발생할 수 있다.

모터 명령은 요구된 로터 추력과 액추에이터 입력 사이의 관계에도 영향을 받는다. 일반적으로 로터 추력은 회전 속도에 대해 비선형적이며, 전자식 속도 제어기(Electronic Speed Controller, ESC)의 명령에도 추가적인 비선형성, 데드존(Dead Zone), 동적 지연(Dynamic Delay)이 존재할 수 있다. 추진 모델(Propulsion Model) 또는 보정된 추력 매핑(Calibrated Thrust Mapping)을 이용하여 목표 로터 힘을 모터 속도 또는 ESC 명령으로 변환하면 단순히 명령과 추력 사이의 선형 관계를 가정하는 대신 물리적으로 의미 있는 힘 단위에서 믹싱 계층을 운용할 수 있다.

집단 추력과 회전 모멘트는 동일한 액추에이터 제어 권한을 공유한다. 항공기가 최대 화물에 가까운 상태에서 이미 대부분의 추력을 기체 중량을 지지하는 데 사용하고 있다면 롤, 피치, 요 또는 수직 가속에 사용할 수 있는 추진 여유(Propulsion Margin)는 제한된다. 할당기는 이러한 결합 제약(Coupled Constraint)을 인식해야 한다. 믹싱 이후 개별 모터 명령을 단순히 클리핑(Clipping)하면 요구된 힘과 모멘트의 조합이 왜곡되어 자세 안정성이 저하될 수 있다.

따라서 포화(Saturation) 상태에서는 제어 우선순위(Control Prioritization)가 중요하다. 일부 운용 조건에서는 요 또는 집단 추력 명령을 완벽하게 만족시키는 것보다 롤과 피치 안정성을 유지하는 것이 더 중요할 수 있다. 할당 전략은 서로 다른 제어 목표에 우선순위 또는 가중치(Weight)를 부여하여 제한된 액추에이터 제어 권한이 가장 안전에 중요한 기능에 우선 사용되도록 할 수 있다. 구체적인 우선순위 정책은 기체 동역학, 비행 단계 및 안전 요구사항과 일관성을 유지해야 한다.

요 제어(Yaw Control)는 추진 시스템 구성에 특히 크게 의존한다. 일반적인 멀티로터에서는 시계 방향(Clockwise)과 반시계 방향(Counterclockwise)으로 회전하는 로터의 반작용 토크 사이에 불균형을 만들어 요 모멘트를 생성한다. 따라서 전체 추력이 거의 일정하게 유지되는 경우에도 요 동작을 생성하면 모터 부하 분포가 변한다. 모터가 포화 상태에 접근하면 사용 가능한 요 제어 권한이 크게 감소할 수 있으므로 제어기는 요 각속도 명령(Yaw-Rate Command)을 제한하거나 일시적인 요 추종 오차를 허용해야 할 수 있다.

화물 적재로 발생하는 무게중심 변화(Center-of-Gravity Change)는 추진 장치와 기체 질량 중심 사이의 유효 모멘트 암(Effective Moment Arm)을 변화시킨다. 따라서 고정된 기준 무게중심을 중심으로 구성한 믹싱 행렬은 무거운 화물이 비대칭적으로 배치될 경우 정확도가 저하될 수 있다. 할당기는 갱신된 무게중심 정보 또는 트림 보정(Trim Correction)을 적용하여 자세 제어기가 할당 편향(Allocation Bias)을 지속적으로 보상하지 않고도 필요한 모멘트를 생성할 수 있도록 해야 한다.

화물 변화(Payload Variation)는 추진 시스템의 운용점(Operating Point)도 변화시킨다. 무화물 항공기는 호버링 상태에서 상당한 추력 여유를 가질 수 있지만 완전 적재된 기체는 연속 모터 한계에 훨씬 가까운 영역에서 운용될 수 있다. 따라서 할당 과정에서는 순간적인 명령 한계뿐만 아니라 지속 가능한 추진 성능(Sustained Propulsion Capability)도 고려해야 한다. 개별 명령이 단시간 허용 범위 내에 있더라도 모터, 인버터, 배터리 또는 열적 한계 부근에서 지속적으로 운용하면 신뢰성이 저하될 수 있다.

변화율 제한(Rate Limiting)을 적용하면 비현실적인 액추에이터 전환을 방지할 수 있다. 비행 제어기는 외란이 발생할 때 요구 모멘트를 빠르게 변경할 수 있지만 모터와 프로펠러는 가속하거나 감속하는 데 유한한 시간이 필요하다. 할당기가 액추에이터의 순간적인 응답을 가정하면 실제 항공기 모멘트가 명령된 값과 크게 달라질 수 있다. 액추에이터 동역학(Actuator Dynamics), 명령 변화율 제한(Command Slew Limit), 달성 가능한 변화율 제약(Achievable-Rate Constraint)을 반영하면 제어 모델과 실제 추진 시스템 응답 사이의 일관성을 향상시킬 수 있다.

모터 고장(Motor Failure)은 더욱 복잡한 할당 문제를 발생시킨다. 하나의 추진 장치를 사용할 수 없게 되거나 추진 효율이 저하되면 기존의 믹싱 행렬은 더 이상 실제 시스템을 정확하게 표현하지 못한다. 고장 인지 할당기(Fault-Aware Allocator)는 고장난 액추에이터를 제거하거나 성능을 제한하고 남아 있는 모터를 이용하여 달성 가능한 힘과 모멘트 분포를 다시 계산할 수 있다. 완전한 자세 및 고도 제어 유지 가능 여부는 추진 시스템의 이중화, 기하학적 구조, 사용 가능한 추력 여유 및 고장난 액추에이터의 위치에 따라 달라진다.

부분적인 액추에이터 성능 저하(Partial Actuator Degradation)도 고려해야 한다. 고장이 항상 완전한 작동 또는 완전한 정지의 이진 상태로 발생하는 것은 아니기 때문이다. 모터, 프로펠러, ESC 또는 전력 채널이 계속 동작하면서 명령된 값보다 낮은 추력을 생성할 수 있다. 이러한 감소된 성능은 효과도 계수(Effectiveness Coefficient)를 이용하여 할당 모델에 표현할 수 있다. 이후 상태 모니터링 정보(Health-Monitoring Information)를 이용하여 할당 행렬 또는 액추에이터 제한값을 수정함으로써 남아 있는 추진 장치가 사용 가능한 여유 범위에서 이를 보상하도록 할 수 있다.

할당 잔차(Allocation Residual)는 중요한 진단 정보를 제공한다. 모터 명령을 계산한 이후 시스템은 실제로 달성 가능한 힘과 모멘트를 추정하고 이를 요구된 제어 벡터(Control Vector)와 비교할 수 있다. 큰 잔차는 포화, 고장, 기하학적 구조 또는 기타 제약으로 인해 요구 명령을 완전히 구현할 수 없음을 의미한다. 이러한 정보는 할당 계층 내부에 숨겨두는 대신 상위 제어기와 안전 기능에 전달해야 한다.

이러한 피드백은 안티와인드업(Anti-Windup) 동작에도 유용하다. 자세 또는 고도 제어기가 모터 할당기가 생성할 수 없는 제어력을 요구하는 경우 적분기는 해당 명령이 정상적으로 실행되고 있는 것처럼 계속 오차를 누적해서는 안 된다. 달성된 힘 추정값(Achieved-Force Estimate), 포화 플래그(Saturation Flag), 할당 잔차 등을 제어 구조의 상위 계층으로 다시 전달할 수 있다. 이를 통해 제어기 계층과 액추에이터 계층 전체에서 조정된 포화 관리(Coordinated Saturation Management)를 구현할 수 있다.

전력 시스템 제약(Power-System Constraint)은 대형 화물 무인항공기에서 특히 중요하다. 여러 모터가 각각의 한계 내에서 동작하더라도 이들의 전체 전력 요구량이 배터리, 발전기, 인버터 또는 전력 분배 시스템의 성능을 초과할 수 있다. 따라서 모터 할당 과정에서는 개별 액추에이터 한계뿐만 아니라 전체 전력 또는 전류 제한(Total Power or Current Constraint)을 포함할 수 있다. 이러한 조정을 통해 공기역학적으로는 유효한 명령이 전기적으로 실행 불가능한 명령으로 변환되는 것을 방지할 수 있다.

실시간 실행(Real-Time Execution)은 할당 알고리즘에 엄격한 요구조건을 부과한다. 고정 믹싱 행렬은 계산 비용이 낮지만 제약 최적화는 더 높은 유연성을 제공하는 대신 결정론적인 제어 주기 내에서 반드시 계산을 완료해야 한다. 최악 조건(Worst-Case Condition)에서 솔버 실행 시간(Solver Execution Time), 수렴 특성, 수치적 조건(Numerical Conditioning), 폴백 로직(Fallback Logic)을 평가해야 한다. 아무리 정교한 할당기라도 비행 제어 컴퓨터에서 실행 시간을 보장할 수 없다면 실제 시스템에 적합하지 않다.

추진 시스템의 기하학적 구조로 인해 조건이 좋지 않은 할당 행렬(Poorly Conditioned Allocation Matrix)이 형성될 수 있으므로 수치적 견고성(Numerical Robustness)도 중요하다. 이러한 경우 작은 명령이나 모델링 오차가 지나치게 큰 액추에이터 변화를 발생시킬 수 있다. 행렬 스케일링(Matrix Scaling), 조건 상태 모니터링(Condition Monitoring), 정규화(Regularization), 액추에이터 가중치(Actuator Weighting), 신중한 기하학적 설계를 통해 견고성을 향상시킬 수 있다. 할당 모델이 특이(Singular)하거나 일관되지 않은 상태가 되면 제어되지 않은 명령을 출력하는 대신 소프트웨어가 잘못된 구성을 감지해야 한다.

검증(Verification)에서는 명령된 힘과 모멘트가 실제 추진 응답으로 변환되는 전체 경로를 시험해야 한다. 시뮬레이션과 소프트웨어 인 더 루프(Software-in-the-Loop, SIL), 하드웨어 인 더 루프(Hardware-in-the-Loop, HIL) 환경에서 정상 믹싱, 포화, 비대칭 적재, 무게중심 이동, 모터 비선형성, 명령 지연, 부분적인 성능 저하 및 완전한 모터 고장을 시험할 수 있다. 시험에서는 할당 오차, 잔여 제어 권한, 액추에이터 사용률, 타이밍 성능 및 정상 구성과 성능 저하 구성 사이의 전환 특성을 평가해야 한다.

믹싱 및 모터 할당 계층(Mixing and Motor-Allocation Layer)은 궁극적으로 비행 제어기의 수학적 목표를 물리적으로 실행 가능한 추진 동작으로 변환한다. 이를 위해 기체 형상, 액추에이터 모델, 포화 처리, 제어 우선순위, 화물 영향, 전력 제약 및 고장 허용(Fault Tolerance)을 통합하여 설계해야 한다. 전체 비행 제어 구조에서 이 계층은 이후의 부하 적응 제어(Load-Adaptive Control), 바람 외란 제거, 비행 모드 관리 및 비행 제어 시스템 검증(FCS Verification)이 동작할 수 있는 액추에이터 수준의 기반을 제공한다.

##  

## 03.07. Load Change Adaptive Control Cargo Weight [w/Code]

![](images/image7.png){width="7.268055555555556in" height="7.268055555555556in"}

Load-change adaptive control allows a cargo UAV to maintain predictable flight behavior as payload weight changes between missions or during operation. Cargo mass directly affects total vehicle weight, hover thrust, translational acceleration, rotational response, propulsion margin, and energy consumption. A controller designed only for one nominal loading condition may therefore become sluggish, aggressive, inefficient, or saturated when the aircraft operates across a wide payload range.

The fundamental effect of payload variation appears in the force relationship between vehicle mass and acceleration. For the same commanded acceleration, a heavier aircraft requires proportionally greater force. Vertical control is particularly sensitive because propulsion must continuously balance gravity before producing additional climb acceleration. If the controller assumes an incorrect mass, its feedforward thrust prediction becomes inaccurate and feedback loops must compensate for the resulting systematic error.

Payload variation can also modify rotational dynamics. Cargo contributes to the aircraft's moments of inertia according to both its mass and its location relative to the vehicle center of gravity. A heavy load concentrated near the center may mainly change total mass, whereas cargo positioned farther from the center can significantly increase roll, pitch, or yaw inertia. Consequently, identical torque commands can produce different angular accelerations under different loading configurations.

Center-of-gravity displacement introduces another important control effect. An asymmetric cargo arrangement can shift the mass center away from the nominal geometric reference used by the propulsion model. The aircraft may then require continuous trim moments simply to maintain level flight. If this change is ignored, attitude-controller integrators must compensate continuously, reducing available control margin and potentially producing uneven motor loading or premature actuator saturation.

Adaptive control begins with obtaining a useful estimate of the current vehicle configuration. Payload weight may be provided by a cargo-management system, measured through loading equipment, inferred from propulsion and acceleration response, or estimated online from flight data. The control system should distinguish between trusted externally supplied mass information and dynamically estimated values because their uncertainty, update rate, and potential failure modes are different.

A mass estimate can be incorporated directly into vertical-thrust feedforward. The expected force required to balance gravity is approximately proportional to total aircraft mass, so updating the mass model immediately improves hover-thrust prediction. The altitude and vertical-speed controllers can then operate primarily on residual errors instead of relying on integral action to discover the new hover operating point after every payload change.

Horizontal control can similarly use estimated mass when converting desired acceleration into force demand. The position and velocity controllers determine the translational acceleration required to follow a trajectory, while the adaptive model calculates the approximate force necessary for the current vehicle weight. Feedback remains responsible for correcting modeling errors, aerodynamic effects, and disturbances, but improved feedforward reduces transient tracking error when payload varies significantly.

Attitude-control adaptation requires information about inertia rather than mass alone. If the cargo configuration is known, approximate inertia parameters can be calculated from payload geometry and mounting position. Controller gains or model-based terms can then be scheduled according to the expected rotational dynamics. When detailed cargo geometry is unavailable, conservative gain scheduling based on payload classes may provide a simpler and more verifiable alternative.

Gain scheduling is one practical form of adaptive control for cargo UAVs. Controller parameters can be defined for several validated loading regions, such as light, medium, and heavy configurations, and interpolated according to measured or estimated mass. This approach preserves much of the transparency of conventional fixed-gain control while extending acceptable performance across a wider operating envelope without requiring unrestricted online modification of safety-critical control laws.

Online parameter estimation provides a more dynamic alternative. The system can compare commanded force or moment with measured acceleration and infer effective mass, inertia, or actuator effectiveness. Recursive estimation techniques can gradually update these parameters during flight. However, aircraft acceleration is also affected by wind, aerodynamic forces, sensor noise, and propulsion uncertainty, so adaptation must avoid interpreting temporary disturbances as permanent changes in vehicle properties.

Adaptation rate therefore requires careful limitation. A payload attached to the aircraft usually changes slowly or only at known mission events, while wind and turbulence can change rapidly. If mass estimates are allowed to respond too quickly, the estimator may incorrectly absorb environmental disturbances into the vehicle model. Filtering, bounded parameter ranges, persistence checks, and confidence measures can separate genuine load changes from transient external forces.

Cargo pickup or release creates a more abrupt operating transition. When a payload is attached, released, or transferred, total mass and possibly center of gravity can change within a short period. If the event is known in advance, the flight-control system can coordinate controller parameters, thrust feedforward, motor allocation, and trajectory limits with the cargo-management system. Such event-based adaptation can respond more reliably than waiting for feedback errors to reveal the configuration change.

A sudden cargo release requires particular attention because the thrust level appropriate for the loaded aircraft may be excessive immediately after mass decreases. Without rapid compensation, the aircraft can accelerate upward and produce a significant altitude transient. Updating the mass-dependent feedforward term at the release event, while applying appropriate command smoothing, allows the vertical controller to transition toward the new equilibrium without generating an unnecessarily large response.

Payload increase has the opposite effect. If cargo is acquired while the propulsion command remains unchanged, vertical acceleration may decrease or the aircraft may begin descending. The controller must increase collective thrust while preserving attitude authority and avoiding abrupt structural loading. Before accepting a payload, the mission system should also confirm that the resulting mass remains within validated propulsion, structural, battery, and flight-envelope limits.

Adaptive control cannot create propulsion capability that does not physically exist. As payload increases, hover thrust consumes a larger portion of the available actuator range, leaving less reserve for climb, horizontal acceleration, disturbance rejection, and fault recovery. The adaptive system should therefore modify not only controller parameters but also operational limits such as maximum climb rate, horizontal acceleration, tilt angle, and maneuver aggressiveness according to remaining control authority.

Motor allocation should be coordinated with load adaptation because changing weight and center of gravity alter the required propulsion distribution. Updated mass and center-of-gravity information can improve trim-force and moment calculations, while the allocator can report remaining thrust margin and actuator saturation back to the adaptive controller. This interaction prevents adaptation from demanding performance that the propulsion system cannot achieve under the current load.

Suspended cargo introduces dynamics that cannot be represented by a simple rigid increase in aircraft mass. A load hanging below the vehicle can swing like a pendulum, creating time-varying forces and moments. Aggressive acceleration may amplify this motion and degrade position or attitude stability. The control system may therefore reduce acceleration and jerk limits, introduce load-motion damping, or use additional payload-state measurements when suspended-load operations are part of the mission.

Adaptation must remain bounded by safety constraints. Estimated mass, inertia, center-of-gravity position, controller gains, and feedforward terms should remain within validated ranges. Implausible parameter changes should be rejected or frozen rather than immediately applied to flight-critical control. Confidence monitoring is especially important because a faulty payload sensor or estimator could otherwise modify control parameters in a direction that reduces stability.

Fallback behavior should be defined when load information becomes unavailable or inconsistent. The controller may retain the last validated parameters, transition to a conservative configuration, reduce maneuver limits, or request mission termination depending on the uncertainty and available propulsion margin. Adaptive functionality should therefore enhance nominal performance without making basic stabilization dependent on continuously perfect payload information.

Verification must cover the complete validated payload envelope rather than a single nominal configuration. Simulation and SIL/HIL testing can vary total mass, center-of-gravity position, inertia, cargo pickup and release timing, propulsion capability, wind, sensor uncertainty, and estimator errors. Tests should examine altitude transients, trajectory tracking, attitude response, actuator saturation, adaptation convergence, parameter bounds, and behavior when load information is incorrect.

Flight testing should expand payload conditions progressively from known baseline configurations toward maximum approved loading. Hover, climb, descent, acceleration, braking, turns, disturbance recovery, and landing can be repeated at representative mass and center-of-gravity combinations. Particular attention should be given to transitions between configurations because stable performance at two steady loading conditions does not automatically guarantee a safe transition between them.

Load-change adaptive control ultimately coordinates the flight-control system with the physical configuration of the cargo aircraft. Mass adaptation improves thrust and translational-force prediction, inertia adaptation preserves rotational response, center-of-gravity compensation reduces trim error, and load-aware limits protect remaining actuator authority. In the chapter structure, this function extends the basic attitude, altitude, position, and motor-allocation controllers before wind-disturbance rejection and flight-mode management are addressed.

부하 변화 적응 제어(Load-Change Adaptive Control)는 임무 간 또는 운용 중 화물 중량이 변화하더라도 화물 무인항공기(Cargo UAV)가 예측 가능한 비행 특성을 유지하도록 한다. 화물 질량(Cargo Mass)은 전체 기체 중량, 호버 추력(Hover Thrust), 병진 가속도(Translational Acceleration), 회전 응답, 추진 여유(Propulsion Margin), 에너지 소비에 직접적인 영향을 준다. 따라서 하나의 기준 적재 조건만을 대상으로 설계된 제어기는 넓은 화물 범위에서 운용될 경우 응답이 느려지거나 지나치게 민감해지고, 효율이 저하되거나 포화(Saturation)에 도달할 수 있다.

화물 변화의 기본적인 영향은 기체 질량과 가속도 사이의 힘 관계에서 나타난다. 동일한 명령 가속도를 생성하기 위해 무거운 항공기는 비례적으로 더 큰 힘을 필요로 한다. 특히 수직 제어(Vertical Control)는 추진 시스템이 추가적인 상승 가속도를 생성하기 전에 지속적으로 중력을 상쇄해야 하므로 화물 변화에 매우 민감하다. 제어기가 잘못된 질량을 가정하면 피드포워드 추력 예측(Feedforward Thrust Prediction)이 부정확해지고, 피드백 루프가 이에 따른 체계적인 오차를 보상해야 한다.

화물 변화는 회전 동역학(Rotational Dynamics)도 변화시킬 수 있다. 화물은 자체 질량뿐만 아니라 기체 무게중심(Center of Gravity)에 대한 위치에 따라 항공기의 관성 모멘트(Moment of Inertia)에 영향을 준다. 무거운 화물이 중심 부근에 집중되어 있으면 주로 전체 질량이 증가하지만 중심에서 멀리 배치된 화물은 롤(Roll), 피치(Pitch), 요(Yaw) 관성을 크게 증가시킬 수 있다. 따라서 동일한 토크 명령이라도 서로 다른 적재 구성에서는 서로 다른 각가속도(Angular Acceleration)를 발생시킬 수 있다.

무게중심 변위(Center-of-Gravity Displacement)는 또 다른 중요한 제어 영향을 발생시킨다. 비대칭 화물 배치는 추진 모델에서 사용하는 기준 기하학적 위치로부터 질량 중심을 이동시킬 수 있다. 이 경우 항공기는 단순히 수평 비행 자세를 유지하기 위해서도 지속적인 트림 모멘트(Trim Moment)를 필요로 할 수 있다. 이러한 변화를 무시하면 자세 제어기의 적분기(Integrator)가 지속적으로 보상해야 하므로 사용 가능한 제어 여유가 감소하고 모터 부하 불균형이나 조기 액추에이터 포화가 발생할 수 있다.

적응 제어(Adaptive Control)는 현재 기체 구성에 대한 유용한 추정값을 확보하는 것에서 시작한다. 화물 중량은 화물 관리 시스템(Cargo-Management System)에서 제공하거나 적재 장비를 통해 측정할 수 있으며, 추진 시스템과 가속도 응답으로부터 추론하거나 비행 데이터를 이용하여 온라인으로 추정할 수도 있다. 제어 시스템은 신뢰할 수 있는 외부 시스템에서 제공된 질량 정보와 동적으로 추정된 값을 구분해야 한다. 두 정보는 불확실성, 갱신 주기 및 잠재적인 고장 형태가 서로 다르기 때문이다.

질량 추정값(Mass Estimate)은 수직 추력 피드포워드(Vertical-Thrust Feedforward)에 직접 적용할 수 있다. 중력을 상쇄하기 위해 필요한 예상 힘은 전체 항공기 질량에 거의 비례하므로 질량 모델을 갱신하면 호버 추력 예측을 즉시 개선할 수 있다. 이후 고도 및 수직 속도 제어기는 화물 변화가 발생할 때마다 적분 동작을 이용하여 새로운 호버 운용점(Hover Operating Point)을 찾아야 하는 대신 주로 남아 있는 잔여 오차를 보정하도록 동작할 수 있다.

수평 제어에서도 목표 가속도를 힘 요구량으로 변환할 때 추정된 질량을 사용할 수 있다. 위치 및 속도 제어기(Position and Velocity Controller)는 궤적을 추종하는 데 필요한 병진 가속도를 결정하고, 적응 모델(Adaptive Model)은 현재 기체 중량을 기준으로 필요한 힘을 계산한다. 피드백 제어는 모델링 오차, 공기역학적 영향 및 외란을 계속 보정하지만 향상된 피드포워드는 화물이 크게 변화할 때 발생하는 과도 추종 오차(Transient Tracking Error)를 감소시킨다.

자세 제어 적응(Attitude-Control Adaptation)에는 질량뿐만 아니라 관성에 대한 정보가 필요하다. 화물 구성을 알고 있다면 화물 형상과 장착 위치를 이용하여 대략적인 관성 매개변수(Inertia Parameter)를 계산할 수 있다. 이후 예상되는 회전 동역학에 따라 제어기 이득 또는 모델 기반 항을 조정할 수 있다. 상세한 화물 형상 정보를 사용할 수 없는 경우에는 화물 등급(Payload Class)에 따른 보수적인 이득 스케줄링(Gain Scheduling)이 보다 단순하고 검증하기 쉬운 대안이 될 수 있다.

이득 스케줄링은 화물 무인항공기에 적용할 수 있는 실용적인 적응 제어 방식 중 하나이다. 경량(Light), 중간(Medium), 중량(Heavy) 구성과 같이 검증된 여러 적재 영역에 대해 제어기 매개변수를 정의하고 측정 또는 추정된 질량에 따라 이를 보간(Interpolation)할 수 있다. 이러한 방식은 기존 고정 이득 제어(Fixed-Gain Control)의 투명성을 상당 부분 유지하면서 안전 중요 제어 법칙을 제한 없이 온라인으로 변경하지 않고도 더 넓은 운용 영역에서 적절한 성능을 확보할 수 있다.

온라인 매개변수 추정(Online Parameter Estimation)은 보다 동적인 대안을 제공한다. 시스템은 명령된 힘 또는 모멘트와 측정된 가속도를 비교하여 유효 질량(Effective Mass), 관성 또는 액추에이터 효과도(Actuator Effectiveness)를 추정할 수 있다. 재귀적 추정 기법(Recursive Estimation Technique)을 이용하면 비행 중 이러한 매개변수를 점진적으로 갱신할 수 있다. 그러나 항공기 가속도는 바람, 공기역학적 힘, 센서 잡음 및 추진 불확실성에도 영향을 받으므로 일시적인 외란을 기체 특성의 영구적인 변화로 잘못 해석하지 않도록 해야 한다.

따라서 적응 속도(Adaptation Rate)를 신중하게 제한해야 한다. 항공기에 장착된 화물은 일반적으로 천천히 변화하거나 알려진 임무 이벤트에서만 변경되는 반면 바람과 난류는 빠르게 변화할 수 있다. 질량 추정값이 지나치게 빠르게 변화하도록 허용하면 추정기가 환경 외란을 기체 모델의 변화로 잘못 흡수할 수 있다. 필터링, 제한된 매개변수 범위(Bounded Parameter Range), 지속성 검사(Persistence Check), 신뢰도 지표(Confidence Measure)를 이용하여 실제 화물 변화와 일시적인 외력을 구분할 수 있다.

화물 픽업(Payload Pickup) 또는 방출(Release)은 더욱 급격한 운용 전환을 발생시킨다. 화물을 장착하거나 방출 또는 이전하면 전체 질량과 경우에 따라 무게중심이 짧은 시간 안에 변화할 수 있다. 이러한 이벤트를 사전에 알고 있다면 비행 제어 시스템은 화물 관리 시스템과 연동하여 제어기 매개변수, 추력 피드포워드, 모터 할당(Motor Allocation), 궤적 제한을 조정할 수 있다. 이러한 이벤트 기반 적응(Event-Based Adaptation)은 피드백 오차를 통해 기체 구성 변화를 감지할 때까지 기다리는 방식보다 더욱 신뢰성 있게 대응할 수 있다.

갑작스러운 화물 방출(Sudden Cargo Release)은 특별한 주의가 필요하다. 화물 탑재 상태에 적합했던 추력 수준은 질량이 감소한 직후에는 과도할 수 있다. 신속한 보상이 이루어지지 않으면 항공기가 위쪽으로 가속하여 상당한 고도 과도 응답(Altitude Transient)을 발생시킬 수 있다. 화물 방출 이벤트와 동시에 질량 의존 피드포워드 항(Mass-Dependent Feedforward Term)을 갱신하고 적절한 명령 평활화(Command Smoothing)를 적용하면 불필요하게 큰 응답 없이 수직 제어기를 새로운 평형 상태로 전환할 수 있다.

화물 증가(Payload Increase)는 반대의 영향을 발생시킨다. 화물을 획득한 이후에도 추진 명령이 동일하게 유지되면 수직 가속도가 감소하거나 항공기가 하강하기 시작할 수 있다. 제어기는 자세 제어 권한을 유지하고 급격한 구조 하중을 방지하면서 집단 추력(Collective Thrust)을 증가시켜야 한다. 또한 화물을 인수하기 전에 임무 시스템은 증가된 질량이 검증된 추진, 구조, 배터리 및 비행 영역(Flight Envelope)의 한계 내에 있는지를 확인해야 한다.

적응 제어는 물리적으로 존재하지 않는 추진 능력을 만들어낼 수 없다. 화물 중량이 증가하면 호버 추력이 사용 가능한 액추에이터 범위의 더 큰 부분을 차지하므로 상승, 수평 가속, 외란 제거 및 고장 복구에 사용할 수 있는 여유가 감소한다. 따라서 적응 시스템은 제어기 매개변수뿐만 아니라 남아 있는 제어 권한에 따라 최대 상승률, 수평 가속도, 기울기 각도(Tilt Angle), 기동 공격성(Maneuver Aggressiveness)과 같은 운용 제한도 조정해야 한다.

모터 할당은 중량 및 무게중심 변화에 따라 필요한 추진력 분포가 달라지므로 부하 적응(Load Adaptation)과 조정되어야 한다. 갱신된 질량과 무게중심 정보를 이용하면 트림 힘과 모멘트 계산을 개선할 수 있으며, 할당기는 남아 있는 추력 여유와 액추에이터 포화 정보를 적응 제어기에 다시 전달할 수 있다. 이러한 상호작용은 현재 화물 조건에서 추진 시스템이 달성할 수 없는 성능을 적응 제어기가 요구하는 것을 방지한다.

현수 화물(Suspended Cargo)은 단순한 강체 질량 증가만으로 표현할 수 없는 동역학을 발생시킨다. 기체 아래에 매달린 화물은 진자(Pendulum)처럼 흔들리면서 시간에 따라 변화하는 힘과 모멘트를 생성할 수 있다. 공격적인 가속 명령은 이러한 운동을 증폭시켜 위치 또는 자세 안정성을 저하시킬 수 있다. 따라서 현수 화물 운용이 임무에 포함되는 경우 제어 시스템은 가속도와 저크(Jerk) 제한을 감소시키거나 화물 운동 감쇠(Load-Motion Damping)를 적용하고 추가적인 화물 상태 측정값을 사용할 수 있다.

적응 동작은 안전 제약(Safety Constraint) 범위 내에서 제한되어야 한다. 추정된 질량, 관성, 무게중심 위치, 제어기 이득 및 피드포워드 항은 검증된 범위를 벗어나지 않아야 한다. 비현실적인 매개변수 변화가 감지되면 이를 비행 중요 제어(Flight-Critical Control)에 즉시 적용하기보다 거부하거나 현재 값을 고정해야 한다. 특히 고장난 화물 센서나 추정기가 안정성을 감소시키는 방향으로 제어 매개변수를 변경할 수 있으므로 신뢰도 모니터링(Confidence Monitoring)이 중요하다.

화물 정보가 사용할 수 없거나 서로 일관되지 않은 경우를 위한 폴백 동작(Fallback Behavior)도 정의해야 한다. 제어기는 마지막으로 검증된 매개변수를 유지하거나 보수적인 구성으로 전환하고, 기동 제한을 감소시키거나 불확실성과 사용 가능한 추진 여유에 따라 임무 종료를 요청할 수 있다. 따라서 적응 기능은 정상 운용 성능을 향상시키면서도 기본적인 안정화 기능이 지속적으로 완벽한 화물 정보에 의존하지 않도록 설계되어야 한다.

검증(Verification)은 하나의 기준 구성만이 아니라 검증된 전체 화물 운용 영역(Payload Envelope)을 포함해야 한다. 시뮬레이션과 소프트웨어 인 더 루프(Software-in-the-Loop, SIL), 하드웨어 인 더 루프(Hardware-in-the-Loop, HIL) 시험을 통해 전체 질량, 무게중심 위치, 관성, 화물 픽업 및 방출 시점, 추진 능력, 바람, 센서 불확실성 및 추정기 오차를 변화시킬 수 있다. 시험에서는 고도 과도 응답, 궤적 추종, 자세 응답, 액추에이터 포화, 적응 수렴(Adaptation Convergence), 매개변수 한계 및 잘못된 화물 정보가 입력된 경우의 동작을 평가해야 한다.

비행 시험(Flight Testing)은 알려진 기준 구성에서 시작하여 최대 승인 적재 조건까지 화물 조건을 점진적으로 확장해야 한다. 대표적인 질량 및 무게중심 조합에서 호버링, 상승, 하강, 가속, 제동, 선회, 외란 복구 및 착륙 시험을 반복할 수 있다. 두 개의 정상 적재 조건에서 각각 안정적인 성능을 확보했다고 해서 두 구성 사이의 전환 과정까지 자동으로 안전한 것은 아니므로 구성 전환 과정에 특히 주의를 기울여야 한다.

부하 변화 적응 제어는 궁극적으로 비행 제어 시스템을 화물 항공기의 실제 물리적 구성과 연계한다. 질량 적응(Mass Adaptation)은 추력 및 병진력 예측을 개선하고, 관성 적응(Inertia Adaptation)은 회전 응답을 유지하며, 무게중심 보상(Center-of-Gravity Compensation)은 트림 오차를 감소시키고, 부하 인지 제한(Load-Aware Limit)은 남아 있는 액추에이터 제어 권한을 보호한다. 전체 비행 제어 구조에서 이 기능은 기본 자세, 고도, 위치 및 모터 할당 제어를 확장하며 이후의 바람 외란 제거(Wind-Disturbance Rejection)와 비행 모드 관리(Flight-Mode Management)를 위한 기반을 제공한다.

##  

## 03.08. Wind Disturbance Rejection and Estimation [w/Code]

![](images/image8.png){width="7.268055555555556in" height="7.268055555555556in"}

Wind disturbance rejection allows a cargo UAV to preserve attitude, altitude, velocity, and position despite external aerodynamic forces generated by steady wind, gusts, turbulence, and localized airflow. Because large cargo aircraft present substantial surface area and often operate with limited thrust margin, wind can produce significant translational and rotational disturbances. The flight-control system must therefore estimate disturbance effects and reject them without creating excessive actuator activity.

Wind influences the aircraft through both aerodynamic force and moment. A steady crosswind can produce continuous horizontal force, while gusts create rapid changes in acceleration and attitude. Vertical air motion can alter climb or descent rate, and asymmetric airflow around the fuselage, payload, or propulsion structure can generate rotational moments. These effects vary with vehicle geometry, airspeed, payload configuration, and the relative direction of the surrounding airflow.

The basic feedback-control loops already provide an important level of disturbance rejection. The angular-rate controller suppresses unexpected rotational motion, the attitude controller restores commanded orientation, the velocity controller counters translational deviations, and the position controller removes accumulated displacement. However, relying entirely on feedback requires an error to develop before corrective action occurs, which can produce larger deviations during strong or rapidly changing wind.

Integral control is particularly useful for rejecting approximately constant disturbances. During position hold in steady wind, the vehicle may need to maintain a persistent tilt and horizontal thrust component to remain stationary. The velocity or position integrator can gradually develop the required compensation. Integral action must remain bounded, however, because strong wind may demand more force than the propulsion system can provide and cause windup when control authority becomes saturated.

Explicit disturbance estimation can improve performance by identifying the external force acting on the aircraft before large tracking errors accumulate. A disturbance observer can compare expected vehicle acceleration from commanded thrust with measured or estimated acceleration. The unexplained difference can be interpreted, within model uncertainty, as an external disturbance force. This estimate can then be used as a feedforward compensation term in the translational controller.

Wind estimation and disturbance estimation are related but not identical. Wind velocity describes the motion of the surrounding air relative to an Earth-fixed frame, whereas disturbance force represents the aerodynamic effect that this airflow produces on the vehicle. Converting between them requires an aerodynamic model containing parameters such as projected area and drag characteristics. For many control applications, estimating disturbance force directly can therefore be more useful than reconstructing precise atmospheric wind velocity.

An extended state observer or augmented state estimator can represent external disturbances as additional slowly varying states. These states are propagated together with the vehicle dynamics and corrected using measured motion. When the predicted acceleration differs consistently from observed acceleration, the estimator adjusts the disturbance state. Appropriate process noise determines how quickly the estimate can follow changing wind without responding excessively to sensor noise or modeling errors.

The estimator must distinguish wind from errors in mass, thrust calibration, attitude, or sensor bias. If the vehicle mass is underestimated, for example, measured acceleration may differ from the model even in calm air. Similarly, propulsion degradation can appear mathematically similar to an opposing external force. Wind-disturbance estimation should therefore operate consistently with load-adaptive control, actuator-health monitoring, and state estimation rather than treating every model residual as atmospheric disturbance.

Accurate attitude information is essential because thrust must be transformed correctly between the body and navigation coordinate frames before external force can be inferred. A small attitude error can create an apparent horizontal acceleration that resembles wind. Accelerometer bias and vibration can introduce similar effects. Filtering, covariance information, estimator-health monitoring, and bounded disturbance states help prevent these imperfections from producing unrealistic wind compensation.

GNSS-derived ground velocity combined with an airspeed measurement can provide another basis for estimating wind velocity. The difference between the aircraft velocity relative to the ground and its velocity relative to the surrounding air contains information about the wind vector. This approach is more direct for aircraft equipped with reliable air-data sensors, but low-speed multirotor operation, rotor downwash, sensor placement, and disturbed flow can make accurate airspeed measurement difficult.

Cargo configuration can significantly change wind sensitivity. A large external payload may increase projected area and aerodynamic drag without producing a proportional increase in mass. The same wind can therefore generate different acceleration depending on cargo geometry. Asymmetric cargo can also produce aerodynamic moments. A disturbance estimator based only on a fixed clean-airframe model should account for this uncertainty rather than assuming identical aerodynamic behavior for every mission configuration.

Suspended loads introduce an additional challenge because wind can act independently on both the aircraft and the cargo. Load swing may then produce oscillatory forces that resemble changing wind disturbances when observed only through vehicle motion. Aggressive disturbance compensation can unintentionally excite this pendulum behavior. Filtering and bandwidth separation should prevent the wind-rejection loop from attempting to cancel every high-frequency load-induced acceleration.

Feedforward disturbance compensation can be applied by subtracting the estimated external force from the commanded vehicle force. If a crosswind is estimated to push the aircraft eastward, the controller can generate an opposing westward force before a large position error develops. The required force is translated into an appropriate thrust-vector orientation and collective thrust while the normal feedback loops correct residual estimation and modeling errors.

Disturbance compensation must respect attitude and propulsion limits. Counteracting strong horizontal wind requires the aircraft to tilt, which reduces the vertical component of available thrust. A heavily loaded cargo UAV may therefore reach its tilt or thrust limit earlier than an unloaded aircraft. The controller should prioritize stability and altitude preservation when full horizontal disturbance rejection is physically impossible rather than continuously demanding an unattainable position-hold condition.

Wind rejection should consequently interact with control-authority monitoring. Available thrust margin, maximum tilt, motor saturation, battery condition, and payload mass determine how much disturbance can be rejected safely. When estimated wind demand approaches these limits, the flight-control system can reduce trajectory aggressiveness, increase position tolerance, restrict mission functions, or inform higher-level flight-mode management that the requested operation is becoming unsustainable.

Gust rejection requires a different balance from steady-wind compensation. Rapid gusts contain higher-frequency components that cannot always be followed effectively by a slowly adapting disturbance estimator. Fast attitude and rate feedback should handle much of the immediate response, while the disturbance estimator captures the lower-frequency component that persists afterward. Attempting to estimate every rapid fluctuation can amplify sensor noise and create unnecessary actuator commands.

Estimator bandwidth is therefore a critical design parameter. A very slow estimator produces stable disturbance estimates but responds poorly when wind conditions change, whereas a very fast estimator can confuse measurement noise, vibration, and unmodeled dynamics with real wind. The selected bandwidth should reflect vehicle size, controller bandwidth, sensor quality, expected turbulence spectrum, and payload dynamics while maintaining clear separation from fast attitude stabilization.

Wind estimation can also support trajectory planning beyond immediate feedback control. A persistent wind vector may affect achievable ground speed, energy consumption, stopping distance, and return-to-home capability. Although mission planning is performed above the basic flight-control layer, exposing validated wind estimates allows higher-level software to adjust routes or speed profiles. The flight controller should provide uncertainty or confidence information together with the estimated disturbance.

Fault detection is important because an implausible disturbance estimate may indicate a problem elsewhere in the system. A sudden large estimated force without corresponding environmental evidence could result from an incorrect attitude estimate, propulsion fault, sensor bias, payload shift, or navigation error. Consistency checks should compare disturbance magnitude, persistence, vehicle response, actuator behavior, and estimator confidence before allowing large compensation terms to influence flight-critical commands.

When disturbance estimates become unreliable, the system should degrade gracefully to conventional feedback control. Feedforward wind compensation can be reduced or disabled while attitude, altitude, and velocity loops continue operating from validated state estimates. This separation ensures that disturbance estimation improves nominal performance without becoming a single point of failure for basic stabilization. Conservative maneuver limits may be applied until estimator confidence recovers.

Verification should include steady wind, directional changes, discrete gusts, turbulence, vertical airflow, and combined payload variations. Simulation and software-in-the-loop or hardware-in-the-loop testing can introduce aerodynamic forces independently from the controller model to evaluate estimation accuracy. Important measures include position deviation, velocity error, attitude excursion, disturbance-estimation error, recovery time, actuator utilization, and behavior near thrust or tilt saturation.

Flight testing should progressively expand wind conditions while monitoring both rejection performance and remaining control authority. Position hold, hover, climb, descent, waypoint tracking, and braking can be evaluated under representative payload configurations. Particular attention should be given to strong crosswinds and gusts near maximum cargo weight because reduced propulsion margin can expose interactions among wind compensation, altitude control, attitude stabilization, and motor allocation.

Wind disturbance rejection and estimation ultimately extend the basic feedback controller with awareness of external aerodynamic forces. Fast inner loops stabilize immediate motion, slower translational loops remove residual errors, and disturbance estimation provides predictive compensation for persistent environmental effects. Within the flight-control architecture, this function complements load-change adaptive control and prepares the system for flight-mode management and integrated FCS verification under realistic cargo-UAV operating conditions.

바람 외란 제거(Wind Disturbance Rejection)는 정상풍(Steady Wind), 돌풍(Gust), 난류(Turbulence), 국부적인 기류(Localized Airflow)로 발생하는 외부 공기역학적 힘에도 불구하고 화물 무인항공기(Cargo UAV)가 자세, 고도, 속도 및 위치를 유지할 수 있도록 한다. 대형 화물 항공기는 상당한 표면적을 가지며 제한된 추력 여유(Thrust Margin)에서 운용되는 경우가 많기 때문에 바람은 상당한 병진 및 회전 외란을 발생시킬 수 있다. 따라서 비행 제어 시스템(Flight-Control System)은 과도한 액추에이터 동작을 발생시키지 않으면서 외란의 영향을 추정하고 제거해야 한다.

바람은 공기역학적 힘(Aerodynamic Force)과 모멘트(Moment)를 통해 항공기에 영향을 준다. 일정한 측풍(Crosswind)은 지속적인 수평력을 발생시킬 수 있으며, 돌풍은 가속도와 자세에 급격한 변화를 발생시킨다. 수직 기류(Vertical Air Motion)는 상승률 또는 하강률을 변화시킬 수 있고, 동체, 화물 또는 추진 구조물 주변의 비대칭 기류(Asymmetric Airflow)는 회전 모멘트를 생성할 수 있다. 이러한 영향은 기체 형상, 대기속도(Airspeed), 화물 구성 및 주변 기류의 상대 방향에 따라 달라진다.

기본적인 피드백 제어 루프(Feedback-Control Loop)는 이미 중요한 수준의 외란 제거 기능을 제공한다. 각속도 제어기(Angular-Rate Controller)는 예상하지 못한 회전 운동을 억제하고, 자세 제어기(Attitude Controller)는 명령된 자세를 복원하며, 속도 제어기(Velocity Controller)는 병진 운동의 편차를 보정하고, 위치 제어기(Position Controller)는 누적된 위치 변위를 제거한다. 그러나 피드백에만 의존하면 보정 동작이 시작되기 전에 오차가 먼저 발생해야 하므로 강하거나 빠르게 변화하는 바람에서 더 큰 편차가 발생할 수 있다.

적분 제어(Integral Control)는 거의 일정하게 유지되는 외란을 제거하는 데 특히 유용하다. 일정한 바람에서 위치 유지(Position Hold)를 수행할 경우 기체는 정지 상태를 유지하기 위해 지속적인 기울기와 수평 추력 성분을 필요로 할 수 있다. 속도 또는 위치 적분기(Integrator)는 필요한 보상량을 점진적으로 생성할 수 있다. 그러나 강한 바람이 추진 시스템이 제공할 수 있는 힘보다 큰 힘을 요구하면 제어 권한(Control Authority)이 포화되고 와인드업(Windup)이 발생할 수 있으므로 적분 동작은 제한되어야 한다.

명시적인 외란 추정(Disturbance Estimation)은 큰 추종 오차가 누적되기 전에 항공기에 작용하는 외력을 식별하여 성능을 향상시킬 수 있다. 외란 관측기(Disturbance Observer)는 명령된 추력으로부터 예상되는 기체 가속도와 측정 또는 추정된 실제 가속도를 비교할 수 있다. 모델 불확실성을 고려한 상태에서 설명되지 않는 차이를 외부 외란 힘으로 해석할 수 있으며, 이렇게 추정된 값은 병진 제어기(Translational Controller)의 피드포워드 보상(Feedforward Compensation)에 사용할 수 있다.

바람 추정(Wind Estimation)과 외란 추정은 서로 관련되어 있지만 동일한 개념은 아니다. 풍속(Wind Velocity)은 지구 고정 좌표계(Earth-Fixed Frame)를 기준으로 주변 공기의 움직임을 나타내는 반면 외란 힘(Disturbance Force)은 이러한 기류가 기체에 발생시키는 공기역학적 영향을 의미한다. 두 값 사이를 변환하려면 투영 면적(Projected Area), 항력 특성(Drag Characteristics) 등의 매개변수를 포함하는 공기역학 모델이 필요하다. 따라서 많은 제어 응용에서는 정확한 대기 풍속을 복원하는 것보다 외란 힘을 직접 추정하는 것이 더 유용할 수 있다.

확장 상태 관측기(Extended State Observer) 또는 증강 상태 추정기(Augmented State Estimator)는 외부 외란을 추가적인 저속 변화 상태(Slowly Varying State)로 표현할 수 있다. 이러한 상태는 기체 동역학과 함께 전파되고 측정된 운동을 이용하여 보정된다. 예측된 가속도와 관측된 가속도가 지속적으로 다르면 추정기는 외란 상태를 조정한다. 적절한 프로세스 잡음(Process Noise)을 설정하면 센서 잡음이나 모델링 오차에 과도하게 반응하지 않으면서 변화하는 바람을 추정할 수 있다.

추정기는 바람을 질량, 추력 보정(Thrust Calibration), 자세 또는 센서 바이어스(Sensor Bias)의 오차와 구분해야 한다. 예를 들어 기체 질량을 실제보다 작게 추정하면 무풍 상태에서도 측정된 가속도가 모델과 다르게 나타날 수 있다. 마찬가지로 추진 시스템의 성능 저하도 수학적으로는 반대 방향으로 작용하는 외력과 유사하게 나타날 수 있다. 따라서 바람 외란 추정은 모든 모델 잔차를 대기 외란으로 처리하는 대신 부하 적응 제어(Load-Adaptive Control), 액추에이터 상태 모니터링(Actuator-Health Monitoring), 상태 추정(State Estimation)과 일관되게 동작해야 한다.

추력을 기체 좌표계(Body Frame)와 항법 좌표계(Navigation Frame) 사이에서 정확하게 변환한 후 외력을 추론해야 하므로 정확한 자세 정보가 필수적이다. 작은 자세 오차도 바람과 유사한 겉보기 수평 가속도(Apparent Horizontal Acceleration)를 발생시킬 수 있다. 가속도계 바이어스와 진동도 비슷한 영향을 줄 수 있다. 필터링, 공분산 정보(Covariance Information), 추정기 상태 모니터링(Estimator-Health Monitoring), 제한된 외란 상태를 이용하면 이러한 불완전성이 비현실적인 바람 보상으로 이어지는 것을 방지할 수 있다.

위성항법시스템(Global Navigation Satellite System, GNSS)에서 얻은 지상 속도(Ground Velocity)와 대기속도 측정값을 결합하는 방법도 풍속 추정의 기반이 될 수 있다. 지면에 대한 항공기 속도와 주변 공기에 대한 항공기 속도의 차이는 바람 벡터(Wind Vector)에 대한 정보를 제공한다. 신뢰할 수 있는 대기자료 센서(Air-Data Sensor)를 갖춘 항공기에서는 이러한 방법이 더욱 직접적이지만 저속 멀티로터 운용, 로터 하강류(Rotor Downwash), 센서 배치 및 교란된 기류로 인해 정확한 대기속도 측정이 어려울 수 있다.

화물 구성(Cargo Configuration)은 바람에 대한 민감도를 크게 변화시킬 수 있다. 대형 외부 화물은 질량 증가에 비례하지 않으면서 투영 면적과 공기역학적 항력을 크게 증가시킬 수 있다. 따라서 동일한 바람에서도 화물 형상에 따라 서로 다른 가속도가 발생할 수 있다. 비대칭 화물은 공기역학적 모멘트도 발생시킬 수 있다. 고정된 무화물 기체 모델(Clean-Airframe Model)에만 기반한 외란 추정기는 모든 임무 구성에서 동일한 공기역학적 특성을 가정하지 않고 이러한 불확실성을 고려해야 한다.

현수 화물(Suspended Load)은 바람이 항공기와 화물에 독립적으로 작용할 수 있기 때문에 추가적인 문제를 발생시킨다. 화물 흔들림(Load Swing)은 기체 운동만을 관측하는 경우 변화하는 바람 외란과 유사한 진동성 힘을 발생시킬 수 있다. 공격적인 외란 보상은 의도하지 않게 이러한 진자 운동(Pendulum Behavior)을 가진할 수 있다. 따라서 필터링과 대역폭 분리(Bandwidth Separation)를 적용하여 바람 제거 루프가 모든 고주파 화물 유발 가속도를 제거하려고 시도하지 않도록 해야 한다.

피드포워드 외란 보상(Feedforward Disturbance Compensation)은 명령된 기체 힘에서 추정된 외력을 상쇄하는 방식으로 적용할 수 있다. 예를 들어 측풍이 항공기를 동쪽으로 밀고 있다고 추정되면 제어기는 큰 위치 오차가 발생하기 전에 서쪽 방향의 반대 힘을 생성할 수 있다. 필요한 힘은 적절한 추력 벡터 방향(Thrust-Vector Orientation)과 집단 추력(Collective Thrust)으로 변환되며, 일반적인 피드백 루프는 남아 있는 추정 및 모델링 오차를 보정한다.

외란 보상은 자세 및 추진 한계(Propulsion Limit)를 준수해야 한다. 강한 수평 바람에 대응하려면 항공기를 기울여야 하며, 이에 따라 사용 가능한 추력의 수직 성분이 감소한다. 무거운 화물을 탑재한 화물 무인항공기는 무화물 상태보다 기울기 또는 추력 한계에 더 빠르게 도달할 수 있다. 수평 외란을 완전히 제거하는 것이 물리적으로 불가능한 경우 제어기는 달성할 수 없는 위치 유지 조건을 계속 요구하기보다 안정성과 고도 유지를 우선해야 한다.

따라서 바람 외란 제거 기능은 제어 권한 모니터링(Control-Authority Monitoring)과 상호작용해야 한다. 사용 가능한 추력 여유, 최대 기울기, 모터 포화, 배터리 상태 및 화물 질량은 안전하게 제거할 수 있는 외란의 크기를 결정한다. 추정된 바람에 대응하기 위한 요구량이 이러한 한계에 접근하면 비행 제어 시스템은 궤적의 공격성을 감소시키거나 위치 허용 오차를 증가시키고, 임무 기능을 제한하거나 상위 비행 모드 관리(Flight-Mode Management)에 현재 요구된 운용이 지속 불가능해지고 있음을 전달할 수 있다.

돌풍 제거(Gust Rejection)는 정상풍 보상과 다른 균형이 필요하다. 빠른 돌풍에는 느리게 적응하는 외란 추정기가 효과적으로 추종하기 어려운 고주파 성분이 포함된다. 빠른 자세 및 각속도 피드백이 즉각적인 응답의 상당 부분을 처리하고, 외란 추정기는 이후 지속되는 저주파 성분을 추정해야 한다. 모든 빠른 변동을 추정하려고 하면 센서 잡음이 증폭되고 불필요한 액추에이터 명령이 발생할 수 있다.

따라서 추정기 대역폭(Estimator Bandwidth)은 핵심적인 설계 매개변수이다. 매우 느린 추정기는 안정적인 외란 추정값을 제공하지만 바람 조건이 변화할 때 응답성이 떨어지며, 지나치게 빠른 추정기는 측정 잡음, 진동 및 모델링되지 않은 동역학을 실제 바람으로 잘못 판단할 수 있다. 선택되는 대역폭은 기체 크기, 제어기 대역폭, 센서 품질, 예상 난류 스펙트럼(Turbulence Spectrum), 화물 동역학을 반영하면서 빠른 자세 안정화 루프와 명확한 분리를 유지해야 한다.

바람 추정은 즉각적인 피드백 제어뿐만 아니라 궤적 계획(Trajectory Planning)도 지원할 수 있다. 지속적인 바람 벡터는 달성 가능한 지상 속도, 에너지 소비, 정지 거리 및 자동 귀환(Return-to-Home) 능력에 영향을 줄 수 있다. 임무 계획은 기본 비행 제어 계층보다 상위에서 수행되지만 검증된 바람 추정값을 제공하면 상위 소프트웨어가 경로나 속도 프로파일을 조정할 수 있다. 비행 제어기는 추정된 외란과 함께 불확실성 또는 신뢰도 정보도 제공해야 한다.

비현실적인 외란 추정값은 시스템의 다른 부분에서 발생한 문제를 나타낼 수 있으므로 고장 감지(Fault Detection)가 중요하다. 환경적 근거 없이 갑자기 매우 큰 힘이 추정된다면 잘못된 자세 추정, 추진 시스템 고장, 센서 바이어스, 화물 이동 또는 항법 오차가 원인일 수 있다. 큰 보상 항이 비행 중요 명령에 영향을 주도록 허용하기 전에 외란의 크기와 지속성, 기체 응답, 액추에이터 동작 및 추정기 신뢰도를 비교하는 일관성 검사(Consistency Check)를 수행해야 한다.

외란 추정값의 신뢰성이 저하되면 시스템은 기존의 피드백 제어(Conventional Feedback Control)로 점진적으로 전환해야 한다. 피드포워드 바람 보상은 감소시키거나 비활성화할 수 있으며, 자세, 고도 및 속도 루프는 검증된 상태 추정값을 기반으로 계속 동작할 수 있다. 이러한 분리는 외란 추정이 정상 운용 성능을 향상시키면서도 기본 안정화를 위한 단일 고장점(Single Point of Failure)이 되지 않도록 한다. 추정기 신뢰도가 회복될 때까지 보수적인 기동 제한을 적용할 수도 있다.

검증(Verification)은 정상풍, 풍향 변화, 불연속적인 돌풍, 난류, 수직 기류 및 화물 변화가 결합된 조건을 포함해야 한다. 시뮬레이션과 소프트웨어 인 더 루프(Software-in-the-Loop, SIL) 또는 하드웨어 인 더 루프(Hardware-in-the-Loop, HIL) 시험에서는 제어기 모델과 독립적으로 공기역학적 힘을 주입하여 추정 정확도를 평가할 수 있다. 주요 평가 항목에는 위치 편차, 속도 오차, 자세 변동, 외란 추정 오차, 복구 시간, 액추에이터 사용률 및 추력 또는 기울기 포화 부근에서의 동작이 포함된다.

비행 시험(Flight Testing)은 외란 제거 성능과 남아 있는 제어 권한을 동시에 모니터링하면서 바람 조건을 점진적으로 확대해야 한다. 대표적인 화물 구성에서 위치 유지, 호버링, 상승, 하강, 웨이포인트 추종 및 제동 성능을 평가할 수 있다. 특히 최대 화물 중량에 가까운 상태에서 강한 측풍과 돌풍이 발생하면 감소된 추진 여유로 인해 바람 보상, 고도 제어, 자세 안정화 및 모터 할당 사이의 상호작용이 명확하게 나타날 수 있으므로 특별한 주의가 필요하다.

바람 외란 제거 및 추정(Wind Disturbance Rejection and Estimation)은 궁극적으로 외부 공기역학적 힘을 인식하는 기능을 기본 피드백 제어기에 추가한다. 빠른 내부 루프(Fast Inner Loop)는 즉각적인 운동을 안정화하고, 상대적으로 느린 병진 제어 루프(Translational Control Loop)는 남아 있는 오차를 제거하며, 외란 추정은 지속적인 환경 영향에 대한 예측 보상(Predictive Compensation)을 제공한다. 전체 비행 제어 구조에서 이 기능은 부하 변화 적응 제어(Load-Change Adaptive Control)를 보완하고 실제 화물 무인항공기 운용 조건에서 비행 모드 관리와 통합 비행 제어 시스템 검증(Integrated FCS Verification)을 수행하기 위한 기반을 제공한다.

##  

## 03.09. Flight Mode Manager Manual Semi Auto Full Auto [w/Code]

![](images/image9.png){width="7.268055555555556in" height="7.268055555555556in"}

The flight mode manager coordinates how pilot commands, autonomous functions, navigation systems, and low-level flight controllers share authority over the cargo UAV. Rather than implementing a separate controller for every operational condition, it selects which command sources and control loops are active. Manual, semi-automatic, and fully autonomous modes therefore represent different levels of command authority built on the same underlying stabilization architecture.

Manual mode gives the human operator the greatest direct influence over aircraft motion while retaining essential stabilization functions. Pilot inputs may command attitude, angular rate, collective thrust, or vertical motion depending on the selected implementation. Even in manual operation, the flight-control system normally maintains rate damping, command limits, actuator allocation, and safety constraints so that operator commands are translated into physically achievable aircraft responses.

Manual control should not imply unrestricted actuator access. Commands must remain bounded by validated roll, pitch, yaw-rate, thrust, acceleration, and structural limits. The flight mode manager routes pilot commands through the appropriate control loops rather than allowing direct motor manipulation during normal flight. This preserves consistent stabilization behavior and prevents command sources from bypassing critical protection mechanisms embedded in the flight-control architecture.

Semi-automatic mode transfers selected stabilization or navigation tasks to the onboard system while leaving higher-level intent under human control. The operator may command altitude, heading, velocity, or position rather than raw attitude and thrust. The corresponding automatic loops then determine the lower-level commands required to achieve those objectives. This reduces pilot workload while preserving immediate human authority over mission direction and operational decisions.

Different semi-automatic functions can be composed from the existing controller hierarchy. An altitude-hold mode can maintain vertical position while the pilot controls horizontal motion, while position hold can automatically counter wind and navigation drift. Velocity-command modes can allow the operator to specify desired translational motion without continuously controlling attitude. The mode manager determines which loops are closed automatically and which references remain controlled by the operator.

Fully autonomous mode moves command generation to the mission and navigation software. Desired trajectories, waypoints, altitude profiles, and mission actions are generated without continuous pilot input and passed through the position, velocity, altitude, and attitude-control hierarchy. The flight mode manager remains responsible for determining whether the required navigation, estimation, propulsion, and safety conditions are valid enough to permit continued autonomous operation.

The three autonomy levels should share common low-level control functions wherever practical. Attitude estimation, angular-rate stabilization, motor allocation, saturation management, payload adaptation, and actuator-health monitoring should not be unnecessarily duplicated between modes. A common control foundation reduces implementation differences and ensures that switching command authority does not also replace the fundamental dynamics used to stabilize the aircraft.

Mode transitions are as important as the steady-state behavior of each mode. Switching from manual to autonomous control can produce abrupt motion if the new controller begins with references that differ from the aircraft's current state. The mode manager should therefore initialize target attitude, altitude, velocity, position, and controller states from current validated conditions whenever appropriate, enabling a bumpless transfer between command sources.

Transition from autonomous to manual control requires similar coordination. If the aircraft is executing a turn or climb when the operator takes control, immediately replacing autonomous references with unrelated pilot commands may produce a discontinuity. Input blending, reference synchronization, command ramping, and integrator management can smooth the transfer while still allowing rapid human intervention when operational circumstances require it.

Control authority must be explicitly defined for every mode. The software should know which subsystem owns attitude, altitude, horizontal velocity, position, yaw, and mission-level commands at any moment. Ambiguous ownership can allow two controllers to issue conflicting references or leave an axis uncontrolled. A structured authority model prevents command arbitration from becoming an implicit side effect of message timing or software execution order.

A state-machine architecture provides a practical framework for mode management. Each flight mode can define entry conditions, active control functions, allowed transitions, exit conditions, and fallback behavior. Transition guards evaluate information such as estimator validity, navigation availability, propulsion health, payload status, communication condition, and flight phase before permitting a requested change. This prevents invalid modes from becoming active solely because a command was received.

Preconditions are particularly important for fully autonomous flight. Position-based autonomy should not activate when the navigation solution is invalid or excessively uncertain. Automatic altitude functions require a valid vertical state, while trajectory tracking requires adequate propulsion and control authority. The mode manager should verify these dependencies before engagement and continuously monitor them afterward because a condition that was valid at mode entry can degrade during flight.

Manual override should be treated as an intentional authority-transfer mechanism rather than an uncontrolled interruption. Depending on the system design, a sufficiently strong pilot command, dedicated switch, or supervisory command can request transition from autonomous to a lower automation level. The transition logic should clearly define which automatic functions remain active, ensuring that human intervention does not unintentionally disable basic attitude stabilization or other essential protections.

Communication loss requires mode-dependent handling. In a manually controlled aircraft, loss of the command link may eliminate the active source of mission intent and therefore require position hold, return-to-home, controlled landing, or another predefined contingency response. In fully autonomous operation, temporary communication loss may not immediately prevent mission execution, but the system must still apply mission-specific communication and safety policies.

Navigation degradation also affects modes differently. Manual attitude control may remain possible without global position information, whereas position hold and waypoint navigation depend strongly on a valid navigation solution. The mode manager should therefore degrade functionality according to actual dependency rather than treating every sensor failure as requiring immediate termination of all control. This layered degradation preserves useful capability while avoiding unsupported autonomous functions.

Propulsion and control-authority degradation must also influence mode availability. A motor fault, reduced battery capability, excessive payload, or persistent saturation can leave enough authority for stable flight but not for aggressive autonomous trajectory tracking. The mode manager can restrict maximum speed, acceleration, tilt, or climb rate, transition to a conservative mode, or initiate landing according to the severity of the remaining capability.

Cargo operations can introduce mission-specific modes or substates within the same management framework. Pickup, transport, hover during loading, payload release, and post-release stabilization may require different motion limits and control parameters. The flight mode manager can coordinate these phases with load-change adaptive control so that changes in mass, center of gravity, and propulsion margin are reflected before aggressive maneuvering resumes.

Wind conditions can similarly influence mode selection without requiring an entirely separate control architecture. If disturbance estimation indicates that position hold requires nearly all available horizontal or vertical control authority, the manager can reduce trajectory demands or prevent entry into modes requiring precise station keeping. This links wind-disturbance rejection with supervisory decisions about whether the requested mission behavior remains physically sustainable.

Safety modes should be designed as controlled configurations rather than simply as emergency labels. A degraded-navigation mode, return-to-home function, controlled descent, emergency landing, or reduced-authority stabilization mode should specify exactly which state sources, control loops, limits, and command owners are active. Deterministic definitions make safety behavior testable and prevent unexpected combinations of controllers during abnormal conditions.

Integrator and controller-state management is critical during mode changes. An inactive controller may accumulate stale state or retain a value from a previous operating period. If it is reactivated without initialization, a sudden command can result. Integrators can therefore be frozen, reset, tracked to the active controller, or initialized during transition according to the control architecture. Similar treatment is required for filters, trajectory generators, and disturbance estimates.

Mode status must be observable to both onboard systems and human operators. The flight-control software should expose the active mode, requested mode, transition state, command authority, relevant validity flags, and reason for any rejected or automatic transition. Clear status information is essential for diagnosing behavior because identical aircraft motion can result from different combinations of pilot commands, autonomous references, and safety interventions.

Mode arbitration should follow deterministic priorities. Safety-critical transitions normally take precedence over mission convenience, while conflicting manual, autonomous, and supervisory requests should be resolved according to explicitly defined authority rules. The implementation should avoid relying on whichever command message arrives last because communication timing does not provide a reliable representation of operational priority.

Verification must test transitions as extensively as individual modes. Simulation and software-in-the-loop or hardware-in-the-loop testing should exercise manual-to-semi-automatic, semi-automatic-to-autonomous, autonomous-to-manual, and normal-to-degraded transitions under different attitudes, velocities, payloads, winds, and failure conditions. Testing should examine command continuity, transient motion, controller-state initialization, rejected transitions, and recovery behavior.

Failure-injection testing should include navigation loss, communication interruption, estimator degradation, propulsion faults, actuator saturation, payload-information errors, and invalid mode commands. The objective is not merely to confirm that the correct mode name is selected, but to verify that the expected controllers, command sources, limits, and fallback behaviors actually become active within the required timing constraints.

The flight mode manager ultimately provides the supervisory logic that connects stabilized flight control with human operation and autonomous mission execution. Manual mode emphasizes direct operator intent, semi-automatic modes delegate selected control objectives, and full autonomy delegates trajectory and mission command generation while retaining safety supervision. Within the chapter structure, this supervisory layer follows the fundamental controllers, motor allocation, load adaptation, and wind rejection, preparing the complete FCS for integrated verification and validation.

비행 모드 관리자(Flight Mode Manager)는 조종사 명령, 자율 기능(Autonomous Function), 항법 시스템(Navigation System), 저수준 비행 제어기(Low-Level Flight Controller)가 화물 무인항공기(Cargo UAV)에 대한 제어 권한을 어떻게 공유할지를 조정한다. 각각의 운용 조건마다 별도의 제어기를 구현하는 대신 어떤 명령 소스와 제어 루프를 활성화할 것인지를 선택한다. 따라서 수동(Manual), 반자동(Semi-Automatic), 완전 자율(Full Autonomous) 모드는 동일한 기본 안정화 구조 위에서 서로 다른 수준의 명령 권한을 제공한다.

수동 모드(Manual Mode)는 필수적인 안정화 기능을 유지하면서 인간 조종자에게 항공기 운동에 대한 가장 높은 수준의 직접적인 제어 권한을 제공한다. 구현 방식에 따라 조종사 입력은 자세, 각속도, 집단 추력(Collective Thrust) 또는 수직 운동을 명령할 수 있다. 수동 운용에서도 비행 제어 시스템은 일반적으로 각속도 감쇠(Rate Damping), 명령 제한, 액추에이터 할당(Actuator Allocation), 안전 제약을 유지하여 조종사의 명령을 물리적으로 달성 가능한 항공기 응답으로 변환한다.

수동 제어가 제한 없는 액추에이터 접근을 의미해서는 안 된다. 명령은 검증된 롤(Roll), 피치(Pitch), 요 각속도(Yaw Rate), 추력, 가속도 및 구조적 한계 내에서 유지되어야 한다. 비행 모드 관리자는 정상 비행 중 직접적인 모터 조작을 허용하는 대신 적절한 제어 루프를 통해 조종사 명령을 전달한다. 이를 통해 일관된 안정화 동작을 유지하고 명령 소스가 비행 제어 구조에 포함된 핵심 보호 메커니즘을 우회하는 것을 방지한다.

반자동 모드(Semi-Automatic Mode)는 선택된 안정화 또는 항법 작업을 기내 시스템(Onboard System)에 위임하면서 상위 수준의 운용 의도는 인간이 제어하도록 한다. 조종자는 원시 자세 및 추력 대신 고도, 헤딩(Heading), 속도 또는 위치를 명령할 수 있다. 해당 자동 제어 루프는 이러한 목표를 달성하는 데 필요한 저수준 명령을 결정한다. 이를 통해 임무 방향과 운용 결정에 대한 인간의 즉각적인 권한을 유지하면서 조종사의 작업 부하를 줄일 수 있다.

다양한 반자동 기능은 기존 제어기 계층(Controller Hierarchy)을 조합하여 구성할 수 있다. 고도 유지 모드(Altitude-Hold Mode)는 조종사가 수평 운동을 제어하는 동안 수직 위치를 자동으로 유지할 수 있으며, 위치 유지(Position Hold)는 바람과 항법 드리프트(Navigation Drift)를 자동으로 보상할 수 있다. 속도 명령 모드(Velocity-Command Mode)를 사용하면 조종자가 자세를 지속적으로 제어하지 않고도 원하는 병진 운동을 지정할 수 있다. 비행 모드 관리자는 어떤 루프를 자동으로 폐루프 제어하고 어떤 기준값을 조종자가 직접 제어할지를 결정한다.

완전 자율 모드(Fully Autonomous Mode)에서는 명령 생성 기능이 임무 및 항법 소프트웨어(Mission and Navigation Software)로 이동한다. 목표 궤적, 웨이포인트(Waypoint), 고도 프로파일 및 임무 동작은 지속적인 조종사 입력 없이 생성되어 위치, 속도, 고도 및 자세 제어 계층으로 전달된다. 비행 모드 관리자는 요구되는 항법, 상태 추정, 추진 및 안전 조건이 지속적인 자율 운용을 허용할 만큼 충분히 유효한지를 판단하는 역할을 계속 수행한다.

세 가지 자율화 수준은 가능한 경우 공통 저수준 제어 기능(Common Low-Level Control Function)을 공유해야 한다. 자세 추정(Attitude Estimation), 각속도 안정화(Angular-Rate Stabilization), 모터 할당(Motor Allocation), 포화 관리(Saturation Management), 화물 적응(Payload Adaptation), 액추에이터 상태 모니터링(Actuator-Health Monitoring)은 모드별로 불필요하게 중복 구현해서는 안 된다. 공통 제어 기반을 사용하면 구현 차이를 줄일 수 있으며 명령 권한을 전환하더라도 항공기를 안정화하는 기본 동역학 구조가 변경되지 않도록 할 수 있다.

모드 전환(Mode Transition)은 각 모드의 정상 상태 동작만큼 중요하다. 수동 제어에서 자율 제어로 전환할 때 새로운 제어기가 항공기의 현재 상태와 다른 기준값으로 시작하면 갑작스러운 운동이 발생할 수 있다. 따라서 비행 모드 관리자는 적절한 경우 현재의 검증된 상태를 이용하여 목표 자세, 고도, 속도, 위치 및 제어기 상태를 초기화해야 한다. 이를 통해 명령 소스 사이에서 무충격 전환(Bumpless Transfer)을 구현할 수 있다.

자율 제어에서 수동 제어로 전환할 때도 유사한 조정이 필요하다. 항공기가 선회 또는 상승을 수행하는 동안 조종자가 제어권을 인수할 경우 자율 기준값을 관련성이 없는 조종사 명령으로 즉시 교체하면 불연속이 발생할 수 있다. 입력 블렌딩(Input Blending), 기준값 동기화(Reference Synchronization), 명령 램핑(Command Ramping), 적분기 관리(Integrator Management)를 이용하여 전환을 부드럽게 만들면서도 운용 상황에서 필요한 경우 인간이 신속하게 개입할 수 있도록 해야 한다.

각 모드에서는 제어 권한(Control Authority)을 명확하게 정의해야 한다. 소프트웨어는 특정 시점에 어떤 서브시스템이 자세, 고도, 수평 속도, 위치, 요 및 임무 수준 명령에 대한 권한을 갖는지 알고 있어야 한다. 권한이 모호하면 두 개의 제어기가 서로 충돌하는 기준값을 생성하거나 특정 축이 제어되지 않는 상태가 발생할 수 있다. 구조화된 권한 모델(Structured Authority Model)은 명령 중재(Command Arbitration)가 메시지 타이밍이나 소프트웨어 실행 순서의 암묵적인 결과로 결정되는 것을 방지한다.

상태 머신 구조(State-Machine Architecture)는 모드 관리를 위한 실용적인 프레임워크를 제공한다. 각 비행 모드는 진입 조건(Entry Condition), 활성 제어 기능, 허용된 전환, 종료 조건(Exit Condition), 폴백 동작(Fallback Behavior)을 정의할 수 있다. 전환 가드(Transition Guard)는 요청된 모드 변경을 허용하기 전에 추정기 유효성, 항법 가용성, 추진 시스템 상태, 화물 상태, 통신 상태 및 비행 단계를 평가한다. 이를 통해 단순히 명령이 수신되었다는 이유만으로 유효하지 않은 모드가 활성화되는 것을 방지한다.

완전 자율 비행에서는 사전 조건(Precondition)이 특히 중요하다. 위치 기반 자율 기능은 항법 해(Navigation Solution)가 유효하지 않거나 불확실성이 지나치게 큰 경우 활성화되어서는 안 된다. 자동 고도 기능에는 유효한 수직 상태가 필요하며, 궤적 추종에는 충분한 추진 능력과 제어 권한이 필요하다. 비행 모드 관리자는 모드 진입 전에 이러한 의존성을 검증하고 이후에도 지속적으로 모니터링해야 한다. 모드 진입 시점에 유효했던 조건도 비행 중에는 성능이 저하될 수 있기 때문이다.

수동 오버라이드(Manual Override)는 제어되지 않은 중단이 아니라 의도적으로 설계된 권한 전환 메커니즘(Authority-Transfer Mechanism)으로 처리해야 한다. 시스템 설계에 따라 충분히 큰 조종사 명령, 전용 스위치 또는 감독 명령(Supervisory Command)을 이용하여 자율 모드에서 더 낮은 자동화 수준으로의 전환을 요청할 수 있다. 전환 로직은 어떤 자동 기능이 계속 활성 상태로 유지되는지를 명확하게 정의하여 인간의 개입으로 기본 자세 안정화나 기타 필수 보호 기능이 의도하지 않게 비활성화되는 것을 방지해야 한다.

통신 손실(Communication Loss)은 모드에 따라 서로 다른 방식으로 처리해야 한다. 수동 제어 항공기에서는 명령 링크(Command Link)의 손실로 활성 임무 명령 소스가 사라질 수 있으므로 위치 유지, 자동 귀환(Return-to-Home), 제어된 착륙(Controlled Landing) 또는 사전에 정의된 다른 비상 대응이 필요할 수 있다. 완전 자율 운용에서는 일시적인 통신 손실이 즉시 임무 수행을 불가능하게 만들지는 않을 수 있지만 시스템은 여전히 임무별 통신 및 안전 정책을 적용해야 한다.

항법 성능 저하(Navigation Degradation)도 모드에 따라 서로 다른 영향을 준다. 수동 자세 제어는 전역 위치 정보 없이도 유지될 수 있지만 위치 유지 및 웨이포인트 항법은 유효한 항법 해에 크게 의존한다. 따라서 비행 모드 관리자는 모든 센서 고장을 전체 제어 기능의 즉각적인 종료로 처리하기보다 실제 의존성에 따라 기능을 단계적으로 저하시켜야 한다. 이러한 계층적 성능 저하(Layered Degradation)는 지원되지 않는 자율 기능을 방지하면서도 사용 가능한 기능을 최대한 유지할 수 있도록 한다.

추진 시스템 및 제어 권한의 성능 저하도 모드 가용성에 영향을 주어야 한다. 모터 고장, 배터리 성능 저하, 과도한 화물 또는 지속적인 포화는 안정적인 비행에는 충분한 제어 권한을 남길 수 있지만 공격적인 자율 궤적 추종에는 부족할 수 있다. 비행 모드 관리자는 남아 있는 성능의 심각도에 따라 최대 속도, 가속도, 기울기 또는 상승률을 제한하고, 보수적인 모드로 전환하거나 착륙을 시작할 수 있다.

화물 운용(Cargo Operation)은 동일한 관리 프레임워크 내에서 임무별 모드 또는 하위 상태(Substate)를 추가할 수 있다. 화물 픽업(Pickup), 운송, 적재 중 호버링, 화물 방출(Payload Release), 방출 후 안정화에는 서로 다른 운동 제한과 제어 매개변수가 필요할 수 있다. 비행 모드 관리자는 이러한 단계를 부하 변화 적응 제어(Load-Change Adaptive Control)와 조정하여 질량, 무게중심 및 추진 여유의 변화가 반영된 이후에 공격적인 기동이 다시 시작되도록 할 수 있다.

바람 조건 역시 완전히 별도의 제어 구조를 요구하지 않으면서 모드 선택에 영향을 줄 수 있다. 외란 추정(Disturbance Estimation)을 통해 위치 유지에 사용 가능한 수평 또는 수직 제어 권한의 대부분이 필요하다고 판단되면 관리자는 궤적 요구량을 감소시키거나 정밀한 위치 유지가 필요한 모드의 진입을 제한할 수 있다. 이를 통해 바람 외란 제거(Wind-Disturbance Rejection)를 요구되는 임무 동작이 물리적으로 지속 가능한지를 판단하는 감독 수준의 의사결정과 연결할 수 있다.

안전 모드(Safety Mode)는 단순한 비상 상태의 명칭이 아니라 제어된 구성(Controlled Configuration)으로 설계해야 한다. 성능 저하 항법 모드(Degraded-Navigation Mode), 자동 귀환 기능, 제어된 하강(Controlled Descent), 비상 착륙(Emergency Landing), 제한된 제어 권한 안정화 모드는 각각 어떤 상태 정보 소스, 제어 루프, 제한값 및 명령 소유자가 활성화되는지를 정확하게 정의해야 한다. 결정론적인 정의를 사용하면 안전 동작을 시험할 수 있으며 비정상 조건에서 예상하지 못한 제어기 조합이 발생하는 것을 방지할 수 있다.

모드 변경 중에는 적분기와 제어기 상태 관리(Controller-State Management)가 매우 중요하다. 비활성 제어기는 오래된 상태를 누적하거나 이전 운용 시점의 값을 유지할 수 있다. 이러한 제어기를 초기화하지 않고 다시 활성화하면 갑작스러운 명령이 발생할 수 있다. 따라서 제어 구조에 따라 적분기를 정지하거나 재설정하고, 활성 제어기의 상태를 추종하도록 하거나 전환 과정에서 초기화할 수 있다. 필터, 궤적 생성기(Trajectory Generator), 외란 추정값에도 유사한 처리가 필요하다.

모드 상태(Mode Status)는 기내 시스템과 인간 조종자 모두가 확인할 수 있어야 한다. 비행 제어 소프트웨어는 현재 활성 모드, 요청된 모드, 전환 상태, 명령 권한, 관련 유효성 플래그(Validity Flag), 거부된 전환 또는 자동 전환의 원인을 제공해야 한다. 동일한 항공기 운동이라도 조종사 명령, 자율 기준값 및 안전 개입(Safety Intervention)의 서로 다른 조합으로 발생할 수 있으므로 명확한 상태 정보는 동작을 진단하는 데 필수적이다.

모드 중재(Mode Arbitration)는 결정론적인 우선순위(Deterministic Priority)를 따라야 한다. 일반적으로 안전 중요 전환(Safety-Critical Transition)은 임무 편의성보다 우선하며, 서로 충돌하는 수동, 자율 및 감독 명령은 명확하게 정의된 권한 규칙에 따라 해결해야 한다. 통신 메시지의 도착 순서는 운용 우선순위를 신뢰성 있게 나타내지 못하므로 마지막으로 수신된 명령이 자동으로 우선권을 갖는 방식에 의존해서는 안 된다.

검증(Verification)에서는 개별 모드뿐만 아니라 모드 사이의 전환도 동일한 수준으로 시험해야 한다. 시뮬레이션과 소프트웨어 인 더 루프(Software-in-the-Loop, SIL) 또는 하드웨어 인 더 루프(Hardware-in-the-Loop, HIL) 시험을 통해 서로 다른 자세, 속도, 화물, 바람 및 고장 조건에서 수동에서 반자동, 반자동에서 자율, 자율에서 수동, 정상에서 성능 저하 상태로의 전환을 시험해야 한다. 명령 연속성, 과도 운동, 제어기 상태 초기화, 거부된 전환 및 복구 동작을 평가해야 한다.

고장 주입 시험(Failure-Injection Testing)에는 항법 손실, 통신 중단, 추정기 성능 저하, 추진 시스템 고장, 액추에이터 포화, 화물 정보 오류 및 유효하지 않은 모드 명령이 포함되어야 한다. 시험의 목적은 단순히 올바른 모드 이름이 선택되는지를 확인하는 것이 아니라 예상된 제어기, 명령 소스, 제한값 및 폴백 동작이 요구되는 시간 제약 내에서 실제로 활성화되는지를 검증하는 것이다.

비행 모드 관리자(Flight Mode Manager)는 궁극적으로 안정화된 비행 제어와 인간 운용 및 자율 임무 수행을 연결하는 감독 로직(Supervisory Logic)을 제공한다. 수동 모드는 직접적인 조종자 의도를 강조하고, 반자동 모드는 선택된 제어 목표를 시스템에 위임하며, 완전 자율 모드는 안전 감독을 유지하면서 궤적 및 임무 명령 생성을 자율 시스템에 위임한다. 전체 비행 제어 구조에서 이 감독 계층은 기본 제어기, 모터 할당, 부하 적응 및 바람 외란 제거 이후에 위치하며 완전한 비행 제어 시스템(FCS)의 통합 검증 및 유효성 확인(Integrated Verification and Validation)을 준비한다.

##  

## 03.10. FCS SIL HIL Test DO 178C Verification [w/Code]

![](images/image10.png){width="7.268055555555556in" height="7.268055555555556in"}

Flight-control-system verification establishes evidence that the FCS behaves correctly across its intended operating envelope and that its software implementation satisfies defined safety and functional requirements. For a cargo UAV, verification must cover attitude, altitude, position, motor allocation, payload adaptation, disturbance rejection, and mode management as an integrated control chain rather than validating each algorithm only in isolation.

A structured verification process begins with traceable requirements. System-level flight-control requirements are decomposed into software high-level and low-level requirements that define control behavior, interfaces, timing, limits, failure responses, and mode transitions. Each requirement should be connected to one or more verification activities so that test results demonstrate what was verified and why the resulting evidence is relevant to the intended FCS behavior.

Software-in-the-loop testing, commonly called SIL, executes flight-control software against simulated aircraft dynamics without requiring the actual flight computer or propulsion hardware. The simulator supplies sensor and navigation data, receives actuator commands, and propagates the aircraft state through aerodynamic and propulsion models. SIL enables large numbers of repeatable scenarios to be executed early in development before hardware integration becomes practical.

SIL environments are particularly useful for evaluating nominal control-law behavior. Step, ramp, trajectory, and disturbance inputs can measure rise time, settling time, overshoot, steady-state error, and tracking accuracy. Payload mass, center of gravity, wind, sensor noise, actuator limits, and model parameters can be varied systematically. Automated regression testing can repeat these scenarios whenever control software or configuration parameters are changed.

Simulation fidelity should match the verification objective. Simple rigid-body models can efficiently support early controller development, while higher-fidelity models may include propulsion dynamics, aerodynamic drag, actuator delays, sensor errors, flexible payload effects, and suspended-load motion. The simulation should not become unnecessarily complex, but it must represent the physical effects required to expose the failure modes and performance limitations addressed by each test.

SIL also supports abnormal-condition and fault-injection testing that would be difficult or unsafe to reproduce repeatedly in flight. GNSS loss, estimator divergence, sensor bias, communication interruption, motor degradation, actuator saturation, excessive wind, payload-information errors, and invalid mode commands can be introduced at controlled times. The resulting transitions and fallback behavior can then be compared with explicit safety and control requirements.

Hardware-in-the-loop testing, or HIL, extends verification by executing the production or representative flight software on the target flight-control computer while a real-time simulator represents the aircraft and environment. Physical processor timing, communication interfaces, I/O drivers, scheduling behavior, and hardware-dependent software therefore become part of the test. HIL bridges the gap between algorithm-level simulation and actual aircraft testing.

Real-time synchronization is essential in HIL testing. The aircraft simulation must produce sensor updates and receive actuator commands according to timing relationships representative of the real vehicle. Artificial latency, timing jitter, dropped messages, delayed sensor measurements, and communication faults can be injected deliberately. These tests reveal problems that may remain invisible in SIL environments where execution timing is idealized or substantially faster than real time.

The HIL environment should reproduce important flight-control interfaces with sufficient realism. IMU, GNSS, barometer, radar or laser altitude data, air-data information, propulsion feedback, power-system status, payload information, and command links can be simulated or electrically emulated as required. Motor outputs can be captured and converted into simulated thrust so that the complete closed control loop operates without physically flying the aircraft.

Verification of the cascaded controller structure should examine interactions among control-loop bandwidths. Angular-rate and attitude loops must remain stable while altitude, velocity, and position loops generate changing references. Tests should include aggressive trajectories, rapid command reversals, saturation, and disturbance recovery. The objective is to verify not only individual-loop performance but also that the combined hierarchy remains stable when multiple control objectives interact.

Motor mixing and control allocation require dedicated verification because correct high-level controller output does not guarantee correct actuator commands. Tests should confirm rotor numbering, rotation direction, force and moment signs, allocation coefficients, saturation behavior, priority rules, and degraded configurations. Incorrect allocation geometry can produce dangerous behavior even when the upstream attitude and position controllers are mathematically correct.

Cargo-specific verification should span the validated mass, inertia, and center-of-gravity envelope. SIL and HIL scenarios can vary payload configurations systematically and test pickup, release, asymmetric loading, and suspended-load conditions where applicable. Load-adaptive parameters should remain bounded, transitions should remain stable, and control authority should be sufficient for the maneuver limits permitted by the active flight mode.

Wind-disturbance testing should combine steady wind, gusts, turbulence, and vertical airflow with representative payload conditions. The verification environment can inject aerodynamic disturbances independently of the estimator model to avoid giving the controller perfect knowledge of the disturbance. Position deviation, attitude excursion, estimator convergence, actuator usage, and remaining thrust margin can then be evaluated against defined acceptance criteria.

Flight-mode verification must focus heavily on transitions and authority management. Manual, semi-automatic, and fully autonomous modes should be entered and exited under different positions, velocities, attitudes, and system-health conditions. Tests should verify transition guards, reference initialization, integrator handling, manual override, communication-loss response, navigation degradation, and automatic transition into predefined safety configurations.

DO-178C provides a framework for developing assurance that airborne software performs its intended functions with an appropriate level of rigor. Its verification perspective emphasizes requirements-based testing, traceability, reviews, analyses, configuration control, independence where required, and objective evidence. Applying these principles to cargo-UAV flight-control software requires a disciplined lifecycle rather than treating final flight testing as the primary demonstration of software correctness.

Software assurance rigor is related to the consequences of software failure through the assigned software level. More critical software requires stronger verification objectives and greater independence. The applicable level is determined through the system safety and development-assurance process rather than selected merely because software performs flight control. FCS development should therefore maintain a clear relationship among system safety assessment, software requirements, implementation, and verification evidence.

Requirements-based testing verifies that executable software satisfies specified behavior rather than merely demonstrating that the aircraft appears to fly correctly. Each test should identify the requirement being exercised, initial conditions, input sequence, expected results, actual results, and pass or fail determination. This structure makes verification repeatable and allows changes in software or requirements to trigger targeted regression testing.

Structural coverage analysis provides complementary evidence by examining which portions of the implemented software have been exercised by requirements-based tests. Depending on the applicable assurance objectives, statement, decision, and modified condition/decision coverage may become relevant. Coverage analysis is not a substitute for requirements-based testing; instead, uncovered structure can reveal missing tests, incomplete requirements, or unintended functionality requiring investigation.

Robustness testing evaluates behavior outside ideal nominal inputs. Boundary values, invalid messages, stale data, numerical extremes, sensor dropouts, timing violations, and inconsistent state information should be exercised deliberately. Flight-control software must respond deterministically without uncontrolled outputs, numerical instability, or undefined mode transitions. Defensive behavior should itself be connected to defined requirements rather than added as undocumented implementation behavior.

Verification should also address numerical properties of control software. Floating-point limits, saturation arithmetic, division by small values, matrix conditioning, quaternion normalization, coordinate transformations, and filter initialization can create failures that are not visible in nominal simulations. Long-duration tests are useful for identifying accumulated numerical drift, slowly growing integrator states, timestamp problems, and resource-related effects.

Traceability connects system requirements, software requirements, source code, test cases, procedures, and results into an auditable evidence chain. Bidirectional traceability helps demonstrate both that every requirement has been implemented and verified and that implemented software exists for an identified requirement. This is especially valuable in an FCS where safety logic, control laws, estimator interfaces, and mode-management functions interact extensively.

Configuration management ensures that verification evidence corresponds to a known software and hardware baseline. Controller gains, vehicle parameters, mixing matrices, payload limits, simulator models, test scripts, compiler settings, and flight-computer software versions should be controlled alongside source code. Otherwise, a successful test may not provide meaningful evidence for the configuration eventually installed on the aircraft.

Problem reporting and regression testing complete the verification feedback cycle. Failed tests and unexpected behavior should be documented, analyzed, corrected, and retested with traceability to the affected requirements and software components. Changes to one control function can influence other loops, so automated SIL regression suites and selected HIL regression scenarios are valuable for detecting unintended effects across the integrated FCS.

SIL and HIL verification do not eliminate the need for ground and flight testing. Instead, they reduce uncertainty before the aircraft enters higher-risk test phases. Flight tests validate effects that cannot be represented completely in simulation, including real aerodynamics, propulsion variability, structural interaction, environmental disturbances, and operational procedures. Results can also expose model deficiencies that should be incorporated into subsequent simulation and regression campaigns.

A mature FCS verification strategy therefore forms a progression from requirements and analysis through SIL, HIL, ground integration, and controlled flight testing. Each stage increases physical realism while retaining evidence from earlier stages. Combined with DO-178C-oriented requirements traceability, coverage, configuration control, reviews, and repeatable test records, this process provides systematic assurance that cargo-UAV flight-control software remains predictable under nominal, boundary, degraded, and failure conditions.

비행 제어 시스템 검증(Flight-Control-System Verification)은 비행 제어 시스템(FCS)이 의도된 운용 영역(Operating Envelope) 전체에서 올바르게 동작하고 소프트웨어 구현이 정의된 안전 및 기능 요구사항을 충족한다는 증거를 확립하는 과정이다. 화물 무인항공기(Cargo UAV)의 경우 자세, 고도, 위치, 모터 할당, 화물 적응, 외란 제거 및 모드 관리를 각각 독립적으로 검증하는 것에 그치지 않고 하나의 통합된 제어 체인(Integrated Control Chain)으로 검증해야 한다.

체계적인 검증 프로세스(Verification Process)는 추적 가능한 요구사항(Traceable Requirements)에서 시작한다. 시스템 수준의 비행 제어 요구사항은 제어 동작, 인터페이스, 타이밍, 제한값, 고장 대응 및 모드 전환을 정의하는 소프트웨어 상위 수준 요구사항(High-Level Requirements)과 하위 수준 요구사항(Low-Level Requirements)으로 분해된다. 각 요구사항은 하나 이상의 검증 활동과 연결되어 시험 결과가 무엇을 검증했으며 해당 증거가 의도된 FCS 동작과 어떤 관련성을 갖는지를 입증할 수 있어야 한다.

소프트웨어 인 더 루프 시험(Software-in-the-Loop Testing, SIL)은 실제 비행 컴퓨터나 추진 하드웨어를 사용하지 않고 시뮬레이션된 항공기 동역학과 비행 제어 소프트웨어를 연동하여 실행한다. 시뮬레이터는 센서 및 항법 데이터를 제공하고 액추에이터 명령을 수신하며 공기역학 및 추진 모델을 이용하여 항공기 상태를 전파한다. SIL을 이용하면 실제 하드웨어 통합이 가능해지기 전 개발 초기 단계부터 반복 가능한 많은 시험 시나리오를 수행할 수 있다.

SIL 환경은 정상적인 제어 법칙 동작(Control-Law Behavior)을 평가하는 데 특히 유용하다. 계단 입력(Step Input), 램프 입력(Ramp Input), 궤적 및 외란 입력을 이용하여 상승 시간, 정착 시간, 오버슈트(Overshoot), 정상 상태 오차 및 추종 정확도를 측정할 수 있다. 화물 질량, 무게중심, 바람, 센서 잡음, 액추에이터 제한 및 모델 매개변수를 체계적으로 변경할 수 있으며, 자동화된 회귀 시험(Automated Regression Testing)을 통해 제어 소프트웨어나 구성 매개변수가 변경될 때마다 이러한 시나리오를 반복할 수 있다.

시뮬레이션 충실도(Simulation Fidelity)는 검증 목적에 적합해야 한다. 단순한 강체 모델(Rigid-Body Model)은 초기 제어기 개발을 효율적으로 지원할 수 있으며, 고충실도 모델(High-Fidelity Model)에는 추진 동역학, 공기역학적 항력, 액추에이터 지연, 센서 오차, 유연 화물 영향 및 현수 화물 운동을 포함할 수 있다. 시뮬레이션을 불필요하게 복잡하게 만들 필요는 없지만 각 시험에서 다루는 고장 형태와 성능 한계를 드러내는 데 필요한 물리적 효과는 반드시 표현해야 한다.

SIL은 실제 비행에서 반복적으로 재현하기 어렵거나 위험한 비정상 조건 및 고장 주입 시험(Fault-Injection Testing)도 지원한다. 위성항법시스템(Global Navigation Satellite System, GNSS) 손실, 추정기 발산(Estimator Divergence), 센서 바이어스, 통신 중단, 모터 성능 저하, 액추에이터 포화, 과도한 바람, 화물 정보 오류 및 잘못된 모드 명령을 통제된 시점에 주입할 수 있다. 이후 발생하는 모드 전환과 폴백 동작(Fallback Behavior)을 명확한 안전 및 제어 요구사항과 비교할 수 있다.

하드웨어 인 더 루프 시험(Hardware-in-the-Loop Testing, HIL)은 실제 또는 대표적인 비행 제어 컴퓨터에서 양산용 비행 소프트웨어를 실행하고 실시간 시뮬레이터가 항공기와 환경을 모사함으로써 검증 범위를 확장한다. 실제 프로세서 타이밍, 통신 인터페이스, 입출력 드라이버(I/O Driver), 스케줄링 동작 및 하드웨어 의존 소프트웨어가 시험 대상에 포함된다. 따라서 HIL은 알고리즘 수준의 시뮬레이션과 실제 항공기 시험 사이의 간극을 연결한다.

HIL 시험에서는 실시간 동기화(Real-Time Synchronization)가 필수적이다. 항공기 시뮬레이션은 실제 기체를 대표하는 타이밍 관계에 따라 센서 업데이트를 생성하고 액추에이터 명령을 수신해야 한다. 인위적인 지연시간(Latency), 타이밍 지터(Timing Jitter), 메시지 손실, 지연된 센서 측정값 및 통신 고장을 의도적으로 주입할 수 있다. 이러한 시험은 실행 타이밍이 이상적이거나 실제 시간보다 훨씬 빠른 SIL 환경에서는 발견하기 어려운 문제를 드러낼 수 있다.

HIL 환경은 중요한 비행 제어 인터페이스를 충분한 현실성으로 재현해야 한다. 관성측정장치(Inertial Measurement Unit, IMU), GNSS, 기압계(Barometer), 레이더 또는 레이저 고도 데이터, 대기자료(Air-Data Information), 추진 시스템 피드백, 전력 시스템 상태, 화물 정보 및 명령 링크를 필요에 따라 시뮬레이션하거나 전기적으로 모사할 수 있다. 모터 출력을 수집하여 시뮬레이션 추력으로 변환하면 실제 항공기를 비행시키지 않고도 전체 폐루프 제어(Closed Control Loop)를 동작시킬 수 있다.

캐스케이드 제어기 구조(Cascaded Controller Structure)의 검증에서는 제어 루프 대역폭 사이의 상호작용을 평가해야 한다. 각속도 및 자세 루프는 고도, 속도 및 위치 루프가 변화하는 기준값을 생성하는 동안에도 안정성을 유지해야 한다. 시험에는 공격적인 궤적, 빠른 명령 반전, 포화 및 외란 복구가 포함되어야 한다. 목적은 개별 루프의 성능뿐만 아니라 여러 제어 목표가 상호작용할 때 전체 계층 구조가 안정적으로 유지되는지를 검증하는 것이다.

모터 믹싱(Motor Mixing)과 제어 할당(Control Allocation)은 상위 제어기의 출력이 올바르다고 해서 액추에이터 명령까지 자동으로 올바른 것은 아니므로 별도의 검증이 필요하다. 시험에서는 로터 번호, 회전 방향, 힘과 모멘트의 부호, 할당 계수, 포화 동작, 우선순위 규칙 및 성능 저하 구성을 확인해야 한다. 잘못된 할당 형상(Allocation Geometry)은 상위 자세 및 위치 제어기가 수학적으로 정확하더라도 위험한 기체 동작을 발생시킬 수 있다.

화물 특화 검증(Cargo-Specific Verification)은 검증된 질량, 관성 및 무게중심 운용 영역 전체를 포함해야 한다. SIL 및 HIL 시나리오에서 화물 구성을 체계적으로 변화시키고 필요한 경우 화물 픽업, 방출, 비대칭 적재 및 현수 화물 조건을 시험할 수 있다. 부하 적응 매개변수(Load-Adaptive Parameter)는 허용 범위 내에 유지되어야 하며, 전환은 안정적으로 이루어지고 활성 비행 모드에서 허용되는 기동 한계를 충족할 만큼 충분한 제어 권한이 확보되어야 한다.

바람 외란 시험(Wind-Disturbance Testing)은 정상풍, 돌풍, 난류 및 수직 기류를 대표적인 화물 조건과 결합해야 한다. 검증 환경에서는 제어기가 외란을 완벽하게 알고 있는 상황을 방지하기 위해 추정기 모델과 독립적으로 공기역학적 외란을 주입할 수 있다. 이후 위치 편차, 자세 변동, 추정기 수렴(Estimator Convergence), 액추에이터 사용량 및 남아 있는 추력 여유를 정의된 합격 기준(Acceptance Criteria)에 따라 평가할 수 있다.

비행 모드 검증(Flight-Mode Verification)은 전환과 권한 관리(Authority Management)에 특히 중점을 두어야 한다. 수동, 반자동 및 완전 자율 모드는 서로 다른 위치, 속도, 자세 및 시스템 상태 조건에서 진입하고 종료되어야 한다. 시험에서는 전환 가드(Transition Guard), 기준값 초기화, 적분기 처리, 수동 오버라이드(Manual Override), 통신 손실 대응, 항법 성능 저하 및 사전에 정의된 안전 구성으로의 자동 전환을 검증해야 한다.

DO-178C는 항공 소프트웨어(Airborne Software)가 적절한 수준의 엄격성을 바탕으로 의도된 기능을 수행한다는 보증을 확보하기 위한 프레임워크를 제공한다. 검증 관점에서는 요구사항 기반 시험(Requirements-Based Testing), 추적성(Traceability), 검토(Review), 분석(Analysis), 형상 관리(Configuration Control), 필요한 경우의 독립성(Independence), 객관적 증거(Objective Evidence)를 강조한다. 이러한 원칙을 화물 UAV 비행 제어 소프트웨어에 적용하려면 최종 비행 시험만을 소프트웨어 정확성의 주요 증명 수단으로 사용하는 것이 아니라 체계적인 개발 생명주기(Development Lifecycle)를 구축해야 한다.

소프트웨어 보증의 엄격성(Software Assurance Rigor)은 할당된 소프트웨어 수준(Software Level)을 통해 소프트웨어 고장이 초래하는 결과와 연계된다. 중요도가 높은 소프트웨어에는 더욱 강력한 검증 목표와 높은 수준의 독립성이 요구된다. 적용 수준은 단순히 해당 소프트웨어가 비행 제어를 수행한다는 이유로 선택하는 것이 아니라 시스템 안전 및 개발 보증 프로세스(System Safety and Development-Assurance Process)를 통해 결정된다. 따라서 FCS 개발에서는 시스템 안전 평가, 소프트웨어 요구사항, 구현 및 검증 증거 사이의 명확한 관계를 유지해야 한다.

요구사항 기반 시험은 항공기가 겉보기에 정상적으로 비행하는지를 단순히 확인하는 것이 아니라 실행 가능한 소프트웨어가 명시된 동작을 충족하는지를 검증한다. 각 시험은 검증 대상 요구사항, 초기 조건, 입력 순서, 예상 결과, 실제 결과 및 합격 또는 불합격 판정을 식별해야 한다. 이러한 구조는 검증의 반복 가능성을 확보하며 소프트웨어나 요구사항이 변경될 경우 해당 변경과 관련된 회귀 시험을 수행할 수 있도록 한다.

구조적 커버리지 분석(Structural Coverage Analysis)은 요구사항 기반 시험에서 구현된 소프트웨어의 어느 부분이 실제로 실행되었는지를 분석하여 보완적인 검증 증거를 제공한다. 적용되는 보증 목표에 따라 문장 커버리지(Statement Coverage), 결정 커버리지(Decision Coverage), 수정 조건/결정 커버리지(Modified Condition/Decision Coverage, MC/DC)가 관련될 수 있다. 커버리지 분석은 요구사항 기반 시험을 대체하지 않으며, 실행되지 않은 구조는 누락된 시험, 불완전한 요구사항 또는 조사해야 할 의도하지 않은 기능을 드러낼 수 있다.

견고성 시험(Robustness Testing)은 이상적인 정상 입력 범위를 벗어난 조건에서의 동작을 평가한다. 경계값, 잘못된 메시지, 오래된 데이터(Stale Data), 수치적 극한값, 센서 손실, 타이밍 위반 및 서로 일관되지 않은 상태 정보를 의도적으로 시험해야 한다. 비행 제어 소프트웨어는 제어되지 않은 출력, 수치적 불안정성 또는 정의되지 않은 모드 전환 없이 결정론적으로 대응해야 한다. 방어적 동작(Defensive Behavior) 자체도 문서화되지 않은 구현 동작으로 추가되는 것이 아니라 정의된 요구사항과 연결되어야 한다.

검증에서는 제어 소프트웨어의 수치적 특성(Numerical Properties)도 다루어야 한다. 부동소수점 한계(Floating-Point Limit), 포화 연산, 작은 값에 의한 나눗셈, 행렬 조건(Matrix Conditioning), 쿼터니언 정규화(Quaternion Normalization), 좌표 변환 및 필터 초기화는 정상적인 시뮬레이션에서는 나타나지 않는 고장을 발생시킬 수 있다. 장시간 시험은 누적되는 수치 드리프트, 점진적으로 증가하는 적분기 상태, 타임스탬프 문제 및 자원 관련 영향을 식별하는 데 유용하다.

추적성은 시스템 요구사항, 소프트웨어 요구사항, 소스 코드, 시험 사례(Test Case), 시험 절차 및 결과를 감사 가능한 증거 체인(Auditable Evidence Chain)으로 연결한다. 양방향 추적성(Bidirectional Traceability)은 모든 요구사항이 구현되고 검증되었음을 입증하는 동시에 구현된 소프트웨어가 식별 가능한 요구사항을 근거로 존재한다는 사실을 확인하는 데 도움을 준다. 이는 안전 로직, 제어 법칙, 추정기 인터페이스 및 모드 관리 기능이 광범위하게 상호작용하는 FCS에서 특히 중요하다.

형상 관리(Configuration Management)는 검증 증거가 명확하게 식별된 소프트웨어 및 하드웨어 기준선(Baseline)에 대응하도록 한다. 제어기 이득, 기체 매개변수, 믹싱 행렬, 화물 제한, 시뮬레이터 모델, 시험 스크립트, 컴파일러 설정 및 비행 컴퓨터 소프트웨어 버전도 소스 코드와 함께 관리되어야 한다. 그렇지 않으면 성공적인 시험 결과가 실제 항공기에 설치되는 최종 구성에 대한 의미 있는 검증 증거가 되지 못할 수 있다.

문제 보고(Problem Reporting)와 회귀 시험(Regression Testing)은 검증 피드백 주기를 완성한다. 실패한 시험과 예상하지 못한 동작은 문서화하고 분석한 후 수정하고, 영향을 받는 요구사항 및 소프트웨어 구성요소와의 추적성을 유지하면서 다시 시험해야 한다. 하나의 제어 기능 변경이 다른 제어 루프에 영향을 줄 수 있으므로 자동화된 SIL 회귀 시험과 선별된 HIL 회귀 시나리오는 통합 FCS 전체에서 의도하지 않은 영향을 탐지하는 데 유용하다.

SIL 및 HIL 검증이 지상 시험(Ground Testing)과 비행 시험(Flight Testing)의 필요성을 제거하는 것은 아니다. 대신 항공기가 더 높은 위험 수준의 시험 단계로 진입하기 전에 불확실성을 감소시키는 역할을 한다. 비행 시험은 실제 공기역학, 추진 시스템 변동, 구조적 상호작용, 환경 외란 및 운용 절차와 같이 시뮬레이션에서 완전하게 표현하기 어려운 영향을 검증한다. 비행 시험 결과는 모델의 부족한 부분을 발견하여 이후의 시뮬레이션 및 회귀 시험에 반영하는 데도 사용할 수 있다.

성숙한 비행 제어 시스템 검증 전략(FCS Verification Strategy)은 따라서 요구사항과 분석에서 시작하여 SIL, HIL, 지상 통합 시험(Ground Integration Testing), 통제된 비행 시험으로 이어지는 단계적 구조를 형성한다. 각 단계에서는 이전 단계에서 확보한 검증 증거를 유지하면서 물리적 현실성을 점진적으로 증가시킨다. DO-178C 지향 요구사항 추적성, 커버리지, 형상 관리, 검토 및 반복 가능한 시험 기록과 결합하면 이러한 과정은 화물 무인항공기 비행 제어 소프트웨어가 정상, 경계, 성능 저하 및 고장 조건에서 예측 가능한 동작을 유지한다는 체계적인 보증을 제공한다.
