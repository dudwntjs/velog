<p>Swift의 기본 타입 대부분이 구조체 기반으로 설계되어 있고, 
<strong>값 의미(Value Semantics)를 우선하는 언어</strong>이기 때문에 <strong>구조체 중심 언어</strong>라고 불린다고 합니다!
<br />
<strong>Swift가 구조체를 선호하는 진짜 이유는 무엇일까요?🧐</strong>
그 답은 바로 Swift가 값을 저장하고 관리하는 <strong>메모리 구조(Heap &amp; Stack)</strong>에서 시작됩니다.</p>
<hr />
<h2 id="swift-메모리의-두-영역-span-stylebackground-color-ffdce0heapspan--span-stylebackground-color-d1ebf9stackspan">Swift 메모리의 두 영역: <span style="background-color: #ffdce0;">Heap</span> &amp; <span style="background-color: #d1ebf9;">Stack</span></h2>
<h3 id="span-stylebackground-color-d1ebf9struct-값-타입span"><span style="background-color: #d1ebf9;">Struct (값 타입)</span></h3>
<ul>
<li>인스턴스의 <code>실제 데이터 자체(x, y, 프로퍼티 값)</code>가 <strong>Stack에 직접 저장</strong></li>
<li>다른 변수에 대입하면 <strong>값 전체가 복사되어 서로 완전히 독립</strong></li>
<li>함수가 끝나면 Stack 메모리도 <strong>즉시 자동 해제 → 매우 빠름</strong><ul>
<li>예: <code>Point</code>, <code>UserProfile</code>, <code>Int</code>, <code>String</code>, <code>Array</code> 등 대부분의 Swift 기본 타입</li>
</ul>
</li>
</ul>
<blockquote>
<pre><code>Stack
  └─ Struct 인스턴스 데이터 (직접 값 저장)
  └─ 복사하면 새로운 독립 인스턴스 생성
  └─ 함수 종료 시 자동 제거</code></pre></blockquote>
<pre><code>
### &lt;span style='background-color: #ffdce0'&gt;Class (참조 타입)&lt;/span&gt;

- 인스턴스의 `실제 객체 데이터(name, age 같은 값들)`는 **Heap에 저장**
- 변수 자체는 Stack에 존재하지만, **값이 아니라 Heap 객체의 주소만 저장**
- 다른 변수에 대입하면 **주소만 복사 → 같은 객체를 공유**
- 객체 생명 주기는 Swift가 `ARC(참조 카운팅)`으로 추적하고, **참조가 0이 될 때 해제**
    - 예: `NetworkManager`, `ViewModel`, `파일 핸들`, `UIKit 클래스 대부분`


&gt;```
Stack
  └─ Class 변수 = Heap 객체 주소 보관
Heap
  └─ 실제 Class 객체 데이터 저장
  └─ 주소 복사 시 여러 변수에서 객체 공유
  └─ ARC로 메모리 생명 주기 관리</code></pre><h3 id="그래서-span-stylebackground-color-ffdce0classspan와-span-stylebackground-color-d1ebf9structspan-비교해보자면">그래서 <span style="background-color: #ffdce0;">Class</span>와 <span style="background-color: #d1ebf9;">Struct</span> 비교해보자면?</h3>
<table>
<thead>
<tr>
<th><strong>구분</strong></th>
<th><strong><span style="background-color: #ffdce0;">Class</span></strong></th>
<th><strong><span style="background-color: #d1ebf9;">Struct</span></strong></th>
</tr>
</thead>
<tbody><tr>
<td>타입</td>
<td>참조 타입</td>
<td>값 타입</td>
</tr>
<tr>
<td>저장 위치</td>
<td>힙</td>
<td>주로 스택</td>
</tr>
<tr>
<td>복사 방식</td>
<td>주소만 복사 → 공유</td>
<td>값 전체 복사 → 독립</td>
</tr>
<tr>
<td>1️⃣ 상속</td>
<td>⭕</td>
<td>❌</td>
</tr>
<tr>
<td>2️⃣ mutating 필요</td>
<td>❌</td>
<td>⭕</td>
</tr>
<tr>
<td>3️⃣ COW 적용</td>
<td>❌</td>
<td>표준 라이브러리에서 적용</td>
</tr>
<tr>
<td>4️⃣ ARC 관리</td>
<td>⭕</td>
<td>❌</td>
</tr>
<tr>
<td>5️⃣ 스레드 안전성</td>
<td>주의 필요</td>
<td>상대적으로 안전</td>
</tr>
</tbody></table>
<h3 id="1️⃣-상속-가능-여부">1️⃣ 상속 가능 여부</h3>
<h4 id="span-stylebackground-color-ffdce0classspan에서는-⭕"><span style="background-color: #ffdce0;">Class</span>에서는 ⭕</h4>
<p>클래스는 <strong>참조 타입</strong>이고 <strong>힙에 객체를 저장</strong>하며
클래스는 <strong>타입 메타데이터(vtable)</strong> 를 갖고 있어 <strong>동적 디스패치(Dynamic Dispatch)</strong>를 수행</p>
<ul>
<li>부모 클래스의 <strong>프로퍼티와 메서드를 그대로 물려받을 수 있음</strong></li>
<li>자식 클래스에서 <strong>메서드를 재정의(override)</strong> 가능</li>
<li>런타임에 어떤 메서드를 호출할지 <strong>동적으로 결정</strong> (다형성의 핵심)</li>
<li><code>super</code> 로 부모의 동작을 확장 가능</li>
<li><strong>Polymorphism(다형성)</strong> 구현 가능</li>
</ul>
<blockquote>
<p>클래스의 구조 자체가 <strong>확장(상속)</strong>을 염두에 두고 설계된 타입</p>
</blockquote>
<h4 id="span-stylebackground-color-d1ebf9structspan에서는-❌"><span style="background-color: #d1ebf9;">Struct</span>에서는 ❌</h4>
<ul>
<li>값 타입(Value Type)</li>
<li>스택 기반 저장</li>
<li>복사 시 독립적인 인스턴스 생성</li>
<li>mutating 기반 자체 치환</li>
<li>메타데이터나 vtable 없음</li>
</ul>
<blockquote>
<p>구조체는 <strong>상태를 공유하는 상속 구조</strong>와 맞지 않고
→ <strong>안전하고 예측 가능한 값 모델링</strong>에 최적화되어 있음</p>
</blockquote>
<p>⇒ <strong>그래서 Swift는 struct 상속을 금지하고 protocol을 제공</strong></p>
<p>대신, </p>
<ul>
<li><p>여러 기능을 조합할 때는 <strong>프로토콜(protocol)</strong> 을 사용</p>
</li>
<li><p>필요하다면 <strong>프로토콜 확장(protocol extension)</strong> 으로 기능 추가</p>
<p>  ⇒ 이 방식은 상속보다 더 유연하고 안전합니다</p>
<p>  (상속은 하나만 가능하기도 하고, 부모-자식 의존성 큼…)</p>
</li>
</ul>
<hr />
<h3 id="2️⃣-mutating이-필요-여부">2️⃣ mutating이 필요 여부</h3>
<h4 id="span-stylebackground-color-ffdce0classspan에서는-❌"><span style="background-color: #ffdce0;">Class</span>에서는 ❌</h4>
<p>클래스는 힙 객체 내부 프로퍼티만 바꾸고,
<strong>변수 자체(주소 박스)는 변하지 않기 때문에 <code>mutating</code>이 필요 없습니다.</strong></p>
<h4 id="span-stylebackground-color-d1ebf9structspan에서는-⭕"><span style="background-color: #d1ebf9;">Struct</span>에서는 ⭕</h4>
<p>구조체는 값 전체가 교체되는 개념이라
<strong>내부 상태 변경 시 자기 자신을 새 값으로 치환하므로 <code>mutating</code> 선언이 필요합니다.</strong></p>
<hr />
<h3 id="3️⃣-cow-copy-on-write-최적화">3️⃣ COW (Copy-On-Write) 최적화</h3>
<p>Swift의 대표적인 큰 값 타입(<code>Array</code>, <code>String</code>, <code>Dictionary</code>)은
겉보기엔 복사처럼 보이지만 <strong>실제로는 Heap 버퍼를 공유, 변경이 발생하는 순간에만 진짜 복사</strong>합니다.</p>
<h4 id="span-stylebackground-color-d1ebf9structspan-자체는-stack에-저장"><span style="background-color: #d1ebf9;">Struct</span> 자체는 Stack에 저장</h4>
<p>하지만 큰 데이터는 Heap 버퍼를 COW로 공유합니다.
변경 시에만 복사 → 메모리 절약+ 성능 최적화가 가능합니다.</p>
<h4 id="span-stylebackground-color-ffdce0classspan에서는-❌-1"><span style="background-color: #ffdce0;">Class</span>에서는 ❌</h4>
<p>참조 타입은 항상 객체를 공유하도록 설계되어 있어
<strong>독립 복사 비용이 없고 COW가 필요하지 않기 때문에 적용되지 않습니다.</strong></p>
<hr />
<h3 id="4️⃣-arc-관리-여부">4️⃣ ARC 관리 여부</h3>
<h4 id="span-stylebackground-color-ffdce0classspan에서는-⭕-1"><span style="background-color: #ffdce0;">Class</span>에서는 ⭕</h4>
<ul>
<li>힙 기반 참조 타입 → ARC 관리 필수</li>
<li>참조 카운트 증가/감소를 통해 객체 생명 주기 관리</li>
</ul>
<hr />
<h3 id="5️⃣-thread-safety스레드-안전성">5️⃣ Thread Safety(스레드 안전성)</h3>
<table>
<thead>
<tr>
<th>타입</th>
<th>안전성</th>
</tr>
</thead>
<tbody><tr>
<td><span style="background-color: #ffdce0;">Class</span></td>
<td>힙 객체 공유 → 여러 스레드 동시 수정 가능 → <strong>동기화 필요</strong></td>
</tr>
<tr>
<td>작은 <span style="background-color: #d1ebf9;">Struct</span></td>
<td>Stack에 직접 저장 + 값 복사 → <strong>충돌 가능성 낮음 → 상대적으로 안전</strong></td>
</tr>
<tr>
<td>큰 <span style="background-color: #d1ebf9;">Struct</span></td>
<td>COW로 Heap 버퍼 공유 중일 때는 결국 공유 상태 → <strong>동기화 고려 필요</strong></td>
</tr>
</tbody></table>
<br />

<h2 id="swift가-span-stylebackground-color-d1ebf9structspan-중심-언어인-이유❓">Swift가 <span style="background-color: #d1ebf9;">Struct</span> 중심 언어인 이유❓</h2>
<ul>
<li><strong>독립적인 값 모델링에 최적화 → Struct</strong></li>
<li><strong>Stack에 직접 저장되어 생성/해제 비용이 거의 없음</strong></li>
<li><strong>복사해도 안전(값이 공유되지 않음)</strong></li>
<li><strong>Swift 기본 타입 대부분이 구조체 기반 + COW 최적화까지 적용</strong></li>
<li>반면 Class는 <strong>Heap 공유 + ARC 비용 + Thread 동기화 고려</strong>가 따라옴
  → 꼭 필요한 경우에만 선택하자</li>
</ul>
<hr />
<h2 id="span-stylebackground-color-ffdce0classspan를-써야-하는-경우"><span style="background-color: #ffdce0;">Class</span>를 써야 하는 경우</h2>
<ol>
<li><p><strong>여러 곳에서 하나의 상태를 공유해야 할 때</strong></p>
<ul>
<li>앱 설정, ViewModel, 싱글톤 등</li>
</ul>
</li>
<li><p><strong>상속이 필요할 때</strong></p>
<ul>
<li>UIKit의 <code>UIView</code>, <code>NSObject</code> 기반 API 등</li>
</ul>
</li>
<li><p><strong>ARC로 생명 주기를 관리해야 할 때</strong></p>
<ul>
<li>네트워크 세션, 파일 핸들, I/O 객체</li>
</ul>
</li>
<li><p><strong>Identity(동일성 개념)가 중요할 때</strong></p>
<ul>
<li>값이 아니라 객체의 실체가 하나여야 하는 경우</li>
<li>예: Document, Actor, Session, Connection 등</li>
</ul>
</li>
</ol>
<blockquote>
<p>상속 필요 + 상태 공유 + Identity 중요 → Class</p>
</blockquote>
<br />

<h2 id="span-stylebackground-color-d1ebf9structspan를-써야-하는-경우"><span style="background-color: #d1ebf9;">Struct</span>를 써야 하는 경우</h2>
<ol>
<li><p><strong>독립적인 값 자체를 모델링할 때</strong></p>
<ul>
<li>Point, Size, Money, Score, Profile 등</li>
</ul>
</li>
<li><p><strong>상속이 필요 없는 단순 데이터 컨테이너</strong></p>
</li>
<li><p><strong>복사본끼리 서로 영향을 주면 안 되는 경우</strong></p>
<ul>
<li>사용자 정보, 주문 정보, 좌표 등</li>
</ul>
</li>
<li><p><strong>멀티 스레드 환경에서 독립성이 보장되어야 할 때</strong></p>
</li>
<li><p><strong>Swift 표준 타입과 철학을 맞추고 싶을 때</strong></p>
<ul>
<li><code>Array</code>, <code>String</code>, <code>Int</code>, <code>Bool</code>, <code>Date</code> 등 모두 Struct 기반</li>
</ul>
</li>
<li><p><strong>대부분의 성능 최적화 케이스</strong></p>
<ul>
<li>스택 저장 + COW 최적화</li>
</ul>
</li>
</ol>
<blockquote>
<p>상태 공유 ❌ + 상속 ❌ + 독립 값 → Struct</p>
</blockquote>