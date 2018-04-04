# utl_using_position_of_specifc_values_in_one_array_as_indexes_to_second_array
Using the position specifc values in one array as indexes to a second array.  Keywords: sas sql join merge big data analytics macros oracle teradata mysql sas communities stackoverflow statistics artificial inteligence AI Python R Java Javascript WPS Matlab SPSS Scala Perl C C# Excel MS Access JSON graphics maps NLP natural language processing machine learning igraph DOSUBL DOW loop stackoverflow SAS community.
    SAS Forum: Using the position of specifc values in one array as indexes to a second array

    Same results in WPS and SAS

    github
    https://tinyurl.com/yb9znnrk
    https://github.com/rogerjdeangelis/utl_using_position_of_specifc_values_in_one_array_as_indexes_to_second_array

    Original topic Scanning variable columns in search of specific values

    Nice example of using array pointers

    sas forum
    https://tinyurl.com/ybt4zz9a
    https://communities.sas.com/t5/SAS-Procedures/Scanning-variable-columns-in-search-of-specific-values-for-f-u/m-p/450659

    Ballard W profile
    https://communities.sas.com/t5/user/viewprofilepage/user-id/13884


    INPUT
    =====

    WORK.HAVE total obs=6                             |  RULES for WANT          VALUE
                                                      |                          AT
     VAR1  VAR2  VAR3  VAR4    VAL1  VAL2  VAL3  VAL4 |  VARFIRST0  VARFIRST1    FIRST0  FIRST1
                                                      |
       1     .     0     1      11    22    33    44  |      3 VAR3=0   1 VAR1=0   33      11  VAL[WHERE  VAR3=1]
       .     0     .     .      22    33    44    55  |      2          0          33       .
       .     .     1     0      33    44    55    66  |      4          3          66      55  VAL[WHERE  VAR3=1]
       .     .     0     .      44    55    66    77  |      3          0          66       .
       .     1     0     1      55    66    77    88  |      3          2          77      66
       .     0     0     1      66    77    88    99  |      2 VAR2=0   4 VAR4=1   77      99  VAL[WHERE  VAR3=1]


    PROCESS
    =======

        data want;

           set have;

           array vars[*] var1-var4;
           array vals[*] val1-val4;

           * Index of first zero;
           varfirst0 = whichn(0,of vars[*]);
           varfirst1 = whichn(1,of vars[*]);

           * use indesx to look up value in vals array;
           if varfirst0 then first0 = vals[varfirst0];
           if varfirst1 then first1 = vals[varfirst1];
        run;quit;


    OUTPUT
    ======

     WORK.WANT total obs=6

      VAR1    VAR2    VAR3    VAR4    VAL1    VAL2    VAL3    VAL4    VARFIRST0    VARFIRST1    FIRST0    FIRST1

        1       .       0       1      11      22      33      44         3            1          33        11
        .       0       .       .      22      33      44      55         2            0          33         .
        .       .       1       0      33      44      55      66         4            3          66        55
        .       .       0       .      44      55      66      77         3            0          66         .
        .       1       0       1      55      66      77      88         3            2          77        66
        .       0       0       1      66      77      88      99         2            4          77        99

    *                _              _       _
     _ __ ___   __ _| | _____    __| | __ _| |_ __ _
    | '_ ` _ \ / _` | |/ / _ \  / _` |/ _` | __/ _` |
    | | | | | | (_| |   <  __/ | (_| | (_| | || (_| |
    |_| |_| |_|\__,_|_|\_\___|  \__,_|\__,_|\__\__,_|

    ;

    data have;
       input var1-var4 val1-val4;
    cards4;
    1 . 0 1 11 22 33 44
    . 0 . . 22 33 44 55
    . . 1 0 33 44 55 66
    . . 0 . 44 55 66 77
    . 1 0 1 55 66 77 88
    . 0 0 1 66 77 88 99
    ;;;;
    run;quit;

    *          _       _   _
     ___  ___ | |_   _| |_(_) ___  _ __
    / __|/ _ \| | | | | __| |/ _ \| '_ \
    \__ \ (_) | | |_| | |_| | (_) | | | |
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|

    ;

    * SAS;
    data want;
       set have;
       array vars[*] var1-var4;
       array vals[*] val1-val4;
       varfirst0 = whichn(0,of vars[*]);
       varfirst1 = whichn(1,of vars[*]);
       if varfirst0 then first0 = vals[varfirst0];
       if varfirst1 then first1 = vals[varfirst1];
    run;quit;

    *WPS;
    %utl_submit_wps64('
    libname wrk sas7bdat "%sysfunc(pathname(work))";
    data wrk.want;
       set wrk.have;
       array vars[*] var1-var4;
       array vals[*] val1-val4;
       varfirst0 = whichn(0,of vars[*]);
       varfirst1 = whichn(1,of vars[*]);
       if varfirst0 then first0 = vals[varfirst0];
       if varfirst1 then first1 = vals[varfirst1];
    run;quit;
    ');

