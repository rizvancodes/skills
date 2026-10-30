# Diagrams

Draw standard diagrams in Mermaid and share them as images, readable on a phone. Use tables where a
table is clearer than a picture.

| Step | Show it as | Convention |
| --- | --- | --- |
| 1. Context | System Context diagram: the product as one box, its people and outside systems | [C4](https://c4model.com), level 1 |
| 2. Spine | Sequence diagram: who sends what to whom, in time order | UML sequence diagram |
| 3. What gets deployed | Container diagram: each deployable app and data store, and how they talk; plus a table | C4, level 2 |
| 4. Capability map | Tiers as rows: product capabilities, shared foundations, platform services, and the bought systems behind them, with arrows only pointing down; plus the capabilities × apps matrix as a table | Layered architecture |
| 5. Pressure test | Verdict tables; future features walked through the map to show where they land | |
| 6. Ownership | Context map: capabilities and who hands what to whom | Domain-driven design |
| 7. Data models | ER diagram | Crow's-foot notation |
| Lifecycles, wherever they appear | State diagram: every state and every transition | UML state machine |
| A slice being shaped | Component diagram, one capability at a time: its folders, layers and the platform services it calls | C4, level 3 |

Platform services live inside apps, so they appear as components (level 3), not containers. The
systems behind them appear in the context and container diagrams.

Leave out call stacks, class diagrams and C4's code level. They describe code, and at this stage
there is none.

## Mermaid tips

- Mermaid's C4 syntax is experimental. A flowchart styled the C4 way (people, the system, outside
  systems; containers inside a boundary) reads just as well.
- Draw each tier as a subgraph. Mermaid often ignores a subgraph's `direction` when arrows cross
  its edge, so if the tiers come out as one long column, flip the outer direction and render again.
- Keep each diagram small enough to read on a phone. Split a wide one, for example one component
  diagram per capability.
- Render and look at each image before sharing it. If the layout crosses lines or runs off the
  screen, redraw it.
