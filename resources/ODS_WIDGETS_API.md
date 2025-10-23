# ODS Widgets - API Reference

**Généré automatiquement depuis:** `ods-widgets.js`

Ce document contient toutes les directives et filtres AngularJS disponibles dans la bibliothèque ODS Widgets.

---

## Directives (90)

### `autoResize`

*Pas de documentation disponible*

### `infiniteScroll`

*Pas de documentation disponible*

### `inject`

*Pas de documentation disponible*

### `odsAdvAnalysis`

@ngdoc directive
@name ods-widgets.directive:odsAdvAnalysis
@scope
@restrict A
@param {string} odsAdvAnalysis This name can be used as the `data` attribute of the display widgets that support it (e.g. `odsAdvTable`).
@param {string} odsAdvAnalysisContext Insert here the name of the context to use.
@param {string} [odsAdvAnalysisSelect] Type here the query to make. More use cases are available below. The documentation about the ODSQL select clause is available here. This clause will contain the values (i.e., the y-axis in case of a chart).
@param {string} [odsAdvAnalysisWhere] This parameter allows to filter rows with a combination of expressions. The documentation about the ODSQL `where` clause is available [here](https://help.opendatasoft.com/apis/ods-explore-v2/#section/Opendatasoft-Query-Language-%28ODSQL%29/Where-clause).
@param {string} [odsAdvAnalysisGroupBy] This parameter helps regroup the calculation according to specific criteria. The `group-by` in this clause can become either y-axis or series in a chart. The documentation about the ODSQL `GROUP BY` clause is available [here](https://help.opendatasoft.com/apis/ods-explore-v2/#section/Opendatasoft-Query-Language-%28ODSQL%29/Group-by-clause).
@param {string} [odsAdvAnalysisOrderBy] This parameter is used to sort the results of an aggregation using the `ASC` and `DESC` keywords (e.g., `myField ASC` or ). The documentation about the ODSQL `ORDER BY` clause is available [here](https://help.opendatasoft.com/apis/ods-explore-v2/#section/Opendatasoft-Query-Language-%28ODSQL%29/Order-by-clause).
@param {string} [odsAdvAnalysisLimit] Limits the number of items to return.
@description
The odsAdvAnalysis widget exposes the results of an aggregation function over a context.
It uses the ODS Explore API V2.1 and its [ODSQL language](https://help.opendatasoft.com/apis/ods-explore-v2/#section/Opendatasoft-Query-Language-%28ODSQL%29), which offers greater flexibility than the v1.
The parameters for this widgets are dynamic, which implies two benefits:
- First, changes in context parameters will refresh the results of the widget.
- Second, AngularJS variables are accepted as attributes.
The results can then be displayed in three different ways:
- To create specific visualizations, using custom-made HTML and CSS
- A table view is also available using `odsAdvTable` (examples are provided below).
- As the widget is creating an AngularJS variable, it can be displayed through a simple `{{myData.results[X]}}`. This usage is not documented here, as it regards HTML code and widgets already documented in [the introduction](https://help.opendatasoft.com/widgets/#/introduction/).
For retro-compatibility purposes, similarly to API V2.0, if the `groupBy` is done on a field that contains null values, they will be removed. If you are
using the `limit` parameter, this may cause the widget to return one less category as expected, because the null group was included. You can
prevent this by using `where` to exclude null values from this field, using `IS NOT NULL` (e.g. `my_field IS NOT NULL`).
<h2>Examples of requests to make</h2>
How to compute a weighted average:
In this example, the widget will return the average height of the trees according to the population size of each species in Paris districts.
<pre>
<ods-dataset-context
context="ctx"
ctx-domain="https://documentation-resources.opendatasoft.com/"
ctx-dataset="les-arbres-remarquables-de-paris">
<div ods-adv-analysis="myData"
ods-adv-analysis-context="ctx"
ods-adv-analysis-select="(sum(hauteur_en_m)/count(espece)) as y_axis"
ods-adv-analysis-where="arrondissement LIKE 'paris'"
ods-adv-analysis-group-by="espece as x_axis">
{{myData}}
</div>
</ods-dataset-context>
</pre>
How to create multiple time series:
In this example, the widget returns the average gold price by month in 2018 and 2019. The `group-by` year allows to compare each year with the others.
<pre>
<ods-dataset-context
context="ctx"
ctx-domain="https://documentation-resources.opendatasoft.com/"
ctx-dataset="gold-prices">
<div ods-adv-analysis="myData"
ods-adv-analysis-context="ctx"
ods-adv-analysis-select="avg(price) as y_axis"
ods-adv-analysis-where="date > date'2017'"
ods-adv-analysis-group-by="month(date) as x_axis, year(date) as series">
{{myData}}
</div>
</ods-dataset-context>
</pre>
<h2>How to use odsAdvancedAnalysis with odsAdvTable</h2>
<b>odsAdvancedTable</b> was designed to accept the JSON created by <b>odsAdvancedAnalysis</b>.
Its purpose is to offer a table view that matches the widget and to provide an accessible way of displaying data as an alternative to charts.
For more information, see {@link ods-widgets.directive:odsAdvTable the documentation for odsAdvTable}.
<pre>
<ods-dataset-context
context="ctx"
ctx-domain="https://documentation-resources.opendatasoft.com/"
ctx-dataset="les-arbres-remarquables-de-paris">
<div ods-adv-analysis="myData"
ods-adv-analysis-context="ctx"
ods-adv-analysis-select="count(objectid) as quantite_arbres, AVG(circonference_en_cm) as circonference_moyenne"
ods-adv-analysis-group-by="arrondissement">
<ods-adv-table
data="myData"
sticky-header="true"
sticky-first-column="true"
columns-order="['arrondissement', 'quantite_arbres', 'circonference_moyenne']"
totals="['quantite_arbres']"
sort="arrondissement ASC">
</ods-adv-table>
</div>
</ods-dataset-context>
</pre>

### `odsAdvResults`

*Pas de documentation disponible*

### `odsAdvTable`

@ngdoc directive
@name ods-widgets.directive:odsAdvTable
@scope
@restrict E
@param {array} data The input array of value which feeds the table.
@param {array} [columnsOrder] An array of strings representing the columns' order.
@param {object} [columnsOptions] An object representing the formatting to apply on the columns. Two options are available: `label` is used to rename the column's header and `decimals` to set the number of decimals on each cell of the column (e.g., `{ label: 'New name', decimals: 2 }`).
@param {string} [sort] Name of the column to sort on, following by the suffix `ASC` or `DESC` (e.g., `columnName ASC`).
@param {array} [totals] An array of strings containing the names of the columns whose totals must be calculated.
@param {boolean} [stickyHeader=false] When set to `true`, the header will be fixed at the top of the table.
@param {boolean} [stickyFirstColumn=false] When set to `true`, the first column will be fixed on the left side of the table.
@description
The odsAdvTable widget is used to analyze data from a table perspective.
It is especially interesting to use this widget in conjunction with an odsAdvAnalysis widget, but you can feed it with static data.
The odsAdvTable widget gives you the ability to:
- compute totals,
- sort, reorder and rename columns,
- format numbers as text and define the number of decimal places to round the number to, and
- set the header and/or the first column in a fixed position.
@example
<example module="ods-widgets">
<file name="a_simple_example_with_static_data.html">
<div ng-init="pets = [{ species: 'dog', name: 'Rex'}, { species: 'cat', name: 'Felix'}, { species: 'Mouse', name: 'Pikachu'}, { species: 'Owl', name: 'Hedwig'}]">
<ods-adv-table
data="pets"
columns-order="['species', 'name']">
</ods-adv-table>
</div>
</file>
</example>
<example module="ods-widgets">
<file name="an_example_using_odsAdvAnalysis.html">
<ods-dataset-context
context="ctx"
ctx-domain="https://documentation-resources.opendatasoft.com/"
ctx-dataset="les-arbres-remarquables-de-paris">
<div ods-adv-analysis="data"
ods-adv-analysis-context="ctx"
ods-adv-analysis-select="count(objectid) as count"
ods-adv-analysis-where="arrondissement like 'PARIS'"
ods-adv-analysis-group-by="espece, genre, arrondissement, hauteur_en_m">
<ods-adv-table
data="data"
sort="espece ASC"
totals="['count']"
columns-order="['espece', 'count', 'genre', 'arrondissement', 'hauteur_en_m']"
columns-options="{
espece: {
label: 'The species',
},
count: {
decimals: 0,
label: '#',
},
genre: {
label: 'The genus',
},
arrondissement: {
label: 'The district',
},
hauteur_en_m: {
decimals: 2,
label: 'The height (in meters)',
},
}"
sticky-header="true"
sticky-first-column="true">
</ods-adv-table>
</div>
</ods-dataset-context>
</file>
</example>

### `odsAggregation`

@ngdoc directive
@name ods-widgets.directive:odsAggregation
@scope
@restrict A
@param {string} [odsAggregation=aggregation] <i>(mandatory)</i> Name of the variable that holds the result of the aggregation. For multiple aggregations, variable names must be separated with commas.
@param {DatasetContext} odsAggregationContext <i>(mandatory)</i> {@link ods-widgets.directive:odsDatasetContext Dataset Context} to use.
@param {DatasetContext} odsAggregation[Variablename]Context Context specific to the <code>[Variablename]</code> aggregation. `[Variablename]` must be replaced with the name of the variable, declared through the **odsAggregation** parameter.
@param {string} [odsAggregationFunction=COUNT] <i>(mandatory)</i> Aggregation function to apply:
- AVG: average
- COUNT
- MIN: minimum
- MAX: maximum
- STDDEV: standard deviation
- SUM
@param {string} [odsAggregation[Variablename]Function=COUNT] Function specific to the <code>[Variablename]</code> aggregation. `[Variablename]` must be replaced with the name of the variable, declared through the **odsAggregation** parameter.
@param {string} [odsAggregationExpression=none] <i>(optional only with the COUNT function)</i> Expression to apply the function on (e.g., the name of a field).
@param {string} [odsAggregation[Variablename]Expression=none] Expression specific to the <code>[Variablename]</code> aggregation. `[Variablename]` must be replaced with the name of the variable, declared through the **odsAggregation** parameter.
@description
The odsAggregation widget creates a variable that contains the result of an aggregation function based on a context.
Aggregations are functions that enable to group records and compute statistical values for numeric fields. For instance, aggregations can determine, based on several records, the smallest or biggest value among them, compute the average value or count the number of values for a chosen field.
odsAggregation supports multiple declarations of aggregations.
@example
<example module="ods-widgets">
<file name="simple_aggregation.html">
<ods-dataset-context context="tree"
tree-dataset="les-arbres-remarquables-de-paris"
tree-domain="https://documentation-resources.opendatasoft.com/">
<div ods-aggregation="height"
ods-aggregation-context="tree"
ods-aggregation-expression="hauteur_en_m"
ods-aggregation-function="AVG">
Average height is {{ height | number }} meters.
</div>
</ods-dataset-context>
</file>
</example>
<example module="ods-widgets">
<file name="multiple_aggregations.html">
<ods-dataset-context context="commute,demographics"
commute-dataset="average-commute-time-by-county"
commute-domain="https://documentation-resources.opendatasoft.com/"
demographics-dataset="us-cities-demographics"
demographics-domain="https://documentation-resources.opendatasoft.com/">
<div ods-aggregation="people, time"
ods-aggregation-people-context="demographics"
ods-aggregation-people-function="SUM"
ods-aggregation-people-expression="count"
ods-aggregation-time-context="commute"
ods-aggregation-time-function="AVG"
ods-aggregation-time-expression="commuting_time">
For {{ people }} people in the US, the average commute time in 2015 was {{ time|number:0 }} minutes.
</div>
</ods-dataset-context>
</file>
</example>
<example module="ods-widgets">
<file name="multiple_aggregations_same_context.html">
<ods-dataset-context context="tree"
tree-dataset="les-arbres-remarquables-de-paris"
tree-domain="https://documentation-resources.opendatasoft.com/">
<div ods-aggregation="total, mingirth, maxgirth"
ods-aggregation-context="tree"
ods-aggregation-total-function="COUNT"
ods-aggregation-maxgirth-expression="circonference_en_cm"
ods-aggregation-maxgirth-function="MAX"
ods-aggregation-mingirth-expression="circonference_en_cm"
ods-aggregation-mingirth-function="MIN">
There are {{ total }} remarkable trees in Paris, with girth ranging from {{ mingirth }} to {{ maxgirth }} cm.
</div>
</ods-dataset-context>
</file>
</example>

### `odsAnalysis`

@ngdoc directive
@name ods-widgets.directive:odsAnalysis
@scope
@restrict A
@param {string} [odsAnalysis=analysis] <i>(mandatory)</i> Name of the variable
@param {DatasetContext} odsAnalysisContext <i>(mandatory)</i> {@link ods-widgets.directive:odsDatasetContext Dataset Context} to use.
@param {number} [odsAnalysisMax=all] Maximum number of results to show.
@param {string} [odsAnalysisX=none] Name of the field used as X-axis (e.g., date or datetime field, facet, etc.)
@param {string} [odsAnalysisSort=none] Name of the series to sort on.
Note that `-` before the name of the series indicates that the sorting will be descending instead of ascending (e.g., `-serieName`).
@param {string} [odsAnalysisSerieName=none] Function to apply:
- AVG: average
- COUNT
- MIN: minimum
- MAX: maximum
- STDDEV: standard deviation
- SUM
Must be written in the following form: `FUNCTION(fieldname)`.
@description
The odsAnalysis widget creates a variable that contains the result of an analysis (i.e., an object containing a results array and optionally an aggregations object).
odsAnalysis allows applying functions to chosen groups of data to analyze them with the same logic as a chart visualization. For instance, an analysis can obtain the average value for several data series, broken down by a chosen field used as an X-axis. The result can then be sorted by another series.
odsAnalysis can be used with the AngularJS ngRepeat directive to build a table of analysis results.
@example
<example module="ods-widgets">
<file name="index.html">
<ods-dataset-context context="tree"
tree-dataset="les-arbres-remarquables-de-paris"
tree-domain="https://documentation-resources.opendatasoft.com/">
<table class="table table-bordered table-condensed table-striped">
<thead>
<tr>
<th>Tree name</th>
<th>Height</th>
<th>Girth</th>
</tr>
</thead>
<tbody>
<tr ods-analysis="analysis"
ods-analysis-context="tree"
ods-analysis-max="10"
ods-analysis-x="espece"
ods-analysis-sort="girth"
ods-analysis-serie-height="AVG(hauteur_en_m)"
ods-analysis-serie-height-cumulative="false"
ods-analysis-serie-girth="AVG(circonference_en_cm)"
ng-repeat="result in analysis.results">
<td>{{ result.x }}</td>
<td>{{ result.height|number:2 }}</td>
<td>{{ result.girth|number:2 }}</td>
</tr>
</tbody>
</table>
</ods-dataset-context>
</file>
</example>

### `odsAnalysisSerie`

@deprecated
@name ods-widgets.directive:odsAnalysisSerie
@scope
@restrict A
@param {string} odsAnalysisSerie Analysis results
@param {string} odsAnalysisSerieCondition The condition to that the value must validate to be part of the serie. 'y' will be replaced by the value
@param {string} odsAnalysisSerieName name of the serie to check for validation
@param {string} odsAnalysisSerieSeparateOnX name of the x axis in the analysis response used to split series
@param {string} odsAnalysisSerieMode if mode is set to "reduce", keep only the longest serie of all splited series. Requires separate-on-x parameter.
@description
This widget exposes only keeps the longest serie in the results from an analysis.
Results can be used as if coming from an analysis widget (and use a subaggregation on it for example)
@example
<example module="ods-widgets">
<file name="index.html">
<ods-dataset-context context="tree" tree-dataset="les-arbres-remarquables-de-paris" tree-domain="https://documentation-resources.opendatasoft.com/">
<div
ods-analysis="analysis"
ods-analysis-context="tree"
ods-analysis-max="10"
ods-analysis-x="genre"
ods-analysis-x="espece"
ods-analysis-sort="circonference"
ods-analysis-serie-hauteur="AVG(hauteur_en_m)"
ods-analysis-serie-hauteur-cumulative="false"
ods-analysis-serie-circonference="AVG(circonference_en_cm)">
<div
ods-analysis-serie="analysis.results"
ods-analysis-serie-condition="y > 20"
ods-analysis-serie-name="hauteur"
ods-analysis-serie-separate-on-x="genre">
{{ results }}
</div>
</div>
</ods-dataset-context>
</file>
</example>
reduce the results to the longest serie
@example
<example module="ods-widgets">
<file name="index.html">
<ods-dataset-context context="tree"
tree-dataset="les-arbres-remarquables-de-paris"
tree-domain="https://documentation-resources.opendatasoft.com/">
<div ods-analysis="analysis"
ods-analysis-context="tree"
ods-analysis-max="10"
ods-analysis-x-genre="genre"
ods-analysis-x-espece="espece"
ods-analysis-sort="circonference"
ods-analysis-serie-hauteur="AVG(hauteur_en_m)"
ods-analysis-serie-hauteur-cumulative="false"
ods-analysis-serie-circonference="AVG(circonference_en_cm)">
<div ods-analysis-serie="analysis.results"
ods-analysis-serie-condition="y > 20"
ods-analysis-serie-name="hauteur"
ods-analysis-serie-separate-on-x="genre"
ods-analysis-serie-mode="reduce">
Longest serie: {{ results.global.length }}
</div>
</div>
</ods-dataset-context>
</file>
</example>

### `odsAnalyze`

Created by manu on 20/10/15.

### `odsAutoResize`

*Pas de documentation disponible*

### `odsCalendar`

*Pas de documentation disponible*

### `odsCalendarTooltip`

*Pas de documentation disponible*

### `odsCatalogContext`

@ngdoc directive
@name ods-widgets.directive:odsCatalogContext
@scope
@restrict AE
@param {string} context <i>(mandatory)</i> Name, or list of names separated by commas, of context(s) to declare. Context names must be in lowercase, can only contain alphanumerical characters, and cannot begin with a number, "data", or "x".
@param {string} [domain=ODSWidgetsConfig.defaultDomain] Domain where the dataset(s) can be found. Since the domain value is used to construct an URL to an API root, it can be:
- an alphanumeric string (e.g., *mydomain*): it will assume that it is an Opendatasoft domain (e.g., *mydomain.opendatasoft.com*)
- a hostname (e.g., *data.mydomain.com*)
- a relative path (e.g., _/monitoring_): it will be relative to the hostname of the current page
- a hostname and a path (e.g., *data.mydomain.com/monitoring*)
By default, if the domain parameter is not set, {@link ods-widgets.ODSWidgetsConfigProvider ODSWidgetsConfig.defaultDomain} is used.
@param {string} [apikey=none] API key to use in every API call for the context. For more information, see {@link https://user-guide.opendatasoft.com/en/articles/2044226 Generating an API key}).
@param {object} [parameters=none] Object holding parameters to apply to the context when it is created
@param {boolean} [urlSync=none] Enables synchronization of the parameters to the page's parameters (query string). When sharing the page with parameters in the URL, the context will use them; and if the context parameters change, the URL parameters will change as well. Note that if this parameter is enabled, `parameters` and `parametersFromContext` won't have any effect. There can also only be a single context with URL synchronization enabled, else the behavior will be unpredictable.
@description
The odsCatalogContext widget represents the entire catalog of datasets of a chosen domain and a set of parameters used to query this catalog. One or more widgets can use a catalog context: it allows them to share information (i.e., the query parameters).
For example, a widget that displays a time filter ({@link ods-widgets.directive:odsTimerange odsTimerange}) can be plugged into the same context as a results list ({@link ods-widgets.directive:odsResultEnumerator odsResultEnumerator}) so that the user can filter the displayed results.
odsCatalogContext creates a new child scope, within which its declared contexts are available for any other widget used inside that odsCatalogContext element. odsCatalogContext widgets can also be nested inside each other.
A single odsCatalogContext can declare one or several contexts, which are initialized when declared through the **context** parameter. Each context is configured using parameters prefixed by the context name (`contextname-setting`, e.g., mycontext-domain).
<b>Properties of odsCatalogContext used as variable</b>
Once created, the context is accessible as a variable named after it. The context contains properties that can be accessed directly:
* `domainUrl`: full URL of the domain of the context that can be used to create links
* `parameters`: parameters object of the context
@example
<example module="ods-widgets">
<file name="simple_example.html">
<ods-catalog-context context="examples"
examples-domain="https://documentation-resources.opendatasoft.com/">
<ods-most-popular-datasets context="examples"></ods-most-popular-datasets>
</ods-catalog-context>
</file>
</example>
<example module="ods-widgets">
<file name="odsresultenumerator_with_catalog_context.html">
<ods-catalog-context context="examples"
examples-domain="https://documentation-resources.opendatasoft.com/">
<ul>
<ods-result-enumerator context="examples">
<li>
<a ng-href="{{context.domainUrl + '/explore/dataset/' + item.datasetid + '/'}}" target="_blank">{{item.datasetid}}</a>
</li>
</ods-result-enumerator>
</ul>
</ods-catalog-context>
</file>
</example>

### `odsChart`

@ngdoc directive
@name ods-widgets.directive:odsChart
@restrict E
@scope
@param {string} [timescale=none] Works only with timeseries. If it defines the default timescale to use to display the X-axis. It does not affect how the different series are requested (they have their own timescale) but enforces X-axis intervals.
@param {string} [labelX=none] If set, it overrides the default X-axis label. The default label is generated from the series.
@param {boolean} [singleYAxis=false] Enforces the use of only one Y-axis for all series. In this case, specific Y-axis parameters defined for each series will be ignored.
@param {string} singleYAxisLabel Sets the label for the single Y-axis.
@param {integer} [min=null] Sets the min displayed value for Y-axis. Active only when singleYAxis is true.
@param {integer} [max=null] Sets the max displayed value for Y-axis. Active only when singleYAxis is true.
@param {integer} [step=null] Specifies the step between each tick on the Y axis. If not defined, it is computed automatically. Active only when singleYAxis is true.
@param {boolean} [scientificDisplay=true] When set to false, force the full display of the numbers on the Y-axis. Active only when singleYAxis is true.
@param {boolean} [logarithmic=false] Uses a logarithmic scale for Y-axis. Active only when singleYAxis is true.
@param {boolean} [displayLegend=true] Enables or disables the display of series legend. Active only when singleYAxis is true.
@param {boolean} [alignMonth=true] Aligns the month values with the month label. The old behavior aligns values with the middle of the month. Setting this parameter to false reverts to the old behavior.
@param {integer} [labelsXLength=12] Sets the maximum number of characters displayed for the X-axis labels.
@description
The odsChart widget is the base widget allowing to display charts from Opendatasoft datasets.
A Chart is defined by one or more series that get their data from form one or more datasets represented by a {@link ods-widgets.directive:odsDatasetContext Dataset Context},
a type of chart, and multiple parameters to fine-tune the chart's appearance.
Note: `min` and `max` parameters are dynamic, which means that if they change, the chart will be refreshed accordingly.
Basic example:
<pre>
<ods-dataset-context context="trees"
trees-dataset="les-arbres-remarquables-de-paris"
trees-domain="documentation-resources">
<ods-chart>
<ods-chart-query context="trees" field-x="espece" maxpoints="10">
<ods-chart-serie expression-y="circonference_en_cm" chart-type="column" function-y="MAX" color="#66c2a5">
</ods-chart-serie>
</ods-chart-query>
</ods-chart>
</ods-dataset-context>
</pre>
You can display multiple series from the same dataset on the same chart:
<pre>
<ods-dataset-context context="trees"
trees-dataset="les-arbres-remarquables-de-paris"
trees-domain="documentation-resources">
<ods-chart>
<ods-chart-query context="trees" field-x="espece" maxpoints="10">
<ods-chart-serie expression-y="circonference_en_cm" chart-type="column" function-y="AVG" color="#66c2a5">
</ods-chart-serie>
<ods-chart-serie expression-y="hauteur_en_m" chart-type="column" function-y="AVG" color="#fc8d62">
</ods-chart-serie>
</ods-chart-query>
</ods-chart>
</ods-dataset-context>
</pre>
You can display multiple series from multiple datasets on the same chart:
<pre>
<ods-dataset-context context="commute,demographics"
commute-dataset="commute-time-us-counties"
commute-domain="https://documentation-resources.opendatasoft.com/"
demographics-dataset="us-cities-demographics"
demographics-domain="https://documentation-resources.opendatasoft.com/">
<ods-chart align-month="true">
<ods-chart-query context="commute" field-x="state" maxpoints="20">
<ods-chart-serie expression-y="mean_commuting_time" chart-type="column" function-y="AVG" color="#66c2a5" scientific-display="true">
</ods-chart-serie>
</ods-chart-query>
<ods-chart-query context="demographics" field-x="state" maxpoints="20">
<ods-chart-serie expression-y="count" chart-type="column" function-y="SUM" color="#fc8d62" scientific-display="true">
</ods-chart-serie>
</ods-chart-query>
</ods-chart>
</ods-dataset-context>
</pre>

### `odsChartQuery`

@ngdoc directive
@name ods-widgets.directive:odsChartQuery
@restrict E
@scope
@param {string} fieldX Sets the field that is used to compute the aggregations during the analysis query.
@param {string} [timescale="year"] Works only with timeseries (when fieldX is a date or datetime). Y values will be computed against this interval. For example, if you have daily values in a dataset and ask for a "month" timescale, the Y values for the {@link ods-widgets.directive:odsChartSerie series} inside this query will aggregated month by month and computed.
@param {integer} [maxpoints=50] Defines the maximum number of points fetched by the query. With a value of 0, all points will be fetched by the query.
@param {string} [stacked=null] Stacks the resulting charts. Stacked values can 'normal' or 'percent'. Only works with columns, bar, line, spline, area, and spline area charts.
@param {boolean} [reverseStacks=false] Reverses the order of the displayed stack. Only works with stacked charts when the singleYAxis option is not active on the chart.
@param {string} [seriesBreakdown=none] When declared, all series are broken down by the defined facet.
@param {string} [seriesBreakdownTimescale=true] If the breakdown facet is a time serie (date or datetime), it defines the aggregation level for this facet.
@param {object} [categoryColors={}] A object containing a color for each category name. For example: {'my value': '#FF0000', 'my other value': '#0000FF'}
@param {string} [sort=none] Displays the results in a specific order. The following values are available:
- To sort based on horizontal axis, use `x` or `-x`. For date-based axes, you need to include the name of
the field, and the highest precision in the displayed data. For example, if the field name is `mydate`, and
the data includes the year, you can use `x.mydate.year`.
- To sort based on the displayed values, use `y` or `-y` if there is a single serie. If there are multiple
series, use `series{n}-{m}` where `n` is the {n}th `ods-chart-query`, and m is the {m}th `ods-chart-serie`
@description
The odsChartQuery widget is the sub widget that defines the queries for the series defined inside.
For complete examples, see {@link ods-widgets.directive:odsChart odsChart}.
Note: All parameters are dynamic, which means that if they change, the chart will be refreshed accordingly.

### `odsChartSerie`

@ngdoc directive
@name ods-widgets.directive:odsChartSerie
@restrict E
@scope
@param {string} [chartType] Available types are: 'line', 'spline', 'arearange', 'areasplinerange', 'columnrange', 'area', 'areaspline', 'column', 'bar', 'pie', 'scatter'
@param {string} [functionY] Sets up the function that will be used to calculate aggregation value. 'COUNT' counts the number of documents for each category defined by expressionY.
@param {string} [expressionY] Sets up the facet used for aggregation
@param {string} [color] Defines the color used for this serie. see colors below
@param {string} [labelY] Specifies a custom label for the series
@param {string} [labelsposition='outside'] Specifies the position of labels. The authorized values are 'inside' or 'outside' (for pie charts only).
@param {number} [innersize=0] This parameter can be used to change a pie chart into a donut by creating a hole in the center. The value is expressed in pixels.
@param {boolean} [cumulative] Y values are accumulated
@param {boolean} [logarithmic=false] Displays the serie using a logarithmic scale
@param {integer} [min=null] Minimum value to be displayed on the Y-axis. If not defined, it is computed automatically.
@param {integer} [max=null] Maximum value to be displayed on the Y axis. If not defined, it is computed automatically.
@param {integer} [step=null] Specifies the step between each tick on the Y-axis. If not defined, it is computed automatically.
@param {integer} [index=null] Forces the display order of the serie. The higher is on top, the lower is below (starts from 1).
@param {boolean} [scientificDisplay=true] When set to false, force the full display of the numbers on the Y-axis.
@param {boolean} [displayUnits] Enables the display of the units defined for the field in the tooltip
@param {boolean} [displayValues] Enables the display of each invidual values in stacks
@param {boolean} [displayStackValues] Enables the display of the cumulated values on top of stacks
@param {number} [multiplier] Multiplies all values for this serie by the defined number
@param {string} [colorThresholds] An array of (value, color) objects. For each threshold value, if the Y value is above the threshold, the defined color is used. The format for this parameter is color-thresholds="[{'value': 5, 'color': '#00ff00'},{'value': 10, 'color': '#ffff00'}]"
@param {string} [subsets] Used when functionY is set to 'QUANTILES' to define the wanted quantile
@param {boolean} [subseries] An array of subseries. They are used for range, columnrange, and boxplot charts. Each item of the array contains an object like: {"func": "AVG", "yAxis": "myfield"}
@param {string} [refineOnClickContext] Context name or array of contexts name on which to refine when the series is clicked on. It won't work properly if the fieldX attribute of the parent odsChartQuery is a date or datetime field and if the associated timescale is not one of 'year', 'month', 'day', 'hour', 'minute'.
@param {string} [refineOnClick[context]ContextField] Name of the field that will be refined for each context
@description
The odsChartSerie widget is the sub widget that defines a series in the chart with all its parameters.
For complete examples, see {@link ods-widgets.directive:odsChart odsChart}.
# Available chart types:
There are two available types of charts: simple series and areas that take a minimal and a maximal value.
## Simple series
- line
- spline
- area
- areaspline
- column
- bar
- pie
- scatter
- polar
- spiderweb
- funnel
## Areas
- arearange
- areasplinerange
- columnrange
# Available functions
- COUNT
- AVG
- MIN
- MAX
- STDDEV
- SUM
- QUANTILES
- CONSTANT

### `odsClearAllFilters`

@ngdoc directive
@name ods-widgets.directive:odsClearAllFilters
@scope
@restrict E
@param {CatalogContext|DatasetContext|CatalogContext[]|DatasetContext[]} context
{@link ods-widgets.directive:odsCatalogContext Catalog Context} or
{@link ods-widgets.directive:odsDatasetContext Dataset Context} to display the filters of, or a list of
contexts
@param {String[]} except An array of parameters to exclude from the clearing operation
@description
The odsClearAllFilters widget displays a button that will clear all active filters in the given context.

### `odsColorGradient`

@ngdoc directive
@name ods-widgets.directive:odsColorGradient
@scope
@restrict A
@param {string} odsColorGradient Variable name to use to output the color gradient data structure. variable['colors'] can be used in ods-maps. 'values', 'range' keys are also available.
@param {DatasetContext} odsColorGradientContext {@link ods-widgets.directive:odsDatasetContext Dataset Context} to use
@param {string} odsColorGradientX The X-axis of the analysis
@param {string} odsColorGradientSerie FUNC(expression) where FUNC is AVG, SUM, MIN, MAX, etc... and expression is the field id to work on.
@param {string} [odsColorGradientHigh='rgb(0, 55, 237)'] RGB or HEX color code for highest value of the analysis serie. ex: "rgb(255, 0, 0)", "#abc"
@param {string} [odsColorGradientLow='rgb(180, 197, 241)'] RGB or HEX color code for the lowest value of the analysis serie. ex: "rgb(125, 125, 125)", "#ff009a"
@param {integer} [odsColorGradientNbClasses=undefined] Number of classes, ie number of color to compute. Mandatory to get a consistent legend with the corresponding number of grades/classes.
@param {integer} [odsColorGradientPowExponent=undefined] Set to `1` for a linear scale (default value), to `0.3` to approximate a logarithmic scale. Power scale tends to look like a log scale when the exponent is less than `1` and tends to an exponential scale when bigger than `1`.
@description
The odsColorGradient widget exposes the results of an analysis transposed to a set of colors for each X value.
The results are available in the scope.
This widget can be used directly on the odsMap's `color-categories` parameter with the `display=categories` mode.
It can also be used on the AngularJS ngRepeat directive to build custom scales.
@example
<example module="ods-widgets">
<file name="logarithmic-scale.html">
<ods-dataset-context context="regions,population"
regions-dataset="regions-et-collectivites-doutre-mer-france"
regions-domain="https://documentation-resources.opendatasoft.com/"
regions-parameters="{'q':'NOT (guadeloupe OR mayotte OR guyane OR martinique OR reunion)', 'disjunctive.reg_name':true}"
population-dataset="populations-legales-communes-et-arrondissements-municipaux-france"
population-parameters="{'disjunctive.reg_name':true}"
population-domain="https://documentation-resources.opendatasoft.com/">
<div ods-color-gradient="colorgradient"
ods-color-gradient-context="population"
ods-color-gradient-x="reg_name"
ods-color-gradient-serie="SUM(com_arm_pop_tot)"
ods-color-gradient-high="rgb(20, 33, 96)"
ods-color-gradient-low="rgb(180, 197, 241)">
<ods-map location="5,46.50595,3.40576">
<ods-map-layer context="regions"
color-categories="colorgradient['colors']"
color-by-field="reg_name"
color-categories-other="lightgrey"
display="categories"
shape-opacity="0.85"
title="Sum of cities population">
</ods-map-layer>
</ods-map>
</div>
</ods-dataset-context>
</file>
</example>

### `odsCrossTable`

@ngdoc directive
@name ods-widgets.directive:odsCrossTable
@scope
@restrict E
@param {DatasetContext} context {@link ods-widgets.directive:odsDatasetContext Dataset Context} from which data is
extracted
@param {string} rows A comma-separated list of field names which will be used for row headers' values. These fields must all be facets.
@param {string} column Name of the field which will be used for column header's values. This field must be a facet.
@param {string} serieXxxLabel Label of the series, which will be displayed as column header (Xxx being the
name of the series).
@param {string} serieXxxFunc Function (SUM, AVG, COUNT, etc.) used to aggregate the series analysis (Xxx
being the name of the series)
@param {string} serieXxxExpr Name of the field used for the series analysis (Xxx being the name of the
series)
@param {boolean} [repeatRowHeaders=false] Controls whether to repeat the row headers on each line or not.
@param {boolean} [displayIntermediaryResults=false] Controls whether to display intermediary subtotals, subaverages, etc.
@param {integer} [numberPrecision=3] The number of decimals to display for numeric values
@description
The odsCrossTable widget creates a cross table from a context.
It supports multiple aggregations for a single column field and multiple row fields.
@example
<example module="ods-widgets">
<file name="index.html">
<ods-dataset-context context="trees"
trees-dataset="les-arbres-remarquables-de-paris"
trees-domain="https://documentation-resources.opendatasoft.com/">
<ods-cross-table context="trees"
rows="arrondissement"
column="espece"
serie-height-label="Average height"
serie-height-func="AVG"
serie-height-expr="hauteur_en_m"></ods-cross-table>
</ods-dataset-context>
</file>
</example>

### `odsDatasetContext`

*Pas de documentation disponible*

### `odsDatasetSchema`

@ngdoc directive
@name ods-widgets.directive:odsDatasetSchema
@restrict E
@scope
@description
The odsDatasetSchema widget displays a table describing the schema of a dataset. For each field, it provides the label, name,
description, type, and an example.
@param {DatasetContext} context {@link ods-widgets.directive:odsDatasetContext Dataset Context}
@example
<example module="ods-widgets">
<file name="index.html">
<ods-dataset-context context="tree"
tree-dataset="les-arbres-remarquables-de-paris"
tree-domain="https://documentation-resources.opendatasoft.com/">
<ods-dataset-schema context="tree"></ods-dataset-schema>
</ods-dataset-context>
</file>
</example>

### `odsDateRangeSlider`

@ngdoc directive
@name ods-widgets.directive:odsDateRangeSlider
@restrict E
@scope
@param {DatasetContext|DatasetContext[]} context {@link ods-widgets.directive:odsDatasetContext Dataset Context} or array of context to use
@param {string} [initialFrom=none] Default date for the "from" field: "yesterday", "now", or a string representing a date
@param {string} [initialTo=none] Default date for the "to" field: "yesterday", "now", or a string representing a date
@param {expression} [startBound=none] Beginning bound of the range slider, it will define the minimum selectable from "yesterday", "now", or a string representing a date. As an AngularJS expression is expected, no need to use {{}} syntax for variables or expressions, and if you want to provide a static string value, surround it with simple quotes.
@param {expression} [endBound=none] End bound of the range slider, it will define the maximum selectable to "yesterday", "now", or a string representing a date. As an AngularJS expression is expected, no need to use {{}} syntax for variables or expressions, and if you want to provide a static string value, surround it with simple quotes.
@param {string} [dateFormat='YYYY-MM-DD'] Defines the format to render the two bounds and the selection.
@param {string} [dateField=none] Date field to query on. If no field is provided, the first date type field of the dataset is used.
@param {string} [precision='day'] Defines the precision, 'day', 'month' or 'year', default is 'day'
@param {string} [suffix=none] Context parameter query suffix. Used to avoid collision with other widget queries.
@param {string} [to=none] Sets a variable that will get the iso formatted value of the first input
@param {string} [from=none] Sets a variable that will get the iso formatted value of the second input
@description
The odsDateRangeSlider widget displays a range slider to select the two bounds of a date range.
@example
<example module="ods-widgets">
<file name="index.html">
<ods-dataset-context context="ctx"
ctx-dataset="product-release-notes"
ctx-domain="documentation-resources">
<div class="ods-box" ng-init="obj = {}">
<ods-date-range-slider context="ctx"
date-format="YYYY"
precision="year"
initial-from="2019/01/01"
initial-to="2019/12/31"
start-bound="'2010/01/01'"
end-bound="'2020/03/30'"
from="obj.from"
to="obj.to">
</ods-date-range-slider>
<br/>
<p>
{{ obj.from }} -- {{ obj.to }}
</p>
</div>
<ods-table context="ctx"></ods-table>
</ods-dataset-context>
</file>
</example>

### `odsDatetime`

@ngdoc directive
@name ods-widgets.directive:odsDatetime
@restrict A
@scope
@description
The odsDatetime widget gets the ISO local datetime and stores it into a variable (into the scope).
It is an equivalent to moment().format() javascript call.
The current scope gains a refreshDatetime method that will refresh the variable with the current datetime.
@example
<example module="ods-widgets">
<file name="index.html">
<ANY ods-datetime="datetime">
{{ datetime|moment:'YYYY-MM-DD HH:mm:ss' }}
</ANY>
</file>
</example>

### `odsDisqus`

@ngdoc directive
@name ods-widgets.directive:odsDisqus
@restrict E
@scope
@param {string} shortname Disqus short name for your account. If not specified, {@link ods-widgets.ODSWidgetsConfigProvider ODSWidgetsConfig.disqusShortname} will be used.
@param {string} [identifier=none] By default, the discussion is tied to the URL of the page. If you want to be independent from the URL or share the discussion between two or more pages, you can define an identifier in this parameter. Disqus recommends always doing this from the start.
@description
The odsDisqus widget shows a Disqus panel where users can comment on the page.

### `odsDomainStatistics`

@ngdoc directive
@name ods-widgets.directive:odsDomainStatistics
@scope
@restrict AE
@param {DatasetContext} context {@link ods-widgets.directive:odsCatalogContext Catalog Context} to use
@description
The odsDomainStatistics widget enumerates statistic values for a given catalog and injects them as variables in the context.
The following AngularJS variables are available:
* `CONTEXTNAME.stats.dataset`: the number of datasets
* `CONTEXTNAME.stats.keyword`: the number of keywords
* `CONTEXTNAME.stats.publisher`: the number of publishers
* `CONTEXTNAME.stats.theme`: the number of themes
# First syntax: when declaring a catalog context, directly inject these values
<pre>
<ods-catalog-context context="catalog" catalog-domain="dataset" ods-domain-statistics>
{{ catalog.stats.dataset }} datasets
</ods-catalog-context>
</pre>
# Second syntax : inject them using a dedicated tag
<pre>
<ods-domain-statistics context="catalog">
{{ catalog.stats.dataset }} datasets
</ods-domain-statistics>
</pre>
@example
<example module="ods-widgets">
<file name="index.html">
<ods-catalog-context context="examples"
examples-domain="https://documentation-resources.opendatasoft.com/"
ods-domain-statistics>
<p>Our portal has {{examples.stats.dataset}} datasets, described by {{examples.stats.theme}} themes
and {{examples.stats.keyword}} keywords.</p>
<p>{{examples.stats.publisher}} publishers have contributed.</p>
</ods-catalog-context>
</file>
</example>

### `odsFacet`

*Pas de documentation disponible*

### `odsFacetCategory`

*Pas de documentation disponible*

### `odsFacetCategoryList`

*Pas de documentation disponible*

### `odsFacetResults`

@ngdoc directive
@name ods-widgets.directive:odsFacetResults
@scope
@restrict A
@param {string} [odsFacetResults=results] Variable name to use
@param {CatalogContext|DatasetContext} odsFacetResultsContext {@link ods-widgets.directive:odsCatalogContext Catalog Context} or {@link ods-widgets.directive:odsDatasetContext Dataset Context} to use
@param {string} odsFacetResultsFacetName Name of the facet to enumerate
@param {string} [odsFacetResultsSort=count] Sorting method used on categories:
- `count` or `-count` to sort by number of items in each category
- `num` or `-num` to sort by the name of category, if it is a number
- `alphanum` or `-alphanum` to sort by the name of the category
Note: the `-` character before the name of the sorting method indicates that values will be sorted in descending order instead of ascending order.
@description
The odsFacetResults widget fetches the results of enumerating the values ("categories") of a facet and exposes it in a variable available in the scope.
You can use this widget with the AngularJS ngRepeat directive to build a list of results.
The variable is an array of objects, each containing the following properties:
* `name`: the label of the category
* `path`: the path to use to refine on this category
* `state`: "displayed" or "refined"
* `count`: the number of records in this category
@example
<example module="ods-widgets">
<file name="index.html">
<ods-catalog-context context="catalog"
catalog-domain="documentation-resources">
<label>Select a facet:</label>
<select ng-model="userchoice">
<option ng-repeat="item in items"
ods-facet-results="items"
ods-facet-results-context="catalog"
ods-facet-results-facet-name="publisher"
value="{{item.name}}">{{item.name}}</option>
</select>
</ods-catalog-context>
</file>
</example>

### `odsFacets`

@ngdoc directive
@name ods-widgets.directive:odsFacets
@scope
@restrict E
@param {DatasetContext} context <i>(mandatory)</i> {@link ods-widgets.directive:odsCatalogContext Catalog Context} or {@link ods-widgets.directive:odsDatasetContext Dataset Context} to use
@param {string} name <i>(mandatory)</i> Name of the field the filter is based on
@param {string} [title=none] Title to display above the filter
@param {string} [sort=-count] Sorting method used on categories:
- `count` or `-count` to sort by number of items in each category
- `num` or `-num` to sort by the name of category, if it is a number
- `alphanum` or `-alphanum` to sort by the name of the category
Note: the `-` character before the name of the sorting method indicates that values will be sorted in descending order instead of ascending order.
Configuring a specific order is also possible, by setting a list of value: `['value1', 'value2']`.
@param {number} [visibleItems=6] Number of categories to show. If there are more categories for the filter, they are collapsed by default, but can be expanded by clicking on a "more" link.
@param {boolean} [hideIfSingleCategory=false] When set to `true`, hides filters if only one category to refine on is available.
@param {string} [hideCategoryIf=none] AngularJS expression to evaluate: if it evaluates to `true`, the category is displayed. In the expression, the following elements can be used:
- `category.name` (value of the category)
- `category.path` (complete path to the category, including hierarchical levels)
- `category.state` (refined, excluded, or displayed)
@param {boolean} [disjunctive=false] When set to `true`, the filter is in disjunctive mode, which means that other available values can also be selected after a first value is selected. All selected values are combined as "or". For example, after clicking "red", "green" and "blue" can also be clicked. The resulting values can be green, red, or blue.
Note: this parameter is directly related to the schema of the dataset. For this parameter to work properly, the field must allow multiple selections in filters. For more information, see {@link https://user-guide.opendatasoft.com/en/articles/2044866 Defining a dataset schema}).
@param {boolean} [timerangeFilter=false] When set to `true`, an option to filter using a time range is displayed above the categories. This parameter only works for date and datetime fields and must be used with a context (see **context** parameter).
@param {string} [context=none] Name of the context to refine on. This parameter is mandatory for the **timerangeFilter** parameter.
@param {string} [valueSearch=none] When set to `true`, a search box is displayed above the categories to search within the available categories. If `suggest`, the matching categories are not displayed until there is at least one character typed into the search box, effectively making it into a suggest-like search box.
@param {DatasetContext|CatalogContext|DatasetContext[]|CatalogContext[]} [refineAlso=none] Enables the widget to apply its refinements on other contexts, e.g., for contexts which share a common data. The value of this parameter should be the name of another context or a list of contexts.
@param {string} [[contextName]FacetName=Current facet's name] Name of the facet in one of the other contexts, defined through the **refineAlso** parameter, that the original facet should be mapped on. `[contextName]` must be replaced with the name of that other context.
@description
The odsFacets widget displays filters based on a dataset or a domain's catalog of datasets. This widget allows to dynamically refine on one or more categories for the defined context (i.e., each filter being composed of several categories, which are values of the field the filter is based on).
For example, odsFacet can be used to refine the data displayed in a table ({@link ods-widgets.directive:odsTable odsTable}) to see only the desired data.
Suppose the widget is used without any configuration. In that case, it will display by default filters from all the "facet" fields of a dataset when used with a {@link ods-widgets.directive:odsDatasetContext Dataset Context}. It will display by default filters from typical metadata from a dataset catalog when used with a {@link ods-widgets.directive:odsCatalogContext Catalog Context}.
<pre>
<ods-facets context="mycontext"></ods-facets>
</pre>
<b>odsFacet</b>
The odsFacet widget is a widget that can only be used based on odsFacets. It is used to configure which facets should be displayed by odsFacets, since odsFacets used alone does not allow to display only specific facets among all the default ones of the dataset. odsFacet supports the following parameters:
- name
- sort
- visibleItems
- hideIfSingleCategory
- hideCategoryIf
Note: these parameters are the same as some used for odsFacets. For more information about configuration, see the odsFacets parameters table.
odsFacet allows to configure which facets are displayed using the **name** parameter.
<pre>
<ods-facets context="mycontext">
<h3>First field</h3>
<ods-facet name="myfield"></ods-facet>
<h3>Second field</h3>
<ods-facet name="mysecondfield"></ods-facet>
</ods-facets>
</pre>
Regular HTML is supported within the odsFacet tag to change the display template of each category. The available variables within the template are:
- `facetName`: name of the field that the filter is based on
- `category.name`: value of the category
- `category.path`: complete path to the category, including hierarchical levels
- `category.state`: refined, excluded, or displayed
An `ng-non-bindable` wrapper element must be used around the display template for it to work properly.
Note: There must not be any space character between the odsFacet tag and the span element, as it may prevent the widget from working properly.
<pre>
<ods-facets context="mycontext">
<ods-facet name="myfield"><span ng-non-bindable>
{{category.name}} @ {{category.state}}
</span></ods-facet>
</ods-facets>
</pre>
@example
<example module="ods-widgets">
<file name="odsFacets_with_odsFacet.html">
<ods-dataset-context context="events"
events-domain="https://documentation-resources.opendatasoft.com/"
events-dataset="evenements-publics-openagenda-extract">
<div class="row-fluid">
<div class="span4">
<ods-facets context="events">
<ods-facet name="date_mise_a_jour" title="Date"></ods-facet>
<h3>
<i class="icon-tags"></i> Tags
</h3>
<ods-facet name="mots_cles"><div ng-non-bindable>
{{category.name}}
</div>
</ods-facet>
</ods-facets>
</div>
<div class="span8">
<ods-map context="events"></ods-map>
</div>
</div>
</ods-dataset-context>
</file>
</example>
<example module="ods-widgets">
<file name="refineAlso_parameter.html">
<ods-dataset-context context="geonamescities, countries"
geonamescities-domain="https://documentation-resources.opendatasoft.com/"
geonamescities-dataset="doc-geonames-cities-5000"
countries-domain="https://documentation-resources.opendatasoft.com/"
countries-dataset="natural-earth-countries-110m">
<ods-facets context="geonamescities">
<ods-facet name="country_code"
refine-also="[countries]"
countries-facet-name="sovereignt"></ods-facet>
</ods-facets>
</ods-dataset-context>
</file>
</example>

### `odsFilterSummary`

*Pas de documentation disponible*

### `odsFullClick`

*Pas de documentation disponible*

### `odsGauge`

@ngdoc directive
@name ods-widgets.directive:odsGauge
@scope
@restrict E
@param {string} [displayMode=circle] Type of chart: 'circle' or 'bar'
@param {float} [max=100] The maximum value for the gauge
@param {float} value A number between 0 and the defined `max` value
@description
The odsGauge widget displays a gauge in one of the two following modes: circle or horizontal bar.
The widget relies on CSS3 and SVG. As a result, it is entirely customizable in CSS.
The widget will decide its size based on its width, so you can make it larger or smaller using the CSS `width`
property; however, the widget will always take the necessary height, so forcing the height using CSS won't work.
Values exceeding the given max will be represented as a full gauge, whereas values lower than 0 will be
represented as an empty gauge.
@example
<example module="ods-widgets">
<file name="index.html">
<ods-gauge display-mode="circle" value="33" max="70"></ods-gauge>
</file>
</example>

### `odsGeoNavigation`

*Pas de documentation disponible*

### `odsGeotooltip`

@ngdoc directive
@name ods-widgets.directive:odsGeotooltip
@scope
@restrict E
@param {Array|string} [coords=none] Coordinates of a point to display in the tooltip; either an array of two numbers as [latitude, longitude], or a string under the form of "latitude,longitude".
If you use a string, surround it with simple quotes to ensure Angular treats it as a string. If you are working with a record (e.g., using {@link ods-widgets.directive:odsResultEnumerator odsResultEnumerator}), you can directly use the content of a `geo_point_2d` field.
@param {Object} [geojson=none] GeoJSON object of a shape to display in the tooltip. If you are working with a record (e.g., using {@link ods-widgets.directive:odsResultEnumerator odsResultEnumerator}), you can directly use the content of a `geo_shape` field.
@param {Object} [record=none] A record object (e.g., from {@link ods-widgets.directive:odsResultEnumerator odsResultEnumerator}) from which the geometry will be taken (this is the `geometry` property of the record)
@param {number} [width=200] Width of the tooltip, in pixels
@param {number} [height=200] Height of the tooltip, in pixels
@param {number} [delay=500] Delay before the tooltip appears on hover, in milliseconds
@description
When used to surround a text, the odsGeotooltip widget displays a tooltip showing a point and/or a shape in a map.
@example
<example module="ods-widgets">
<file name="index.html">
<!-- Display specific values -->
<p>
<ods-geotooltip coords="'48.858093,2.294694'">Nice place</ods-geotooltip>
</p>
<p>
<ods-geotooltip coords="[48.841601, 2.284822]">Nice people</ods-geotooltip>
</p>
<ods-dataset-context context="events"
events-domain="https://documentation-resources.opendatasoft.com/"
events-dataset="evenements-publics-openagenda-extract">
<!-- Display values from records -->
<ods-result-enumerator context="events" max="1">
<div>
<!-- Using the value from a field with a "geo_point_2d" type -->
<ods-geotooltip coords="item.fields.latlon">Location</ods-geotooltip>
<!-- Directly passing a record -->
<ods-geotooltip record="item">Same location</ods-geotooltip>
</div>
</ods-result-enumerator>
</ods-dataset-context>
</file>
</example>

### `odsGetElementLayout`

@ngdoc directive
@name ods-widgets.directive:odsGetElementLayout
@scope
@restrict A
@description
The odsGetElementLayout widget gets the height and width of an element. The variable is an object that contains 2 keys: 'height' and 'width'.
@example
<example module="ods-widgets">
<file name="index.html">
<div ods-get-element-layout="layout">
{{ layout.height }} px
</div>
</file>
</example>

### `odsGetWindowLayout`

@ngdoc directive
@name ods-widgets.directive:odsGetWindowLayout
@scope
@restrict A
@description
The odsGetElementLayout widget gets the height and width of the window. The variable is an object that contains 2 keys: 'height' and 'width'.
@example
<example module="ods-widgets">
<file name="index.html">
<div ods-get-window-Layout="mylayout">
{{ mylayout.width }} px
</div>
</file>
</example>

### `odsGist`

@ngdoc directive
@name ods-widgets.directive:odsGist
@restrict E
@scope
@param {string} username The GitHub username
@param {string} gist-id The Gist identifier. See the Gist URL to find it.
@description
The odsGist widget integrates a GitHub Gist widget with a "copy to clipboard" button into a page.
@example
<example module="ods-widgets">
<file name="index.html">
<ods-gist username="Amli" gist-id="b845c8d4b3a2ce08c0a5ce3dd0d7625d"></ods-gist>
</file>
</example>

### `odsHighcharts`

@deprecated
@name ods-widgets.directive:odsHighcharts
@restrict E
@scope
@param {DatasetContext} context {@link ods-widgets.directive:odsDatasetContext Dataset Context} to use
@param {string} fieldX Name of the field used for the X axis
@param {string} expressionY Expression for the Y axis, typically a field name. Optional if the function (function-y) is 'COUNT'.
@param {string} functionY Function applied to the expression for the Y axis: AVG, COUNT, MIN, MAX, STDDEV, SUM
@param {string} timescale If the X axis is time-based, then you can specify the timescale (year, month, week, day, hour)
@param {string} chartType One of the following chart types: line, spline, area, areaspline, column, bar, pie
@param {string} color The color (or comma-separated list of colors in case of a pie chart) to draw the chart in. Colors are in hex color code (e.g. *#2f7ed8*).
If not specified, the colors from {@link ods-widgets.ODSWidgetsConfigProvider ODSWidgetsConfig.chartColors} will be used if they are configured, else Highcharts default colors.
@param {string} [sort=none] How to sort the data in the chart: *x* or *-x* to sort or reverse sort on the X axis; *y* or *-y* to sort or reverse sort on the Y axis.
@param {number} [maxpoints=50] Maximum number of points to chart.
@param {string} [labelX=none] Configure a specific label for the X axis. By default it is named after the field used for the X axis.
@param {string} [labelY=none] Configure a specific label for the charted values and the Y axis. By default it is named after the expression used for the Y axis, or 'Count' if `functionY` is "COUNT".
@param {string|Object} [chartConfig=none] a complete configuration, as a object or as a base64 string. The parameter directly expects an angular expression, so a base64 string needs to be quoted. If this parameter is present, all the other parameters are ignored, and the chart will not change if the context changes.
@description
This widget can be used to integrate a visualization based on Highcharts.
@example
<example module="ods-widgets">
<file name="index.html">
<ods-dataset-context context="hurricanes" hurricanes-domain="public.opendatasoft.com" hurricanes-dataset="hurricane-tracks-1851-2007">
<ods-highcharts context="hurricanes" field-x="track_date" chart-type="line" timescale="year" function-y="COUNT"></ods-highcharts>
</ods-dataset-context>
</file>
</example>

### `odsHighchartsChart`

*Pas de documentation disponible*

### `odsHubspotForm`

@ngdoc directive
@name ods-widgets.directive:odsHubspotForm
@restrict E
@scope
@param {string} portalId The portal ID
@param {string} formId The form ID
@description
The odsHubspotForm widget integrates a HubSpot form given a portal ID and the form ID.
@example
<pre>
<ods-hubspot-form portal-id="1234567" form-id="d1234564-987987987-4564654-7897-456465465"></ods-hubspot-form>
</pre>

### `odsInfiniteScrollResults`

@ngdoc directive
@name ods-widgets.directive:odsInfiniteScrollResults
@scope
@restrict A
@param {CatalogContext|DatasetContext} odsResultsContext {@link ods-widgets.directive:odsCatalogContext Catalog Context} or {@link ods-widgets.directive:odsDatasetContext Dataset Context} to use
@param {boolean} [scrollTopWhenRefresh=false] If the context parameters change (which will probably change the results), the widget scrolls to the top of the window.
@param {string} [listClass=none] A class (or classes) that will be applied to the list of result
@param {string} [resultClass=none] A class (or classes) that will be applied to each result
@param {string} [noResultsMessage] A sentence that will be displayed if there are no results
@param {string} [noMoreResultsMessage] A sentence that will be displayed if there are no more results to fetch
@param {string} [noDataMessage] A sentence that will be displayed if the context has no content at all
@description
The odsInfiniteScrollResults widget displays the results of a query inside an infinite scroll list. It uses the HTML template inside the widget tag
and repeats it for each result.
If used with a {@link ods-widgets.directive:odsCatalogContext Catalog Context}, for each result, the following AngularJS variables are available:
* item.datasetid: Dataset identifier of the dataset
* item.metas: An object holding the key/values of metadata for this dataset
If used with a {@link ods-widgets.directive:odsDatasetContext Dataset Context}, for each result, the following AngularJS variables are available:
* item.datasetid: Dataset identifier of the dataset this record belongs to
* item.fields: An object holding all the key/values for the record
* item.geometry: if the record contains geometrical information, this object is present and holds its GeoJSON representation
@example
<example module="ods-widgets">
<file name="index.html">
<ods-catalog-context context="example"
example-domain="https://data.opendatasoft.com/">
<ul>
<ods-infinite-scroll-results context="example">
<li>
<strong>{{item.metas.title}}</strong>
(<a ng-href="{{context.domainUrl + '/explore/dataset/' + item.datasetid + '/'}}" target="_blank">{{item.datasetid}}</a>)
</li>
</ods-infinite-scroll-results>
</ul>
</ods-catalog-context>
</file>
</example>

### `odsLastDatasetsFeed`

@ngdoc directive
@name ods-widgets.directive:odsLastDatasetsFeed
@scope
@restrict E
@param {CatalogContext} context {@link ods-widgets.directive:odsCatalogContext Catalog Context} to use
@description
The odsLastDatasetsFeed widget displays the last datasets of a catalog based on the *modified* metadata. By default, the widget displays the last five datasets.
@example
<example module="ods-widgets">
<file name="index.html">
<ods-catalog-context context="example" example-domain="data.opendatasoft.com">
<ods-last-datasets-feed context="example"></ods-last-datasets-feed>
</ods-catalog-context>
</file>
</example>

### `odsLastReusesFeed`

@ngdoc directive
@name ods-widgets.directive:odsLastReusesFeed
@scope
@restrict E
@param {CatalogContext} context {@link ods-widgets.directive:odsCatalogContext Catalog Context} to use
@param {number} [max=5] Maximum number of reuses to show
@param {boolean} [externalLinks=false] Clicking on the reuses' titles or images will directly redirect to the reuse.
Otherwise, by default, it will redirect to the dataset.
@description
This widget displays the last five reuses published on a domain.
It is possible to customize the template used to display each reuse by adding HTML inside the widget's tag.
The following variables are available:
* `reuse.url`: URL to the reuse's dataset page
* `reuse.title`: Title of the reuse
* `reuse.thumbnail`: URL to the thumbnail of the reuse
* `reuse.description`: Description of the reuse
* `reuse.created_at`: ISO datetime of reuse's original submission (can be used as `reuse.created_at|moment:'LLL'` to format it)
* `reuse.dataset.title`: Title of the reuse's dataset
* `reuse.user.last_name`: Last name of the reuse's submitter
* `reuse.user.first_name`: First name of the reuse's submitter
@example
<example module="ods-widgets">
<file name="index.html">
<ods-catalog-context context="public" public-domain="https://public.opendatasoft.com">
<ods-last-reuses-feed context="public"></ods-last-reuses-feed>
</ods-catalog-context>
</file>
</example>

### `odsLegend`

@ngdoc directive
@name ods-widgets.directive:odsLegend
@scope
@restrict E
@param {object} colorGradient An object providing colors, values, and a range of value. It also provides the number of classes for the `steps` display mode.
@param {string} title Legend title
@param {string} [subtitle='']  Legend sub-title
@param {string} [noValueColor=undefined] Displays another step or square with the provided default color. The authorized values are any HTML color code.
@param {integer} [decimalPrecision=0] Sets the decimal values precision.
@param {string} [display=linear] Display mode. The authorized values are 'steps' and 'linear'.
@description
The odsLegend widget displays a map legend computed with the color gradient structure from the odsColorGradient widget.
The `steps` display mode is a legend with different steps based on the range of values. Each step has its own color and value range.
The `linear` display mode is a single color gradient from the minimum to the maximum value.
Note: You can use the `steps` display mode only if the ods-color-gradient-nb-classes option has been provided to the odsColorGradient widget.
@example
<example module="ods-widgets">
<file name="legend.html">
<ods-dataset-context context="regions,population"
regions-dataset="regions-et-collectivites-doutre-mer-france"
regions-domain="https://documentation-resources.opendatasoft.com/"
regions-parameters="{'q':'NOT (guadeloupe OR mayotte OR guyane OR martinique OR reunion)',
'disjunctive.reg_name':true}"
population-dataset="populations-legales-communes-et-arrondissements-municipaux-france"
population-parameters="{'disjunctive.reg_name':true}"
population-domain="https://documentation-resources.opendatasoft.com/">
<div ods-color-gradient="colorgradient"
ods-color-gradient-context="population"
ods-color-gradient-x="reg_name"
ods-color-gradient-serie="SUM(com_arm_pop_tot)"
ods-color-gradient-high="rgb(20, 33, 96)"
ods-color-gradient-low="rgb(180, 197, 241)"
ods-color-gradient-nb-classes="4">
<ods-map location="5,46.50595,3.40576">
<ods-map-layer context="regions"
color-categories="colorgradient['colors']"
color-by-field="reg_name"
color-categories-other="lightgrey"
display="categories"
shape-opacity="0.85"
title="Sum of cities population">
</ods-map-layer>
</ods-map>
<ods-legend title="Population by region"
color-gradient="colorgradient"
display="steps"
decimal-precision="0"
subtitle="Cities population dataset - 2019"></ods-legend>
<ods-legend title="A linear alternative"
color-gradient="colorgradient"
display="linear"
no-value-color="lightgrey"
subtitle="With default color"></ods-legend>
</div>
</ods-dataset-context>
</file>
</example>

### `odsMap`

*Pas de documentation disponible*

### `odsMapDisplayControl`

*Pas de documentation disponible*

### `odsMapLayer`

@ngdoc directive
@name ods-widgets.directive:odsMapLayer
@scope
@restrict E
@param {DatasetContext} context <i>(mandatory)</i> {@link ods-widgets.directive:odsDatasetContext Dataset Context} to use
@param {expression} [showIf=none] AngularJS expression to evaluate: if it evaluates to true, the layer is visible.
@param {number} [showZoomMin=none] Makes the layer visible only if the zoom level is superior or equal to the value.
@param {number} [showZoomMax=none] Makes the layer visible only if the zoom level is inferior or equal to the value.
@param {string} [display=auto] Map mode:
- `auto`: automatically chooses the best map mode to easily display the data, based on the number of points and type of geometry
- `heatmap`: displays the data as a heatmap, i.e. a density of points represented by a color intensity variation. It can also be based on the result of an aggregation function.
- `categories`: based on a text field value, categorizes and colors the data
- `choropleth`: based on a number field or aggregation, colors the data using a color scale
- `clusters`: spatially groups the data in clusters ; each cluster displays the number of points it contains. When at maximum zoom, all points are shown.
- `clustersforced`: spatially aggregates the data in clusters ; the number displayed on the cluster is the result of an aggregation function.
- `raw`: displays the data directly without clustering or organizing them. This mode should not be used for large datasets (i.e., datasets with more than 5,000 points to display), as it may freeze the user's browser.
- `aggregation`: data is aggregated based on a geo shape (e.g., 2 records with the exact same shape associated). By default, the color represents the number of aggregated records, but it can be the result of an aggregation function. This mode supports aggregating the context using a join with another context that contains geometrical shapes: use a `joinContext` property, and `localKey` and `remoteKey` to configure the field names of the local and joined datasets. It is also possible to configure one of the fields from the "remote" dataset, for them to be displayed when the mouse hovers the shapes: use `hoverField` and the name of a field to do so.
@param {string} [function=none] For the `heatmap`, `choropleth`, and `clusters` modes only–function used to aggregate the data:
- AVG: average
- COUNT
- MIN: minimum
- MAX: maximum
- STDDEV: standard deviation
- SUM
@param {expression} [expression=none] Expression used to aggregate the data. This parameter is not required when the function is COUNT.
@param {string} [color=none] Color of the displayed shapes and markers
@param {string} [borderColor=white] Color of the shapes' borders
@param {number} [borderSize=1] The width of the shapes' borders, in pixels
@param {string} [borderPattern=solid] Pattern of the shapes' borders:
- `solid`
- `long-dashes`
- `medium-dashes`
- `short-dashes`
- `dots`
- `short-dot`
- `short-dot-dot`
- `medium-short`
@param {number} [borderOpacity=1] Opacity of the shapes' borders. The value must be between `0` (transparent) and `1` (opaque).
@param {number} [shapeOpacity=0.5] Opacity of the shapes. The value must be between `0` (transparent) and `1` (opaque).
@param {number} [pointOpacity=1] Opacity of the markers. The value must be between `0` (transparent) and `1` (opaque).
@param {number} [lineWidth=5] The width of the lines, in pixels. Only applicable for "line" type shapes.
@param {objet} [colorCategories=none] For the `categories` mode only–object that links textual values and colors (e.g., `{'Paris': '#FF0000', 'Nantes: '#00FF00'}`).
@param {string} [colorCategoriesOther=none] For the `categories` mode only–default color for values that were not originally taken into account by the `color-categories` object.
@param {string} [colorUndefined=none] For the `choropleth` mode only–default color for the `undefined` values.
@param {string} [colorOutOfBounds=none] For the `choropleth` mode only–default color for values out of the expected `color-numeric-ranges` scale.
@param {string} [colorNumericRanges=none] For the `choropleth` mode only–color scale used (e.g., `{'0': '#FF0000', '1': '#FFFF00'}`). The key is the upper bound used for this color (e.g., still using the previous example, it would be #FF0000 until 0, then #FFFF00 until 1, etc.)
@param {number} [colorNumericRangeMin=none] For the `choropleth` mode only–minimum bound used. Any value below that bound will be considered out of the scale, and will use the color of the `color-out-of-bounds` parameter.
@param {string} [colorGradient=none] For the `heatmap` mode only–object that links upper numeric bounds and colors (e.g., `{0.2: '#FF0000', 1: '#00FF00'}`)
@param {string} [colorByField=none] For categories and choropleth modes only - Field used to choose the color
@param {number} [radius=4] For the `heatmap` mode only–width of the perimeter
@param {number} [size=4] For markers, 7 for pictograms–size of the markers
@param {number} [sizeMin=3] For the `clusters` mode only–minimum size of the clusters
@param {number} [sizeMax=5] For the `clusters` mode only–maximum size of the clusters
@param {string} [sizeFunction=none] For the `clusters` mode only–calculation function of the clusters size:
- `linear`
- `log` (logarithmic)
@param {string} [picto=none] Pictogram used for the markers
@param {boolean} [showMarker=none] When set to `true`, displays a marker around the pictogram.
@param {string} tooltipSort Identifier of the field used to sort tooltips that represent several records for the same point or shape.
Note that `-` before the name of the sorting method indicates that the sorting will be descending instead of ascending.
By default, numeric fields are sorted in decreasing order, date and datetime are sorted chronologically, and text fields are sorted alphanumerically.
@param {boolean} [tooltipDisabled=none] When set to `true`, clicking on a point or shape does not display the associated tooltip.
@param {boolean} [caption=none] When set to `true`, displays a caption for the map layer in the bottom right corner of the map.
@param {string} [captionTitle=none] Title of the map layer caption.
@param {string} [captionPictoColor=none] Color used for the caption's pictogram
@param {string} [captionPictoIcon=none] Pictogram used in the caption
@param {string} [title=none] Title used in the map layer's control selection
@param {string} [description=none] Description used in the map layer's control selection
@param {boolean} [excludeFromRefit=none] When set to `true`, the calculation that rezooms the map when filters or data change does not take the map layer into account.
@param {string} [refineOnClickContext=none] Name, or list of names separated by commas (`[ctx1, ctx2]`) of contexts that should be refined when clicking on a point or shape of the map layer.
@param {string} [refineOnClickMapField=none] (or `refine-on-click-CONTEXTNAME-map-field` if more than one context) - Field of the map layer that is used to retrieve the value used for the refine
@param {string} [refineOnClickContextField=none] (or `refine-on-click-CONTEXTNAME-context-field` if more than one context) - Field used in the context of the refine (`refine.FIELDNAME=VALUE`)
@param {boolean} [refineOnClickReplaceRefine=none] (or `refine-on-click-CONTEXTNAME-replace-refine` if more than one context) - When set to `true`, each click replaces the previous refine instead of adding to it.
@description
The odsMapLayer widget allows to declare the data layers that can be displayed on a map visualization. odsMapLayer is one of the map-related widgets that can only be used based on {@link ods-widgets.directive:odsMap odsMap}, the primary map-related widgets. For more information on {@link ods-widgets.directive:odsMap odsMap}, see the documentation for this widget.
A map visualization can comprise several data layers, which are dynamic. In other words, if the context changes, the layer is refreshed and displays the new relevant data.
Each data layer is based on a context and can have its own display mode and configurations.
<pre>
<ods-map>
<ods-map-layer context="mycontext" color="#FF0000" display="clusters"></ods-map-layer>
<ods-map-layer context="mycontext2" display="heatmap"></ods-map-layer>
<ods-map-layer context="mycontext3" display="raw" color="#0000FF"></ods-map-layer>
</ods-map>
</pre>
<b>Layers display modes</b>
Map visualizations can either display:
- the layer data itself (i.e., each point is a record from the dataset), or
- an aggregation of data (i.e., each point is the result of an aggregation function).
Several display modes are available (see **display** parameter in the table below). However, only some of them support aggregation functions: `aggregation`, `heatmap`, and `clustersforced`.
Aggregation functions are specified in the odsMapLayer widget through 2 parameters: **function** and **expression**, which define the value used for the function (usually, the name of a field). For more information, see the "Parameters" table.
<pre>
<ods-map>
<!-- Display a heatmap of the average value -->
<ods-map-layer context="mycontext" display="heatmap" expression="value" function="AVG"></ods-map-layer>
</ods-map>
</pre>
<b>Layers display color configurations</b>
Apart from `heatmap`, all display modes support color configuration. Three configuration types are available, depending on the display mode:
- `color`: a color, as an hex code (#FF0F05) or a CSS color name (e.g., "red"). Available for any display mode.
- `colorScale`: the name of a {@link http://colorbrewer2.org/ ColorBrewer} scheme (e.g., "YlGnBu"). Available only for `aggregation`.
- `colorRanges`: a series of colors and ranges separated by a semicolon, to decide a color depending on a value. For example "red;20;orange;40;#00CE00" colors anything between 20 and 40 in orange, below 20 in red, and above 40 in a custom hex color.
It can be combined with a decimal or integer field name in `colorByField` to configure which field will be used to decide on the color (for `raw`) or with `function` and `expression` to determine the calculation used for the color (for `aggregation`).
Available for `raw` and `aggregation` display modes.
An additional `colorFunction` property can contain the `log` value to use logarithmic scales (instead of the default linear scale) for generating the color scale.
Available for `aggregation` and with `color` and `colorScale` display modes, or when none is specified.
On top of color configuration, the icon used as a marker on the map can be configured through the `picto` property. The property supports the keywords listed in the <a href="https://user-guide.opendatasoft.com/en/articles/2042498" target="_blank">Pictograms reference documentation</a>.
When displaying shapes, `borderColor` and `opacity` can be used to configure the color of the shape border and the opacity of the shape's fill.
<b>Layers zoom and hide & show configurations</b>
Layers can be hidden or shown depending on the configuration of the `showIf` parameter, which functions similarly to Angular's `ngIf`.
<pre>
<ods-map>
<ods-map-layer context="mycontext" color="#FF0000" display="clusters"></ods-map-layer>
<ods-map-layer context="mycontext2" display="heatmap" show-if="showHeatmap"></ods-map-layer>
</ods-map>
</pre>
Layers can also be configured to only be visible between certain zoom levels, using the `showZoomMin` and/or
`showZoomMax` parameters.
<pre>
<ods-map>
<!-- This layer is only visible up to zoom 8 -->
<ods-map-layer context="mycontext1" show-zoom-max="8"></ods-map-layer>
<!-- This layer appears between zoom 9 and 14 -->
<ods-map-layer context="mycontext2" show-zoom-min="9" show-zoom-max="14"></ods-map-layer>
<!-- This layer is visible starting at zoom 15 -->
<ods-map-layer context="mycontext3" show-zoom-min="15"></ods-map-layer>
</ods-map>
</pre>
<b>Tooltips</b>
By default, tooltips show the values associated with a point or shape in a simple template. Custom HTML tooltip templates can be added inside the `<ods-map-layer></ods-map-layer>` tag. The custom template is AngularJS-enabled and will be provided with a `record` object; this object contains a `fields` object with all the values associated with the clicked point or shape.
<pre>
<ods-map location="12,48.86167,2.34146">
<ods-map-layer context="mycontext">
<div>my value is: {{record.fields.myvalue}}</div>
</ods-map-layer>
</ods-map>
</pre>
In case the tooltip is not relevant for the map visualization, it can be disabled them using the **tooltipDisabled** parameter set on `true`.
<pre>
<ods-map>
<ods-map-layer context="mycontext" tooltip-disabled="true"></ods-map-layer>
</ods-map>
</pre>
If the map visualization displays multiple points or shapes that are stacked, it is possible to configure the order in which the items will be displayed in the tooltip, using `tooltipSort` and the name of a field, prefixed by `-` to have a reversed sort.
Note: by default, numeric fields are sorted in decreasing order, date and datetime are sorted chronologically, and text fields are sorted alphanumerically.
<pre>
<ods-map>
<!-- Reverse sort on 'field' -->
<ods-map-layer context="mycontext" tooltip-sort="-field"></ods-map-layer>
</ods-map>
</pre>
<b>Refine-on-click map configuration</b>
If a layer is displayed as `raw` or `aggregation`, it can be configured so that a click on an item triggers a refine on another context, using **refineOnClickContext**.
One or more contexts can be defined:
<pre>
<ods-map>
<ods-map-layer context="mycontext" refine-on-click-context="mycontext2"></ods-map-layer>
<ods-map-layer context="mycontext3" refine-on-click-context="[mycontext4, mycontext5]"></ods-map-layer>
</ods-map>
</pre>
By default, the filter occurs on geometry. For example, clicking on a shape filters the other context on the area.
It is also possible to trigger a refine on specific fields, using **refineOnClickMapField** to configure the name of the field to get the value from, and **refineOnClickContextField** to configure the name of the field of the other context to refine on. If there are 2 or more contexts, it is possible to configure the fields by indicating the context in the name of the property, as `refineOnClick[context]MapField` and `refineOnClick[context]ContextField`.
<pre>
<ods-map>
<ods-map-layer context="mycontext"
refine-on-click-context="[mycontext, mycontext2]"
refine-on-click-mycontext-map-field="field1"
refine-on-click-mycontext-context-field="field2"
refine-on-click-mycontext2-map-field="field3"
refine-on-click-mycontext2-context-field="field4"></ods-map-layer>
</ods-map>
</pre>
@example
<example module="ods-widgets">
<file name="odsMap_with_odsMapLayer.html">
<ods-dataset-context context="genderequalityineurope"
genderequalityineurope-dataset="gender-equality-in-europe"
genderequalityineurope-domain="https://documentation-resources.opendatasoft.com/">
<ods-map no-refit="true"
scroll-wheel-zoom="false"
display-control="false"
search-box="false"
toolbar-fullscreen="true"
toolbar-geolocation="true"
location="2,36.19117,-6.26602">
<ods-map-layer context="genderequalityineurope"
color-numeric-ranges="{'43.1':'#AAC3DD','50.9':'#89A0CA','58.7':'#687DB7','66.5':'#475AA4','74.3':'#263892','39.2':'#BBD5E7','47.0':'#99B2D4','54.8':'#788FC1','62.6':'#576CAE','70.4':'#36499B'}"
color-undefined="#F8B334"
color-out-of-bounds="#1BA566"
color-by-field="general_index"
color-numeric-range-min="35.3"
display="choropleth"
shape-opacity="0.5"
point-opacity="1"
border-color="#FFFFFF"
border-opacity="1"
border-size="1"
border-pattern="solid"></ods-map-layer>
</ods-map>
</ods-dataset-context>
</file>
</example>

### `odsMapLayerGroup`

@ngdoc directive
@name ods-widgets.directive:odsMapLayerGroup
@scope
@restrict E
@param {string} title <i>(mandatory)</i> Title of the group of layers
@param {string} [description=none] Description of the group of layers
@param {string} [pictoColor=#000000] Color of the pictogram for the group of layers', in the following format: `#000000`
@param {string} [pictoIcon=none] Name of pictogram for the group of layers
@param {boolean} [displayed=true] When set to `true`, the group of layers is displayed by default.
@description
The odsMapLayerGroup widget allows to declare a group of layers, which are declared through the {@link ods-widgets.directive:odsMapLayer odsMapLayer} widget. odsMapLayerGroup is one of the map-related widgets that can only be used based on {@link ods-widgets.directive:odsMap odsMap}, the primary map-related widgets. For more information on {@link ods-widgets.directive:odsMap odsMap}, see the documentation for this widget.
@example
<example module="ods-widgets">
<file name="odsMap_with_odsMapLayer_odsMapLayerGroup.html">
<ods-dataset-context context="under100000,under500000,greaterthan500000"
under100000-dataset="doc-geonames-cities-5000"
under100000-parameters="{'q.population':' population > 0 AND population < 100000'}"
under100000-domain="https://documentation-resources.opendatasoft.com/"
under500000-dataset="doc-geonames-cities-5000"
under500000-parameters="{'q.population':' population >= 100000 AND population < 500000'}"
under500000-domain="https://documentation-resources.opendatasoft.com/"
greaterthan500000-dataset="doc-geonames-cities-5000"
greaterthan500000-parameters="{'q.population':'population >= 500000'}"
greaterthan500000-domain="https://documentation-resources.opendatasoft.com/">
<ods-map no-refit="true"
scroll-wheel-zoom="false"
display-control="true"
search-box="false"
toolbar-fullscreen="true"
toolbar-geolocation="true"
location="2,22.59373,2.8125">
<ods-map-layer-group>
<ods-map-layer context="under100000"
color="#FA8C44"
picto="ods-circle"
show-marker="true"
display="auto"
shape-opacity="0.5"
point-opacity="1"
border-color="#FFFFFF"
border-opacity="1"
border-size="1"
border-pattern="solid"
caption="true"
caption-picto-color="#FA8C44"
title="Cities with less than 100,000 inhabitants"
size="4"
size-min="3"
size-max="5"
size-function="linear"></ods-map-layer>
</ods-map-layer-group>
<ods-map-layer-group>
<ods-map-layer context="under500000"
color="#93117E"
picto="ods-circle"
show-marker="true"
display="auto"
shape-opacity="0.5"
point-opacity="1"
border-color="#FFFFFF"
border-opacity="1"
border-size="1"
border-pattern="solid"
caption="true"
caption-picto-color="#93117E"
title="Cities with a population beetween 100,000 & 500,000 inhabitants"
size="4"
size-min="3"
size-max="5"
size-function="linear"></ods-map-layer>
</ods-map-layer-group>
<ods-map-layer-group>
<ods-map-layer context="greaterthan500000"
color="#CDBCD9"
show-marker="true"
display="auto"
shape-opacity="0.5"
point-opacity="1"
border-color="#FFFFFF"
border-opacity="1"
border-size="1"
border-pattern="solid"
caption="true"
caption-picto-color="#CDBCD9"
title="Cities with more than 500,000 inhabitants"
size="4"
size-min="3"
size-max="5"
size-function="linear"></ods-map-layer>
</ods-map-layer-group>
</ods-map>
</ods-dataset-context>
</file>
</example>

### `odsMapLegacy`

@deprecated
@name ods-widgets.directive:odsMapLegacy
@restrict E
@scope
@param {DatasetContext} context {@link ods-widgets.directive:odsDatasetContext Dataset Context} to use
@param {boolean} [autoResize=false] If true, the map will attempt to resize itself to always take up all the space to the bottom of the viewport.
It is only useful in very specific cases, when the map is the main focus of the page and should take all the window real estate available.
@param {string} [location=none] Initial location of the map, under the format "zoom,latitude,longitude" (e.g. *12,48.85887,2.3292*)
@param {string} [basemap=default basemap] Identifier of the basemap to apply. Basemaps are configured using {@link ods-widgets.ODSWidgetsConfigProvider ODSWidgetsConfig.basemaps}.
@param {boolean} [isStatic=false] If true, the map can't be panned or zoomed; in other words the map is static and can only show the initial view. Interaction with the data is still active,
for example you can still click on a marker to have a tooltip.
@param {boolean} [showFilters=false] If true, displays additional tools to use the map to filter the data in the context. For example if you use a table and a map on the same context,
this makes you able to use the map to refine the data displayed in the table.
@param {Object} [mapContext=none] An object that you can use to share the map state (location and basemap) between two or more map widgets when they are not in the same context.
@param {DatasetContext} [itemClickContext=none] Instead of popping a tooltip when you click on an item on the map, you can decide to add a filter to another context using this parameter.
Clicks that would normally make a popup appear (markers, clusters that can't be expanded more, shapes) will instead filter the specified context.
By default this is a spatial filter:
if you clicked a point, then the filter is the exact location; if you clicked a shape, then the filter is the content of this shape.
Note that you can specify more than one context by passing an array:
<pre>
<ods-map-legacy context="myctx"
item-click-context="[context2, context3]">
</ods-map-legacy>
</pre>
In that case, the `itemClickMapField` and `itemClickContextField` (as described below) need to contain the name of the context they apply to:
<pre>
<ods-map-legacy context="myctx"
item-click-context="[trees, roads]"
item-click-trees-map-field="field1"
item-click-trees-context-field="field2"
item-click-roads-map-field="field1"
item-click-roads-context-field="field3">
</ods-map-legacy>
</pre>
@param {string} [itemClickMapField=none] If you are using `itemClickContext` and want to filter on the value of a field instead of a spatial query, you can use this parameter to specify the name of the field to take
the value from. This must be a field from the dataset displayed on the map. It must be used together with `itemClickContextField`.
@param {string} [itemClickContextField=none] This parameter specifies the field to filter on in the context configured in `itemClickContext`. It must be used together with `itemClickMapField`.
The field must be a facet.
@example
<example module="ods-widgets">
<file name="index.html">
<ods-dataset-context context="arrondissements"
arrondissements-domain="documentation-resources"
arrondissements-dataset="arrondissements-paris">
<ods-map-legacy context="arrondissements"></ods-map-legacy>
</ods-dataset-context>
</file>
</example>

### `odsMapLegend`

*Pas de documentation disponible*

### `odsMapPicto`

*Pas de documentation disponible*

### `odsMapSearchBox`

*Pas de documentation disponible*

### `odsMapTooltip`

*Pas de documentation disponible*

### `odsMediaGallery`

@ngdoc directive
@name ods-widgets.directive:odsMediaGallery
@restrict E
@scope
@param {DatasetContext} context {@link ods-widgets.directive:odsDatasetContext Dataset Context} to use
@param {string} [displayedFields=all] A comma-separated list of fields to display in the details for each thumbnail. If no value is specified, the options configured for the dataset are used or all fields if nothing configured.
@param {string} [imageFields=all] A comma-separated list of fields to display in the gallery as thumbnails. If no value is specified, the options configured for the dataset are used or all media fields if nothing is configured.
@param {string} [odsWidgetTooltip] For more information, see {@link ods-widgets.directive:odsWidgetTooltip Widget Tooltip}.
@param {boolean} [odsAutoResize] For more information, see {@link ods-widgets.directive:odsAutoResize Auto Resize}.
@param {boolean} [refineOnClick] For more information, see {@link ods-widgets.directive:refineOnClick Refine on click}. This option takes precedence over the widget tooltip.
@description
The odsMediaGallery widget displays an image gallery of a dataset containing media with thumbnails (images, PDF files, etc.) with infinite scroll.
You can use the {@link ods-widgets.directive:odsWidgetTooltip Widget Tooltip} directive to customize the detail view appearing when selecting a thumbnail.
@example
<example module="ods-widgets">
<file name="index.html">
<ods-dataset-context context="affiches"
affiches-domain="https://documentation-resources.opendatasoft.com/"
affiches-dataset="affiches-anciennes">
<ods-media-gallery context="affiches" ods-auto-resize ods-widget-tooltip>
<h3>My custom tooltip</h3>
{{ getRecordTitle(record) }}
</ods-media-gallery>
</ods-dataset-context>
</file>
</example>

### `odsMostPopularDatasets`

@ngdoc directive
@name ods-widgets.directive:odsMostPopularDatasets
@scope
@restrict E
@param {CatalogContext} context {@link ods-widgets.directive:odsCatalogContext Catalog Context} to use.
@param {integer} [max=5] Number of datasets to show in the list.
@param {string} [orderBy=downloads] List order.
Datasets can be sorted by most downloaded or popularity. The authorized values are `downloads` and `popularity`.
@description
The odsMostPopularDatasets widget displays the top datasets of a catalog based on the number of downloads. By default, the widget displays the top five datasets.
@example
<example module="ods-widgets">
<file name="index.html">
<ods-catalog-context context="example"
example-domain="data.opendatasoft.com">
<ods-most-popular-datasets context="example"></ods-most-popular-datasets>
</ods-catalog-context>
</file>
</example>

### `odsMostUsedThemes`

@ngdoc directive
@name ods-widgets.directive:odsMostUsedThemes
@scope
@restrict E
@param {CatalogContext} context {@link ods-widgets.directive:odsCatalogContext Catalog Context} to use
@description
The odsMostUsedThemes widget displays the five most used themes.
@example
<example module="ods-widgets">
<file name="index.html">
<ods-catalog-context context="example" example-domain="data.opendatasoft.com">
<ods-most-used-themes context="example"></ods-most-used-themes>
</ods-catalog-context>
</file>
</example>

### `odsMultiHighcharts`

@deprecated
@name ods-widgets.directive:odsMultiHighcharts
@restrict E
@scope
@param {CatalogContext} context {@link ods-widgets.directive:odsCatalogContext Catalog Context} to use
@param {string|Object} [chartConfig=none] A complete configuration, as a object or as a base64 string. The parameter directly expects an angular expression, so a base64 string needs to be quoted.
@description
This widget can display a multiple chart generated using the "Charts" interface of Opendatasoft.

### `odsPageRefresh`

@ngdoc directive
@name ods-widgets.directive:odsPageRefresh
@scope
@restrict AE
@param {Number} [delay=10000] The number of milliseconds to wait before refreshing the page. The minimum value is `10000`.
@description
The odsPageRefresh widget can be used to periodically refresh the page.

### `odsPaginationBlock`

@ngdoc directive
@name ods-widgets.directive:odsPaginationBlock
@scope
@restrict E
@param {CatalogContext|DatasetContext} context {@link ods-widgets.directive:odsCatalogContext Catalog Context} or {@link ods-widgets.directive:odsDatasetContext Dataset Context} to use
@param {number} [perPage=10] Controls the number of results per page.
@param {boolean} [nofollow=false] When set to `true`, all links within the widget (used to change page) will contain a `rel="nofollow"` attribute.
It should be used if you don't want search engines to crawl all the pages of your widget.
@param {string} [containerIdentifier] By default, changing the page will trigger a scroll to the top of the window.
You can use this parameter to specify the ID of the element that will contain the results (e.g., "my-results")
so that the behavior is more precise:
- If your results are inside a container that is used to vertically scroll the results, the container's scroll
will be set at the start.
- If your results are inside a container that doesn't have a scrollbar, the page itself will scroll to the start of the container.
Note: In the second situation, some CSS properties may prevent the widget from understanding that it doesn't have a scrollbar. As a result, the widget won't be able to scroll scrolling to the top of the container.
This issue may be caused by the odsPaginationBlock widget slightly overflowing its container, typically because of large fonts or higher line-height settings.
In this situation, forcing a height on the widget may fix the issue.
@description
The odsPaginationBlock widget displays a pagination control that you can use to make the context "scroll" through a list of results.
The widget doesn't display results. Therefore, it should be paired with another widget.
The widget doesn't control the number of results fetched by the context. The `perPage` parameter should be the same as the `rows` parameter on the context.
If you just want to display results with a pagination system, you can use {@link ods-widgets.directive:odsResultEnumerator odsResultEnumerator}, which already includes this directive (if the relevant parameter is active on the widget).

### `odsPicto`

@ngdoc directive
@name ods-widgets.directive:odsPicto
@scope
@restrict E
@param {string} url The URL of the SVG or image to display
@param {string} localId The ID of the SVG to use in the current page
@param {string} color The color to use to fill the SVG
@param {Object} colorByAttribute An object containing a mapping between elements within the SVG, and colors.
The elements within the SVG with a matching `data-fill-id` attribute take the corresponding color.
@param {string} classes The classes to directly apply to the SVG element
@description
The odsPicto widget displays a pictogram specified by a URL or the ID of a SVG to duplicate from the same page,
and forces a fill color on it.
This element can be styled (height, width, etc.), especially if the pictogram is vectorial (SVG).
Either the `url` or `localId` attributes have to be used.
In the case of `localId`, the recommended use is to include the code of the SVG inside your HTML document,
with a `display: none` style attribute at the root, on the `svg` node. This inlined SVG will be duplicated,
the `display: none` removed, and this new duplicated and colored SVG will be inserted in place of the odsPicto
element.
All parameters expect javascript variables or literals. If you want to provide hardcoded strings, you'll have to wrap them in quotes, as shown in the following example.
@example
<example module="ods-widgets">
<file name="index.html">
<ods-picto url="'assets/opendatasoft-logo.svg'"
color="'#33629C'" style="width: 64px; height: 64px"></ods-picto>
</file>
</example>

### `odsPopIn`

@ngdoc directive
@name ods-widgets.directive:odsPopIn
@scope
@restrict E
@param {string} name The name of the pop-in, used internally to uniquely reference it (required)
@param {string} [title=''] The title displayed inside the pop-up windows
@param {number} [displayAfter=10] The delay in second before displaying the pop-up window
@param {boolean} [displayOnlyOnce=true] When set to `false`, the pop-up window will be displayed at each browsing session of the user.
@description
The odsPopIn widget displays a pop-in on the page with the provided content.
You can define the time before displaying the pop-in (the timer start when the widget is loaded).
In the content, you can access a `hidePopIn()` function that you can use in an `ng-click`.
@example
<example module="ods-widgets">
<file name="index.html">
<ods-pop-in display-after="5" name="test" display-only-once="false">
<div style="text-align: center;">
<i class="fa fa-thumbs-o-up" style="color: #FFD202; font-size: 40px"></i>
</div>
<div style="text-align: center;">
<h2>
Signup to improve your data experience
</h2>
</div>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.
</p>
<div style="text-align: center;">
<a href="" ng-click="hidePopIn()">Signup now to improve your experience <i class="fa fa-arrow-right"></i></a>
</div>
</ods-pop-in>
</file>
</example>

### `odsRecordImage`

@ngdoc directive
@name ods-widgets.directive:odsRecordImage
@restrict E
@scope
@param {DatasetContext} context {@link ods-widgets.directive:odsDatasetContext Dataset Context} to use
@param {Object} record Record to take the image from
@param {string} field <i>(mandatory)</i> Field to use.
@param {string} [domainUrl=none] The base URL of the domain where the dataset can be found. By default, the current domain is used.
@description
The odsRecordImage widget displays an image from a record.

### `odsResultEnumerator`

@ngdoc directive
@name ods-widgets.directive:odsResultEnumerator
@scope
@restrict E
@param {CatalogContext|DatasetContext} context {@link ods-widgets.directive:odsCatalogContext Catalog Context} or {@link ods-widgets.directive:odsDatasetContext Dataset Context} to use
@param {number} [max=10] Maximum number of results to show. The value can be changed dynamically using a variable.
@param {boolean} [showHitsCounter=false] Displays the number of hits (search results). This is the number of results available on the API, not the number of results displayed in the widget.
@param {boolean} [showPagination=false] Displays a pagination block below the results to be able to browse them all.
@description
The odsResultEnumerator widget enumerates the search results (records for a {@link ods-widgets.directive:odsDatasetContext Dataset Context}, datasets for a {@link ods-widgets.directive:odsCatalogContext Catalog Context}). It repeats the template (the content of the directive element) for each of them.
If used with a {@link ods-widgets.directive:odsCatalogContext Catalog Context}, for each result, the following AngularJS variables are available:
* `item.datasetid`: Dataset identifier of the dataset
* `item.metas`: An object holding the key/values of metadata for this dataset
If used with a {@link ods-widgets.directive:odsDatasetContext Dataset Context}, for each result, the following AngularJS variables are available:
* `item.datasetid`: Dataset identifier of the dataset this record belongs to
* `item.fields`: an object hold all the key/values for the record
* `item.geometry`: if the record contains geometrical information, this object is present and holds its GeoJSON representation
@example
<example module="ods-widgets">
<file name="index.html">
<ods-catalog-context context="example"
example-domain="https://data.opendatasoft.com">
<ul>
<ods-result-enumerator context="example">
<li>
<strong>{{item.metas.title}}</strong>
(<a ng-href="{{context.domainUrl + '/explore/dataset/' + item.datasetid + '/'}}" target="_blank">{{item.datasetid}}</a>)
</li>
</ods-result-enumerator>
</ul>
</ods-catalog-context>
</file>
</example>

### `odsResults`

@ngdoc directive
@name ods-widgets.directive:odsResults
@scope
@restrict A
@param {string} [odsResults=results] Variable name to use
@param {CatalogContext|DatasetContext} odsResultsContext {@link ods-widgets.directive:odsCatalogContext Catalog Context} or {@link ods-widgets.directive:odsDatasetContext Dataset Context} to use
@param {number} [odsResultsMax=10] Maximum number of results to show. The value can be changed dynamically using a variable.
@description
The odsResults widget exposes the results of a search as an array in a variable available in the scope.
It can be used with the AngularJS ngRepeat directive to build a list of results simply.
It also adds to the context variable a `nhits` property containing the total number of records matching the query regardless of the odsResultsMax value.
@example
<example module="ods-widgets">
<file name="index.html">
<ods-dataset-context context="tree"
tree-dataset="les-arbres-remarquables-de-paris"
tree-domain="https://documentation-resources.opendatasoft.com/"
tree-parameters="{'sort': '-objectid'}">
<table class="table table-bordered table-condensed table-striped">
<thead>
<tr>
<th>Tree name</th>
<th>Addrese</th>
</tr>
</thead>
<tbody>
<tr ng-repeat="item in items"
ods-results="items"
ods-results-context="tree"
ods-results-max="10">
<td>{{ item.fields.libellefrancais }}</td>
<td>{{ item.fields.adresse }}</td>
</tr>
</tbody>
</table>
</ods-dataset-context>
</file>
</example>
@example
<example module="ods-widgets">
<file name="index.html">
<ods-dataset-context context="tree"
tree-dataset="les-arbres-remarquables-de-paris"
tree-domain="https://documentation-resources.opendatasoft.com/">
<p ods-results="items" ods-results-context="tree" ods-results-max="10">
Total number of trees : {{ tree.nhits }}
</p>
</ods-dataset-context>
</file>
</example>

### `odsReuses`

@ngdoc directive
@name ods-widgets.directive:odsReuses
@scope
@restrict E
@param {CatalogContext} context {@link ods-widgets.directive:odsCatalogContext Catalog Context} to use
@description
The odsReuses widget displays all reuses published on a domain in an infinite list of large boxes, presenting reuses in a clear display. The list shows the more recent reuses first.
You can optionally insert HTML code inside the `<ods-reuses></ods-reuses>` element, in which case it will be used
as a template for each displayed reuse. The following variables are available in the template:
* `reuse.url`: URL to the reuse's dataset page
* `reuse.title`: Title of the reuse
* `reuse.thumbnail`: URL to the thumbnail of the reuse
* `reuse.description`: Description of the reuse
* `reuse.created_at`: ISO datetime of reuse's original submission (can be used as `reuse.created_at|moment:'LLL'` to format it)
* `reuse.dataset.title`: Title of the reuse's dataset
* `reuse.user.last_name`: Last name of the reuse's submitter
* `reuse.user.first_name`: First name of the reuse's submitter
@example
<example module="ods-widgets">
<file name="index.html">
<ods-catalog-context context="paris" paris-domain="https://opendata.paris.fr">
<ods-reuses context="paris"></ods-reuses>
</ods-catalog-context>
</file>
</example>

### `odsSearchbox`

@ngdoc directive
@name ods-widgets.directive:odsSearchbox
@scope
@restrict E
@param {string} placeholder Controls the text to display as a placeholder when the search box is empty.
@param {string} sort Controls the default sort order for the results.
@param {CatalogContext} [context=none] {@link ods-widgets.directive:odsCatalogContext Catalog Context} indicating the domain to redirect the user to show the search results.
If `none`, the search is performed on the local domain; that is, the domain to which the widget has been added.
@param {string} [autofocus] Adds the autofocus attribute to set the focus in the text search input. No value is required.
@param {string} [formId=none] Configures the `id` attribute of the form generated internally by the widget, which can be used from other HTML elements. For example, it can be used to submit the search from another button.
@description
The odsSearchbox widget displays a wide search box that redirects the search on the Explore homepage of the domain.

### `odsSelect`

@ngdoc directive
@name ods-widgets.directive:odsSelect
@scope
@restrict E
@param {array} options The input array of value which feeds the list of options
@param {array} selectedValues The variable name to use to store the selected options' values
@param {expression} [labelModifier=none] An expression to apply on the options' label
@param {expression} [valueModifier=none] An expression to apply on the options' value. This parameter is used to modify the form of the values exposed by `selected-value`.
@param {expression} [onChange=none] An expression to evaluate whenever an option has been (de)selected
@param {boolean} [multiple=false] When set to `true`, the menu will support multiple selections.
@param {boolean} [isLoading=false] Specifies whether the widget should initially display a loader. This parameter will be automatically set to `false` as soon as options are loaded.
@param {boolean} [disabled=false] Specifies whether the widget should be disabled.
@param {string} [placeholder="Select one or more elements" or "Select one element"] Specifies a short hint that describes the expected value of the select field.
@description
The odsSelect widget shows a list of options from which users can select one or more options. This list can be made up of strings or objects.
If the `options` variable provided to the widget represents a simple array of strings, option labels and values will be automatically calculated by the widget.
If the options provided to the widget are objects, use the `label-modifier` and `value-modifier` parameters to define how to handle those objects.
The `label-modifier` and `value-modifier` parameters each take an expression applied to each object representing an option.
Finally, the selection will be stored in the variable specified in the `selected-values` parameter.
<h1>Explanation of the examples</h1>
<h2>First example</h2>
The first example shows a list of options from which users can select one or multiple trees.
This example uses the `ods-dataset-context`, `ods-result`, and `ods-select` widgets:
- The `ods-dataset-context` declares a context based on the `les-arbres-remarquables-de-paris` dataset.
- The `ods-results` widget is nested within `ods-dataset-context`. It stores the result of the search request in the variable `items`.
The value of the variable `items` will have this form:
<pre>
[
{ fields: { libellefrancais: "Noyer", espece: "nigra",  ... }, ... },
{ fields: { libellefrancais: "Marronnier", espece: "hippocastanum",  ... }, ... },
{ fields: { libellefrancais: "Chêne", espece: "cerris",  ... }, ... },
...
]
</pre>
- The `ods-select` widget is nested within `ods-results`.
It defines the list of options users can select, using the `options` parameter set to `items`.
`items` corresponds to the variable storing the results in `ods-results`.
<b>The parameter `label-modifier`</b>
In this example, the desired value for the option label is the field `libellefrancais` from the source dataset.
To access the value of this field, the `label-modifier` parameter for `ods-select` is set to `"fields.libellefrancais"`.
<b>The parameter `value-modifier`</b>
The desired structure for the values returned by `selected-values` is the following:
<pre>
[
{ name: "Noyer", species: "nigra" },
{ name: "Marronnier", species: "hippocastanum" },
...
]
</pre>
To achieve this, the `value-modifier` for `ods-select` is set to `"{ 'name': fields.libellefrancais, 'species': fields.espece }"`.
<h2>Second example</h2>
The second example shows two lists of options from which users can select one or multiple options:
- From the first list, users can select districts.
- From the second list, users can select tree species.
In the second example, context parameters are updated by injecting the selected values returned by `ods-select`.
This example uses the `ods-dataset-context`, `ods-result`, and `ods-select` widgets:
- The `ods-dataset-context` declares a context based on the `les-arbres-remarquables-de-paris` dataset.
- Two `ods-results` widgets are nested within `ods-dataset-context`. They fetch the values of the facets "arrondissement" and "libellefrancais" from the source dataset and store them in the variables `facetsArrondissement` and `facetsLibelleFrancais`, respectively.
The values of `facetsArrondissement` and `facetsLibelleFrancais` will have this form:
<pre>
[
{ count: 1, path: "PARIS 1ER ARRDT", state: "displayed", name: "PARIS 1ER ARRDT" },
{ count: 8, path: "PARIS 17E ARRDT", state: "displayed", name: "PARIS 17E ARRDT" },
{ count: 11, path: "PARIS 7E ARRDT", state: "displayed", name: "PARIS 7E ARRDT" },
...
]
</pre>
<pre>
[
{ count: 32, path: "Platane", state: "displayed", name: "Platane" },
{ count: 12, path: "Hêtre", state: "displayed", name: "Hêtre" },
{ count: 11, path: "Chêne", state: "displayed", name: "Chêne" },
...
]
</pre>
- An `ods-select` widget is nested within each `ods-results`.
They define the lists of options users can select, using the `options` parameter set to `facetsArrondissement` and `facetsLibelleFrancais`, respectively.
To update the context each time an option is selected, the `selected-values` parameters for `ods-select` are set to `ctx.parameters['refine.arrondissement']` and `ctx.parameters['refine.libellefrancais']`, respectively.
@example
<example module="ods-widgets">
<file name="first_example.html">
<ods-dataset-context
context="ctx"
ctx-domain="https://documentation-resources.opendatasoft.com/"
ctx-dataset="les-arbres-remarquables-de-paris">
<div ods-results="items" ods-results-context="ctx" ods-results-max="10">
<ods-select
disabled="items.length < 0"
selected-values="selectedTrees"
multiple="true"
options="items"
label-modifier="fields.libellefrancais"
value-modifier="{ 'name': fields.libellefrancais, 'species': fields.espece }">
</ods-select>
</div>
</ods-dataset-context>
</file>
</example>
<example module="ods-widgets">
<file name="second_example.html">
<ods-dataset-context
context="ctx"
ctx-domain="https://documentation-resources.opendatasoft.com/"
ctx-dataset="les-arbres-remarquables-de-paris"
ctx-parameters="{ 'disjunctive.arrondissement': true, 'disjunctive.libellefrancais': true }">
<div ods-facet-results="facetsArrondissement"
ods-facet-results-context="ctx"
ods-facet-results-facet-name="arrondissement">
<ods-select
options="facetsArrondissement"
selected-values="ctx.parameters['refine.arrondissement']"
label-modifier="name + ' (' + count + ')'"
value-modifier="name"
placeholder="Select one or more districts"
multiple="true">
</ods-select>
<br/>
</div>
<div ods-facet-results="facetsLibelleFrancais"
ods-facet-results-context="ctx"
ods-facet-results-facet-name="libellefrancais">
<ods-select
options="facetsLibelleFrancais"
selected-values="ctx.parameters['refine.libellefrancais']"
label-modifier="name"
value-modifier="name"
placeholder="Select one or more trees"
multiple="true">
</ods-select>
<br/>
</div>
<ods-table
context="ctx"
displayed-fields="libellefrancais, genre, arrondissement">
</ods-table>
</ods-dataset-context>
</file>
</example>

### `odsSimpleTab`

@ngdoc directive
@name ods-widgets.directive:odsSimpleTab
@restrict E
@requires ods-widgets.directive:odsSimpleTabs
@param {string} label The label to be displayed in the tab
@param {string} fontawesomeClass The Font Awesome icon name used for the tab, without the 'fa-' prefix
@param {boolean} [keepContent=false] By default, the widget destroys and rebuilds the pane content at deselection/selection. It acts like an ng-if when the panel is selected/deselected.
When set to `true`, the widget does not destroy and rebuild the pane content at deselection/selection.

### `odsSimpleTabs`

@ngdoc directive
@name ods-widgets.directive:odsSimpleTabs
@scope
@restrict E
@description
The odsSimpleTabs widget generates a tabbed interface that allows you to switch between separate views.
@param {string} [syncToScope='simpleTabActive'] Name of parent scope variable to sync the current active tab

### `odsSlideshow`

@ngdoc directive
@name ods-widgets.directive:odsSlideshow
@restrict E
@scope
@param {DatasetContext} context {@link ods-widgets.directive:odsDatasetContext Dataset Context} to use
@param {string} imageField The name of the field containing the image
@param {string} [titleFields] A comma-separated list of field names to display as comma-separated values in the title
@param {string} [domainUrl] The URL of the domain
@description
The odsSlideshow widget displays an image slideshow of a dataset containing media with thumbnails (images, PDF files, etc.).
You will need to set a height for the `.ods-slideshow` class to work correctly or set the height through the style attribute.
You can also include a tooltip to access the image's record through the `record` variable.
@example
<example module="ods-widgets">
<file name="index.html">
<ods-dataset-context context="affiches"
affiches-domain="https://documentation-resources.opendatasoft.com/"
affiches-dataset="affiches-anciennes">
<ods-slideshow context="affiches"
image-field="image"
title-fields="titre"
style="height: 300px">
<strong>{{ record.fields.titre }}</strong> <br>
</ods-slideshow>
</ods-dataset-context>
</file>
</example>

### `odsSocialButtons`

@ngdoc directive
@name ods-widgets.directive:odsSocialButtons
@scope
@restrict A
@param {string} [buttons='twitter,facebook,linkedin,email'] A comma-separated list of buttons you want to display
@param {string} [title=current page's title] Title of the post on social media
@param {string} [url=current page's url] URL attached to the post on social media
@description
The odsSocialButtons widget displays a series of buttons for easy sharing on social media.
@example
<example module="ods-widgets">
<file name="index.html">
<ods-social-buttons buttons="twitter,linkedin"></ods-social-buttons>
</file>
</example>

### `odsSpinner`

@ngdoc directive
@name ods-widgets.directive:odsSpinner
@scope
@restrict E
@description
The odsSpinner widget displays the custom Opendatasoft spinner.
Its size and color match the current font.
If the browser doesn't support SVG animation via CSS, an animated GIF will be displayed instead.
@example
<example module="ods-widgets">
<file name="index.html">
<ods-spinner></ods-spinner> Loading
</file>
</example>

### `odsSubaggregation`

@ngdoc directive
@name ods-widgets.directive:odsSubaggregation
@scope
@restrict A
@param {string} odsSubaggregation Analysis results
@param {number} odsSubaggregationSerie* Aggregation expression
@description
The odsSubaggregation widget computes aggregations on an analysis result.
You can use this widget with the AngularJS ngRepeat directive to simply build a table of analysis results.
@example
<example module="ods-widgets">
<file name="index.html">
<ods-dataset-context context="tree" tree-dataset="les-arbres-remarquables-de-paris" tree-domain="https://documentation-resources.opendatasoft.com/">
<div
ods-analysis="analysis"
ods-analysis-context="tree"
ods-analysis-max="10"
ods-analysis-x-genre="genre"
ods-analysis-x-espace="espece"
ods-analysis-sort="circonference"
ods-analysis-serie-height="AVG(hauteur_en_m)"
ods-analysis-serie-height-cumulative="false"
ods-analysis-serie-girth="AVG(circonference_en_cm)">
<div
ods-subaggregation="analysis.results"
ods-subaggregation-serie-maxheight="MAX(height)"
ods-subaggregation-serie-avggirth="MEAN(girth)">
max height: {{ results[0].maxheight|number:2 }}<br>
average girth: {{ results[0].avggirth|number:2 }}
</div>
</div>
</ods-dataset-context>
</file>
</example>

### `odsTable`

@ngdoc directive
@name ods-widgets.directive:odsTable
@restrict E
@scope
@param {DatasetContext} context {@link ods-widgets.directive:odsDatasetContext Dataset Context} to use
@param {string} [displayedFields=all] A comma-separated list of fields to display. By default, all the available fields are displayed.
@description
The odsTable widget displays a table view of a dataset, with infinite scroll and an ability to sort columns depending on the column types.
@example
<example module="ods-widgets">
<file name="index.html">
<ods-dataset-context context="commute"
commute-domain="https://documentation-resources.opendatasoft.com/"
commute-dataset="average-commute-time-by-county">
<ods-table context="commute"></ods-table>
</ods-dataset-context>
</file>
</example>

### `odsTagCloud`

@ngdoc directive
@name ods-widgets.directive:odsTagCloud
@scope
@restrict E
@param {CatalogContext|DatasetContext} context
{@link ods-widgets.directive:odsCatalogContext Catalog Context} or
{@link ods-widgets.directive:odsDatasetContext Dataset Context} to use
@param {string} facetName Name of the facet to build the tag cloud from
@param {number} [max=all] Maximum number of tags to show in the cloud
@param {string} [redirectTo=none] URL.
If specified, a click on any tag will redirect to the given URL and apply the filter there.
@param {CatalogContext|DatasetContext} [contextToRefine=current context] Specifies the context that will be
refined. If not specified at all, the refined context will be the one defined through the `context` parameter.
@description
The odsTagCloud widget displays a "tag cloud" of the values available in a facet.
This facet can be the facet of a dataset or a facet from the dataset catalog.
The "weight" (size) of a tag depends on the number of occurrences (count) for this tag.
@example
<example module="ods-widgets">
<file name="index.html">
<ods-catalog-context context="catalog" catalog-domain="data.opendatasoft.com">
<ods-tag-cloud context="catalog" facet-name="keyword"></ods-tag-cloud>
</ods-catalog-context>
</file>
</example>

### `odsTextSearch`

@ngdoc directive
@name ods-widgets.directive:odsTextSearch
@scope
@restrict E
@param {CatalogContext|DatasetContext|CatalogContext[]|DatasetContext[]} context <i>(mandatory)</i> {@link ods-widgets.directive:odsCatalogContext Catalog Context}, {@link ods-widgets.directive:odsDatasetContext Dataset Context}, or array of context to use
@param {string} [placeholder=none] Text to display as a placeholder when the search box is empty
@param {string} [field=none] Name of a field the search will be restricted on (i.e., the widget will only allow to search on the textual content of the chosen field).
If more than one context is declared, it is possible to specify different fields depending on these contexts, using the following syntax: mycontext-field. If a specific field is not set for a context, the value of the field parameter will be used by default. The search will be a simple text search that won't support query languages and operators.
@param {string} [suffix=none] Changes the default `q` query parameter into `q.suffixValue`. This parameter prevents widgets from overriding one another, for instance when multiple odsTextSearch widgets are used on the same page.
@param {string} autofocus Makes the search box automatically selected at loading of the page to start typing the search without selecting the search box manually beforehand. No value is required for this parameter to function.
@param {string} id Adds an `id` attribute to the search's text box, for example, to integrate the widget to a clickable label.
@description
The odsTextSearch widget displays a search box to perform a full-text search in a context.
@example
<example module="ods-widgets">
<file name="simple_text_search.html">
<ods-dataset-context context="events"
events-domain="https://documentation-resources.opendatasoft.com/"
events-dataset="evenements-publics-openagenda-extract">
<ods-text-search context="events" field="titre"></ods-text-search>
<ods-table context="events"></ods-table>
</ods-dataset-context>
</file>
</example>
<example module="ods-widgets">
<file name="text_search_with_multiple_contexts.html">
<ods-dataset-context context="events,trees"
events-domain="https://documentation-resources.opendatasoft.com/"
events-dataset="evenements-publics-openagenda-extract"
trees-domain="https://documentation-resources.opendatasoft.com/"
trees-dataset="les-arbres-remarquables-de-paris">
<ods-text-search context="[events,trees]"
events-field="titre"
trees-field="libellefrancais"></ods-text-search>
<ods-table context="events"></ods-table>
<ods-table context="trees"></ods-table>
</ods-dataset-context>
</file>
</example>

### `odsThemeBoxes`

@ngdoc directive
@name ods-widgets.directive:odsThemeBoxes
@scope
@restrict E
@param {CatalogContext} context {@link ods-widgets.directive:odsCatalogContext Catalog Context} to pull the theme list from
@description
The odsThemeBoxes widget enumerates the themes available on the domain by showing their pictograms and the number of datasets they contain.
They require the `themes` setting to be configured in {@link ods-widgets.ODSWidgetsConfigProvider ODSWidgetsConfig}.

### `odsThemePicto`

@ngdoc directive
@name ods-widgets.directive:odsThemePicto
@scope
@restrict E
@param {string} theme The label of the theme to display the pictogram of
@description
The odsThemePicto widget displays the pictogram of a theme based on the `themes` setting in {@link ods-widgets.ODSWidgetsConfigProvider ODSWidgetsConfig}.
This element can be styled (height, width, etc.), especially if the pictogram is vectorial (SVG).

### `odsTimer`

@ngdoc directive
@name ods-widgets.directive:odsTimer
@scope
@restrict E
@param {Number} [delay=1000] The number of milliseconds to wait before executing the expression. The minimum value is `100`.
@param {Expression} [stopCondition=false] An AngularJS expression returning 'true' or 'false'. The timer stops when the condition is 'false'.
@param {Boolean} [autoStart=false] Starts the timer automatically when the page loads.
@param {Expression} [exec] An AngularJS expression to execute.
@description
The odsTimer widget is a simple timer. It executes the AngularJS expression `exec` every `delay` milliseconds.
It doesn't stop until the user clicks on the pause button or when the `stopCondition` is true.
It can be used to animate dashboards to go over a date field and add one day every two seconds, like in the following example. "From" and "To" values will increase by one day until the user clicks on the pause button.
@example
<example module="ods-widgets">
<file name="index.html">
<ods-dataset-context context="events"
events-domain="https://documentation-resources.opendatasoft.com/"
events-dataset="evenements-publics-openagenda-extract">
<div ng-init="values = {'from':undefined,'to':undefined}">
<ods-timerange context="events"
default-from="2019/01/01"
default-to="2019/02/31"
from="values.from"
to="values.to">
</ods-timerange>
<ods-timer stop-condition="false"
delay="3000"
exec="values.from = (values.from | momentadd : 'day' : 1 | moment : 'YYYY-MM-DD');
values.to = (values.to | momentadd : 'day' : 1 | moment : 'YYYY-MM-DD');">
</ods-timer>
<div>
<h1 ods-aggregation="cnt"
ods-aggregation-context="events"
ods-aggregation-function="COUNT">
# events : {{ cnt  | number}}
</h1>
<ods-table context="events"></ods-table>
</div>
</div>
</ods-dataset-context>
</file>
</example>

### `odsTimerange`

@ngdoc directive
@name ods-widgets.directive:odsTimerange
@restrict E
@scope
@param {DatasetContext|DatasetContext[]} context {@link ods-widgets.directive:odsDatasetContext Dataset Context} or array of context to use
@param {string=} [timeField=first date/datetime field available] The value is the name of the field (date or datetime) to filter on.<br><br><em>Use this form if you apply the timerange to only one context.</em>
@param {string=} [{context}TimeField=first date/datetime field available] The value is the name of the field (date or datetime) to filter on.<br><br><em>Use this form when you apply the timerange to multiple contexts. {context} must be replaced by the context name.</em>
@param {string} [defaultFrom=none] Default datetime for the "from" field: either "yesterday", "now" or a string representing a date. This value always uses the `YYYY-MM-DD HH:mm` or `YYYY-MM-DD` format.
@param {string} [defaultTo=none] Default datetime for the "to" field: either "yesterday", "now", or a string representing a date. This value always uses the `YYYY-MM-DD HH:mm` or `YYYY-MM-DD` format.
@param {string} [displayTime=true] Defines if the date selector displays the time selector as well
@param {string} [dateFormat='YYYY-MM-DD HH:mm'] Defines the format for the date displayed in the inputs
@param {string} [suffix='fieldname'] (optional) Adds a suffix to the q.timerange, q.from_date or q.to_date parameter. This prevents widgets from overriding each other.
@param {string} [labelFrom='From'] Sets the label before the first input
@param {string} [labelTo='to'] Sets the label before the second input
@param {string} [placeholderFrom=''] Sets the label before the first input
@param {string} [placeholderTo=''] Sets the label before the second input
@param {string} [to=none] Sets a variable that will get the iso formatted value of the first input
@param {string} [from=none] Sets a variable that will get the iso formatted value of the second input
@description
The odsTimerange widget displays two fields to select the two bounds of a date and time range.
The values for the `defaultTo` and `defaultFrom` parameters MUST be in `YYYY-MM-DD HH:mm`
(or `YYYY-MM-DD` for date only) whatever the displayFormat.
@example
<example module="ods-widgets">
<file name="index.html">
<ods-dataset-context context="events"
events-domain="https://documentation-resources.opendatasoft.com/"
events-dataset="evenements-publics-openagenda-extract">
<ods-timerange context="events"
default-from="yesterday"
default-to="now"
display-time="true"></ods-timerange>
<ods-table context="events"></ods-table>
</ods-dataset-context>
<ods-dataset-context context="events"
events-domain="https://documentation-resources.opendatasoft.com/"
events-dataset="evenements-publics-openagenda-extract">
<ods-timerange context="events"
date-format="DD/MM/YYYY"
default-from="2019-01-01"
default-to="2019-02-01"></ods-timerange>
<ods-table context="events"></ods-table>
</ods-dataset-context>
<ods-dataset-context context="events"
events-domain="https://documentation-resources.opendatasoft.com/"
events-dataset="evenements-publics-openagenda-extract">
<div ods-datetime="datenow">
<ods-timerange context="events"
date-format="DD/MM/YYYY"
default-from="{{ datenow|moment:'YYYY-MM-DD' }}"
default-to="{{ (datenow | momentadd:'months':3)|moment:'YYYY-MM-DD' }}"></ods-timerange>
</div>
<ods-table context="events"></ods-table>
</ods-dataset-context>
</file>
</example>

### `odsTimescale`

@ngdoc directive
@name ods-widgets.directive:odsTimescale
@restrict E
@scope
@param {DatasetContext|DatasetContext[]} context {@link ods-widgets.directive:odsDatasetContext Dataset Context} or array of context to use
@param {string=} [timeField=first date/datetime field available] Name of the field (date or datetime) to filter on
@param {string=} [*TimeField=first date/datetime field available] For each context, you can set the name of the field (date or datetime) to filter on.
@param {string=} [defaultValue=everything] Sets the default timescale.
@description
The odsTimescale widget displays a control to select:
* the last day,
* the last week,
* the last month, or
* the last year.
@example
<example module="ods-widgets">
<file name="index.html">
<ods-dataset-context context="events"
events-domain="https://documentation-resources.opendatasoft.com/"
events-dataset="evenements-publics-openagenda-extract">
<ods-timescale context="events" default-value="everything"></ods-timescale>
<ods-map context="events"></ods-map>
</ods-dataset-context>
</file>
</example>

### `odsToggleModel`

@ngdoc directive
@name ods-widgets.directive:odsToggleModel
@restrict A
@scope
@param {Object} odsToggleModel Object to apply the toggle on
@param {string} odsToggleKey The key holding the toggled value
@param {string} odsToggleValue The toggled value
@param {string} [odsStoreAs=array] The type of the resulting variable. The authorized values are 'array' and 'csv'.
@description
The odsToggleModel widget, when used on a checkbox, allows the checkbox to be used to "toggle" a value in an object. In other words, the value is added when the checkbox is selected and removed when the checkbox is cleared.
Multiple checkboxes can be used on the same model and key, in which case if two or more are toggled, an array will be created to hold the values.
@example
<example module="ods-widgets">
<file name="index.html">
<ods-catalog-context context="catalog"
catalog-domain="https://data.opendatasoft.com"
catalog-parameters="{'disjunctive.publisher':true}">
<label><input type="checkbox" ods-toggle-model="catalog.parameters" ods-toggle-key="refine.publisher" ods-toggle-value="OpenStreetMap"> OpenStreetMap</label>
<label><input type="checkbox" ods-toggle-model="catalog.parameters" ods-toggle-key="refine.publisher" ods-toggle-value="Eurostat"> Eurostat</label>
<div ods-results="items" ods-results-context="catalog" ods-results-max="10">
{{items.length}}
<table>
<tr>
<td>Publisher</td>
<td>Dataset</td>
</tr>
<tr ng-repeat="item in items">
<td>{{item.metas.publisher}}</td>
<td>{{item.datasetid}}</td>
</tr>
</table>
</div>
</ods-catalog-context>
</file>
</example>

### `odsTopPublishers`

@ngdoc directive
@name ods-widgets.directive:odsTopPublishers
@scope
@restrict E
@param {CatalogContext} context {@link ods-widgets.directive:odsCatalogContext Catalog Context} to use
@description
The odsTopPublishers widget displays the five top publishers.
@example
<example module="ods-widgets">
<file name="index.html">
<ods-catalog-context context="example" example-domain="data.opendatasoft.com">
<ods-top-publishers context="example"></ods-top-publishers>
</ods-catalog-context>
</file>
</example>

### `odsTwitterTimeline`

@deprecated
@name ods-widgets.directive:odsTwitterTimeline
@restrict E
@scope
@param {string} widgetId The identifier of the Twitter widget you want to integrate. See https://twitter.com/settings/widgets for more information.
@param {number} [width=300] Forces the width of the Twitter timeline widget.
@param {number} [height=600] Forces the height of the Twitter timeline widget.
@description
Note: this twitter works with the former Twitter Widget system, which provided an ID, and was available until
late 2016. Newly created Twitter Widgets are not supported, and can usually directly be integrated in pages
by pasting the code given by Twitter.
This widget integrates a Twitter "widget" using the widget ID provided by Twitter.
@example
<example module="ods-widgets">
<file name="index.html">
<ods-twitter-timeline widget-id="502475045042544641"></ods-twitter-timeline>
</file>
</example>

### `odsWidgetTooltip`

@ngdoc directive
@name ods-widgets.directive:odsWidgetTooltip
@restrict A
@transclude
@description
The odsWidgetTooltip widget is a helper for displaying custom tooltips.
It allows to configure the usable fields in the tooltip and the template and does the HTML rendering giving back the compiled HTML to the calling widget.
By default, the template for the custom tooltip can access the record and a displayedFields array that lists the record fields that should appear in the tooltip.
@example
<example module="ods-widgets">
<file name="index.html">
<ods-dataset-context context="affiches"
affiches-domain="https://documentation-resources.opendatasoft.com/"
affiches-dataset="affiches-anciennes">
<ods-media-gallery context="affiches" ods-auto-resize ods-widget-tooltip>
<h3>My custom tooltip</h3>
{{ getRecordTitle(record) }}
</ods-media-gallery>
</ods-dataset-context>
</file>
</example>

### `refineOnClick`

*Pas de documentation disponible*

### `refineOnClickContext`

*Pas de documentation disponible*

### `translate`

@ngdoc directive
@module gettext
@name translate
@requires gettextCatalog
@requires https://docs.angularjs.org/api/ng/service/$parse $parse
@requires https://docs.angularjs.org/api/ng/service/$animate $animate
@requires https://docs.angularjs.org/api/ng/service/$compile $compile
@requires https://docs.angularjs.org/api/ng/service/$window $window
@restrict AE
@param {String} [translatePlural] plural form
@param {Number} translateN value to watch to substitute correct plural form
@param {String} translateContext context value, e.g. {@link doc:context Verb, Noun}
@description Annotates and translates text inside directive
Full interpolation support is available in translated strings, so the following will work as expected:
```js
<div translate>Hello {{name}}!</div>
```

---

## Filtres (47)

### `capitalize`

@ngdoc filter
@name ods-widgets.filter:capitalize
@function
@param {string} text A string to capitalize
@return {string} The input string, capitalized (ie with its first character in capital letter)

### `displayImageValue`

@ngdoc filter
@name ods-widgets.filter:isEmpty
@function
@param {Object} object An object.
@return {Boolean} Return true if the object is empty (has no key)

### `fieldsFilter`

@ngdoc filter
@name ods-widgets.filter:fieldsFilter
@function
@param {string[]} fieldNames A list of field names.
@param {Object[]} fields A list of fields as returned by the API.
@return {Object[]} A sublist of the `fields` input, containing only fields which are referenced in the
`fieldNames` attribute.

### `fieldsForLanguageDisplay`

*Pas de documentation disponible*

### `fieldsForVisualization`

*Pas de documentation disponible*

### `filesize`

*Pas de documentation disponible*

### `firstValue`

@ngdoc filter
@name ods-widgets.filter:firstValue
@function
@param {Array} array An array of anything
@return {String|Number|Boolean|Array|Object} If the input value is an array, returns the first of its
values, otherwise return the value itself.

### `formatFieldValue`

*Pas de documentation disponible*

### `fromjson`

@ngdoc filter
@name ods-widgets.filter:fromjson
@function
@param {string} A string value containing a stringified JSON object
@return {json} The resulting, parsed, Json object
@example
<pre>
value = '{\"key\":\"value\"}'
</pre>
<pre>value|fromjson</pre>
<pre>
{
"key": "value"
}
</pre>

### `imageUrl`

@ngdoc filter
@name ods-widgets.filter:imageUrl
@function
@param {Object} fieldValue A record field of type file
@param {DatasetContext|CatalogContext} context The context from which the record is extracted
@return {string} A url pointing to the file itself.

### `imagify`

@ngdoc filter
@name ods-widgets.filter:imageify
@function
@param {string} url A url pointing to an image file (with a jpg, jpeg, png or gif extension)
@return {string} An `img` tag pointing to the image.

### `isAfter`

@ngdoc filter
@name ods-widgets.filter:isAfter
@function
@param {string|Date|Number|Array|Moment} date1 A date
@param {string|Date|Number|Array|Moment} date2 Another date, which doesn't need to be in the same format as
date1.
@return {Boolean} Whether date1 is strictly after date2 or not, down to the millisecond.

### `isBefore`

@ngdoc filter
@name ods-widgets.filter:isBefore
@function
@param {string|Date|Number|Array|Moment} date1 A date
@param {string|Date|Number|Array|Moment} date2 Another date, which doesn't need to be in the same format as
date1.
@return {Boolean} Whether date1 is strictly before date2 or not, down to the millisecond.

### `isDefined`

@ngdoc filter
@name ods-widgets.filter:isDefined
@function
@param {string|number|Object|Boolean} value Any variable
@return {Boolean} true if the value is defined.

### `isEmpty`

@ngdoc filter
@name ods-widgets.filter:isEmpty
@function
@param {Object} object An object.
@return {Boolean} Return true if the object is empty (has no key)

### `join`

@ngdoc filter
@name ods-widgets.filter:join
@function
@param {string[]} values  A list of strings
@param {string} [separator] The separator (default: `', '`)
@return {string} All strings joined with the given separator.

### `keys`

@ngdoc filter
@name ods-widgets.filter:keys
@function
@param {Object} object An object.
@return {string[]} The keys of the input object.

### `math`

@ngdoc filter
@name ods-widgets.filter:math
@function
@param {string|Date|Number|Array} value A numerical value to process
@param {string} function A Math library function. Can be any of:
<b>abs</b> (absolute value),
<b>cbrt</b> (cube root),
<b>ceil</b> (smallest integer >= value),
<b>cos</b> (cosine),
<b>exp</b> (E**value),
<b>floor</b> (largest integer <= value),
<b>log` or `ln</b> (natural logarithm),
<b>log2</b> (base 2 logarithm),
<b>log10</b> (base 10 logarithm),
<b>pow</b> (power) ex: {{ val | math : 'pow' : 2 }} for val^2
<b>random</b> (pseudo-random number between 0 and 1 * value),
<b>round</b> (value rounded to the nearest integer),
<b>sign</b> (sign of value, pos. = 1, neg = -1, zero = 0),
<b>sin</b> (sine),
<b>sqrt</b> (positive square root),
<b>tan</b> (tangent),
<b>trunc</b> (integer part of value)
<br/> or static properties: <b>PI</b> or <b>E</b>.
@return {Number} The result

### `max`

*Pas de documentation disponible*

### `min`

*Pas de documentation disponible*

### `moment`

@ngdoc filter
@name ods-widgets.filter:moment
@function
@description Render a given date in a specified format.
@param {string|Date|Number|Array|Moment} date A date
@param {string} format See http://momentjs.com/docs/#/displaying/format/ for the full list of options
@return {string} The input date, formatted.

### `momentadd`

@ngdoc filter
@name ods-widgets.filter:momentadd
@function
@param {string|Date|Number|Array|Moment} date A date
@param {string} precision A unit describing the type of the `number` parameter. Can be any of `years`,
`quarters`, `months`, `weeks`, `days`, `hours`, `minutes`, `seconds` or `milliseconds`.
@param {number} number How many years, hours, minutes (depending on `precision`) should be added. Can be a
negative number.
@return {Moment} A date

### `momentdiff`

@ngdoc filter
@name ods-widgets.filter:momentdiff
@description This filter returns the difference between two dates, in the given measurement. For example
you could use it to calculate how many days there are between two dates.
@function
@param {string|Date|Number|Array|Moment} date1 A date
@param {string|Date|Number|Array|Moment} date2 A date
@param {string|Date|Number|Array|Moment} [measurement=milliseconds] The measurement to use ("years",
"months", "weeks", "days", "hours", "minutes", and "seconds"). By default, milliseconds are used.
@return {string} The difference in measurement between the two dates.

### `nofollow`

@ngdoc filter
@name ods-widgets.filter:nofollow
@function
@param {string} html A string of html code.
@return {string} The input html code with all link tags now including the attributes `target="_blank"` and
`rel="nofollow"`

### `normalize`

@ngdoc filter
@name ods-widgets.filter:normalize
@function
@param {string} text Some text
@return {string} The text cleaned of all of its diacritical signs.

### `numKeys`

@ngdoc filter
@name ods-widgets.filter:numKeys
@function
@param {Object} object An object.
@return {string[]} The number of keys of the input object.

### `odsSelectFilter`

*Pas de documentation disponible*

### `odsSelectItemIsSelected`

*Pas de documentation disponible*

### `prettyText`

Prepares a text value to be displayed

### `propagateAppendedURLParameters`

@ngdoc filter
@name ods-widgets.filter:isAfter
@function
@param {string|Date|Number|Array|Moment} date1 A date
@param {string|Date|Number|Array|Moment} date2 Another date, which doesn't need to be in the same format as
date1.
@return {Boolean} Whether date1 is strictly after date2 or not, down to the millisecond.

### `safenewlines`

*Pas de documentation disponible*

### `shortSummary`

@ngdoc filter
@name ods-widgets.filter:shortSummary
@function
@param {string} text Some HTML
@param {number} length The maximum length of the summary
@return {string} A short summary from the given text, usually the first paragraph. If longer than the
required length, an ellipsis will be made. This function should not be used with unsafe HTML.

### `shortTextSummary`

@ngdoc filter
@name ods-widgets.filter:shortTextSummary
@function
@param {string} text Some text
@param {number} length The maximum length of the summary
@return {string} A short summary from the given text, usually the first paragraph. If longer than the
required length, an ellipsis will be made.

### `slugify`

@ngdoc filter
@name ods-widgets.filter:slugify
@function
@param {string} text Some text
@return {string} The slugified (that is normalized, with dashes instead of spaces) version of the input text.

### `split`

@ngdoc filter
@name ods-widgets.filter:split
@function
@param {string} arrayAsString  A string representing an array of values
@param {string} [separator] The separator (default: `';'`)
@return {Array} An array containing all strings generated by the String.split method.

### `stringify`

@ngdoc filter
@name ods-widgets.filter:stringify
@function
@param {Object} jsonObject A JSON object
@return {string} The stringified version of the input object (generated through JSON.stringify)

### `themeColor`

@ngdoc filter
@name ods-widgets.filter:themeColor
@function
@param {string} theme A theme's slug (that is, its name normalized, see
{@link ods-widgets.filter:themeSlug themeSlug})
@return {string} The hexadecimal color code for this theme, as defined through
{@link ods-widgets.ODSWidgetsConfigProvider ODSWidgetsConfig}'s `theme` setting.

### `themeSlug`

@ngdoc filter
@name ods-widgets.filter:themeSlug
@function
@param {string} themeName A theme's full name
@return {string} The slugified (that is normalized, with dashes instead of spaces) version of themeName.

### `thumbnailUrl`

@ngdoc filter
@name ods-widgets.filter:thumbnailUrl
@function
@param {Object} fieldValue A record field of type file
@param {DatasetContext|CatalogContext} context The context from which the record is extracted
@return {string} A url pointing to a thumbnail of the file.

### `timesince`

@ngdoc filter
@name ods-widgets.filter:timesince
@function
@param {string|Date|Number|Array|Moment} date A date
@return {string} A fully localized string describing the time between the input date and now. For example:
"A few seconds ago"

### `toObject`

@ngdoc filter
@name ods-widgets.filter:toObject
@function
@param {Array} array An array of objects.
@param {String} key The key for the transformation.
@return {Object} The array of objects converted into an object.
@description Transform an array of objects into an objet, using a key passed as a parameter.
@example
<pre>
[
{ "name": "foo", "count": 201 },
{ "name": "bar", "count": 202 }
]
</pre>
<pre>mylist|toObject:'name'</pre>
<pre>
{
"foo": { "name": "foo", "count": 201 },
"bar": { "name": "bar", "count": 202 }
}
</pre>

### `translate`

@ngdoc filter
@module gettext
@name translate
@requires gettextCatalog
@param {String} input translation key
@param {String} context context to evaluate key against
@returns {String} translated string or annotated key
@see {@link doc:context Verb, Noun}
@description Takes key and returns string
Sometimes it's not an option to use an attribute (e.g. when you want to annotate an attribute value).
There's a `translate` filter available for this purpose.
```html
<input type="text" placeholder="{{'Username'|translate}}" />
```
This filter does not support plural strings.
You may want to use {@link guide:custom-annotations custom annotations} to avoid using the `translate` filter all the time. * Is

### `truncate`

@ngdoc filter
@name ods-widgets.filter:truncate
@function
@param {string} text Original text to truncate.
@param {number} length Max length of the truncated text.
@return {string} The `length` first chars of the input `text`, or the full input `text` if it is shorter
than `length`.

### `uriComponentEncode`

@ngdoc filter
@name ods-widgets.filter:uriComponentEncode
@function
@description This filter can be used to prepare a string to be used as a parameter when building a link.
It's important to understand that 'uriComponentEncode' filters the entire string, whereas 'uriEncode' (see {@link ods-widgets.filter:uriEncode reference page}) filter ignores
protocol prefix ('http://') and domain name.
This filter uses the 'encodeURIComponent' JavaScript function under the hood.
@param {string} A string.
@return {string} A URL encoded value (be aware that this filter will also encode protocol prefix ('http://') and domain name).
@example
<pre>
value = 'cats&dogs'
</pre>
<pre>
value|uriComponentEncode
</pre>
<pre>
cats%26dogs
</pre>

### `uriEncode`

@ngdoc filter
@name ods-widgets.filter:uriEncode
@function
@description This filter can be used to prepare a string to be used when building a link.
It's important to understand that this filter encodes a string but ignores protocol prefix ('http://') and domain name.
This filter uses the 'encodeURI' JavaScript function under the hood.
@param {string} A string.
@return {string} A URL encoded value (but ignores protocol prefix ('http://') and domain name).
@example
<pre>
value = 'https://website.com?x=шеллы'
</pre>
<pre>
value|uriEncode
</pre>
<pre>
https://website.com?x=%D1%88%D0%B5%D0%BB%D0%BB%D1%8B
</pre>

### `values`

@ngdoc filter
@name ods-widgets.filter:values
@function
@param {Object} object An object.
@return {Array} An array containing all of the object's values

### `videoify`

@ngdoc filter
@name ods-widgets.filter:videoify
@function
@param {string} url A youtube, dailymotion or vimeo URL.
@return {string} An iframe tag including the relevant video player configured with the input url

---

## Résumé

- **Directives:** 90
- **Filtres:** 47
- **Total:** 137
