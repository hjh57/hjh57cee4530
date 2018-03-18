## Laboratory 1 PreLab Questions
### Helena Harris

1. Why are contact lenses hazardous in the laboratory?

    Contact lenses can absorb and retain chemical vapors

2. What is the minimum information needed on the label for each chemical? When are Right-To-Know labels required?

    Chemicals must have at minimum a label with the full chemical name (not just the chemical formula), concentration, and date prepared. Right-To-Know labels are required if a chemical is designated as a hazardous material, that is having the characteristics of corrosivity, ignitability, toxicity (generally meaning a highly toxic material with an LD50 of 50 mg/kg or less), reactivity, etc., and if it is made into a solution or repackaged as a solid or liquid in a concentration greater than 1% (0.1% for a carcinogen)

3. Why is it important to label a bottle even if it only contains distilled water?

    Disposing of unknown chemicals is a pain so it is important to label everything including water, because you can't necessarily tell that it's water.

4. Find an SDS for sodium nitrate.
    1. Who created the SDS?

        ScienceLab.com, Inc.

    2. What is the solubility of sodium nitrate in water?

        92.1g/100 ml @ 25 deg. C.; 180 g/100 ml @ 100 deg.C

    3. Is sodium nitrate carcinogenic?

        Not available

    4. What is the LD50 oral rat?

        1267 mg/kg

    5. How much sodium nitrate would you have to ingest to give a 50% chance of death (estimate from available LD50 data).
    ```python
    from aide_design.play import*
    Mass_person = 70*(u.kg)
    LD50 = 1267*(u.mg/u.kg)
    Mass50Death = (Mass_person*LD50).to(u.g)
    print ('The mass of sodium nitrate required to give a 50% chance of death is ' ,+Mass50Death)
    ```
    6. How much of a 1 M solution would you have to ingest to give a 50% chance of death?
    ```python
molmassnano3 = 84.99*(u.g/u.mol)
conc = 1*(u.mol/u.l)
nano3perkgsol = molmassnano3*conc
amtsoltokill = (Mass50Death/nano3perkgsol).to(u.l)
print ('The amount of a 1 M solution required to give a 50% chance of death is ' ,+amtsoltokill)
    ```
    7. Are there any chronic effects of exposure to sodium nitrate?

        Mutagenic for bacteria and/or yeast. May be toxic to blood. Repeated or prolonged exposure to the substance can produce target organs damage.

5. You are in the laboratory preparing chemical solutions for an experiment and it is lunchtime. You decide to go to CTB to eat. What must you do before leaving the laboratory?

    Clearly label all chemicals and leave a note with a contact number in case of emergency.

## Laboratory 2 PreLab Questions

1. You need 100 mL of a 1 µM solution of zinc that you will use as a standard to calibrate an atomic adsorption spectrophotometer. Find a source of zinc ions combined either with chloride or nitrate (you can use the internet or any other source of information). What is the molecular formula of the compound that you found? Zinc disposal down the sanitary sewer is restricted at Cornell and the solutions you prepare may need to be disposed of as hazardous waste. As an environmental engineering student you strive to minimize waste production. How would you prepare this standard using techniques readily available in the environmental laboratory so that you minimize the production of solutions that you don’t need? Note that we have pipettes that can dispense volumes between 10 μL and 1 mL and that we have 100 mL and 1 L volumetric flasks. Include enough information so that you could prepare the standard without doing any additional calculations. Consider your ability to accurately weigh small masses. Explain your procedure for any dilutions. Note that the stock solution concentration should be an easy multiple of your desired solution concentration so you don’t have to attempt to pipette a volume that the digital pipettes can’t be set for such as 13.6 μL.
```Python
# 1 micromolarity solution = 1 micromole of zinc / liter solution
# = 0.1 micro mole zinc / 100 ml solution
molar_mass_zinc = 65.38*(u.g/u.mol)
molar_mass_zncl = 136.29*(u.g/u.mol)
mass_frac_zn_zncl = molar_mass_zinc/molar_mass_zncl
mass_zinc_100ml_1Msol = (0.1*(10**(-6))*(u.mol))*molar_mass_zinc
mass_zncl_100ml_1Msol = mass_zinc_100ml_1Msol/mass_frac_zn_zncl
print('The amount of zinc chloride in one liter of a 1 M solution of zinc would be ',+mass_zncl_100ml_1Msol)
#To produce this solution you would
# first dilute 13.625mg of zinc
# chloride to 100 ml. Then you would
# take 1 ml of that solution and
# dilute it to 100 ml. Then you would
# again take 1 ml of the solution and
# dilute it again to 100 ml. The
# calcualtion below verifies this.
conc_final_sol = ((13.625*(u.mg))/(100*(u.ml))/(100*100)).to(u.g/u.l)
print (conc_final_sol)
```



2. The density of sodium chloride solutions as a function of concentration is approximately 0.6985C + 998.29 (kg/m3) (C is kg of salt/m3). What is the density of a 1 M solution of sodium chloride?

```python
molmassnacl = 58.4*(u.g/u.mol)
conc_nacl = 1*(u.mol/u.l)
C = molmassnacl*conc_nacl
densitynaclsol = (0.6985*C + 998.29*(u.kg/u.m**3)).to(u.kg/u.m**3)
print('The density of a 1 M solution of sodium chloride is ' ,+densitynaclsol)
```
