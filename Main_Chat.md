---
title: "Main_Chat_Form"
author: "Micheal"
date: "Friday, May 22, 2015"
output: html_document
---

## Main Chat Code Form

### ' defining vb methods

Imports System.Net.Sockets
Imports System.Threading
Imports System.IO
Imports System.Net
Imports System.Text
Imports System.Net.Dns


Public Class frmIPChat

###    ' variable declarations

    Dim listener As New TcpListener(55555)
    Dim client As TcpClient
    Dim MySockect As Socket
    Dim message As String = ""
    Dim serveriplist As IPHostEntry = System.Net.Dns.GetHostEntry(System.Net.Dns.GetHostName())
    Dim machineip As String
   
    Dim nameip As String
    Dim y As String = System.Net.Dns.GetHostName


    Private Sub frmIPChat_FormClosing(ByVal sender As Object, ByVal e As System.Windows.Forms.FormClosingEventArgs) Handles Me.FormClosing
###        ' terminating the chat application network listener
        listener.Stop()

    End Sub

    Private Sub frmIPChat_Load(ByVal sender As System.Object, ByVal e As System.EventArgs) Handles MyBase.Load

        On Error Resume Next
###        ' display the PC's IPV4 address (xp, vista, win7...)

        machineip = serveriplist.AddressList(0).ToString()
        machineip = serveriplist.AddressList(1).ToString()
        machineip = serveriplist.AddressList(2).ToString()
        machineip = serveriplist.AddressList(3).ToString()

        frmstartup.Show()

###        ' Display the system ip address
        txtuserip.Text = machineip

        txthostname.Text = y

    End Sub

    Private Sub listening()
###        ' Listening port subroutine
        listener.Start()

    End Sub

    Private Sub btnsend_Click(ByVal sender As System.Object, ByVal e As System.EventArgs) Handles btnsend.Click

        Try

###            'Connect to the Client's PC with its IP Address and Port details

            client = New TcpClient(txtip.Text, 55555)
            Dim writer As New StreamWriter(client.GetStream())

###            'Save the message the message unto the client's listening socket buffer

            writer.Write(txtusername.Text & "[" & txtuserip.Text & "]" & ">>" & " " & txtsend1.Text)
            writer.Flush()

            RichTextBox1.Text += (txtusername.Text & "[" & txtuserip.Text & "]" & ">>" & " " & txtsend1.Text) + vbCrLf

###            'clears the inputbox after sending the last message
            txtsend1.Clear()
            txtsend1.Focus()

        Catch exc As Exception
            MsgBox(exc.Message)
        End Try

    End Sub


    Private Sub Timer1_Tick(ByVal sender As System.Object, ByVal e As System.EventArgs) Handles Timer1.Tick
###        'set the listening port to continue listening for connections
        If listener.Pending = True Then
            message = ""
            client = listener.AcceptTcpClient()

            Dim reader As New StreamReader(client.GetStream())
            While reader.Peek > -1
                message = message + Convert.ToChar(reader.Read()).ToString
            End While

###            'display the text stored in the Listening Port's buffer
            RichTextBox1.Text = RichTextBox1.Text + Environment.NewLine + " >> " + " " + message + vbCrLf


###            'set the cursor and scroolbar to the end of the text
            RichTextBox1.SelectionStart = Len(RichTextBox1.Text)
            RichTextBox1.ScrollToCaret()

        Else : End If
    End Sub

    Private Sub Connecterror()
###        ' Error detection and handling subroutine

        On Error GoTo 50

50:     MsgBox("Pls check the client IP address is valid")
        txtip.Clear()
        txtip.Focus()

    End Sub


    Private Sub btnclear_Click(ByVal sender As System.Object, ByVal e As System.EventArgs) Handles btnclear.Click
        txtsend1.Clear()
        txtsend1.Focus()

    End Sub


    Private Sub AboutIPChatAppToolStripMenuItem1_Click(ByVal sender As System.Object, ByVal e As System.EventArgs) Handles AboutIPChatAppToolStripMenuItem1.Click
        AboutForm.Show()
        Me.Enabled = False

    End Sub


    Private Sub btnConnect_Click(ByVal sender As System.Object, ByVal e As System.EventArgs) Handles btnConnect.Click
        'Error detection and handling
###       'validating recipient ip address field
        Try

            If txtip.Text = "" Then
                MsgBox("Please enter the Client IP Address")
                txtip.Focus()
                txtsend1.Enabled = False

            Else

###                'set the Listening port to start listening for connection
                Dim listthread As New Thread(New ThreadStart(AddressOf listening))
                listthread.Start()

                Timer1.Enabled = True

                client = New TcpClient(txtip.Text, 55555)
                Dim writer As New StreamWriter(client.GetStream())


                writer.Write(txthostname.Text & "-PC" & "[" & txtuserip.Text & "]" & " is connected @ " & TimeOfDay)
                writer.Flush()

                txtsend1.Enabled = True

                btnConnect.Enabled = False

            End If
        Catch exc As Exception
            MsgBox(exc.Message)
        End Try
    End Sub

    Private Sub SaveToolStripMenuItem_Click(ByVal sender As System.Object, ByVal e As System.EventArgs) Handles SaveToolStripMenuItem.Click
###        'save a chat discussion on the chat box

        Try

            SaveFD.InitialDirectory = "C:\"
            SaveFD.Title = "Save Chat to File"
            SaveFD.Filter = "RTF Files|*.rtf|Text Files|*.txt|Word Files|*.doc|Wordx Files|*.docx"

            SaveFD.ShowDialog()
            SaveFD.OverwritePrompt = True

###            'SaveFileDialog1.ShowDialog()
            System.IO.File.WriteAllText(SaveFD.FileName, RichTextBox1.Text & vbCrLf & "Chat saved on: " & Date.Now)

        Catch exc As Exception
            MsgBox(exc.Message)
        End Try
    End Sub

    Private Sub RichTextBox1_TextChanged(ByVal sender As System.Object, ByVal e As System.EventArgs) Handles RichTextBox1.TextChanged
###        ' display time logs of the most recent chat message received

        lbltimer.Text = "Chat Timer: " & TimeOfDay

###        'set the cursor and scroolbar to the end of the text
        RichTextBox1.SelectionStart = Len(RichTextBox1.Text)
        RichTextBox1.ScrollToCaret()

    End Sub

    Private Sub ExitToolStripMenuItem_Click(ByVal sender As System.Object, ByVal e As System.EventArgs) Handles ExitToolStripMenuItem.Click
        End
    End Sub

    Private Sub txtip_TextChanged(ByVal sender As System.Object, ByVal e As System.EventArgs) Handles txtip.TextChanged
        btnConnect.Enabled = True
    End Sub

    Private Sub txtsend1_KeyPress1(ByVal sender As Object, ByVal e As System.Windows.Forms.KeyPressEventArgs) Handles txtsend1.KeyPress
###        ' enabling the Enter key to auto-send chat message
        If e.KeyChar = ChrW(Keys.Enter) Then
            Try

                btnsend.Focus()

            Catch exc As Exception
                MsgBox(exc.Message)
            End Try

        End If

    End Sub

    Private Sub txtsend1_TextChanged_1(ByVal sender As System.Object, ByVal e As System.EventArgs) Handles txtsend1.TextChanged
###        ' sending a msg using the send button

        btnsend.Enabled = True
        If txtsend1.Text = "" Then
            btnsend.Enabled = False
        Else
            btnsend.Enabled = True
        End If

    End Sub

    Private Sub HelpTopicsToolStripMenuItem_Click(ByVal sender As System.Object, ByVal e As System.EventArgs) Handles mnuOptionsLogout.Click
###        ' logging out of a chat discussion

        Timer1.Enabled = False

        MsgBox(txthostname.Text & "-PC" & "[" & txtuserip.Text & "]" & " has logged out : " & TimeOfDay)
        RichTextBox1.Text += (txtusername.Text & "[" & txtuserip.Text & "]" & ">>" & " " & "has logged out: " & TimeOfDay) + vbCrLf

        txtsend1.Enabled = False
        mnuOptionsLogin.Enabled = True
        mnuOptionsLogout.Enabled = False
    End Sub

    Private Sub ToolStripMenuItem2_Click(ByVal sender As System.Object, ByVal e As System.EventArgs) Handles ToolStripMenuItem2.Click
###        ' opening a saved chat discussion
        SavedChatForm.Show()
    End Sub

    Private Sub LoginToolStripMenuItem_Click(ByVal sender As System.Object, ByVal e As System.EventArgs) Handles mnuOptionsLogin.Click
###        're-logging into the chat session

        frmstartup.Show()
        Me.Opacity = 0
        btnConnect.Enabled = True
        mnuOptionsLogout.Enabled = True

    End Sub

    Private Sub HelpTopicsToolStripMenuItem1_Click(ByVal sender As System.Object, ByVal e As System.EventArgs) Handles HelpTopicsToolStripMenuItem1.Click
        frmHelpTopics.Show()

    End Sub
End Class
