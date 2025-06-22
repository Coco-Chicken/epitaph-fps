# Simple State Machine

A lightweight state machine implementation for Unity games.

## Quick Start

### 1. Define your state enum
```csharp
public enum EnemyState
{
    Idle,
    Patrol,
    Chase,
    Attack
}
```

### 2. Create state classes with controller reference
```csharp
using StateMachine;

public class IdleState : StateBase<EnemyState>
{
    private EnemyController enemy;
    
    public IdleState(EnemyController enemy)
    {
        this.enemy = enemy;
    }
    
    public override EnemyState StateType => EnemyState.Idle;
    
    public override void OnEnter()
    {
        Debug.Log("Entering Idle State");
        enemy.StopMoving();
    }
    
    public override void OnUpdate()
    {
        // Access controller properties and methods
        if (enemy.CanSeePlayer())
        {
            enemy.stateMachine.TransitionTo(EnemyState.Chase);
        }
    }
}
```

### 3. Set up the state machine
```csharp
using StateMachine;

public class EnemyController : MonoBehaviour
{
    private StateMachine<EnemyState> stateMachine;
    
    void Start()
    {
        // Method 1: Pass all states in constructor (with controller reference)
        stateMachine = new StateMachine<EnemyState>(
            new IdleState(this),
            new PatrolState(this),
            new ChaseState(this),
            new AttackState(this)
        );
        
        // Method 2: Create empty and add states
        // stateMachine = new StateMachine<EnemyState>();
        // stateMachine.AddStates(new IdleState(this), new PatrolState(this), new ChaseState(this), new AttackState(this));
        
        // Initialize with starting state
        stateMachine.Initialize(EnemyState.Idle);
    }
    
    void Update()
    {
        // Update current state
        stateMachine.Update();
        
        // Handle transitions based on game logic
        if (ShouldChasePlayer())
        {
            stateMachine.TransitionTo(EnemyState.Chase);
        }
    }
}
```

## API Reference

### StateMachine<TStateType>
- `new StateMachine<T>(params IState<T>[] states)` - Create with initial states
- `AddState(IState<TStateType> state)` - Register a single state
- `AddStates(params IState<TStateType>[] states)` - Register multiple states
- `Initialize(TStateType startingState)` - Set initial state
- `Initialize()` - Initialize with first registered state
- `TransitionTo(TStateType nextStateType)` - Change to another state
- `Update()` - Update current state (call in Update/FixedUpdate)
- `CurrentStateType` - Get current state enum value

### StateBase<TStateType>
- `StateType` - The enum value for this state (must override)
- `OnEnter()` - Called when entering the state
- `OnUpdate()` - Called every frame while in this state
- `OnExit()` - Called when leaving the state

## Best Practices

1. **Keep states focused** - Each state should handle one specific behavior
2. **Pass controller reference** - States need access to the controller to be useful
3. **Use StateBase** - Inherit from StateBase instead of implementing IState directly
4. **Consider using switch statements** - For simple state machines, a switch statement might be cleaner than this framework
5. **Make controller methods public** - States need to access controller properties and methods

## Example: Simple AI Enemy

```csharp
public class SimpleEnemy : MonoBehaviour
{
    private StateMachine<EnemyState> fsm;
    private Transform player;
    private float detectionRange = 10f;
    
    void Start()
    {
        // States automatically register themselves based on their StateType property
        fsm = new StateMachine<EnemyState>(
            new IdleState(this),
            new ChaseState(this)
        );
        
        fsm.Initialize(EnemyState.Idle);
        player = GameObject.FindWithTag("Player").transform;
    }
    
    void Update()
    {
        fsm.Update();
        
        // Handle state transitions
        float distance = Vector3.Distance(transform.position, player.position);
        
        if (fsm.CurrentStateType == EnemyState.Idle && distance < detectionRange)
        {
            fsm.TransitionTo(EnemyState.Chase);
        }
        else if (fsm.CurrentStateType == EnemyState.Chase && distance > detectionRange * 1.5f)
        {
            fsm.TransitionTo(EnemyState.Idle);
        }
    }
}
```