# VBA-Challenge
Sub Stocks()

Dim headers() As Variant
Dim MainWs As Worksheet
Dim wb As Workbook

Set wb = ActiveWorkbook

headers() = Array("Ticker ", "Date ", "Open", "High", "Low", "Close", "Volume", " ", "Ticker", "Yearly_Change", "Percent_Change", "Stock_Volume", " ", " ", " ", "Ticker", "Value")

For Each MainWs In wb.Sheets
    With MainWs
    .Rows(1).Value = ""
    For i = LBound(headers()) To UBound(headers())
    .Cells(1, 1 + i).Value = headers(i)
    
    Next i
    .Rows(1).Font.Bold = True
    .Rows(1).VerticalAlignment = xlCenter
    End With
Next MainWs

    For Each MainWs In Worksheets
    
    Dim Ticker_Name As String
    Ticker_Name = " "
    Dim Total_Ticker_Volume As Double
    Total_Ticker_Volume = 0
    Dim Beg_Price As Double
    Beg_Price = 0
    Dim Yearly_Price_Change As Double
    Yearly_Price_Change = 0
    Dim Yearly_Price_Change_Percent As Double
    Yearly_Price_Change_Percent = 0
    Dim MAX_TICKER_NAME As String
    MAX_TICKER_NAME = " "
    Dim MIN_TICKER_NAME As String
    MIN_TICKER_NAME = " "
    Dim MAX_PERCENT As Double
    MAX_PERCENT = 0
    Dim Max_Volume_Ticker_Name As String
    Max_Volume_Ticker_Name = " "
    Dim MAX_VOLUME As Double
    MAX_VOLUME = 0
    
Dim Summary_Table_Row As Long
Summary_Table_Row = 2

Dim Lastrow As Long

Lastrow = MainWs.Cells(Rows.Count, 1).End(xlUp).Row

Beg_Price = MainWs.Cells(2, 3).Value

For i = 2 To Lastrow

    If MainWs.Cells(i + 1, 1).Value <> MainWs.Cells(i, 1).Value Then
    
    Ticker_Name = MainWs.Cells(i, 1).Value
    
    End_Price = MainWs.Cells(i, 6).Value
    Yearly_Price_Change = End_Price - Beg_Price
    
    If Beg_Price <> 0 Then
        Yearly_Price_Change_Percent = (Yearly_Price_Change / Beg_Price) * 100
        
    End If

    Total_Ticker_Volume = Total_Ticker_Volume + MainWs.Cells(i, 7).Value
    
    MainWs.Range("I" & Summary_Table_Row).Value = Ticker_Name
    
    MainWs.Range("J" & Summary_Table_Row).Value = Yearly_Price_Change
    
    If (Yearly_Price_Change > 0) Then
        MainWs.Range("J" & Summary_Table_Row).Interior.ColorIndex = 4
        
    ElseIf (Yearly_Price_Change <= 0) Then
        MainWs.Range("J" & Summary_Table_Row).Interior.ColorIndex = 3
    
    End If
    
        MainWs.Range("K" & Summary_Table_Row).Value = (CStr(Yearly_Price_Change_Percent) & "%")
    
        MainWs.Range("L" & Summary_Table_Row).Value = Total_Ticker_Volume
    
        Summary_Table_Row = Summary_Table_Row + 1
    
        Beg_Price = MainWs.Cells(i + 1, 3).Value
    
    If (Yearly_Price_Change_Percent > MAX_PERCENT) Then
        MAX_PERCENT = Yearly_Price_Change_Percent
        MAX_TICKER_NAME = Ticker_Name
    
    ElseIf (Yearly_Price_Change_Percent < MIN_PERCENT) Then
        MIN_PERCENT = Yearly_Price_Change_Percent
        MIN_TICKER_NAME = Ticker_Name
    
    End If
    
    If (Total_Ticker_Volume > MAX_VOLUME) Then
        MAX_VOLUME = Total_Ticker_Volume
        Max_Volume_Ticker_Name = Ticker_Name
    End If
    
    Yearly_Price_Change_Percent = 0
    Total_Ticker_Volume = 0
    
Else

    Total_Ticker_Volume = Total_Ticker_Volume + MainWs.Cells(i, 7).Value

End If

Next i

    MainWs.Range("Q2").Value = (CStr(MAX_PERCENT) & "%")
    MainWs.Range("Q3").Value = (CStr(MIN_PERCENT) & "%")
    MainWs.Range("P2").Value = MAX_TICKER_NAME
    MainWs.Range("P3").Value = MIN_TICKER_NAME
    MainWs.Range("Q4").Value = MAX_VOLUME
    MainWs.Range("P4").Value = MAX_VOLUME_TICKER
    MainWs.Range("O2").Value = "Greatest % Increase"
    MainWs.Range("O3").Value = "Greatest % Decrease"
    MainWs.Range("O4").Value = "Greatest Total Volume"
    
Next MainWs
End Sub
