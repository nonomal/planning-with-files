# Git Checkpoint Workflow

> **Pattern from Anthropic:** This workflow is a direct implementation of the "git commits as checkpoints" pattern described in Anthropic's article, [Effective harnesses for long-running agents][1].

## The Core Principle: One Phase, One Commit

The Git Checkpoint workflow treats your git history as a series of reliable "save points" for your project. Each commit marks the successful completion of a single phase from your `task_plan.md`.

This creates a clean, verifiable, and easy-to-navigate history of your project. If you make a mistake in a later phase, you can always roll back to the last known-good state.

> "We found that the best way to elicit this behavior was to ask the model to commit its progress to git with descriptive commit messages. This allowed the model to use git to revert bad code changes and recover working states of the code base."
> — Anthropic Engineering [1]

## The Workflow

Follow these steps for every phase in your `task_plan.md`.

### Step 1: Complete the Phase

Do all the work required for the current phase. This includes writing code, running tools, creating files, and conducting research.

### Step 2: Verify the Phase

**This is the most critical step.** Before committing, you must verify that the phase is truly complete and everything works as expected. Prematurely marking a phase as complete is a common failure mode for AI agents.

1.  Run tests (unit tests, integration tests, manual verification).
2.  Fill out the `Test Results` table for the current phase in `task_plan.md`.
3.  Ensure all tests pass.

### Step 3: Update the Task Plan

Once verified, update your `task_plan.md`:

1.  Change the phase **Status** to `✅ Pass`.
2.  Fill in the `Test Results` table with the outcomes.

### Step 4: Stage and Commit

Now, create your git checkpoint.

1.  **Stage all your changes.** This includes the updated `task_plan.md` and any other files you created or modified.

    ```bash
    # Stage all changes in the project
    git add .
    
    # Or, stage specific files
    git add src/feature.js tests/feature.test.js task_plan.md
    ```

2.  **Commit with a descriptive message.** The commit message should clearly state which phase was completed and what was achieved.

    ```bash
    # Format: type(Phase X): Description of accomplishment
    git commit -m "feat(Phase 2): Implement user authentication endpoint"
    ```

    **Commit Message Best Practices:**
    - Use a standard prefix (e.g., `feat`, `fix`, `docs`, `chore`).
    - Reference the phase number (e.g., `(Phase 3)`).
    - Write a concise, clear summary of what was done.

### Step 5: Log the Checkpoint

1.  Get the hash of your latest commit:

    ```bash
    git log -1 --pretty=%h
    # Example output: a1b2c3d
    ```

2.  Copy this hash into the `Git Checkpoints` table in your `task_plan.md` for the phase you just completed.

    ```markdown
    ## Git Checkpoints
    | Phase | Commit Hash | Message                                       | Date       |
    |-------|-------------|-----------------------------------------------|------------|
    | 1     | 9f8e7d6     | chore: Initialize planning files              | 2026-01-17 |
    | 2     | a1b2c3d     | feat(Phase 2): Implement user auth endpoint   | 2026-01-17 |
    ```

Your checkpoint is now complete and logged.

## Example: Completing Phase 2

Let's say you just finished implementing the project structure.

1.  **Verify**: You check that all the directories and files exist as planned.
2.  **Update Plan**: In `task_plan.md`, you update Phase 2:
    - `Status:` ⏸️ Pending → `✅ Pass`
    - `Test Results:` You fill in the table showing the file structure was verified.
3.  **Stage & Commit**:
    ```bash
    git add task_plan.md src/ db/ migrations/ .env.example
    git commit -m "feat(Phase 2): Set up initial project structure and database config"
    ```
4.  **Log Checkpoint**: You run `git log -1 --pretty=%h`, get `a1b2c3d`, and add it to the `Git Checkpoints` table in `task_plan.md`.

## How to Roll Back to a Checkpoint

If you start Phase 3 and realize you've made a major mistake, you can easily revert to your last known-good state (the end of Phase 2).

1.  **Find your checkpoint hash** in the `Git Checkpoints` table in `task_plan.md`.

2.  **Ensure your working directory is clean.** You can either commit or stash your current, broken changes.

    ```bash
    # Option A: Stash (save) your broken changes temporarily
    git stash
    
    # Option B: Discard all your current changes (DANGEROUS!)
    git reset --hard
    ```

3.  **Reset to the checkpoint**:

    ```bash
    # This command reverts your project to the exact state of the commit
    git reset --hard a1b2c3d
    ```

Your project is now back in the clean, verified state it was in at the end of Phase 2. You can now restart Phase 3 from a solid foundation.

## Summary of Commands

| Action | Command | When to Use |
|---|---|---|
| View history | `git log --oneline -10` | To see recent checkpoints. |
| See current changes | `git status --porcelain` | To see what you've done in the current phase before committing. |
| Create checkpoint | `git commit -m "..."` | After a phase is complete & verified. |
| Rollback to checkpoint | `git reset --hard <hash>` | When a phase goes wrong and you need to start over from a known good state. |

---

### References

[1] Anthropic. (2025, November 26). *Effective harnesses for long-running agents*. https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents
