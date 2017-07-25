
# Teacher tutorial

### ~avatar avatar

Code for teachers to build @boardname@ to control quizes developed by students.

### ~

### Step 1

In the JavaScript part of the code paste below code.

```blocks
let answers: string[] = []
let timerLengths: number[] = []
let serialNumbers: number[] = []
let scores: number[] = []
let studentAnswer: string[] = []
let questionNumber: number = 0

radio.setGroup(62)

// test values
answers = ["A", "A", "B", "B", "B"]
timerLengths = [25, 25, 25, 25, 25]
questionNumber = 0


//Showing question number
basic.showNumber(questionNumber + 1)

//Sending number of questions
radio.sendValue("no_questions", answers.length)

input.onButtonPressed(Button.B, () => {
    //Moving to next question
    questionNumber++
    basic.showNumber(questionNumber + 1)
})

input.onButtonPressed(Button.A, () => {
    //send timer
    radio.sendValue("timer", timerLengths[questionNumber])
})

input.onButtonPressed(Button.AB, () => {
    //send all scores back to students
    sendScores()
})

//receiving radio packet from students
radio.onDataPacketReceived(({serial, receivedString}) => {
    //checking if serial number is already in list
    if (findIndex(serial) < 0) {
        // adding to list if not
        serialNumbers.push(serial)
        //setting up score for student
        scores.push(0)
    }

    //Marking student answer
    markStudentAnswer(questionNumber, serial, receivedString)

})

//goes through each serial number and sends a serialNumber/score pair
function sendScores() {
    let i: number
    //for each serial number, send score
    for (i = 0; i < serialNumbers.length; i++) {
        //sending serialNumber/score pair
        radio.sendValue(serialNumbers[i].toString(), scores[i])
    }
}

// checks if the student's answer is right and adds 1 to their score if so
function markStudentAnswer(questionNumber: number, serialNumber: number, answer: string) {
    let index: number = findIndex(serialNumber)
    if (checkAnswer(questionNumber, answer)) {
        addToScore(index)
    }
}
// finds the index of a serial number
function findIndex(serialNumber: number) {
    let i: number
    for (i = 0; i < serialNumbers.length; i++) {
        if (serialNumbers[i] == serialNumber) {
            return i
        }
    }
    return -1
}
// checks if an answer is correct
function checkAnswer(questionNumber: number, studentAnswer: string) {
    if (answers[questionNumber] == studentAnswer) {
        return true
    }
    else {
        return false
    }
}
// adds 1 to the score at the specified index
function addToScore(index: number) {
    scores[index]++
}
```
In the code we first declare variables.

Next step is to set the starting values for the variables. Update this section of the code with the correct answers per question and how much time in seconds should students have per question. In example below correct answers are A, A, B, B, B and all questions have a time limit of 25 seconds.

```blocks
answers = ["A", "A", "B", "B", "B"]
timerLengths = [25, 25, 25, 25, 25]
```
### Step 2
Download the code to @boardname@ and start the quiz with the students.

### How to use it?

Press A to send the timer event for the first question to all the students.

Press B to move to the next question (display will show the new question number). Only do this once the timer has expired. You will still need to click on button A to start the new question.

Press A+B at the end of the quiz to send the students their score.

### ~
