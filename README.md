Schedule Days:
1. Planned Date Of Completion = 
MAX ( 'Project Data'[plannedEndDate] )

--------------------------------
2. Task Name (Max Planned Date) = 
VAR _MaxPlannedDate =
    [Planned Date Of Completion]
RETURN
CALCULATE (
    MAX ( 'Project Data'[taskName] ),
    FILTER (
        'Project Data',
        'Project Data'[plannedEndDate] = _MaxPlannedDate
    )
)

------------------------------
3. Actual End Date = 
VAR _MaxPlannedDate = [Planned Date Of Completion]
RETURN
CALCULATE (
    MAX ( 'Project Data'[actualEndDate] ),
    'Project Data'[plannedEndDate] = _MaxPlannedDate
)

-----------------------------
4. Completion Status = 
VAR _Planned = [Planned Date Of Completion]
VAR _Actual  = [Actual End Date]
RETURN
SWITCH (
    TRUE (),
    ISBLANK ( _Actual ), "🔴",
    _Actual <= _Planned, "🟢",
    _Actual > _Planned, "🔴"
) //🟡, "🔴Not Completed", "🟢Completed On Time", "🔴Delayed"


