.. _gsoc-proposal-Himanshu kohale:

Librobotcontrol support for newer boards
########################################

Introduction
*************

Introducing librobotcontrol package support with newer beagleboards like BeagleV-fire and BeagleBone-AI64.

Summary links
=============

- **Contributor:** `Himanshu kohale <https://forum.beagleboard.org/u/ayush1325>`_
- **Mentors:** `Jason Kridner <https://forum.beagleboard.org/u/jkridner>`_, `Deepak khatri <https://forum.beagleboard.org/u/lorforlinux/summary>`_
- **Code:** `Google Summer of Code / Himanshuk / librobotcontrol · GitLab <https://openbeagle.org/Himanshuk/librobotcontrol>`_
- **Documentation:** `Himanshuk / docs.beagleboard.io · GitLab <https://openbeagle.org/Himanshuk/docs.beagleboard.io>`_
- **GSoC:** NA

Status
=======

This project is currently just a proposal.

Proposal
========

Completed all the requirements listed on the `ideas page <https://gsoc.beagleboard.io/ideas/>`_.
- Source dive and get to know with packages and examples.
- The code for the cross-compilation task can be found, Submitted through the pull request :- `pull request <https://github.com/jadonk/gsoc-application/pull/191>`_ .
- Proposal :- `librobotcontrol support for newer boards <https://gsoc-beagleboard-io-himanshuk-849994ca45b62e3ba1c5b2e74d1ebb233.beagleboard.io/proposals/himanshu-kohale.html>`_ .

About 
=====

- **Forum:** :fab:`discourse` `Himanshuk (Himanshu kohale) <https://forum.beagleboard.org/u/himanshuk/summary>`_
- **OpenBeagle:** :fab:`gitlab` `Himanshuk (Himanshu kohale) <https://openbeagle.org/Himanshuk>`_
- **Github:** :fab:`github` `Himanshukohale22 (Himanshu kohale) <https://github.com/Himanshukohale22>`_
- **School:** :fas:`school` `Veermata Jijabai Technological Institute <https://vjti.ac.in/>`_
- **Country:** :fas:`flag` India
- **Primary language:** :fas:`language` English, Hindi, Marathi
- **Typical work hours:** :fas:`clock` 8AM-5PM Indian standard timeline
- **Previous GSoC participation:** :fab:`google` N/A

Project
********

**Project name:** librobotcontrol support for newer boards (AI-64 and BeagleV-Fire).

Description
============

Librobotcontrol is package of C library which contains examples and testing programs for Robotic control projects used by `robotic cape <https://www.beagleboard.org/boards/beaglebone-robotics-cape>`_  which is sold by beagleboard.org,
which supports the BBB and AI thanks to Deepak khatri who Previouly worked upon cape compatibility layer on BBB. 
There are many capes beagleboard sold with compatibility for beagleboards but with robotic cape only BBB and AI support the librobotcontrol packages, we need to refine the device trees overlays to use this librobotcontrol package with AI-64 and BeagleV-Fire. 
The main goal for this projects is to Update librobotcontrol for Robotics Cape on BeagleBone AI-64(TDA4VM) and BeagleV-Fire(polarV-soc).
librobotcontrol required various pheripherals for sensors and motor configuration with BeagleBoards. 
Different pheripherals are to be updated for various sensors, motors and satellite modules are  to be used with newer bords.

- I2C 
- SPI 
- UART 
- GPIO 

As BeaglBboneBlack(am335x) support librobotcontrol package due to modification in devic tree overlayes for robotic cape.
we need to modify the device tree overlay for beaglebone-AI64. beaglebone-AI64 is TDMA4VM soc based board which support the j721e device tree binary file.
Here is the example of device tree for beaglebone-AI64. in which GPIO LED are configured.

.. image:: https://docs.beagleboard.org/latest/_images/board-block-diagram.svg
    :alt: beaglebone-AI64
    :align: center  

BeagleBone AI-64 uses TI J721E-family TDA4VM system-on-chip (SoC) which is part of the K3 Multicore SoC architecture.
Each TI evm has an unique device tree binary file required by the kernel. As BeaglBboneBlack need (ti,am335x) similarly beaglebone-AI64 support.


base_dtb = "k3-j721e-beagleboneai64.dts;

specific compatibility/model for beaglebone AI-64

compatible = "beagle,j721e-beagleboneai64", "ti,j721e";
model = "BeagleBoard.org BeagleBone AI-64";

here the example for custom device tree for external LED gpio support on beaglebone-AI64.
Easy LED control through GPIO ddriver.

code::

    &{/chosen} {
	overlays {
		BONE-LED_P8_03 = __TIMESTAMP__;
	    };
    };

    &bone_led_P8_03 {
    status = "okay";                    
    // access: sys/class/leds/led_P8_03
    label = "led_P8_03";
    linux,default-trigger = "heartbeat";
    default-state = "on";
    };


.. image:: https://devicetree-specification.readthedocs.io/en/stable/_images/graphviz-58c8267ade85edeca7b1b0299af2b1e473987ddc.png
    :alt: Device tree Implementation
    :align: center

Similar to above GPIO example for custom device tree overlay we need to write for varoius pheripherals according to P8 and P9 header expansion details for robotics cape. 
Each I2C, SPI, UART pheripherals have specific register addresess based on which bone identify the device connected to pheripherals.understanding the hardware of robotic-cape and requirements according to librobotcontrol package we 
can write customized device tree overlay for robotic cape `interface <https://elinux.org/Beagleboard:BeagleBone_cape_interface_spec#cite_note-2>`_ with beaglebone-AI64 expansion headers.



In case of using librobotcontrol package in BeagleV-Fire there is already support for robotic-cape but only device tree Nodes are for PWM's,I2C and GPIO's. as it need to update according to librobotcontrol example as it will support the librobotcontrol package.
its process in which, need to create custom cape gateware for different examples like blinky, motor and communication interfaces.it will be done as `here <>`_ is the guide for customized gateware for beagleV-fire.
The PolarFire SoC device used on BeagleV-Fire is an SoC FPGA which includes a RISC-V processors subsystem and a PolarFire FPGA on the same die. The gateware configures the Microcprosessor subsystem’s hardware and programs the FPGA with digital logic allowing customization of the use of BeagleV-Fire P8/P9 expansion headers.
The cape gateware handles the P8 and P9 connectors signals.

.. image:: https://docs.beagleboard.org/latest/_images/Gateware-Flow-simplified-overview.png
    :alt: BeagleV-fire gateware architecture
    :align: center

robotics_cape.dts file in gateware of beagleV-fire PWM and GPIO's are configured we need to write device tree sript for I2C, SPI and UART which will need to support librobotcontrol package.


Previous work:-

Previously Deepak khatri who worked upon the cape compatibility for beagleboards. use the robotic cape for various tasks. 
using pre-work upon robotic cape, i can take a deep dive to robotic cape compatibility with BeaglBboneBlack (BBB) and how its works.
In previous gsoc-2022 participation kai yamada work upon same project which was about robotic-cape support with BeagleBone-AI (BB-AI).
In both projects implementation was about the device tree overlayes for BBB and AI for specific pheripherals to enabling functionality of PWM, I2C and SPI and UART for robotic capes.  

Problem and solution:

BB AI-64:
BeagleBone-AI64 is ti,j721e based TDA4VM SOC board which is for AI and Machine learning applications. to support librobotcontrol package there is not proper device tree overlay for specific robotic-cape.
In case of BB AI-64 board(TDA4VM) support j721e device tree which will helpful to creat .dts files for robotic cape.

BeagleV-fire:
There is device tree overlayes gateware for beagleV-fire to use with robotic capes to support librobotcontrol but its only unable SPI and GPIO's pheripheral. Need to update cape gateware.
first thing to do is to creat device tree overlayes for beagleV-fire using reference as `BeaglBboneBlack(BBB) device tree overlayes <https://github.com/beagleboard/librobotcontrol/blob/master/device_tree/dtb-4.14-ti/am335x-bone-common-universal-pins.dtsi>`_ also 
there are some examples available to customized use of `device trees with relay capes <https://www.beagleboard.org/blog/2022-02-15-using-device-tree-overlays-example-on-beaglebone-cape-add-on-boards>`_ 
which can be very useful during deep understand and functionality of board.


Software
=========

- Device tree's overlays for beagleboards will be used.The project requires the use of the device tree compiler (dtc) for compiling the device tree source (ex. *.dts, *.dtsi) files.
- Primarily VScode and gitlab with web-IDE is use in this project for deep dive into code and firmware of librobotcontrol and rc (robot control library) examples.
- Eclipes IDE can be used for starting phase of project.
- C language.

Hardware
========

A list of hardware that you are going to use for this project.

- `Beaglebone Black <https://www.digikey.in/en/products/detail/beagleboard-by-seeed-studio/102110420/12719590?cur=INR&lang=en&utm_adgroup=&utm_source=google&utm_medium=cpc&utm_campaign=PMax%20Shopping_Product_High%20ROAS&utm_term=&productid=12719590&utm_content=&utm_id=go_cmp-20122528480_adg-_ad-__dev-c_ext-_prd-12719590_sig-Cj0KCQjw8J6wBhDXARIsAPo7QA8aIQNqlJuRD5bNfrHXhCPfGk6LSU2nxmVaauLzHgc6BreuyUqskmEaAsJoEALw_wcB&gad_source=1&gclid=Cj0KCQjw8J6wBhDXARIsAPo7QA8aIQNqlJuRD5bNfrHXhCPfGk6LSU2nxmVaauLzHgc6BreuyUqskmEaAsJoEALw_wcB>`_
- `Beaglebone AI 64 <https://www.digikey.in/en/products/detail/beagleboard-by-seeed-studio/102110646/15929655?cur=INR&lang=en&utm_adgroup=&utm_source=google&utm_medium=cpc&utm_campaign=PMax%20Shopping_Product_High%20ROAS&utm_term=&productid=15929655&utm_content=&utm_id=go_cmp-20122528480_adg-_ad-__dev-c_ext-_prd-15929655_sig-Cj0KCQjw8J6wBhDXARIsAPo7QA8OHJluOkNDsca6onRdfGL-SiAdurymvfiCgGq1_E1YqW2WvDsyjZYaAnUmEALw_wcB&gad_source=1&gclid=Cj0KCQjw8J6wBhDXARIsAPo7QA8OHJluOkNDsca6onRdfGL-SiAdurymvfiCgGq1_E1YqW2WvDsyjZYaAnUmEALw_wcB>`_
- `BeagleV-fire <https://www.digikey.in/en/products/detail/beagleboard-by-seeed-studio/102110898/21706497>`_
- Beaglebone-capes
   - `Robotic cape <https://in.element14.com/beagleboard/bb-cape-robotics/robotics-cape-for-beaglebone-black/dp/2612581>`_
- Additional hardware for project:-
  - `Jumper cables <https://www.renaissancerobotics.com/JST_Jumper_Bundle.html>`_ :-
   - 4-wire jst cables 
   - 6-wire jst cables
  - `DC motors <https://www.sparkfun.com/products/13302>`_
  - `Servo motor <https://www.digikey.in/en/products/detail/900-00005/900-00005-ND/361277?WT.mc_id=IQ_7595_G_pla361277&wt.srch=1&wt.medium=cpc&WT.srch=1&gclid=CJz-qdC9n9ICFRO4wAodOjYLuQ>`_ 
  - `FTDI-TTL serial wire <https://www.adafruit.com/product/70>`_
  - `SD-card <https://www.amazon.in/SanDisk-Ultra-microSD-UHS-I-120MB/dp/B08L5FM4JC/ref=sr_1_3?dchild=1&keywords=64gb+sd+card&qid=1617689846&sr=8-3>`_ 
  - `power supply 12v <https://www.amazon.in/REES52-Adapter-Switch-Charger-Raspberry/dp/B07WJ34VJL>`_
- Useful testing tools:-
  - Oscilloscope
  - Multimeter
  - Soldering station
  - Mechanical toolbox


Timeline
********

Timeline summary
=================

.. table:: 

    +------------------------+---------------------------------------------------------------------------------------------------------------+
    | Date                   | Activity                                                                                                      |                                  
    +========================+===============================================================================================================+
    | February 26            | Connect with possible mentors and request review on first draft                                               |
    +------------------------+---------------------------------------------------------------------------------------------------------------+
    | March 4                | Complete prerequisites, verify value to community and request review on second draft                          |
    +------------------------+---------------------------------------------------------------------------------------------------------------+
    | March 11               | Finalized timeline and request review on final draft                                                          |
    +------------------------+---------------------------------------------------------------------------------------------------------------+
    | March 21               | Submit application                                                                                            |
    +------------------------+---------------------------------------------------------------------------------------------------------------+
    | May 1                  | Start bonding <bonding>                                                                                       |
    +------------------------+---------------------------------------------------------------------------------------------------------------+
    | May 27                 | Start coding and introductory video                                                                           |
    +------------------------+---------------------------------------------------------------------------------------------------------------+
    | June 3                 | Release introductory video and complete milestone #1<milestone1>`                                             |
    +------------------------+---------------------------------------------------------------------------------------------------------------+
    | June 10                | Complete milestone #2                                                                                         |
    +------------------------+---------------------------------------------------------------------------------------------------------------+
    | June 17                | Complete milestone #3                                                                                         |
    +------------------------+---------------------------------------------------------------------------------------------------------------+
    | June 24                | Complete milestone #4                                                                                         |
    +------------------------+---------------------------------------------------------------------------------------------------------------+
    | July 1                 | Complete milestone #5                                                                                         |
    +------------------------+---------------------------------------------------------------------------------------------------------------+
    | July 8                 | Submit midterm evaluations                                                                                    |
    +------------------------+---------------------------------------------------------------------------------------------------------------+
    | July 15                | Complete milestone #6                                                                                         |
    +------------------------+---------------------------------------------------------------------------------------------------------------+
    | July 22                | Complete milestone #7                                                                                         |
    +------------------------+---------------------------------------------------------------------------------------------------------------+
    | July 29                | Complete milestone #8                                                                                         |
    +------------------------+---------------------------------------------------------------------------------------------------------------+
    | August 5               | Complete milestone #9                                                                                         |
    +------------------------+---------------------------------------------------------------------------------------------------------------+
    | August 12              | Complete milestone #10                                                                                        |
    +------------------------+---------------------------------------------------------------------------------------------------------------+
    | August 19              | Submit final project video, submit final work to GSoC site and complete final mentor evaluation               |
    +------------------------+---------------------------------------------------------------------------------------------------------------+


Timeline detailed
=================

Community Bonding Period (May 1st - May 26th)
==============================================

- Get to know with community, Read resources for librobotcontrol and beagleboards, get up to speed to begin working on the projects.
- At current period of time, all the required hardware will be available.
- Setup all the beagleboard hardware (Flashing OS and test hello world).
- Check all hardware with beagleboard like DC motors, Servo motors and available sensors.
- Use robotic cape with beagleboard BeaglBboneBlack (BBB) and librobotcontrol.
- Creat merge request for accesing BBB pins.
- Creat example for devic tree overlay with AI-64 and creat merge request.

Coding begins (May 27th)
=========================

- Discuss with mentor for librobotcontrol support package with Ai-64. 
- Understand device tree overlays for BeaglBboneBlack (BBB) and AI written for robotic cape. 
- Start to write Device tree for PWM 
- Test Device tree with AI-64.
- Creat merge request for PWM test.

Milestone #1, Introductory YouTube video (June 3rd)
===================================================

- Include introductory video.
- Prepare documentation for the process. 


Milestone #2 (June 10th)
==========================

- For RoboticsCape, Test a device tree overlay to allow AI-64 to light the power LEDs with GPIO support.
- Test PWM devic tree overlay with robotics cape with help of Hardware specification and check with oscilloscope.
- Get feeback from mentors.


Milestone #3 (June 17th)
=========================

- Write I2C device tree for AI-64.
- Get feedback from mentor.

Milestone #4 (June 24th)
==========================

- Test I2C with IMU and robotic cape.
- Creat merge request for I2C Device tree overlays.
- Write SPI device tree overlay for AI-64.
- Test with robotic cape.
- get feedback from mentor.

Milestone #5 (July 1st)
========================

- Creat .dts file for robotic cape which will support AI-64 using pre-work.
- Test .dts file with robotic cape with AI-64.
- Test example of librobotcontrol with AI-64.
- get feedback from mentor.

Submit midterm evaluations (July 8th)
=====================================

.. important:: 
    
    **July 12 - 18:00 UTC:** Midterm evaluation deadline (standard coding period) 

Milestone #6 (July 15th)
=========================

- Discuss with mentor for librobotcontrol support package with beaeglV-fire board as librobotcontrol support beaeglV-fire.
- Test RoboticsCape with cape gateware for beagleV-fire.
- Understand the customization process for cape Gateware. 

Milestone #7 (July 22nd)
=========================

- Customized LED example for robotic-cape gateware.
- Test GPIO's, Robotic cape with beaeglV-fire.
- Creat merge request for LED blink with beaeglV-fire. 

Milestone #8 (July 29th)
=========================

- Examine SPI support for beagleV-fire with robotic-cape.
- Create I2C device tree to test barometer on robotic cape.
- Creat merge request for I2C support. 
- Discuss results and features with mentor.

Milestone #9 (Aug 5th)
=======================

- Test all pre-work for librobotcontrol and robotic-cape with beaeglV-fire.
- Upgrade robotic_cape.dts file gateware for beaeglV-fire using pre-work.
- Creat Documentation and feeback from mentors.

Milestone #10 (Aug 12th)
========================

- Finalize the work on robotic-cape.dts for beaeglV-fire and test example of librobotcontrol.
- Creat documentation for current process.
- Fixing other bugs, typos, etc. found during documentation.

Final YouTube video (Aug 19th)
===============================

- Submit final project video, submit final work to GSoC site 
and complete final mentor evaluation.

Final Submission (Aug 24nd)
============================

.. important::

    **August 19 - 26 - 18:00 UTC:** Final week: GSoC contributors submit their final work 
    product and their final mentor evaluation (standard coding period)

    **August 26 - September 2 - 18:00 UTC:** Mentors submit final GSoC contributor 
    evaluations (standard coding period)

Initial results (September 3)
=============================

.. important:: 
    **September 3 - November 4:** GSoC contributors with extended timelines continue coding

    **November 4 - 18:00 UTC:** Final date for all GSoC contributors to submit their final work product and final evaluation

    **November 11 - 18:00 UTC:** Final date for mentors to submit evaluations for GSoC contributor projects with extended deadline

Experience and approch
***********************

Experience: 
    • I’m well experienced with embedded system and C . I’ve in-hand experienced with embedded programming and hardware design for various boards and projects.
    • Below are some projects which are about embedded system and robotics.
    1. `Martian rover used in  IRC (International rover challenge ) <https://github.com/vishwaspace>`_
        Martian rover is a prototype of curosity the nasa mars rover which performed funcion like soil testing, sample collection and monitoring planet.
        project required embedded hardware and firmware design for motor control and sensors configuration with ROS. 
    2. `STM32 custom board <https://github.com/Himanshukohale22/stm32-custom-board-v1.2>`_
        STM32 was custom boad which is made in purpose to learn embedded programming and hardware. it's a open source development project. 
    3. `Vaayu – AQI and various concentration calculation for gases present in air <https://github.com/Himanshukohale22/FYP_GreenSpace>`_ 
        VAAYU is air quility measument device which calibrate the different gases concentration and display with a GUI and TFT-display.
    4. `TVC rocketry – Thrust vector control <https://github.com/Himanshukohale22/CYRUS>`_
        TVC rocketry is project based learning model about Thrust vector control rockets, which based on PID implementations and senors configuration.
	
	More projects done by me can be found on my `github <https://github.com/Himanshukohale22>`_.
    • I’ve designed various double and four layer board for clients and projects  using  Kicad , Eagle and Altium designer `(Designs) <github/Himanshu/my_designs>`_. And this shows that I’ve very good understanding for reading schematics and Circuit design for embedded development, which is required for This project. 

Approach:

In my experience, projects often demand a comprehensive understanding of both software and hardware components Before changing the  main packages, Hardware setup and debug will required more time than software. This involves meticulous reading of documentation and references, demanding patience and focus. I believe that this content can be completed without any problems.

Contingency
===========

What will you do if you get stuck on your project and your mentor isn’t around?

Unexpected software and hardware problems are most common in any projects. In such cases,

1. In the event of encountering compatibility issues between BeagleBoard and librobotcontrol, I'll to use the BeagleBone Black (BBB) platform for testing purposes, as BBB offers native support for the librobotcontrol package.
2. If there is any hardware related issue to board,first ill review the datasheets and manule of hardware and if there is any issue related to circuitry I’ll use oscilloscope, multimeter and other testing devices for debugging.
3. If the problem is about SOC, I’ll check the datasheets of perticular SOC.
4. Here are a few references you can quickly glance at during debugging for guidance.
    - `librobotcontrol package Documentation <http://strawsondesign.com/docs/librobotcontrol/>`_
    - `librobotcontrol github <https://github.com/beagleboard/librobotcontrol>`_ 
    - `Getting started with beaglebone AI-64 <https://docs.beagleboard.org/latest/boards/beaglebone/ai-64/index.html>`_
    - `Getting started with beagleV-fire <https://docs.beagleboard.org/latest/boards/beaglev/fire/index.html>`_
    - Device tree: `github <https://github.com/Himanshukohale22/BeagleBoard-DeviceTrees>`_ , `example blog <https://www.beagleboard.org/blog/2022-02-15-using-device-tree-overlays-example-on-beaglebone-cape-add-on-boards>`_ , `FDT <https://devicetree-specification.readthedocs.io/en/stable/flattened-format.html>`_ , `ref <https://elinux.org/Device_Tree_Reference>`_ `tutorial <https://octavosystems.com/app_notes/osd335x-design-tutorial/osd335x-lesson-2-minimal-linux-boot/linux-device-tree-overlay/>`_
    - `Cape interface docs <https://elinux.org/Beagleboard:BeagleBone_cape_interface_spec#cite_note-2>`_
    - `TDA4VM device tree <https://software-dl.ti.com/jacinto7/esd/processor-sdk-linux-sk-tda4vm/09_01_00/exports/docs/linux/Foundational_Components_Kernel_Users_Guide.html>`_
    - `Validatin scripts for understand device tree <https://github.com/jadonk/validation-scripts>`_ 

Benefit
========

If successfully completed, what will its impact be on the `BeagleBoard.org <https://www.beagleboard.org/>`_ community? Include quotes from `BeagleBoard.org <https://www.beagleboard.org/>`_.
community members who can be found on our `Discord <https://bbb.io/gsocchat>`_ and `BeagleBoard.org forum <https://bbb.io/gsocml/13>`_.
 
* Librobotcontrol packages will support the beaglebone-AI, beaglebone-AI 64 and BeagleV-fire. 
* Various tutorials, Documentation will be added to the Robotic Capes to help the user understand how to use it using llibrobotcotrol packages.

Misc
====

Please complete the requirements listed in the `General Requirements <https://gsoc.beagleboard.io/guides/contributor#general-requirements>`_ . Provide link to merge request.

- All prerequisite tasks have been completed.
 * Source dive for Librobotcontrol packages and read all the documentation for packages
 * Check hardware specification, setup and device trees for BBB.
 * Here the 'Hello world' task Pull request : `merge request <https://github.com/jadonk/gsoc-application/pull/191>`_


Suggestions
===========

