## Laboratory 4 PreLab Questions
### Helena Harris

1. equation 1.21 Compare the ability of Cayuga lake and Wolf pond (an Adirondack lake) to withstand an acid rain runoff event (from snow melt) that results in 20% of the original lake water being replaced by acid rain. The acid rain has a pH of 3.5 and is in equilibrium with the atmosphere. The ANC of Cayuga lake is 1.6 meq/L and the ANC of Wolf Pond is 70 µeq/L. Assume that carbonate species are the primary component of ANC in both lakes, and that they are in equilibrium with the atmosphere. What is the pH of both bodies of water after the acid rain input? Remember that ANC is the conservative parameter (not pH!).

```python
from aide_design.play import*

cayuga_ANC_0 = 1.6*(10**(-3))
ph_in = 3.5
conc_hplus = 10**(-1*ph_in)
ANC_in = -1*conc_hplus
wolf_ANC_0 = 70*(10**(-6))
cayuga_ANC_out = (ANC_in*(1-(np.exp(-0.2))))+(cayuga_ANC_0*np.exp(-0.2))
wolf_ANC_out = (ANC_in*(1-(np.exp(-0.2))))+(wolf_ANC_0*np.exp(-0.2))
wolf_ANC_out
cayuga_ANC_out

Kw = 10**(-14)
K1_carbonate = 10**(-6.37)
K2_carbonate = 10**(-10.25)
K_Henry_CO2 = 10**(-1.5)
P_CO2 = 10**(-3.5)

def invpH(pH):
  return 10**(-pH)

def alpha0_carbonate(pH):
   alpha0_carbonate = 1/(1+(K1_carbonate/invpH(pH))*(1+(K2_carbonate/invpH(pH))))
   return alpha0_carbonate

def alpha1_carbonate(pH):
  alpha1_carbonate = 1/((invpH(pH)/K1_carbonate) + 1 + (K2_carbonate/invpH(pH)))
  return alpha1_carbonate

def alpha2_carbonate(pH):
  alpha2_carbonate = 1/(1+(invpH(pH)/K2_carbonate)*(1+(invpH(pH)/K1_carbonate)))
  return alpha2_carbonate

def ANC_closed(pH,Total_Carbonates):
  return Total_Carbonates*(alpha1_carbonate(pH)+2*alpha2_carbonate(pH)) + Kw/invpH(pH) - invpH(pH)

def ANC_open(pH):
  return ANC_closed(pH,P_CO2*K_Henry_CO2/alpha0_carbonate(pH))
import scipy
from scipy import optimize

# Strip the units off of the ANC function so that scipy can calculate the root.
def ANC_open(pH):
  return (ANC_open(pH)-cayuga_ANC_out)####put your ANC value in here)

def pH_open():
  return optimize.brentq(ANC_open_unitless_cayuga, 1, 12)

pH_open()

Kw = 10**(-14)
K1_carbonate = 10**(-6.37)
K2_carbonate = 10**(-10.25)
K_Henry_CO2 = 10**(-1.5)
P_CO2 = 10**(-3.5)

def invpH(pH):
  return 10**(-pH)

def alpha0_carbonate(pH):
   alpha0_carbonate = 1/(1+(K1_carbonate/invpH(pH))*(1+(K2_carbonate/invpH(pH))))
   return alpha0_carbonate

def alpha1_carbonate(pH):
  alpha1_carbonate = 1/((invpH(pH)/K1_carbonate) + 1 + (K2_carbonate/invpH(pH)))
  return alpha1_carbonate

def alpha2_carbonate(pH):
  alpha2_carbonate = 1/(1+(invpH(pH)/K2_carbonate)*(1+(invpH(pH)/K1_carbonate)))
  return alpha2_carbonate

def ANC_closed(pH,Total_Carbonates):
  return Total_Carbonates*(alpha1_carbonate(pH)+2*alpha2_carbonate(pH)) + Kw/invpH(pH) - invpH(pH)

def ANC_open(pH):
  return ANC_closed(pH,P_CO2*K_Henry_CO2/alpha0_carbonate(pH))

import scipy
from scipy import optimize

def ANC_open_unitless(pH):
  return (ANC_open(pH)-wolf_ANC_out) ####put your ANC value in here)

def pH_open():
  return optimize.brentq(ANC_open_unitless, 1, 12)

pH_open()

```
2. What is the ANC of a water sample containing only carbonates and a strong acid that is at pH 3.2?
```python
ph = 3.2
conc_hplus_sample = 10**(-1*ph)
ANC = -1*conc_hplus_sample
ANC

```

3. Why is [H+] not a conserved species?

It is not a conserved species because it reacts with carbonates.
