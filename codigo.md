# Parte1RR
Parte 1 del Programa Round Robin
//PARTE 1


Public Class frmPlanificacion

    'Esta variable sirve para el random de los colores
    Private m_Rnd As New Random

    Private Ultima_Fila As Integer = 0
    Private Ultima_Columa As Integer = 0

    Private Sub frmPlanificacion_Load(ByVal sender As Object, ByVal e As System.EventArgs) Handles Me.Load

        'Agrega 100 lineas a la lista de planificación
        For i As Integer = 0 To 300
            dgridRoundRobin.Rows.Add()
        Next

    End Sub

    Private Sub cmdAgregar_Click(ByVal sender As System.Object, ByVal e As System.EventArgs) Handles cmdAgregar.Click

        Try

            'Este procedimiento agrega los procesos con el requerimiento del CPU
            If (txtProceso.Text.Trim = "") Then

                MsgBox("Ingrese el nombre del proceso", MsgBoxStyle.Information, Me.Text)

            ElseIf txtRequerimientoCPU.Value = 0 Then

                MsgBox("Ingrese el requerimiento de CPU >0 ", MsgBoxStyle.Information, Me.Text)

            Else

                Dim vColor As Color = RandomColor()
                tblprocesos.Rows.Add(txtProceso.Text, txtRequerimientoCPU.Value, vColor.Name.ToString, txtRequerimientoCPU.Value)

                txtProceso.Text = ""
                txtProceso.Focus()

            End If

        Catch ex As Exception
            MsgBox(ex.Message)
        End Try

    End Sub

    Private Function RandomColor() As Color

        Try

            'Asigna un color random al proceso
            Dim names As KnownColor() = DirectCast([Enum].GetValues(GetType(KnownColor)), KnownColor())
            Dim randomColorName As KnownColor = names(m_Rnd.[Next](names.Length))
            RandomColor = Color.FromKnownColor(randomColorName)

        Catch ex As Exception
            MsgBox(ex.Message)
        End Try


    End Function

    Private Sub txtProceso_KeyDown(ByVal sender As Object, ByVal e As System.Windows.Forms.KeyEventArgs) Handles txtProceso.KeyDown
        If e.KeyCode = Keys.Enter Then
            If txtProceso.Text.Trim <> "" Then
                txtRequerimientoCPU.Focus()
            End If
        End If
    End Sub

    Private Sub cmdProcesar_Click(ByVal sender As System.Object, ByVal e As System.EventArgs) Handles cmdProcesar.Click
        'time.Enabled = True
        Procesar()
    End Sub

    'RECIBE CUANTAS FILAS CONTIENE LA TABLA PROCESOS
    Public Function fiprocesos() As Integer
        Dim filas As Integer = tblprocesos.Rows.Count
        Return filas
    End Function
    'METOD QUE SE USA PARA ESCRIBIR EN LA TABLA DE PLANIFICACION
    Public Sub escribir_planificacion(ByVal dato As String, ByVal x As Integer, ByVal y As Integer, ByVal tabla As DataGridView)
        tabla(y, x).Value = dato
    End Sub
