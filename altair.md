# Altair snippets

```python
brain = pd.read_csv("https://raw.githubusercontent.com/rezpe/datos_viz/master/brain.csv")
```

## Import altair
```python
import altair as alt
```
## Bar Chart
```python
alt.Chart(brain).mark_bar().encode(
    x = alt.X('Brain Weight', bin=True),
    y=  'count()'  
)
# Choosing bins size

alt.Chart(brain).mark_bar().encode( 
  x=alt.X('Body Weight',bin=alt.Bin(maxbins=100)), 
  y="count()" 
).interactive()
```
## Line Chart
```python
alt.Chart(trends).mark_line().encode(
    x=alt.X('yearmonth(date):T'),# :T converte a temporal
    y=alt.Y('mean(value)'),
    color='search_term'
)
```
## Scatter Chart
```python
alt.Chart(brain).mark_point().encode(
    x = 'Brain Weight',
    y=  'Body Weight'
)
```

## Map Chart
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

## Horizontal and Vertical composition
```python
#Horizontal: use the "&" symbol
scatter_bb & histo_body

#Vertical: use the "|" symbol
scatter_bb | histo_body

#Combined: use the "&" and "|" symbol
scatter_bb & (histo_body|histo_brain)
```
## Interval selection

```python
# Use single for single selection and interval for interval selection

temp = weather.groupby(["date","country"]).mean().reset_index() 

select_country = alt.selection(type="single",encodings=["x"]) 

 

line_weather = alt.Chart(temp).mark_line().encode( 
x='date', 
y="mean(temp)" 
).properties( 
width=800, 
height=200,
selection=select_country 
) .transform_filter(
    select_country
) 

bar_weather = alt.Chart(temp).mark_bar().encode( 
x=alt.X('country', 
sort=alt.Sort(field="temp", 
op="mean", 
order="descending"), 
axis=alt.Axis(labels=False) 
), 
y="mean(temp)", 
tooltip="country" 
).properties( 
width=800, 
height=200,
selection=select_country  
)

bar_weather&line_weather
```



