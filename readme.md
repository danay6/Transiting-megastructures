# make_defined_lcs.ipynb
This is the notebook where we create lightcurves using different defined shapes,
mainly circles, triangles, and squares. Any other shapes can be added by creating
a new section in the function where shape == ’shape name’. The inputs are image
ratio, velocity, shape, and t_ref (the time of deepest transit).

The image ratio is something that I designed so that the length of the object will
be 1/image ratio by adding invisible padding above and below the image in pyplot.
The radius of the star is defined as 1 so the velocity is in units of radii/day (1/day).
If varying widths, it will cycle through velocities of .3, .4 , .5, .7, 1.0, 1.8 and will
have a consistent image ratio of 2, unless it’s a Triangle, in which case it will have
an image ratio of 1.4. This is because this creates about a 70 percent max depth for
circle, square, and triangle that way so they are comparable.

If varying depths, it will cycle through a formula of image_ratio = 1 / sqrt(1-
value) where value ranges from 0.07, 0.16, 0.25, 0.34, 0.43, 0.52, 0.61, 0.70, 0.79,
0.88, 0.97, .99. This formula is used because it creates linear changes in depths
rather than exponential changes in depths that would be created using image_ratio
= 1,2,3,4,5,6,7,8,9,10. (If it’s a triangle, it only goes through values 0.07, 0.16, 0.25,
0.34, 0.43, .49 because the code is not written for the triangle to surpass the edges
of the star)
# 0.4-mstat-nb.ipynb
This takes the lightcurves from make_defined_lcs.ipynb and calculates the mstats
for them based on the width/depth. It is only formulated to have one variable, either
width or depth.
# 6.1-injection-scoring-refactor.ipynb
This notebook calcualtes the rankings of all the injected lightcurves from -.4-mstat-
nb.ipynb and outputs files into a TICid_subplot file to create the 3x3 subplots that
analyze depth vs mstat vs ranking or width vs mstat vs ranking
# make_subplot.ipynb
This makes the subplots that are a culmination of running the last 3 notebooks. It’s
located within subplot_data_tic251630511.
# detectability-heatmap.ipynb
This is a combination of all the previous files, but it makes a heatmap instead
of the subplots. It varies over widths, depths, TIC IDs, and t_refs (the time of
transit). It creates files in the Random_LCs folder and Injected_LCs folder and
eventually creates a folder for the magnitude bin’s heatmap. You will have to run
each magnitude bin individually.

We calculated how to vary the widths using the formulae:

len_obj = 1/d (where d = image_ratio)

v = 2*(1 + (len_obj)) / w (where w is the desired width)

We had to use this formula because otherwise, as the image_ratio and depth
change, the size of the object changes, which slightly changes the width of the transit.
In order to fix this, we had to use this modified version of the transit duration formula
where the length of the transiting object is considered when deciding the velocity of
the object.

Once you have a set of random_lcs that you’re going to keep using, you can
comment out make_shape in process_data function, because those same shapes will
keep getting injected into different TESS lightcurves.
This notebook is set up to inject into the first 10 TIC IDs in the mag bucket that
are in the middle 50 percent of ranks and within 1 std of the median of mstats.
