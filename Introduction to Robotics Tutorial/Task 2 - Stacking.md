```Epson
Integer height
Integer i
Integer counter
Function Main
	Motor On
	Power High
	Speed 30
	Accel 30, 30
	SpeedS 300
	AccelS 5000
	height = 6
	counter = 9
	Tool 1
	Go start
	Wait Sw(0) = On
	Do While Sw(0) = On
		If Sw(0) = On Then
			Xqt Task2
		EndIf

		Do While Sw(0) = On And Sw(7) = Off
			Do While Sw(4) = On
				Wait Sw(4) = On
				On 12
				Halt Task2
				If Sw(2) = On Then
					Off 8
				EndIf
				If Sw(6) = On Then
					On 8
				EndIf
				If Sw(4) = Off Then
					Resume Task2
					Off 12
				EndIf
			Loop
		Loop

	  	If Sw(7) = On Then
			Quit Task2
			Off 8
	    		Wait 2
	    		Go Safe
	    		Go start
			
	  	EndIf

	  	Wait Sw(7) = Off
		Wait Sw(0) = Off
	Loop
Fend

' Ejecucion del programa principal
Function Task2
  For i = 0 To 19
  	If (i Mod 2) = 0 Then
  		Pick_Block()
  	Else
  		Pick_Token()
  		counter = counter - 1
  	EndIf
  Next i
  Go start
Fend

' Definition of functions for Epson RC+
' Function to grab a block
Function Pick_Block
  Move position_block +Z(80)
  Move position_block +Z(counter * height)
  On 8
  Move position_block +Z(counter * height + 5) +Y(2) +X(2)
  Wait 0.1
  Move position_block +Z(80) +Y(2) +X(2)
  Move place_position +Z(i * height + 20)
  Move place_position +Z(i * height)
  Off 8
  Move place_position +Z(i * height + 20)
Fend

' Function to grab a token
Function Pick_Token
  Move position_token +Z(80)
  Move position_token +Z(counter * height)
  On 8
  Move position_token +Z(counter * height + 5) -Y(2) -X(2)
  Wait 0.1
  Move position_token +Z(80) -Y(2) -X(2)
  Move place_position +Z(i * height + 20)
  Move place_position +Z(i * height)
  Off 8
  Move place_position +Z(i * height + 20)
Fend
