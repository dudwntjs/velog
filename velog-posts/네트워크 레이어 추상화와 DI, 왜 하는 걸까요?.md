<p>네트워크 연결하려고 보니까,</p>
<blockquote>
<p>그냥 API 한 번 치면 되는데… 네트워크 폴더는 왜 파일이 이렇게 많지??</p>
</blockquote>
<p>폴더도 너무 많고, 이해가 안 가더라구요 ㅠ..ㅠ
그래서 직접 구조를 뜯어보며 공부해봤습니다 🤓</p>
<p>이 글은 <strong>Swift(iOS) 환경에서 URLSession을 직접 추상화한 네트워크 레이어 설계</strong>를 기준으로 설명합니다.
(외부 라이브러리 없이, <code>URLRequest 생성 → URLSession 통신 → Codable 디코딩 → Service DI/Mock 구조</code>로 설계된 형태입니다)</p>
<h3 id="배고픈-controller가-식당에-갑니다">배고픈 Controller가 식당에 갑니다</h3>
<p><img alt="" src="https://velog.velcdn.com/images/dudwntjs/post/cb835a88-791f-41c4-848a-7dd3c9efcaa1/image.png" /></p>
<ul>
<li>손님(Controller)은 “<strong>돈카츠 하나요!</strong>”라고만 말하면 됩니다</li>
<li>손님은 <code>어떻게 튀기고, 소스는 뭘 쓰고, 몇 도에 굽는지</code> 전혀 알 필요가 없습니다</li>
<li>그냥 “<strong><code>getUser(id: 1)</code></strong>” 라는 메뉴 이름만 알고 있으면 되겟조</li>
</ul>
<pre><code class="language-swift">protocol UserServiceProtocol {
    func getUser(id: Int) async throws -&gt; User
}

final class UserViewController: UIViewController {
    private let service: UserServiceProtocol  // ← 손님이 요리사한테만 말함

    init(service: UserServiceProtocol) {
        self.service = service
        super.init(nibName: nil, bundle: nil)
    }

    func viewDidLoad() {
        super.viewDidLoad()
        Task {
            let user = try await service.getUser(id: 1)
            // 여기서 UI 업데이트
        }
    }
}</code></pre>
<hr />
<h3 id="메뉴판api-protocol">메뉴판(API Protocol)</h3>
<pre><code class="language-swift">protocol UserServiceProtocol {
    func getUser(id: Int) async throws -&gt; User
}</code></pre>
<h4 id="💬-이건-뭐냐면">💬 이건 뭐냐면?</h4>
<ul>
<li><p>우리 가게에 있는 <strong>메뉴 이름들만 적힌 메뉴판</strong></p>
</li>
<li><p>돈가스, 라멘, 우동 같은 메뉴명만 적혀있고</p>
<p>  <strong>조리법(레시피)은 전혀 없음!</strong></p>
</li>
</ul>
<h4 id="📝-중요한-특징">📝 중요한 특징</h4>
<ul>
<li><p><strong>“우리 가게는 <code>getUser</code>라는 메뉴를 반드시 제공해야 합니다!”</strong></p>
<p>  라는 <strong>규칙서</strong> 역할</p>
</li>
<li><p>요리사는 이 규칙을 반드시 따라야 함</p>
</li>
<li><p>하지만 <em>어떻게 만들지</em>는 절대 안 적힘</p>
</li>
</ul>
<p>→ 즉, <strong>Controller(손님)가 볼 수 있는 건 메뉴 이름뿐!</strong>
→ 구현(조리방법)을 숨겨놓기 위한 추상화!</p>
<h3 id="레시피-카드targettype">레시피 카드(TargetType)</h3>
<pre><code class="language-swift">enum UserAPI: TargetType {
    case getUser(id: Int)

    var path: String { &quot;/users/\(id)&quot; }
    var method: HTTPMethod { .get }
    var task: NetworkTask { .requestPlain }
}</code></pre>
<h4 id="💬-이건-뭐냐면-1">💬 이건 뭐냐면?</h4>
<ul>
<li>돈가스를 만들기 위한 <strong>조리법 레시피 카드</strong></li>
<li>가게주인이 이걸 보고 음식(네트워크 요청)을 만드러용</li>
</ul>
<h4 id="📌-레시피에-적혀-있는-내용들">📌 레시피에 적혀 있는 내용들</h4>
<ul>
<li><p><strong>path</strong> = 어떤 주소로 갈까?</p>
</li>
<li><p><strong>method</strong> = 어떤 방식으로?</p>
</li>
<li><p><strong>task</strong></p>
<p>  → plain인지</p>
<p>  → body 넣는지</p>
</li>
<li><p><strong>headers</strong></p>
<p>  → JSON인지, 토큰 필요한지</p>
</li>
</ul>
<p><strong>⇒ 네트워크 요청을 만들기 위한 100% 구체적인 조리법</strong>!</p>
<hr />
<h3 id="가게-주인service가-요리사provider에게-이-레시피대로-만들어달라고-부탁">가게 주인(Service)가 요리사(Provider)에게 '이 레시피대로 만들어달라'고 부탁</h3>
<pre><code class="language-swift">final class UserService: UserServiceProtocol {
    private let provider: NetworkProvider&lt;UserAPI&gt;

    func getUser(id: Int) async throws -&gt; User {
        let response: BaseResponseBody&lt;UserDTO&gt; =
            try await provider.request(with: .getUser(id: id))

        return response.data.toDomain()
    }
}</code></pre>
<p><strong>가게 주인(Serivce)</strong>이 하는 일:</p>
<blockquote>
<p>요리사야, 여기 <code>UserAPI.getUser</code> 레시피 카드니까 이대로 요리 좀 해줘!</p>
</blockquote>
<p>요리사가 이 레시피(<strong><code>target</code></strong>)를 보고:</p>
<ul>
<li>어떤 주소로 갈지(<code>path</code>)</li>
<li>GET/POST 어떤 방법인지(<code>method</code>)</li>
<li>뭘 넣어야 하는지(<code>headers</code>, <code>parameters</code>)를 다 파악</li>
</ul>
<p><img alt="" src="https://velog.velcdn.com/images/dudwntjs/post/04c398be-82d6-4fe6-910e-704f6969739a/image.png" />
<span style="color: gray;">가게 주인이 시킨대로 요리사는 요리해야지잉~~ 시키는 말 알아들엇심!</span></p>
<hr />
<h3 id="요리사provider는-배달기사urlsession에게-장-봐와라고-시킴">요리사(Provider)는 배달기사(URLSession)에게 '장 봐와!'라고 시킴</h3>
<pre><code class="language-swift">let (data, response) = try await URLSession.shared.data(for: request)</code></pre>
<ul>
<li>Provider는 직접 마트에 가지 않아요‼️</li>
<li>대신 배달기사(<code>URLSession</code>)에게 시킴</li>
</ul>
<p>요리사 말:</p>
<blockquote>
<p>이 장보기 리스트(<code>request</code>)대로 재료 좀 사와!</p>
</blockquote>
<p>배달기사 말:</p>
<blockquote>
<p>🚚 다녀왔습니다~ 여기 데이터요!</p>
</blockquote>
<p>요리사는 <strong>배달기사에게 의존하지 않습니당 ⇒ 갈아끼우기 가능</strong></p>
<hr />
<h3 id="요리사provider가-재료data를-손질해서-요리를-만듦decode">요리사(Provider)가 재료(Data)를 손질해서 요리를 만듦(Decode)</h3>
<pre><code class="language-swift">return try JSONDecoder().decode(T.self, from: data)</code></pre>
<ul>
<li>데이터(JSON)를 깨끗하게 손질해서</li>
<li>그걸 손질하고(Decode), 요리 완성(Decoded DTO)해서 
가게 주인(Service)에게 전달</li>
</ul>
<p>손님 입장에서는 아무것도 이 과정을 전혀 모르는 게 포인트입니다🌟</p>
<hr />
<h3 id="가게-주인service이-플레이팅dto-→-domain-model">가게 주인(Service)이 플레이팅(DTO → Domain Model)</h3>
<pre><code class="language-swift">return response.data.toDomain()</code></pre>
<ul>
<li>요리사는 반조리 음식(DTO)을 '그릇'에 올리지 않음</li>
<li>최종 플레이팅은 가게 주인(Service)의 역할</li>
<li>DTO → Domain 변환</li>
<li>필요한 정보만 남기고 모델을 깔끔하게 만듦</li>
</ul>
<p><img alt="" src="https://velog.velcdn.com/images/dudwntjs/post/4bf5199a-4b39-4c27-9132-1123da3231ee/image.jpeg" /> <span style="color: gray;">가게 주인만 변환 책임을 가지고 있다!</span></p>
<hr />
<h3 id="손님controller은-완성된-음식user을-받음">손님(Controller)은 완성된 음식(User)을 받음</h3>
<pre><code class="language-swift">let user = try await service.getUser(id: 1)
nameLabel.text = user.name</code></pre>
<p>손님(Controller)은:</p>
<ul>
<li>고기 어디서 샀는지</li>
<li>어느 서버에서 받아왔는지</li>
<li>JSON 파싱이 어떻게 됐는지</li>
</ul>
<p>전혀 신경 안 씀.
그냥 <strong>먹기 좋은 User 모델만 받으면 됨😋</strong></p>
<p>흐름 전체는 이렇게 돌아옵니다</p>
<blockquote>
<p><strong>URLSession → Provider → Service → Controller</strong></p>
</blockquote>
<p><strong>손님이 요리사한테 직접 “이거 해줘요~”라고 안 하잖아요.</strong>
주문·결제는 <strong>가게 주인</strong>에게 하고, 주인이 <strong>요리사에게 전달</strong>하죠.</p>
<p>그러니까 <strong>Controller가 Provider(URLSession)를 직접 찾으면 안 됩니다</strong>
항상 Service(가게 주인)를 통해서 가야 해요!</p>
<table>
<thead>
<tr>
<th>역할</th>
<th>설명</th>
</tr>
</thead>
<tbody><tr>
<td><strong>손님(Controller)</strong></td>
<td>&quot;getUser(id: 1) 주세요&quot; 라고만 주문 (네트워크/조리 과정 모름)</td>
</tr>
<tr>
<td><strong>가게주인(Service)</strong></td>
<td>주문 접수 → API 스펙 전달 → DTO → Domain 모델로 변환(플레이팅)</td>
</tr>
<tr>
<td><strong>요리사(Provider)</strong></td>
<td>API 스펙으로 <code>URLRequest</code> 생성 → <code>URLSession</code> 통신 실행 → JSON <code>Codable</code> 디코딩</td>
</tr>
<tr>
<td><strong>배달기사(URLSession)</strong></td>
<td>실제 서버 통신을 담당 (데이터를 가져오는 유일한 네트워크 실행 주체)</td>
</tr>
</tbody></table>
<hr />
<h3 id="네트워크-통신-추상화-주방을-숨기는-설계">네트워크 통신 추상화: 주방을 숨기는 설계</h3>
<h4 id="👎-추상화-안-한-경우">👎 추상화 안 한 경우</h4>
<ul>
<li>손님이 직접 마트 가서 고기 사고</li>
<li>직접 튀기고</li>
<li>직접 그릇 꺼내고</li>
<li>직접 설거지까지 함</li>
</ul>
<p>→ Controller가 직접 URLSession, JSONDecode, NetworkError, Request 다 하는 코드🥵</p>
<h4 id="👍-추상화한-경우">👍 추상화한 경우</h4>
<ul>
<li>손님은 “돈카츠 하나요”만 말함</li>
<li>가게 주인이(Service)가<ul>
<li>요리사(Provider)에게 레시피 카드(TargetType)를 넘기고</li>
<li>배달기사(URLSession)가 다녀온 재료(Data)를 받아서</li>
<li>플레이팅하고(DTO→Domain)</li>
<li>완성된 음식(User Model)을 손님에게 올려줌</li>
</ul>
</li>
</ul>
<p>손님은:</p>
<ul>
<li>고기 어디서 샀는지</li>
<li>소스 브랜드가 뭔지</li>
<li>프라이팬인지 오븐인지 
  → <strong>하나도 몰라도 됨</strong></li>
</ul>
<p>이게 바로 URLSession + Request + Decode + Error를 다 감춘 <code>추상화</code> 🤩</p>
<hr />
<h3 id="di의존성-주입-가게-주인을-갈아끼우는-주문-시스템">DI(의존성 주입): 가게 주인을 갈아끼우는 주문 시스템</h3>
<p>우리가 가게주인이 바뀌었다고 식당을 안가진 않자나요? 
일단 <strong>메뉴판(Protocol)</strong> 보고 먹고싶은 돈카츠가 있다~~ 하면 가자나요</p>
<pre><code class="language-swift">protocol UserServiceProtocol {
    func getUser(id: Int) async throws -&gt; User
}</code></pre>
<p>프로토콜에서 명시를 해줍니당 → 가게 주인이라면 <code>getUser(id:)</code> 메뉴는 반드시 제공해야 합니다!</p>
<p>그럼 손님 입장에서는 가짜 주인이 와서 원래 대로 요리사 써서 음식을 내놓을지, 편의점 음식을 데워 올지 모르는 거조ㅠ..ㅠ
그럼 손님은 <strong>메뉴판(Protocol)</strong>만 보고 <code>getUser</code> 메뉴 있죠? 그거 주세요~ 할거란 말이조!!</p>
<blockquote>
<p>이 구조에서 <strong>“진짜 가게주인” 대신 
“가짜(모의) 가게주인”를 넣는 게 DI + Mock입니당</strong></p>
</blockquote>
<pre><code class="language-swift">class MockUserService: UserServiceProtocol {
    func getUser(id: Int) async throws -&gt; User {
        return User(name: &quot;Mock User&quot;, age: 999)
    }
}</code></pre>
<p>컨트롤러 생성 시:</p>
<pre><code class="language-swift">let vc = UserViewController(service: MockUserService())
// 이제 이 화면은 절대 네트워크 안 탐</code></pre>
<ul>
<li><p>실제 가게: 진짜 요리사가 주방, 배달기사 다 씀 → 실제 돈카츠</p>
</li>
<li><p>테스트용: 편의점 도시락 데워서 손님한테 내는 가짜 주인이던 뭐든 잘 모름</p>
<p>  하지만 손님 눈에는 “그냥 돈카츠”로 보임</p>
</li>
</ul>
<p>손님(Controller)은
굳이 주인이 여기 주인이 맞나?????? <strong>알 필요가 없음</strong></p>
<p>손님은 ‘돈카츠 하나요’만 말하고, 
주인–요리사–배달이 알아서 처리해 완성된 요리만 받는 구조 
<strong>= 잘 추상화된 네트워크 레이어라고 볼 수 있습니다</strong></p>
<hr />
<h4 id="전체-흐름-요약✨">전체 흐름 요약✨</h4>
<table>
<thead>
<tr>
<th>단계</th>
<th>동작</th>
<th>사용 파일</th>
</tr>
</thead>
<tbody><tr>
<td>1</td>
<td>Controller → Service 호출</td>
<td>(없음)</td>
</tr>
<tr>
<td>2</td>
<td>Service → BaseService.request()</td>
<td><strong>BaseService</strong></td>
</tr>
<tr>
<td>3</td>
<td>BaseService가 TargetType을 사용하여 URLRequest 생성</td>
<td><strong>NetworkProvider</strong></td>
</tr>
<tr>
<td>4</td>
<td>baseURL 불러옴</td>
<td><strong>Environment</strong></td>
</tr>
<tr>
<td>5</td>
<td>요청 DTO 인코딩</td>
<td><strong>ModelType (RequestModelType)</strong></td>
</tr>
<tr>
<td>6</td>
<td>TargetType 프로토콜로 API 스펙 제공</td>
<td><strong>TargetType</strong></td>
</tr>
<tr>
<td>7</td>
<td>요청 로그 출력</td>
<td><strong>NetworkLogger</strong></td>
</tr>
<tr>
<td>8</td>
<td>URLSession으로 서버 요청</td>
<td>(URLSession)</td>
</tr>
<tr>
<td>9</td>
<td>응답 로그 출력</td>
<td><strong>NetworkLogger</strong></td>
</tr>
<tr>
<td>10</td>
<td>HTTP 상태 코드 검사</td>
<td><strong>NetworkError</strong></td>
</tr>
<tr>
<td>11</td>
<td>응답 JSON 디코딩</td>
<td><strong>BaseResponseBody</strong>, <strong>ResponseModelType</strong></td>
</tr>
<tr>
<td>12</td>
<td>DTO → Domain 변환 후 반환</td>
<td>(Service 내부)</td>
</tr>
<tr>
<td>13</td>
<td>Controller가 DomainModel로 UI 업데이트</td>
<td>(없음)</td>
</tr>
</tbody></table>