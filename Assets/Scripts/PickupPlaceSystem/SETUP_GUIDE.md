# Setup Guide - Pickup & Place System

## Prerequisites

1. Unity 2020.3 or newer
2. Unity's New Input System installed
3. An outline effect solution (see Outline Setup section below)

## Step-by-Step Setup

### 1. Import the Package

- Copy the entire `PickupPlaceSystem` folder to your Unity project's Assets folder
- Unity will compile the scripts automatically

### 2. Configure Project Layers

Add these layers to your project (Edit → Project Settings → Tags and Layers):
- `Overlay` (for carried objects)
- `Ignore Raycast` (if not already present)

### 3. Set Up Input Actions

Create or modify your Input Actions asset:

1. Create an Action Map called "Player"
2. Add an Action called "Interact" 
3. Set binding to Left Mouse Button (or preferred key)
4. Set Action Type to "Button"

### 4. Player Setup

On your Player GameObject:

1. **On the Main Player Object:**
   - Add `PlayerManager` component
   - Add `ReticleManager` component

2. **On the Player's Camera:**
   - Add `CameraRayController` component
   - Add `InteractableDetector` component
   - Add `MovableDetector` component
   - Add `SnapableDetector` component (optional)

3. **Configure Components:**
   - On `CameraRayController`: Assign your Input Actions asset and set the interact action name
   - On `InteractableDetector`: Set `maxDistance` (default: 10f)  
   - On `ReticleManager`: Assign your reticle UI elements

### 5. Create Pickupable Objects

For each object you want to be pickupable:

1. Add a Collider component
2. Add an outline effect component (see Outline Setup below)
3. Add one of these components:
   - `PickUp` - For basic pickupable objects
   - `Extractable` - For objects that need to be "pulled out" first
   - Or extend `Movable` for custom behavior

### 6. UI Setup

Create a simple reticle:
1. Create a Canvas (if you don't have one)
2. Add an Image as child of Canvas
3. Set to center of screen
4. Assign to `ReticleManager.reticle`

### 7. Outline Setup

Choose one of these outline solutions:

#### Option A: Simple Built-in Outline (Recommended for beginners)
1. Add the `SimpleOutline` component to each pickupable object
2. This uses Unity's emission system for a basic glow effect

#### Option B: QuickOutline (If you have it from Asset Store)
1. Install QuickOutline from the Asset Store
2. Add the `QuickOutlineAdapter` component to each pickupable object
3. Uncomment the implementation section in `QuickOutlineAdapter.cs`

#### Option C: Custom Outline Solution
1. Create a class that implements `IOutlineEffect`
2. Add your custom outline component to pickupable objects

## Testing Your Setup

1. Place a cube in your scene
2. Add `SimpleOutline` component to it
3. Add `PickUp` component to it
4. Play the scene
5. Look at the cube - it should show an outline
6. Click to pick it up
7. Click again to place it down

## Common Issues

**No outline appears:**
- Ensure the object has an IOutlineEffect component (like SimpleOutline)
- Check that the component is properly configured
- Verify the object's material supports emission (for SimpleOutline)

**Can't pick up objects:**
- Check Input Actions asset is assigned to CameraRayController
- Verify PlayerManager and detectors are set up on player
- Ensure object has a Collider and pickup component
- Check that the interact action name matches in your Input Actions

**Objects fall through surfaces:**
- Add Rigidbody to pickupable objects
- Ensure placement surfaces have colliders

## Next Steps

- Customize the visual feedback
- Adjust interaction distances
- Create snap points for precise placement
- Extend Movable class for special object behaviors