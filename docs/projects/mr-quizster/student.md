
# Student tutorial

### ~avatar avatar

Did you know your can build you own @boardname@ and use it to take part in classroom quizzes? Simply follow the steps below and you'll be done in no time!

### ~

### Step 1 

Start by allowing your device to broadcast its serial number and set the radio frequency to the value chosen by your teacher, for example 62. Also, when you will later get a question, you will want to know how much time you have left for your answer, that's why you should set the variable show_timer to be true.

```blocks
radio.setTransmitSerialNumber(true)
radio.setGroup(62)
show_timer = true
```

### Step 2

Set up the buttons A and B so that you can submit the answers to the quiz questions by pressing the button corresponding to the correct answer. 

Here's how to do it for A:

```blocks
input.onButtonPressed(Button.A, () => {
    if (show_timer == true) {
        show_timer = false
        radio.sendString("A")
        basic.showString("A")
    }
})
```

Now try reproducing the same steps for B!

In the next steps you'll set up your @boardname@ to listen to the radio frequencies on which the teacher is broadcasting and then broadcast back your answer.

### Step 3

First, you want to know how much time left to answer the question. 

```blocks
        show_timer = true
```
Let's start by having all the pins lit and decrease the number of lit pins as the time runs out.

```blocks
        basic.showLeds(`
            # # # # #
            # # # # #
            # # # # #
            # # # # #
            # # # # #
            `)
 ```
            
For each question, the teacher will set the number of seconds you have to answer. The teacher will send this number in seconds, but @boardname@ can only understand miliseconds, so you'll have to multiply the number of seconds by 1000. There are 5 x 5 = 25 pins on the board, and thus you have to devide by 25 to get the number of miliseconds before another pin will become unlit.

```blocks
        total_wait_time = value
        wait_time = total_wait_time * 1000 / 25
```

### Step 4

Now that you know how the timer should work, let's write the code that actually unlights the pins, one by one, from the top left corner to the bottom right corner.

```blocks
  for (let y = 0; y <= 4; y++) {
           for (let x = 0; x <= 4; x++) {
                if (show_timer) {
                    led.unplot(x, y)
                    basic.pause(wait_time)
                }
            }
        }
```

### Step 5

Now we will bring together the blocks we build in Steps 3 and 4.

```blocks
    if (radio_control == "timer") {
        total_wait_time = value
        wait_time = total_wait_time * 1000 / 25
        show_timer = true
        basic.showLeds(`
            # # # # #
            # # # # #
            # # # # #
            # # # # #
            # # # # #
            `)
        for (let y = 0; y <= 4; y++) {
            for (let x = 0; x <= 4; x++) {
                if (show_timer) {
                    led.unplot(x, y)
                    basic.pause(wait_time)
                }
            }
        }
        show_timer = false
    }
```
### Step 6

Now we will show the scores!

If the message you get from the teacher is not the timer for another question, but the serial number of your device, then you must be receiving your score. We will display the number of points you achieved as: "Score: your_points". Below you can see the code that does just that.

```blocks
    if (parseInt(radio_control) == control.deviceSerialNumber()) {
        basic.showString("Score:" + value)
    }
 ```
 
### Step 7

And finally, here is how we bring it all together: if the teacher broadcasts a timer, it means the countdown for the next question is about to start; if the teacher broadcasts the serial number of your device, you are about to get your score!

```blocks
    radio.onDataPacketReceived( ({ receivedString: radio_control, receivedNumber: value }) =>  {
    if (radio_control == "timer") {
        total_wait_time = value
        wait_time = total_wait_time * 1000 / 25
        show_timer = true
        basic.showLeds(`
            # # # # #
            # # # # #
            # # # # #
            # # # # #
            # # # # #
            `)
        for (let y = 0; y <= 4; y++) {
            for (let x = 0; x <= 4; x++) {
                if (show_timer) {
                    led.unplot(x, y)
                    basic.pause(wait_time)
                }
            }
        }
        show_timer = false
    }
    if (parseInt(radio_control) == control.deviceSerialNumber()) {
        basic.showString("Score:" + value)
    }
})
radio.setTransmitSerialNumber(true)
radio.setGroup(62)
show_timer = true
```
    
Good luck!

### ~

