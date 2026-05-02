---
title: "Carry: HotStuff Linearity with Tail-Forking-Resilience"
date: 2025-09-27
categories:
  - consensus
tags:
  - bft
  - consensus
  - hotstuff
  - tail-forking
excerpt: "Defending streamlined BFT consensus against tail-forking attacks with only linear per-view communication."
---

*Originally published at [Decentralized Thoughts](https://decentralizedthoughts.github.io/2025-09-27-carry-the-tail/) on September 27, 2025. Co-authored with Suyash Gupta, Dahlia Malkhi, and Mohammad Sadoghi.*

### Synopsis

Streamlined Byzantine Fault Tolerant (BFT) consensus protocols like HotStuff use a single quorum exchange between a leader and voters per view. This design exposes a subtle vulnerability: because two consecutive successful views are required to commit, a malicious leader can skip the previous proposal — a *tail-forking* attack — and degrade throughput.

Prior defenses inherited from PBFT cost either quadratic communication or computationally heavy SNARKs. **Carry** addresses tail-forking with only **linear communication overhead per view** while remaining compatible with HotStuff's structure.

### Key mechanisms

- **Voting and certification.** Leaders broadcast proposals; replicas reply with signature shares (votes). A threshold signature from 2F+1 replicas forms a Quorum Certificate (QC).
- **Safety and liveness.** Proposals reference prior certified tails via QCs. Replicas lock on received proposals to ensure safety. Commits require two consecutive successful views; the Pacemaker module triggers leader timeouts.
- **Empty Certificates (EC).** When a leader justifiably skips a proposal, it produces an EC — a threshold signature from 2F+1 replicas on an empty vote (⊥). A malicious leader cannot forge an EC if F+1 honest replicas already voted for the proposal.
- **Proposal reinstatement.** Leaders can reinstate uncertified proposals via reinstate references, helping benignly delayed proposals reach consensus without re-collecting a full quorum.

### ρ-Tail-Resilience

To defend against ρ consecutive bad leaders, Carry extends tail protection across the ρ preceding views, with communication overhead **O(ρN) words per view**. The guarantee: in each rotation of N leaders, all proposals except (F_actual / ρ) are protected. ρ=0 disables protection, ρ=1 defends against single bad leaders, ρ=2 handles two consecutive bad leaders.

### Comparison with prior work

| Aspect | BeeGees | Carry |
|---|---|---|
| Honest-leader commit guarantee | Always | All except (F_actual / ρ) |
| Per-view communication | O(n²) or SNARK | O(ρ·n) |
| Worst-case communication | O(n³) or O(n)-SNARKs | O(ρ·n²) |
| Sluggish-leader support | No | Yes |

Carry reaches the same Any-Honest-Leader property as BeeGees with simpler logic and a tunable parameter that suits production systems.

---

Read the full preprint: [arXiv:2508.12173](https://arxiv.org/abs/2508.12173). The brief announcement appeared at **[DISC 2025](https://doi.org/10.4230/LIPIcs.DISC.2025.59)**.
