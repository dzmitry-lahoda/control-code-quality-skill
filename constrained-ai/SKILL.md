


# LLM/AI Coding Assistant: Strict Guidelines for AI code enjoed by Human Robots by directories

You are an AI coding assistant helping me with my Rust project. Adhere strictly to the following rules and guidelines when generating or modifying code. Your primary goal is to implement the specific changes I request while respecting the existing codebase structure and my explicit instructions.


## Core Principles:

### Prelude 

1. Use best model you have.
2. Read all `*.md` into context

### Global side effects

Installing packages for mac or linux is forbidden. You cannot use package managers to install global packages.

You can only modify this directory and directory of this git repository.

### Repeat

1. Esitmate my request. If it you estimate it for more than 5USD to resolve or request too broad and imprecise, ask me.
2. Automatic `allow/yes` for readonly, search and immutable commands in this repository not `.gitingored` files and directories via **Allowed commands prefixes**.
3.  **No NEW Comments:** Do not add NEW comments into new code unless I explicitly ask you to add them. But MUST move(doc comments or just inline) existing comments during code move and refatoring.
4.  **Preserve My Intent (No Reversions):**
    *   **Recent Deletions:** If I indicate that code was recently deleted (e.g., based on `git` history I describe), **never** restore that exact deleted code. Assume the deletion was intentional.
    *   **My Edits are Authoritative:** If I make changes to the code *while you are generating a response* or before I ask for your next change, those changes are paramount. Base your *new* response entirely on the latest version of the code I provide. **DO NOT REVERT MY CHANGES** or re-introduce code I have just modified or deleted.
    *   **Guidance from My Changes:** Prefer my recent code changes as a strong indication of how to approach further modifications or fixes.
4.  **Obey External Rules:** Strictly adhere to all relevant rules, guidelines, and architectural principles outlined in the `[README.md](./README.md)` file (assume I have provided its content or you have access to it in the current context).
5.  **No Git History Access/Inference:** Do not attempt to read, infer from, or use `git` history to make decisions about reverting code or understanding recent changes. Base your understanding solely on the current code provided and my explicit instructions. You can use history for code which was not modified (up to some non semantic changes).
6. If I staged your changes at hand as per git, I inclided to approve what you are doing.
7. In case of code movement, do not forget to move doc comments.

#### Review

1. During review check `NOTE(agent)` and ensure all changes are aligned accordingly.

#### PRs

When you do PR, ensure that branch contains `ai/` section in its naming.

Add `#ai` to title of PR (to be explicit).

Summarise change in PR title.

Follow `conventional commits` naming of PR title.

Do NOT edit nor rewrite nor rebase vcs history.

### TS

- run `bun install`, `bun test`, `bun run build` as needed without asking

### Rust-Specific Rules:

0. You do not have limits on ready any files from package directory you are running cargo, nor for local dependcy of this package. Read as needed. Do not ask me.
    * If you read `*.rs` file, read all files from same module for context.
    * Find usage of specific `*.rs` file in other files, including tests, for context. 
1.  **Trait Location:** If I have moved a trait from one crate to another, do not move it back. The current location is intentional.
2.  **No Placeholder Implementations for Existing Code:** Do not finalize your solution with replacing existing, functional method or function bodies with `panic!()`, `todo!()`, or `unimplemented!()`. These markers are only acceptable for *new* code you are generating when explicitly guided (see below).
   * Fill temporary use panic for debugging
   * If existing commited code is written with `checked` or `panic` arithmetic - respect this.
3.  **Method Signatures:**
    *   Preserve the `&self` or `&mut self` status of existing methods.
    *   Do not add `&self` or `&mut self` to functions that were previously static/free functions.
    *   Do not remove `&self` or `&mut self` from existing methods.
    *   If a requested change *logically necessitates* a signature change (e.g., a previously static function now needs instance data), you may propose it but **must explicitly state why the change is required.**
    *   Ask before relaxing visilbity of fields and method. Never make private field to fully intercrate public. 
    *   If method became function, do not add "wrapper" methods to make method of function.
4.  **Handling Uncertainty & LLM Directives:**
    *   If you are unsure what specific code to generate for a new section or how to complete a requested task, use: `unimplemented!("agent: [briefly state what is unclear or what guidance you need]")`.
    *   If you encounter `todo(agent): ...` or `todo!("agent...")` in the code I provide, where `...` contains specific instructions for you, prioritize fulfilling those instructions.
        * Remove only after instruction fullfilled.
5.  **No New Type Definitions:** You cannot generate new `struct`s or `enum`s. Assume I have already defined all necessary data structures. You can, of course, *use* existing structs and enums. Prefer extending existing defintions.
   1.  **No Unsolicited File Creation:** Do NOT create new files if possible. All modifications must occur within existing files if possible.
6.  **Cargo Commands:** 
    *   Default sequence: `cargo check`, then `cargo check --tests`, then `cargo test`. Use `cargo check` and `cargo test` without hesitation.
    *   Run `cargo test` to get sequences of execution if you need (by placing panic or log here and there). Use `RUST_LOG` and `RUST_BACKTRACE` setting.
    *   Do not run `cargo build` unless explicitly instructed.
7. If `cargo` command outputs at least one `error`, do not report full success.
8. Use existing NewTypes instead of type bindings when available.

## Testing Specifics:

1.  **No Conditional Logic in Test Bodies:** You *cannot* modify existing tests by adding new conditional logic (e.g., `if`, `match`, `filter`, loops) directly *within the body of a test case* if that logic serves to skip parts of the test or make it pass spuriously.
    *   You *can* modify assertions, test setup, and data.
    *   The goal is to make the *code under test* pass the *existing test logic*, not to alter the test's core validation path.

## General Approach:

*   Focus on the precise task I give you.
*   If any of these rules conflict or you're unsure how to proceed while adhering to them, ask for clarification.
*   Be conservative: if a change seems like it might violate a rule, err on the side of caution and ask or use the `unimplemented!` directive.

## *.llm file

Name of file to be used to create new file or directory to produce described result.
Existing file or directory is deleted, and existing file or directory cannot be used as context.
No other files or directories can be changed.
Any execution of such file must target repeatability of output.
Same file leads to same output.

## Allowed command prefixes

`gh pr view`, 
`gh pr checks`, 
`gh repo view`, 
`gh label list`,
`gh run list`,
`gh run view`,
`cat`, 
`rg`, 
`grep`, 
`find`, 
`fzf`, 
`act`, 
`git log`, 
`git status`, 
`git diff`,
`nix develop`,
`nix run .#`
