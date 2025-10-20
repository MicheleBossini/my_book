@startuml coffee_machine

title ☕ Automatic Coffee Machine - Functional Diagram

component coffee_machine_application {

    [water_boiler]
    [grinder]
    [pump]
    [dispenser]
    [cup_sensor]
    [bean_sensor]
    [water_sensor]
    [cabinet]
    [user_panel]

    () "bean\nlevel" - grinder
    () "water\nlevel" - water_boiler
    () "cup\npresence" - dispenser

    // Power connections
    () "main\npower" -- cabinet
    cabinet --> water_boiler
    cabinet --> grinder
    cabinet --> pump
    cabinet --> dispenser

    // Sensors and signals
    bean_sensor -l-> "bean\nlevel" : connected
    water_sensor -l-> "water\nlevel" : connected
    cup_sensor -r-> "cup\npresence" : connected

    // Control flow
    user_panel -> grinder : start
    grinder -> pump : ground coffee ready
    pump -> water_boiler : request hot water
    water_boiler -> dispenser : send hot water
    dispenser -> user_panel : coffee ready!

}

[building]
() energy -u- building
() location -u- building

cabinet -d-> energy
user_panel -u-> location

@enduml