.. _gsoc-proposal-Himanshu Kohale:

Librobotcontrol support for newer boards
########################################

Introduction
*************

Introducing librobotcontrol package support with newer boards.

Summary links
=============

- **Contributor:** `Himanshu Kohale <https://forum.beagleboard.org/u/ayush1325>`_
- **Mentors:** `Jason Kridner <https://forum.beagleboard.org/u/jkridner>`_, `Deepak Khatri <https://forum.beagleboard.org/u/lorforlinux/summary>`_
- **Code:**  TBD
- **Documentation:**  TBD
- **GSoC:** NA

Status
=======

This project is currently just a proposal.

Proposal
========

Completed all the requirements listed on the `ideas page <https://gsoc.beagleboard.io/ideas/>`_

* Created accounts on `openbeagle <https://openbeagle.org/Himanshuk>`_ , `forum <https://forum.beagleboard.org/u/himanshuk/summary>`_ , `Discord <https://discord.com/users/869908108565168198>`_ 
* Source dive and get to know with packages and examples.
* The code for the cross-compilation task can be found, Submitted through the pull request :- `pull request <https://github.com/jadonk/gsoc-application/pull/191>`_ 
* Proposal :- `librobotcontrol support for newer boards <https://gsoc-beagleboard-io-himanshuk-afa51c0f037cce3ef5f7bf31158de2bf3.beagleboard.io/proposals/himanshuk.html>`_ 

About 
=====

- **Forum:** :fab:`discourse` `Himanshuk (Himanshu Kohale) <https://forum.beagleboard.org/u/himanshuk/summary>`_
- **OpenBeagle:** :fab:`gitlab` `Himanshuk (Himanshu Kohale) <https://openbeagle.org/Himanshuk>`_
- **Github:** :fab:`github` `Himanshukohale22 (Himanshu Kohale) <https://github.com/Himanshukohale22>`_
- **School:** :fas:`school` `Veermata Jijabai Technological Institute <https://vjti.ac.in/>`_
- **Country:** :fas:`flag` India
- **Primary language:** :fas:`language` English, Hindi, Marathi
- **Typical work hours:** :fas:`clock` 8AM-5PM Indian standard timeline
- **Previous GSoC participation:** :fab:`google` N/A

Project
********

**Project name:** librobotcontrol support for newer boards .

Description
============

Librobotcontrol is package of C library which contains examples and testing programs for Robotic control projects used by `robotic-cape <https://www.beagleboard.org/boards/beaglebone-robotics-cape>`_  which is sold by beagleboard.org.
BeaglBboneBlack(BBB) supports the librobotcontrol package thanks to Deepak khatri, Who Previouly worked upon cape compatibility layer on BBB.
BeaglBboneBlack support robotic-cape cape with librobotcontrol package due to creation of device tree ovelayes which identify the robotics cape and symlink help to address the location of specific peripherals in data structure.
as below is simple dts example for accesing GPIO's for blink LED's.
since P9_12 supports the GPIO's functinoality as output signal. 

.. code-block::

    /{
        compatible = "ti,BeaglBbone-Black";
        part-number = "DM-GPIO-TEST";
        version = "00A0";
        fragment@0{
            pinctrl-singl.pins = <
                0X078 0x07 /* connected LED, resister address and offset */ 
            
            >
        
        } 
    }

    
   

Main goal of project is to update librobotcontrol package for beaglebone-AI(am5x), beaglebone-AI64(j721e) and beaeglV-fire(olarV-soc) boards.
Primarily librobotcontrol support all the boards but not able to make roboitic-cape as flexible as BBB board for librobotcontrol package.

BeagleBone-AI support librobotcontrol package but its been draft. and there is not a stable device tree overlays for robotic cape in AI image.
So for using package, have to check the passed results for various drivers and use cape with AI, and make changes accordingly.
BeagleBone AI is based on the Texas Instruments AM5729 dual-core Cortex-A15 SoC with flexible BeagleBone Black header and mechanical compatibility.
which supports the AM572x device tree binary files. this .dtb file will include in binary file (.dts) files in AI which will help to call root nodes to create device tree for cape.
but as AI-support the cape, there is only need to cross verify the package and update accordingly.

**BeagleBone-AI64**

BeagleBone AI-64 uses TI J721E-family TDA4VM system-on-chip (SoC) which is part of the K3 Multicore SoC architecture.
Each TI evm has an unique device tree binary file required by the kernel. As BeaglBboneBlack need (ti,am335x) similarly beaglebone-AI64 support (ti,j721e) device tree binary (.dtb).
BeagleBone AI-64 also support librobotcontrol packages but there are less tutorial and not refine code for support librobotcontrol package with new boards. Need to refine the device trees overlays to use this librobotcontrol package with AI-64.
As in librobotcontrol there is no robotic-cape dtb support for beaglebone-AI64 we need to write the device tree ovelays with (ti,j721e) binary file (.dtb).

How it will be possible, see an example for blink LEDs via device tree ovelay which is for GPIO's:

.. code-block:: 

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




Implementation of device tree :

.. image :: https://devicetree-specification.readthedocs.io/en/stable/_images/graphviz-58c8267ade85edeca7b1b0299af2b1e473987ddc.png
    :alt: Device tree Implementation
    :align: center

**BeagleV-Fire** 

BeagleV®-Fire is a revolutionary SBC powered by the Microchip’s PolarFire® MPFS025T RISC-V System on Chip (SoC) with FPGA fabric.
It has the same P8 & P9 cape header pins as BeagleBone Black allowing to stack BeagleBone cape on top to expand it’s capability. Built around the powerful and energy-efficient RISC-V instruction set architecture (ISA) along with its versatile FPGA fabric.
BeagleV-Fire also support the robotic cape with librobotcontrol package and cape gateware for robotic cape is preinstalled in V-fire image `here <https://openbeagle.org/himanshuk/gateware/-/tree/main/sources/FPGA-design/script_support/components/CAPE/ROBOTICS?ref_type=heads>`_ .
but as BBB support the librobotcontrol with more functionality and flexible application, beagleV-fire is not capable for librobotcontrol package due to less number of device_tree fragment present in robotic-cape (.dts) file which is pre-installed in V-fire image.
With help of customization for cape gateware in V-fire which Provide more flexibility to board with cape, librobotcontrol package will support V-fire with robotic cape.

V-fire Gateware architecture:

.. image :: https://docs.beagleboard.org/latest/_images/Gateware-Flow-simplified-overview.png
    :alt: V-fire Gateware architecture
    :align: center

In `robotics_cape.dts <https://openbeagle.org/himanshuk/gateware/-/blob/main/sources/FPGA-design/script_support/components/CAPE/ROBOTICS/device-tree-overlay/robotics-cape.dtso?ref_type=heads>`_ file in cape gateware of beagleV-fire are used PWM and GPIO's, we need to write device tree sript for I2C, SPI and UART and also add simlink which will need to support librobotcontrol package.



Previous work:-

Previously Deepak khatri who worked upon the cape compatibility for beagleboards. use the robotic cape for various tasks. 
using pre-work upon robotic cape, i can take a deep dive to robotic cape compatibility with BeaglBboneBlack (BBB) and how its works.
In previous gsocted with one of Bea-2022 participation kai yamada work upon same project which was about robotic-cape support with BeagleBone-AI (BB-AI).
In both projects implementation was about the device tree overlayes for BBB and AI for specific pheripherals to enabling functionality of PWM, I2C and SPI and UART for robotic-cape.  


Software
=========

- Device tree's overlays for beagleboards will be used.The project requires the use of the device tree compiler (dtc) for compiling the device tree source (ex. *.dts, *.dtsi) files.
- Primarily VScode and gitlab with web-IDE is use in this project for deep dive into code and firmware of librobotcontrol and rc (robot control library) examples.
- C language.

Hardware
========

A list of hardware that you are going to use for this project.

- `Beaglebone Black <https://www.digikey.in/en/products/detail/beagleboard-by-seeed-studio/102110420/12719590?cur=INR&lang=en&utm_adgroup=&utm_source=google&utm_medium=cpc&utm_campaign=PMax%20Shopping_Product_High%20ROAS&utm_term=&productid=12719590&utm_content=&utm_id=go_cmp-20122528480_adg-_ad-__dev-c_ext-_prd-12719590_sig-Cj0KCQjw8J6wBhDXARIsAPo7QA8aIQNqlJuRD5bNfrHXhCPfGk6LSU2nxmVaauLzHgc6BreuyUqskmEaAsJoEALw_wcB&gad_source=1&gclid=Cj0KCQjw8J6wBhDXARIsAPo7QA8aIQNqlJuRD5bNfrHXhCPfGk6LSU2nxmVaauLzHgc6BreuyUqskmEaAsJoEALw_wcB>`_
- `BeagleBone-AI <https://www.digikey.in/en/products/detail/seeed-technology-co-ltd/102110362/10492208>`_
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
- Use robotic-cape with beagleboard BeaglBboneBlack (BBB) and librobotcontrol.
- Use robotic-cape with BeagleBone-AI.

Coding begins (May 27th)
=========================

- Understand device tree overlays for BeaglBboneBlack (BBB) and AI written for robotic cape. 
- Start to write Device tree for GPIO's and PWM support for AI-64. 

Milestone #1, Introductory YouTube video (June 3rd)
===================================================

- Include introductory video.
- Prepare documentation for the process. 


Milestone #2 (June 10th)
==========================

- For RoboticsCape, Test a device tree overlay to allow AI-64 to light the power LEDs with GPIO support.
- Test PWM Device tree overlay with robotics-cape with help of Hardware specification and check with oscilloscope.
- Get feeback from mentors.


Milestone #3 (June 17th)
=========================

- Write I2C device tree for AI-64.
- Test I2C with IMU and robotic-cape.
- Get feedback from mentor.

Milestone #4 (June 24th)
==========================

- Create merge request for I2C Device tree overlays.
- Write SPI device tree overlay for AI-64.
- Test with robotic-cape.
- Get feedback from mentor.

Milestone #5 (July 1st)
========================

- Create RoboticsCape.dts file for robotic-cape which will support AI-64 using pre-work.
- Test .dts file with robotic cape with AI-64.
- Test example of librobotcontrol with AI-64.
- get feedback from mentor.
- Creat merge request for RoboticsCape.dts.

Submit midterm evaluations (July 8th)
=====================================

.. important:: 
    
    **July 12 - 18:00 UTC:** Midterm evaluation deadline (standard coding period) 

Milestone #6 (July 15th)
=========================

- Test RoboticsCape with cape gateware for beagleV-fire pre-installed in image.
- Understand the customization process for cape Gateware. 

Milestone #7 (July 22nd)
=========================

- Customized LED example for robotic-cape gateware.
- Test GPIO's, Robotic cape with beaglV-fire.
- Create merge request for LED blink with beaglV-fire. 

Milestone #8 (July 29th)
=========================

- Examine SPI support for beagleV-fire with robotic-cape.
- Create I2C device tree to test barometer on robotic-cape.
- Create merge request for I2C support. 
- Discuss results and features with mentor.

Milestone #9 (Aug 5th)
=======================

- Test all pre-work for librobotcontrol and robotic-cape with beaeglV-fire.
- Upgrade robotic_cape.dts file gateware for beaeglV-fire using pre-work.
- Create Documentation and feeback from mentors.

Milestone #10 (Aug 12th)
========================

- Finalize the work on robotic-cape.dts for beaeglV-fire and test examples of librobotcontrol.
- Create documentation for current process.
- Fixing other bugs, typos, etc. found during documentation.

Final YouTube video (Aug 19th)
===============================

- Submit final project video, submit final work to GSoC site 
and complete final mentor evaluation.

Final Submission (Aug 24th)
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
    • I’m well experienced with Embedded System and C. I’ve in-hand experienced with Embedded programming and Hardware design for various boards and projects.
    • Here are my projects which demonstrate my proficiency in Embedded system and Robotics.
    1. `Martian rover used in  IRC (International rover challenge ) <https://github.com/vishwaspace>`_
        Martian rover is a prototype of curosity the nasa mars rover which performed function like soil testing, sample collection and monitoring planet.
        Project required Embedded hardware and firmware design for motor control, arm control and science sensor's configuration with ROS. 
    2. `STM32 custom board <https://github.com/Himanshukohale22/stm32-custom-board-v1.2>`_
        STM32 was custom boad which is made in purpose to learn Embedded programming and hardware design. it's a open source development project. 
    3. `Vaayu – AQI and various concentration calculation for gases present in air <https://github.com/Himanshukohale22/FYP_GreenSpace>`_ 
        VAAYU is air quality monitoring system device which calibrate the different gases concentration and display with a GUI and TFT-display.
    4. `TVC rocketry – Thrust vector control <https://github.com/Himanshukohale22/CYRUS>`_
        TVC rocketry is learning based model project about Thrust vector control rockets, which based on PID implementation and sensors configuration.
	
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
 * Here the 'Hello world' cross-compilation task Pull request : `merge request <https://github.com/jadonk/gsoc-application/pull/191>`_


Suggestions
===========

