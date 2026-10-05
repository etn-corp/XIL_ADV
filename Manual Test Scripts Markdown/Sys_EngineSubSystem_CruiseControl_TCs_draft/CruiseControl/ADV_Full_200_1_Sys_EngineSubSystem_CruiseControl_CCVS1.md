| Test case name | Description |
|---|---|
| ADV_Full_200_1_Sys_EngineSubSystem_CruiseControl_CCVS1 | The system shall activate the Cruise Control Activation State when the CCVS1 message CruiseCtrlActive signal indicates SWITCHED ON.<br><br>While Cruise Control Activation State is active, the system shall set the Shift Point Factor to use the engine torque demand instead of the accelerator pedal map for gear shift point determination.<br><br>The system shall deactivate the Cruise Control Activation State when the CCVS1 message CruiseCtrlActive signal indicates SWITCHED OFF. |

| Step | Action | Expected result |
|---:|---|---|
| 1 | Collect the following<br><br>XCP with 10 ms cyclic:<br><br>OprFootOnBrakePedal<br>OprFootOnAccelPedal<br>OprPRNDLSelect<br>OprPRNDL<br>AMTCurrentGear<br>AMTSelectedGear<br>AMTShiftState<br>EngineSpeed<br>VehSpeed<br>OprCruiseControlStatus<br>HWin_OprCruiseControlStatus<br>rtDWork.HWin_OpsCCVS_CruiseControlActive<br><br>J1939 TRAFFIC<br><br>SET TE.Battery = 24000 (mV)<br>SET TE.Ignition = 24000 (mV)<br>SET VEH.Engine = ON<br><br>#Summary:<br>1. Set battery power and ignition to 24V, then start engine | . |
| 2 | Start recording | . |
| 3 | SET VEH.ParkingBrake = OFF<br>SET VEH.ServiceBrake = ON<br>SET VEH.PRNDL = Drive (1)<br>VERIFY EXPECTED RESULTS<br><br>#Summary<br>1. Depress brake pedal. Release parking brake.<br>2. Select Drive mode. Confirm Drive mode is selected and start gear is engaged. | . |
| 4 | SET VEH.ServiceBrake = OFF<br>SET VEH.AccelPedal = ON<br>WAIT VEH.VehSpeed >= 14<br>SET VEH.CruiseControl = ON<br>SET VEH.AccelPedal = OFF<br>WAIT 1 minute<br><br>#Summary<br>1. Release brake pedal.<br>2. Apply accelerator pedal.<br>3. Reach speed greater than or equal to 50 km/h (Standard Cruise Control: Needs at least 25 mph to 30 mph (40 km/h to 50 km/h) to turn on. Please adjust speed accordingly.)<br>4. Activate cruise control and release accelerator pedal. Continue driving. | . |
| 5 | SET VEH.CruiseControl = OFF<br>SET VEH.ServiceBrake = ON<br><br>#Summary<br>1. Disable cruise control.<br>2. Bring vehicle to a stop. | . |
| 6 | Verify following requirements. | VERIFY ECU.HWin_OprCruiseControlStatus = 1 WHEN:<br>- CAN1.CCVS_SA_00.CruiseCtrlActive = 1 (CruiseCtrlSwitchedOn) |
| 7 | Verify following requirements. | VERIFY ECU.HWin_OprCruiseControlStatus = 0 WHEN:<br>- CAN1.CCVS_SA_00.CruiseCtrlActive = 0 (CruiseCtrlSwitchedOff) |
| 8 | Stop recording and save data file | . |
