The CRR is the cascade routing and re-infiltration concept. The CRR routes the available water from the upslope areas to the adjacent downslope areas.
The available water depends on the conceptualization of the study area, but mainly, the available water can be the rejected infiltration (the amount of precipitation that will not infiltrate into the ground; RI) plus the groundwater exfiltration (the groundwater discharge to the land surface when the water table rises up or close to the land surface level; Exf_gw). With applying the CRR concept, the available water can subsequently be (a) evaporated (RE^e); (b) re-infiltrated back to the subsurface (RE^i); or (c) discharged as direct runoff (RE^s) into the nearest surface water body (e.g. streams or lakes). The CRR is grid-based, designed mainly for use with the MODFLOW 6 groundwater model, but can be applied to other similar grid-based models. The flow partitioning among the 3 components (RE^e, RE^i, RE^s) depends on many factors (Figure 1). RE^i is controlled by the vertical saturated hydraulic conductivity of topsoil (K_sat). RE^i is the amount of RI+Exf_gw, which is less than the K_sat-P_e term. This term determines the availability of soil to still infiltrate more water (RE^i) after the initial infiltration (I) into the subsurface from the effective precipitation (P_e). The remaining water (RI+Exf_gw-RE^i) is then partitioned between RE^s and RE^e through a flow partitioning factor (α_(i,j)), which is the fraction of flow from cell i to the neighbouring j cell.

![plot](https://github.com/MostafaGomaa93/CRR_concept/blob/main/images/CRR_concept.png)
 
Figure 1. Schematic diagram of the CRR concept showing: (a) it’s three components (highlighted by the dash-line purple large circle); the two components surrounded by green dash-line rectangles are grouped as RE^e (total evaporated water), while the two components surrounded by red dash-line rectangles are grouped as RE^i (total re-infiltrated water), and the two surrounded by blue dash-line rectangles are grouped as RE^s (total direct runoff). P_e is the effective precipitation (precipitation – canopy interception), I is the initial amount of water infiltrated into the subsurface, and d_surf is the surface depth – a user-specified depth relative to the land surface where the groundwater exfiltration starts; (b) the mathematical design.\

**Flow fraction (α_(i,j)) calculations**\
The flow fraction (α_(i,j)) from a cell i to the neighbouring cell j is calculated as follows:

<p align="center">
  <img src="https://github.com/MostafaGomaa93/CRR_concept/blob/main/images/CRR%20equation.png" />
</p>

where S_(i,j)=((elv_i-elv_j))⁄l_ij  is the slope gradient between the center cell i and j-cells surrounding the cell i (Figure 2), elv_i and elv_j are land surface elevations of cells i and j respectively, l_ij is a distance between the center of the cell i and the center of one of j-cells surrounding i-cell, and m is the number of connected j cells to the cell i; note, the negative value of S_(i,j) means that the cell i has a lower elevation than cell j and for such m-connection α_(i,j) = 0 and no flow occurs between that i-j connection. β_(i,j) is an additional flow factor used in model calibration, allowing for appropriate partitioning between evaporated water and direct runoff. The α_(i,j) and β_(i,j) range from 0 to 1.
Considering cell i surrounded by m-number of j-cells – so also the m-number of cell connections each described by S_(i,j) – there are three possible flow scenarios: (1) if among all m-connections around i-cell, only one S_(i,j) is positive (the elevation of the i-cell is higher than the elevation of only one connected j cell), while all the other S_(i,j) are negative (so they all will be considered as zero in the sum in the denominator of Eq. 1), then α_(i,j) = 1 representing the single flow direction (SFD); (2) if among all m-connections around i-cell, there are two or more positive S_(i,j), then corresponding individual α_(i,j) values will be within the range 0 < α_(i,j) < 1; and (3) if all m-connections around i-cell are negative S_(i,j), then that i-cell will be a sink cell where all the water will be evaporated.

<p align="center">
  <img src="https://github.com/MostafaGomaa93/CRR_concept/blob/main/images/CRR%20alphas%20example.png" />
</p>

Figure 2. Example of the CRR concept showing the flow fractions (α_(i,j)) from one Upslope cell to the neighbouring downslope cells.\

**Applying in MODFLOW 6**\
The CRR is applied to MODFLOW 6 through the MOVER (MVR) package (Morway et al., 2021). The MVR package allows moving water from a feature in one package as a provider to a feature in the same package or in another package as a receiver. There are four different options for controlling such water movement (Langevin et al., 2017), the CRR concept uses the “FACTOR” option to control the transfer of available water (RI + Exf_gw) from the upslope UZF cells’ providers, within a given time step, to the adjacent downslope feature(s) (receivers), where that water can be: (i) evaporated (RE^e) and/or transferred downslope to be either (ii) reinfiltrated back to subsurface (RE^i) if the adjacent feature(s) represent UZF cell(s) – not fully saturated yet, or (iii) discharged as direct runoff (RE^s) into a stream, to contribute to streamflow, if the adjacent feature(s) represent SFR reach(es).\

**Inputs**\
As shown in Equation 1, the inputs for applying the CRR concept are the following:
	Elevation of every model cell.
	The neighbouring cells to each cell.
	Distance from the cell to each of its neighbouring cells.
All the above inputs can be retrieved from the discretization file and converting it to a shapefile. The notebooks in this repo show some examples of the CRR concept with different discretization packages (DIS, DISV, and DISU) of the MODFLOW 6 model.

