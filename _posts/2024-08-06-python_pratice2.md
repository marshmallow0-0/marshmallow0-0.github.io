---
layout: single
title:  "파이썬 기초 연습 코드2"
categories: BootCamp
tag: [Python, pratice, while문, if문]
toc: true
author_profile: false
sidebar: 
    nav: "docs"
search: false

excerpt: "기존의 파이썬 코드에서 반복문 추가"
---

## 반복해서 입력받고 조건에 따라 종료하기

### 코드 설명 
부트캠프에서 반복문과 문자열을 늘리는 방법을 추가로 배웠습니다.

### 코드 구현 
```python

# 추가
# 반복해서 입력받기 - "/" 연산자를 입력하면 break로 종료
# "*" 연산자를 사용하면 추가적으로 숫자를 입력받아 문자열을 늘리기
while True:
    input_char = ''
    result_string = ""
    idx = -1
    input_num = 0
    op = (input("연산자 입력(+,-,/,*): ")).strip()
    if op == "/":
        print("종료")
        break

    input_string = (input("문자열 입력 (2글자 이상, 소문자): ")).strip().lower()
    if op == "*":
        input_num = int(input("숫자 입력: "))
        input_char = (input("문자입력 (1글자): ")).strip().lower()

    elif op in ["+", "-"]:
        input_char = (input("문자입력 (1글자): ")).strip().lower()

    # 간단한 입력 검증
    if len(input_string) < 2:
        print("2글자 이상을 입력하지 않았습니다.")
    elif len(input_char) != 1:
        print("1글자만 입력해야 합니다.")
    elif op not in ["+", "-", "/", "*"]:
        print("연산자는 '+', '-', '/', '*' 만 가능합니다.")
    else:
        idx = input_string.find(input_char)

    # 연산자에 따른 동작 실행
    if op == "+":
        if idx == -1:
            result_string = input_string + str(input_char)
        elif idx != -1:
            result_string = input_string[:idx] + input_string[idx].upper() + input_string[idx + 1:]
        print(result_string)

    elif op == "-":
        if idx == -1:
            print("존재하지 않는 문자입니다.")
        elif idx != -1:
            result_string = input_string.replace(input_char, '', 1)
        print(result_string)

    elif op == "*":
        result_string = input_string + str(input_char)*input_num
        print(result_string)

```

### 결과 
![image](/img/python_pratice2_result.png)

### 기존과의 차이
- 입력단계에서 문제가 발생해도 다시 재입력을 받을 수 있게되었습니다.

### 개선점
- 현재는 하나의 문자열과 한 글자만 처리할 수 있어서 유연성이 부족합니다.
- 입력단계에서 문제가 발생하면 재입력받지 않고 종료합니다. ->
- 연산자라는 입력을 받아 결과를 처리하는데 직관성이 부족합니다. -> 파이썬의 GUI 사용  

### 해결방안 
- 직관성 문제는 파이썬 GUI를 사용해볼 예정입니다.
- 하나의 문자열과 한 글자만 처리하는 기능은 후에 리스트를 사용하여 시도할 예정입니다.