| Test case name | Description |
|---|---|
| ADV_Full_200_1_Sys_ClutchSubSystem_Lock_PTO | The System shall activate Cutch Locked while the following conditions are true:<br>- Engine OFF;<br>- Vehicle is stationary;<br>- Output shaft speed is less than C_PtmMotorOffSpeedThd.<br><br>The System shall activate Clutch Locking while the following conditions are true:<br>- Vehicle is stationary;<br>- Transmission is in Neutral gear;<br>- PTO is engaged;<br>- Clutch Locked is deactivated. |

| Step | Action | Expected result |
|---:|---|---|
| 1 | Collect the following<br><br>XCP with 10 ms cyclic:<br><br>OprFootOnBrakePedal<br>OprFootOnAccelPedal<br>OprPRNDLSelect<br>OprPRNDL<br>AMTCurrentGear<br>AMTSelectedGear<br>AMTShiftState<br>EngineSpeed<br>VehSpeed<br><br>PtmCluBelowLockThd<br>PtmCluAboveLockThd<br>PtmCluShiftLaunch<br>PtmCluVehicleLaunch<br>TrnInputShaftControlMode<br>AMTNeutralConfirmed<br>rtDWork.engine_torque_cmd<br>PtmCluEngineNeeded<br>PtmCluLocking<br>PtmCluLocked<br>PtmCluUnlock<br>CluLocked<br>PtmClutchMode<br>PtmCluRRate<br>CluPosition<br>CluPositionCmd<br>CluPositionCmdActive<br>CluEngageFraction<br>CluEngageFractionCmd<br>AMTInputShaftSpeed<br>AMTOutputShaftSpeed<br>CluInputShaftSpeed<br>CluOutputShaftSpeed<br>PtmEnergyCoastCmd<br>EngMode<br><br>SET TE.Battery = 24000 (mV)<br>SET TE.Ignition = 24000 (mV)<br>SET VEH.Engine = OFF<br><br>#Summary:<br>1. Set battery power and ignition to 24V, do not start the engine. | . |
| 2 | Start recording | . |
| 3 | SET VEH.Engine = OFF<br><br>#Summary:<br>1. Set battery power and ignition to 24V, do not start engine | VERIFY ECU.PtmCluLocked = 1 WHEN all below conditions are met:<br>- ECU.EngMode = -1 (ENGINE_OFF)<br>- ECU.VehSpeed = 0<br>- ECU.AMTOutputShaftSpeed < C_PtmMotorOffSpeedThd |
| 4 | SET VEH.Engine = ON<br><br>#Summary:<br>1. Start the engine | VERIFY ECU.EngMode = 3 (ENGINE_RUNNING) |
| 5 | SET VEH.ServiceBrake = ON<br>SET VEH.CSPTO = ON<br><br>#Summary<br>1. Press brake pedal, this will cause clutch to open<br>2. Engage PTO<br>3. Engaging PTO will close the clutch | VERIFY ECU.PtmCluLocking = 1 WHEN all below conditions are met:<br>- ECU.VehSpeed = 0<br>- ECU.AMTCurrentGear = 0<br>- ECU.VehPTODevActive = 1<br>- ECU.PtmCluLocked = 0 |
| 6 | SET VEH.ServiceBrake = ON<br>SET VEH.CSPTO = OFF<br><br># Summary:<br>1. Disable PTO | . |
| 7 | Stop recording and save data file | . |
