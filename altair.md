# Altair snippets

```python
brain = pd.read_csv("https://raw.githubusercontent.com/rezpe/datos_viz/master/brain.csv")
```

# Import altair

```python
import altair as alt
# if in notebook
alt.renderers.enable("notebook")
```

# Histogram

```python
alt.Chart(df).mark_bar().encode(
    x=alt.X('Brain Weight',bin=True,title="Brain Weight Binned"),
    y="count()"
).properties(
    width=200,
    height=200,
    title="Histogram of Brain Weight"
)
``` 

# Scatter Plot 

```python
scatter = alt.Chart(df).mark_circle().encode(
    x="Body Weight",
    y="Brain Weight"
)
``` 
# Bar Chart
```python
trends_bar=alt.Chart(trends).mark_bar().encode(
    x="search_term",
    y="sum(value)",
    color="search_term"
)
```

# Line Chart
```python
trends_line=alt.Chart(trends).mark_line().encode(
    x=alt.X("date:T",timeUnit="yearmonth"),
    y="sum(value)",
    color="search_term"
)
```


# Map Chart
```python
states = alt.topo_feature("https://unpkg.com/world-atlas@1.1.4/world/110m.json",feature="countries") 

background = alt.Chart(states).mark_geoshape(
fill="lightgray", 
stroke="white" 
) 

temp = weather.groupby(["city", 'date']).mean().reset_index() 

points = alt.Chart(temp).mark_point().encode( 
latitude="lat:Q", 
longitude="lng:Q", 
color=alt.Color("mean(temp)",scale=alt.Scale(range=["blue","orange","red"])) 
) 

(background+points).properties( 
title="Weather Stations", 
width=800, 
height=800 
)
```

# Horizontal concatenation

```python
trends_bar|trends_line
```

# Vertical concatenation

```python
trends_bar&trends_line
```

# Single selection

```python
select_search_term = alt.selection_single(encodings=["x"])

trends_line=alt.Chart(trends).mark_line().encode(
    x=alt.X("date:T",timeUnit="yearmonth"),
    y="sum(value)",
    color="search_term"
).transform_filter(
    select_search_term
)

trends_bar=alt.Chart(trends).mark_bar().encode(
    x="search_term",
    y="sum(value)",
    color="search_term"
).properties(
    selection=select_search_term
)

trends_bar|trends_line
```

# Interval Selection

```python
selection = alt.selection_interval(encodings=["x"])

trends_line=alt.Chart(trends).mark_line().encode(
    x=alt.X("date:T",timeUnit="yearmonth"),
    y="sum(value)",
    color="search_term"
).transform_filter(
    selection
)

trends_line_small=alt.Chart(trends).mark_line().encode(
    x=alt.X("date:T",timeUnit="yearmonth"),
    y="sum(value)",
    color="search_term"
).properties(
    height=50,
    selection=selection
)

(trends_line&trends_line_small)
```

## Custom config
```python
config = {'arc': {'fill': '#30a2da'}, 
'area': {'fill': 'black'}, 
'axisBand': {'grid': False}, 
'axisBottom': {'domain': False, 
'domainColor': '#999', 
'domainWidth': 3, 
'grid': False, 
'gridColor': '#cbcbcb', 
'gridWidth': 1, 
'labelColor': '#999', 
'labelFontSize': 14, 
'labelPadding': 4, 
'tickColor': 'white', 
'tickSize': 10, 
'titleFontSize': 14, 
'titlePadding': 10}, 
'axisLeft': {'domainColor': 'white', 
'domainWidth': 1, 
'grid': False, 
'gridColor': 'white', 
'gridWidth': 1, 
'labelColor': 'white', 
'labelFontSize': 10, 
'labelPadding': 4, 
'tickColor': 'white', 
'tickSize': 10, 
'ticks': True, 
'titleFontSize': 14, 
'titlePadding': 10}, 
'axisRight': {'domainColor': '#333',
'domainWidth': 1, 
'grid': True, 
'gridColor': '#cbcbcb', 
'gridWidth': 1, 
'labelColor': 'white', 
'labelFontSize': 10, 
'labelPadding': 4, 
'tickColor': 'white', 
'tickSize': 10, 
'ticks': True, 
'titleFontSize': 14, 
'titlePadding': 10}, 
'axisTop': {'domain': False, 
'domainColor': '#333', 
'domainWidth': 3, 
'grid': False, 
'gridColor': '#cbcbcb', 
'gridWidth': 1, 
'labelColor': '#999', 
'labelFontSize': 10, 
'labelPadding': 4, 
'tickColor': '#cbcbcb', 
'tickSize': 10, 
'titleFontSize': 14, 
'titlePadding': 10}, 
'background': 'white', 
'group': {'fill': 'black',"stroke":"white"}, 
'legend': {'labelColor': '#333', 
'labelFontSize': 14, 
'padding': 1, 
'symbolSize': 30, 
'symbolType': 'square', 
'titleColor': '#333', 
'titleFontSize': 14, 
'titlePadding': 10}, 
'line': {'stroke': '#30a2da', 'strokeWidth': 2}, 
'path': {'stroke': '#30a2da', 'strokeWidth': 0.5}, 
'rect': {'fill': '#30a2da'}, 
'range': {'category': ['#30a2da', 
'#fc4f30', 
'#e5ae38', 
'#6d904f', 
'#8b8b8b', 
'#b96db8', 
'#ff9e27', 
'#56cc60', 
'#52d2ca', 
'#52689e', 
'#545454', 
'#9fe4f8'], 
'diverging': ['#cc0020', 
'#e77866', 
'#f6e7e1', 
'#d6e8ed', 
'#91bfd9', 
'#1d78b5'], 
'heatmap': ['#d6e8ed', '#cee0e5', '#91bfd9', '#549cc6', '#1d78b5']}, 
'symbol': {'filled': True, 'shape': 'circle'}, 
'shape': {'stroke': 'white'}, 
'style': {'bar': {'binSpacing': 2, 'fill': '#30a2da', 'stroke': None}}, 
'title': {'anchor': 'start', 'fontSize': 24, 'fontWeight': 600, 'offset': 20},
"view": { 
"stroke": "transparent" 
}}
```

