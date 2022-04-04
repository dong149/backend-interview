# OS

#### 프로세스 vs 스레드 ?

<details>
<summary style="color:skyblue">정답보기</summary>
<Blockquote>
<br>

### 프로세스

프로그램을 메모리 상에서 실행중인 작업

Code, Data, Heap, Stack를 할당받는다.

### 스레드

프로세스 안에서 실행되는 여러 흐름의 단위
Stack , PC Register만 따로 할당 받고, 나머지 영역은 서로 공유한다.

</Blockquote>
</details>

#### 멀티프로세스 vs 멀티스레드 ?

<details>
<summary style="color:skyblue">정답보기</summary>
<Blockquote>
<br>

### 멀티프로세스

하나의 컴퓨터에 여러 CPU를 장착하고 -> 하나 이상의 프로세스들을 동시에 처리한다.

장점 : 안전성 ( 메모리 침범 문제를 OS 차원에서 해결한다.)
단점 : 각각 독립된 메모리 영역을 갖고 있어서, 작업량이 많을 수록 오버헤드가 발생하고, Context Switching으로 인한 성능 저하가 발생한다.

### 멀티스레드

하나의 응용 프로그램에서 여러 스레드를 구성해, 각 스레드가 하나의 작업을 처리하는 것.

장점 : 독립적인 프로세스에 비해 공유 메모리만큼의 시간, 자원 손실이 감소.
전역 변수와 정적 변수에 대한 자료 공유 가능.

단점 : 안정성 문제.

</Blockquote>
</details>
