# OOSD Summary
## Hoofdstuk 1 Overerving
primitieve var -> waarde  
referentie var -> instantie (klasse, array)  
referentie var -> null (leeg, niets)  

SUBKLASSE -> ```extends```   
Check of subklasse is -> ```instanceof```  

OPERATOR "==" vergelijkt waarden, primitief of adressen in geheugen van referentievariabelen  
METHODE equals vergelijkt op basis van inhoud  
--> indien niet overschreven: overerven van Object Klasse  
--> overschrijven is zelf bepalen waarop vergeleken wordt = source - Generate hashCode() and equals()

Let op constructor in subklasses! Moet constructor van SUPERklasse aanroepen

Methode binnen subklasse overschrijven --> ```@override```   
--> methode van superklasse aanroepen binnen subklasse --> ```super.``` 

**Abstracte klasse** = kan geen instanties maken van deze klasse  
--> (*cursief in UML*)  
--> ```public abstract class``` 
**Abstracte methode** = methode zonder implementatie, enkel signatuur  
--> (*cursief in UML*)  
--> ```public abstract Type naam();``` (let op ; en geen {})  

Attribuut **static**: er bestaat slechts 1 instantie van attribuut = klassevariabele  
--> wordt gedeeld over alle instanties, ongeacht aantal  
--> onderlijnt in UML  
--> om methode te gebruiken moet EERST een object aangemaakt worden

Keyword **final**
- Attribuut: 
  - final klassevariabele: waarde toekennen bij declaratie
  - final instantievariabele: waarde toekennen binnen constructor
- Variabele: lokaal in methode, eenmalig waarde toegekend
- Methode: vb setter, voorkomen dat methode kan overschreven worden in subklasse

## Hoofdstuk 2 Polymorfisme & Interfaces
**Polymorfisme** = veelvormigheid, komt voort uit de 'IS EEN' relatie  
- Upcasting
- Downcasting  
![image](https://user-images.githubusercontent.com/68321900/119004300-b9e1a700-b98e-11eb-8836-687f70fe762b.png)  
Later wordt motor uitgebreidt:  
![image](https://user-images.githubusercontent.com/68321900/119004361-c6fe9600-b98e-11eb-8ded-fa193b11e429.png)  
![image](https://user-images.githubusercontent.com/68321900/119004397-d1209480-b98e-11eb-9839-d9287f0218f5.png)  
Let op hoe we nu de motor doorgeven bij aanmaken object:  
![image](https://user-images.githubusercontent.com/68321900/119004471-e5fd2800-b98e-11eb-8426-53b4972b4e15.png)  

**Interfaces**
= referentietype, klasse, maar kan enkel het volgende bevatten:
- constanten
- abstracte methodes (zonder private,default, static, ...)  
![image](https://user-images.githubusercontent.com/68321900/119005419-ba2e7200-b98f-11eb-8aca-7c22e7647ebd.png)  
- default methodes met implementatie
- static methodes met implementatie

UML: ```<<Interface>>```

Implementatie gebeurt in subklasse, je belooft dit: ```implemets```  
![image](https://user-images.githubusercontent.com/68321900/119005834-15f8fb00-b990-11eb-87e5-4cdb8c96e7b5.png)  

VOORDEEL: een klasse kan **meerdere** interfaces implementeren maar je belooft om de abstracte methodes van die interface te implementeren

**Sorteren**
- Comparable interface: natuurlijke sorteerwijze  
  - Resultaat int < 0 --> object waar methode op aangeroepen wordt is KLEINER dan vergeleken
  - Resultaat int = 0 --> object waar methode op aangeroepen wordt is GELIJK aan vergeleken
  - Resultaat int > 0 --> object waar methode op aangeroepen wordt is GROTER dan vergeleken
```
public interface Comparable<T> {
  public int compareTo(T o);
}
//in klasse & als methode
public class X implements Comparable<X> {
  public int compareTo(X o) {
    int getal = this.attribute.compareTo(o.getAttribute()); //we onthouden de waarde van EERSTE sortering
    //we sorteren verder indien eerste gelijk is
    return getal == 0 ? this.otherAttribute.compareTo(o.getOtherAttribute()) : getal; 
  }
}

//use the method
Collections.sort(list of objects);
```
- Comparator interface: zelf gekozen sorteerwijze 
```
public interface Comparator<T> {
  int compare(T o1, T o2);
}

//vb:
public class FirstNameComparator implements Comparator<Name> {
  public int compare(Name o1, Name o2) {
    int firstCmp = o1.getFirstName().compareTo(o2.getFirstName());
    return (firstCmp != 0 ? firstCmp : o1.getLastName().compareTo(o2.getLastName()));
  }
}

//use the method
Collections.sort(list of objects, new FirstNameComparator());
```

## Hoofdstuk 3 Lambda expressies
**Anonieme innerklasse**  
Een anonieme innerklasse is een klasse die binnen een methode wordt gedeclareerd maar geen naam heeft. De syntax van een anonieme inner klasse expressie is analoog aan het aanroepen van een constructor, behalve dat deze constructor vervangen wordt door een klasse definitie binnen een code blok.  
```
HelloWorld frenchGreeting = new HelloWorld() { --> aanroepen van de interface klasse
    public void greetSomeone(String someone) {
        System.out.println("Yooo " + someone);
    }
}

//uitvoer
frenchGreeting.greetSomeone("Robin");
```
**Functionele interface**  
Een functionele interface is een interface met exact 1 abstracte methode.  
```
@FunctionalInterface
public interface MyInterface{
    // the single abstract method
    String reverse(String n);
}
```
**Lambda expressie**  
Lambda expressies kunnen enkel gebruikt worden om functionele interfaces te implementeren. Dit is een expressie die je zoals anonieme klassen toelaat om in een stap een functionele interface te declareren en te instantiëren.
```
// Een lambda expressie bevat volgende onderdelen:
(parameterlijst) -> {statements}

//Vb 1
HelloWorld frenchGreeting = someone -> System.out.println("Yooo " + someone);

//uitvoer
frenchGreeting.greetSomeone("Robin");

//Vb 2
MyInterface ref = (str) -> {
    String result = "";
    for(int i = str.length();-1; i >= 0; i--){
        result += str.charAt(i);
    }
    return result;
}
```
**Method reference**  
Method referenties laten je toe om te refereren naar een bestaande methode gebruik makende van zijn naam. Op die manier zijn methode referenties compacte en eenvoudig leesbare lambda expressies voor methodes die al een naam hebben.  
LET OP: returntype en parameterlijst zijn belangrijk --> moet hetzelfde zijn als abstracte methode uit interface  
```
//implementatie als het over een static methode gaat en dus geen object nodig om methode aan te roepen
HelloWorld fg = HelloWorldApp::printGreeting; 
fg.greetSomeone("Fred");

//zelfde maar een gewone methode (niet static) dus er moet een object worden aangemaakt
HelloWorldApp hwa = new HelloWorldApp(); 
HelloWorld fg2 = hwa::printGreeting2;
fg2.greetSomeone("Sara");

//parameter en returntype moet hetzelfde zijn als zoals die van in de interface
private static void printGreeting(String someone) { 
	System.out.println("Salut " + someone);
}
	
private void printGreeting2(String someone) {
	System.out.println("Salut " + someone);
}
```

## Hoofdstuk 4 Exception handling
```
//maak gebruik van do while
boolean flag = true;

do {
   try {
   	...
	...
	...
	flag = false;
   } catch (...) {
   
   } catch (...) {
   
   }
} while(flag);
```
Let op als je vb met tekst werkt, gebruik ```scanner.nextLine``` in catch blok om dat verkeerde stuk tekst in te lezen.  
--> anders oneindige lus  

Om eigen ExceptionKlasse te maken: nieuwe package "exceptions" --> naam volgens UML  
(JAVA-regel: we laten alles eindigen op -exception)  
- Binnen klasse: ```extends (naam gegeven van exceptie in opgave)```  
- Source -> generate constructors from superclass
- Enkel nog boodschap aanpassen in default constructor

Deze kan nu vb gebruikt worden bij een methode: ```throw new naamException();```  
Bij catch-blokken: let op volgorde! Als klasse waarvan erft boven eigen exception staat zal deze nooit bereikt worden  

Let op met Checked VS Unchecked Exceptions:
- Checked (roze): compiler gaat **altijd** verifieren of jij de exceptie wel afhandelt
  - Je **moet** deze afhandelen = ```try catch```
  - Ofwel ben je je bewust dat deze exception komt en laat je dit ook weten aan programma: ```Throws declaratie```
- Unchecked (blauwe)

## Hoofdstuk 5 JavaFX
Project aanmaken: New - Other - JavaFX - JavaFX Project  
Create module-info.java mag aangevinkt blijven (Kan je ook zelf laten genereren)  
Libraries - Modulepath - Add external JARs - base/controls/fxml/graphics  
Package name: main  
Finish

![image](https://user-images.githubusercontent.com/68321900/120315836-0d33ed80-c2dd-11eb-879a-81d83330c41d.png)  

### Soorten elementen
- Label
- TextField
- PasswordField
- Tooltip
- Button
- Hyperlink

### StartUp
```Object root = new Object()``` Maak een object van je scherm/layout  
```Scene scene = new Scene(root, px, px)``` Maak je scene aan

In aparte packages maken we onze schermen, die extenden van een Layout Lib (Pane, VBox, HBox, etc.)  
- Maak constructor zonder parameters  
 - Methode buildGui()  
- Werk methode uit  
 - Maak items (label, buttons, etc.)  
 - Foto is ```ImageView naam = new ImageView(new Image(getClass().getResourceAsStream("path")));``` \ = root = src  
- Schik items op scherm met coordinaten: ```item.setLayoutX(px);``` ```item.setLayoutY(px);```  
- Koppeling leggen: ```this.getChildren.addAll(alle nodes);```  
- Creer eventueel module-info.java (zelfde naam als project)  (Configure - ...)  

Bij CLASS NOT FOUND error: Run as - Run Configurations - Java Application, verwijder lijst (moet nu werken)  
Om gebruik te maken van SceneBuilder: op je gui packages (waar je schermen maakt) - New - JavaFX - New FXML Document - RC - Open with SceneBuilder  
- Save file als je klaar bent  
- Ga in FXML file staan - Klik rechts - Source - Generate Controller  
- Belangrijk dat deze file erft van **dezelfde** layout  
- Voeg contructor toe, from superclass zonder parameters:  
```
FXMLLoader loader = new FXMLLoader(getClass().getResource("name.fxml"));
loader.setController(this);
loader.setRoot(this);

try {
  loader.load()
} catch (IOException e) {
  throw new RuntimeException(e);
}
```
- Zorg ook voor juiste naam in StartUp  
- Maak je module-info en voeg manueel ```requires javafx.controls;``` toe  

### Gridpane
- Aligneer grid in midden ```this.setAlignment(Pos.BOTTOM_LEFT);```  
- Ruimte tussen kolommen ```this.setHgap(10);```  
- Ruimte tussen rijen ```this.setVgap(10);```  
- Ruimte rond/van randen ```this.setPadding(new Insets(25,25,25,25));```  
Om elementen toe te voegen:  
```
//3 parameters
this.add(naam, kolom, rij);
//5 parameters
this.add(naam, kolom, rij, kolspan, rijspan);
```
- Elementen aligneren: ```setHalignment(element, HPos.LEFT);```  
- ToolTip:  
```
Tooltip t = new Tooltip();
t.setText("blabla");
element.setTooltip(t);
```

### EventHandling
1) Event listener registreren op je component  
2) Implementeer een event-handling methode  
- Method ref (SceneBuilder)  
```
//Deze vereist een methode binnen zelfde klasse met zelfde returntype als interface
element.setOnAction(this::methodeNaam);
```  
Via SceneBuilder  
- Bij properties rechts ga je naar Code  
- Geef fx:id, vb btnKlikHier  
- OnAction: naam van methode  
- Safe en genereer controller opnieuw, kopieer eerst wat je al had!!  
- Werk onAction methode uit  
- Module-info: add ```opens gui;```  

- Lambda expressie  
```
element.setOnAction(evt -> acties);
```
- Anonieme innnerklasse
```
element.setOnAction(new Eventhandler<ActionEvent>() {
  @Override
  public void handle(ActionEvent evt) {
    beschrijf acties
  }
}
```

### Wisselen van scherm
Deze heeft sowieso een tweede file nodig onder package met vb naam tweedeScherm  
Zorg er voor dat de constructor van je tweede scherm sws 1 parameter meekrijgt, nl. een scene

In de methode van je eerste scherm, waar je na actie een nieuw scherm wilt openen:  
- Maak object van tweede scherm aan ```naamTweedeScherm ts = new naamTweedeScherm(parameters, this)```  
 - Let op the ```this```, hier geef je je volledig eerste scherm mee om te kunnen terugkeren  
- Maak nieuwe scene: ```Scene scene = new Scene(ts, px, px);```  
- Vraag stage op: ```Stage stage = (Stage) this.getScene().getWindow();```  
- Set stage: ```stage.setScene(scene);```  

In methode van je terugkeerknop eventhandles van je tweede scherm:
```
btnTerug.setOnAction(evt -> {Stage stage = (Stage) (getScene().getWindow());
				stage.setScene(login.getScene());
			    });
```  

### Menu balk
Zie Hoofdstuk 5 voorbeeld 5

### Alert scherm
```
Alert alert = new Alert(Alertype.NAAMTYPE);
alert.setTitle("");
alert.setHeaderText("");
alert.setContentText("");
alert.showAndWait();

//Als type CONFIRMATION is, krijg je 2 opties
//Sla antwoord van showAndWait op!
Optional<ButtonType> result = alert.showAndWait();
if(result.get() == ButtonType.OK) {
  blabla
} else {
  blabla
}
```
 
## Hoofdstuk 6 Collections
Belangrijk om te weten: Array hoort **niet** thuis in Collection framework!  
--> zie conversions

Collections overlopen:
- streams
- for-each loops: tijdens itereren **geen wijzigingen**
  - enhanced for
- iterators: **optioneel** er kunnen elementen verwijderd worden
  - ```hasNext() of hasPrevious()```
  - ```next()```
  - ```remove```

In java code:  
```
//enhanced for
public void iterateWithEnhancedFor(Collection<String> colors) {
  for (String color : colors) {
    System.out.printf("%s ", color);
  }
}
```
```
//for each
public void iterateWithEnhancedFor(Collection<String> colors) {
  colors.forEach(c -> System.out.printf("%s ", c));
}
```
```
//iterator
public void iterateWithIterator(Collection<String> colors) {
  Iterator<String> iterator = colors.iterator();
  while (iterator.hasNext()) {
    System.out.printf("%s ", iterator.next());
  }
}
```
### List
- ArrayList: veel opzoekingen
- LinekdList: veel invoegen/verwijderen  

### Iterators
iterator --> kan enkel elementen verwijderen
listIterator --> kan wijzigen/toevoegen/verwijderen  

## Hoofdstuk 7 Streams
Voor het gebruik van een stream vertrekken we altijd van een Array of een Collection  

#Intermediate operations
```.filter()``` --> gebruikt om te filteren, returns boolean, if true = added to stream (eg. filter(age > 18);) (=Predicate)  
```.map()``` --> takes in one parameter and transforms elements(eg. map(e -> e.toUpperCase());) (=Function)  
```disctinct()``` --> unieke elemeten uit oorspronkelijke stream, wordt gebruikt gemaakt van **equals** methode  
```sorted()``` --> 2 types:
- Natuurlijk: zoals methode hier boven (sorted()) --> compare methode = comparable interface implementeren
- Zelf gekozen: comparator methode --> interface implementeren  

#Terminal operations
```sum()```
```count()```
```min()``` --> Returns OptionalInt	```min(comparator)``` = eerste ordenen dan min bepalen
```max()``` --> Returns OptionalInt	```max(comparator)``` = eerste ordenen dan max bepalen
- Use ```getAsInt/Long/Double()``` ```orElse()``` ```isPresent()```  
- De klasse Optional heeft ook een methode map vb (.map(e -> e.getName());)  
![image](https://user-images.githubusercontent.com/68321900/120065419-7adbe180-c071-11eb-92a0-e4c3ef1a12e5.png)  
```anyMatch()``` --> true als een element voldoet aan voorwaarde
```allMatch()``` --> true als **alle** elementen voldoen aan voorwaarde
```forEach()``` --> actie op elk element van stream, kan vanalles zijn maar deze methode is **termninal**
```reduce()``` --> alle elementen samengevat tot 1 element  

#Stream to Collection/Array
```.toArray()``` --> vb. toArray(Employee[]::new);  
```.collect()``` --> vb. collect(Collectors.toList());  

## Hoofdstuk 8 Strings



## Hoofdstuk 9 Bestandsverwerking
### Tekst wegschrijven:  
![image](https://user-images.githubusercontent.com/68321900/120449654-e682bf00-c38f-11eb-993d-cd903ca50917.png)  

Methodes van de scanner:  
- hasNext(), hasNextInt()
- next(), nextLine()
- nextInt()
- ...

Exceptions bij het lezen:  
- InvalidPathException: het opgegeven pad klopt niet  
- IOException: het opgegeven bestand wordt niet gevonden  
- InputMismatchException: indien de organisatie/type gegevens niet overeenstemmen  
- NoSuchElementException: er ontbreken elemenenten  
- IllegalStateException: in geval van lezen terwijl Scanner reeds gesloten is  

Gebruik steeds **Try with resources** 

Uitlezen tekstbestanden: check tot einde met hasNext  
Uitlezen objecten serialisatie: while(true) en wacht tot EOFileException  

Gebruik **H9 Voorbeelden**  
- tekstbestand.persistentie.AccountRecordMapper  
- serialisatie.persistentie.AccountRecordMapper  

### Objecten wegschrijven: implements Serializable!   
![image](https://user-images.githubusercontent.com/68321900/120449729-fd291600-c38f-11eb-8ae8-ff33cd086e5c.png)  



# OOSD Conversions
## String to Int
```
String s = "1";
int i = Integer.parseInt(s);
```
## Int to String
```
int i = 10;
String s = String.valueOf(i);
```
## String to char
```
String s = "hello";
char c = s.charAt(0); //returns "h"
```
## char to String
```
char c = 's';
String s = String.valueOf(c);
```
## 1-Dim Array to List
```
String[] colors = {"Black","Blue","Red"}; //array
LinkedList<String> lijst = new LinkedList<>(Arrays.asList(colors)); //list
```
## List to 1-Dim Array
Zie hierboven
```
lijst.add("Pink")
colors = lijst.toArray(new String[lijst.size()]);
```
## 2-Dim Array to ArrayList
```
//we voegen niet elk element apart toe maar gewoon de rijen
list  = new ArrayList<>();
for (String[] rij: kist)
  list.addAll(Arrays.asList(rij));
```

# Collections
## Methode voor afdrukken van lijst
```
private void printList(Collection<String> collection) {
  //1. met enhanced for
  for (String color : collection)
    System.out.printf("%s ", color);
      
  // 2. met iterator
  Iterator<String> iterator = collection.iterator();
  while (iterator.hasNext())
    System.out.printf("%s ", iterator.next());
}
```
## Methode voor verwijderen elementen uit Collection
```
private void removeItems(Collection<String> col1, Collection<String> col2) {
  col1.removeAll(col2);
}
```
# List
## Methode voor verwijderen elementen uit List
```
private void removeItems(List<String> list, int start, int eind) {
  list.subList(start, end).clear();
}
```
## Methode om List reversed weer te geven
```
private void printReverse(List<String> list) {
  ListIterator<String> iterator = list.listIterator(list.size()); //we beginnen achteraan
  while(iterator.hasPrevious())
    System.out.printf("%s ", iterator.previous());
}
```
## Methode omzetten naar hoofdletters in List
```
private void convertToUpperCase(List<String> list) {
  ListIterator<String> iterator = list.listIterator(); //vooraan
  while(iterator.hasNext())
    String s = iterator.next(); //get item
    iterator.set(s.toUpperCase());
}
```
