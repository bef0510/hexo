---
title: Python 高階繪圖套件 Bokeh
date: 2019-07-15 11:50:53
tags:
 - Python Module
 - Data Visualization
categories:
 - Python
---

# 高階繪圖套件 Bokeh
1. 安裝模組 **pip install bokeh**
2. 使用 **JavaScript** 縮放圖形的效果

## 分散標記
#### 圓形
    from bokeh.plotting import figure, output_file, show

    output_file("line.html")
    p = figure(plot_width=400, plot_height=400)
    p.circle([1, 2, 3, 4, 5], [6, 7, 2, 4, 5], size=20, color="navy", alpha=0.5)

    show(p)
![Architecture](1.png)

#### 正方形
    from bokeh.plotting import figure, output_file, show

    output_file("square.html")
    p = figure(plot_width=400, plot_height=400)
    p.square([1, 2, 3, 4, 5], [6, 7, 2, 4, 5], size=20, color="olive", alpha=0.5)

    show(p)
![Architecture](2.png)

#### 折線圖
    from bokeh.plotting import figure, output_file, show

    output_file("line.html")
    p = figure(plot_width=400, plot_height=400)
    p.line([1, 2, 3, 4, 5], [6, 7, 2, 4, 5], line_width=2)

    show(p)
![Architecture](3.png)

#### Multiple Lines
    from bokeh.plotting import figure, output_file, show

    output_file("patch.html")
    p = figure(plot_width=400, plot_height=400)
    p.multi_line([[1, 3, 2], [3, 4, 6, 6]], [[2, 1, 4], [4, 7, 8, 5]],
             color=["firebrick", "navy"], alpha=[0.8, 0.3], line_width=4)

    show(p)
![Architecture](4.png)

#### Missing Points
    from bokeh.plotting import figure, output_file, show

    output_file("line.html")
    p = figure(plot_width=400, plot_height=400)
    nan = float('nan')
    p.line([1, 2, 3, nan, 4, 5], [6, 7, 2, 4, 4, 5], line_width=2)

    show(p)
![Architecture](5.png)

#### Bars and Rectangles
    from bokeh.plotting import figure, show, output_file

    output_file('vbar.html')
    p = figure(plot_width=400, plot_height=400)
    p.vbar(x=[1, 2, 3], width=0.5, bottom=0,
        top=[1.2, 2.5, 3.7], color="firebrick")

    show(p)
![Architecture](6.png)

#### Stacked Bars
    from bokeh.models import ColumnDataSource
    from bokeh.plotting import figure, output_file, show

    output_file("hbar_stack.html")
    source = ColumnDataSource(data=dict(
        y=[1, 2, 3, 4, 5],
        x1=[1, 2, 4, 3, 4],
        x2=[1, 4, 2, 2, 3],
    ))
    
    p = figure(plot_width=400, plot_height=400)
    p.hbar_stack(['x1', 'x2'], y='y', height=0.8, color=("grey", "lightgrey"), source=source)

    show(p)
![Architecture](7.png)

#### Multiple MultiPolygons
    from bokeh.plotting import figure, show, output_file

    output_file('multipolygons.html')
    p = figure(plot_width=400, plot_height=400)
    p.multi_polygons(
        xs=[
            [[ [1, 1, 2, 2], [1.2, 1.6, 1.6], [1.8, 1.8, 1.6] ], [ [3, 3, 4] ]],
            [[ [1, 2, 2, 1], [1.3, 1.3, 1.7, 1.7] ]]],
        ys=[
            [[ [4, 3, 3, 4], [3.2, 3.2, 3.6], [3.4, 3.8, 3.8] ], [ [1, 3, 1] ]],
            [[ [1, 1, 2, 2], [1.3, 1.7, 1.7, 1.3] ]]],
        color=['blue', 'red'])

    show(p)
![Architecture](8.png)

#### Wedges and Arcs
    from bokeh.plotting import figure, show

    p = figure(plot_width=400, plot_height=400)
    p.annulus(x=[1, 2, 3], y=[1, 2, 3], inner_radius=0.1, outer_radius=0.25,
            color="orange", alpha=0.6)

    show(p)
![Architecture](9.png)

#### Twin Axes
    from numpy import pi, arange, sin, linspace
    from bokeh.plotting import output_file, figure, show
    from bokeh.models import LinearAxis, Range1d

    x = arange(-2*pi, 2*pi, 0.1)
    y = sin(x)
    y2 = linspace(0, 100, len(y))

    output_file("twin_axis.html")

    p = figure(x_range=(-6.5, 6.5), y_range=(-1.1, 1.1))

    p.circle(x, y, color="red")

    p.extra_y_ranges = {"foo": Range1d(start=0, end=100)}
    p.circle(x, y2, color="blue", y_range_name="foo")
    p.add_layout(LinearAxis(y_range_name="foo"), 'left')

    show(p)
![Architecture](10.png)

# Reference
[Plotting with Basic Glyphs](https://bokeh.pydata.org/en/latest/docs/user_guide/plotting.html#userguide-plotting)