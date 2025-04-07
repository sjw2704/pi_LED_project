# Count8 LED Project

## **1. 📝개요**

<img src= "https://github.com/sjw2704/pi_LED_project/blob/main/count_8/images/count_8_ex.png" width="300" />

이 프로젝트는 3개의 LED가 1초 간격으로 0~7까지의 숫자를 나타냅니다. 각 LED는 비트를 의미합니다. 

## **2. 🎥 시연 영상**

[201901975 서종원 count8 mission](https://youtu.be/P0GNWgpgyLE?si=IXShPqnLCRYGFmLB)

## **3. 🕹️회로 구성**

<img src= "https://github.com/sjw2704/pi_LED_project/blob/main/count_8/images/count8_circuit.png" width="500" />

*팅커캐드에 라즈베리파이가 없는 관계로 아두이노로 대체하였습니다. 정확한 핀 번호는 아래 표를 참고해주시면 감사하겠습니다.

| **PIN** | 부품 |
| --- | --- |
| **G21** | Red_LED |
| **G20** | Red_LED |
| **G19** | Red_LED |

## **4. 💻코드 구성**

```bash
#!/bin/bash

PINS=(19 20 21) # 사용하는 핀 번호들

# 핀 모드 설정
for PIN in "${PINS[@]}"; do
  pinctrl set $PIN op # op: output mode
done

while true # 무한 루프
do
    for i in {0..7}
    do
        # 0~7값을 비트와 매칭시키기
        for j in {0..2}
        do
            ComparedBit=1 # 각 PIN을 켤지 끌지 결정하는 비트
            Val=$((((ComparedBit << j)) & i )) #비교 후에 Val에 저장
            if [ "$Val" -gt 0 ]; then # Val이 0보다 크면
                pinctrl set ${PINS[$j]} dh # dh: high
            elif [ "$Val" -eq 0 ]; then # Val이 0이면
                pinctrl set ${PINS[$j]} dl # dl: low
            fi
        done
        sleep 1
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
