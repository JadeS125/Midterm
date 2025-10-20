import java.util.Scanner;
import java.util.Random;
import java.util.ArrayList;
import java.util.List;



public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        Random rand = new Random();

        double[] values = {
            0.01, 1, 5, 10, 25, 50, 75, 100, 200, 300, 400, 500,
            750, 1000, 5000, 10000, 25000, 50000, 75000, 100000,
            200000, 300000, 400000, 500000, 750000, 1000000
        };

        for (int i = values.length - 1; i > 0; i--) {
            int j = rand.nextInt(i + 1);
            double temp = values[i];
            values[i] = values[j];
            values[j] = temp;
        }

        List<Integer> availableCases = new ArrayList<>();
        for (int i = 1; i <= 26; i++) {
            availableCases.add(i);
        }

        System.out.println("WELCOME TO DEAL OR NO DEAL!");
        System.out.println("There are 26 cases. And one has $1,000,000!");

        int playerCase = 0;
        while (playerCase < 1 || playerCase > 26) {
            System.out.print("Pick a case to keep until the end (1-26): ");
            playerCase = sc.nextInt();
        }

        availableCases.remove(Integer.valueOf(playerCase));
        int casesOpened = 0;
        boolean dealTaken = false;
        double offer = 0.0;

        while (availableCases.size() > 1 && !dealTaken) {
            System.out.println("\nAvailable cases: " + availableCases);
            int chosenCase = 0;

            while (chosenCase < 1 || chosenCase > 26 || chosenCase == playerCase || !availableCases.contains(chosenCase)) {
                System.out.print("Choose a case to open: ");
                chosenCase = sc.nextInt();  

            int index = chosenCase - 1;
            System.out.printf("Case #%d contained $%,.2f\n", chosenCase, values[index]);
            availableCases.remove(Integer.valueOf(chosenCase));
            casesOpened++;

            if (casesOpened % 3 == 0) {
                double sum = 0;
                for (int c : availableCases) {
                    sum += values[c - 1];
                }
                offer = sum / availableCases.size();
                System.out.printf("\nBanker's offer: $%,.2f\n", offer);

                String decision = "";
                while (!decision.equals("deal") && !decision.equals("no deal")) {
                    System.out.print("Do you take the deal? (deal/no deal): ");
                    decision = sc.nextLine();
                }

                if (decision.equals("deal")) {
                    System.out.printf("You accepted $%,.2f. Game over!\n", offer);
                    dealTaken = true;
                } else {
                    System.out.println("You said NO DEAL! Keep playing.");
                }
            }
        }

        if (!dealTaken && availableCases.size() == 1) {
            int lastCase = availableCases.get(0);
            System.out.println("\nFinal round!");
            System.out.println("Your case #" + playerCase + " vs Case #" + lastCase);

            String decision = "";
            while (!decision.equals("yes") && !decision.equals("no")) {
                System.out.print("Do you want to switch your case? (yes/no): ");
                decision = sc.next();
            }

            int finalCase = decision.equals("yes") ? lastCase : playerCase;
            System.out.printf("Your final case #%d contains $%,.2f\n", finalCase, values[finalCase - 1]);
            System.out.println("Thanks for playing!");
        }

        if (dealTaken) {
            System.out.println("Thanks for playing!");
        }

        sc.close();
    }
}
