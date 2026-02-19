# USB Hub Switch

Version: V2.0.2

The USB Hub Switch is ideal for equipment that is operated by an attached computer, such as laser cutters, tethered 3D printers, CNC machines, and more.

The USB Hub Switch is a 4-port USB 2.0 hub, providing 4 USB-A ports. All ports are Hi-Speed, Low-Speed, and Full-Speed compatible, with an upstream Hi-Speed or Full-Speed port. The power to the 4 downstream ports is enabled/disabled by the switching mechanism, with a USB-compliant soft start ramp-up. 

This enables you to lock out any USB bus-powered device to stop users from interacting with a machine. Common examples are USB keyboards or mice, USB connections from a computer to the equipment, or the USB port used to load programs. 

## Example Deployment: Epilog Laser Cutter

The SHED has 3 Epilog Laser Cutters. While we could interrupt AC power to them, it would require running through the 60-second startup procedure every time someone wants to use the machine. It also meant that removing your keycard would immediately and irrecoverably stop the in-progress job. Since some laser cutter jobs can be in the realm of hours, and have multiple team members swap out to watch it, this would be infeasible. 

Instead, we decided to lock out the computer that is used to prepare files. Without an authorized user's keycard present, you are unable to use the keyboard or mouse of the computer, or plug in your own USB drive or similar to import files. The end result is no modification of the laser or its operation, while stopping anyone unauthorized from using the laser, since they cannot prepare files.

This implementation allows students to "hand off" larger jobs to other students if they need to go to class without having to leave their ID, and also allows one student to be cutting something while another uses the computer to prepare their on-deck file, push it to the laser cutter, and then hang on to their ID.

Since these are shared community computers, it also provides the critical information of who was using the computer at what specific time. If we ever need to investigate a specific file or if a user attempted to load a malicious program, we will know exactly who was using the computer. 
