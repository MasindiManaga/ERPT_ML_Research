# Data Acquisition and Integrity Verification

## 1. Purpose

This document records the official sources, original filenames, acquisition procedures and integrity checks for the raw data used in the dissertation.

The raw files are excluded from version control because of redistribution and repository-size considerations. Researchers must obtain the files from the official providers and verify them against the SHA-256 checksums in `docs/data_checksums.csv`.

## 2. Source inventory

| File | Provider | Purpose | Coverage in source file | Role in the study |
|---|---|---|---|---|
| `CPI_Average Prices_All urban(202604).xlsx` | Statistics South Africa | National average retail prices | January 2017–April 2026 | Complementary descriptive data |
| `CPI_Average prices_Provinces(202604).xlsx` | Statistics South Africa | Provincial average retail prices | January 2017–April 2026 | Complementary descriptive data |
| `EXCEL - CPI (COICOP 2018 - 8digit) (202604).xlsx` | Statistics South Africa | Detailed CPI indices classified at the COICOP 2018 eight-digit level | January 2008–April 2026 | Primary food-price dataset |
| `HistoricalRateDetail.csv` | South African Reserve Bank | Daily rand-per-US-dollar exchange rate | 2 January 2015–18 May 2026 | Primary exchange-rate dataset |

The modelling analysis was restricted to observations ending in December 2025. Values dated after December 2025 were not used in model estimation or locked-test evaluation.

## 3. Statistics South Africa CPI files

### Official source

Statistics South Africa, Consumer Price Index, Statistical Release P0141.

Official time-series page:

https://www.statssa.gov.za/?page_id=1847

Consumer Price Index publication page:

https://www.statssa.gov.za/?PPN=P0141&page_id=1854

April 2026 CPI release information:

https://www.statssa.gov.za/?p=19553

### Acquisition date

27 June 2026

### Acquisition procedure

1. Open the Statistics South Africa time-series or Consumer Price Index publication page.
2. Locate publication series `P0141`.
3. Select the April 2026 time-series release.
4. Download the following Excel datasets:
   - CPI Average Prices All urban;
   - CPI Average Prices Provinces; and
   - CPI at the COICOP 2018 eight-digit level.
5. Extract the downloaded files if they are supplied in ZIP archives.
6. Preserve the original filenames listed in the source inventory.
7. Place the files in `data/raw/`.
8. Verify their SHA-256 hashes against `docs/data_checksums.csv`.

The two average-price files contain retail prices expressed in South African rand and were used as complementary descriptive sources. The COICOP eight-digit CPI workbook contains the category-level CPI indices used as the primary dependent-price data.

## 4. South African Reserve Bank exchange-rate file

### Official source

South African Reserve Bank, Selected Historical Rates:

https://www.resbank.co.za/en/home/what-we-do/statistics/key-statistics/selected-historical-rates

### Acquisition date

27 June 2026

### Acquisition procedure

1. Open the SARB Selected Historical Rates page.
2. Select the historical rand-per-US-dollar exchange-rate series.
3. Select a date range covering at least 2 January 2015 to 18 May 2026.
4. Display the historical observations.
5. Download the observations in CSV format.
6. Preserve the downloaded file as `HistoricalRateDetail.csv`.
7. Place the file in `data/raw/`.
8. Verify its SHA-256 hash against `docs/data_checksums.csv`.

The downloaded file contains daily exchange-rate observations. The preparation workflow removes the source metadata rows, parses the dates and values, aggregates the daily observations to monthly frequency and restricts the analytical sample to December 2025.

## 5. Directory placement

After acquisition, the raw directory must contain:

```text
data/raw/
|-- CPI_Average Prices_All urban(202604).xlsx
|-- CPI_Average prices_Provinces(202604).xlsx
|-- EXCEL - CPI (COICOP 2018 - 8digit) (202604).xlsx
`-- HistoricalRateDetail.csv
