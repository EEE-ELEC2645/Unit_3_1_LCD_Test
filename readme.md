# ELEC2645 Unit 3 - LCD Test 

This lab demonstrates the capabilities of the ST7789V2 LCD display connected to the Nucleo-L476RG board. It showcases various graphics operations including text rendering, geometric shapes, sprites, and colour palettes. The program also outputs information to the serial console at 115200 baud.

The most important file is `main.c` which is found in [Core/Src/main.c](Core/Src/main.c). 

## The assignment

Get the code running and understand how each LCD function works. Observe the demonstrations on the screen and understand what each section is displaying. 


## Setup
Please follow the prelab setup on the Minerva page, but a brief reminder here:
- Install [ST Link drivers](https://www.st.com/en/development-tools/stsw-link009.html) (if using your own PC)
- Install the [STM32CubeIDE for Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=stmicroelectronics.stm32-vscode-extension) a bundle of 15 extensions
- Open the Unit_3_1_LCD_Test folder and in the bottom corner when asked "*Would you like to configure discovered CMake project as STM32Cube project*" say "**Yes**" you want to open this folder as an STM32 Project. Let the extension finish the setup
- If prompted choose the "Debug" configuration
- You can click "Build" at the bottom of the screen to check it compiles
- Check the board is connected under "STM32CUBE Devices and Boards" in the "Run and Debug" page. You can tell it to Blink, and if you have not yet done so, upgrade the firmware. 
- Then Click "Run and Debug" and select "STM32Cube: STLink GDB Server"
- The code will now run, and you can continue past the breakpoints by hitting F5 or clicking the green arrow
- (optional) add the [Serial Monitor](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-serial-monitor) extension as it is easier to use than the one bundled with the STM32CubeIDE

## LCD Hardware Setup

Make sure you have followed the instructions on the Minerva page to hook up the LCD screen to the Nucleo through the breadboard.

| LCD Pin | Signal  | Nucleo Pin | Purpose |
|---------|---------|-----------|---------|
| VDD     | Power (3.3V-5V) | VDD | Display power supply |
| GND     | Ground  | GND | Ground reference |
| MOSI    | Serial Data | PB15 | SPI Master Output Slave Input |
| SCK     | Clock   | PB13 | SPI Clock signal |
| CS      | Chip Select | PB12 | SPI Chip Select |
| DC      | Data/Command | PB11 | Command vs Data mode |
| BL      | Backlight | PB1 | Backlight control (PWM) |


## What the Program Displays

When you run the program, you will see a series of demonstrations on the LCD screen. The serial console will also print messages showing the progress through each section. Here's what to expect:

### 1. Welcome Message
The first screen sets a black background and then displays a welcome message with text "Welcome", "To", and "ELEC2645" appearing progressively with a 200ms delay between each line.

### 2. Shapes and Lines
This section demonstrates drawing geometric shapes:
- **Solid Rectangle**: A filled rectangle (50×30 pixels) in colour index 5
- **Rectangle Outline**: An unfilled rectangle (30×50 pixels) in colour index 4 showing just the border
- **Circles**: Two circles are drawn with different radii (10 and 30 pixels) in different colours (6 and 8)
- **Lines**: Two diagonal lines demonstrating the line drawing function in colours 7 and 14
The background is red for this section.

### 3. Text Rendering
This section shows text in various sizes using the built-in 5×7 pixel font:
- **Small text**: Font scale of 1 (5×7 pixels per character)
- **Medium text**: Font scale of 3 (15×21 pixels per character)
- **Large text**: Font scale of 5 (25×35 pixels per character)
- **Single character**: Demonstrates rendering a single character ('A')
The background for this section is green.

### 4. Sprites
This is the most visually interesting section demonstrating sprite rendering:
- **Simple Face Sprite**: A 10×10 pixel black smiley face drawn multiple times
- **Colour Override**: The same sprite rendered in different colours (3, 5, 6) without changing the sprite data
- **Scaled Sprites**: The face sprite drawn at scale factors of 1, 2, and 3
- **Larger Sprites**: Pre-defined sprites from `sprites.h` including a tree (40×40) and robot (42×27)
- **Animation**: A Stardew Valley cat animation loops 5 times with multiple frames, demonstrating how to create simple animations
The background is white for this section.

### 5. Colour Palettes
This section demonstrates 4 different colour palettes by cycling through all 16 colours in each palette:
- **Default Palette**: Standard colour mapping
- **Greyscale Palette**: Black, white, and grey tones
- **Vintage Palette**: Warm, muted colours based on vintage game consoles
- **Custom Palette**: User-defined colours
Each colour is displayed for 200ms with the colour index and palette name shown on screen.

## LCD Functions Used

The following key LCD functions are demonstrated in this program:

- `LCD_init()` - Initialize the LCD display
- `LCD_Fill_Buffer(colour)` - Fill the entire screen with a single colour
- `LCD_Refresh()` - Copy the buffer contents to the physical display
- `LCD_printString(text, x, y, colour, size)` - Draw text at specified position with size scaling
- `LCD_printChar(char, x, y, colour)` - Draw a single character
- `LCD_Draw_Rect(x, y, width, height, colour, filled)` - Draw a rectangle (filled or outline)
- `LCD_Draw_Circle(x, y, radius, colour, filled)` - Draw a circle
- `LCD_Draw_Line(x1, y1, x2, y2, colour)` - Draw a line between two points
- `LCD_Draw_Sprite(x, y, width, height, sprite_data)` - Draw a sprite with transparent pixels
- `LCD_Draw_Sprite_Colour(x, y, width, height, sprite_data, colour)` - Draw sprite with colour override
- `LCD_Draw_Sprite_Scaled(x, y, width, height, sprite_data, scale)` - Draw sprite with scaling
- `LCD_Draw_Sprite_Colour_Scaled()` - Draw sprite with both colour and scale
- `LCD_Set_Palette(palette)` - Switch between different colour palettes

## GPIO - General Purpose Input Output

These are the basic digital input and output functions described by the Hardware Abstraction Layer (HAL) for reading and writing values to pins. 

- `HAL_GPIO_WritePin(port, pin, state)` - Write a digital value (0 or 1) to a GPIO pin
- `HAL_GPIO_ReadPin(port, pin)` - Read the current state of a GPIO pin (returns `GPIO_PIN_SET` or `GPIO_PIN_RESET`)
- `HAL_GPIO_TogglePin(port, pin)` - Toggle a GPIO pin (if it was HIGH, set it LOW, and vice versa)


## Serial

Here we have redirected the printf function to the serial port, in the `_write` definition at the top of main.c. This way we can use `printf` as normal like in Unit 1&2 and it automatically sends it over serial. 



## Other HAL functions

- `HAL_Delay(ms)` - Delay execution for a specified number of milliseconds
- `HAL_UART_Transmit(huart, data, len, timeout)` - Send data over UART serial port (this is what printf uses internally via the `_write` function)
- `HAL_GetTick()` - Get the current system tick count (useful for timing and seeding random numbers)