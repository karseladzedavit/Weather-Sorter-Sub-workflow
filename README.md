# 🌍 Global Weather Dashboard Automation (n8n)

This project is a fully automated weather monitoring system built with **n8n**. It fetches live weather data for a list of global capitals every 10 minutes and organizes them into two different Google Sheet views: a Master Log and a Categorized Dashboard.

## 🚀 How It Works

The system consists of two main workflows working together:

### 1. Main Workflow (`Weathersheet.json`)
*   **Trigger:** Runs automatically every 10 minutes via a Schedule Trigger.
*   **Data Sourcing:** Fetches country data from the `restcountries` API and precise weather coordinates.
*   **Weather Fetching:** Calls the `Open-Meteo` API to get current temperatures.
*   **Master Update:** Updates a "Master List" in Google Sheets row-by-row with a timestamp.
*   **Batch Handoff:** Once all cities are processed, it clears the previous summary and sends the entire batch to the Sub-workflow.

### 2. Categorization Sub-workflow (`WeatherSorter.json`)
*   **Logic Engine:** Uses a JavaScript Code Node to sort cities into 7 specific temperature categories:
    *   ❄️ **Freezing** (< 0°C)
    *   🥶 **Cold** (0 - 9.9°C)
    *   🍃 **Cool** (10 - 14.9°C)
    *   🌤️ **Mild** (15 - 19.9°C)
    *   ☀️ **Warm** (20 - 24.9°C)
    *   🔥 **Hot** (25 - 29.9°C)
    *   🌋 **Very Hot** (>= 30°C)
*   **Vertical Stacking:** The workflow calculates the optimal layout to "stack" cities vertically in columns without leaving empty rows, ensuring a clean dashboard view.

## 📊 Spreadsheet Structure
The automation populates two sheets:
1.  **Master Sheet:** A list of countries with their current temperature and "Last Updated" time.
2.  **Category Sheet:** A dynamic table where cities appear in columns based on their current weather conditions.

## 🛠️ Tech Stack
*   **n8n:** Workflow automation and logic.
*   **JavaScript:** Advanced data transformation and array manipulation.
*   **Google Sheets API:** Data storage and dashboarding.
*   **Open-Meteo API:** Free weather data.
*   **RestCountries API:** Geographical data.

## ⚙️ Setup Instructions
1.  Import the `.json` files into your n8n instance.
2.  Configure your **Google Sheets Credentials**.
3.  Replace the `Spreadsheet ID` in the Google Sheets nodes with your own sheet IDs.
4.  Ensure the Sub-workflow is saved so the Main Workflow can call it by name/ID.
5.  Set both workflows to **Active**.
