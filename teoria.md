## Definire il concetto di lambda function e illustrarne l’utilizzo tramite un esempio in Java (3 punti)
Una lamba function è una struttura utilizzata per compattare alcuni pezzi di codice in modo da renderlo più semplice da comprendere.
Nello specifico possiamo utilizzare le lamba function quando abbiamo a che fare con classi o interfacce composte da un solo metodo astratto.
Ad esempio
```Java
BinaryOperator somma = new BinaryOperator(){
  public int eval(a, b){
    return a + b;
  }
}
```
possiamo semplificarlo sfruttando la lamba function in
```Java
BinaryOperator somma = (a, b) -> a + b;
```

Oltre ad aumentare la leggibilità del codice un'altro vantaggio che offrono le lamba function è la possibilità di dichiarare un blocco di codice in un punto ed utilizzarlo succesivamente in un altro, anche più volte.

## Descrivere il modello MVC e la sua applicazione in Java/swing (4 punti)
Il modello MVC è un modello di programazione dato dalle seguenti componenti
- Model, possiamo definirlo come il "cervello" del programma, in esso viene gestita tutta la logica del programma, lo stato delle variabili ecc
- Controller, è l'organo che permette la comunicazione tra utente e Model
- View, è l'implementazione della grafica del programma, grazie ad essa l'utente finale può comunicare col controller.

Per una corretta implementazione di questo modello Model e View non devono mai comunicare tra loro ma sempre attraverso il Controller.
Possiamo immaginare la comunicazione tra Model e Controller come se il Model fosse una stazione radio e il controller un ricevitore, ogni qualvolta il Model apporta delle modifiche al programma esso invia un segnale
che deve essere ascoltato dal Controller in modo che poi possa aggiornare la View.
Anche la comunicazione tra Controller e View non avviene direttamente, essa è mediata da messaggi o eventi: il Controller definisce un target su se stesso che potrà essere invocato dalla View al verificarsi di determinati eventi.
