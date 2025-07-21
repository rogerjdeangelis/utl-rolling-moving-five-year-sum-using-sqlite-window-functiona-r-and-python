# utl-rolling-moving-five-year-sum-using-sqlite-window-functiona-r-and-python
    %let pgm=utl-rolling-moving-five-year-sum-using-sqlite-window-functiona-r-and-python;

    %stop_submission;

    TWO SOLUTIONS

        1 sql python
        2 sql r
        3 related repos on end

    github
    https://tinyurl.com/43mnr4ms
    https://github.com/rogerjdeangelis/utl-rolling-moving-five-year-sum-using-sqlite-window-functiona-r-and-python

    Rolling-moving-five-year-sum-using-sqlite-window-functiona-r-and-python

    Variable code just confuses the problem, year is a primary key, so you can add other variables.

    sas communities
    https://tinyurl.com/5h29asm4
    https://communities.sas.com/t5/SAS-Programming/Conditional-count-last-5-years-observations/m-p/760229#M240375

    INPUT                               PROCESS
    =====                               =======


    /**************************************************************************************************************************/
    /* INPUT                   | PROCESS                                                 | OUTPUT                             */
    /* =====                   | =======                                                 | ======                             */
    /* Obs    YEAR    EI_D     |              moving_                                    |                                    */
    /*                         | moving 5 year sums                                      |                                    */
    /*   1    2010      1      |                                                         |                                    */
    /*   2    2011      0      | 2010 1  NA                                              |                                    */
    /*   3    2012      1      | 2011 0  NA                                              |                                    */
    /*   4    2013      1      | 2012 1  NA                                              |                                    */
    /*   5    2014      0      | 2013 1  NA                                              |                                    */
    /*   6    2015      1      | 2014 0  3  1+0+1+1+0                                    |                                    */
    /*   7    2016      0      | 2015 1  3  0+1+1+0+1                                    |                                    */
    /*   8    2017      0      | 2016 0                                                  |                                    */
    /*   9    2018      1      | 2017 0                                                  |                                    */
    /*  10    2014      1      | 2018 1                                                  |                                    */
    /*  11    2015      1      | 2014 1                                                  |                                    */
    /*  12    2016      0      | 2015 1                                                  |                                    */
    /*  13    2017      1      | 2016 0                                                  |                                    */
    /*  14    2018      0      | 2017 1                                                  |                                    */
    /*                         | 2018 0                                                  |                                    */
    /* options                 |                                                         |                                    */
    /*  validvarname=upcase;   |                                                         |                                    */
    /* libname sd1 "d:/sd1";   | 1 SQL PYTHON                                            |   year  ei_d    sum                */
    /* data sd1.have;          | ============                                            |                                    */
    /*   input year $ ei_d;    |                                                         | 2010.0   1.0    NaN                */
    /* cards4;                 | proc datasets lib=sd1 nolist nodetails;                 | 2011.0   0.0    NaN                */
    /* 2010 1                  |  delete pywant;                                         | 2012.0   1.0    NaN                */
    /* 2011 0                  | run;quit;                                               | 2013.0   1.0    NaN                */
    /* 2012 1                  |                                                         | 2014.0   0.0    3.0                */
    /* 2013 1                  | %utl_pybeginx;                                          | 2015.0   1.0    3.0                */
    /* 2014 0                  | parmcards4;                                             | 2016.0   0.0    3.0                */
    /* 2015 1                  | exec(open('c:/oto/fn_pythonx.py').read());              | 2017.0   0.0    2.0                */
    /* 2016 0                  | have,meta=ps.read_sas7bdat('d:/sd1/have.sas7bdat');     | 2018.0   1.0    2.0                */
    /* 2017 0                  | want=pdsql('''                                          | 2014.0   1.0    3.0                */
    /* 2018 1                  |  select                                                 | 2015.0   1.0    3.0                */
    /* 2014 1                  |    year                                                 | 2016.0   0.0    3.0                */
    /* 2015 1                  |   ,ei_d                                                 | 2017.0   1.0    4.0                */
    /* 2016 0                  |   ,case                                                 | 2018.0   0.0    3.0                */
    /* 2017 1                  |      when (rowid < 5 ) then NULL                        |                                    */
    /* 2018 0                  |      else five_day_sums                                 |                                    */
    /* ;;;;                    |   end as moving_sum                                     | SAS                                */
    /* run;quit;               |  from                                                   |            MOVING_                 */
    /*                         |   (select                                               | YEAR EI_D    SUM                   */
    /*                         |     year                                                |                                    */
    /*                         |    ,ei_d                                                | 2010   1      .                    */
    /*                         |    ,rowid                                               | 2011   0      .                    */
    /*                         |    ,sum(ei_d) over (                                    | 2012   1      .                    */
    /*                         |       order by code                                     | 2013   1      .                    */
    /*                         |       rows between 4 preceding and current row          | 2014   0      3                    */
    /*                         |     ) as five_day_sums                                  | 2015   1      3                    */
    /*                         |   from                                                  | 2016   0      3                    */
    /*                         |     have)                                               | 2017   0      2                    */
    /*                         |   ''')                                                  | 2018   1      2                    */
    /*                         | print(want)                                             | 2014   1      3                    */
    /*                         | fn_tosas9x(want                                         | 2015   1      3                    */
    /*                         |     ,outlib='d:/sd1/'                                   | 2016   0      3                    */
    /*                         |     ,outdsn='pywant'                                    | 2017   1      4                    */
    /*                         |     ,timeest=3);                                        |                                    */
    /*                         | ;;;;                                                    |                                    */
    /*                         | %utl_pyendx;                                            |                                    */
    /*                         |                                                         |                                    */
    /*                         | proc print data=sd1.pywant;                             |                                    */
    /*                         | run;quit;                                               |                                    */
    /*                         |                                                         |                                    */
    /*                         |                                                         |                                    */
    /*                         |                                                         |                                    */
    /*                         | 2 SQL R                                                 | R            moving                */
    /*                         | =======                                                 |    year ei_d   sum                 */
    /*                         |                                                         | 1  2010    1    NA                 */
    /*                         | proc datasets lib=sd1 nolist nodetails;                 | 2  2011    0    NA                 */
    /*                         |  delete want;                                           | 3  2012    1    NA                 */
    /*                         | run;quit;                                               | 4  2013    1    NA                 */
    /*                         |                                                         | 5  2014    0     3                 */
    /*                         | %utl_rbeginx;                                           | 6  2015    1     3                 */
    /*                         | parmcards4;                                             | 7  2016    0     3                 */
    /*                         | library(haven)                                          | 8  2017    0     2                 */
    /*                         | library(sqldf)                                          | 9  2018    1     2                 */
    /*                         | source("c:/oto/fn_tosas9x.R")                           | 10 2014    1     3                 */
    /*                         | options(sqldf.dll = "d:/dll/sqlean.dll")                | 11 2015    1     3                 */
    /*                         | have<-read_sas("d:/sd1/have.sas7bdat")                  | 12 2016    0     3                 */
    /*                         | want<-sqldf('                                           | 13 2017    1     4                 */
    /*                         |  select                                                 | 14 2018    0     3                 */
    /*                         |    year                                                 |                                    */
    /*                         |   ,ei_d                                                 | SAS                                */
    /*                         |   ,case                                                 |             MOVING_                */
    /*                         |      when (rowid < 5 ) then NULL                        | YEAR EI_D  SUM                     */
    /*                         |      else five_day_sums                                 |                                    */
    /*                         |   end as moving_sum                                     | 2010   1    .                      */
    /*                         |  from                                                   | 2011   0    .                      */
    /*                         |   (select                                               | 2012   1    .                      */
    /*                         |     year                                                | 2013   1    .                      */
    /*                         |    ,ei_d                                                | 2014   0    3                      */
    /*                         |    ,rowid                                               | 2015   1    3                      */
    /*                         |    ,sum(ei_d) over (                                    | 2016   0    3                      */
    /*                         |       order by code                                     | 2017   0    2                      */
    /*                         |       rows between 4 preceding and current row          | 2018   1    2                      */
    /*                         |     ) as five_day_sums                                  | 2014   1    3                      */
    /*                         |   from                                                  | 2015   1    3                      */
    /*                         |     have)                                               | 2016   0    3                      */
    /*                         |   ')                                                    | 2017   1    4                      */
    /*                         | want                                                    |                                    */
    /*                         | fn_tosas9x(                                             |                                    */
    /*                         |       inp    = want                                     |                                    */
    /*                         |      ,outlib ="d:/sd1/"                                 |                                    */
    /*                         |      ,outdsn ="want"                                    |                                    */
    /*                         |      )                                                  |                                    */
    /*                         | ;;;;                                                    |                                    */
    /*                         | %utl_rendx;                                             |                                    */
    /*                         |                                                         |                                    */
    /*                         | proc print data=sd1.want;                               |                                    */
    /*                         | run;quit;                                               |                                    */
    /**************************************************************************************************************************/

    Select -rollin|-moving from d:/git/git_010_repos.sasbdat

    REPO
    --------------------------------------------------------------------------------------------------------------------------------------------
    https://github.com/rogerjdeangelis/utl-betas-for-rolling-regressions
    https://github.com/rogerjdeangelis/utl-calculating-a-weighted-or-moving-sum-for-a-window-of-size-three
    https://github.com/rogerjdeangelis/utl-calculating-three-day-moving-sum-by-city-using-r-python-excel-sql
    https://github.com/rogerjdeangelis/utl-calculating-three-year-rolling-moving-weekly-and-annual-daily-standard-deviation
    https://github.com/rogerjdeangelis/utl-centered-moving-average-of-three-observations-using-sas-r-zoo-package-and-sqlite-sqldf
    https://github.com/rogerjdeangelis/utl-clever-algorithms-to-calculate-a-three-month-rolling-averages-and-sums-sas-multilabel-format
    https://github.com/rogerjdeangelis/utl-compute-the-partial-and-total-rolling-sums-for-window-of-size-of-three
    https://github.com/rogerjdeangelis/utl-creating-rolling-sets-of-monthly-tables
    https://github.com/rogerjdeangelis/utl-find-the-nth-largest-value-in-a-moving-window-of-size-m-by-group-r-slider-package-language
    https://github.com/rogerjdeangelis/utl-forecast-the-next-four-months-using-a-moving-average-time-series
    https://github.com/rogerjdeangelis/utl-forecast-the-next-seven-days-using-a--moving-average-model-in-R
    https://github.com/rogerjdeangelis/utl-how-to-compare-price-observations-in-rolling-time-intervals
    https://github.com/rogerjdeangelis/utl-moving-ten-month-average-by-group
    https://github.com/rogerjdeangelis/utl-parallell-processing-a-rolling-moving-three-month-ninety-day-skewness-for-five-thousand-variable
    https://github.com/rogerjdeangelis/utl-proof-of-concept-using-dosubl-to-create-a-fcmp-like-function-for-a-rolling-sum-of-size-three
    https://github.com/rogerjdeangelis/utl-python-r-compute-the-slope-e-of-rolling-window-ofe-size-seven-based-for-a-sine-curve
    https://github.com/rogerjdeangelis/utl-rolling-moving-sum-and-count-over-3-day-window-by-id
    https://github.com/rogerjdeangelis/utl-rolling-sum_of-six-months-by-group
    https://github.com/rogerjdeangelis/utl-timeseries-rolling-three-day-averages-by-county
    https://github.com/rogerjdeangelis/utl-tumbling-goups-of-ten-temperatures-similar-like-rolling-and-moving-means-wps-r-python
    https://github.com/rogerjdeangelis/utl-weight-loss-over-thirty-day-rolling-moving-windows-using-weekly-values
    https://github.com/rogerjdeangelis/utl-weighted-moving-sum-for-several-variables
    https://github.com/rogerjdeangelis/utl_calculate-moving-rolling-average-with-gaps-in-years

    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */
