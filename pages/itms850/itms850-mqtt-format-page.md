---
title: MQTT 데이터 포맷 
keywords: simulator
summary: "스마트콤보(ITMS-850) 디바이스에서 사용하는 MQTT 데이터 포맷에 대해서 설명하고 있는 기술자료입니다."
sidebar: itms850_sidebar
permalink: itms850-mqtt-format-page.html
folder: itms850
---

## 데이터 구성 ##

스마트콤보(ITMS-850) 디바이스에서 MQTT 서버로 전송하는 데이터는 Json형식이며, 다음과 같이 구성되어 있습니다.

| Key | Data Type | Description | Example |
|-----|-----------|-------------|---------|
|"time"| String   | 데이터 송신시간 | "2022-08-23T12:10:27.000300Z" |
|"eui" | Integer | 콤보디바이스 ID | 1001 |
|"payload" | List | 센서 데이터 | [1,0,0,0,0,0,0,0,0,0,4,22,0,220,0,0,0,0,0,62,37,115,0,0,0,0] |
|"gateway_id" | Integer | 게이트웨이 ID(사업장 구분코드로 활용) | 1 |

**샘플 데이터**
```
{"time":"2022-08-23T12:10:27.000300Z","eui":1001,"payload":[1,0,0,0,0,0,0,0,0,0,4,22,0,220,0,0,0,0,0,62,37,115,0,0,0,0],"gateway_id":2}
```

**Payload 데이터 포맷**

|offset|Data|Unit|Description|
|:----:|----|----|-----------|
|0| Command Code | N/A | 스마트콤보 센서인 경우 0x01로 고정값 사용 |
|1| Header | N/A | * bit 7 : N/A <br> * bit 6 : N/A <br> * bit 5 : high인 경우 압력값 부호 -, low인 경우 압력값 부호 + <br> * bit 4 : high인 경우 온도값 부호 -, low인 경우 온도값 부호 + <br> * bit 3 : DI Port 4 Output <br> * bit 2 : DI Port 3 Output <br> * bit 1 : DI Port 2 Output <br> * bit 0 : DI Port 1 Output |
|2~3| AI Port 1 Output |mA| 4~20mA AI 출력 값 (Big Endian)|
|4~5| AI Port 2 Output |mA| 4~20mA AI 출력 값 (Big Endian)|
|6~7| Differential Pressure Sensor |mmH2O| 차압 센서 값 (Big Endian) |
|8~9| Pressure Sensor | mBar | 압력 센서 값 (Big Endian) |
|10~11| Temperature | ℃ | 온도 값 (Big Endian) <br> **참고)** 온도 값을 100으로 나눠서 사용해야 합니다.  |
|12~13| Voltage | V | 전압 값 (Big Endian) |
|14~17| Current | mA | 전류 값 (Big Endian) |
|18~21| Power | wH | 소비 전력 값 (Big Endian) |
|22~25| Active Power | W | 유효 전력 값 (Big Endian) |

## 관련 페이지

### 테스트 툴

<ul>
<li>
    <a href="./itms850-mqtt-parser-tool.html">ITMS-850 MQTT 데이터 분석툴</a>
</li>
</ul>

### 예제 소스코드 (Github)

<ul>
<li>
    <a href="https://github.com/aerixdev/itms850_dataparser" target="_blank">스마트콤보(제품명 : ITMS-850) 데이터 MQTT 수신 예제</a> <br>
</li>
<li>
    <a href="https://github.com/aerixdev/itms850_simulator" target="_blank">스마트콤보(제품명 : ITMS-850) MQTT 시뮬레이터</a>
</li>

