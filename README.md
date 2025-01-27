# utl-change-comprensive-math-score-to-passed-if-student-passed-algebra-or-geometry-sql-r-sas-python
Change comprensive math score to passed if student passed algebra or geometry sql r sas python
    %let pgm=utl-change-comprensive-math-score-to-passed-if-student-passed-algebra-or-geometry-sql-r-sas-python;

    Change comprensive math score to passed if student passed algebra or geometry sql r sas python

    %stop_submission;

          SOLUTIONS

               1 sas r python excel

                 for r python and excel see
                 https://tinyurl.com/ymu776ft
                 https://stackoverflow.com/questions/79378851/mutating-to-return-the-average-of-a-column-in-each-row

               2 r dplyr language

                 I realize this solution is more general
                 but requires substantial knowledge
                 of the dplyr language

                 %>%, across, ==, (but not !== it uses != for ^= ne )
                 uses both == and =, any, .by, .(for any sometimes),
                 ,.default, no else clause with case, ~ sometimes for then,
                 arrange, desc

                 It looks like dplyr and tidyverse make extensive
                 use of special characters which obfuscates r scripts.
                 Reminds me of perl.

    github
    https://tinyurl.com/4xw2mb7w
    https://github.com/rogerjdeangelis/utl-change-comprensive-math-score-to-passed-if-student-passed-algebra-or-geometry-sql-r-sas-python


    related to
    https://stackoverflow.com/questions/79389247/swapping-strings-or-values-in-grouped-data-based-on-condition

    /*               _     _
     _ __  _ __ ___ | |__ | | ___ _ __ ___
    | `_ \| `__/ _ \| `_ \| |/ _ \ `_ ` _ \
    | |_) | | | (_) | |_) | |  __/ | | | | |
    | .__/|_|  \___/|_.__/|_|\___|_| |_| |_|
    |_|
    */

    /**************************************************************************************************************************/
    /*                                  |                                                     |                               */
    /*      INPUT                       |                     PROCESS                         |         OUTPUT                */
    /*                                  |                                                     |                               */
    /*                                  |                                                     |                          NEW_ */
    /* STUDENT EXAM     GRADE           |1 SAS R PYTHON                                       |STUDENT EXAM    OLDGRADE GRADE */
    /*                                  |===============                                      |                               */
    /*   al    acomp     pass           |                                                     |  al    acomp     pass    PASS */
    /*   al    alebra    fail           |select                                               |  al    alebra    fail    FAIL */
    /*   al    geometry  fail           |   l.student                                         |  al    geometry  fail    FAIL */
    /*                                  |  ,l.exam                                            |                               */
    /*   bo    acomp     fail change    |  ,l.grade as prevGrade                              |  bo    acomp     fail  * PASS */
    /*   bo    algebra   pass to pass   |  ,case                                              |  bo    algebra   pass    FAIL */
    /*                        passed    |     when (exam='acomp' and r.anyPass=1) then 'PASS' |                               */
    /*                                  |     else 'FAIL'                                     |  mo    acomp     fail    FAIL */
    /*   mo    acomp     fail no change |   end as new_grade                                  |  mo    geometry  fail    FAIL */
    /*   mo    geometry  fail           |from                                                 |                               */
    /*                                  |   sd1.have as l left join                           |                               */
    /*                                  |     /*--- anyPass=1 if student passed any exam ---*/| * Changed comprehesive math   */
    /* options validvarname=upcase;     |      (select                                        |   to pass because bo passed   */
    /* libname sd1 "d:/sd1";            |          student                                    |   algebra                     */
    /* data sd1.have;                   |         ,max(grade='pass') as anyPass               |                               */
    /*  input  student$ exam$ grade$;   |       from                                          |                               */
    /* cards4;                          |          sd1.have                                   |                               */
    /* al acomp pass                    |       group                                         |                               */
    /* al algebra fail                  |          by student ) as r                          |                               */
    /* al geometry fail                 |on                                                   |                               */
    /* bo acomp fail                    |   l.student = r.student                             |                               */
    /* bo algebra pass                  |order                                                |                               */
    /* mo acomp fail                    |   by l.student, l.exam                              |                               */
    /* mo geometry fail                 |                                                     |                               */
    /* ;;;;                             |                                                     |                               */
    /* run;quit;                        |3 R DPLYR LANGUAGE                                   |                               */
    /*                                  |==================                                   |                               */
    /*                                  |-------------------------------------------------------------------------------------*/
    /*                                  |                                                     |R                              */
    /*                                  |have %>%                                             |                               */
    /*                                  | mutate(                                             | STUDENT EXAM     GRADE        */
    /*                                  |  across(GRADE,                                      |                               */
    /*                                  |   ~ case_when(                                      | al      acomp    pass         */
    /*                                  |    EXAM  == "acomp' & any(GRADE == "pass") ~ "pass",| al      algebra  fail         */
    /*                                  |    EXAM  != "acomp" & . == "yes" ~ "no",            | al      geometry fail         */
    /*                                  |    .default = .)), .by = STUDENT)                   | bo      algebra  pass         */
    /*                                  |want <- have %>% arrange(STUDENT, desc(GRADE))       | bo      acomp    fail         */
    /*                                  |                                                     | mo      acomp    fail         */
    /*                                  |                                                     | mo      geometry fail         */
    /*                                  |                                                     |                               */
    /*                                  |                                                     |SAS                            */
    /*                                  |                                                     |                               */
    /*                                  |                                                     |ROWNAMES STUDENT EXAM     GRADE*/
    /*                                  |                                                     |                               */
    /*                                  |                                                     |    1      al    acomp    pass */
    /*                                  |                                                     |    2      al    algebra  fail */
    /*                                  |                                                     |    3      al    geometry fail */
    /*                                  |                                                     |    4      bo    algebra  pass */
    /*                                  |                                                     |    5      bo    acomp    fail */
    /*                                  |                                                     |    6      mo    acomp    fail */
    /*                                  |                                                     |    7      mo    geometry fail */
    /*                                  |                                                     |                               */
    /**************************************************************************************************************************/
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
     input  student$ exam$ grade$;
    cards4;
    al acomp pass
    al algebra fail
    al geometry fail
    bo acomp fail
    bo algebra pass
    mo acomp fail
    mo geometry fail
    ;;;;
    run;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* STUDENT    EXAM        GRADE                                                                                           */
    /*                                                                                                                        */
    /*   al       acomp       pass                                                                                            */
    /*   al       alebra      fail                                                                                            */
    /*   al       geometry    fail                                                                                            */
    /*   bo       acomp       fail                                                                                            */
    /*   bo       algebra     pass                                                                                            */
    /*   mo       acomp       fail                                                                                            */
    /*   mo       geometry    fail                                                                                            */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*                                      _   _                                    _
    / |  ___  __ _ ___   _ __   _ __  _   _| |_| |__   ___  _ __    _____  _____ ___| |
    | | / __|/ _` / __| | `__| | `_ \| | | | __| `_ \ / _ \| `_ \  / _ \ \/ / __/ _ \ |
    | | \__ \ (_| \__ \ | |    | |_) | |_| | |_| | | | (_) | | | ||  __/>  < (_|  __/ |
    |_| |___/\__,_|___/ |_|    | .__/ \__, |\__|_| |_|\___/|_| |_| \___/_/\_\___\___|_|
                               |_|    |___/
    */

    proc sql;
        create
           table want as
        select
           l.student
          ,l.exam
          ,l.grade as prevGrade
          ,case
             when (exam='acomp' and r.anyPass=1) then 'PASS'
             else 'FAIL'
           end as new_grade
        from
           sd1.have as l left join
              /*--- anyPass=1 if student passed any exam ---*/
              (select
                  student
                 ,max(grade='pass') as anyPass
               from
                  sd1.have
               group
                  by student ) as r
        on
           l.student = r.student
        order
           by l.student, new_grade descending
    ;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*          OUTPUT                                                                                                        */
    /*                                                                                                                        */
    /*                           NEW_                                                                                         */
    /* STUDENT EXAM    OLDGRADE GRADE                                                                                         */
    /*                                                                                                                        */
    /*   al    acomp     pass    PASS                                                                                         */
    /*   al    alebra    fail    FAIL                                                                                         */
    /*   al    geometry  fail    FAIL                                                                                         */
    /*                                                                                                                        */
    /*   bo    acomp     fail  * PASS                                                                                         */
    /*   bo    algebra   pass    FAIL                                                                                         */
    /*                                                                                                                        */
    /*   mo    acomp     fail    FAIL                                                                                         */
    /*   mo    geometry  fail    FAIL                                                                                         */
    /*                                                                                                                        */
    /*                                                                                                                        */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*___               _       _             _
    |___ \   _ __    __| |_ __ | |_   _ _ __ | | __ _ _ __   __ _ _   _  __ _  __ _  ___
      __) | | `__|  / _` | `_ \| | | | | `__|| |/ _` | `_ \ / _` | | | |/ _` |/ _` |/ _ \
     / __/  | |    | (_| | |_) | | |_| | |   | | (_| | | | | (_| | |_| | (_| | (_| |  __/
    |_____| |_|     \__,_| .__/|_|\__, |_|   |_|\__,_|_| |_|\__, |\__,_|\__,_|\__, |\___|
                         |_|      |___/                     |___/             |___/
    */

    proc datasets lib=sd1 nolist nodetails;
     delete want;
    run;quit;

    %utl_rbeginx;
    parmcards4;
    library(haven)
    library(dplyr)
    source("c:/oto/fn_tosas9x.R")
    have<-read_sas("d:/sd1/have.sas7bdat")
    have
    want <-have %>%
      mutate(
        across(GRADE, ~ case_when(
          EXAM == 'acomp' & any(. == "yes") ~ "yes",
          EXAM != 'acomp' & . == "yes" ~ "no",
          .default = .)), .by = STUDENT)
    want
    want <- want %>% arrange(STUDENT, desc(GRADE))
    want
    fn_tosas9x(
          inp    = want
         ,outlib ="d:/sd1/"
         ,outdsn ="want"
         )
    ;;;;
    %utl_rendx;

    proc print data=sd1.want;
    run;quit;

    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */






























































options validvarname=upcase;
libname sd1 "d:/sd1";
data sd1.have;
 input  student$ exam$ grade$;
cards4;
al acomp pass
al algebra fail
al geometry fail
bo acomp fail
bo algebra pass
mo acomp fail
mo geometry fail
;;;;
run;quit;

Obs    STUDENT    EXAM        GRADE

 1       al       acomp       pass
 2       al       algebra     fail
 3       al       geometry    fail
 4       bo       acomp       fail
 5       bo       algebra     pass
 6       mo       acomp       fail
 7       mo       geometry    fail

proc sql;
    create
       table want as
    select
       l.student
      ,l.exam
      ,l.grade as prevGrade
      ,case
         when (exam='acomp' and r.anyPass=1) then 'PASS'
         else 'FAIL'
       end as new_grade
    from
       sd1.have as l left join
          /*--- anyPass=1 if student passed any exam ---*/
          (select
              student
             ,max(grade='pass') as anyPass
           from
              sd1.have
           group
              by student ) as r
    on
       l.student = r.student
    order
       by l.student, new_grade descending
;quit;




proc datasets lib=sd1 nolist nodetails;
 delete want;
run;quit;

%utl_rbeginx;
parmcards4;
library(haven)
source("c:/oto/fn_tosas9x.R")
have<-read_sas("d:/sd1/have.sas7bdat")
print(have)
library(dplyr)
have %>%
  mutate(
   across(GRADE,
    ~ case_when(
      EXAM  == "acomp'  & any(GRADE == "pass") ~ "pass",
      EXAM  != "acomp"  & . == "yes" ~ "no",
      .default = .)), .by = STUDENT)
have
fn_tosas9x(
      inp    = have
     ,outlib ="d:/sd1/"
     ,outdsn ="want"
     )
;;;;
%utl_rendx;

proc print data=sd1.want;
run;quit;



library(dplyr)

df %>%
  mutate(
    across(X:Z, ~ case_when(
      row_number() == 1 & any(. == "yes") ~ "yes",
      row_number() > 1 & . == "yes" ~ "no",
      .default = .)), .by = id)













STUDENT  EXAM     PREVGRADE

  al     acomp      pass
  al     alebra     fail
  al     geometry   fail
  bo     acomp      fail
  bo     algebra    pass
  mo     acomp      fail
  mo     geometry   fail



'acomp ,'algebra ,'geometry ,'acomp ,'algebra ,'acomp ,'geometry'



























proc sql;;



id   X   Y   Z
 1 yes yes yes
 1  no  no  no
 1  no  no  no
 2  no yes  no
 2  no  no  no
 3  no  no  no
 3  no  no  no

yes  yes  yes
no   no   no
no   no   no
no   yes  no
no   no   no
no   no   no



%utl_rbeginx;
parmcards4;
library(haven)
library(dplyr)
source("c:/oto/fn_tosas9x.R")
have<-read_sas("d:/sd1/have.sas7bdat")
print(have)
STUDENT <- c("al","al","al","bo","bo","mo","mo")
EXAM<-c('acomp','algebra','geometry','acomp','algebra','acomp','geometry')
GRADE<- c("yes", "no", "no", "no", "no", "no", "no")
df <- data.frame(STUDENT,EXAM, GRADE)
have %>%
  mutate(
    across(GRADE, ~ case_when(
      EXAM == 'acomp' & any(. == "yes") ~ "yes",
      EXAM != 'acomp' & . == "yes" ~ "no",
      .default = .)), .by = STUDENT)
      !> arrange(STUDENT, desc(GRADE))
;;;;
%utl_rendx;




%utl_rbeginx;
parmcards4;
library(haven)
source("c:/oto/fn_tosas9x.R")
have<-read_sas("d:/sd1/have.sas7bdat")
print(have)
STUDENT <- c("al","al","al","bo","bo","mo","mo")
EXAM<-
GRADE<- c("yes", "no", "no", "no", "no", "no", "no")
df <- data.frame(STUDENT, GRADE)
library(dplyr)
df %>%
  mutate(
    across(GRADE, ~ case_when(
      row_number() == 1 & any(. == "yes") ~ "yes",
      row_number() > 1 & . == "yes" ~ "no",
      .default = .)), .by = STUDENT)
;;;;
%utl_rendx;
































%utl_rbeginx;
parmcards4;
library(haven)
source("c:/oto/fn_tosas9x.R")
have<-read_sas("d:/sd1/have.sas7bdat")
print(have)
STUDENT <- c("al","al","al","bo","bo","mo","mo")
EXAM<- c("yes", "no", "no", "no", "no", "no", "no")
df <- data.frame(STUDENT, EXAM)
library(dplyr)
df %>%
  mutate(
    across(EXAM, ~ case_when(
      row_number() == 1 & any(. == "yes") ~ "yes",
      row_number() > 1 & . == "yes" ~ "no",
      .default = .)), .by = STUDENT)
;;;;
%utl_rendx;



%utl_rbeginx;
parmcards4;
library(haven)
source("c:/oto/fn_tosas9x.R")
have<-read_sas("d:/sd1/have.sas7bdat")
print(have)
id <- c(1,1,1,2,2,3,3)
X <- c("yes", "no", "no", "no", "no", "no", "no")
df <- data.frame(id, X)
library(dplyr)
df %>%
  mutate(
    across(X, ~ case_when(
      row_number() == 1 & any(. == "yes") ~ "yes",
      row_number() > 1 & . == "yes" ~ "no",
      .default = .)), .by = id)
;;;;
%utl_rendx;












 e check each group of rows sharing the same id:

If any row within that group has yes, then the first row of the group is changed to yes. For all subsequent rows of the group, any yes is flipped to no. All other values remain unchanged.



proc sql;
  create
    table want as
  select
     l.row as lrow
    ,r.row as rrow
    ,l.id  as lid
    ,r.id  as rid
    ,l.x as ix
    ,r.x  as  rx
    ,l.y as  ly
    ,r.y as  ry
    ,l.z as  lz
    ,r.z as  rz
  from
    sd1.have as l full outer join sd1.have as r
  on
    l.row ne r.row
  group
    by l.row
  order
    by l.row, r.row
;quit;


proc sql;
  create
    table want as
  select
    distinct
     l.row as lrow
    ,l.id
    ,
  from
    sd1.have as l full outer join sd1.have as r
  on
    l.row ne r.row
  group
    by l.row, l.id
  order
    by l.row, r.row
;quit;

    ,max(((l.x ='yes') + (r.x='yes'))) as xmax


    ,l.y as  ly
    ,l.z as  lz
    ,r.x  as  rx
    ,r.y as  ry
    ,r.z as  rz


proc sql;

  select
     id
    ,case
       when ( row=1 and (select max(x='yes') from sd1.have)) then 'yes'
       else 'no'
     end as x
    ,case
       when ( row=1 and (select max(y='yes') from sd1.have)) then 'yes'
       else 'no'
     end as y
    ,case
       when ( row=1 and (select max(z='yes') from sd1.have)) then 'yes'
       else 'no'
     end as z
  from
     sd1.have
  group
     by id
;quit;

proc sql;
select id, max(x='yes') as fx,max(y='yes') as fy,max(z='yes') as fz from sd1.have group by id
;quit;


    ;;;;%end;%mend;/*'*/ *);*};*];*/;/*"*/;run;quit;%end;end;run;endcomp;%utlfix;






%utl_rbeginx;
parmcards4;
id <- c(1,1,1,2,2,3,3)
X <- c("yes", "no", "no", "no", "no", "no", "no")
Y <- c("no", "no", "yes", "no", "yes", "no", "no")
Z <- c("no", "yes", "no", "no", "no", "no", "no")
df <- data.frame(id, X, Y, Z)
df
;;;;
%utl_rendx;



proc datasets lib=sd1 nolist nodetails;
 delete want;
run;quit;

%utl_rbeginx;
parmcards4;
library(haven)
source("c:/oto/fn_tosas9x.R")
have<-read_sas("d:/sd1/have.sas7bdat")
print(have)
fn_tosas9x(
      inp    = have
     ,outlib ="d:/sd1/"
     ,outdsn ="want"
     )
;;;;
%utl_rendx;

proc print data=sd1.want;
run;quit;
