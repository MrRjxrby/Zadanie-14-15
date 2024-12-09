# Zadanie-14-15
#14.1
```
import java.util.Scanner;
import java.util.regex.Pattern;
import java.util.regex.Matcher;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        String inputString = ""; 
        boolean exit = false;

        while (!exit) {
            System.out.println("\nВыберете Действие:");
            System.out.println("1. Ввести строку");
            System.out.println("2. Разбить строку на элементы");
            System.out.println("3. Найти совпадения в строке");
            System.out.println("4. Заменить элементы в строке");
            System.out.println("0. Выход");
            System.out.print("Ваш выбор: ");
            int choice = scanner.nextInt();
            scanner.nextLine();

            switch (choice) {
                case 1:
                    System.out.print("Введите строку: ");
                    inputString = scanner.nextLine();
                    System.out.println("Строка сохранена.");
                    break;

                case 2:
                    if (inputString.isEmpty()) {
                        System.out.println("Строка не задана. Используйте опцию 1, чтобы ввести строку.");
                    } else {
                        System.out.print("Введите регулярное выражение для разбивки (Пример: [,]): ");
                        String splitRegex = scanner.nextLine();
                        String[] elements = inputString.split(splitRegex);
                        System.out.println("Результат разбиения:");
                        for (int i = 0; i < elements.length; i++) {
                            System.out.println((i + 1) + ": " + elements[i]);
                        }
                    }
                    break;

                case 3:
                    if (inputString.isEmpty()) {
                        System.out.println("Строка не задана. Используйте опцию 1, чтобы ввести строку.");
                    } else {
                        System.out.print("Введите регулярное выражение для поиска: ");
                        String matchRegex = scanner.nextLine();
                        Pattern pattern = Pattern.compile(matchRegex);
                        Matcher matcher = pattern.matcher(inputString);
                        System.out.println("Совпадения:");
                        boolean found = false;
                        while (matcher.find()) {
                            System.out.println("Найдено: " + matcher.group() + " на позиции [" + matcher.start() + ", " + matcher.end() + ")");
                            found = true;
                        }
                        if (!found) {
                            System.out.println("Совпадений не найдено.");
                        }
                    }
                    break;

                case 4:
                    if (inputString.isEmpty()) {
                        System.out.println("Строка не задана. Используйте опцию 1, чтобы ввести строку.");
                    } else {
                        System.out.print("Введите регулярное выражение для замены: ");
                        String replaceRegex = scanner.nextLine();
                        System.out.print("Введите текст для замены: ");
                        String replacement = scanner.nextLine();
                        inputString = inputString.replaceAll(replaceRegex, replacement);
                        System.out.println("Обновленная строка: " + inputString);
                    }
                    break;

                case 0:
                    System.out.println("Выход из программы...");
                    exit = true;
                    break;

                default:
                    System.out.println("Неверный выбор. Попробуйте снова.");
                    break;
            }
        }

        scanner.close();
    }
}
```
Задание 14.2
```
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        System.out.print("Введите строку для проверки: ");
        String input = scanner.nextLine();

        String regex = "^abcdefghijklmnopqrstuv18340$";

        if (input.matches(regex)) {
            System.out.println("Строка правильная!");
        } else {
            System.out.println("Строка неправильная.");
        }

        scanner.close();
    }
}
```
Задание 15.4 (Калькулятор на swing, Компилятор - NetBeans IDE 23)
```
package com.mycompany.simplecalculato1;

import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

public class Simplecalculato1 extends JFrame {
    private JTextField display; // Поле текста
    private double firstNumber = 0; // Первое число
    private String operator = ""; // Оператор
    private boolean isOperatorPressed = false; // Проверка нажатия на оператор

    public Simplecalculato1() { // Настройка окна
        setTitle("Simple Calculator");
        setSize(300, 400);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setLayout(new BorderLayout());

        // Поле ввода/вывода
        display = new JTextField();
        display.setEditable(false);
        display.setFont(new Font("Arial", Font.BOLD, 36));
        display.setHorizontalAlignment(SwingConstants.RIGHT);
        display.setPreferredSize(new Dimension(300, 50)); 
        add(display, BorderLayout.NORTH);

        // Панель с кнопками
        JPanel buttonPanel = new JPanel();
        buttonPanel.setLayout(new GridLayout(5, 4, 5, 5));

        // Кнопки цифр и операций
        String[] buttons = {
                "7", "8", "9", "/",
                "4", "5", "6", "*",
                "1", "2", "3", "-",
                "0", ".", "=", "+",
                "C", "^" 
        };

        for (String button : buttons) {
            JButton btn = new JButton(button);
            btn.setFont(new Font("Arial", Font.BOLD, 20));
            buttonPanel.add(btn);
            btn.addActionListener(new ButtonClickListener());
        }

        add(buttonPanel, BorderLayout.CENTER);

        // Отображение окна
        setVisible(true);
    }

    private class ButtonClickListener implements ActionListener {
        @Override
        public void actionPerformed(ActionEvent e) {
            String command = ((JButton) e.getSource()).getText();

            if (command.matches("\\d") || command.equals(".")) {
                if (isOperatorPressed) {
                    display.setText("");
                    isOperatorPressed = false;
                }
                display.setText(display.getText() + command);
            } 
            else if (command.matches("[+\\-*/^]")) { // 
                firstNumber = Double.parseDouble(display.getText());
                operator = command;
                isOperatorPressed = true;
            } 
            else if (command.equals("=")) {
                double secondNumber = Double.parseDouble(display.getText());
                double result = calculate(firstNumber, secondNumber, operator);
                display.setText(Double.toString(result));
                isOperatorPressed = true;
            }
            else if (command.equals("C")) { 
                display.setText(""); // Очищаем дисплей
                firstNumber = 0;
                operator = "";
                isOperatorPressed = false; 
            }
        }

        // Метод для выполнения операций
        private double calculate(double num1, double num2, String operator) {
            switch (operator) {
                case "+":
                    return num1 + num2;
                case "-":
                    return num1 - num2;
                case "*":
                    return num1 * num2;
                case "/":
                    if (num2 != 0) {
                        return num1 / num2;
                    } else {
                        // Обработка деления на ноль
                        JOptionPane.showMessageDialog(null, "Ошибка: деление на ноль!");
                        return 0;
                    }
                case "^": 
                    return Math.pow(num1, num2);
                default:
                    return 0; 
            }
        }
    }

    public static void main(String[] args) {
        SwingUtilities.invokeLater(Simplecalculato1::new);
    }
}
