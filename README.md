# Solwezi_city_council_Data
# Solwezi Municipal Council CDF Project Analysis

## Project Overview
This project involves the extraction, cleaning, integration, and analysis of data related to the Constituency Development Fund (CDF) projects from the Solwezi Municipal Council website. The primary objective is to understand funding distribution across various sectors, wards, and group types, identify key patterns, and visualize the findings.

## Data Source
Data was extracted from the Solwezi Municipal Council website: [https://www.solwezicouncil.gov.zm/?page_id=11011](https://www.solwezicouncil.gov.zm/?page_id=11011). The extraction specifically targeted HTML tables containing information about CDF projects.

## Data Extraction & Preparation
1.  **Web Scraping**: HTML tables were extracted from the specified URL using `pandas.read_html` and `requests` (with SSL verification disabled due to potential certificate issues).
2.  **CSV Export**: Each extracted table was saved as a separate CSV file (e.g., `table_1.csv`, `table_2.csv`) in the `extracted_tables` directory for easier future access and portability.

## Data Cleaning & Preprocessing
*   **Column Renaming**: Inconsistent column names (e.g., `Engineer�s estimates`, `S/NO`) were standardized to a consistent snake_case format for clarity and mergeability (e.g., `engineers_estimates`, `project_no`).
*   **Summary Row Removal**: Rows containing 'Grand total' or 'Total amount' were identified and removed to prevent aggregation errors.
*   **Monetary Value Conversion**: Columns representing monetary values were cleaned by removing non-numeric characters (e.g., 'K', '$', commas) and converted to a numeric (`float64`) data type. Missing monetary values were imputed using the median of their respective columns.
*   **String Standardization**: Object (string) columns were converted to title case, leading/trailing whitespace was removed, and `NaN` values (resulting from `str(np.nan)`) were replaced with 'Unknown'.
*   **Identifier Conversion**: Identifier columns like `project_no` were converted to nullable integer type (`Int64`) to accurately represent their nature while handling missing values.
*   **Duplicate Removal**: Duplicate rows were identified and removed across all DataFrames to ensure data integrity.

## Data Integration
The cleaned tables were integrated into a single comprehensive DataFrame, `integrated_df`, through a series of merges:
1.  `table_3` and `table_4` were merged to form `projects_df` based on common project-related columns.
2.  `table_1` and `table_2` were merged to form `groups_df` based on common group-related columns.
3.  `projects_df` and `groups_df` were then merged to create the final `integrated_df`, leveraging shared `project_no` and `ward` columns.

## Exploratory Data Analysis (EDA)
EDA was performed on the `integrated_df` to understand data distributions and relationships:
*   **Initial Inspection**: Displayed head, info, descriptive statistics, and missing values of the `integrated_df`.
*   **Categorical Variable Analysis**: Examined unique values and their counts for `sector`, `ward`, `zone`, `group_type`, and `venture_type` to understand their distributions.
*   **Numerical Variable Analysis**: Visualized the distributions of `engineers_estimates`, `allocated_amount_cdf`, `amount_requested`, and `amount_recommended_cdf` using histograms and calculated descriptive statistics.
*   **Funding Analysis by Category**: Calculated average requested and recommended amounts, as well as funding ratios (recommended/requested) by `sector`, `ward`, `group_type`, and `venture_type` to identify funding patterns and disparities.

## Key Findings
1.  **Agriculture Dominance**: Agriculture stands out with the highest number of projects and total funding requests among categorized sectors, showing a healthy funding success ratio.
2.  **Group Funding Disparities**: Community groups generally receive a higher proportion of their requested funds compared to Women's and Youth groups. This highlights potential areas for targeted support to youth entrepreneurship.
3.  **Ward-Level Activity**: Wards such as Kyawama, Tuvwanganai, Kyalalankuba, and Kimasala are highly active in proposing projects, with Kyawama demonstrating a particularly high funding ratio.

## Visualizations
Key findings are supported by visualizations including:
*   Bar chart showing the distribution of projects by sector.
*   Bar chart comparing total requested vs. recommended funding by ward.
*   Bar chart illustrating funding ratios by group type.

## Conclusion
This analysis provides a foundational understanding of the CDF project landscape in Solwezi Municipal Council. It identifies key sectors, group types, and wards that are most active and successful in securing funding. Further in-depth analysis could explore factors influencing funding success, the impact of projects, and potential areas for policy intervention to promote equitable development.

## Output Data
The integrated and cleaned dataset (`integrated_df`) has been saved to a CSV file:
`extracted_tables/db-unza26-csc4792-solwezi_municipal_council_cdf_projects.csv`
