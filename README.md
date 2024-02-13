# Co2 Calc

### Objective
The Co2 Calc program is designed to provide a transparent and sophisticated interface, enabling passengers to assess their individual Co2 emissions contribution. This assessment is intricately calculated based on an array of pivotal travel-related factors including the chosen route, the month of travel, vehicle type, and the nature of the journey (roundtrip or one-way). A distinctive feature of this program is the integration of a scalar factor, which meticulously adjusts the emissions calculation to align with the relative passenger load for a given month. This ensures a nuanced and contextually estimation of each individual's Co2 responsibility.

It is imperative to recognize the variability in trip compositions—ranging from voyages predominantly consisting of cargo with a minimal passenger count, to those heavily laden with passengers and minimal cargo. In scenarios where a voyage carries a mere handful of passengers amidst substantial cargo, it is inequitable for these few individuals to bear the full environmental cost typically distributed among a larger passenger cohort.

This program addresses these complexities head-on. Given the dynamic nature of travel variables such as weather conditions, trip frequency, passenger count, and cargo load, the Co2 Calc employs averages and scalar factors. This approach enables the program to provide a fair and statistically reasoned overview of Co2 emissions, offering passengers a credible and clear understanding of their environmental footprint.

### Co2 Calculation Methodology

The mathematical model underpinning the Co2 Calc program is meticulously crafted to ensure precise and contextually relevant computations of Co2 emissions. This model incorporates advanced mathematical constructs and statistical methodologies to account for a comprehensive range of variables that influence the environmental impact of travel. The core of this model lies in its ability to dynamically adjust Co2 emission calculations in alignment with passenger load variations, vehicle types, and travel routes. The following is an in-depth overview of the computational steps and methodologies employed:

1. **Data Retrieval and Initialization**:
   - The model begins by collating essential input parameters from the user. These parameters include `month`, `route`, `passengers`, `vehicle`, and `roundtrip` status.
   - It then retrieves the corresponding data for the specified month (`MonthData`), the distance of the chosen route (`routeDistance`), and the weight of the selected vehicle (`vehicleWeight`). This data forms the foundational base for subsequent calculations.

2. **Weight Distribution and Emission Calculation**
   - The Co2 emission per nautical mile per passenger (`co2PerNauticalMilePerPax`) is split between the weight distribution of the passengers and the vehicle. This is done by calculating the weight percentages of passengers and vehicles, respectively.

$$
\text{paxCo2} = \text{co2PerNauticalMilePerPax} \times \text{paxWeightPercent}
$$

$$
\text{vehicleCo2} = \text{co2PerNauticalMilePerPax} \times \text{vehicleWeightPercent}
$$
   
- The vehicle's Co2 emission is computed by multiplying its weight with its corresponding Co2 emission factor (`vehicleCo2`).

$$
\text{vehicleEmission} = \left(\frac{\text{vehicleWeight}}{1000}\right) \times \left(\text{vehicleCo2} \times \text{routeDistance}\right)
$$

- The base Co2 emission per passenger is determined by multiplying the passenger-specific Co2 emission factor (`paxCo2`) with the route distance.

$$
\text{baseCo2PerPax} = \text{paxCo2} \times \text{routeDistance}
$$

3. **Passenger Load Normalization**:
   - In this crucial step, the number of passengers is normalized against the observed range (minimum to maximum) of passenger numbers. This normalization process translates the raw passenger count into a standardized value between 0 and 1, reflecting the relative load compared to historical data.

$$
\text{normPassengers} = \frac{\text{passengers} - \text{minPassengers}}{\text{maxPassengers} - \text{minPassengers}}
$$

4. **Scalar Factor Application**:
   - The normalized passenger load is further refined by applying a scalar factor. This factor adjusts the normalized value, ensuring that the Co2 emission per passenger is scaled appropriately to reflect periods of low or high passenger load.

$$
\text{inverseNormalizedPassengers} = (1 + \text{scalarFactor}) - \text{normPassengers} \times (2 \times \text{scalarFactor})
$$

5. **Adjusted Co2 Per Passenger**:
   - The Co2 emission per passenger is recalibrated using the scalar-adjusted load factor. This adjusted value represents a more accurate and equitable distribution of Co2 emissions among passengers, accounting for the variable passenger load.
   
$$
\text{adjustedCo2PerPax} = \text{inverseNormalizedPassengers.map}(scalar \Rightarrow (\text{baseCo2PerPax} \times scalar).toFixed(2))
$$

6. **Passenger Emission Calculation**:
   - The overall passenger emission is calculated by multiplying the adjusted Co2 per passenger with the total number of passengers.
   
$$
\text{paxEmission} = \text{adjustedCo2PerMonth}[month] \times passengers
$$

7. **Total Emission Computation**:
   - The total Co2 emission for the journey is determined by summing the passenger emission and the vehicle emission. This sum is then adjusted based on whether the journey is a roundtrip or one-way.
   
$$
\text{totalEmission} = (\text{paxEmission} + \text{vehicleEmission}) \times \text{roundtrip}
$$

8. **Result Formatting and Presentation**:
   - Finally, the total emission figure is formatted for presentation. The emission is displayed in grams and also converted to kilograms for enhanced clarity and understanding.
   
$$
\text{totalGrams} = \text{totalEmission.toFixed}(2)
$$
$$
\text{totalKg} = (\text{totalEmission} / 1000).toFixed(2)
$$

This refined methodology ensures that the Co2 Calc program provides an accurate, fair, and contextually relevant assessment of individual Co2 emissions, empowering passengers with precise insights into their environmental footprint.

### Understanding the Usage of `scalarFactor`

The `scalarFactor` plays a crucial role in the Co2 emissions calculation by providing a dynamic adjustment to the per-passenger emission based on the passenger count relative to the observed data range. This factor ensures that the emissions are not just a linear function of the number of passengers, but also take into account the variability and distribution of passenger numbers over different periods.

### Purpose of `scalarFactor`:
1. **Dynamic Adjustment**: The primary purpose of the `scalarFactor` is to introduce a dynamic component into the Co2 calculation. It adjusts the emission per passenger not just based on the direct number of passengers but also considers how this number relates to the typical (minimum and maximum) passenger counts observed.

2. **Normalization and Scaling**: It normalizes the passenger data, scaling the Co2 emissions calculation to be proportional to the number of passengers while considering the data's range. This means that in months with exceptionally high or low passenger numbers, the emissions per passenger will be adjusted to reflect this abnormality, ensuring a fair distribution of Co2 responsibility.

3. **Flexibility in Adjustment**: By allowing the `scalarFactor` to be a variable rather than a fixed value, the model introduces flexibility. Different values of `scalarFactor` can represent different scales of adjustment, making the model adaptable to various scenarios or assumptions about how passenger numbers should influence individual Co2 responsibility.

### Working of `scalarFactor`:
The `scalarFactor` works in conjunction with the normalization of passenger numbers. Here's a step-by-step breakdown of its function:

1. **Normalize Passenger Count**:
   - The passenger count is normalized to a value between 0 and 1, representing where the current passenger count stands relative to the historical minimum and maximum.

2. **Apply `scalarFactor`**:
   - The normalized value is then adjusted by the `scalarFactor`, ensuring that the final value represents a scaled and adjusted version of the Co2 emissions per passenger. This adjustment is crucial to ensure that the emissions per passenger are fair and representative of their actual share, especially in scenarios where the number of passengers is significantly higher or lower than usual.

### Significance of `scalarFactor`:
The introduction of `scalarFactor` adds a layer of sophistication to the Co2 emissions model, ensuring that it is sensitive to fluctuations in passenger numbers and can provide a more accurate and fair distribution of Co2 responsibility. It ensures that the model is not overly simplistic and can handle the complexities and variabilities inherent in passenger data.

### Approximation of scalarFactor
The scalarFactor is determined by taking the Pearson correlation coefficient between passenger count and CO2 emissions. The equation for Pearson correlation coefficient:

$$
r_{xy} = \frac{n(\sum xy) - (\sum x)(\sum y)}{\sqrt{[n\sum x^2 - (\sum x)^2][n\sum y^2 - (\sum y)^2]}}
$$

the breakdown of that is:

The correlation between variables x and y: $r_{xy}$
The number of data points: $n$
The sum of the product of x and y: $\sum xy$
The sums of x and y respectively: $\sum x$ and $\sum y$
The sums of the squares of x and y respectively: $\sum x^2$ and $\sum y^2$

In our case, x represents the CO2 emissions and y represents the passenger count.

After calculating the correlation coefficient, its absolute value is taken as the scalarFactor.

$$
scalarFactor = |r_{xy}|
$$

It is approximately 0.037. This indicates a very strong positive relationship between the two variables. The absolute value of the correlation coefficient hence can be a good choice for the scalarFactor.
This means 3.7% of the variability in CO2 emissions can be explained by the passenger count. Using this as a scalarFactor would thus imply that the CO2 emissions calculation would be scaled and adjusted according to the passenger count, giving a fair distribution of CO2 responsibility.

In summary, the `scalarFactor` is a vital component of the Co2 emissions calculation, ensuring that the model is dynamic, adaptable, and fair in attributing Co2 responsibility to individual passengers.


## Data Overview

The data is a projection based on the previous year's data as not much changes from year to year. The Pax Emission of gram pr Nautical mile is an averge through the year. Same goes for freight emission.

### General Information
- **Year**: 2023
- **CO2 Emissions per Nautical Mile per Passenger**: 197.68
- **CO2 Passenger Scalar Factor**: 0.037

### Ferry Routes (Distance in Nautical Miles)
| Route                     | Distance (Nautical Miles) |
|---------------------------|--------------------------:|
| Torshavn-Hirtshals        | 634.9892                  |
| Hirtshals-Torshavn        | 634.9892                  |
| Torshavn-Seydisfjordur    | 275.378                   |
| Seydisfjordur-Torshavn    | 275.378                   |
| Hirtshals-Seydisfjordur   | 910.3672                  |
| Seydisfjordur-Hirtshals   | 910.3672                  |

### Vehicle Types and Weight Range

| Code | Vehicle Type                                     | Weight Low (Kg) | Weight High (Kg) |
|------|--------------------------------------------------|----------------|-----------------|
| BIKE | Bike                                            | 7              | 14              |
| BUS  | Coach up to 14m L                               | 206.9          | 333.33           |
| CAR  | Car below 1.9m H and Max 5m L                   | 1500           | 2200            |
| CARE | Electric car below 1.9m H and 5m L              | 1800           | 2500            |
| CARH | Car over 2.5 meters Height and max 5 meters Length | 2000       | 3000            |
| CARHE| Electric car over 2.5 meters Height and max 5 meters Length | 2500 | 3500        |
| CARM | Car 1.9-2.5 meters Height and max 5 meters Length | 1800          | 2300            |
| CARMD| Car for disabled 1.9-2.5 meters Height and max 5 meters Length | 2000 | 3000      |
| CARME| Electric car 1.9-2.5 meters Height and max 5 meters Length | 2000 | 2800        |
| CVAN | Car and caravan                                 | 3500           | 7000            |
| CVANE| Electric car and caravan                        | 4000           | 8000            |
| MC   | Motorcycle                                      | 150            | 400             |
| MCS  | Motorcycle with sidecar                         | 350            | 500             |

### Monthly Data

| Month     | CO2 Emissions | Passengers | Vehicle Weight (kg) |
|-----------|--------------:|-----------:|------------------:|
| January   | 1049.83       | 361        | 319,135.50           |
| February  | 2377.17       | 1070       | 876,400.00           |
| March     | 2948.08       | 5312       | 2,790,852.50           |
| April     | 3154.51       | 8752       | 4,500,232.50           |
| May       | 3294.43       | 9492       | 7,375,448.00           |
| June      | 5792.69       | 15871      | 13,506,153.00           |
| July      | 6800.8        | 23693      | 16,942,257.50           |
| August    | 5644.69       | 19739      | 15,177,597.50           |
| September | 3424.61       | 9664       | 8,048,166.50           |
| October   | 3746.82       | 8348       | 5,029,556.00           |
| November  | 3194.62       | 5682       | 3,002,336.50           |
| December  | 2327.04       | 820        | 677,160.50           |


## Final Note

Co2 Mission pr passenger and freight as well vehicle weight and more has been collected from a deep dive in our booking system and Co2 reports from 2023.. these values are then used to approximate the Co2 Emission of a passengers journey for the next year. this will of course need to be mended on any given year and perhaps also refined.