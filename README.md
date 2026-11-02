# Bulk-Cargo-Transportation-Optimization

> A Mixed-Integer Linear Programming (MILP) case study for cost-effective coastal vessel allocation and bulk-cargo transportation.

**Project team:** Team VisionX  
**Case study:** HP Power Lab 2.0 — Coastal Vessel Optimization  
**Repository:** https://github.com/bibekn414/Bulk-Cargo-Transportation-Optimization

## Table of Contents

- [Project Overview](#project-overview)
- [Business Problem](#business-problem)
- [Network and Input Data](#network-and-input-data)
- [Operational Constraints](#operational-constraints)
- [Optimization Methodology](#optimization-methodology)
- [Objective Function](#objective-function)
- [Reported Results](#reported-results)
- [Business Value](#business-value)
- [Repository Contents](#repository-contents)
- [Reproducibility and Future Improvements](#reproducibility-and-future-improvements)
- [Data Validation Notes](#data-validation-notes)
- [Team](#team)

---

## Project Overview

This project develops an optimization approach for planning coastal tanker movements in HPCL's maritime logistics network. The objective is to allocate vessels to loading and unloading ports, determine feasible trip sequences, fulfil unloading-port demand, and minimize total charter-hire transportation cost.

The case study considers a fixed fleet of **nine coastal tankers**, **six loading ports**, and **eleven unloading ports**. The proposed Mixed-Integer Linear Programming (MILP) framework considers direct deliveries and trips with two discharge ports.

### Project at a glance

| Parameter | Case-study configuration |
|---|---:|
| Loading ports | 6 (L1–L6) |
| Unloading ports | 11 (U1–U11) |
| Tankers | 9 (T1–T9) |
| Tanker capacities | 50,000 MT and 25,000 MT |
| Permitted discharge pattern | One or two ports per trip |
| Main cost driver | Daily charter hire × trip duration |
| Optimization approach | Mixed-Integer Linear Programming |
| Reported transportation cost | **₹3.4009 crore** |

## Business Problem

Coastal tanker allocation is a fleet-planning problem. Each tanker must be assigned to trips with a defined loading port, discharge port or ports, cargo quantity, and trip sequence. Vessel capacity, charter-hire rates, sailing times, and port demand influence the total transportation cost.

The challenge is to develop a transportation plan that:

1. Meets the demand specified at every unloading port.
2. Loads cargo from only one loading port per trip.
3. Allows a maximum of two unloading ports on a complete trip.
4. Accounts for sailing time from the loading port to the first unloading port and, where applicable, between unloading ports.
5. Minimizes the total charter-hire cost of the trips.

## Network and Input Data

The case-study input tables contain:

- Six loading ports (L1–L6), with loading availability treated as unlimited.
- Eleven unloading ports (U1–U11), each with a specified demand.
- Nine tankers with vessel-specific capacities and daily charter-hire rates.
- A loading-port-to-unloading-port trip-time matrix.
- An unloading-port-to-unloading-port sailing-time matrix.

### Fleet information

The following values are transcribed from the case-study input table. The source labels the charter-hire column as **Rs Cr/day**.

| Tanker | Capacity (MT) | Charter hire (Rs Cr/day) |
|---|---:|---:|
| T1 | 50,000 | 0.63 |
| T2 | 50,000 | 0.49 |
| T3 | 50,000 | 0.51 |
| T4 | 50,000 | 0.51 |
| T5 | 50,000 | 0.53 |
| T6 | 50,000 | 0.57 |
| T7 | 50,000 | 0.65 |
| T8 | 25,000 | 0.39 |
| T9 | 25,000 | 0.38 |

### Unloading-port demand

| Unloading port | Demand (MT) |
|---|---:|
| U1 | 40,000 |
| U2 | 135,000 |
| U3 | 5,000 |
| U4 | 20,000 |
| U5 | 20,000 |
| U6 | 20,000 |
| U7 | 110,000 |
| U8 | 30,000 |
| U9 | 20,000 |
| U10 | 20,000 |
| U11 | 20,000 |
| **Total of listed values** | **440,000** |

The trip-time tables in the case-study document provide sailing durations for each loading-to-unloading pair and each unloading-to-unloading pair. These matrices are used to calculate the duration of candidate trip sequences.

## Operational Constraints

The case study specifies the following mandatory operating rules:

1. **Demand satisfaction:** All demand at the eleven unloading ports must be met.
2. **Single loading port per trip:** Each tanker loads from only one loading port during a trip.
3. **Full-load rule:** The stated rule requires a tanker to load its full capacity from that loading port.
4. **Maximum two discharge ports:** A complete trip may serve one or two unloading ports.
5. **Fleet assignment:** The model assigns the available nine tankers to trips and specifies their route sequences and cargo quantities.
6. **Unlimited loading availability:** Loading-port cargo availability is treated as non-limiting.

## Optimization Methodology

The project presentation describes the following model workflow.

### 1. Input data preparation

Organize unloading-port demands, tanker capacities, charter-hire rates, and voyage-time matrices. These inputs define the fleet and transportation network.

### 2. Candidate route generation

Consider the two route patterns permitted by the case study:

- **Single-drop route:** `L → U`
- **Two-drop route:** `L → U1 → U2`

For a two-drop route, the trip duration includes the loading-to-first-discharge time and the sailing time between the two unloading ports.

### 3. MILP formulation

Represent tanker allocation and trip decisions using a Mixed-Integer Linear Programming framework. The objective captures charter-hire cost, while constraints represent demand fulfilment and operating rules.

### 4. Optimization

Evaluate feasible combinations of tanker assignments, routes, trip frequencies, and cargo quantities to identify a minimum-cost plan.

### 5. Output generation

The expected model output includes:

- Tanker assigned to each trip.
- Loading port and discharge sequence.
- Number of trips.
- Cargo delivered to each unloading port.
- Trip-level transportation cost.
- Total transportation cost for the system.

### Conceptual workflow

```text
Input data
    |
    v
Data preparation
    |
    v
Candidate route generation
    |
    v
MILP formulation
    |-- Cost objective
    |-- Demand constraints
    |-- Vessel and route constraints
    |
    v
Optimization
    |
    v
Tanker allocation and trip schedule
    |
    v
Cargo delivery and transportation-cost report
```

## Objective Function

The case study calculates the transportation cost of a trip as the product of the tanker's daily charter-hire rate and the total number of days consumed by the trip.

For tanker `t` operating trip `r`:

\[
C_{t,r} = H_t \times D_{t,r}
\]

where:

- \(C_{t,r}\) = transportation cost of trip `r` by tanker `t`
- \(H_t\) = daily charter-hire rate of tanker `t`
- \(D_{t,r}\) = total trip duration in days

The system-level objective can be expressed conceptually as:

\[
\boxed{\text{Minimize } Z = \sum_t \sum_r H_t D_{t,r}}
\]

subject to the demand, loading, capacity, route, and fleet-allocation requirements.

**Formulation note:** This is the objective expressed from the case-study description. The complete algebraic MILP formulation, decision-variable definitions, and solver implementation are not included in the supplied presentation materials, so they are not claimed here.

## Reported Results

The Team VisionX presentation reports a total transportation cost of:

# ₹3.4009 Crore

The following tanker-wise allocation is transcribed from the result table in the presentation.

| Tanker | Capacity (MT) | Loading port | Route sequence | Trips | Delivered cargo (MT) | Reported cost (₹ crore) |
|---|---:|---|---|---:|---:|---:|
| T1 | 50,000 | L1 | L1 → U1 → U9 | 1 | 50,000 | 0.3087 |
| T2 | 50,000 | L2 | L2 → U2 | 3 | 135,000 | 0.8820 |
| T3 | 50,000 | L3 | L3 → U3 → U10 | 1 | 25,000 | 0.3519 |
| T4 | 50,000 | L4 | L4 → U4 → U5 | 1 | 40,000 | 0.2142 |
| T5 | 50,000 | L5 | L5 → U8 | 1 | 30,000 | 0.2120 |
| T6 | 50,000 | L6 | L6 → U6 → U11 | 1 | 40,000 | 0.4731 |
| T7 | 50,000 | L2 | L2 → U7 | 2 | 100,000 | 0.6500 |
| T8 | 25,000 | L2 | L2 → U7 | 1 | 10,000 | 0.1950 |
| T9 | 25,000 | L1 | L1 → U9 | 1 | 10,000 | 0.1140 |
| | | | **Total** | **12** | **440,000** | **3.4009** |

*The table reflects the allocation and cost values displayed in the project presentation. The total trip count is the sum of the tanker-wise trip frequencies shown there.*

### Result interpretation

- The reported plan assigns all nine tankers.
- Both direct-delivery and two-discharge-port routes are represented.
- Repeated trips are assigned to T2 and T7 to serve the larger unloading-port requirements.
- The tanker-wise cost entries add up to **₹3.4009 crore**.
- The delivered cargo entries add up to **440,000 MT**, matching the sum of the individual unloading-port demands listed in the input table.

## Business Value

The proposed optimization framework can support maritime logistics planning through:

- **Cost-aware scheduling:** Evaluating vessel and route combinations using charter-hire rates and trip durations.
- **Demand fulfilment:** Providing a structured allocation of cargo to unloading ports.
- **Fleet utilization:** Showing how vessels of different capacities are assigned across routes.
- **Operational clarity:** Reporting tanker-wise loading ports, discharge sequences, trip frequencies, and cargo quantities.
- **Scalability:** Providing a framework that can be extended to additional ports, vessels, or operational restrictions when the corresponding data and model logic are incorporated.

## Repository Contents

The project materials supplied for this case study include:

| Material | Description |
|---|---|
| Case-study PDF | Problem statement, input tables, and transportation data |
| Team VisionX presentation | Optimization workflow, allocation results, and business impact |

Update this section with the exact filenames present in the GitHub repository if they differ from the names above.

## Reproducibility and Future Improvements

**Current documentation limitation:** The supplied project materials describe the problem, input data, methodology, and reported result. They do not provide enough implementation detail to independently reproduce the optimization.

For a reproducible technical repository, consider adding:

- MILP model source code.
- Input datasets for port demand, vessel characteristics, charter rates, and voyage-time matrices.
- Solver name and version, with required packages.
- Instructions for environment setup and model execution.
- Scripts or output files that generate the tanker allocation and cost summary.
- Validation checks for demand fulfilment, vessel capacity, and route feasibility.

No programming language, solver package, or software stack is claimed here because these details are not established by the supplied case-study materials.

## Data Validation Notes

Two points in the source presentation should be reconciled before treating the result as a fully validated operational plan.

1. **Total demand discrepancy:** The strategic-impact slide states a total demand of **455,000 MT**. However, the eleven individual unloading-port demand values in the input table sum to **440,000 MT**. The allocation table also reports **440,000 MT** delivered.
2. **Full-load rule versus reported allocation:** The stated operating rule requires a tanker to load its full capacity. However, the result table includes delivered quantities below the listed tanker capacity—for example, 40,000 MT for a 50,000 MT tanker and 10,000 MT for a 25,000 MT tanker. The intended interpretation of the full-load rule and the treatment of cargo over repeated trips should be confirmed.

These discrepancies are documented transparently rather than silently changing the source figures.

## Team

**Team VisionX**

- Chidambar Swain
- Himansu Nahak
- Tusharkant Gardia
- Bibek Nayak

## Acknowledgement

Developed as part of the **HP Power Lab 2.0** case-study challenge on optimizing maritime logistics and trade operations.

---

*This repository documents an academic case-study optimization exercise. The reported cost is the result presented by the project team; it should not be interpreted as a verified live operating cost or a production deployment.*
