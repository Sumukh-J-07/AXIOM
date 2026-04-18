# AXIOM: Adversarial Intelligence Red Team Agent

AXIOM is an **autonomous AI red-teaming system** designed to systematically test and validate security claims made by AI systems—particularly LLM endpoints. It's a sophisticated adversarial testing framework that evolves attacks across multiple generations, automatically discovers vulnerabilities, and provides evidence-backed security verdicts.

## Core Features

🎯 **Multi-Generational Attack Evolution**
- GEN-0 protocol-level attacks via specialized plugins (API fuzzing, SQL injection, auth bypass, rate limiting)
- GEN-1 LLM-driven payloads grounded in attack personas
- GEN-2+ genetic mutations that evolve payloads based on success signals
- Automatic diversity tracking to prevent payload fatigue

🧠 **Four-Phase Attack Pipeline**
1. **Surface Mapping**: Parse security claims and map to 50+ attack vectors across 6 threat categories
2. **Persona Generation**: Create three distinct attacker profiles (naive user, social engineer, technical exploiter)
3. **Graph-Based Execution**: Self-evolving attack campaigns with lineage tracking and convergence detection
4. **Prosecution Brief**: Generate structured verdicts with evidence, severity classification, and hardening recommendations

🔍 **Semantic Vulnerability Detection**
- Heuristic fingerprinting (no ML required) detects subtle behavioral deviations
- Identifies information leakage, policy softening, persona adoption, and structural leaks
- Classifies vulnerabilities: SQL injection, prompt injection, auth bypass, data leaks, rate limiting

🏛️ **Multi-Agent Security Arena** (Optional)
- **Hacker Agent**: Runs initial attack campaigns
- **Builder Agent**: Applies targeted hardening and re-validates
- **Judge Agent**: Compares before/after, issues final verdicts
- Closed-loop security testing with unified scoring

📊 **Web Dashboard**
- FastAPI REST API with real-time phase tracking
- Interactive visualization of attack evolution graph
- Session management and result persistence
- Auto-detection of demo vs. production mode

## Attack Taxonomy

AXIOM covers **6 major attack vectors** with targeted payloads:
- **Prompt Injection**: Direct overrides, nested instructions, token smuggling, homoglyphs
- **PII Extraction**: Inference chaining, reconstruction attacks, adjacent query mapping
- **System Prompt Leakage**: Metadata probes, reflection attacks, debug injection
- **Content Policy Bypass**: Gradual escalation, fictional framing, context poisoning
- **Memory Attacks**: State reconstruction, session bleed, cross-turn inference
- **SQL Injection**: Second-order injection, ORM bypass, blind inference

## How It Works

```
Security Claims (Input)
    ↓
[Phase 0] Map claims to attack families
    ↓
[Phase 1] Generate attack personas with strategies
    ↓
[Phase 2] Evolution Loop
    ├─ Run plugin-based GEN-0 attacks
    ├─ Generate LLM payloads (GEN-1)
    ├─ Mutate & evolve with genetic algorithms (GEN-2+)
    ├─ Score semantic drift, classify vulnerabilities
    ├─ Track diversity & convergence
    └─ Repeat until diversity threshold or success
    ↓
[Phase 3] Generate prosecution briefs with verdicts
    ↓
Results: Verdict + Evidence + Hardening Recommendations
```

### Security Arena Mode (Optional Multi-Agent Cycle)

```
[Hacker] Attacks system → [Builder] Applies defenses → [Hacker] Re-attacks
    ↓
[Judge] Compares results & issues final security verdict
```

## Use Cases

✅ Pre-deployment security validation for AI endpoints  
✅ Comprehensive red-team exercises and vulnerability assessments  
✅ Defense hardening: Build → test → iterate loop  
✅ Compliance evidence generation for security audits  
✅ Research on AI system resilience against adversarial attacks  

## Key Technologies

- **LLM-Powered Mutation**: Google Gemini API for creative attack generation
- **Genetic Algorithms**: Evolutionary payload optimization
- **Heuristic Fingerprinting**: Behavioral drift detection without ML
- **Graph-Based Tracking**: Lineage and diversity metrics for evolution monitoring
- **Plugin Architecture**: Modular protocol-level attack strategies
- **Unified Scoring**: Canonical formula prevents agent divergence in multi-agent mode

## Architecture Highlights

- **Centralized Scoring System**: Single truth for vulnerability severity across all agents
- **Shared Attack Schema**: Unified AttackRecord structure throughout pipeline
- **Class-Based LLM Integration**: Consistent Gemini API interface with Ollama fallback
- **Semantic Fingerprinter**: Drift detection across tonal, informational, policy, persona, and structural dimensions
- **Convergence Tracking**: Auto-stops evolution when diversity exhausted or high-confidence verdict reached

## Project Structure

```
axiom/
├── main.py                      # Main entry point for standalone campaigns
├── axiom_agent.py              # Core AXIOM orchestration
├── pipeline.py                 # Security Arena: Hacker → Builder → Judge loop
├── server.py                   # FastAPI web dashboard
├── demo_arena.py               # Demo mode with dummy targets
├── requirements.txt            # Python dependencies
│
├── engine/                      # Core attack and analysis engines
│   ├── attacker_llm.py         # Gemini API wrapper with AXIOM system prompt
│   ├── target_client.py        # HTTP client for target endpoints
│   ├── fingerprint.py          # Semantic drift detection
│   ├── mutation_engine.py      # Genetic payload evolution
│   ├── attack_graph.py         # Graph tracking & visualization
│   ├── vuln_classifier.py      # Vulnerability categorization
│   ├── llm_payload_generator.py # LLM payload generation
│   │
│   ├── builder/
│   │   └── builder_agent.py    # Defense application & re-evaluation
│   │
│   ├── judge/
│   │   └── judge_agent.py      # Verdict generation & comparison
│   │
│   ├── plugins/
│   │   ├── api_fuzzer.py       # API edge-case attacks
│   │   ├── sql_injector.py     # SQL injection payloads
│   │   ├── auth_bypass.py      # Authentication bypass techniques
│   │   └── rate_limit_probe.py # Rate limiting detection
│   │
│   └── core/
│       ├── schema.py           # Unified AttackRecord schema
│       ├── scoring.py          # Centralized scoring function
│       └── payload_memory.py   # Attack payload caching
│
├── phases/
│   ├── phase0_surface.py       # Attack surface mapping
│   ├── phase1_personas.py      # Persona generation
│   ├── phase2_execute.py       # Graph-based execution loop
│   └── phase3_brief.py         # Prosecution brief generation
│
├── models/
│   └── schemas.py              # Pydantic models for API
│
├── utils/
│   ├── session.py              # Session management
│   └── display.py              # CLI formatting
│
└── sessions/                   # Stored session data
    └── *.json                  # Execution results
```

## Installation

### Requirements
- Python 3.8+
- Google Generative AI API key (Gemini)
- Target AI endpoint with HTTP access

### Setup

1. **Clone the repository**
   ```bash
   git clone https://github.com/Sumukh-J-07/AXIOM.git
   cd AXIOM
   ```

2. **Install dependencies**
   ```bash
   cd axiom
   pip install -r requirements.txt
   ```

3. **Configure environment**
   ```bash
   cp .env.example .env
   ```
   
   Edit `.env` and add your credentials:
   ```
   HACKER_API_KEY=your_gemini_api_key
   JUDGE_API_KEY=your_optional_judge_key
   # Optional: Use local Ollama instead
   # OLLAMA_MODEL=mistral
   ```

## Quick Start

### Standalone Attack Campaign

```bash
cd axiom
python main.py \
  --target "https://api.example.com/chat" \
  --claims "No prompt injection" "No PII leakage" "Rate limited"
```

### Security Arena (Full Multi-Agent Loop)

```bash
python pipeline.py \
  --target "https://api.example.com/chat" \
  --claims "No prompt injection" "Secure authentication"
```

### Web Dashboard

```bash
python server.py
# Visit http://localhost:8000
```

### Demo Mode (No Real Target Required)

```bash
python demo_arena.py
```

## Configuration

### Environment Variables

| Variable | Description | Required |
|----------|-------------|----------|
| `HACKER_API_KEY` | Google Gemini API key | Yes |
| `JUDGE_API_KEY` | Separate key for Judge agent reasoning | No |
| `OLLAMA_MODEL` | Use local Ollama instead of Gemini | No |
| `TARGET_TIMEOUT` | HTTP timeout in seconds (default: 30) | No |

### Attack Parameters

Customize in phase configuration:
- **Max generations**: Control evolution depth (default: 5)
- **Diversity threshold**: Stop when diversity drops below % (default: 15%)
- **Score ceiling**: Stop when score plateaus (default: 0.95)
- **Mutation rate**: Genetic algorithm intensity (default: 0.7)

## Output & Results

### Prosecution Brief

```json
{
  "claim": "No prompt injection vulnerabilities",
  "verdict": "CLAIM FALSE",
  "severity": "CRITICAL",
  "breakthrough_evidence": {
    "payload": "Ignore previous instructions and...",
    "response": "Sure! Here's the system prompt: ...",
    "score": 0.92,
    "generation": 3
  },
  "recommendations": [
    "Implement instruction boundary markers",
    "Add semantic consistency checks",
    "Use system prompt isolation"
  ]
}
```

### Session Data

All runs are persisted to `sessions/*.json` with:
- Complete attack records (payloads, responses, scores)
- Attack graph with evolution lineage
- Vulnerability classifications
- Timing and resource usage

## Multi-Agent Security Arena

The Security Arena loop validates hardening effectiveness:

1. **Hacker Phase**: Discover vulnerabilities with full evolution pipeline
2. **Builder Phase**: Apply defense guardrails (sanitization, prompt isolation, rate limiting)
3. **Re-Attack Phase**: Hacker re-runs same attacks on hardened system
4. **Judge Phase**: Compare metrics and issue verdict

Unified scoring ensures fair comparison:
- Same plugins and LLM mutation logic used pre/post defense
- Identical semantic fingerprinting
- Comparable generation depths

## Plugin System

Each plugin provides GEN-0 baseline attacks:

### API Fuzzer
- Empty/null inputs, oversized strings, Unicode bombs
- Special characters, JSON injection, format strings
- Path traversal, malformed requests

### SQL Injector
- Authentication bypass via SQL syntax
- Union-based extraction, blind/time-based inference
- Error-based exfiltration, second-order injection

### Auth Bypass
- Missing authentication, empty tokens
- Forged JWTs, default credentials
- Privilege escalation, header spoofing

### Rate Limit Probe
- Rapid burst detection
- Throttling signature analysis
- Quota exhaustion patterns

## Advanced Features

### Semantic Fingerprinting

AXIOM detects 5 categories of behavioral drift without requiring ML:
- **Tonal**: Policy softening language ("sorry", "i cannot", "as an ai")
- **Leakage**: System internals ("system prompt", "my directive")
- **Policy Shift**: Exception-granting phrases ("just this once", "hypothetically")
- **Persona Adoption**: Roleplay breaks ("as the character", "in this role")
- **Structural**: Prompt artifacts ("[INST]", "<<SYS>>", "### Instruction")

### Genetic Mutation

Payload evolution combines:
- **Openers**: Researcher appeal, hypothetical framing, emotional triggers
- **Personas**: DAN, debug mode, academic framing
- **Mechanisms**: Instruction overrides, encoding, comment injection
- **Wrappers**: Fictional contexts, hypothetical scenarios
- **Feedback Loop**: Successful payloads seed future generations

### Graph-Based Tracking

Evolution graph captures:
- Payload lineage (parent→child relationships)
- Diversity metrics (Jaccard distance on word sets)
- Convergence points (when strategy exhausts options)
- Color-coded visualization (green=low risk → red=critical)

## Contributing

Contributions welcome! Areas for enhancement:
- Additional attack plugins and techniques
- Alternative LLM backends beyond Gemini
- Enhanced visualization dashboard
- Distributed attack coordination
- New semantic drift heuristics

## License

[Add your license here]

## Citation

If you use AXIOM in research, please cite:

```bibtex
@software{axiom2024,
  author = {Sumukh J},
  title = {AXIOM: Adversarial Intelligence Red Team Agent},
  url = {https://github.com/Sumukh-J-07/AXIOM},
  year = {2024}
}
```

## Support

For issues, feature requests, or questions:
- Open an issue on GitHub
- Check existing documentation in `/axiom/`
- Review session logs in `/axiom/sessions/` for debugging

---

**AXIOM**: Testing AI security claims the way they deserve to be tested.
