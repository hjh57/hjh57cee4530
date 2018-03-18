```python
from aide_design.play import*
```
# Lab 3 Report
## Helena Harris, Andrew Kang, Simon Fines

### Data Analysis

1. Plot measured pH of the lake versus hydraulic residence time (t/θ)

```python
file = pd.read_csv('Lab 2 - Acid Rain ak694 Part3.xls.csv',header = 1)
data_array = np.array(file)
time_array = (data_array[:,0])
ph_array = data_array[:,2]
#print(data_array)
#print(ph_array)
#print(time_array)
plt.figure('ax',(10,8))
plt.plot(time_array,ph_array)
# put in your x and y variables
plt.xlabel('Time (sec)', fontsize=15)
plt.ylabel('pH', fontsize=15)
plt.show()

```
2. Calculate α0, α1, and α2 based on the pH measured at each time point. Recall that each α represents the fraction of a particular carbonate system species at that pH. As such, the sum of the alphas at each measured pH should be 1.0!

```python
# Define functions and variables
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

3. Assuming that the lake can be modeled as a completely mixed flow reactor and that ANC is a conservative parameter, equation 1.21 can be used to calculate the expected ANC in the lake effluent as the experiment proceeds. Graph the expected ANC in the lake effluent versus the hydraulic residence time (t/θ) based on the completely mixed flow reactor equation with the plot labeled (in the legend) as “conservative ANC.”

```python
ANC_in = -0.001*(u.mole/u.L)
V = 4*u.L
Q = (0.267*(u.L/u.min)).to(u.L/u.s)
theta = V/Q
ANC_0 = 0*(u.mole/u.L)
t = time_array*(u.s)
ANC_con = ANC_in*(1-np.exp(-(t/theta)))+(ANC_0*np.exp(-(t/theta)))
hy_res_time = t/theta
plt.figure('ax',(10,8))
plt.plot(t,ANC_con, label = 'conservative ANC')
# put in your x and y variables
plt.xlabel('Time (sec)', fontsize=15)
plt.ylabel('conservative ANC', fontsize=15)
plt.legend()
plt.show()

```
4. If we assume that there are no carbonates exchanged with the atmosphere during the experiment, then we can calculate ANC in the lake effluent by using equation 1.11 describing the ANC of a closed system. Calculate the ANC under the assumption of a closed system and plot it on the same graph produced in answering question 3 with the plot labeled (in the legend) as “closed ANC.”
```python
Kw = 10**(-14)
P_CO2 = 10**(-3.5)
Kh = 10**(-1.5)
MW = 84.007
NaHCO3 = 0.623
Ct = NaHCO3/(MW*4)
closed_ANC = (Ct*(a1+(2*a2)))+(Kw/H_conc)-H_conc
plt.figure('ax',(10,8))
plt.plot(t,ANC_con, label = 'conservative ANC')
plt.plot(time, closed_ANC, label = 'closed ANC')
# put in your x and y variables
plt.xlabel('Time (sec)', fontsize=15)
plt.ylabel('ANC', fontsize=15)
plt.legend()
plt.show()
```
5. If we assume that there is exchange with the atmosphere and that carbonates are at equilibrium with the atmosphere, then we can calculate ANC in the lake effluent by using equation 1.15 describing the ANC of an open system. Calculate the ANC under the assumption of an open system and plot it on the same graph produced in answering question 3 with the plot labeled (in the legend) as “open ANC.”
```python

open_ANC = (((P_CO2*Kh)/a0)*(a1+(2*a2)))+(Kw/H_conc)-H_conc
plt.figure('ax',(10,8))
plt.plot(t,ANC_con, label = 'conservative ANC')
plt.plot(time, closed_ANC, label = 'closed ANC')
plt.plot(time, open_ANC, label = 'open ANC')
# put in your x and y variables
plt.xlabel('Time (sec)', fontsize=15)
plt.ylabel('ANC', fontsize=15)
plt.legend()
plt.show()
```

6. Derive an equation for CT (the concentration of carbonate species) as a function of time based on the input of NaHCO3 and its dilution in the completely mixed lake assuming that no carbonate species are lost or gained to the atmosphere (the equation will be the same form as equation 1.21). Graph CT versus the hydraulic residence time (t/θ) with the plot labeled (in the legend) as “conservative CT.”
```python
con_Ct = Ct*np.exp(-t/theta)
plt.figure('ax',(10,8))
plt.plot(t,con_Ct, label = 'conservative Ct')
# put in your x and y variables
plt.xlabel('Time (sec)', fontsize=15)
plt.ylabel('Ct', fontsize=15)
plt.legend()
plt.show()
```
7. Derive an equation for CT as a function of ANC and pH based on equation 1.11. Use the CMFR model to obtain ANC as a function of time (the results from question 3). Plot this measured CT versus hydraulic residence time (t/θ) on the same graph produced in answering question 6 with the plot labeled (in the legend) as “closed CT.”

```python
ANC_con_dimensionless = ANC_con.magnitude
ANC_con_5 = [ANC_con_dimensionless[0], ANC_con_dimensionless[300], ANC_con_dimensionless[600], ANC_con_dimensionless[900], ANC_con_dimensionless[1200]]
closed_Ct = ((ANC_con_5)-(Kw/H_conc)+(H_conc))/(a1+(2*a2))*(np.exp(-(time)/theta.magnitude))
plt.figure('ax',(10,8))
plt.plot(t,con_Ct, label = 'conservative Ct')
plt.plot(time,closed_Ct, label = 'closed Ct')
# put in your x and y
plt.xlabel('Time (sec)', fontsize=15)
plt.ylabel('Ct', fontsize=15)
plt.legend()
plt.show()
```
8. Plot the equilibrium concentration of CT (a function of pH) versus hydraulic residence time (t/θ) on the same graph produced in answering question 6 with the plot labeled (in the legend) as “equilibrium CT.”.

```python
equilibrium_Ct = (P_CO2*Kh)/a0
equilibrium_Ct
plt.figure('ax',(10,8))
plt.plot(t,con_Ct, label = 'conservative Ct')
plt.plot(time,closed_Ct, label = 'closed Ct')
plt.plot(time,equilibrium_Ct, label = 'equilibrium Ct')
# put in your x and y variables
plt.xlabel('Time (sec)', fontsize=15)
plt.ylabel('Ct', fontsize=15)
plt.legend()
plt.show()
```
9. Compare the two plots and determine whether the lake is best modeled as a volatile or non volatile system. What changes could be made to the lake to bring the lake into equilibrium with atmospheric CO2?

    The lake is best modeled as a closed, non volatile system. More surface area would bring the lake into equilibrium with atmospheric CO2.

10. Analyze the data from the 2nd experiment and graph the data appropriately. What did you learn from the 2nd experiment?

```python
file = pd.read_csv('Lab 2 - Acid Rain ak694 Part2.csv',header = 1)
data_array_2 = np.array(file)
#print (data_array_2)
time_array_2 = (data_array_2[:,0])
ph_array_2 = data_array_2[:,2]
#print (time_array_2)
#print(data_array_2)
#print(ph_array_2)

plt.figure('ax',(10,8))
plt.plot(time_array_2,ph_array_2)
# put in your x and y variables
plt.xlabel('Time (sec)', fontsize=15)
plt.ylabel('pH', fontsize=15)
plt.show()
```

From the second experiment we learned the same mass of calcium carbonate does not neutralize acid nearly as well as NaHCO3, and also to carefully read labels.

### Questions

1. What do you think would happen if enough NaHCO3 were added to the lake to maintain an ANC greater than 50 µeq/L for 3 residence times with the stirrer turned off? How much NaHCO3 would need to be added?

    The solution will stay blue longer because there are more weak bases that need to react with the strong acid for titration. 6750 milligrams of NaHCO3 would need to be added.

2. What are some of the complicating factors you might find in attempting to remediate a lake using CaCO3? Below is a list of issues to consider.
    - extent of mixing
    - solubility of CaCO3 (find the solubility and compare with NaHCO3)
    - density of CaCO3 slurry (find the density of CaCO3)

### Conclusions
Acid precipitation can cause serious effects on areas around the world, one of which is the acidification of lakes and streams. However,
nature has found a way to minimize the decrease in pH. The carbonate species found in these bodies of water are able to take up free hydrogen ions coming from the acid rain, thus hindering the acidification of the lake. The amount of acidification it can resist is known as the Acid Neutralizing Capacity, or ANC. This value is a function of the concentrations of the carbon series, including carbon dioxide, carbonic acid, bicarbonate, and carbonate. The experiment done in lab featured a lake with the addition of carbonate in the form of sodium bicarbonate. Acid rain was pumped in the lake while keeping the volume constant, and the pH of the lake was recorded as a function of time. From t was a modeled as a closed non-volatile system due to the conservation of ANC as well as total concentration of carbon species. After

Thoughts on where changes are needed as well as what is working well:

The software could be a little more user-friendly. Our pH probe was not consistent even after calibrating it. It took an average of about 8 minutes for it to go to steady state. The procedure could have included that two trials would need to be run at the beginning rather than the end. Also, because many students are still acclimatizing to python it might be helpful if the lectures were completed on Mondays and then Fridays could be like office hours.
