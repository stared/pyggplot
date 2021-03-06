pyggplot is a pythonic wrapper around the R ggplot2 library ( http://had.co.nz/ggplot2/ ).

Unlike the python ggplot (https://github.com/yhat/ggplot) this is not a reimplementation
based on matplotlib, but a straight forward 'take pandas DataFrames and shove them into R via rpy2'
approach.

Usage:
df = pandas.DataFrame({'x': numpy.uniform(size=100), 'y' = numpy.uniform(size=100), 'group' = ['A','B'] * 50})
p = pyggplot.Plot(df)
p.add_scatter('x','y', color='group')
p.render('output.png')

Takes a pandas.DataFrame object, then add layers with the various add_xyz
functions (e.g. add_scatter).

Refer to the ggplot documentation about the layers (geoms), and simply
replace geom_* with add_*.
See http://docs.ggplot2.org/0.9.3.1/index.html

You do not need to separate aesthetics from values - the wrapper
will treat a parameter as value if and only if it is not a column name.
(so y = 0 is a value, color = 'blue 'is a value - except if you have a column
'blue', then it's a column!. And y = 'value' doesn't work, but that seems to be a ggplot issue).

When the DataFrame is passed to R:
    - row indices are turned into columns with 'reset_index'
    - multi level column indices are flattened by concatenating them with ' ' 
        -> (X, 'mean) becomes 'x mean'

Error messages are not great - most of them translate to 'one or more columns were not found',
but they can appear as a lot of different actual messages such as
    - argument "env" is missing, with no default
    - object 'y' not found
    - object 'dat_0' not found
    - requires the following missing aesthetics: x
    - non numeric argument to binary operator
without actually quite pointing at what is strictly the offending value.
Also, the error appears when rendering (or printing in Ipython notebook),
not when adding the layer.

Open questions:
    - the stat support is not great - it doesn't easily map into pythonic objects. For now, do your stats in pandas - more powerful anyhow! 
    - how could error messages be improved?
 



