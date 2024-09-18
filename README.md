# CRR Concept
The CRR is the cascade routing and re-infiltration concept (Daoud et al., 2022; Daoud et al., 2024). The CRR define the flow direction based on the slopes (gradient), then, it routes the available water from the upslope areas to the adjacent downslope areas.
The available water depends on the conceptualization of the study area, but mainly, the available water can be the rejected infiltration (the amount of precipitation that will not infiltrate into the ground; RI) plus the groundwater exfiltration (the groundwater discharge to the land surface when the water table rises up or close to the land surface level; Exf<sub>gw</sub>). With applying the CRR concept, the available water can subsequently be: (a) evaporated (RE<sup>e</sup>); (b) re-infiltrated back to the subsurface (RE<sup>i</sup>); or (c) discharged as direct runoff (RE<sup>s</sup>) into the nearest surface water body (e.g. streams or lakes).

- The CRR is grid-based, designed mainly for use with the MODFLOW 6 groundwater model (Langevin et al., 2017), but can be applied to other similar grid-based models.
- The CRR can work with any grid type (e.g. rectangular, quadtree, Voronoi, etc).
- The CRR is a multi-flow directional, which partitions the flow among the downslope neighbouring cells based on the land surface gradient of the connected cells. 
- The flow partitioning among the 3 components (RE<sup>e</sup>, RE<sup>i</sup>, RE<sup>s</sup>) depends on many factors (Figure 1).
	- RE<sup>i</sup> is controlled by the vertical saturated hydraulic conductivity of topsoil (K<sub>sat</sub>). RE<sup>i</sup> is the amount of RI+Exf<sub>gw</sub>, which is less than the K<sub>sat</sub>-P<sub>e</sub> term. This term determines the availability of soil to still infiltrate more water RE<sup>i</sup>) after the initial infiltration (I) into the subsurface from the effective precipitation (P<sub>e</sub>).
- The remaining water (RI+Exf<sub>gw</sub>-RE<sup>i</sup>) is then partitioned between RE<sup>e</sup> and RE<sup>s</sup> through a flow partitioning factor (α<sub>i,j</sub>), which is the fraction of flow from cell i to the neighbouring j cell.


![plot](https://github.com/MostafaGomaa93/CRR_concept/blob/main/images/CRR_concept.png)
Figure 1. Schematic diagram of the CRR concept showing: (a) it’s three components (highlighted by the dash-line purple large circle); the two components surrounded by green dash-line rectangles are grouped as RE<sup>e</sup> (total evaporated water), while the two components surrounded by red dash-line rectangles are grouped as RE<sup>i</sup> (total re-infiltrated water), and the two surrounded by blue dash-line rectangles are grouped as RE<sup>s</sup> (total direct runoff). P<sub>e</sub> is the effective precipitation (precipitation – canopy interception), I is the initial amount of water infiltrated into the subsurface, and d<sub>surf</sub> is the surface depth – a user-specified depth relative to the land surface where the groundwater exfiltration starts; (b) the mathematical design.

## Flow fraction (α<sub>i,j</sub>) calculations
The flow fraction (α<sub>i,j</sub>) from a cell i to the neighbouring cell j is calculated as follows:

<p align="center">
  <img src="https://github.com/MostafaGomaa93/CRR_concept/blob/main/images/CRR%20equation.png" />
</p>

where
- S<sub>i,j</sub> = ((elv<sub>i</sub>-elv<sub>j</sub>) ⁄ l<sub>ij</sub>) is the slope gradient between the centre cell i and j-cells surrounding the cell i (Figure 2)
- elv<sub>i</sub> and elv<sub>j</sub> are land surface elevations of cells i and j respectively
- l<sub>ij</sub> is a distance between the centre of the cell i and the centre of one of j-cells surrounding i-cell
- m is the number of connected j cells to the cell i
- Note: the negative value of S<sub>i,j</sub> means that the cell i has a lower elevation than cell j and for such m-connection α<sub>i,j</sub> = 0 and no flow occurs between that i-j connection.
- β<sub>i,j</sub> is an additional flow factor used in model calibration, allowing for appropriate partitioning between evaporated water and direct runoff.
- The α<sub>i,j</sub> and β<sub>i,j</sub> range from 0 to 1.

Considering cell i surrounded by m-number of j-cells – so also the m-number of cell connections each described by S<sub>i,j</sub> – there are three possible flow scenarios:
1) if among all m-connections around i-cell, only one S<sub>i,j</sub> is positive (the elevation of the i-cell is higher than the elevation of only one connected j cell), while all the other S<sub>i,j</sub> are negative (so they all will be considered as zero in the sum in the denominator of Equation 1), then α<sub>i,j</sub> = 1;
2) if among all m-connections around i-cell, there are two or more positive S<sub>i,j</sub>, then corresponding individual α<sub>i,j</sub> values will be within the range 0 < α<sub>i,j</sub> < 1
3) if all m-connections around i-cell are negative S<sub>i,j</sub>, then that i-cell will be a sink cell where all the water will be evaporated.


<p align="center">
  <img src="https://github.com/MostafaGomaa93/CRR_concept/blob/main/images/CRR%20alphas%20example.png" />
</p>

Figure 2. Example of the CRR concept showing the flow fractions (α<sub>i,j</sub>) from one upslope cell to the neighbouring downslope cells.

## Applying in MODFLOW 6
- The CRR is applied to MODFLOW 6 through the MOVER (MVR) package (Morway et al., 2021).
- The MVR package allows moving water from a feature in one package as a provider to a feature in the same package or in another package as a receiver.
- The CRR concept uses the “FACTOR” option of the MVR package to control the transfer of available water (RI + Exf<sub>gw</sub>) from the upslope UZF cells’ providers, within a given time step, to the adjacent downslope feature(s) (receivers), where that water can be:
	- evaporated (RE<sup>e</sup>)
 	- reinfiltrated back to subsurface (RE<sup>i</sup>) if the adjacent feature(s) represent UZF cell(s) – not fully saturated yet
  	- discharged as direct runoff (RE<sup>s</sup>) into a stream, to contribute to streamflow, if the adjacent feature(s) represent SFR reach(es).

## Inputs
As shown in Equation 1, the inputs for applying the CRR concept are the following:
- Elevation of every model cell.
- The neighbouring cells to each cell.
- Distance from the cell to each of its neighbouring cells.

All the above inputs can be retrieved from the discretization file and converting it to a shapefile. The notebooks in this repo show some examples of the CRR concept with different discretization packages (DIS, DISV, and DISU) of the MODFLOW 6 model.

## Examples

## References
Daoud, M. G., Lubczynski, M. W., Zoltan, V., & Francés, A. P. (2022). Application of a novel cascade-routing and reinfiltration concept with a Voronoi unstructured grid in MODFLOW 6, for an assessment of surface-water/groundwater interactions in a hard-rock catchment (Sardon, Spain). Hydrogeology Journal, 1–27. https://doi.org/10.1007/S10040-021-02430-Z

Daoud, M.G., Morway, E.D., White, J.T., van der Tol, C., Lubczynski, M.W., (2024). Remote sensing evapotranspiration in ensemble-based framework to
enhance cascade routing and re-infiltration concept in integrated hydrological model applied to support decision making. J. Hydrol. 637, 131411 https://doi.org/10.1016/j.jhydrol.2024.131411

Langevin, C. D., Hughes, J. D., Banta, E. R., Niswonger, R. G., Panday, S., & Provost, A. M. (2017). Documentation for the MODFLOW 6 Groundwater Flow Model. In U.S. Geological Survey Techniques and Methods 6-A55. https://doi.org/10.3133/tm6a55

Morway, E. D., Langevin, C. D., & Hughes, J. D. (2021). Use of the MODFLOW 6 Water Mover Package to Represent Natural and Managed Hydrologic Connections. Groundwater, 59(6), 913–924. https://doi.org/10.1111/GWAT.13117
