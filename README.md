# utl_convex_hull_polygons_encompassing_a_three_dimensional_scatter_plo
Convex hull polygons encompassing a three dimensional scatter plot.  Keywords: sas sql join merge big data analytics macros oracle teradata mysql sas communities stackoverflow statistics artificial inteligence AI Python R Java Javascript WPS Matlab SPSS Scala Perl C C# Excel MS Access JSON graphics maps NLP natural language processing machine learning igraph DOSUBL DOW loop stackoverflow SAS community.

    Convex hull polygons encompassing a three dimensional scatter plot

    for graphic output
    https://tinyurl.com/ydh5bgvm
    https://github.com/rogerjdeangelis/utl_convex_hull_polygons_encompassing_a_three_dimensional_scatter_plot/blob/master/utl_convex_hull_polygons_encompassing_a_three_dimensional_scatter_plot

    github
    https://tinyurl.com/y99hsyqp
    https://github.com/rogerjdeangelis/utl_convex_hull_polygons_encompassing_a_three_dimensional_scatter_plot

    StackOverflow R
    https://tinyurl.com/yc9fg5dd
    https://stackoverflow.com/questions/41145959/picture-convex-hull-in-3d-scatter-plot

    Derek Corcoran
    https://stackoverflow.com/users/3808018/derek-corcoran


    INPUT (SASHELP.IRIS)
    ====================

    SD1.HAVE total obs=50

      SEPALLENGTH    SEPALWIDTH    PETALLENGTH

           50            33             14
           46            34             14
           46            36             10
           51            33             17
           55            35             13
           48            31             16
           52            34             14
         ....

    see graph for graphical output

    EXAMPLE ENVELOP POINTS (ALL OF THEM)
    ------------------------------------

    WORK.WANTUNQ total obs=14

     SEPALLENGTH    PETALLENGTH    SEPALWIDTH

          43             11            30
          44             13            32
          44             14            29
          45             13            23
          46             10            36
          48             19            34
          50             16            30
          51             19            38
          52             15            41
          54             17            34
          55             13            35
          57             15            44
          57             17            38
          58             12            40



    PROCESS  (WORKING CODE)
    ======================

        * plot the points;
        plot3d(x, y, z, col=c(rep("grey",50)), box = FALSE,type ="s", radius = 0.25);

        * convert to matrix
        ps1 <- matrix(c(x[1:50],y[1:50],z[1:50]), ncol=3);

        * calculate converse hull;
        ts.surf1 <- t(convhulln(ps1));

        * instead of lines plot triangles;
        convex1 <-  rgl.triangles(ps1[ts.surf1,1],ps1[ts.surf1,2],ps1[ts.surf1,3],col="grey",alpha=.6);


    OUTPUT
    ======

      https://tinyurl.com/ydh5bgvm

      And the envelope points

      WORK.WANTUNQ total obs=14

       SEPALLENGTH    PETALLENGTH    SEPALWIDTH

          43             11            30
          44             13            32
          44             14            29
          45             13            23
          46             10            36
          48             19            34
          50             16            30
          51             19            38
          52             15            41
          54             17            34
          55             13            35
          57             15            44
          57             17            38
          58             12            40

    *                _               _       _
     _ __ ___   __ _| | _____     __| | __ _| |_ __ _
    | '_ ` _ \ / _` | |/ / _ \   / _` |/ _` | __/ _` |
    | | | | | | (_| |   <  __/  | (_| | (_| | || (_| |
    |_| |_| |_|\__,_|_|\_\___|   \__,_|\__,_|\__\__,_|

    ;

    options validvarname=upcase;
    libname sd1 "d:/sd1";
    data sd1.have;
     set sashelp.iris(where=(species="Setosa"));
     keep SepalLength
          PetalLength
          SepalWidth;
    run;quit;

    *          _       _   _
     ___  ___ | |_   _| |_(_) ___  _ __
    / __|/ _ \| | | | | __| |/ _ \| '_ \
    \__ \ (_) | | |_| | |_| | (_) | | | |
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|

    ;

    * This can be simplified;

    %utl_submit_wps64('
    libname sd1 "d:/sd1";
    options set=R_HOME "C:/Program Files/R/R-3.3.2";
    libname wrk sas7bdat "%sysfunc(pathname(work))";
    proc r;
    submit;
    source("C:/Program Files/R/R-3.3.2/etc/Rprofile.site", echo=T);
    library(haven);
    library(rgl);
    library(geometry);
    have<-read_sas("d:/sd1/have.sas7bdat");
    r3dDefaults$windowRect <- c(0,50, 700, 700);
    iris<-iris[1:50,];
    x <- have$SEPALLENGTH;
    y <- have$PETALLENGTH;
    z <- have$SEPALWIDTH;
    rgl.viewpoint(zoom = .5, interactive = FALSE);
    plot3d(x, y, z, col=c(rep("grey",50)), box = FALSE,type ="s", radius = 0.25);
    ps1 <- matrix(c(x[1:50],y[1:50],z[1:50]), ncol=3);
    ts.surf1 <- t(convhulln(ps1));
    convex1 <-  rgl.triangles(ps1[ts.surf1,1],ps1[ts.surf1,2],ps1[ts.surf1,3],col="grey",alpha=.6);
    rgl.snapshot("d:/png/utl_convex_hull_polygons_encompassing_a_three_dimensional_scatter_plot.png");
    rgl.clear();
    want<-cbind(ps1[ts.surf1,1],ps1[ts.surf1,2],ps1[ts.surf1,3]);
    colnames(want)<-c("SEPALLENGTH", "PETALLENGTH", "SEPALWIDTH");
    want<-as.data.frame(want);
    endsubmit;
    import r=want data=wrk.want;
    run;quit;
    ');

    proc sort data=want out=wantunq nodupkey;
    by sepallength petallength sepalwidth;
    run;quit;

