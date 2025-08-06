import java.util.*;
import java.io.*;
import java.time.LocalDate;

public class CyberHygieneTracker {
    static Scanner scanner = new Scanner(System.in);
    static String[] tasks = {
        "Change one password",
        "Scan PC for malware",
        "Check if email is in a data breach",
        "Enable 2FA on any account",
        "Update software/apps"
    };
    static boolean[] completed = new boolean[tasks.length];
    static int score = 0;
    static final String LOG_FILE = "progress_log.txt";

    public static void main(String[] args) {
        System.out.println("🛡️ Welcome to Cyber Hygiene Tracker 🛡️");
        showRandomTip();
        displayTasks();

        while (true) {
            System.out.print("\nType task number to mark as done, 'view' to see progress, or 'exit' to save & quit: ");
            String input = scanner.nextLine();

            if (input.equalsIgnoreCase("exit")) {
                saveProgress();
                System.out.println("✅ Progress saved. Stay safe!");
                break;
            } else if (input.equalsIgnoreCase("view")) {
                displayTasks();
            } else {
                try {
                    int taskIndex = Integer.parseInt(input) - 1;
                    if (taskIndex >= 0 && taskIndex < tasks.length) {
                        if (!completed[taskIndex]) {
                            completed[taskIndex] = true;
                            score += 20;
                            System.out.println("✔️ Task marked as complete!");
                        } else {
                            System.out.println("⚠️ Task already completed.");
                        }
                    } else {
                        System.out.println("❌ Invalid task number.");
                    }
                } catch (NumberFormatException e) {
                    System.out.println("❌ Invalid input.");
                }
            }
        }
    }

    static void displayTasks() {
        System.out.println("\nToday's Tasks:");
        for (int i = 0; i < tasks.length; i++) {
            String status = completed[i] ? "[✔]" : "[ ]";
            System.out.println((i + 1) + ". " + status + " " + tasks[i]);
        }
        System.out.println("\n🔐 Cyber Hygiene Score: " + score + " / 100");
    }

    static void showRandomTip() {
        String[] tips = {
            "💡 Never reuse passwords across websites.",
            "💡 Use a password manager to store your credentials.",
            "💡 Don’t click suspicious links in emails.",
            "💡 Keep your software up to date.",
            "💡 Enable Two-Factor Authentication (2FA)."
        };
        Random rand = new Random();
        System.out.println("\n" + tips[rand.nextInt(tips.length)]);
    }

    static void saveProgress() {
        try (BufferedWriter writer = new BufferedWriter(new FileWriter(LOG_FILE, true))) {
            writer.write("Date: " + LocalDate.now() + "\n");
            for (int i = 0; i < tasks.length; i++) {
                writer.write((completed[i] ? "[✔] " : "[ ] ") + tasks[i] + "\n");
            }
            writer.write("Score: " + score + " / 100\n");
            writer.write("--------------------------------------------------\n");
        } catch (IOException e) {
            System.out.println("❌ Error saving progress: " + e.getMessage());
        }
    }
}
