```python
from aide_design.play import*
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import matplotlib
from scipy import stats
import Environmental_Processes_Analysis as EPA
import importlib
importlib.reload(EPA)
import scipy
from scipy import special
from scipy.optimize import curve_fit
import collections
import os
import tkinter as tk
from tkinter import filedialog
root = tk.Tk()
root.withdraw()
```
# Lab 5 Report
## Helena Harris, Andrew Kang, Simon Fines
## Reactors Laboratory

## Introduction and Objective

Reactors physically confines chemical, biological, and physical processes in nature with a real or imaginary boundary. Reactors often mimic the properties that are found in lakes, treatment plant tanks, and segments of rivers, which usually experience a continuous influent and effluent. Defining the mixing level and residence time is essential because they both affect the process and reactions of the fluid and its pollutants as it travels through the reactor.

The objective of the lab is to determine which characteristics of a reactor would be beneficial in the design of a chlorine contact tank. The effectiveness of chlorine is dependent on the amount of contact time between the chlorine and  contaminants in the water. For that reason it is beneficial to have a reactor that maximizes the amount of contact time with the chlorine, but also provides a consistent amount of contact time for all the water passing through it.

## Procedure
The very first step of this experiment is calibrating the spectrophometric probe. Follow the instructions in the "Calibration of photometer section" of the "Reactor Characteristic Lab Manual." With a 1 L bottle replacing the reactor, setup the plumbing copying the image in Figure 1-8. Continue through the calibration, saving the calibration settings for the upcoming experiment. Make sure that the spectrophometric probe is oriented so the tubes are vertical, and that the water would be flowing upwards. If the calibration is off, tap the probe to knock out any potential air bubbles.

Before running the experiment, we first needed to define and calculate our parameters. Since the range of the spectrophometric probe is between 0.01 mg/L and 30 mg/L, we need to add an appropriate amount of dye so that the maximum concentration of dye in the reactor is less than 30 mg/L. We decided to aim for 25 mg/L. Given the reactor volume of 2 liters, and a bottle of 100 g/L red dye, we used conservation of mass to calculate the volume of tracer dye.
# Equations
Set up the plumbing of the reactor to copy the three images in the "Reactor Design" section of the manual. Before filling the reactor with water, we agreed on and implemented 3 different combinations of baffle, using 3 different baffle designs (See figure # for baffle design). Tape the sides and bottom of the baffle down to the reactor before starting each experiment. This is to avoid any water leaking through any crevices in between the baffle and reactor wall.  Fill the reactor with deionized water until the water starts flowing out of the outflow tube (~2.25 L). Start pumping tap water from a 20 L Jerrican, and the start recording data from the spectrophometer. After ensuring that the water is properly flowing through the plumbing, the sensor is at a stable votage of around 3.5 V, and there are no air bubbles in the probe, add the appropriate amount of red dye to the reactor section that is the farthest from the probe. Make sure to mark down the moment the dye was added. Let the reactor run well after the sensor reads a peak concentration, and stop collecting data when the voltage reading goes back to around 3.5 V. Repeat the experiment for other reactor baffle combinations.

Further customizations can be done to this experiment. For our next three experiments, we decided to use a single coiled tube to imitate a plug flow reactor design. Since the reactor design has changed drastically, we had to calculate new parameters. By weighing the tube coil empty and then completely filled with water and taking the difference of the two masses, the volume of the tube coil reactor can be determined assuming the density of water to be 1 g/mL. At 0.336 L, the tube has a much smaller volume than the reactor. This would change the amount of dye needed in order to still be within the spectrophometer range. Using conservation of mass, the amount of 100 g/L dye needed for a 25 mg/L concentration in 0.336 L of water is 0.08 mL. The plumbing setup involved the influent tube going through the pump, which is then connected to a syringe connector leading into the coiled tubing reactor with the spectrophometrer at the other end. The outflow tubing is then connected to the opposite end of the probe. For the tubing experiments, we decided the variable we would change would be the amount of dye injected into the reactor. Since 0.08 mL of the dye resulted in around 25 mg/L, we decided to run the experiment at 0.01 mL, 0.05 mL, and 0.1 mL of dye. For each experiment, extract the necessary amount of dye using the syringe. Have the pump running and start logging the data. Insert the syringe into the connector, inject the dye, and immediately pull the syringe out. Make sure this process is done as fast as possible to imitate a single pulse of red dye. Log the data until the red color has died out of the reactor. Repeat this process for the other amounts of red dye.

## Results and Discussion

Lab 5 Analysis

1.	Use multivariable nonlinear regression to obtain the best fit between the experimental data and the two models by minimizing the sum of the squared errors. Use EPA.Solver_AD_Pe and EPA.Solver_CMFR_N. These functions will minimize the error by varying the values of average residence time, (mass of tracer/reactor volume), and either the number of CMFR in series or the Peclet number.

```python
# Baffle experiments
# Baffle Design 1
Q = 380*(u.ml/u.min)
V = 2*u.L
theta = (V/Q)
Tracer_mass = 50*u.mg
theta_guess = theta
time_data1 = EPA.ftime('Lab 5 Plug Flow Exp1.txt',7,141)
photoarray1 = EPA.Column_of_data('Lab 5 Plug Flow Exp1.txt',7,141,1,'mg/L')
C_bar_guess1 = np.max(photoarray1)
C_bar_guess1
CMFR_N_1 = EPA.Solver_CMFR_N(time_data1, photoarray1, theta_guess, C_bar_guess1)
CMFR_N_1
theta_guess_AD = theta_guess/3
C_bar_guess1_AD = C_bar_guess1/3
AD_Pe_1 = EPA.Solver_AD_Pe(time_data1, photoarray1, theta_guess_AD, C_bar_guess1_AD)
AD_Pe_1
time_data2 = EPA.ftime('/Users/helenaharris/Documents/GitHub/hjh57cee4530/Lab 5 Plug Flow Exp2.txt',4,-1)
photoarray2 = EPA.Column_of_data('/Users/helenaharris/Documents/GitHub/hjh57cee4530/Lab 5 Plug Flow Exp2.txt',4,-1,1,'mg/L')
C_bar_guess2 = np.max(photoarray2)
C_bar_guess2
CMFR_N_2 = EPA.Solver_CMFR_N(time_data2, photoarray2, theta_guess, C_bar_guess2)
CMFR_N_2
AD_Pe_2 = EPA.Solver_AD_Pe(time_data2, photoarray2, (theta_guess/3), (C_bar_guess2/3))
AD_Pe_2
time_data3 = EPA.ftime('/Users/helenaharris/Documents/GitHub/hjh57cee4530/Lab 5 Plug Flow Exp3.txt',5,-55)
photoarray3 = EPA.Column_of_data('/Users/helenaharris/Documents/GitHub/hjh57cee4530/Lab 5 Plug Flow Exp3.txt',5,-55,1,'mg/L')
C_bar_guess3 = np.max(photoarray3)
CMFR_N_3 = EPA.Solver_CMFR_N(time_data3, photoarray3, theta_guess, C_bar_guess1)
CMFR_N_3
AD_Pe_3 = EPA.Solver_AD_Pe(time_data3, photoarray3, (theta_guess/2), (C_bar_guess3/2))
AD_Pe_3
V_tubing = .336*u.L
theta_tubing = (V_tubing/Q)
Tracer_m1 = 10*u.mg
C_bar_guess4 = (Tracer_m1/V_tubing)
theta_guess_tube = theta_tubing
time_data4 = EPA.ftime('/Users/helenaharris/Documents/GitHub/hjh57cee4530/Lab5Tubing01mlpart2.txt',9,28)
photoarray4 = EPA.Column_of_data('/Users/helenaharris/Documents/GitHub/hjh57cee4530/Lab5Tubing01mlpart2.txt',9,28,1,'mg/L')
CMFR_N_4 = EPA.Solver_CMFR_N(time_data4, photoarray4, theta_guess_tube, C_bar_guess4)
CMFR_N_4
AD_Pe_4 = EPA.Solver_AD_Pe(time_data4, photoarray4, theta_guess_tube, C_bar_guess4)
AD_Pe_4
Tracer_m2 = 5*u.mg
C_bar_guess5 = (Tracer_m2/V_tubing)
time_data5 = EPA.ftime('/Users/helenaharris/Documents/GitHub/hjh57cee4530/Lab5Tubing005mlpart2.txt',4,25)
photoarray5 = EPA.Column_of_data('/Users/helenaharris/Documents/GitHub/hjh57cee4530/Lab5Tubing005mlpart2.txt',4,25,1,'mg/L')
CMFR_N_5 = EPA.Solver_CMFR_N(time_data5, photoarray5, theta_guess_tube, C_bar_guess5)
CMFR_N_5
AD_Pe_5 = EPA.Solver_AD_Pe(time_data5, photoarray5, theta_guess_tube, C_bar_guess5)
AD_Pe_5
Tracer_m3 = 1*u.mg
C_bar_guess6 = (Tracer_m3/V_tubing)
time_data6 = EPA.ftime('/Users/helenaharris/Documents/GitHub/hjh57cee4530/Lab5Tubing001mlpart2.txt',5,-1)
photoarray6 = EPA.Column_of_data('/Users/helenaharris/Documents/GitHub/hjh57cee4530/Lab5Tubing001mlpart2.txt',5,-1,1,'mg/L')
CMFR_N_6 = EPA.Solver_CMFR_N(time_data6, photoarray6, theta_guess_tube, C_bar_guess6)
CMFR_N_6
AD_Pe_6 = EPA.Solver_AD_Pe(time_data6, photoarray6, theta_guess_tube, C_bar_guess6)
AD_Pe_6
```

2. Generate a plot showing the experimental data as points and the model results as thin lines for each of your experiments. Explain which model fits best and discuss those results based on your expectations.

```python
CMFR_E_Model_1 = EPA.E_CMFR_N((time_data1/theta),CMFR_N_1[2])
AD_E_Model_1 = EPA.E_Advective_Dispersion((time_data1/theta),AD_Pe_1[2])
E_1 = (photoarray1*V)/Tracer_mass
plt.figure()
plt.plot((time_data1/theta).to(u.dimensionless),CMFR_E_Model_1, label = 'CMFR Model')
plt.plot((time_data1/theta).to(u.dimensionless),AD_E_Model_1, label = 'AD Model')
plt.plot((time_data1/theta).to(u.dimensionless),E_1, 'ro', label = 'Experimental Data')
plt.xlabel('Residence Time')
plt.ylabel('E(t)')
plt.title('Experimental E vs Residence Time: 1st Baffle Arrangement')
plt.legend()
#plt.savefig('EvsResTimeBaffles1.png')
plt.show()

CMFR_E_Model_2 = EPA.E_CMFR_N((time_data2/theta),CMFR_N_2[2])
AD_E_Model_2 = EPA.E_Advective_Dispersion((time_data2/theta),AD_Pe_2[2])
E_2 = (photoarray2*V)/Tracer_mass
plt.figure()
plt.plot((time_data2/theta).to(u.dimensionless),CMFR_E_Model_2, label = 'CMFR Model')
plt.plot((time_data2/theta).to(u.dimensionless),AD_E_Model_2, label = 'AD Model')
plt.plot((time_data2/theta).to(u.dimensionless),E_2, 'ro', label = 'Experimental Data')
plt.xlabel('Residence Time')
plt.ylabel('E(t)')
plt.title('Experimental E vs Residence Time: 2nd Baffle Arrangement')
plt.legend()
#plt.savefig('EvsResTimeBaffles2.png')
plt.show()

CMFR_E_Model_3 = EPA.E_CMFR_N((time_data3/theta),CMFR_N_3[2])
AD_E_Model_3 = EPA.E_Advective_Dispersion((time_data3/theta),AD_Pe_3[2])
E_3 = (photoarray3*V)/Tracer_mass
plt.figure()
plt.plot((time_data3/theta).to(u.dimensionless),CMFR_E_Model_3, label = 'CMFR Model')
plt.plot((time_data3/theta).to(u.dimensionless),AD_E_Model_3, label = 'AD Model')
plt.plot((time_data3/theta).to(u.dimensionless),E_3, 'ro', label = 'Experimental Data')
plt.xlabel('Residence Time')
plt.ylabel('E(t)')
plt.title('Experimental E vs Residence Time: 3rd Baffle Design')
plt.legend()
#plt.savefig('EvsResTimeBaffles3.png')
plt.show()

CMFR_E_Model_tube_1 = EPA.E_CMFR_N((time_data4/theta_tubing),CMFR_N_4[2])
AD_E_Model_tube_1 = EPA.E_Advective_Dispersion((time_data4/theta_tubing),AD_Pe_4[2])
E_tube_1 = (photoarray4*V_tubing)/Tracer_m1
plt.figure()
plt.plot((time_data4/theta_tubing).to(u.dimensionless),CMFR_E_Model_tube_1, label = 'CMFR Model')
plt.plot((time_data4/theta_tubing).to(u.dimensionless),AD_E_Model_tube_1, label = 'AD Model')
plt.plot((time_data4/theta_tubing).to(u.dimensionless),E_tube_1, 'ro', label = 'Experimental Data')
plt.xlabel('Residence Time')
plt.ylabel('E(t)')
plt.title('Experimental E vs Residence Time: Tube Reactor with 0.1 mL Dye')
plt.legend()
#plt.savefig('EvsResTimeTubes1.png')
plt.show()

CMFR_E_Model_tube_2 = EPA.E_CMFR_N((time_data5/theta_tubing),CMFR_N_5[2])
AD_E_Model_tube_2 = EPA.E_Advective_Dispersion((time_data5/theta_tubing),AD_Pe_5[2])
E_tube_2 = (photoarray5*V_tubing)/Tracer_m2
plt.figure()
plt.plot((time_data5/theta_tubing).to(u.dimensionless),CMFR_E_Model_tube_2, label = 'CMFR Model')
plt.plot((time_data5/theta_tubing).to(u.dimensionless),AD_E_Model_tube_2, label = 'AD Model')
plt.plot((time_data5/theta_tubing).to(u.dimensionless),E_tube_2, 'ro', label = 'Experimental Data')
plt.xlabel('Residence Time')
plt.ylabel('E(t)')
plt.title('Experimental E vs Residence Time: Tube Reactor with 0.05 mL Dye')
plt.legend()
#plt.savefig('EvsResTimeTubes2.png')
plt.show()

CMFR_E_Model_tube_3 = EPA.E_CMFR_N((time_data6/theta_tubing),CMFR_N_6[2])
AD_E_Model_tube_3 = EPA.E_Advective_Dispersion((time_data6/theta_tubing),AD_Pe_6[2])
E_tube_3 = (photoarray6*V_tubing)/Tracer_m3
plt.figure()
plt.plot((time_data6/theta_tubing).to(u.dimensionless),CMFR_E_Model_tube_3, label = 'CMFR Model')
plt.plot((time_data6/theta_tubing).to(u.dimensionless),AD_E_Model_tube_3, label = 'AD Model')
plt.plot((time_data6/theta_tubing).to(u.dimensionless),E_tube_3, 'ro', label = 'Experimental Data')
plt.xlabel('Residence Time')
plt.ylabel('E(t)')
plt.title('Experimental E vs Residence Time: Tube Reactor with 0.01 mL Dye')
plt.legend()
#plt.savefig('EvsResTimeTubes3.png')
plt.show()

```

3. Compare the trends in the estimated values of N and Pe across your set of experiments. How did your chosen reactor modifications effect dispersion?

Our baffle modifications in the first part of the experiment **blah blah blah**. In the second part of the experiment we modified our reactor to be a long tub into which we injected dye. This presented much less of an opportunity for dispersion than our baffled reactors.

4. Report the values of t* at F = 0.1 for each of your experiments. Do they meet your expectations?

```python

```
The values of t* **do/do not** meet our expectations.

5.	Evaluate whether there is any evidence of “dead volumes” or “short circuiting” in your reactor.

There **blank** evidence of dead volumes in our reactors.

7. Make a recommendation for the design of a full scale chlorine contact tank. As part of your recommendation discuss the parameter you chose to vary as part of your experimentation and what the optimal value was determined to be.

In the design of a full scale contact tank we recommend **blank**. We determined that this modification would be an improvement by changing **blank** in our experiments, and the optimal value from our experiment was found to be **blank**.


1. Create the experimental E curve for each of your experiments

```python
# Baffle experiments
# Baffle Design 1
file = pd.read_csv('Lab 5 Plug Flow Exp1.csv',header = 1)
EPA.notes('Lab 5 Plug Flow Exp1.csv')
data1 = np.array(file)
start1 = 7
end1 = 141
start_time1 = pd.to_numeric(file.iloc[start1,0])*u.day
day_times1 = pd.to_numeric(file.iloc[start1:end1,0])
time_data1 = np.subtract((np.array(day_times1)*u.day),start_time1)
time_data_sec1 = (time_data1*24*3600).magnitude
photoarray1 = data1[7:141,1]
Q = 380*(u.ml/u.L)
V = 2*u.L
theta = (V/Q)
H_residence1 = time_data_sec1/(theta.magnitude)
Tracer_mass = 50*u.mg
E1 = (photoarray1*V)/Tracer_mass
#Baffle Design 2
file = pd.read_csv('Lab 5 Plug Flow Exp2.csv',header = 1)
EPA.notes('Lab 5 Plug Flow Exp2.csv')
data2 = np.array(file)
start2 = 4
end2 = -1
start_time2 = pd.to_numeric(file.iloc[start2,0])*u.day
day_times2 = pd.to_numeric(file.iloc[start2:end2,0])
time_data2 = np.subtract((np.array(day_times2)*u.day),start_time2)
time_data_sec2 = (time_data2*24*3600).magnitude
photoarray2 = data2[4:-1,1]
Q = 380*(u.ml/u.L)
V = 2*u.L
theta = (V/Q)
H_residence2 = time_data_sec2/(theta.magnitude)
Tracer_mass = 50*u.mg
E2 = (photoarray2*V)/Tracer_mass
#Baffle Design 3
file = pd.read_csv('Lab 5 Plug Flow Exp3.csv',header = 1)
EPA.notes('Lab 5 Plug Flow Exp3.csv')
data3 = np.array(file)
start3 = 5
end3 = -55
start_time3 = pd.to_numeric(file.iloc[start3,0])*u.day
day_times3 = pd.to_numeric(file.iloc[start3:end3,0])
time_data3 = np.subtract((np.array(day_times3)*u.day),start_time3)
time_data_sec3 = (time_data3*24*3600).magnitude
photoarray3 = data3[5:-55,1]
Q = 380*(u.ml/u.L)
V = 2*u.L
theta = (V/Q)
H_residence3 = time_data_sec3/(theta.magnitude)
Tracer_mass = 50*u.mg
E3 = (photoarray3*V)/Tracer_mass
plt.figure()
plt.plot(H_residence1,E1, label = 'Baffle Config 1')
plt.plot(H_residence2,E2, label = 'Baffle Config 2')
plt.plot(H_residence3,E3, label = 'Baffle Config 3')
# put in your x and y variables
plt.xlabel('Residence Time')
plt.ylabel('E(t*)')
plt.title('Experimental E vs Residence Time: Baffle Design')
plt.legend()
#plt.savefig('EvsResTimeBaffles1.png')
plt.show()
# Tubing Reactors
#0.1 mL of Tracer
V_tubing = .336*u.L
theta = (V_tubing/Q)
Tracer_m1 = 10*u.mg
file = pd.read_csv('Lab5Tubing01mlpart2.csv',header = 1)
EPA.notes('Lab5Tubing01mlpart2.csv')
data_1 = np.array(file)
start_1 = 9
end_1 = 28
start_time_1 = pd.to_numeric(file.iloc[start_1,0])*u.day
day_times_1 = pd.to_numeric(file.iloc[start_1:end_1,0])
time_data_1 = np.subtract((np.array(day_times_1)*u.day),start_time_1)
time_data_sec_1 = (time_data_1*24*3600).magnitude
photoarray_1 = data_1[9:28,1]
H_residence_1 = time_data_sec_1/(theta.magnitude)
E_tube_1 = (photoarray_1*V_tubing)/Tracer_m1

#0.05 mL of Tracer
Tracer_m2 = 5*u.mg
file = pd.read_csv('Lab5Tubing005mlpart2.csv',header = 1)
EPA.notes('Lab5Tubing005mlpart2.csv')
data_2 = np.array(file)
start_2 = 4
end_2 = 25
start_time_2 = pd.to_numeric(file.iloc[start_2,0])*u.day
day_times_2 = pd.to_numeric(file.iloc[start_2:end_2,0])
time_data_2 = np.subtract((np.array(day_times_2)*u.day),start_time_2)
time_data_sec_2 = (time_data_2*24*3600).magnitude
photoarray_2 = data_2[4:25,1]
H_residence_2 = time_data_sec_2/(theta.magnitude)
E_tube_2 = (photoarray_2*V_tubing)/Tracer_m2

#0.01 mL of Tracer
Tracer_m3 = 1*u.mg
file = pd.read_csv('Lab5Tubing001mlpart2.csv',header = 1)
EPA.notes('Lab5Tubing001mlpart2.csv')
data_3 = np.array(file)
start_3 = 5
end_3 = -1
start_time_3 = pd.to_numeric(file.iloc[start_3,0])*u.day
day_times_3 = pd.to_numeric(file.iloc[start_3:end_3,0])
time_data_3 = np.subtract((np.array(day_times_3)*u.day),start_time_3)
time_data_sec_3 = (time_data_3*24*3600).magnitude
photoarray_3 = data_3[5:-1,1]
H_residence_3 = time_data_sec_3/(theta.magnitude)
E_tube_3 = (photoarray_3*V_tubing)/Tracer_m3

plt.figure()
plt.plot(H_residence_1,E_tube_1, label = '0.1 mL Concentration')
plt.plot(H_residence_2,E_tube_2, label = '0.05 mL Concentration')
plt.plot(H_residence_3,E_tube_3, label = '0.01 mL Concentration')
# put in your x and y variables
plt.xlabel('Residence Time')
plt.ylabel('E(t*)')
plt.title('Experimental E vs Residence Time: Concentration Design')
plt.legend()
#plt.savefig('EvsResTimeBaffles1.png')
plt.show()
```
2. Use multivariable nonlinear regression to obtain the best fit between the experimental data and the two models by minimizing the sum of the squared errors. Use EPA.Solver_AD_Pe and EPA.Solver_CMFR_N. These functions will minimize the error by varying the values of average residence time, (mass of tracer/reactor volume), and either the number of CMFR in series or the Peclet number.

```python

N=3 #number of reactors in series
E_bffle_mdl = (N**N)/math.factorial(N-1)
```






### Conclusions


### Suggestions/Comments

I value your thoughts on where changes are needed as well as what is working well. Include ideas for:

improving the data acquisition software
modifying the experimental apparatus
making the experiment easier to understand
modifying the experiment to include some new experimental aspect related to the main topic
alternate ways to analyze the data
