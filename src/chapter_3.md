# Chapter 3
@startuml
title ☕ Coffee Maker - Linear Process Flow
skinparam backgroundColor transparent
skinparam participantPadding 20

participant "User" as User
participant "GVL\n(Global Vars)" as GVL
participant "Water Level" as Water
participant "Heater" as Heater
participant "Brewer" as Brewer
participant "Timer" as Timer
participant "Finish" as Finish

== Start Process ==

User -> GVL : Start := TRUE

GVL -> Water : Check Water_Level
Water -> GVL : Water_Level

alt Water_Level < 100%
Water -> Water : Start Filling
Water -> GVL : FILLING := TRUE
Water -> GVL : Water_Level := 100%
Water -> GVL : FILLING := FALSE
end

Water -> Heater : Start Heating
Heater -> GVL : HEATING := TRUE
Heater -> Timer : Start 5s
Timer --> Heater : Timer.Q = TRUE
Heater -> GVL : HEATING := FALSE
Heater -> GVL : Water_Temp := 90.0
Heater -> GVL : Water_Ready := TRUE

== Brewing Process ==

GVL -> Brewer : Check Water_Ready
alt Water_Ready = TRUE
Brewer -> Brewer : Start Brewing
Brewer -> GVL : BREWING := TRUE
Brewer -> Timer : Start 7s
Timer --> Brewer : Timer.Q = TRUE
Brewer -> GVL : BREWING := FALSE
Brewer -> GVL : Coffee_Ready := TRUE
end

== End Process ==

alt coffe_ready := TRUE
GVL -> Finish : Finish :=TRUE
end

@enduml