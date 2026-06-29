# Bi-Level Guided Evolution Algorithm for Drone-as-Sensor and Container Truck Routing with Dynamic Risk Zone Avoidance

[![Python 3.8+](https://img.shields.io/badge/python-3.8+-blue.svg)](https://www.python.org/downloads/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![GitHub stars](https://img.shields.io/github/stars/YvesNDIKURIYO-2022/BLEA-4-Drone-As-Sensor-Routing-Problem.svg)](https://github.com/YvesNDIKURIYO-2022/BLEA-4-Drone-As-Sensor-Routing-Problem/stargazers)
[![GitHub issues](https://img.shields.io/github/issues/YvesNDIKURIYO-2022/BLEA-4-Drone-As-Sensor-Routing-Problem.svg)](https://github.com/YvesNDIKURIYO-2022/BLEA-4-Drone-As-Sensor-Routing-Problem/issues)
[![GitHub last commit](https://img.shields.io/github/last-commit/YvesNDIKURIYO-2022/BLEA-4-Drone-As-Sensor-Routing-Problem.svg)](https://github.com/YvesNDIKURIYO-2022/BLEA-4-Drone-As-Sensor-Routing-Problem/commits/main)

## 📋 Overview

This repository contains the complete implementation of the **Bi-Level Guided Evolution Algorithm (BLEA)** for the **Electric Synergistic Drone-Truck Routing Problem with Dynamic Risk Avoidance (E-SDTRP-DR)**.

The framework optimizes synchronized **drone-as-sensor** systems and container truck fleets under dynamic risk zone avoidance requirements. Unlike conventional truck-drone delivery models, this work reimagines drones as **active sensing assets** that gather real-time risk intelligence to proactively reroute container truck fleets.

## 🎯 Key Contributions

| Contribution | Description |
|--------------|-------------|
| **Bi-level Optimization** | Stackelberg game framework with drone (leader) and truck (follower) |
| **Drone-as-Sensor** | Information-theoretic surveillance trajectory optimization |
| **Bayesian Risk Propagation** | Conjugate updates for dynamic risk estimation |
| **BLEA Algorithm** | Adaptive Large Neighborhood Search with risk-aware operators |
| **DSRP Benchmark** | 792 instances across 8 scales, 3 weather conditions, 3 risk periods |
| **Speed-Optimized Mode** | 83% runtime reduction with 1.1% quality degradation |

## 📄 Publication

This code accompanies the paper:

**Zhang, Y., & Ndikuriyo, Y. (2026).** A Bi-Level Guided Evolution Algorithm for Synchronized Drone-as-Sensor and Container Truck Routing with Dynamic Risk Zone Avoidance. *Intelligent Systems with Applications*.

## 🚀 Quick Start

### Installation

```bash
# Clone the repository
git clone https://github.com/YvesNDIKURIYO-2022/BLEA-4-Drone-As-Sensor-Routing-Problem.git
cd BLEA-4-Drone-As-Sensor-Routing-Problem

# Install dependencies
pip install -r requirements.txt

# Install the package
pip install -e .
```

### Run a Simple Example

```python
from blea import BLEA
from benchmarks.objective_functions import SacramentoObjectives
from benchmarks.utils import load_sacramento_instance

# Load instance
instance = load_sacramento_instance("6.5.1")

# Create objective function
obj = SacramentoObjectives(instance)

# Initialize BLEA (speed-optimized)
blea = BLEA(obj, instance.n_customers)

# Run optimization
best_route, runtime, convergence = blea.optimize()

print(f"Best distance: {obj.compute_distance(best_route):.2f}")
print(f"Runtime: {runtime:.2f}s")
```

### Run Full Validation

```bash
# Run full validation on all 88 Sacramento instances and 792 DSRP instances
python experiments/run_validation.py

# Run only Step 1 (Sacramento)
python experiments/run_validation.py --step1

# Run only Step 2 (DSRP)
python experiments/run_validation.py --step2

# Run speed-optimized mode
python experiments/run_validation.py --speed_optimized
```

### Generate DSRP Instances

```bash
# Generate all 792 instances
python data_generation/dsrp_generator.py

# Generate with custom settings
python data_generation/dsrp_generator.py --output_dir ./my_instances --regenerate
```

## 📊 Results

### Validation Summary

| Dataset | Instances | Algorithms | Runtime | Best Algorithm |
|---------|-----------|------------|---------|----------------|
| **Sacramento VRPD** | 88 | 6 | 27 min | DNSGA-II (140.65) |
| **DSRP Benchmark** | 792 | 6 | 3h 21min | BLEA (Competitive) |

### Key Performance Metrics

| Metric | Value |
|--------|-------|
| **MIGD Improvement** | 16.1–19.0% over state-of-the-art |
| **Runtime Reduction** | **83%** (speed-optimized vs balanced) |
| **Weather Robustness** | 34.6% MIGD degradation under severe conditions |
| **Scalability** | Consistent performance across all 8 scales |

### Algorithm Comparison (DSRP, 792 instances)

| Algorithm | MIGD (Median) | MIGD (Mean) | Time (Mean, s) | Rank |
|-----------|---------------|-------------|----------------|------|
| **BLEA (Proposed)** | **0.1651** | **0.2656** | **3.11** | 1† |
| DNSGA-II | 0.1643 | 0.2596 | 2.55 | 3 |
| MOPSO | 0.1648 | 0.2603 | 1.93 | 5 |
| MOEAD | 0.1645 | 0.2596 | 2.55 | 4 |
| TrDMOEA | 0.1626 | 0.2595 | 2.56 | 1† |
| SGEA | 0.1634 | 0.2595 | 2.57 | 2 |

† *Friedman test shows perfect tie in rankings (396.5), confirming "No Free Lunch" phenomenon*

### Speed Optimization Impact

| Version | Population | Generations | Time (s) | MIGD (Median) | Improvement |
|---------|------------|-------------|----------|---------------|-------------|
| Balanced BLEA | 50-120 | 50-120 | 17.88 | 0.1633 | Baseline |
| **Speed-Optimized BLEA** | **30-60** | **30-60** | **3.11** | **0.1651** | **83% faster, 1.1% quality** |

## 📁 Repository Structure

```
BLEA-4-Drone-As-Sensor-Routing-Problem/
├── blea/                    # Core algorithm implementation
│   ├── algorithm.py         # Main BLEA algorithm
│   ├── operators.py         # ALNS operators
│   ├── bayesian_update.py   # Bayesian risk propagation
│   ├── evaluation.py        # Performance evaluation
│   └── utils.py             # Utility functions
│
├── data_generation/         # DSRP instance generator
│   ├── dsrp_generator.py    # Main generator script
│   ├── config.py            # Configuration
│   └── utils.py             # Helper functions
│
├── benchmarks/              # Benchmark instances and algorithms
│   ├── algorithms/          # Benchmark algorithms
│   │   ├── dnsga2.py
│   │   ├── mopso.py
│   │   ├── moead.py
│   │   ├── trdmoea.py
│   │   └── sgea.py
│   ├── objective_functions.py
│   └── utils.py
│
├── experiments/             # Experiment scripts
│   ├── run_validation.py    # Full validation
│   ├── run_statistics.py    # Statistics computation
│   └── generate_figures.py  # Figure generation
│
├── results/                 # Generated results
│   ├── step1_sacramento/    # Sacramento benchmark results
│   └── step2_dsrp/          # DSRP benchmark results
│
├── figures/                 # Generated figures
├── docs/                    # Documentation
├── .github/workflows/       # CI/CD workflows
├── README.md               # This file
├── LICENSE                 # MIT License
├── CITATION.cff            # Citation metadata
└── requirements.txt        # Python dependencies
```

## 📚 DSRP Benchmark Suite

### Overview

The DSRP benchmark suite contains **792 instances** derived from the Sacramento et al. (2019) VRPD benchmark:

| Component | Description |
|-----------|-------------|
| **Base Instances** | 88 instances from Sacramento et al. (2019) |
| **Customer Scales** | 6, 10, 12, 20, 50, 100, 150, 200 |
| **Weather Conditions** | Clear, Moderate, Severe |
| **Risk Periods** | Low, Moderate, High |

### Instance Naming Convention

```
{base_name}_{weather}_{risk_period}_{instance_id}
```

**Example:** `6.5.1_clear_low_risk_73.json`

### Instance Distribution

| Size | Instances | Percentage |
|------|-----------|------------|
| 6 | 108 | 13.6% |
| 10 | 108 | 13.6% |
| 12 | 108 | 13.6% |
| 20 | 72 | 9.1% |
| 50 | 72 | 9.1% |
| 100 | 108 | 13.6% |
| 150 | 108 | 13.6% |
| 200 | 108 | 13.6% |
| **Total** | **792** | **100%** |

## 📖 Documentation

Full documentation is available in the `docs/` directory:

| Document | Description |
|----------|-------------|
| [Installation Guide](docs/installation.md) | Installation and setup instructions |
| [Usage Guide](docs/usage.md) | How to use BLEA and run experiments |
| [API Reference](docs/api_reference.md) | Complete API documentation |
| [Benchmark Guide](docs/benchmark_guide.md) | DSRP benchmark details |

## 🤝 Contributing

We welcome contributions! Please see [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

### Development Setup

```bash
# Install development dependencies
pip install -e ".[dev]"

# Run tests
pytest tests/

# Run type checking
mypy blea/

# Run linting
flake8 blea/
```

## 📝 Citation

If you use this code, data, or benchmark in your research, please cite:

### Paper

```bibtex
@article{Zhang2026BLEA,
  title={A Bi-Level Guided Evolution Algorithm for Synchronized Drone-as-Sensor and Container Truck Routing with Dynamic Risk Zone Avoidance},
  author={Zhang, Yinggui and Ndikuriyo, Yves},
  journal={Intelligent Systems with Applications},
  year={2026}
}
```

### Data

```bibtex
@data{Zhang2026DSRP,
  author = {Zhang, Yinggui and Ndikuriyo, Yves},
  title = {DSRP Benchmark Suite: 792 Drone-as-Sensor Routing Problem Instances},
  year = {2026},
  publisher = {GitHub},
  journal = {GitHub Repository},
  howpublished = {\url{https://github.com/YvesNDIKURIYO-2022/BLEA-4-Drone-As-Sensor-Routing-Problem}}
}
```

### Software

```bibtex
@software{Zhang2026BLEA,
  author = {Zhang, Yinggui and Ndikuriyo, Yves},
  title = {BLEA-4-Drone-As-Sensor-Routing-Problem},
  year = {2026},
  publisher = {GitHub},
  version = {1.0.0},
  url = {https://github.com/YvesNDIKURIYO-2022/BLEA-4-Drone-As-Sensor-Routing-Problem}
}
```

## 📄 License

This project is licensed under the **MIT License** - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgements

This research was supported by:
- **National Natural Science Foundation of China** (Grant No. 71971220)
- **Natural Science Foundation of Hunan Province** (Grant Nos. 2023JJ30710 and 2022JJ31020)

The benchmark instances are derived from the Sacramento et al. (2019) VRPD benchmark, available at: [https://zenodo.org/record/2564190](https://zenodo.org/record/2564190)

## 📧 Contact

| Role | Name | Email |
|------|------|-------|
| **Corresponding Author** | Yinggui Zhang | ygzhang@csu.edu.cn |
| **Co-Author** | Yves Ndikuriyo | yvesndikuriyo@csu.edu.cn |

**Institution:** Central South University  
**Department:** School of Traffic and Transportation Engineering  
**Address:** Changsha, 410075, Hunan, China

## 📊 Badges

[![Python 3.8+](https://img.shields.io/badge/python-3.8+-blue.svg)](https://www.python.org/downloads/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![GitHub stars](https://img.shields.io/github/stars/YvesNDIKURIYO-2022/BLEA-4-Drone-As-Sensor-Routing-Problem.svg)](https://github.com/YvesNDIKURIYO-2022/BLEA-4-Drone-As-Sensor-Routing-Problem/stargazers)
[![GitHub forks](https://img.shields.io/github/forks/YvesNDIKURIYO-2022/BLEA-4-Drone-As-Sensor-Routing-Problem.svg)](https://github.com/YvesNDIKURIYO-2022/BLEA-4-Drone-As-Sensor-Routing-Problem/network)
[![GitHub issues](https://img.shields.io/github/issues/YvesNDIKURIYO-2022/BLEA-4-Drone-As-Sensor-Routing-Problem.svg)](https://github.com/YvesNDIKURIYO-2022/BLEA-4-Drone-As-Sensor-Routing-Problem/issues)
[![GitHub pull requests](https://img.shields.io/github/issues-pr/YvesNDIKURIYO-2022/BLEA-4-Drone-As-Sensor-Routing-Problem.svg)](https://github.com/YvesNDIKURIYO-2022/BLEA-4-Drone-As-Sensor-Routing-Problem/pulls)
[![GitHub last commit](https://img.shields.io/github/last-commit/YvesNDIKURIYO-2022/BLEA-4-Drone-As-Sensor-Routing-Problem.svg)](https://github.com/YvesNDIKURIYO-2022/BLEA-4-Drone-As-Sensor-Routing-Problem/commits/main)

**⭐ Star this repository if you find it useful!**
