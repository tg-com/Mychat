---
title: "Save_Chat"
author: "Micheal"
date: "Friday, May 22, 2015"
output: html_document
---

## Saved Chat Code

### System Reference
Imports System.IO


Public Class SavedChatForm

    Private Sub Button1_Click(ByVal sender As System.Object, ByVal e As System.EventArgs) Handles Button1.Click
###        ' open another saved chat from the saved chat screen

        Dim filereader As StreamReader

        OpenFD.InitialDirectory = "C:\"
        OpenFD.Title = "Open a Text File"
        OpenFD.Filter = "Text Files|*.txt|Word Files|*.doc|Wordx Files|*.docx"
        If OpenFD.ShowDialog = DialogResult.Cancel Then
            MsgBox("Cancel clicked")
        Else

            filereader = New StreamReader(OpenFD.FileName)

            RichTextBox1.Text = filereader.ReadToEnd()
            TextBox1.Text = OpenFD.FileName

            filereader.Close()
        End If
    End Sub

    Private Sub Form3_Load(ByVal sender As System.Object, ByVal e As System.EventArgs) Handles MyBase.Load
        Dim filereader As StreamReader

        OpenFD.InitialDirectory = "C:\"
        OpenFD.Title = "Open a Text File"
        OpenFD.Filter = "RTF Files|*.rtf|Text Files|*.txt|Word Files|*.doc|Wordx Files|*.docx"
        If OpenFD.ShowDialog = DialogResult.Cancel Then
            MsgBox("Cancel clicked")
        Else

            filereader = New StreamReader(OpenFD.FileName)

            RichTextBox1.Text = filereader.ReadToEnd()
            TextBox1.Text = OpenFD.FileName
            filereader.Close()
        End If

    End Sub

    Private Sub Button2_Click(ByVal sender As System.Object, ByVal e As System.EventArgs) Handles Button2.Click
        Me.Close()
    End Sub
End Class
