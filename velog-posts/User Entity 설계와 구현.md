<h3 id="user-상속-구조-설계">User 상속 구조 설계</h3>
<p>이번 프로젝트에서 유저의 역할이 분명하게 구분되어 있어 부모 테이블인 User를 시작으로 Manager, Customer가 이를 상속받는 형태로 설계하여 진행하려고 한다.</p>
<p>JPA의 @Inheritance(strategy = InheritanceType.JOINED) 전략을 사용했다. 공통 필드는 User에 두고, 역할별로 필요한 필드는 하위 엔티티에서 관리한다.</p>
<pre><code class="language-java">@Entity
@Inheritance(strategy = InheritanceType.JOINED)
public class User {
    @Id @GeneratedValue
    private Long id;
    private String email;
    private String password;
    private String name;
    private String phone;
    @Enumerated(EnumType.STRING)
    private UserRole role;
}

@Entity
public class Manager extends User {
    private String career;
    private String experience;
}

@Entity
public class Customer extends User {
    @OneToMany(mappedBy = &quot;customer&quot;, cascade = CascadeType.ALL, orphanRemoval = true)
    private List&lt;CustomerAddress&gt; addressList = new ArrayList&lt;&gt;();
}</code></pre>
<hr />
<h3 id="상속-전략-선택-기준">상속 전략 선택 기준</h3>
<p>JOINED 전략을 사용함으로써 정규화된 테이블 구조를 유지했다.
조회 시에는 조인이 발생하지만, 역할별 필드 구성이 명확하게 분리되어 있기 때문에 JOINED 전략이 적합하다고 판단했다.</p>
<p>전략    설명    장점    단점
SINGLE_TABLE    하나의 테이블에 모두 저장    조회 빠름, JOIN 없음    NULL 컬럼 다수
JOINED    부모/자식 테이블 분리    정규화, 역할별 분리 용이    조회 시 JOIN 비용
TABLE_PER_CLASS    자식 클래스마다 테이블 따로 생성    독립성    중복, 쿼리 복잡도</p>
<hr />
<h3 id="orphanremoval-옵션">orphanRemoval 옵션</h3>
<p>자식 엔티티가 컬렉션에서 제거되었을 때 자동으로 삭제되도록 하기 위해 orphanRemoval = true를 사용했다.
단순히 연관 객체를 null로 설정하는 것이 아닌, 컬렉션에서 직접 remove 해야 작동한다.</p>
<pre><code class="language-java">@OneToMany(mappedBy = &quot;manager&quot;, cascade = CascadeType.ALL, orphanRemoval = true)
private List&lt;ManagerAvailability&gt; availabilityList = new ArrayList&lt;&gt;();

public void removeAvailability(ManagerAvailability availability) {
    availabilityList.remove(availability);
    availability.setManager(null);
}</code></pre>
<hr />
<h3 id="연관관계-편의-메서드">연관관계 편의 메서드</h3>
<p>연관관계 주인 쪽(setter)이 누락될 경우 저장되지 않는 문제를 방지하기 위해 편의 메서드를 정의했다.</p>
<pre><code class="language-java">public void addAvailability(ManagerAvailability availability) {
    availabilityList.add(availability);
    availability.setManager(this);
}

public void addAddress(CustomerAddress address) {
    addressList.add(address);
    address.setCustomer(this);
}
</code></pre>
<hr />
<h3 id="lombok-사용-원칙">Lombok 사용 원칙</h3>
<p>Lombok은 최소한으로 사용했다. 필드 전체에 무분별하게 적용하기보다는 필요한 곳에만 제한적으로 적용했다.
    •    @Getter, @NoArgsConstructor(access = PROTECTED)는 모든 엔티티에 공통 적용
    •    @Setter는 연관관계 설정 또는 특정 필드에만 사용
    •    @Builder는 생성자에만 사용
    •    @AllArgsConstructor, @Data는 사용하지 않음</p>
<pre><code class="language-java">@Getter
@NoArgsConstructor(access = AccessLevel.PROTECTED)
@Entity
public class CustomerAddress {
    @Id @GeneratedValue
    private Long id;

    @ManyToOne(fetch = FetchType.LAZY)
    private Customer customer;

    private String address;
    private String addressDetail;

    @Builder
    public CustomerAddress(String address, String addressDetail) {
        this.address = address;
        this.addressDetail = addressDetail;
    }

    public void setCustomer(Customer customer) {
        this.customer = customer;
    }
}</code></pre>
<hr />
<h3 id="도메인-상태-변경-메서드">도메인 상태 변경 메서드</h3>
<p>승인 여부를 변경하는 로직은 엔티티 내부에서 상태를 명확히 표현하는 메서드로 정의했다.</p>
<pre><code class="language-java">public void approve() {
    this.verified = true;
    this.verifiedAt = LocalDateTime.now();
}</code></pre>
<hr />
<h3 id="정리">정리</h3>
<p>역할이 명확히 구분된 도메인에서는 상속 구조가 설계의 복잡도를 줄여주었다.
JOINED 전략을 통해 테이블 정규화를 유지하고, 역할별 책임을 나눌 수 있었다.
연관관계는 cascade와 orphanRemoval을 함께 사용하고, 편의 메서드를 통해 안정성을 확보했다.
Lombok은 예측 가능한 범위에서 제한적으로 사용하는 것을 원칙으로 두었다.</p>
<h3 id="참고자료">참고자료</h3>
<p>[Jpa Entity 어노테이션 사용 주의] (<a href="https://chaewsscode.tistory.com/178">https://chaewsscode.tistory.com/178</a>)</p>