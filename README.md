# Applied Regression Analysis

## Basics of ggplot2 

```
ggplot()+geom_point(data=dat, aes(x=x, y=y), size=10, color='red')
```
- `geom_point`: geometry for plotting points
  - `data`: dataframe that contains data
  - `aes`
    - `x=x`: x values come from the x vector 
    - `y=y`: y values come from the y vector
  - `size`: size of each point on plot
  - `color`: color of each point on plot
