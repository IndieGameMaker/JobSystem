# JobSystem

# SIMD (Single Instruction, Multiple Data)

## 1. SIMD란?
- 하나의 명령어로 여러 데이터를 동시에 처리하는 기술
- CPU, GPU가 벡터 레지스터를 이용해 여러 데이터를 병렬로 계산
- 주로 수치 연산(덧셈, 곱셈, 비교 등) 성능을 크게 향상

---

## 2. 일반 연산 vs SIMD 연산

### 일반 CPU 처리 (Scalar 연산)
- for문으로 한 번에 하나씩 계산
- 예시: input[0] * 2 → input[1] * 2 → input[2] * 2 순차 처리.

### SIMD CPU 처리 (Vector 연산)
- 여러 데이터를 한 번에 묶어 동시에 계산
- 예시: (input[0], input[1], input[2], input[3]) × 2를 한 번에 처리.

→ 3~4배 이상 속도 향상 가능.

---

## 3. Unity와 SIMD

- Unity의 Burst Compiler는 자동으로 SIMD 최적화를 적용
- 지원하는 SIMD 명령어 세트: SSE, AVX, NEON 등.
- NativeArray를 순차적으로 접근하는 구조에서 최적화 효과가 가장 큼

---

## 4. SIMD 최적화 잘 되는 코드 예
- 복잡한 조건문(if), 분기(branch)가 많으면 SIMD 적용이 어렵다.
 
✅ 좋은 예 (벡터화 가능)
```csharp
for (int i = 0; i < array.Length; i++)
{
    array[i] *= 2f;
}
```

❌ 나쁜 예 (벡터화 어려움)
```csharp
for (int i = 0; i < array.Length; i++)
{
    if (array[i] > 0)
        array[i] *= 2f;
}
```


classDiagram
    class IDamageable {
        <<interface>>
        +Damage(int damage) void
        +IsDead bool
    }
    
    class EntityBase {
        <<abstract>>
        #float maxHealth
        #float currentHealth
        #bool isDead
        #Animator animator
        #Awake() void
        +Damage(int damage) void
        +IsDead bool
        #Die() void
    }
    
    class EnemyBase {
        <<abstract>>
        +enum State
        #State state
        #float traceDist
        #float attackDist
        #Transform playerTr
        #NavMeshAgent agent
        #bool isStunned
        #OnEnable() void
        #OnDisable() void
        #Awake() void
    }
    
    class Barrel {
        -int hitCount
        -BarrelDataSO barrelDataSO
        -AudioSource audio
        -CinemachineImpulseSource impulseSource
        +Start() void
        -ExpBarrel() void
        +Damage(int damage) void
    }
    
    class SpiderController {
        -BehaviorGraphAgent spiderAgent
        -float _hp
        +Start() void
        +OnCollisionEnter(Collision) IEnumerator
    }
    
    IDamageable <|.. EntityBase
    EntityBase <|-- EnemyBase
    IDamageable <|.. Barrel
    
    note for EntityBase "모든 엔티티의 기본 클래스"
    note for EnemyBase "적 캐릭터의 기본 동작"
    note for Barrel "데미지를 받으면 폭발하는 배럴"
    note for SpiderController "EnemyBase로 리팩토링 필요"
