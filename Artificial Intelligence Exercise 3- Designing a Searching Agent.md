# **<u>Artificial Intelligence Exercise 3: Designing a Searching Agent</u>** 

Musyimi John Kioko- 189983 

Kuria Jason Wainaina- 189946 

Wangui Michelle Wachanga- 176833 

## **<u>1. Designing the Self-Driving Trailer Agent</u>** 

## **<u>1.1 PEAS Framework</u>** 

PEAS stands for Performance measure, Environment, Actuators and Sensors. It describes the 

tasks an intelligent agent performs and the conditions under which it operates. 

|**Component**|**Description**|
|---|---|
|Performance|Minimize total road distance and travel time, avoid collisions, obey traffic|
|Measure|laws, reduce fuel consumption, protect cargo and deliver it safely to Kisumu.|
|Environment|Kenyan highways, towns, junctions, roundabouts, bridges, road signs,<br>traffic, pedestrians, other vehicles, weather and road conditions.|
|Actuators|Steering, accelerator, brakes, gear control, indicators, headlights, horn and<br>communication systems.|
|Sensors|GPS, cameras, radar, lidar, wheel-speed sensors, odometry, engine sensors,<br>digital maps and traffic-information systems.|



The agent receives information from its sensors, evaluates available actions and controls the trailer's movement. It uses a road-network graph to select a route and navigation information to follow that route safely. 

## **<u>1.2 Defining the Agent Environment</u>** 

The self-driving trailer environment can be classified using five standard dimensions. 

**Dimension Classification Explanation** 

|Observability|Partially<br>observable|Sensors cannot reveal every relevant condition. Blind<br>spots, hidden hazards and unexpected events may limit<br>available information.|
|---|---|---|
|Determinism|Stochastic|Traffic, weather, other drivers and road incidents make<br>future outcomes uncertain.|
|Task structure|Sequential|Each steering, braking and routing decision affects later<br>states and decisions.|
|Environment<br>changes|Dynamic|Traffic, pedestrians, road conditions and weather can<br>change while the agent operates.|
|State and action<br>space|<br>Hybrid|Towns and road junctions can be represented discretely,<br>while speed, time, position and steering involve continuous<br>values.|



**Conclusion:** The self-driving trailer operates in a partially observable, stochastic, sequential, dynamic and hybrid environment. It requires continuous sensing, route planning, decisionmaking and control. 

## **<u>2. Directed Graph and Search Tree</u>** 

## **<u>2.1 Problem Representation</u>** 

The search problem is defined as follows: 

- **Initial state:** Nairobi. 

- **Goal state:** Kisumu. 

- **States:** Towns and cities represented as nodes in the road-network graph. 

- **Actions:** Travel along an available directed road edge to an adjacent town. 

- **Transition model:** Travelling along an edge changes the agent's current location to the connected town. 

- **Path cost:** The sum of the road distances travelled, measured in kilometres. 

- **Goal test:** Determine whether the current state is Kisumu. 

The graph is weighted because each road edge has an associated distance. It is directed because each arrow represents a permitted transition in the indicated direction. A reverse edge should be included only when the corresponding road connection is valid, with its own verified distance. 

## **<u>2.2 Abstract Directed Graph</u>** 

The following simplified graph illustrates the main corridors considered in the search. It is an abstract planning model, not a complete representation of every Kenyan highway. 



<!-- Start of picture text -->
s 215 km 100 km<br>se (os) o=—a<br>Zoe 2a 102 km<br>89 km 406 km 71km 56 kay<br>91 km 04km 73 km (a)<br>58km/ Tf cokm 193 km 91 km 52 km<br>47k,<br>One 36 km S<br>50km \ oo ae<br>38km<br><!-- End of picture text -->

**_Figure 1: Simplified directed graph of selected Kenyan highway connections._** 

Each node represents a town or city, and each directed arrow represents a possible road transition. The edge weights represent road distances in kilometres. 

The graph is a simplified model of selected Kenyan highway connections, not a complete representation of the country's road network. Before calculating the optimal route, every edge used in the calculation must be checked against Google Maps to confirm that the road connection and its distance are valid. 

## **<u>2.3 Abstraction in the Search Tree</u>** 

Abstraction reduces the complexity of the problem by representing the journey as a sequence of important towns rather than individual road junctions, bends and metres of road. 

For example, the agent does not need to represent every junction between Nairobi and Naivasha as a separate search state. It can treat the journey between the two towns as one transition with an associated distance. 

The following tree illustrates alternative high-level routes. 



<!-- Start of picture text -->
Nairobi<br>(Start)<br>N a hi Central Highlands<br>ses) aed<br>=<br>Ceres)<br><a<br>‘cs Kisumu<br>bis Kisumu (Goal)<br>(Goal)<br><!-- End of picture text -->

**_Figure 2: Abstract search tree showing alternative route branches._** 

The search tree illustrates alternative high-level routes from Nairobi towards Kisumu. Each branch represents a possible sequence of states explored by the agent. The tree is an abstract representation of route exploration and does not, by itself, establish which route is shortest. 

A search tree differs from the road graph because it represents paths explored from the initial state. The same town may appear more than once if it is reached through different paths. Before a branch is included in the final route comparison, its road connections and cumulative distance must be verified. 

## **<u>2.4 Search Strategy</u>** 

Dijkstra's algorithm is suitable for finding the minimum-distance route because the road distances represented by the graph are non-negative. 

The algorithm operates as follows: 

1. Assign Nairobi a cumulative cost of zero. 

2. Assign all other towns an initial cost of infinity. 

3. Select the unvisited town with the smallest known cumulative cost. 

4. Examine its outgoing edges and calculate the new cumulative cost of reaching each neighbouring town. 

5. Update a neighbour's cost and predecessor whenever a shorter path is found. 

6. Repeat the process until Kisumu is selected as the unvisited town with the smallest cumulative cost, or until no reachable unvisited towns remain. 

7. Trace the predecessor links backwards from Kisumu to reconstruct the minimum-cost route. 

Unlike breadth-first search, Dijkstra's algorithm accounts for the weights of road segments rather than treating every connection as having equal cost. Its result is only as reliable as the road connections and distances entered the graph. 

## **<u>3. Determining the Optimal Route from Nairobi to Kisumu</u>** 

## **<u>3.1 Path Cost Function</u>** 

The cost of a route is the sum of the road distances between consecutive towns. 

## _[_ 

_C(P)=\sum_{i=0}^{n-1}d(v_i,v_{i+1})_ 

_]_ 

_Where:_ 

- _(P) is a candidate path._ 

- _(v_i) is a town on the path._ 

- _(d(v_i,v_{i+1})) is the road distance between consecutive towns._ 

- _(C(P)) is the total route distance in kilometres._ 

_The optimal path is the valid path with the lowest total cost:_ 

_P_ *= _arg minP ∈ P_<sup>_C_(</sup><sup>_P_)Here, (\mathcal{P}) represents the set of valid paths from Nairobi to</sup> 

Kisumu. The optimal path can be identified only after the valid candidate routes have been evaluated using verified edge distances. 

## **<u>3.2 Preliminary Distance Calculations</u>** 

The following values are preliminary road-distance estimates from the references listed in Section 5. They must be checked against Google Maps using consistent starting and ending locations before they are treated as final values. 

|**Road segment**|**Preliminary distance**|
|---|---|
|Nairobi to Naivasha|91 km|
|Naivasha to Nakuru|71 km|
|Nakuru to Kericho|106 km|
|Kericho to Kisumu|89 km|
|**Total for Candidate Route A **|**357 km**|



For Candidate Route A, the estimated path cost is: 

_[_ 

_C(P_A)=91+71+106+89=357\text{_ 

_km}_ 

_]_ 

Therefore, the preliminary estimated cost of Candidate Route A is 357 km. 

However, this result does not establish that Candidate Route A is optimal. The directed graph also shows a Nairobi–Kericho connection labelled 193 km. That connection and its direction must be verified against Google Maps. If it is a valid edge, the route through Kericho must be included in the candidate-route comparison using the correct directional distances. 

All distances used in the final comparison must be obtained consistently, using the same method and clearly identified locations. The final shortest route should be reported only after the candidate routes have been checked and compared. 

## **<u>3.3 Comparing Alternative Paths</u>** 

The agent compares the cumulative road distances of alternative routes from Nairobi to Kisumu to identify the route with the lowest total path cost. Each route must consist of valid road connections, and all distances must be obtained using a consistent method. 

The following table presents the preliminary calculations based on the estimates currently available. 

|**Candidate **|**Route**|**Distance**<br>**calculation**|**Preliminary**<br>**total**|
|---|---|---|---|
|A|Nairobi → Naivasha → Nakuru → Kericho<br>→ Kisumu|<br>91 + 71 + 106 + 89|357 km|
|B|Nairobi → Kericho → Kisumu|275 + 89|364 km|
|C|Nairobi → Nakuru → Eldoret → Kisumu|157 + 156.3 + 117|430.3 km|



## **Candidate A** 

[ C(P_A)=91+71+106+89=357\text{ ] 

km} 

## **Candidate B** 

[ C(P_B)=275+89=364\text{ km} ] 

## **Candidate C** 

[ 

C(P_C)=157+156.3+117=430.3\text{ km} ] 

Based on these preliminary figures, Candidate A has the lowest calculated distance at 357 km, followed by Candidate B at 364 km and Candidate C at 430.3 km. 

However, these totals are preliminary estimates and may use different routing methods or town reference points. In addition, the Nairobi–Kericho connection in the directed graph is labelled 

193 km, whereas Candidate B uses 275 km for the same segment. This discrepancy must be resolved before the comparison can be considered reliable. 

## **<u>3.4 Conclusion</u>** 

The self-driving trailer can be modelled as a goal-based intelligent agent that searches a weighted, directed graph representing selected connections in Kenya's highway network. The PEAS framework describes its performance measures, environment, actuators and sensors, while the five environment dimensions explain the complexity and uncertainty of its operating conditions. 

Dijkstra's algorithm is suitable for finding the minimum-distance route when all edge costs are non-negative. Based on the preliminary distances currently used, Candidate Route A has an estimated total cost of 357 km. However, this calculation does not establish that the route is optimal because the road connections and distances have not yet been fully verified. 

The graph and candidate-route calculations must be made consistent, and the alternative routes must be compared using verified distances before the shortest route from Nairobi to Kisumu can be confirmed. 

## **<u>4. Groupwork Evidence</u>** 

## **<u>4.1 Group Members and Contributions</u>** 

|No.|Full Name||Admission<br>Number|Contribution|
|---|---|---|---|---|
|1|Musyimi<br>Kioko|John|<br>189983|Coordinated the assignment and prepared the PEAS<br>framework, agent environment classification, and overall<br>Markdown documentation.|
|2|Kuria<br>Wainaina|Jason|<br>189946|Researched Kenya's highway network, identified relevant<br>towns and road connections, and developed the directed<br>graph and abstract search tree.|
||Wangui|||Worked on the road-distance calculations, comparison of|
|3|Michelle||176833|candidate routes, shortest-path analysis, and compilation|
||Wachanga|||of references.|



**<u>4.2 Group Discussion and Meeting Minutes</u>** 

**Agenda** : Designing a searching agent for a self-driving trailer travelling from Nairobi to Kisumu. 

Discussion points: 

1. Discussed the PEAS framework and identified the performance measures, environment, actuators, and sensors of the self-driving trailer. 

2. Classified the agent's environment using the five standard dimensions. 

3. Examined Kenya's highway network and identified towns and road connections relevant to the journey. 

4. Discussed graph representation, abstraction, search trees, and the use of Dijkstra's algorithm to determine the minimum-distance route. 

5. Reviewed the road-distance calculations and organised the findings into a Markdown document for GitHub submission. 

**Outcome:** The group organised the assignment into the PEAS analysis, environment classification, highway graph, abstract search tree, and shortest-path calculation. The final submission will be maintained in the PS-Agents GitHub repository in a file named design.md. 

## **<u>5. References</u>** 

1. Google Maps. (n.d.). _Google Maps: Road directions and driving distances in Kenya_ . <u>https://www.google.com/maps/</u> 

2. Russell, S. J., & Norvig, P. (2021). _Artificial Intelligence: A Modern Approach_ (4th ed.). Pearson. 

3. DistancesFrom.com. (n.d.). _Directions from Nairobi to Naivasha_ . <u>https://www.distancesfrom.com/ke/directions-from-Nairobi-to-Naivasha/ DirectionHistory/133527.aspx</u> 

4. DistancesFrom.com. (n.d.). _Directions from Naivasha to Nakuru_ . <u>https://www.distancesfrom.com/ke/directions-from-naivasha-to-Nakuru/ DirectionHistory/400810.aspx</u> 

5. DistancesFrom.com. (n.d.). _Directions from Nakuru to Kericho, Kenya_ . <u>https://www.distancesfrom.com/ke/directions-from-Nakuru-to-Kericho-Kenya/ DirectionHistory/3342061.aspx</u> 

6. DistancesFrom.com. (n.d.). _Directions from Kericho to Kisumu, Kenya_ . <u>https://www.distancesfrom.com/ke/directions-from-Kericho-to-Kisumu-Kenya/ DirectionHistory/6057885.aspx</u> 

7. DistancesFrom.com. (n.d.). _Directions from Nairobi to Kericho._ <u>https://www.distancesfrom.com/ke/directions-from-Nairobi-to-Kericho/ DirectionHistory/2389414.aspx</u> 

8. DistancesFrom.com. (n.d.). _Directions from Nairobi to Nakuru._ 

<u>https://www.distancesfrom.com/ke/directions-from-Nairobi-to-Nakuru/ DirectionHistory/1981382.aspx</u> 

9. DistancesFrom.com. (n.d.). _157 Km - Distance from Nakuru to Eldoret._ <u>https://www.distancesfrom.com/ke/distance-from-Nakuru-to-Eldoret/DistanceHistory/ 105782.aspx</u> 

10. DistancesFrom.com. (n.d.). _Directions from Eldoret Town to Kisumu._ 

<u>https://www.distancesfrom.com/ke/directions-from-eldoret-town-to-Kisumu-Kenya/ DirectionHistory/10602495.aspx</u> 

