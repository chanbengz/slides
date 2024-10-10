---
layout: section
---
# About Rust
`rustc` is what makes Rust great!

---
level: 2
---
# Why Rust?
When we talk about Rust, we refer to words like **memory safety**, **efficiency**, **expressiveness**, **open-source** and so on... Nowadays, when we talk about system safety, we can't avoid mentioning Rust.
### What makes Rust different from other languages?
Rust Compiler `rustc` may give you the answers.
<v-click> 
<li> 🛠 It has a very powerful frontend system whereas the backend is backed up by the famous <span v-mark.underline.orange="1">LLVM</span>. (The same backend for compilers like clang/intel DPC/C++ compilers)</li>
<li> 🤔 The explicit definition of ownership enables the compiler to perform <span v-mark.underline.orange="2">static analysis</span> on the code, resolving issues only using frontend. </li>
</v-click>
---
level: 2
---
# Compiling Rust
<v-switch>
  <template #1>
    <h3> A general compiler flow </h3>
    <br>
    <div class="flex flex-row items-center justify-center">
```mermaid {scale: 0.5, alt: 'A simple sequence diagram'}
%%{init: {"flowchart": {"htmlLabels": false}} }%%
flowchart LR
    A[Invocation]
    B[Lexical Analysis]
    C[Syntax Analysis]
    D[Semantic Analysis]
    E[Intermediate Representation]
    F[Optimization]
    G[Code Generation]
    H[Linking]
    I[Executable]
    A --> **Frontend**
    subgraph **Frontend**
      direction TB
        B --> C
        C --> D
        D --> E
    end
    **Frontend** --> **Backend**
    subgraph **Backend**
      direction TB
        F --> G
        G --> H
    end
    **Backend** --> I
```
    </div>
  </template>
  <template #2>
    <h3> Rust Compiler Flow </h3>
    <br>
    <div class="flex flex-row items-center justify-center">
```mermaid {scale: 0.4, alt: 'A simple sequence diagram'}
%%{init: {"flowchart": {"htmlLabels": false}} }%%
flowchart LR
    %% Main Stages %%
    A[Invocation] --> B[Lexing]
    B -->|TokenStream| C[Parsing]
    C -->|AST| D[HIR]
    D -->|Type Inferencing
      Type Checking| E[MIR]
    E -->|Borrow Checking
      Optimization| F[LLVM IR]
    F -->|Backend| G[LLVM Backend]
    G --> H[Executable]

    %% Frontend %%
    subgraph Frontend [**Frontend**]
        B
        C
        D
        E
        F
    end

    %% Backend %%
    subgraph Backend [**Backend**]
      direction TB
        G
    end

    I[Procedural Macros] --> |TokenStream| M{Type?}
    M --> |Procedural Macro| N[AST]
    N --> O[TokenStream]
    M --> |Declarative Macro| O[TokenStream]

    %% Macro Handler %%
    subgraph MacroHandler [**Macro Handler**]
      direction TB
        I
        M
        N
        O
    end

    %% Connect Macro Handlers to Main Flow %%
    MacroHandler --> C
```
    </div>
  </template>
</v-switch>