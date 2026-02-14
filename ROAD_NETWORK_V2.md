# Road Network V2 Blueprint

## Goal
Move from spline-only track editing to a graph-based road system that can scale from city streets to highway interchanges, including elevated roads with automatic structural supports.

## Baseline Requirements
1. Network graph, not single ribbon:
- Nodes: intersections, merges/diverges, roundabouts, ramp terminals, grade-separated crossings.
- Edges: directed road segments with lane metadata and design class.

2. Road hierarchy (SimCity-style, but engineering-backed):
- `local` (access priority)
- `collector`
- `arterial`
- `freeway`

3. Junction strategy:
- Use an Intersection Control Evaluation-style approach to pick control type by demand/speeds/space.
- Supported controls: stop/signal, roundabout, RCUT/MUT/DLT, DDI/ramp terminal variants.

4. Geometry rules:
- Horizontal continuity via clothoid transitions.
- Vertical alignment with grade + vertical curves (crest/sag behavior).
- Class-based limits for min radius, max grade, lane count and lane width targets.

5. Grade separation:
- Overpasses/bridges with explicit deck clearance targets.
- Automatic substructure generation (piers/caps) when deck-to-ground gap exceeds threshold.

6. Lane connectivity:
- Explicit incoming-lane -> outgoing-lane mapping at each node.
- Turn restrictions and lane use control (through-only, left-only, right-only, ramp-only).

## Data Model (Target)
```txt
RoadNetwork
  nodes: Node[]
  edges: Edge[]
  junctions: Junction[]

Node
  id, pos3
  type: normal|merge|diverge|roundabout|interchange

Edge
  id, fromNode, toNode
  class: local|collector|arterial|freeway|ramp
  centerline: parametric alignment
  profile: vertical profile
  lanesForward, lanesReverse
  speedTarget

Junction
  id, nodeIds
  controlType: stop|signal|roundabout|rcut|mut|dlt|ddi
  laneConnections: [fromLane -> toLane]
  conflictZones
```

## Generation Pipeline
1. Topology pass:
- Build/validate graph, detect disconnected components, ensure lane continuity.

2. Geometric pass:
- Solve centerlines with clothoids and vertical curves.
- Resolve junction throat geometry and lane tapers.

3. Surface pass:
- Build road/shoulder/curb meshes per edge and per junction patch.

4. Structure pass:
- Build supports for elevated sections against terrain and underpasses.

5. Traffic metadata pass:
- Create turn restrictions, priorities, and lane-level routing graph.

## Implementation Stages
1. Stage A (current focus):
- Keep existing UI/tools.
- Treat main spline + branch sections as a network.
- Generate all theme meshes per branch + main.
- Add automatic support mesh generation from terrain gap.

2. Stage B:
- Introduce road classes and per-class defaults.
- Add lane count + directionality per edge.
- Add lane connection model at Y-splits/roundabouts.

3. Stage C:
- Add interchange node templates (diamond, partial cloverleaf, trumpet, stack-lite).
- Add ICE-style junction chooser helper.

4. Stage D:
- Add routing/traffic simulation hooks.

## Current Implementation Snapshot (2026-02-13)
- Stage A complete in this repo: network theme continuity + automatic supports for elevated segments.
- Stage B implemented in this repo:
- Road class/lane/direction defaults, per-branch edge overrides, and class-aware width scaling.
- Export descriptor now includes directed edges, node/junction typing hints, and explicit lane-connection mappings (`fromLane -> toLane`) at network nodes (including Y-splits/joins).
- 2D editor now draws lane-direction arrows and per-edge lane labels for both main and branch edges.
- Stage C partial in this repo:
- Added interchange generator templates (`diamond`, `partialClover`, `trumpet`, `stackLite`) with automatic ramp branch plans.
- Added ICE-style control chooser metadata per junction (`stop`, `signal`, `roundabout`, `rcut`, `mut`, `dlt`, `ddi`, `ramp_terminal`) plus node-level rationale/metrics.
- Added 2D junction control badges and topology issue highlighting for quick network QA.
- Stage D partial in this repo:
- Added routing hooks in network descriptor (`routing.directedEdges`, turn transitions, adjacency, movement penalties) for traffic/sim integration.
- Remaining work:
- Full clothoid + vertical-curve solver as primary centerline generator (current pipeline still starts from Bezier control curves with sanitization).
- Interchange template authoring UI (currently available through generator style presets, not dedicated edit widgets).
- Full traffic simulation runtime over routing hooks.

## Standards Anchors Used
- FHWA roundabouts: speed reduction + yield-controlled circular flow + safety/conflict reduction.
- FHWA intersection control evaluation and innovative intersection families (RCUT, MUT, DLT, DDI).
- TxDOT roadway design criteria for hierarchy concepts and vertical clearance targets.
- SUMO network model concepts (nodes/edges/lanes/connections/roundabout declarations) for lane-connectivity structure.
