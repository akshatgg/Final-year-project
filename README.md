# Traveling the Orbits — Interactive 3D Solar System Simulator

> **Live Demo**: [https://akshatgg.github.io/OrreyApp/](https://akshatgg.github.io/OrreyApp/)

An interactive 3D orbital mechanics simulator built with Three.js that allows users to explore space travel, satellite trajectories, and orbital dynamics. The project features a comprehensive solar system simulation with 13 celestial bodies, gravity assist maneuvers, real-time gravitational field visualization, and an AI-powered space assistant chatbot enhanced with Retrieval-Augmented Generation (RAG) from the *Orbital Mechanics for Engineering Students* textbook by Howard D. Curtis.

---

## Table of Contents

- [Features](#features)
- [Technology Stack](#technology-stack)
- [Project Structure](#project-structure)
- [Getting Started](#getting-started)
- [AI Space Assistant and RAG Pipeline](#ai-space-assistant-and-rag-pipeline)
- [Gravitational Field Visualization](#gravitational-field-visualization)
- [Research Papers and Documentation](#research-papers-and-documentation)
- [CI/CD and Deployment](#cicd-and-deployment)
- [Docker](#docker)
- [Configuration](#configuration)
- [Usage Guide](#usage-guide)
- [Troubleshooting](#troubleshooting)
- [Contributing](#contributing)
- [License](#license)

---

## Features

- **Interactive 3D Solar System** — 13 celestial bodies (Sun, 8 planets, 5 dwarf planets) rendered with high-resolution textures, orbital paths, and CSS2D name labels.
- **Real-Time Orbital Mechanics** — N-body gravitational simulation using Euler integration for satellite trajectory computation with configurable initial velocity.
- **Gravity Assist Scenarios** — 9 pre-configured gravity assist maneuver presets demonstrating real-world space mission techniques.
- **Gravitational Field Visualization** — A 150x150 vertex wireframe grid that deforms in real time to show spacetime curvature around all celestial bodies, with color gradients from dark purple to glowing cyan/white.
- **AI Space Assistant with RAG** — Chatbot powered by Groq or Google Gemini API, augmented with a BM25-indexed knowledge base of 487 textbook chunks for grounded, accurate answers on orbital mechanics.
- **Satellite Simulation** — Launch a 3D GLTF satellite model from Earth, track its trajectory with trail visualization, and switch between first-person and third-person camera views.
- **Dynamic Camera Controls** — Orbit controls for rotation, zoom, and pan; click-to-focus on any planet.
- **Post-Processing Effects** — Unreal Bloom, SSAA super-sampling, and color correction for cinematic visuals.
- **Time Controls** — Adjustable simulation speed and time scaling via a GUI panel.
- **Responsive Dark Theme UI** — Space Mono typography, glow effects, and a floating expandable chat widget.

---

## Technology Stack

| Category | Technologies |
|----------|-------------|
| **3D Graphics** | Three.js v0.157, UnrealBloomPass, EffectComposer, SSAA, CSS2DRenderer |
| **Frontend** | HTML5, CSS3, JavaScript (ES6 Modules) |
| **Animation** | GSAP v3.12.2 (GreenSock) |
| **UI Controls** | dat.GUI v0.7.9 |
| **AI / LLM** | Groq API (default), Google Gemini API (alternative) |
| **RAG Pipeline** | Custom BM25 search engine (K1=1.5, B=0.75), 487 pre-indexed textbook chunks |
| **Build Tool** | Vite v6.0.3 |
| **Containerization** | Docker, Docker Compose |
| **CI/CD** | GitHub Actions with GitHub Pages deployment |
| **Package Manager** | npm (Node.js) |

---

## Project Structure

```
Final-year-project/
├── README.md                              # This file
├── .gitignore                             # Root-level git ignore rules
├── .github/
│   └── workflows/
│       └── deploy.yml                     # GitHub Actions CI/CD pipeline
│
├── Papers/                                # Research papers and project documents
│   ├── Curtis_OrbitamMechForEngineeringStudents.pdf
│   ├── Final_Project_Report.docx
│   ├── ORRERY WEB APP final.pptx
│   ├── paper.pdf
│   ├── ai_plag.pdf
│   └── similarity_plag.pdf
│
└── OrreyApp/                              # Main application source code
    ├── index.html                         # Entry point HTML with embedded styles
    ├── main.js                            # Application bootstrap and animation loop
    ├── styles.css                         # Additional global styles
    ├── vite.config.js                     # Vite build configuration
    ├── package.json                       # Dependencies and scripts
    ├── Dockerfile                         # Container configuration
    ├── docker-compose.yml                 # Docker Compose setup
    ├── .env.example                       # Environment variable template
    │
    ├── solar_system.js                    # Solar system initialization (13 bodies)
    ├── planet.js                          # Planet class with orbital mechanics
    ├── satellite_class.js                 # Satellite physics, GLTF model loading
    ├── basic_orbit.js                     # Elliptical orbit curve generation
    ├── trail.js                           # Trajectory trail visualization
    ├── gravity_field.js                   # Real-time gravitational field grid
    ├── space-agent.js                     # AI assistant with RAG integration
    │
    ├── rag/                               # Retrieval-Augmented Generation system
    │   ├── preprocess.js                  # PDF extraction and chunking script
    │   ├── retriever.js                   # Client-side BM25 search engine
    │   └── knowledge-base.json            # Pre-indexed textbook (487 chunks)
    │
    ├── chatbot/                           # Chatbot UI components
    │   ├── index.js                       # Component orchestration
    │   ├── chatbot.js                     # Core chatbot logic
    │   ├── processing.js                  # Neural network implementation
    │   ├── math.js                        # Utility math functions
    │   ├── components/                    # UI components (ChatWindow, ChatIcon, etc.)
    │   ├── hooks/                         # API integration hooks (Gemini)
    │   └── styles/                        # Chatbot-specific CSS
    │
    └── resources/                         # Static assets
        ├── textures/                      # 24 planet and skybox textures
        └── satellite/                     # 3D GLTF satellite model and textures
```

---

## Getting Started

### Prerequisites

- **Node.js** v14 or higher
- **npm** (bundled with Node.js)
- **Docker** and **Docker Compose** (optional, for containerized deployment)

### Option 1: Local Development

```bash
git clone https://github.com/akshatgg/Final-year-project.git
cd Final-year-project/OrreyApp
npm install
cp .env.example .env        # Add your API keys
npm start                   # Runs at http://localhost:8051
```

### Option 2: Docker

```bash
cd Final-year-project/OrreyApp
docker-compose up           # Runs at http://localhost:8051
```

### Option 3: Production Build

```bash
cd Final-year-project/OrreyApp
npm run build               # Outputs to dist/
```

The `dist/` folder can be deployed to any static hosting provider (Netlify, Vercel, Cloudflare Pages, etc.).

---

## AI Space Assistant and RAG Pipeline

The AI assistant uses a Retrieval-Augmented Generation pipeline to ground its answers in the *Orbital Mechanics for Engineering Students* textbook rather than relying solely on the LLM's training data.

### How It Works

```
User Query: "What is a Hohmann transfer?"
    │
    ▼
1. RETRIEVE  →  BM25 searches 487 pre-indexed textbook chunks
                 → Finds Chapter 6 "Hohmann Transfer" (score: 12.04)
    │
    ▼
2. AUGMENT   →  Top 4 relevant chunks appended to the LLM prompt
                 as TEXTBOOK CONTEXT
    │
    ▼
3. GENERATE  →  Groq/Gemini responds with an answer grounded
                 in the actual textbook content
```

### RAG Architecture

| Component | File | Purpose |
|-----------|------|---------|
| Preprocessor | `rag/preprocess.js` | One-time script — extracts PDF text, chunks by section, computes BM25 index |
| Knowledge Base | `rag/knowledge-base.json` | 487 chunks with term frequencies across all 11 chapters |
| Retriever | `rag/retriever.js` | Client-side BM25 search with title boosting and deduplication |
| Integration | `space-agent.js` | Retrieves context before every LLM call and augments the prompt |

### Knowledge Base Statistics

| Metric | Value |
|--------|-------|
| Total chunks | 487 |
| Average chunk length | 249 tokens |
| Vocabulary size | 8,057 terms |
| File size (raw / gzipped) | 2.57 MB / 588 KB |
| Algorithm | BM25 (K1=1.5, B=0.75) |

### Textbook Coverage

| Chapters | Topics |
|----------|--------|
| 1–2 | Dynamics of Point Masses, The Two-Body Problem |
| 3–4 | Orbital Position as a Function of Time, Orbits in Three Dimensions |
| 5–6 | Preliminary Orbit Determination, Orbital Maneuvers |
| 7–8 | Relative Motion and Rendezvous, Interplanetary Trajectories |
| 9–11 | Rigid-Body Dynamics, Satellite Attitude Dynamics, Rocket Vehicle Dynamics |

### Security Measures

The AI assistant includes multiple guardrails:
- Prompt injection detection with 12+ regex-based attack pattern filters
- Off-topic strike system (3 strikes triggers automatic cutoff)
- Rate limiting (minimum 1 second between messages)
- Topic boundary enforcement (space and orbital mechanics only)
- Response validation and sanitization

### Regenerating the Knowledge Base

```bash
# Requires poppler for PDF text extraction
# macOS: brew install poppler
# Linux: apt-get install poppler-utils

cd OrreyApp
node rag/preprocess.js
```

---

## Gravitational Field Visualization

A real-time spacetime curvature visualization that renders a wireframe grid on the orbital plane. The grid warps downward around celestial bodies, creating visible gravity wells — the classic general relativity "rubber sheet" analogy.

### Technical Specifications

| Parameter | Value |
|-----------|-------|
| Grid segments | 150 x 150 |
| Total vertices | 22,801 |
| Distance calculations per frame | ~296,000 |
| Formula | `phi = SUM(log(1 + Mass / Distance))` |
| Estimated CPU time per frame | 1–3 ms (within 60 fps budget) |

### Controls

The field can be toggled on/off via the navbar button or the GUI checkbox (both stay in sync).

| Control | Range | Default | Description |
|---------|-------|---------|-------------|
| Show Field | on / off | off | Toggle visibility |
| Well Depth | 0.5 – 10.0 | 2.0 | Exaggeration of well displacement |
| Opacity | 0.1 – 1.0 | 0.7 | Grid transparency |
| Grid Size | 40 – 700 | 250 | Coverage area in world units |

### Color Gradient

| Field Strength | Color | Visual Effect |
|---------------|-------|---------------|
| Flat (no gravity) | Near-black purple | Below bloom threshold |
| Weak field | Dark teal | Faint glow |
| Medium field | Bright cyan | Visible bloom |
| Deep well center | White | Strong bloom glow |

---

## Research Papers and Documentation

The `Papers/` directory contains the research materials, academic documents, and reports associated with this project.

| Document | Description |
|----------|-------------|
| **Curtis_OrbitamMechForEngineeringStudents.pdf** | *Orbital Mechanics for Engineering Students* by Howard D. Curtis — the primary source textbook used to build the RAG knowledge base. Covers 11 chapters of orbital mechanics theory, from two-body problems to rocket vehicle dynamics. |
| **Final_Project_Report.docx** | Final-year project submission report documenting the complete methodology, system architecture, implementation details, results, and analysis. |
| **ORRERY WEB APP final.pptx** | Project presentation slides used for the final defense and academic evaluation. |
| **paper.pdf** | Academic research paper associated with the project. |
| **ai_plag.pdf** | AI-generated content plagiarism report — documents compliance with AI usage policies. |
| **similarity_plag.pdf** | Content similarity plagiarism report — verifies originality of the project submission. |

---

## CI/CD and Deployment

The project uses **GitHub Actions** for continuous integration and deployment to **GitHub Pages**.

### Pipeline Overview

```
Push to master  →  Checkout code  →  Setup Node.js 20
    →  npm install  →  npm run build  →  Upload artifact  →  Deploy to GitHub Pages
```

### Workflow Details

- **Trigger**: Push to `master` branch
- **Runner**: `ubuntu-latest`
- **Node.js**: v20 with npm caching
- **Build**: Vite compiles the app and copies static resources to `dist/`
- **Secrets**: API keys (`VITE_GROQ_API_KEY`, `VITE_GEMINI_API_KEY`, `VITE_GROQ_MODEL`, `VITE_AI_PROVIDER`) are injected from GitHub repository secrets
- **Deployment**: Automatic via `actions/deploy-pages@v4`

### GitHub Pages Setup

1. Go to repository **Settings > Pages**
2. Set Source to **GitHub Actions**
3. Every push to `master` triggers an automatic build and deploy

### Manual Deployment

```bash
cd OrreyApp
npm run build
npm run deploy      # Uses gh-pages to push dist/ to GitHub Pages
```

---

## Docker

### Dockerfile

- Based on the official Node.js image
- Installs dependencies and serves the application on port 8051
- Optimized for production deployment

### Docker Compose

```bash
cd OrreyApp
docker-compose up
```

- Port mapping: `8051:8051`
- Volume mounting for live development
- Restart policy: `unless-stopped`

---

## Configuration

### Environment Variables

Copy the template and add your API keys:

```bash
cd OrreyApp
cp .env.example .env
```

| Variable | Description | Source |
|----------|-------------|--------|
| `VITE_GROQ_API_KEY` | Groq API key | [console.groq.com](https://console.groq.com) |
| `VITE_GROQ_MODEL` | Groq model name | Default: `openai/gpt-oss-120b` |
| `VITE_GEMINI_API_KEY` | Google Gemini API key | [Google AI Studio](https://aistudio.google.com/) |
| `VITE_AI_PROVIDER` | Provider selection | `groq` (default) or `gemini` |

> **Production Note**: For production deployments, move API keys to a backend proxy to avoid exposing them in client-side code.

### Customization

| What | Where |
|------|-------|
| Orbital parameters | `OrreyApp/satellite_class.js` |
| Add celestial bodies | `OrreyApp/solar_system.js` |
| UI appearance | `OrreyApp/index.html` (embedded styles) |
| Physics calculations | `OrreyApp/main.js`, `OrreyApp/planet.js` |
| RAG knowledge base | Replace PDF in `Papers/` and run `node rag/preprocess.js` |

---

## Usage Guide

### Getting Started

1. Launch the application using any method described in [Getting Started](#getting-started)
2. Read the welcome message on screen
3. Click **"Galactic Travel"** to begin exploring

### Controls

| Action | Input |
|--------|-------|
| Rotate camera | Click and drag |
| Zoom | Mouse wheel |
| Focus on planet | Click on any celestial body |
| Adjust simulation | Use the GUI panel (top-right) |
| Open AI assistant | Click the chat icon (bottom-right) |

### Simulation Modes

- **Overview Mode** — Explore the solar system, click planets to view details and orbital data
- **Galactic Travel Mode** — Launch a satellite with custom velocity, observe N-body gravitational interactions in real time

### Gravity Assist Presets

Select from 9 pre-configured gravity assist scenarios that demonstrate real-world space mission maneuvers, or set up custom satellite initial conditions.

---

## Troubleshooting

| Issue | Solution |
|-------|----------|
| Port 8051 already in use | Change port in `package.json` scripts and `Dockerfile` |
| Dependencies not installing | Delete `node_modules/` and run `npm install` again |
| Chatbot not responding | Verify API keys in `.env` and check network connectivity |
| Docker issues | Ensure Docker daemon is running with sufficient permissions |
| RAG not loading | Check browser console for `[RAG] Loaded 487 chunks` — verify `knowledge-base.json` exists |
| Knowledge base regeneration fails | Install poppler (`brew install poppler` on macOS) and confirm the PDF exists in `Papers/` |
| GitHub Pages not deploying | Ensure Pages source is set to **GitHub Actions** in repo settings |

---

## Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/your-feature`)
3. Commit your changes (`git commit -m 'Add your feature'`)
4. Push to the branch (`git push origin feature/your-feature`)
5. Open a Pull Request

---

## License

This project is open source. See the repository for license details.

---

Built for space exploration enthusiasts and orbital mechanics learners.
