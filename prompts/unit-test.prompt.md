---
mode: "agent"
description: "Unit test generation in your project according to defined instructions"
---

## Role

You're a senior expert software engineer with extensive experience in writing and maintaining unit tests for various programming languages and frameworks.

## Objective

Analyze the current project’s test suite to understand the existing testing setup and generate comprehensive test cases for upcoming code enhancements, following the current testing style and tooling.

## Tasks

### 1. Analyze Existing Tests

Review the project files, especially:

- `tests/`, `test/`, or `__tests__/` directories
- Configuration files like `package.json`, `pyproject.toml`, `jest.config.js`, etc.

And identify the following:

- **Testing Framework(s)** used (e.g., Jest, Mocha, PyTest, JUnit, etc.)
- **Testing Utilities/Tools** (e.g., Sinon, Enzyme, FactoryBoy, Mockito, Testcontainers, etc.)
- **Testing Style**:
  - Unit tests
  - Integration tests
  - End-to-end tests
  - Or a combination
- **Test File Structure** and naming conventions
- **Coverage Tools** and how coverage is tracked and reported
- **Example Patterns**: Summarize the structure of 1–2 typical test cases

### 2. Evaluate Test Coverage

- Determine if coverage thresholds are enforced
- Identify where reports are stored (e.g., `coverage/`, HTML reports, CI outputs)
- Note any test gaps or missing coverage (e.g., missing branch testing or error conditions)

### 3. Generate New Tests

Based on the analysis:

- Use the **same tools, frameworks, and style**
- Write **comprehensive, maintainable, and idiomatic test cases**
- Cover new logic, edge cases, and integration points for modified or added code

## ✅ Output Format

Provide:

1. **Summary of Current Test Suite** (frameworks, tools, structure, style, coverage)
2. **Generated Test Cases** for the given code enhancement (matching the existing conventions)

---

## 🧠 Notes for Agent

- Maintain consistency with existing test patterns
- Use descriptive test names and setup/teardown patterns if applicable
- Include mocks/stubs/fakes if the project uses them
- Ensure generated tests can be run with existing test commands (e.g., `npm test`, `pytest`, etc.)
