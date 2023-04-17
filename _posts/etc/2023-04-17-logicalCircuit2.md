---
layout: post
title:  "[논리회로] 불 대수"
author: yj
category: [ Etc💬 ]
tags: [ etc, 논리회로 ]
---

### <a href="#">논리식의 표현</a>

기본 불 대수식은 AND, OR, NOT을 이용하여 표현한다.
- AND식은 곱셈의 형식 → ‘∙’ 기호 또는 생략 
- OR 식은 덧셈의 형식 → ‘+’ 기호
- NOT식은 Aˊ 로 표현 → ‘not’, ‘바’ 또는 ‘프라임’으로 발음

**계산 우선순위**

1. 괄호
2. NOT
3. AND
4. OR

**논리식**

입력 항목들의 상태에 따른 출력을 결정하는 식
ex) F = A'B

- 입력의 개수에 따라 1입력, 2입력, 3입력..으로 표현

### <a href="#">불 대수 법칙</a>

**공리**

1. A = 0 or A = 1
2. 0 ∙ 0 = 0
3. 1 ∙ 1 = 1
4. 0 + 0 = 0
5. 1 + 1 = 1
6. 1 ∙ 0 = 0 ∙ 1 = 0 
7. 1 + 0 = 0 + 1 = 1

**기본 법칙**

① A+0=0+A=A ② A·1=1·A=A ③ A+1=1+A=1

④ A·0=0·A=0 ⑤ A+A=A ⑥ A·A=A

⑦ A+A'=1 ⑧ A·A'=0 ⑨ A''=A

**교환 법칙**

⑩ A+B=B+A ⑪ AB=BA

**결합 법칙**

⑫ (A + B) + C = A + (B + C) ⑬ (AB) C = A (BC)

**분배 법칙**

⑭ A (B + C) = AB + AC ⑮ A + BC = (A+B)(A+C)

**드모르간 정리**

⑯ A' + B' = A' B' ⑰ A'B' = A' + B'

**흡수 법칙**

⑱ A + AB = A ⑲ A(A+B) = A

**합의의 정리**

⑳ AB + BC + A'C = AB + A'C

**쌍대 관계**

부울 대수에서 하나의 논리식과 다른 논리식 사이에서 다음 관계에 있을 때 쌍대 관계라 한다.
- 모든 And연산자와 OR연산자를 바꾸어 만들어진 논리식
    ex) a∙(b+c) = (a∙b)+(a∙c)

- 하나의 정리를 입증하면 쌍대가 되는 수식은 자동 입증

#### 불 대수 증명 방법

1. 회로를 통한 증명
2. 진리표를 통한 증명
3. 벤다이어그램
    - 부울 대수를 하나의 집합으로 본다.
    - AND연산은 교집합
    - OR연산은 합집합
    - NOT연산은 여집합

### <a href="#">불 대수의 표현 형태</a>

**곱의 합과 최소항**

곱의 합(SOP)
- SOP의 구성 1 단계는 AND항으로 구성되고 2단계는 OR항으로 만들어진 논리식

최소항: 표준 곱의 항
- 함수의 모든 변수를 포함하고 있는 항
- ex) F = ABCD ...

**합의 곱(POS)와 최대항**

합의 곱
- 1단계는 OR항, 2단계는 AND항으로 만들어진 논리식
- ex) (A + B)(A + C)

최대항
- 모든 변수를 포함하는 OR항
- ex) A + B + C + D

**최소항과 최대항의 관계**

- 최소항은 출력이 1인 항을 SOP형태로 나타낸 것
- 최대항은 출력이 0인 항을 POS형태로 나타낸 것
- 최소항과 최대항은 상호 보수의 성질을 갖는다

### <a href="#">논리식 간소화 방법</a>

1. 대수적 방법
- 어떤 순서로 적용해야 하는지 정하는 것이 번거롭고 많은 경험과 기술이 필요
- 다른 방법의 이론적 기초

2. 도표 방법
- 카르노 맵
- 사람에게는 직관적이지만 프로그램화에 부적합하다.

3. 알고리즘 방법
- 프로그램화에 적합해 많은 변수를 갖고 있는 불 함수의 간소화에 적당
