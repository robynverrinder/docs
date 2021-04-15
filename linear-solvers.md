# Optimisers and linear solvers

## Ipopt + HSL solvers

- [Official Ipopt documentation](https://coin-or.github.io/Ipopt/index.html)
- The [GAMS documenation](https://www.gams.com/latest/docs/S_IPOPT.html) describes Ipopt's options quite nicely.
- [HSL for IPOPT](https://www.hsl.rl.ac.uk/ipopt/), which supplies `coinhsl`
- [HSL_MA86](https://www.hsl.rl.ac.uk/catalogue/hsl_ma86.html) (note that you likely don't want to download this directly)

### Compiling and installing on Ubuntu (tested), maybe Mac (untested)

In the following guide, lines that start with `$` are terminal/bash commands, `#` indicates a comment and everything else is output. Showing all output is a bit much, so dots (`...`) indicate unimportant output which has been removed.

First, download system packages (compilers, ...) as mentioned in the [official installation instructions](https://coin-or.github.io/Ipopt/INSTALL.html). On Linux that's one big `sudo apt install ...` command.

Start in an empty folder, and copy your `coinhsl-YYYY.MM.DD.tar.gz` file there. For me, that's:

```bash
# Show files in folder
$ ls
coinhsl-2015.06.23.tar.gz*

# Coinbrew (https://coin-or.github.io/coinbrew/) seems to be the current
# advised way of installing Ipopt. So, download it and make it executable
$ wget https://raw.githubusercontent.com/coin-or/coinbrew/master/coinbrew
$ chmod u+x coinbrew

# Fetch Ipopt
$ ./coinbrew fetch Ipopt --no-prompt

# Unpack HSL solvers, and move them to the newly-created ThirdParty folder
$ tar -xvzf coinhsl-2015.06.23.tar.gz
$ mv coinhsl-2015.06.23 ThirdParty/HSL/coinhsl
```

All of that was basic project setup. Your file structure should look something like,

```
.
├── coinbrew
├── coinhsl-2015.06.23.tar.gz
├── Ipopt
└── ThirdParty
    ├── ASL
    ├── HSL
    │   └── coinhsl
    └── Mumps
```

where the folders contain other stuff. To build and install:

```bash
# Build Ipopt and put it in a local folder called `build`
$ ./coinbrew build Ipopt --prefix=build --test --no-prompt --verbosity=3
...
Install completed. If executing any of the installed
binaries results in an error that shared libraries cannot
be found, you may need to
  - add 'export LD_LIBRARY_PATH=/home/alex/Projects/aru-docs/tmp/coinbrew-install/./build/lib' to your ~/.bashrc (Linux)
  - add 'export DYLD_LIBRARY_PATH=/home/alex/Projects/aru-docs/tmp/coinbrew-install/./build/lib' to ~/.bashrc (OS X)
# For what it's worth, I didn't need to export those library paths
```

Assuming there weren't any errors, let's create a simple Python test environment:

```bash
# Create a simple test environment
conda create --name test-ipopt python=3.9
conda activate test-ipopt
conda install -c conda-forge pyomo
```

I also installed IPython (`conda install ipython`) because it's a nicer REPL than the default. Launch Python (either using `python` or `ipython`) and run a [test problem](https://pyomo.readthedocs.io/en/stable/pyomo_overview/simple_examples.html):

```python
import pyomo.environ as pyo
def create_new_model():
    model = pyo.ConcreteModel()
    model.x = pyo.Var([1,2], domain=pyo.NonNegativeReals)
    model.OBJ = pyo.Objective(expr = 2*model.x[1] + 3*model.x[2])
    model.Constraint1 = pyo.Constraint(expr = 3*model.x[1] + 4*model.x[2] >= 1)
    return model

# let's first solve it using plain ipopt, to check that it's working:
from pyomo.opt import SolverFactory
opt = SolverFactory('ipopt', executable='build/bin/ipopt')
opt.solve(create_new_model())

# and then solve it again with MA86:
opt.options['linear_solver'] = 'ma86'
opt.options['OF_ma86_scaling'] = 'none'  # a random parameter
opt.solve(create_new_model(), tee=True)
```

Hopefully that worked! Now you just need to remember to tell Pyomo where to find the Ipopt executable whenever you solve a problem. It's a little extra work, but means you always know exactly which Ipopt executable is being used. That's super helpful when debugging, say, a missing/faulty linear solver.

If you want to remove the environment, you can use:

```bash
# switch to your `base` environment
$ conda activate
$ conda env remove --name test-ipopt
```

### Compiling and installing on Windows
TODO: apparently there's a PDF which describes this well. Could someone please link to it here?
