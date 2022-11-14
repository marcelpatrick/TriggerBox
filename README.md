# TriggerBox

# Ingridients

## Add a new Blueprint class for the actor to be Spawned

### Add a mesh to it

## Spawner Actor

### Add a C++ AActor class

### Create a BP based on this C++ class

### Drag the BP component into the world

## Trigger Box

### Add a new C++ Trigger Box class

### Drag the trigger box C++ component into the world

#### Uncheck "actor hidden in game" so that the trigger box is visible
#### Add a static mesh component to the trigger box component - select a mesh and a material for it
#### Select the collision of the mesh as "NoCollision".

# Preparation

## Connect the trigger box and the Actor to be spawned to the Spawner

### Inside Spawner C++, create an instance of the actor to be spawned using TSubclassOf<type> and exposing it to Unreal with UPROPERTY
### In Unreal select the Spawner object and select the BP of the actor to be spawner in the UPROPERTY dropdown you just created

### Inside Spawner C++, Create a TriggerBox instance variable. Expose it to Unreal with UPROPERTY. In Unreal, in the Spawner component, select your trigger box component in the dropdown for this UPROPERTY field
#### In the Spawner BP component details select your trigger box component as the UPROPERTY field you just created
#### Inside Spawner C++, Create a TriggerAction function 
##### Inside the TriggerAction function use the TriggerBox variable to call OnActorBeginOverlap and OnActorEndOverlap functions and pass your custom functions that each of these will call

## Define your custom functions for BeginOverlap and EndOverlap

