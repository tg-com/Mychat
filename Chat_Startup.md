---
title: "Startup_Form"
author: "Micheal"
date: "Friday, May 22, 2015"
output: html_document
---

## Chat Start-up Code Form

Public Class frmstartup

###    'Display Startup Name in IPChat Username Field
    Public Shared username As TextBox

    Private Sub frmstartup_Load(ByVal sender As System.Object, ByVal e As System.EventArgs) Handles MyBase.Load

        txtname.Focus()
        username = txtname

    End Sub

    Private Sub btnok_Click(ByVal sender As System.Object, ByVal e As System.EventArgs) Handles btnok.Click
###        'validating username field
        If txtname.Text = "" Then
            MsgBox("Please input your Name")
            txtname.Focus()

        Else
###            'display username in the Chat Application; username field
            frmIPChat.txtusername.Text = username.Text

###            'closing the startup screen and loading the chat application
            Me.Close()
            frmIPChat.Opacity = 100%
        End If
    End Sub

    Private Sub btnexit_Click(ByVal sender As System.Object, ByVal e As System.EventArgs) Handles btnexit.Click
###        'exit the entire application
        End

    End Sub

End Class