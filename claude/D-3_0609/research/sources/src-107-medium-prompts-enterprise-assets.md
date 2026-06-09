# src-107: Medium / Segev Shmueli – Managing Prompts as Enterprise Assets

**URL:** https://medium.com/@segev_shmueli/managing-prompts-as-enterprise-assets-a-portfolio-approach-e9552e24f327  
**Title:** Managing Prompts as Enterprise Assets: A Portfolio Approach  
**AccessDate:** 2026-06-09  
**Related section:** Enterprise Governance / Portfolio Classification / Versioning Conventions

---

## Relevant Excerpt

### Enterprise Context
78% of organizations now use AI in at least one business function (McKinsey 2024). Yet most organizations treat prompts as disposable scripts rather than strategic assets requiring systematic management.

### Portfolio Classification Framework
**By Business Impact:**
- Revenue-Critical: Customer-facing interactions, sales automation
- Operational: Process automation, efficiency optimization
- Strategic: Decision support, competitive intelligence
- Compliance: Regulatory requirements, risk management

**By Technical Complexity:**
- Atomic Prompts: Single-purpose, deterministic outputs (data extraction, classification)
- Composite Workflows: Multi-step prompt sequences with conditional logic
- Adaptive Systems: Context-aware prompts that learn from interaction patterns
- Integration Layers: API-style prompts embedded within enterprise systems

**By Governance Requirements:**
- Highly Regulated: Financial services, healthcare, legal compliance
- Moderate Oversight: Internal operations, HR, supply chain
- Low Risk: Creative content, ideation

### Proposed Organizational Hierarchy
```
Enterprise AI Assets
├── Business Domain Collections (Finance, Marketing, Operations)
│   ├── Process-Specific Clusters (Invoice Processing, Lead Qualification)
│   │   ├── Versioned Prompt Assets (v1.2.3, v1.3.0)
│   │   ├── Performance Baselines (Accuracy, Latency, Cost)
│   │   └── Usage Analytics (Adoption, Frequency, User Satisfaction)
```

### Semantic Versioning for Prompts (X.Y.Z)
- **Major (X.0.0):** Fundamental changes to prompt logic, output format, or business requirements that break downstream dependencies
- **Minor (x.Y.0):** New capabilities, improved accuracy, enhanced functionality (backward compatible)
- **Patch (x.y.Z):** Bug fixes, clarity improvements, minor optimizations without functional changes

### Ownership Model
- **Business Process Owners:** Define functional requirements, success criteria, business context
- **Prompt Engineers:** Technical implementation, optimization, testing, maintenance
- **Portfolio Governance Council:** Cross-functional oversight (architecture, security, strategy)
- **Quality Assurance Teams:** Human + automated evaluation

### KPIs for Enterprise Prompt Portfolio
- Prompt reuse rate across departments: Industry benchmark 40-60%
- Time-to-production for new AI capabilities: Target reduction from weeks to days
- Security incidents attributed to prompt vulnerabilities: Target zero tolerance
- Regulatory compliance audit success rate: Target 100%
- Response accuracy and consistency against baselines
- Latency and throughput relative to business requirements

### Deployment Strategies
- Canary Releases: Gradual rollout to subset of users
- Blue-Green Deployment: Parallel environments for instant rollback
- Feature Flags: Runtime switching between prompt versions based on conditions
