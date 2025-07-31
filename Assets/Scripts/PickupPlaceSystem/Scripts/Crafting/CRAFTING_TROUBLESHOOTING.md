# Crafting System Troubleshooting

## Setup Checklist

### 1. CraftingSlot Setup
Each crafting slot needs:
- ✅ `CraftingSlot` component
- ✅ `Snapable` component (CraftingSlot inherits from it)
- ✅ Collider with `isTrigger = true`
- ✅ Proper `slotIndex` (0-8)
- ✅ Reference to parent CraftingTable

### 2. CraftingTable Setup
The crafting table needs:
- ✅ `CraftingTable` component
- ✅ All 9 `craftingSlots` assigned in array
- ✅ `previewSlot` Transform assigned
- ✅ At least one recipe in `availableRecipes`

### 3. Item Setup
Items you want to craft with need:
- ✅ `Movable` or `PickUp` component
- ✅ Collider
- ✅ Proper layer (not "Overlay" or "Ignore Raycast")

## Debugging Steps

### 1. Check Console Logs
With the debug logging added, you should see:
- "CraftingSlot X: OnTriggerEnter" when item enters slot
- "CraftingSlot X: Item placed" when item is placed
- "CraftingTable: Checking for valid recipe" when grid changes
- "CraftingTable: Found matching recipe" when pattern matches

### 2. Common Issues & Solutions

**Issue: Items snap but no preview appears**

Solution 1: Check slot assignment
```
1. Select CraftingTable GameObject
2. Check CraftingTable component
3. Ensure all 9 slots are assigned (no null entries)
4. Ensure preview slot is assigned
```

Solution 2: Check recipe configuration
```
1. Select your recipe asset
2. Ensure Result Item is assigned
3. For shaped recipes: Check the 3x3 pattern
4. For shapeless: Check ingredients list
```

Solution 3: Check item detection
```
1. Make sure CraftingSlot colliders are triggers
2. Ensure items have colliders
3. Check that items aren't on ignored layers
```

### 3. Manual Setup Steps

If auto-setup didn't work, manually configure:

1. **Create Crafting Table Structure:**
```
CraftingTable (GameObject)
├── CraftingTable (Component)
├── CraftingManager (Component)
├── SlotsParent
│   ├── CraftingSlot_0 (with CraftingSlot component)
│   ├── CraftingSlot_1 (with CraftingSlot component)
│   ├── ... (9 total slots)
└── PreviewSlot (empty transform)
```

2. **Configure Each Slot:**
- Add `CraftingSlot` component
- Add `BoxCollider` with `isTrigger = true`
- Set `slotIndex` (0-8)
- Set `target` to the slot GameObject itself

3. **Wire Everything:**
- On CraftingTable component:
  - Drag all 9 slots to `craftingSlots` array
  - Drag PreviewSlot to `previewSlot`
  - Add your recipes to `availableRecipes`

### 4. Test Sequence

1. **Basic Test:**
   - Place one item on any slot
   - Check console for "OnSlotChanged" message
   - Should see grid update in console

2. **Recipe Test:**
   - Create simple 1-item recipe
   - Place that item on grid
   - Preview should appear

3. **Full Test:**
   - Set up full recipe pattern
   - All items should register
   - Preview appears
   - Click preview to craft

## Quick Fixes

### Fix 1: Regenerate Slots
```csharp
// In play mode console:
FindObjectOfType<CraftingManager>().CreateCraftingSlots();
FindObjectOfType<CraftingManager>().AssignSlotsToTable();
```

### Fix 2: Force Recipe Check
```csharp
// In play mode console:
FindObjectOfType<CraftingTable>().OnSlotChanged(0, null);
```

### Fix 3: Debug Current State
```csharp
// Check what's in the grid:
var table = FindObjectOfType<CraftingTable>();
for(int i = 0; i < 9; i++) {
    var slot = table.craftingSlots[i];
    Debug.Log($"Slot {i}: {(slot.CurrentItem != null ? slot.CurrentItem.name : "empty")}");
}
```

## Still Not Working?

1. **Check Layer Setup:**
   - Crafting slots should be on "Default" layer
   - Items should switch layers properly when picked up

2. **Check Scale:**
   - Slot colliders might be too small
   - Try increasing collider size

3. **Check Scripts:**
   - Ensure all scripts compiled without errors
   - Check that PickupPlaceSystem namespace is accessible

4. **Simple Test Recipe:**
   Create a recipe that needs just 1 item in slot 0:
   - Set all other slots to empty
   - Place item in top-left slot only
   - Should see preview immediately