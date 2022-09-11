# Mail Merge Operations in Microsoft Word

* Save each page as a separate pdf
  + https://www.extendoffice.com/documents/word/5420-word-save-each-page-as-separate-pdf.html
  
``` vbnet
Sub SaveAsSeparatePDFs()
'Updated by Extendoffice 20180906
    Dim I As Long
    Dim xStr As String
    Dim xPathStr As Variant
    Dim xDictoryStr As String
    Dim xFileDlg As FileDialog
    Dim xStartPage, xEndPage As Long
    Dim xStartPageStr, xEndPageStr As String
    Set xFileDlg = Application.FileDialog(msoFileDialogFolderPicker)
    If xFileDlg.Show <> -1 Then
        MsgBox "Please chose a valid directory", vbInformation, "Kutools for Word"
        Exit Sub
    End If
    xPathStr = xFileDlg.SelectedItems(1)
    xStartPageStr = InputBox("Begin saving PDFs starting with page __? " & vbNewLine & "(ex: 1)", "Kutools for Word")
    xEndPageStr = InputBox("Save PDFs until page __?" & vbNewLine & "(ex: 7)", "Kutools for Word")
    If Not (IsNumeric(xStartPageStr) And IsNumeric(xEndPageStr)) Then
        MsgBox "The enterng start page and end page should be number format", vbInformation, "Kutools for Word"
        Exit Sub
    End If
    xStartPage = CInt(xStartPageStr)
    xEndPage = CInt(xEndPageStr)
    If xStartPage > xEndPage Then
        MsgBox "The start page number can't be larger than end page", vbInformation, "Kutools for Word"
        Exit Sub
    End If
    If xEndPage > ActiveDocument.BuiltInDocumentProperties(wdPropertyPages) Then
        xEndPage = ActiveDocument.BuiltInDocumentProperties(wdPropertyPages)
    End If
    For I = xStartPage To xEndPage
        ActiveDocument.ExportAsFixedFormat xPathStr & "\Page_" & I & ".pdf", _
        wdExportFormatPDF, False, wdExportOptimizeForPrint, wdExportFromTo, I, I, wdExportDocumentWithMarkup, _
        False, False, wdExportCreateHeadingBookmarks, True, False, False
    Next
End Sub
```
