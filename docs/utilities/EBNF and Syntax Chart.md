---
layout: default
title: EBNF and Syntax Chart
parent: Utilities
---
# EBNF and Syntax Chart

복습 여부: No
유형: 강의

# Extended BNF(EBNF)

### EBNF = BNF + additional meta symbol

EBNF는 기존의 BNF(Backus-Naur Form)에 추가적인 meta symbol이 생긴것이다.

이러한 추가적인 meta symbol로 인해서 BNF에 비해서 표현을 생략할 수 있다.

### Meta Symbol of EBNF

- Optional Element : [ … ]
    - 생략가능한 선택 사항을 표현할 때 사용
    - <number> ::= <digit>[ .<digit>]
- Alternative Element: ( …. | … | … )
    - 여러개중 하나를 선택해서 사용할 때 사용
    - <signed number> ::= ( ‘+’ | ‘-’ )<number>
- Sequence of Element: { … } or { … }*
    - 여러 element를 여러번 사용, 즉 { }안의 element가 반복될 때 이를 사용
    - <identifier> ::= <letter>{<letter> | <digit>}

# Power(EBNF) = Power(BNF)

이때 EBNF와 BNF의 표현력은 동일하다.

예시를 보자

### Optional Element: [ … ]

- EBNF
    - <number> ::= <digits>[.<digits>]
- BNF
    - <number> ::= <digits> | <digits>.<digits>

### Alternative Elements: ( … | … | … )

- EBNF
    - <signed number> ::= (’+’ | ‘-’)<number>
- BNF
    - <signed number> ::= ‘+’<number> | ‘-’<number>

### Sequence of Elements: { … }

- EBNF
    - <identifier> ::= <letter>{ <letter> | <digit> }
- BNF
    - <identifier> ::= <letter><lds>
    - <lds> ::= $\varepsilon$  | <letter><lds> | <digit><lds>
    

# Syntax Chart(Syntax Diagram)

- Syntax Chart는 Syntatic rule의 graphical 표현이다.
- LHS(Left Hand Side) is given at the beginning of the arrow
- 원은 terminal, 사각형은 non terminal을 나타낸다.

아래의 LHS를 Syntax Chart로 표현해보자.

<expr> ::= <term> { (’+’ | ‘-’) <term> }

![Untitled](EBNF%20and%20Syntax%20Chart%203209a6ec5abb49a3b52f25b37431d157/Untitled.png)

# More Realistic Syntax Chart

<If_then_else> ::= ‘if’ <Boolean Expression> ‘then’ <Compound> ‘elseif’ B

![Untitled](EBNF%20and%20Syntax%20Chart%203209a6ec5abb49a3b52f25b37431d157/Untitled%201.png)

# Parser and EBNF

- Parser란?
    - language의 grammar가 주어졌을 때 해당 language의 syntax를 확인하는 프로그램을 말한다.
    - Parser는 보통 syntax tree를 생성한다.
- Parser Generators
    - grammar를 읽고 parser를 생성하는 일종의 프로그램
    - parser를 생성하는 여러 tool이 있다. (YACC, AntLR, CUP ….)
- Writing a Parser
    - parser는 수동으로 적힐 수 있다.
    - manual coding을 위해서 EBNF는 보통 parser의 청사진으로 사용된다.
    

# Recursive-Descent Parser(순환하강 구문분석기)

- Recursive Descent Parsing
    - Recursive Descent Parser는 parser를 수작업으로 적기위해서 전형적으로 사용되는 방법이다.
    - 각각 대응하는 nonterminal을 가진 recursive routione의 set으로 구성된다.
- Grammar Transformation
    - grammar의 설계에서 시작되는 parser를 쓴다.
    - 이때 Grammar는 left-factored이고 left-recursion이고 이는 EBNF를 사용해서 제거되어야 한다.
- The Transformation from the Grammar into the Code
    - 각각의 A $\in$ N에 대해서 production rule에 대해서 right hand side를 simulating하는 subprogram을 만든다.
    - right-hand side of nontermial을 simulate
    - 그다음 아래의 2개 중 1개를 한다.
        - a $\in$ T인 a 각각에 대해서 terminal symbol로 바꿔라
        - B $\in$ N인 B 각각에 대해서 B에 대응하는 subprogram을 호출해라

# The Helpers

- The Global Variable
    - 현재 token을 나타내는 lookhead variable(LA)가 필요하다.
    - 추가적인 속성 변수들을 선언할 수 있다.
- The Helper Functions
    - yylex() 함수는 next token(terminal)을 반환한다.
    - match(int t) 함수는 현재 token t를 test하고 다음 token을 읽는다.

예를 들어서 다음과 같은 grammar가 있다고 하자.

$$
S \rightarrow (S) | \epsilon
$$

위의 Grammar는 오직 1개의 줄에 대해서 판별하므로 여러줄에 대해서 적용하기 위해 Grammar 형태를 변경하면 다음과 같다.

A → **eof** | L A

L → S **newline**

S → (S) S | $\epsilon$

따라서 위와 같이 EBNF 형태로 나타낸 blueprint가 있으니 이를 통해서 program을 manually coding 할 수 있다.

```cpp
int LA;
int yylex(){
		return getchar();
}

void match(int t){
		if(LA == t) LA = yylex();
		else fprintf(stderr, "Syntax Error\n");
}
int S(){
		if(LA =='('){
				match('(');
				S();
				match(')');
				S();
		}
		else 
		;
}
int L() {
		S(); match('\n');
}
int A(){
		if (LA == EOF)
				;
		else {
				L(); A();
		}
}
int main(){
		LA = yylex();
		A();
		return 0;
}
```

ㄸ또한 이를 이용해서 속성 계산 규칙의 depth를 계산하는 기능 역시 구현할 수 있다.

```cpp

```
