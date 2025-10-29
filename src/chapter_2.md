@startuml smart_coffee_maker

title ☕ Smart Coffee Maker - Process and Safety Overview

package "GVL (Global Variable List)" as GVL {
  [Start : BOOL]
  [Water_Level : INT]
  [Water_Temp : REAL]
  [Water_Ready : BOOL]
  [Coffee_Ready : BOOL]
  [FILLING : BOOL]
  [HEATING : BOOL]
  [BREWING : BOOL]
  [INTENSITY : INT]
  [TIME : TIME]
}

rectangle "TASK 1 - Water Contact" as Task1 {
  [Read_Temperature]
  [Read_Level]
  [Filling_Process]
  [Heating_Process]
  [Water_Ready_Flag]
}

rectangle "TASK 2 - Coffee Process" as Task2 {
  [Wait_Water_Ready]
  [Start_Brewing]
  [Update_Coffee_Ready]
}

' --- Connections for TASK 1 ---
[Read_Temperature] --> [Read_Level] : check water level
[Read_Level] --> [Filling_Process] : if <100%
[Filling_Process] --> [Heating_Process] : when level=100%
[Heating_Process] --> [Water_Ready_Flag] : when hot enough
[Water_Ready_Flag] --> GVL : set Water_Ready=TRUE

' --- Connections for TASK 2 ---
[Wait_Water_Ready] --> [Start_Brewing] : if Water_Ready=TRUE
[Start_Brewing] --> [Update_Coffee_Ready] : brewing complete
[Update_Coffee_Ready] --> GVL : set Coffee_Ready=TRUE

' --- Function block for estimated preparation time ---
rectangle "FUNCTION: Coffee_Preparation_Time" as FCT {
  () "Inputs:\n- level : INT\n- strength : INT" 
  () "Output:\n- estimated time (TIME)"
}

GVL --> FCT
FCT --> Task2 : time info

' --- Safety / status signals ---
GVL -[hidden]-> Task1
GVL -[hidden]-> Task2

@enduml