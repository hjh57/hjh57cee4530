```python
from aide_design.play import*
import Environmental_Processes_Analysis as EPA
import importlib
importlib.reload(EPA)
```
1. Calculate the volume of a 40g/L red dye stock that would need to be added to 1L of water to produce 0, 5, 10, 15, 20, 25, and 30 mg/L calibration points.

```python
# For 1 L of water you would need 0, 5, 10, 15,
# 20, 25, and 30 mg to produce concentrations of
# 0, 5, 10, 15, 20, 25, and 30 mg/L
masses_of_dye = np.array([0, 5, 10, 15, 20, 25, 30])
stock_amts = ((masses_of_dye*(u.mg))/(40*(u.g/u.l))).to(u.ml)
print('The amount of stock solution needed to create the calibration points are ' ,+stock_amts.magnitude)
```

2. Calculate the change in hydraulic grade line between baffled sections of a reactor with a flow rate of 380 mL/min. The reactor baffles are perforated with 24 holes 1 mm in diameter.

```python
Q_reactor = 380*(u.ml/u.min)
n_orifice = 24
d_orifice = 1*(u.mm)
K_orifice = 0.6
delta_h = ((((4*Q_reactor)/(np.pi*n_orifice*K_orifice*(d_orifice**2)))**2)/(2*pc.gravity)).to(u.mm)
print ('The change in hydraulic grade line is ' ,+delta_h)
```
3. On a single graph from Excel, plot the exit age distribution (E(t*)) for a reactor with a volume of 1L and a flow rate of 380 mL/min that operates as a 1-dimensional advection-dispersion reactor with Peclet numbers of 1, 10, and 100 (there will be three lines on the graph). The x-axis should be t* from 0.0 to 3.0. Comment on the shapes of the curves as a function of Pe.

```python
time_graph = np.linspace(0,3,500)
E_1 = EPA.E_Advective_Dispersion(1,time_graph)
E_10 = EPA.E_Advective_Dispersion(10,time_graph)
E_100 = EPA.E_Advective_Dispersion(100,time_graph)
plt.plot(time_graph, E_1)
plt.plot(time_graph, E_10)
plt.plot(time_graph, E_100)
plt.xlabel(r'$\frac{t}{\theta}$')
plt.ylabel(r'$\frac{C}{C_0}$')
plt.legend(['Pe = 1','Pe = 10','Pe =100'])
plt.show()

#time_graph = np.linspace(0,3,500)
#E_AD = EPA.E_Advective_Dispersion(1,time_graph)

#t_star = np.linspace(0.000001,3,500)
#Pe_1 = 1
#E_t_star_1 = np.sqrt(Pe_1/(4*np.pi*t_star))*np.exp(((-1*((1-t_star))**2)*Pe_1)/4*t_star)
#Pe_2 = 10
#E_t_star_2 = np.sqrt(Pe_2/(4*np.pi*t_star))*np.exp(((-1*((1-t_star))**2)*Pe_2)/4*t_star)
#Pe_3 = 100
#E_t_star_3 = np.sqrt(Pe_3/(4*np.pi*t_star))*np.exp(((-1*((1-t_star))**2)*Pe_3)/4*t_star)
#plt.figure()
#plt.plot(t_star,E_t_star_1)
#plt.plot(t_star,E_t_star_2)
#plt.plot(t_star,E_t_star_3)
#plt.xlabel('t*')
#plt.ylabel('E(t*)')
#plt.show()
```
