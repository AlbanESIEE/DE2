# Lab 1 : FERRACANI Alban

### Morse code

**1. Listing of C code which repeats one "dot" and one "comma" *(here i use a dash, maybe it's simpler to visualize)* (BTW, in Morse code it is letter `A`) on a LED. Always use syntax highlighting, meaningful comments, and follow C guidelines:**

In this part of code, I defined the constants (delays) which are used after to emit the morse code. 

>*We have to note that in comparaison with the original template you have given us, I added ```LONG_DELAY```constant, I could multiply 
the ```SHORT_DELAY```constant but I thought it's simpler to adjust by adding this constant.* 

```c
//////////////////////////////////////////////////////////////////
////////// DECLARATION OF CONSTANTS FOR MORSE CODE ///////////////
//////////////////////////////////////////////////////////////////
/*
In this part, we define the delay in morse code to express one dot and 
one dash. 
--> Short delay is the duration of a dot
--> Long delay is the duration of a dash
*/
#define SHORT_DELAY 200 // Delay in milliseconds for a dot (.)
#define LONG_DELAY 700 // Delay in milliseconds for a dash (__)

/*
In this part, we define the delay in morse code to express one letter.
*/
#define SPACE_SAME_LETTER 2000
//////////////////////////////////////////////////////////////////
```

In this part of the code, I created a ```main``` function (execution in loop) and a ```blink``` function which is called in the ```blink``` function.

```c
/* Blink function ----------------------------------------------*/
/**********************************************************************
 * Function: Function with a string in parameter : "SHORT" or "LONG"
 * --> This function permit to blink the led (ON, delay, OFF, delay) defined 
 *     previously with SHORT_DELAY and LONG_DELAY. 
 * Purpose:  Toggle one LED and use delay library.
 * Returns:  none (void)
 **********************************************************************/

void blink (String delay_short_long){
    /*
    If the parameter sent is "SHORT", then we use a short delay which corresponds 
    to a dot. We make the led blink only once per function "blink" call.
    */
    if (delay_short_long == "SHORT"){
        digitalWrite(LED_RED, HIGH);
        delay(SHORT_DELAY);
        digitalWrite(LED_RED, LOW);
        delay(SHORT_DELAY);
    }
    /*
    If the parameter sent is "LONG", then we use a long delay which corresponds 
    to a dash. We make the led blink only once per function "blink" call.
    */
    if (delay_short_long == "LONG"){
        digitalWrite(LED_RED, HIGH);
        delay(LONG_DELAY);
        digitalWrite(LED_RED, LOW);
        delay(LONG_DELAY);
    }
}

/* Function definitions ----------------------------------------------*/
/**********************************************************************
 * Function: Main function where the program execution begins
 * Purpose:  Toggle one LED and use delay library.
 * Returns:  none
 **********************************************************************/


int main(void)
{
    uint8_t led_value = LOW;  // Local variable to keep LED status

    // Set pin where on-board LED is connected as output
    pinMode(LED_GREEN, OUTPUT);
    pinMode(LED_RED, OUTPUT);

    // Infinite loop
    while (1)
    {   
        /*
        In this part, we generate the sequence 'DE2' : (.__  .  ..__ __ __). 
        * Beetween two leters, we use the SPACE_SAME_LETTER delay. 
        * To generate a brief flash (dot .), we call the blink() function with "SHORT" parameter.
        * To generate a long flash (dash __), we call the blink() function with "LONG" parameter.
        */

        //Generating the letter 'D' (.__)
        blink("SHORT");
        blink("LONG");
        delay(SPACE_SAME_LETTER);

        //Generating the letter 'E' (.)
        blink("SHORT");
        delay(SPACE_SAME_LETTER);

        //Generating the letter '2' (..__ __ __)
        blink("SHORT");
        blink("SHORT");
        blink("LONG");
        blink("LONG");
        blink("LONG");

        //Then, we wait 5 seconds before repeating the morse sequence 'DE2' (to simplify visual interpretation of the morse code)
        delay(5000);  
    }

    // Will never reach this
    return 0;
}
```

**2. Scheme of Morse code application, i.e. connection of AVR device, LED, resistor, and supply voltage. The image can be drawn on a computer or by hand. Always name all components and their values!**

>On this scheme I designed on [tinkercad](https://www.tinkercad.com/things/2sn0hOOQTmw-start-simulating/editel?lessonid=EHD2303J3YPUS5Z&projectid=OT2JZ1PL20FZRMO&collectionid=undefined&sharecode=D2L9wMkQyfJz_Y5SIL3fVYiO3MtQYJY7FO_ZcHH1aUc), I represented the Arduino Uno developement board which is powered by the USB connection. Moreover, we have to connect an external LED (we could use the embedded LED of the Arduino, but the brightness is lower than an external one, so I decided to use it with an active high configuration (as on [TOMÁŠ FRÝZA diagram](https://github.com/tomas-fryza/digital-electronics-2/raw/master/labs/01-tools/images/gpio_high_low.png)). Finally, the last component to use is a resistor to lower the voltage across the LED. *As a reminder, the output voltage of the pins of the arduino is 5V in the ```HIGH``` state, we must reduce this voltage to about 2.2V, it may vary slightly depending on the type of LED used.* 

 <img width="1043" alt="lab01- thinkercad" src="https://user-images.githubusercontent.com/114081879/191607304-b43a0346-bf4c-40c1-9286-e7a3f5e7a8c2.png">

>I did this second representation on EasyEda software, here's a electrical diagram of the morse emitter. We have to note that it's not the arduino board which is represented here but just the ATMEGA328P chip. I didn't represent oscillator 16MHz, capacitors and input voltage regulators to simplify the diagram. 

<img width="600" alt="schema_morse_easy_eda" src="https://user-images.githubusercontent.com/114081879/191756032-b32538d8-1de0-44f5-8964-157a56951e32.png">


### Additional comments
1. In this part, you will find the simulation that I did on
[tinkercad](https://www.tinkercad.com/things/2sn0hOOQTmw-start-simulating/editel?lessonid=EHD2303J3YPUS5Z&projectid=OT2JZ1PL20FZRMO&collectionid=undefined&sharecode=D2L9wMkQyfJz_Y5SIL3fVYiO3MtQYJY7FO_ZcHH1aUc) (there is also a video, you can see below). 
>We can see that se morse code **(.__    .    ..__ __ __)** is correctly running on the simulation. Moreover, the thinkercad IDE is based on the Arduino IDE, so I changed the ```while(1)```into ```void loop()```and moved the ```pinMode```into the ```void setup()```function. 
>I already knew the Autocad suite so that's why I used tinkercad tool to simulate the morse code. But next time I will use *simulide*.

https://user-images.githubusercontent.com/114081879/191607787-015a3b2f-c630-4eba-a618-275412de9f4e.mov



2. Here is the morse "alphabet" I extracted from [this webpage](https://en.wikipedia.org/wiki/Morse_code) and I used to generate the ```DE2```word.

<img width="300" alt = "morse" src="https://user-images.githubusercontent.com/114081879/191604578-2fcbe14d-88e2-4801-b795-4e4eabc882fb.png">

