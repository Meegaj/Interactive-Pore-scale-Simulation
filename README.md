# Interactive-Pore-scale Simulation
**Interactive pore-scale flow simulation — D2Q9 lattice Boltzmann in a single HTML file.**

Single-phase fluid is driven through a randomly packed bed. A
live velocity heatmap with streamline tracers is simulated and bulk permeability in real time is computed,
showing how Darcy's law emerges from messy pore-scale physics.

**[▶ Live demo](https:/Meegaj/.github.io/lbm-pore-flow/)** *(enable GitHub Pages — see below)*

No build step, no dependencies, no server. Open `index.html` in any modern browser.

---

## What it simulates

| Aspect | Implementation |
|---|---|
| Lattice | D2Q9, 240 × 120 cells |
| Collision | Single-relaxation-time BGK |
| Solid boundaries | Halfway bounce-back (no-slip) on grains and channel walls |
| Driving | Uniform body force in +x (velocity-shift forcing), periodic in x |
| Geometry | Random circular grains, periodic-wrapped in x so the pack tiles seamlessly |
| Outputs | Local speed field, streamline tracers, porosity φ, mean velocity ū, permeability k |

Permeability is computed from Darcy's law each frame:

```
k = ν · ū / g        [lattice units²]
```

where `ν = (τ − ½)/3` is the kinematic viscosity, `ū` the pore-averaged x-velocity,
and `g` the body force.

### Units

`k` is reported in **lattice units squared (lu²)**. To convert to physical units, choose
a voxel size Δx and scale by its square:

```
k_physical = k_lattice · (Δx)²
```

Example: `k = 7.7 lu²` at Δx = 1 µm → 7.7 × 10⁻¹² m² ≈ 7.8 darcy (clean-sandstone range).
This mirrors digital rock physics workflows, where Δx comes from micro-CT scanner resolution.

## Controls

- **Pressure drive** — body force g. In the Darcy (slow-flow) regime, k is independent of g;
  push it high and watch k dip slightly as inertial effects appear.
- **Viscosity** — relaxation time τ, hence ν = (τ − ½)/3.
- **Grain density** — number of grains in the pack. Regenerate to see k swing with porosity.
- **New grain pack** — new random geometry. The core teaching moment: permeability is a
  property of the *geometry*, not the push.
- **Flow tracers** — toggle the streamline particles.

## Key Insights

1. **Flow channels.** Fluid finds preferential paths through wide pores; dead-end throats stall.
2. **Permeability is geometric.** Change the drive → k holds steady. Change the pack → k jumps.
3. **Darcy's law is an average.** The single number k in the HUD is the spatial average of the
   heterogeneous field on screen.
4. **Porosity ≠ permeability.** Two packs with similar φ can have different k, depending on
   how connected the pore space is.

## Limitations

This is an educational 2D model, not production digital rock physics:

- 2D slice, not a 3D micro-CT-resolved volume — absolute k values are illustrative.
- Single-relaxation-time BGK; bounce-back permeability carries a known mild
  viscosity dependence (MRT/TRT collision operators reduce this).
- Single-phase, incompressible-limit, isothermal, no slip-flow or Knudsen effects.

## License

MIT — use it freely in courses, posts, and talks. Kindly reference appropriately.
