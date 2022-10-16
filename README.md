# TinyIRremote
https://github.com/ndroid/TinyIRremote

Adaptation of TinyIRReceiver from Arduino IRRemote library <a href="https://github.com/Arduino-IRremote/">https://github.com/Arduino-IRremote/</a>
Modified to work with MSP432 boards in Energia and add IR transmit.


*Define IR_FEEDBACK_LED_PIN to change default feedback LED pin

*Define DO_NOT_USE_FEEDBACK_LED to save 12 bytes and reduce ISR handling time

*By default, repeats are included and decodeIR() returns all commands.
Define EXCLUDE_REPEATS to ignore repeats in decodeIR() and return only new commands. 

*Define HANDLE_IR_EVENT in order to provide user-defined handler for IR event.
If enabled, must define 
  void handleReceivedTinyIRData(uint16_t aAddress, uint8_t aCommand, bool isRepetition)
This function is called if a complete command was received, regardless if it is 
a repeat (not impacted by EXCLUDE_REPEATS constant).
!!!!!!!!!!!!!!!!!!!!!!
  Function called in interrupt context - should be running as short as possible, so if you 
  require longer action, save the data (address + command) and handle it in the main loop.
!!!!!!!!!!!!!!!!!!!!!

*For IR receiver, IR input pin must be specified in call to initTinyIRReceiver() - returns true on success.
Any GPIO pin which supports interrupts may be used.
  bool initTinyIRReceiver(uint8_t aRcvPin, bool aEnableLEDFeedback = false, uint8_t aFeedbackLEDPin = USE_DEFAULT_FEEDBACK_LED_PIN)

*For IR transmitter, declare an IRsend object and initialize with 
  bool IRsend::begin(uint8_t aSendPin, bool aEnableLEDFeedback = false, uint8_t aFeedbackLEDPin = USE_DEFAULT_FEEDBACK_LED_PIN)

*Both IR receive and transmit use IRData struct, defined as
  struct {
    decode_type_t protocol;     ///< UNKNOWN, NEC, SONY, RC5, ...
    uint16_t address;           ///< Decoded address
    uint16_t command;           ///< Decoded command
    bool isRepeat;
  } 
