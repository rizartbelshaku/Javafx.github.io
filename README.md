package pacman;

import javafx.animation.Animation;
import javafx.animation.KeyFrame;
import javafx.animation.Timeline;
import javafx.application.Application;
import javafx.geometry.Bounds;
import javafx.scene.Scene;
import javafx.scene.control.Label;
import javafx.scene.image.Image;
import javafx.scene.image.ImageView;
import javafx.scene.layout.AnchorPane;
import javafx.scene.paint.Color;
import javafx.scene.shape.Circle;
import javafx.stage.Stage;
import javafx.util.Duration;

public class Pacman extends Application {

    private int score = 0;
    private Label scoreLabel;
    private ImageView player1;
    private Circle player2;
    private ImageView player3;
    private ImageView player4;
    private ImageView player5;
    private ImageView player6;
    private ImageView gameOverImage;
    private boolean gameEnded = false;

    public static void main(String[] args) {
        launch(args);
    }

    @Override
    public void start(Stage primaryStage) {
        primaryStage.setTitle("Mini Game");
        Image backgroundImage = new Image("desktop-1366x768.png");
        ImageView backgroundImageView = new ImageView(backgroundImage);
        
        

        AnchorPane root = new AnchorPane();
        Scene scene = new Scene(root, 800, 800);
        backgroundImageView.fitWidthProperty().bind(scene.widthProperty());
        backgroundImageView.fitHeightProperty().bind(scene.heightProperty());
        root.getChildren().add(backgroundImageView);
        
      
        player1 = createPlayer("imageee.gif.png");
        player2 = createPlayer(Color.YELLOW);
        player3 = createPlayer("Ima.gif.png");
        player4= createPlayer("Ima.gif.png");
        player5=createPlayer("Ima.gif.png");
        player6=createPlayer("anyrgb.com.png");

        root.getChildren().addAll(player1, player2, player3,player4,player5,player6);

        scoreLabel = new Label("Score: 0");
        scoreLabel.setLayoutX(10);
        scoreLabel.setLayoutY(10);
        root.getChildren().add(scoreLabel);

        gameOverImage = createGameOverImage();
        gameOverImage.setVisible(false);
        root.getChildren().add(gameOverImage);

        movePlayer(scene);

        primaryStage.setScene(scene);
        primaryStage.show();
    }

    private ImageView createPlayer(String imagePath) {
        Image image = new Image(imagePath);
        ImageView player = new ImageView(image);
        player.setFitWidth(70);
        player.setFitHeight(70);
        player.setLayoutX(Math.random() * 360+20);
        player.setLayoutY(Math.random() * 360+20);
        return player;
    }

    private Circle createPlayer(Color color) {
        Circle player = new Circle(30, color);
        player.setLayoutX(Math.random() * 860+20);
        player.setLayoutY(Math.random() * 860+20);
        return player;
    }

    private ImageView createGameOverImage() {
          Image gameOverImg = new Image("Game over.png");
        ImageView imageView = new ImageView(gameOverImg);
        imageView.setLayoutX(50);
        imageView.setLayoutY(150);
        return imageView;
    }
    private void movePlayer(Scene scene) {
        Timeline timeline = new Timeline(new KeyFrame(Duration.millis(10), event -> {
            if (gameEnded) {
                return;
            }

            double player3Angle = player3.getRotate() + 1;
            double player3X = Math.cos(Math.toRadians(player3Angle)) * 700 + 300;
            double player3Y = Math.sin(Math.toRadians(player3Angle)) * 700 + 300;
            player3.setRotate(player3Angle);
            player3.setLayoutX(player3X);
            player3.setLayoutY(player3Y);

            double player4Angle = player4.getRotate() - 1;
            double player4X = Math.cos(Math.toRadians(player4Angle)) * 500+ 200;
            double player4Y = Math.sin(Math.toRadians(player4Angle)) * 500 + 200;
            player4.setRotate(player4Angle);
            player4.setLayoutX(player4X);
            player4.setLayoutY(player4Y);
            
            double player5Angle=player5.getRotate()+1;
            double player5X=Math.cos(Math.toRadians(player5Angle))*500+200;
            double player5Y=Math.sin(Math.toRadians(player5Angle))*500+200;
            player5.setRotate(player5Angle);
            player4.setLayoutX(player5X);
            player5.setLayoutX(player5Y);
            
            double player6Angle=player6.getRotate()-1;
            double player6X=Math.cos(Math.toRadians(player6Angle))*700+300;
            double player6Y=Math.sin(Math.toRadians(player6Angle))*700+300;
            player6.setRotate(player6Angle);
            player6.setLayoutX(player6X);
            player6.setLayoutX(player6Y);


            checkCollision();
        }));
        timeline.setCycleCount(Animation.INDEFINITE);
        timeline.play();

        scene.setOnKeyPressed(event -> {
            if (gameEnded) {
                return;
            }
            double player1X = player1.getLayoutX();
            double player1Y = player1.getLayoutY();

            switch (event.getCode()) {
                case UP:
                    if (player1Y > 0) {
                        player1.setLayoutY(player1Y - 10);
                    }
                    break;
                case DOWN:
                    if (player1Y < 1000) {
                        player1.setLayoutY(player1Y + 10);
                    }
                    break;
                case LEFT:
                     if (player1X > 0) {
                        player1.setLayoutX(player1X - 10);
                    }
                    break;
                case RIGHT:
                    if (player1X < 1000) {
                        player1.setLayoutX(player1X + 10);
                    }
                    break;
            }

            checkCollision();
        });
    }

    private void checkCollision() {
        Bounds player1Bounds = player1.getBoundsInParent();
        Bounds player2Bounds = player2.getBoundsInParent();
        Bounds player3Bounds = player3.getBoundsInParent();
        Bounds player4Bounds = player4.getBoundsInParent();
        Bounds player5Bounds= player5.getBoundsInParent();
        Bounds player6Bounds=player6.getBoundsInParent();

        if (player1Bounds.intersects(player2Bounds)) {
            score++;
            scoreLabel.setText("Score: " + score);

            player2.setLayoutX(Math.random() * 500 + 20);
            player2.setLayoutY(Math.random() * 500 + 20);
        } else if (player1Bounds.intersects(player3Bounds)) {
            gameOver();
        }else if (player1Bounds.intersects(player4Bounds)){
            gameOver();
        }else if(player1Bounds.intersects(player5Bounds)){
            gameOver();
        }else if(player1Bounds.intersects(player6Bounds)){
            gameOver();
        }
    }

    private void gameOver() {
        gameEnded = true;
        player1.setVisible(false);
        player2.setVisible(false);
        player3.setVisible(false);
        player4.setVisible(false);
        player5.setVisible(false);
        scoreLabel.setVisible(false);
        gameOverImage.setVisible(true);
    }
}
