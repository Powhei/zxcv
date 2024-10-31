# zxcv
zxcv
import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

public class SimpleCalculator extends JFrame implements ActionListener {
    // Components of the Calculator
    private JTextField result;
    private JButton btn2, btnSignChange, btn3, btnBackspace;
    private boolean signChangeClickedOnce = false;

    // Constructor
    public SimpleCalculator() {
        setTitle("Simple Calculator");
        setSize(300, 200);
        setDefaultCloseOperation(EXIT_ON_CLOSE);

        // Initialize components
        result = new JTextField();
        result.setEditable(false);
        result.setHorizontalAlignment(JTextField.RIGHT);
        result.setFont(new Font("Arial", Font.PLAIN, 24));

        btn2 = new JButton("2");
        btnSignChange = new JButton("+/-");
        btn3 = new JButton("3");
        btnBackspace = new JButton("Backspace");

        // Set action commands and listeners
        btn2.setActionCommand("2");
        btnSignChange.setActionCommand("SIGN_CHANGE");
        btn3.setActionCommand("3");
        btnBackspace.setActionCommand("BACKSPACE");

        btn2.addActionListener(this);
        btnSignChange.addActionListener(this);
        btn3.addActionListener(this);
        btnBackspace.addActionListener(this);

        // Layout configuration
        setLayout(new BorderLayout());

        JPanel panel = new JPanel();
        panel.setLayout(new GridLayout(1, 4));

        panel.add(btn2);
        panel.add(btnSignChange);
        panel.add(btn3);
        panel.add(btnBackspace);

        add(panel, BorderLayout.CENTER);
        add(result, BorderLayout.SOUTH);
    }

    // Handle button click events
    @Override
    public void actionPerformed(ActionEvent e) {
        String command = e.getActionCommand();

        switch (command) {
            case "2":
                result.setText(result.getText() + "2");
                break;
            case "3":
                result.setText(result.getText() + "3");
                break;
            case "SIGN_CHANGE":
                handleSignChange();
                break;
            case "BACKSPACE":
                backspace();
                break;
        }
    }

    private void handleSignChange() {
        if (!signChangeClickedOnce) {
            signChangeClickedOnce = true;
            Timer timer = new Timer(400, new ActionListener() {
                @Override
                public void actionPerformed(ActionEvent e) {
                    signChangeClickedOnce = false;
                }
            });
            timer.setRepeats(false);
            timer.start();
        } else {
            subtract();
            signChangeClickedOnce = false;
        }
    }

    private void add() {
        try {
            double currentValue = Double.parseDouble(result.getText());
            result.setText(String.valueOf(currentValue + 1));
        } catch (NumberFormatException ex) {
            result.setText("1");
        }
    }

    private void subtract() {
        try {
            double currentValue = Double.parseDouble(result.getText());
            result.setText(String.valueOf(currentValue - 1));
        } catch (NumberFormatException ex) {
            result.setText("-1");
        }
    }

    private void backspace() {
        String currentText = result.getText();
        if (!currentText.isEmpty()) {
            result.setText(currentText.substring(0, currentText.length() - 1));
        }
    }

    // Main method to run the calculator
    public static void main(String[] args) {
        SwingUtilities.invokeLater(() -> {
            SimpleCalculator calculator = new SimpleCalculator();
            calculator.setVisible(true);
        });
    }
}
