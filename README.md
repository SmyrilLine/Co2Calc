# Co2 Calc

### Objective
Create a simple user interface for passengers ability to review their Co2 emissions pr trip.

## Math
The equation for calculating the average passenger trip Co2 is done as the following.

**Co2 Factor**
   $$
   \text{Co2Factor} = \frac{\text{MonthData.co2}}{\text{MonthData.passengers}}
   $$

This will give us a factor of 2.8 in January and 0.3 in June .. as passengers increase the emission responsibility is distributed. 

**Vehicle Emission**
   $$
   \text{vehicleEmission} = (\text{vehicleWeight} \times \text{MonthData.freight\_emission}) \times \text{Co2Factor}
   $$

**Passenger Emission (paxEmission)**
   $$
   \text{paxEmission} = (\text{MonthData.passenger\_emission} \times \text{routeDistance}) \times \text{Co2Factor}
   $$

Passenger emission is calculated by the yearly average passenger emission pr nautical mile multiplied with route distance. which is then multiplied with the Co2 factor to give a total passenger responsibility value.

**Total Emission**
   $$
   \text{totalEmission} = (\text{paxEmission} + \text{vehicleEmission}) \times \text{roundtrip}
   $$

The total emission aims to sum the vehicle with the passenger emission and doubles up if a round trip is selected.

The singular equation would be:

$$
\begin{align*}
\text{totalEmission} = \bigg( &\left( \text{MonthData.passenger\_emission} \times \text{routeDistance} \right) \\
+ &\left( \text{vehicleWeight} \times \text{MonthData.freight\_emission} \right) \\
\bigg) &\times \frac{\text{MonthData.co2}}{\text{MonthData.passengers}} \\
&\times \text{roundtrip}.
\end{align*}
$$

In this equation, `roundtrip` takes the value of either 1 or 2, effectively doubling the total emission for a round trip.

## Data Overview

The data is a projection based on the previous year's data as not much changes from year to year. The Pax Emission of gram pr Nautical mile is an averge through the year. Same goes for freight emission.

## Routes

| Route                      | Distance (NM) |
|----------------------------|---------------|
| Torshavn-Hirtshals         | 634.9892      |
| Hirtshals-Torshavn         | 634.9892      |
| Torshavn-Seydisfjordur     | 275.378       |
| Seydisfjordur-Torshavn     | 275.378       |
| Hirtshals-Seydisfjordur    | 910.3672      |
| Seydisfjordur-Hirtshals    | 910.3672      |

## Vehicles

| Vehicle    | Weight (kg) |
|------------|-------------|
| Small_Car  | 1500        |
| SUV        | 2200        |
| Motorcycle | 400         |
| Truck      | 5000        |

## Monthly Data

| Month     | Freight Emission | Pax Emission (g/NM)| CO2      | Passengers |
|-----------|------------------|--------------------|----------|------------|
| January   | 279.61           | 137.89             | 1049.83  | 361        |
| February  | 279.61           | 137.89             | 2377.17  | 1070       |
| March     | 279.61           | 137.89             | 2948.08  | 5312       |
| April     | 279.61           | 137.89             | 3154.51  | 8752       |
| May       | 279.61           | 137.89             | 3294.43  | 9492       |
| June      | 279.61           | 137.89             | 5792.69  | 15871      |
| July      | 279.61           | 137.89             | 6800.8   | 23693      |
| August    | 279.61           | 137.89             | 5644.69  | 19739      |
| September | 279.61           | 137.89             | 3424.61  | 9664       |
| October   | 279.61           | 137.89             | 3746.82  | 8348       |
| November  | 279.61           | 137.89             | 3194.62  | 5682       |
| December  | 279.61           | 137.89             | 2327.04  | 820        |

