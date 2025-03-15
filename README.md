# Cooling_Trackers_Rack
Cooling of Trackers in the Rack

Tento FLOW není samostatně funkční, chybí mu globální funkce a nastavení, které jsou součástí: https://github.com/hacesoft/Linea/tree/main?tab=readme-ov-file#linea

![image](https://github.com/user-attachments/assets/0e920405-32e4-4dee-8a06-68bc3f3edf43)

**Popis funkce:**

Funkce je implementována v prostředí Node-RED a slouží k ovládání ventilátorů připojených přes Shelly Plug. Na základě údajů z MPPT trackeru Victron se vyčítá aktuální výkon, který se sečte. Podle nastavených triggerů se pak ovládají Shelly Plug zásuvky, které spínají ventilační jednotky. Jedna ventilační jednotka je namířena na měniče Victron a druhá na MPPT tracker.

