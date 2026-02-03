<h1 id="클래스classvs-구조체struct">클래스(class)vs 구조체(struct)</h1>
<p>Swift에서는 <strong>클래스(Class)</strong> 와 <strong>구조체(Struct)</strong> 가 매우 비슷해 보이지만,
실제로는 <code>메모리 구조부터 동작 방식, 상속 가능 여부</code>까지 모든 면에서 다른 타입이라 자세히 공부해두면 좋을 것 같아요!!</p>
<blockquote>
<p>Swift에서 클래스와 구조체의 가장 큰 차이는 <strong>값을 다루는 방식</strong>입니다
이걸 쉽게 <strong>가방🎒 예시로</strong> 들어서 설명해볼게요</p>
</blockquote>
<h2 id="클래스class--공유하는-가방">클래스(class) = 공유하는 가방</h2>
<p>클래스는 여러 변수가 <strong>하나의 가방을 함께 쓰는 개념</strong>입니다</p>
<p>A와 B가 <em>같은 가방</em>을 들고 있다고 생각해보세요.
<img src="https://velog.velcdn.com/images/dudwntjs/post/79e7d0c0-4e5c-44ab-8066-b6e4d498177a/image.png" width="500" /></p>
<p>B가 그 가방에서 <strong>지갑을 꺼내 버리면</strong>?</p>
<p>👉 A가 나중에 그 가방을 열어도 지갑이 없습니다
👉 둘 다 <strong>동일한 가방(객체)을 공유하고 있기 때문</strong>이조!!</p>
<pre><code class="language-swift">class Bag {
    var items: [String]
    init(items: [String]) { self.items = items }
}

let bagA = Bag(items: [&quot;지갑&quot;, &quot;이어폰&quot;])
let bagB = bagA

bagB.items.remove(at: 0)</code></pre>
<h4 id="1-baga-생성">1. <code>bagA</code> 생성</h4>
<ul>
<li>힙(Heap)에 <code>[&quot;지갑&quot;, &quot;이어폰&quot;]</code>을 담고 있는 Bag 객체가 만들어짐</li>
<li>스택(Stack)의 <code>bagA</code> 변수는 이 Bag 객체의 <strong>주소</strong>만 가짐</li>
</ul>
<h4 id="2-bagb--baga">2. <code>bagB = bagA</code></h4>
<ul>
<li><p><code>bagA</code>가 들고 있던 <strong>동일한 주소</strong>가 <code>bagB</code>에도 복사</p>
<p>  ⇒ <code>bagA</code>와 <code>bagB</code>는 서로 다른 변수가 아니라 <strong>같은 Bag 객체를 함께 바라보는 두 개의 포인터</strong>가 된다</p>
</li>
</ul>
<h4 id="3-bagbitemsremoveat-0">3. <code>bagB.items.remove(at: 0)</code></h4>
<ul>
<li><code>bagB</code>가 바라보고 있는 힙 객체의 배열에서 <code>&quot;지갑&quot;</code>이 제거<ul>
<li>하지만 <code>bagA</code>도 같은 힙 객체를 참조하고 있으므로 <code>bagA</code>가 바라보는 items도 동일하게 변경되어 있겠조?</li>
</ul>
</li>
</ul>
<h4 id="4-출력-결과가--이어폰-인-이유">4. 출력 결과가  <code>[&quot;이어폰&quot;]</code> 인 이유?</h4>
<ul>
<li><p>bagB로 값을 변경했지만 bagA도 같이 바뀜</p>
<p>  → bagA와 bagB가 <strong>같은 힙 객체의 주소</strong>를 공유하기 때문‼️</p>
</li>
</ul>
<blockquote>
<h3 id="클래스는-참조-타입">클래스는 <strong>참조 타입</strong></h3>
</blockquote>
<ul>
<li>여러 변수가 <strong>힙에 있는 동일 객체의 주소를 공유</strong></li>
<li>그래서 한쪽에서 값을 바꾸면 <strong>다른 변수에서도 변경이 그대로 보임</strong></li>
<li>동작 과정에서 <strong>스택은 주소를 저장</strong>, <strong>힙은 실제 데이터를 저장</strong>하며 두 공간이 함께 협력해 객체를 관리</li>
</ul>
<hr />
<h2 id="구조체struct--복사해준-개인-가방">구조체(struct) = <strong>복사해준 개인 가방</strong></h2>
<p>구조체는 반대로 <strong>가방을 통째로 복사해서 주는 개념</strong></p>
<p>A가 자기 가방을 B에게 복사해서 하나 더 만들어 줬다고 해봅시다.
<img src="https://velog.velcdn.com/images/dudwntjs/post/eddd124e-9712-4ad0-bcfe-59e8c267ebee/image.png" width="500" /></p>
<p>B가 자신의 가방에서 지갑을 꺼내 버려도?</p>
<p>👉 A의 가방에는 여전히 지갑이 그대로 있음
👉 둘은 <strong>완전히 다른 가방(값)</strong> 을 가지기 때문</p>
<pre><code class="language-swift">struct Bag {
    var items: [String]
}

var bagA = Bag(items: [&quot;지갑&quot;, &quot;이어폰&quot;])
var bagB = bagA

bagB.items.remove(at: 0)
print(bagA.items)</code></pre>
<h4 id="1-baga-생성-1">1. <code>bagA</code> 생성</h4>
<ul>
<li>스택(Stack)에 <code>&quot;지갑&quot;, &quot;이어폰&quot;</code>을 담은 <strong>Bag 구조체 값 자체</strong>가 저장</li>
</ul>
<h4 id="2-bagb--baga-1">2. <code>bagB = bagA</code></h4>
<ul>
<li><p>구조체는 <strong>값 타입</strong>이므로 <code>bagA</code>의 내용을 그대로 <strong>복사</strong>해서 <code>bagB</code>에 저장</p>
<p>  ⇒  <code>bagA</code>와 <code>bagB</code>는 <strong>서로 다른 공간에 존재하는 독립적인 값</strong>을 가짐</p>
</li>
</ul>
<h4 id="3-bagbitemsremoveat-0-1">3. <code>bagB.items.remove(at: 0)</code></h4>
<ul>
<li><p><code>bagB</code> 내부의 items에서 <code>&quot;지갑&quot;</code>이 제거</p>
<p>  ⇒ <strong>bagB의 복사본</strong>만 변경하는 것이므로 <code>bagA</code>의 items에는 아무 영향도 주지 않음😎</p>
</li>
</ul>
<blockquote>
<h3 id="구조체는-값-타입">구조체는 <strong>값 타입</strong></h3>
</blockquote>
<ul>
<li>각 변수가 <strong>자신만의 데이터를 개별적으로 보관</strong></li>
<li>다른 변수에 할당하면 <strong>값 자체가 복사되어 독립적인 복사본이 만들어짐</strong></li>
<li>따라서 한쪽에서 값을 변경해도 <strong>다른 변수에는 아무 영향이 가지 않음</strong></li>
</ul>
<br />


<p>지금까지는 <strong>Class와 Struct의 본질 차이(참조 vs 값, 힙 vs 스택, 복사 방식)</strong>을 다뤘구요!
  다음 글에서 <code>힙/스택 메모리 구조, Dynamic Dispatch(vtable), Copy-On-Write, ARC, Protocol 확장</code>에 대해서 이어서 적어보도록 하겠습니다🤓</p>