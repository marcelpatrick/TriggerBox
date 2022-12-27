# TriggerBox

# Ingridients
## 1. Actor to be Spawned
   - Add NEW Blueprint class of Actor type for the actor to be Spawned
   - Add a mesh to it
   - on details turn simulate physics on 

## 2- Spawner
   - Add NEW C++ AActor class
   - Create a BP based on this C++ class
   - Drag the BP component into the world to the place where you want it to spawn the actor

## 3A- Trigger Box
   - Add a new C++ Trigger Box class
   - Drag the trigger box C++ component into the world
   - in Details:
     - Uncheck "actor hidden in game" so that the trigger box is visible
     - Click on the collision component and Add a static mesh component to it > select a mesh and a material for it so that the trigger box is vizible in the game
     - Select the collision of the mesh as "NoCollision".

## 3B- Trigger Volume
   - Add a new C++ Trigger Volume class
   - Drag the trigger Volume C++ component into the world
   - Drag a cube component to overlap on the trigger volume so that it is visible. set cube collision as no collision.

# Preparation

## 1- Connect the trigger box / Volume and the Actor to be spawned to the Spawner
   - Inside Spawner C++, create an instance of the actor to be spawned using TSubclassOf<type> and exposing it to Unreal with UPROPERTY
```
- TSubclassOf:
   - TSubclassOf creates a variable that is an instance of a class of the specific type defined inside <>
   - The difference between TSubclassOf<AActor> MyActor and AActor MyActor is that the former is a reference to a class from which I can create new objects of the same type and the later is a reference to an object itself.
   - We can only reference objects that are already created in the world at gameplay. To spawn new objects that are not yet created we need to reference a class of that object type. So that when we spawn we create a new object derived from that class object.
   - We use TSubclassOf<ClassType> instead of UClass* because TSubclassOf allows us to pass into it only objects of that specific type - the dropdown in the blueprint will only show objects of that type - representing a safety measure to avoid errors of assigning to it objects that do not belong to the type of actor we want to spawn. Whereas UClass* allows to assign any type of object.
```
   - In Unreal open the Spawner BP and select the BP of the actor to be spawned in the UPROPERTY dropdown you just exposed
   - in Spawner C++, #include "Engine/TriggerBox.h" in the header file, Create a TriggerBox / Volume instance pointer variable. Expose it to Unreal with UPROPERTY. 
   - In the Spawner BP component details select your trigger box component as the UPROPERTY field you just created
   - OR, In Unreal, select the Spawner component and go to the Details tab, select your trigger box / trigger volume component in the dropdown 
   
   - Custom overlap functions
     - In Spawner C++ header file Declare your custom Callback function MySpawnActor() passing instances of the AActor class OverlappedActor and OtherActor pointers as params and expose it with UFUNCTION()
     
```
 - UFUNCTION:
   - Just like with UPROPERTY, which exposes variables to a Blueprint, UFUNCTION allows Callback functions in your C++ code to be called by a Blueprint event. 
   - Use UFUNCTION to allow the Callback function - MyOnBeginOverlap - to bind to the Delegate - OnActorBeginOverlap.
   - The Delegate OnActorBeginOverlap will listen to the overlap event on the TriggerBox component in the world and will broadcast the data of this event to the Callback function MyOnBeginOverlap so that it can perform a custom action when this event occurs.
  
```
   
  - Define the MySpawnActor() custom function. Inside it, use the TriggerBox variable to call the Delegate function OnActorBeginOverlap with AddDynamic and pass the address of your custom Callback MySpawnActor() function for it to call

   
```
- Delegates:
   - Delegates are methods that call other methods (Callback functions) when an event occurs.
   - For this to happen the Callback function address needs to be passed in the Delegate parameters and its return type needs to match that of its Delegate.
   - The Callback function bound to it will listen to the event broadcasted by the Delegate and perform its action.
```

```
- AddDynamic:
   - A Dynamic Delegate is a type of Delegate that can call Callback functions through events triggered from a Blueprint in the world.
   - We use .AddDynamic() to bind a Dynamic Delegate to a Callback function.
   - Callback functions that respond to a Dynamic Delegate broadcast must be exposed to the Blueprint through UFUNCTION().
```
     
## 2- Spawn actor
   - On Begin Play get actor location and rotation
   - Inside MySpawnActor() Use GetWorld() to call SpawnActor function of AActor type passing in the actor to be spawned, the location and rotation where to spawn this actor.
   
## 3- Spawn actor continuously with a timer

   
   - TriggerAction function
     - Declare a TriggerAction function.
     - Define the TriggerAction funciton. Inside the TriggerAction function use the TriggerBox variable to call the Multicast Delegate functions OnActorBeginOverlap and OnActorEndOverlap functions and pass your custom Callback functions that each of these will call
     - Call the TriggerAction function on BeginPlay
     
   - Remove the MySpawnActor() function from inside MyOnBeginOverlap() and include it inside MySpawnActor() function
   
   - On the Spawner header file create a FTimerHandle variable
   - On Spawner.cpp, OnBeginPlay, set timer and make it call MySpawnActor()
     - Use GetWorldTimeManager() function to call SetTimer() passing the TimerHandle variable, "this" as the object, the address for the call back function to spawn the actor -MySpawnActor()-, the time range for timer count and loop boolean.
   
   - Declare a IsOverlapping bool for when the actor is overlapping the TriggerBox / TriggerVolume
   - Set IsOverlapping as true inside MyOnBeginOverlap() and false inside MyOnEndOverlap()
   - Inside MySpawnActor, wrap the SpawnActor function with an if statement based on the IsOverlapping bool so that it only gets called if the actor is currently overlapping the TriggerBox / TriggerVolume

## Components connection flow
![image](https://user-images.githubusercontent.com/12215115/208666394-9ed64d1f-6ef8-4ccf-96f6-37a4d411998d.png)

   


