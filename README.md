# mathematica

Artistic visualizations created with Mathematica and the Wolfram Language

## Weiszfeld's Algorithm

Iterative paths converging to the geometric median of 20 terminals using [Weiszfeld's algorithm](https://en.wikipedia.org/wiki/Geometric_median), starting from 10,000 random initial locations.

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

![facets](https://raw.githubusercontent.com/marcusvolz/mathematica/main/plots/weiszfeld.png "Weiszfeld's Algorithm")
