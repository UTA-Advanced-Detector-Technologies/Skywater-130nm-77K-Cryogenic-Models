# Skywater-130nm-77K-Cryogenic-Models
This repo contains the newly developed cryogenic model cards needed for easy implementation into NGSpice software and the cryogenic I-V data curves used to obtain the models.

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

### How to implement the 77K model into the Skywater PDK

### 77K model notes

The length and width model bins of available 77K CMOS models are listed in the tables below, <ins>with the sizes in microns</ins>. (Lmin is the minimum length, Lmax is the maximum length, and similar notation is for width minimum and maximum).

<img width="404" height="417" alt="Screenshot 2026-01-12 at 3 15 14 PM" src="https://github.com/user-attachments/assets/6db8de00-5c8e-46b8-b815-7b9582caff26" />
<img width="391" height="317" alt="Screenshot 2026-01-12 at 3 15 22 PM" src="https://github.com/user-attachments/assets/b9021a34-81af-4b6f-9f4a-fe39a1d75a8e" />



Message feb9528@mavs.uta.edu for any questions.
