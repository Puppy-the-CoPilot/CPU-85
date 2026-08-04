# CPU-85
8085 CPU based 1970s to 1980s era S-100 IEEE696 board outline card
last update: 28 July 2026

Purpose:
     * update MITS/Altair CPU-A board for modern EMC PCB layout principles
              ** have more Vcc and Ground to avoid switching niose risk
                    *** this helps prevent self induced switching noise (Conducted Immunity)
                    *** helps prevent Radiated Emissions in AM Band
              ** place decopupling capacitors next their respective ICs
     * improve CPU speed above 1975 limitations of the early 8080A as implemented by MITS/Altair/IMSAI
              ** HMOS 8085A upto 6MHz 
              ** CMOS 80C85A upto 5MHz, lower power
              ** CMOS CA80C85B upto 8MHz, B indicates a silicon die revision
     * add low voltage or 5Vcc < 4.7V forced RESET to help power up reliability
     * support IMSAI 8080A Front Panel and MITS/Altair 8800 Front Panel
              ** have 2 ribbon cable connectors
              ** S-100 jumpers and circuits for supporting signal varations
     * add Power On Jump to a user defined address (1K increments)
     * minimalist hardware, no IO, no RAM, no ROM, etc... just CPU
     * allow easy changing of CPU MHz speed 
              ** use DIP oscillator as flexible source
              ** have PCB holes to allow HC case quartz crystal
              ** allow use of 8 or 14 pin DIP oscillators
     * use jumper blocks to support IMSAI and MIS/Altair variations (they are not 100% the same)
     * maybe design a spin off varation to use the NSC800 (a Z80A hybrid with 8085 like pin out)
     * avoid using 8212 latch and 8216 bidirectional drivers (availability issues + old plastic encapsulation manufacturing)
  
Schematic: Completed 10 July 2026
           filename=CPU-85_schematic_21July2026.pdf
           (a living document, I will add more notes and validation results as those become available)

Software:  what ever the user had to run IMSAI 8080 or MITS/Altair 8800
           (this board lacks dedicated I/O so no special I/O drivers are needed)

PLD or PLA:  Yes there is 1, U23 is a GAL16V8 used as a CPU State Decoder
             filename=CPU-85_U23.jed

PCB Layout:  Gerbers Completed 17 July 2026
             filename=CPU-85_r104-F_Silkscreen.gbr

Bill Of Material:  Completed 18 July 2026
                   filename=CPU-85_rev1dot04_BOM_18July2026.xls

PCB Status: first blank check samples are being made in July 2026

First Validation Sample: yet to be made in August 2026

Planned Validation:
     * test in IMSAI non Front Panel IMSAI PCS 80/30 
     * test in IMSAI 8080 system with IMSAI Front Panel 8080A
     * test in MITS/Altair 8800 system with MITS/Altair Front Panel

Errata:  None Known

contact email:  greglori_mi@wowway.com



Answers to Questions people asked:

Q:  8080A S-100 Boards had unique circuits for using their Front Panels
A:  I compared the schematics of IMSAI 8080A MPU-A, IMSAI 8085A MPU-B, and MITS/ALTAIR 8080 CPU-A.
    On IMSAI MPU-B, they just ran out of time and physical PCB space for the needed circuits... or just gave up on trying to achieve Front Panel compatibility.
    This board has different circuits to choose from in places where IMSAI, Intel, IEEE696, and MITS/Altair had some different circuits or logic.
    Most logic circuits were the same between IMSAI and MITS/Altair boards, but a few were differences... thus jumper blocks are used for circuit selections.
    So yes, there is some extra circuitry on this panel.
    See Page 12 of the schematic.  


Q: 8080A had a quirk or mirroring A0-A7 on A8-A15 during IO operations, does this panel do that?
A: Yes, the 8085A does this itself already.  Intel copied this characteristic from the 8080A to the 8085A.
   The OKI80C85A and CA80C85B specifications do not mention the IO address mirroring (they do state 100% pin and signal compatible).
   When A8-A15 are TriState, there are 5V pull ups to make FFH as a default for the Front Panel.


Q: One person wrote this panel uses 74HCTxx logic gates, they had mixed results using 74HCT family gates...
A: Lately I've been using 74HCT on my designs for lower power consumption.   
   For me, 74LS and 74HCT have been interchangeable for CPU speed <5MHz, I've had no bad experiences.
   Only difference I found was on 74HCT07 is clamped to Vcc or 5V, where as 74LS07 is true Open Collector with Vce=30V (no connection to Vcc)... 
   74HC07/74HCT07 and 7407/74LS07 are not interchangeable when pull up voltage is >5V.


Q. a person made a program to use 8085's RIM and SIM instructions...can SID and SOD be used?
A. Yes, SID and SOD are brought out from CPU to solder holes and pads. 


Q. Hard Gold on S-100 Edge Card fingers?
A. Yes, although Gold is now very expensive, 20u" (Industrial grade) is used for durabiliity vs. 30u" (Medical/Military grade)


Q. Hot Air Solder Pre Tin solder pads
A. No.  To prevent any surface oxidation before parts get soldered and to allow for a long shelft life, a thin 3u" gold coating has been applied


Q. NCS800 CPU version?
A. Probably not to be designed or made.  There is no interest, there are already Z80 CPU panels with IMSAI Front Panel ribbon connectors
   that go faster than the NSC800.  NCS800 offers no speed or any other improvement over the existing Z80 PCBs.  
   The NSC800 was not developed further as a product, only a 4MHz version appears to exist.


Q. M1 OpCode Fetch and other CPU State info on the S-100 Data Out Bus during PSYNC was missing...
A. Added as a result of an early July Vintage Computer Group schematic review (hence the July 10 reivsion);
   the CPU States are now latched and placed on the S-100 DO0-DO7 signals during the PSYNC Cycle.
   On the 8080A, the PSYNC cycle lasted 4 XTAL clock cycles, or 2T machine states; on this panel
   the user can select PSYNC to be 1 of 3 sources: sM1, ALE as used by IMSAI MPU-B (1T 8085A machine cycle), or a 
   lengthened 8080A like signal (2T cycles long).


Q. what is done with extended address A16 to A23?
A. just like 1975 and 1970 era CPU panels, the pre IEEE-696 S-100 pins for A16-A23 are left floating, no pull ups or pull downs on this panel;
   if the receiving S-100 system has bus card termination, they will be pulled up to about 2.3V to 2.5V or logic 1 by the signal line terminators;
   this panel can only address 64K bytes like the original 1970s era CPU panels


