# Legacy Post-production Storage Methodology

> **Legacy methodology:** This stage is no longer part of the digital advertising methodology. Post-production storage is a cross-channel consideration and should be addressed at campaign level. This page is retained for reference only.

## Storage

The storage stage accounted for emissions arising from storing the assets created for a campaign. Following consultation with post-production experts, it was determined that tracking the full flow of files between post-production teams would be too complicated and diverse for a standard methodology. As such, the storage of the final files, also known as the masters bundle, was accounted for.

The methodology accounted for four storage media:

- Local HDD
- Local SSD
- Cloud
- Linear tape

Assets were assumed to be stored for 10 years. Use-phase emissions were excluded for drives because external drives were assumed to be accessed infrequently.

### Data

| Variable | Type | Value | Unit | Description | Notes |
| --- | --- | --- | --- | --- | --- |
| Total Masters Size | Input |  | GB | Total size of all final files for the campaign. | Files could be shared between channels or campaigns. |
| HDD Copies | Input |  |  | Number of copies stored on local HDDs. | |
| SSD Copies | Input |  |  | Number of copies stored on local SSDs. | |
| Cloud Copies | Input |  |  | Number of copies stored in the cloud. | |
| LTO Copies | Input |  |  | Number of copies stored on tape drives. | |
| HDD Intensity | Constant | $1.60 \times 10^{-1}$ | kg CO2e / GB | Embodied emissions of storing 1 GB of data on a local HDD. | |
| SSD Intensity | Constant | $2.00 \times 10^{-2}$ | kg CO2e / GB | Embodied emissions of storing 1 GB of data on a local SSD. | |
| Cloud Intensity | Constant | $2.53 \times 10^{-2}$ | kg CO2e / GB | Embodied emissions of storing 1 GB of data in the cloud. | |
| LTO Intensity | Constant | $1.14 \times 10^{-3}$ | kg CO2e / GB | Embodied emissions of storing 1 GB of data on tape drives. | |

### Equation

$$
    {Storage\ Emissions} = \text{Total\ Masters\ Size} \times \Big( (\text{HDD\ copies} \times \text{HDD\ intensity}) + (\text{SSD\ copies} \times \text{SSD\ intensity}) + (\text{LTO\ copies} \times \text{LTO\ intensity}) + (\text{Cloud\ copies} \times \text{Cloud\ intensity}) \Big)
$$

### Example

#### Example Input Data

| Variable | Value | Unit |
| --- | --- | --- |
| Total Masters Size | 50 | GB |
| HDD Copies | 1 | |
| SSD Copies | 1 | |
| Cloud Copies | 3 | |
| LTO Copies | 2 | |

#### Applying the Equation

$$
\begin{align*}
    {Storage\ Emissions} &= 50 \times \Big( (1 \times 1.60 \times 10^{-1}) + (1 \times 2.00 \times 10^{-2}) + (2 \times 1.14 \times 10^{-3}) + (3 \times 2.53 \times 10^{-2}) \Big) \\
    &= 50 \times \Big( 0.16 + 0.02 + 0.00228 + 0.0759 \Big) \\
    &= 50 \times 0.25818 \\
    &= 12.909 \text{ kg CO2e}
\end{align*}
$$

## Data Sources

| Variable | Source |
| --- | --- |
| HDD Intensity | [Tannu, S., & Nair, P. J. (2023).](https://arxiv.org/pdf/2207.10793) |
| SSD Intensity | [Tannu, S., & Nair, P. J. (2023).](https://arxiv.org/pdf/2207.10793) |
| Cloud Intensity | [ADEME, Base Empreinte](https://base-empreinte.ademe.fr/) |
| LTO Intensity | [Fujifilm estimates on LTO-8](https://asset.fujifilm.com/www/de/files/2023-10/97ddc3473883421cef1fb820d236dfa2/Improving_IT_Sustainability_with_Tape_BJC_0.pdf) |
