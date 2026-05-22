---
name: topo-page
description: Generate an interactive SVG topology diagram with dagre.js layout engine as a single-file HTML fragment. Produces colored node boxes by category, typed edge paths (solid/dashed/color-coded), edge labels with collision avoidance, stub dangling arrows, and hover tooltips. Use when the user wants to create an architecture topology, flow diagram, pipeline topology, or system flow chart. Triggers: "拓扑图", "架构拓扑", "topo page", "topology page", "场景拓扑", "流程拓扑图", "dagre拓扑图".
---

# Topo Page

Generate an interactive SVG topology diagram powered by dagre.js. A single-file HTML fragment that renders colored node boxes by category, typed edge paths, edge labels with collision avoidance, stub dangling arrows, and hover tooltips.

## Visual System

Nodes and edges are styled by their `category` and `type` fields via CSS classes. **Define your own categories and edge types** based on the domain — the skill provides the rendering mechanism, not the domain classification.

### Node Categories

Each `category` maps to a CSS class `c-xxx` that controls rect fill/stroke and text fill. Define as many categories as needed. Example pattern:

```css
.c-gateway rect { fill: #ecfeff; stroke: #06b6d4; stroke-width: 1.5; }
.c-gateway text { fill: #0e7490; }
```

Guidelines:
- Use pale tinted fills (`#fXXXXX` range) for rects, saturated strokes for borders
- Text fill matches the stroke color but darker
- All rects get `rx: 6; ry: 6` via the base `.node-box rect` rule
- One category can have a taller node (e.g. 44px vs 40px default) if visually important

### Edge Types

Each `type` maps to a CSS class `arrow-xxx` that controls stroke color, width, and dash pattern. Define as many types as needed. The base class `arrow-line` is always applied; the type class overrides stroke properties. Example:

```css
.arrow-line   { fill: none; stroke: #78716c; stroke-width: 1.8; }
.arrow-data   { fill: none; stroke: #15803d; stroke-width: 2.2; }           /* solid */
.arrow-signal { fill: none; stroke: #a16207; stroke-width: 1.8; stroke-dasharray: 6 3; }
.arrow-cmd    { fill: none; stroke: #c2410c; stroke-width: 1.8; stroke-dasharray: 4 2; }
```

Guidelines:
- Use solid lines for primary/data flows, dashed lines for secondary/control flows
- Primary flow edges get slightly wider stroke (2.2) than control edges (1.8)
- Edge labels use `.arrow-label` (muted gray, 11px) with `.arrow-label-bg` capsule background

## Data Format

Define scenarios as a JS object keyed by scenario id:

```js
const SCENARIOS = {
  s0: {
    id: 's0',
    nodes: [
      { id: 's0_user',   label: '用户',  category: 'c-external',
        layer: 0, fontSize: 14, fontWeight: 700,
        tip: '终端用户', repo: '' },
      { id: 's0_gw',     label: 'Gateway', category: 'c-gateway',
        layer: 1, fontSize: 13, fontWeight: 600,
        tip: '请求接入网关', repo: 'org/gateway-service' },
    ],
    edges: [
      { from: 's0_user', to: 's0_gw', type: 'arrow-signal',
        label: '请求', labelColor: '#a16207' },
    ]
  }
};
```

**Node fields**:
- `id` — unique within the scenario, prefixed with scenario id
- `label` — display text inside the rect
- `category` — CSS class name (e.g. `c-gateway`)
- `fontSize`, `fontWeight` — font properties for the label text
- `tip` — tooltip description text (shown on hover)
- `repo` — repository path or source reference (shown in tooltip, empty string if none)
- `layer` — **optional**, informs vertical intent but dagre assigns ranks independently
- `col` — **optional**, ignored by dagre (handles horizontal ordering)

**Edge fields**:
- `from`, `to` — source/target node ids
- `type` — CSS class name for stroke style (e.g. `arrow-data`)
- `label` — edge label text (null for no label)
- `labelColor` — CSS color for the label text
- **Stub edges** (dangling arrows): `to: null` with optional `stubLength` (default 50px) and `stubDirection` (`down|up|right|left`). Use when the target is unknown, dynamic, or intentionally omitted:
  - **调度分配**：节点发出分配指令，具体目标由运行时决定
  - **广播通知**：节点广播，接收方不确定
  - **概要省略**：下游链路属于另一个子系统或太长，此处仅示意"还有后续"

## Rendering Pipeline

### Dependencies
Load **dagre.js v0.8.5** via CDN before the inline script:
```html
<script src="https://cdn.jsdelivr.net/npm/dagre@0.8.5/dist/dagre.min.js"></script>
```

### Function Chain

1. **`buildDagreGraph(scenario)`** — Core layout engine:
   - Creates `dagre.graphlib.Graph` with `rankdir: 'TB'`, `network-simplex` ranker (default), `ranksep/nodesep/edgesep/marginx/marginy` config
   - Measures node widths via hidden SVG `<text>`, adds nodes with `{ width, height }`
   - Adds edges with `{ label, width, height }` for label layout; stub edges (`to: null`) collected separately
   - Runs `dagre.layout(g)`, extracts node center positions → `{ x, y, width, height, cx, cy }`
   - Extracts edge waypoints + label positions; returns `{ positions, dagreEdges, stubEdges, svgWidth, svgHeight }`

2. **`buildSvgPathFromDagrePoints(points)`** — Smooth SVG path from dagre waypoints:
   - 2 points → vertical cubic Bezier
   - 3+ points → Catmull-Rom → Bezier conversion (tension 1/6), curves pass through all waypoints

3. **`getStubPorts(e, positions)`** — Stub edge port calculation for dangling arrows (exits from node border, extends by `stubLength` in `stubDirection`)

4. **`resolveLabelCollisions(labelInfos)`** — Iterative pairwise relaxation (MIN_GAP=10, LABEL_H=22, MAX_ITER=24)

5. **`renderNode(node, pos)`** — Creates `<g class="node-box {category}">` with `<rect>` + `<text>`, sets `data-tip` and `data-repo` attributes for tooltips

6. **`renderScenario(scenario, container)`** — 4-pass orchestration:
   - Pass 0: `buildDagreGraph(scenario)`
   - Pass 1: Generate path data from dagre edges + stub edges
   - Pass 2: Compute label positions (dagre label center or waypoint midpoint) + `resolveLabelCollisions`
   - Pass 3: Render SVG DOM in z-order: edges → labels (bg capsule + text) → nodes

### Arrow Markers
`createDefs(prefix)` creates SVG `<marker>` arrowheads for each edge type plus a default, with `orient: 'auto'`, `markerWidth: 4`, `markerHeight: 2.8`, `refX: 3.5`. Each scenario gets a unique prefix to avoid ID collisions. Arrowhead fill color matches the edge type's stroke color.

## Interactive Features

**Tooltip**: Event delegation on the SVG container. `mouseover` on `.node-box[data-tip]` shows a fixed-position `<div>` with tip title, repo path, and description. `mouseout` hides it.

**Tab switching** (optional): `.tab` buttons toggle `.scenario` panels for multi-scenario views.

## Hard Constraints

1. **dagre.js layout engine** with `network-simplex` ranker. No manual layer/col positioning.
2. **z-order**: edges (bottom) → labels (middle) → nodes (top), achieved by DOM append order.
3. **Catmull-Rom smooth paths** through dagre waypoints. No polyline/orthogonal segments.
4. **Real data** — no placeholder labels or fake nodes. Fill in actual service names, descriptions, and repos.
5. **CJK-first** — Chinese labels/descriptions preferred.
6. **No max-width** on the topology container — SVG uses full available width.
7. **Tooltip** displays `tip` description and `repo` path from node data.
8. **Single-file HTML** — all CSS and JS inline, only dagre.js as external CDN dependency.

## Reference

See `example.html` in this skill directory for a minimal working topology with 3 scenarios covering forward edges, back-edges, and stub edges.