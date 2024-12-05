import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

public class SimpleCalculator {
    public static void main(String[] args) {
        // Create frame
        JFrame frame = new JFrame("Simple Calculator");
        frame.setSize(400, 500);
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.setLayout(null);

        // Create text field
        JTextField textField = new JTextField();
        textField.setBounds(30, 40, 330, 50);
        textField.setHorizontalAlignment(SwingConstants.RIGHT);
        frame.add(textField);

        // Create buttons
        String[] buttons = {
                "7", "8", "9", "/", 
                "4", "5", "6", "*", 
                "1", "2", "3", "-", 
                "0", "C", "=", "+"
        };

        int x = 30, y = 100;
        for (int i = 0; i < buttons.length; i++) {
            JButton button = new JButton(buttons[i]);
            button.setBounds(x, y, 70, 70);
            frame.add(button);

            x += 80;
            if ((i + 1) % 4 == 0) {
                x = 30;
                y += 80;
            }

            // Add action listener for each button
            button.addActionListener(new ActionListener() {
                @Override
                public void actionPerformed(ActionEvent e) {
                    String command = e.getActionCommand();

                    if (command.equals("C")) {
                        textField.setText("");
                    } else if (command.equals("=")) {
                        try {
                            textField.setText(Double.toString(evaluate(textField.getText())));
                        } catch (Exception ex) {
                            textField.setText("Error");
                        }
                    } else {
                        textField.setText(textField.getText() + command);
                    }
                }
            });
        }

        frame.setVisible(true);
    }

    // Method to evaluate a mathematical expression
    public static double evaluate(String expression) {
        // Remove spaces and parse expression
        expression = expression.replaceAll("\\s", "");
        char[] tokens = expression.toCharArray();

        double result = 0;
        double currentNumber = 0;
        char operation = '+';

        for (int i = 0; i < tokens.length; i++) {
            char current = tokens[i];

            if (Character.isDigit(current)) {
                currentNumber = currentNumber * 10 + (current - '0');
            }

            if (!Character.isDigit(current) && current != ' ' || i == tokens.length - 1) {
                switch (operation) {
                    case '+':
                        result += currentNumber;
                        break;
                    case '-':
                        result -= currentNumber;
                        break;
                    case '*':
                        result *= currentNumber;
                        break;
                    case '/':
                        if (currentNumber != 0) {
                            result /= currentNumber;
                        } else {
                            throw new ArithmeticException("Division by zero");
                        }
                        break;
                }
                operation = current;
                currentNumber = 0;
            }
        }
        return result;
    }
}
