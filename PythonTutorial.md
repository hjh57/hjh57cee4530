```python
from aide_design.play import*
import math
```

## Assorted Fluids Functions

| Equation Name                         |                                                                                            Equation                                                                                           |                       Physchem function                      |
|---------------------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|:------------------------------------------------------------:|
| Reynolds Number                       |                                                                                 $Re= \frac{{4Q}}{{\pi D\nu }}$                                                                                |                 `re_pipe(FlowRate, Diam, Nu)`                |
| Swamee-Jain Turbulent Friction factor           |                ${\rm{f}} = \frac{{0.25}}{{{{\left[ {\log \left( {\frac{\varepsilon }{{3.7D}} + \frac{{5.74}}{{{{{\mathop{\rm Re}\nolimits} }^{0.9}}}}} \right)} \right]}^2}}}$                |             `fric(FlowRate, Diam, Nu, PipeRough)`            |
| Laminar Friction factor           |                ${\rm{f}} = \frac{64}{Re}$                |                         |
| Hagen Pousille laminar flow head loss |                                                   ${h_{\rm{f}}} = \frac{{32\mu LV}}{{\rho g{D^2}}} = \frac{{128\mu LQ}}{{\rho g\pi {D^4}}}$                                                   |                                                              |
| Darcy Weisbach head loss              |                                                             ${h_{\rm{f}}} = {\rm{f}}\frac{8}{{g{\pi ^2}}}\frac{{L{Q^2}}}{{{D^5}}}$                                                            |    `headloss_fric(FlowRate, Diam, Length, Nu, PipeRough)`    |
| Swamee-Jain equation for diameter                              | $0.66\left ( \varepsilon ^{1.25}\left ( \frac{LQ^{2}}{gh_{f}} \right )^{4.75}+\nu Q^{9.4}\left ( \frac{L}{gh_{f}} \right )^{5.2} \right )^{0.04}$| `diam_swamee(FlowRate, HeadLossFric, Length, Nu, PipeRough)` |

# Design Challenge 1, learning Python, Jupyter, and some AguaClara Design Functions

### 1)
Calculate the minimum inner diameter of a PVC pipe that can carry a flow of at least 10 L/s for the town of Ojojona. The population is 4000 people. The water source is a dam with a surface elevation of 1500 m. The pipeline connects the reservoir to the discharge into a distribution tank at an elevation of 1440 m. The pipeline length is 2.5 km. The pipeline is made with PVC pipe with an SDR (standard diameter ratio) of 26.

The pipeline inlet at the dam is a square edge with a minor loss coefficient (${K_e}$) of 0.5. The discharge at the top of the distribution tank results in a loss of all of the kinetic energy and thus the exit minor loss coefficient is 1. See the minor loss equation below.

${h_e} = {K_e}\frac{{{V^2}}}{{2g}}$

The water temperature ranges from 10 to 30 Celsius. The roughness of a PVC pipe is approximately 0.1 mm. Use the fluids functions `pc.visocity_kinematic` and `pc.diam_pipe` function found [here](https://github.com/AguaClara/aide_design/blob/master/physchem.py) to help calculate the minimum inner pipe diameter to carry this flow from the dam to the distribution tank.

Report the following
* critical design temperature
* kinematic viscosity (maximum viscosity will occur at the lowest temperature)
* the minimum inner pipe diameter (in mm).


```python
print ('The critical design temperature is 10 degrees Celcius.')
temp = (10*(u.degC)).to(u.degK)
vk = pc.viscosity_kinematic(temp)
print ('The kinematic viscosity is '+ut.sig(vk,2))
pd = pc.diam_pipe(.01,60,2500,vk,.0001,0.5).to(u.mm)
print ('The minimum inner pipe diameter is '+ut.sig(pd,2))
```

### 2)
What is the maximum flow rate that can be carried by this pipe at the coldest design temperature? The actual inner pipe diameter is different from the minimum inner pipe diameter because actual pipes are sold in discrete sizes. The actual inner diameter is found by using the function `pc.ND_SDR_available` and is 4 inches.
Display the flow rate in L/s using the .to method.

```python
IDmin = pd
SDR = 26
ND_my_pipe = pipe.ND_SDR_available(IDmin,SDR)
Diam = ND_my_pipe
HeadLoss = 60*(u.m)
Length = 2.5*(u.km)
Nu = vk
PipeRough = 0.1* (u.mm)
KMinor = 0.5
mfr = pc.flow_pipe(Diam, HeadLoss, Length, Nu, PipeRough, KMinor)
print('The maximum flow rate is ' +ut.sig(mfr.to(u.L/u.s),4))

```

### 3)
What is the Reynolds number and friction factor for this maximum flow? Assign these values to variable names so you can plot them later on the Moody diagram.
```python
FlowRate = mfr
rn = pc.re_pipe(FlowRate, Diam, Nu)
print ('The reynolds number is ' +ut.sig(rn,2)+'.')
ff = pc.fric(FlowRate, Diam, Nu, PipeRough)
print ('The friction factor is ' +ut.sig(ff,2))

```

### 4)
Check to see if the fluids functions are internally consistent by calculating the head loss given the flow rate that you calculated and comparing that head loss with the elevation difference. Note that the Moody diagram has an accuracy of about ±5% for smooth pipes and ±10% for rough pipes [Moody, 1944](http://user.engineering.uiowa.edu/~me_160/lecture_notes/MoodyLFpaper1944.pdf).
```python
hl = pc.headloss(FlowRate, Diam, Length, Nu, PipeRough, KMinor)
print ('The calculated headloss is ' +ut.sig(hl,4))
print ('They are internally consistent.')

```

### 5)
Suppose an AguaClara plant is designed to be built up the hill from the distribution tank. The  transmission line will need to be lengthened by 30 m and the elevation of the inlet to the entrance tank will be 1450 m. The rerouting will also require the addition of 3 elbows with a minor loss coefficient of 0.3 each. What is the new maximum flow from the water source?
```python
newlength = Length + 30*(u.m)
newheadloss = 50*(u.m)
newminor = KMinor + 3*.3
newfr = pc.flow_pipe(Diam, newheadloss, newlength, Nu, PipeRough, newminor)
print ('The new flow rate is ' +ut.sig(newfr.to(u.l/u.s),4))

```
### 6)
There exists a function within the physchem file called `pc.fric(FlowRate, Diam, Nu, PipeRough)` that returns the friction factor for both laminar and turbulent flow. In this problem, you will be creating a new function which you shall call `fofRe()` that takes the Reynolds number and the dimensionless pipe roughness (ε/D) as inputs.

Recall that the format for defining a function is

`def fofRe(input1, input2):
    f = buncha stuff
    return f`

Since the equation for calculating the friction factor is different for laminar and turbulent flow (with the transition Reynolds number being defined within the physchem file), you will need to use an `if, else` statement for the two conditions. The two friction factor equations are given in the **Assorted Fluids Functions** table.

```python
def fofRe(pipeline_Re,roughness):
    if pipeline_Re <= 2300:
        f = 64/pipeline_Re
    else:
        f = .25/(math.log10((roughness/3.7)+(5.74/pipeline_Re**(.9))))**2
    return f
print(ut.sig(fofRe(10000,.01),2))

```

### 7a)
You will be creating a Moody diagram showing Reynolds number vs friction factor for multiple dimensionless pipe roughnesses. The first step to do this is to define the number of dimensionless pipe roughnesses you want to plot. We will plot 8 curves for the following values: 0, 0.0001, 0.0003, 0.001, 0.003, 0.01, 0.03, 0.1. We will plot an additional curve, which will be a straight line, for laminar flow, since it is not dependent on the pipe roughness value (see the Moody diagram above).

* Create an array for the dimensionless pipe roughness values, using `np.array([])`.
* Specify the amount of data points you want to plot for each curve. We will be using 50 points.

Because the Moody diagram is a log-log plot, we need to ensure that all 50 points on the diagram we are creating are equally spaced in log-space. Use the `np.logspace(input1, input2, input3)` function to create an array for turbulent Reynolds numbers and an array for laminar Reynolds numbers.
* `input1` is the exponent for the lower bound of the range. For example, if you want your lower bound to be 1000, your input should be `math.log10(1000)` which is equal to 3.
* `input2` is the exponent for the upper bound of the range. Format this input as you have formatted `input1`.
* `input3` is the number of data points you are using for each curve.

**7a) Deliverables**
* Array of dimentionless pipe roughnesses. Call this array `eGraph`.
* Variable defining the amount of points on each pipe roughness curve
* Two arrays created using `np.logspace` which for turbulent and laminar Reynolds numbers, which will be the x-axis values for the Moody diagram

Note: The bounds for the laminar Reynolds numbers array should span between 670 and the predefined transition number used in Problem 11. The bounds for the turbulent Reynolds numbers array should span between 3,500 and 100,000,000. These ranges are chosen to make the curves fit well within the graph and to intentionally omit data in the transition range between laminar and turbulent flows.

```python
eGraph = np.array([0, 0.0001, 0.0003, 0.001, 0.003, 0.01, 0.03, 0.1])
pts = 50
lamGraph = np.logspace(math.log10(670), math.log10(2100), pts)
turbGraph = np.logspace(math.log10(3500), math.log10(100000000), pts)

print(eGraph)
print(lamGraph)
print(turbGraph)

```

### 7b)

Now you will create the y-axis values for turbulent flow (based on dimensionless pipe roughness) and laminar flow (not based on dimensionless pipe roughness). To do this, you will use the `fofRe()` function you wrote in Problem 6 to find the friction factors.

Begin by creating an empty 2-dimensional array that will be populated by the turbulent-flow friction factors for each dimensionless pipe roughness. Use `np.zeros(number of rows, number of columns)`. The number of rows should be the number of dimensionless pipe roughness values (`len(eGraph)`), while the number of columns should be the number of data points per curve as defined above.

Populating this array with friction factor values will require two `for` loops, one to iterate through rows and one to iterate through columns. Recall that `for` loop syntax is as follows:

`example = np.zeros((40, 30))
for i in range(0, 40):
    for j in range(0, 30):
        example[i,j] = function(buncha[i],stuff[j])`

where `buncha` and `stuff` are arrays.

You will repeat this process to find the friction factors for laminar flow. The only difference between the turbulent and laminar friction flow arrays will be that the laminar array will only have one dimension since it does not affected by the dimensionless pipe roughness. Start by creating an empty 1-dimensional array and then use a single `for` loop.

**7b) Deliverables**
* One 1-D array containing friction factor values for laminar flow.
* One 2-D array containing friction factor values for each dimensionless pipe roughness for turbulent flow.

```python
yaxis_lam = np.zeros((pts))    
for k in range(pts):
    yaxis_lam[k]= fofRe(lamGraph[k],0)

print(yaxis_lam)
#print(len(yaxis_lam))

yaxis_turb = np.zeros((8, pts))
for i in range(0,8):
    for j in range(0,pts):
        yaxis_turb[i,j] = fofRe(turbGraph[j],eGraph[i])

print(yaxis_turb)

```

### 7c)

Now, we are ready to start making the Moody diagram!!!!!!!! The plot formatting is included for you in the cell below. You will add to this cell the code that will actually plot the arrays you brought into existence in 7a) and 7b) with a legend. For the sake of your own sanity, please only add code where specified.

* First, plot your arrays. See the plots in the tutorial above for the syntax. Recall that each dimensionless pipe roughness is a separate row within the 2-D array you created. To plot these roughnesses as separate curves, use a `for` loop to iterate through the rows of your array. To plot all columns in a particular row, use the `[1,:]` call on an array, where 1 is the row you are calling.


* Plotting the laminar flow curve does not require a `for` loop because it is a 1-D array.
    * Use a linewidth of 4 for all curves.



* Now plot the data point you calculated in in problem 3 and use the Reynolds number and friction factor obtained. Because this is a single point, it should be plotted as a circle instead of a line since a line composed of a single point does not exist.


* You will need to make a legend for the graph using `leg = plt.legend(stringarray, loc = 'best')`
    * The first input, `stringarray`, must be an array composed of strings instead of numbers. The array you created which contains the dimensionless pipe roughness values (`eGraph`) can be converted into a string array for your legend (`eGraph.astype('str'))`. You will need to add 'Laminar' and 'Pipeline' as strings to the new ` eGraph ` string array. Perhaps you will find `np.append(basestring, [('string1','string2')])` to be useful ;)

```python
#Set the size of the figure to make it big!
plt.figure('ax',(10,8))


#--------------------------------------------------------------------------------------
#---------------------WRITE CODE BELOW-------------------------------------------------
#--------------------------------------------------------------------------------------
print(len(eGraph))
for n in range (0,len(eGraph)):
    plt.plot(turbGraph,yaxis_turb[n,:], '-',linewidth = 4) #,label = eGraph[n]

plt.plot(lamGraph,yaxis_lam, '-', linewidth = 4) #, label = 'laminar'

plt.plot(rn, ff, 'o') #,label = 'pipeline')


legstringarray = eGraph.astype('str')
legstringarray = np.append(legstringarray, [('Laminar','Pipeline')])

leg = plt.legend(legstringarray, loc = 'best')



#--------------------------------------------------------------------------------------
#---------------------WRITE CODE ABOVE-------------------------------------------------
#--------------------------------------------------------------------------------------

#LOOK AT ALL THIS COOL CODE!
plt.yscale('log')
plt.xscale('log')
plt.grid(b=True, which='major', color='k', linestyle='-', linewidth=0.5)

#Set the grayscale of the minor gridlines. Note that 1 is white and 0 is black.
plt.grid(b=True, which='minor', color='0.5', linestyle='-', linewidth=0.5)

#The next 2 lines of code are used to set the transparency of the legend to 1.
#The default legend setting was transparent and was cluttered.


plt.xlabel('Reynolds number', fontsize=30)
plt.ylabel('Friction factor', fontsize=30)

plt.show()
```

### 8)
Researchers in the AguaClara laboratory collected the following head loss data through a 1/8" diameter tube that was 2 m long using water at 22°C. The data is in a comma separated data (.csv) file named ['Head_loss_vs_Flow_dosing_tube_data.csv'](https://github.com/AguaClara/CEE4540_DC/blob/master/Head_loss_vs_Flow_dosing_tube_data.csv). Use the pandas read csv function (`pd.read_csv('filename.csv')`) to read the data file. Display the data so you can see how it is formatted.

```python
hlfdt = pd.read_csv('Head_loss_vs_Flow_dosing_tube_data.csv')
hlfdt
```
### 9)
Using the data table from Problem 8, assign the head loss **and flow rate** data to separate 1-D arrays. Attach the correct units. `np.array` can extract the data by simply inputting the text string of the column header. Here is example code to create the first array:

`HL_data=np.array(head_loss_data['Head loss (m)'])*u.m`

```python
HL_data = np.array(hlfdt['Head loss (m)'])*u.m
FR_data = np.array(hlfdt['Flow rate (mL/min)'])*u.mL/u.min

print (HL_data.magnitude,HL_data.units)
print(FR_data.magnitude,FR_data.units)
```

### 10)
Calculate and report the maximum and minimum Reynolds number for this data set. Use the tube and temperature parameters specified in Problem 8. Use the `min` and `max` functions which take arrays as their inputs.
```python
diam=1/8*u.inch
length=2*u.m
temp=22*u.degC



maxFR = max(FR_data)
minFR = min(FR_data)
nu = pc.viscosity_kinematic(temp)

pc.re_pipe(FR_data, diam, nu);

MaxRe = (pc.re_pipe(maxFR, diam, nu))
MinRe = (pc.re_pipe(minFR, diam, nu))

print (MaxRe)
print (MinRe)
```

### 11)
What did you find most difficult about learning to use Python? Create a brief example as an extension to this tutorial to help students learn the topic that you found most difficult.
```python
print ('I found graphing to be the most difficult part of the tutorial so below I have an example where you graph a smiley face.')

#create a smiley face! play!
x = np.linspace(-5, 5, 1000);
y = 2*x**2  + 2

plt.plot(-2.5, 100, 'o')
plt.plot(2.5, 100, 'o')
plt.plot(0, 60, 'o')
plt.plot(x, y,'-');
plt.show()
```
