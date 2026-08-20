<div align="center">

<img src="https://raw.githubusercontent.com/deepcausality-rs/deep_causality/main/img/logo_background.jpg" width="820" alt="DeepCausality">

[Website](https://deepcausality.com) ·
[Documentation](https://docs.deepcausality.com) ·
[Examples](https://deepcausality.com/examples/) ·
[Blog](https://deepcausality.com/blog/) ·
[Discord](https://discord.gg/Bxj9P7JXSj) ·
[Crates.io](https://crates.io/crates/deep_causality)

</div>

---

## Cause and effect in systems that change

Most software that predicts anything runs on correlation. A model reads what happened before, finds the patterns that held, and assumes they will hold again. This works right up to the moment the system itself changes.

Equities and bonds move in opposite directions for a decade, then fall together on a single afternoon. Steel deforms in proportion to load until it yields, and then it does not. An aircraft answers the stick one way in clean airflow and another way in a stall. The variables stayed the same. The relationships between them did not.

Causal reasoning asks what produces what, and what follows if you change something. Judea Pearl separated that into three levels: what you observe, what happens when you intervene, and what would have happened had the past gone differently.

DeepCausality answers all three. The causal structure itself can also move while the program runs. A system can add, retire, or rewire rules as it crosses from one regime into another, and time need not run in a straight line. That is what the project means by **dynamic causality**.

## What it looks like

```rust
use deep_causality_core::CausalFlow;

// What happened: 10 mg is absorbed, then metabolized, and the patient responds.
let observed = CausalFlow::value(10.0)
    .map(|dose| dose * 0.8)      // absorption -> 8.0
    .map(|level| level - 2.0)    // metabolism -> 6.0
    .map(|level| level > 5.0)    // response   -> effective
    .finish();

// What would have happened at a blood level of 3.0 instead.
let counterfactual = CausalFlow::value(10.0)
    .map(|dose| dose * 0.8)
    .alternate_value(3.0)        // substitute mid-chain, and log the substitution
    .map(|level| level - 2.0)    // -> 1.0
    .map(|level| level > 5.0)    // -> ineffective
    .finish();

assert_eq!(observed, Ok(true));
assert_eq!(counterfactual, Ok(false));
```

Two runs, one chain. They differ by a single call. Both carry an audit log of every step and every substitution, so you can replay and inspect a conclusion long after the fact. The framework targets control systems in IoT, monitoring in cloud infrastructure, market models in finance, avionics, and scientific computing.

## Governance

DeepCausality is a sandbox project at the [LF AI & Data Foundation](https://lfaidata.foundation/), part of the Linux Foundation. The [Center for Dynamic Causality](https://www.causalcenter.com) contributes research and offers commercial support for larger projects.

## Start here

- **[Install and first model](https://docs.deepcausality.com/getting-started/install/)** if you want code running in the next ten minutes.
- **[What dynamic causality is](https://docs.deepcausality.com/concepts/dynamic-causality/)** if you want the idea first.
- **[Worked examples](https://deepcausality.com/examples/)** in medicine, physics, aerospace, and mathematics.
- **[Discord](https://discord.gg/Bxj9P7JXSj)** and **[Discussions](https://github.com/orgs/deepcausality-rs/discussions)** for questions. Newcomers are welcome; the concepts take a while to land, and asking is faster than reading.
