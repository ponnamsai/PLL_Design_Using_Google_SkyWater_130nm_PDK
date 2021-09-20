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
#############################

Depending on the Up and Down values, we have the following state diagram
#############################

But there is an issue in the above circuit. When the phase difference between reference signal and feedback signal is very small, the PFD circuit does not detect as Up/Down signals are not given enough time to toggle completely. This condition is called Dead Zone and in order to avoid this, a more precise and sensitive PFD circuit should be designed.

# Charge pump (CP)
The output of the PFD is fed to Charge Pump. This coverts the digital input to a analog signal which is used by Voltage Controlled Oscillator(VCO) circuit. Charge Pump can ge designed using current steering circuits.

Below is the circuit diagram of a Charge Pump

################################

If the Up signal is 1, the current flows from Vdd to output capacitor and charges it.

If Down signal is 1, the current flows from output capacitor to Ground and discharges the Capacitor.

The output of the charge pump depends on the average time of Up being 1 and Down being 1. If the average time with Up being 1 is more, output voltage increases and the voltage across capacitor decreases in other case. 

But there can be leakage current even if both Up and Down are 0.This leakage current can impact the output signal and inturn the performance of VCO. Moreover, there can be frequency fluctuations at the output of the Charge Pump and this can be taken care with the help of a Low Pass Filter (LPF) at the output. 

########################

Charge Pump Circuit with LPF at the output

#########################

LPF not only helps in limiting high frequency fluctuations but also maintains the stability of the entire PLL control system as we are adding a Pole to the system.
 
Thumb rules that are to be followed while selecting the component values
1) Output Capacitance = Capacitance of LPF/10
2) Loop Filter Bandwidth ~= (Highest output Frequency)/10 
Loop filter Bandwidth is 1/(1+RC1) where C1 = (C*Cx)/C+Cx

# Voltage Controlled Oscillator (VCO) 
The functionality of this circuit is to generate a signal with a frequency that varies depending on the voltage of the input signal. It can be implemented using a Ring Oscillator which is a circuit with odd number of inverters with a specific delay connected in a series fashion. This makes the output signal to toggle after a certain amount of delay.

###########################################

Example Circuit of a VCO

  Time Period of the signal generated = 2*(Delay of each inverter)*(number of inverters)
  
  Frequency depends on the time period which inturn depends on the delay and which inturn depends on the current supplied. If a large current is supplied at the output, the output gets charged faster. Current starving mechanism can be used to obtain variable frequencies. The important design criteria that should be noted while designing this circuit is that the frequencies that VCO generates must be in the same range of frequencies that the PLL supports.
  
  # Frequency Divider Circuit (FD):
  A frequency divider circuit generates an output signal with a frequency reduced by a certain factor compared to the input signal.
  
  For example, if a signal with frequency F is given to a frequency divided by 2 circuit, it generates an output signal with frequency F/2.
