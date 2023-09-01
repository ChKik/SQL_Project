import javafx.application.*;
import javafx.scene.control.Button;
import javafx.scene.text.Font;
import javafx.stage.*;
import javafx.scene.*;
import javafx.scene.layout.*;
import javafx.scene.control.*;
import javafx.geometry.*;
import javafx.scene.paint.*;
import java.sql.*;
import javafx.scene.image.*;

import javax.swing.plaf.basic.BasicViewportUI;


public class Main extends Application {
    TextField username;
    PasswordField password;
    Label loginlbl,patraslbl,athenslbl,thessalonikilbl,banshkolbl,buslbl,welcomescreen,userlbl,passwordlbl;
    Button btnexit,btnenter;

    @Override
    public void start(Stage primaryStage) {
        try {
            primaryStage.setTitle("Kikidis Travel Agency");  //einai to root pane poy tha mpoyne ola ta ypoloipa
            BorderPane root = new BorderPane();



            //left edw tha valw ta buttons
            StackPane leftPane = new StackPane();
            leftPane.setBackground(new Background(new BackgroundFill(Color.DIMGRAY,CornerRadii.EMPTY,Insets.EMPTY)));
            btnexit=new Button("Exit");
            btnexit.setAlignment(Pos.TOP_CENTER);
            btnexit.setLayoutX(100);
            btnexit.setLayoutY(100);
            btnexit.setPrefSize(100,50);
            btnexit.setFont(Font.font("Verdana",25));
            btnexit.setOnAction(e->System.exit(0));
            leftPane.getChildren().add(btnexit);
            root.setLeft(leftPane);

            //right edw eikones

            VBox rightPane = new VBox();
            patraslbl=new Label();
            thessalonikilbl=new Label();
            athenslbl=new Label();
            banshkolbl=new Label();
            buslbl=new Label();

            Image patra=new Image("Patra.png");
            Image thessaloniki=new Image("Thessaloniki.png");
            Image athens=new Image("Athens.png");
            Image banshko=new Image("banshko.png");
            Image bus=new Image("bus.png");
            ImageView patraview=new ImageView(patra);
            ImageView thessalonikiview=new ImageView(thessaloniki);    //eikones
            ImageView banshkoview=new ImageView(banshko);
            ImageView busview=new ImageView(bus);
            ImageView athensview=new ImageView(athens);

            patraview.setFitWidth(270);
            patraview.setPreserveRatio(true);
            patraview.setSmooth(true);
            athensview.setFitWidth(270);
            athensview.setPreserveRatio(true);
            athensview.setSmooth(true);
            thessalonikiview.setFitWidth(270);
            thessalonikiview.setPreserveRatio(true);            //edit eikones
            thessalonikiview.setSmooth(true);
            banshkoview.setFitWidth(270);
            banshkoview.setPreserveRatio(true);
            banshkoview.setSmooth(true);
            busview.setFitWidth(270);
            busview.setPreserveRatio(true);
            busview.setSmooth(true);

            rightPane.setBackground(new Background(new BackgroundFill(Color.DIMGRAY,CornerRadii.EMPTY,Insets.EMPTY)));
            rightPane.getChildren().addAll(patraview,thessalonikiview,busview,athensview,banshkoview);
            rightPane.setAlignment(Pos.CENTER);
            rightPane.setPadding(new Insets(20));
            root.setRight(rightPane);

            //center edw tha einai ta login ktlp
            VBox centerPane = new VBox();

            centerPane.setBackground(new Background(new BackgroundFill(Color.DIMGRAY,CornerRadii.EMPTY,Insets.EMPTY)));
            welcomescreen=new Label("Welcome to Kikidis Travel Agency");
            welcomescreen.setFont(Font.font("Arial Bold",40));
            welcomescreen.setTextFill(Color.color(0.1,0.1,0.2));
            welcomescreen.setLayoutY(300);

            userlbl=new Label("Username:");
            passwordlbl=new Label("Password:");
            userlbl.setFont(Font.font("Verdana",14));
            passwordlbl.setFont(Font.font("Verdana",14));
            username=new TextField();
            password=new PasswordField();
            username.setPromptText("eg KostasPapadopoylos");
            username.setPrefWidth(80);
            password.setPrefWidth(80);
            btnenter=new Button("Login");
            btnenter.setLayoutY(50);

            HBox pane1=new HBox();
            pane1.setAlignment(Pos.CENTER);
            HBox pane2=new HBox();                          //ftiaxnw 2 pane gia ta stoixeia toy xrhsth
            pane2.setAlignment(Pos.CENTER);
            pane1.getChildren().addAll(userlbl,username);
            pane2.getChildren().addAll(passwordlbl,password);

            btnenter.setDefaultButton(true);  //molis pathsei enter o xrhsths energopoieitai to button
            btnenter.setOnAction(e->getUserPass());
            centerPane.getChildren().addAll(welcomescreen,pane1,pane2,btnenter);
            centerPane.setAlignment(Pos.TOP_CENTER);
            centerPane.setPadding(new Insets(40));
            root.setCenter(centerPane);

            // scene me to  BorderPane
            Scene scene = new Scene(root, 1200, 900);
            primaryStage.setScene(scene);
            primaryStage.setResizable(false);
            primaryStage.show();

        }
        catch(Exception e){
            System.out.println(e.getMessage());
        }
    }
            //tha checkarei an einai o admin aytos sto server kai bgazei katallhlo munima
    public void getUserPass(){
        Label successlbl= new Label("Successful login!");
        Button fail=new Button("Login Failed.Check you credentials");
        Button continuebtn =new Button("Press to continue");
        continuebtn.setAlignment(Pos.CENTER);
        continuebtn.setFont(Font.font("Verdana"));

        HBox h1=new HBox(successlbl,continuebtn);
        HBox h2=new HBox(fail);
        Scene popup=new Scene(h1);
        Scene tempscene2=new Scene(h2);
        Pane testpane= new Pane();
        testpane.setBackground(new Background(new BackgroundFill(Color.BLACK,CornerRadii.EMPTY,Insets.EMPTY)));


        StageStyle style= StageStyle.UTILITY;
        Modality modality= Modality.APPLICATION_MODAL;

        Stage popupwindow=new Stage();

        String user;
        String pass;
        user=username.getText();
        pass=password.getText();

         boolean userauthentication;
         SQLengine ref=new SQLengine();
         userauthentication=ref.Authentication(user,pass);  //epistrefei true ama yparxei o user

        popupwindow.initStyle(style);
        popupwindow.initModality(modality);
        if(userauthentication){
            popup.setFill(Color.TRANSPARENT);
            h1.setStyle("-fx-background-color: rgba(0,0,0,0);");   //xrwmata me css
           continuebtn.setOnAction(e->ref.itscreen());


           popupwindow.setScene(popup);


        }
        else {
            popup.setFill(Color.TRANSPARENT);
            h1.setStyle("-fx-background-color: rgba(0,0,0,0);");

            popupwindow.setScene(tempscene2);
        }
        popupwindow.show();
    }



    public static void main(String[] args) {
        launch(args);
    }
}