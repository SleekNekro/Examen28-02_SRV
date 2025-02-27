# Guía Paso a Paso: ServerSocket en Java

## 1. ¿Qué es un ServerSocket?

Un `ServerSocket` en Java permite que un programa actúe como servidor y escuche conexiones entrantes de clientes. Su funcionamiento se basa en:

- Crear un `ServerSocket` en un puerto específico.
- Esperar conexiones de clientes.
- Aceptar una conexión y manejar la comunicación.

---

## 2. Código del Servidor Explicado

```java
import java.io.*;
import java.net.*;

public class Servidor {
    public static void main(String[] args) {
        int puerto = 5000; // Puerto donde escuchará el servidor

        try (ServerSocket serverSocket = new ServerSocket(puerto)) {
            System.out.println("Servidor escuchando en el puerto " + puerto);

            while (true) {
                // Esperar conexión de cliente
                Socket socket = serverSocket.accept();
                System.out.println("Cliente conectado: " + socket.getInetAddress());

                // Crear flujo de entrada y salida
                BufferedReader entrada = new BufferedReader(new InputStreamReader(socket.getInputStream()));
                PrintWriter salida = new PrintWriter(socket.getOutputStream(), true);

                // Leer mensaje del cliente
                String mensaje = entrada.readLine();
                System.out.println("Cliente dice: " + mensaje);

                // Responder al cliente
                salida.println("Mensaje recibido: " + mensaje);

                // Cerrar conexión
                socket.close();
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

---

## 3. Explicación Paso a Paso

### Crear un ServerSocket

```java
ServerSocket serverSocket = new ServerSocket(5000);
```

Se define el puerto en el que el servidor escuchará conexiones.

### Esperar una conexión

```java
Socket socket = serverSocket.accept();
```

El servidor se bloquea hasta que un cliente se conecte.

### Abrir canales de comunicación

```java
BufferedReader entrada = new BufferedReader(new InputStreamReader(socket.getInputStream()));
PrintWriter salida = new PrintWriter(socket.getOutputStream(), true);
```

Se crean flujos para leer y escribir datos entre cliente y servidor.

### Leer mensaje del cliente

```java
String mensaje = entrada.readLine();
```

Se recibe un mensaje enviado por el cliente.

### Enviar respuesta al cliente

```java
salida.println("Mensaje recibido: " + mensaje);
```

Se envía una confirmación de que el mensaje fue recibido.

### Cerrar la conexión

```java
socket.close();
```

Se finaliza la comunicación con el cliente.

---

## 4. Código del Cliente

```java
import java.io.*;
import java.net.*;

public class Cliente {
    public static void main(String[] args) {
        String servidor = "localhost";
        int puerto = 5000;

        try (Socket socket = new Socket(servidor, puerto);
             PrintWriter salida = new PrintWriter(socket.getOutputStream(), true);
             BufferedReader entrada = new BufferedReader(new InputStreamReader(socket.getInputStream()))) {

            // Enviar mensaje al servidor
            salida.println("Hola, servidor!");

            // Leer respuesta del servidor
            String respuesta = entrada.readLine();
            System.out.println("Servidor dice: " + respuesta);

        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

---

## 5. Instrucciones para Ejecutar

### Compilar los archivos

```sh
javac Servidor.java Cliente.java
```

### Ejecutar el servidor

```sh
java Servidor
```

### Ejecutar el cliente en otra terminal

```sh
java Cliente
```

### Ver la comunicación entre cliente y servidor

El cliente enviará un mensaje y el servidor lo responderá.
