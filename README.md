# 정규표현식(Regular-Expression)
## 정규표현식이란?
정규표현식(Regular Expression)은 텍스트 데이터에서 특정한 패턴을 찾거나 매칭하기 위해 사용되는 형식 언어이다.


## 정규표현식의 필요성
:정규표현식은 텍스트 데이터 처리와 검색에 필수적인 도구

* 복잡한 패턴을 표현하고 이를 기반으로 텍스트 데이터에서 원하는 정보를 효율적으로 검색
* 텍스트 데이터에서 원하는 정보를 추출하고 가공 
* 입력 데이터의 유효성을 검사하는 데에도 유용
* 간결한 코드 작성 가능
* 데이터의 형식을 통일시키거나 불필요한 문자열을 제거하는 등의 작업을 통해 데이터 전처리 과정에서 필요한 작업을 간편하게 수행


## 기본 사용법
### 1. 메타문자
* 메타 문자란 원래 그 문자가 가진 뜻이 아닌 특별한 용도로 사용하는 문자이다.
<pre><code>. ^ $ * + ? { } [ ] \ | ( )</code></pre>

### 2. 문자 클래스
|제목|내용|
|:---:|:---:|
|\d|숫자와 매치|
|\D|숫자가 아닌 것과 매치|
|\s|whitespace 문자와 매치|
|\S|whitespace 문자가 아닌 것과 매치|
|\w|문자+숫자(alphanumeric)와 매치|
|\W|문자+숫자(alphanumeric)가 아닌 문자와 매치|

### 3. Dot(.)
* 정규 표현식의 Dot(.) 메타 문자는 줄바꿈 문자인 \n을 제외한 모든 문자와 매치됨을 의미한다.

### 4. 반복 (*)
|정규식|문자열|Match 여부|설명|
|---|---|---|---|
|ca*t|ct|Yes|"a"가 0번 반복되어 매치|
|ca*t|cat|Yes|"a"가 0번 이상 반복되어 매치 (1번 반복)|
|ca*t|caaat|Yes|"a"가 0번 이상 반복되어 매치 (3번 반복)|

### 5. 반복 (+)
|정규식|문자열|Match 여부|설명|
|---|---|---|---|
|ca+t|ct|No|"a"가 0번 반복되어 매치되지 않음|
|ca+t|cat|Yes|"a"가 1번 이상 반복되어 매치 (1번 반복)|
|ca+t|caaat|Yes|"a"가 1번 이상 반복되어 매치 (3번 반복)|

### 6. re Python 정규표현식 모듈
* Python 에서는 re 모듈을 통해 정규표현식을 사용한다.
<pre><code>import re</code></pre>

### 7. 정규식을 이용한 문자열 검색
|Method|목적|
|---|---|
|match()|문자열의 처음부터 정규식과 매치되는지 조사|
|search()|문자열 전체를 검색하여 정규식과 매치되는지 조사한다.|
|findall()|정규식과 매치되는 모든 문자열(substring)을 리스트로 리턴|
|finditer()|정규식과 매치되는 모든 문자열(substring)을 반복 가능한 객체로 리턴|

* 7-1. match 시작부터 일치하는 패턴 찾기
<pre><code>p.match('aaaaa')
<_sre.SRE_Match object; span=(0, 5), match='aaaaa'>

p.match('bbbbbbbbb')
<_sre.SRE_Match object; span=(0, 9), match='bbbbbbbbb'>

p.match('1aaaa')
None

p.match('aaa1aaa')
<_sre.SRE_Match object; span=(0, 3), match='aaa'></code></pre>

* 7-2. search 전체 문자열에서 첫 번째 매치 찾기
<pre><code>p.search('aaaaa')
<_sre.SRE_Match object; span=(0, 5), match='aaaaa'>

p.search('11aaaa')
<_sre.SRE_Match object; span=(2, 6), match='aaaa'>

p.search('aaa11aaa')
<_sre.SRE_Match object; span=(0, 3), match='aaa'>

p.search('1aaa11aaa1')
<_sre.SRE_Match object; span=(1, 4), match='aaa'></code></pre>

* 7-3. findall 모든 매치를 찾아 리스트로 반환
<pre><code>p.findall('aaa')
['aaa']

p.findall('11aaa')
['aaa']

p.findall('1a1a1a1a1a')
['a', 'a', 'a', 'a', 'a']

p.findall('1aa1aaa1a1aa1aaa')
['aa', 'aaa', 'a', 'aa', 'aaa']</code></pre>

* 7-4. finditer: 모든 매치를 찾아 반복가능 객체로 반환
<pre><code>p.finditer('a1bb1ccc')
<callable_iterator object at 0x7f850c4285f8></code></pre>

### 8. 매치 객체의 메서드
|Method|목적|
|---|---|
|group()|매치된 문자열 출력|
|start()|매치 시작지점 인덱스 출력|
|end()|매치 끝지점 인덱스 출력|
|span()|(start(), end())를 튜플로 출력|

## 코드 예제
### 주민등록번호 추출 예제
<pre>
<code>
import re

text = "주민등록번호는 950101-1234567입니다."
pattern = r"\b\d{6}-\d{7}\b"

residents = re.findall(pattern, text)
print(residents)
</code>
</pre>
* 'r"\d{6}-\d{7}"은 6자리 숫자, 하이픈, 그리고 7자리 숫자로 이루어진 패턴을 의미합니다.
* 're.findall(pattern, text)은 주어진 텍스트에서 패턴과 일치하는 모든 부분을 추출하여 리스트로 반환합니다.
* 'residents는 추출된 주민등록번호를 담은 리스트입니다.



