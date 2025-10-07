# Methodology

This document contains the methodology for estimating digital ad emissions and the models it relies upon. For an overview of the data and sources, please go to the [Data](data.md) page.

## Components

The methodology is divided into four sections that represent different stages of a digital advertising campaign.

Each stage features one or more **levels**, or ways to estimate emissions. Each level uses different data. Higher levels use more granular data and should yield more representative and lower emissions figures.

The data inputs for each stage are divided into two categories:
- **Inputs** that are specific to each campaign and should change between applications of the methodology.
- **Constants** that should not vary between applications of the methodology. Recommended figures are provided.

At the end of this document are the server, network, and foreign grid intensity models that power different pieces of the methodology.

## Grid Emissions Factors

For the purposes of consistency between estimations, the Methodology & Framework Working Group recommends using local grid emission intensities from [Ember](https://ember-energy.org/data/yearly-electricity-data/). When comparing results obtained by applying the proposed methodology, using the reference EF database reduces variance. 

Organisations applying the methodology may still opt to utilise proprietary grid EF databases. In that case, the following guidance applies:
- Average, location-based figures should be used.
- Market-based figures can be reported in addition and in compliance with GHG Protocol Scope 2 guidance.

Furthermore, organizations applying the proposed methodology should be mindful of the potential error in the consumption stage if using hourly figures as charging of certain devices does not necessarily occur at the same time as ad delivery.

## Stages

### Storage

The storage stage accounts for emissions arising from storing the assets created for a campaign. Following consultation with post-production experts, it was determined that tracking the full flow of files between post-production teams would be too complicated and diverse for a standard methodology. As such, the storage of the final files, also known as the masters bundle, is accounted for.

The methodology accounts for four storage mediums:
- Local HDD
- Local SSD
- Cloud
- Linear Tape

Assets are assumed to be stored for 10 years. Use-phase emissions are excluded for drives as external drives are assumed to be used with infrequent access.

#### Data

| Variable | Type | Value | Unit | Description | Notes |
| --- | --- | --- | --- | --- | --- |
| Total Masters Size | Input |  | GB | Total size of all final files for the campaign. | Files could be shared between channels or campaigns. |
| HDD Copies | Input |  |  | Number of copies stored on local HDDs. ||
| SSD Copies | Input |  |  | Number of copies stored on local SSDs. ||
| Cloud Copies | Input |  |  | Number of copies stored on the cloud. ||
| LTO Copies | Input |  |  | Number of copies stored on tape drives. ||
| HDD Intensity | Constant | $1.60 \times 10^{-1}$ | kg CO2e / GB | Embodied emissions of storing 1 GB of data on a local HDD. ||
| SSD Intensity | Constant | $2.00 \times 10^{-2}$ | kg CO2e / GB | Embodied emissions of storing 1 GB of data on a local SSD. ||
| Cloud Intensity | Constant | $2.53 \times 10^{-2}$ | kg CO2e / GB | Embodied emissions of storing 1 GB of data on Cloud. ||
| LTO Intensity | Constant | $1.14 \times 10^{-3}$ | kg CO2e / GB | Embodied emissions of storing 1 GB of data on tape drives. ||

#### Equation

$$
    {Storage\ Emissions} = \text{Total\ Masters\ Size} \times \Big( (\text{HDD\ copies} \times \text{HDD\ intensity}) + (\text{SSD\ copies} \times \text{SSD\ intensity}) + (\text{LTO\ copies} \times \text{LTO\ intensity}) + (\text{Cloud\ copies} \times \text{Cloud\ intensity}) \Big)
$$

#### Example
##### Example Input Data
| Variable | Value | Unit |
|---|---|---|
| Total Masters Size | 50 | GB |
| HDD Copies | 1 | |
| SSD Copies | 1 | |
| Cloud Copies | 3 | |
| LTO Copies | 2 | |

##### Applying the Equation
$$
\begin{align*}
	{Storage\ Emissions} &= 50 \times \Big( (1 \times 1.60 \times 10^{-1}) + (1 \times 2.00 \times 10^{-2}) + (2 \times 1.14 \times 10^{-3}) + (3 \times 2.53 \times 10^{-2}) \Big) \\
&= 50 \times \Big( 0.16 + 0.02 + 0.00228 + 0.0759 \Big) \\
&= 50 \times 0.25818 \\
&= 12.909 \text{ kg CO2e}
\end{align*}
$$


### Selection

The selection stage accounts for the emissions associated with server usage and network transfer during the buying and selling of ad space. It is hardest stage to model due to the complexity of supply chains and limited transparency. Providing default data also necessitates aggregation across supply chains that have diverse characteristics, increasing the estimation's error. However, since it is one of the most impactful areas in terms of emissions and an area where there is a lot of potential for waste reduction, having a standard methodology is vital.

Three distinctions are made between transaction types:
- **Programmatic**, defined as any real-time transaction of ad space or any transaction where the programmatic supply chain is activated.
- **Direct**, defined as any transaction where the programmatic supply chain is never activated (even for other bidders).
- **End-to-end platforms**, defined as any transaction of ad space that does not leave a single platform's ecosystem (e.g. buying ad slots on a platform's owned & operated media through a self-serve solution).

Selection has two major components: estimating the volume of server and networking activity and estimating this activity's carbon intensity.

#### Levels - Activity

| Level | Method | Notes |
|-------|--------|-------|
|0|Default|For cases where no ads.txt is available.|
|1|Ads.txt|Based on ads.txt length as a proxy and aggregate data on supply chain depth.|
|2|Contributed data|Based on publishers, SSPs and DSPs sharing aggregate data on volumes of bidding activity for each partner they work with. **WIP**|

#### Levels - Server Intensity

| Level | Method | Notes |
|-------|--------|-------|
|0|Default|Server model based on life cycle assessments of VMs.|
|1|Allocation from global data|Based on contributed data on the impact of server operations, allocated to the impression from a global level. **WIP**|
|2|Allocation from regional data|Based on contributed data on the impact of server operations, allocated to the impression from a regional level. **WIP**|
|3|Allocation from location data|Based on contributed data on the impact of server operations, allocated to the impression from a location level. **WIP**|

#### Data

| Variable | Type | Value | Unit | Description | Notes |
| --- | --- | --- | --- | --- | --- |
| Impressions | Input | | | Number of impressions. | Disaggregated by location, buy type.|
| Location | Input | | | Country of users consuming ads. | |
| Ads.txt Lines | Input | | | Number of ads.txt lines per publisher. | If a publisher has not adopted the authorised digital sellers specification and still sells ad space programmatically, a conservative default of 3000 lines may be used. |
| Local Grid Intensity | Input | | kg CO2e per kWh | Emissions intensity of electricity grid in the country where the user consuming the ad is located. | 50% of servers are assumed to be located in the same country as the consuming user. |
| Foreign Grid Intensity | Input | | kg CO2e per kWh | Emissions intensity of electricity grid in the region where the user consuming the ad is located, weighted by data center locations. | 50% of servers are assumed to be distributed across the consuming user's region. See table further below. |
| Server Use-phase Intensity | Constant | $3.41 \times 10^{-7}$ | kWh per processed ad opportunity | Energy intensity of a server processing an opportunity. | Based on server model. |
| Server Embodied Intensity | Constant | $1.50 \times 10^{-8}$ | kg CO2e per processed ad opportunity | Embodied emissions of a server processing an opportunity. | Based on server model. |
| RTB Payload | Constant | 3 | KB | Average weight of a real-time bidding payload. | |
| Fixed Network Use-phase Intensity | Constant | $1.65 \times 10^{-8}$ | kWh per KB | Energy intensity of transfering 1 KB over fixed networks. | Based on network model. |
| Fixed Network Embodied Intensity | Constant | $2.14 \times 10^{-9}$ | kg CO2e per KB | Embodied emissions of transfering 1 KB over fixed networks. | Based on network model. |
| Display Server Factor | Constant | 1.412 | | Number of servers assumed to be activated per ads.txt line for a display ad. | Based on proprietary data from IAB Europe members. |
| Display Call Factor | Constant | 1.464 | | Number of requests/bids assumed to be transferred per ads.txt line for a display ad. | Based on proprietary data from IAB Europe members. |
| Video Server Factor | Constant | 1.316 | | Number of servers assumed to be activated per ads.txt line for a video ad. | Based on proprietary data from IAB Europe members. |
| Video Call Factor | Constant | 1.334 | | Number of requests/bids assumed to be transferred per ads.txt line for a video ad. | Based on proprietary data from IAB Europe members. |

The following adjustments are made depending on buy type:
- **Direct:**
    - Number of activated servers = 2
    - Number of calls = 4
- **End-to-end platforms:**
    - Number of activated servers = 500
    - Number of calls = 0

#### Equations
$$
\begin{align*}
    \text{Server Use-phase Emissions} &= \text{Number of Activated Servers} \times \text{Server Use-phase Intensity} \\
    &\quad \times \left(0.5 \times \text{Local Grid Intensity} + 0.5 \times \text{Foreign Grid Intensity}\right) \times \text{Impressions}
\end{align*}
$$

$$
\begin{align*}
    \text{Server Embodied Emissions} &= \text{Number of Activated Servers} \times \text{Server Embodied Intensity} \times \text{Impressions}
\end{align*}
$$

$$
\begin{align*}
    \text{Network Use-phase Emissions} &= \text{Number of Calls} \times \text{Network Use-phase Intensity} \times \text{RTB Payload} \\
    &\quad \times \left(0.5 \times \text{Local Grid Intensity} + 0.5 \times \text{Foreign Grid Intensity}\right) \times \text{Impressions}
\end{align*}
$$

$$
\begin{align*}
    \text{Network Embodied Emissions} &= \text{Number of Calls} \times \text{RTB Payload} \times \text{Network Embodied Intensity} \times \text{Impressions}
\end{align*}
$$

#### Example
In this example, the methodology is used to estimate the impact of Selection for programmatically-traded display ads on a German publisher using the level 1 Activity methodology. The local grid intensity for Germany is sourced from the reference database and the foreign grid intensity for Europe is sourced from the model at the end of this document. The level 1 Activity methodology uses ads.txt lines as a proxy.

##### Example Input Data
| Variable | Value | Unit |
|---|---|---|
| Impressions | 100,000 | |
| Ads.txt Lines | 150 | |
| Local Grid Intensity | $3.44 \times 10^{-1}$ | kg CO2e per kWh |
| Foreign Grid Intensity | $2.50 \times 10^{-1}$ | kg CO2e per kWh |


##### Applying the Equations
$$
\begin{align*}
    {Number\ of\ Activated\ Servers} &= \text{Ads.txt\ Lines} \times \text{Display\ Server\ Factor} \\
    &= 150 \times 1.412  \\
    &= 211.8
\end{align*}
$$

$$
\begin{align*}
    {Number\ of\ Calls} &= \text{Ads.txt\ Lines} \times \text{Display\ Call\ Factor} \\
    &= 150 \times 1.464  \\
    &= 219.6
\end{align*}
$$

$$
\begin{align*}
	{Server\ Use-phase\ Emissions} &= \text{Number\ of\ Activated Servers} \times \text{Server\ Use-phase\ Intensity} \\
&\qquad \times \Big(0.5 \times \text{Local Grid Intensity} + 0.5 \times \text{Foreign Grid Intensity}\Big) \times \text{Impressions} \\
&= 211.8 \times 3.41 \times 10^{-7} \times (0.5 \times 0.344 + 0.5 \times 0.25) \times 100{,}000 \\
&= 211.8 \times 3.41 \times 10^{-7} \times 0.297 \times 100{,}000 \\
&= 211.8 \times 3.41 \times 10^{-7} \times 29{,}700 \\
&= 211.8 \times 0.0101277 \\
&= 2.144 \text{ kg CO2e}
\end{align*}
$$

$$
\begin{align*}
    {Server\ Embodied\ Emissions} &= \text{Number\ of\ Activated\ Servers} \times \text{Server\ Embodied\ Intensity} \times \text{Impressions} \\
    &= 211.8 \times 1.50 \times 10^{-8} \times 100{,}000 \\
    &= 211.8 \times 0.0015 \\
    &= 0.318 \text{ kg CO2e}
\end{align*}
$$

$$
\begin{align*}
    {Network\ Use-phase\ Emissions} &= \text{Number\ of\ Calls} \times \text{Network\ Use-phase\ Intensity} \times \text{RTB\ Payload} \\
    &\qquad \times \Big(0.5 \times \text{Local\ Grid\ Intensity} + 0.5 \times \text{Foreign\ Grid\ Intensity}\Big) \times \text{Impressions} \\
    &= 219.6 \times 1.65 \times 10^{-8} \times 3 \times (0.5 \times 0.344 + 0.5 \times 0.25) \times 100{,}000 \\
    &= 219.6 \times 1.65 \times 10^{-8} \times 3 \times 0.297 \times 100{,}000 \\
    &= 219.6 \times 1.65 \times 10^{-8} \times 3 \times 29{,}700 \\
    &= 219.6 \times 1.65 \times 10^{-8} \times 89{,}100 \\
    &= 219.6 \times 0.00147015 \\
    &= 0.323 \text{ kg CO2e}
\end{align*}
$$

$$
\begin{align*}
    {Network\ Embodied\ Emissions} &= \text{Number\ of\ Calls} \times \text{RTB\ Payload} \times \text{Network\ Embodied\ Intensity} \times \text{Impressions} \\
    &= 219.6 \times 3 \times 2.14 \times 10^{-9} \times 100{,}000 \\
    &= 219.6 \times 3 \times 0.000214 \\
    &= 219.6 \times 0.000642 \\
    &= 0.141 \text{ kg CO2e}
\end{align*}
$$

### Delivery

The delivery section accounts for the emissions resulting from transferring ads to user devices over fixed or mobile connections. The main input is the payload of this transfer. The methodology accounts for content delivery networks and includes an overhead for the origin server. An overhead is applied in certain data levels to account for additional payloads (wrappers, players etc.) - the Methodology & Framework Working Group acknowledges that these are often cached on the user device, but no data was found to integrate a representative assumption.


#### Levels

The table below contains information on how to calculate payload depending on the data that is available. The overhead should be added to the payload if using any of the level 0-2 methods. The payload refers to the weight of the assets that are delivered to the user, not those upload to the ad server - transcoding may be applied. If granular data is available showing distribution across different transcodes, delivery should be calculated per transcode. Defaults based on aggregate data submitted by Methodology & Framework Working Group members. 

| Level | Method | Display Default | Video Default | Notes |
|-------|--------|-----------------|---------------|-------|
| 0 | Default payload | 0.25 MB | 4 MB, 6 MB instream | Instream figure should be used in environments where heavy ad intervention does not apply. |
| 1 | 100% of creative data assumed to be transferred | | | |
| 2 | Completion rate proxy | | | E.g. 50% video completion rate &rarr; 50% of video data transferred. If view time data is available in quartiles rather than as a mean, calculations should overestimate by using the higher bound of the quartile (e.g. if a given portion of users is reported to have watched between 0 and 3 seconds of a video ad, they are assumed to have watched 3 seconds).|
| 3 | Data transfer measurement | | | Option when tooling that enables logging precise payloads is available. Eliminates need for overhead term. |
| Overhead | Default non-creative asset payload | 0.05 MB | 0.35 MB | Add-on to levels 0-2. Overestimation due to caching. |

#### Data

| Variable | Type | Value | Unit | Description | Notes |
| --- | --- | --- | --- | --- | --- |
| Impressions | Input | | | Number of impressions. | Disaggregated by location. |
| Payload | Input | | MB | Total size of files transferred. | See levels above.|
| Mobile Connection Ratio | Input | | | Ratio of ads served over a mobile connection (e.g. 4G, 5G). | |
| Fixed Connection Ratio | Input | | | Ratio of ads served over a fixed connection (e.g. WiFi). | |
| Local Grid Intensity | Input | | kg CO2e per kWh | Emissions intensity of electricity grid in the country where the user consuming the ad is located. | |
| Fixed Network Use-phase Energy Intensity | Constant | $1.65 \times 10^{-5}$ | kWh per MB | Energy consumption associated with transfering 1 MB over fixed networks. | |
| Fixed Network Embodied Intensity | Constant | $2.14 \times 10^{-6}$ | kg CO2e per MB | Embodied emissions associated with transfering 1 MB over fixed networks. | |
| Mobile Network Use-phase Energy Intensity | Constant | $1.17 \times 10^{-4}$ | kWh per MB | Energy consumption associated with transfering 1 MB over fixed networks. | |
| Mobile Network Embodied Intensity | Constant | $8.70 \times 10^{-6}$ | kg CO2e per MB | Embodied emissions associated with transfering 1 MB over fixed networks. | |
| Edge Node Use-phase Energy Intensity | Constant | $4.30 \times 10^{-7}$ | kWh per MB | Energy consumption associated with transferring 1MB from an edge node. | |
| Edge Node Embodied Intensity | Constant | $5.88 \times 10^{-7}$ | kg CO2e per MB | Embodied emissions associated with transferring 1MB from an edge node. | |

##### Fixed / Mobile Defaults

These can be used in cases where no data is available on connection type and are based on aggregate data submitted by Teads.

| Region | Countres Included in Sample | Fixed Ratio | Mobile Ratio |
|--------|-----------------------------|-------------|--------------|
| Europe | Austria, Belgium, Bulgaria, Croatia, Republic of Cyprus, Czech Republic, Denmark, Estonia, Finland, France, Germany, Greece, Hungary, Ireland, Italy, Latvia, Lithuania, Luxembourg, Malta, Netherlands, Poland, Portugal, Romania, Slovakia, Slovenia, Spain, Sweden, United Kingdom,Switzerland, Iceland, Liechtenstein, Norway | 0.7431 | 0.2569 |
| APAC | Australia, Bangladesh, Brunei, Cambodia, China, Cook Islands, Fiji, India, Indonesia, Japan, Kiribati, Laos, Malaysia, Maldives, Marshall Islands, Micronesia, Mongolia, Myanmar, Nepal, New Caledonia, New Zealand, Niue, North Korea, Pakistan, Palau, Papua New Guinea, Philippines, Singapore, Solomon Islands, South Korea, Sri Lanka, Thailand, Timor Leste, Tonga, Tuvalu, Vietnam | 0.6768 | 0.3232 |
| NA | United States of America, Canada | 0.8608 | 0.1392 |
| LATAM | Mexico, Guatemala, Honduras, Nicaragua, El Salvador, Costa Rica, Panama, Belize, Haiti, Cuba, Dominican Republic, Jamaica, Trinidad & Tobago, Bahamas, Barbados, St. Lucia, Grenada, St. Vincent & Grenadines, Antigua & Barbuda, Dominica, St. Kitts & Nevis, Brazil, Colombia, Argentina, Peru, Venezuela, Chine, Ecuador, Bolivia, Paraguay, Uruguay, Suriname, Guyana | 0.7145 | 0.2855 |

#### Equation

$$
	{Delivery\ Use-phase\ Emissions} = \text{Payload} \times \text{Local\ Grid\ Intensity} \\
\qquad \times \Big(\text{Mobile\ Connection\ Ratio} \times \text{Mobile\ Network\ Use-phase\ Energy\ Intensity} + \text{Fixed\ Connection\ Ratio} \times \text{Fixed\ Network\ Use-phase\ Energy\ Intensity} + \text{Edge\ Node\ Use-phase\ Energy\ Intensity}\Big) \\ \times \text{Impressions}
$$

$$
	{Delivery\ Embodied\ Emissions} = \text{Payload} \\
\qquad \times \Big(\text{Mobile\ Connection\ Ratio} \times \text{Mobile\ Network\ Embodied\ Intensity} + \text{Fixed\ Connection\ Ratio} \times \text{Fixed\ Network\ Embodied\ Intensity} + \text{Edge\ Node\ Embodied\ Intensity}\Big) \\ \times \text{Impressions}
$$

#### Example

This example shows how to estimate the emissions of delivering a video ad to users in Italy. It uses the level 1 Payload methodology, meaning the entire file is assumed to be transferred every time the ad is displayed and an overhead assumption is applied. The payload is thus increased by 0.35 MB. It also uses default figures for users on mobile and fixed connections in Europe.

##### Example Input Data
| Variable | Value | Unit |
|---|---|---|
| Impressions | 100,000 | |
| Payload | 2.85 | MB |
| Local Grid Intensity | $2.87 \times 10^{-1}$ | kg CO2e per kWh |
| Mobile Connection Ratio | 0.2569 ||
| Fixed Connection Ratio | 0.7431 ||

##### Applying the Equations

$$
\begin{align*}
    {Delivery\ Use-phase\ Emissions} &= \text{Payload} \times \text{Local\ Grid\ Intensity} \\
    &\qquad \times \Big(\text{Mobile\ Connection\ Ratio} \times \text{Mobile\ Network\ Use-phase\ Energy\ Intensity} \\
    &\qquad\qquad + \text{Fixed\ Connection\ Ratio} \times \text{Fixed\ Network\ Use-phase\ Energy\ Intensity} \\
    &\qquad\qquad + \text{Edge\ Node\ Use-phase\ Energy\ Intensity}\Big) \\
    &\qquad \times \text{Impressions} \\
    &= 2.85 \times 2.87 \times 10^{-1} \\
    &\qquad \times \Big(0.2569 \times 1.17 \times 10^{-4} + 0.7431 \times 1.65 \times 10^{-5} + 4.30 \times 10^{-7}\Big) \\
    &\qquad \times 100{,}000 \\
    &= 2.85 \times 0.287 \\
    &\qquad \times \Big(0.2569 \times 0.000117 + 0.7431 \times 0.0000165 + 0.00000043\Big) \\
    &\qquad \times 100{,}000 \\
    &= 0.81795 \\
    &\qquad \times \Big(0.00003006 + 0.00001226 + 0.00000043\Big) \\
    &\qquad \times 100{,}000 \\
    &= 0.81795 \times 0.00004275 \times 100{,}000 \\
    &= 0.00003497 \times 100{,}000 \\
    &= 3.497 \text{ kg CO2e}
\end{align*}
$$

$$
\begin{align*}
    {Delivery\ Embodied\ Emissions} &= \text{Payload} \\
    &\qquad \times \Big(\text{Mobile\ Connection\ Ratio} \times \text{Mobile\ Network\ Embodied\ Intensity} \\
    &\qquad\qquad + \text{Fixed\ Connection\ Ratio} \times \text{Fixed\ Network\ Embodied\ Intensity} \\
    &\qquad\qquad + \text{Edge\ Node\ Embodied\ Intensity}\Big) \\
    &\qquad \times \text{Impressions} \\
    &= 2.85 \\
    &\qquad \times \Big(0.2569 \times 8.70 \times 10^{-6} + 0.7431 \times 2.14 \times 10^{-6} + 5.88 \times 10^{-7}\Big) \\
    &\qquad \times 100{,}000 \\
    &= 2.85 \\
    &\qquad \times \Big(2.236 \times 10^{-6} + 1.591 \times 10^{-6} + 5.88 \times 10^{-7}\Big) \\
    &\qquad \times 100{,}000 \\
    &= 2.85 \\
    &\qquad \times 4.415 \times 10^{-6} \\
    &\qquad \times 100{,}000 \\
    &= 1.257 \times 10^{-5} \times 100{,}000 \\
    &= 1.257 \text{ kg CO2e}
\end{align*}
$$

### Consumption

The consumption stage accounts for emissions arising from resource usage on user devices, including both use-phase and embodied emissions across 4 device groups: mobile, tablet, PC, and TV. In the absence of more specific data relating digital ads to hardware usage, two assumptions are included in the proposed methodology. First, the entire device’s lifecycle emissions are allocated to each digital ad. Second, view time is used as a proxy to facilitate the time-based allocation.

#### Levels

The table below contains information on how to calculate view time depending on the data that is available. The minimum view time should be used if ads that fail to meet viewability specifications are not included in averages used for the level 1 / 2 methods - this ensures that some device usage is still accounted for. If view time data is available in quartiles rather than as a mean, calculations should overestimate by using the higher bound of the quartile (e.g. if a given portion of users is reported to have watched between 0 and 3 seconds of a video ad, they are assumed to have watched 3 seconds).

| Level | Method | Display Default | Video Default | Notes |
|-------|--------|-----------------|---------------|-------|
| 0 | Default view time | 3 seconds | 30 seconds | For cases where no view time data is available. |
| 1 | Average campaign-level view time | | | Flat campaign average view time used for every device type. |
| 2 | Average device-type view time | | | Average view time available per device type. |
| Overhead | Minimum view time | 1 second | 2 seconds | Based on MRC standard for viewability - used to account for device usage in cases where view time is not reported for ads that fail to meet viewability specifications. |

#### Data

| Variable | Type | Value | Unit | Description | Notes |
| --- | --- | --- | --- | --- | --- |
| Impressions | Input | | | Number of impressions. | Disaggregated by location. |
| Average View Time | Input | | seconds | Average view time of ads. | See levels above.|
| Device Type | Input | | | Type of device on which ads are delivered. | |
| Local Grid Intensity | Input | | kg CO2e per kWh | Emissions intensity of electricity grid in the country where the user consuming the ad is located. | |
| Mobile Use-phase Energy Intensity | $1.30 \times 10^{-6}$ | kWh per second | Energy consumption associated with viewing an ad on a mobile for 1 second. | |
| Mobile Embodied Intensity | $6.55 \times 10^{-6}$ | kg CO2e per second | Embodied emissions associated with viewing an ad on a mobile for 1 second. | |
| Tablet Use-phase Energy Intensity | $1.40 \times 10^{-6}$ | kWh per second | Energy consumption associated with viewing an ad on a tablet for 1 second. | |
| Tablet Embodied Intensity | $2.57 \times 10^{-5}$ | kg CO2e per second | Embodied emissions associated with viewing an ad on a tablet for 1 second. | |
| TV Use-phase Energy Intensity | $3.80 \times 10^{-5}$ | kWh per second | Energy consumption associated with viewing an ad on a PC for 1 second. | |
| TV Embodied Intensity | $8.65 \times 10^{-6}$ | kg CO2e per second | Embodied emissions associated with viewing an ad on a PC for 1 second. | |
| PC Use-phase Energy Intensity | $1.54 \times 10^{-5}$ | kWh per second | Energy consumption associated with viewing an ad on a TV for 1 second. | |
| PC Embodied Intensity | $5.45 \times 10^{-6}$ | kg CO2e per second | Embodied emissions associated with viewing an ad on a TV for 1 second. | |

##### Device Distribution Defaults

These can be used in cases where no data is available on device type and are based on aggregate data submitted by Impact Plus.

| Device Type | Ratio |
|-------------|-------|
| Mobile | 0.61 |
| Tablet | 0.04 |
| PC | 0.18 |
| TV | 0.17 |

#### Equation
For each device type:

$$
{Device\ Use-phase\ Emissions} = \text{View\ Time} \times \text{Device\ Use-phase\ Energy\ Intensity} \times \text{Local\ Grid\ Intensity} \times \text{Impressions}
$$

$$
{Device\ Embodied\ Emissions} = \text{View\ Time} \times \text{Device\ Embodied\ Intensity} \times \text{Impressions}
$$

#### Example

This example estimates the emissions of loading ads on mobiles in Austria. It uses the level 2 methodology, assuming the average view time on mobiles is known to be 3 seconds. It also assumes that ads that failed to meet the MRC viewability standard of 1 second are included in this average, so no adjustment to the average is required.

##### Example Input Data
| Variable | Value | Unit |
|---|---|---|
| Impressions | 100,000 | |
| View Time | 3 | seconds |
| Local Grid Intensity | $1.02 \times 10^{-1}$ | kg CO2e per kWh |

##### Applying the Equations

$$
\begin{align*}
{Device\ Use-phase\ Emissions} 
    &= \text{View\ Time} \times \text{Device\ Use-phase\ Energy\ Intensity} \times \text{Local\ Grid\ Intensity} \times \text{Impressions} \\
    &= \text{View\ Time} \times \text{Mobile\ Use-phase\ Energy\ Intensity} \times \text{Local\ Grid\ Intensity} \times \text{Impressions} \\
    &= 3 \times 1.30 \times 10^{-6} \times 1.02 \times 10^{-1} \times 100{,}000 \\
    &= 3 \times 1.30 \times 10^{-6} \times 0.102 \times 100{,}000 \\
    &= 3 \times 1.326 \times 10^{-7} \times 100{,}000 \\
    &= 3.978 \times 10^{-7} \times 100{,}000 \\
    &= 0.03978 \text{ kg CO2e}
\end{align*}
$$

$$
\begin{align*}
{Device\ Embodied\ Emissions} 
    &= \text{View\ Time} \times \text{Device\ Embodied\ Intensity} \times \text{Impressions} \\
    &= \text{View\ Time} \times \text{Mobile\ Embodied\ Intensity} \times \text{Impressions} \\
    &= 3 \times 6.55 \times 10^{-6} \times 100{,}000 \\
    &= 3 \times 0.00000655 \times 100{,}000 \\
    &= 0.00001965 \times 100{,}000 \\
    &= 1.965 \text{ kg CO2e}
\end{align*}
$$

## Models

### Server Model

For the level 0 default on server processing intensity, the Methodology & Framework Working Group evaluated an approach based on time allocation from LCA data and assumptions around virtualization, drawing from the Digital Carbon Footprint framework. The sub-group elected to use figures from a life cycle assessment of virtual machines for homogeneity (avoiding an additional assumption on VMs per physical machine) and opted for the VM configuration deemed most representative given the set of assumptions and based on feedback from industry experts.

The figures below produce the server figures in the data table for Selection.

| Variable | Type | Value | Unit | Description | Notes |
| --- | --- | --- | --- | --- | --- |
| VM Electricity Consumption | Constant | 55.2 | kWh per year | Annual electricity consumption of a VM. | 1 vCPU, 4 GB memory, 5-year lifespan. |
| VM Embodied Emissions | Constant | 3.79 | kg CO2e per year | Annual embodied emissions of a VM. | 1 vCPU, 4 GB memory, 5-year lifespan. |
| Processing Time | Constant | 100 | ms | Assumed length of time it takes for a server to process an ad opportunity. | Used to allocate server emissions to processing of an ad opportunity. |
| Overhead Factor | Constant | 1.25 |  | Assumed server overhead for other processing tasks. | |
| Average PUE | Constant | 1.56 |  | Average server power usage effectiveness. | |

### Network Model

The network emissions model is based on ADEME's work on the carbon footprint of networks,  specifically their linear model with a constant term (referred to as the a * x + b model). Using average broadband speed figures, the model is converted to a simple linear equation with payload as an input for ease of use.

| Network | Term | Variable | Value | Unit |
|---------|------|----------|-------|------|
| Fixed | a | Use-phase Energy Intensity | $5.00 \times 10^{-6}$ | kWh per MB |
| Fixed | a | Embodied Intensity | $9.05 \times 10^{-7}$ | kg CO2e per MB |
| Fixed | b | Use-phase Energy Intensity | $9.18 \times 10^{-6}$ | kWh per (user) second |
| Fixed | b | Embodied Intensity | $9.90 \times 10^{-7}$ | kg CO2e per (user) second |
| Mobile | a | Use-phase Energy Intensity | $1.03 \times 10^{-4}$ | kWh per MB |
| Mobile | a | Embodied Intensity | $6.20 \times 10^{-6}$ | kg CO2e per MB |
| Mobile | b | Use-phase Energy Intensity | $8.50 \times 10^{-6}$ | kWh per (user) second |
| Mobile | b | Embodied Intensity | $1.52 \times 10^{-6}$ | kg CO2e per (user) second |
| Fixed | | Average Broadband Speed | 0.80 | MB per second |
| Mobile | | Average Broadband Speed | 0.61 | MB per second |

<br> The values above result in the following factors in MB terms:

| Network | Variable | Value | Unit |
|---------|----------|-------|------|
| Fixed | Use-phase Energy Intensity | $1.65 \times 10^{-5}$ | kWh per MB |
| Fixed | Embodied Intensity | $2.14 \times 10^{-6}$ | kg CO2e per MB |
| Mobile | Use-phase Energy Intensity | $1.17 \times 10^{-4}$ | kWh per MB |
| Mobile | Embodied Intensity | $8.70 \times 10^{-6}$ | kg CO2e per MB |


### Foreign Grid Intensities

The following grid intensities are based on country intensities in each region weighted by data center locations. They are based on [publicly-accessible information](https://www.datacentermap.com/datacenters/) and the [Ember](https://ember-energy.org/data/yearly-electricity-data/) database. A global figure is provided in case location is unknown - if that is the case, this figure should be used alone instead of averaging between a local and a foreign grid intensity. These intensities have been calculated by Good-Loop.

| Continent | kg CO2e per kWh|
|---|---|
| Africa | $4.72 \times 10^{-1}$ |
| Asia | $5.93 \times 10^{-1}$ |
| Europe | $2.50 \times 10^{-1}$ |
| North America | $3.78 \times 10^{-1}$ |
| South America | $1.91 \times 10^{-1}$ |
| Oceania | $4.78 \times 10^{-1}$ |
| Global | $3.76 \times 10^{-1}$ |