Solar power management module
====================================

Power management module for solar charging of small capacity LiPo cells controllable
over I2C bus. Primarily intended to be used on uMeshFw solar-powered nodes.


Main features
--------------------

  * operation with any battery with voltages between 3V and 5V, chemistry independent
    (charging is software controlled). Can be used with 3x NiMH cells or 1x LiPo/Li-ion cell
  * internal 2.5V LDO regulator for embedded microcontroller
  * STM32F030 microcontroller in TSOP20 package for controlling charging/discharging
    and monitoring voltages/currents
  * power output buck converter providing 2.85V for other systems
  * solar voltage and current measurement
  * battery voltage and current measurement (bidirectional)
  * power output voltage and current measurement. Power output can by disconnected
    by software
  * software operated boost converter for solar battery charging, accepts solar
    cells/panels with voltages up to 3V
  * designed to allow 0-500mA charging, 0-100mA discharging


Assembled prototype
-------------------------

[Assembled prototype 1](img/prototype1.jpg)
[Assembled prototype 2](img/prototype2.jpg)




Resources
--------------------

  * [Project on GitHub](https://github.com/iqyx/solar-pmm)


