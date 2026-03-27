# Backend Engineering Portfolio

## Weather Forecast Service

### Project Overview

A production ready Go microservice that delivers real time weather forecasts for random US locations by orchestrating multiple external APIs. The service fetches location data, coordinates it with the National Weather Service API, and returns detailed forecasts through a lightweight HTTP interface. Built as a proof of concept with full containerization and Kubernetes deployment pipelines, demonstrating modern cloud native development practices.

### Architecture & Tech Stack

**Core Technologies:**
- Go 1.24+ with standard library (`net/http`, `encoding/json`)
- Docker multi stage builds for optimized container images
- Kubernetes orchestration with NodePort services
- GitHub Container Registry (GHCR) for image distribution
- RESTful API design patterns

**Infrastructure:**
- Alpine Linux base images (minimal 3.21 runtime)
- Single endpoint HTTP service (port 5000)
- Stateless microservice architecture
- External API integration with locations.patch3s.dev and weather.gov

### Scale & Complexity

- **API Integration:** Coordinates 3 distinct API calls per request (location fetch, coordinates to forecast URL mapping, forecast retrieval)
- **Deployment Pipeline:** Automated CI/CD via shell scripting (build, tag, push, deploy)
- **Container Optimization:** Multi stage Docker builds reducing image size by using `CGO_ENABLED=0` for static compilation
- **Cloud Native:** Full Kubernetes deployment with configurable replica scaling

### Key Engineering Challenges Solved

**Multi API Orchestration**
Designed a sequential API flow that chains three external services (random location provider, NWS coordinates API, and forecast API) with comprehensive error handling at each step. Implemented nil safety checks and graceful degradation when upstream services fail.

**Production Ready Containerization**
Architected multi stage Docker builds separating compilation and runtime environments. Used Alpine Linux to minimize attack surface and image size while maintaining Go binary compatibility through static compilation (`CGO_ENABLED=0`).

**Kubernetes Deployment Strategy**
Created flexible deployment configurations supporting both local development (with `imagePullPolicy: Never`) and production workflows (GitHub Container Registry with `imagePullPolicy: Always`). Implemented NodePort services for external access patterns.

**Observability & Debugging**
Added structured logging throughout the request lifecycle, tracking API calls, URL construction, and response marshalling. Identified areas for enhancement including log level granularity and environment based configuration.

**API Error Handling**
Built resilient error handling that translates upstream API failures into appropriate HTTP status codes and user friendly messages. Implemented validation for empty/nil responses and malformed data from third party services.

### My Contributions

Built this microservice end to end from initial scaffolding through production deployment. Started with core HTTP server implementation, then progressively added external API integrations (random location fetching, National Weather Service coordinates mapping, and forecast retrieval). Implemented comprehensive logging for request tracing and debugging. Containerized the application using Docker multi stage builds for optimization, then created Kubernetes deployment manifests supporting both local and cloud environments. Developed automated deployment scripts to streamline the build tag push deploy workflow. Documented architectural decisions, API flows, and future scaling considerations for production readiness.

Key ownership areas include the complete request handling pipeline, external API integration patterns, containerization strategy, and Kubernetes orchestration. Also identified and documented technical debt items around concurrency patterns, HTTP client timeouts, and logging granularity for future iterations.

---

## Go Algorithm & Data Structures Practice

### Project Overview

This repository demonstrates systematic practice of core backend engineering concepts through algorithmic problem solving in Go. It showcases proficiency in data structures, algorithm optimization, API integration, and data processing, foundational skills for building scalable backend systems. The codebase includes solutions to 18+ problems spanning string manipulation, linked list operations, interval merging, API consumption, and CSV data processing. Each solution emphasizes clean code, efficient algorithms, and practical Go patterns that translate directly to production backend development.

### Architecture & Tech Stack

**Language:** Go 1.x with idiomatic patterns (structs, interfaces, goroutines ready design)

**Data Processing:** CSV parsing with buffered I/O, JSON marshaling/unmarshaling, reflection based generic filtering

**External Integration:** HTTP client implementation for REST API consumption (OMDB API), proper error handling and resource cleanup

**Core Patterns:**
- Dynamic function dispatch registry for extensible CLI tooling
- Two pointer algorithms for optimal O(n) traversals
- Hash map based deduplication and lookup optimization
- Linked list in place manipulation without extra space allocation
- Sliding window techniques for subarray analysis

**Infrastructure:** Command line interface with flag based routing, modular function organization supporting 15+ independent problem domains

### Scale & Complexity

**Code Volume:** 1,200+ lines across 18 modules with zero external dependencies beyond standard library

**Algorithm Complexity:** Solutions optimized for time complexity ranging from O(n) to O(n²) depending on problem constraints, with space complexity minimized through in place operations where possible

**Data Handling:** CSV processing capability for datasets with buffered reading, JSON payload parsing with struct mapping, HTTP response streaming with proper connection management

**Problem Domains:**
- 8 advanced data structure problems (linked lists, arrays, hash maps)
- 4 string processing challenges (palindrome detection, rotation verification, bracket matching)
- 3 API/data integration tasks (REST consumption, CSV parsing, JSON transformation)
- 3 business logic implementations (card game rules, pricing calculations, inventory management)

### Key Engineering Challenges Solved

**Efficient Interval Merging**
Implemented O(n log n) interval merge algorithm handling overlapping time ranges through sorting and single pass consolidation, applicable to scheduling systems and time series data processing.

**Zero Allocation Linked List Operations**
Developed in place linked list reversal and even/odd node extraction using pointer manipulation without additional memory overhead, demonstrating understanding of memory efficient data structure operations critical for high throughput systems.

**Generic Data Filtering with Reflection**
Built reflection based generic filter supporting arbitrary struct field queries, enabling reusable data access patterns without code duplication. Pattern applicable to building flexible query layers over heterogeneous data sources.

**HTTP Client Resilience**
Implemented proper resource cleanup patterns (defer for response body closing), error propagation, and JSON deserialization with type safety. Handles API failures gracefully and demonstrates production ready HTTP client patterns.

**Balanced Permutation Analysis**
Solved complex combinatorial problem (identifying balanced subsequences in O(n²)) through optimized sliding window with min/max tracking, showcasing ability to transform brute force algorithms into practical solutions.

**Dynamic Function Registry**
Designed extensible CLI architecture using function maps and closures, allowing runtime function selection without switch/case sprawl. Pattern scales to plugin systems and handler registration in web frameworks.

### My Contributions

This repository represents continuous skill development in Go backend engineering, with implementations spanning algorithmic problem solving, data processing pipelines, and API integration patterns. All solutions written independently with focus on code clarity, performance optimization, and Go best practices. The modular structure demonstrates ability to organize complex codebases with clear separation of concerns and extensible architecture. Solutions reflect understanding of production engineering tradeoffs between time/space complexity, readability, and maintainability.

---

## Golf Statistical Analysis System

### Project Overview

A statistical analysis platform built in Go for evaluating professional golf players across multiple performance dimensions. The system aggregates player performance data from multiple sources, applies normalized scoring models, and generates ranked player evaluations. It includes a post tournament evaluation engine that analyzes selection patterns, player uniqueness, and performance probability through Monte Carlo simulation.

### Architecture & Tech Stack

Built as a modular data pipeline in **Go 1.x**, leveraging standard library components for maximum performance and minimal dependencies:

- **HTTP client** for real time API consumption (DataGolf stats API)
- **CSV/JSON parsers** for multi source data ingestion and export
- **Excel processing** for integration with external modeling tools
- **Statistical computing** using native math libraries for Z score normalization and variance calculations

The system follows a functional pipeline architecture with clear separation between data ingestion, transformation, and export stages. State is managed through in memory maps with deterministic ordering for reproducible results.

### Scale & Complexity

- **Data volume:** Processes 70+ player records weekly with 15+ statistical attributes per player
- **External integrations:** 3+ data sources (DataGolf API, projection systems, analytical weight configurations)
- **Computation:** 10,000 Monte Carlo simulations per evaluation with variance modeling
- **Output generation:** Performance stratified player pools across 6 tiers
- **Combinatorial analysis:** Evaluates feasible comparison space of ~7.4M combinations from 156M theoretical possibilities

### Key Engineering Challenges Solved

**Statistical Normalization Across Heterogeneous Data Sources**
Built a Z score normalization engine that transforms raw statistics (strokes gained, driving distance, greens in regulation) into comparable metrics. This enables apples to apples comparison across different measurement scales and accounts for population variance. The solution handles missing data gracefully and maintains statistical validity when merging datasets.

**Dynamic Weighting System for Context Specific Optimization**
Implemented a configurable weighting model that adjusts statistical importance based on course characteristics. For example, a tight tree lined course emphasizes driving accuracy over distance, while a links course rewards distance and wind play. The system recalculates player rankings in real time as weights change, enabling rapid iteration on analytical models.

**Monte Carlo Simulation for Probabilistic Outcome Modeling**
Developed a simulation engine that runs 10,000 trials per evaluation to estimate performance rates and outcome distributions. The challenge was calibrating variance (settled on 20% based on PGA historical volatility) and ensuring the simulation runtime remained practical. Optimized by pre computing player distributions and using efficient random number generation.

**Combinatorial Explosion in Comparison Space Analysis**
Solved the computational challenge of analyzing player uniqueness in a space with 156M theoretical combinations. Implemented constraint filtering to reduce feasible space to 7.4M, then built correlation matrices to identify evaluation clusters. This revealed that uniqueness optimization has diminishing returns and strategic leverage matters more than pure differentiation.

**Data Pipeline Orchestration with Error Recovery**
Built a fault tolerant ingestion pipeline that handles missing data, API failures, and schema mismatches. The system includes name normalization (handling "Ludvig Åberg" vs "Ludvig Aberg"), retry logic with exponential backoff, and intermediate result caching to enable fast iteration without re fetching external data.

### My Contributions

Built this system from the ground up as the sole developer. Key ownership areas include:

- **Core analysis pipeline** (main.go): Player struct modeling, Z score calculation engine, weighted scoring system, performance stratified pool generation
- **Evaluation framework** (stats/review/): Combinatorial analysis, Monte Carlo simulation engine, uniqueness scoring, leverage metrics, correlation analysis
- **Data integration:** Multi source CSV/JSON parsing, API client implementation, Excel interop for external tool compatibility
- **Statistical modeling:** Z score normalization, variance calibration, correlation matrix generation, entropy calculations for evaluation rarity

The project demonstrates end to end ownership of a statistical computing pipeline from data ingestion through algorithmic analysis to actionable output generation.

---

## PGA Player Analytics Platform

### Project Overview

A comprehensive golf analytics system for head to head player comparisons and tournament performance analysis. The platform ingests live tournament data, historical player statistics, and comparative performance metrics to generate probability based matchup evaluations using advanced statistical modeling. Built to process real time data from multiple sources (Power BI dashboards, sports analytics platforms, and tournament feeds), transform it through multi stage analytical pipelines, and output actionable insights via Excel reports with visual indicators and ranked recommendations.

### Architecture & Tech Stack

**Core Technologies:**
- Go 1.24 (primary computation engine)
- JavaScript/TypeScript (data extraction and scraping)
- Excel/CSV pipelines for data I/O

**Key Libraries & Tools:**
- `go-rod` for browser automation and web scraping
- `excelize` for programmatic Excel manipulation and report generation
- Vanilla JavaScript for Power BI virtualized table parsing with async scroll handling

**Data Flow:**
1. Web scraping layer extracts live comparison data using headless browser automation
2. Power BI scrapers parse virtualized tables (handling lazy loaded rows) for player statistics across multiple dimensions (trends, bounceback metrics, course fit)
3. Go based analytics engine applies statistical models (normal distribution, probability adjustments)
4. Report generation creates multi format Excel outputs with conditional styling and sorted recommendations

**Infrastructure Approach:**
- File based data pipeline with CSV/XLSX intermediate formats
- Stateless computation model for reproducibility
- Local execution optimized for rapid iteration during live tournaments

### Scale & Complexity

- **Data Volume:** Processes 120+ player statistical profiles per tournament with 10+ metrics per player
- **Real time Constraints:** Scrapes and analyzes 50+ live matchup comparisons within tournament windows
- **Transformation Pipeline:** 4 stage data flow (scrape, parse, analyze, report) with validation at each stage
- **Output Artifacts:** Generates 3 distinct Excel reports per run (full matchups, sorted evaluations, statistics lookup tables)
- **Data Deduplication:** Implements Map based deduplication across virtualized scrolling to handle 1000+ DOM mutations
- **Computational Complexity:** Calculates adjusted probability distributions with volatility weighting for every matchup pair

### Key Engineering Challenges Solved

**Virtualized Table Scraping at Scale**
Power BI dashboards use infinite scroll virtualization, only rendering visible rows. Built incremental scroll algorithms with configurable step sizes (50px increments), deduplication via hash maps keyed on player names, and validation passes to recover missing data. Handles cross iframe navigation, fallback selectors for evolving DOM structures, and progress logging for monitoring.

**Statistical Model Pipeline Architecture**
Designed a composable probability calculation system: (1) compute adjusted Z score differences using volatility weighted denominators, (2) apply standard normal CDF transformation, (3) convert comparative metrics to implied probabilities, (4) apply statistical corrections to remove systematic bias, (5) calculate confidence deltas as adjusted vs baseline probability differences. Each stage isolated for testability and transparency.

**Confidence Scoring with Variance Adjustment**
Implemented tiered fractional confidence system to manage high variance golf outcomes. Uses 4 tier progressive evaluation based on calculated confidence magnitude, with validation thresholds at each tier. Includes variance modifier (1.5x dampening) for practical interpretation in high volatility scenarios.

**Multi Source Data Reconciliation**
Coordinated player name matching across heterogeneous sources (comparison feeds, Power BI analytics dashboards, DataGolf tournament stats). Built fuzzy matching with whitespace normalization and missing data warnings to surface reconciliation failures early in the pipeline.

**Dynamic Report Generation with Visual Encoding**
Programmatically generates Excel workbooks with conditional formatting (green for positive evaluations, red for filtered results, blue separators), sorted output tables by highest confidence magnitude, and statistical summaries. Implements row based layouts with consistent 7 row per matchup structure for scanning efficiency.

### My Contributions

Owned end to end development of this analytics platform over 5 major iterations:

- **Initial System Design (4181acf):** Architected the dual binary approach (twoballs.go for scraping, generate_matchups.go for analysis) with CSV based handoff between stages
- **Head to Head Generation Logic (e2266f2):** Implemented the core matchup pairing algorithm and probability calculation pipeline with normal distribution transformations
- **Statistical Enhancement (6b1474a):** Added probability adjustment methods to extract true comparative probabilities, improving evaluation accuracy
- **Confidence Scoring Refinement (fb6c6b1):** Evolved from fixed tier approach to sophisticated tiered fractional system with variance dampening and per tier validation
- **Transaction & Bounceback Parsers (b618c5f):** Expanded data ingestion capabilities with JavaScript scrapers for transaction history analysis and bounceback metrics from nested iframe Power BI reports

The codebase demonstrates ownership of statistical modeling, ETL pipeline design, browser automation, and report generation. Work focused on balancing computational rigor (statistical correctness) with practical constraints (execution speed during live tournaments, human readable output formats).
