import dash
from dash import dcc, html
import pandas as pd
import plotly.express as px
import plotly.graph_objs as go
from dash.dependencies import Input, Output

# Load data
province_data = pd.read_csv('/content/drive/MyDrive/province_data.csv')
poverty_rate_data = pd.read_csv('/content/drive/MyDrive/nepal_poverty_data.csv')
geographic_data = pd.read_csv('/content/drive/MyDrive/geographic_data.csv')
demographic_data = pd.read_csv('/content/drive/MyDrive/demographic_data.csv')
income_expenditure_data = pd.read_csv('/content/drive/MyDrive/income_expenditure_data.csv')
health_data = pd.read_csv('/content/drive/MyDrive/health_data.csv')
economic_data = pd.read_csv('/content/drive/MyDrive/economic_data.csv')
education_data = pd.read_csv('/content/drive/MyDrive/education_data.csv')
infrastructure_data = pd.read_csv('/content/drive/MyDrive/infrastructure_data.csv')

# Initialize the Dash app
app = dash.Dash("Poverty of Nepal by UNU")

app.layout = html.Div([
    html.H1("Nepal Poverty Data Dashboard"),
    
    dcc.Tabs([
        dcc.Tab(label='Province Data', children=[
            dcc.Graph(
                id='province-bar-chart',
                figure=px.bar(province_data, x='Province', y='Poverty Rate (%)', title="Poverty Rate by Province")
            )
        ]),
        dcc.Tab(label='Poverty Rate Over Time', children=[
            dcc.Graph(
                id='poverty-rate-line-chart',
                figure=px.line(poverty_rate_data, x='Indicator', y='Value', title="Poverty Rate Over Time")
            )
        ]),
        dcc.Tab(label='Geographic Data', children=[
            dcc.Graph(
                id='geographic-bar-chart',
                figure=px.bar(geographic_data, x='Region', y='Poverty Rate (%)', title="Poverty Rate by Region")
            )
        ]),
        dcc.Tab(label='Demographic Data', children=[
            dcc.Graph(
                id='demographic-bar-chart',
                figure=px.bar(demographic_data, x='Population Segment', y='Poverty Rate (%)', title="Poverty Rate by Population Segment")
            )
        ]),
        dcc.Tab(label='Income and Expenditure Data', children=[
            dcc.Graph(
                id='income-expenditure-line-chart',
                figure=go.Figure()
                    .add_trace(go.Scatter(x=income_expenditure_data['Year'], y=income_expenditure_data['Average Annual Consumption Spending (Rs)'], mode='lines+markers', name='Average Annual Consumption Spending (Rs)'))
                    .add_trace(go.Scatter(x=income_expenditure_data['Year'], y=income_expenditure_data['Non-Food Spending (%)'], mode='lines+markers', name='Non-Food Spending (%)'))
                    .update_layout(title="Income and Expenditure Over Time", xaxis_title="Year", yaxis_title="Value")
            )
        ]),
        dcc.Tab(label='Health Data', children=[
            dcc.Graph(
                id='health-bar-chart',
                figure=px.bar(health_data, x='Health Indicator', y='Value', title="Health Indicators")
            )
        ]),
        dcc.Tab(label='Economic Data', children=[
            dcc.Graph(
                id='economic-bar-chart',
                figure=px.bar(economic_data, x='Economic Indicator', y='Value', title="Economic Indicators")
            )
        ]),
        dcc.Tab(label='Education Data', children=[
            dcc.Graph(
                id='education-bar-chart',
                figure=px.bar(education_data, x='Education Indicator', y='Value', title="Education Indicators")
            )
        ]),
        dcc.Tab(label='Infrastructure Data', children=[
            dcc.Graph(
                id='infrastructure-bar-chart',
                figure=px.bar(infrastructure_data, x='Infrastructure Indicator', y='Value', title="Infrastructure Indicators")
            )
        ])
    ])
])

if __name__ == '__main__':
    app.run_server(debug=True)

