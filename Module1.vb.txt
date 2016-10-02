
Imports System
Imports System.Net
Imports System.IO

Module Module1

    Sub Main()

        Dim linea As String
        Dim inicio As Long
        Dim fin As Long
        Dim total As Long
        Dim NumUrl As Long

        NumUrl = 0
        total = 0
        
        FileOpen(1, "URLs.txt", OpenMode.Input, OpenAccess.Read, OpenShare.Shared)
        linea = LineInput(1)
        Do
            linea = LineInput(1)
            Console.WriteLine(linea)
            inicio = Environment.TickCount()
            NumUrl = NumUrl + 1
            geturl(linea)
            fin = Environment.TickCount()
            Console.WriteLine(fin - inicio)

            total = total + (fin - inicio)
        Loop Until EOF(1)
        FileClose(1)
        Console.Write("Total en milisegundos:")
        Console.WriteLine(total)
        Console.Write("Cantidad de Urls Leidas:")
        Console.WriteLine(NumUrl)
    End Sub


    Sub geturl(sURL As String)
        Try
            Dim wrGETURL As WebRequest
            wrGETURL = WebRequest.Create(sURL)

            Dim myProxy As New WebProxy("myproxy", 80)
            myProxy.BypassProxyOnLocal = True

            wrGETURL.Proxy = WebProxy.GetDefaultProxy()

            Dim objStream As Stream
            objStream = wrGETURL.GetResponse.GetResponseStream()



        Catch ex As System.Net.WebException
            Console.WriteLine("down")
            log(sURL & " down")


        Catch ex1 As System.UriFormatException
            Console.WriteLine("error")
            log(sURL & " con error")

        End Try

    End Sub

    Sub log(s As String)
        FileOpen(2, "log.txt", OpenMode.Append, OpenAccess.Write, OpenShare.Shared)
        PrintLine(2, s)
        FileClose(2)

    End Sub

End Module
