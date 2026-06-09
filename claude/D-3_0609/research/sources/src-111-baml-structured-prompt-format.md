# src-111: BAML – Boundary AI Markup Language (Structured Prompt Format)

**URL:** https://github.com/BoundaryML/baml  
**Title:** GitHub – BoundaryML/baml: The AI framework that adds the engineering to prompt engineering  
**AccessDate:** 2026-06-09  
**Related section:** Structured Prompt File Format / Type-Safe Templates / Harness Analogy

---

## Relevant Excerpt

### What BAML Is
BAML (Boundary AI Markup Language) is a domain-specific language (DSL) for defining LLM functions with strict input/output typing. It transpiles into native Python, TypeScript, Ruby, Java, C#, Rust, and Go code — providing autocomplete and static analysis without additional dependencies.

### Core Concept: Prompts as Typed Functions
"The fundamental building block in BAML is a function. Every prompt is a function that takes in parameters and returns a type."

```baml
function ExtractOrderInfo(ticket: string) -> OrderInfo {
  client GPT4
  prompt #"
    Extract the order information from this customer ticket:
    {{ ticket }}
    
    Return the result as JSON matching the schema.
  "#
}

class OrderInfo {
  order_id string
  customer_name string
  issue_summary string
}
```

### Asset-Management Value
BAML's primary value is organizational: it reduces prompt sprawl by centralizing prompts as code, creating clear separation between AI logic and application code while enabling reuse across projects and languages.

When prompts are functions with clear signatures, they become reusable components — different teams can consume the same typed interfaces without worrying about implementation details.

### Harness Analogy
BAML `.baml` files are a concrete example of a "harness" concept: they bundle the prompt template + input schema (typed parameters) + output schema (typed return value) + model client configuration into a single, reusable file that can be version-controlled.

The `baml dev` hot-reload and LSP integration treat `.baml` files like first-class code artifacts, enabling the same engineering rigor (find references, unit tests, type checking) that software developers apply to regular code.

### Key Distinction vs. Plain Prompts
BAML makes prompt engineering "schema engineering" — mostly focusing on the models (types) of your prompt to get more reliable outputs. This aligns with the D-3 topic's emphasis on STRUCTURE and SCHEMA, not clever wording.
