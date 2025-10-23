Opendatasoft Query Language (ODSQL)

Filtering features are built in the core of the Huwise API engine.

The Opendatasoft Query Language (ODSQL) makes it possible to express complex queries as a filtering context for datasets or records and build aggregations or computed fields.

A given filtering context can simply be copied from one API to another. For example, it is possible to build a user interface that allows the user to visually select the records they are interested in, using full-text search, facets, and geo-filtering. Then, it allows them to download these records with the same filtering context.

The ODSQL is split into five different kinds of clauses:

    The select clause allows choosing the returned fields, giving them an alias, manipulating them with functions like count, sum, min, max, etc.
    The where clause acts as a filter for the returned datasets or records, thanks to boolean operations, filter functions, arithmetic expressions, etc.
    The group by clause allows aggregating rows together based on fields, numeric ranges, or dates.
    The order by and limit clauses allow choosing the order and quantity of rows received as a response.

These clauses are used as parameters in the Explore API v2 for searching, aggregating, and exporting datasets and records. Depending on the used endpoint, some features of the query language are available or not in the request.

Note: the whole query language is case insensitive, and spaces are optional. In this documentation, the uppercase is used for language keywords, only for clarity purposes.
Language elements

ODSQL clauses are composed of basic language elements. These can either be field names or aliases, literals or reserved keywords.
Field names

A field name is made of alphanumeric characters and underscores and refers to a field of a dataset or to a dynamically created field that only exists during the query (a.k.a. an alias).

Note: if a field name contains only numbers or is a keyword, it must be enclosed in back quotes.

    Examples of a field names:

my_field > 10  -- my_field is a field name

`12` > 10  -- without back quotes, 12 would be considered a numeric literal

`and`: "value" -- AND is a keyword, `and` represents a field name then

Literals in ODSQL clauses

Literals are fixed values of a specific type and can be used in comparison, assignments, or functions.

There are 6 types of literal:

    string
    numeric
    date
    boolean
    geometry
    null

String literal

A string literal is a literal enclosed in either single or double quotes.

    Examples of a string literal:

"Word"

"Multiple words"

'Using single quotes'

Note: \ (backslash) character can be used to escape special characters. For example to escape a single quote: 'Don\'t worry'.
Numeric literal

A numeric literal is either an integer or a decimal value. It is not enclosed in quotes.

    Examples of numeric literals:

100

5.8

my_field > 108.7

Date literal

A date literal is defined with a date keyword followed by a valid date format enclosed in single quotes.

A valid date can be:

    an ISO 8601 date, or
    a slash-separated date in the YYYY/MM/DD (year/month/day) format.

    Examples of a date literal:

date'2017-04-03T08:02'

date'2018/04/01'

Boolean literal

A boolean literal can either be a TRUE or a FALSE keyword (case insensitive). It should be used in boolean filters.

    Example of a boolean literal:

my_boolean_field is TRUE

my_boolean_field: FALSE

Geometry literal

A geometry literal is defined with a geom keyword followed by a valid geometry expression enclosed in single quotes.

Supported geometry expressions are:

    WKT/WKB
    GeoJSON geometry

    Example of a geometry literal:

within_distance(my_geo_field, geom'POINT(1 1)', 10km)

geometry(my_geo_field, geom'{"type": "Polygon","coordinates":[[[100.0,
0.0],[101.0, 0.0],[101.0, 1.0],[100.0, 1.0],[100.0,0.0]]]}')

Null literal

The null literal (case insensitive) is used to represent the absence of a value.

It is present in the is null filter to test whether a field has a value or not.
Reserved keywords in ODSQL clauses

Reserved keywords can be used inside clauses for building ODSQL expressions.

When used in a clause as a field literal, the reserved keyword must be escaped with back quotes.

List of reserved keywords:

    and
    as
    asc
    avg
    by
    count
    date_format
    day
    dayofweek
    desc
    distinct
    equi
    false
    group
    hour
    ifnull
    or
    limit
    lower
    max
    millisecond
    min
    minute
    month
    not
    null
    quarter
    range
    search
    second
    select
    sum
    top
    true
    upper
    where
    year

For example, not is a reserved keyword and must be escaped with back quotes if referred to as a field literal:

my_field_literal is not true -- my_field_literal is not a reserved keyword, there's no need to escape it

`not` is not true -- not is a reserved keyword and must be escaped

Handling null values

A null value in a dataset is used when the value in a field is unknown or missing. It means that there is no data for a field in a record.

Each clause behaves differently to handle null values:

    When selecting a field in a select clause, null values are represented as null.
    When filtering with a where clause, a comparison involving at least one null value is false, meaning that null values are filtered out of the result.
    When grouping with a group_by clause, no group exists for null values in v2.0, a null group do exist starting with v2.1
    When sorting with an order_by clause, null values come after all other values, regardless of the sorting direction (i.e., ascending or descending).

Default handling of null values can be changed by filtering using the is null filter or replacing null values by an alternative value or expression using the ifnull function.
Select clause

The select clause can be used in records Explore APIs as the parameter select.

The select clause allows:

    choosing the fields that will be returned for each row,
    transforming fields using arithmetic,
    renaming fields,
    adding computed virtual fields to fields, and
    including or excluding fields based on a pattern.

A select clause is composed of a single select expression or a list of comma-separated expressions.

A select expression can be:

    a field literal,
    an include/exclude function,
    an arithmetic expression, or
    an aggregation function.

Except for the include/exclude function, a select expression can define a label with the keyword AS. This label will be used in the output of the API as key for the select expression result.
Select field names

A select field name is the simplest form of select expression. It takes a field name that must be returned in the result. It also accepts the special character * to select all fields (it is the default behavior).

If a select expression is used in conjunction with a group by clause, the selected field name must be in the group by clause.

    Examples of a select field literal:

*                           -- Select all fields

field1, field2, field3      -- Only select field1, field2, and field3

field1 AS my_field, field2  -- Renaming field1 as my_field and select field2

Select aggregation

Like in the SQL language, a select can also express an aggregation expression.

The following aggregation functions are available:

    avg (average)
    count
    count distinct
    envelope
    max (maximum)
    median
    min (minimum)
    percentile
    sum

    Examples of an aggregation expression:

SUM(population) as sum_population -- Will compute the sum of all values for the field `population` returned as sum_population

COUNT(*) -- Return number of elements

Where clause

The where clause can be used in the whole Explore API as the parameter where.

The where clause allows one to filter rows with a combination of boolean expressions.

A where expression can be:

    a search query
    a filter function
    a comparison filter
    an equality filter

Where expressions can be combined with boolean operators and grouped via parenthesis.

    Example of a where clause with boolean operators:

my_numeric_field > 10 and my_text_field like "paris" or within_distance(my_geo_field, geom'POINT(1 1)', 1km)

    This where clause filters results where numeric_field > 10 and (my_text_field contains the word paris or distance between my_geo_field and the point with 1,1 as lat,lon is under 1 kilometer)

Note: it is generally possible to use multiple where clauses on an API endpoint. They are combined with a boolean AND in that case.
Boolean operators

Where expressions can use boolean operators to express boolean filter.

There are 3 different boolean operations:

    AND: results must match left and right expressions
    OR: results must match left or right expression
    NOT: inverses the next expression

AND has precedence over the OR operator. It means that, in the expression a or b and c, the sub-expression b and c is interpreted and executed first. It can also be written with parenthesis: a or (b and c).

In order to change operator precedence, it is possible to use parenthesis in the expression. To give precedence to the OR operator, the above expression can be written (a or b) and c.

    Examples of a boolean operator:

my_boolean_field OR my_numeric_field > 50 and my_date_field > date'1972'
-- Results can have my_boolean_field to true. They can also have my_numeric_field greater than 50 and my_date_field older than 1972

(my_boolean_field OR my_numeric_field > 50) and my_date_field > date'1972'
-- Results must have my_date_field older than 1972. They also must have my_boolean_field to true or my_numeric_field greater than 50

Search query filter

Filter search queries are queries that don’t refer to fields. They only contain quoted strings and boolean operators. Filter search queries perform full-text searches on all visible fields of each record and return matching rows.

If the string contains more than one word, the query will be an AND query on each tokenized word.

    Examples of a search query:

"tree"

"tree" AND "flower"

"tree" OR "car"

NOT "dog"

"dog" AND NOT "cat"

    Examples of a search query with multiple words:

"film"           -- returns results that contain film

"action movies"  -- returns results that contain action and movies.

Filter functions

Filter functions are built-in functions that can be used in a where clause.

Available filter functions are:

    search function, to perform a full-text search
    suggest function
    startswith function
    in_bbox function, to filter in a geographical area defined by a bounding box
    within_distance function, to filter in a geographical area defined by a circle
    intersects, disjoint and within to filter in a geographical area defined by a geometry

Comparison operators

Three types of comparison operators can be used in a where clause:

    text comparison operators
    numeric comparison operators
    date comparison operators

Group by clause

The group by clause can be used in the Explore API as the parameter group_by.

The group by clause creates groups from data depending on a group by expression. Groups of data cannot be returned directly and aggregation functions in the select clause have to be used to "summarize" and return a value for each group. An operation of "aggregation" can then be described by two parts: the group_by part that make groups of rows of data from a specific criterion and an aggregation function in the select clause to reduce each group to a row.

A group by clause can contain:

    a single group by expression, or
    a list of comma-separated group by expressions.

Like select expressions, a group by expression can have an AS statement to give it a label.

A group by expression can be:

    empty,
    a field,
    static ranges,
    ranges of equal widths,
    the result of a function applied on a field value (e.g. a date function, or a date format)

    Example of a simple group by expression with a label:

group_by=my_field as myfield

    Example of multiple group by expressions with a label:

group_by=my_field1,my_field2 as my_field

Empty group by

When no group_by part is expressed, rows of data are implicitly grouped into an sole group and aggregation functions are computed on the whole set of records.
Group by field

A group by field expression allows the grouping of specified field values. It creates a group for each different field value.

Format: group_by=<field_literal>

    Example of a simple group by field expression

group_by=my_field

Note:

    Starting with v2.1, grouping by geopoint or geoshape fields is not supported directly anymore. Please use the geo_cluster() grouping function to make groups out of geo points.
    Starting with v2.1, grouping by a date field now formats the key of each group as a string representing the ISO formatting of the date, when it was an integer timestamp in v2.0

Order by clause

The order by clause can be used to sort rows returned by a query.

The parameter order_by adds an order by clause to an API query. It accepts a list of comma-separated expressions followed by a direction:

    ASC for ascending
    DESC for descending

Format: order_by = expression [ ASC | DESC ], ...

An order by expression can be:

    a field
    an aggregation function
    a random function

The direction, if not specified, is ASC (ascending) by default. The random sorting will circumvent the default direction.

Note: when ordering by both aggregations and fields, the aggregation order must be at the head of the list. For example, order_by = avg(age), gender works, but order_by = gender, avg(age) returns an error.

    Examples of an order by clause

group_by=city & order_by=city ASC -- Order cities alphabetically

group_by=city & order_by=count(*) DESC -- Order each city by its number of records

select=count(*) as population_count & group_by=city  & order_by=population_count DESC -- Order each city by its number of records, using a label

group_by=city, year(birth_date) as birth_year & order_by=city DESC, birth_year ASC -- Order by city and then by year of birth

ODSQL functions
length()

Syntax: length(<text_literal>|<text_field>)

Returned type: integer

Clauses where it can be used: select, where, order_by

Returns the string length of its parameter, i.e. the number of characters that composes the string.
now()

Syntax: now(<optional_named_parameters>)

Returned type: datetime

Clauses where it can be used: select, where, order_by
Parameters to the now() function

    Examples, assuming the current date time is 2021-05-06 12:34:55.450500+00:00, which is a Thursday

now() -- Returns '2021-05-06T12:34:55.450500+00:00'
now(year=2000) -- Sets the year component to return '2000-05-06T12:34:55.450500+00:00'
now(years=-1) -- Sets the year to one year ago which is '2020-05-06T12:34:55.450500+00:00'
now(year=2001, months=-1) -- Sets the year to 2001 and subtract 1 month to return '2000-04-06T12:34:55.450500+00:00'
now(day=31,month=2) -- Sets the day to 31, then the month to 2. The actual day part is rounded to 28 '2021-02-28T12:34:55.450500+00:00'
now(weekday=0) -- Sets the day to the next Monday which is '2021-05-10T12:34:55.450500+00:00'
now(mondays=+1) -- Sets the day to the next Monday which is also '2021-05-10T12:34:55.450500+00:00'
now(mondays=-1) -- Sets the day to the previous Monday which is '2021-05-03T12:34:55.450500+00:00'

Without any parameters, the now() function returns the current date and time.

The function may also be called with named parameters to set or modify certain parts of the current date and time.

With each parameter, an integer value is required, interpreted as an absolute value or as a relative value to a part of the current date and time.

Parameter names in their singular form will set a certain part of the current date and time to the given value. Parameter names written in plural will add or subtract the given value to a part of the current date and time.

If a parameter is used multiple times in the call, only the last one is actually used, the others are ignored.
Parameter name 	Accepted values 	Description
year 	1 to 9999 	Year component
years 	Any integer 	Value to add to or subtract from the year component
month 	1 to 12 	Month component
months 	Any integer 	Value to add to or subtract from the month component, then the year component in case of overflow
day 	Any positive integer 	Day component, rounded to the maximum valid day number for the current month
days 	Any integer 	Value to add to or subtract from the day component, then the month component in case of overflow
hour 	0 to 23 	Hour component
hours 	Any integer 	Value to add to or subtract from the hour component, then the day component in case of overflow
minute 	0 to 59 	Minute component
minutes 	Any integer 	Value to add to or subtract from the minute component, then the hour component in case of overflow
second 	0 to 59 	Second component
seconds 	Any integer 	Value to add to or subtract from the second component, then the minute component in case of overflow
microsecond 	0 to 999999 	Microsecond component
microseconds 	Any integer 	Value to add to or subtract from the microsecond component, then the second component in case of overflow
weekday 	0 to 6 	Day of the week, 0 for monday to 6 for sunday
mondays 	Any integer 	Number of Mondays to add to or subtract from the current date
tuesdays 	Any integer 	Number of Tuesdays to add to or subtract from the current date
wednesdays 	Any integer 	Number of Wednesdays to add to or subtract from the current date
thursdays 	Any integer 	Number of Thursdays to add to or subtract from the current date
fridays 	Any integer 	Number of Fridays to add to or subtract from the current date
saturdays 	Any integer 	Number of Saturdays to add to or subtract from the current date
sundays 	Any integer 	Number of Sundays to add to or subtract from the current date
year()

Syntax: year(<date_literal>|<date_field>|<datetime_literal>|<datetime_field>)

Returned type: integer

Clauses where it can be used: select, where, order_by, group_by

Returns the year number of a date or datetime as a string.
month()

Syntax: month(<date_literal>|<date_field>|<datetime_literal>|<datetime_field>)

Returned type: integer

Clauses where it can be used: select, where, order_by, group_by

Returns the month number (between 1 and 12) of a date or datetime as a string.
day()

Syntax: day(<date_literal>|<date_field>|<datetime_literal>|<datetime_field>)

Returned type: integer

Clauses where it can be used: select, where, order_by, group_by

Returns the day number of the month (between 1 and 31) of a date or datetime as a string.
hour()

Syntax: hour(<date_literal>|<date_field>|<datetime_literal>|<datetime_field>)

Returned type: integer

Clauses where it can be used: select, where, order_by, group_by

Returns the hour number (between 0 and 23) of a date or datetime as a string.
minute()

Syntax: minute(<date_literal>|<date_field>|<datetime_literal>|<datetime_field>)

Returned type: integer

Clauses where it can be used: select, where, order_by, group_by

Returns the minute number (between 0 and 59) of a date or datetime as a string.
second()

Syntax: second(<date_literal>|<date_field>|<datetime_literal>|<datetime_field>)

Returned type: integer

Clauses where it can be used: select, where, order_by, group_by

Returns the second number (between 0 and 59) of a date or datetime as a string.
date_format()

Syntax: date_format(<date>, <date_format>)

Arguments:

    <date>: a date field,
    <date_format>: a string describing how to format the date (see below)

Returned type: string.

Clauses where it can be used: select, where, order_by, group_by

<date_format> is a string, where each character or group of characters will be replaced by parts of the date in the returned string.

The following formats are available for a date format expression:
Symbol 	Description 	Examples
yy or YY 	year on two digits 	20
yyyy or YYYY 	year on four digits 	2020
xx 	weekyear* on two digits 	96
xxxx 	weekyear* on four digits 	1996
w 	week of weekyear 	7
ww 	week of weekyear, left-padded with 0 	07
e 	day of week, as a number, 1 for Monday to 7 for Sunday 	2
E 	day of week, abbreviated name 	sun.
EEEE 	day of week, full name 	Sunday
D 	day of year 	89
DDD 	day of year, left-padded with 0 	089
M 	month of year 	7
MM 	month of year, left-padded with 0 	07
MMMM 	month of year, full name 	July
d 	day of month 	8
dd 	day of month, left-padded with 0 	08
H 	hour of day, 0-23 	9
HH 	hour of day, 00-23, left-padded with 0 	09
m 	minute of hour, 0-59 	13
mm 	minute of hour, 00-59, left-padded with 0 	09
s 	second of minute, 0-59 	13
ss 	second of minute, 00-59, left-padded with 0 	09

*Years and week years differ slightly. For more information, see the definition of week years.

The date format can contain free text that won't be interpreted. The free text must be surrounded by single quotes '.

To insert a single quote in the final string, it must be doubled.

Some special characters can also be used as delimiters between date components: ?, ,, ., :, / and -.

    Examples of a date_format function, where date_field = '2007-11-20T01:23:45':

date_format(date_field, 'dd/MM/YYYY') -- Returns '20/11/2007'

date_format(date_field, "'The date is 'dd/MM/YYYY") -- Returns 'The date is
20/11/2007'

date_format(date_field, "'The date is '''dd/MM/YYYY''") -- Returns "The date
is '20/11/2007'"

date_format(date_field, 'E') -- Returns 'mar.'

date_format(date_field, 'EEEE') -- Returns 'mardi'

date_format(date_field, 'H') -- Returns '1'

date_format(date_field, 'HH') -- Returns '01'

date_format(date_field, 'yy') -- Returns '07'

date_format(date_field, 'yyyy') -- Returns '2007'

date_format(date_field, 'M') -- Returns '11'

date_format(date_field, 'MM') -- Returns '11'

When used in the where clause, date_format must be compared to string values.

    Example of a date_format function used in a where clause:

where=date_format(date_field, 'dd') = '08'

You can use the lang parameter to force the output language.
json_format()

Syntax: json_format(<text_field>,[<fallback>[<null>|<text_literal>]])

Returned type: text or json

Clause where it can be used: select

Description:

Formats the text field into JSON if possible. If the text can be transformed into valid JSON, it returns the formatted JSON string. If the text cannot be transformed into valid JSON, it returns either the fallback value if provided or the original string.

    <text_field> (mandatory): A text field to be formatted into JSON. It cannot be multivalued.

    <fallback> (optional): A fallback string to return if the text cannot be transformed into valid JSON. If omitted, the original string is returned in case of invalid JSON.

Note: On /exports, except with the JSON export format, this function returns the original text value.

    Example of a json_format function used in a select clause:

select=json_format(text_field)

select=json_format(text_field, 'bad json')

select=json_format(text_field, null)

ifnull()

Syntax: ifnull(<expression>, <alternative_expression>)

Arguments:

    <expression>: a field or an expression
    <alternative_expression>: an alternative field, expression or literal

Clauses where it can be used: select, where, order_by, group_by

Returned type: the type of <expression> when not null

Returned value: the result of <alternative_expression> if <expression> returns a null value. The result of <expression> otherwise.

The returned type of <expression> and <alternative_expression> should be the same.

For group_by clause, expressions are restricted to fields and literals.

    Examples of ifnull function, where int_field contains some null values:

ifnull(int_field, 0) -- value of int_field is 0 for each row that contains a null value

lower()

Syntax: lower(<text_literal>|<text_field>)

Returned type: string

Clauses where it can be used: select, where, order_by and group_by

Returns a string in lowercase.

    Some examples:

lower('JAZZ') -- returns 'jazz'

lower(text_field) -- returns the lowercase representation of the field

include() and exclude()

Syntax: include(<field_name_pattern>)

Syntax: exclude(<field_name_pattern>)

Clauses where it can be used: select only

Include and exclude are functions that accept fields names.

Fields listed in an include function are present in the result, whereas fields listed in an exclude function are absent from the result.

Fields can contain a wildcard suffix (the * character). In that case, the inclusion/exclusion works on all field names beginning with the value preceding the wildcard.

Note: include() and exclude() are pseudo functions: they do not return a value, but are used as a declaration to constrain the list of returned fields.

    Examples of an include/exclude:

include(pop) -- will only include fields which name is pop

exclude(pop) -- will exclude fields which name is pop

include(pop*) -- Will include fields beginning with pop

Arithmetic operators

An arithmetic expression accepts simple arithmetic operations. It accepts field names, numeric constants or text values, and scalar functions. More complex arithmetic expressions can be formed by connecting these elements with arithmetic operators:

    +: add
    -: subtract
    *: multiply
    /: divide

Note: A division by zero returns a null value.

Arithmetic operators are only defined on numeric values.

    Examples of arithmetic expressions:

2 * population -- the value of the field `population` doubled

"hello" -- the constant string "hello"

length(country_name) -- the string length of the field `country_name`

random()

Syntax: random ( <integer> )

Clauses where it can be used: order_by only

The <integer> is the seed of the random function. When using the random function with one same seed, it will return the same random order each time.

    Examples of an order by random

group_by=city & order_by=random(1) -- Order cities randomly

group_by=city & order_by=random(1) -- Order cities randomly in the exact same order as the first example

group_by=city & order_by=random(2) -- Order cities randomly in a different order than the first example

distance()

Syntax: distance(<geo_field>, <center_geometry>)

Clauses where it can be used: select, order_by

Returned type: numeric

    Examples of a distance function:

distance(field_name, GEOM'<geometry>')

The distance function computes arc distance between geo_point field and a point geometry as reference. Distance (in m) can be returned using select and/or used to sort records.
vector_similarity()

Syntax: vector_similarity("<search query>")

Clauses where it can be used: only order_by

Returned type: float

This function is able to compute a semantic distance between your search query and the catalog metadata, e.g the titles, keywords, themes and descriptions. It can only be used for a catalog search. The results will be semantically sorted, i.e. the first results will have some content where the meaning should be close to your search query. For instance if you search "funny kitty", the results should have some content which contains the query terms but also cats, fun, pets, etc.

    Examples of a vector_similarity function:

vector_similarity("jazz concerts in nyc")

ODSQL predicates

Predicates are functions that return a boolean value (true or false). They can be used to filter results in the where clause.
search()

Syntax: search(<text_field>|*, <text_literal>) where:

    first parameters are the set of fields on which the search is done:
        * or empty to search on all visible fields
        a subset of field names separated with a comma ,
    the string to search for as last parameter

Clauses where it can be used: where only

Returned type: boolean

The search() function performs a full-text query on all selected fields of each record and return matching records.

It is a fuzzy search and a prefix search: <test_literal> is first split into terms separated by a space, the first terms are searched for with a certain level of fuziness (see below), and the last term is a prefix search. The level of fuziness for each term depends on the length of the term:

    for terms with a length > 5, it matches strings with a Levenshtein distance of 2,
    for terms with a length > 2, it matches strings with a Levenshtein distance of 1

The matching is case insensitive.

    Examples of a search function:

search(title, "bok of secret")  -- will match "THE BOOK OF SECRETS"

suggest()

Syntax: suggest(<text_field>|*, <text_literal>) where:

    first parameters are the set of fields on which the search is done:
        * or empty to search on all visible fields
        a subset of field names separated with a comma ,
    the string to search for as last parameter

Clauses where it can be used: where only

Returned type: boolean

    Examples of a suggest function:

suggest(*, "film")  -- returns results that contain film, films, filmography, etc. in at least one visible field
suggest("film")  -- equivalent to the above query
search(title, "secret")  -- will match "THE BOOK OF SECRETS"

suggest(text_field, other_text_field, "film")  -- same search but in text_field or other_text_field
suggest(text_field, "film") OR suggest(other_text_field, "film") -- equivalent to the above query

suggest(text_field, "film") AND suggest(other_text_field, "film") -- returns results that contain film, films, filmography, etc. in both fields

The suggest() function performs a full-text query on all selected fields of each record and return matching records. It is a prefix search: it matches the text fields that contain terms beginning with the searched string.

The matching is case insensitive.

Note: this function may miss some results that match the prefix when it is used with small prefixes.
vector_similarity_threshold()

Syntax: vector_similarity_threshold(<text_literal>) where:

    the string of your search query as unique parameter

Clauses where it can be used: where only

Ony available for the catalog search. It won't work if you want to use it to filter records from a dataset.

It can be used to carry out a semantic search of your catalog instead of a classic lexical search (based on the term and not the meaning). With this function, you can search using a sentence, synonyms, or in languages other than the original language of your catalog.

This semantic search analyzes only the following asset metadata to compute the similarity score: title, keywords, themes and description. When the territory dataset metadata is available, it can also be used.

Returned type: boolean

Contrary to the vector_similarity() function used within order_by which returns all catalog results, this function can automatically compute a semantic score threshold in order to avoid irrelevant assets in your search results. This "threshold method" is based on the "Kneedle" algorithm to find the inflection point from the semantic scores.

Source: Finding a "Kneedle" in a Haystack: Detecting Knee Points in System Behavior, Satopaa, Ville and Albrecht, Jeannie and Irwin, David and Raghavan, Barath, 2011, IEEE Computer Society -- https://doi.org/10.1109/ICDCSW.2011.20

    Examples of a vector_similarity_threshold function:

-- can return results that contain film, movie, pulp fiction, filmography,
-- or content about other movie directors.
vector_similarity_threshold("quentin tarantino movies")

-- can also return results in another language, for instance in French or German depending on the portal.
vector_similarity_threshold("content about music & jazz")

Note: The semantic method may generate irrelevant results. This potential inaccuracy is due to several underlying issues:

    The vector content may prove insufficient in detail or context to reliably produce accurate results.
    A significant challenge is the inherent asymmetry between the concise nature of user search queries and the more extensive data within asset embeddings, which can impair result precision.

startswith()

Syntax: startswith(<text_field>|*, <text_literal>) where:

    first parameters are the set of fields on which the search is done:
        * or empty to search on all visible fields
        a subset of field names separated with a comma ,
    the string to search for as last parameter

Clauses where it can be used: where only

Returned type: boolean

    Examples of a startswith function:

startswith(id, "ID4536")  -- will match id that start with "ID4536"
startswith(title, "SECRET")  -- will match "SECRET DEFENSE" but not "THE BOOK OF SECRETS", nor "book of secret"

The startswith() function performs a text query on all selected fields of each record and return matching records. It is a prefix search: it matches the text fields that contain strings beginning with the searched string. Contrary to the suggest() function, the comparison is made on the whole string, without splitting it by spaces and forming terms before.

The matching is case sensitive.
within_distance()

Syntax: within_distance(<geo_field>, <center_geometry>, <distance><unit>)

Clauses where it can be used: where only

Returned type: boolean

    Examples of a within_distance function:

within_distance(field_name, GEOM'<geometry>', 1km)

within_distance(field_name, GEOM'<geometry>', 100yd)

The within_distance function limits the result set to a geographical area defined by a circle. This circle must be defined by its center and a distance.

    The center of the circle is expressed as a geometry literal.

    The distance is numeric and can have a unit in:
        miles (mi)
        yards (yd)
        feet (ft)
        meters (m)
        centimeters (cm)
        kilometers (km)
        millimeters (mm)

in_bbox()

Syntax: in_bbox(<geo_field>, lat1, lon1, lat2, lon2)

Clauses where it can be used: where only

Returned type: boolean

This function limits the results to records that have their <geo_field> contained in a given bounding box. The bounding box is expressed by giving its two extreme points: (lat1, lon1) for the latitude and longitude of the first point and (lat2, lon2) for the latitude and longitude of the second point.
intersects()

Syntax: intersects(<geo_field>, <geometry_literal>)

Clauses where it can be used: where only

Returned type: boolean

    Examples of a geometry function:

intersects(geo_shape, geom'POLYGON((2.331161 48.869762, 2.3600006 48.87574, 2.373046875 48.85101, 2.3503875 48.84209, 2.3376846 48.85451, 2.3311614 48.869762))')

The intersects function limits the result set to a geographical area that intersects a given geometry.

This function must be defined with a geometry literal.
disjoint()

Syntax: disjoint(<geo_field>, <geometry_literal>)

Clauses where it can be used: where only

Returned type: boolean

    Examples of a geometry function:

disjoint(geo_shape, geom'POLYGON((2.331161 48.869762, 2.3600006 48.87574, 2.373046875 48.85101, 2.3503875 48.84209, 2.3376846 48.85451, 2.3311614 48.869762))')

The disjoint function limits the result set to a geographical area that is disjoint from a given geometry.

This function must be defined with a geometry literal.
within()

Syntax: within(<geo_field>, <geometry_literal>)

Clauses where it can be used: where only

Returned type: boolean

    Examples of a within function:

within(geo_shape, geom'POLYGON((2.331161 48.869762, 2.3600006 48.87574, 2.373046875 48.85101, 2.3503875 48.84209, 2.3376846 48.85451, 2.3311614 48.869762))')

The within function limits the result set to a geographical area that lie within a given geometry.

This function must be defined with a geometry literal.
Text comparison operators

Clauses where it can be used: where only
Operator
Description
= 	Perform an exact query (not tokenized and not normalized) on the specified field
Numeric comparison operators

Clauses where it can be used: where only
Operator
Description
= 	Match a numeric value
>,<,>=,<= 	Return results whose field values are larger, smaller, larger or equal, smaller or equal to the given value
Date comparison operators

Clauses where it can be used: where only
Operator
Description
= 	Match a date
>,<,>=,<= 	Return results whose field date are after or before the given value
Boolean field filter

Syntax:

    <boolean_field>
    <boolean_field> is (true|false)

Clauses where it can be used: where only

A boolean field filter takes a boolean field and restricts results only if the boolean value is true.

There are 2 ways of creating a filter expression:

    with a field literal only: in that case, it filters the result where the field literal value is true
    with a field literal followed by the is keyword, then true or false keywords

    Examples of a boolean field filter:

my_boolean_field          -- Filters results where boolean_field is true

my_boolean_field is false -- Filters results where boolean_field is false

where <field_literal> must be a valid boolean field
IN filter

Syntax:

    on a numeric range: <field_literal> IN (]|[)<numeric_literal> (TO|..) <numeric_literal>(]|[)
    on a date range: <field_literal> (IN|:) (]|[)<date_literal> (TO|..) <date_literal>(]|[)
    on a list: <field_literal> IN (<literal>, <literal>*)
    on a multivalued field: <literal> IN <field_literal>

Clauses where it can be used: where only

An IN filter restricts results using a search in a list or a range of values.

There are 3 ways of using an IN filter:

    to search that a field's value is present in a numeric or a date range.
    to search that a field's value is present in a list of literals.
    to search that a literal value is present in a multivalued field's values.

    Example of an IN filter expression on a numeric range:

numeric_field IN [1..10] -- Filters results such as 1 <= numeric_field <= 10

numeric_field IN ]1..10[ -- Filters results such as 1 < numeric_field < 10

    Example of an IN filter expression on a date range:

date_field IN [date'2017'..date'2018'] -- Filters results such as date_field date is between year 2017 and 2018

You can also use the : syntax:

date_field:['2022-03-12' TO '2022-04-27'] -- single quotes for the date parameters

date_field:[20220312 TO 20220427] -- the date parameters as a full integer

    Note that in the previous example, you don't have to cast the parameter if you use quotes or a full integer. However, this does not work if you don't use quotes or a specific cast such as date:'YYYY-MM-DD'. For instance the following example will raise an ODSQL error.

date_field IN [2022-03-12 TO 2022-04-27] -- this is not valid

    Example of an IN filter expression on a list of literals:

my_field IN ("Paris", "Nantes", "Lorient", "Besançon") -- Filters results such as my_field is equal to "Paris", "Nantes", "Lorient" or "Besançon"

    Example of an IN filter expression on a multivalued field:

"Paris" IN multivalued_text_field -- Filters results such as the literal "Paris" is present in the multivalued field
15 IN multivalued_int_field -- Same as above but with an integer literal
12.087 IN mutlivalued_decimal_field -- Same as above but with a decimal literal
true IN mutlivalued_boolean_field -- Same as above but with a boolean literal

IS NULL filter

Syntax:

    <field> is null
    <field> is not null

Clauses where it can be used: where only

A null field filter takes a field and restricts results only if the field values are null. The opposite filter, is not null, takes a field and restricts results only if the field values are not null.

    Examples of a null filter expression:

film_name is null      -- matches records where film_name is null

film_name is not null  -- matches records where film_name is not null

ODSQL aggregate functions

Aggregation functions are functions that perform a computation on a set of values and return one value. They are usually used in conjunction with a group_by clause.
avg()

Syntax: avg(<numeric_field>)

Clauses where it can be used: select, order_by

Returned type: numeric

This function takes a numeric field. It returns the average (avg) of this field over a group.

    Example of an avg aggregation:

avg(population) as avg_population -- Return the average of the population

count()

Syntax: count(<field>|*)

Clauses where it can be used: select, order_by

Returned type: integer

This function computes a number of elements.

It accepts the following parameters:

    a field name: only returns the count for non-null values of this field
    a *: returns the count of all elements

    Examples of a count aggregation:

count(*) -- Return number of elements

count(population) as population_count_not_empty -- Return number of elements where `population` field is not empty

count(distinct)

Syntax: count(distinct <field>|*)

Clauses where it can be used: select, order_by

Returned type: integer

This function computes the unique numbers of elements, eliminating the repetitive appearance of the same data.

It accepts the following parameters:

    a field name: only returns the number of unique non-null values of this field.
    the function ifnull(<ods_field>, <alternative_expression>): same as above, but replace all null values with an alternative expression before counting. See the documentation of the ifnull function for more details on its syntax.

Note: For performance reasons, the count is always approximated.

    Examples of a count distinct aggregation:

count(distinct species) -- Return the number of unique values for the field species

count(distinct ifnull(species, "'unknown'")) -- Same as above, but null values will be counted as equals to 'unknown'

envelope()

Syntax: envelope(<geo_point_field>)

Clauses where it can be used: select

Returned type: geo_shape

This function takes a geo_point field. It returns the convex hull (envelope) of all the points of the geo_point field.

    Example of an envelope aggregation:

envelope(geo_point) as convex_hull -- Return the convex_hull for the geo_point field

bbox()

Syntax: bbox(<geo_field>)

Clauses where it can be used: select

Returned type: geo_shape

This function takes a geo_point or a geo_shape field. It returns the bounding box of all the geometries.

    Example of an bbox aggregation:

bbox(geo_point) -- Return the bounding box of all the points

max()

Syntax: max(<numeric_field>|<date_field>)

Clauses where it can be used: select, order_by

Returned type: numeric or date

This function takes a numeric or a date field name. It returns the maximum value (max) of this field.

    Example of a max aggregation:

max(population) as max_population -- Return max value for population field

median()

Syntax: median(<numeric_field>)

Clauses where it can be used: select, order_by

Returned type: numeric

This function takes a numeric field name. It returns the median (median) of this field's values. Since the median is the 50th percentile, it is a shortcut for percentile(field, 50).

    Example of a median aggregation:

median(age) as med -- Return the median of the age field

min()

Syntax: max(<numeric_field>|<date_field>)

Clauses where it can be used: select, order_by

Returned type: numeric or date

This function takes a numeric or a date field name. It returns the minimum value (min) of this field.

    Example of a min aggregation:

min(population) as min_population -- Return min value for population field

percentile()

Syntax: percentile(<numeric_field>, <percentile>)

Clauses where it can be used: select, order_by

Returned type: numeric

This function takes a numeric field name and a percentile. It returns the nth percentile (percentile) of this field. Percentile must be a decimal value between 0 and 100.

    Example of a percentile aggregation:

percentile(age, 1) as first_percentile -- Return the first percentile of the age field

sum()

Syntax: sum(<numeric_field>)

Clauses where it can be used: select, order_by

Returned type: numeric

This function takes a numeric field name as an argument. It returns the sum of all values for a field.

    Example of a sum aggregation:

sum(population) as sum_population -- Return the sum of all values for the population field

ODSQL grouping functions

Grouping functions are functions that can be used in the group_by clause to separate a set of records into different sets that share a common property. An aggregate function can then be applied on each group separately.
range() - group by static ranges

Syntax for numerical ranges: range(<field_literal> [, *]?, <numeric_literal> [,<numeric_literal>]* [, *]?) where <field_literal> must be a numeric field

Syntax for date/datetime ranges: range(<field_literal> [, *]?, <date_literal> [,<date_literal>]* [, *]?) where <field_literal> must be a date or datetime field.

Clauses where it can be used: group_by only

The static range function takes a variable number of parameters:

    a field name, and
    a variable number of parameters. Each parameter can be a numerical literal, a date literal or the special syntax * to denote infinity.

A * as first step makes values lower than the lower bound included in the first group, a * as last step makes values greater than the upper bound included in the last group.

Note that the resulting aggregation includes the lower bound and excludes the higher bound.

Ranges can be set on numerical fields and on date/datetime fields.

For a recall, date literals are composed of the date identifier followed by a date in ISO format, e.g. date'2021-02-01'

    Examples of a group by static ranges expression:

RANGE(population, *, 10, 50, 100, *)               -- Creates 4 groups: [*, 9], [10, 49], [50, 99] and [100, *]
RANGE(population, 20.5, *)                         -- Creates 1 group: [20.5, *[
RANGE(population, 1,2,3)                           -- Creates 2 groups: [1-1], [2, 2]
RANGE(date, *, date'2020-11-13', date'2021-01-01') -- Creates 2 groups: [*, 2020-11-13T00:00:00.000Z[ and [2020-11-13T00:00:00.000Z, 2021-01-01T00:00:00.000Z[

range() - group by ranges of equal widths

Syntax for numerical fields: group_by=range(<field_literal>, <numeric_literal>) where <field_literal> must be a numeric field

Syntax for date/datetime fields: group_by=range(<field_literal>, <integer><interval_unit>) where <field_literal> must be a date/datetime field, and <interval_unit> is one of the following (case sensitive) string constants:

    ms, millisecond or milliseconds,
    s, second or seconds,
    m, minute or minutes,
    h, hour or hours,
    d, day or days,
    w, week or weeks,
    M, month or months,
    q, quarter or quarters,
    y, year or years.

Note: For some interval units (week, month, quarter, and year), an interval value of more than one is not supported yet.

Clauses where it can be used: group_by only

It is possible to group values of a field by ranges of equal widths, also known as histograms.

Ranges of equal widths are supported for numerical fields and date/datetime fields.

The range function for ranges of equal widths takes for parameters:

    a field name, and
    the desired width of each group.

For date/datetime fields, the width of each group is expressed by a time interval with a special syntax (see above).

Note: groups that do not contain any data are not returned.

    Example of a group by ranges of equal widths expression:

RANGE(population, 5)

    5 is the desired width of each returned group. For values of a population field that span from 10 to 28, it creates the following groups:

- [10, 15[
- [15, 20[
- [20, 25[
- [25, 30[

  Example of a date histogram:

RANGE(date, 1 day)

    Groups created (one for each day):

- [2020-01-01T00:00:00.000Z, 2020-01-02T00:00:00.000Z[
- [2020-01-02T00:00:00.000Z, 2020-01-03T00:00:00.000Z[
- [2020-01-04T00:00:00.000Z, 2020-01-05T00:00:00.000Z[
- ...

  No group is created for 2020-01-03 since no data is available for this day.

geo_cluster()

Syntax: group_by=geo_cluster(<geo_point_field>, <zoom_level>[, <radius>]) where:

    <zoom_level> is an integer between 0 and 25
    <radius> is an optional integer. It defaults to 40.

Clauses where it can be used: group_by only

This function groups points that are close to each other.

It first groups points by their geohash of a certain level. The level (or precision) of the used geohash grid is determined by both the zoom and the radius parameters:

    zoom_level follows the "slippy map" zoom level hierarchy: at zoom level 0, one tile represents the whole planet and each sub level sub divides the tile into 4 sub tiles.
    radius is expressed in pixels on a tile of 256x256 pixels at the given zoom_level
    the geohash precision is the one where radius in meter is smaller than the geohash cell dimension

A second step merges groups that may have points that are very close. This is to circumvent the "grid" effect of the first step. e.g. At geohash grid level 1, France is split into 4 geohash cells "g", "u", "e" and "s" that cross somewhere north-east from Bordeaux. A dense group of points that lie in a small area around Bordeaux may be split into more than 1 bucket with the first step. This second step is here to join them back.

The join step is done by:

    computing the centroid of each group in the first step, in addition to the list of points that lie within,
    merging groups that have centroids that are closer than the distance given by the radius parameter

