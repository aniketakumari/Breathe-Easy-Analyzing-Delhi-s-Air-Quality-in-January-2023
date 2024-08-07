# Breathe Easy : Clear Insights into Delhi's Air Quality

## Project Overview

**Breathe Easy** is a comprehensive data analysis project focusing on the Air Quality Index (AQI) of Delhi, India, during January 2023. This project utilizes Python to gather, clean, and analyze air quality data, providing insights into the distribution of pollutants and the overall air quality in one of the world's most polluted cities.

## Objectives

- **Data Collection and Preprocessing:** Import and clean air quality data for accurate analysis.
- **AQI Calculation:** Compute the AQI based on pollutant concentrations using standard guidelines.
- **Visualization:** Create informative visualizations to explore trends in air quality over time.
- **Comparison and Insights:** Compare AQI metrics with standard guidelines and analyze pollutant distributions.

## Tools and Libraries Used

This project utilizes several Python libraries for data manipulation, visualization, and analysis:

- **Pandas:** For data manipulation and analysis.
- **Plotly:** For creating interactive visualizations.
- **NumPy:** To assist with numerical operations.
- **DateTime:** To handle and manipulate date and time data.

## Dataset

The dataset used in this project contains hourly air quality data for Delhi during January 2023. It includes concentrations of several pollutants, such as CO, NO, NO2, O3, SO2, PM2.5, PM10, and NH3.

## Project Structure

1. **Data Loading and Exploration**
    - **Data Import:** The dataset is loaded into a Pandas DataFrame.
    - **Data Cleaning:** The `date` column is converted to the DateTime format for time series analysis.
    - **Descriptive Statistics:** Basic statistical analysis is performed to understand the data.

2. **Data Visualization**
    - **Time Series Analysis:** A line plot is created to visualize the concentration of pollutants over time.
    - **AQI Calculation:**
        - Breakpoints and corresponding AQI values are defined according to standard guidelines.
        - AQI is calculated for each pollutant and the overall AQI is determined as the maximum of these values.
    - **AQI Visualization:**
        - Bar charts visualize the AQI over time.
        - Histograms show the distribution of AQI categories.
        - Donut charts depict the distribution of pollutant concentrations.
    - **Correlation Analysis:** A heatmap visualizes the correlations between different pollutants.

3. **Further Analysis**
    - **Hourly Trends:** The average AQI is calculated and visualized for each hour of the day.
    - **Weekly Trends:** The average AQI is analyzed by day of the week to identify patterns.

## How to Run the Project

### Prerequisites

Ensure you have the following Python libraries installed:
```bash
pip install pandas plotly
```

### Code Explanation

1. **Data Import and Cleaning:**
    ```python
    import pandas as pd
    
    data = pd.read_csv("delhiaqi.csv")
    data['date'] = pd.to_datetime(data['date'])
    ```

    - **Explanation:** The dataset is imported, and the `date` column is converted to a datetime format to facilitate time-based analysis.

2. **Descriptive Statistics:**
    ```python
    print(data.describe())
    ```

    - **Explanation:** Basic statistics are computed for all pollutants, providing insights into the data distribution and ranges.

3. **Time Series Visualization:**
    ```python
    import plotly.graph_objects as go

    fig = go.Figure()
    for pollutant in ['co', 'no', 'no2', 'o3', 'so2', 'pm2_5', 'pm10', 'nh3']:
        fig.add_trace(go.Scatter(x=data['date'], y=data[pollutant], mode='lines', name=pollutant))
    fig.update_layout(title='Time Series Analysis of Air Pollutants in Delhi', xaxis_title='Date', yaxis_title='Concentration (µg/m³)')
    fig.show()
    ```

    - **Explanation:** This block of code creates a line plot to visualize the concentration of each pollutant over time.

4. **AQI Calculation:**
    ```python
    aqi_breakpoints = [(0, 12.0, 50), (12.1, 35.4, 100), ...]

    def calculate_aqi(pollutant_name, concentration):
        for low, high, aqi in aqi_breakpoints:
            if low <= concentration <= high:
                return aqi
        return None

    def calculate_overall_aqi(row):
        aqi_values = []
        pollutants = ['co', 'no', 'no2', 'o3', 'so2', 'pm2_5', 'pm10', 'nh3']
        for pollutant in pollutants:
            aqi = calculate_aqi(pollutant, row[pollutant])
            if aqi is not None:
                aqi_values.append(aqi)
        return max(aqi_values)

    data['AQI'] = data.apply(calculate_overall_aqi, axis=1)
    ```

    - **Explanation:** AQI is calculated based on pollutant concentrations, and the maximum value is used to determine the overall AQI for each time point.

5. **AQI Visualization:**
    ```python
    import plotly.express as px

    fig = px.bar(data, x="date", y="AQI", title="AQI of Delhi in January")
    fig.show()
    ```

    - **Explanation:** A bar chart is created to visualize the AQI values over time, showing how air quality fluctuates throughout the month.

6. **Correlation Analysis:**
    ```python
    correlation_matrix = data[['co', 'no', 'no2', 'o3', 'so2', 'pm2_5', 'pm10', 'nh3']].corr()
    fig = px.imshow(correlation_matrix, title="Correlation Between Pollutants")
    fig.show()
    ```

    - **Explanation:** A heatmap is used to visualize the correlation between different pollutants, providing insights into potential relationships and common sources.

## Conclusion

The **Breathe Easy** project offers a detailed analysis of air quality in Delhi during January 2023, using data science techniques to calculate and visualize AQI. By understanding the distribution and correlation of pollutants, this project contributes to environmental awareness and public health management.

## Future Work

- Extend the analysis to other months or locations.
- Implement predictive modeling to forecast AQI trends.
- Integrate real-time data for dynamic AQI monitoring.
