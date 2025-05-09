---
external help file: PSScriptTools-help.xml
Module Name: PSScriptTools
online version: https://jdhitsolutions.com/yourls/864c12
schema: 2.0.0
---

# Get-MyTimeInfo

## SYNOPSIS

Display time settings for a collection of locations.

## SYNTAX

```yaml
Get-MyTimeInfo [[-Locations] <OrderedDictionary>] [-HomeTimeZone <String>]
[-DateTime <DateTime>] [-AsTable] [-AsList] [<CommonParameters>]
```

## DESCRIPTION

This command is designed to present a console-based version of a world clock. You provide a hashtable of locations and their respective time zones and the command will write a custom object to the pipeline. Be aware that TimeZone names may vary depending on the .NET Framework version. You may need to enumerate using a command like [System.TimeZoneInfo]::GetSystemTimeZones().ID or the Get-TZList command.

A Note on Formatting:

Normally, a PowerShell command should write an object to the pipeline and then you could use Format-Table or Format-List as you wanted. Those commands will continue to work. However, given the way this command writes to the pipeline, that is with dynamically generated properties, it is difficult to create the usual format ps1xml file. To provide some nicer formatting this command has optional parameters to help your format the output. Note that even though it may look like a table, the output object will be a string.

This command was added in v2.3.0.

## EXAMPLES

### Example 1

```powershell
PS C:\> Get-MyTimeInfo

Now               : 3/26/2025 11:27:14 AM
Home              : 3/26/2025 11:27:14 AM
UTC               : 3/26/2025 3:27:14 PM
Singapore         : 3/26/2025 11:27:14 PM
Seattle           : 3/26/2025 8:27:14 AM
Stockholm         : 3/26/2025 4:27:14 PM
IsDaylightSavings : True
```

The default output is a custom object with each timezone as a property.

### Example 2

```powershell
PS C:\> Get-MyTimeInfo -AsList

   Now: 03/26/2025 11:28:29
   UTC: 03/26/2025 15:28:29

Home              : 3/26/2025 11:28:29 AM
Singapore         : 3/26/2025 11:28:29 PM
Seattle           : 3/26/2025 8:28:29 AM
Stockholm         : 3/26/2025 4:28:29 PM
IsDaylightSavings : True
```

Get current time info formatted as a list. You can also try the AsTable parameter.

### Example 3

```powershell
PS C:\> $loc = [ordered]@{"Hong Kong"="China Standard Time";Honolulu="Hawaiian Standard Time";Mumbai = "India Standard Time"}

PS C:\> Get-MyTimeInfo -Locations $loc -ft

   Now: 03/26/2025 11:29:33
   UTC: 03/26/2025 15:29:33

Home                  Hong Kong             Honolulu             Mumbai              DaylightSavings
----                  ---------             --------             ------               --------------
3/26/2025 11:29:33 AM 3/26/2025 11:29:33 PM 3/26/2025 5:29:33 AM 3/26/2025 8:59:33 PM           True
```

Using a custom location hashtable, get time zone information formatted as a table. This example is using the -ft alias for the AsTable parameter. Even though this is formatted as a table the actual output is a string.

### Example 5

```powershell
PS C:\> Get-MyTimeInfo -Locations ([ordered]@{Seattle="Pacific Standard time";"New Zealand" = "New Zealand Standard Time"}) -HomeTimeZone "central standard time" | Select Now,Home,Seattle,'New Zealand'

Now                   Home                  Seattle              New Zealand
---                   ----                  -------              -----------
3/26/2025 11:30:36 AM 3/26/2025 10:30:36 AM 3/26/2025 8:30:36 AM 3/27/2025 4:30:36 AM
```

This is a handy command when traveling and your laptop is using a locally derived time and you want to see the time in other locations. It is recommended that you set a PSDefaultParameter value for the HomeTimeZone parameter in your PowerShell profile.

## PARAMETERS

### -Locations

Use an ordered hashtable of location names and timezones. You can find timezones with the Get-TimeZone cmdlet or through the .NET Framework with an expression like

```powershell
[System.TimeZoneinfo]::GetSystemTimeZones()
```

The hashtable key should be the location or city name and the value should be the time zone ID. Be careful as it appears time zone IDs are case-sensitive.

The default value is:

```powershell
[ordered]@{
   Singapore = "Singapore Standard Time";
   Seattle   = "Pacific Standard Time";
   Stockholm = "Central Europe Standard Time";
}
```

You might want to define a default value in $PSDefaultParameterValues with your own defaults.

It is recommended you limit this hashtable to no more than 5 locations, especially if you want to format the results as a table.

```yaml
Type: OrderedDictionary
Parameter Sets: (All)
Aliases:

Required: False
Position: 1
Default value: see note
Accept pipeline input: False
Accept wildcard characters: False
```

### -HomeTimeZone

Specify the timezone ID of your home location. You might want to set this as a PSDefaultParameterValue

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: Eastern Standard Time
Accept pipeline input: False
Accept wildcard characters: False
```

### -DateTime

Specify the datetime value to use. The default is now.

```yaml
Type: DateTime
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: $(Get-Date)
Accept pipeline input: False
Accept wildcard characters: False
```

### -AsTable

Display the results as a formatted table. This parameter has an alias of ft.

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases: ft

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -AsList

Display the results as a formatted list. This parameter has an alias of fl.

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases: fl

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### CommonParameters

This cmdlet supports the common parameters: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction, and -WarningVariable.
For more information, see about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

## INPUTS

### Datetime

## OUTPUTS

### myTimeInfo

### System.String

## NOTES

Learn more about PowerShell: https://jdhitsolutions.com/yourls/newsletter

## RELATED LINKS

[Get-TimeZone]()
