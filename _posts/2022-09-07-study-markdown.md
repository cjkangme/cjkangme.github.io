---
layout: single
title: "Markdown 작성법"
categories: til
tags: GitHub
---

# Markdown 작성법

## 1. Markdown이 작동하는 원리

- 작성된 Markdown file(.md or .markdown)을 Markdown application이 HTML 형태로 변환하여 웹 사이트에 표시되도록 해준다. (정확한 단계는 application 별로 다소 상이할 수 있다)

  - GitHub 에서는 Jekyll을 사용한다.@

## 2. Markdown syntax

### 2-1. 헤딩(Headings) : 문서의 제목 및 글머리에 해당

- HTML의 h1~h6 태그에 대응한다. (6까지 지원)

---

    # Heading level 1
    ## Heading level 2
    ### Heading level 3
    #### Heading level 4
    ##### Heading level 5
    ###### Heading level 6

# Heading level 1

## Heading level 2

### Heading level 3

#### Heading level 4

##### Heading level 5

###### Heading level 6

---

- Heading level 1과 Heading level 2는 아래 라인에 '===' 및 '---' 를 추가하는 것도 가능하다. (3개 이상 추가해야 인식)

  # Heading level 1

  ## Heading level 2

# Heading level 1

## Heading level 2

---

### 2-2. 강조

- 텍스트를 강조해준다.

#### Bold

    **bold_text1**
    __bold_text2__ #글자 중간에 언더바가 있을 경우 작동하지 않으므로 * 사용
    none_bold_text

**bold_text1**

**bold_text2**

none_bold_text

---

#### Italic

    *italic_text1*
    _italic_text2_ #글자 중간에 언더바가 있을 경우 작동하지 않으므로 * 사용
    none_italic_text

_italic_text1_

_italic_text2_

none_italic_text

---

#### Bold & Italic

    ***bold_italic_text1***
    ___bold_italic_text2___ #글자 중간에 언더바가 있을 경우 작동하지 않으므로 * 사용

**_bold_italic_text1_**

**_bold_italic_text2_**

---

- MarkText 편집기를 사용하여 문서를 작성중인데, 이 편집기에서는 Bold는 가운데 언더바가 있어도 작동하지만, Italic은 작동하지 않는 것 같다. 그냥 되도록 \*만 사용하자.

### 2-3.BlockQuote

- 내용을 정리할 때 쓸만한 블록을 생성해준다.

---

    > first blockquote1
    >
    > first blockquote2
    >> second blockquote
    >>> third blockquote

> first blockquote1

>

> first blockquote2

> > second blockquote
> >
> > > third blockquote

- 편집기 문제로 blockquote가 끊어져 보이는데 실제로는 연결되어 있다. (markdown을 제대로 쓰려면 편집기를 안쓰는 것이 오히려 좋아보인다)

---

- blockquote 안에서 다른 요소를 사용할 수 있다.
  '''
  > ## This is Heading level 2 > > - List 1 > - **List 2(Bold)**
  >
  > '''
  >
  > ## This is Heading level 2
  >
  > - List 1
  >
  > - **List 2(Bold)**

---

### 2-4. 목록

#### 정렬된 목록

- 숫자를 어떻게 입력하던지 상관 없이 번호순으로 정렬된다.
  '''

  1.  list1 2. list2

            3. list3

            4. list1

            5. list2

            6. list3

      '''

1. list1

2. list2

3. list3

4. list1

5. list2

6. list3

---

#### 정렬되지 않은 목록

- 첫 단어로 '-', '\*', '+' 중 하나를 입력 후 띄어쓰기를 하는 것으로 정렬되지 않은 목록을 만들 수 있다.
  '''
- first item
- second item
  - second intended item
  - second intended item

* first item
* second item
* third item
  '''

- first item

- second item

  - second intended item

  - second intended item

* first item

* second item

* third item

---

#### 목록 내 요소 추가

- 목록 안에서 일반 문장(탭 또는 띄어쓰기 4회), Blockquotes, Code Block(2회 탭 또는 띄어쓰기 8회), Images를 추가할 수 있다.
  '''
  1. list1
     paragraphs
  2. list2
  - first item
    > blockquotes
  - second item
    '''

1. list1

   paragraphs

2. list2

- first item

  > blockquotes

- second item

---

### 2-5.코드(Code)

- 코드는 ('<코드내용>')을 통해 입력하는 방법과 코드블럭을 통한 방법 두가지가 있다.

- 띄어쓰기 4회 또는 탭 1회를 통해 코드를 입력할 수 있는 블럭을 만들 수 있다.

- 사실 내가 예시를 위해 만든 블럭이 코드 블럭이다.

        '<html>

  <head>
      </head>

      <html>'

      <html>

  <head>
  </head>
  <html>

---

### 2-6. 수평선 (Horizontal Rules)

- 3회 이상 '\*', '-', '\_'을 입력하는 것을 통해 수평선을 표현할 수 있다.
  '''

---

---

---

'''

---

---

---

---

### 2-7. 링크(URL 및 이메일)

- URL 및 이메일을 곧바로 표현하거나, 다른 이름으로 표기하거나, 주석을 달 수 있다.

        <https://www.naver.com>
        [e-mail](nureongi0214@gmail.com)
        [naver][1]
        [1]: https://www.naver.com "naver homepage"

<https://www.naver.com>

[e-mail](nureongi0214@gmail.com)

[naver] [1]

[1]: https://www.naver.com "naver homepage"

---

### 2-8. 이미지 삽입

- '!' 를 통해 이미지를 삽입할 수 있다. 문법은 링크와 유사하다.

       ![ImageName](/assets/image/image.png)

---

- 사실 이미지는 편집기를 통해 넣는 것이 가장 좋아보인다.

-
