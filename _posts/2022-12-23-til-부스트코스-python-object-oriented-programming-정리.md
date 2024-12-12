

---
title: [TIL] 부스트코스 Python Object-Oriented Programming 정리
date: 2022-12-23 08:39:17.827 +0000
categories: [부스트코스]
tags: ['프리코스']
description: OOP를 쓰는 이유 : 만들어 놓은 코드의 재사용남들이 만들어 놓은 코드(모듈 또는 패키지 등)를 이용해 코딩을 하는 현재 시점에서 프로그래밍의 가장 기본이 되는 기법이다.객체의 사전적 의미는 실생활에 있는 일종의 물건이다.		\- 축구의 경우 선수, 공, 골대, 심판


---


# 객체 지향 프로그래밍(OOP)
- OOP를 쓰는 이유 : 만들어 놓은 코드의 재사용
- 남들이 만들어 놓은 코드(모듈 또는 패키지 등)를 이용해 코딩을 하는 현재 시점에서 프로그래밍의 가장 기본이 되는 기법이다.

# 클래스와 객체
## 객체 (Object)
- 객체의 사전적 의미는 실생활에 있는 일종의 물건이다.
	- 축구의 경우 선수, 공, 골대, 심판 등이 모두 객체에 해당
- 모든 객체는 속성(Attribute)과 행동(Action)을 가진다.
- OOP에서는 이러한 객체를 하나의 프로그램으로 표현한다.
	- 속성은 변수, 행동은 함수로 표현된다.

## 클래스 (Class)와 인스턴스(Instance)
- 클래스는 OOP에서 객체의 설계도에 해당한다.
- 인스턴스는 클래스를 이용한 실제 구현체이다.

# 파이썬의 객체
## 클래스 선언
- 클래스는 함수와 비슷한 문법으로 선언할 수 있다.
```python
class SoccerPlayer(object):
```
- `class` : 클래스임을 나타내는 예약어
- `SoccerPlayer` : 클래스의 이름 (파이썬에서 네이밍 규칙으로 PascalCase를 사용한다)
- `object` : 상속받는 객체를 나타낸다. 가장 기본이 되는 객체는 `object`를 사용한다.
	- 파이썬 3부터는 선언하지 않아도 자동 상속이 된다.
### 속성 추가
- 클래스의 `__init__` 예약 함수를 통해 클래스의 속성을 추가한다.
```python
def __init__(self, name : str, position : str, back_number : int):
    self.name = name
    self.position = position
    self.back_number = back_number
```
- **`self`** : 생성된 인스턴스 자신을 의미하는 인자
	- 클래스 내의 모든 함수가 가져야 하는 인자로, 첫번째로 선언되어야 한다.
    - abc라는 객체를 생성했을 때, 클래스 바깥에서는 `abc.name`이 클래스 내에서는 `self.name`이 된다.
- `name, position ...` : 속성 인자의 이름
- `:str, :int ...` : 필수 요소는 아니며, 인스턴스 생성 시 사용자에게 힌트를 제공한다.

### __init__
- `__init__`은 컨스트럭터라고 불리는 클래스의 초기화를 위한 함수이다.
- 새로운 객체를 생성할 때 실행된다.
	- 예를들어 컨스트럭터에 print문을 넣으면, 새 인스턴스 생성 시 print문이 실행되어 결과가 출력된다.

### 그외 '__'의 의미
- `__`는 특수한 예약 함수나 변수, 함수명 변경(맨글링)에 사용된다.
- 예시
	- `__add__` : 객체에서 더하기 연산을 했을 때 실행되는 함수이다.
    - `__str__` : print문을 객체에 적용했을 때 실행되는 함수이다.
## 객체(인스턴스) 사용하기
- 객체의 이름을 선언하며 초기값을 입력하는 것을 통해 생성한다.
```python
son = SoccerPlayer("손흥민", "FW", 7)
```
- 클래스에 선언된 함수를 객체를 통해 접근할 수 있다.
```python
def change_back_number(self, number):
    print(f'{self.name} 선수의 등번호를 변경합니다. {self.back_number} to {number}')
    self.back_number = number

...

son.change_back_number(25)
# 손흥민 선수의 등번호를 변경합니다. 7 to 25
```
- 생성된 객체는 기본적으로 외부에서 접근 및 변경이 가능하다.
```python
print(son.back_number) # 7 
son.back_number = 25
print(son.back_number) # 25
```
# OOP 구현 예시
- note에 content를 작성하고, notebook에 추가하는 프로그램
```python
class NoteBook(object):
    def __init__(self, title:str):
        self.title = title
        self.page_number = 1
        self.notes = {}
    def add_note(self, note, page = 0): # page를 입력안했을 때 default로 0 사용
        if self.page_number < 301:
            if page == 0:
                self.notes[self.page_number] = note
                self.page_number += 1
            else:
                self.notes = {page : note} # 딕셔너리 형태로 노트 저장
                self.page_number += 1
        else:
            print("Page가 꽉 찼습니다.")
    def remove_note(self, page_number):
        if page_number in self.notes.keys():
            return self.notes.pop(page_number)
        else:
            print("해당 페이지는 존재하지 않습니다")
    def get_number_of_pages(self):
        return len(self.notes.keys())

class Note(object):
    def __init__(self,content:str):
        self.content = content
    def write_content(self, content = None):
        self.content = content
        print("노트에 내용이 추가되었습니다.")
    def remove_note(self):
        self.content = ""
        print("노트를 지웠습니다.")
    def __add__(self, other):
        return self.content + other.content
    def __str__(self):
        return self.content
```
# OOP의 특성, 개념
## 상속(Inheritance)
- 클래스를 선언할 때, 부모클래스로 부터 속성과 메소드를 모두 물려받을 수 있다.
- super() 함수를 통해 부모 클래스로부터 함수를 불러올 수 있다.
	- 자식 클래스에서 부모클래스의 함수를 사용할 때 super()와 함께 사용한다.
## 다형성
- 같은 이름을 쓰는 메소드에서 내부 로직을 다르게 작성한 것
	- 같은 이름을 쓰지만, 인자, 자료형에 따라 다르게 출력해야할 때 필요하다.
    - 특히 파이썬은 Dynamic Typing 특성으로 인해 부모클래스 상속 시 자주 발생한다.
- 현재 단계에서는 너무 깊게 다루지 않는다
## 가시성
- Encapsulation을 통해 객체의 접근할 수 있는 수준(레벨)을 조절하는 것
- 모든 사용자가 객체 안에 모든 변수에 접근 및 수정이 가능하다면 위험하다.
	- 잘못된 사용으로 인한 오작동, 보안 문제 등
### Encapsulation
- 사용자는 클래스의 인터페이스만 알면 클래스를 사용할 수 있도록 하는 개념
- 클래스를 설계할 때 클래스 간 간섭 및 정보공유를 최소화
### 예시
```python
class Inventory(object):
	def __init__(self):
    	self.__items = ['item1', 'item2'] # 변수명 앞에 '__'를 붙이면 private 변수로 선언된다.
    def __str__(self):
    	return ' '.join(self.__items) # 클래스 내부에서는 접근 가능하다

my_inventory = Inventory()
print(my_inventory.__items) # AttributeError
print(my_inventory) # item1 item2
```
### getter and setter
- 클래스에서 값을 불러오는 함수(메서드)를 getter, 값을 변경(저장)하는 함수를 setter라 한다.
- property decorator를 통해 private 변수에 대해 getter, setter 함수를 쉽게 구현할 수 있다.
```python
class Inventory(object):
	def __init__(self):
    	self.__items = ['item1', 'item2']
        
    @property #getter
    def items(self):
    	return self.__items
        
    @items.setter #메서드명.setter를 통해 setter가 동작한다.
    def items(self, value):
    	self.__items = value

my_inventory = Inventory()
print(my_inventory.items) # ['item1', 'item2']
my_inventory.items = [] # item1 item2
print(my_inventory.items) # []
```





        