<p>Swift에서 공식적으로 <strong>컬렉션 타입(Collection Types)</strong>이라 부르는 건 <code>Array</code>, <code>Set</code>, <code>Dictionary</code> 세 가지입니다.
다만 여러 값을 묶어 사용하는 경우가 많아서, 실전 활용 관점에서 <code>Tuple</code>도 함께 정리해보았습니다!</p>
<p><a href="https://developer.apple.com/documentation/swift/collections">Collections | Apple Developer Documentation</a></p>
<h2 id="1-배열-array-순서가-있는-데이터와-스택">1. 배열 (Array): 순서가 있는 데이터와 스택</h2>
<p>인덱스를 통한 빠른 접근이 필요할 때 사용하는 가장 기본적인 자료형입니다.</p>
<blockquote>
<ul>
<li><strong>스택(Stack) 활용</strong>: 스위프트는 별도의 스택 자료형이 없으므로 배열의 <code>append()</code>와 <code>popLast()</code>를 활용합니다. 두 연산 모두 시간 복잡도가 O(1)입니다.</li>
<li><strong>탐색시간 주의:</strong> 맨 앞 요소를 지우는 <code>removeFirst()</code>는 나머지 요소를 모두 앞으로 당겨야 하므로 O(N)의 시간이 걸립니다.
🚨큐(Queue)가 필요할 때는 지양해야 합니다🚨</li>
</ul>
</blockquote>
<h4 id="1단계-2차원-격자grid-초기화하기"><strong>1단계: 2차원 격자(Grid) 초기화하기</strong></h4>
<p>코딩테스트의 단골 문제인 <strong>미로 찾기(BFS/DFS)</strong>나 <strong>동적 계획법(DP)</strong>을 풀 때 가장 먼저 하는 작업입니다.</p>
<pre><code class="language-swift">let rows = 5
let cols = 5
var grid = Array(repeating: Array(repeating: 0, count: cols), count: rows)</code></pre>
<p><code>Array(repeating:count:)</code> 구문을 중첩해서 사용합니다.</p>
<p>먼저 <code>0</code>이 <code>cols</code>개만큼 들어있는 1차원 배열(열)을 만들고, 그 배열을 다시 <code>rows</code>번 반복해서(행) 5x5 크기의 2차원 배열을 단 한 줄 만에 0으로 초기화합니다.</p>
<h4 id="2단계-스택stack으로-활용하기"><strong>2단계: 스택(Stack)으로 활용하기</strong></h4>
<p>스위프트는 스택 자료형이 따로 없어서 배열을 스택처럼 사용해야 합니다.</p>
<pre><code class="language-swift">var stack = [Int]()
stack.append(10)
stack.append(20)

if let top = stack.popLast() { 
    print(top) 
}</code></pre>
<ul>
<li><code>append()</code>로 맨 뒤에 값을 밀어 넣고(Push), <code>popLast()</code>로 맨 뒤의 값을 빼냅니다.</li>
<li><code>popLast()</code>는 배열이 비어있을 때 앱이 튕기지 않고 안전하게 <code>nil</code>을 반환합니다.
  ⇒ 이 두 작업은 모두 <strong>시간 복잡도가 O(1)</strong>이라서 코딩테스트에서 시간 초과 걱정 없이 쓸 수 있습니다👍</li>
</ul>
<br /> 

<hr />
<h2 id="2-집합-set-중복-제거와-o1-빠른-탐색">2. 집합 (Set): 중복 제거와 O(1) 빠른 탐색</h2>
<p>순서가 없는 대신 해시(Hash) 알고리즘을 사용하여 데이터의 '존재 여부'를 파악하는 데 압도적인 속도💨를 냅니다.</p>
<blockquote>
<ul>
<li><strong>빠른 탐색</strong>: 배열의 <code>contains()</code>는 배열 전체를 훑어야 해서 O(N)이 걸리지만, 집합의 <code>contains()</code>는 O(1)만에 찾아냅니다.</li>
</ul>
</blockquote>
<ul>
<li><strong>중복 제거</strong>: 배열의 중복 데이터를 단숨에 제거할 때 유용합니다.</li>
</ul>
<h4 id="1단계-o1-속도로-방문-처리하기"><strong>1단계: O(1) 속도로 방문 처리하기</strong></h4>
<p>배열로 '내가 여길 방문했었나?'를 찾으면 전체를 다 뒤져야 해서 매우 느려요😂
이럴 때 집합을 써야합니다❗️</p>
<pre><code class="language-swift">var visited = Set&lt;Int&gt;()
visited.insert(1)

if !visited.contains(2) {
    visited.insert(2)
}</code></pre>
<p>해시(Hash) 알고리즘을 사용하기 때문에 <code>contains()</code>로 특정 값이 있는지 찾는 속도가 <strong>O(1)</strong>입니다.
그래프 탐색 문제에서 <code>visited</code> 배열을 <strong>Set으로 바꾸기</strong>만 해도 시간 초과를 해결하는 경우가 많습니다✨</p>
<h4 id="2단계-배열의-중복-단숨에-제거하기"><strong>2단계: 배열의 중복 단숨에 제거하기</strong></h4>
<pre><code class="language-swift">let withDuplicates = [1, 1, 2, 2, 3]
let uniqueNumbers = Array(Set(withDuplicates))</code></pre>
<p>집합은 중복을 절대 허용하지 않는다는 점을 이용한 꼼수입니다ㅎㅎ
중복이 있는 배열을 <code>Set</code>에 한 번 담갔다 빼서 다시 <code>Array</code>로 감싸주면, 중복이 완벽하게 제거된 배열을 얻을 수 있습니다🤩</p>
<br />

<hr />
<h2 id="3-튜플-tuple-가벼운-데이터-묶음과-다중-반환">3. 튜플 (Tuple): 가벼운 데이터 묶음과 다중 반환</h2>
<p>서로 다른 타입의 데이터를 가볍게 하나로 묶어 사용할 때 유용합니다. 한 번 선언되면 요소를 추가하거나 삭제할 수 없는 상수 성격을 가집니다.</p>
<blockquote>
<ul>
<li><strong>좌표 처리</strong>: 별도의 구조체(Struct)를 선언하기 번거로운 코딩테스트 환경에서 좌표 <code>(r, c)</code>를 묶어 관리하기 좋습니다.</li>
</ul>
</blockquote>
<ul>
<li><strong>다중 반환값</strong>: 함수에서 두 개 이상의 결과값(예: 몫과 나머지, 최소/최대값)을 반환할 때 안전하고 직관적으로 사용할 수 있습니다.</li>
</ul>
<h4 id="1단계-큐queue에-여러-정보-한-번에-담기"><strong>1단계: 큐(Queue)에 여러 정보 한 번에 담기</strong></h4>
<p>좌표 문제에서 <code>x</code>, <code>y</code> 값과 이동 횟수를 구조체(Struct)로 만들기는 너무 번거로워요😫</p>
<pre><code class="language-swift">var queue = [(r: Int, c: Int, dist: Int)]()
queue.append((r: 0, c: 0, dist: 1))

let current = queue.removeFirst()
print(current.r)
print(current.c)</code></pre>
<p>소괄호 <code>()</code> 안에 이름과 타입을 지정해서 가벼운 데이터 묶음을 만듭니다.
꺼내서 쓸 때도 <code>current.r</code>처럼 직관적인 이름으로 바로 접근할 수 있어 가독성이 아주 좋아집니다.</p>
<h4 id="2단계-함수에서-여러-값-반환하기"><strong>2단계: 함수에서 여러 값 반환하기</strong></h4>
<pre><code class="language-swift">func getMinMax(array: [Int]) -&gt; (min: Int, max: Int) {
    return (array.min() ?? 0, array.max() ?? 0)
}

let result = getMinMax(array: [5, 2, 9, 1])</code></pre>
<p>Swift의 함수는 기본적으로 값을 하나만 반환하지만, 튜플을 사용하면 2개 이상의 값을 안전하고 깔끔하게 넘겨줄 수 있습니다🌟</p>
<br />

<hr />
<h2 id="4-딕셔너리-dictionary-키-값-매핑과-빈도수-카운팅">4. 딕셔너리 (Dictionary): 키-값 매핑과 빈도수 카운팅</h2>
<p>고유한 '키(Key)'를 해시 연산하여 '값(Value)'과 연결합니다.</p>
<blockquote>
<ul>
<li><strong>빈도수 계산</strong>: 문자열이나 배열 내 요소의 등장 횟수를 셀 때 매우 강력한 무기가 됩니다.</li>
</ul>
</blockquote>
<ul>
<li><strong>빠른 조회</strong>: 키를 통한 값의 조회, 수정, 삭제가 O(1)에 수렴하므로 특정 ID나 문자에 매칭되는 데이터를 빠르게 찾을 때 필수적입니다.</li>
</ul>
<h4 id="1단계-빈도수-측정"><strong>1단계: 빈도수 측정</strong></h4>
<p>어떤 단어나 숫자가 몇 번 나왔는지 셀 때 가장 많이 쓰는 '치트키' 같은 코드입니다🔥
코테에서 많이 쓰여요!</p>
<pre><code class="language-swift">let words = [&quot;apple&quot;, &quot;banana&quot;, &quot;apple&quot;]
var countDict = [String: Int]()

for word in words {
    countDict[word, default: 0] += 1 
}</code></pre>
<p><code>default: 0</code> 부분이 핵심인데요🤓 딕셔너리에 &quot;apple&quot;이라는 키가 없으면 기본값인 0을 넣고 1을 더해줍니다.
키가 있는지 없는지 <code>if-else</code>로 검사할 필요 없이 이 한 줄로 빈도수 계산이 끝납니다.</p>
<h4 id="2단계-값을-기준으로-내림차순-정렬하기"><strong>2단계: 값을 기준으로 내림차순 정렬하기</strong></h4>
<p>가장 많이 등장한 단어 순서대로 답을 내야 할 때 사용합니다.</p>
<pre><code class="language-swift">let sortedKeys = countDict.sorted { $0.value &gt; $1.value }.map { $0.key }</code></pre>
<p>딕셔너리 자체는 순서가 없지만, <code>sorted</code>를 사용하면 (키, 값) 형태의 튜플 배열로 바뀝니다.</p>
<p>클로저 <code>{ $0.value &gt; $1.value }</code>를 통해 값을 기준으로 내림차순 정렬을 하고, <code>map</code>을 붙여서 정렬된 결과의 키(단어)들만 쏙 빼내어 새로운 배열로 만듭니다.</p>
<h4 id="3단계-값-삭제하기"><strong>3단계: 값 삭제하기</strong></h4>
<pre><code class="language-swift">countDict[&quot;banana&quot;] = nil</code></pre>
<p>딕셔너리에서 특정 데이터를 지우고 싶다면 키에 <code>nil</code>을 할당해주면 깔끔하게 삭제됩니다.</p>
<br />


<hr />
<h2 id="📝-핵심-요약-비교표">📝 핵심 요약 비교표</h2>
<table>
<thead>
<tr>
<th>자료형</th>
<th>데이터 중복</th>
<th>탐색(<code>contains</code>) 시간</th>
<th>코딩테스트 핵심 용도</th>
</tr>
</thead>
<tbody><tr>
<td><strong>Array</strong></td>
<td>허용</td>
<td>O(N)</td>
<td>스택(Stack) 구현, 2차원 맵 생성, 순차 탐색</td>
</tr>
<tr>
<td><strong>Set</strong></td>
<td>불가</td>
<td>O(1)</td>
<td>중복 제거, 그래프 탐색 시 방문(<code>visited</code>) 처리</td>
</tr>
<tr>
<td><strong>Tuple</strong></td>
<td>허용</td>
<td>-</td>
<td><code>(행, 열, 거리)</code> 등 가벼운 좌표 처리 및 다중 반환</td>
</tr>
<tr>
<td><strong>Dictionary</strong></td>
<td>키: 불가, 값: 허용</td>
<td>O(1)(Key 기준)</td>
<td>단어 빈도수 카운팅, 1:1 빠른 데이터 해시 매핑</td>
</tr>
</tbody></table>
<h3 id="🎯-언제-어떤-자료형을-써야-할까">🎯 언제 어떤 자료형을 써야 할까?</h3>
<h4 id="array"><strong>Array</strong></h4>
<ul>
<li><p><strong>순서</strong>가 중요할 때</p>
</li>
<li><p>뒤에서 넣고 빼는 구조가 필요할 때 (<code>append</code>, <code>popLast</code> → O(1))</p>
</li>
<li><p>2차원 격자 생성이 필요할 때</p>
</li>
<li><p>⚠️ <code>removeFirst()</code>는 O(N) → 큐로는 비추천</p>
<p>  <strong>⇒ 스택, 2차원 배열, 순차 탐색 문제</strong></p>
</li>
</ul>
<h4 id="set"><strong>Set</strong></h4>
<ul>
<li><p>특정 값의 존재 여부를 <strong>빠르게 확인</strong>해야 할 때</p>
</li>
<li><p><strong>중복을 제거</strong>해야 할 때</p>
</li>
<li><p>방문 체크가 필요할 때 (<code>visited</code>)</p>
<p>  <strong>⇒ 그래프 탐색, 중복 제거, 빠른 contains</strong></p>
</li>
</ul>
<h4 id="tuple"><strong>Tuple</strong></h4>
<ul>
<li><p><strong>여러 값</strong>을 가볍게 묶고 싶을 때</p>
</li>
<li><p>구조체 선언 없이 빠르게 좌표를 다뤄야 할 때</p>
</li>
<li><p>함수에서 여러 값을 반환할 때</p>
<p>  ⇒  <strong>(row, col, dist), (min, max) 반환</strong></p>
</li>
</ul>
<h4 id="dictionary"><strong>Dictionary</strong></h4>
<ul>
<li><p>키를 통해 값을 빠르게 찾아야 할 때</p>
</li>
<li><p><strong>빈도수</strong>를 세야 할 때</p>
</li>
<li><p>ID → <strong>값 형태의 매핑</strong>이 필요할 때</p>
<p>  ⇒ <strong>문자열/숫자 카운팅, 해시 매핑 문제</strong></p>
</li>
</ul>
<br />

<p><span style="color: gray;">저도 코테 잘 못푸는데😂 그래도 이렇게 개념을 정리해보니, 막연했던 부분들이 조금은 선명해진 느낌이에요ㅎㅎ 다들 도움이 되셨으면 좋겠네요!</span></p>