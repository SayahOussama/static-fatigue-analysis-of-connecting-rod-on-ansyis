# Connecting Rod — ANSYS Static and Fatigue Analysis

## Project overview

This project studies a connecting rod using ANSYS Static Structural and
Fatigue analysis. It includes a preliminary analytical estimate of the
compression load and static ANSYS result plots.

The pressure calculation is a simplified compression-only model. It does not
include combustion and should not be treated as a validated maximum engine load.

## Engine and geometry parameters

| Parameter | Symbol | Value |
|---|---:|---:|
| Cylinder bore | D | 50 mm |
| Piston stroke | S | 32 mm |
| Crank radius | r = S/2 | 16 mm |
| Connecting-rod length | L | 160.15 mm |
| Compression ratio | CR | 12:1 |
| Engine speed | N | 3600 rpm |
| Assumed reciprocating mass | m_rec | 0.28585 kg |
| Polytropic exponent | n | 1.30 |
| Assumed initial cylinder pressure | P1 | 1 bar absolute |
| Assumed crankcase pressure | P_carter | 1 bar absolute |

The assumed reciprocating mass must be checked against the actual engine
assembly. It should represent the reciprocating components used in the
calculation, not automatically the entire connecting-rod mass.

## Calculated geometry

Piston area:

\[
A_p = \frac{\pi D^2}{4}
    = 1.9635 \times 10^{-3}\ \mathrm{m^2}
\]

Swept volume per cylinder:

\[
V_d = A_p S
    = 6.2832 \times 10^{-5}\ \mathrm{m^3}
    \approx 62.83\ \mathrm{cm^3}
\]

Clearance volume:

\[
V_c = \frac{V_d}{CR-1}
    = 5.712 \times 10^{-6}\ \mathrm{m^3}
    \approx 5.712\ \mathrm{cm^3}
\]

Volume at bottom dead center:

\[
V_{\mathrm{BDC}} = V_d + V_c
                  = 6.8544 \times 10^{-5}\ \mathrm{m^3}
\]

## Preliminary compression model

The crank angle is defined so that \(\theta=0^\circ\) is top dead center (TDC)
and \(\theta=180^\circ\) is bottom dead center (BDC).

Piston displacement from TDC:

\[
x(\theta) =
r(1-\cos\theta)
+L-\sqrt{L^2-r^2\sin^2\theta}
\]

Cylinder volume:

\[
V(\theta) = V_c + A_p x(\theta)
\]

Polytropic compression pressure:

\[
P(\theta) =
P_1\left(\frac{V_{\mathrm{BDC}}}{V(\theta)}\right)^n
\]

Using \(P_1=1\) bar absolute, \(CR=12\), and \(n=1.30\), the estimated
compression pressure at TDC is:

\[
P_{\mathrm{TDC}} = P_1 CR^n
                 \approx 25.26\ \text{bar absolute}
\]

This estimates compression pressure before combustion. It is not a measured
cylinder-pressure trace and does not include combustion pressure.

## Preliminary load estimate at TDC

Angular speed:

\[
\omega = \frac{2\pi N}{60}
       = 376.99\ \mathrm{rad/s}
\]

Approximate piston acceleration at TDC:

\[
a_p(0^\circ) =
r\omega^2\left(1+\frac{r}{L}\right)
\approx 2501\ \mathrm{m/s^2}
\]

Inertial-force magnitude:

\[
F_{\mathrm{inertia}} =
m_{\mathrm{rec}}a_p
\approx 715\ \mathrm{N}
\]

Using the assumed crankcase pressure of 1 bar absolute:

\[
\Delta P =
P_{\mathrm{TDC}}-P_{\mathrm{carter}}
= 24.26\ \text{bar}
\]

Gas-force magnitude:

\[
F_{\mathrm{gas}} = \Delta P A_p
\approx 4763\ \mathrm{N}
\]

Using the adopted sign convention:

\[
F_p(0^\circ) =
F_{\mathrm{gas}}-F_{\mathrm{inertia}}
\approx 4048\ \mathrm{N}
\]

At TDC, the connecting rod is aligned with the cylinder axis, so the
preliminary axial rod-load magnitude is also approximately:

\[
F_{\mathrm{rod}}(0^\circ) \approx 4048\ \mathrm{N}
\]

This is a preliminary compression-only estimate. Confirm that this is the load
and direction actually used in the ANSYS model.

## ANSYS model and results

### Analysis setup

| Item | Value |
|---|---|
| ANSYS version | [enter version] |
| Material | [enter material] |
| Material properties and source | [enter details] |
| Element type | [enter element type] |
| Mesh size and mesh-convergence check | [enter details] |
| Contacts and boundary conditions | [describe] |
| Applied load and direction | [describe] |
| Fatigue method and material data | [describe] |

### Visible static results

Values below are read approximately from the supplied screenshots. Confirm
them in the ANSYS result details before treating them as final.

| Result | Approximate value | Observation |
|---|---:|---|
| Maximum von Mises stress | 75.35 MPa | Concentrated near the small-end/upper-eye region |
| Maximum equivalent elastic strain | 2.7044 × 10⁻⁴ m/m | Also appears near the small end |
| Maximum total deformation | Not legible in supplied screenshot | Enter the exact ANSYS value and units |

Equivalent elastic strain is dimensionless (m/m). It is not displacement and
is not a fatigue-life result.

### Fatigue setup and results

The supplied fatigue plot is labeled **Constant Amplitude Load — Fully Reversed**.
This represents a load alternating equally in tension and compression, with a
zero mean load. Verify that this is an appropriate representation of the
connecting rod’s actual load cycle.

The plot shows mean-stress correction options, but it does not show a
numerical fatigue-life result.

| Fatigue result | Value |
|---|---:|
| Fatigue life | [enter ANSYS result] |
| Fatigue damage | [enter ANSYS result] |
| Fatigue safety factor | [enter ANSYS result] |
| S-N data and source | [enter material data source] |
| Load ratio / cycle definition | [confirm and enter] |

A fatigue assessment requires suitable fatigue material data and a load cycle
representative of engine operation. A single static force does not by itself
define fatigue life.

## Interpretation and limitations

- The static result screenshots show the highest displayed stress and strain
  near the small end. Review the loaded and constrained surfaces there for
  realistic load transfer and possible local numerical concentration.
- The displayed stress and strain are not enough to declare the rod safe.
  Compare the stress with the actual material properties and review the model
  setup, contacts, and mesh convergence.
- The 4048 N value is based on an idealized polytropic compression model and
  assumed crankcase pressure. It excludes combustion.
- A realistic engine load assessment should use an engine-specific or
  measured cylinder-pressure curve over crank angle, including combustion and
  inertia.
- Compressive loading should also be assessed for connecting-rod buckling.

## Suggested repository structure

- `geometry/` — CAD geometry
- `ansys/` — ANSYS project files
- `results/` — exported result tables and plots
- `images/` — screenshots used in this report
- `README.md` — project description, assumptions, and summarized results
