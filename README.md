# Skywater 130nm 77K Cryogenic Models
This repo contains the newly developed cryogenic model cards needed for easy implementation into NGSpice software and the cryogenic I-V data curves used to obtain the models.

## Associated paper

This repository accompanies the paper:

**DC Cryogenic Modeling of Open-Source SkyWater 130nm MOSFETs at 77 K Using BSIM4**  
F. Beall, A. Rimal, O. Seidel, Y. Mei, A. D. McDonald, I. Parmaksiz, V. A. Chirayath, J. Asaadi, D. Braga, and J. B. R. Battat  
arXiv:2604.21625, 2026  
[https://arxiv.org/abs/2604.21625](https://arxiv.org/abs/2604.21625)

The paper describes the 77K DC characterization and BSIM4-based cryogenic modeling workflow used to generate the SPICE-compatible SkyWater 130nm MOSFET models provided here.

## `cryo_data` folder
This folder contains the 77K I-V characteristic data taken at Fermilab. Each transistor's data is separated into its own folder. The naming convention follows the format:
**nmos_FET_len_0p15_wid_1p6**
- **nmos**: dopant type, either nMOS or pMOS
- **len_XpXX_wid_XpXX**: size of the transistor in microns (1e-6 meters) with the length value first and the width value second.

Within these folders hold both IdVg (transfer) and IdVd (output) characteristic curve data points, with each curve having its own corresponding CSV file. For example:
**idvd_Vg0p37.csv**
- **idvd**: curve type, either IdVg or IdVd
- **VgXpXX**: bias voltage held constant with its corresponding voltage value. Vg (gate voltage) bias for IdVd curves and Vd (drain voltage) bias for IdVg curves. Negative bias for pMOS and positive bias for nMOS.

The columned data in the CSV is organized by voltage (current) applied (measured). The columns in each folder represent:
- **Vd_src**: gate (Vg) or drain (Vd) voltage value referenced to source.
- **Id**: measured drain current value for particular Vg and Vd bias.
- **Ig**: measured gate current value for particular Vg and Vd bias. (we did not take gate current measurements due to the resolution of the source-measure unit)

The source and body were both held at ground for all measurements.

## `models` folder
This folder contains the 77K models extracted using the cryo data. 

### How to implement the 77K model into the SkyWater PDK

From Volare download the updated PDK using the following commands:

```bash
pip install volare

export PDK_ROOT="/home/<your_username>/<desired_PDK_directory>"

volare ls-remote --pdk sky130
# Lists all available versions of the Sky130 PDK

volare enable --pdk sky130 a918dc7c8e474a99b68c85eb3546b4ed91fe9e7b
# This version from 10/08/2024 was used for 77K PDK development
```
Place  77k both nMOS and pMOS models to 
```bash
/home/<your_username>/<desired_PDK_directory>/skywater-pdk/libraries/sky130_fd_pr/latest/cells/nfet_01v8_lvt/77k_models
```

Create a new corner inside the file:

```
/home/<your_username>/<desired_PDK_directory>/skywater-pdk/libraries/sky130_fd_pr/latest/models/sky130.lib.spice
```

Include 77K nMOS and pMOS model files along with other necessary model files.
For example,
```spice
******** SkyWater sky130 model library ********

* Typical corner (tt) at 77K
.lib tt_77k

.include "../cells/nfet_01v8_lvt/77k_models/sky130_fd_pr__nfet_01v8_lvt__tt_77k.corner.spice"
.include "../cells/pfet_01v8_lvt/77k_models/sky130_fd_pr__pfet_01v8_lvt__tt_77k.corner.spice"

.include "fc/res_typical__cap_typical.spice"
.include "fc/res_typical__cap_typical__lin.spice"

* Special cells
.include "corners/tt/specialized_cells.spice"

* All models
.include "all.spice"

* Corner
.include "corners/tt/ff.spice"

.endl
```

After this modification, the 77K corner can be used in simulations by calling the netlist in the following way:

```spice
.lib \"/home/<your_username>/<desired_PDK_directory>/skywater-pdk/libraries/sky130_fd_pr/latest/models/sky130.lib.spice" tt_77k
```
### 77K model notes

The length and width model bins of available 77K CMOS models are listed in the tables below, <ins>with the sizes in microns</ins>. (Lmin is the minimum length, Lmax is the maximum length, and similar notation is for width minimum and maximum).

<img width="404" height="417" alt="Screenshot 2026-01-12 at 3 15 14 PM" src="https://github.com/user-attachments/assets/6db8de00-5c8e-46b8-b815-7b9582caff26" />
<img width="391" height="317" alt="Screenshot 2026-01-12 at 3 15 22 PM" src="https://github.com/user-attachments/assets/b9021a34-81af-4b6f-9f4a-fe39a1d75a8e" />




Message feb9528@mavs.uta.edu for any questions.
