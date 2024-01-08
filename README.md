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

Un progetto particolarmente complesso può implementare piu MVC.

## Illustrare l’utilizzo duale di puntatori ed array in C (3 punti)
```c
int main() {
  //Dichiaro un numero intero
  int num = 10;

  // Dichiaro un puntatore e lo "allaccio" alla variabile num
  int *p_num = &num;

  printf("%d\n", num); // 10

  // Modifico il valore di num attraverso il puntatore ad esso
  *p_num = 12;

  printf("%d\n", num); // 12

}
```

## Illustrare l’utilizzo degli ExecutorService in Java (3 punti)
```Java
import java.util.ArrayList;
import java.util.concurrent.*;

// Search for even numbers from 0 to 1 million
public class Main {
    public static void main(String[] args) {

        int nThread = 5;  // Imposta il numero desiderato di thread
        ExecutorService executor = Executors.newFixedThreadPool(nThread);

        ArrayList<Future<Integer>> results = new ArrayList<>();

        int max = 1_000_000;
        int numberForThread = max / nThread;

        for (int i = 0; i < max; i += numberForThread) {
            final int start = i;
            // Utilizza Callable per poter restituire un risultato
            Callable<Integer> task = () -> {
                int count = 0;
                for (int j = start; j < start + numberForThread; j++)
                    if (j % 2 == 0) count++;
                return count;
            };

            // Usa submit per ottenere una Future<Integer> e aggiungila alla lista di risultati
            results.add(executor.submit(task));
        }

        executor.shutdown();

        int nEven = 0;
        for (Future<Integer> f : results) {
            try {
                // Ottieni il risultato dalla Future e sommalo a nEven
                nEven += f.get();
            } catch (InterruptedException | ExecutionException e) {
                e.printStackTrace();
            }
        }

        System.out.println("Found " + nEven + " even numbers!");
    }
}
```
