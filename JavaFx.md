JavaFx
======

## Resources:

For a lot of custom JavaFX elements enhancing the basic elements check
[this](http://www.drdobbs.com/jvm/javafx-8-combining-new-language-features/240168210).

## Basics

- The main class for a JavaFX application extends the javafx.application.Application class.
The start() method is the main entry point for all JavaFX applications.
- A JavaFX application defines the user interface container by means of a STAGE and a SCENE.
    - The JavaFX Stage class is the top-level JavaFX container. Stage extends Class Window.
    - The JavaFX Scene class is the container for all content. Scene listens to stuff like resize and changes content accordingly.

    A stage (which is usually a window) contains a scene, which in turn contains all other FXML elements.
    A stage can switch between different scenes.


- A javafx application window always has to have a root container as first child(?)

- the constructor of a controller class is always called before the initialize() method

- after the corresponding fxml file connected to a controller is loaded, the initialize() method in the controller is executed.

- if we use lists, tables etc, we need to tell what should be contained where. Cell factories in the initialize method can be used for that.

        @FXML
        private TableView<Person> personTable;
        @FXML
        private TableColumn<Person, String> firstNameColumn;
        @FXML
        private TableColumn<Person, String> lastNameColumn;

        public void initialize() {
            firstNameColumn.setCellValueFactory(cellData -> cellData.getValue().firstNameProperty());
            lastNameColumn.setCellValueFactory(cellData -> cellData.getValue().lastNameProperty());
        }


- [FXML tutorial](https://docs.oracle.com/javafx/2/get_started/fxml_tutorial.htm)
- [JFX overviw](https://docs.oracle.com/javase/8/javafx/get-started-tutorial/jfx-overview.htm)


FXML
====

## Introduction

Introduction to FXML by Oracle (its pretty good, actually) can be found
[here](https://docs.oracle.com/javase/8/javafx/api/javafx/fxml/doc-files/introduction_to_fxml.html#overview).


## Basics

In FXML, an XML element represents one of the following:

- A class instance
- A property of a class instance
- A "static" property
- A "define" block
- A block of script code

## The FXML loader
The FXML loader is executed after the constructor of the corresponding controler class has been called and
before the initialize method of the controller is executed. So stuff, that is created by the FXML loader can
only be accessed in the initialze method, not in the constructor of the controller class.

## Instantiating Elements in FXML
There are three possible ways to do exactly that:

### Instance declarations (of Bean types)
    <?import javafx.scene.control.Label?>
    <Label />

An element tag is considered an instance declaration, if it starts with a capital letter and the corresponding
class is imported in the file header. When the FXML loader encounters such an element, it creates an instance
of that class (using the default constructor?).

Properties of a class can be accessed vie XML attributes. e.g.:

    <Label text="I sneakily access the Label text property when my label is constructed!" />

Bean types are constructed when an elements start tag is processed.

### Instantiation by fx:factory

This attribute is used to instantiate classes w/o default constructor e.g.:

    <FXCollections fx:factory="observableArrayList">
        <String fx:value="A"/>
        <String fx:value="B"/>
        <String fx:value="C"/>
    </FXCollections>

### Instantiation by Builders

FXML provides two interfaces, the Builder (construct the object) and the BuilderFactory (instantiate a given type)
interface to create instances of classes not conform to Bean conventions.

Builder types are constructed only when an element's end tag has been processed (in contrast to Bean types
that are constructed after the start tag has been processed)


## `<fx:include>`

The `<fx:include>` tag creates an object from FXML markup defined in another file. It is used as follows:

    <fx:include source="filename"/>

Where filename is the name of the FXML file to include. Values that begin with a leading slash character
are treated as relative to the classpath. Values with no leading slash are considered relative to the path
of the current document.

## `<fx:constant>`

The `<fx:constant>` element creates a reference to a class constant. For example, the following markup sets
the value of the "minWidth" property of a Button instance to the value of the NEGATIVE_INFINITY constant defined
by the java.lang.Double class:

    <Button>
        <minHeight><Double fx:constant="NEGATIVE_INFINITY"/></minHeight>
    </Button>


## FXML Property Elements

Elements whose tag names begin with a lowercase letter represent object properties. A property element may represent
one of the following:
- A property setter
- A read-only list property
- A read-only map property

Tags starting with a lowercase letter within a class element are considered property setters

    <!-- this accesses the setter of a label text property; -->
    <Label>
        <text>Hello, World!</text>
    </Label>


FXML uses "type coercion" using the BeanAdapter coerce() method to convert what it encounters
in the FXML from String to Class/Enum/boolean or int to double.
Custom conversions can be implemented by defining a static valueOf() method on the target type
(but where and how? nobody knows the troubles I have seen...)


## Controller method event handlers

Methods associated with event handlers have to be accessible in the corresponding controller and can be accessed
by using a hash followed by the methods name in the FXML file.
This requires, that the root element of the FXML file contains the fx:controller attribute connecting the FXML
file to a specific controller where the method is defined.

    <VBox fx:controller="com.foo.MyController"
        xmlns:fx="http://javafx.com/fxml">
        <children>
            <Button text="Click Me!" onAction="#handleButtonAction"/>
        </children>
    </VBox>


NOTE: a handler method should take a single argument of a type that extends javafx.event.Event and should return void.

Event handlers can also be implemented thus:

    <!-- in the FXML file -->
    <VBox fx:controller="com.foo.MyController"
        xmlns:fx="http://javafx.com/fxml">
        <children>
            <Button text="Click Me!" onAction="$controller.onActionHandler"/>
        </children>
    </VBox>

    // in the corresponding controller:
    public class MyController {
        @FXML
        public EventHandler onActionHandler = new EventHandler<>() { ... }
        ...
    }


## Controllers
The fx:controller attribute in the root tag of the fxml file connects the fxml with a java controller class. 

The controller class can contain the following method, which is called,
once the content of the fxml file is completely loaded:

    public void initialize() {}

This allows post loading processing of content of the fxml file.

Controller member fields or methods can be defined as private, but can still be associated
with the contents of the FXML file, if they are prefixed with @FXML at their declaration.


## Nested controllers

If a main FXML file includes a second FXML file, the first controller can get access to the controller
of the second FXML file:

    <!-- main FXML file -->
    <VBox fx:controller="com.foo.MainController">
       <fx:define>
          <fx:include fx:id="dialog" source="dialog.fxml"/>
       </fx:define>
       ...
    </VBox>

    // main controller
    public class MainController extends Controller {
        @FXML private Window dialog;
        @FXML private DialogController dialogController;
        ...
    }

When the controller's initialize() method is called, the dialog field will contain the root element
loaded from the "dialog.fxml" include, and the dialogController field will contain the include's controller.
The main controller can then invoke methods on the included controller, to populate and show the dialog, for example.
Note that as the content of the file referenced by fx:include otherwise would become part of the scene graph spanned
from main_window_content.fxml, it is necessary to wrap fx:include by fx:define to separate
the scene graphs of both windows.


## The FXMLLoader

The FXML loader is responsible for actually loading an FXML source file and returning the resulting object graph.
Internally FXMLLoader uses the javafx.xml.stream API (StAX, efficient XML parsing API) to load an FXML document.
The XML document is processed in a single pass and is not loaded into an intermediate DOM (document object model)
structure.















