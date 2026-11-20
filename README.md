# Nonsense_Quiz
 
**Name:** Jaiza Mae D. Gonzales
**Section:** SE - 3A
**Topic:** About me, and Government
 
## Features Completed
10 multiple-choice questions
Random question order
15-second countdown timer
Answer checking and time-out handling
Score and percentage
Answer review
High score saved in a text file
Restart button
 
## How the Questions Are Stored and Shuffled
All questions live in QUESTIONS, which is a Python list of dict objects. Each dictionary holds three keys: "question" (the prompt string), "choices" (a list of four option strings starting with "A." through "D."), and "answer" (a single uppercase letter matching the right choice).
When a quiz begins or restarts in start_quiz(), the program uses random.sample(QUESTIONS, len(QUESTIONS)) to create a newly shuffled copy stored in quiz_questions. Using random.sample() leaves the original QUESTIONS list untouched so every playthrough can randomize the order cleanly without mutating the source data.
 
## How the Timer Works
How the Timer Works
Each question starts with time_left = TIME_LIMIT (15 seconds) in show_question() and calls update_timer(). Inside update_timer():
If answered is already True, the function returns immediately without scheduling another tick.
It updates timer_label with the remaining seconds (turning the text WRONG_COLOR when time_left is 5 seconds or less).
If time_left == 0, it calls time_up() to lock the question and record it as unanswered/incorrect.
Otherwise, it decrements time_left by 1 and schedules itself to run again after 1000 milliseconds (1 second) using root.after(1000, update_timer), saving the scheduled ID in timer_job.
 
## How the Program Identifies the Correct Answer
Each tk.Radiobutton uses choice[0] (the leading letter like "A", "B", "C", or "D") as its value bound to the shared tk.StringVar() named selected_answer.
When submit_answer() runs, it reads the user's pick with choice = selected_answer.get(). After checking that choice != "", it compares choice == question["answer"]:
If they match: score increases by 1 and feedback_label shows "Correct!".
If they don't match: get_choice_text(question, letter) finds the full choice string in question["choices"] starting with letter + "." so both the user's choice and the right choice can be displayed clearly in the feedback label and appended to wrong_answers for the end-of-quiz review.

## How the Score and High Score Work
Score: Starts at 0 in start_quiz() and increments by 1 each time submit_answer() sees choice == question["answer"].
Percentage: Calculated in show_results() as (score / total) * 100, where total = len(quiz_questions), and formatted with {percentage:.0f}% (rounded to a whole number). It also determines the closing encouragement message (greater than or equal to 90%, 70%, 50%, or below).
Reading the high score file: get_high_score() checks if high_score.txt (located in the same folder as the script via os.path.abspath(__file__)) exists. If it does, it opens the file in read mode ("r"), strips whitespace, and converts the text to int(). If the file is missing or contains non-integer text (ValueError), it returns 0.
Saving the high score file: In show_results(), save_high_score(score) compares new_score > get_high_score(). Only when the new score strictly beats the saved high score does it open high_score.txt in write mode ("w"), overwrite it with str(new_score), and return True (triggering "New high score!").
 
## Function Explanations
​get_high_score() reads and returns the saved integer high score from high_score.txt (or 0 if the file is missing or invalid), and it is called when rendering the start screen, checking whether a new score beats the record, and displaying the high score on the results screen.
​save_high_score(new_score) writes new_score to high_score.txt only when it is higher than the current high score (returning True if updated), and it is called while calculating results in show_results().
​clear_screen() destroys all child widgets inside main_frame to make way for a new screen, and it is called whenever the program switches to the start screen, displays a question, or shows the final results.
​get_choice_text(question, letter) finds and returns the full choice string that starts with letter + "." (or "No answer" if none is found), and it is called when recording wrong answers or displaying the correct choice in feedback and the answer review.
​show_start_screen() builds the title, quiz rules and timer summary, high score display, and Start Quiz button, and it is called once during initial startup before root.mainloop().
​start_quiz() shuffles QUESTIONS into quiz_questions and resets current_question, score, and wrong_answers, and it is called when clicking Start Quiz or restarting through restart_quiz().
​show_question() resets per-question state and the timer before rendering the progress indicator, score, question text, radio choice buttons, and action buttons, and it is called when starting a quiz and advancing to each subsequent question in next_question().
​update_timer() updates the countdown label every second, calls time_up() when time_left hits 0, and schedules the next one-second tick, and it is called at the end of show_question() and once every second via root.after(1000, ...) while the question remains unanswered.
​stop_timer() cancels any pending root.after timer callback and clears timer_job, and it is called when submitting an answer in submit_answer() and closing the window in on_close().
​lock_choices() disables all radio choice buttons so the user cannot change their selection after answering or running out of time, and it is called in submit_answer() and time_up().
​submit_answer() validates that an answer is selected, stops the timer, locks the choices, scores the answer, updates the feedback label, and enables the next button, and it is called when clicking Submit Answer.
​time_up() locks the choices, disables the submit button, logs the question as timed out in wrong_answers, shows the right answer, and enables the next button, and it is called when update_timer() reaches time_left == 0.
​next_question() increments current_question and either displays the next question or transitions to show_results() once all questions are finished, and it is called when clicking Next Question or See Results.
​show_results() calculates the percentage score, saves a new high score if achieved, and displays the summary stats, encouragement message, scrollable wrong-answer review, and restart or exit buttons, and it is called after completing the final question in next_question().
​restart_quiz() starts a new quiz run by reshuffling the questions and resetting the score, and it is called when clicking Restart Quiz on the results screen.
​on_close() stops any active timer and closes the Tkinter root window cleanly, and it is called when clicking Exit or clicking the window's close (X) button (WM_DELETE_WINDOW).

## Screenshots
![Start screen](screenshots/start.png)
![Question screen](screenshots/question.png)
![Answer feedback](screenshots/feedback.png)
![Results screen](screenshots/results.png)
![High score after reopening](screenshots/high_score.png)
 
## Challenges I Faced
Lack of device
