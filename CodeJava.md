```import java.io.*;
import java.net.Socket;

public class Client {
    public static void main(String[] args) {
        String serveurAdresse = "localhost";
        int port = 4000;

        try (Socket socket = new Socket(serveurAdresse, port)) {
            System.out.println("Connecté au serveur sur " + serveurAdresse + ":" + port);

            BufferedReader in = new BufferedReader(new InputStreamReader(socket.getInputStream()));
            PrintWriter out = new PrintWriter(socket.getOutputStream(), false);

            
            String messageServeur;
            String reponseClient;
            String messageNettoye = "";
            while ((messageServeur = in.readLine()) != null) {
                System.out.println("Serveur : " + messageServeur);
                reponseClient = "";
                if (messageServeur.startsWith(">>> ")){
                    messageNettoye = messageServeur.substring(4).trim();
                    for (int i = messageNettoye.length()-1 ; i >= 0 ; i--) {
                        reponseClient += (char) messageNettoye.charAt(i);
                    }
                    System.out.println("Client : " + reponseClient);
                    out.print(reponseClient + "\n");
                    out.flush();
                }
            }

            socket.close();
            System.out.println("Déconnecté du serveur.");
        } catch (IOException e) {
            System.err.println("Erreur - " + e.getMessage());
        }
    }
}
```
