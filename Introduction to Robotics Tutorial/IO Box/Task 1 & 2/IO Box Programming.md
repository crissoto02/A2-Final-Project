```Epson RC+7.0
Function Main
	Motor On
	Power High
	Speed 30
	Accel 30, 30
	SpeedS 300
	AccelS 5000
	Tool 1
	Go start
	Wait Sw(0) = On
	Do While Sw(0) = On
		If Sw(0) = On Then
			Xqt Task1
		EndIf

		Do While Sw(0) = On And Sw(7) = Off
			Do While Sw(4) = On
				Wait Sw(4) = On
				On 12
				Halt Task1
				If Sw(2) = On Then
					Off 8
				EndIf
				If Sw(6) = On Then
					On 8
				EndIf
				If Sw(4) = Off Then
					Resume Task1
					Off 12
				EndIf
			Loop
		Loop

	  	If Sw(7) = On Then
			Quit Task1
			Off 8
	    		Wait 2
	    		Go Safe
	    		Go start
			
	  	EndIf

	  	Wait Sw(7) = Off
		Wait Sw(0) = Off
	Loop
Fend