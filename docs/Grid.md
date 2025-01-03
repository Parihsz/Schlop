# Grid

This module manages various entity positions on a grid using spatial hashing. It provides methods efficiently update, query, and manage the grid. Helper methods allow you to quickly query the grid selectively by type. It is recommended that you use a registry module to manage your entities and add to the grid. 

## Constants

- `GRID_SIZE` - The size of each grid cell.
- `UPDATE_INTERVAL` - The interval at which the grid is updated.

## Properties

- `grid: { [Vector3]: { [string]: { any } } }` - The main grid storage.
- `entities: { [Model]: string }` - The tracked entities with their types.

## Functions

### `UpdateGrid()`

Updates the grid with the current positions of all tracked entities. This function is called at a fixed interval defined by `UPDATE_INTERVAL`.

### `QueryGrid(position: Vector3, range: number, entityTypes: { string }?): { any }`

Efficiently queries the grid for specified types of entities within a specified range from the given position.

#### Parameters

- `position: Vector3` - The center position to query around.
- `range: number` - The range to query within.
- `entityTypes: { string }?` - Optional. The types of entities to query.

#### Returns

- `{ any }` - The entities found within the range.

### `AddEntity(entity: Model, entityType: string)`

Adds the given entity to the grid under the specified type.

#### Parameters

- `entity: Model` - The entity to add to the grid.
- `entityType: string` - The type of the entity (e.g., "players", "ais").

### `RemoveEntity(entity: Model)`

Removes the given entity from the grid.

#### Parameters

- `entity: Model` - The entity to remove from the grid.

### `GetNearbyEntities(model: Model?, range: number, entityTypes: { string }?): { any }`

Retrieves entities within a specified range of the given model.

#### Parameters

- `model: Model?` - The model to get nearby entities for.
- `range: number` - The range to query within.
- `entityTypes: { string }?` - Optional. The types of entities to query.

#### Returns

- `{ any }` - The entities found within the range.

## Usage

```lua
local Grid = require(Grid)

local playerModel = workspace.PlayerModel
local npcModel = workspace.NPCModel

Grid.AddEntity(playerModel, "players")
Grid.AddEntity(npcModel, "ais")

local nearbyEntities = Grid.GetNearbyEntities(playerModel, 100, { "players", "ais" }) --gets nearby players AND ais
for _, entity in nearbyEntities do
    print(entity.Name) 
end

local nearbyPlayers = Grid.GetNearbyEntities(playerModel, 100, { "players" }) --gets nearby players only
for _, player: Model in nearbyPlayers do --keep in mind this function returns the characters
    print(player.Name) 
end
```