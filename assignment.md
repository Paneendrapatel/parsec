# Parsec Intern Starter Assignment 🚀

Welcome to the Parsec team! This starter assignment is designed to help you get hands-on with the codebase, set up your development environment, and make your first code contribution.

---

## 🎯 Objectives

1. Clone the repository and set up the local development environment.
2. Verify existing tests pass cleanly.
3. Implement a small, self-contained feature (detailed below).
4. Write comprehensive unit tests for your implementation.
5. Ensure code quality checks (`fmt`, `clippy`, and tests) pass.
6. Submit your work via a Git pull request or branch.

---

## 🛠️ Step 1: Environment Setup

### Prerequisites
- **Git** installed.
- **Rust toolchain** (Rust 1.80+ recommended, managed via [rustup](https://rustup.rs/)).
- **Python 3.10+** (optional, for Python scripts/helpers).

### Clone & Build
1. Clone the repository:
   ```bash
   git clone https://github.com/daseinlabs/parsec.git
   cd parsec
   ```
2. Verify that everything builds and tests pass:
   ```bash
   cargo check --workspace
   cargo test --workspace
   ```
3. (Optional) Run the workspace linter & formatter checks:
   ```bash
   cargo fmt --all --check
   cargo clippy --workspace --all-targets --no-deps -- -D warnings
   ```

---

## 📝 Step 2: The Assignment

Choose **Option A** (Rust / Engine track) or **Option B** (Python / Tooling track).

---

### Option A: Chunk & Prompt Statistics Analyzer (Rust)

#### Goal
Implement a small utility module in `packages/engine` that analyzes a list of text chunks / message strings and produces structured summary statistics.

#### Specifications
Create a new module `packages/engine/src/stats.rs` (and export it in `lib.rs`) containing:

1. A struct `ChunkSummary`:
   ```rust
   #[derive(Debug, Clone, PartialEq, Eq)]
   pub struct ChunkSummary {
       pub total_chunks: usize,
       pub total_chars: usize,
       pub total_words: usize,
       pub estimated_tokens: usize, // Simple heuristic: (chars + 3) / 4 or word-based
       pub max_chunk_chars: usize,
       pub min_chunk_chars: usize,
   }
   ```

2. A function `analyze_chunks`:
   ```rust
   pub fn analyze_chunks<T: AsRef<str>>(chunks: &[T]) -> ChunkSummary {
       // Your implementation here
   }
   ```

3. **Behavior & Edge Cases**:
   - If `chunks` is empty: `total_chunks`, `total_chars`, `total_words`, `estimated_tokens`, `max_chunk_chars`, `min_chunk_chars` should all be `0`.
   - Correctly count words (separated by whitespace).
   - Ignore empty whitespace-only chunks appropriately when calculating metrics if applicable, or document behavior.

4. **Unit Tests**:
   - Write unit tests in `packages/engine/src/stats.rs` (under `#[cfg(test)] mod tests`) covering:
     - Empty slice input.
     - Single chunk with multiple words and special characters.
     - Multiple chunks of varying lengths.
     - Unicode / multi-byte character strings.

---

### Option B: Request Turn Inspector Script (Python)

#### Goal
Create a standalone helper script `scripts/inspect_turns.py` that reads an Anthropic API formatted JSON request payload and prints a clear, human-readable summary of the conversation turns.

#### Specifications
1. Accepts a path to a JSON file as a command-line argument (or reads from `stdin` if `-` or no file is passed).
2. Parses the JSON payload (containing `messages`, `system`, and model parameters).
3. Prints a clean terminal summary:
   - Model name and max tokens.
   - Total number of turns (user vs. assistant).
   - Character count and rough token estimate for each turn.
   - Summary statistics (total characters, total estimated tokens across all turns).
4. Handles malformed JSON or missing fields gracefully with clear error messages.
5. Add a simple test or verification script in `scripts/test_inspect_turns.py` using `pytest` or `unittest`.

---

## 🌟 Bonus (Optional Stretch Goals)

- **For Option A**: Add a helper function `filter_empty_or_whitespace_chunks` or compute a histogram of chunk length buckets (e.g. `<100` chars, `100-500` chars, `>500` chars).
- **For Option B**: Add a `--json` or `--markdown` flag to output the summary as structured JSON or a markdown table.

---

## ✅ Step 3: Verification Checklist

Before submitting, make sure all quality checks pass:

```bash
# 1. Format code
cargo fmt --all

# 2. Check for clippy warnings
cargo clippy --workspace --all-targets --no-deps -- -D warnings

# 3. Run all tests including your new tests
cargo test --workspace
```

---

## 📬 Step 4: Submission

1. Create a new branch:
   ```bash
   git checkout -b intern/<your-name>-assignment
   ```
2. Commit your changes with a clear commit message:
   ```bash
   git add .
   git commit -m "engine: add chunk stats analyzer utility with tests"
   ```
3. Push your branch to your fork and open a Pull Request (or export a patch file with `git format-patch` if instructed).
4. In your PR description, include:
   - A brief overview of what you built.
   - Output from running tests (`cargo test` output).
   - Any design decisions or edge cases you handled.

---

## 💡 Tips & Resources

- Look at existing tests in `packages/engine/tests/` and `packages/engine/src/` for idiomatic Rust examples.
- Don't hesitate to ask questions if you run into any setup issues.
- Have fun exploring the codebase!
