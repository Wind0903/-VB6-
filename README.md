# -VB6-
2019新版

Private Declare Function OpenUsbDevice Lib "USBIO.dll" (ByVal a As Integer, ByVal b As Integer) As Boolean
Private Declare Sub OutDataCtrl Lib "USBIO.dll" (ByVal a As Byte, ByVal b As Byte)
Dim a, c As Integer
Dim r(20) As Byte, g(20) As Byte
Private Sub Command1_Click(Index As Integer)
    c = 0
    a = Index + 1
    If a = 3 Then End
End Sub
Private Sub Form_Load()
    r(0) = 1: r(1) = 2: r(2) = 4: r(3) = 8: r(4) = 16: r(5) = 32: r(6) = 64: r(7) = 128
    g(0) = 128: g(1) = 192: g(2) = 224: g(3) = 240: g(4) = 248: g(5) = 252: g(6) = 254: g(7) = 255
End Sub
Private Sub Form_Unload(Cancel As Integer)
    OutDataCtrl 0, 0: OutDataCtrl 0, &H30
End Sub
Private Sub Timer1_Timer()
    Label1.Caption = vbCrLf & "Current Time : " & Time$
    Dim conn As Boolean
    conn = OpenUsbDevice(&H1234, &H6789)
    For i = 0 To 15
        Shape1(i).FillStyle = IIf(conn, 0, 1)
        Shape1(i).FillColor = IIf(i < 8, RGB(128, 0, 0), RGB(0, 128, 0))
    Next i
    If a = 1 Then OutDataCtrl g(c), 0
    If a = 2 Then OutDataCtrl 0, 0: OutDataCtrl r(c), &H30
    For i = 0 To 7
        If a = 1 And (g(c) And 2 ^ i) Then Shape1(i + 8).FillColor = RGB(0, 255, 0)
        If a = 2 And (r(c) And 2 ^ i) Then Shape1(i).FillColor = RGB(255, 0, 0)
    Next i
    c = c + 1: If c > 20 Then c = 20
    
End Sub
