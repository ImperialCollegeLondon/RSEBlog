---
date: 2025-12-10
authors:
  - stephwills
categories:
  - category_a
  - category_b
  - category_c
tags:
  - tag_a
  - tag_b
  - tag_c
---

# Background

Last year, I was involved in developing one of the RSE team's internal projects, [ProCAT](https://github.com/ImperialCollegeLondon/proCAT),  our internal web application for Project Charging and AnalyTics. ProCAT allow us to monitor key metrics such as time spent on project work and the remaining funding, automatically generating the monthly charges to be made to each funding source. It also helps the team to compare projected team capacity with anticipated workload, making it easier to plan ahead.

We built ProCAT using [Django](https://www.djangoproject.com/), the RSE team's preferred framework for creating web applications. One important design choice was selecting which library to use for generating our analytics plots, which forms the subject of today's blogpost.

## Choosing a plotting library

For ProCAT, the requirement was to provide interactive timeseries charts, allowing the user to zoom in, use tooltips, select the time range to be displayed and, in the future, apply filters to select which data to display.

In some of our past projects, we have used [Plotly Dash](https://django-plotly-dash.readthedocs.io/en/latest/index.html#), a framework well-suited to creating interactive, data-driven dashboards. However, based on these experiences, we decided to explore lighter-weight options that could render faster. As this was an internal project, it also provided a convenient opportunity to try out a new library. For this purpose, we chose to use [Bokeh](https://docs.bokeh.org/en/latest/#), a library for creating interactive visualizations powered by JavaScript.

Below, I will summarize our experiences using Bokeh with Django and provide an easy tutorial to begin creating your own visualizations!

## Creating a Bokeh plot

First, we'll run through how to create a basic plot in Bokeh and embed it in your Django application. As described in their [documentation](https://docs.bokeh.org/en/latest/docs/first_steps/first_steps_1.html), Bokeh combines a Python library, where you define your plot and the interactive functionality, and a JavaScript library, BokehJS, which works in the background to display your plots in the browser.

The `bokeh.plotting` module contains all the necessary functions to create your visualization. To add data to  your plot, you need to define your data source. The most common type of data source is the `ColumnDataSource` object; this is automatically created if you pass a list or array to your Bokeh function, but you can also create a `ColumnDataSource` yourself from a dictionary of data or a Pandas dataframe. Here, we use some example data stored in a csv to create a dataframe.

```python
import pandas as pd
from bokeh.models import ColumnDataSource

df = pd.read_csv(csv)
df["dates"] = pd.to_datetime(df["dates"]).dt.date
source = ColumnDataSource(df)
```

The `figure` class creates a Bokeh figure for plotting. We can then plot data; for line charts, this is done using the `line()` glyph method. The code snippet also demonstrates how to define features such as the title, figure dimensions, background colour, axis range and labels.

```python
from bokeh.plotting import figure

plot = figure(
    title="An example Bokeh plot",
    height=500,
    width=800,
    x_axis_type="datetime"
  )
plot.yaxis.axis_label = "Value"
plot.xaxis.axis_label = "Date"

plot_config= {
    "series_A": "green",
    "series_B": "orange",
    "series_C": "blue"
}

for label, colour in plot_config.items():
    plot.line(
        "dates",
        label,
        name=label,
        source=source,
        line_width=2,
        legend_label=label,
        color=colour
    )
```

It is also easy to add some basic interactivity functionality at this point, using the `tools` argument. For example, in the above example, this provides a toolbar attached to the plot that allows the user to save the image, adjust the data displayed on the x-axis and reset the image to the original range displayed. We can easily add some tooltips information using the following:

```python
from bokeh.models import HoverTool

hover = HoverTool()
hover.tooltips = [
    ("Month", "@months"),
    ("Total", "£@values"),
]
plot.add_tools(hover)
```

### Adding a Bokeh plot to your template

Once you have defined your plot, the next step is displaying it within your Django view. This uses the `bokeh.embed` module, which enables embedding of the Bokeh figures into a web page. Specifically, `embed.components` is used to return HTML components that can then be added to the HTML template.

```python
from bokeh.embed import components

script, div = components(plot)
```

This can then be added to Django's context, for use in the template.

```python
import bokeh

# Within your Django view
def get_context_data(self, **kwargs: Any):
    context = super().get_context_data(**kwargs)

    # Get Bokeh figure from relevant function
    plot = make_plot()
    script, div = components(plot)
    context["script"] = script
    context["div"] = div

    # We also need to add the bokeh_version (see below)
    context["bokeh_version"] = bokeh.__version__
    return context
```

It is important to note that, to use these components, the BokehJS resources must be loaded, which can be done by referencing the relevant files from Bokeh's CDN (more information can be found here). This requires specifying the Bokeh version, which we can also add to the Django context to use in the template here.

```HTML
<script src="https://cdn.bokeh.org/bokeh/release/bokeh-{{ bokeh_version }}.min.js"></script>
<script src="https://cdn.bokeh.org/bokeh/release/bokeh-widgets-{{ bokeh_version }}.min.js"></script>
<script src="https://cdn.bokeh.org/bokeh/release/bokeh-tables-{{ bokeh_version }}.min.js"></script>
<script src="https://cdn.bokeh.org/bokeh/release/bokeh-gl-{{ bokeh_version }}.min.js"></script>
<script src="https://cdn.bokeh.org/bokeh/release/bokeh-mathjax-{{ bokeh_version }}.min.js"></script>
```

Following this, you should now have your Bokeh plot rendered in your Django view!

![Example plot](images/bokeh_django/plot.png)

## Widgets

Next, we'll move onto how we can add widgets to our Bokeh plots, which is where Bokeh's powerful interactive functionality comes in.

Here, we'll demonstrates the use of two types of widget to control the date range shown in our timeseries plot: two `DatePicker` widgets, to select the start and end dates for the plots; and two `Button` widgets, to automatically display selected months.

To create the `DatePicker` widgets, we define some text to display, the default date selected and a minimum and maximum date:

```python
from datetime import date
from bokeh.models.widgets import  DatePicker

start_picker = DatePicker(
    title="Start date:",
    value=date(2025, 1, 1),
    min_date=date(2024, 1, 1),
    max_date=date(2026, 1, 31)
)

end_picker = DatePicker(
    title="End date:",
    value=date(2025, 6, 30),
    min_date=date(2024, 1, 1),
    max_date=date(2026, 1, 31)
)
```

To control the behaviour that results when interacting with a widget, we have to attach callbacks to the widgets. If using a standalone Bokeh plot, this is done using [JavaScript callbacks](https://docs.bokeh.org/en/latest/docs/user_guide/interaction/js_callbacks.html), where we can provide some custom JavaScript to run when an event is detected. If running a Bokeh server, this can be done using [Python callbacks](https://docs.bokeh.org/en/latest/docs/user_guide/interaction/python_callbacks.html).

This is a simple example of how to control the plot's x-range using the date pickers:

```python
from bokeh.models import CustomJS

callback = CustomJS(
    args=dict(
        start_picker=start_picker,
        end_picker=end_picker,
        x_range=plot.x_range
    ),
    code="""const start = Date.parse(start_picker.value);
        const end = Date.parse(end_picker.value);
        x_range.start = start
        x_range.end = end""",
)

start_picker.js_on_change("value", callback)
end_picker.js_on_change("value", callback)
```

### Adding widgets to a layout

A useful feature in Bokeh is the ability to create [layouts](https://docs.bokeh.org/en/latest/docs/user_guide/basic/layouts.html), consisting of a grid of components (plots and widgets) making it easier to arrange and size the visualizations in your window. We can also set the `sizing_mode`, controlling how the dimensions of the plot change with the size of the window.

To create a layout with our plot and two widgets:

```python
from bokeh.layouts import column, row

layout = row(
    column(start_picker, end_picker),
    column(plot),
    sizing_mode="stretch_width"
)
```

The layout can then be used to generate the HTML components to be added to the template, as shown above.

![Example plot with widgets](images/bokeh_django/plot_with_widgets.png)

## Ajax data sources (plus more advanced widgets)

Sometimes we may want our plots to be able to update dynamically without having to run a Bokeh server. Bokeh provides a convenient way of doing this using the [`AjaxDataSource`](https://docs.bokeh.org/en/latest/docs/reference/models/sources.html#bokeh.models.AjaxDataSource), a data source that can fetch data from REST endpoints using Ajax requests.

To demonstrate the capabilities of both the `AjaxDataSource` and Bokeh's widgets, the following example shows how we can create separate data sources for each trace, supplied by a new `DataView`, using a new widget to control which we want to display.

First, we create an `AjaxDataSource` for each series, each pointing to a different URL. Each line in the plot is bound to its own data source and is given a polling interval, meaning the data can be dynamically updated. Note we also assign a `name` to each renderer, which we will use later to link to the widget.

```python
 plot = figure(
        title="An example plot with an Ajax datasource",
        height=500,
        width=800,
        x_axis_type="datetime"
    )
    plot.yaxis.axis_label = "Value"
    plot.xaxis.axis_label = "Date"

    for label, colour in plot_config.items():
        source = AjaxDataSource(
            data_url=f"/data/{label}",
            polling_interval=1000,
            method="GET"
        )

        plot.line(
            "dates",
            label,
            name=label,
            source=source,
            line_width=2,
            legend_label=label,
            color=colour
        )
```

We then need to create a new Django view to return the data requested by each data source. The view receives the label from the URL and retrieves the data (a dictionary containing the dates and values) using our `get_series_data` function. This is returned as a `JsonResponse`.

```python
from django.http import HttpRequest, JsonResponse
from django.views.generic import View

class DataView(View):
    """View for returning data to the AjaxDataSource."""

    def get(
        self, request: HttpRequest, label: str, *args: Any, **kwargs: Any
    ) -> JsonResponse:
        """Method to handle GET requests for Ajax data."""
        data = get_series_data(label)  # Get data dictionary
        return JsonResponse(data)
```

The URL has to be added to `urls.py` to expose the endpoint, as follows:

```python
urlpatterns = [
    # ... other URLs
    path("data/<str:label>", views.DataView.as_view(), name="data"),
]
```

Now, we can add a widget to allow users to select which trace(s) should be displayed in the plot. We can use a `CheckboxButtonGroup` to do this, which allows multiple values to be selected simultaneously.

The JavaScript callback in this case reacts to button clicks by by retrieving the indices of the selected label(s) and updating which renderers are visible using the `name` property.

```python
from bokeh.models.widgets.groups import CheckboxButtonGroup

# The active argument determines which options are selected as default
button = CheckboxButtonGroup(labels=labels, active=[0, 1, 2])

callback = CustomJS(
        args=dict(
            button=button,
            plot=plot,
            labels=labels  # This is the list of label names
        ),
        code="""
            const selection = button.active;

            plot.renderers.forEach(renderer => {
                const index = labels.indexOf(renderer.name);
                const visible = selection.includes(index);
                renderer.visible = visible;
            });"""
    )
    button.js_on_event("button_click", callback)
```

Now, we should have a working plot using `AjaxDataSource`s with our `CheckboxButtonGroup` widget.

![Example plot](images/bokeh_django/ajax_plot.png)
