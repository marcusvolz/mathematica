# mathematica

Artistic visualizations created with Mathematica and the Wolfram Language

## Vector Flow Field

Trajectories of particles moving across a vector flow field defined by equations with coefficients c and powers p.

![facets](https://raw.githubusercontent.com/marcusvolz/mathematica/main/plots/vector-flow-field.png "Vector Flow Field")

```mathematica
SeedRandom[865];
n = 40000;
m = 50;
s = 0.001;
c = {3.3, 0.9, -0.8, -5.4, 5.3, -5.6, 3.7, 7.6, 5.0, 0.9, -6.1, -6.1};
p = {6, 2, 3, 2, 3, 6};

nextPoint = Function[xy,
 dx = c[[1]]*(Sin[c[[2]]*xy[[1]]])^(p[[1]]) +
      c[[3]]*(Sin[c[[4]]*xy[[2]]])^(p[[2]]) +
      c[[5]]*(Cos[c[[6]]*xy[[2]]])^(p[[3]]);
 
 dy = c[[7]]*(Sin[c[[8]]*xy[[2]]])^(p[[4]]) +
      c[[9]]*(Sin[c[[10]]*xy[[1]]])^(p[[5]]) +
      c[[11]]*(Cos[c[[12]]*xy[[1]]])^(p[[6]]);
 
 d = Sqrt[dx^2 + dy^2];
   
 xy + {dx, dy}*s/d
];

points = Table[
 NestList[
  nextPoint[#] &,
  RandomReal[{-0.5, 0.5}, 2],
  m
 ],
 n
];

ListLinePlot[
 points,
 AspectRatio -> 1,
 Axes -> False,
 ImageSize -> {800, 800},
 PlotStyle -> Directive[{Black, Thickness[0.00015]}],
 PlotRange -> {{-0.55, 0.55}, {-0.55, 0.55}}
]
```

## Weiszfeld's Algorithm

Iterative paths converging to the geometric median of 20 terminals using [Weiszfeld's algorithm](https://en.wikipedia.org/wiki/Geometric_median), starting from 10,000 random initial locations.

![facets](https://raw.githubusercontent.com/marcusvolz/mathematica/main/plots/weiszfeld.png "Weiszfeld's Algorithm")

```mathematica
SeedRandom[938463];

terminals = RandomReal[1, {20, 2}];

nextPoint = Function[{p, terminals},
 (#/EuclideanDistance[p, #] & /@ terminals // Total)/
 (1/EuclideanDistance[p, #] & /@ terminals // Total)
];

points = Table[
 NestWhileList[
  nextPoint[#, terminals] &,
  RandomReal[1, 2],
  EuclideanDistance[#1, #2] > 0.0001 &,
  2
 ],
 10000
];

Show[
 ListLinePlot[
  points,
  PlotMarkers -> {Style[\[FilledCircle], Black], Scaled[0.0001]},
  PlotStyle -> Directive[{Black, Opacity[0.05], Thickness[0.0003]}],
  PlotRange -> {{-0.05, 1.05}, {-0.05, 1.05}}
 ],
 ListPlot[
  terminals,
  PlotStyle -> {Black, PointSize[Large]},
  PlotRange -> {{-0.05, 1.05}, {-0.05, 1.05}}
 ],
 AspectRatio -> 1,
 Axes -> False,
 ImageSize -> {800, 800}
]
```
