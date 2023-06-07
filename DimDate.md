La création d'une dimension Date est un besoin fondamental (sauf dans les cas où l'équipe d'architecture de données a créé une table Date qui s'adapte aux besoins de l'analyse demandée).

Pour la création de la table DimDate, il y a n variations qui peuvent être faites dans DAX pour créer la nouvelle table, j'attache 3 options avec différents niveaux de complexité compte tenu du besoin et de la complexité du projet.

Documentation:
https://learn.microsoft.com/es-es/power-bi/guidance/model-date-tables


### OPTION 1
```
DimDate = CALENDAR("1/1/2013",TODAY())
```
![image](https://github.com/Cristianfllc3/Power_BI_DAX_101/assets/72107370/ddfcd45d-b79e-43ad-9fe3-fe490d34b5d8)


### OPTION 2
```
DimDate = CALENDAR("1/1/2013",TODAY())
```


### OPTION 3
```
DimDate = 
VAR _minYear = YEAR(MIN(DimEmployee[HireDate]))
VAR _maxYear = YEAR(MAX(DimEmployee[HireDate]))
VAR _fiscalStart = 4 

RETURN
ADDCOLUMNS(

    CALENDAR(

                DATE(_minYear,1,1),

                DATE(_maxYear,12,31)

),

"Year",YEAR([Date]),
"Year Start",DATE( YEAR([Date]),1,1),
"YearEnd",DATE( YEAR([Date]),12,31),
"MonthNumber",MONTH([Date]),
"MonthStart",DATE( YEAR([Date]), MONTH([Date]), 1),
"MonthEnd",EOMONTH([Date],0),
"DaysInMonth",DATEDIFF(DATE( YEAR([Date]), MONTH([Date]), 1),EOMONTH([Date],0),DAY)+1,
"YearMonthNumber",INT(FORMAT([Date],"YYYYMM")),
"YearMonthName",FORMAT([Date],"YYYY-MMM"),
"DayNumber",DAY([Date]),
"DayName",FORMAT([Date],"DDDD"),
"DayNameShort",FORMAT([Date],"DDD"),
"DayOfWeek",WEEKDAY([Date]),
"MonthName",FORMAT([Date],"MMMM"),
"MonthNameShort",FORMAT([Date],"MMM"),
"Quarter",QUARTER([Date]),
"QuarterName","Q"&FORMAT([Date],"Q"),
"YearQuarterNumber",INT(FORMAT([Date],"YYYYQ")),
"YearQuarterName",FORMAT([Date],"YYYY")&" Q"&FORMAT([Date],"Q"),
"QuarterStart",DATE( YEAR([Date]), (QUARTER([Date])*3)-2, 1),
"QuarterEnd",EOMONTH(DATE( YEAR([Date]), QUARTER([Date])*3, 1),0),
"WeekNumber",WEEKNUM([Date]),
"WeekStart", [Date]-WEEKDAY([Date])+1,
"WeekEnd",[Date]+7-WEEKDAY([Date]),
"FiscalYear",if(_fiscalStart=1,YEAR([Date]),YEAR([Date])+ QUOTIENT(MONTH([Date])+ (13-_fiscalStart),13)),
"FiscalQuarter",QUARTER( DATE( YEAR([Date]),MOD( MONTH([Date])+ (13-_fiscalStart) -1 ,12) +1,1) ),
"FiscalMonth",MOD( MONTH([Date])+ (13-_fiscalStart) -1 ,12) +1
)
```

![image](https://github.com/Cristianfllc3/Power_BI_DAX_101/assets/72107370/fa14c3bf-1b5f-4596-a3fa-1b567e6e8336)

