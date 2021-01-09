## Experiment 10:

# A Hershey's Special Dark Chocolate Kisses Dispenser<sup>[1](#myfootnote1)</sup>

![](/arduino/img/kisses.png)

Today we are going to look at a servo. In particular, we are going to look at how a servo can deliver us chocolate.

To get in the mood (and provided you can tolerate heavy metal music) [Here is a link to my favorite song about chocolate](https://www.youtube.com/watch?v=WIKqgE4BwAY).

![](/arduino/img/heavyChoc.png)

And here are the [lyrics](https://genius.com/Babymetal-gimme-chocolate-english-translation-lyrics))

#### The Servo

![](/arduino/img/servo.png)

A servo is a motor that can turn a specific number of degrees. For example, we can tell a servo to move exactly 45 degrees and then change direction and move 10 degrees. There are many uses of servos including

- robotics - every joint of a robot might have a servo so an arm, for example, can move in a very precise way.
- cameras - many cameras have built in servos that enable them to do auto-focusing.
- satellite-tracking antennas and solar panel movement (both need to track something)
- the hard disk in your laptop (controls arm movement inside the disk)
- remote controlled hobby planes, cars, and boats.
- and many others

We are going to use a servo in its most important role: Delivering Hershey's Special Dark Kisses (or, alternatively, M&Ms)

### Let's get started

#### Hardware Hookup

![](/arduino/img/chocolate_bb.png)

[link to larger picture](/arduino/img/chocolate_bb.png)

Before you continue make sure to put an arm on the servo. It just makes it more fun!

### The Code

    #include <Servo.h>

    Servo myservo;
    int button = 5;
    int servoPin = 3;
    int pos = 0;        //variable to keep track of the servo's position
    int max_position = 360;

    void setup() {
      pinMode(button, INPUT_PULLUP);
      Serial.begin(115200);
      myservo.attach(servoPin);  //Initialize the servo
      myservo.write(180);        //set servo to furthest position
      delay(500);                //delay to give the servo time to move to its position
      myservo.detach();          //detach the servo to prevent it from jittering

    }

    void loop() {
        if(digitalRead(button) == LOW) //if a button press has been detected...
        {
          myservo.attach(servoPin);
          myservo.write(pos);
          delay(500);           //debounce and give servo time to move
          myservo.detach();
          Serial.println(pos);  //prints to the serial port to keep track of the position
          pos = (pos + 180) % max_position;
        }
    }

### What You Should See

When you first upload the code, give it a few seconds, then you should see the motor move to the 180° position. Once there, it will stay until the button is pressed. Press the button to move it to 0°. Press the button again and it will move back to the first position. The purpose of this code it to give you an idea for the full range of motion for the servo motor and to help you plan out your Hershey's Special Dark Kisses Dispenser.

### Libraries

Explaining the line

```
#include <Servo.h>
```

Instead of writing all of the code themselves, software developers rely on code written by others. For example, if I wrote some code for an Arduino to control a Roomba vacuum cleaner: `roomba.TurnLeft(45)`, `roomba.TurnRight(90)`, `roomba.Forward(10)`, I might share that code with others. This collection of shared procedures is called a **library**. Before you can use the procedures in a library you need load the library into your code. This is done with the include statement. So

```
#include <Servo.h>
```

instructs the Arduino IDE to include the Servo library and this allows us to use procedures like `attach`,`detach`, and `write`. The Arduino IDE comes with a set of standard libraries we can use, including the Servo library. We can also install and use libraries written by others. For example, people might download and install my Roomba library and include it in their code:

```
#include <Roomba.h>
```

### Modulo

Anyone who know me knows I love, love, love the **modulo operator**, **%** that we see in this line:

       pos = (pos + 180) % max_position;

The modulo operator gives us the reminder when we divide the first number by the second (think grade school math). So

    4 % 2

is `0` because there is no remainder when we divide 4 by 2.

    4 % 3

is 1 because when we divide 4 by 3 we have 1 left over and

    26 % 5

is also 1 because when we divide 26 by 5 we have a remainder of 1. Finally,

     5 % 25

is 5 because no 25s go into 5 so we have 5 left over.

So far so good.

So look at the code above and see what the values are of

     pos
     max_position

Take the time to actually look at the code while I count to 10

- 1
- 2
- 3
- 4
- 5
- 6
- 7
- 8
- 9
- 10

Okay, so

     pos = 0
     max_position = 360

One other thing I should mention. The maximum degrees our servo can turn is 180.

So our servo starts at 180 degrees.

```
myservo.write(180);        //set servo to furthest position
```

The first time we press the button we want it to move to 0. The second time we press the button we want it to move to 180. The third time 0, the fourth 180 and so on. `pos` is initially zero so

    myservo.write(pos);

is the same as

    myservo.write(0);

since `pos` equals 0 and the servo moves to position 0. Good so far.

Then we execute my favorite line.

     pos = (pos + 180) % 360;

So since pos is currently 0 this is the same as

     pos = (0 + 180) % 360;

so that is `180 % 360` which is 180. (180 divided by 360---360 goes into 180 zero times with 180 remainder)

The next time we press the button and execute:

    myservo.write(pos);

we go to position 180 and then execute our favorite:

     pos = (pos + 180) % 360;

Since our current value of pos is 180 that equals `(180 + 180) % 360` which equals 0. There is no remainder when we divide 360 by 360. So this line of our code makes the value of pos alternate between 0 and 180. Cool. Let's say we want our button presses to do the following

- first press - 0 degrees
- second press - 45 degrees
- third press - 90 degrees
- fourth press - 135 degrees
- fifth press - 180 degrees
- sixth press - back to 0 and repeat pattern

So pos is initially zero. Each time we press we don't want

    pos = (pos + 180) % 360;

We are going to have to change that 180 and, especially important we need to change 360 because the servo can only move to 180 max.

### Remix 1: The Super Servo Challenge.

Can you implement this?

If you do this (and understand it) you get the **Modulo Merit Badge**

### Remix 2: Hershey's Special Dark Kisses Dispenser

For this you will need two things:

- a supply of **Hershey's Special Dark Kisses** (the official sponsor of this lab) or suitable substitute (M&Ms, peanuts, etc.)
- a drink cap which you screw onto the arm of your servo.

![](/arduino/img/cap00.png)

<sub>photos by Sparkfun</sub>

Now load the original button code (the one that goes from 0 to 180).
You will need to attach the cap arm assembly to the servo so that in the initial position the cap is upright and can hold the chocolate, but when you press the button it dispenses it. You will need to alter the code so that when the button is pressed and the chocolate is delivered it, automatically resets back to the initial upright position.

![](/arduino/img/cap1.png)
![](/arduino/img/cap2.png)

You may want to put the servo assembly at the edge of a table weighed down by a book and test that it dispenses a chocolate.

Now for the creative part. Can you construct a more interesting Hershey's Special Dark Kisses dispenser using, for example, cardboard & glue, or whatever you have on hand.

<a name="myfootnote1">1</a>: Tutorials are [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/). Original page at [Sparkfun Inventor's Kit for Photon](https://learn.sparkfun.com/tutorials/sparkfun-inventors-kit-for-photon-experiment-guide/experiment-7-automatic-fish-feeder). This chocolate remix by Ron Zacharski
