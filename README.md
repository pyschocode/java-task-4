import java.util.*;

public class QuizGame {
    static class Question {
        String questionText;
        String[] options;
        int correctAnswer;

        public Question(String questionText, String[] options, int correctAnswer) {
            this.questionText = questionText;
            this.options = options;
            this.correctAnswer = correctAnswer;
        }

        public void display() {
            System.out.println("\n" + questionText);
            for (int i = 0; i < options.length; i++) {
                System.out.println((i + 1) + ". " + options[i]);
            }
            System.out.println("Answer within 5 seconds...");
        }
    }

    static List<Question> questions = new ArrayList<>();
    static int score = 0;
    static List<Boolean> results = new ArrayList<>();
    static Scanner scanner = new Scanner(System.in);

    public static void main(String[] args) {
        loadQuestions();
        runQuiz();
        showResults();
    }

    static void loadQuestions() {
        questions.add(new Question(
            "Which language runs in a web browser?",
            new String[]{"Java", "C", "Python", "JavaScript"},
            3
        ));

        questions.add(new Question(
            "What is the capital of France?",
            new String[]{"Berlin", "Madrid", "Paris", "Rome"},
            2
        ));

        questions.add(new Question(
            "Which number is prime?",
            new String[]{"4", "9", "11", "15"},
            2
        ));

        questions.add(new Question(
            "What is 5 * 6?",
            new String[]{"30", "25", "35", "20"},
            0
        ));

        questions.add(new Question(
            "What is the chemical symbol for water?",
            new String[]{"O2", "CO2", "H2O", "NaCl"},
            2
        ));
    }

    static void runQuiz() {
        for (Question q : questions) {
            q.display();
            boolean answered = false;
            long startTime = System.currentTimeMillis();

            System.out.print("Your answer (1-4): ");
            while ((System.currentTimeMillis() - startTime) < 5000) {
                if (scanner.hasNextInt()) {
                    int userAnswer = scanner.nextInt();
                    answered = true;
                    if (userAnswer - 1 == q.correctAnswer) {
                        System.out.println("✅ Correct!");
                        score++;
                        results.add(true);
                    } else {
                        System.out.println("❌ Wrong!");
                        results.add(false);
                    }
                    break;
                }
            }

            if (!answered) {
                System.out.println("⏰ Time's up!");
                results.add(false);
                scanner.nextLine(); // clear input buffer
            }
        }
    }

    static void showResults() {
        System.out.println("\n=== Quiz Finished ===");
        System.out.println("Your Score: " + score + " out of " + questions.size());

        for (int i = 0; i < questions.size(); i++) {
            Question q = questions.get(i);
            System.out.println("\nQ" + (i + 1) + ": " + q.questionText);
            System.out.println("Correct Answer: " + q.options[q.correctAnswer]);
            System.out.println("Your Answer: " + (results.get(i) ? "Correct" : "Incorrect"));
        }
    }
}
