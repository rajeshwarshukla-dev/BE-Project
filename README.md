Phase = 
VAR SubCat = 'Sheet1'[Sub-Category]
RETURN
SWITCH (
    TRUE(),

    /* RCC – Sub structure */
    SubCat IN {
        "Lift Pit RCC Works",
        "Raft RCC Works",
        "Retaining Wall RCC Works",
        "Foundation RCC Works",
        "Staircase & Ramp RCC Works"
    }, "RCC - Sub structure",

    /* RCC – Super structure */
    SubCat IN {
        "Core Wall RCC Works",
        "RCC Works - Vertical",
        "RCC Works - Slab"
    }, "RCC - Super structure",

    /* MEP 1st Fix */
    SubCat IN {
        "FPS 1st Fix - MEP & Finishes",
        "Electrical 1st Fixes",
        "HVAC Ducting",
        "Plumbing"
    }, "MEP 1st Fix",

    /* Facade */
    SubCat = "Façade Works", "Facade",

    /* Finishes (default bucket) */
    SubCat IN {
        "Gypsum Plastering Works",
        "Blockwork",
        "Painting Works",
        "Ceiling Painting/ PT Marking",
        "Flooring & Dado",
        "False Ceiling",
        "Door & Window Frame work",
        "Lobby Plastering",
        "Final Painting",
        "Staircase Flooring",
        "Door & Window Fixing",
        "Electrical",
        "FPS 2nd Fix - MEP & Finishes",
        "Electrical 2nd Fix - MEP & Finishes",
        "FPS final fixes",
        "HVAC Grill Fixing"
    }, "Finishes",

    /* Safety fallback */
    "Unmapped"
)
_________________________________________________________________________________________
Phase_Order = 
SWITCH (
    'Sheet1'[Phase],
    "RCC - Sub structure", 1,
    "RCC - Super structure", 2,
    "MEP 1st Fix", 3,
    "Finishes", 4,
    "Facade", 5
)
_________________________________________________________________________________________
Phase_Order = 
DATATABLE (
    "Phase", STRING,
    "Phase_Order", INTEGER,
    {
        {"RCC - Sub structure", 1},
        {"RCC - Super structure", 2},
        {"MEP 1st Fix", 3},
        {"Finishes", 4},
        {"Facade", 5}
    }
)

_______________________________________
Not Committed
Not Ready
Forced Ready
Stopped
Warning
Started
Milestone Started
Quality checked
Complete


------------------------------
Final Category (Measure) =
VAR _Status =
    SELECTEDVALUE ( 'TableName'[Status] )
RETURN
SWITCH (
    TRUE(),

    _Status IN {
        "Not Committed",
        "Not Ready",
        "Forced Ready",
        "Stopped"
    }, "Not Ready / Risk",

    _Status IN {
        "Warning",
        "Started",
        "Milestone Started"
    }, "In Progress / Attention",

    _Status IN {
        "Quality checked",
        "Complete"
    }, "Completed / Done",

    BLANK()
)
--------------------------------------------
Final Status Color =
VAR _Status =
    SELECTEDVALUE ( 'TableName'[Status] )
RETURN
SWITCH (
    TRUE(),

    _Status IN {
        "Not Committed",
        "Not Ready",
        "Forced Ready",
        "Stopped"
    }, "#D93025",        -- Red

    _Status IN {
        "Warning",
        "Started",
        "Milestone Started"
    }, "#FBBC04",        -- Amber

    _Status IN {
        "Quality checked",
        "Complete"
    }, "#34A853",        -- Green

    "#D3D3D3"            -- Default / fallback
)
