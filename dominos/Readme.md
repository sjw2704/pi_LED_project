# Dominos LED Project

## **1. 📝개요**

<img src= "https://github.com/sjw2704/pi_LED_project/blob/main/dominos/images/dominos_ex.png" width="300" />

이 프로젝트는 4개의 LED가 도미노와 같이 1초 간격으로 순서대로 켜지고 꺼지도록 설계되었습니다.

## **2. 🎥 시연 영상**

[201901975 서종원 dominos mission](https://youtu.be/PACQUmWjisE?si=0zH98LLbTcU3jSqP)

## **3. 🕹️회로 구성**

<img src= "https://github.com/sjw2704/pi_LED_project/blob/main/dominos/images/dominos_circuit.png" width="500" />

*팅커캐드에 라즈베리파이가 없는 관계로 아두이노로 대체하였습니다. 정확한 핀 번호는 아래 표를 참고해주시면 감사하겠습니다.

| **PIN** | 부품 |
| --- | --- |
| **G21** | Red_LED |
| **G20** | Red_LED |
| **G19** | Red_LED |
| **G18** | Red_LED |

## **4. 💻코드 구성**

```bash
#!/bin/bash

PINS=(21 20 19 18) # 사용하는 핀 번호들

# 핀 모드 설정
for PIN in "${PINS[@]}"; do
  pinctrl set $PIN op # op: output mode
done

# 순차적으로 점등
while true; do
  for PIN in "${PINS[@]}"; do
    pinctrl set $PIN dh # dh: high
    sleep 1
    pinctrl set $PIN dl # dl: low
  done
done
```

## **5. 👨‍🏫사용 방법**

- nano 텍스트 편집기를 이용하여 파일 생성 (다른 편집기도 사용 가능)
    - `nano /파일이름/파일이름…`
- 코드 입력
- `ctrl + c`로 저장후 enter
- `ctrl + x`로 텍스트 편집기 나오기
- `./파일이름`으로 파일 실행
