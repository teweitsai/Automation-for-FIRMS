Automation Software for ultrasound force-induced remnant magnetization spectroscopy (usFIRMS)

A. Overview

To achieve the automatic measurement of ultrasound force-induced remnant magnetization spectroscopy (usFIRMS), LabVIEW is used to control the hardwares of instrument such as the lock-in amplifier, function generator, and servo motor. Each component’s function in the instrument is programmed individually to a subVI, and all subVIs are integrated into a LabVIEW project, "Sine Generator - R Series.lvproj." The subVI to communicate global variables among all subVIs is "Global vab.vi." Each subVI’s name is based on the hardware and function. For example, "SR530 read.vi" is to read the output of lock-in amplifier, SR530. "SR830 sweep update tw sonication soft 2.vi" is the main code to execute the automatic measurement.

B. Function Generator

For the function generator DS345, the subVI, "DS345 tw.vi," is used to control the amplitude and frequency of AC voltage signal for the laser’s frequency modulation (FM) and ultrasound generation. "DS345 tw sonication soft.vi" is used to provide the ultrasound radiation force in the soft mode as by using the centrifuge, which means the applied voltage ramps in a linear fashion.

C. Lock-in Amplifier

"SR830 sweep tw.vi" is used to find the central modulation frequency of cesium atom in a fixed magnetic field. The lock-in amplifier SR830 sweeps in a range of modulation frequency, and the current signal is fitted by a user-defined function to get the central modulation frequency. This process repeats with different power and voltage of laser’s controller until the best fitting is reached. The subVIs, "SR830 lock to phase tw.vi" and "SR830 DS345 Set OF and Read Single.vi," read the values on x- and y-panel on the lock-in amplifier for the analysis.

D. Servo Motor

To scan the sample, "SM motor tw.vi" is used after putting the biological sample on the holder fixed to a servo motor (Moog Animatics SM17). "SM motor repeat 2 tw.vi" does the repeated scanning by calling "SM motor tw.vi" repeatedly.

E. Sonication

The subVI, "TDC001 motor control tw.vi," controls the motion of motorized actuator to fix the sample for the sonicaiton. "Apply force DS345 TDC001 tw.vi" can fix the sample, apply the acoustic radiation force, and release the sample automatically. "SM motor repeat TDC DS345 tw 2.vi" combines the servo motor’s motion and sonication device to do the scanning and apply the mechanical force to change the state of biological samples.

If the application of ultrasound is in the soft mode, the codes are "Apply force DS345 TDC001 soft tw.vi" and "SM motor repeat TDC DS345 tw 2 soft.vi," which calls "DS345 tw sonication soft.vi" instead of "DS345 tw.vi" to provide AC signal in "Apply force DS345 TDC001 tw.vi" and "SM motor repeat TDC DS345 tw 2.vi."

F. Dichroic Atomic Vapor Laser Lock

To achieve the high sensitivity of magnetic signal, the dichroic atomic vapor laser lock (DAVLL) is critical to stabilize the laser’s wavelength. The signal by linear magneto-optical resonance (LMOR) is magnified by the lock-in amplifier and read by the computer. Once the dispersion signal of lock-in amplifier is less than or equal to the threshold, the remote relay closes to connect the lock-in amplifier and mixer to give a stable wavelength of laser.

The subVI, "USB relay.vi," controls the switch of remote relay. It is noted that the subVI, "usbsimplecommand.vi," is used to sent the command, which can be download from the website of Numato Lab. "SR530 read.vi" reads the signal of lock-in amplifier through the RS232 interface. "SR530 threshold.vi" controls the time to open and close the remote relay.

G. Main Code

The main execution code is "SR830 sweep update tw sonication.vi." It uses "Read From Spreadsheet 2015.vi" to read a csv file as input for a series of measurements with different acoustic radiation force application, which is modified from the original version on LabVIEW website. The transfer of parameters among all subVIs is by "Global vab.vi."

The program sweeps the modulation frequency in a wide range, and does the fitting to get the central modulation frequency. If the fitting is bad, the program can do the auto-phase to change the reference phase in lock-in amplifier. It is to minus the fitted phase value from the original reference phase, and return the new value to the lock-in amplifier. In addition, it can also update the output voltages of function generator and lock-in amplifier to get a better signal-to-noise ratio. The data of sweep can be saved once the optimal modulation frequency is determined. After this, the jobs of sonicator are read and queued. The mode of motor can be chosen to do one-way scanning, one time of forward-backward scanning, and completed scanning based on the queued jobs. The motion of motor can always be stopped or change the mode. After the press of “lock” button, the motor moves, and the sample’s magnetic signal is measured. And the scanning data is saved after the measurement is done.

