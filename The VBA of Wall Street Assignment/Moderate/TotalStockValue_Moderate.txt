'Total Stock Volume
Sub TotalStockVolume():

 'Loop Through All Worksheets
 For Each ws In Worksheets

 'Set our Initial Variables
 Dim i, j As Long
 Dim totalv As Double
 Dim ticker As String
 Dim lastrow As Long
 Dim ychng As Double
 Dim pchng As Double
 Dim oprice As Double
 Dim cprice As Double
 Dim oprice_row As Long

 'Add Header Name to Display Data
 ws.Range("I1").Value = "Ticker"
 ws.Range("J1").Value = "Yearly Change"
 ws.Range("K1").Value = "Percent Change"
 ws.Range("L1").Value = "Total Stock Value"

 'Set Initial Total
 totalv = 0
 j = 2
 oprice_row = 2

 'Determine the last Row
 lastrow = ws.Cells(Rows.Count, 1).End(xlUp).Row

 'Loop Through Each Year of Stock Data
 For i = 2 To lastrow
     
     'Compare Each Ticker
     If ws.Range("A" & i + 1).Value = ws.Range("A" & i).Value Then

         'Calculate Total Volume for Each Ticker If Tickers are same
         totalv = totalv + ws.Range("G" & i).Value

     Else
         'Grab Ticker when it change
         ticker = ws.Range("A" & i).Value

         'Calculate Yearly Change and Percent Change
         oprice = ws.Range("C" & oprice_row)
         cprice = ws.Range("F" & i)
         ychng = cprice - oprice

         'Calculate Percent Change
         if oprice = 0 Then
            pchng = 0
         Else
            pchng = ychng / oprice
         End if

         'Insert Grabbed Ticker,Total Volume,Yearly Change and Percent Change into Display Cells
         ws.Range("I" & j).Value = ticker
         ws.Range("L" & j).Value = totalv + ws.Range("G" & i).Value
         ws.Range("J" & j).Value = ychng
         ws.Range("K" & j).Value = pchng
         ws.Range("K" & j).NumberFormat = "0.00%"
         
         'Conditional Formating Yearly Change, Positive Green/ Negative Red
         If ws.Range("J" & j).Value > 0 Then
            ws.Range("J" & j).Interior.ColorIndex = 4
         Else
            ws.Range("J" & j).Interior.ColorIndex = 3
         End If

         'Add a New Row itno Display Cells for Next Ticker, Set New open rice row and Reset Total
         j = j + 1
         totalv = 0
         oprice_row = i + 1
         
     End If
 Next i
 Next ws
End Sub
