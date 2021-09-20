# PLL_Design_Using_Google_SkyWater_130nm_PDK
PLL Design using open source ngspice and magic tools with 130nm open source PDK 
# Introduction to PLL (Phase Locked Loop)
PLL is widely used to recover the clock signal from a receiver signal in some of the wireless applications. It can also be used to generate a signal with a frequency that is multiplied by a fixed factor compared to the frequency of a reference signal.
# Uses of PLL
A clock signal can be generated with the help of a Quartz crystal or using a VCO(Voltage Controlled Oscillator). But they do have their own limitations. Quartz Crystal can give a signal with pure spectrum( With single frequency) but we do not have the flexibility of changing the frequency of the signal generated. On the other hand, VCO can generate signals with flexible frequencies but there is phase noise. Therefore, the purpose of the PLL is to leverage both the features of VCO and Quartz crystal and get a flexible signals with less noise.
# Modules/Blocks inside the PLL
![PLL_generic_inline_optional_N svg](https://user-images.githubusercontent.com/18748519/133935585-1615114a-3647-458b-b194-b158aa9a513f.png)
(Source: By Krishnavedala - Own work, CC0, https://commons.wikimedia.org/w/index.php?curid=64092961)

Below is the description of each circuit present inside the PLL
# Phase Frequency Detector (PFD)
The basic functionality of this circuit is to compare the input reference and feedback signals and generate Up and Down signals depending on whether feedback signal leads or lags compared to reference signal.

Below is the circuit of PFD 
![PFD](https://user-images.githubusercontent.com/18748519/133994506-4659a2a5-e34b-4369-a4bc-95c055137270.jpg)

Depending on the Up and Down values, we have the following state diagram
![FSM](https://user-images.githubusercontent.com/18748519/133994526-bf05c670-b001-4ca2-a01d-859904bb4362.jpg)


But there is an issue in the above circuit. When the phase difference between reference signal and feedback signal is very small, the PFD circuit does not detect as Up/Down signals are not given enough time to toggle completely. This condition is called Dead Zone and in order to avoid this, a more precise and sensitive PFD circuit should be designed.

# Charge pump (CP)
The output of the PFD is fed to Charge Pump. This coverts the digital input to a analog signal which is used by Voltage Controlled Oscillator(VCO) circuit. Charge Pump can ge designed using current steering circuits.

Below is the circuit diagram of a Charge Pump

![CP](https://user-images.githubusercontent.com/18748519/133994557-593f08d7-0149-4a95-b740-4a4076126731.jpg)


If the Up signal is 1, the current flows from Vdd to output capacitor and charges it.

If Down signal is 1, the current flows from output capacitor to Ground and discharges the Capacitor.

The output of the charge pump depends on the average time of Up being 1 and Down being 1. If the average time with Up being 1 is more, output voltage increases and the voltage across capacitor decreases in other case. 

But there can be leakage current even if both Up and Down are 0.This leakage current can impact the output signal and inturn the performance of VCO. Moreover, there can be frequency fluctuations at the output of the Charge Pump and this can be taken care with the help of a Low Pass Filter (LPF) at the output. 

Charge Pump Circuit with LPF at the output

![CP_with_lpf](https://user-images.githubusercontent.com/18748519/133994576-2b25a8c3-6c0a-4103-8acb-edf82cc6e2f4.jpg)


LPF not only helps in limiting high frequency fluctuations but also maintains the stability of the entire PLL control system as we are adding a Pole to the system.
 
Thumb rules that are to be followed while selecting the component values
1) Output Capacitance = Capacitance of LPF/10
2) Loop Filter Bandwidth ~= (Highest output Frequency)/10 
Loop filter Bandwidth is 1/(1+RC1) where C1 = (C*Cx)/C+Cx

# Voltage Controlled Oscillator (VCO) 
The functionality of this circuit is to generate a signal with a frequency that varies depending on the voltage of the input signal. It can be implemented using a Ring Oscillator which is a circuit with odd number of inverters with a specific delay connected in a series fashion. This makes the output signal to toggle after a certain amount of delay.

![VCO](https://user-images.githubusercontent.com/18748519/133994595-59331aba-5c3f-4e9f-8164-8c8a8dcb0970.jpg)

Example Circuit of a VCO

  Time Period of the signal generated = 2*(Delay of each inverter)*(number of inverters)
  
  Frequency depends on the time period which inturn depends on the delay and which inturn depends on the current supplied. If a large current is supplied at the output, the output gets charged faster. Current starving mechanism can be used to obtain variable frequencies. The important design criteria that should be noted while designing this circuit is that the frequencies that VCO generates must be in the same range of frequencies that the PLL supports.
  
  # Frequency Divider Circuit (FD):
  A frequency divider circuit generates an output signal with a frequency reduced by a certain factor compared to the input signal.
  
  For example, if a signal with frequency F is given to a frequency divided by 2 circuit, it generates an output signal with frequency F/2.
  
  ![FD_BY_2](https://user-images.githubusercontent.com/18748519/133994616-23867a68-a1fc-47dd-9d2f-161e5923c1f5.jpg)

  Frequency divider by 2 circuit
  
  The cascaded Frequency divider by 2 circuit can be used to get frequency divided by a certain factor (multiple of 2) circuit. This circuit is optional in case of PLL.
  # PLL Jargon:
  1) Lock range: It is the range of frequencies for which PLL will maintain its locked state given that it is already in locked state.
  
  2) Capture Range: It is the range of frequencies for which PLL comes to locked state from unlocked state. Generally this rage is less than Lock range.
  
  3) Settling Time: It is the time taken by the PLL to attain a lock from the initial unlocked state.
# PLL Circuit Design:
8x multiplier PLL specifications used for this workshop
1) TT - Corner (Typical Typical)
2) VDD (Digital Supply) - 1.8V
3) Temperature - Room Temeperature (27C)
4) Both VCO and PLL Modes
5) Reference Clock Frequency - 5MHz to 12.5MHz
6) Output Clock Frequency    - 40MHz to 100MHz
7) Jitter (RMS) <~20ns
8) Duty Cycle -50%

Tools Used: ngspice and magic

# Individual Circuits used for this PLL

Below are the circuits used for designing PLL in this workshop 

![Screenshot (7)](https://user-images.githubusercontent.com/18748519/133987960-36185f2b-365a-4e19-94b8-bde71402d701.png)

![Screenshot (5)](https://user-images.githubusercontent.com/18748519/133987785-d72c7014-15f3-43d6-bd83-2301bae3ac97.png)

![Screenshot (8)](https://user-images.githubusercontent.com/18748519/133988039-52bcd016-e487-44e9-be19-2012c310840b.png)

![Screenshot (6)](https://user-images.githubusercontent.com/18748519/133987897-1e0e03ee-7612-4888-bee8-4d6461095834.png)


# Design Flow Followed for this project

![Design Flow](https://user-images.githubusercontent.com/18748519/133962158-9ecdf238-7bb6-49da-8f91-5da65ac9ac30.jpg)

# Circuits Design and Pre-Layout Simulations:

1) Phase Frequency Detector(PFD): 
  
  Below are the simulation results for PFD circuit
  
  ![Screenshot (15)](https://user-images.githubusercontent.com/18748519/133963093-3d0146a7-6a8e-4180-8ffd-d09d3fb94286.png)
  
  
  
  When we zoom in waveform, it looks as below
  
  ![Screenshot (43)](https://user-images.githubusercontent.com/18748519/133963391-3f4faded-8f07-444a-b7fc-cd8e9979be58.png)
  
  ![Screenshot (44)](https://user-images.githubusercontent.com/18748519/133963578-7abea961-21d9-4547-95ee-e25ec7c4fc74.png)
  
  We can observe that Down signal toggled detecting the phase difference between input reference signal and feedback signal.
  
2) Charge Pump (CP): 

   Below are the simulation results for CP circuit
   
   ![Screenshot (46)](https://user-images.githubusercontent.com/18748519/133963979-cb444841-acab-4200-b974-eda404737de5.png)
   
   
   When we zoom in waveform, it looks as below
   
   ![Screenshot (13)](https://user-images.githubusercontent.com/18748519/133964085-b20c8819-a454-4e0a-9cf8-e2ba83c89795.png)
   
   We can observe that output voltage across the capacitor increased exponentially with small ups and downs which is because of Up and Down signals. As Average time of Up being 1 is more than Down being 1, voltage increased.
   
3) Voltage Controlled Oscillator (VCO) :

    Below are the simulation results for VCO circuit
    
    ![Screenshot (47)](https://user-images.githubusercontent.com/18748519/133964532-e1cc89b5-993b-4fc6-8913-c92f9c4eb19c.png)
    
    When we zoom in waveform, it looks as below
    
    ![Screenshot (21)](https://user-images.githubusercontent.com/18748519/133964590-72957431-c054-46f7-9e6c-45e8b903334d.png)
    
    We can observe that oscillations are with full swing and this is because of an extra inverter that is added at the output.
    
 
4) Frequency Divider Circuit (FD) :

   Below are the simulation results for FD circuit
    
   ![Screenshot (18)](https://user-images.githubusercontent.com/18748519/133964873-36a0ecf1-0bdf-402a-848c-dc8e1c33e0a2.png)
   
   When we zoom in waveform, it looks as below
   
   ![Screenshot (48)](https://user-images.githubusercontent.com/18748519/133965039-9b013889-1faf-4a33-adcf-fa4d85499d1b.png)
   
   We can observe that the output signal's frequency is exactly of the frequency of the input signal
   
 
 As individual circuits are working as expected, we create the entire PLL circuit joining all these components using subcircuit block.

5) Phase Locked Loop (PLL) :

   Below are the simulation results for PLL circuit
   
   ![Screenshot (46)](https://user-images.githubusercontent.com/18748519/133965595-05d00eda-aa17-40c4-9f76-5acb38ebe7df.png)
   
   
   When we zoom in waveform, it looks as below
   
   ![Screenshot (47)](https://user-images.githubusercontent.com/18748519/133965675-b7577b1e-e415-4c57-aac9-8ad52cd9a847.png)

   ![Screenshot (45)](https://user-images.githubusercontent.com/18748519/133965727-e7aa06f4-1228-4f68-affb-9de490d89b05.png)

   We can observe that reference signal and feedback signal are mimicing each other. In an ideal case, both signals should overlap without any phase noise. We can also observe signals with frequency X8, X4, X2 which can also be used as frequency multiplier.
   
# Layout Development:
  
  Layouts for the above working circuits are developed individually and then connected to form entire PLL circuit.
  
  1) PFD Circuit: 
     
     ![Screenshot (49)](https://user-images.githubusercontent.com/18748519/133968603-29768479-c428-4a58-815e-f82779cffcb7.png)
     
     
    Area of the layout can be find using box command in the terminal
     
   ![Screenshot (23)](https://user-images.githubusercontent.com/18748519/133968975-eeef78d1-e395-4e70-b01f-b8f15ee079fe.png)
     
     Area is 49.09 µm2
     
  2) CP Circuit:
     
     ![Screenshot (27)](https://user-images.githubusercontent.com/18748519/133969434-f496c22d-bbf8-4d4c-8a98-c7fee2647e3b.png)
    
    Area is 132.29 µm2
    
  3) VCO Cirucit: 
     
     ![Screenshot (28)](https://user-images.githubusercontent.com/18748519/133969707-3afb764a-d68c-440d-9b80-1a0156bbd6ee.png)
     
     Area is 57.73 µm2
  
  4) FD Circuit: 
     
     ![Screenshot (32)](https://user-images.githubusercontent.com/18748519/133969862-9a6c7688-d6b8-4954-b4a9-d0365e449ca7.png)
     
     Area is 29.92 µm2
     
  5) PLL Circuit: 
  
      By placing all instances and connecting all the blocks, below PLL is designed.
      
      ![Screenshot (33)](https://user-images.githubusercontent.com/18748519/133976241-ae430eaf-23e9-45b4-9d33-6a36f239f43a.png)
      
      ![Screenshot (36)](https://user-images.githubusercontent.com/18748519/133976342-66ed829b-ca61-436c-b6d1-87bb94d3c7e4.png)

      Area is 496.03 µm2

# Post Layout Simulations:

Paracitics are extracted from the layout of each of the files (.mag). The .ext files are then converted into .spice file which then used for post_layout simulations. Post_layout simulations are carried with a 10ns delay.

1) PFD: 
   
   Below is the Post-Layout simulation of PFD circuit
   
   ![Screenshot (38)](https://user-images.githubusercontent.com/18748519/133980687-6a37a8b4-4b0b-48d3-9b13-1b675e88f5f6.png)
   
   When we zoom in waveform, it looks as below
   
   ![Screenshot (39)](https://user-images.githubusercontent.com/18748519/133980871-988a2dae-488e-4484-8783-998d44d535e9.png)
   
  Simulation Illustrating Dead-Zone: 
  
   When delay is changed from 10ns to 0.25ns as done in below code, we observe this condition. 
   
   ![Screenshot (47)](https://user-images.githubusercontent.com/18748519/133988990-4652de61-4ae3-4d55-9576-2b0c1c8bcb5c.png)
   
   Below is the Post-Layout simulation of PFD circuit with Dead-Zone situation:
   
   ![Screenshot (48)](https://user-images.githubusercontent.com/18748519/133989115-99790356-a8b0-40b2-afc5-8f6bf750dd61.png)
   
   When we zoom in waveform, it looks as below
    
   ![Screenshot (49)](https://user-images.githubusercontent.com/18748519/133989175-3b1c05cc-ad96-4998-a315-2599a8be2957.png)
   
   
   We can observe that as there is very less phase difference between reference and feedback signal, Up signal is unable to toggle completely.

 
   
   
   
2) CP:
   
   Below is the Post-Layout simulation of CP circuit
  
   ![Screenshot (40)](https://user-images.githubusercontent.com/18748519/133980981-11b88354-e262-4131-8895-8addff2c45aa.png)
   

3) VCO: 
   
   Below is the Post-Layout simulation of VCO circuit
   
   ![Screenshot (41)](https://user-images.githubusercontent.com/18748519/133981074-ef6e75d0-e296-4f02-89b7-9c6701d787fa.png)
   
  
 PLL Post_Layout Simulation: 
 
  Below is the Post-Layout simulation of PLL circuit
 
  ![Screenshot (51)](https://user-images.githubusercontent.com/18748519/133981455-41dc32c5-9bad-438b-a5b2-24bc03d77a8a.png)
    
  When we zoom in waveform, it looks as below
    
  ![Screenshot (52)](https://user-images.githubusercontent.com/18748519/133981548-02c24334-c2fa-4801-b0c4-456ab59abaa7.png)
    
  ![Screenshot (53)](https://user-images.githubusercontent.com/18748519/133981591-2209edbb-b4c0-4ac1-a229-c14e847f1be9.png)


# Tapeout:
Before tapeout, there can be some additional requirements that are to be catered so that the PLL or any other circuit can be accessed/used without any drawbacks. Some of them are :
       
       1) I/O Pads --> To connect to external world
       
       2) Peripherals
       
       3) Memory
       
       4) Some Mechanisms to test (Example: BIST circuits)
       
       
Making sure that the design meets the above expectations is not only difficult but tiresome. These expectations can be met using Caravel SoC.
       
Below is the diagram of Caravel SoC
       
   ![CaravelSoCTemplate](https://user-images.githubusercontent.com/18748519/133986523-50c620c4-6d3f-4df6-b2d5-0b3c1b835c2a.jpg)
   
   We need to place our design in the Mega project area as shown in the figure and make nencessary connections. The entire SoC along with our design is verified and then fabricated.
   
# References:
1) https://github.com/lakshmi-sathi/avsdpll_1v8
2) https://freevideolectures.com/course/3101/rf-integrated-circuits/31
3) http://opencircuitdesign.com/
4) https://github.com/RTimothyEdwards/open_pdks






    












