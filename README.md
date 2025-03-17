# utl-dropping-down-to-spss-using-the-pspp-free-clone-and-running-a-spss-linear-regression
Dropping down to spss using the pspp free clone and running a spss linear regression
    %let pgm=utl-dropping-down-to-spss-using-the-pspp-free-clone-and-running-a-spss-linear-regression;

    %stop_submission;

    Dropping down to spss using the pspp free clone and running a spss linear regression;

    SOAPBOX ON
      Could not find a free clone of STATA?
    SOAPBOX OFF

    Thus runs a regression using SPSS  program on a sas dataset and returns a sas dataset

    Download and install pspp statistical package
    https://www.gnu.org/software/pspp/get.html

        CONTENTS

          1 r convert sas table to spss
          2 spss linear regression output spss sav table
          3 r convert spss table to sas dataset
          4 drop down macros
          5 compare sas proc reg


    github
    https://tinyurl.com/3rs385x9
    https://github.com/rogerjdeangelis/utl-dropping-down-to-spss-using-the-pspp-free-clone-and-running-a-spss-linear-regression
   Related repos
   REPO
   ----------------------------------------------------------------------------------------------------------------------------------
   https://github.com/rogerjdeangelis/utl-creating-spss-tables-from-a-sas-datasets-using-sas-r-and-python
   https://github.com/rogerjdeangelis/utl-identifying-the-html-table-and-exporting-to-spss-then-sas-scraping
   https://github.com/rogerjdeangelis/utl-import-dbf-dif-ods-xlsx-spss-json-stata-csv-html-xml-tsv-files-without-sas-access-products
   https://github.com/rogerjdeangelis/utl-removing-factors-and-preserving-type-and-length-when-importing-spss-sav-tables
   https://github.com/rogerjdeangelis/utl-sas-to-and-from-sqllite-excel-ms-access-spss-stata-using-r-packages-without-sas

    /*               _     _
     _ __  _ __ ___ | |__ | | ___ _ __ ___
    | `_ \| `__/ _ \| `_ \| |/ _ \ `_ ` _ \
    | |_) | | | (_) | |_) | |  __/ | | | | |
    | .__/|_|  \___/|_.__/|_|\___|_| |_| |_|
    |_|
    */

    /**************************************************************************************************************************/
    /*            INPUT              |        PROCESS                |                 OUTPUT (SPSS REGRESSION)               */
    /*            =====              |        =======                |                 ========================               */
    /*                               |                               |                                                        */
    /* NAME    SEX AGE HEIGHT WEIGHT | 1 R SAS TABLE TO SPSS TABLE   | SAS SPSS(PSPP) LOG (also in c:/temp/ps.pgm.log)        */
    /*                               | ===========================   |                                                        */
    /* Alfred   M   14  69.0   112.5 |                               |                                                        */
    /* Alice    F   13  56.5    84.0 | %utl_rbeginx;                 |             Model Summary (HEIGHT)                     */
    /* Barbara  F   13  65.3    98.0 | parmcards4;                   | +---+--------+-----------+-------------------+         */
    /* Carol    F   14  62.8   102.5 | library(haven);               | | R |R Square|AdjR Square|Std. Error Estimate|         */
    /* Henry    M   14  63.5   102.5 | have<-read_sas(               | +---+--------+-----------+-------------------+         */
    /* James    M   12  57.3    83.0 |  "d:/sd1/have.sas7bdat");     | |.88|     .77|        .76|               2.53|         */
    /* Jane     F   12  59.8    84.5 | str(have);                    | +---+--------+-----------+-------------------+         */
    /* Janet    F   15  62.5   112.5 | write_sav(have,               |                                                        */
    /* Jeffrey  M   13  62.5    84.0 |  "d:/sav/want.sav");          |                     ANOVA (HEIGHT)                     */
    /* John     M   12  59.0    99.5 | ;;;;                          | +----------+--------+--+-------+-----+----+            */
    /* Joyce    F   11  51.3    50.5 | %utl_rendx;                   | |          | Sum    | f| Mean  |     |   .|            */
    /* Judy     F   14  64.3    90.0 |                               | |          | Squares|df| Square|  F  |Sig.|            */
    /* Louise   F   12  56.3    77.0 |                               | +----------+--------+--+-------+-----+----+            */
    /* Mary     F   15  66.5   112.0 | 2 SPSS LINEAR REGRESSION      | }Regression|  364.58| 1| 364.58|57.08|.000|            */
    /* Philip   M   16  72.0   150.0 | ========================      | |Residual  |  108.59|17|   6.39|     |    |            */
    /* Robert   M   12  64.8   128.0 |                               | |Total     |  473.16|18|       |     |    |            */
    /* Ronald   M   15  67.0   133.0 | %utl_psppbeginx;              | +----------+--------+--+-------+-----+----+            */
    /* Thomas   M   11  57.5    85.0 | parmcards4;                   |                                                        */
    /* William  M   15  66.5   112.0 | GET FILE="d:/sav/want.sav" .  |                    Coefficients (HEIGHT)               */
    /*                               | REGRESSION                    | +----------+----------------------+----+-----+----+    */
    /*                               | /VARIABLES=weight height      | |          | Unstandard|Standard  |    |     |    |    */
    /* options validvarname=upcase;  |   /DEPENDENT=height           | |          | Coefficien|Coefficien|    |     |    |    */
    /* libname sd1 "d:/sd1";         |   /METHOD=ENTER               | |          +-----------+-----+----+    |     |    |    */
    /* data sd1.have;                |   /SAVE PRED RESID.           | |          |     B     |  StdError|Beta|  t  |Sig.|    */
    /*   set sashelp.class;          | SAVE                          | +----------+-----------+-------_--+----+-----+----+    */
    /* run;quit;                     |  OUTFILE="d:\sav\wantout.sav".| |(Constant)|      42.57|      2.68| .00|15.89|.000|    */
    /*                               | ;;;;                          | |WEIGHT    |        .20|       .03| .88| 7.55|.000|    */
    /*                               | %utl_psppendx;                | +----------+-----------+----------+----+-----+----+    */
    /*                               |                               |                                                        */
    /*                               |                               |                                   OUTPUT               */
    /*                               | 3 R SPSS TABLE TO SAS DATASET | R dataframe & SAS dataset      Residual Pred           */
    /*                               | ============================= |                               ============++           */
    /*                               |                               | NAME    SEX  AGE HEIGHT WEIGHT    RES1 PRED1           */
    /*                               |                               |                                                        */
    /*                               | %utl_rbeginx;                 | Alfred  M     14   69    112.   4.20    64.8           */
    /*                               | parmcards4;                   | Alice   F     13   56.5   84   -2.67    59.2           */
    /*                               | library(haven);               | Barbara F     13   65.3   98    3.36    61.9           */
    /*                               | source("c:/oto/fn_tosas9x.R") | Carol   F     14   62.8  102.  -0.0257  62.8           */
    /*                               | want_sas<-read_sav(           | Henry   M     14   63.5  102.   0.674   62.8           */
    /*                               | "d:/sav/wantout.sav");        | James   M     12   57.3   83   -1.67    59.0           */
    /*                               | want_sas                      | Jane    F     12   59.8   84.5  0.531   59.3           */
    /*                               | fn_tosas9x(                   | Janet   F     15   62.5  112.  -2.30    64.8           */
    /*                               |       inp    = want_sas       | Jeffrey M     13   62.5   84    3.33    59.2           */
    /*                               |      ,outlib ="d:/sd1/"       | John    M     12   59     99.5 -3.23    62.2           */
    /*                               |      ,outdsn ="want_sas"      | Joyce   F     11   51.3   50.5 -1.25    52.5           */
    /*                               |      )                        | Judy    F     14   64.3   90    3.94    60.4           */
    /*                               | ;;;;                          | Louise  F     12   56.3   77   -1.49    57.8           */
    /*                               | %utl_rendx;                   | Mary    F     15   66.5  112    1.80    64.7           */
    /*                               |                               | Philip  M     16   72    150   -0.212   72.2           */
    /*                               | libname sd1 "d:/sd1";         | Robert  M     12   64.8  128   -3.06    67.9           */
    /*                               | proc print                    | Ronald  M     15   67    133   -1.85    68.9           */
    /*                               |  data=sd1.want_sas;           | Thomas  M     11   57.5   85   -1.87    59.4           */
    /*                               | run;quit;                     | William M     15   66.5  112    1.80    64.7           */
    /***************************************************************************************************************************/

     /*                   _
    (_)_ __  _ __  _   _| |_
    | | `_ \| `_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|
    */

     options validvarname=upcase;
     libname sd1 "d:/sd1";
     data sd1.have;
       set sashelp.class;
     run;quit;

    /***************************************************************************************************************************/
    /*    NAME    SEX AGE HEIGHT WEIGHT                                                                                        */
    /*                                                                                                                        */
    /*    Alfred   M   14  69.0   112.5                                                                                       */
    /*    Alice    F   13  56.5    84.0                                                                                       */
    /*    Barbara  F   13  65.3    98.0                                                                                       */
    /*    Carol    F   14  62.8   102.5                                                                                       */
    /*    Henry    M   14  63.5   102.5                                                                                       */
    /*    James    M   12  57.3    83.0                                                                                       */
    /*    Jane     F   12  59.8    84.5                                                                                       */
    /*    Janet    F   15  62.5   112.5                                                                                       */
    /*    Jeffrey  M   13  62.5    84.0                                                                                       */
    /*    John     M   12  59.0    99.5                                                                                       */
    /*    Joyce    F   11  51.3    50.5                                                                                       */
    /*    Judy     F   14  64.3    90.0                                                                                       */
    /*    Louise   F   12  56.3    77.0                                                                                       */
    /*    Mary     F   15  66.5   112.0                                                                                       */
    /*    Philip   M   16  72.0   150.0                                                                                       */
    /*    Robert   M   12  64.8   128.0                                                                                       */
    /*    Ronald   M   15  67.0   133.0                                                                                       */
    /*    Thomas   M   11  57.5    85.0                                                                                       */
    /*    William  M   15  66.5   112.0                                                                                       */
    /***************************************************************************************************************************/

    /*                                        _                    _        _     _        _
    / |  _ __    ___ ___  _ ____   _____ _ __| |_   ___  __ _ ___ | |_ __ _| |__ | | ___  | |_ ___    ___ _ __  ___ ___
    | | | `__|  / __/ _ \| `_ \ \ / / _ \ `__| __| / __|/ _` / __|| __/ _` | `_ \| |/ _ \ | __/ _ \  / __| `_ \/ __/ __|
    | | | |    | (_| (_) | | | \ V /  __/ |  | |_  \__ \ (_| \__ \| || (_| | |_) | |  __/ | || (_) | \__ \ |_) \__ \__ \
    |_| |_|     \___\___/|_| |_|\_/ \___|_|   \__| |___/\__,_|___/ \__\__,_|_.__/|_|\___|  \__\___/  |___/ .__/|___/___/
                                                                                                         |_|
    */

    %utlfkil(d:/sav/want.sav);

    %utl_rbeginx;
    parmcards4;
    library(haven);
    have<-read_sas(
     "d:/sd1/have.sas7bdat");
    str(have);
    have
    write_sav(have,
     "d:/sav/want.sav");
    ;;;;
    %utl_rendx;

    /**************************************************************************************************************************/
    /*  d:/sav/want.sav SPSS Table                                                                                            */
    /*                                                                                                                        */
    /*    NAME    SEX  AGE HEIGHT WEIGHT                                                                                      */
    /*  1 Alfred  M     14   69    112.                                                                                       */
    /*  2 Alice   F     13   56.5   84                                                                                        */
    /*  3 Barbara F     13   65.3   98                                                                                        */
    /*  4 Carol   F     14   62.8  102.                                                                                       */
    /*  5 Henry   M     14   63.5  102.                                                                                       */
    /*  6 James   M     12   57.3   83                                                                                        */
    /*  7 Jane    F     12   59.8   84.5                                                                                      */
    /*  8 Janet   F     15   62.5  112.                                                                                       */
    /*  9 Jeffrey M     13   62.5   84                                                                                        */
    /*  0 John    M     12   59     99.5                                                                                      */
    /*  1 Joyce   F     11   51.3   50.5                                                                                      */
    /*  2 Judy    F     14   64.3   90                                                                                        */
    /*  3 Louise  F     12   56.3   77                                                                                        */
    /*  4 Mary    F     15   66.5  112                                                                                        */
    /*  5 Philip  M     16   72    150                                                                                        */
    /*  6 Robert  M     12   64.8  128                                                                                        */
    /*  7 Ronald  M     15   67    133                                                                                        */
    /*  8 Thomas  M     11   57.5   85                                                                                        */
    /*  9 William M     15   66.5  112                                                                                        */
    /**************************************************************************************************************************/

    /*___                        _ _                                                         _
    |___ \   ___ _ __  ___ ___  | (_)_ __   ___  __ _ _ __  _ __ ___  __ _ _ __ ___  ___ ___(_) ___  _ __
      __) | / __| `_ \/ __/ __| | | | `_ \ / _ \/ _` | `__|| `__/ _ \/ _` | `__/ _ \/ __/ __| |/ _ \| `_ \
     / __/  \__ \ |_) \__ \__ \ | | | | | |  __/ (_| | |   | | |  __/ (_| | | |  __/\__ \__ \ | (_) | | | |
    |_____| |___/ .__/|___/___/ |_|_|_| |_|\___|\__,_|_|   |_|  \___|\__, |_|  \___||___/___/_|\___/|_| |_|
                |_|                                                  |___/
    */

    %utlfkil(d:\sav\wantout.sav);

    %utl_psppbeginx;
    parmcards4;
    GET FILE="d:/sav/want.sav" .
    REGRESSION
    /VARIABLES=weight height
      /DEPENDENT=height
      /METHOD=ENTER
      /SAVE PRED RESID.
    SAVE
     OUTFILE="d:\sav\wantout.sav".
    ;;;;
    %utl_psppendx;

    /**************************************************************************************************************************/
    /* SAS LOG (also in c:/temp/ps.pgm.log)                                                                                   */
    /*                                                                                                                        */
    /*                                                                                                                        */
    /*                    Model Summary (HEIGHT)                                                                              */
    /* +---+--------+-----------------+--------------------------+                                                            */
    /* | R |R Square|Adjusted R Square|Std. Error of the Estimate|                                                            */
    /* +---+--------+-----------------+--------------------------+                                                            */
    /* |.88|     .77|              .76|                      2.53|                                                            */
    /* +---+--------+-----------------+--------------------------+                                                            */
    /*                                                                                                                        */
    /*                     ANOVA (HEIGHT)                                                                                     */
    /* +----------+--------------+--+-----------+-----+----+                                                                  */
    /* |          |Sum of Squares|df|Mean Square|  F  |Sig.|                                                                  */
    /* +----------+--------------+--+-----------+-----+----+                                                                  */
    /* |Regression|        364.58| 1|     364.58|57.08|.000|                                                                  */
    /* |Residual  |        108.59|17|       6.39|     |    |                                                                  */
    /* |Total     |        473.16|18|           |     |    |                                                                  */
    /* +----------+--------------+--+-----------+-----+----+                                                                  */
    /*                                                                                                                        */
    /*                              Coefficients (HEIGHT)                                                                     */
    /* +----------+----------------------------+-------------------------+-----+----+                                         */
    /* |          | Unstandardized Coefficients|Standardized Coefficients|     |    |                                         */
    /* |          +-----------+----------------+-------------------------+     |    |                                         */
    /* |          |     B     |   Std. Error   |           Beta          |  t  |Sig.|                                         */
    /* +----------+-----------+----------------+-------------------------+-----+----+                                         */
    /* |(Constant)|      42.57|            2.68|                      .00|15.89|.000|                                         */
    /* |WEIGHT    |        .20|             .03|                      .88| 7.55|.000|                                         */
    /* +----------+-----------+----------------+-------------------------+-----+----+                                         */
    /*                                                                                                                        */
    /*                                                                                                                        */
    /*   d:\sav\wantout.sav                                                                                                   */
    /*                                                                                                                        */
    /*                                     OUTPUT                                                                             */
    /*   R dataframe & SAS dataset      Residual Pred                                                                         */
    /*                                 ============++                                                                         */
    /*   NAME    SEX  AGE HEIGHT WEIGHT    RES1 PRED1                                                                         */
    /*                                                                                                                        */
    /*   Alfred  M     14   69    112.   4.20    64.8                                                                         */
    /*   Alice   F     13   56.5   84   -2.67    59.2                                                                         */
    /*   Barbara F     13   65.3   98    3.36    61.9                                                                         */
    /*   Carol   F     14   62.8  102.  -0.0257  62.8                                                                         */
    /*   Henry   M     14   63.5  102.   0.674   62.8                                                                         */
    /*   James   M     12   57.3   83   -1.67    59.0                                                                         */
    /*   Jane    F     12   59.8   84.5  0.531   59.3                                                                         */
    /*   Janet   F     15   62.5  112.  -2.30    64.8                                                                         */
    /*   Jeffrey M     13   62.5   84    3.33    59.2                                                                         */
    /*   John    M     12   59     99.5 -3.23    62.2                                                                         */
    /*   Joyce   F     11   51.3   50.5 -1.25    52.5                                                                         */
    /*   Judy    F     14   64.3   90    3.94    60.4                                                                         */
    /*   Louise  F     12   56.3   77   -1.49    57.8                                                                         */
    /*   Mary    F     15   66.5  112    1.80    64.7                                                                         */
    /*   Philip  M     16   72    150   -0.212   72.2                                                                         */
    /*   Robert  M     12   64.8  128   -3.06    67.9                                                                         */
    /*   Ronald  M     15   67    133   -1.85    68.9                                                                         */
    /*   Thomas  M     11   57.5   85   -1.87    59.4                                                                         */
    /*   William M     15   66.5  112    1.80    64.7                                                                         */
    /**************************************************************************************************************************/

    /*____                                        _                        _        _     _        _
    |___ /   _ __    ___ ___  _ ____   _____ _ __| |_   ___ _ __  ___ ___ | |_ __ _| |__ | | ___  | |_ ___    ___  __ _ ___
      |_ \  | `__|  / __/ _ \| `_ \ \ / / _ \ `__| __| / __| `_ \/ __/ __|| __/ _` | `_ \| |/ _ \ | __/ _ \  / __|/ _` / __|
     ___) | | |    | (_| (_) | | | \ V /  __/ |  | |_  \__ \ |_) \__ \__ \| || (_| | |_) | |  __/ | || (_) | \__ \ (_| \__ \
    |____/  |_|     \___\___/|_| |_|\_/ \___|_|   \__| |___/ .__/|___/___/ \__\__,_|_.__/|_|\___|  \__\___/  |___/\__,_|___/
                                                           |_|
    */

    proc datasets lib=sd1 nolist nodetails;
     delete want_sas;
    run;quit;

    %utl_rbeginx;
    parmcards4;
    library(haven);
    source("c:/oto/fn_tosas9x.R")
    want_sas<-read_sav(
    "d:/sav/wantout.sav");
    want_sas
    fn_tosas9x(
          inp    = want_sas
         ,outlib ="d:/sd1/"
         ,outdsn ="want_sas"
         )
    ;;;;
    %utl_rendx;

    libname sd1 "d:/sd1";
    proc print
     data=sd1.want_sas;
    run;quit;

    /**************************************************************************************************************************/
    /* R                                                                                                                      */
    /*                                                                                                                        */
    /* NAME    SEX     AGE HEIGHT WEIGHT    RES1 PRED1                                                                        */
    /* <chr>   <chr> <dbl>  <dbl>  <dbl>   <dbl> <dbl>                                                                        */
    /* Alfred  M        14   69    112.   4.20    64.8                                                                        */
    /* Alice   F        13   56.5   84   -2.67    59.2                                                                        */
    /* Barbara F        13   65.3   98    3.36    61.9                                                                        */
    /* Carol   F        14   62.8  102.  -0.0257  62.8                                                                        */
    /* Henry   M        14   63.5  102.   0.674   62.8                                                                        */
    /* James   M        12   57.3   83   -1.67    59.0                                                                        */
    /* Jane    F        12   59.8   84.5  0.531   59.3                                                                        */
    /* Janet   F        15   62.5  112.  -2.30    64.8                                                                        */
    /* Jeffrey M        13   62.5   84    3.33    59.2                                                                        */
    /* John    M        12   59     99.5 -3.23    62.2                                                                        */
    /* Joyce   F        11   51.3   50.5 -1.25    52.5                                                                        */
    /* Judy    F        14   64.3   90    3.94    60.4                                                                        */
    /* Louise  F        12   56.3   77   -1.49    57.8                                                                        */
    /* Mary    F        15   66.5  112    1.80    64.7                                                                        */
    /* Philip  M        16   72    150   -0.212   72.2                                                                        */
    /* Robert  M        12   64.8  128   -3.06    67.9                                                                        */
    /* Ronald  M        15   67    133   -1.85    68.9                                                                        */
    /* Thomas  M        11   57.5   85   -1.87    59.4                                                                        */
    /* William M        15   66.5  112    1.80    64.7                                                                        */
    /*                                                                                                                        */
    /* SAS                                                                                                                    */
    /*                                                                                                                        */
    /* ROWNAMES    NAME       SEX    AGE    HEIGHT    WEIGHT      RES1       PRED1                                            */
    /*                                                                                                                        */
    /*     1       Alfred      M      14     69.0      112.5     4.19817    64.8018                                           */
    /*     2       Alice       F      13     56.5       84.0    -2.66980    59.1698                                           */
    /*     3       Barbara     F      13     65.3       98.0     3.36359    61.9364                                           */
    /*     4       Carol       F      14     62.8      102.5    -0.02568    62.8257                                           */
    /*     5       Henry       M      14     63.5      102.5     0.67432    62.8257                                           */
    /*     6       James       M      12     57.3       83.0    -1.67219    58.9722                                           */
    /*     7       Jane        F      12     59.8       84.5     0.53139    59.2686                                           */
    /*     8       Janet       F      15     62.5      112.5    -2.30183    64.8018                                           */
    /*     9       Jeffrey     M      13     62.5       84.0     3.33020    59.1698                                           */
    /*    10       John        M      12     59.0       99.5    -3.23283    62.2328                                           */
    /*    11       Joyce       F      11     51.3       50.5    -1.24970    52.5497                                           */
    /*    12       Judy        F      14     64.3       90.0     3.94451    60.3555                                           */
    /*    13       Louise      F      12     56.3       77.0    -1.48650    57.7865                                           */
    /*    14       Mary        F      15     66.5      112.0     1.79698    64.7030                                           */
    /*    15       Philip      M      16     72.0      150.0    -0.21239    72.2124                                           */
    /*    16       Robert      M      12     64.8      128.0    -3.06486    67.8649                                           */
    /*    17       Ronald      M      15     67.0      133.0    -1.85294    68.8529                                           */
    /*    18       Thomas      M      11     57.5       85.0    -1.86742    59.3674                                           */
    /*    19       William     M      15     66.5      112.0     1.79698    64.7030                                           */
    /**************************************************************************************************************************/

    /*  _         _                       _
    | || |     __| |_ __ ___  _ __     __| | _____      ___ __   _ __ ___   __ _  ___ _ __ ___  ___
    | || |_   / _` | `__/ _ \| `_ \   / _` |/ _ \ \ /\ / / `_ \ | `_ ` _ \ / _` |/ __| `__/ _ \/ __|
    |__   _| | (_| | | | (_) | |_) | | (_| | (_) \ V  V /| | | || | | | | | (_| | (__| | | (_) \__ \
       |_|    \__,_|_|  \___/| .__/   \__,_|\___/ \_/\_/ |_| |_||_| |_| |_|\__,_|\___|_|  \___/|___/
    */

    filename ft15f001 "c:/oto/utl_psppbeginx.sas";
    parmcards4;
    %macro utl_psppbeginx;
    %utlfkil(c:/temp/ps_pgm.ps);
    %utlfkil(c:/temp/ps_pgm.log);
    filename ft15f001 "c:/temp/ps_pgm.ps1";
    %mend utl_psppbeginx;
    ;;;;

    %macro utl_psppendx(returnvar=N);
    options noxwait noxsync;
    filename rut pipe  "c:\PROGRA~1\PSPP\bin\pspp.exe c:/temp/ps_pgm.ps1 >  c:/temp/ps_pgm.log";
    run;quit;
      data _null_;
        file print;
        infile rut recfm=v lrecl=32756;
        input;
        put _infile_;
        putlog _infile_;
      run;
      filename ft15f001 clear;
      * use the clipboard to create macro variable;
      %if %upcase(%substr(&returnVar.,1,1)) ne N %then %do;
        filename clp clipbrd ;
        data _null_;
         length txt $200;
         infile clp;
         input;
         putlog "macro variable &returnVar = " _infile_;
         call symputx("&returnVar.",_infile_,"G");
        run;quit;
      %end;
    data _null_;
      file print;
      infile rut;
      input;
      put _infile_;
      putlog _infile_;
    run;quit;
    data _null_;
      infile "c:/temp/ps_pgm.log";
      input;
      putlog _infile_;
    run;quit;
    filename ft15f001 clear;
    %mend utl_psppendx;

    /*___
    | ___|    ___ ___  _ __ ___  _ __   __ _ _ __ ___   ___  __ _ ___  _ __  _ __ ___   ___   _ __ ___  __ _
    |___ \   / __/ _ \| `_ ` _ \| `_ \ / _` | `__/ _ \ / __|/ _` / __|| `_ \| `__/ _ \ / __| | `__/ _ \/ _` |
     ___) | | (_| (_) | | | | | | |_) | (_| | | |  __/ \__ \ (_| \__ \| |_) | | | (_) | (__  | | |  __/ (_| |
    |____/   \___\___/|_| |_| |_| .__/ \__,_|_|  \___| |___/\__,_|___/| .__/|_|  \___/ \___| |_|  \___|\__, |
                                |_|                                   |_|                              |___/
    */

    proc reg data=sashelp.class;
     model height=weight;
    run;quit;

    /**************************************************************************************************************************/
    /* Dependent Variable: HEIGHT                                                Model Summary (HEIGHT)                       */
    /*                                                               +---+--------+-----------+-------------------+           */
    /* Number of Observations Read  19                               | R |R Square|AdjR Square|Std. Error Estimate|           */
    /* Number of Observations Used  19                               +---+--------+-----------+-------------------+           */
    /*                                                               |.88|     .77|        .76|               2.53|           */
    /*                                                               +---+--------+-----------+-------------------+           */
    /*                      Analysis of Variance                                                                              */
    /*                                                                                   ANOVA (HEIGHT)                       */
    /*                          Sum of       Mean                    +----------+--------+--+-------+-----+----+              */
    /* Source           DF     Squares     Square  F Value Pr > F    |          | Sum    | f| Mean  |     |   .|              */
    /*                                                               |          | Squares|df| Square|  F  |Sig.|              */
    /* Model             1   364.57626  364.57626    57.08 <.0001    +----------+--------+--+-------+-----+----+              */
    /* Error            17   108.58795    6.38753                    }Regression|  364.58| 1| 364.58|57.08|.000|              */
    /* Corrected Total  18   473.16421                               |Residual  |  108.59|17|   6.39|     |    |              */
    /*                                                               |Total     |  473.16|18|       |     |    |              */
    /*                                                               +----------+--------+--+-------+-----+----+              */
    /* Root MSE         2.52736 R-Square     0.7705                                                                           */
    /* Dependent Mean  62.33684 Adj R-Sq     0.7570                                     Coefficients (HEIGHT)                 */
    /* Coeff Var        4.05435                                      +----------+----------------------+----+-----+----+      */
    /*                                                               |          | Unstandard|Standard  |    |     |    |      */
    /*                                                               |          | Coefficien|Coefficien|    |     |    |      */
    /*                 Parameter Estimates                           |          +-----------+-----+----+    |     |    |      */
    /*                                                               |          |     B     |  StdError|Beta|  t  |Sig.|      */
    /*              Parameter  Standard                              +----------+-----------+-------_--+----+-----+----+      */
    /* Variable  DF  Estimate     Error t Value    Pr > |t|          |(Constant)|      42.57|      2.68| .00|15.89|.000|      */
    /*                                                               |WEIGHT    |        .20|       .03| .88| 7.55|.000|      */
    /* Intercept  1  42.57014   2.67989   15.89      <.0001          +----------+-----------+----------+----+-----+----+      */
    /* WEIGHT     1   0.19761   0.02616    7.55      <.0001                                                                   */
    /**************************************************************************************************************************/

    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */
