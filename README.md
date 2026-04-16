OVERVIEW
--------
This toolkit provides Python scripts for analyzing air pollutant concentration 
time series using q-statistical mechanics. It fits q-Gamma distributions to 
concentration data and q-Exponential functions to autocorrelation decays, 
allowing comparison across pollutants, site types, and diurnal cycles.

The scripts are designed for data from the UK-AIR Data Selector 
(https://uk-air.defra.gov.uk/data/data_selector) but can be adapted to other 
similarly formatted files.

SCRIPTS INCLUDED
----------------
1. Single-site time series plot
2. Multi-pollutant q-Gamma distribution fitting (one site)
3. q-Gamma parameter comparison across site types
4. Single-site day/night autocorrelation q-exponential fitting
5. Multi-site day/night autocorrelation q-exponential fitting

INSTALLATION
------------
Required Python packages:
  pandas, numpy, matplotlib, scipy, statsmodels, openpyxl, tqdm

Install via pip:
  pip install pandas numpy matplotlib scipy statsmodels openpyxl tqdm

DATA FORMAT EXPECTATIONS
------------------------
All scripts expect CSV (or Excel) files with:
- First two columns: 'Date' (DD/MM/YYYY) and 'Time' (HH:MM:SS)
- Remaining columns: alternating value and status columns for each 
  pollutant or location

Example for a multi-pollutant file:
  Date,Time,Nitric oxide,Status,Nitrogen dioxide,Status,...
  01/01/2023,00:00:00,12.5 V ugm-3,V,45.2 V ugm-3,V,...

For single-site files, the column name is taken as the site identifier.
Values may be embedded in strings like "12.5 V ugm-3" – the script extracts 
the numeric part automatically.

DOWNLOADING DATA FROM UK-AIR
----------------------------
1. Go to https://uk-air.defra.gov.uk/data/data_selector
2. Choose "Search Hourly Networks" and select a network (e.g., AURN)
3. Select date range, sites, and pollutants
   IMPORTANT: Select pollutant names exactly as:
     Nitric oxide
     Nitrogen dioxide
     PM10 particulate matter
     PM2.5 particulate matter
     Ozone
4. Choose "Hourly measured" statistics
5. Generate and download CSV

The downloaded CSV will have the required format.

USAGE INSTRUCTIONS (by script)
------------------------------

SCRIPT 1: Single-Site Time Series Plot
--------------------------------------
File: First block in the provided code (or save as single_site_plot.py)

Purpose:
  Load a CSV with Date, Time, and one pollutant column, plot concentration 
  over time, and save the figure.

Steps:
  1. Open the script and locate the filepath variable in the main() function.
  2. Update the path to your downloaded CSV file.
  3. Run the script. It will:
     - Print number of records and date range.
     - Display a time series plot.
     - Save the plot as "NO2_TimeSeries.png" (edit the save path if needed).

Example:
  filepath = r"C:\Users\YourName\Data\my_site.csv"

SCRIPT 2: Multi-Pollutant q-Gamma Fitting (One Site)
----------------------------------------------------
File: Second block in the provided code (save as multipollutant_fit.py)

Purpose:
  Fit q-Gamma distributions to all pollutants present in an Excel/CSV file 
  from a single monitoring site. Generate grid plots for non-ozone pollutants 
  and a separate plot for ozone.

Steps:
  1. Update the filepath variable in main() to your file.
  2. Run the script. It will:
     - Parse all pollutants.
     - Fit using MLE and curve_fit.
     - Print fit parameters and goodness-of-fit stats.
     - Save plots to C:\Users\qp24695\Documents\ (change this path in the code).

Example:
  filepath = r"C:\Users\YourName\Data\Leicester_University.xlsx"

SCRIPT 3: q-Gamma Parameter Comparison Across Site Types
--------------------------------------------------------
File: Third block in the provided code (save as parameter_comparison.py)

Purpose:
  For each pollutant (NO2, NO, PM10, PM2.5), load separate CSV files for six 
  site types, fit q-Gamma to every location's full dataset, and create scatter 
  plots of (alpha, q, lambda) relationships.

Steps:
  1. Inside main(), locate the pollutant_configs dictionary.
  2. Update all file paths to your own CSV files (one per pollutant-site type).
  3. Run the script. It will:
     - Process each file and fit parameters for each location.
     - Print summary statistics.
     - Save two plots: "all_pollutants_vertical_All_Data.png" and 
       "q_vs_alpha_comparison_All_Data.png".

Note: This script uses all data (no seasonal/day-night splitting).

SCRIPT 4: Single-Site Day/Night Autocorrelation q-Exponential Fit
-----------------------------------------------------------------
File: Fourth block (save as autocorr_single_site.py)

Purpose:
  Compute autocorrelation function up to lag 5 for a single site, fit a 
  q-exponential decay model separately for daytime (06:00-17:59) and nighttime 
  (18:00-04:59), and plot the results.

Steps:
  1. Update the pd.read_csv filepath near the top of the script.
  2. Ensure the CSV contains columns named exactly:
     "Nitric oxide", "Nitrogen dioxide", "PM10 particulate matter", 
     "PM2.5 particulate matter".
  3. Run the script. It will:
     - Print tables of fit parameters and goodness-of-fit.
     - Generate a two-panel plot saved as "qexp_fit_comparison.png".

Example read line:
  df = pd.read_csv(r"C:\Users\YourName\Data\London_Marylebone_Road.csv")

SCRIPT 5: Multi-Site Day/Night Autocorrelation q-Exponential Fit
----------------------------------------------------------------
File: Fifth block (save as autocorr_all_sites.py)

Purpose:
  Extend Script 4 to all site types. For each pollutant, load the six site-type 
  CSV files, compute ACF for day and night periods, fit q-exponential, and 
  produce combined q vs lambda scatter plots.

Steps:
  1. In the filepaths_dict dictionary near the top, update all file paths.
  2. Optionally change the output_dir variable.
  3. Run the script. It will:
     - Save a CSV of fit results per pollutant.
     - Generate a combined 2x2 plot "all_pollutants_q_vs_lambda_combined.png".
     - Generate individual plots for each pollutant.


Last updated: April 2026
