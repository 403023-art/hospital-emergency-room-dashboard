### Power Query

###### hospital emergency  dashboard :

&#x20;Source = Csv.Document(File.Contents("C:\\Users\\AMAN YADAV\\Downloads\\Hospital Emergency Room Data.csv"),\[Delimiter=",", Columns=12, Encoding=65001, QuoteStyle=QuoteStyle.None]),

&#x20;   #"Promoted Headers" = Table.PromoteHeaders(Source, \[PromoteAllScalars=true]),

&#x20;   #"Changed Type" = Table.TransformColumnTypes(#"Promoted Headers",{{"Patient Id", type text}, {"Patient Admission Date", type datetime}, {"Patient First Inital", type text}, {"Patient Last Name", type text}, {"Patient Gender", type text}, {"Patient Age", Int64.Type}, {"Patient Race", type text}, {"Department Referral", type text}, {"Patient Admission Flag", type logical}, {"Patient Satisfaction Score", Int64.Type}, {"Patient Waittime", Int64.Type}, {"Patient Admission Flag\_1", type logical}}),

&#x20;   #"Merged Columns" = Table.CombineColumns(#"Changed Type",{"Patient First Inital", "Patient Last Name"},Combiner.CombineTextByDelimiter(". ", QuoteStyle.None),"Merged"),

&#x20;   #"Replaced Value" = Table.ReplaceValue(#"Merged Columns","M","Male",Replacer.ReplaceValue,{"Patient Gender"}),

&#x20;   #"Replaced Value1" = Table.ReplaceValue(#"Replaced Value","F","Female",Replacer.ReplaceValue,{"Patient Gender"}),

&#x20;   #"Changed Type1" = Table.TransformColumnTypes(#"Replaced Value1",{{"Patient Admission Flag", type text}}),

&#x20;   #"Replaced Value2" = Table.ReplaceValue(#"Changed Type1","true","admitted",Replacer.ReplaceText,{"Patient Admission Flag"}),

&#x20;   #"Replaced Value3" = Table.ReplaceValue(#"Replaced Value2","false","not admitted",Replacer.ReplaceText,{"Patient Admission Flag"}),

&#x20;   #"Removed Columns" = Table.RemoveColumns(#"Replaced Value3",{"Patient Admission Flag\_1"}),

&#x20;   #"Split Column by Delimiter" = Table.SplitColumn(Table.TransformColumnTypes(#"Removed Columns", {{"Patient Admission Date", type text}}, "en-IN"), "Patient Admission Date", Splitter.SplitTextByDelimiter(" ", QuoteStyle.Csv), {"Patient Admission Date.1", "Patient Admission Date.2"}),

&#x20;   #"Changed Type2" = Table.TransformColumnTypes(#"Split Column by Delimiter",{{"Patient Admission Date.1", type date}, {"Patient Admission Date.2", type time}}),

&#x20;   #"Renamed Columns" = Table.RenameColumns(#"Changed Type2",{{"Patient Admission Date.1", "Patient Admission Date"}, {"Patient Admission Date.2", "Patient Admission Time"}}),

&#x20;   #"Sorted Rows" = Table.Sort(#"Renamed Columns",{{"Patient Admission Date", Order.Ascending}})

in

&#x20;   #"Sorted Rows"

##### &#x20;    



###### calender\_table: 

&#x20;Source = List.Dates(#date(2023,01,01),731,#duration(1,0,0,0)),

&#x20;   #"Converted to Table" = Table.FromList(Source, Splitter.SplitByNothing(), null, null, ExtraValues.Error),

&#x20;   #"Changed Type" = Table.TransformColumnTypes(#"Converted to Table",{{"Column1", type date}}),

&#x20;   #"Renamed Columns" = Table.RenameColumns(#"Changed Type",{{"Column1", "Date"}})

in

&#x20;   #"Renamed Columns"

