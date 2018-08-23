#BASH

##finding largest files on hard drive in terminal

The following yields files around 5GB. You can grab ones that exceed that by adding another zero the the number.

- mdfind 'kMDItemFSSize > 2000000000'

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
coordinates aren't relative to, say, the _body_ tag in your HTML document, but the particular div that
you're working with on screen.

#JavaScript

## Array destructuring

When value is an array:

- const [index] = value

is equal to

- const index = value[0]

and

- const [x,y] = d3.mouse()

is equal to

- const x = d3.mouse()[0]

and

- const y = d3.mouse()[1]

## Triggering events to take place after a D3 transition

There are two ways of chaining multiple transitions together. In the first, we make sure to calculate the timing of these
transitions in order to set the start of the subsequent transition immediately after the first finishes. This method, however,
is somewhat flawed because execution time may be longer than we expect. e.g.,

- const transition1 = $item
  .transition()
  .duration(500)
  .style('fill','green')

- const transition2 = $item
  .transition()
  .delays(500)
  .duration(500)
  .style('fill','red')

So if transition1 actually executes in 550ms, but the delay we set leads to transition2 firing after 500ms, we've got an overlap.
This may not be a drastic bug in some situations, but for best practice, there's another way of chaining transitions:

- const transition2 = () =>{
  $item
  .transition()
  .delays(500)
  .duration(500)
  .style('fill','red')
  }

- const transition1 = $item
  .transition()
  .duration(500)
  .style('fill','green')
  .on('end',()=>{
  const transition1Done = false;

  if(!transition1Done) transition2()
  transition1Done = true;
  }

  })

## Adding another package to node so that others pulling repo automatically have it included on install

- npm install packageName --save

## CDN - content delivery network

Pulls the data from a single server and distributes across many others, allowing
users to access that data from the closest server on this network; these data are cached.

## nodes vs. d3 objects vs. jquery objects vs. etc.

The DOM consists of nodes, which have children, parents, and siblings — that's the DOM tree, and it forms the basis for the
document object model. Elements, which are nodes that are directly represented in HTML, are just one type of node
(others include the document node, which we access with commands such as _document.getElementById()_).

When we use the d3 object in order to select a particular node, that selection returns a _d3 selection_ rather than the node!

This distinction is important because those selection (whether they're arrays/objects/etc.) have different properties.

## Making slight tweaks to chains of functions

Occasionally, you've written a function which, later, turns out to need to have multiple small differences in output based
on some other variable. You can, of course, add multiple _if_ statements in order to account for this. What may be easier,
however, passing a true/false value to the function you've written.

Then, at the instances where this value is going to decide how the program runs (e.g., which values are passed to
your function) you can use ternary operators to change code flow.

## Working with dates

We can use _new Date(someString)_ to create a date object from a string. We can then use

- item.getDate() or item.getYear()

to find the specific date parts (https://stackoverflow.com/questions/1531093/how-do-i-get-the-current-date-in-javascript).
Note that using _getMonth()_ will mean that you need to add 1 to the result, because January is counted as 0 in JS.

## Promises

Promises are a way to make sure certain steps are performed in order in JS. A promise is created thus:

- new Promise((resolve,reject)=>{
  if(somethingBad) {
  reject(somethingBad) //logs the error
  }
  else {
  resolve()
  }
  })

Usually, it's placed inside a function, thus:

- function countNumber(){
  return new Promise((resolve,reject)=>{
  if(errorOccurrs){reject(errorOccurrs)}
  else{resolve()}
  })
  }

JavaScript executes things asynchronously, and promises allow us to avoid some of the issues with this. Let's say you want to
only execute certain functions after a particular function has been executed. We can use promises thus:

- countNumber()
  .then(()=>{
  doTheNextThing();
  doOtherThings();
  })
  .catch(console.log) //this logs the error thrown following if _reject(arg1)_ runs.

## D3: Entering and exiting items using a particular key, and the general enter(), exit(), update() pattern

NB: useful Bostock block: https://bl.ocks.org/mbostock/3808218

Most of the time, updating data in D3 doesn't require object constancy. You have a master array, and you view/hide certain parts of it.
At other times, however, we may need to incoporate some object constancy when loading data. If, for example, we are updating the
data tied to our DOM elements, but only _certain_ elements must be updated while others remain the same (albeit with updated
attributes or styles), we need to make sure that our script can differentiate between a wholly new data point about to be bound to a
new DOM element, and an extant data point which is _already_ bound to a DOM element, but that needs to have its attributes updated.

We do this using _keys_. Specifically, when pairing data, we make sure that data joins are performed using a paritcular data key,
rather than an index number (which is the default):

- let $playerDivs = d3.selectAll('div.soccer-player')
  .data(data, d=>d.player-name)

If there are already player divs paired with a previous array of player names, and if there are a number of player names
in the previous selection that match the current data, it is _those_ that will be selected using the above syntax.

We can then update these existing divs, thus:

- $playerDivs  
  .st('background-colour', 'red')

Next, we can add the new elements present in the data join but that don't yet have DOM elements associated with them:

- let $newPlayerDivs = $playerDivs  
  .enter()
  .append('div.soccer-player')
  .at('background-color','green')

At any point after this, we can then select the DOM elements that no longer have data paired with them using
the .exit() selection and remove them:

- $playerDivs
  .exit()
  .remove()

There remains, however, a key step: we need to merge the newly added DOM elements with the previously added DOM elements into
a single selection if we want to be able to deal with all of them simultaneously/ run the equivalent of d3 for-loops on them
all at once:

- let $mergedPlayers = $newPlayerDivs
  .merge($playerDivs)
  .text(d=>d.player-name)

Note that it doesn't matter whether you merge the updated selection with the newly-appended one, or the newly appended
selection with the updated one:

- $newPlayerDivs
  .merge($playerDivs)

is the same as

- $playerDivs
  .merge($newPlayerDivs)

## Map vs forEach

Running a loop over an array using _forEach_ manipulates the array in-place and doesn't create a new one. Using
map, however, returns a new array.

## loading data in jetpack

d3 jetpack allows you to load data in the following manner:

- d3.loadData(path, (error,reponse)={

})

where response is an array of files loaded.

## Nesting in d3

- d3.nest()
  .key(d=>d.key)
  .entries(data)

## Remove duplicate values from arrays

Similar to python, ES6 allows us to create a de-duplicated array from another array thus:

- let uniqueArray = [...new Set(array)];

## Less verbose ways of evaluating variables - ES6

Normally, when evaluating a variable, you'd have to use a command along the lines of

- d3.select()

## Splitting a string and limiting the number of items in the resulting array

It's common enough to split a string using _.split()_, but this function actually takes 2 arguments: the
first is the sequence of characters that will be used to split your string, and the second, an optional
one, which demarcates the maximum number of items in the resultant array.

- "hello, this, is, an, array, to, be, split".split(", ", 2)

will result in the following array:

- ["hello", "this"]

## Getting an attribute from a particular object (e.g., "this")

D3 has a getter function to perform this. The following function, for example,
will retrieve the x coordinate of the currently selected object:

-d3.select(this).attr("x")

## Using the _this_ keyword in d3 jetpack

D3 jetpack doesn't allow the use of the _this_ keyword, so in order to grab the current DOM element,
you must refer to it in a different way; specifically, by referring to the index of the item you've
selected in the array of DOM nodes:

- function(d, i, n){
  const thisObject = d3.select(n[i])
  }

## Accessing the parent object of a particular DOM element

In order to access a DOM node's parent in D3, we can get the node of any D3 object using _.node()_ :

- const thisObject = d3.select(this).node()

We can then access its parent by using _.node().parentNode_:

- const parentObject = thisObject.parentNode;

## Replacing all occurrences of a phrase within a larger strings

We use the /text/g format to do this:

- "Some 11 Text 11 You'd 11 Replace".replace(/11/g, ' ')

## Executing the html written inside of a string while displaying this strings

Using D3, there's a simple way to do this, using the _.html()_ function. This will include the
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

## SQL in QGIS

SQL IN QGIS

An example string would look like this:

- "City" in ('New York','Philadelphia','Boston')

##Converting GeoJSON to topoJSON
We use geo2topo for this:

- geo2topo file1.geojson > file2.json

## Heat maps vs. hotspot analysis (Gettis-Ord statistic, etc.)

"Heatmaps are typically about creating a raster interpolation of the density of observations given point data. Hotspot analysis is about locating areas of statistically significant geospatial clusters of observations with a particular attribute within a larger sample population. Put more simply, if we were mapping crime, a heatmap could let us see where crime is taking place, full stop; a hotspot analysis could show where crime is taking place at higher than expected levels given the existing population." -https://gis.stackexchange.com/questions/24175/how-can-i-reproduce-getis-ord-gi-hot-spot-analysis-tool-in-qgis

## Command line cartography

Note that using http://mapshaper.org/ for changing shapefile granularity and converting it
to other file formats is likely easier.

### CONVERTING GeoJSON TO NDJSON

- ndjson-split 'd.features' < INPUT.json > OUTPUT.ndjson
- ndjson-split 'd.features' < all_autowgs84.geojson > all_autowgs84.ndjson

### DELETING IRRELEVANT PROPERTIES

- ndjson-filter 'delete d.properties.VARNAME_1, true, delete d.properties.ENGTYPE_1, true, delete d.properties.ID_0, true, delete d.properties.ID_1, true, delete d.properties.ISO, true, delete d.properties.NL_NAME_1, true, delete d.properties.TYPE_1, true, delete HASC_1, true, delete d.properties.CCN_1, true, delete d.properties.CCA_1, true’ < afg_admin.ndjson > afg_admin_filtered.ndjson

### TRANSFORMING NDJSON > JSON

- ndjson-reduce < afg_admin_filtered.ndjson | ndjson-map '{type: "FeatureCollection", features: d}' > afg_admin_filtered.json

### TRANSFORMING GeoJSON > TopoJSON

- geo2topo -o afg_admin_no_proj.json afg_admin.json

### SIMPLIFYING TopoJSON

- toposimplify -p 1 -f < afg_admin_no_proj.json > afg_admin_no_proj-simple.json

### QUANTIZING SIMPLIFIED TopoJSON

- topoquantize 1e5 < afg_admin_no_proj-simple.json > afg_admin_no_proj-simple-quantized_1e5.json

#Python + Pandas

## Finding out which items in list 1 aren't in list 2

- set(list1) - set(list2)

## Converting data from daily/other time interval to weekly/hourly/etc (daily, weekly, monthly here):

- df['count'].resample('D', how='sum')
- df['count'].resample('W', how='sum')
- df['count'].resample('M', how='sum')

# Converting a series to list

- pd.Series.tolist()

##Glob (import glob)
Glob is a library that allows you to search for all files that meet a certain
set of criteria, returning a list of path names.

- glob.glob(r'/folder/\*.filetype')
  this would return a list of all paths to files of the particular file type in question.

## String literals in python

There are two ways of indicating a string literal/presence of escape characters. The more specific one
entails the use of the backslash to indicate the fact that the following character should be escaped.

The more general, one, however, allows _all_ the characters in a particular string to be read as escape characters (this
is akin to applying the backslash character to every single character in the string). It entails using the _r_
character preceding the opening quote of a string.

- string_literal = r'http://www.website.com'

## Masking: not conditions

Oftentimes, masking takes the form of

- df[df['colname'] == "X" ]

Frequently, however, we want to mask a data frame according by specifying the condition that
it _doesn't_ meet, in order to filter that particular result out of the data set. We perform this operation thus,
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

| Month | CongressmanJones | CongressmanWilliams | CongressmanHarris |
| ----- | ---------------- | ------------------- | ----------------- |
| Jan   | 15               | 2                   | 11                |
| Feb   | 7                | 12                  | 4                 |
| Mar   | 4                | 11                  | 9                 |

Each of these shows the number of bill that each congressman sponsored during each month. This sort of shape is great
if we're interested in calculating the total number of bills sponsored with each passing month, but it's also
an awkward format if we want to, say, pair this file with another data set and add coordinates to every Congressman's
HQ (i.e., each row would need 6 additional columns, 3 for 3 Congressmen's latitudes and 3 for 3 of their longitudes).

Needless to say, this becomes a somewhat frustrating data wrangling scenario quickly. Pandas has a function to resolve
these issues with minimum hassle, and will allow us to turn this data frame into the much more managable:

| Month | Congressman         | BillsPassed |
| ----- | ------------------- | ----------- |
| Jan   | CongressmanJones    | 15          |
| Jan   | CongressmanWilliams | 2           |
| Jan   | CongressmanHarris   | 11          |
| Feb   | CongressmanJones    | 7           |
| Feb   | CongressmanWilliams | 12          |
| Feb   | CongressmanHarris   | 4           |

How do we do this?

- pd.melt(dfConggressmen, id_vars=['Month'], var_name='Congressman', value_name='BillsPassed')

Note: the _id_vars_ parameter lets you specify which columns to keep, and _var_name_ specifies the name that all
the other values from the top row, which will be grouped into a new column, will be called.

The _values_ parameter specifies the name of the column that will contain the values for the newly created column
from the top row variables.

##Converting between data types

When using _pd.melt_, the categories that used to be in the top row of the data frame, and which have now been melted
into the previously mentioned _Congressman_ column, would have become data typed as objects if they had been numerical (i.e.,CongressmanJones = 1; CongressmanWilliams = 2; CongressmanHarris = 3). This isn't especially useful to deal with,
so the best way to convert these to integers would be to first convert them to strings, and then to ints:

- df[Congressmen] = df[Congressmen].astype(str).astype(int)

##Converting coordinates to lat/long
In order to work with projections, we'll need to install _pyproj_. Certain code snippets online incorporate the use
of Basemap (from mpl_toolkits.basemap import Basemap) but I haven't needed it for this sort of thing/didn't spend too
much time exploring the necessity for it.

- import pyproj as pp

First, you need to find out the coordinate system used to derive the x/y coordinates you've got to work with. Once
you've figured this out, save this variable. You'll also need to save the projection you'll be transforming the
x + y coords into.

- Brit_Natl_Grid = pp.Proj(init='epsg:27700') <!-- projection you're transforming from (i.e., input) -->
- WGS84 = pp.Proj(init='epsg:4326') <!-- projection  you're transforming to (i.e., output) -->

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

## Month | Rank | Value |

18 | 1 | 2 |
18 | 1 | 12 |
19 | 2 | 98 |
19 | 2 | 10 |
20 | 3 | 60 |
20 | 3 | 18 |

There's a relatively painless way to do this using Pandas:

- df['Rank']=df['Month'].rank(ascending=True, method = 'dense')

The _method = 'dense'_ is the crucial bit of code here, and allows you to rank individual items within
the Month column, rather than do so on a by-row basis.

##Saving a data frame to TSV

This is relatively simple, and employs the _.to_csv()_ function, thus:

- df.to_csv("df_name.tsv", sep='\t')

##Removing the pandas index from a dF in order to output to a CSV file

Again, this is a relatively straightforward procedure using the same function as above:

- df.to_csv("df_name.tsv", sep='\t', index=False)

##Evaluating a string to a variable name/object name:
_repr()_ returns a value that, when passed to _eval()_ or _exec()_ yields an object with the same value. -https://stackoverflow.com/questions/4010840/generating-variable-names-on-fly-in-python

Instead of doing this, however, it's better to use a dictionary, or lists.

- for x in list_of_variable_names:

##Creating multiple data frames from a single, global data frame programatically:

We first create an empty dictionary. Then, for each string/int/float in the list of variable names
(indicated below as _range(1985,2018)_), we create a dictionary key. The data corresponding to this
key will be generated by referring to the data frame, df, using the _global()_ function, which
precedes it, as below.

- df = pd.read_csv("data.csv")
- df_dict = {}

- for i in range(1985,2018):
-     df_dict[i] = (globals()['df'])[(globals()['df'])['year']==str(i)]

##Splitting single rows whose cells contain multiple items that must be split into several rows (while retaining
##all other data from previous rows):

If a data frame has several rows whose values you must split, and place in adjacent rows with remaining cells
which contain otherwise duplicate data, we first turn all letters to lower case.

- df_dict.text = df_dict.text.apply(lambda x: x.lower())

Next, split the column on the required string, turn the data into a series with the top level of column values
moved from column names to categories/repeated fields

- s = df_dict.text.str.split("dear abby: ").apply(pd.Series, 1).stack()

Then, set the index:

- s.index = s.index.droplevel(-1)

And set the name of the column:

- s.name = 'text'

Then remove the old column in your initial data frame:

- del df_dict['text']

And join it to the newly created series.

- df_dict = df_dict.join(s)

-https://stackoverflow.com/questions/12680754/split-explode-pandas-dataframe-string-entry-to-separate-rows -https://stackoverflow.com/questions/17116814/pandas-how-do-i-split-text-in-a-column-into-multiple-rows

##Merging 2 data frames:

If we've got _df1_ and _df2_:

- pd.merge(df1, df1, left_on="column_to_join_in_left_df", right_on="column_to_join_in_right_df")

##Splitting up a string into several parts in each row of a df column

To do so, split a string into two parts — the string used here is ' (' — and select the first,
or second, part of the resultant array.

- jan_df['state']=jan_df['region'].apply(lambda x: x.split(' (')[0])

##Replacing strings using regex: STRINGS IN W/IN PATTERN

First, ensure that you've imported the regex library:

- import re

Then, apply a lambda function to search for whatever pattern we're referring to — say, whatever's
between brackets — within a string. To do so, we need to use groups.

- r'\( (.\*?) \)'

In the code above, there are two groups — the first, outer one, which contains the string with two
parentheses, and the inner one, which contains whatever characters ( indicated by .\*, which means any
character, repeating any number of times) are contained within them. we can specify
which part of a matched string, X, we use, by using this group parameter:

- re.search(r'\((.\*?)\)',x).group(1)

Putting it all together, the code reads thus:

- raw_df['new_column']=raw_df['old_column'].apply(lambda x: re.search(r'\((.\*?)\)',x).group(1))

##Deleting all but a selected number of columns in a data frame

This is v simple: simple subset the df with the list of columns you'd like to keep, thus

- df_filtered = df[['column_1', 'column_2', 'column_16']]

Note the double bracket.

##Filtering out duplicate rows

- df.drop_duplicates()

##Finding out the name of the column containing the maximum value for every row

- df['max']=df[['column1','column2','column3']].idxmax(axis=1)

##For a particular column, filling in null values with values of another column

For one project, we worked on some tricky geographic manipulation. We had a shapefile with a certain number
of regions; our data, which needed to be bound to each of these regions, occasionally had different spelling
of region names.

How to get around this?

First: For all of the regions in our data, we tried to match them, by name, with the geo data. For
any data region that didn't have a pairing, we manually matched geo regions from the shapefile — which we
exported to an alphabetically-ordered, side-by-by side list in Excel — to our regions.

Second, since our data had insufficient coverage compared to the shapefile we used, we needed to
use country-level averages from our data to fill in any regional blanks. How did we do this?

- df.loc[df['some_regional_null_values'].isnull(),'some_regional_null_values'] = df['country_avg']

## Scraper - write out planning + pseudo code + incorporate into blog post
