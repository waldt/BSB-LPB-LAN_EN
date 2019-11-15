#2. General informations about BSB, LPB and PPS#  
   
BSB (Boiler System Bus), LPB (Local Process Bus) and PPS (point to point connection) are different types of bus systems (well, PPS isn't really a bus). They aren't compatible between each other, so e.g. you can't connect a BSB unit to a LPB.  
Every of the controllers mentioned in this manual which are versions of RVS and LMS (and LMU7x) have at least one BSB port to offer. LPB isn't available at each type of these controllers, but for the usage of BSB-LAN it's not necessary - just use the BSB.  
PPS isn't used anymore at younger controllers, mostly old ones like RVA, RVP or LMU5x/6x are based on this type of connection system.  
   
In the folloowing I#ll give a shot overview of the main aspects and differences of these bus/connection systems.  
   
---   
      
##2.1 BSB and LPB##  
   
BSB (Boiler System Bus) and LPB (Local Process Bus) are two different bus types, which can be broke down to two different usage purposes:  
  
1. The BSB is a 'local' bus, where e.g. parts like the operating unit or a room unit are connected to the controller of the heating system. It offers 'local' access to the controller.  
   
2. The LPB is a bus, which offers access across connected controllers (if the installation was set up right!). Using the LPB you could e.g. connect two or more heating units to realize a burner cascade or to connect the controller of you heating system with the controller of your solarthermic system.  
If you have an existing installtion like that you could connect the BSB-LPB-LAN adapter to one of the mentioned controllers and would have access to certain parameters of both controllers. in that case you would have to pay attention to use the correct bus address of the units to make sure you reach the desired controller.  

Both the BSB and LPB ports are double-pole and are labeled different sometimes by certain manufacturers. The most common names are:  
- BSB port: BSB, FB, BSB & M, CL+ & CL-  
- LPB port: LPB, DB & MB  
   
[BILDER]  

   
---  
   
###2.1.1 Addressing within the BSB###  
   
Because of the bus structure, each participant gets a specific address. The following addresses are already defined (the address of the BSB-LPB-LAN adapter is preset to address 66 in  `BSB_lan_config.h`):  
   
Bus address 0x00 = 0 = controller itself (appears as „HEIZ“ in the serial monitor)
Bus address 0x03 = 3 = Erweiterungsmodul 1 (appears as „EM1“ in the serial monitor) / Mischer-ClipIn AGU
Bus address 0x04 = 4 = Erweiterungsmodul 2 (appears as „EM2“ in the serial monitor) / Mischer-ClipIn AGU
Bus address 0x06 = 6 = room unit 1 (appears as „RGT1“ in the serial monitor)
Bus address 0x07 = 7 = room unit 2 (appears as „RGT2“ in the serial monitor)
Bus address 0x08 = 8 = OCI700 Servicetool (appears as „CNTR“ in the serial monitor)
Bus address 0x0A = 10 = operating unit (display) of the heater itself (appears as „DISP“ in the serial monitor)
Bus address 0x0B = 11 = service unit (QAA75 defined as service unit) (appears as „SRVC“ in the serial monitor)
Bus address 0x31 = 49 = OZW672 webserver
**Bus address 0x42 = 66 = BSB-LPB-LAN adapter (appears as „LAN“ in the serial monitor)**
Bus address 0x7F = 127 = broadcast message (appears as „INF“-messages in the serial monitor)  
   
---  
    
###2.1.2 Addressing within the LPB###  
   
The addressing within the LPB is different than the one within the BSB. Basically there are two 'addresses': an address of a segment and an address of a unit. Both have different meanings. Because the topic LPB is pretty complex, please search for further informations by yourself. Especially the documents about the LPB of "Siemens Building Technologies - Landis & Staefa Division" should be regarded as they are the main sources for these informations.  
   
---  
   
##2.2 PPS##  
   
Right now, the PPS will just be mentioned really short here, because it's only available at very old controllers and therefore not relevant for most of the users. As already said, PPS is not a real bus. It's more a point to point communication protocoll for the usage of connecting a room unit to a cotroller for example. So if you have an old heating system like a Broetje WGB 2N.x and you have (or can connect) a room unit like a QAA50 or QAA70, then you are using PPS.  
The adapter has to be connected the same way the room unit would have to be. In most cases the two pins of the connectors at the controller are labeled as "A6" and "M". In that case, you have to connect "A6" to "CL+"  and "M" to "CL-" of the adapter.  
  
The functionality of this 'bus' is very limited, so you probably only have a dozen of parameters available. In the Webinterface of BSB-LAN you only have access to the category "PPS-Bus" (category 30).  

Note:  
If there's already a room unit like QAA70 connected to the controller, BSB-LAN only can read values. If you want BSB-LAN to be able to set certain values, you would have to disconnect the room unit for the time you want to have the BSB-LAN adapter connected!  
Please take notice of the comments at the specific PPS definements in the file `BSB_lan_config.h` when using PPS!  
   
---  
   
#2.3 Connecting the adapter to the controller#  
  
Basically the connection of the BSB-LPB-LAN adapter to the controller is made in the same way and at the same port where a room unit will be connected. To localize the specific port at your controller, please read the manual of your heating system.  
  
In cases where only one BSB port is available at the controller (e.g. RVS21 controller within heat pumps) you can connect the adapter parallel to an already installed room unit.  

*Adapter:*  
The PCB of the adapter is already labeled with "CL+ / DB" and "CL- / MB".  
If you are building an adapter completely by your own, please look at the schematics.  
  
*BSB:*  
The connection of the adapter takes places at the already described ports and pins.  
Please connect 
"CL+" (adapter) to "CL+" (controller) and 
"CL-" (adapter) to "CL-" (controller).    
  
An additional pin "G+" which could be found sometimes at the controller is only for the backlight of a QAA75 room unit (because it offers 12V) - please make sure that you DON'T use that pin by accident!  
   
*LPB:*  
The connection of the adapter takes places at the already described ports and pins.  
Please connect  
"DB (adapter)" to "DB (controller)" and  
"MB (adapter)" to "MB (controller)".     
   
*PPS:*  
The connection of the adapter takes places at the already described ports and pins.  
In most of the cases it's "A6" and "M", therefore please connect  
"CL+" (adapter) to "A6" (controller) and  
"CL-" (adapter) to "M" (controller).  

*Notes:*  
*When connecting or disconnecting the adapter, please make sure that you switched off both units before (Arduino and controller of your heating system)!*  
*Please make sure you are using the right pins and regard the polarity!*  
   

