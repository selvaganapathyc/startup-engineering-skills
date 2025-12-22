---
name: microservice-reviewer
description: Simple microservice assessment tool for startups. Evaluates services on quantitative metrics (lines of code, endpoints, tech debt score) and qualitative principles (SRP, deployability, testability, ownership, change frequency, bounded context, fault isolation). Use when users paste code/metadata asking for microservice evaluation, health scores, or architectural feedback.
license: MIT
---

# Microservice Reviewer

Simple, focused microservice assessment tool for startup engineering teams.

## Overview

Evaluates microservices using a straightforward scoring system:
- **Quantitative Metrics (60%)**: Lines of code, endpoints, tech debt
- **Qualitative Principles (40%)**: Architecture and operational excellence
- **Total Score**: 100 points with letter grade

## Quantitative Metrics (60 points)

### 1. Lines of Code (20 points)

**Ideal Range**: 2000-5000 lines
- **20 points**: 2000-5000 lines (ideal)
- **15 points**: 1000-2000 or 5000-7000 lines (acceptable)
- **10 points**: 500-1000 or 7000-10000 lines (warning)
- **5 points**: <500 or >10000 lines (critical)

### 2. Number of Endpoints (20 points)

**Ideal Range**: 8-15 endpoints
- **20 points**: 8-15 endpoints (ideal)
- **15 points**: 5-8 or 15-20 endpoints (acceptable)
- **10 points**: 3-5 or 20-30 endpoints (warning)
- **5 points**: <3 or >30 endpoints (critical)

### 3. Tech Debt Score (20 points)

**Formula**: `(Lines/Standard Max) × (Endpoints/Standard Max) × Complexity Factor`

Where:
- **Standard Max Lines**: 5000
- **Standard Max Endpoints**: 15
- **Complexity Factor**: Based on user input or code analysis
  - Simple (1.0): Few conditionals, clean structure
  - Moderate (1.5): Some nested logic, acceptable complexity
  - High (2.0): Heavy nesting, complex logic, many dependencies

**Calculation Example**:
```
Service with 4000 lines, 12 endpoints, moderate complexity:
Tech Debt Score = (4000/5000) × (12/15) × 1.5 = 0.96

Scoring:
- < 0.5: 20 points (low debt)
- 0.5-1.0: 15 points (moderate debt)
- 1.0-1.5: 10 points (high debt)
- > 1.5: 5 points (critical debt)
```

## Qualitative Principles (40 points)

Score each principle on a simple scale:
- **Excellent (6 points)**: Fully implemented
- **Good (4 points)**: Mostly implemented
- **Partial (2 points)**: Somewhat implemented
- **Poor (0 points)**: Not implemented

### 1. Single Responsibility Principle (6 points)

**Question**: Does the service do ONE thing well?
- ✓ Excellent: Clear single purpose, name matches function
- ✓ Good: Primary purpose clear, minor secondary functions
- ⚠ Partial: Multiple related responsibilities
- ✗ Poor: Multiple unrelated domains

### 2. Independent Deployability (6 points)

**Question**: Can you deploy this service without touching other services?
- ✓ Excellent: Deploy anytime, zero dependencies
- ✓ Good: Deploy with minimal coordination
- ⚠ Partial: Requires coordination with 1-2 services
- ✗ Poor: Requires coordinated deployment of 3+ services

### 3. Testability (6 points)

**Question**: How easy is it to test this service?
- ✓ Excellent: >70% test coverage, clear test structure
- ✓ Good: 40-70% coverage, tests exist
- ⚠ Partial: <40% coverage, some tests
- ✗ Poor: No tests or untestable code

### 4. Team Ownership (6 points)

**Question**: Is there clear team ownership?
- ✓ Excellent: Dedicated team, clear ownership, documented
- ✓ Good: Assigned team, mostly clear ownership
- ⚠ Partial: Shared ownership, some confusion
- ✗ Poor: No clear owner, abandoned code

### 5. Change Frequency (6 points)

**Question**: How often does this service change?
- ✓ Excellent: Stable, changes 1-2x per quarter (mature service)
- ✓ Good: Regular changes, 1-2x per month (active development)
- ⚠ Partial: Frequent changes, weekly (scope creep or instability)
- ✗ Poor: Daily changes or no changes in 6+ months (unstable or abandoned)

### 6. Bounded Context (5 points)

**Question**: Does the service own its data domain?
- ✓ Excellent (5 pts): Owns all tables, clear boundaries
- ✓ Good (3 pts): Owns most tables, minor sharing
- ⚠ Partial (1 pt): Shares several tables
- ✗ Poor (0 pts): Shared database anti-pattern

### 7. Fault Isolation (5 points)

**Question**: Are failures contained?
- ✓ Excellent (5 pts): Circuit breakers, timeouts, graceful degradation
- ✓ Good (3 pts): Basic timeouts and error handling
- ⚠ Partial (1 pt): Some error handling
- ✗ Poor (0 pts): Failures cascade to other services

## Assessment Workflow

### Step 1: Gather Information

Ask user for:
1. Lines of code (or analyze if code provided)
2. Number of API endpoints (or count if code provided)
3. Complexity assessment (simple/moderate/high)
4. Answers to qualitative questions

### Step 2: Calculate Quantitative Score (60 points)

```
LOC Score (20) + Endpoints Score (20) + Tech Debt Score (20) = Quantitative Total
```

### Step 3: Calculate Qualitative Score (40 points)

```
SRP (6) + Deployability (6) + Testability (6) + Ownership (6) + 
Change Frequency (6) + Bounded Context (5) + Fault Isolation (5) = Qualitative Total
```

### Step 4: Calculate Final Score

```
Final Score = Quantitative (60 max) + Qualitative (40 max) = Total out of 100
```

### Step 5: Assign Grade

- **A (85-100)**: Excellent - Production ready
- **B (70-84)**: Good - Minor improvements needed
- **C (55-69)**: Acceptable - Significant improvements needed
- **D (40-54)**: Poor - Major refactoring required
- **F (0-39)**: Critical - Needs redesign

## Report Format

```
=== MICROSERVICE ASSESSMENT ===

Service: [name]
Grade: [A/B/C/D/F]
Final Score: XX/100

--- QUANTITATIVE METRICS (XX/60) ---
✓ Lines of Code: XXXX (Score: XX/20)
✓ API Endpoints: XX (Score: XX/20)
✓ Tech Debt Score: X.XX (Score: XX/20)

--- QUALITATIVE PRINCIPLES (XX/40) ---
✓ Single Responsibility: [Excellent/Good/Partial/Poor] (X/6)
✓ Independent Deployability: [Excellent/Good/Partial/Poor] (X/6)
✓ Testability: [Excellent/Good/Partial/Poor] (X/6)
✓ Team Ownership: [Excellent/Good/Partial/Poor] (X/6)
✓ Change Frequency: [Excellent/Good/Partial/Poor] (X/6)
✓ Bounded Context: [Excellent/Good/Partial/Poor] (X/5)
✓ Fault Isolation: [Excellent/Good/Partial/Poor] (X/5)

--- TOP 3 RECOMMENDATIONS ---
1. [Highest impact improvement]
2. [Second priority improvement]
3. [Third priority improvement]

--- SUMMARY ---
[2-3 sentence overall assessment]
```

## Simple Metadata Format

If user can't share code:

```json
{
  "service_name": "user-service",
  "lines_of_code": 3500,
  "num_endpoints": 12,
  "complexity": "moderate",
  "has_tests": true,
  "test_coverage": 65,
  "team_owner": "Platform Team",
  "deploys_independently": true,
  "owns_database_tables": true,
  "has_circuit_breakers": false,
  "change_frequency": "monthly"
}
```

## Quick Assessment Questions

When assessing, ask these simple questions:

**Quantitative**:
1. How many lines of code?
2. How many API endpoints?
3. Complexity level (simple/moderate/high)?

**Qualitative**:
1. What's the single responsibility? (test SRP)
2. Can you deploy without other services? (test independence)
3. What's your test coverage? (test testability)
4. Who owns this service? (test ownership)
5. How often does it change? (test stability)
6. Do you own your database tables? (test bounded context)
7. Do you have circuit breakers/timeouts? (test fault isolation)

## Example Assessment

```
Service: payment-processor
Lines: 3200
Endpoints: 10
Complexity: Moderate

QUANTITATIVE (47/60):
- LOC: 3200 → 20/20 (ideal range)
- Endpoints: 10 → 20/20 (ideal range)
- Tech Debt: (3200/5000)×(10/15)×1.5 = 0.64 → 15/20 (moderate)

QUALITATIVE (30/40):
- SRP: Good (4/6) - Handles payments but also notifications
- Deployability: Excellent (6/6) - Fully independent
- Testability: Good (4/6) - 60% coverage
- Ownership: Excellent (6/6) - Payment Team owns it
- Change Frequency: Partial (2/6) - Changes weekly (unstable)
- Bounded Context: Good (3/5) - Owns tables, shares one with billing
- Fault Isolation: Partial (1/5) - Has timeouts, no circuit breakers

FINAL SCORE: 77/100
GRADE: B (Good)

TOP RECOMMENDATIONS:
1. Add circuit breakers for external payment gateway calls
2. Stabilize change frequency - investigate why changing weekly
3. Extract notification logic to separate service
```

## Tips for Startups

- **Start simple**: Don't optimize prematurely
- **Focus on Grade B+**: Perfect (Grade A) is expensive
- **Track over time**: Monthly assessments show trends
- **Use for learning**: Explain WHY principles matter
- **Be realistic**: Context matters - adjust thresholds for your stage

## When to Reassess

- Before major releases
- After significant refactoring
- Monthly for critical services
- When team changes
- When performance/reliability issues appear
