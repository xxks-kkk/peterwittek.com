Title: Ground state of spinless fermions with DMRG
Date: 2014-05-03 16:06
Author: Peter
Category: Python
Slug: ground-state-of-spinless-fermions-with-dmrg

We want to find the ground state energy of a free spinless fermionic
system, described by the following Hamiltonian ([Corboz et al.,
2009](#corboz2009simulation)):

\\[ H\_{\\mathrm{free}}=\\sum\_{}\\left[c\_{r}\^{\\dagger}
c\_{s}+c\_{s}\^{\\dagger} c\_{r}-\\gamma(c\_{r}\^{\\dagger}
c\_{s}\^{\\dagger}+c\_{s}c\_{r}
)\\right]-2\\lambda\\sum\_{r}c\_{r}\^{\\dagger}c\_{r}, \\]  
where \<rs\> goes through nearest neighbour pairs in a two-dimensional
lattice. The fermionic operators are subject to the following
constraints:  
\\[\\{c\_r, c\_s\^\\dagger\\}=\\delta\_{rs}I\_r,\\]

\\[\\{c\_r\^\\dagger, c\_s\^\\dagger\\}=0,\\]

\\[\\{c\_r, c\_s\\}=0.\\]

To calculate the ground state energy, we use the density matrix
renormalization group technique, which is included in ALPS ([Bauer et
al., 2011](#bauer2011alps)). The documentation of the package is sparse,
and it is often hard to figure out how to do even such a simple task.
Error messages from the console are not particularly helpful either.

Some templates for Hamiltonians are included in the /usr/xml/models.xml
file. There is a model for spinless fermions, but with fourth-order
terms:

\\[ H\_{\\mathrm{free}}=\\sum\_{}\\left(-t[c\_{r}\^{\\dagger}
c\_{s}+c\_{s}\^{\\dagger}c\_r]+V n\_i
n\_j\\right)-\\mu\\sum\_{r}c\_{r}\^{\\dagger}c\_{r},\\]  
where \$\$t\$\$, \$\$V\$\$, and \$\$\\mu\$\$ are parameters, and
\$\$n\_i\$\$ is the number operator. Setting \$\$\\gamma\$\$ to zero in
the original Hamiltonian, we can calculate the ground state. The
following Python code generates the problem and prints the ground state
energy and the truncation error:

<div class="highlight">

    import pyalps
     
    parms = [{
      'LATTICE'        : "open square lattice",  # Set up the lattice
      'MODEL'          : "spinless fermions",    # Select the model
      'L'              : 4,                      # Lattice dimension
      't'              : -1 ,                    # This and the following
      'mu'             : 2,                      # are parameters to the
      'U'              : 0 ,                     # Hamiltonian.
      'V'              : 0,
      'Nmax'           : 2 ,                     # These parameters are
      'SWEEPS'         : 4,                      # specific to the DMRG
      'MAXSTATES'      : 100,                    # solver.
      'NUMBER_EIGENVALUES' : 1,         
      'MEASURE_ENERGY' : 1
    }]

    prefix = '2D_spinless_fermions'
    input_file = pyalps.writeInputFiles(prefix,parms)
    res = pyalps.runApplication('dmrg',input_file,writexml=True)
     
    data = pyalps.loadEigenstateMeasurements(pyalps.getResultFiles(prefix=prefix))
     
    for s in data[0]:
        print s.props['observable'], ' : ', s.y[0]

</div>

References
==========

<a name="corboz2009simulation"></a>Corboz, P.; Evenbly, G.; Verstraete,
F. & Vidal, G. [Simulation of interacting fermions with entanglement
renormalization](http://arxiv.org/abs/0904.4151). *Physics Review A*,
2010, 81, pp. 010303.

<a name="bauer2011alps"></a>Bauer, B.; Carr, L.; Evertz, H.; Feiguin,
A.; Freire, J.; Fuchs, S.; Gamper, L.; Gukelberger, J.; Gull, E.;
Guertler, S. & others. [The ALPS project release 2.0: open source
software for strongly correlated
systems](http://arxiv.org/abs/1101.2646). *Journal of Statistical
Mechanics: Theory and Experiment*, IOP Publishing, 2011, 2011, P05001.
