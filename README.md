# utl_identifying-first-occurrence-after-trigger-event
Identifying first occurrence after trigger event.  Keywords: sas sql join merge big data analytics macros oracle teradata mysql sas communities stackoverflow statistics artificial inteligence AI Python R Java Javascript WPS Matlab SPSS Scala Perl C C# Excel MS Access JSON graphics maps NLP natural language processing machine learning igraph DOSUBL DOW loop stackoverflow SAS community.

    Identifying first occurrence after trigger event

    Same result in SAS and WPS

    For every ID I want to record all 'trigger' events, namely when a=1 and then I need to how long
    it takes to the next occurrence of b=1.

    see
    https://tinyurl.com/yal2omu6
    https://stackoverflow.com/questions/51248793/identifying-first-occurrence-after-trigger-event


    INPUT
    =====                    |  RULES (Two records out)
                             |
     WORK.HAVE total obs=30  |
                             |
       ID     T    A    B    |
                             |
        1     1    0    0    |     First  Next
        1     2    0    0    |       A      B     B-B
        1     3    1    0    | T    BEG    END    DURS
        1     4    0    0    |
        1     5    0    1    | 5     3       5      2

        1     6    1    0    |
        1     7    0    0    |
        1     8    0    0    |
        1     9    0    0    |
        1    10    0    1    |10     6      10      4


    PROCESS
    =======

    data want;
      do until(last.id);
        set have;
        by id;
        retain beg;
        if a=1 then beg=t;
        if b=1 then do;
          end=t;
          durs=end-beg;
          keep id t beg end durs;
          output;
        end;
      end;
    run;quit;


    OUTPUT
    ======

     WORK.WANT total obs=7

      ID     T    BEG    END    DURS

       1     5     3       5      2
       1    10     6      10      4
       2     5     2       5      3
       2     6     2       6      4
       2     7     2       7      5
       2     8     2       8      6
       2    10     9      10      1

    *                _               _       _
     _ __ ___   __ _| | _____     __| | __ _| |_ __ _
    | '_ ` _ \ / _` | |/ / _ \   / _` |/ _` | __/ _` |
    | | | | | | (_| |   <  __/  | (_| | (_| | || (_| |
    |_| |_| |_|\__,_|_|\_\___|   \__,_|\__,_|\__\__,_|

    ;

    data have;
       input id t a b ;
    datalines;
    1 1 0 0
    1 2 0 0
    1 3 1 0
    1 4 0 0
    1 5 0 1
    1 6 1 0
    1 7 0 0
    1 8 0 0
    1 9 0 0
    1 10 0 1
    2 1 0 0
    2 2 1 0
    2 3 0 0
    2 4 0 0
    2 5 0 1
    2 6 0 1
    2 7 0 1
    2 8 0 1
    2 9 1 0
    2 10 0 1
    3 1 0 0
    3 2 0 0
    3 3 0 0
    3 4 0 0
    3 5 0 0
    3 6 0 0
    3 7 1 0
    3 8 0 0
    3 9 0 0
    3 10 0 0
    ;
    run;


    *          _       _   _
     ___  ___ | |_   _| |_(_) ___  _ __
    / __|/ _ \| | | | | __| |/ _ \| '_ \
    \__ \ (_) | | |_| | |_| | (_) | | | |
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|

    ;

    %utl_submit_wps64('
    libname wrk sas7bdat "%sysfunc(pathname(work))";
    data wrk.wantwps;
      do until(last.id);
        set wrk.have;
        by id;
        retain beg;
        if a=1 then beg=t;
        if b=1 then do;
          end=t;
          durs=end-beg;
          keep id t beg end durs;
          output;
        end;
      end;
    run;quit;
    ');

    proc print data=wantwps;
    run;quit;


