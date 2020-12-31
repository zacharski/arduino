## Variables

We all are familiar with variable in mathematics. For example,

```
x = 3
y = 2x + 10
```

And looking at that we know that _y_ equals 16. Let's look at another example. Suppose we execute the following statements sequentially.

```
x = 5
x = x + 1
```

So now, the value of x is 6. In this case the variable name is _x_ and the value of the variable _x_ is 6. Variable names are not limited to _x_ and _y_

```
weight = 145
weightKG = weight * .454
radius = 7
```

So the name of the first variable is _weight_ and the value of _weight_ is 145.

### Variables in Programming

Variables in programming are similar. There are 3 things you can do:

1. Declare a new variable
2. Set the value of a variable
3. Use the variable.

#### declaring a new variable

In the Arduino language, as with most programming languages, we must specify what type of values the variable can take. For example, if the value of the variable _x_ is an integer, we would declare that variable with

```
int x;
```

where `int` stands for integer.

We can also declare variables that can contain floating point numbers:

```
float pi;
```

strings of characters:

```
String  firstname;
```

and many others.

Almost everything we do in this course will use integer variables.

We can name a variable anything we want:

```
int redLed;
int fred;
int variable1;
int iHeartMary;
```

Following our **work to code** bullet, we give names that are meaningful. For example, if we have a red led and a blue led

```
int redLed;
int blueLed;
```

is a better choice then

```
int led1;
int led2;
```

and a far better choice than

```
int x;
int y;
```

Another convention is that variable names start with a lower case letter.

#### setting the value of a variable

We set the value of a variable by using a single equals sign =

```
int redLed;
redLed = 13;
```

Alternatively, we can declare and set the value of a variable in one line:

```
int redLed = 13;
```

This is the preferred method.

#### Using a variable.

We can get the value of a variable by just using its name, much like the math example:

```
x = 5
y = 2x + 6
```

On the _y_ line, the _x_ is replaced with its value:

```
y = 2 * 5 + 6
```

and then the expression is evaluated.

Similarly in programming if we had

```
1.  int redLed = 13;
2.
3.  int setup(){
4.      pinMode(redLed, OUTPUT);
5.  }
```

on line 4, _redLed_ is replaced with its value:

```
pinMode(13, OUTPUT);
```

and that line is evaluated.

### Scope

There are two major types of variable scope: local and global.

#### Local Variables

Local variables are those declared inside of a set of braces---{}

For example,

```
1.  void setup(){
2.        int redLed = 3;
3.        int blueLed = 4;
4.        pinMode(redLed, OUTPUT);
5.        pinMode(blueLed, OUTPUT);
6.  }
7.
8.  void loop(){
9.      int wait = 200;
10.     digitalWrite(redLed, HIGH);
11.     digitalWrite(blueLed, LOW);
12.     delay(wait);
13.     digitalWrite(redLed, LOW);
14.     digitalWrite(blueLed, HIGH);
15.     delay(wait);
16. }

```

`redLed` and `blueLed` are local variables because they are defined within the braces of the setup procedure. `wait` is also a local variable because it is defined within the braces of the loop procedure. Local variables can only be seen by lines of code within the block where they were declared. This is called the scope of the variable. The scope of `redLed` is the block from lines 2 through 6. So when the variable is used in line 4, it can be seen and the line executes correctly. `redLed` cannot be seen by line 11, since line 11 is outside of the scope (outside of the braces {}) so this line will result in an error.

#### Global Variables

Global variables can be seen anywhere in your program. They are declared at the start of your code, outside of `void setup` and `void loop`. For example,

```

1.  int redLed = 3;
2.  int blueLed = 4;
3.  int wait = 200;
4.
5.  void setup(){
6.          pinMode(redLed, OUTPUT);
7.          pinMode(blueLed, OUTPUT);
8.  }
9.
10. void loop(){
11.       digitalWrite(redLed, HIGH);
12.       digitalWrite(blueLed, LOW);
13.       delay(wait);
14.       digitalWrite(redLed, LOW);
15.       digitalWrite(blueLed, HIGH);
16.       delay(wait);
17. }

```

In this example, `redLed`, `blueLed`, and `wait` are global variables because they are declared outside of setup and loop. As a result, they can be used anywhere in the program. For example, `redLed` can be seen both by line 6 and by line 11.

It may seem like local variables are somewhat limited so why not always use globals which are more powerful.

As programs get more complex, there is a growing need to make them more modular. Each module will have a limited set of inputs and outputs.

Here is an analogy that isn't perfect but may be illustrative. Suppose Sharon and Louis have six children:

- Lisa
- David
- Gretchen
- Amy
- Tiffany
- Paul

and they all share an account on the family's desktop computer. So anyone can do anything with anybody's files. Paul, the youngest, might accidentally delete something Louis was working on for his job. David might intentionally send email under Amy's account. This is sort of equivalent to global variables where any line of a large program might change the value of the variable.

The obvious solution is for everyone to have separate accounts. Amy's files are not accessible to David, as an example. This is the equivalent of local variables.

In this course, we will start with using global variables then introduce local variables later on.

**Work to Code** means to use variables whenever possible.

Suppose we have the following code

```
    // setup runs exactly once
    void setup() {
       pinMode(13, OUTPUT);
       pinMode(3, OUTPUT);
    }

    // the loop function runs over and over again forever
    void loop() {
        digitalWrite(13, HIGH);
        digitalWrite(3, HIGH);
        delay(1000);
        digitalWrite(13, LOW);
        digitalWrite(3, LOW);
        delay(1000);
        digitalWrite(13, HIGH);
        delay(1000);
        digitalWrite(13, LOW);
        delay(1000);
        digitalWrite(3, HIGH);
        delay(1000);
        digitalWrite(3, LOW);
        delay(1000);

    }
```

And let's say that instead of hooking the leds to pins 13 and 3, we hooked them to pins 0 and 4. Now we would have to change all the 13s to 0s and all the 3s to 4s. If we used variables:

```
    int led1 = 13;
    int led2 = 3;

    // setup runs exactly once
    void setup() {
       pinMode(led1, OUTPUT);
       pinMode(led2, OUTPUT);
    }

    // the loop function runs over and over again forever
    void loop() {
        digitalWrite(led1, HIGH);
        digitalWrite(led2, HIGH);
        delay(1000);
        digitalWrite(led1, LOW);
        digitalWrite(led2, LOW);
        delay(1000);
        digitalWrite(led1, HIGH);
        delay(1000);
        digitalWrite(led1, LOW);
        delay(1000);
        digitalWrite(led2, HIGH);
        delay(1000);
        digitalWrite(led2, LOW);
        delay(1000);

    }
```

Now if we move the led from pin 13 to pin 0 we only need to change one line:

```
   int led1 = 0;

```

Now suppose we like the pattern of the above code but would like it to blink faster. That would require changing all the occurrences of 1000 to 200. But if we used a variable, that change would be trivial:

```
    int led1 = 13;
    int led2 = 3;
    int wait = 200;

    // setup runs exactly once
    void setup() {
       pinMode(led1, OUTPUT);
       pinMode(led2, OUTPUT);
    }

    // the loop function runs over and over again forever
    void loop() {
        digitalWrite(led1, HIGH);
        digitalWrite(led2, HIGH);
        delay(wait);
        digitalWrite(led1, LOW);
        digitalWrite(led2, LOW);
        delay(wait);
        digitalWrite(led1, HIGH);
        delay(wait);
        digitalWrite(led1, LOW);
        delay(wait);
        digitalWrite(led2, HIGH);
        delay(wait);
        digitalWrite(led2, LOW);
        delay(wait);

    }
```

Finally, there are some things we can do only with using variables for example:

```
    int led1 = 13;
    int wait = 100;

    // setup runs exactly once
    void setup() {
       pinMode(led1, OUTPUT);
    }

    // the loop function runs over and over again forever
    void loop() {
        digitalWrite(led1, HIGH);
        delay(wait);
        digitalWrite(led1, LOW);
        delay(wait);
        wait = wait + 10;

    }
```

What do you think the result would be?

The blinking would start off fast and go slower and slower.
