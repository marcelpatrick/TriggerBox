# TriggerBox

# Ingridients
## 1. Actor to be Spawned
   - Add NEW Blueprint class of Actor type for the actor to be Spawned
   - Add a mesh to it

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
   - Inside Spawner C++
     - create an instance of the actor to be spawned using TSubclassOf<type> and exposing it to Unreal with UPROPERTY
     - In Unreal open the Spawner BP and select the BP of the actor to be spawned in the UPROPERTY dropdown you just exposed
     - Create a TriggerBox / Volume instance variable. Expose it to Unreal with UPROPERTY. 
     - In the Spawner BP component details select your trigger box component as the UPROPERTY field you just created
     - OR, In Unreal, select the Spawner component and go to the Details tab, select your trigger box / trigger volume component in the dropdown 
     - Create a TriggerAction function. call it on OnBeginPlay
       - Inside the TriggerAction function use the TriggerBox variable to call OnActorBeginOverlap and OnActorEndOverlap functions and pass your custom functions that each of these will call

   - Define your custom functions for BeginOverlap and EndOverlap
     - Create your custom functions passing OverlappedActor and OtherActor as params and expose it with UFUNCTION()
  
## 2- Spawn actor with a timer
   - Inside Spawner C++,
     - create the SpawnActor function
     - OnBeginPlay get Location and Rotation for this Spawner
     - On the header file create a FTimerHandle variable
     - OnBeginPlay set timer
       - Use GetWorldTimeManager() function to call SetTimer() passing the TimerHandle variable, the object, the address for the call back function to the actor, the time range and loop boolean. 

