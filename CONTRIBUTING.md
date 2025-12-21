# Contributing to System Topology Manifest (STM)

Thank you for your interest in contributing to the **System Topology Manifest (STM)** project.
This document outlines how to contribute changes, report issues, and propose improvements.

STM is a **descriptive topology layer**. Contributions should preserve that scope.

## Table of Contents

1. [How to Contribute](#how-to-contribute)
2. [Scope Guidelines](#scope-guidelines)
3. [Reporting Issues](#reporting-issues)
4. [Submitting Changes](#submitting-changes)
5. [Community](#community)

### How to Contribute

1. Fork the repository and clone it locally.
2. Create a feature branch:

   ```bash
   git checkout -b feature-name
   ```

3. Make changes following existing structure and schema rules.
4. Validate JSON/YAML against provided schemas.
5. Commit with a clear message:

   ```bash
   git commit -m "Describe topology/schema change"
   ```

6. Push your branch and open a pull request.

Small, focused PRs are preferred.  
Large changes should be discussed before implementation.

### Scope Guidelines

STM is **strictly descriptive**.

Valid contributions include:
- system or application declarations
- signal definitions (inputs, outputs, triggers)
- schema improvements
- documentation clarifications

STM does **not** define:
- architecture rules
- guardrails or enforcement
- coding standards
- runtime or deployment behavior

Those concerns belong to the
**System Architecture Manifest (SAM)**.

### Reporting Issues

When reporting issues:
- Check existing issues first
- Clearly describe the problem
- Include examples or schema references if applicable

### Submitting Changes

- Follow existing schema conventions
- Avoid introducing prescriptive logic
- Update documentation if behavior or meaning changes
- Reference related issues when applicable

### Community

STM is an open project and welcomes thoughtful, scoped contributions.

Thank you for helping keep system topology clear, accurate, and machine-readable.
