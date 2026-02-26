let
    // Add source column to first table
    Niyara =
        Table.AddColumn(
            #"Niyara-History data",
            "Source_Table",
            each "Niyara",
            type text
        ),

    // Add source column to second table
    Trimaya =
        Table.AddColumn(
            #"Trimaya-History data",
            "Source_Table",
            each "Trimaya",
            type text
        ),

    // Combine both tables
    Source =
        Table.Combine({ Niyara, Trimaya })
in
    Source
    
