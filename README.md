# PLL_Design_Using_Google_SkyWater_130nm_PDK
PLL Design using open source ngspice and magic tools with 130nm open source PDK 
# Introduction to PLL (Phase Locked Loop)
PLL is widely used to recover the clock signal from a receiver signal in some of the wireless applications. It can also be used to generate a signal with a frequency that is multiplied by a fixed factor compared to the frequency of a reference signal.
# Necessity to use PLL
A clock signal can be generated with the help of a Quartz crystal or using a VCO(Voltage Controlled Oscillator). But they do have their own limitations. Quartz Crystal can give a signal with pure spectrum( With single frequency) but we do not have the flexibility of changing the frequency of the signal generated. On the other hand, VCO can generate signals with flexible frequencies but there is phase noise. Therefore, the purpose of the PLL is to leverage both the features of VCO and Quartz crystal and get a flexible signals with less noise.
# Modules/Blocks inside the PLL
![PLL_generic_inline_optional_N svg](https://user-images.githubusercontent.com/18748519/133935585-1615114a-3647-458b-b194-b158aa9a513f.png)
(Source: By Krishnavedala - Own work, CC0, https://commons.wikimedia.org/w/index.php?curid=64092961)

Below is the description of each circuit present inside the PLL
# Phase Frequency Detector (PFD)
