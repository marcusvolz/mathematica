# mathematica

Artistic visualizations created with Mathematica and the Wolfram Language

## Vector Flow Field 005

Trajectories of particles moving across a vector flow field.

!["Vector Flow Field 005](https://raw.githubusercontent.com/marcusvolz/mathematica/main/plots/vector-flow-field-005.png "Vector Flow Field 005")

```mathematica
n = 10000;
m = 200;
s = 0.05;

nextPoint = Function[
 xy,
 dx = -10*(xy[[1]] + Sin[-15*xy[[2]]] - Sin[-10*xy[[2]]] + Cos[25*xy[[1]]]); 
 dy = -10*(xy[[2]] - Sin[-10*xy[[2]]] - Sin[20*xy[[2]]] - Cos[15*xy[[1]]]);
 d = Sqrt[dx^2 + dy^2];
 xy + {dx, dy}*s/d
];

points = Table[NestList[nextPoint[#] &, RandomReal[{-2, 2}, 2], m], n];

ListLinePlot[
 points,
 AspectRatio -> 1,
 Axes -> False,
 ImagePadding -> 5,
 ImageSize -> {800, 800}, 
 PlotStyle -> Directive[{Black, Thickness[0.000025]}]
]
```

## Vector Flow Field 004

Trajectories of particles moving across a vector flow field.

!["Vector Flow Field 004](https://raw.githubusercontent.com/marcusvolz/mathematica/main/plots/vector-flow-field-004.png "Vector Flow Field 004")

```mathematica
n = 10000;
m = 200;
s = 0.005;

nextPoint = Function[xy,
 dx = -Cos[10*xy[[2]]^2]^3;
 dy = -Sin[10*xy[[1]]^2*xy[[2]]]^3;
 
 d = Sqrt[dx^2 + dy^2];

 xy + {dx, dy}*s/d
];

points = Table[
 NestList[
  nextPoint[#] &,
  RandomReal[{-4, 4}, 2],
  m
 ],
 n
];

ListLinePlot[
 points,
 AspectRatio -> 1,
 Axes -> False,
 ImagePadding -> 5,
 ImageSize -> {800, 800},
 PlotStyle -> Directive[{Black, Thickness[0.00025]}]
]
```

## Vector Flow Field 003

Trajectories of particles moving across a vector flow field, starting from 10,000 points on the perimeter of the unit circle.

!["Vector Flow Field 003](https://raw.githubusercontent.com/marcusvolz/mathematica/main/plots/vector-flow-field-003.png "Vector Flow Field 003")

```mathematica
n = 10000;
m = 50;
s = 0.025;
c = {4.5, -1.2, 5.4, 9.1, -7.1, -0.9};

nextPoint = Function[xy,
 dx = c[[1]]*xy[[1]] + c[[2]]*Tan[c[[3]]*xy[[2]]];
 dy = c[[4]]*xy[[2]] + c[[5]]*Tan[c[[6]]*xy[[1]]];
 
 d = Sqrt[dx^2 + dy^2];
 
 xy + {dx, dy}*s/d
];

points = Table[
 NestList[
  nextPoint[#] &,
  {Cos[i/n*2*Pi], Sin[i/n*2*Pi]},
  m
 ],
 {i, 1, n}
];

ymax = Max[points];

ListLinePlot[
 points,
 AspectRatio -> 1,
 Axes -> False,
 ImagePadding -> 5,
 ImageSize -> {800, 800},
 PlotRange -> {{-ymax, ymax}, {-ymax, ymax}},
 PlotStyle -> Directive[{Black, Thickness[0.00006]}]
]
```

## Vector Flow Field 002

Trajectories of particles moving across a vector flow field.

![Vector Flow Field 002](https://raw.githubusercontent.com/marcusvolz/mathematica/main/plots/vector-flow-field-002.png "Vector Flow Field 002")

```mathematica
SeedRandom[865];
n = 10000;
m = 200;
s = 0.05;

nextPoint = Function[xy,
 dx = -(5*Sin[0.1*xy[[2]]*xy[[2]]*xy[[1]]])^3;
 dy = -(5*Cos[0.1*xy[[1]]*xy[[1]]*xy[[2]]])^3;
 
 d = Sqrt[dx^2 + dy^2];
 
 xy + {dy, dx}*s/d
];

points = Table[
 NestList[
  nextPoint[#] &,
  RandomReal[{-4, 4}, 2],
  m
 ],
 n
];

ListLinePlot[
 points,
 AspectRatio -> 1,
 Axes -> False,
 ImageSize -> {800, 800},
 PlotStyle -> Directive[{Black, Thickness[0.00006]}]
];
```

## Vector Flow Field

Trajectories of particles moving across a vector flow field defined by equations with coefficients c and powers p.

![Vector Flow Field](https://raw.githubusercontent.com/marcusvolz/mathematica/main/plots/vector-flow-field.png "Vector Flow Field")

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

![Weiszfeld's Algorithm](https://raw.githubusercontent.com/marcusvolz/mathematica/main/plots/weiszfeld.png "Weiszfeld's Algorithm")

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
