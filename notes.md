#HTML/css

## CSS - to upper case, and to capitalize first letter of each word

- text-transform: uppercase
- text-transform: capitalize

## Using relative and absolute position to place tooltips

We know that it's important to provide a relatively-positioned container if you want to
use x and y coordinates for an absolutely positioned div (this comes in handy for tooltips).

It's important to remember, however, that the relatively positioned tooltip is the one that provides
the coordinate grid for the absolutely positioned div. When giving the tooltip coordinates, for example,
you must ensure that the grid you're working is, itself, appropriately positioned, so that your tooltip's
coordinates aren't relative to, say, the *body* tag in your HTML document, but the particular div that
you're working with on screen.

#JavaScript

## Less verbose ways of evaluating variables - ES6

Normally, when evaluating a variable, you'd have to use a command along the lines of

- d3.select()

## Splitting a string and limiting the number of items in the resulting array

It's common enough to split a string using *.split()*, but this function actually takes 2 arguments: the
first is the sequence of characters that will be used to split your string, and the second, an optional
one, which demarcates the maximum number of items in the resultant array.

- "hello, this, is, an, array, to, be, split".split(", ", 2)

will result in the following array:

- ["hello", "this"]

## Getting an attribute from a particular object (e.g., "this")

D3 has a getter function to perform this. The following function, for example,
will retrieve the x coordinate of the currently selected object:

-d3.select(this).attr("x")

## Using the *this* keyword in d3 jetpack

D3 jetpack doesn't allow the use of the *this* keyword, so in order to grab the current DOM element,
you must refer to it in a different way; specifically, by referring to the index of the item you've
selected in the array of DOM nodes:

- function(d, i, n){
    const thisObject = d3.select(n[i])
}


## Accessing the parent object of a particular DOM element

In order to access a DOM node's parent in D3, we can get the node of any D3 object using *.node()*:

- const thisObject = d3.select(this).node()

We can then access its parent by using *.node().parentNode*:

- const parentObject = thisObject.parentNode;


## Replacing all occurrences of a phrase within a larger strings

We use the /text/g format to do this:

- "Some 11 Text 11 You'd 11 Replace".replace(/11/g, ' ')

## Executing the html written inside of a string while displaying this strings

Using D3, there's a simple way to do this, using the *.html()* function. This will include the
html inside your string as html rather than as text. For line breaks, for example,

- obj.html(d.text_and_html)


## Joining two arrays on a common key

While it's relatively simple to create objects/entries that include a combination of data fields in SQL or pandas, the data structures
available in JavaScript are much less suitable for doing so.

When necessary, it is best to do this as part of the data pre-processing step. If it's necessary to perform this during the front-end
step, it's possible to create a third, key object, which you employ as the "join key."

For example, if you've got an array of objects:

- congressmenAge = [
{name: James,
age: 50},

{name: Rogers,
age: 71},

{name: Whittaker,
age: 45},
]

and want to create objects that include the home state of these congressmen, included in this array:

- congressmenHomeState = [
{name: James,
state: Iowa},

{name: Rogers,
state: Tennessee},

{name: Whittaker,
state: Texas},
]

The simplest way to do so would be to create a new key object, thus:

- keyObject = {}
congressmenAge.forEach(function(d){
  keyObject[d.name] = congressmenHomeState[d.name]
})

In later parts of code, you can then always use the names of each congressman as a key value to get their age using the keyObject.


#Mapping

##Converting GeoJSON to topoJSON
We use geo2topo for this:

- geo2topo file1.geojson > file2.json


## Heat maps vs. hotspot analysis (Gettis-Ord statistic, etc.)
"Heatmaps are typically about creating a raster interpolation of the density of observations given point data. Hotspot analysis is about locating areas of statistically significant geospatial clusters of observations with a particular attribute within a larger sample population. Put more simply, if we were mapping crime, a heatmap could let us see where crime is taking place, full stop; a hotspot analysis could show where crime is taking place at higher than expected levels given the existing population." -https://gis.stackexchange.com/questions/24175/how-can-i-reproduce-getis-ord-gi-hot-spot-analysis-tool-in-qgis

##Overlapping layers of polygons in QGIS

-

#Python + Pandas

#  Converting a series to list

- pd.Series.tolist()

##Glob (import glob)
Glob is a library that allows you to search for all files that meet a certain
set of criteria, returning a list of path names.

- glob.glob(r'/folder/\*.filetype')
  this would return a list of all paths to files of the particular file type in question.

## String literals in python
There are two ways of indicating a string literal/presence of escape characters. The more specific one
entails the use of the backslash to indicate the fact that the following character should be escaped.

The more general, one, however, allows *all* the characters in a particular string to be read as escape characters (this
is akin to applying the backslash character to every single character in the string). It entails using the *r*
character preceding the opening quote of a string.

- string_literal = r'http://www.website.com'

## Masking: not conditions
Oftentimes, masking takes the form of
- df[df['colname'] == "X" ]

Frequently, however, we want to mask a data frame according by specifying the condition that
it *doesn't* meet, in order to filter that particular result out of the data set. We perform this operation thus,
using the tilde operator:

- df[ ~ (df['colname1']=="X") & (df['colname2']=="Y") ]



##Running a loop across multiple lists simultaneously

The standard pythonic way of running loops takes the form of

- for x in list:
    x=x^2

The syntax for iterating across multiple lists (in this example, with the same length), however, is somewhat
different:

- for x, y in zip(list_x, list_y):
    x=x^2
    y=y^2


## Awkward df reshaping: Melting
Sometimes, our data frames are awkwardly shaped. Take, for example, a df (dfConggressmen) with the
following shape (i.e., columns):

Month | CongressmanJones | CongressmanWilliams | CongressmanHarris |
-------------------------------------------------------------------|
Jan   |     15           |       2             |         11        |
Feb   |     7            |       12            |         4         |
Mar   |     4            |       11            |         9         |


Each of these shows the number of bill that each congressman sponsored during each month. This sort of shape is great
if we're interested in calculating the total number of bills sponsored with each passing month, but it's also
an awkward format if we want to, say, pair this file with another data set and add coordinates to every Congressman's
HQ (i.e., each row would need 6 additional columns, 3 for 3 Congressmen's latitudes and 3 for 3 of their longitudes).

Needless to say, this becomes a somewhat frustrating data wrangling scenario quickly. Pandas has a function to resolve
these issues with minimum hassle, and will allow us to turn this data frame into the much more managable:

Month |    Congressman      | BillsPassed|
-----------------------------------------|
Jan   |CongressmanJones     |     15     |
Jan   |CongressmanWilliams  |     2      |  
Jan   |CongressmanHarris    |     11     |
Feb   |CongressmanJones     |     7      |
Feb   |CongressmanWilliams  |     12     |  
Feb   |CongressmanHarris    |     4      |   

How do we do this?

- pd.melt(dfConggressmen, id_vars=['Month'], var_name='Congressman', value_name='BillsPassed')

Note: the *id_vars* parameter lets you specify which columns to keep, and *var_name* specifies the name that all
the other values from the top row, which will be grouped into a new column, will be called.

The *values* parameter specifies the name of the column that will contain the values for the newly created column
from the top row variables.

##Converting between data types

When using *pd.melt*, the categories that used to be in the top row of the data frame, and which have now been melted
into the previously mentioned *Congressman* column, would have become data typed as objects if they had been numerical (i.e.,CongressmanJones = 1; CongressmanWilliams = 2; CongressmanHarris = 3). This isn't especially useful to deal with,
so the best way to convert these to integers would be to first convert them to strings, and then to ints:

- df[Congressmen] = df[Congressmen].astype(str).astype(int)

##Converting coordinates to lat/long
In order to work with projections, we'll need to install *pyproj*. Certain code snippets online incorporate the use
of Basemap (from mpl_toolkits.basemap import Basemap) but I haven't needed it for this sort of thing/didn't spend too
much time exploring the necessity for it.

- import pyproj as pp

First, you need to find out the coordinate system used to derive the x/y coordinates you've got to work with. Once
you've figured this out, save this variable. You'll also need to save the projection you'll be transforming the
x + y coords into.

- Brit_Natl_Grid = pp.Proj(init='epsg:27700') <!-- projection you're transforming from (i.e., input) -->
- WGS84          = pp.Proj(init='epsg:4326') <!-- projection  you're transforming to (i.e., output) -->

If you're dealing with a single set of coordinates, you can get their lon/lat pairings thus:

- lon, lat = pp.transform(Brit_Natl_Grid, WGS84, x_coord, y_coord)

Still, usually we do this in pandas, and are working with columns of text. There is likely an easier way to do this
in place, but here's a quick hack that gets results. First, convert each of the coord columns to a list:

- geo_x = df['geometry_x'].tolist()
- geo_y = df['geometry_y'].tolist()

We then iterate this previous function through each item across both of these lists:

- list_of_coords = []
  for x, y in zip(geo_x, geo_y):
    coords = {}
    lon = 0
    lat = 0
    lon, lat = pp.transform(Brit_Natl_Grid, WGS84, x, y)
    coords['lon'] = lon
    coords['lat'] = lat
    list_of_coords.append(coords)

We can then take this list of dictionaries and transform them to a data frame, which we then merge with
the original df:

- coords_only_df = pd.DataFrame(list_of_coords)

  complete_df = pd.concat([df, coords_only_df], axis=1)

##Ranking variables

Oftentimes, you want to rank a variable in a way such that not every row receives its own individual
ordinal designation. You may, for example, want to rank sections of rows within a data frame, such that


Month |       Rank       |   Value  |
-------------------------------------
18    |     1            |       2  |
18    |     1            |       12 |
19    |     2            |       98 |
19    |     2            |       10 |
20    |     3            |       60 |
20    |     3            |       18 |

There's a relatively painless way to do this using Pandas:

- df['Rank']=df['Month'].rank(ascending=True, method = 'dense')

The *method = 'dense'* is the crucial bit of code here, and allows you to rank individual items within
the Month column, rather than do so on a by-row basis.

##Saving a data frame to TSV

This is relatively simple, and employs the *.to_csv()* function, thus:

- df.to_csv("df_name.tsv", sep='\t')

##Removing the pandas index from a dF in order to output to a CSV file

Again, this is a relatively straightforward procedure using the same function as above:

- df.to_csv("df_name.tsv", sep='\t', index=False)


##Evaluating a string to a variable name/object name:
*repr()* returns a value that, when passed to *eval()* or *exec()* yields an object with the same value.
-https://stackoverflow.com/questions/4010840/generating-variable-names-on-fly-in-python

Instead of doing this, however, it's better to use a dictionary, or lists.

- for x in list_of_variable_names:


##Creating multiple data frames from a single, global data frame programatically:

We first create an empty dictionary. Then, for each string/int/float in the list of variable names
(indicated below as *range(1985,2018)*), we create a dictionary key. The data corresponding to this
key will be generated by referring to the data frame, df, using the *global()* function, which
precedes it, as below.

- df = pd.read_csv("data.csv")
- df_dict = {}

- for i in range(1985,2018):
-     df_dict[i] = (globals()['df'])[(globals()['df'])['year']==str(i)]



##Splitting single rows whose cells contain multiple items that must be split into several rows (while retaining
##all other data from previous rows):

If a data frame has several rows whose values you must split, and place in adjacent rows with remaining cells
which contain otherwise duplicate data, we first turn all letters to lower case.

-  df_dict.text = df_dict.text.apply(lambda x: x.lower())

Next, split the column on the required string, turn the data into a series with the top level of column values
moved from column names to categories/repeated fields

-  s = df_dict.text.str.split("dear abby: ").apply(pd.Series, 1).stack()

Then, set the index:

-  s.index = s.index.droplevel(-1)

And set the name of the column:

-  s.name = 'text'

Then remove the old column in your initial data frame:
-  del df_dict['text']

And join it to the newly created series.
-  df_dict = df_dict.join(s)

-https://stackoverflow.com/questions/12680754/split-explode-pandas-dataframe-string-entry-to-separate-rows
-https://stackoverflow.com/questions/17116814/pandas-how-do-i-split-text-in-a-column-into-multiple-rows


##Merging 2 data frames:

If we've got *df1* and *df2*:

- pd.merge(df1, df1, left_on="column_to_join_in_left_df", right_on="column_to_join_in_right_df")
