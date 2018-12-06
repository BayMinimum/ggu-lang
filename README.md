# ggu-lang
커여운 한글 프로그래밍 언어

## 목표
난해한 언어보다는 최대한 **커여운** 한글 프로그래밍 언어를 만드는 것을 목표로 합니다.

## 스펙
### 글자
사용 가능한 글자의 목록입니다.
```
꾸 뀨 까 꺄 끼 뿌 쀼 삐 뚜 우 아 이 ! ? ' " . [space] \n
```

### 변수/스택/큐
꾸, 뀨, 까, 꺄, 뿌, 쀼는 각각 정수 변수처럼 작동하며, 초기값은 모두 0입니다. 끼, 삐는 각각 스택/큐로, 사용법은 다른 변수들과 비슷하며(값을 더하려고 하면 그 값을 집어넣고, 값을 다른 연산에 사용하려고 하면 값을 하나 빼서 사용합니다), 초기 상태는 둘 다 비어있습니다.

### 제어
뚜는 프로그램의 현재 실행 위치를 가리키는 변수입니다. 첫째 줄은 0, 둘째 줄은 1...의 값을 가집니다. 일반적으로 한 줄의 실행이 끝나면 뚜의 값은 자동으로 1 증가하고, 뚜의 값이 프로그램의 범위를 초과한다면 프로그램이 종료됩니다. 쌍따옴표로 둘러싸인 줄은 그 줄에서 가장 마지막으로 계산한(가장 왼쪽) 변수의 값이 0이라면 그 다음 줄을 실행하고, 0이 아니라면 그 다음 줄을 건너뜁니다. 따옴표로 둘러써인 줄은 반대로 값이 0이라면 그 다음 줄을 건너뛰고, 0이 아니라면 다음 줄을 실행합니다. 쌍따옴표/따옴표 구문과 뚜의 값 변경을 적절히 활용하면 조건문이나 루프 등을 구현할 수 있습니다.

### 입출력
변수 뒤에 !를 붙이면 정수 값을 그대로 출력한 뒤 줄을 바꿉니다.
변수 뒤에 !!를 붙이면 그 정수의 유니코드 값에 해당하는 문자를 출력하고, 줄을 바꾸지 않습니다.
변수 뒤에 ?를 붙이면 입력 값을 해당 변수에 더합니다. 숫자가 입력된 경우 숫자를, 문자가 입력된 경우 그 문자의 유니코드 값을 더합니다.

### 0
변수 뒤에 .을 붙이면 해당 변수에 0을 대입합니다.

### 단어
변수 뒤에 우/아를 발음에 따라 적절히 조합하여 붙인 것 또는 ?의 뒤에 !/!!/아무것도 붙이지 않을 경우를 단어라고 합니다. (정규표현식: (((뀨|꾸|뿌|쀼)(우*))|((꺄|까)(아*))|(끼)|"?"|(!{0,2})))

### 연산/코드 예시
변수 뒤에 우/아를 발음에 따라 적절히 조합하여 붙이고, 그 단어가 줄의 마지막 단어일 경우, 그 개수만큼 더합니다.
```
뀨우우
꺄아아아아아아
뀨아
```
변수 뀨에 2를 더하고, 변수 꺄에 6을 더합니다. 마지막 줄은 에러가 발생합니다.

변수 뒤에 다른 변수의 이름을 붙이면 해당 변수를 더합니다. 연산은 각 줄마다 오른쪽부터 왼쪽으로 이루어집니다.
```
꾸뀨
까꺄
까뀨꾸
```
변수 꾸에 뀨를 더하고, 변수 까에 꺄를 더한 뒤, 변수 뀨에 꾸를 더하고, 변수 까에 뀨를 더합니다.

변수 뒤에 우/아를 발음에 따라 적절히 조합하여 붙였는데 그 단어가 줄의 마지막 단어가 아닐 경우, 뒤에 오는 문장에서 마지막으로 연산된 변수를 더하고 우/아의 개수만큼 뺍니다.
```
꾸우뀨
꺄아아아꾸
뀨우우우까아꾸까
```
변수 꾸에 뀨-1을 더하고, 변수 꺄에 꾸-3을 더한 뒤, 꾸에 까를 더하고, 까에 꾸-1을 더하고, 뀨에 까-3을 더합니다.

!나 !!는 변수 위에 우/아가 붙은 뒤, 다른 단어로 넘어가기 전에 붙입니다. 그러면 해당 변수에 대한 연산이 완료된 후의 값을 출력합니다. 스택/큐의 경우, 값을 하나 뽑아내어 출력합니다. !를 3개 이상 연달아 붙이는 행위, 단어 뒤가 아닌 곳에 붙이는 행위는 허용되지 않습니다.
```
뀨!
꾸우!뀨우
꺄아아!!꾸!
까!!!!
!
```
꾸의 값을 숫자로 출력하고, 뀨에 1을 더하고, 꾸에 뀨-1을 더한 뒤 꾸의 값을 출력하고, 꺄에 꾸-2를 더한 뒤, 꺄의 값에 해당하는 유니코드 문자를 출력합니다. 마지막 두 줄은 에러가 발생합니다.

?나 .는 하나의 단어로 취급할 수 있으며, 출력도 가능하지만, 직접 ?나 .에 값을 더할 수는 없습니다. 즉, 모든 단어는 그 연산 결과물을 왼쪽에 더하기 때문에, ?나 .의 뒤에는 !나 !!만 올 수 있습니다.
```
뀨?
뀨우우?
꺄아?!
?!
꾸우?우!
```
입력받은 값을 뀨에 더하고, 입력받은 값-2를 뀨에 더하고, 입력받은 값을 그대로 출력한 뒤 1을 빼서 꺄에 더하고, 꺄에 1을 더한 뒤 꺄의 값을 출력하고, 입력받은 값을 그대로 출력하고 변수에 저장하지 않습니다. 마지막 줄은 ?에 우를 더하려고 했기 때문에 오류가 발생합니다.

```
뀨.
뀨우우.
꺄아.!
꾸우.꺄아아!
```
뀨의 값을 0으로 만들고, 뀨의 값을 -2로 만들고, 꺄의 값을 -1로 만든 뒤 출력하고, 마지막 줄은 에러가 발생합니다.

## 실행
인터프리터는 다음과 같이 GguLang 코드를 실행합니다.
1. 첫 줄부터 실행하고(뚜=0), 실행이 완료되면 뚜의 값을 알맞게(일반적으로 1, 따옴표 구문을 만족시키지 않는다면 2) 증가시키고 그에 해당하는 줄을 실행한다.
1. 각 줄을 실행하기 전해는 체꾸(Checkggu) 프로세스를 통해 문법 오류가 없음을 확인한 후 실행해야 한다. 즉, 중간에 오류를 포함한 줄은 오른쪽의 일부만 실행되는 것이 아니라, 전체 줄이 실행되지 않아야 한다.
1. 다음으로 실행해야 할 줄(뚜+1)이 코드를 초과할 경우 프로그램은 종료된다.

## 구현체 목록
- [BayMinimum/ggu.py](https://github.com/BayMinimum/ggu.py) (WIP)
- [leejseo/ggu.py](https://github.com/leejseo/ggu.py) (WIP)
