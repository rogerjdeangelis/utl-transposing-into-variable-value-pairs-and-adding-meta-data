# utl-transposing-into-variable-value-pairs-and-adding-meta-data
Transposing into variable value pairs and adding meta data

    Transposing into variable value pairs and adding meta data

    https://tinyurl.com/ycqol3qb
    https://github.com/rogerjdeangelis/utl-transposing-into-variable-value-pairs-and-adding-meta-data

    StackOverflow
    https://tinyurl.com/yaep4a4o
    https://stackoverflow.com/questions/53540521/how-to-group-50-columns-for-every-row-in-sas-and-create-a-new-column-for-their-v

    Data_null_
    https://stackoverflow.com/users/2196220/data-null

    INPUT
    =====

     WORK.HAVE total obs=3

      ID     column1    column2    column3  ...   column50

      JAN       1          2          3     ...      5
      FEB       6          7          8              0
      APR       2          7          2              4


     EXAMPLE OUTPUT
     --------------

     WORK.WANT total obs=150

             Grouped
      ID      Column     Value

      JAN    column1       1
      JAN    column2       2
      JAN    column3       3
      JAN    column4       4
      JAN    column5       5
     ...
      JAN    column48      2
      JAN    column49      3
      JAN    column50      4


     FORMAT AND LABEL ADD USING PROC TRANSPOSE
     -----------------------------------------

        Variables in Creation Order

      Variable           Format    Label

      id                 $9.       Month
      GroupedColumn                Column1-50
      Value              F2.       Source Value


    PROCESS
    =======

    proc transpose data=have out=want(rename=(_name_=GroupedColumn col1=Value)) ;

      * adjust input meta data;
       format id $9.;
       label id="Month";

      * adjust output meta data;
       attrib _name_ :   label='Source Variable name' format=10.
              value  :   label='Source Value' format=2.
              GroupedColumn : label='Column1-50'
       ;
    by id notsorted;
    var column1-column50;
    run;

    I believe other proceedures support adding or changing meta data attributes
    ie 'proc sort'


    OUTPUT
    ======

    see above

    *                _               _       _
     _ __ ___   __ _| | _____     __| | __ _| |_ __ _
    | '_ ` _ \ / _` | |/ / _ \   / _` |/ _` | __/ _` |
    | | | | | | (_| |   <  __/  | (_| | (_| | || (_| |
    |_| |_| |_|\__,_|_|\_\___|   \__,_|\__,_|\__\__,_|

    ;

    DATA have;
     INPUT ID$ column1-column50;
    cards4;
    JAN 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6  1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4
    FEB 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 1 2 3 4 5  6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 1 2 3
    APR 2 7 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 1 2 3 4 5 6 7 8 9  2 7 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 1 2 3 4 5 6 7
    ;;;;
    run;quit;




