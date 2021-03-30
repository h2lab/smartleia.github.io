.. _tools:

Demonstration and educational tools
-----------------------------------

Some tools have been published by H2Lab in order to demonstrate some ways to use the Leia board.

Smartleia target applet
^^^^^^^^^^^^^^^^^^^^^^^

Smartleia target applet for testing APDUs and cryptography that can be found on the following repository: https://github.com/h2lab/smartleia-target-applet.git

Purpose of the applet
"""""""""""""""""""""

The purpose of the applet is to provide:

   * Tests for APDU cases 1, 2, 3 and 4 (for short and extended APDUs), as well as time extensions. Please refer to the ISO7816-3 standard for more information on this.
   * Tests for AES computations using the Javacard API.

The details of the APDU commands and expected responses are provided in the cmd.txt file, where the opensc tool is used as a command line way of cimmunicating with the smart card.

Compilation
"""""""""""

In order to compile the project, you will need java with JDK of version 8 or 11, this is a strong requirement from ant-javacard as reminded here. You can fetch OpenJDK versions from AdoptOpenJDK.

The 3.0.3 Javacard SDK (jc303_kit) (Javacard API 3.0.1) is also needed, and must be downloaded and put in the sdk folder. You can find Javacard SDKs for example here. Just drop the jc303_kit folder in it as explained in sdks/README.txt.

When this is done, you can compile the applet using a simple:

.. code:: bash

    $ make

Pushing the applet
""""""""""""""""""

Pushing the applet uses the gp.jar tool, and can be done, having the LEIA-Solo connected, using:

.. code:: bash

    $ make push



Funcard demo firmware
^^^^^^^^^^^^^^^^^^^^^

A Funcard weak cryptographic implementation is proposed on the following repository: https://github.com/h2lab/smartleia-target-funcard

This software handle an unprotected AES128 encryption, as well as a dummy PIN code verification algorithm.
The implementations have been tested on a WB Electronics 64 Kbit ATMega chipcard.

Compilation
"""""""""""

Go to the src/ folder and run make. You must have avr-gcc, e.g. from avr-gcc, and the avr-libc installed on your PC: these are usually packaged with popular distros such as Debian or Ubuntu. Make should create aes-<DDMMYY>-<HHMMSS>.hex and eedata-<DDMMYY>-<HHMMSS>.hex in the src/build/ folder.

Loading in the ATMega8515 card
""""""""""""""""""""""""""""""

Load the files eedata.hex (in EEPROM int.) and aes-<DDMMYY>-<HHMMSS>.hex (in FLASH) in the ATMega8515 component. You can for instance use the Infinity USB Unlimited Reader and IDE from WB Electronics for this step.

If you have a recent LEIA board connected with the flashing mode feature, you can simply execute the local flashing script:

.. code:: bash

    sh flash_funcard.sh

This will compile and push the firmware on the funcard inserted in your LEIA board.

Using the testing scripts
"""""""""""""""""""""""""

Two test scripts are provided here: ``script-AES128-enc.py`` and ``pin_timing_attacks.py``.

The testing scripts are mainly Python based, and have been tested with Python3. The dependency requirements for these scripts are:

    The smartleia package in its version v1.0.1-1 at least (this contains a small fix for funcards usage through PCSC relay), available here.
    The pyscard, numpy and crypto packages, all available with pip.

Each of these scripts can be used in two modes: using LEIA's direct access through /dev/ttyACMx with the toggle USE_LEIA=True as an environment variable, or using PCSC daemon either through a regular smart card reader (or LEIA in PCSC relay mode) by using the toggle USE_LEIA=False as an environment variable:

.. code:: bash

    $ USE_LEIA=False python3 pin_timing_attacks.py
    [+] Using PCSC reader
    ...
    $ USE_LEIA=True python3 pin_timing_attacks.py
    [+] Using LEIA raw access
    ...

The ``script-AES128-enc.py`` tests AES-128 encryption and decryption APDUs: this can be a basis to mount some side-channel attacks on an unprotected AES (NOTE: although some APDUs setting masks are present, these are not used and are here for future evolutions).

The ``pin_timing_attacks.py`` extracts a secret PIN from the programmed funcard using a timing attack that exploits the dummy algorithm used to check the PIN. In order for this attack to succeed, a timing oracle is needed. Since such a timing oracle exploits variations of less than milliseconds, a proper time measurement for APDUs is necessary. This script shows that LEIA's timing feature can be of use here: a regular smart card reader is not able to extract the secret (at least with the basic approach used using LEIA). You can test LEIA's timing extraction with the USE_LEIA=True, and PCSC based (using a regular reader or LEIA in PCSC mode) using the USE_LEIA=False toggle. The first one should extract the secret PIN successfully, while the second will not succeed.
