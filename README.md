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

## Si sviluppi un programma in linguaggio Java che implementa un server che permette a più client (telnet) di connettersi, e mandare messaggi al server.
Le funzionalità da implementare sono:
1. creazione del un server che accetta connessioni da più client sulla porta 1234, ad
ogni client viene dato un identificativo univoco numerico (2 punti)
2. inviando tramite telnet al server un testo preceduto dal simbolo @ il server rimanda
al client lo stesso testo in lettere maiuscole (2 punti)
3. Inviando tramite telnet al server il comando “!LIST” il server restituisce al client
l’elenco di tutti i client collegati (elenco degli identificativi) (3 punti)
4. Inviando tramite telnet al server un testo preceduto dal simbolo # il server rimanda
lo stesso testo a tutti i client collegati (3 punti)


```Java
import java.net.*;

public class Main
{

  private static final int port = 1234;
  private static Map < Integer, PrintWriter > clients = new HashMap <> ();
  private static int globalClientId;

  public static void main ()
  {
    System.out.println ("Avvio del server...");

    try (ServerSocket server = new ServerSocket (port);)
    {
      System.out.println ("Server avviato...");
      while (true)
	{
	  System.out.println ("In attesa di connessioni...");

	  Socket client = server.accept ();
	  int clientId = globalClientId++;
	    System.out.println ("Client " + clientId + " connesso");

	  PrintWriter writer =
	    new PrintWriter (client.getOutputStream (), true);
	    clients.put (clientId, writer);
	  Thread clientThread =
	    new Thread (()->handleClient (clientId, clientSocket));
	    clientThread.start ();
	}
    } catch (Exception e)
    {
      e.printStackTrace ();
    }

  }

  private static void handleClient (int clientId, Socket clientSocket)
  {
    try (BufferedReader reader =
	 new BufferedReader (new
			     InputStream (clientSocket.getInputStream ()));)
    {
      String inputLine;
      while (inputLine = reader.readLine () != null)
	{
	  handleInput (clientId, inputLine);
	}
    }
    catch (Exception e)
    {
      e.printStackTrace ();
    }
    finally
    {
      client.remove (clientId);
      System.out.println ("Client " + clientId + " disconnesso.");
    }
  }

  private static void handleInput (int clientId, String inputLine)
  {
    if (inputLine.startsWith ("@"))
      {
	sendToClient (clientId, inputLine.subString (1).toUpperCase ());
      }
    else if (inputLine.equals ("!LIST"))
      {
	sendClientList (clientId);
      }
    else if (inputLine.startsWith ("#"))
      {
	sendToAll (clientId, inputLine.subString (1));
      }
  }

  private static void sendToClient (int clientId, String message)
  {
    PrintWriter writer = clients.get (clientId);
    if (writer != null)
      writer.pritln ("Server: " + message);
  }

  private static void sendClientList (int clientId)
  {
    PrintWriter writer = clients.get (clientId);
    if (writer != null)
      writer.println ("Server: Ecco la lista dei client connessi " +
		      clients.keySet ());
  }

  private static void sendToAll (int sender, String message)
  {
  for (Map.Entry < Integer, PrintWriter > entry:clients.entrySet ())
      {
	int clientId = entry.getKey ();
	PrintWriter writer = entry.getValue ();

	if (clientId != sender)
	  writer.println ("Client " + sender + ": " + message);
      }
  }
}
```





