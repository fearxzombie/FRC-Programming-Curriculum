<div id="table-of-contents">
<h2>Table of Contents</h2>
<div id="text-table-of-contents">
<ul>
<li><a href="#sec-1">1. Account setup</a></li>
<li><a href="#sec-2">2. Software Installation &amp; Tool Setup</a></li>
<li><a href="#sec-3">3. WPI lib</a></li>
<li><a href="#sec-4">4. PID controllers</a></li>
<li><a href="#sec-5">5. Motor Controllers and WPI built-in functions</a></li>
<li><a href="#sec-6">6. Advanced Git and Github</a></li>
<li><a href="#sec-7">7. VS Code</a></li>
<li><a href="#sec-8">8. Driver station</a></li>
<li><a href="#sec-9">9. Networking Basics</a></li>
<li><a href="#sec-10">10. General Computer Science</a></li>
<li><a href="#sec-11">11. Debugging code</a></li>
<li><a href="#sec-12">12. What is it like to be a Programmer?</a></li>
<li><a href="#sec-13">13. Basic Physics</a></li>
<li><a href="#sec-14">14. Electronics, Wiring, and Control Systems</a></li>
<li><a href="#sec-15">15. Programming Projects</a></li>
<li><a href="#sec-16">16. Suggested Programming Calendar</a></li>
<li><a href="#sec-17">17. Resources</a></li>
</ul>
</div>
</div>



# Account setup<a id="sec-1" name="sec-1"></a>

-   github (join team group)
-   trello
-   slack (and channels, #programming)
-   MS Teams

# Software Installation & Tool Setup<a id="sec-2" name="sec-2"></a>

-   github
-   <https://help.github.com/en/desktop/getting-started-with-github-desktop>
-   <https://frc-docs.readthedocs.io/en/latest/docs/getting-started/getting-started-frc-control-system/wpilib-setup.html>
-   VS Code
-   WPI libs install
-   Driver Station
-   motor controller utilities
-   flashing firmware roboRIO, Limelight, radio
-   How to update software

# WPI lib<a id="sec-3" name="sec-3"></a>

-   API overview
-   Command based robot
    -   Robot.java
    -   RobotMap.java
    -   OI.java (xbox controllers)
    -   subsystems
    -   commands
    -   command Groups
    -   controlling motors
    -   sensor input
    -   drivetrains (tank, arcade style)
    -   Network tables (reading/writing values)
    -   subsystems and command interaction (scheduler)
-   Missing Documentation
    -   Source material:
    -   Command.java
        -   Command()
            -   constructor will have the same name as your Command
            -   call requires() for every subsystem used, to prevent
                conflicts. more than one command cannot use the same
                subsystem at the same time
            -   CAREFUL: do not put initialization code here, it belongs in initialize()
        -   initialize()
            -   Anything that would need to happen before the command starts.
            -   Example: Getting the starting position from an encoder.
            -   CAREFUL: not to put initialize code into execute, if it only needs to be run once
                it should be in initialize() not execute()
        -   execute()
            -   The action that needs to be repeated over and over again.
            -   Treat this method as a single iteration of a loop.
            -   will keep getting called every time through the scheduler, until 
                1.  isFinished() returns true OR
                2.  the command is interrupted, another command that requires the same subsystem
            -   CAUTION: While this command's execute() is running, no other
                code is being run.  if this code takes too long (say more
                than 10ms) it will make other parts of the robot have
                problems. Drive may stutter. Sensors won't update quickly
                enough. controls will be sluggish and erratic. Waiting for
                input, highly iterative code, slow computation are all a
                no-no.
            -   Example: Reading the values from a joystick and sending them to the drivetrain.
        -   isFinished()
            -   is checked after every execute()
            -   default commands should always return False
            -   if execute() only runs once, then should always return True
        -   end()
            -   This method will run after the isFinished() method returns a true value.
            -   This should contain anything that needs to happen after a command finishes.
            -   Example: Set the drivetrain motors to stop.
        -   interrupted()
            -   This command will run if another command takes control of a subsystem that this command was using.
            -   may be different action than end() may be the same
            -   Example: Setting a motor to stop
    -   OI.java
        -   button.whenPressed(new ExampleCommand());
            Will execute when the button is first pressed down
        -   button.whenReleased(new ExampleCommand());
            Will execute when the button is first released
        -   button.whileHeld(new ExampleCommand());
            Will repeatedly execute while the button is held down. The interrupted() method of the command will be called when the button is released.
        -   OTHERS?
    -   RobotMap.java
        -   This is where you can set up all of your device port configurations.
        -   This allows you to have all of your configuration information in one place.
        -   This is also a good place to set up constant values to use in your code.

# PID controllers<a id="sec-4" name="sec-4"></a>

-   Bang-Bang controller
-   define P, I, and D
-   arm example
-   speed example
-   steering

# Motor Controllers and WPI built-in functions<a id="sec-5" name="sec-5"></a>

-   motor control
    -   setting motor settings
    -   get position
    -   direct PID control from motor controller
        -   vs. WPI lib PID controller
    -   Motion Profiling
        -   setting max acceleration for motors
    -   lead-follow  (speed and position)
    -   motion magic
-   OI
    -   <https://frc-docs.readthedocs.io/en/develop/docs/software/commandbased/binding-commands-to-triggers.html>
    -   WhenPressed, WhilePressed, Toggle, etc
    -   instantiating of commands classes
-   CAN Bus
    -   Pneumatic: Compressor, Solenoid, DoubleSolenoid
    -   Electrical: PowerDistributionPanel
    -   3rd Party: Talon SRX, Victor SPX, SPARK MAX, Pideon IMU
-   WPI Sensors
    -   Gyro
        -   <https://pdocs.kauailabs.com/navx-mxp/examples/automatic-balancing/>
    -   Gyro + Accelerometer + Magnetometer
        -   <https://pdocs.kauailabs.com/navx-mxp/guidance/terminology/>
    -   distance sensors
    -   Hall effect, etc&#x2026;
-   WPI USBCamera class
-   LimeLight
    -   walk through limelight's very good examples
-   Pixy cam
    -   object detection
    -   line follow
-   Pathfollowing basics
    -   position & orientation, start and desired target
    -   path selection
    -   path follow (motor speeds)
    -   tracking changes
    -   adapting to errors
-   Pathfollow libraries
    -   PathWeaver
    -   Pure Pursuit
    -   Chezy Drive
-   PathWeaver
    -   Overview <https://wpilib.screenstepslive.com/s/currentCS/m/84338>
    -   <https://wpilib.screenstepslive.com/s/currentCS/m/84338/l/1021628-creating-a-pathweaver-project>
    -   <http://wpilib.screenstepslive.com/s/currentCS/m/84338/l/1021631-integrating-path-following-into-a-robot-program>
    -   Chief Delphi thread <https://www.chiefdelphi.com/t/help-with-pathweaver/342464>

# Advanced Git and Github<a id="sec-6" name="sec-6"></a>

-   <https://docs.wpilib.org/en/latest/docs/software/basic-programming/git-getting-started.html>
-   Github Cheat Sheet <https://github.github.com/training-kit/downloads/github-git-cheat-sheet.pdf>
-   <https://learngitbranching.js.org/>
-   branching
-   pull requests
-   merging

# VS Code<a id="sec-7" name="sec-7"></a>

-   work flow
-   quick commands for building, saving,
-   Github integration
    -   pull, branch, code, commit, push
-   Build and Gradle
-   installing libraries
-   updating libraries
-   special files
    .gitignore .vscode vendordeps
-   <https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf>
-   <https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf>
-   Java formatting <https://github.com/redhat-developer/vscode-java/wiki/Formatter-settings>

# Driver station<a id="sec-8" name="sec-8"></a>

-   install
-   setup
-   networktables
    -   reading values
    -   writing values
-   exampe PID tuning with DS parameters

# Networking Basics<a id="sec-9" name="sec-9"></a>

-   <https://frc-docs.readthedocs.io/en/latest/docs/networking/networking-introduction/index.html>
-   Wired vs WiFi
-   Switches, routers, Radios, Mesh Networks
-   Network wiring
-   TCP, UDP, HTTP, HTTPS
-   bandwidth, latency, packet loss
-   WiFi
    -   2.4Ghz vs 5Ghz
    -   WiFi interference
-   FRC specific network setup
    -   <https://frc-docs.readthedocs.io/en/latest/docs/networking/ip-networking.html>
    -   10.29.30.1/24  subnet
    -   mDNS
    -   assigned IP addresses for radio, roboRIO, driver station
    -   allowed ports
    -   allowed bandwidth
    -   effects of dropped packets & latency

# General Computer Science<a id="sec-10" name="sec-10"></a>

-   data structures
    -   basic types, int, float double, char, strings
    -   lists/arrays
    -   structures
    -   enum (?)
-   Constants
-   Abstract Data Types and functions
    -   Java classes 
        -   basic differences between public and private
    -   Java class organization
        -   one per file
        -   how to import
        -   initialization
        -   constructors
        -   destructors
        -   ADVANCED: garbage collection
    -   accessing Java methods and variables
        -   dot notation ClassName.methodName() Classname.variableName
    -   Class inheritance
        -   base classes for Robot subsystem, command, etc
        -   interaction with constructors
        -   scheduling methods you don't see under the hood
-   logic
    -   if, then, else
    -   while
    -   for
-   function calls
    -   parameter passing
    -   return values
    -   side effects
-   Style
    -   code readability
    -   variable naming, camel case, underscores, capitalization, prefixes
        -   no funny joke names
        -   name variables for what they are
        -   \_foo for local variables
        -   other conventions
    -   comments
        -   need to be accurate and up to date
        -   add to the code to increase understanding
        -   comments should explain WHY not WHAT, the what should be obvious from your code.
        -   include references to documentation, API, etc
    -   adopt a style
        -   <https://wpilib.screenstepslive.com/s/currentCS/m/java/l/145309-java-conventions-for-objects-methods-and-variables>
        -   automatic java style on file save w/ VSCode

# Debugging code<a id="sec-11" name="sec-11"></a>

-   print to the console
    -   print values, weird conditions, errors, println()
-   BETTER: display values to Smartdashboard
    -   <https://wpilib.screenstepslive.com/s/currentCS/m/smartdashboard/c/92705>
-   debug commands and subsystems:
    -   <https://wpilib.screenstepslive.com/s/currentCS/m/smartdashboard/l/255422-displaying-the-status-of-commands-and-subsystems>
-   check for expected and unexpected values (assert statements)
-   defensive programming (ignore bad input, or just STOP)
-   check your assumptions
-   If you are confused about what the code is doing, then you don't
    understand the code or you don't understand what's going
    on. LEARN what's really happening, don't keep changing the code.
-   debug tool for robot <https://github.com/wpilibsuite/robot-characterization>

# What is it like to be a Programmer?<a id="sec-12" name="sec-12"></a>

-   expect to spend lots of time googling and reading web pages
    -   always backup your code
        -   ABC: Always Be Committing. Use version control (github for example)
    -   Use branches
        -   bookmark working code, keep experimental code off the main branch
    -   nothing ever works the first time
    -   every line of code is an opportunity for a bug
        -   off by one errors ( < vs <= )
    -   study other peoples code
    -   copy from the best
    -   learning how to debug
        -   ask for help
        -   explain your problem to someone (or rubber duck)
-   typing skills are important
-   Peter Norvig on how to be a good programmer: <http://norvig.com/21-days.html>

# Basic Physics<a id="sec-13" name="sec-13"></a>

-   speed, acceleration, gravity, motion
    -   newton's laws
-   force, momentum, energy
-   levers
-   Vectors
    -   adding force and motion vectors
-   Friction (?)
-   Elastic vs Inelastic collisions (?)
    -   <https://www.khanacademy.org/science/physics/linear-momentum/elastic-and-inelastic-collisions/v/elastic-and-inelastic-collisions>

# Electronics, Wiring, and Control Systems<a id="sec-14" name="sec-14"></a>

-   roboRIO anatomy
    -   layout
    -   power
    -   CPU
    -   input/output (CAN, digital IO, USB)
    -   USB camera
-   CAN bus 
    -   wiring
    -   debugging
    -   Phoenix Tuner CAN debugging tool
    -   <https://phoenix-documentation.readthedocs.io/en/latest/ch03_PrimerPhoenixSoft.html#what-is-phoenix-tuner>
-   Example wiring diagrams

# Programming Projects<a id="sec-15" name="sec-15"></a>

-   the best way to learn is to have a project, a reason to learn and use the skills
-   Clean room program last year's robot from scratch
-   add features to 2019 Robot
    -   gyro for level climb
    -   gyro for drive straight, field alignment
    -   limit switch or hall effect for arm or stilt
    -   only idle intake after running intake and until eject
    -   fully autonomous hatch placement
    -   PathWeaver or other path planning
    -   autonomous 2 hatch & one cargo pickup sandstorm
    -   add LED lights
    -   monitor for brownout (alert w/ LED, turn something off, reduce power to motors)

# Suggested Programming Calendar<a id="sec-16" name="sec-16"></a>

-   Pre-Season
    -   practice robot, with basic drive and a few sensors
-   week -2
    -   update software for new season
        = driverstation
        = firmware for roboRIO, radio
        = VSCode
        = motor controller firmware
        = everything
-   Week 0
    -   survey build teams, make list of components
    -   plan subsystems, basic commands,
    -   create code for drivable robot
    -   plan Driverstation layout
    -   plan vision systems and camera placement
    -   start writing subsystem (even if just stubs)
    -   pic CAN bus id, digital IO assignments for sensors, etc.
    -   Give specs to Control sub-team
-   Week 1
-   Week 2
-   Week 3
-   Week 4
-   Week 5
-   Week n+1

# Resources<a id="sec-17" name="sec-17"></a>

-   General
    -   <http://wpilib.screenstepslive.com/s/currentCS>
    -   <https://frc-docs.readthedocs.io/en/latest/>
    -   <https://www.team254.com/resources/>
    -   <https://betawolves.org/resources/>
    -   <https://github.com/FRCTeam3255/FRC-Java-Tutorial>
-   Places to Ask for Help
    -   <https://www.chiefdelphi.com/tags/programming>
    -   <https://discord.gg/frc>
-   Programming Java
    -   <https://www.codecademy.com/learn/learn-java>
    -   <https://www.udemy.com/java-tutorial/> Java Tutorial for Complete Beginners
    -   <https://frc-pdr.readthedocs.io/en/latest/>
    -   Practice: <https://codingbat.com/java>
    -   Practice: <https://practiceit.cs.washington.edu/>
    -   Online Textbook: <http://math.hws.edu/javanotes/>
    -   Textbook: <https://www.amazon.com/Building-Java-Programs-Basics-Approach/dp/0136091814/>
    -   <https://dev.to/javinpaul/top-20-websites-to-learn-coding-with-java-python-sql-algorithms-and-git-for-free-in-2019-best-of-lot-l2l>
-   Intro to Command Based Programming (VS Code & Java)
    -   Part 1: <https://youtu.be/wW_djLkD1B8>
    -   Part 2: <https://youtu.be/9MpJgUUsLZw>
    -   Part 3: <https://youtu.be/5Zr7K_2mnrw>
    -   Part 4: <https://youtu.be/YNluD_TNj5E>
    -   Part 5: <https://youtu.be/oGMy4FJLKy4>
-   Installing 3rd Party Software
    -   <https://phoenix-documentation.readthedocs.io/en/latest/ch05a_CppJava.html>
    -   <https://wpilib.screenstepslive.com/s/currentCS/m/getting_started/l/682619-3rd-party-libraries>
-   FRC Robobuilder Tool
    -   Brad Miller, 6 part video series
        <https://www.youtube.com/playlist?list=PLYA9eZLlgz7t9Oleid2wtlgnvhGObeKzp>
-   Online Lessons
    -   <https://github.com/FRCTeam3255/FRC-Java-Tutorial>
    -   <https://frc-west.github.io/>
    -   <http://team2363.org/2016/11/command-based-java-for-frc/>
    -   FRC & Java <https://stemrobotics.cs.pdx.edu/node/4196?root=4196>
    -   AP CS principles <https://www.khanacademy.org/computing/ap-computer-science-principles>
-   Command Based Programming
    -   <https://docs.wpilib.org/en/latest/docs/software/commandbased/command-based-changes.html> (changes for 2020)
    -   <https://frc-docs.readthedocs.io/en/develop/docs/software/commandbased/index.html> (GOOD, official FRC docs)
    -   <https://docs.google.com/presentation/d/1kVDppzkow4M19QsfyUDZrhe_evU_ATksjlAW-aqYXe8/edit> (GOOD! slides)
    -   <https://docs.google.com/presentation/d/11xui-66VAjulfWKHVRLzJZ1m9SdGiW3B3Cz6o3vX-os/edit> (GOOD! slides)
    -   <https://wpilib.screenstepslive.com/s/currentCS/m/java/c/88893>
    -   <https://wpilib.screenstepslive.com/s/currentCS/m/java/l/599745-scheduling-commands>
-   Code Examples:
    -   Motor controllers
    -   <https://github.com/CrossTheRoadElec/Phoenix-Examples-Languages/tree/master/Java>
    -   <https://github.com/REVrobotics/SPARK-MAX-Examples>
    -   Sensors/Gyro
    -   <https://wpilib.screenstepslive.com/s/currentCS/m/java/c/88895>
    -   <https://pdocs.kauailabs.com/navx-mxp/examples/automatic-balancing/> (sample code)
    -   <https://pdocs.kauailabs.com/navx-mxp/>
-   CAN Bus programming:
    -   <https://frc-docs.readthedocs.io/en/develop/software.html#can-devices>
    -   Cross the Road Electronics documentation, CAN, APIs, installation
    -   <https://phoenix-documentation.readthedocs.io/en/latest/index.html>
-   Control:
    -   <https://www.team254.com/documents/control/>
-   Vision:
    -   <https://wpilib.screenstepslive.com/s/currentCS/m/vision>
    -   <https://www.team254.com/documents/vision-control/>
    -   <http://docs.limelightvision.io/en/latest/>  (in particular Programming section)
    -   <https://chameleon-vision.gitlab.io/chameleon-site/>
-   Motion Profiling, Motion Control:
    -   <https://www.youtube.com/watch?v=i_6eh1C_1V8>
    -   <https://www.youtube.com/watch?v=4rbT-oscpx0> (part 1)
    -   <https://www.youtube.com/watch?v=VqhAtgtoB-8> (part 2)
    -   <https://www.chiefdelphi.com/t/a-deep-dive-into-wpilib-trajectories/367057>
    -   <https://pietroglyph.github.io/trajectory-presentation/#/>
    -   <https://www.youtube.com/watch?v=fEVU7dVc8B4>
    -   <http://www2.informatik.uni-freiburg.de/~lau/students/Sprunk2008.pdf>
-   Path Planning:
    -   Intro: <https://wpilib.screenstepslive.com/s/currentCS/m/84338>
    -   <https://www.youtube.com/watch?v=8319J1BEHwM>
    -   SLIDES: <https://docs.google.com/presentation/d/1xjtQ5m3Ay4AYxS_SfloF2n_vWZnCU25aXZuu9A59xPY/pub?start=false&loop=false&delayms=3000&slide=id.p>
    -   pure pursuit: <https://www.chiefdelphi.com/t/paper-implementation-of-the-adaptive-pure-pursuit-controller/166552>
    -   <https://github.com/Dawgma-1712/FRC-2018/wiki/pure-pursuit>
-   Swerve Drive Resources:
    -   <https://www.chiefdelphi.com/t/team-4048-swerve-drive-code-release/166605>
    -   <https://www.strykeforce.org/resources/Mechanical_Design_Description_of_Stryke_Force_Swerve_Drive_Units.pdf>
    -   <https://github.com/strykeforce/cookiecutter-robot>
    -   <https://www.chiefdelphi.com/t/paper-4-wheel-independent-drive-independent-steering-swerve/107383>
-   Related Chief Delphi threads:
    -   <https://www.chiefdelphi.com/t/your-team-sucks-at-programming-heres-why/358026>
    -   <https://www.chiefdelphi.com/t/what-is-the-best-way-to-teach-incoming-freshmen/362661/16>
    -   <https://www.chiefdelphi.com/t/what-is-the-best-way-to-teach-incoming-freshmen/362661/23>
-   Misc Programming Resources:
    -   Python FRC robot simulator <https://github.com/robotpy/pyrobottraining>
-   Other online FRC Courses/Lectures/Notes
    -   <https://www.citruscircuits.org/fallworkshops.html> A collection of videos and slides
-   Robot Control Theory
    -   <https://file.tavsys.net/control/controls-engineering-in-frc.pdf>
    -   <https://www.matthewpeterkelly.com/research/MatthewKelly_IntroTrajectoryOptimization_SIAM_Review_2017.pdf>
-   Odometry:
    -   <https://www.cs.princeton.edu/courses/archive/fall11/cos495/COS495-Lecture5-Odometry.pdf>
-   Advanced Programming (not FRC specific):
    -   <https://www.toptal.com/robotics/programming-a-robot-an-introductory-tutorial>
    -   <https://www.coursera.org/learn/mobile-robot/>
