---
layout: single
title:  "파이썬 기초 연습 코드"
categories: BootCamp
tag: [Python, pratice]
toc: true
author_profile: false
sidebar: 
    nav: "docs"
search: false

excerpt: "조건문과 문자열 함수를 활용한 간단한 파이썬 연습"
---

## 문자열에서 문자 제거와 추가해보기

### 코드 설명 
이 코드는 부트캠프에서 학습한 조건문과 문자열 함수를 연습하기 위한 용도입니다.

### 코드 구현 
```python
#연산자와 조건문을 활용한 문자열에서 문자 제거 및 추가하기
#입력받은 문자열과 문자는 모두 소문자로 변경 -> 대소문자 구분하지 않기위해
#이미 존재하는 문자는 대문자로 변경
#존재하지 않는 문자를 제거하면 존재하지 않는다고 알림
#입력된 문자열과 문자는 자동으로 공백 제거

op = (input("연산자 입력(+,-): ")).strip()
input_string = (input("문자열 입력 (2글자 이상, 소문자): ")).strip().lower()
input_char = (input("문자입력 (1글자): ")).strip().lower()
result_string = ""
idx = -1

#간단한 입력 검증
if len(input_string)<2:
    print("2글자 이상을 입력하지 않았습니다.")
elif len(input_char) != 1:
    print("1글자만 입력해야 합니다.")
elif op not in ["+", "-"]:
    print("연산자는 '+' 와 '-' 만 가능합니다.")
else:
    idx = input_string.find(input_char)


if op == "+":
    if idx == -1:
        result_string = input_string + str(input_char)
    elif idx != -1:
        result_string = input_string[:idx]+input_string[idx].upper()+input_string[idx+1:]
elif op == "-":
    if idx == -1:
        print("존재하지 않는 문자입니다.")
    elif idx != -1:
        result_string = input_string.replace(input_char, '', 1)

print(result_string)

```

### 결과 
#### 연산자 + 결과 
![image](/img/python_pratice1_result1.png)
#### 연산자 - 결과 
![image](/img/python_pratice1_result2.png)

### 자체 피드백
- 현재는 하나의 문자열과 한 글자만 처리할 수 있어서 유연성이 부족합니다.
- 입력단계에서 문제가 발생하면 재입력받지 않고 종료합니다.
- 연산자라는 입력을 받아 결과를 처리하는데 직관성이 부족합니다. 