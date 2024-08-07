---
layout: single
title:  "파이썬 기초 연습 코드3"
categories: BootCamp
tag: [Python, practice, while문, if문, set]
toc: true
author_profile: false
sidebar: 
    nav: "docs"
search: false

excerpt: "기존의 파이썬 코드에서 자료구조 사용"
---

## 자료구조를 추가하고 이용하기

### 코드 설명 
부트캠프에서 파이썬의 자료구조에 대해 학습했습니다. 그 중 set() 자료구조 변수를 추가하여 코드를 구성하였습니다.
이에따라 기존의 코드에서 데이터를 추가하거나 삭제할때 add()함수와 remove()함수를 사용하였습니다.

### 코드 구현 
```python

# 집합 사용한 문자열 집합 관리 프로그램
string_set = set() # 문자열 집합 생성

'''
현재 이 코드는 연산자, 문자열, 문자입력을 하면 
'+' 연산자의 경우 두 가지 경우(존재하면 알파벳을 대문자로, 존재하지 않으면 새로 추가)
'-' 연산자의 경우 문자 삭제 
'*' 연산자의 경우 문자를 반복해서 추가
'''

while True:
    input_char = '' # 문자 입력
    result_string = "" # 결과 문자열
    input_num = 0

    #연산자 입력
    op = input("연산자 입력 (+, -, *, /): ").strip()

    if op not in ["+", "-", "/", "*"]:
        print("연산자는 '+', '-', '/', '*' 만 가능합니다.")
        continue

    if op == "/":
        print("종료")
        break

    # 문자열 입력
    input_string = input("문자열 입력 (2글자 이상): ").strip()

    # 간단한 입력 검증
    if len(input_string) < 2:
        print("2글자 이상을 입력하지 않았습니다.")
        continue

    # '*' 연산자인 경우 숫자를 추가로 입력받아 문자열 반복횟수를 정함
    if op == "*":
        input_num = int(input("숫자 입력: "))
        input_char = input("문자입력 (1글자): ").strip()
    # '+', '-' 연산자인 경우 문자를 입력받고
    elif op in ["+", "-"]:
        input_char = input("문자입력 (1글자): ").strip()
        if len(input_char) != 1:
            print("1글자만 입력해야 합니다.")
            continue


    # 집합에 입력 문자열이 있는지 확인
    if input_string in string_set:
        idx = input_string.find(input_char)
    else: # 존재하지 않으면 idx = -1 로 설정
        idx = -1

    if op == "+":
        if idx == -1:
            # 문자열이 집합에 없으면 추가
            string_set.add(input_string)
            print(f"'{input_string}' 추가하였습니다.")
        else:
            # 문자열이 집합에 있으면 삭제하고 다시 대문자로 변경해서 추가
            string_set.remove(input_string)
            result_string = input_string[:idx] + input_string[idx].upper() + input_string[idx + 1:]
            string_set.add(result_string)
            print(f"'{input_string}'에 '{input_char}'가 존재하여 대문자로 변경하고'{result_string}' 추가하였습니다.")
        print("현재 문자열 집합:", string_set)


    elif op == "-":
        if idx == -1: # 존재하지 않는 문자는 제거할 수 없으므로 제거할 수 없음을 알려줌
            print(f"'{input_char}'는 '{input_string}'에 존재하지 않습니다.")
        else: # 입력 문자열을 삭제한 후 대문자로 변경한 이후 다시 집합에 추가
            string_set.remove(input_string)
            result_string = input_string.replace(input_char, '', 1)
            if result_string == "":
                print(f"'{input_string}'에서 '{input_char}'를 제거하여 빈 문자열이 되어 제거하였습니다.")
            else:
                string_set.add(result_string)
                print(f"'{input_string}'에서 '{input_char}'를 제거하여 '{result_string}' 입니다.")
            print("현재 문자열 집합:", string_set)

        # print("현재 문자열 집합:", string_set)


    elif op == "*":
        # 문자열에 입력된 문자를 주어진 숫자만큼 추가
        result_string = input_string + str(input_char) * input_num
        string_set.add(result_string)
        print(f"'{input_string}'에 '{input_char}'를 {input_num}번 반복하여 '{result_string}' 입니다.")
        print("현재 문자열 집합:", string_set)

```

### 결과 
![image](/img/python_pratice3_result.png)

### 기존과의 차이
- 연산의 결과 이후 print문을 추가하여 변경된 사항을 파악할 수 있도록 수정하였습니다. 
- set 에 있는 내장 메서드를 사용하여 요소를 추가하고 제거하였습니다.

### 개선점
- 충분한 예외처리가 되지 않아 에러 처리 능력이 부족합니다.
- 함수를 사용하지 않아 가독성이 떨어지고 동일한 구문이 반복되고 있습니다.

### 해결방안 
- try-except 블록을 사용하여 숫자 입력에서 발생할 수 있는 예외를 처리할 것입니다.
- 입력 검증과 메시지 출력 로직을 함수로 분리하여 중복을 줄일 것입니다.