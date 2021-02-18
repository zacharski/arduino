## Experiment 4:

# Procedures

<iframe width="560" height="315" src="https://www.youtube.com/embed/rLi2wXxPTYk" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Introduction

In this experiment, no additional hardware is introduced. Instead, we focus on an important software concept: **procedures**. The common definition of the word procedure is a series of actions conducted in a certain order. In this sense of the word we already have been writing procedures. In everyday life, there are all sorts of procedures. For example, [Bridgestone Tires](https://www.bridgestonetire.com/tread-and-trend/drivers-ed/how-to-change-a-flat-tire) offers an 18 step procedure to change a car tire:

1. find a safe location
2. turn on your hazard lights
3. apply the parking brake
4. apply wheel wedges
5. remove the hubcap
6. loosen lug nuts
   ...

The cool thing is that we have internalized many procedures, meaning someone can just tell us the name of the procedure and we know how to do it.

When someone says _mow the lawn_ we know the steps involved. Perhaps the first step is to walk around the yard looking for and removing dog toys, branches and other obstacles. Then we might check the lawn mower for gas. If a friend tells us to _come over to their house_ we know the procedure for that: the roads to take and the turns to make. Perhaps the first time we visited our friend told us how to get there:

#### Here is how to get to my house

1. get on NM-599 South
2. continue on NM-599 South
3. take the onramp to I-25 South to Albuquerque.
4. Take exit 149
5. ...

Once we learn that procedure, our friend can just say _come over_ and we know how to do that.

Wouldn't it be great if we could just tell the computer _this is how you blink the red led_ and give it instructions like

```
digitalWrite(red, HIGH);
delay(wait);
digitalWrite(red, LOW);
delay(wait);
```

and after the computer learns that, we can tell it to _blink red_ instead of repeating those instructions?

The good news is that we can. We can give a sequence of instructions a name, and then use that name when we want to execute those instructions. The magical incantation to do that is

```
void blinkRed(){

}
```

And my English gloss of that is _this is how to **blinkRed**_. I am calling it a magical incantation because at this point I am not going to explain what `void` does or what can go inside the parentheses. That will be a later experiment.

Let's look at a complete example:

```
int redLed = 3;
int blueLed = 4;
int wait = 200;

void setup(){
  pinMode(redLed, OUTPUT);
  pinMode(blueLed, OUTPUT);
}

void blinkRed(){
  digitalWrite(redLed, HIGH);
  delay(wait);
  digitalWrite(redLed, LOW);
  delay(wait);
}


void blinkBlue(){
  digitalWrite(blueLed, HIGH);
  delay(wait);
  digitalWrite(blueLed, LOW);
  delay(wait);
}

void loop(){
  blinkRed();
  blinkRed();
  blinkBlue();
  blinkBlue();
}
```

The procedures `setup` and `loop` are required for any Arduino program.

#### setup

I gloss `void setup()` as _this is how to setup_ and then in the setup procedure we set the pinMode of the red and blue led pins to OUTPUT.

#### blinkRed

`void blinkRed()` tells the computer how to blinkRed. Namely, execute the four instructions (turn on the red led, wait a while, turn off the red led and wait some more)

#### blinkBlue

`void blinkBlue()` tells the computer the set of instructions for the blinkBlue procedure

#### loop

`void loop()` describes the required loop procedure. In it we tell the computer to execute the `blinkRed()` procedure which we defined earlier in the program. And then blink it again. And then execute `blinkBlue()` twice.

Notice that we define a procedure with

```
void blinkBlueAndRed()
```

and after we define it, the syntax for using it is:

```
blinkBlueAndRed()
```

without the `void`.

As with variable names, we can name procedures pretty much anything we want. There is nothing special about the names `blinkRed` and `blinkBlue`

The following program would execute identically to the one above:

```
int banana = 3;
int apple = 4;
int blueberry = 200;

void setup(){
  pinMode(banana, OUTPUT);
  pinMode(apple, OUTPUT);
}

void ann(){
  digitalWrite(banana, HIGH);
  delay(blueberry);
  digitalWrite(banana, LOW);
  delay(blueberry);
}


void mary(){
  digitalWrite(apple, HIGH);
  delay(blueberry);
  digitalWrite(apple, LOW);
  delay(blueberry);
}

void loop(){
  ann();
  ann();
  mary();
  mary();
}
```

They execute the same, but this one is much harder for us to read. `blinkRed` describes what the procedure does while `ann` does not.

### Procedures can be nested

Notice that the procedure `blinkRed` in the above is nested inside the `loop` procedure. Suppose we want to blink Morse Code messages. The fundamental units of Morse code are dots and dashes. Here are the rules:

1. a dot is one time unit long
2. a dash is three time units long (i.e., 3 dots)
3. the space between a dot or dash within a letter is one time unit long
4. the space between letters is three time units long
5. the space between words is seven time units long

With that we can start our program:

```
int led = 13;
int unit = 200;

void setup(){
  pinMode(led, OUTPUT);
}
```

Next, we define the procedures for dot and dash:

```
void dot(){
  // a dot is one time unit long
  digitalWrite(led, HIGH);
  delay(unit);
  // the space between a dot or dash within a
  // letter is one time unit long
  digitalWrite(led, LOW);
  delay(unit);
}

void dash(){
  // a dash is three time units long (i.e., 3 dots)
  digitalWrite(led, HIGH);
  delay(unit * 3);
  digitalWrite(led, LOW);
  delay(unit);

}
```

Now with dots and dashes defined, we can define a few letters.

![](/arduino/img/morse.png)

Let's say we want to send out an SOS. So we need the letters S (three dots) and O (three dashes):

```
void S(){
  dot();
  dot();
  dot();
  // since the space between letters is three
  // time units long, we need to add two more
  delay(unit * 2);
}

void O(){
  dash();
  dash();
  dash();
  delay(unit * 2);
}
```

Finally, we can write the SOS procedure:

```
void SOS(){
  S();
  O();
  S();
  delay(unit * 4);
}

void loop(){
  SOS();
}
```

### Advantages of Using Procedures

#### Readability and Debugability

Code that uses procedures is highly organized. The lines of your code that do one particular thing are within their own procedure. This often results in programs that are shorter. All this makes your code easier to debug. Notice how much easier it is to read something like:

```

...

void when(){
w();
h());
e();
n();
delay(unit\*4);
}
...
void chapter1(){
when();
I();
wrote();
the();
following()
pages();
or();
rather();
the()
bulk();
of();
them();
}
...
void loop(){
chapter1();
chapter2();
chapter3();
}

```

then something like:

```

void loop(){
digitalWrite(led, HIGH);
delay(unit);
digitalWrite(led, LOW);
delay(unit);
digitalWrite(led, HIGH);
delay(unit _ 3);
digitalWrite(led, LOW);
delay(unit);
digitalWrite(led, HIGH);
delay(unit _ 3);
digitalWrite(led, LOW);
delay(unit \* 3);
digitalWrite(led, HIGH);
delay(unit);
digitalWrite(led, LOW);
delay(unit);
digitalWrite(led, HIGH);
delay(unit);
digitalWrite(led, LOW);
delay(unit);
digitalWrite(led, HIGH);
delay(unit);
digitalWrite(led, LOW);
delay(unit);
...

}

```

which is Morse code for only _wh_. You can see where this would be hard to debug.

#### Ability to reuse code

You can reuse code in different parts of your program without copying the lines. In our SOS program we used the S procedure twice.

### The Remixes

#### Remix 1: Morse Code Phrase

Construct a circuit with two LEDs. Using the SOS example above as an example write and upload code that will blink the Leds to display a 2 or 3 word message. For example,

```

void loop(){
work();
to();
code()
delay(unit \* 5);
}

```

#### Remix 2: The Beats version 3

This is a remix of the beats from the button experiment:

We are going to use the patterns we constructed for experiment one: double tap, slow n fast, and Cylon. Here is the behavior of your circuit:

1. when no buttons are pressed, no LEDs are on
2. when button1 is pressed, the pattern is double tap
3. when button2 is pressed, the pattern is slow n fast
4. when button3 is pressed, the pattern is Cylon

You will need to construct four procedures:

- allOff
- doubleTap
- slowNfast
- cylon

and then use them with the _if_-statements inside the loop procedure.

#### Remix 3: Procedural Bike Light

This is a remix of the bike light from the potentiometer experiment. As with the remix above, you will need to construct procedures for each of your patterns. For example,

- allOff
- allOn
- chaser
- etc.

Then, use these procedures inside the _if_-statements in the loop function.
