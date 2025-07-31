# Recipe Setup Guide

## Creating Recipes - Step by Step

### 1. Create a Recipe Asset
1. Right-click in your Project window
2. Go to: **Create → PickupPlaceSystem → Crafting Recipe**
3. Name your recipe (e.g., "Sword Recipe", "Bread Recipe")

### 2. Configure the Recipe
Select your recipe asset and you'll see these options:

#### Basic Info:
- **Recipe Name**: Display name for the recipe
- **Result Item**: The prefab that gets created when crafted
- **Result Count**: How many items to create (usually 1)

#### Recipe Type:
- **Is Shaped Recipe**: 
  - ✅ **Checked** = Position matters (like Minecraft sword recipe)
  - ❌ **Unchecked** = Position doesn't matter (like bread recipe)

#### For Shaped Recipes (Position Matters):
You'll see a 3x3 grid in the inspector:
- Drag item prefabs into the grid slots
- Empty slots = nothing required in that position
- Set count for each ingredient if needed

#### For Shapeless Recipes (Position Doesn't Matter):
You'll see a list:
- Add ingredients to the "Shapeless Ingredients" list
- Set how many of each item is needed

### 3. Add Recipe to Crafting Table
1. Select your CraftingTable GameObject in the scene
2. Find the **CraftingTable** component
3. Expand "Available Recipes"
4. Increase the size and drag your recipe assets here

## Recipe Examples

### Example 1: Sword (Shaped Recipe)
```
Recipe Name: "Iron Sword"
Result Item: IronSwordPrefab
Is Shaped Recipe: ✅ Checked

3x3 Grid:
[ ]  [Iron]  [ ]
[ ]  [Iron]  [ ]
[ ]  [Stick] [ ]
```

### Example 2: Bread (Shapeless Recipe)
```
Recipe Name: "Bread"
Result Item: BreadPrefab  
Is Shaped Recipe: ❌ Unchecked

Shapeless Ingredients:
- Wheat x3
```

## Quick Recipe Testing

In the recipe inspector, use these buttons:
- **Clear Recipe**: Removes all ingredients
- **Test Recipe**: Shows a preview of what the recipe looks like

## Common Issues

**Recipe not working?**
- Make sure the recipe is added to your CraftingTable's "Available Recipes" list
- Check that item names match (the system compares prefab names)
- For shaped recipes, position must match exactly
- For shapeless recipes, you need the exact count of each ingredient

**Items not matching?**
- The system compares prefab names with "(Clone)" removed
- Make sure your pickupable items use the same prefabs as in the recipe

## Pro Tips

1. **Start Simple**: Create a basic 1-item recipe first to test
2. **Use Shapeless**: For simple recipes where position doesn't matter
3. **Test in Play Mode**: Place items on the crafting table to see if recipes work
4. **Check Console**: Look for any error messages while crafting

## Recipe Templates

### Tools (Shaped)
```
Pickaxe:          Sword:           Axe:
[I][I][I]         [ ][I][ ]        [I][I][ ]
[ ][S][ ]         [ ][I][ ]        [I][S][ ]
[ ][S][ ]         [ ][S][ ]        [ ][S][ ]
```

### Food (Shapeless)
```
Bread: 3x Wheat
Cake: 3x Milk + 2x Sugar + 1x Egg + 3x Wheat
Stew: 1x Bowl + 1x Mushroom + 1x Carrot
```

### Blocks (Shaped)
```
Stairs:           Chest:
[I][ ][ ]         [W][W][W]
[I][I][ ]         [W][ ][W]
[I][I][I]         [W][W][W]
```

Now you can create recipes and assign them to your crafting tables!