# LLMs for math education

I have been working on this project with a math teacher at my school and a few other students. The main goal is to provide useful LLM-based tools to teachers and students.

## Tutoring based on assignment questions

I am developing an application that will allow students to get help on assignments provided by the teacher. This is a common use-case of current LLMs, but they often are bad at helping students, make silly mistakes, and have other issues.

### Planned solution

We would implement the following workflow:

* Student chooses a problem on an assignment
* LLM is asked to solve the problem
* LLM answer is checked against the correct answer
* If answer is incorrect, try again
* If answer is incorrect again, give LLM correct answer and solution writeup and have it redo it
* If answer is correct, give LLM the solution writeup and allow the user to start chatting

The student would want to 

### Current progress

% TODO: Add pictures

## Evaluation of LLMs for educational use

There are very few evaluations of LLMs for educational use. Google's LearnLM has been working on making LLMs for educational use, but did not produce a public evaluation suite that could be used to evaluate other LLMs. We plan to find benchmarks that we could evaluate current state-of-the-art LLMs on, and potentially create our own benchmark if it's necessary.

We would need to evaluate:
* Sycophancy - tendency to agree with what the user says
  * There are some benchmarks out there, but none of them have been run on recent models
  * We need to minimize sycophancy because otherwise the model will think the user's mistakes are correct 
* Math performance
  * Of course, it needs to solve the math problems
  * This is well established
* Use of interpreter
  * This seems to be something that only proprietary vendors and Alibaba are interested in
  * Interpreter is important to make sure it gets the correct answer while solving the problem by itself
* Tutoring performance
  * This is a bit hard to define, but it's the core of what we want to do
  * We will probably have to follow the LearnLM paper, as there is barely anything else publicly available

We haven't really started this project yet, as we don't have much compute and are quite busy.
