# Dust Sensor Arduino와 Python을 이용한 데이터 수집 및 InfluxDB 저장

이 프로젝트는 Arduino에서 측정된 미세먼지 센서 데이터를 Python을 이용해 읽어들인 후, InfluxDB에 저장하는 시스템입니다. Arduino와 Python 간의 시리얼 통신을 통해 실시간으로 미세먼지 농도 데이터를 InfluxDB에 저장하고, 이를 후속 분석에 사용할 수 있습니다.

## 필요 부품

- Arduino 보드 (예: Arduino Uno)
- 미세먼지 센서 (예: GP2Y1010AU0F)
- 점퍼 와이어
- Python 환경 (필요한 라이브러리 설치 포함)
- InfluxDB 서버 (로컬 또는 클라우드 인스턴스)

## 사용 방법

### 1. Arduino 코드
Arduino 보드에서 미세먼지 센서의 데이터를 읽어들인 후, 시리얼 통신을 통해 Python 코드로 데이터를 전송합니다. Arduino 코드는 기본적으로 센서 데이터를 "key=value" 형식으로 전송합니다.

### 2. Python 코드

Python 코드는 시리얼 포트를 통해 Arduino로부터 미세먼지 농도 데이터를 읽어들인 후, InfluxDB에 데이터를 저장합니다. InfluxDB는 시계열 데이터를 처리하기 위한 데이터베이스입니다.

#### Python 코드 실행 준비

1. **필요한 Python 라이브러리 설치**

   Python 환경에서 `serial`, `influxdb_client`, `time` 라이브러리가 필요합니다. 필요한 라이브러리를 설치하려면 아래 명령어를 사용하세요:

   ```bash
   pip install pyserial influxdb-client
   ```

2. **InfluxDB 설정**

   InfluxDB 서버가 로컬 또는 클라우드에 설치되어 있어야 하며, 해당 서버에 대한 URL, 토큰, 조직명, 버킷명을 설정합니다. 아래와 같이 `influxdb_url`, `influxdb_token`, `influxdb_org`, `influxdb_bucket` 값을 설정해야 합니다:

   ```python
   influxdb_url = "http://localhost:8086"
   influxdb_token = "YOUR_INFLUXDB_TOKEN"
   influxdb_org = "YOUR_ORG_NAME"
   influxdb_bucket = "YOUR_BUCKET_NAME"
   ```

3. **Python 코드 실행**

   Python 코드를 실행하면, Arduino에서 보내는 데이터가 실시간으로 시리얼 포트를 통해 읽어들여지고, 읽힌 데이터는 InfluxDB에 기록됩니다.

### 3. 코드 설명

- **Serial Communication**:
  - Python 코드에서 `serial.Serial()`을 사용하여 Arduino와의 시리얼 통신을 설정합니다.
  - `ser.readline()`으로 Arduino로부터 데이터를 한 줄씩 읽어옵니다.
  - 읽어온 데이터에서 키와 값을 분리하여 InfluxDB에 기록합니다.

- **InfluxDB**:
  - InfluxDBClient를 사용하여 데이터베이스에 연결하고 데이터를 기록합니다.
  - 데이터는 `sensor_data`라는 측정항목(measurement) 아래에 기록됩니다. 데이터 형식은 `"sensor_data,device=arduino {key}={value}"` 입니다.

- **Error Handling**:
  - 데이터 형식이 잘못되었을 경우 `ValueError`를 처리하여 오류를 출력합니다.
  - 키보드 인터럽트(Ctrl+C)로 프로그램을 종료할 수 있습니다.

### 4. 실행 예시

```bash
python dust_sensor_arduino.py
```

프로그램 실행 후, Arduino로부터 미세먼지 농도 데이터가 수신되면 InfluxDB에 기록됩니다. 예를 들어, `dust_density`라는 키가 Arduino에서 보내어진다면, InfluxDB에서 아래와 같은 데이터가 기록됩니다:

```
sensor_data,device=arduino dust_density=12.34
```

### 5. InfluxDB에서 데이터 확인

InfluxDB에 저장된 데이터를 쿼리하여 확인할 수 있습니다. 예를 들어, InfluxDB의 **Data Explorer**에서 `sensor_data` 측정항목에 대한 쿼리를 실행하여 저장된 미세먼지 농도 값을 확인할 수 있습니다.

## 주의 사항

- **시리얼 포트 설정**: `serial_port` 변수에서 실제 Arduino가 연결된 시리얼 포트를 설정해야 합니다. 예를 들어, Windows에서는 `COMx`, macOS/Linux에서는 `/dev/ttyUSB0`와 같은 형식을 사용합니다.
  
- **InfluxDB 서버**: 로컬 환경에서 InfluxDB 서버를 실행 중이어야 하며, 서버 URL과 인증 정보가 정확하게 설정되어야 합니다.
