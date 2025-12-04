# Stable Denotations: Effects as Interactions

This notebook explores **stable denotations** and **extensible effects** in Rust, following the [okmij blog](https://okmij.org/ftp/Computation/having-effect.html).
Our goal is to write an interpreter for a simple (functional) programming language in an extensible way, i.e., as new features are added existing definitions need not be modified.
We will do this through different experiments and (re)discover Effects and Monads.
After this tutorial, we recommend reading the original to reinforce the concepts and also learn how to incorporate first-class function in the language, which is not covered here.

Our journey will cover the following:

| Interpreter | Description |
|-------------|-------------|
| I-0         | Traditional definitional interpreter for integers and increment (`Expr`, `Dom`, `eval`). |
| I-1         | Extended interpreter adding booleans, equality, and conditionals by redefining syntax and eval. |
| I-2         | Tagless-final style core language (`EBasic`) with abstract domain and reusable terms like `ttinc`. |
| I-2+        | Compositional extension adding conditionals (`ECond`) without changing existing definitions. |
| I-3         | State-threading interpretation using explicit state transformers (`DomIntState`). |
| I-4         | Effects-as-interactions with request/handler model over computations `Comp<ReqT>`. |
| I-4s        | I-4 extended with a sequencing operator (`seq`) built from `bind` for effectful composition. |
| I-4s+       | I-4s with conditionals reintroduced over the computation monad. |

## Setup

To run this notebook, you will need to install Rust and notebook kernel. Do the following:
1. Install Rust,
2. install EvCxR (`cargo install evcxr_jupyter`),
3. add Rust kernel to Jupyter (`evcxr_jupyter --install`), and,
4. select Rust kernel in the Jupyter notebook (e.g, top right corner of notebook in VSCode).

> [!NOTE]
> Sometimes the EvCxR state might become corrupt. Restarting the kernel and running cells again solves the problem.
