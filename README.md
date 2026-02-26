Period Table =
VAR StartDate = DATE(2023, 6, 1)
VAR EndDate   = DATE(2028, 7, 1)
RETURN
ADDCOLUMNS (
    CALENDAR ( StartDate, EndDate ),
    
    /* Keep only first day of each month */
    "PeriodStart", DATE ( YEAR ( [Date] ), MONTH ( [Date] ), 1 ),

    "PeriodEnd", EOMONTH ( [Date], 0 ),

    "PeriodLabel",
        UPPER ( FORMAT ( [Date], "MMM" ) )
            & "'"
            & RIGHT ( FORMAT ( [Date], "YY" ), 2 ),

    "SortOrder",
        YEAR ( [Date] ) * 100 + MONTH ( [Date] )
)


------------------------------

Period Table =
VAR StartDate = DATE(2023, 6, 1)
VAR EndDate   = DATE(2028, 7, 1)
RETURN
ADDCOLUMNS (
    SUMMARIZE (
        CALENDAR ( StartDate, EndDate ),
        YEAR ( [Date] ),
        MONTH ( [Date] )
    ),

    "PeriodStart", DATE ( YEAR ( [Date] ), MONTH ( [Date] ), 1 ),

    "PeriodEnd", EOMONTH ( DATE ( YEAR ( [Date] ), MONTH ( [Date] ), 1 ), 0 ),

    "PeriodLabel",
        UPPER ( FORMAT ( DATE ( YEAR ( [Date] ), MONTH ( [Date] ), 1 ), "MMM" ) )
            & "'"
            & RIGHT ( FORMAT ( DATE ( YEAR ( [Date] ), MONTH ( [Date] ), 1 ), "YY" ), 2 ),

    "SortOrder",
        YEAR ( [Date] ) * 100 + MONTH ( [Date] )
)
