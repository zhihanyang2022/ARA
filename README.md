# Applied Regression Analysis

* TOC 
{:toc} 
## Read in csv files

- Set working directory that contains the interested csv file
- `dat<-read.csv("test.csv")`

## Basics of dataframe

A dataframe can be created from vectors, each containing a particular feature for all examples.

```R
x<-c(1, 2, 3)  # feature x of all 3 examples
y<-c(5.5, 6.1, 231)  # feature y of all 3 examples
z<-c(4, 7, 8)  # feature z of all 3 examples

df<-data.frame(x, y, z)  # create a dataframe

df$x  # access the vector (col) "x" of the dataframe "df"
df$x<-c(2, 3, 4)  # edit the content of a vector inside a dataframe
```

## ggplot2

### Install ggplot2

```R
install.packages("ggplot2")
```

### Bring ggplot2 to current session

```r
library(ggplot2)
```

### Plot a single point (4, 9) using `geom_point`

```r
x<-4
y<-9
dat<-data.frame(x, y)
ggplot()+geom_point(data=dat, aes(x=x, y=y), size=10, color="red")
```

More info on `geom_point`:

```R
ggplot()+geom_point(data=dat, aes(x=x, y=y), size=10, color="red")
```
- `geom_point`: geometry for plotting points
  - `data`: dataframe that contains data
  - `aes`
    - `x=x`: x values come from the x vector 
    - `y=y`: y values come from the y vector
  - `size`: size of each point on plot
  - `color`: color of each point on plot
      - http://www.stat.columbia.edu/~tzheng/files/Rcolor.pdf
  - `shape`: shape of each point on plot, given by integers
      - http://sape.inf.usi.ch/quick-reference/ggplot2/shape

## Control axis limits

```r
ggplot()+
	geom_point(data=dat, aes(x=x, y=y), size=10, color="red")+
	scale_x_continuous(limits=c(-10, 30))+
	scale_y_continuous(limits=c(-20, 80))
```

### Plot multiple points

Plot point (2, 6), (5, 4) and (1, 9):

```r
x<-c(2, 5, 1)
y<-c(6, 4, 9)
dat<-data.frame(x, y)
ggplot()+geom_point(data=dat, aes(x=x, y=y), size=10, color="red")
```

### Graph line from endpoints

Plot line from (1, 3) to (8, 10); plot line from (-2, 13) to (7, -5).

```r
x<-c(1, 8)
x<-c(3, 10)
dat<-data.frame(x, y)
ggplot()+geom_line(data=dat, ase(x=x, y=y))
```

### Graph line from equation

Graph $y=3x+1$ where $x\in[0, 10]$:

```r
x<-c(0, 10)
x<-3*x+1
dat<-data.frame(x, y)
ggplot()+geom_line(data=dat, ase(x=x, y=y))
```

## Sampling from populations

### Random integer

```r
sample(1:10, 100, replace=TRUE)  
# 1st arg: sample from 1 to 10 inclusive
# 2nd arg: sample 100 points
# replace: whether to sample with replacement
```

### Normal populations

```r
rnorm(100, 50, 10)
# 1st arg: number of points to sample
# 2nd arg: mean
# 3rd arg: standard deviation
```

### Plot a vertical sample

```r
x<-rep(1, 100)  # repeat 1 100 times
y<-rnorm(100, 50, 10)
dat<-data.frame(x, y)

x<-1
y<-50
mean<-data.frame(x, y)

ggplot()
	+geom_point(data=dat, aes(x=x, y=y))
	+geom_point(data=mean, aes(x=x, y=y), size=7, color="red")
```

### Plot several vertical samples

Task: plot three distinct vertical samples of 100 points each.

- **Sample 1:** x=1, mean=50, std=10
- **Sample 2:** x=9, mean=30, std=10
- **Sample 3:** x=15, mean=78, std=10

```r
x<-rep(1, 100)
x<-c(x, rep(9, 100))
x<-c(x, rep(15, 100))

y<-rnorm(100, 50, 10)
y<-c(y, rnorm(100, 30, 10))
y<-c(y, rnorm(100, 78, 10))

dat<-data.frame(x, y)

x<-c(1, 9, 15)
y<-c(50, 30, 78)
means<-data.frame(x, y)

ggplot()+
	geom_point(data=dat, aes(x=x, y=y))+
  geom_point(data=means, aes(x=x, y=y), color="red")
```

### Samples along a line

Create four vertical samples of 100 points each. Means must lie on the line $y=3x+1$. Locations of x are 1, 9, 15, 22.

```r
# generate data for line
x<-c(0, 25)
y<-3*x+1
line<-data.frame(x, y)

# generate means
x<-c(1, 9, 15, 22)
y<-3*x+1
means<-data.frame(x, y)

# generate samples
x<-c(rep(1, 100), rep(9, 100), rep(15, 100), rep(22, 100))
y<-c(rnorm(100, means[1], 10), rnorm(100, means[2], 10), rnorm(100, means[3], 10), rnorm(100, means[4], 10))
samples<-data.frame(x, y)

ggplot()+
	geom_line(data=line, aes(x=x, y=y))+
  geom_point(data=means, aes(x=x, y=y), size=7, color="red")+
	geom_point(data=samples, aes(x=x, y=y))
```

### `sapply`

```r
x<-c(2, 4, 9, 15)

# these two are equivalent
sqrt(x)
sapply(x, function(x) sqrt(x))
```

### Cloud of points

Problem:

- Generate 100 data points. 
- The x-coordinates are drawn from a normal population of mean 10 and standard deviation 5. 
- For each x-value, one y-value is drawn from a normal population with mean 3x+1 and standard deviation 10.

```r
x<-rnorm(100, 10, 5)
y<-3*x+1
means<-data.frame(x, y)

# generate one y value for each x value
y<-sapply(x, function(x) rnorm(1, 3*x+1, 10))
samples<-data.frame(x, y)
          
ggplot()+
    geom_point(data=means, aes(x=x, y=y), color="red")+
    geom_point(data=samples, aes(x=x, y=y))
```

<img src="/Users/yangzhihan/My Files/Typora Pics/image-20200414180032216.png">

## Simple linear regression in R

### Father and son heights

```r
library(UsingR)
head(father.son)  # view the first few rows

x<-c(60, 75)
y<-c(63, 78)
line<-data.frame(x, y)

# plot data, and add a line
ggplot()+
	geom_point(data=father.son, aes(x=fheight, y=sheight))+
	geom_line(data=lie, aes(x=x, y=y))
```

### Residual visualization

```r
x<-father.son$fheight
y<-father.son$sheight

group<-1:1078
dat<-data.frame(x, y, group)

# create endpoints for plotting the line
x<-c(0, 100)
y<-x+3
line<-data.frame(x, y)

# for each data point in the dataset, get a point with the same x value but is on the line
y<-x+3  # the values of y predicted by the model
means<-data.frame(x, y, group)

# similar to np.vstack
d<-rbind(dat, means)

ggplot()+
	geom_point(data=dat, aes=(x=x, y=y))+
	geom_line(data=line, aes=(x=x, y=y))+
	geom_point(data=means, aes=(x=x, y=y), color="red")+
	geom_point(data=d, aes=(x=x, y=y, group=group))
# for each group, the "group" argument plots one line for points in that group
```

### Sum of squared residuals

Square each residual and then add them all up.

```r
residuals<-means$y-dat$y
squared_residuals<-residuals^2
sum_of_squared_residuals<-sum(squared_residuals)
```

### The least-squares line

```r
lm(y~x, data=dat)

# read from the output of the command above
slope<-0.5141
intercept<-33.8866
x<-c(57, 78)
y<-slope*x+intercept
line<-data.frame(x, y)

x<-means$x
x<-slope*x+intercept
means<-data.frame(x,y,group)

d=rbind(dat, means)

ggplot()+
	geom_point(data=dat, aes=(x=x, y=y))+
	geom_line(data=line, aes=(x=x, y=y))+
	geom_point(data=means, aes=(x=x, y=y), color="red")+
	geom_point(data=d, aes=(x=x, y=y, group=group))
```

### Prediction

```r
pred<-slope*input+intercept
```

























