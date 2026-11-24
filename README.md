# Polar Energy Management System

An interactive, browser-based architecture diagram for a **polar / remote-site energy management system**. The visual walks through how weather and SCADA telemetry could feed a forecast, dispatch optimization, safety checks, subsystem control, and operational reporting.

> **Prototype notice:** This repository contains a static visualization only. It does not connect to sensors, run forecasting or optimization models, issue control commands, or provide operational safety assurances. The system shown is an architectural concept—not a deployed controller.

## At a glance

- One self-contained `index.html` with embedded HTML, CSS, JavaScript, and SVG
- Animated flow paths between architecture components
- Hover or keyboard-focus a component to highlight its connected paths
- Toggle flow animation on or off
- Light and dark themes; the theme follows the system preference by default and remembers a manual choice in browser storage
- Responsive layout with a horizontally scrollable diagram on smaller screens
- Respects the browser's reduced-motion preference
- No build step, package installation, backend, or external JavaScript dependency

## Architecture shown

The diagram is organized into five stages:

| Stage | Role | Example components in the diagram |
|---|---|---|
| **1. Polar Telemetry & Multi-Modal Forecast** | Collect weather and SCADA inputs, prepare features, and produce forecast quantiles | Temperature, wind, solar, battery state of charge; normalization; Gradient Boosting forecast (P10 / P50 / P90) |
| **2. Multi-Timeframe Dispatch Engine** | Choose a dispatch plan and handle an infeasible optimization result | MILP unit commitment with a HiGHS solver; continuous LP fallback / relaxation |
| **3. Safety Interlock & Fallback Matrix** | Check operational limits before dispatch; use a deterministic fallback if validation fails | C-rate limits, minimum battery SOC, Tier-1 load protection, priority load shedding, dump load |
| **4. Multi-Subsystem Energy Dispatch** | Route a validated plan to energy assets | LFP battery, supercapacitor, diesel, thermal CHP, snow-melter, hydrogen fuel cell and electrolyzer |
| **5. Real-Time SCADA & Audit Reporting** | Surface system state and operational reporting | Dashboard, automated audit/report pipeline, fuel-savings and CO₂ telemetry |

### Decision flow

1. Weather and SCADA telemetry is normalized and sent to the forecast stage.
2. The dispatch engine attempts a mixed-integer unit-commitment plan.
3. If the MILP is infeasible, the diagram routes the plan through a continuous LP optimizer.
4. A safety interlock validates either plan. A passing plan proceeds to energy dispatch; a failed check routes to a deterministic fallback controller.
5. The diagram connects dispatch to the SCADA dashboard, auxiliary energy subsystems, and reporting paths.

The diagram describes an intended architecture. It does not implement these algorithms or verify their engineering suitability.

## View it locally

Clone the repository and open `index.html` in a modern browser:

```bash
git clone https://github.com/Mohitmourya22/flow.git
cd flow
```

Then double-click `index.html`, or serve the folder locally:

```bash
python -m http.server 8000
```

Visit <http://localhost:8000>.

## Publish with GitHub Pages

This is a static site and can be hosted with GitHub Pages:

1. Open the repository's **Settings → Pages**.
2. Select **Deploy from a branch**.
3. Choose the `main` branch and `/ (root)` folder, then save.

Once Pages finishes publishing, use the URL shown in the Pages settings.

## Project structure

```text
flow/
├── index.html   # Complete interactive SVG architecture and UI behavior
└── README.md    # Project overview and usage guide
```

## Built with

- HTML for page structure
- CSS for themes, layout, node/stage styling, and animation
- Vanilla JavaScript for SVG rendering, interactions, and theme/animation controls
- Inline SVG for the architecture diagram, animated connectors, and decision nodes

## Accessibility & motion

Architecture nodes are keyboard-focusable, have accessible labels, and highlight their connected edges on focus as well as hover. Visible focus styling is included. The flow animation can be paused, and the page disables animation by default when the operating system requests reduced motion.

## Contributing

Ideas and improvements are welcome. For substantial changes, please open an issue first so the diagram's scope and assumptions can be discussed. Pull requests should keep the visualization usable on smaller screens and preserve its reduced-motion behavior.
