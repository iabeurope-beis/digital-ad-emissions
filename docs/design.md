# Design

Overall, the methodology is designed to be as robust and representative as possible while still being practical and implementable by actors in the digital advertising ecosystem.

## Purpose

The methodology's purpose is estimating the carbon footprint of a digital advertising campaign, and therefore it is primary developed from an advertiser perspective.

However, organisations from other market segments can still use the methodology:
- **Publishers** can estimate how much carbon buying ad space on their properties would add to a campaign.
- **Intermediaries** can estimate how much carbon transacting through them would add to a campaign.

## Boundaries

The methodology covers the following stages of a digital advertising campaign:

- **Storage** refers to the storage of all assets produced for a campaign at a post-production level.
- **Selection** refers to the buying and selling of digital ad space, including real-time bidding.
- **Delivery** refers to transfering the ad creative to the consuming user.
- **Consumption** refers to the activity on the consuming user's device that is required to display the ad.

### Corporate Overhead

While supportive of including an allocation of enterprise-level emissions in the GMSF, the Methodology & Framework Working Group believes that additional guidance is required to ensure consistency between the figures shared by each company and how they are used. As such, it opted to set aside Corporate Overhead as an area for future work.

The objective of the Working Group is to avoid large inconsistencies in emissions estimates, such as those that arise without clear guidance on how to integrate figures from enterprise-level emissions reports.

## Principles

Below are key design principles that underpin development of the methodology.

### Quality

The methodology should be as robust as possible, drawing from the latest industry work and research. Given the complexity of the activities involved in a digital advertising campaign, the methodology will inevitably feature proxies and approximations. These should be vetted to ensure they are as representative of actual digital ad operations and their emissions as possible.

### Flexibility

The methodology should be implementable by all digital advertising stakeholders at some level of data granularity. Granularity refers to how specifically the data describes the activities involved in a digital advertising campaign.

To operationalise this principle, the methodology features **levels**. Levels provide options for how to apply each part of the methodology, allowing for the methodology to be applied with a variety of data inputs. Level 0, commonly referred to as the default level, allows the application of a part of the methodology with no input data at all.

The methodology is modular and applications are likely to feature multiple levels for each part, especially for large campaigns where data is combined from multiple sources.

### Conservativeness

The methodology should incentivise the use of more granular data as it leads to better emissions estimates. Using more granular data should yield reduced emissions figures compared to coarser data. For example, the default level calculations tend to overestimate emissions by design.

## Guidance

### Uncertainty

Figures expressing the environmental impact of digital ad products rely heavily on assumptions, proxies, and third-party data. As a result, the Sustainability Standards Committee recommends that these are referred to as **estimates** and that the tools that yield such figures are referred to as **models** rather than calculators. Claims regarding the environmental impact of digital advertising products or campaigns should be communicated as such, making it clear that the quantified impact is characterized by uncertainty. Calculating the statistical uncertainty of the estimates produced by this methodology is impossible due to the lack of relevant data in the sources the WG relied upon.

Transparency is key when it comes to improving the quantification of digital ad emissions. As more ecosystem stakeholders make data about their operations available, the accuracy and precision of environmental KPIs will improve.

### Reductions on Paper

Emissions estimates calculated using the methodology can vary substantially depending on the input data. One of the guiding principles in designing the methodology was that implementation of best practices and campaign optimization for lower environmental footprint should be representable in the results. In other words, when a stakeholder takes an action that likely reduces their carbon footprint, the proposed methodology should yield lower emissions estimates as well. However, organizations applying the methodology should be mindful of the fact that, in some cases, changing the inputs may yield lower results when in reality it is doubtful that greenhouse gas emissions have been reduced

As an example, let’s assume the proposed methodology is being used to estimate the impact of targeting mobiles instead of tablets on a campaign’s estimated emissions. While the methodology yields lower use-phase emissions for ads served to tablets as a result of the emission factor that is included, there is no direct causal link between opting to advertise to users on tablets and whether users will opt to use their tablets or mobiles. These events are largely independent of each other and the real amount of electricity consumed by the devices that users decide to consume media on will be unchanged.

## Future Work

The Methodology & Framework Working Group identifies the following areas for future work to improve the robustness of the methodology:

- Server emissions based on contributed data.
- Programmatic activity estimation based on contributed data.
- Guidance on inclusion of enterprise-level emissions.
- Modeling of DMP & other intermediary activity.
- Precise modeling of device usage due to ads.
- Precise modeling of network transfer between servers.