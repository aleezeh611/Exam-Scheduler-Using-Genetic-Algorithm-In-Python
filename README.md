# Exam-Schedule-Generator
Exam Schedule Generator built for an Artificial Intelligence Project using Genetic Algorithm

- IMP: both the colab python notebook and a simple .py file are available. The code and logic is the exact same in both

# Data Set
The data set for this project requires a minimum of 4 documents:
- List of Student Names
- List of each student name and the courses they take. This will have 3 columns, first column is index number and second is student's name, third is the course student is enrolled in
- List of teacher names
- List of courses and their codes. First column is course code and second course name.

## Data Collection
All data has been collected and stored in python lists.
A class called **Student Data** stores name of the student along with the entire list of courses the student has
registered in, we can keep a check here to disregard any student which has less than 3 courses however
currently we are assuming that the data we are reading has been processed and contains no students with less
than 3 registered courses.
We have made a list of the StudentData class which stores each student and their corresponding courses.
Then we have our most important class: **ExamDetails**. This stores:
* course
* time for exam
* invigilator
* day

Each of our chromosomes are built in a way that each is a **3D list** storing the entire schedule. Details for the list is as follows:
            * Outer most row of the chromosomes depicts the day for the exam season (Mon/Wed/Tues etc)
            * Middle layer is the time slot of the day (9am/1am etc) (depending on constraints it is possible that some days only have morning exam and no evening exams) 
            * Inner most layer will have entire ExamDetails Object since on a day, at a given time slot, there can be more than one exams

## Chromosome Generation:
Our **GenerateChromose()** function generates one single chromosome (entire schedule). It runs in a loop trying to
fill up each slot (morning and evening) of each day randomly but the random generation is conditional and
smartly implemented so our algorithm is intelligently making random guesses instead of naïve guesses.
A course is randomly chosen from the list of courses and then that course is removed from the list so that for
the next random choice of courses the same course cannot be repeated and so each course is scheduled only
once, and for a single time slot, the list of all students appearing in a course are stored in a temporary list so
while scheduling another course in the same time slot when a course is randomly selected it is also checked to
see if any of the students appearing in that course overlap with the course already scheduled.
Similarly when chosing our invigilators for the same time slot, the chosen invigilator’s name is randomly picked
and then removed from the list so in next random name generation the chosen invigilator cannot be chosen.
This list is then reset after every time slot

**Evolution** is done using 2 main reproduction techniques:
- Crossover:
      2 children are made by crossing over the second and first slots of both parents
- Mutation:
      Randomly 3 slots are interchanged with 3 other slots of each parent and 2 more children are created
note: children only replace the chromosomes with worst fitness in the population if their own fitness is an improvement

