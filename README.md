# Calculations supplement to *The Feasibility of a Hardwired Pause of Frontier AI Training*

This repository holds the calculations behind the quantitative claims in Table 3.1, §4.4 and Sections 5–6 of the working paper *The Feasibility of a Hardwired Pause of Frontier AI Training* (Working Group on AI Pause Feasibility, October 2026). Where the paper says "see calculations supplement", this is what it refers to.

## Contents

| | |
|---|---|
| `feasibility_calculations.Rmd` | The notebook: one section per derivation, giving the inputs, the code, the judgment calls and the references. |
| `feasibility_calculations.html` | The same notebook, knitted. Open it in a browser to read the results without running anything. |
| `data/` | Every dataset the notebook reads, as it stood when it was retrieved. The notebook reads no URL. |
| `LICENSE` | The licence for the notebook and the two lists of named owners (MIT). |

## Running it

The notebook needs R with the packages `rmarkdown`, `knitr`, `ggplot2` and `patchwork`. It was last knitted with R 4.5.3 (rmarkdown 2.29, ggplot2 4.0.0). From this folder:

```
Rscript -e 'rmarkdown::render("feasibility_calculations.Rmd")'
```

It takes well under a minute and reads nothing from the network. `rmarkdown` needs pandoc, which RStudio includes. The knitted HTML loads MathJax from the web to typeset its formulas.

## Data

### Epoch AI

Most of the files are snapshots of datasets published by [Epoch AI](https://epoch.ai/data). Epoch states that its data is "free to use, distribute, and reproduce provided the source and authors are credited" under the [Creative Commons Attribution 4.0 licence](https://creativecommons.org/licenses/by/4.0/). The files are unchanged from Epoch's downloads except where the table says otherwise. They are frozen at the dates shown; Epoch updates most of these datasets, and the current versions are at the pages listed.

| File | Dataset | Source page | Epoch's version | Retrieved |
|---|---|---|---|---|
| `ai_chip_owners_cumulative_by_designer.csv`, `ai_chip_owners_cumulative_by_chip_type.csv`, `ai_chip_owners_quarters_by_chip_type.csv` | Epoch AI, "Data on AI Chip Owners" | https://epoch.ai/data/ai-chip-owners | May 12, 2026 | September 15, 2026 |
| `ai_chip_sales_timelines_by_chip.csv` | Epoch AI, "Data on AI Chip Sales" | https://epoch.ai/data/ai-chip-sales | August 27, 2026 | September 15, 2026 |
| `ai_chip_users_year_end_by_lab.csv` | Epoch AI, "Data on AI Chip Users" | https://epoch.ai/data/ai-chip-users | September 9, 2026 | September 16, 2026 |
| `data_centers.csv`, `data_center_timelines.csv` | Epoch AI, "Data on AI Data Centers" | https://epoch.ai/data/ai-data-centers | September 11, 2026 | September 15 and 16, 2026 |
| `gpu_clusters.csv` | Epoch AI's GPU clusters dataset (since superseded by its data-centers dataset) | https://epoch.ai/data/gpu-clusters | March 13, 2026 | September 15, 2026 |
| `frontier_ai_models.csv`, `all_ai_models.csv` | Epoch AI, "Data on AI Models" | https://epoch.ai/data/ai-models | updated continuously | about September 5 and September 18, 2026 |
| `nvidia_flops_by_generation.csv` | Luke Emberson and David Owen, "The Stock of Computing Power from NVIDIA Chips Is Doubling Every 10 Months" (Epoch AI, 2025); the chart's data file | https://epoch.ai/data-insights/nvidia-chip-production | November 21, 2025 | October 2, 2026 |
| `customs_server_lanes.csv` | Isabel Juniewicz, "Trade Data Is Consistent with More Than $3 Billion of Chips Smuggled into China via Malaysia" (Epoch AI, 2026); thirteen monthly series taken, unaltered, from the customs panel published with it. **An extract, not the whole file.** | https://epoch.ai/data-insights/malaysia-china-chip-smuggling | panel dated August 19, 2026 | October 2, 2026 |

To credit one of these datasets, Epoch suggests the form: Epoch AI, 'Data on AI chip sales'. Published online at epoch.ai. Retrieved from 'https://epoch.ai/data/ai-chip-sales' [online resource]. Accessed (date).

The compute unit in Epoch's files is the H100-equivalent (H100e) as Epoch defines it; §S.0 of the notebook says how it relates to the paper's.

### Hong Kong trade statistics

`hk_server_imports_by_consignment.csv` gives Hong Kong's monthly imports of servers (code 8471 5000) by place of consignment, for Taiwan, Malaysia, Indonesia and Thailand, from January 2023 to August 2026, in value and units. It was compiled for this notebook from the Census and Statistics Department's [Interactive Data Dissemination Service for Trade Statistics](https://tradeidds.censtatd.gov.hk/) on October 3, 2026. Values were converted from Hong Kong dollars to U.S. dollars at each month's average rate (European Central Bank reference rates).

### The two lists of named owners

`large_owners_us.csv` and `large_owners_cn.csv` list accelerator holdings that can be tied to a named owner from public sources: for the United States, holdings outside the seven largest firms; for China, holdings in China (a few rows do not state where). They were compiled for this notebook from company filings and releases, government budgets, press reports and Epoch's datasets, and **were prepared by an AI model, Claude Opus 5.5**. They are used in §S.6.4 and §S.6.5 of the notebook, and footnote 62 of the paper refers to them.

Each row is one holding, in H100e. A chip that Epoch rates is converted at Epoch's rating; a chip type Epoch does not rate is converted on the same basis from its specifications; Tesla's row uses Tesla's own H100-equivalent figures. The lists contain only the holdings the census counts.

| Column | Meaning |
|---|---|
| `owner`, `site`, `state` or `province` | Who holds the chips and where. `?`, `multi`, `multiple`, `unstated` and `unclear` stand for a location the source does not give or a holding spread over several. |
| `category` | The kind of owner (for example `neocloud`, `government`, `academic`, `telecom_soe`). `clusters_data` marks a U.S. row taken as it stands from Epoch's GPU-clusters dataset. |
| `basis` | What the source says and how the row's number follows from it. The notebook matches a few chip names in this column of the China list to decide which part of the stock a row is set against, so its wording is part of the calculation. |
| `e25` | Holdings dated to the end of 2025. A holding with no date is left out of this column. |
| `firm` | Holdings reported operational or delivered by September 2026, as of each row's source; not re-confirmed since. |
| `sched` | `firm` plus deliveries that the owner or the site says are due within 2026. |
| `ownership` | `owns`, or a note where the source leaves ownership unclear. |
| `in_tally` | `y` in every published row: the census counts it. Rows it does not count (overlaps with other rows, orders, plans, holdings that could not be quantified) are not published. |
| `source` | Links to the pages the row rests on. An Epoch dataset page stands for a row taken from that dataset. |
| `t0`, `ms` | U.S. list: the date from which the end-2025 holding counts, where the row gives one, and dated milestones as `year:H100e` pairs (decimal years at quarter ends). China list: `ms` only. |
| `family` (China) | `epoch` in every published row: chips of the types in Epoch's count of China's stock. |
| `evidence` (China) | `A`: the owner's own filing, paper or official statement. `B`: a press report naming its source, or several reports. `C`: an analyst's or an anonymous estimate. `E`: an estimate from Epoch's data-centers dataset. |
| `units` (China) | Empty in the published rows. |

Conversions that appear in `basis`: a chip count is multiplied by Epoch's H100e per chip of that type (for example 2.527 for a Blackwell GPU, 0.315 for an A100); `x 1,150` is H100e per megawatt of facility power at a GPU site, a round figure near the median of sites in Epoch's data-centers dataset; `x PF16` is 1 H100e per PFLOPS where the source states FP16; `x PUNS` is 0.5 H100e per PFLOPS where the source does not state the precision; `x UNK` is 0.315 H100e per card of unstated type. The megawatt rate and the last two are judgments.

**How the lists were checked.** On October 9, 2026, AI agents opened every link in the two lists and compared each page with the row that cites it. The authors then reviewed the rows that matter most to the results. Rows that the check could not confirm and that the authors did not review themselves were removed and are not counted. The lists remain a compilation of public claims, many of them the owners' own, and a row here is not a finding by the authors that the holding exists as stated.

## Use of AI

Claude Fable 5.1 and Claude Opus 5.5 were used to prepare the notebook. The two lists of named owners were prepared by Claude Opus 5.5, and their links were checked by AI agents as described above.

## Licence

The notebook (its code and text) and the two lists of named owners are available under the MIT License; see `LICENSE`. Third-party data in `data/` is not relicensed by that grant: Epoch AI's files, and the rows of the two lists that are taken from Epoch's datasets, remain under CC BY 4.0, as stated above, and the Hong Kong trade statistics remain subject to their source's terms.
