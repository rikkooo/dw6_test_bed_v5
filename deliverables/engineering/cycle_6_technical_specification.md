
# dw6_test_bed_v5 - Cycle 6 - Technical Specification

**Date:** 2025-06-25

## 1. Overview

This cycle aims to significantly improve the robustness and automation of the DW6 development workflow. The primary goal is to fix critical bugs found during the initial setup and usage of the protocol, focusing on environment creation, dependency installation, CLI command stability, and GitHub integration. This will deliver immediate value by making the workflow more reliable and developer-friendly.

## 2. Scope

### In Scope

- Replace the standard `venv`/`pip` setup with `uv` for environment and package management.
- Integrate the `github-mcp-server` to fully automate GitHub repository creation and linking.
- Permanently fix the `AttributeError` and silent output bugs in the `dw6 status` command.
- Permanently fix the `KeyError` bug in the `dw6 engineer start` command.

### Out of Scope

- Any changes to the core state transition logic (beyond fixing the CLI commands).
- Introducing new stages or modifying the purpose of existing stages.
- UI/UX enhancements not directly related to the CLI command fixes.

## 3. System Architecture

The changes will be confined to the DW6 project setup scripts and the core `dw6` Python package, specifically the `cli.py` and `state_manager.py` modules. The interaction with external systems involves using `uv` as a command-line tool and making API calls to the `github-mcp-server`.

## 4. Data Model

No changes to the data model or the `workflow_state.txt` format are required. The `KeyError` fix involves correctly referencing an existing key (`RequirementPointer`) rather than adding a new one.

## 5. Functional Requirements (User Stories)

- **US-01:** As a developer, I want the project setup to reliably create a virtual environment and install all dependencies so that I can start working on the project without manual intervention.
  - **Acceptance Criteria 1:** The setup process uses `uv` to create a virtual environment.
  - **Acceptance Criteria 2:** The setup process uses `uv` to install all dependencies from `pyproject.toml`.
- **US-02:** As a developer, I want the project setup to automatically create a GitHub repository and link it to my local project so that I don't have to perform these steps manually.
  - **Acceptance Criteria 1:** The workflow uses an MCP server to create a new GitHub repository.
  - **Acceptance Criteria 2:** The local repository is correctly configured with the new remote URL.
- **US-03:** As a developer, I want to run `dw6 status` and see a clear, accurate representation of the project's current state so that I know what stage I am in.
  - **Acceptance Criteria 1:** The command executes without error.
  - **Acceptance Criteria 2:** The command prints the current stage and requirement pointer.

## 6. Implementation Plan

*A numbered, logical list of tasks for the Coder. This is the direct input for the next stage.*

1. **Task:** Modify the `start-project-dw6.md` workflow to use `uv venv` and `uv pip install` instead of `python -m venv` and `pip`.
2. **Task:** Update the `start-project-dw6.md` workflow to use the `github-mcp-server` to create and link the remote repository, removing the manual step.
3. **Task:** Verify the fix in `src/dw6/cli.py` that changes the `manager.get_status()` call to `manager.get_state()`.
4. **Task:** Verify the fix in `src/dw6/cli.py` that prints the output from the `get_state()` method.
5. **Task:** Verify the fix in `src/dw6/cli.py` that corrects the `KeyError` by using `state['RequirementPointer']`.

## 7. Questions & Assumptions

- **Question:** None at this time.
- **Assumption:** The user's environment has `uv` installed and the `github-mcp-server` is accessible and configured with valid credentials.
