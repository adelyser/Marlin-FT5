## Folgertech FT-5 Firmware for MKS Gen v1.4

Marlin 2.0.7.1 base

**FT-5 stock configuration changes:**
For a bone stock FT-5, either R1 or R2, these changs from the base Marlin repository are needed.

In file *Configuration.h*:

    #ifndef MOTHERBOARD
       #define MOTHERBOARD BOARD_MKS_GEN_13
    #endif
    
   Substitue R2 here if it applies or you desire
   
    #define CUSTOM_MACHINE_NAME "FT-5 R1"
Enable the bed temperature sensor

     #define TEMP_SENSOR_BED 1

Set default extruder PIDS. It's a good idea to run a PID calibration after updating the firmware. You can use your values from the calibration to set as the default for your next firmware build.

    // FT-5 R1 Extruder PID
    #define DEFAULT_Kp 17.6
    #define DEFAULT_Ki 1.35
    #define DEFAULT_Kd 57.32

Set the type of endstop switches used on the FT-5

    #define X_MIN_ENDSTOP_INVERTING true // Set to true to invert the logic of the endstop.
    #define Y_MIN_ENDSTOP_INVERTING true // Set to true to invert the logic of the endstop.
    #define Z_MIN_ENDSTOP_INVERTING true // Set to true to invert the logic of the endstop.
    #define X_MAX_ENDSTOP_INVERTING true // Set to true to invert the logic of the endstop.
    #define Y_MAX_ENDSTOP_INVERTING true // Set to true to invert the logic of the endstop.
    #define Z_MAX_ENDSTOP_INVERTING true // Set to true to invert the logic of the endstop.

Define the stepper motor steps. The values are in order: X, Y, Z, Extruder

    #define DEFAULT_AXIS_STEPS_PER_UNIT   { 80, 80, 400, 105 }

Describe the accelerations of the printer axis.

    #define DEFAULT_MAX_ACCELERATION      { 1000, 1000, 200, 10000 }
    
    #define DEFAULT_ACCELERATION          1000    // X, Y, Z and E acceleration for printing moves
    #define DEFAULT_RETRACT_ACCELERATION  2000    // E acceleration for retracts
    #define DEFAULT_TRAVEL_ACCELERATION   1000    // X, Y, Z acceleration for travel (non printing) moves
    
       #define DEFAULT_XJERK 15.0
       #define DEFAULT_YJERK 15.0
       #define DEFAULT_ZJERK  0.4

Disable the Z probe since we use an end stop switch in stock configuration

    //#define Z_MIN_PROBE_USES_Z_MIN_ENDSTOP_PIN

Invert the Z direction. You may find other axis are wired backward and require reversing the direction as well.

    #define INVERT_Z_DIR true

Set Bed size and build height

    #define X_BED_SIZE 300
    #define Y_BED_SIZE 300
    
    #define Z_MAX_POS 380

Enable SD card support

    #define SDSUPPORT
    
Define the 2004 2-line LCD

    #define REPRAP_DISCOUNT_SMART_CONTROLLER
    #define ST7920_DELAY_1 DELAY_NS (0)
    #define ST7920_DELAY_2 DELAY_NS (450)
    #define ST7920_DELAY_3 DELAY_NS (0)

Allow saving parameters to the EEPROM. This allows you to use custom set parameters such as PID values and Axis step values set from the display.

    #define EEPROM_SETTINGS     // Persistent storage with M500 and M501


**Nice-to-have Options**

In file *Configuration.h*:
Set your personal name in the firmware.

    #define STRING_CONFIG_H_AUTHOR "(Your Name here, Folgertech FT-5)"

Enable PID auto-tuning. This allows the firmware to determine the best settings for heating and keeping the extruder heater stable.

    #define PID_AUTOTUNE_MENU

#define S_CURVE_ACCELERATION

This adds the bed leveling option into the LCD menu for manually leveling the bed.

    #define MESH_BED_LEVELING
    
    #define LCD_BED_LEVELING
    
    #define LEVEL_BED_CORNERS
    #define LEVEL_CORNERS_INSET_LFRB { 50, 50, 50, 50 } // (mm) Left, Front, Right, Back insets

Set the default pre-heat values that I use. Modify these for your personal usage.

    // Preheat Constants
    #define PREHEAT_1_LABEL       "PLA"
    #define PREHEAT_1_TEMP_HOTEND 215
    #define PREHEAT_1_TEMP_BED     60
    #define PREHEAT_1_FAN_SPEED     0 // Value from 0 to 255
    
    #define PREHEAT_2_LABEL       "PETG"
    #define PREHEAT_2_TEMP_HOTEND 245
    #define PREHEAT_2_TEMP_BED    85
    #define PREHEAT_2_FAN_SPEED     0 // Value from 0 to 255

I prefer the Prusa style of screen

    #define LCD_INFO_SCREEN_STYLE 1

This allows you to home a single axis at a time.

    #define INDIVIDUAL_AXIS_HOMING_MENU

In file *Configuration_adv.h*:
#define ADAPTIVE_STEP_SMOOTHING

#define STATUS_MESSAGE_SCROLLING

This times out the LCD menu and returns you to the main screen after the timeout. Certain menus will not timeout, such as the Bed Leveling menu.

    #define LCD_TIMEOUT_TO_STATUS 15000

#define LONG_FILENAME_HOST_SUPPORT

#define SCROLL_LONG_FILENAMES

**Change home from Back Right to Front Left:**
I found it frustrating that all my prints would come out backwards compared to other Cartesian printers I've used. I decided to fix this to make it similar.
In order to accomplish this, you must move the endstop plugs for the X and Y axis to the MAX connectors on the board.
Then in the code, make the following changes:

    //#define USE_XMIN_PLUG
    //#define USE_YMIN_PLUG
    
    #define USE_XMAX_PLUG
    #define USE_YMAX_PLUG
    
    #define X_HOME_DIR 1
    #define Y_HOME_DIR 1

**Change to Estimated Time Remaining on LCD:**
In file *Configuration_adv.h*:

    #define LCD_SET_PROGRESS_MANUALLY
    
    #if EITHER(SDSUPPORT, LCD_SET_PROGRESS_MANUALLY) && ANY(HAS_MARLINUI_U8GLIB, HAS_MARLINUI_HD44780, IS_TFTGLCD_PANEL)
      #define SHOW_REMAINING_TIME       // Display estimated time to completion
      #if ENABLED(SHOW_REMAINING_TIME)
        #define USE_M73_REMAINING_TIME  // Use remaining time from M73 command instead of estimation
        //#define ROTATE_PROGRESS_DISPLAY // Display (P)rogress, (E)lapsed, and (R)emaining time
      #endif
