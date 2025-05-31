# FSM-Based Vulnerability Detection in Tokenized Assembly: 
A Case Study on CWE-457 (Use of Uninitialized Variable)

> ⚠️*Note:*⚠️ <br>
> This repository represents Phase 1 of a multi-stage research project and will likely serve as the foundation for my future dissertation work. 
> Due to its academic sensitivity, the source code is not currently public, but I’ve included the associated research paper to outline the approach, methodology, and results. 
> The goal of this repository is to showcase the scope of work, technical direction, and research contributions, not to release the full implementation at this stage. 
> Additional materials (e.g., visualizations, presentation slides, and selected data samples) may be added in the future as publication timelines permit.

<br>
Presentation discussing approach, methodology, and results can be found [here](https://youtu.be/3k3uCzpEjzs).

## Project Overview
This project explores the feasibility of using a Finite State Machine (FSM) approach to detect vulnerabilities directly from disassembled binary code. Specifically, it targets the detection of **CWE-457: Use of Uninitialized Variable** without relying on source code availability.

The workflow includes:
- Disassembly normalization and abstraction
- Tokenization of critical memory operations
- FSM simulation of variable initialization and usage
- Full dataset evaluation using precision, recall, and F1 metrics

This approach demonstrates how binary-level static analysis can emulate compiler or source-aware vulnerability detection in a purely compiled environment.

## Dataset Preparation

> **Note:** This project assumes access to a compiled disassembly dataset from the Juliet Test Suite (CWE-457 subset).  
>  
> However, a complete automated preparation pipeline is provided for future use and reproducibility.

1. Clone the Juliet Test Suite Linux port by Alexander Richardson:

```bash
git clone https://github.com/arichardson/juliet-test-suite-c.git
cd juliet-test-suite-c
```

2. Build the test cases into ELF binaries:

```bash
python3 juliet.py -457 --generate --make -k --run | tee full_build.log
```

- This compiles only CWE-457 from the Juliet C/C++ test cases (excluding Windows-specific ones).


```bash
python3 juliet.py --all --generate --make -k --run | tee full_build.log
```

- This compiles all available Juliet C/C++ test cases (excluding Windows-specific ones).

3. Run the provided `generate_dataset.py` script:

```bash
python3 generate_dataset.py
```
This script:
- Disassembles all binaries automatically using objdump
- Normalizes disassembly format
- Generate structured outputs:
    - dataset.csv: Basic metadata table
    - dataset.jsonl: Full samples with disassembly + tokenized form
    - disasm/: Organized disassembly files by CWE and good/bad labels

This script prepares clean, reusable datasets to support both:
- FSM-based vulnerability analysis (this project)
- Future LLM or machine learning-based vulnerability classification

## Project Setup

1. Create and activate a Python virtual environment:

```bash
python3 -m venv my_venv
source my_venv/bin/activate
```

2. Install project dependencies:

```bash
pip install --upgrade pip
pip install -r "requirements.txt"
```

The `setup_env.sh` script will:
- Create a virtual environment (if not already created)
- Install all required Python libraries listed in `requirements.txt`

## Requirements
Python 3.9 or newer is recommended.

Key libraries:
- numpy
- pandas
- argparse
- glob
- os
- re

Full list is available in `requirements.txt`.

## Running the Project
Once dependencies are installed:
<br>
Open and run the provided `FSM_Binary_Analyzer.ipynb` notebook for training and evaluation.

Alternatively, you can run the FSM evaluation script standalone via CLI for full dataset testing.
```bash
python3 -m fsm_binary_analyzer.tests.functional.test_fsm --mode full
```

## Notes
The project uses a lightweight dataset derived from the Juliet Test Suite.

## Acknowledgments
- [Juliet Test Suite (NIST)](https://samate.nist.gov/SARD/test-suites/112)
- [arichardson's Juliet Test Suite C Port](https://github.com/arichardson/juliet-test-suite-c)
