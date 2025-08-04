# Minecraft-Style Crafting System

This crafting system provides a 3x3 crafting grid similar to Minecraft, with recipe definitions, live preview, and material consumption.

## Features

- **3x3 Crafting Grid**: Uses Snapable system for precise item placement
- **Recipe System**: Define shaped and shapeless recipes via ScriptableObjects
- **Live Preview**: Shows craftable items in real-time as you place materials
- **Material Consumption**: Automatically consumes ingredients when grabbing crafted items
- **Integration**: Works seamlessly with existing PickupPlaceSystem

## Quick Setup

### 1. Create a Crafting Table

1. Create an empty GameObject named "CraftingTable"
2. Add the `CraftingManager` component
3. Create a child GameObject named "SlotsParent" 
4. Create another child GameObject named "PreviewSlot"
5. Assign these in the CraftingManager:
   - `Crafting Table Parent` → SlotsParent
   - `Preview Slot Transform` → PreviewSlot

### 2. Auto-Generate the Grid

1. Select the CraftingTable GameObject
2. In the CraftingManager, click "Create Crafting Slots" button
3. This creates a 3x3 grid of CraftingSlot objects
4. Click "Assign Slots to Table" to link them to the CraftingTable

### 3. Create Recipes

1. Right-click in Project → Create → PickupPlaceSystem → Crafting Recipe
2. Configure the recipe:
   - **Recipe Name**: Descriptive name
   - **Result Item**: The prefab to create when crafted
   - **Is Shaped Recipe**: Check for position-specific recipes
   - **Shaped Pattern**: 3x3 grid of required items (for shaped recipes)
   - **Shapeless Ingredients**: List of required items (for shapeless recipes)

### 4. Add Recipes to Table

1. Select your CraftingTable
2. In the CraftingTable component, expand "Available Recipes"
3. Add your recipe ScriptableObjects to the list

## Usage

1. **Place Items**: Drag items onto the crafting slots using the pickup system
2. **See Preview**: When a valid recipe pattern is formed, a preview appears
3. **Craft Item**: Click on the preview to consume materials and get the crafted item
4. **Clear Grid**: Items are automatically consumed when crafting

## Recipe Examples

### Shaped Recipe: Sword
```
[ ]  [I]  [ ]
[ ]  [I]  [ ]  → Sword
[ ]  [S]  [ ]

I = Iron Ingot
S = Stick
```

### Shapeless Recipe: Bread
```
3x Wheat (any position) → Bread
```

## Component Reference

### CraftingManager
- **Auto Setup**: Automatically creates and configures crafting slots
- **Slot Spacing**: Distance between crafting slots
- **Preview Slot**: Where crafted item previews appear

### CraftingTable
- **Crafting Slots**: Array of 9 CraftingSlot components (0=top-left, 8=bottom-right)
- **Preview Slot**: Transform where preview items spawn
- **Available Recipes**: List of recipes this table can craft
- **Preview Material**: Optional material for preview items

### CraftingSlot
- **Slot Index**: Position in 3x3 grid (0-8)
- **Current Item**: Currently placed item (read-only)
- Extends Snapable for precise item placement

### CraftingRecipe (ScriptableObject)
- **Recipe Name**: Display name
- **Result Item**: Prefab to create when crafted
- **Result Count**: Number of items to create
- **Is Shaped Recipe**: Whether position matters
- **Shaped Pattern**: 3x3 array for shaped recipes
- **Shapeless Ingredients**: List for shapeless recipes

### CraftingPreview
- **Auto-added** to preview items
- Handles interaction and material consumption
- Implements Interactable interface for pickup system

## Advanced Usage

### Custom Crafting Slots
```csharp
// Create custom slot behavior
public class SpecialCraftingSlot : CraftingSlot
{
    public override void OnTriggerEnter(Collider other)
    {
        // Custom logic for special items
        base.OnTriggerEnter(other);
    }
}
```

### Recipe Validation
```csharp
// Custom recipe matching
public class CustomRecipe : CraftingRecipe
{
    public override bool MatchesPattern(GameObject[] gridItems)
    {
        // Custom matching logic
        return base.MatchesPattern(gridItems);
    }
}
```

### Multiple Crafting Tables
- Each CraftingTable can have different available recipes
- Create specialized crafting stations (furnace, anvil, etc.)
- Different grid sizes possible with custom implementations

## Troubleshooting

**Preview not appearing:**
- Check that recipes are added to CraftingTable.availableRecipes
- Verify recipe pattern matches exactly
- Ensure preview slot is assigned

**Items not snapping to slots:**
- Verify CraftingSlot colliders are triggers
- Check that items have Movable components
- Ensure slots are properly indexed (0-8)

**Materials not consumed:**
- Check that preview has CraftingPreview component
- Verify CraftingTable reference is set
- Ensure PlayerManager.Instance exists

**Recipe not matching:**
- For shaped recipes, position matters exactly
- For shapeless recipes, only item types and counts matter
- Check item name matching (removes "(Clone)" suffix)

## Integration Notes

- Works with existing Movable/PickUp system
- Uses SnapableDetector for slot detection  
- Requires PlayerManager for inventory management
- Compatible with outline effects for visual feedback