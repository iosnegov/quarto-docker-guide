# Quarto Docker Guide: Transparent Multi-Language Environment Setup

*A guide demonstrating how to build isolated Docker environments for Quarto from scratch, covering R, Python, and Julia support with full PDF and HTML rendering.*

[![License: MIT-0](https://img.shields.io/badge/License-MIT--0-blue.svg)](LICENSE.txt)
[![Third-Party: Included](https://img.shields.io/badge/Third--Party-Included-orange.svg)](LICENSE.txt)

> [!WARNING]
> **Project Archived**
> 
> This project is no longer actively maintained. 
> 
> **Context:**  
> Originally, this repository was conceived as an attempt to build a fully isolated, multi-language environment using a custom `Docker` and `Dev Container` approach to achieve a higher level of isolation than `conda` provides. Over time, I realized that the level of isolation provided by `conda` is actually sufficient for my workflow. Furthermore, I discovered [`pixi`](https://pixi.prefix.dev/latest/), a modern package manager that leverages `conda` packages to deliver the robust, reproducible environment management I was originally seeking — without the overhead of maintaining custom Docker images.
> 
> **Reason for Archiving:**  
> Consequently, I no longer have an interest in further maintaining this custom Docker-based approach, as my workflow needs are now fully met by `pixi`. This repository remains available purely as an educational reference for those interested in building custom Docker and Dev Container environments from scratch.

## Project Philosophy

This guide prioritizes **transparency over optimization**. Every package installation is explicitly documented and justified, allowing users to understand exactly what gets installed on their system and why. While not the most minimal approach, this methodology provides complete visibility into the environment construction process.

## Target Audience

- **Quarto users** seeking to containerize their workflows.
- **Researchers** requiring reproducible multi-language environments.
- **Data scientists** working with R, Python, and Julia in Quarto documents.
- **Users** who want to understand system dependencies for PDF/HTML rendering.

> **Note**:  
> 1. Basic Docker knowledge is assumed. This guide focuses on Quarto-specific containerization, not Docker fundamentals.  
> 2. Core system dependencies are prioritized over styling customization. LaTeX templates and CSS themes are separate concerns not covered in this guide.  
> 3. While this project works on any Docker-supported platform, all command examples are written for GNU/Linux operating systems.  

## Prerequisites

1. **Docker Engine:** Docker Desktop or Docker Engine version.
2. **Development Environment:** Visual Studio Code with Dev Containers extension for optimal development experience.

## Project Structure

```bash
quarto-docker-guide/
├── 1.python/               # Python environment setup
├── 2.julia/                # Julia environment setup  
├── 3.r-lang/               # R environment setup
├── 4.quarto-pdf/           # PDF rendering capabilities
├── 5.diagrams/             # Diagram generation support
├── 6.quarto-multilang/     # Multi-language integration
├── Dockerfile              # Complete environment
├── resources/              # Fonts, styles, templates
│   ├── fonts/              # Typography resources
│   ├── fonts.css           # Font configurations
│   ├── preamble.tex        # LaTeX customizations
│   └── styles.css          # CSS styling
├── test.qmd                # Sample Quarto document
└── README.md               # This documentation
```

## Quick Start

### I. Clone Repository

```bash
git clone https://github.com/snegoviol/quarto-docker-guide.git
cd quarto-docker-guide
```

### II. Configure User Mapping for Dev Containers

For seamless file permissions between host and container, configure user IDs in `.devcontainer/devcontainer.json`:

**Check your current user and group IDs:**

```bash
# Get your user ID
id -u

# Get your group ID  
id -g

# Get both user and group info
id
```

**Update the build arguments in `.devcontainer/devcontainer.json`:**

```jsonc
"args": {
    "DEV_USER": "vscode",        // Username for development user (VSCode default)
    "DEV_USER_ID": "1000",       // User ID - replace with your actual user ID
    "DEV_USER_GID": "1000"       // Group ID - replace with your actual group ID
}
```

**Example for user ID 1001 and group ID 1001:**

```jsonc
"args": {
    "DEV_USER": "vscode",
    "DEV_USER_ID": "1001",
    "DEV_USER_GID": "1001"
}
```

### III. Open in Dev Container

1. Open the project in Visual Studio Code.
2. Press `Ctrl + Shift + P`.
3. Type and select: `Dev Containers: Rebuild and Reopen in Container`.
4. Wait for the container to build and initialize.

### IV. Verify Installation

Once inside the container:

```bash
# Comprehensive system verification
micromamba --version && \
    python --version && \
    R --version && \
    julia --version && \
    quarto --version && \
    tlmgr --version && \
    quarto check

# Test document rendering capabilities
quarto render test.qmd --to html
quarto render test.qmd --to pdf
```

**Expected output verification:**
- **Micromamba**: Package manager version.
- **Python/R/Julia**: Language runtime versions. 
- **Quarto**: CLI version and system check results.
- **TinyTeX (tlmgr)**: LaTeX package manager version.
- **Rendering**: Successful HTML and PDF generation from test document.

## Step-by-Step Learning Path

### 1. Basic Language Environments

Start with individual language setups to understand core dependencies.

### 2. PDF Rendering Capabilities

Add LaTeX support for PDF generation.

### 3. Multi-Language Integration

Combine all languages with cross-language communication.

## Key Design Decisions

### Package Management Strategy

- **Micromamba over pip/apt**: Pre-compiled packages save build time and provide unified dependency resolution across languages.
- **Conda-forge channel**: Community-maintained packages with consistent quality standards.

### LaTeX Distribution Choice

- **TinyTeX over full TeX Live**: Minimal footprint with automatic package installation.
- **On-demand package loading**: Only required packages downloaded during rendering.
- **Quarto integration**: Native support via `quarto install tinytex`.

### User Security Model

- **Non-root execution**: All operations run as unprivileged user.
- **Explicit permissions**: Clear ownership of conda environments and user directories.
- **Volume mount compatibility**: User ID mapping for seamless file access.

## License

This project contains both original source code and third-party components. 
As a result, different parts of the repository are distributed under different licenses. 
See [LICENSE.txt](LICENSE.txt) for details.
