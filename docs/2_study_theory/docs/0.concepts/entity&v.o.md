---
layout: default
title: "• DDD: Entity&V.O"
grand_parent: "study(방법론)"
parent: "concepts"
nav_order: 7
has_children: true
---


## 0. Legacy
<br>
지금까지 일반적으로 구축해온 소프트웨어 시스템은 대부분 관계형(RDS) 데이터베이스를 사용해왔다. BESTCare 2.0도 당연히 아주 많은 테이블을 기반으로 한 관계형 기반의 시스템이며, 오라클 함수(Oracle procedure)에 비즈니스가 녹아있기 때문에 사실상 테이블이 도메인과 1:1 관계를 가지는 것이나 마찬가지다.
<br><br>
**Legay 시스템 :  데이터베이스 테이블 == 도메인 모델 == entity들의 집합**
<br><br>
모든 테이블은 유일키(primary key)를 가지고 있으므로 BESTCare2.0를 포함한 대부분의 과거 시스템은 DDD의 관점에서 'Entity로만 이루어진 시스템'을 가지고 있다. 그렇기 때문에 Cloud HIS 프로젝트를 진행할 때 특별히 DDD를 공부하지 않은 대부분의 개발자들은 Value Object의 필요성에 대해 잘 느끼지 못해왔다. 그 결과 많은 도메인의 모양이 기존 BESTCare2.0의 테이블을 그대로 모방해서 entity로 정의되는 결과가 만들어졌고, 이는 DDD의 이점을 많이 상실한 결과라고 볼 수 있다.
<br><br>
이번 글은 'Entity/Value Object가 무엇이냐'보다는 '**왜, 어떻게 entity의 개수를 줄이는가**'에 초점을 맞춰서 보면 쉬울 수 밖에 없다.
<br><br>
aggregate보다는 명확하고 쉬운 개념이기 때문에 googling으로 설명들을 많이 찾아볼 수 있다.
<br>
[entity vs v.o 1](https://enterprisecraftsmanship.com/posts/entity-vs-value-object-the-ultimate-list-of-differences/)
<br>
[entity vs v.o 2](https://dzone.com/articles/ddd-part-ii-ddd-building-blocks)
<br>
## 1. Entity
* **정의**
<br><br>
id(identity)를 가진 object
<br><br>
* **특징**
<br><br>
-- id가 같은 두개의 entity는 같다고 취급된다. 즉, 소프트웨어 세계 내의 entity들은 id로 인해 '같은 존재'가 정의된다.
<br><br>
-- 변화 가능(mutable)하다. id 외의 속성들은 변화할 수 있고, 변화했다 해도 entity는 여전히 그 entity이다.
<br><br>
-- Q: '주민등록번호'는 도메인 id로 써도될까?
<br><br>
* **구현**

```csharp
public abstract class EntityBase
{
    public string Id { get; set; }

    public override int GetHashCode()
    {
        return Id.GetHashCode();
    }

    public override bool Equals(object obj)
    {
        if (obj == null || !(obj is EntityBase))
        {
            return false;
        }

        if (this.GetType() != obj.GetType())
        {
            return false;
        }

        EntityBase entity = (EntityBase)obj;

        return entity.Id == this.Id;
    }

    public static bool operator ==(EntityBase left, EntityBase right)
    {
        if (object.Equals(left, null))
        {
            return object.Equals(right, null);
        }
        else
        {
            return left.Equals(right);
        }
    }

    public static bool operator !=(EntityBase left, EntityBase right)
    {
        return !(left == right);
    }
}
```
<br><br>
-- Id가 있다.
<br><br>
-- equal 비교가 id에 의해서 이루어진다.
<br><br>
-- hashcode 또한 id에 의해서 해석된다.
<br><br><br><br>

## 2. Value Obejct
* **정의**
<br><br>
id가 없는 object
<br><br>
* **특징**
<br><br>
-- 각각이 가진 속성(attribute)에 의해서만 같다는 비교를 할 수 있다.
<br><br>
-- 변화가 불가능하다.(immutable) 만약 변경이 필요하다면, 그 변경된 객체는 원래 것과 본질적으로 다른 것이므로 새로 생성한다는 코드를 짜야 한다.
<br><br>
* **코드**
<br><br>

```csharp
public abstract class ValueObjectBase
{
    protected static bool EqualOperator(ValueObjectBase left, ValueObjectBase right)
    {
        if (ReferenceEquals(left, null) ^ ReferenceEquals(right, null))
        {
            return false;
        }
        return ReferenceEquals(left, null) || left.Equals(right);
    }


    protected static bool NotEqualOperator(ValueObjectBase left, ValueObjectBase right)
    {
        return !(EqualOperator(left, right));
    }

    protected abstract IEnumerable<object> GetAtomicValues();

    public override bool Equals(object obj)
    {
        if (obj == null || obj.GetType() != GetType())
        {
            return false;
        }
        ValueObjectBase other = (ValueObjectBase)obj;
        IEnumerator<object> thisValues = GetAtomicValues().GetEnumerator();
        IEnumerator<object> otherValues = other.GetAtomicValues().GetEnumerator();
        while (thisValues.MoveNext() && otherValues.MoveNext())
        {
            if (ReferenceEquals(thisValues.Current, null) ^ ReferenceEquals(otherValues.Current, null))
            {
                return false;
            }
            if (thisValues.Current != null && !thisValues.Current.Equals(otherValues.Current))
            {
                return false;
            }
        }
        return !thisValues.MoveNext() && !otherValues.MoveNext();
    }

    public override int GetHashCode()
    {
        return GetAtomicValues()
            .Select(x => x != null ? x.GetHashCode() : 0)
            .Aggregate((x, y) => x ^ y);
    }
}
```
<br>
value object 예시
<br>

```csharp
public class ValueObjectExample : ValueObjectBase
{
    public string Property1 { get; private set; }
    public string Property2 { get; private set; }

    public ValueObjectExample(string entity, string entityId)
    {
        Property1 = entity;
        Property2 = entityId;
    }

    protected override IEnumerable<object> GetAtomicValues()
    {
        yield return Property1;
        yield return Property2;
    }
}
```

<br><br>
-- id가 없다.
<br><br>
-- 가지고 있는 주요한 속성들이 그 객체의 비교에 사용된다.
<br><br>

## 3. 바람직한 소프트웨어: 최소한의 Entity
* Eric Evans가 DDD 책에서 이야기한 것에 의하면
<br>
entity의 식별성을 관리하는 일은 매우 중요하지만 그 밖의 객체에 식별성을 추가한다면 **시스템의 성능이 저하**되고, **분석 작업이 별도로 필요**하며, **모든 객체를 동일한 것으로 보이게** 해서 모델이 혼란스러워질 수 있다.
<br>
소프트웨어는 복잡성과의 끊임 없는 전투다. 그러므로 우리는 **특별하게 다뤄야할 부분과 그렇지 않은 부분을 구분**해야한다.
<br><br>
-- 위와 같은 이유로 eric evans는 'entity로만 이루어진 시스템'을 피해야한다고 설명하고 있다.
<br><br>
## 4. Entity vs Value Object
<br><br>
그렇다면 entity와 value object를 어떻게 구분하는가? 정답은 각자의 경험에 있지만, 이미 경험을 해본 사람들의 (까다로운) 예시를 봐보는 것도 좋겠다.
<br><br>
* DDD 책의 'chapter five. a model expressed in software' - 'value object' 중
<br><br>
![image](https://user-images.githubusercontent.com/55048882/71131833-31128900-2239-11ea-8a29-4730fd5eba81.png)
<br><br>
"주소"는 Value object일까?
<br><br>
1. 우편 주문 회사에서 사용하는 소프트웨어에서는 신용카드를 확인하고 물건을 보낼 주소가 필요하다. 하지만 룸메이트도 같은 회사에 물건을 주문하더라도 두 사람이 같은 곳에 있다는 사실은 중요하지 않다. 이 경우 주소는 xxx다.
<br><br>
2. 우편 서비스에 사용하는 소프트웨어에서는 배송 경로를 조직화하기 위해 그 나라를 지역, 도시, 우편구역, 블록, 개별 주소로 끝나는 계층 구조의 형태로 만들 수도 있다. 이 같은 주소 객체는 계층구조상 부모로부터 우편번호를 도출해낼 수 있는데, 만약 우편 서비스에서 배달구역을 재할당하기로 했다면 거기에 속하는 모든 주소는 부모 계층의 주소를 따라 바뀔 것이다. 이 경우 주소는 xxx다.
<br><br>
3. 전기 설비 회사에서 사용하는 소프트웨어의 경우 주소는 전선 및 전기공급의 목적지에 해당한다. 룸메이트가 각각 전기점검을 요청한다면 전기 설비 회사에서는 점검 목적지를 파악할 필요가 있다. (1)이 경우 주소는 xxx다. 아니면 모델에서 주소 속성이 포함된 Entity인 "거주지"와 설비 점검을 연관시킬 수도 있다. (2)이 경우 주소는 xxx다.
<br><br><br><br>
답:
<br><br>
1. Value Object
2. Entity
3. (1)Entity (2)Value Object