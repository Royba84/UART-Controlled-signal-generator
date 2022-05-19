# UART-Controlled-signal-generator

TL:DR - Real Time Digital signal processing: UART Controlled Arduino UNO signal generator

April 13 2021, Roy Ben Avraham;Mk;Ge

In this project, the goal is to write a skech for Arduino Uno which will enable to produce (by using 4 bits R2R DAC) the following Analog signals:

# 1) Sin wave

![image](https://user-images.githubusercontent.com/105777016/169202326-7da994aa-9281-4a92-b345-391f80e8941b.png)

# 2) Sawtooth wave

![image](https://user-images.githubusercontent.com/105777016/169202365-f2e582ff-1819-45b8-90fc-3dd4a032c811.png)

# 3) Dumped cosine wave

![image](https://user-images.githubusercontent.com/105777016/169202423-67ee739f-8621-4cd4-973e-9ece926fec8c.png)

# Show a menu for the user (by pressing '?') - User interface

![image](https://user-images.githubusercontent.com/105777016/169202465-e7c25f1d-9eae-45ce-9ecc-3ae7dce64e84.png)

In the user interface the user will be able to change certain values: (and choose the signals 1-3)

A- Amplitude in Volts in the range [0-5] [V]
F- Signal frequency, F=1/T (T- period in seconds, created signal must contain 64 counts per period)

B- Amplitude of the Pseudo-random flat noise (in the range [0-5] [V])
C- Number of positive bumps per period T with amplitude 1V, C is in the range [0-5][V]

In this project I will use TinkerCAD simulation to demonstrate some of the functions of this project.

System layout:

![image](https://user-images.githubusercontent.com/105777016/169202499-e71eec83-a2bd-4133-8af6-39c3f7a321f7.png)



