# FRC2016

package org.usfirst.frc.team4598.robot;

import edu.wpi.first.wpilibj.CameraServer;
import edu.wpi.first.wpilibj.PIDOutput;
import edu.wpi.first.wpilibj.PIDOutput;
import edu.wpi.first.wpilibj.Compressor;
import edu.wpi.first.wpilibj.CounterBase.EncodingType;
import edu.wpi.first.wpilibj.DigitalInput;
import edu.wpi.first.wpilibj.Encoder;
import edu.wpi.first.wpilibj.IterativeRobot;
import edu.wpi.first.wpilibj.Joystick;
import edu.wpi.first.wpilibj.PIDController;
import edu.wpi.first.wpilibj.PIDOutput;
import edu.wpi.first.wpilibj.PIDSource;
import edu.wpi.first.wpilibj.PIDSourceType;
import edu.wpi.first.wpilibj.Solenoid;
import edu.wpi.first.wpilibj.Victor;
import edu.wpi.first.wpilibj.livewindow.LiveWindow;
import edu.wpi.first.wpilibj.smartdashboard.SmartDashboard;


/**
 * The VM is configured to automatically run this class, and to call the
 * functions corresponding to each mode, as described in the IterativeRobot
 * documentation. If you change the name of this class or the package after
 * creating this project, you must also update the manifest file in the resource
 * directory.
 */

public class Robot extends IterativeRobot 	
{									


	
	
	DigitalInput limitSwitchOut = new DigitalInput(3);				
	DigitalInput limitSwitchIn = new DigitalInput(4);
	Victor leftDrive0 = new Victor(0);			//Left Motor  0
	Victor leftDrive1 = new Victor(1);			//Left Motor  1
	Victor rightDrive0 = new Victor(2);			//Right Motor 0
	Victor rightDrive1 = new Victor(3);			//Right Motor 1
	Victor launchWheel0 = new Victor(4);		//Shooter Left 
	Victor launchWheel1 = new Victor(5);		//Shooter Right
					//Shooter Arm  
	Victor pnuarm = new Victor(7);				//Grabber Arm  
												
	CameraServer server;						//Camera


	
	Joystick controller = new Joystick(0);		//Controller
	

	
	Solenoid Punch = new Solenoid(1,3);			//Solenoids	
	
						
	
	PIDController armPID;
	Encoder encoder;
	Victor arm;

	

																	
	Compressor Compressy = new Compressor(1);						//Compressor
																	
	int autoLoopCounter;
	
	
	public Robot(double KpConstant1, double KiConstant2, double KdConstant3, double KfConstant4, PIDSource source, PIDOutput output)
	{
		
		
		armPID = new PIDController(KpConstant1,KiConstant2,KdConstant3,KfConstant4,source, output);
    	encoder = new Encoder(1,2,true,EncodingType.k4X);
    	arm = new Victor(6);	
    	
    	KpConstant1 = .1;
    	KiConstant2 = .1;
    	KdConstant3 = .1;
    	


	}
	
	
    /**
     * This function is run when the robot is first started up and should be
     * used for any initialization code.\
     */

	
	
    public void robotInit() 
    {															
		Compressy.setClosedLoopControl(true);					//Compressor Setup
																
		
    	CameraServer server = CameraServer.getInstance();		
    	server.setQuality(50);									//Camera Setup
    	server.startAutomaticCapture("cam0");										
    															
    															
    	//encoder.startLiveWindowMode();							//Encoder Initialize
    															
    	
    	//armPID.isEnabled();																	
    	//armPID.setInputRange(0, 550);							
    	//armPID.setOutputRange(-1, 1);							//PID Config
    	//armPID.setAbsoluteTolerance(5);
    	//armPID.startLiveWindowMode();
    }
    
    /**
     * This function is run once each time the robot enters autonomous mode
     */
    
    public void autonomousInit() 
    {
    	
    }

    /**
     * This function is called periodically during autonomous
     */
    
    public void autonomousPeriodic() 
    {

    }
    
    /**
     * This function is called once each time the robot enters tele-operated mode
     */
    
    public void teleopInit() 
    {
    	
    }

    /**
     * This function is called periodically during operator control
     */
    
    //Where are you getting these numbers?
    public void teleopPeriodic()
    {
    	//armset = (533*controller.getRawAxis(2)+17);
    	//armPID.setSetpoint(250);
    	SmartDashboard.putNumber("L Trigger Value", controller.getRawAxis(2));
    	//SmartDashboard.putNumber("Encoder Value", encoder.getRaw());
    	//SmartDashboard.putBoolean("ARMPID", armPID.isEnabled());
  
    	
    	//arm.set(controller.getRawAxis(2));
    	
    	if (controller.getRawButton(2))
    	{
    		pnuarm.set(.85);
    	}
    	else if (controller.getRawButton(3))
    	{
    		pnuarm.set(-.85);
    	}
    	else
    	{
    		pnuarm.set(0);
    	}
    	
    	if (controller.getRawAxis(3) > .5) 
    	{ 
    			launchWheel0.set(.5);
    			launchWheel1.set(-.5);	
    			
    	}
    	else if(controller.getRawButton(6))
    	{
    			launchWheel0.set(-1);
    			launchWheel1.set(1); 
    	}
    	else
    	{
    			launchWheel0.set(0);
    			launchWheel1.set(0);
    	}
    	
    	
    	if (controller.getRawButton(1)) 
    	{
    		Punch.set(true);
    	}
    	else
    	{
        	Punch.set(false);
    	}
    		
    	//Drive
    	leftDrive0.set(-controller.getRawAxis(1));
    	leftDrive1.set(-controller.getRawAxis(1));
    	rightDrive0.set(controller.getRawAxis(5));
    	rightDrive1.set(controller.getRawAxis(5));
    	//arm.set(armPID.get());
    	//if  (controller.getRawButton(4))
    	//{
    		//if (encoder.get() < 530)
    		//{
    	    	//arm.set(controller.getRawAxis(2)+.4);
    		//}
    		//else
    		//{
    			//arm.set(0.1);
    		//}
    	//}
    	
    }
    
    /**
     * This function is called periodically during test mode
     */
    
    public void testPeriodic() 
    {
    	LiveWindow.run();
    }
    
    //public void setPIDSourceType(PIDSourceType pidSource)
   // {
    	//pidSource =  ; 
   // }
    
   
    
} 
