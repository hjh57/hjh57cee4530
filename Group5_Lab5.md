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
# Lab 5 Report
## Helena Harris, Andrew Kang, Simon Fines
## Reactors Laboratory

## Introduction and Objective

## Procedure

## Results and Discussion

Lab 5 Analysis

1. Create the experimental E curve for each of your experiments

```python
file = pd.read_csv('Lab 5 Plug Flow Exp1.csv',header = 1)
EPA.notes('Lab 5 Plug Flow Exp1.csv')
data1 = np.array(file)
timearray1 = 24*3600*data1[7:141,0]
print (timearray1)
photoarray1 = data1[7:141,1]
print (photoarray1)
```
2. Create model E curves for a CMFR in series and for the one dimensional advection-dispersion equation.

```python

```

3. Use multivariable nonlinear regression to obtain the best fit between the experimental E curve and model E curves by minimizing the sum of the squared errors. Minimize the error by varying the values of either N or Pe (depending on the model).

```python

```

4. Generate a plot showing the experimental data as points and the model results as thin lines for each of your experiments. Explain which model fits best and discuss those results based on your expectations.

```python

```

5. Compare the trends in the estimated values of N and Pe across your set of experiments. How did your chosen reactor modifications effect dispersion?



6. Your models (parametrized in problem #3) are based on measured values of the flowrate and volume of your reactor (and consequently ϴ) and the mass of red dye added at time 0 (Mtr). It is possible that those measurements are not accurate. Repeat #3 but this time vary the values of N or Pe AND either ϴ or Mtr. Note the differences in the values of the estimated N or Pe and either ϴ or Mtr. Which models are yielding more convincing results, the original model (#3) or this modified model? Why?

```python

```

7. Report the values of t* at F = 0.1 for each of your experiments. Do they meet your expectations?

```python
```

8. Evaluate whether there is any evidence of “dead volumes” or “short circuiting” in your reactor.



9. Make a recommendation for the design of a full scale chlorine contact tank. As part of your recommendation discuss the parameter you chose to vary as part of your experimentation and what the optimal value was determined to be.



### Conclusions
