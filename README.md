# TriggerBox

# Ingridients

## Actor to be Spawned
### Add NEW Blueprint class of Actor type for the actor to be Spawned
### Add a mesh to it

## Spawner
### Add NEW C++ AActor class
### Create a BP based on this C++ class
### Drag the BP component into the world

## Trigger Box
### Add a new C++ Trigger Box class
### Drag the trigger box C++ component into the world
### in Details:
#### Uncheck "actor hidden in game" so that the trigger box is visible
#### Click on the collision component and Add a static mesh component to it > select a mesh and a material for it so that the trigger box is vizible in the game
#### Select the collision of the mesh as "NoCollision".

## Trigger Volume
### In Details for this trigger volume 
#### brush settings > click on create static mesh > then select a material for this mesh so that the trigger volume is vizible during game

# Preparation

## Connect the trigger box / Volume and the Actor to be spawned to the Spawner

### Inside Spawner C++
#### create an instance of the actor to be spawned using TSubclassOf<type> and exposing it to Unreal with UPROPERTY
#### In Unreal select the Spawner object and select the BP of the actor to be spawner in the UPROPERTY dropdown you just created
#### Create a TriggerBox / Volume instance variable. Expose it to Unreal with UPROPERTY. 
#### In Unreal, in the Spawner component, select your trigger box component in the dropdown for this UPROPERTY field
#### In the Spawner BP component details select your trigger box component as the UPROPERTY field you just created
  
#### Inside Spawner C++, Create a TriggerAction function OnBeginPlay
##### Inside the TriggerAction function use the TriggerBox variable to call OnActorBeginOverlap and OnActorEndOverlap functions and pass your custom functions that each of these will call

### Define your custom functions for BeginOverlap and EndOverlap
  #### Create your custom functions passing OverlappedActor and OtherActor as params and expose it with UFUNCTION()
  
## Spawn actor with a timer
  ### Inside Spawner C++, create the SpawnActor function
    #### Spawn
  ### Inside Spawner C++, OnBeginPlay get Location and Rotation for this component
  ### Inside Spawner C++, OnBeginPlay set timer

