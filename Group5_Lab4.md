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
```
## Lab 4 Report
## Helena Harris, Andrew Kang, Simon Fines

## Acid Neutralizing Capacity Laboratory

## Introduction and Objective
Acid rain has been a growing issue in several parts of the world due to increase in car emissions and electricity generation from natural gas. The acidic rainfall can cause lakes and rivers, to drop in pH. However, these bodies of water are able to resist the acidification with the use of bicarbonates. These molecules are able to soak up the free hydrogen ions that cause the acidification. The susceptibility of the lake to acid rain can be measured by the Acid Neutralizing Capacity, or ANC. This lab will look at how bicarbonates can act as a buffer to the acidification of a lake, and what are the different parameters and variables for this process.

## Procedure

We first calibrated our pH probe to ProCoDA II for the pH data collection. To calibrate the pH probe, please follow steps 1 through 9 in the "Experimental Procedures" section in Acid Precipitation and Remediation of Acid Lakes Lab Manual. Follow Figure 1-2 in the Lab Manual to correctly set up the experimental apparatus, with the lake effluent properly draining out into the drain next to the work bench. It is important to give the desired flow rate of 267 mL/min based on the pump tubing set up in the experimental apparatus; refer to the table under step 11 in the Lab Manual. Place the container on top of the magnetic stirrer, add 4 L of distilled water and a stir bar. Set the stirrer speed to 8 then add about 1 mL of bromocresol green indicator solution to the lake. Add in 623 mg of NaHCO3 into the lake. After the lake is well stirred, take a 100 mL sample from the lake and label it (t=0). Place the pH probe away from the influent and effluent of the lake. Follow steps 19 through 20 in the Lab Manual, to properly collect and save data from ProCoDA software. Start the pump at t=0, and start pumping in acid rain from the solution tank. Take 100 mL samples from the lake effluent at time 5, 10, 15, and 20 minutes and label each sample. After the 20 minute sample is collected and labeled, measure the effluent flow rate. Turn off the pump and stop measuring pH. Measure the lake volume by taking the mass of the water in the lake. Repeat this experiment and change one of the following parameters: stirring, initial ANC, ANC source (CaCO3 instead of NaHCO3), and amount of ANC added.

Using the samples collected above, we must determine the ANC. We must create a titration curve by using 0.100 mL titrant increments throughout the entire titration. Put 50 mL of the acid lake sample into a 100 mL beaker and place it on the magnetic stirrer. Set the stir speed low. Place the pH probe in the solution. If the initial pH is less than 4.5, titration is not needed and use equation 1.13 in "Measurement of Acid Neutralizing Capacity" Lab Manual, to calculate the ANC. Record the initial pH and the initial sample volume. Follow steps 7 through 8 in the Lab Manual to properly use the Gran plot analysis and correctly collect and record data. Record the ANC and the equivalent volume.

## Results and Discussion

Lab 3 Analysis

### Figure 1: pH of Lake vs. Hydraulic Residence time


```python
file = pd.read_csv('Lab 2 - Acid Rain ak694 Part3.xls.csv',header = 1)
data_array = np.array(file)
time_array = (data_array[:,0])
ph_array = data_array[:,2]
V = 4*u.L
Q = (0.267*(u.L/u.min)).to(u.L/u.s)
theta = V/Q
res_array = time_array/theta
#print(data_array)
#print(ph_array)
#print(time_array)
plt.figure()
plt.plot(res_array,ph_array)
# put in your x and y variables
plt.xlabel('Residence Time', fontsize=15)
plt.ylabel('pH', fontsize=15)
plt.title('pH vs Residence Time')
plt.savefig('pHvshydraul.png')
plt.show()
```
![phgraph1](https://github.com/ak694/githubcee4530-1/blob/master/pHvshydraul1.png?raw=true).

**Figure 1**: As the hydraulic residence time increased, more acidic solution was pumped into the lake. Therefore, the graph shows the pH gradually decreasing as the hydraulic residence time increases. We calculated the alpha values based on our pH values and used then to calculate the conservative, closed and equilibrium ANC and graphed each of those vs. the hydraulic residence time.

### Figure 2: ANC Graph

```python
ANC_in = -0.001*(u.mole/u.L)
MW = 84.007
NaHCO3 = 0.623
Ct = NaHCO3/(MW*4)
ANC_0 = Ct*(u.mol/u.l)
t = time_array*(u.s)
ANC_con = ANC_in*(1-np.exp(-(t/theta)))+(ANC_0*np.exp(-(t/theta)))
hy_res_t = t/theta
Kw = 10**(-14)
P_CO2 = 10**(-3.5)
Kh = 10**(-1.5)
MW = 84.007
NaHCO3 = 0.623
Ct = NaHCO3/(MW*4)
hy_res_time = time/theta
closed_ANC = (Ct*(a1+(2*a2)))+(Kw/H_conc)-H_conc
open_ANC = (((P_CO2*Kh)/a0)*(a1+(2*a2)))+(Kw/H_conc)-H_conc
plt.figure()
plt.plot(hy_res_t,ANC_con, label = 'conservative ANC')
plt.plot(hy_res_time, closed_ANC, label = 'closed ANC')
plt.plot(hy_res_time, open_ANC, label = 'open ANC')
# put in your x and y variables
plt.xlabel('Residence Time')
plt.ylabel('ANC')
plt.title('ANC vs. Residence Time')
plt.legend()
plt.savefig('All_ANCgraph.png')
plt.show()
```

![All_ANCgraph](https://github.com/ak694/githubcee4530-1/blob/master/All_ANCgraph.png?raw=true)

**Figure 2**: Figure 2 shows the relationship between ANC and residence time for conservative, closed, and open ANC system. We used -0.001 mol/L for ANC_in and 0.00185 mol/L for ANC_0. The H concentration is found by using the inverse pH function from the collected pH data. The conservative ANC is modeled as a completely mixed flow reactor. The closed ANC is modeled so does not exchange carbonates with the atmosphere. The open ANC is modeled so the carbonates in the lake is in equilibrium with the atmosphere.


### Figure 3: Trial 2

```python
file = pd.read_csv('Lab 2 - Acid Rain ak694 Part2.csv',header = 1)
data_array_2 = np.array(file)
#print (data_array_2)
time_array_2 = (data_array_2[:,0])
ph_array_2 = data_array_2[:,2]
#print (time_array_2)
#print(data_array_2)
#print(ph_array_2)

plt.figure()
plt.plot(time_array_2,ph_array_2)
# put in your x and y variables
plt.xlabel('Residence Time', fontsize=15)
plt.ylabel('pH', fontsize=15)
plt.title('pH Data Trial 2')
plt.savefig('pHvstime.png')
plt.show()
```
![phgraph](https://github.com/ak694/githubcee4530-1/blob/master/pHvstime.png?raw=true)

**Figure 3**:  
Figure 3 shows the data from the second trial of the experiment. In this trial we used the same mass of CaCO3 instead of NaHCO3. There must be a significant difference in the mass fraction of carbonates between the two because there was no ANC after the first 5 minutes.


### Figure 4: Titration Curve

```python
file = pd.read_csv('Lab 4 Group 5 Gran1.csv',header = 6)
Gran1_array=np.array(file)
V_titrant1=Gran1_array[:,0]*u.mL
pH_data1=Gran1_array[:,1]
plt.figure()

plt.plot(V_titrant1, pH_data1)
# put in your x and y variables
plt.axvline(x=0.5, color ='b')
plt.axvline(x=1.0, color = 'g')
plt.xlabel('Volume of Titrant Added (mL)')
plt.ylabel('pH')
plt.title('Titration Curve')
plt.savefig('Titrationgraph.png')
plt.show()
```
![Titrationgraph](https://github.com/ak694/githubcee4530-1/blob/master/Titrationgraph.png?raw=true)

**Figure 4**: Figure 4 is the titration curve of the sample with 0.05 N HCl, plotting the pH as a function of titrant volume. The pH starts to slowly change when the volume of titrant added is around 0.5 mL, as shown by the blue line. The dominant reaction is between the base NaHCO3 and the acid HCl to produce H2CO3 and NaCl. The pH slowly changes when 1.0 mL of titrant is added, as shown with the green line. Because the reactions have used up all of the base, so the added H+ will add on to the acidity of the solution.


### Figure 5: Gran Plot

```python
#Define the gran function.
titrant_norm = 0.1*u.mole/u.L
V_sample = 50*u.mL
def F1(V_sample,V_titrant,pH):
  return (V_sample + V_titrant)/V_sample * EPA.invpH(pH)
#Create an array of the F1 values.
F1_data1 = F1(V_sample,V_titrant1,pH_data1)
#By inspection I guess that there are 4 good data points in the linear region.
N_good_points = 4
#use scipy linear regression. Note that the we can extract the last n points from an array using the notation [-N:]
slope, intercept, r_value, p_value, std_err = stats.linregress(V_titrant1[-N_good_points:],F1_data1[-N_good_points:])
#reattach the correct units to the slope and intercept.
intercept1 = intercept*u.mole/u.L
slope1 = slope*(u.mole/u.L)/u.mL
V_eq1 = -intercept1/slope1

print (V_eq1)
#The equivilent volume agrees well with the value calculated by ProCoDA.
#create an array of points to draw the linear regression line
x=[V_eq1.magnitude,V_titrant1[-1].magnitude ]
y=[0,(V_titrant1[-1]*slope1+intercept1).magnitude]
#Now plot the data and the linear regression
plt.plot(V_titrant1, F1_data1,'o')
plt.plot(x, y,'r')
plt.xlabel('Titrant Volume (mL)')
plt.ylabel('Gran function (mole/L)')
plt.title('Gran Plot')
plt.legend(['data'])
plt.savefig('Granplot.png')
plt.show()

```
![Granplot](https://github.com/ak694/githubcee4530-1/blob/master/Granplot.png?raw=true)

**Figure 5**: Figure 5 shows the Gran plot for the titration curve in Figure 4. Linear regression was performed on the last four values of the Gran plot. The equivalent volume was found to be 0.7969 milliliters which agrees with the Ve calculated by ProCoDA.


### Figure 6: Measured, Conservative, Volatile, and Nonvolatile ANC

```python
#function to go from Veq to ANC
def ANCcalc(V_sample,V_eq,N):
  return V_eq*N/V_sample
#t=5 min sample
file = pd.read_csv('Lab 4 Group 5 Gran2.csv',header = 6)
Gran2_array=np.array(file)
V_titrant2=Gran2_array[:,0]*u.mL
pH_data2=Gran2_array[:,1]
F1_data2 = F1(V_sample,V_titrant2,pH_data2)
slope, intercept, r_value, p_value, std_err = stats.linregress(V_titrant2[-N_good_points:],F1_data2[-N_good_points:])
intercept2 = intercept*u.mole/u.L
slope2 = slope*(u.mole/u.L)/u.mL
V_eq2 = -intercept2/slope2

#t=10 min sample
file = pd.read_csv('Lab 4 Group 5 Gran3.csv',header = 6)
Gran3_array=np.array(file)
V_titrant3=Gran3_array[:,0]*u.mL
pH_data3=Gran3_array[:,1]
F1_data3 = F1(V_sample,V_titrant3,pH_data3)
slope, intercept, r_value, p_value, std_err = stats.linregress(V_titrant3[-N_good_points:],F1_data3[-N_good_points:])
intercept3 = intercept*u.mole/u.L
slope3 = slope*(u.mole/u.L)/u.mL
V_eq3 = -intercept2/slope2

#ANC data
# t = 15 min and t = 20 min samples
pH4 = 4.2
pH5 = 3.6
ANC1=ANCcalc(V_sample,V_eq1,titrant_norm)
ANC2=ANCcalc(V_sample,V_eq2,titrant_norm)
ANC3=ANCcalc(V_sample,V_eq3,titrant_norm)
ANC4=EPA.invpH(pH4)
ANC5=EPA.invpH(pH5)
ANC_array = np.array([ANC1.magnitude,ANC2.magnitude,ANC3.magnitude,ANC4.magnitude,ANC5.magnitude])
plt.plot(hy_res_t,ANC_con, label = 'conservative ANC')
plt.plot(hy_res_time, closed_ANC, label = 'closed ANC')
plt.plot(hy_res_time, open_ANC, label = 'open ANC')
plt.plot(hy_res_time, ANC_array, label = 'titration ANC')
# put in your x and y variables
plt.xlabel('Residence Time')
plt.ylabel('ANC')
plt.title('ANC vs Residence Time')
plt.legend()
plt.savefig('Volatile_nonvolatileANC.png')
plt.show()
```

![non_volatileANCs](https://github.com/ak694/githubcee4530-1/blob/master/Volatile_nonvolatileANC.png?raw=true)

**Figure 6**: Figure 6 shows the graph of measured, conservative, volatile, and nonvolatile ANC. Although the measured ANC does not agree with the conservative model completely, it is much closer to the conservative model than any of the other models.

### Conclusions
Acid precipitation can cause serious effects on areas around the world, one of which is the acidification of lakes and streams. However, nature has found a way to minimize the decrease in pH. The carbonate species found in these bodies of water are able to take up free hydrogen ions coming from the acid rain, thus hindering the acidification of the lake. The amount of acidification it can resist is known as the Acid Neutralizing Capacity, or ANC. This value is a function of the concentrations of the carbon series, including carbon dioxide, carbonic acid, bicarbonate, and carbonate. The experiment done in lab featured a lake with the addition of carbonate in the form of sodium bicarbonate. Acid rain was pumped in the lake while keeping the volume constant, and the pH of the lake was recorded as a function of time. From the results, the system can be modeled as a closed non-volatile system due to the conservation of ANC as well as total concentration of carbon species. Using equations derived from ANC conservation, we were able to come up with hypothetical charts of how the buffering would be effected if the system had not been conserved in certain variables, such as exchange in carbon dioxide.

Thoughts on where changes are needed as well as what is working well:

The software could be a little more user-friendly. Our pH probe was not consistent even after calibrating it. It took an average of about 8 minutes for it to go to steady state. The procedure could have included that multiple trials would need to be run at the beginning rather than the end. Also, because many of the lab questions are being changed and students are still acclimatizing to python it might be helpful if the lectures were completed on Mondays and then Fridays could be like office hours.

## Appendix: Calculations and Derivations

```python
K1 = 10**-6.3
K2 = 10**-10.3
def invpH(pH):
  return 10**(-1*pH)
sample_array = np.array([7.0178, 6.8275, 6.4435, 5.8828, 3.7522])
H_conc = invpH(sample_array)
a0 = 1/(1+(K1/H_conc)+(K1*K2/(H_conc**2)))
a1 = 1/((H_conc/K1)+1+(K2/H_conc))
a2 = 1/(((H_conc**2)/(K1*K2))+(H_conc/K2)+1)
time = np.array([0, 300, 600, 900, 1200])*u.s
```
