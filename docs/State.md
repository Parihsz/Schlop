# State

This module provides a lightweight state machine implementation for managing the states and transitions of entities. It includes networking functionality to synchronize state transitions across server and clients. State machines are referenced by name and have properties to control replication.

## Classes

### `State`

#### Properties

- `states: { [string]: StateConfig }` - The states of the state machine.
- `currentState: string?` - The name of the current state.
- `currentStateConfig: StateConfig?` - The configuration of the current state.
- `name: string` - The name of the state machine.
- `replicateToClients: boolean` - Whether to replicate state transitions to clients.
- `replicateToServer: boolean` - Whether to replicate state transitions to the server.

#### Methods

- `State(stateConfig: StateConfig) -> ()` - Adds a new state to the state machine.
- `Transition(stateName: string, entity: any, networked: boolean?) -> ()` - Transitions the state machine to the specified state.
- `GetState() -> string?` - Returns the name of the current state.
- `Update(entity: any) -> ()` - Updates the current state.
- `SetReplication(replicateToClients: boolean, replicateToServer: boolean) -> ()` - Sets the replication properties of the state machine.

## Interfaces

### `StateConfig`

Represents the configuration for a state in the state machine.

#### Properties

- `Name: string` - The name of the state.
- `OnEnter(entity: any) -> ()` - The function to call when entering the state.
- `OnExit(entity: any) -> ()?` - Optional. The function to call when exiting the state.
- `Update(entity: any) -> ()?` - Optional. The function to call when updating the state.

## Functions

### `CreateMachine`

Creates a new state machine with the specified parameters.

#### Parameters

- `name: string` - The name of the state machine.
- `states: { [string]: StateConfig }?` - Optional. The states of the state machine.

#### Returns

- `StateMachine` - The newly created state machine instance.

## Usage

### Creating a New State Machine

```lua
local function InitializeStateMachine()
	local fsm = StateMachine.CreateMachine("CombatAI")

	fsm:State({
		Name = "Passive",
		OnEnter = StateFunctions.PassiveOnEnter,
		Update = StateFunctions.PassiveUpdate,
	})

	fsm:State({
		Name = "Attacking",
		OnEnter = StateFunctions.AttackingOnEnter,
		Update = StateFunctions.AttackingUpdate,
	})

	fsm:State({
		Name = "Fleeing",
		OnEnter = StateFunctions.FleeingOnEnter,
		OnExit = StateFunctions.FleeingOnExit,
		Update = StateFunctions.FleeingUpdate,
	})

	return fsm
end
```