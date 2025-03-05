---
title: 인터페이스와 추상클래스
description: Interface And AbstractClass
author: unm7925
date: 2025-01-25 04:00:00 +0900
categories: [ Theory ]
tags: [ Csharp , Unity]
pin: false # 고정
# math: false 수학기호
# mermaid: false 수학기호
image:
  path: 'Main.png' # 이미지 이름
  # lqip:  # 저화질 넣기(인터넷)
  alt: 인터페이스와 추상클래스 싸움 수준 실화냐? # 부연설명명
media_subpath: '/assets/img/Github_Pages/Theory/InterfacAndAbstractClass' # 이미지 주소 ( 끝 전)
---

# __인터페이스와 추상클래스__

반갑다. 오늘은 인터페이스와 추상클래스에 관해서 알아보도록 하자. <br>
나는 대충 이거 알아요. or 이거 그냥 대충 쓰면 되는거 아님? 이라는 생각 <br>
나도 처음에는 했었다. 왜냐하면 딱히 큰 문제가 일어나지는 않는다. <br> <br>
하지만 `누군가가 전기톱을 시동을 키지않고 나무를 벤다고하면 어떤 생각이 드는가`<br><br>
비유가 잘 못 되었을 수 있다. 하지만 비슷하게도 누군가에겐 이해가 되지 않는 방법으로 우리가 행하고 있을 수 있다. <br>
인생에 정답은 없지만, 일단 만들어 둔 것은 만들어 둔 방향성에 맞게 사용해보기 위해 알아보도록 하자.

## __추상클래스__

먼저, 추상클래스에 관하여 알아보도록 하자. <br>
이름을 보면 딱 직관적으로 알 수 있을 것이다. `추상` 적인 클래스다~ 라는 말을 <br>
하지만 추상적인 것은 어떤 것인가? 바로 눈에 밟히지 않는 무형의 단어이다.<br>
그렇다면 우리 눈에 보이지 않는 단어들에 대한 것인가? 50% 다가온 것 같다.<br>
일단 내가 이 단어를 처음 봤을 때 받은 느낌은 이러하다. <br>

추상클래스..? 이게 뭔데? 추상.. 추상이니까 흠.. 그냥 먼가 먼가네. <br>

라는 생각밖에 하지 못했다. 왜냐하면 그 때 내가 무엇을 알까?
아직은 초보인 내가 생각하자면 추상이라는 단어와 어울리지는 않지만 <br>

필수적으로 공통적인 부분? 그런느낌이다. <br>
예를 들어 사람은 피가 흐른다. 아니아니 너무 현실적인 이야기야 <br>

나는 Unity를 사용하니까 게임제작에 관하여 말해보도록 할게 <br>
예를 들어 캐릭터들은 체력을 갖는다. 대강 이런것? ㅋㅋㅋㅋ <br>
내가 예시에 자꾸 버벅이는 이유는 이걸 처음보는 사람이 이해할 수 있을까? 라는 생각에 나온 버벅임이기 때문에 이해해주길 바란다.

그럼 이 감상은 그만하고 제대로 알아보도록 하자.

### __추상클래스의 정의와 특징__

#### __추상클래스의 정의__
- 추상클래스는 공통 속성과 동작을 `상속`받는 기본 설계도다.
- 일부는 기본 동작을 구현하고, 일부는 자식 클래스에서 구현하도록 `강제`한다.
  
-> 필자의 언어로 해석하자면
  - 추상클래스는 모두에게 해당되는 것들을 적어놓는 곳이다.
  - 추상클래스 본인이 구현해서 넘겨주던지, 자식 클래스에서 구현하던지 상관없다.
    ++ 하지만 모두 구현은 해야한다.

#### __추상클래스의 특징__
  - 공통 데이터 포함
    - 필드와 속성을 포함하여 공통 데이터를 `관리`할 수 있다.
    - 모든 자식 클래스에서 공통으로 필요한 데이터와 기본 동작 제공 된다.
  - 일부 구현 제공
    - 메서드를 구현하거나 추상메서드로 남겨 자식 클래스에서 구현한다.
  - 단일 상속만 가능
    - 클래스는 `하나의` 추상 클래스만 상속받을 수 있다
  - 공통 동작 재사용
    - 자식 클래스에서 공통 동작을 그대로 사용하거나, 필요에 따라 오버라이드 가능 (override<- 솔리드 본 사람 익숙 할 듯)

-> 필자의 언어로 해석
  - 정의-1 과 같음
  - 정의-2 와 같음
  - 애는 동족혐오 있어서 혼자가 편해.
  - 앞 문장은 정의-2와 같으나 오버라이드가능이란건 이미 구현된 걸 받아도 수정해서 사용할 수 있다는 말이다. 물려입는 옷 리폼 좀 하면 어때

#### ___추상클래스 코드드예제___
```
public abstract class CharacterBase : MonoBehaviour
{
    public int Health { get; protected set; } = 100;
    public float Speed { get; protected set; } = 5f;

    public virtual void Move(Vector3 direction)
    {
        transform.Translate(direction * Speed * Time.deltaTime);
        Debug.Log("Character is moving");
    }

    public virtual void TakeDamage(int damage)
    {
        Health -= damage;
        Debug.Log($"Remaining Health: {Health}");
        if (Health <= 0) Die();
    }

    protected abstract void Die(); // 각 캐릭터가 구현해야 할 동작
}

public class Player : CharacterBase
{
    protected override void Die()
    {
        Debug.Log("Player has died. Game Over!");
    }
}

public class Enemy : CharacterBase
{
    protected override void Die()
    {
        Debug.Log("Enemy has been defeated!");
        Destroy(gameObject);
    }
}
```

솔직히 보면 어떤 느낌인지 바로 올 확률이 높다. <br>
내가 생각해도 예시코드가 너무나도 친절하기 때문이다. <br>
틀 잡아줘. 공통점 찾아줘. 변형 보여줘. 다 해줬잖아. <br>

자 그러면 인터페이스로 넘어가보도록 하자. <br>
만약 추상클래스가 이해가 안된다 하면 인터페이스 설명 후 비교 예시를 들어줄테니 <br>
그것을 한번 참고해보기를 바란다.

## __인터페이스__

자 세상세상 말많은 인터페이스를 알아보도록 하자. <br>
인터페이스 어떤 느낌인가? 사실 물어볼 필요도 없다. <br>

이 단어를 보면 우리가 바로 떠올릴 것은 뭐 간단하다 유저인터페이스 아닌가? <br>
우리가 바라보는 그 화면 !! 바로 정답 외치며 오른 팔 내밀 것 같다. 옛날의 나는 말이지 <br>
하지만 안타깝게도 아니다. 비슷한 역할을 하냐? 음 그것도 아니다 ㅋㅋ <br>
그럼 예측이 안되니 바로 알아보도록 하자.

### __인터페이스 정의와 특징__

#### __인터페이스 정의__
- 클래스나 구조체가 반드시 구현해야 하는 메서드, 속성, 이벤트, 인덱서를 정의한 계약이다.
  - 어떤 객체가 반드시 해야 할 일을 맹세하는 틀 이라고도 한다.
- 구현 방식은 정의하지 않고, 단지 선언만 한다.

-> 필자의 언어로 해석하자면
- 아무거나 잘 먹는다.
- 먹긴 하는데 먹는 그대로 전해주니까 알아서 구현해놔라.
조금 표현이 저속할 수 있으나 내 생각은 그렇다.

#### __인터페이스 특징__
- 순수한 설계 틀
  - 구현 없이 메서드나 속성의 정의만 포함할 수 있지만 상속 받은 클래스는 반드시 구현해야한다. 
  <details> 
  <summary> 예시 </summary> <div markdown ="1">
  ![Image](interface.png)
  </div> 
  </details>
- 다중 구현 가능
  - C#은 클래스 다중 상속을 지원하지 않지만, `interface`를 통해 다중 구현 가능
    - 애는 유명한 방탕아임. 아무데나 다 감.
- 데이터 포함 불가
  - 필드나 구현된 메서드를 포함할 수 없기에 오직 메서드나 속성 정의만 가능
    - 애는 유명한 방탕아임. 책임감이란게 없음.
- 다형성 구현
  - 공통된 계약을 기반으로 객체가 동일한 방식으로 동작하게끔 만듦.
    - 애는 유명한 방탕아임. 남들에겐 엄격함.
- 유연한 역할 추가
  - 구현체에 구애받지 않고 인터페이스를 통해 서로 다른 객체를 교환하거나 확장 가능 ( 느슨한 결합 )
    - 애는 유명한 방탕아임. 여기저기 다 쑤시고 다님.
- 코드 재사용성
  - 동일한 메서드 구조를 강제하여 여러 클래스에서 재사용 가능하게 만듦
    - 애는 유명한 방탕아임. 은근 일 머리가 좋음
- 캡슐화
  - 내부 구현을 감추고 인터페이스로만 상호작용함
    - 애는 유명한 방탕아임. 핸드폰은 죽어도 안보여줌.

이번에는 좀 양이 많아서 내 생각을 바로 아래에 각주달 듯 달아버렸다.<br>
직관적이기 때문에 돌려 생각할 필요는 없다고 본다 <br>

바로 예제로 들어가도록 하자.

#### ___인터페이스 코드예제___
```
public interface IFlyable
{
    void Fly(); // 날기 동작
}

public class Bird : MonoBehaviour, IFlyable
{
    public void Fly()
    {
        Debug.Log("Bird is flying!");
    }
}

public class Airplane : MonoBehaviour, IFlyable
{
    public void Fly()
    {
        Debug.Log("Airplane is flying!");
    }
}
```

추상 클래스와 비슷하지만 뭔가 많이 비어보이지 않는가? <br>
바로 책임감이 없다 이 방탕아. 여기저기 찌르고 다니는 <br>
그래서 아주아주 마음에 든다. ㅋㅋㅋㅋ <br>
아무튼 바로 추상클래스와 인터페이스를 비교하며 한눈에 알아보도록 하자 <br>

## __인터페이스와 추상클래스 비교__

바로 알아볼 수 있도록 표로 나타내 주겠다.

|      ---       |                인터페이스                |                  추상클래스                   |
| :------------: | :--------------------------------------: | :-------------------------------------------: |
|      목적      |          특정 행동(역할)을 강제          |   공통 속성과 동작을 상속받아 재사용성 증대   |
|  데이터 포함   |                  불가능                  |                     가능                      |
|  메서드 구현   | 불가 ( 선언만 가능, 예시: `void Fly();`) | 일부 구현 가능 (예시: `virtual void Move()`)  |
| 다중 상속/구현 |              다중 구현 가능              |               단일 상속만 가능                |
|    사용예시    |      특정 역할 부여(날기, 공격 등)       | 공통 데이터와 동작 제공 (체력, 이동, 로직 등) |
|     유연성     |    높은 유연성 : 역할 추가, 변경 용이    |              제한적 : 단일 상속               |

차이는 간단하게 봤지만 그렇다고 둘이 사이가 안좋은 것은 아니다. <br>
아니 좋아도 너무 좋아.. 나루토와 사스케가 바로 떠오르는건 나뿐인가 <br>
대표적인 예시로는 보통 <br>
추상클래스로 캐릭터의 공통 데이터를 관리 <br>
인터페이스로 추가적인 행동 구현 <br>
시각적인 효과를 위해 바로 코드를 보여주도록 하겠다.

### ___인터페이스와 추상클래스 합작품___
```
public interface IAttackable
{
    void Attack();
}

public interface IFlyable
{
    void Fly();
}

public abstract class CharacterBase : MonoBehaviour
{
    public int Health { get; protected set; } = 100;

    public virtual void TakeDamage(int damage)
    {
        Health -= damage;
        Debug.Log($"Remaining Health: {Health}");
        if (Health <= 0) Die();
    }

    protected abstract void Die();
}

public class FlyingEnemy : CharacterBase, IFlyable, IAttackable
{
    public void Fly()
    {
        Debug.Log("Flying enemy is flying!");
    }

    public void Attack()
    {
        Debug.Log("Flying enemy attacks!");
    }

    protected override void Die()
    {
        Debug.Log("Flying enemy has been defeated!");
        Destroy(gameObject);
    }
}
```
너무나도 이상적인 사스케와 나루토가 아닌가. 누가 사스케고, 누가 나루토인지 무엇이 중하리 <br>

결론을 내가 마무리 하기 전에 스스로 생각해서 한번 메모장에라도 적어보길 바래.
필자의 결론은 토글로 숨겨두도록 할게.

<details>
<summary> 결론 </summary>
<div markdown ="1">
![image](End.png)
</div>
</details>