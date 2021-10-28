package com.example.csc308firstproject;

import javafx.application.Application;
import javafx.event.ActionEvent;
import javafx.event.EventHandler;
import javafx.geometry.Insets;
import javafx.geometry.Pos;
import javafx.scene.Scene;
import javafx.scene.control.Button;
import javafx.scene.control.Label;
import javafx.scene.layout.*;
import javafx.scene.paint.Color;
import javafx.scene.shape.Rectangle;
import javafx.scene.text.Font;
import javafx.scene.text.FontWeight;
import javafx.scene.text.TextAlignment;
import javafx.stage.Stage;

import java.util.ArrayList;

public class Calculator extends Application{
    Label answer;

    Button b0;
    Button b1;
    Button b2;
    Button b3;
    Button b4;
    Button b5;
    Button b6;
    Button b7;
    Button b8;
    Button b9;

    HBox buttonRow1;
    HBox buttonRow2;
    HBox buttonRow3;
    HBox buttonRow4;
    HBox buttonRow5;

    Button bAdd;
    Button bMin;
    Button bMul;
    Button bDiv;

    Button bPI;
    Button bC;
    Button bPercent;
    Button bSign;
    Button bEquals;
    Button bDecimal;

    StringBuilder arg1;
    StringBuilder arg2;
    char curOperator;
    boolean onArg1 = true;
    boolean operatorOkay = false;
    boolean resetOnNum = false;


    public static void main(String[] args) {
        launch(args);
    }


    @Override
    public void start(Stage stage) throws Exception {
        stage.setTitle("Calculator");

        Font arial = Font.font("Arial", FontWeight.BOLD, 35);

        answer = new Label("0");
        answer.setMaxSize(460, 55);
        answer.setMinSize(460, 55);
        answer.setFont(arial);
        Border answerBorder = new Border(new BorderStroke(Color.valueOf("black"), BorderStrokeStyle.SOLID, new CornerRadii(10), BorderWidths.DEFAULT,  new Insets(1)));
        answer.setBorder(answerBorder);
        answer.setWrapText(false);
        answer.setAlignment(Pos.TOP_RIGHT);

        VBox root = new VBox();
        root.setPadding(new Insets(20,20,20,20));
        Scene answerScene = new Scene(root, 500, 550);
        stage.setScene(answerScene);

        b0 = createButton(arial, 0);
        b1 = createButton(arial, 1);
        b2 = createButton(arial, 2);
        b3 = createButton(arial, 3);
        b4 = createButton(arial, 4);
        b5 = createButton(arial, 5);
        b6 = createButton(arial, 6);
        b7 = createButton(arial, 7);
        b8 = createButton(arial, 8);
        b9 = createButton(arial, 9);

        bDiv = createButton(arial, "÷");
        bMul = createButton(arial, "x");
        bMin = createButton(arial, "-");
        bAdd = createButton(arial, "+");

        bC = createButton(arial, "C");
        bEquals = createButton(arial, "=");
        bDecimal = createButton(arial, ".");
        bPercent = createButton(arial, "%");
        bSign = createButton(arial, "+/-");
        bPI = createButton(arial, "π");

        buttonRow1 = createRow(bPI, bPercent, bSign, bC);
        buttonRow2 = createRow(b7, b8, b9, bDiv);
        buttonRow3 = createRow(b4, b5, b6, bMul);
        buttonRow4 = createRow(b1, b2, b3, bMin);
        buttonRow5 = createRow(b0, bDecimal, bEquals, bAdd);

        arg1 = new StringBuilder();
        arg2 = new StringBuilder();
        curOperator = 'C';

        root.getChildren().addAll(answer, buttonRow1, buttonRow2, buttonRow3, buttonRow4, buttonRow5);
        stage.show();
    }

    private HBox createRow(Button but1, Button but2, Button but3, Button but4) {
        HBox newRow = new HBox(20);
        newRow.setPadding(new Insets(20, 0, 0, 0));
        newRow.getChildren().addAll(but1, but2, but3, but4);
        return newRow;
    }

    private Button createButton(Font font, Integer value) {
        Button button = new Button(value.toString());
        button.setMinSize(100, 60);
        button.setMaxSize(100, 60);
        button.setFont(font);
        button.setOnAction(new EventHandler<ActionEvent>() {
            @Override
            public void handle(ActionEvent actionEvent) {
                appendNewNumber(value);
                if (onArg1) {
                    answer.setText(arg1.toString());
                }
                else {
                    answer.setText(arg2.toString());
                }
            }
        });
        return button;
    }

    private Button createButton(Font font, String operator) {
        Button button = new Button(operator);
        button.setMinSize(100, 60);
        button.setMaxSize(100, 60);
        button.setFont(font);
        button.setOnAction(new EventHandler<ActionEvent>() {
            @Override
            public void handle(ActionEvent actionEvent) {
                parseOperation(operator);
            }
        });
        return button;
    }

    private void appendNewNumber(int newNum) {
        if (onArg1) {
            if (arg1.toString().equals("0") || resetOnNum) {
                arg1.setLength(0);
            }
            arg1.append(newNum);
            resetOnNum = false;
            operatorOkay = true;
        }
        else {
            if (arg2.toString().equals("0")) {
                arg2.setLength(0);
            }
            arg2.append(newNum);
            operatorOkay = true;
        }
    }

    private void parseOperation(String operator) {
        switch (operator) {
            case "C":
                resetCalculator();
                break;
            case ".":
                addDecimal();
                break;
            case "π":
                addPI();
                break;
            case "%":
                getPercentage();
                break;
            default:
                if (!operatorOkay) { return; }
        }
        switch(operator) {
            case "+":
            case "-":
            case "x":
            case "÷":
                handleBasicOperator(operator.charAt(0));
                break;
            case "+/-":
                negateAnswer();
                break;
            case "=":
                performOperation(true, '=');
                break;
            default:
                return;
        }

    }

    private void performOperation(boolean equalSign, char newOperator) {
        if (onArg1) {
            return;
        }

        double val1 = Double.parseDouble(arg1.toString());
        double val2 = Double.parseDouble(arg2.toString());

        switch (curOperator) {
            case '+':
                val1 += val2;
                break;
            case '-':
                val1 -= val2;
                break;
            case 'x':
                val1 *= val2;
                break;
            case '÷':
                val1 /= val2;
                break;
            default:
                return;
        }
        arg2.setLength(0);
        arg2.append(0);
        arg1.setLength(0);
        arg1.append(val1);
        answer.setText(arg1.toString());

        if (equalSign) {
            onArg1 = true;
            operatorOkay = true;
            resetOnNum = true;
        }
        else {
            curOperator = newOperator;
            onArg1 = false;
            resetOnNum = false;
        }
    }

    private void resetCalculator() {
        onArg1 = true;
        operatorOkay = false;
        arg1.setLength(0);
        arg2.setLength(0);
        arg1.append(0);
        arg2.append(0);
        answer.setText("0");
    }

    private void addDecimal() {
        if (onArg1 && arg1.indexOf(".") == -1) {
            arg1.append(".");
            answer.setText(arg1.toString());
        }
        else if (arg2.indexOf(".") == -1) {
            arg2.append(".");
            answer.setText(arg2.toString());
        }
        operatorOkay = false;
    }

    private void addPI() {
        if (onArg1) {
            arg1.setLength(0);
            arg1.append("3.14159265359");
            answer.setText(arg1.toString());
        }
        else {
            arg2.setLength(0);
            arg2.append("3.14159265359");
            answer.setText(arg2.toString());
        }
        operatorOkay = true;
    }

    private void getPercentage() {
        if (onArg1) {
            double newVal = Double.valueOf(arg1.toString()) * 0.01;
            arg1.setLength(0);
            arg1.append(newVal);
            answer.setText(arg1.toString());
        }
        else {
            double newVal = Double.valueOf(arg2.toString()) * 0.01;
            arg2.setLength(0);
            arg2.append(newVal);
            answer.setText(arg2.toString());
        }
        operatorOkay = true;
    }

    private void handleBasicOperator(char operator) {
        if (onArg1) {
            curOperator = operator;
            onArg1 = false;
            resetOnNum = false;
        }
        else {
            performOperation(false, operator);
        }
    }

    private void negateAnswer() {
        if (onArg1) {
            if (arg1.charAt(0) == '-') {
                arg1.deleteCharAt(0);
            }
            else {
                arg1.insert(0, "-");
            }
            answer.setText(arg1.toString());
        }
        else {
            if (arg2.charAt(0) == '-') {
                arg2.deleteCharAt(0);
            }
            else {
                arg2.insert(0, "-");
            }
            answer.setText(arg2.toString());
        }
    }

}
