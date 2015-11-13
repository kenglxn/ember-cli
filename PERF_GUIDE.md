# Build performance

When we talk about build performance, it is important to understand that there
are server build types:

+ cold build (booting your app up for the first time)
+ warm build (booting your app up when cache was populated)
+ rebuild (subsequent rebuilds that happen on file change)

Make sure to mention which type of the build seems to be the problem so we can 
help identify and fix issues faster.

## Tools

### DEBUG

Quick, high-level look. Good for finding low hanging fruit.

+ `DEBUG=broccoli-funnel:* ember s`
+ `DEBUG=broccoli-funnel:Funnel*Addon* ember s`
+ `DEBUG=broccoli-merge-trees:TreeMerger* ember s`
+ `DEBUG=broccoli-merge-trees:Addon* ember s`
+ `DEBUG=broccoli-merge-trees:styles ember s`
+ `DEBUG=broccoli-merge-trees:compileTemplates* ember s`

### `broccoli-viz`

#### Visualization

To visualize build tree, we use [graphviz](http://www.graphviz.org/). To install it run `brew install graphviz` or download it directly from [here](http://www.graphviz.org/Download.php).

To generate visualization:

+ `BROCCOLI_VIZ=true ember build`
+ `dot -Tpng grap.<version>.dot > out.png` (each build, will generate an additional graph.<build-number>.dot  graph.<build-number>.json)

#### in-depth look

in-depth tooling, aimed to provide much deeper insight into the given build

+ `dot`: is the input to graphviz, allowing tree visualization
+ `json`: more detailed counts and timings related to the corresponding build

## FAQ/Common Issues & Solutions:

##### Q

npm v3 made my build slow (changed the module topo, accidentaly increasing the number of input files to the vendor tree)

##### A

`DEBUG=broccoli-funnel:Funnel*Addon* ember s` to find it. Solution in many cases, is to look at offending plugins that accidentally include too much.
One such plugin is `ember-cli-ic-ajax`, which has been fixed. So please be sure to upgrade.
