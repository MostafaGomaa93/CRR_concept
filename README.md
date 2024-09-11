Cascade routing and re-infiltration (CRR) concept
The CRR concept simulates the transfer of the available water [e.g. rejected infiltration (RI) plus groundwater exfiltration (Exfgw)] from the upslope areas to the adjacent downslope areas. That water can subsequently be: (a) evaporated (REᵉ); (b) re-infiltrated back to the subsurface (REⁱ); or (c) discharged as direct runoff (REˢ) into the nearest surface water body (e.g. streams or lakes). REᵉ is the sum of the evaporated rejected infiltration (RIᵉ) and evaporated groundwater exfiltration (Exfᵉ). REⁱ is the sum of re-infiltrated rejected infiltration (RIⁱ) and re-infiltrated groundwater exfiltration (Exfⁱ). Lastly, REˢ is the sum of routed rejected infiltration (RIˢ) to the nearest surface water body (e.g. streams or lakes) and routed groundwater exfiltration (Exfˢ) to the nearest surface water body (e.g. streams or lakes). The mathematical design of the CRR concept is explained in Fig.1b of Daoud et al. 2024. REⁱ is controlled by the vertical saturated hydraulic conductivity of top soil (Ksat). REⁱ is the amount of RI + Exfgw, which is less than the Ksₐt -Pₑ term. This term determines the availability of soil to still infiltrate more water (REⁱ) after the initial infiltration (I) into the subsurface from the effective precipitation (Pₑ). The remaining water (RI + Exfgw -REⁱ) is then partitioned between REˢ and REᵉ through a flow partitioning factor (αi,j), which is the fraction of flow from cell i to the neighboring j cell.

To apply the CRR concept (equation 23 in Daoud et al. 2022), the MODFLOW 6 water mover (MVR) package needs to be activated. The following information is required:

The MODFLOW 6 model grid.
The connected grid cells to each grid cell (with their ID (Cellid)).
Land surface elevation of each grid cell.
Distance between each connected grid cells.
Slope between each connected grid cells.
All this information is calculated in this notebook. The only data input is the MODFLOW 6 model files.

References
Daoud et al., 2022 : https://doi.org/10.1007/S10040-021-02430-Z
Daoud et al., 2024 : https://doi.org/10.1016/j.jhydrol.2024.131411
