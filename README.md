# Metadata Intelligence Platform

A Python-based metadata management platform for collecting, organizing, and analyzing metadata across SQL Server and Microsoft Fabric, with AI-assisted discovery through Model Context Protocol (MCP) and Microsoft Copilot Studio.

## Overview

The Metadata Intelligence Platform aims to make data assets easier to discover, understand, and maintain. It brings metadata from databases, semantic models, and Power BI reports into a centralized repository, enabling metadata exploration, lineage analysis, impact analysis, and natural-language questions through an AI assistant.

The project combines Python-based extraction, a metadata repository, modular analysis services, and an MCP server that exposes metadata capabilities to an AI agent.

This is an evolving prototype focused on demonstrating a practical architecture for metadata management and AI-assisted data discovery.

## Architecture

The platform follows a modular architecture:

1. **Metadata sources** — SQL Server databases, Microsoft Fabric semantic models, and Fabric reports.
2. **Extraction layer** — Python extractors collect structural and analytical metadata from supported sources.
3. **Metadata repository** — a centralized repository stores extracted metadata for querying and analysis.
4. **Analysis services** — Python services support lineage, impact analysis, report analysis, and metadata exploration.
5. **MCP server** — exposes selected metadata operations as tools that can be called by compatible AI clients.
6. **AI assistant** — Microsoft Copilot Studio provides a natural-language interface for asking questions about metadata and invoking supported operations.

```text
SQL Server ─────────────┐
                        │
Fabric Semantic Models ─┼──> Python Extractors
                        │           │
Fabric Reports ─────────┘           ▼
                            Metadata Repository
                                    │
                                    ▼
                             Analysis Services
                       ┌────────────┼────────────┐
                       ▼            ▼            ▼
                    Lineage      Impact       Report
                    Analysis    Analysis     Analysis
                                    │
                                    ▼
                                MCP Server
                                    │
                                    ▼
                           Copilot Studio Agent
```

*The diagram represents the logical architecture. Actual connectivity and execution depend on the configured environment and supported extraction workflows.*

## Key Features

### 1. Metadata Extraction

- Extracts database, table, and column metadata from SQL Server.
- Collects metadata from supported Microsoft Fabric semantic models and reports.
- Captures structural information such as columns, measures, relationships, and report elements where supported by the source.
- Supports separate extraction workflows for SQL Server, semantic models, and reports.
- Includes orchestration functionality for running a full metadata refresh.

### 2. Centralized Metadata Repository

The repository provides a central location for storing and querying collected metadata.

Its purpose is to support:

- Discovery of registered data assets.
- Exploration of database objects, tables, columns, and data types.
- Storage of semantic model and report metadata.
- Metadata-based analysis across different source types.
- Tracking of extraction execution and status where supported by the configured workflow.

### 3. Lineage and Impact Analysis

The platform includes services for exploring metadata dependencies and understanding how data assets relate to one another.

Capabilities include:

- Lineage exploration for supported metadata objects.
- Analysis of dependencies between measures, tables, and other assets where dependency metadata is available.
- Impact analysis to help investigate the potential consequences of changes.
- Exploration of report fields, visuals, measures, and related metadata.
- Identification of measures that are not referenced by known visuals, filters, or other measures.

Lineage completeness depends on the metadata extracted from each source and the availability of dependency information.

### 4. AI-Assisted Metadata Discovery

The MCP server exposes selected platform operations to compatible AI clients.

The Microsoft Copilot Studio agent can use these capabilities to help users:

- Ask natural-language questions about available metadata.
- Explore reports, visuals, measures, and semantic models.
- Investigate data lineage and potential impact.
- Identify unused measures.
- Initiate supported metadata extraction and refresh workflows.

AI responses depend on the available repository data, configured tools, authentication, and the capabilities exposed by the agent.

### 5. Modular Python Architecture

The project separates extraction, repository access, analysis services, and MCP integration into dedicated modules. This structure makes the platform easier to maintain and provides a foundation for adding new metadata sources and analysis capabilities.

## Technology Stack

| Technology | Purpose |
|---|---|
| Python | Extraction logic, services, and MCP server |
| SQL Server | Source database metadata |
| Microsoft Fabric | Fabric metadata sources and repository environment |
| pyodbc | SQL Server connectivity |
| pandas | Data processing and tabular metadata handling |
| Microsoft Authentication Library (MSAL) | Authentication support |
| Requests | HTTP/API communication |
| Model Context Protocol (MCP) | Exposing platform operations to AI clients |
| Microsoft Copilot Studio | Natural-language assistant |
| Git and GitHub | Version control and project management |
| Visual Studio Code | Development environment |

## Repository Structure

```text
MetadataPlatform/
├── config/
│   ├── __init__.py
│   ├── config.py
│   └── workspaces.json
├── extractors/
│   ├── __init__.py
│   ├── sql_extractor.py
│   ├── fabric_semantic_model_extractor.py
│   └── fabric_report_extractor.py
├── mcp/
│   ├── metadata_server.py
│   └── metadata_server_stdio.py
├── scripts/
│   └── start_mcp.ps1
├── src/
│   ├── fabric/
│   │   └── client.py
│   ├── repository/
│   │   ├── connection.py
│   │   └── metadata_repository.py
│   └── services/
│       ├── comparison_service.py
│       ├── dependency_service.py
│       ├── extraction_monitor_service.py
│       ├── extraction_service.py
│       ├── impact_analysis_service.py
│       ├── lineage_service.py
│       ├── metadata_quality_service.py
│       ├── report_analysis_service.py
│       └── semantic_model_service.py
├── backup/
│   └── backup_metadata_repository.py
├── requirements.txt
├── README.md
└── .gitignore
```

*This tree highlights the main application modules; it omits local environments, credentials, generated output, and local backup data.*

## Getting Started

### Prerequisites

Before running the platform, prepare:

- Python compatible with the installed dependencies.
- Visual Studio Code or another Python development environment.
- An accessible SQL Server instance and the required ODBC driver for SQL Server extraction.
- Access to the relevant Microsoft Fabric workspace, semantic models, and reports for Fabric extraction.
- A configured metadata repository and the necessary permissions.
- Appropriate authentication for the source systems and APIs.
- Microsoft Copilot Studio access if you want to use the AI assistant integration.

Some features require additional environment-specific setup and permissions.

### Installation

1. Clone the repository:

   ```bash
   git clone https://github.com/simona-markovska/MetadataPlatform.git
   cd MetadataPlatform
   ```

2. Create and activate a Python virtual environment:

   ```bash
   python -m venv .venv
   ```

   Windows PowerShell:

   ```powershell
   .\.venv\Scripts\Activate.ps1
   ```

3. Install the dependencies:

   ```bash
   pip install -r requirements.txt
   ```

4. Configure the connection settings and authentication required by your environment.

5. Review the relevant extractor and MCP server entry points before running them. Extraction workflows may require source-specific permissions, authentication, and repository configuration.

### Configuration and Security

- Keep credentials, access tokens, and environment-specific secrets in local environment configuration.
- Do not commit `.env` files, tokens, passwords, or connection strings containing credentials.
- Configure source connections and Fabric access according to your organization's security policies.
- Review `config/config.py` and the relevant client and repository modules to understand the settings used by each workflow.
- The repository may contain environment-specific workspace identifiers or defaults; review these before reusing the project in another environment.

## Current Development Status

The platform has progressed from SQL Server metadata extraction to a broader metadata intelligence prototype.

Implemented components include:

- SQL Server metadata extraction.
- Fabric semantic model and report extraction workflows.
- Centralized metadata repository integration.
- Modular metadata analysis services.
- MCP server tools for metadata exploration and analysis.
- Copilot Studio integration for AI-assisted metadata questions.
- Extraction job initiation and monitoring capabilities.

End-to-end availability depends on the source environment, authentication, repository configuration, and the particular workflow being executed.

## Roadmap

Potential next steps include:

- **Reliability:** improve extraction orchestration, error handling, retries, and refresh monitoring.
- **Metadata quality:** expand validation, completeness checks, and consistency reporting.
- **Lineage:** improve dependency coverage across physical SQL objects, semantic models, and reports.
- **User experience:** develop a dedicated metadata explorer and visual lineage interface.
- **AI capabilities:** improve context-aware answers, metadata documentation, and change-impact explanations.
- **Automation:** integrate scheduled refreshes and operational monitoring.
- **Extensibility:** add further metadata sources and standardized interfaces for new extractors.

## Project Purpose

This project explores how traditional metadata management can be combined with modern data platforms and AI tooling.

The longer-term objective is to create a lightweight, extensible metadata catalog that helps technical teams discover data assets, understand dependencies, assess the potential impact of changes, and access metadata through both conventional interfaces and natural-language interaction.

## License

This repository is intended for learning, experimentation, and portfolio demonstration. No explicit open-source license is currently specified. Contact the repository owner regarding reuse until a license is added.
