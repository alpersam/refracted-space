---
title: "Building Intuition for Orbital Mechanics"
description: "Why slowing down makes you go faster, and other counterintuitive truths about moving through space."
date: 2025-12-15
---

There's a moment in every orbital mechanics course where the room goes quiet. The professor says: _to catch up to something ahead of you in orbit, you need to slow down._

It breaks your terrestrial intuition completely. On a highway, you accelerate to close the gap. In orbit, you fire retrograde — you drop into a lower, faster orbit, lap ahead, then raise back up.

## The Hohmann Transfer

The most elegant solution is still the simplest. Two burns, two tangent points, minimum energy. Hohmann understood this in 1925, decades before anyone had the hardware to prove him right.

```
Δv₁ = √(μ/r₁) · (√(2r₂/(r₁+r₂)) - 1)
Δv₂ = √(μ/r₂) · (1 - √(2r₁/(r₁+r₂)))
```

The beauty is in the constraint. You don't have infinite fuel. You never do. So you find the path that respects your budget and still gets you there.

## Thinking in Energy

The real shift happens when you stop thinking in positions and start thinking in energy. An orbit isn't a path — it's an energy level. Every maneuver is a transaction: you spend propellant to buy or sell energy.

This framing changes everything. Suddenly rendezvous, station-keeping, and de-orbit all become the same problem expressed differently.

## Why This Matters on Earth

I keep coming back to this idea outside the lab. Constraints don't limit creative solutions — they _generate_ them. The Hohmann transfer exists precisely because fuel is finite. The most elegant engineering often comes from the tightest margins.
