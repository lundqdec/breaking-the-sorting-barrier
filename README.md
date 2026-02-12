# Breaking the Sorting Barrier for Directed Single-Source Shortest Paths

> **Duan, Mao, Mao, Shu, Yin** — July 2025  
> 📄 [arXiv:2504.17033](https://arxiv.org/abs/2504.17033)

## TL;DR

Dijkstra's algorithm has been the gold standard for shortest path computation since **1959**. This paper presents the **first proven improvement** for directed graphs with real non-negative edge weights — a deterministic **O(m log²/³ n)** algorithm that breaks Dijkstra's O(m + n log n) bound on sparse graphs.

**Dijkstra is not optimal for SSSP.**

## The Problem

Single-source shortest paths (SSSP): given a directed graph with weighted edges, find the shortest path from one source to every other vertex.

Dijkstra's algorithm solves this by maintaining a priority queue, always extracting the closest unvisited vertex. This implicitly **sorts** all vertices by distance — creating an **Ω(n log n) bottleneck** that was widely believed to be unavoidable.

## The Breakthrough

The paper merges two classical approaches:

| | Dijkstra | Bellman-Ford |
|---|---|---|
| **Method** | Priority queue, extract minimum | Relax all edges for k steps |
| **Strength** | Precise, optimal ordering | No sorting required |
| **Weakness** | Must sort → Θ(n log n) | Redundant work → O(mk) |

**Key insight:** Use Bellman-Ford as a *coarse filter* to eliminate easy vertices, then apply Dijkstra-like recursion on only the remaining **pivots** — a dramatically smaller set.

The mechanism:

1. Maintain a **frontier** of candidate vertices
2. Run Bellman-Ford for k steps → completes all vertices reachable within k hops (no sorting)
3. Identify **pivots**: vertices with large subtrees (≥k nodes) that still need processing
4. The number of pivots is at most **|frontier| / k** — the frontier shrinks
5. Recurse with Dijkstra-like precision on pivots only
6. Over **log(n)/t** recursion levels, sorting cost per vertex drops to **log²/³(n)**

## Complexity

| Algorithm | Time | Model | Notes |
|---|---|---|---|
| Dijkstra (1959) | O(m + n log n) | Comparison-addition | 66-year standard |
| This paper (2025) | **O(m log²/³ n)** | Comparison-addition | First improvement for directed graphs |

The algorithm is **deterministic** — also making it the first deterministic improvement even for undirected graphs (the prior result by Duan et al. 2023 was randomized).

## Where This Matters

The improvement is sub-logarithmic, so it matters most on **large sparse graphs that can't be precomputed**:

- **Unknown territory routing** — autonomous vehicles, drones, and disaster response building graphs from sensor data in real-time
- **Dynamic networks** — topology changes faster than precomputation allows (outages, congestion, live traffic)
- **Edge/on-device compute** — no cloud access to precomputed data, every CPU cycle counts
- **Massive one-off graph queries** — ad-hoc analysis on financial networks, infrastructure simulations, or supply chain disruptions

In well-mapped, precomputed environments (Google Maps, Waze), the practical gain is negligible. The real value: **when you can't cheat with precomputation.**

## Citation

```bibtex
@article{duan2025breaking,
  title={Breaking the Sorting Barrier for Directed Single-Source Shortest Paths},
  author={Duan, Ran and Mao, Jiayi and Mao, Xiao and Shu, Xinkai and Yin, Longhui},
  journal={arXiv preprint arXiv:2504.17033},
  year={2025}
}
```

## License

This repository contains a summary and visualization of the research paper. The original work belongs to the authors. See the [paper](https://arxiv.org/abs/2504.17033) for full details.