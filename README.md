import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

public class SimpleCalculator extends JFrame implements ActionListener {
    private JTextField input = new JTextField();
    private double result = 0;
    private String operator = "=";
    private boolean calculating = true;

    public SimpleCalculator() {
        setTitle("Simple Calculator");
        setSize(400, 400);
        setDefaultCloseOperation(WindowConstants.EXIT_ON_CLOSE);

        setLayout(new BorderLayout());

        input.setEditable(false);
        input.setHorizontalAlignment(JTextField.RIGHT);
        add(input, "North");

        JPanel panel = new JPanel();
        panel.setLayout(new GridLayout(4, 4));
        String[] buttons = {
                "7", "8", "9", "/",
                "4", "5", "6", "*",
                "1", "2", "3", "-",
                "0", ".", "=", "+"};

        for (int i = 0; i < buttons.length; i++) {
            addButton(panel, buttons[i]);
        }

        add(panel, "Center");
        pack();
        setVisible(true);
    }

    private void addButton(Container c, String text) {
        JButton button = new JButton(text);
        button.addActionListener(this);
        c.add(button);
    }

    public void actionPerformed(ActionEvent evt) {
        String cmd = evt.getActionCommand();
        if ('0' <= cmd.charAt(0) && cmd.charAt(0) <= '9' || cmd.equals(".")) {
            if (calculating)
                input.setText(cmd);
            else
                input.setText(input.getText() + cmd);
            calculating = false;
        } else {
            if (calculating) {
                if (cmd.equals("-")) {
                    input.setText(cmd);
                    calculating = false;
                } else
                    operator = cmd;
            } else {
                double x = Double.parseDouble(input.getText());
                calculate(x);
                operator = cmd;
                calculating = true;
            }
        }
    }

    private void calculate(double n) {
        switch (operator) {
            case "+":
                result += n;
                break;
            case "-":
                result -= n;
                break;
            case "*":
                result *= n;
                break;
            case "/":
                result /= n;
                break;
            case "=":
                result = n;
                break;
        }
        input.setText("" + result);
    }

    public static void main(String[] args) {
        new SimpleCalculator();
    }
}
