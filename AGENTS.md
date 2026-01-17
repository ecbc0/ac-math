This code uses Acorn, a theorem proving language.

**Important:** Always run `acorn` after every change to verify proofs, but skip if only comments were added.

## Development Workflow

Before writing a proof, examine at least one Acorn file to understand syntax. For proof by mathematical induction on natural number `ℕ`, study `src/nat/nat_base.ac`, for proof by mathematical induction on `List`, study `src/list/list_base.ac`.

If in an environment without the verifier, use the "acorn-installation" skill to set it up.

Make sure to run the verifier before telling the user you're finished. For large proofs in progress, comment out incomplete parts so user can check your work.


## Code Style Guidelines

### Documentation Style
Every type, typeclass, and attribute should have a doc comment, starting with `///`.

Comments should be written using mathematical language, not using programming language. It's best to use unicode math. You can also use typst (unicode math first, e.g. ℝ instead of RR), but shound not use LaTeX

```acorn
// Good:
/// The smaller of two elements.

// Bad: "returns" is what a programmer would say.
/// Returns the smaller of two elements.

// Good:
/// True if f is continuous everywhere on the reals.

// Bad: "checks" is what a programmer would say.
/// Checks if f is continuous everywhere on the reals.
```

### Import and Module Organization
- Use explicit imports: `from nat.nat_base import ℕ`
- Place `numerals ℕ` after imports when needed
- Organize imports by dependency: basic types first, then type classes, then specific modules
- Import only what's needed - avoid wildcard imports

### Naming Conventions
- Variable names must be lowercase
- Type names use capitalized snake_case: `List`, `Distance_Space`, `ℕ`, `ℝ`
- Function names use snake_case: `add_zero`, `contains`
- Theorem names use snake_case and describe the property: `add_one_right`, `one_plus_one`

### Types and Literals
- Numeric literals need explicit types: `ℕ.0`, `ℝ.0`
- Check if `numerals` declaration already exists before adding
- Use type annotations for function parameters and return types
- Generic types use `[T]` syntax: `List[T]`

### Error Handling and Proof Strategy
- Check if theorem statement is actually true before attempting proof
- Consider factoring out lemmas - ask user if lemma should be separate
- For large proofs: start with outline, fill in details incrementally (partial completion is ok)
- Check for similar existing theorems to leverage

### Code Structure
- Avoid inline lambdas: Define named helpers with explicit parameters. Example: `range_sum(f, m)` then use `range_sum(f)`, not `function(i) { ... }`
- Avoid complex theorem statements: Extract inline `forall`/`exists`/`function` into separate definitions
- Define helper functions to simplify expressions in theorem signatures
- Place definitions and theorems in the most general file that applies
- Example: `List` is in `list_base.ac`, not in `list_sum.ac` where it's used

### Prover Capabilities
- Rarely need to import theorems (prover is powerful)
- Rarely need explicit theorem names in same file
- Use bounded induction pattern: When inducting over bounded ranges with external constraints, induct on the _distance_ to enable automatic induction
- Be explicit with inequalities - Acorn may not automatically prove `n - k >= big_n` from `big_n + k <= n`

### File Organization
- `src/` contains all source files organized by domain
- `src/nat/` - natural numbers and basic arithmetic
- `src/list/` - list data structures and operations
- `src/real/` - real numbers and analysis
- `src/type_class/` - algebraic structures and type classes
- `src/top/` - topology and metric spaces
- `build/` - auto-generated cache directory (never clean)

## Development Tips

**Always run `acorn` after every change, but skip it if only comments were added.**

Never clean the build directory - it acts as a cache to speed up verification.


## Fixing Proofs

When a statement could not be verified, there are two possibilities.

Possibility 1 is that the statement is false. Rewrite the proof so that it does not use false statements.

Possibility 2 is that the statement is too big of a logical leap from the previous statement. Fix this by filling in the missing steps of reasoning, rather than rewriting the entire proof.
