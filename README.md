----------------------------------------------------------------------------
This software was used for the article [Spuhler et al.(2018)](https://doi.org/10.1016/j.watres.2018.08.021) and the related data has been published under [https://doi.org/10.5281/zenodo.1092686](https://doi.org/10.5281/zenodo.1092686).

If you use this software for scientific work, please cite:
Spuhler, D., Scheidegger, A., & Maurer, M. (2018). Generation of sanitation system options for urban planning considering novel technologies. Water Research. https://doi.org/10.1016/j.watres.2018.08.021

---------------------------------------------------------------------------

[![Build Status](https://travis-ci.org/Eawag-SWW/SanitationSystemBuilder.jl.svg?branch=master)](https://travis-ci.org/Eawag-SWW/SanitationSystemBuilder.jl)

# SanitationSystemBuilder

Finds all combination of technologies that result in a valid
sanitation system.


# Installation

1. Install [Julia](https://julialang.org/) version 0.6 or newer.

2. Then the `SanitationSystemBuilder` package is installed with the Julia command:
```Julia
Pkg.clone("https://github.com/Eawag-SWW/SanitationSystemBuilder.jl.git")
```

# Usage

```Julia
using SanitationSystemBuilder

# -----------
# 1) define technologies

# sources
A = Tech(String[],      # array of input products. If empty, it defines a source.
         ["a1", "a2"],  # array of output products. If empty, it defines a sink.
         "A",           # name  of the technology
         "group1",      # functional group
         0.5)           # appropriateness score

B = Tech(String[], ["b1"], "B", "group1", 0.5)


# technologies
C = Tech(["a1"], String["c1"], "C", "group1", 0.5)
D = Tech(["a2", "b1"], String["d1", "d2", "d3"], "D", "group1", 0.5)

# sinks
E = Tech(["c1", "d1"], String[], "E", "group1", 0.5)
F = Tech(["d2", "d3"], String[], "F", "group1", 0.5)


sources = [A, B]
technolgies = [C, D, E, F]

# -----------
# 2) find systems

allSystems, _ = build_all_systems(sources, technolgies, storeDeadends=false)


# -----------
# 3) plot systems

# Works only if "graphiz" (http://www.graphviz.org/)
# is installed to provide the "dot" command.

# write a pdf for each system
for i in 1:length(allSystems)
    writedotfile(allSystems[i], "temp.dot")
    run(`dot -Tpdf temp.dot -o systems1_$i.pdf`)
end

```

In the example each technology is defined manually. For any real
application the convenience function `importTechFile` should be used
to import input files created with the model [TechAppA](https://github.com/Eawag-SWW/TechAppA).
