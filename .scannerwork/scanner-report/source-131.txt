package com.example.csc308firstproject;

import javafx.application.Application;
import javafx.event.ActionEvent;
import javafx.event.EventHandler;
import javafx.scene.Scene;
import javafx.scene.control.Button;
import javafx.scene.control.Label;
import javafx.scene.layout.VBox;
import javafx.scene.web.WebEngine;
import javafx.scene.web.WebView;
import javafx.stage.Stage;

import java.io.IOException;

public class FirstFX extends Application {

    Label Label1;
    Button button1;
    int i = 1;

    public static void main(String[] args) {
        launch(args);
    }


    @Override
    public void start(Stage stage) throws Exception {
        stage.setTitle("My First Stage Title");
        Label1 = new Label("My first Label");
        VBox root = new VBox();
        Scene scene = new Scene(root, 400, 800);
        stage.setScene(scene);

        button1 =  new Button("My first button");
        button1.setOnAction(new EventHandler<ActionEvent>() {
            @Override
            public void handle(ActionEvent actionEvent) {
                System.out.println("Hello World!!!");
                Label1.setText("Try" + i);
                i++;
            }
        });

        WebView webView = new WebView();
        WebEngine webEngine = webView.getEngine();
        webEngine.load("https://www.calculator.net/");

        root.getChildren().addAll(Label1, button1, webView);
        stage.show();
    }
}
