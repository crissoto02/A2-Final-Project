```epson rc
Integer t_position
Integer b_position
Integer height_token
Integer height_block
'Velocity and Acceleration Configuration
Function Main
	Motor On
	Power High
	Speed 30
	Accel 30, 30
	SpeedS 300
	AccelS 5000
	height_token = 6
	height_block = 6
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

'Function Task 1 (Pick and Place)
Function Task1
	For t_position = 0 To 2
		picktoken()
		FixToken()
		placetoken()
	Next t_position
	For b_position = 0 To 2
		pickblock()
		FixBlock()
		placeblock()
	Next b_position
	Go start
Fend

'Pick Token function
Function picktoken
	Go localpicktoken +Z(40)
	Move localpicktoken -Z(t_position * height_token)
	On 8
	Wait 0.5
	Move localpicktoken +Z(70)
Fend

'Fix token function
Function FixToken
	Move localfixtoken +Z(10) +X(1) -Y(1)
	Go localfixtoken +Z(2) +X(1) -Y(1)
	Off 8
	Wait 0.1
	Move localfixtoken +X(3)
	Go localfixtoken +Z(5)
	Go localfixtoken
	On 8
	Wait 0.1
	Go localfixtoken +Z(50)
Fend

'Place Token Function
Function placetoken
	Go localplacetoken +Z(10) -Y(t_position * 30)
	Move localplacetoken +Z(3) -Y(t_position * 30)
	Off 8
	Wait 0.1
Fend

'Pick Block Function
Function pickblock
	Go localpickblock +Z(20)
	Move localpickblock -Z(b_position * height_block)
	On 8
	Wait 0.5
	Move localpickblock +Z(70)
Fend

'Fix block function
Function FixBlock
	Move localfixblock +Z(20) -X(2) +Y(2)
	Go localfixblock +Z(2) -X(1) +Y(1)
	Off 8
	Wait 0.1
	Go localfixblock
	Move localfixblock +X(3) -Y(3)
	Go localfixblock +Z(5)
	Go localfixblock
	On 8
	Wait 0.1
	Go localfixblock +Z(2)
	Go localfixblock +Z(2) -X(2) +Y(2)
	Go localfixblock +Z(50) -X(2) +Y(2)
Fend

'Place Block Function
Function placeblock
	Go localplaceblock +Z(10) -Y(b_position * 30)
	Go localplaceblock +Z(3) -Y(b_position * 30)
	Off 8
	Wait 0.3
	Go localplaceblock +Z(40)
Fend
