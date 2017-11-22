---
layout: post
title: Gurobi Optimizer Simple Tutorial
categories: Gurobi
description: 总结一下近来对Gurobi工具的学习
keywords: gurobi, optimizer, LP, QP, QCP,MILP
---

**Gurobi** is a mathematical optimizer which is designed to be the fastest and most powerful solver for:
* LP(Linear Programming): [Wiki link](https://en.wikipedia.org/wiki/Linear_programming)
* QP(Quadratic Programming): [Wiki link](https://en.wikipedia.org/wiki/Quadratic_programming)
* QCQP(Quadratic Constrained Quadratic Program): [Wiki link](https://en.wikipedia.org/wiki/Quadratically_constrained_quadratic_program)
* ILP(Integer Linear Programming) or IP(Integer Programming): All of the unknown variables are required to be integers.
* MIP(Mixed Integer Programming): Some of the unknown variables are integers.
* MIQP, MIQCQP

### Features
* programming interface: C++, Java, .Net, **Python**
* free license for student on the local machine.
* Gurobi has client-server and Cloud computing capabilities. 提供云计算服务
* Gurobi vs CPlex: Gurobi is easier to get the academic license and Gurobi has good support for python, which is easy to learn and code.

### Quick Tutorial
After installed on your on computer, there are severak ways to use Gurobi:

#### Gurobi interactive shell
* Start the IS: open the terminal, enter
```
exec gurobi.sh
```

* Read a model from a file and return a Model object
```python
 gurobi > m = read(‘model path’)
```

* Invoke the optimize method on the Model object
如果计算过程被打算，再次启动时，会接着上次打断出继续计算
```python
gurobi > m.optimize()
```

* Reset the optimization and start from the begining
```python
m.reset()
```

* Inspect the value of the model variable in the solution
```python
m.printAttr(‘X’)
v = m.getVars()
print (len(v))
print (v[0].varName, v[0].x)
```

* Change the MIP strategy to find a good feasible solutions instead of the optimized one
```python
m.setParam(‘MIPFocus’,1)
or
m.Param.MIPFocus=1
```

* Show the full set of available parameter
```python
paramHelp()
```
* Show current loaded model
```python
models()
```

#### Gurobi command line tool
* Calculate the optimization model
```
gurobi_cl model_name
```
Output the result to a file:
```
gurobi_cl ResultFile = file_name model_name
```

#### Python script (Gurobi Python Interface)
in python environment:
```python
from gurobipy import *
```

### Python Script Example
A simple example
```python
#!/usr/bin/python

# Copyright 2017, Gurobi Optimization, Inc.

# This example formulates and solves the following simple MIP model:
#  maximize
#        x +   y + 2 z
#  subject to
#        x + 2 y + 3 z <= 4
#        x +   y       >= 1
#  x, y, z binary

from gurobipy import *

try:

    # Create a new model
    m = Model("mip1")

    # Create variables
    x = m.addVar(vtype=GRB.BINARY, name="x")
    y = m.addVar(vtype=GRB.BINARY, name="y")
    z = m.addVar(vtype=GRB.BINARY, name="z")

    # Set objective
    m.setObjective(x + y + 2 * z, GRB.MAXIMIZE)
    # another way to build a objective
    # obj = LinExpr()
    # obj += x
    # obj += y
    # obj += 2*z
    # m.setObjective(obj, GRB.MAXIMIZE)


    # Add constraint: x + 2 y + 3 z <= 4
    m.addConstr(x + 2 * y + 3 * z <= 4, "c0")

    # Add constraint: x + y >= 1
    m.addConstr(x + y >= 1, "c1")

    m.optimize()

    for v in m.getVars():
        print('%s %g' % (v.varName, v.x))

    print('Obj: %g' % m.objVal)

except GurobiError as e:
    print('Error code ' + str(e.errno) + ": " + str(e))

except AttributeError:
    print('Encountered an attribute error')

```


### Reference Document:
Here are the quick start guide, examples and reference manuals.
[Check Here](http://www.gurobi.com/documentation/)
