# [복합키와 식별 관계 매핑](http://acornpub.co.kr/book/jpa-programmig)

- 식별 관계 : 부모 테이블의 키본 키를 자식 테이블의 기본 키와 외래 키로 사용하는 관계
- 비식별 관계 : 부모 테이블의 키본 키를 자식 테이블의 외래 키로만 사용하는 관계

## 복합키
### @IdClass
```
@Entity
@IdClass(ParentId.class)
public class Parent {
  @Id
  @Column(name = "PARENT_ID1")
  private String id1;

  @Id
  @Column(name = "PARENT_ID2")
  private String id2;

  ...
}

public class ParentId implements Serializable {
  private String id1;
  private String id2;

  public ParentId() {}

  public ParentId(String id1, String id2) {
    this.id1 = id1;
    this.id2 = id2;
  }

  @Override
  public boolean equals(Object o) {...}

  @Override
  public int hashCode() {...}
}

```
```
Parent parent = new Parent();
parent.setId1("myId1");
parent.setId2("myId2");
em.persist(parent);
```
- 식별자 클래스의 속성명과 엔티티에서 사용하는 식별자의 속성명이 같아야 한다.
- Serializable 인터페이스 구현
- equals, hashCode 구현
- 기본 생성자
- 식별자 클래스는 public

### @EmbeddedId
```
@Entity
public class Parent {
  @EmbeddedId
  private ParentId id;

  ...
}

@Embeddable
public class ParentId implements Serializable {

  @Column(name = "PARENT_ID1")
  private String id1;

  @Column(name = "PARENT_ID2")
  private String id2;

  public ParentId() {}

  public ParentId(String id1, String id2) {
    this.id1 = id1;
    this.id2 = id2;
  }

  @Override
  public boolean equals(Object o) {...}

  @Override
  public int hashCode() {...}
}
```
```
Parent parent = new Parent();
ParentId parentId = new ParentId("myId1", "myId2");
parent.setId(parentId);
em.persist(parent);
```

- @Embeddable 어노테이션을 붙여주어야 한다
- Serializable 인터페이스 구현
- equals, hashCode 구현
- 기본 생성자
- 식별자 클래스는 public

### 복합 키: 식별 관계 매핑
#### @IdClass
- 식별 관계는 기본 키와 외래 키를 같이 매핑해야 하므로 식별자 매핑인 @Id와 연관관계 매핑을 함께 사용한다.
#### @EmbeddedId
- @EmbeddedId로 식별 관계를 구성 할 때는 식별 관계로 사용할 연관 관계의 속성에 @MapsId를 사용한다.
@IdClass와 다른 점은 @Id 대신에 @MapsId를 사용한다. @MapsId의 속성 값은 @EmbeddedId를 사용한 식별자 클래스의 기본 키 필드를 지정하면 된다.
