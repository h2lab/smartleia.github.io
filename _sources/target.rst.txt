.. _target:


What is LEIA-Solo?
------------------

LEIA-Solo is an open hardware and open source device targeting smart card side-channel analysis (SCA) and evaluation, for educational and evaluation purpose. It implements a fully controlled ISO7816 stack with a dedicated custom hardware platform to acquire clean measurements for SCA characterization.

The LEIA-Solo board is made of two main parts: a STM32 MCU that contains the firmware handling the ISO7816-3 stack, and the ISO7816-3 connector that communicates with the target smart card (i.e. handling the physical connection) and is isolated with optocouplers for clean measurements. Having a fully controlled ISO7816 stack allows to position precise triggers at dedicated events (sending an APDU, receiving the response, etc.), which helps to get synchronized traces of smart card consumption activity (and then analyze and extract secrets using classical SCA techniques). Low-level access to the ISO7816-3 protocol also allows to explore interesting paths such as smart cards conformity checks and so on. With this versatility in mind, We have tried to make the implementation to cover as much as possible of the specification (T=0 and T=1, PTS negotiation, etc.).

A big advantage of the LEIA-Solo board is also its software ecosystem: it is compatible with the ChipWhisperer SDK, and the board can be driven from a PC using an UART TTL or an USB connection with high level and easy to use Python library and scripts.

Supported features
^^^^^^^^^^^^^^^^^^

The LEIA-Solo board is versatile and supports multiple features:

**Smartcard communication**


    * Hardware based ISO7816 stack supporting both ISO7816-3 T=0 and ISO7816-3 T=1
    * BitBanged ISO7816 stack allowing to fully control the communication
    * Timing measurement between ISO7816 transaction
    * PTS negotiation (flexible ETU selection)
    * ISO7816 clock frequency tuning

**Triggering**

    * Up to 4 trigger strategies, each one on 10 possible trigger events corresponding to classical ISO7816 elements (beginning of ATR, end of ATR, sending an APDU and receiving a response, etc.).
    * The triggers also have a configurable delay, and support a ‘single’ mode.
    * All the triggers states (number of observed triggers, etc.) can be recovered.
    * Dedicated trigger pin in SOLO mode (standalone mode)
    * ChipWhisperer triggering through the 20-pins standard interface

**Power analysis and glitching**

    * USB-powered mode with octocouplers for more efficient measurements
    * Direct power mode (external controled power supply not delivered)

**Smartcard reader mode**

    * Fully integrated with PC/SC, which allows to use LEIA-Solo as a classical smart card reader.

Open-Source and Open-Hardware
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

A big advantage of the LEIA-Solo board is also its software ecosystem: it is compatible with the NewAE Technology Inc. ChipWhisperer SDK,
and the board can be driven from a PC an USB connection with high level and easy to use Python library and scripts.

The LEIA-Solo project is fully Open-Source and Open-Hardware. All the software components aims to be published:

   * LEIA-Solo firmware
   * LEIA-Solo host-part tooling (python API, PC/SC backend support)
   * ChipWhisperer SDK add-on
   * Smart Card demonstration applet

