# Collatzscape: Forced Tetration with Direction-Dependent Collapse

Collatzscape is a reproducible implementation of a forced complex dynamical system:

- a cosine-interpolated Collatz gate $C(z)$,
- a phase-kicked effective base $c_{\mathrm{eff}}$,
- exponential update $z_{n+1}=\exp(C(z_n)\log(c_{\mathrm{eff},n}))$,
- direction-dependent coupling that can produce asymmetric bounded/escape behavior.

## Start Here

1. `docs/00_start_here.md`
2. `docs/00_foundations.md`
3. `MODEL.md`
4. `docs/01_theory.md`
5. `docs/02_experiments.md`

## The Model in a Nutshell

We study a non-autonomous iterated map in the complex plane:

1. **Collatz gate** $C(z)$ - a smooth interpolation of the classic Collatz function:

   $C(z) = \frac{z}{4}(1+\cos\pi z) + \frac{3z+1}{2}\(1-\cos\pi z\)$
   
   For integer $z$, this reproduces $C(2k)=k$ (the "brake") and $C(2k+1)=3(2k+1)+1$ (the "accelerator").

2. **Tetration core** - we iterate
   
   $z_{n+1} = \exp\bigl(C(z_n)\,\log c_{\mathrm{eff}}(z_n)\bigr)$

   where $c_{\mathrm{eff}}(z_n)=c\cdot e^{i\phi_n}$, and the phase kick $\phi_n$ depends on $\mathrm{Re}(C(z_n))$ (e.g., $\phi_n=\alpha\tanh(\mathrm{Re}(C(z_n))/s)$, with scale parameter $s$).

3. **State-dependent feedback** - the phase kick acts as a self-interaction that can either stabilise or destabilise the orbit, depending on the base parameter $c$ and the kick strength $\alpha$.

The key discovery is a **"Collatz Bay"** - a large region in the $c$-plane where orbits remain bounded indefinitely. This stability is explained by an exact **period-2 snap-back cycle** $\{c^{\star},2\}$ with

$
c^{\star}\approx 1.9086708647584145,\qquad C(c^{\star})=\frac{\ln 2}{\ln c^{\star}}\approx 1.07230747.
$

Whenever the orbit visits $z\approx 2$, the next step resets the magnitude to $|c|$, creating a negative feedback loop.


Note:


## Extended Theoretical Notes

### I. The “Energy” Landscape: Böttcher Coordinates and Green’s Functions

A physical quasiparticle exists within a potential field. For the map \(f_c(z)\), we can define this field rigorously using potential theory. Instead of viewing \(z_n\) as merely a number, we treat its trajectory as the microstate of the particle \(c\). The continuous “energy” landscape is provided by the Green’s function of the filled Julia set:

\[
G_c(z) = \lim_{n \to \infty} \frac{1}{2^n} \log_+|f_c^n(z)|
\]

This function is harmonic outside the Julia set and yields the exact electrostatic potential analog for our particle. The rate at which the internal state \(z_n\) traverses these equipotential lines to infinity (the escape rate) serves as the observable kinetic energy of the quasiparticle \(c\).

### II. Thermodynamic Formalism

To transition into the realm of statistical mechanics, we apply the Sinai‑Ruelle‑Bowen (SRB) thermodynamic formalism. We redefine the bundle of discrete dynamical systems using a partition function \(Z_n\). If we take a potential function \(\phi(z) = -t \log |f_c'(z)|\) (where \(t\) is analogous to inverse temperature), we can calculate the topological pressure \(P(t)\). The internal state \(z_n\) becomes a statistical distribution of states evolving under the Perron‑Frobenius (transfer) operator. Phase transitions in this thermodynamic limit correspond exactly to the parameters \(c\) crossing the bifurcation locus of the Mandelbrot set.

### III. Formalizing Quasiparticle Kinematics

To satisfy the definition of a quasiparticle, \(c\) must exhibit quantifiable physical properties. We can derive these from the structural stability of the system:

- **Effective Mass (\(m^*\))** – In solid‑state physics, effective mass is derived from the curvature of the energy band. Here, we can define \(m^*\) by examining the multiplier of periodic orbits, \(\lambda = (f_c^p)'(z_0)\). The derivative of this multiplier with respect to \(c\), \(\frac{\partial \lambda}{\partial c}\), measures the “inertia” of the orbit’s stability against perturbation in the parameter space.
- **Observables** – By utilizing the Koopman operator—which acts on the space of observable functions rather than the state space directly—we construct an analog to the Heisenberg picture of quantum mechanics, governing how measurable properties of \(c\) evolve with iteration \(n\).

### IV. Generating Interactions (The Field Theory Requirement)

The most glaring flaw in the current metaphor is that quasiparticles represent collective excitations, whereas each \(c\) currently exists in a vacuum, entirely decoupled from \(c'\). To generate a valid framework, we must introduce a topological coupling. We can embed the parameters into a spatially extended Coupled Map Lattice (CML). If \(c_i\) and \(c_j\) represent adjacent “particles” on a discrete spatial grid, their internal states must exchange information at each iteration:

\[
z_{n+1}^{(i)} = (1-\epsilon)f_{c_i}(z_n^{(i)}) + \frac{\epsilon}{2} \left[ f_{c_{i-1}}(z_n^{(i-1)}) + f_{c_{i+1}}(z_n^{(i+1)}) \right]
\]

Here, \(\epsilon\) acts as the coupling constant. Only with this step does the “bundle of isolated systems” transform into a lattice of interacting computational quasiparticles, allowing for the observation of emergent phenomena like wave propagation, synchronization, and spatiotemporal chaos.

### V. Thermodynamic Phase Transitions via Topological Pressure

To prove that crossing the bifurcation locus constitutes a formal thermodynamic phase transition, we map the dynamical system \(f_c(z) = z^2 + c\) to statistical mechanics using the Bowen‑Ruelle‑Sinai formalism. We define a partition function over the periodic orbits of the system. Let \(\text{Fix}(f_c^n)\) be the set of fixed points of the \(n\)-th iterate of \(f_c\). The partition function \(Z_n(t)\), parameterized by the “inverse temperature” \(t\), is constructed using the multiplier of these orbits:

\[
Z_n(t) = \sum_{x \in \text{Fix}(f_c^n)} |(f_c^n)'(x)|^{-t}
\]

From this, we extract the topological pressure \(P(t)\), which serves as the analog to the negative Gibbs free energy density in statistical mechanics:


$P(t) = \lim_{n \to \infty} \frac{1}{n} \log Z_n(t)$


In rigorous thermodynamics, a phase transition is strictly defined as a point of non‑analyticity in the free energy function. As long as the parameter \(c\) remains within a hyperbolic component of the Mandelbrot set (e.g., the main cardioid), the system is structurally stable, and \(P(t)\) is a real‑analytic function of \(t\). However, when \(c\) crosses the bifurcation locus (the boundary of the Mandelbrot set \(\partial M\)), structural stability fails. The Lyapunov exponent \(\chi\) of the physical invariant measure drops to zero or exhibits a discontinuity. At this precise parametric boundary, the function \(P(t)\) loses real‑analyticity. This proves that the parameter \(c\) crossing the bifurcation locus is not merely a dynamical change, but a strict thermodynamic phase transition, shifting the internal state \(z_n\) of the “particle” from an ordered, localized phase into a chaotic, uniformly distributed phase over the Julia set.

### VI. Lattice Topologies and Quasiparticle Dispersion

To validate the “computational quasiparticle” as a physical entity, it must possess a definable momentum \(k\) and a dispersion relation \(\omega(k)\). This requires embedding the isolated systems into a Coupled Map Lattice (CML) with exact topological boundary conditions. For a 1D spatial lattice of \(N\) sites, each containing an internal state \(z^{(i)}_n\) governed by a local parameter \(c_i\), we impose periodic boundary conditions to ensure discrete translational invariance, creating a torus topology (\(S^1\)):

$z^{(i+N)}_n = z^{(i)}_n \quad \text{and} \quad c_{i+N} = c_i$

We apply a nearest‑neighbor diffusive coupling operator with a coupling strength \(\epsilon\):

$z_{n+1}^{(i)} = (1-\epsilon)f_{c_i}(z_n^{(i)}) + \frac{\epsilon}{2} \left[ f_{c_{i-1}}(z_n^{(i-1)}) + f_{c_{i+1}}(z_n^{(i+1)}) \right]$

To extract the quasiparticle kinematics, we assume a uniform baseline parameter field (\(c_i = c\) for all \(i\)) and linearize the CML operator around a homogeneous stationary state. Because of the periodic boundary conditions, the spatial translation operator commutes with the time evolution operator. This allows us to apply a discrete spatial Fourier transform, decoupling the local perturbations into independent planar wave modes characterized by a wave vector (momentum):


$k = \frac{2\pi j}{N} \quad \text{for } j \in \{0, 1, \dots, N-1\}$

By tracking the evolution of these modes, we derive a set of eigenvalues \(\Lambda_k\) corresponding to each spatial frequency. The functional dependence of the temporal multiplier \(\Lambda_k\) on the spatial momentum \(k\) yields the exact equivalent of a dispersion relation for the lattice. At this stage, the parameter \(c\) formally behaves as a collective excitation—a quasiparticle—capable of propagating waves of computational state across the lattice.


### Visual Highlights

- **Survival maps** - probability that an orbit survives $N$ steps (noise-averaged).
- **Escape-time fractals** - deterministic coloring by iteration count before escape.
- **Entropy fringes** - red/blue maps showing where the orbit spends more time in odd-heavy (red, high entropy) or even-heavy (blue, low entropy) phases.

### What Makes This Interesting

- **Continuous shadow of the Collatz conjecture** - the bay visualizes a stable region where the accelerator/brake balance is tuned.
- **Quasiparticle interpretation** - each $c$ behaves like a computational quasiparticle whose internal state $z$ evolves under Collatz forcing.
- **Topological extensions** - by enforcing conjugate pairing ($w\approx\overline{z}$), the system mimics Majorana-like modes and exhibits directional braiding with non-Abelian fusion rules.
- **Reproducible atlas** - figures in the paper (or planned paper) can be regenerated with the provided scripts and configs.

### Open Questions / Future Directions

- How does the bay change under different Collatz interpolations (for example, $\sin^2$ instead of cosine)?
- Can the fractal dimension of the boundary be linked to known Collatz statistics (for example, average parity ratio)?
- Does the quasiparticle spectral function reveal quantized energy levels?
- Can the Majorana-like braiding be demonstrated with higher particle numbers?

## Quickstart

```bash
python -m venv .venv
# Windows: .venv\Scripts\activate
source .venv/bin/activate
pip install -r requirements.txt
```

Baseline runs:

```bash
python -m collatzscape.cli demo --config configs/default.yaml --out figures/
python -m collatzscape.cli sweep --config configs/default.yaml --out figures/
python -m collatzscape.cli fatou --config configs/default.yaml --out figures/
```

Deterministic tanh phase-kick (no float noise):

```bash
python scripts/Collatz_tetration_quasiparticles.py --deterministic-tanh
python scripts/falseability.py --deterministic-tanh
```

## Applications (Explicit)

1. Benchmarking direction-dependent stability transitions.
2. Reproducible basin/escape atlas generation.
3. Numerical stress-testing for branch-sensitive transcendental maps.
4. Deterministic vs noisy forcing comparisons.
5. Optional exploratory analogy layer for quasiparticle/fusion interpretation.

## Repo Layout

- `src/collatzscape/`: core model and CLI
- `configs/`: run configurations
- `docs/`: foundations, theory, experiments, glossary, references
- `scripts/`: exploratory variants
- `paper/`: preprint-style summary
- `figures/`: generated artifacts

## Citing

See `CITATION.cff`. For archival citation, create a release and attach a DOI via Zenodo.

## License

MIT (`LICENSE`).

=======
# Collatzscape: Forced Tetration with Direction-Dependent Collapse

Collatzscape is a reproducible implementation of a forced complex dynamical system:

- a cosine-interpolated Collatz gate $\(C(z)\)$,
- a phase-kicked effective base $\(c_{\mathrm{eff}}\)$,
- exponential update $\(z_{n+1}=\exp(C(z_n)\log(c_{\mathrm{eff},n}))\)$,
- direction-dependent coupling that can produce asymmetric bounded/escape behavior.

## Start Here

1. `docs/00_start_here.md`
2. `docs/00_foundations.md`
3. `MODEL.md`
4. `docs/01_theory.md`
5. `docs/02_experiments.md`

## The Model in a Nutshell

We study a non-autonomous iterated map in the complex plane:

1. **Collatz gate** $C(z)$ – a smooth interpolation of the classic Collatz function:

   $$C(z) = \frac{z}{4}(1+\cos\pi z) + \frac{3z+1}{2}(1-\cos\pi z)$$
   
   For integer $z$, this reproduces $C(2k)=k$ (the "brake") and $C(2k+1)=3(2k+1)+1$ (the "accelerator").

2. **Tetration core** – we iterate
   
   $$z_{n+1} = \exp\bigl(C(z_n)\,\log c_{\text{eff}}(z_n)\bigr)$$

   where $c_{\text{eff}}(z_n) = c\cdot e^{i\phi_n}$ and the phase kick $\phi_n$ depends on $\Re\!\bigl(C(z_n)\bigr)$ (e.g., $\phi_n = \alpha\,\tanh\!\left(\Re\!\bigl(C(z_n)\bigr)/\text{scale}\right)$.).

3. **State-dependent feedback** - the phase kick acts as a self-interaction that can either stabilise or destabilise the orbit, depending on the base parameter $\(c\)$ and the kick strength $\(\alpha\).$

The key discovery is a **"Collatz Bay"** – a large region in the $c$-plane where orbits remain bounded indefinitely. This stability is explained by an exact **period‑2 snap‑back cycle** $\{c^*,2\}$ with

```math
c^* \approx 1.9086708647584145,\qquad
C(c^*) = \frac{\ln 2}{\ln c^*} \approx 1.07230747.
```

Whenever the orbit visits $z\approx2$, the next step resets the magnitude to $|c|$, creating a negative feedback loop.

### Visual Highlights

- **Survival maps** - probability that an orbit survives \(N\) steps (noise-averaged).
- **Escape-time fractals** - deterministic coloring by iteration count before escape.
- **Entropy fringes** - red/blue maps showing where the orbit spends more time in odd-heavy (red, high entropy) or even-heavy (blue, low entropy) phases.

### What Makes This Interesting

- **Continuous shadow of the Collatz conjecture** - the bay visualizes a stable region where the accelerator/brake balance is tuned.
- **Quasiparticle interpretation** - each $\(c\)$ behaves like a computational quasiparticle whose internal state $\(z\)$ evolves under Collatz forcing.
- **Topological extensions** - by enforcing conjugate pairing $(\(w\approx\overline{z}\))$, the system mimics Majorana-like modes and exhibits directional braiding with non-Abelian fusion rules.
- **Reproducible atlas** - figures in the paper (or planned paper) can be regenerated with the provided scripts and configs.

### Open Questions / Future Directions

- How does the bay change under different Collatz interpolations (for example, $\(\sin^2\)$ instead of cosine)?
- Can the fractal dimension of the boundary be linked to known Collatz statistics (for example, average parity ratio)?
- Does the quasiparticle spectral function reveal quantized energy levels?
- Can the Majorana-like braiding be demonstrated with higher particle numbers?

## Quickstart

```bash
python -m venv .venv
# Windows: .venv\Scripts\activate
source .venv/bin/activate
pip install -r requirements.txt
```

Baseline runs:

```bash
python -m collatzscape.cli demo --config configs/default.yaml --out figures/
python -m collatzscape.cli sweep --config configs/default.yaml --out figures/
python -m collatzscape.cli fatou --config configs/default.yaml --out figures/
```

Deterministic tanh phase-kick (no float noise):

```bash
python scripts/Collatz_tetration_quasiparticles.py --deterministic-tanh
python scripts/falseability.py --deterministic-tanh
```

## Applications (Explicit)

1. Benchmarking direction-dependent stability transitions.
2. Reproducible basin/escape atlas generation.
3. Numerical stress-testing for branch-sensitive transcendental maps.
4. Deterministic vs noisy forcing comparisons.
5. Optional exploratory analogy layer for quasiparticle/fusion interpretation.

## Repo Layout

- `src/collatzscape/`: core model and CLI
- `configs/`: run configurations
- `docs/`: foundations, theory, experiments, glossary, references
- `scripts/`: exploratory variants
- `paper/`: preprint-style summary
- `figures/`: generated artifacts

## Citing

See `CITATION.cff`. For archival citation, create a release and attach a DOI via Zenodo.

## License

MIT (`LICENSE`).
>>>>>>> 17318795d5f68ec29c404562bd3a86b4e82b898f






