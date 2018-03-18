```python
from aide_design.play import*
```
˛
## Name
1. Datasheet for Week 2 Lab: Laboratory Measurements

| Temperature Measurement     | value |
| --------------------------- | ----- |
| Distilled water temperature |   22.0 $^{\circ}$ C   |

| Pipette Technique (use DI-100 or Ohaus 160 balance) | value |
| --------------------------------------------------- | ----- |
| Density of water at that temperature                | 997.8 $kg/m^3$  |
| Actual mass of 990 µL of pure water                 |   0.9878 g  |
| Mass of 990 µL of water (rep 1)                     |   0.9164 g  |
| Mass of 990 µL of water (rep 2)                     |   0.9506 g  |
| Mass of 990 µL of water (rep 3)                     |   0.9112 g  |
| Mass of 990 µL of water (rep 4)                     |   0.9721 g  |
| Mass of 990 µL of water (rep 5)                     |   0.9727 g  |
| Average of the 5 measurements                       |   0.9446 g  |
| Standard deviation of the 5 measurements            |   0.0295    |

| Precision                                              | value |
| ------------------------------------------------------ | ----- |
| Percent coefficient of variation of the 5 measurements |   3.13%    |

| Accuracy                            | value |
| ----------------------------------- | ----- |
| average percent error for pipetting |   4.37%    |

Percent error for pipetting is calculated as follows:
$100*\frac{\left | mass_{density} - mass_{balance} \right |}{mass_{density}}$

| Measure Density (use DI-800 or Ohaus 400D or Prec. Std) | value |
| ------------------------------------------------------- | ----- |
| Molecular weight of NaCl                                |   58.44 g/mol    |
| Mass of NaCl in 100 mL of a 1-M solution                |   5.844 g    |
| Measured mass of NaCl used                              |   5.85 g    |
| Measured mass of empty 100 mL flask                     |   22.404 g    |
| Measured mass of flask + 1M solution                    |   125.996 g    |
| Mass of 100 mL of 1 M NaCl solution                     |   103.592 g    |
| Density of 1 M NaCl solution                            |   1036 $kg/m^3$    |
| Literature value for density of 1 M NaCl solution       |   1041 $kg/m^3$    |
| percent error for density measurment                    |   0.48%    |

| Prepare methylene blue standards of several concentrations | value |
| ---------------------------------------------------------- | ----- |
| Volume of 1 g/L MB diluted to 100 mL to obtain:            |       |
| 1 mg/L MB                                                  |   1000 $\mu L$    |
| 2 mg/L MB                                                  |   2000 $\mu L$    |
| 3 mg/L MB                                                  |   3000 $\mu L$    |
| 4 mg/L MB                                                  |   4000 $\mu L$    |
| 5 mg/L MB                                                  |   5000 $\mu L$    |
| Absorbance of unknown at 660 nm                            |   .6607    |
| Calculated concentration of unknown                        |   3.42 mg/L    |

| Measure absorbance at 660 nm using a spectrophotometer | value |
| ------------------------------------------------------ | ----- |
| Absorbance of distilled water                          |   .0001    |
| Absorbance of 1 mg/L methylene blue                    |   .1733    |
| Absorbance of 2 mg/L methylene blue                    |   .3552    |
| Absorbance of 3 mg/L methylene blue                    |   .5677    |
| Absorbance of 4 mg/L methylene blue                    |   .7293    |
| Absorbance of 5 mg/L methylene blue                    |   1.0319    |
| Slope at 660 nm (m)                                    |   .201    |
| Intercept at 660 nm (b)                                |   -0.0267    |
| Correlation coefficient at 660 nm (r)                  |   .9955    |
| Calculated concentration of unknown                    |   3.42    |                                       


2. Create a graph of absorbance at 660 nm vs. concentration of methylene blue in Atom using the exported data file. Does absorbance at 660 nm increase linearly with concentration of methylene blue?

```python
# import your file
file = pd.read_csv('CEE4530_Lab1_shf48Export.csv')
array = np.array(file)
absorbance = array[236, 1:7]
concentration = [0, 1, 2, 3, 4, 5]*(u.mg/u.L)
## do your data analysis here
## How to make an array?
#  x = [1,2,3,4,5]
# python starts at index 0
# x[2] = 3
# you can multiply an array with units
# y = [4,5,6] * u.mg/u.L
## 2-D Arrays
# z = np.array([[1,2,3],[4,5,6],[7,8,9]])
# z[1,2] = 5
# z[2,:] = [7,8,9]

## plotting
plt.figure('ax',(10,8))
plt.plot(concentration,absorbance)
# put in your x and y variables
plt.xlabel('Methylene Blue concentration (mg/L)', fontsize=15)
plt.ylabel('Absorbance', fontsize=15)
plt.show()
print ('The absorbance at 660 nm increases almost linearly with concentration.')
```
Your answer to the questions here.

3. Plot $\epsilon$ as a function of wavelength for each of the standards on a single graph. Note that the path length is 1 cm. Make sure you include units and axis labels on your graph. If Beer’s law is obeyed what should the graph look like?
```python
b = 1*(u.cm)
wavelengths = array[1:,0]
absorbances_1 = array[1:,2]
absorbances_2 = array[1:,3]
absorbances_3 = array[1:,4]
absorbances_4 = array[1:,5]
absorbances_5 = array[1:,6]

epsilon_1 = absorbances_1/(b*concentration[1])
epsilon_2 = absorbances_2/(b*concentration[2])
epsilon_3 = absorbances_3/(b*concentration[3])
epsilon_4 = absorbances_4/(b*concentration[4])
epsilon_5 = absorbances_5/(b*concentration[5])

## Multiplication of Arrays
# you can multiply and divide arrays by constants!
# x = [1,2]
# y = 3
# z = 4
# a = x/z
# a = [0.25,0.5]
# b = x*y
# b = [3,6]
plt.figure('ax',(10,8))
plt.plot(wavelengths,epsilon_1)
# put in your x and y variables
plt.plot(wavelengths,epsilon_2)
# put in your x and y variables
plt.plot(wavelengths,epsilon_3)
# put in your x and y variables
plt.plot(wavelengths,epsilon_4)
# put in your x and y variables
plt.plot(wavelengths,epsilon_5)
# put in your x and y variables
plt.xlabel('Wavelength (nm)', fontsize=15)
plt.ylabel('Extinction coefficients', fontsize=15)
plt.show()
```

Your answer to the question here

4. Did you use interpolation or extrapolation to get the concentration of the unknown?

    Interpolation.

5. What colors of light are most strongly absorbed by methylene blue?

    Red and green.

6. What measurement controls the accuracy of the density measurement for the NaCl solution? What density did you expect (see prelab 2)? Approximately what should the accuracy be?

    The concentration of NaCl controls the accuracy of the density. The expected density of the NaCl was 1039 $kg/m^3$ from the prelab. The accuracy should be related to the accuracy of the mass of the salt.

7. Don’t forget to write a brief paragraph on conclusions and on suggestions using Markdown.

    In conclusion, through this lab I gain proficiency in calibrating and using electronic balances; using signal conditioning boxes and data acquisition software; digital pipetting; preparing a solution of known concentration; preparing dilutions; and measuring concentrations using a UV Vis spectrophotometer. I've also gained general familiarity with laboratory procedures. It has also afforded me with the opportunity to practice importing and plotting data in python. I think it would have been helpful to have more clearly worded questions because I found a couple of the questions in both the pre and post lab to be a little confusing. I also had a little trouble importing the data for question 2 because the file was not saved as the correct file type and the data was not properly delimited.


8. Verify that your report and graphs meet the requirements as outlined in the course materials.
