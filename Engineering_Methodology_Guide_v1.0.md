# Gulf of Mexico CO₂ Storage Decision Support Platform

## Methodology and Technical Notes

**Version 1.0**

---

# 1. Overview

The Gulf of Mexico CO₂ Storage Decision Support Platform is an engineering screening tool developed to evaluate depleted offshore hydrocarbon reservoirs for geological CO₂ storage using publicly available Bureau of Ocean Energy Management (BOEM) reservoir data.

The objective of the platform is not to replace reservoir simulation or detailed site characterization, but to provide a rapid and consistent method for comparing thousands of potential storage opportunities using a common engineering framework.

The current database contains more than 8,000 offshore storage locations generated from individual BOEM sand records.

The engineering workflow consists of:

1. BOEM data preparation and quality control.
2. Storage capacity estimation for each BOEM sand.
3. Injectivity estimation for each BOEM sand.
4. Aggregation of stacked sands into location-level opportunities.
5. Engineering ranking.
6. Interactive visualization and filtering.

---

# 2. Engineering Methodology

The platform combines published engineering relationships with project-specific data processing and ranking methods developed for this application.

| Component | Method |
|----------|--------|
| Storage Capacity | Gulf of Mexico storage correlation (Agartan et al., 2018) |
| Produced-Water Scenarios | Developed for this platform |
| Injectivity Estimation | Relationship presented by Mishra et al. (2024) |
| Location Aggregation | Developed for this platform |
| Engineering Ranking | Developed for this platform |
| Interactive Screening Workflow | Developed for this platform |

The published methods are used only for estimating storage capacity and injectivity. The engineering workflow, ranking methodology, aggregation procedures, dashboard implementation, and visualization were developed specifically for this platform.

---

# 3. Storage Capacity Estimation

Storage capacity is estimated at the **individual BOEM sand level** before any aggregation is performed.

The methodology is based on the Gulf of Mexico storage-capacity correlation presented by **Agartan et al. (2018)**. Unlike simple pore-volume calculations, the correlation relates **total produced-fluid volume** to estimated CO₂ storage capacity.

Separate empirical relationships are applied to:

- Oil reservoirs
- Gas reservoirs
- Combined oil-and-gas reservoirs

The appropriate relationship is selected automatically using the BOEM sand classification.

## Produced-Water Scenarios

Historical produced-water volumes vary considerably between reservoirs. Rather than assuming a single water-production history, four engineering scenarios are evaluated.

| Dashboard Scenario | Produced-Water Assumption |
|-------------------|---------------------------|
| No Produced Water | Hydrocarbon production only |
| 0.5× | Produced water equals 50% of hydrocarbon withdrawal |
| 1× | Produced water equals hydrocarbon withdrawal |
| 2× | Produced water equals twice the hydrocarbon withdrawal |

These scenarios are intended to evaluate the sensitivity of storage estimates to increasing total fluid withdrawal and should not be interpreted as actual production histories.

---

# 4. Location Aggregation

The original BOEM database contains one record for each producing sand.

Many offshore locations contain multiple stacked reservoirs that share identical surface coordinates. Displaying every individual sand would produce overlapping map symbols and reduce the usefulness of the dashboard.

To simplify interpretation, engineering calculations are first completed at the individual sand level and then aggregated into a single storage location.

The aggregation methodology is summarized below.

| Property | Aggregation Method |
|----------|--------------------|
| Storage Capacity | Sum |
| Maximum Injectivity | Maximum |
| Reservoir Depth | Mean |
| Water Depth | Mean |
| Reservoir Pressure | Mean |
| Net Thickness | Mean |
| Number of Sands | Count |

Storage capacity is summed because stacked reservoirs contribute additional storage volume. Maximum injectivity is retained because multiple storage intervals may be completed independently within the same storage complex.

The number of contributing sands is retained as an indicator of storage complexity and is incorporated into the Balanced Ranking.

---

# 5. Injectivity Estimation

Injectivity is estimated independently for every BOEM sand using the relationship presented by **Mishra et al. (2024)**.

The methodology combines reservoir permeability, net thickness, and the available pressure differential to estimate annual CO₂ injection capacity.

The available pressure differential is controlled by the selected fracture-pressure gradient.

Three engineering scenarios are provided.

| Scenario | Fracture Pressure Gradient |
|----------|---------------------------:|
| Conservative | 0.70 psi/ft |
| Intermediate | 0.80 psi/ft |
| Higher Pressure | 0.90 psi/ft |

The fracture pressure is estimated using the total vertical depth from sea level to the reservoir, which includes both water depth and reservoir depth.

The resulting injectivity estimates are reported in **million metric tonnes of CO₂ per year (Mt/year)**.

Estimated injection time is subsequently calculated by dividing storage capacity by annual injectivity.

Injection time provides an indication of the operational effort required to utilize the estimated storage resource and serves as an additional engineering screening parameter.
# 6. Engineering Ranking

## 6.1 Overview

The platform provides three independent ranking methods to support different engineering objectives. Each ranking emphasizes a different aspect of storage development and allows users to evaluate storage opportunities from multiple perspectives.

The three ranking methods are:

- Storage Ranking
- Injectivity Ranking
- Balanced Ranking

Each ranking is calculated after all engineering properties have been estimated and location-level aggregation has been completed.

---

## 6.2 Storage Ranking

Storage Ranking orders locations solely according to their estimated CO₂ storage capacity.

This ranking is intended for users interested in identifying the largest storage resources regardless of injection performance or drilling considerations.

Locations with greater storage capacity receive higher rankings, even if they require longer injection periods or occur in deeper water.

Storage Ranking is therefore useful when evaluating the maximum regional storage resource but should not be interpreted as an indicator of overall project attractiveness.

---

## 6.3 Injectivity Ranking

Injectivity Ranking orders locations according to their estimated annual CO₂ injection capacity.

Reservoirs possessing favorable permeability, net thickness, and allowable pressure differential generally receive the highest rankings.

Unlike Storage Ranking, this method does not consider the total storage resource. Consequently, relatively small reservoirs with excellent injectivity may rank above larger reservoirs having poor injection performance.

This ranking is particularly useful when identifying reservoirs capable of supporting high annual injection rates.

---

## 6.4 Balanced Ranking

Most storage projects require a compromise between storage capacity and injection performance while also considering drilling depth, offshore conditions, and transportation distance.

To support this objective, the platform includes a Balanced Ranking that combines multiple engineering criteria into a single screening score.

The current implementation incorporates the following engineering parameters.

| Engineering Parameter | Weight |
|----------------------|-------:|
| Storage Capacity | 0.45 |
| Injectivity | 0.25 |
| Reservoir Depth | 0.10 |
| Water Depth | 0.10 |
| Distance to Shore | 0.05 |
| Storage Complexity | 0.05 |

The weighting factors intentionally place greater emphasis on storage capacity and injectivity because these parameters most strongly influence the technical potential of a storage project.

Reservoir depth and water depth represent drilling and offshore development considerations, while distance to shore provides a first-order indication of transportation requirements.

Storage complexity is represented by the number of BOEM sands aggregated into a single location. Locations containing numerous stacked reservoirs may require more complex completion strategies and therefore receive a modest ranking penalty.

The weighting factors adopted in Version 1.0 represent one engineering interpretation of project priorities and may be modified in future versions of the platform.

---

## 6.5 Interpretation of Rankings

The ranking values are intended to provide **relative engineering comparisons** rather than absolute measures of project quality.

Changing the produced-water scenario or fracture-pressure gradient will change the estimated storage capacity or injectivity and may therefore alter the ranking of individual locations.

Users are encouraged to evaluate storage opportunities under multiple engineering scenarios rather than relying on a single ranking result.

---

# 7. Dashboard Interpretation

All engineering calculations are completed during data preparation. Consequently, the dashboard performs filtering and visualization only, allowing rapid interaction with more than 8,000 storage locations.

The principal dashboard controls are summarized below.

| Control | Description |
|----------|-------------|
| Produced Water Scenario | Selects the storage-capacity scenario. |
| Fracture Pressure Gradient | Selects the injectivity scenario. |
| Ranking Method | Storage, Injectivity, or Balanced Ranking. |
| Minimum Storage | Minimum storage capacity displayed. |
| Minimum Injectivity | Minimum annual injectivity displayed. |
| Maximum Water Depth | Filters locations by offshore water depth. |
| Maximum Reservoir Depth | Filters locations by reservoir depth. |
| Maximum Distance to Shore | Filters locations by distance from the coastline. |
| Maximum Injection Time | Filters locations by estimated injection duration. |
| Hide Missing Injectivity | Removes locations lacking sufficient data for injectivity estimation. |

The dashboard also includes a **High-value Opportunities** display mode.

This mode highlights storage locations satisfying predefined engineering criteria while allowing users to switch back to the complete dataset for broader regional evaluation.

The interactive map uses:

- **Circle size** to represent estimated storage capacity.
- **Circle color** to represent estimated annual injectivity.

Selecting a location displays additional engineering information, including storage capacity, injectivity, estimated injection time, reservoir properties, and ranking metrics.

---

# 8. Assumptions and Limitations

The platform is intended as a regional engineering screening tool using publicly available BOEM reservoir data.

Several simplifying assumptions were adopted to ensure that thousands of storage opportunities could be evaluated consistently using a common engineering framework.

Accordingly:

- Storage estimates are based on the Gulf of Mexico correlation presented by Agartan et al. (2018).
- Injectivity estimates are based on the relationship presented by Mishra et al. (2024).
- Engineering calculations are performed at the individual BOEM sand level prior to location aggregation.
- Ranking scores are intended to support engineering screening and should not be interpreted as project-specific design criteria.

The platform does **not** perform:

- numerical reservoir simulation;
- geomechanical analysis;
- pressure interference analysis;
- plume migration prediction;
- economic evaluation; or
- regulatory assessment.

The results should therefore be interpreted as preliminary engineering indicators intended to identify storage opportunities that merit more detailed geological characterization and reservoir engineering studies.

---

# 9. References

1. Agartan, E., et al. (2018). *Correlation between cumulative produced-fluid volume and CO₂ storage capacity for Gulf of Mexico reservoirs.* (Reference used for storage-capacity estimation.)

2. Mishra, A., et al. (2024). *Relationship used for estimating CO₂ injectivity from reservoir properties.* (Reference used for injectivity estimation.)

3. Bureau of Ocean Energy Management (BOEM). Gulf of Mexico reservoir and production databases.

---

## Citation

If this platform contributes to your work, please cite the associated GitHub repository together with the original references used for the storage-capacity and injectivity methodologies.

---

**Disclaimer**

This platform is intended to prioritize engineering effort by providing a transparent and consistent regional screening methodology. The results are not a substitute for detailed geological characterization, numerical reservoir simulation, or project-specific engineering design.