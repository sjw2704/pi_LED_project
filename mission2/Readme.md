# Mission2 - Raspberry Pi GPIO & Python

## **1. 📝개요**

미션을 수행하며 python을 활용한 라즈베리 파이의 GPIO 제어 방법에 대해서 익힙니다. 미션을 다음과 같습니다.

1. 버튼이 눌린 동안만 LED 켜기
2. 버튼이 눌릴 때마다 LED 토글 시키기
3. 버튼이 눌리면 domino4 1회 실행하기
4. 버튼이 눌릴 때마다 4-bit 카운터 값 증가시키기

## **2. 🎥 시연 영상**

[201901975 서종원 6주차 과제 영상](https://youtu.be/y4akjE05cgk?si=dhOpd2MYIXaRDHo4)

## **3. 🕹️회로 구성**

<img src= "https://github.com/sjw2704/pi_LED_project/blob/main/mission2/images/IMG_0361.jpg" width="300" />

| **PIN** | 부품 |
| --- | --- |
| **G21** | Red_LED |
| **G20** | Red_LED |
| **G19** | Red_LED |
| **G18** | Red_LED |
| **G25** | Button |
| **Resistor** | 330Ohm X4 |

## **4. 💻코드 구성**

### Mission2_1 : 버튼이 눌린 동안만 LED 켜기

```python
#!/usr/bin/env python3

from gpiozero import LED, Button
import time

# 핀 번호 설정 (BCM 모드 기준)
OUTPUT_PIN = 21  # 출력 핀
INPUT_PIN = 25   # 버튼 입력 핀

# gpiozero는 내부적으로 핀 모드를 설정해줌
led = LED(OUTPUT_PIN)
button = Button(INPUT_PIN, pull_up=True)  # 내부 풀업 설정 (bash의 pinctrl set 25 pu와 같음)

try:
    while True:
        if button.is_pressed:  # 버튼이 눌리면 lo 상태
            print("lo")
            led.on()  # dh: drive high
        else:
            led.off()  # dl: drive low
        time.sleep(0.3)

except KeyboardInterrupt:
    print("종료합니다.")
```

### Mission2_2 : 버튼이 눌릴 때마다 LED 토글 시키기

```python
#!/usr/bin/env python3

from gpiozero import Button, LED
from signal import pause
from time import sleep

# 핀 설정 (BCM 번호 기준)
OUTPUT_PIN = 21 # 출력 핀
INPUT_PIN = 25 # 버튼 입력 핀

led = LED(OUTPUT_PIN) # LED 핀 설정
button = Button(INPUT_PIN, pull_up=True) # 내부 풀업 설정
# Falling edge (버튼 누를 때) LED 0.5초 ON 
def flash_led():
    led.on()
    sleep(0.5)
    led.off()

# Falling edge 감지 시 flash_led 실행
button.when_pressed = flash_led

pause()  # 프로그램이 종료되지 않도록 대기
```

### Mission2_3 : 버튼이 눌리면 domino4 1회 실행하기

```python
#!/usr/bin/env python3

from gpiozero import Button, LED
from signal import pause
from time import sleep

# 핀 설정 (BCM 번호 기준)
LED_PIN = [21, 20, 19, 18]
INPUT_PIN = 25 # 버튼 입력 핀

leds = [LED(pin) for pin in LED_PIN]
button = Button(INPUT_PIN, pull_up=True, bounce_time=0.1)
# Falling edge (버튼 누를 때) LED 0.5초 ON 
def domino_led():
    for led in leds:
        led.on()
        sleep(1)
        led.off()

# Falling edge 감지 시 flash_led 실행
button.when_pressed = domino_led

pause()  # 프로그램이 종료되지 않도록 대기
```

### Mission2_4 : 버튼이 눌릴 때마다 4-bit 카운터 값 증가시키기

```python
#!/usr/bin/env python3

from gpiozero import Button, LED
from signal import pause

# 핀 설정 (BCM 번호 기준)
LED_PINS = [21, 20, 19, 18]  # LSB부터 MSB 방향이라고 가정
INPUT_PIN = 25

# LED 객체 리스트로 만들기
leds = [LED(pin) for pin in LED_PINS]
button = Button(INPUT_PIN, pull_up=True, bounce_time=0.1)

# 카운터 초기화
count = 0

# 버튼 눌릴 때 실행할 함수
def update_leds():
    global count
    count = (count + 1) % 16  # 0~15까지만 유지 (4-bit)

    binary = f"{count:04b}"  # count를 4비트 이진수 문자열로 변환
    print(f"count = {count} -> binary = {binary}") # 디버깅용 출력

    for i in range(4):
        if binary[3 - i] == '1':  # MSB부터 처리
            leds[i].on()
        else:
            leds[i].off()

# Falling edge에 반응
button.when_pressed = update_leds

pause()
```

## **5. 👨‍🏫사용 방법**

- nano 텍스트 편집기를 이용하여 파일 생성 (다른 편집기도 사용 가능)
    - `nano /파일이름/파일이름…`
- 코드 입력
- `ctrl + c`로 저장후 enter
- `ctrl + x`로 텍스트 편집기 나오기
- `chmod +x 파일이름`로 권한 부여
- `python3 파일이름`으로 파일 실행
