## Laboratory 1 PreLab Questions
### Helena Harris

1. How many grams of NaHCO3 would be required to keep the ANC levels in a lake above 50 µeq/L for 3 hydraulic residence times given an influent pH of 3.0 and a lake volume of 4 L, if the current lake ANC is 0 µeq/L?

```python
from aide_design.play import*
ANC_out = 0.00005 #u.eq/u.L
ANC_in = -0.001 #u.eq/u.L
volume = 4*(u.L)
mol_weight_NaHCO3 = 84*(u.mg/u.mmol)
ANC_0 = (ANC_out-(ANC_in*(1-(np.exp(-3)))))*(np.exp(3))
print (ANC_0)
conc_NaHCO3_0 = ANC_0*(10**3)*(u.mmol/u.L)
print (conc_NaHCO3_0)
mass_NaHCO3 = conc_NaHCO3_0*mol_weight_NaHCO3*volume
print('The mass of NaHCO_3 necessary to maintain an ANC of 50 ueq/L over 3 residence periods is ' ,+mass_NaHCO3)




```
