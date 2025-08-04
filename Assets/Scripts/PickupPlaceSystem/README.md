# Unity Pickup & Place System

A modular, reusable pickup and placement system for Unity games. This system allows players to pick up objects, carry them, and place them down with visual feedback and collision detection.

## Features

- **Single-item inventory** - Players can carry one object at a time
- **Visual feedback** - Outline effects show interactable and placeable states
- **Smart placement** - Collision detection prevents placing objects inside others
- **Snap points** - Define precise placement positions for objects
- **Surface alignment** - Objects align to surface normals when placed
- **Modular architecture** - Easy to extend with new interactable types

## Installation

1. Copy the entire `PickupPlaceSystem` folder into your Unity project's Assets folder
2. Install required dependencies:
   - Unity's New Input System package
   - QuickOutline (or similar outline effect system)

## Quick Start

### 1. Setup the Player

Add these components to your player GameObject:
- `CameraRayController` (on the camera)
- `PlayerManager` 
- `ReticleManager`
- `InteractableDetector`
- `MovableDetector`
- `SnapableDetector` (optional, for snap points)

### 2. Create Pickupable Objects

Add one of these components to any GameObject you want to be pickupable:
- `PickUp` - Basic pickupable object
- `Extractable` - Object that becomes pickupable when interacted with
- `Movable` - Base class for custom pickupable behavior

### 3. Configure Layers

The system uses these layers:
- **Default** - Normal objects
- **Overlay** - Objects being carried (prevents ray interference)
- **Ignore Raycast** - Objects that shouldn't be detected

### 4. Input Configuration

Set up your Input Actions with:
- An "Interact" action (e.g., left mouse button)

## Component Reference

### Core Components

**Movable**
- Base component for all pickupable objects
- Handles movement logic and state management
- Properties:
  - `isBeingMoved` - Whether object is currently being carried
  - `checkForStandingCollisions` - Validate placement position

**PickUp** 
- Extends Movable with basic pickup functionality
- Automatically handles interaction

**Extractable**
- Converts to Movable when interacted with
- Useful for objects embedded in the environment

**Snapable**
- Defines snap points for precise object placement
- Works with SnapableDetector to override placement position

### Detection System

**CameraRayController**
- Central ray casting system
- Broadcasts ray information to all detectors
- Attach to player camera

**InteractableDetector**
- Detects objects player can interact with
- Shows visual feedback (outline, reticle)
- Handles interaction input

**MovableDetector**
- Manages object placement while carrying
- Validates placement positions
- Shows placement preview

**SnapableDetector**
- Detects available snap points
- Overrides placement position when near snap points

### Managers

**PlayerManager**
- Manages player inventory (single item)
- Tracks currently carried object

**ReticleManager**
- UI feedback system
- Shows different reticle states based on interaction

## Usage Examples

### Basic Pickupable Object
```csharp
// Just add the PickUp component to any GameObject with a Collider
GameObject cube = GameObject.CreatePrimitive(PrimitiveType.Cube);
cube.AddComponent<PickUp>();
```

### Custom Pickupable with Special Behavior
```csharp
public class MagicOrb : Movable
{
    public override void StartMoving()
    {
        base.StartMoving();
        // Add glow effect while carrying
        GetComponent<Light>().enabled = true;
    }
    
    public override void StopMoving()
    {
        base.StopMoving();
        // Remove glow when placed
        GetComponent<Light>().enabled = false;
    }
}
```

### Extractable Object
```csharp
// For objects that need to be "pulled out" before picking up
GameObject embeddedGem = new GameObject("Gem");
embeddedGem.AddComponent<Extractable>();
```

### Creating Snap Points
```csharp
// Add Snapable component to define placement positions
GameObject pedestalTop = new GameObject("Pedestal Snap Point");
pedestalTop.AddComponent<Snapable>();
```

## Customization

### Visual Feedback
The system uses outline effects by default. To use your own:
1. Modify the outline code in `Interactable.cs`
2. Replace QuickOutline references with your system

### Interaction Distance
Adjust max interaction distance in:
- `InteractableDetector.maxDistance`
- `MovableDetector` placement distance

### Layer Configuration
Change layer names in the scripts if your project uses different layers

## Architecture Overview

```
Player Input → CameraRayController → Detectors → Object Components
                                         ↓
                                   Visual Feedback
                                         ↓
                                   State Changes
```

## Troubleshooting

**Objects fall through floor when placed**
- Ensure objects have colliders
- Check layer settings

**Can't pick up objects**
- Verify InteractableDetector is on player
- Check object has Interactable-derived component
- Ensure object is on correct layer

**Visual feedback not showing**
- Install outline effect package
- Check outline component references

## Dependencies

- Unity 2020.3 or newer
- Unity's New Input System
- Outline effect system (QuickOutline or similar)

## License

This system was extracted from the Picturesque game project. Feel free to use and modify for your own projects.