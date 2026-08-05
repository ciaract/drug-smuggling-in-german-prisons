# 📨 Smuggled in the Mail: The Flood of Synthetic Cannabis in German Prisons 

The website for the research is [here](https://ciaract.github.io/drug-smuggling-in-german-prisons/). 

## 🚩 Goal
I developed this investigation as part of the __Lede Program at Columbia University in New York__. The project is primarily intended as an opportunity to apply the methods and tools taught during the program in a practical setting. However, I quickly became a little overambitious and decided not only to put my newly acquired skills into practice, but also to combine them with an investigative project that I can continue working on after the program as part of my professional work as a journalist.

As an investigative journalist, I have repeatedly covered drug use and developments in Germany's illicit drug market. In recent years, one trend in particular caught my attention: the growing importance of new psychoactive substances (NPS). At the same time, German media increasingly reported on the emergence of NPS in the country's prison system. For example, the tabloid newspaper "BILD" ran the headline: "[Zombie Drugs Are Flooding Germany's Prisons: The Danger Comes in the Mail](https://www.bild.de/politik/inland/unsichtbare-gefahr-so-fluten-neue-drogen-deutschlands-gefaengnisse-6a47ce922e8f13ca8ed2b342) (translated from the original German headline). Despite the growing public attention, there has been no comprehensive, data-driven assessment of how widespread NPS use in German prisons actually is or how drug smuggling into correctional facilities has evolved over time. __My primary goal was to document how patterns of drug use among incarcerated people have changed over time and, above all, how smuggling routes and methods have adapted in response to increased security measures.__ By analyzing available data, this investigation assesses the scale of the issue, identifies long-term trends, and examines whether the public narrative is supported by the evidence. 

Due to the limited availability and inconsistent documentation of official data, these objectives could not be fully achieved. Nevertheless, this investigation provides the closest possible approximation by compiling and analyzing the available evidence and highlighting important gaps in official reporting.

## 📖 Repository Guide

The repository is structured as follows:
```
├── analysis.ipynb
├── docs
│   ├── charts
│   │   ├── 01_prison_berlin.svg
│   │   ├── 02_prison_hamburg_2.svg
│   │   ├── 02_prison_hamburg.svg
│   │   └── 03_prison_baden_wuerttemberg.svg
│   ├── index.html
│   └── JWH-018.jpg
├── input
│   └── penal_system_data.xlsx
├── output
│   ├── df_NPS_BaWue.xlsx
│   ├── df_NPS_Berlin.xlsx
│   ├── df_NPS_HH_grouped_hochrechnung.xlsx
│   ├── df_NPS_HH_grouped_trend.xlsx
│   ├── df_NPS_HH_grouped.xlsx
│   ├── df_NPS_HH_pivot_1.xlsx
│   ├── df_NPS_HH_pivot_2.xlsx
│   └── df_NPS.xlsx
└── README.md
```
`analysis.ipynb` contains the Python code that analyzes the input file <br> 
`docs/charts` contains the published output <br> 
`docs/charts` contains all the graphics created based on my analysis <br> 
`input` contains the raw data that I manually aggregated based on my press inquiries (s. [Data Gathering](#data-gathering)) <br> 
`output` contains the processed datasets, they form the basis for my charts 

<a id="data-gathering"></a>
## 🗂️ Data Gathering
__Comprehensive data on drug seizures in German prisons is not publicly available. Therefore, I had to obtain the information through press inquiries.__

Germany has nearly 200 correctional facilities. Requesting data from every single prison as part of this project was and remains virtually impossible. I therefore initially decided to contact a sample of 12 prisons. However, some facilities either had not collected the relevant data, did not have the capacity to respond to my request, or referred me to the respective state authorities.

As a second approach, I submitted inquiries to the justice ministries and senate departments of some German federal states. However, not all state justice ministries had access to the required data and some referred me back to individual prisons.

In the end, I decided to pursue both approaches in parallel and work with all available data I was able to obtain. This is also an important limitation of the analysis, as the current dataset is not yet representative of the entire German prison system. More on this can be found in the limitations section (s. [Limitations and Future Steps](#limitations-and-future-steps)). 

As expected, __all correctional facilities, state justice ministries and senate departments collect their data differently__. Some prisons, for example, only record how often specific substances were found, while others document the exact quantities seized. These different approaches make a comparative data analysis challenging. The categories used to document substances also vary significantly. Some for example, use categories such as NPS paper consumption units, while others record NPS-treated materials by surface area in cm². __I manually reviewed all responses and datasets received and transferred the relevant information into this Excel table, which served as the underlying dataset for this analysis__ (s. `input/penal_system_data.xlsx`).

<details>
<summary>Contacted prisons and justice ministries / senate departments </summary>

**Prisons (JVA):** Tegel, Köln, Freiburg, Wolfenbüttel, Aachen, Adelsheim, Aichach, Amberg, Arnstadt, Asperg, Asperg (Justizvollzugskrankenhaus), Attendorn

**State justice ministries / senate departments:**
- Senatsverwaltung für Justiz und Verbraucherschutz Berlin
- Ministerium der Justiz und für Migration Baden-Württemberg
- Bayerisches Staatsministerium der Justiz
- Ministerium der Justiz des Landes Brandenburg
- Die Senatorin für Justiz und Verfassung (Bremen)
- Behörde für Justiz und Verbraucherschutz (Hamburg)
- Ministerium der Justiz des Landes Nordrhein-Westfalen

</details>

<details>
<summary>Information requested (example: inquiry submitted to a state justice ministry)</summary>

Where possible, we requested data for the past 10 years (or the respective available period), ideally broken down by year and by correctional facility (JVA).

1. Recorded drug seizures
   - Number of detected drug seizures or confiscations within correctional facilities
   - Where documented: breakdown by substance (e.g. cannabis, cocaine, opioids, synthetic cannabinoids, other NPS)
   - Where documented: confirmed routes of introduction (e.g. mail/package deliveries, visitors, return from temporary leave or unsupervised release, staff members, drones, other)
2. Indicators of drug use
   - Number of drug screenings conducted
   - Number of positive test results
3. Medical and security-related incidents associated with drug use
   - Number of documented drug-related medical emergencies caused by intoxication
   - Number of hospital transfers related to drug use
   - Number of life-threatening incidents / resuscitations related to substance use
   - Number of deaths with suspected or confirmed links to drug use
   - Where recorded: number of acute psychiatric incidents (e.g. drug-induced psychosis)

_For data protection reasons, the responses provided by the press offices are not reproduced here. The figures for the requested data points are compiled in `input/penal_system_data.xlsx`_.

</details>

## 📊 Data Analysis and Methodological Decisions

All subsequent analyses were conducted in Python. The following sections describe the key methodological decisions taken throughout the analysis: 

#### 1. Recorded Drug Seizures 
#### 1.1. Harmonizing data 
As already described in the Data Gathering section (s. [Data Gathering](#data-gathering)), all correctional facilities, state justice ministries, and senate departments document their data differently. My initial approach was to harmonize the different recording methods by converting all available data into consumption units. To ensure that such a conversion would be methodologically sound, I consulted several experts. However, all experts advised against this approach because even a single piece of paper can contain different concentrations of a substance and therefore represent different numbers of consumption units. I therefore decided not to create a unified dataset based on conversions. Instead, I focused on analysing groups of data where the recording method was consistent within the dataset. One example is the state of Baden-Württemberg, which provided aggregated data covering all correctional facilities within the state.

However, the categories “Spice_g” and “NPS_Mischung_g” were combined, as follow-up inquiries confirmed that they represent the same physical product category (herbal mixtures) and transition seamlessly into one another.

#### 1.2. My request for "confirmed routes of introduction" 
Regarding my request for "confirmed routes of introduction" (e.g., mail/package deliveries, visitors, return from temporary leave or unsupervised release, staff members, drones, or other routes), it became clear that no correctional facility systematically records data on smuggling routes. In a few cases, facilities indicated that such information was not available or could not be shared.

#### 2. Indicators of drug use
The requested data on "indicators of drug use" were too inconsistent and incomplete to be included in the analysis. This was primarily because these incidents were not systematically recorded by most correctional facilities.


#### 3. Medical and security-related incidents associated with drug use
The requested data on "medical and security-related incidents associated with drug use", were too inconsistent and incomplete to be included in the analysis. This was primarily because these incidents were not systematically recorded by most correctional facilities.

<a id="limitations-and-future-steps"></a>
## ⚠️ Limitations and Future Steps
This analysis provides an initial overview of the role of NPS in the German prison system. However, it is important to note that the findings represent an approximation due to existing data gaps. The analysis highlights initial trends and patterns but does not yet provide a complete nationwide assessment.

First, not all state justice ministries, senate departments, and correctional facilities have been contacted. A next step would therefore be to expand the data collection in order to create a more comprehensive picture of developments across Germany.

Second, many federal states are only at the beginning of systematically recording NPS-related incidents. Some categories have only been documented for a few years or were introduced very recently. This can distort observed trends: An increase in recorded cases may reflect either an actual change in smuggling patterns or improved data collection. Ultimately, a data analysis can only be as reliable as the underlying data collection.

Beyond the prison system, further research could examine the role of NPS on the wider drug market. While this analysis focuses on correctional facilities, it would be important to investigate how widespread synthetic cannabinoids and other new psychoactive substances are outside prisons and which developments can be observed there.