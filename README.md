# Resumen Resumizado
Aqui voy a poner los metodos y pasos para que sea un resumen mas del estilo de repasar y no de estudiar
<br />

## Overview
ServerSocket y Criptografia 28/02


## ServerSocket
### Inicializar ServerSocket
```java
ServerSocket serverSocket = new ServerSocket(8080); 
```
Se inicializa el socket y le indicamos puerto (superior a 1023). Luego hay q cerrar conexion

<br />

### Enviar datos

```java
PrintWriter socketWriter = new PrintWriter(socket.getOutputStream(), true);
socketWriter.println("This is the data");
```

Enviamos datos con un PrintWritter (es caso de que queramos enviar un String) <br />
Tambien tenemos el OutputStream (imagenes/archivos) y el DataOutputStream (numeros/booleans)

### Recibir Datos
```java
var socketReader = new BufferedReader(new InputStreamReader(socket.getInputStream()));

socketReader.lines();     // Stream<String>
socketReader.readLine();  // String
```
Para el resto de tipos de datos DataInputStream

###  Conectar a un servidor

```java
Socket socket = new Socket("15.6.17.18", 7000);
```
La clase Socket para conectar usando "String" para la ip, y un Integer para el puerto que habiamos puesto en el server <br />
Una vez establecida la conexión se pueden usar `PrintWriter` o un `BufferedReader` para comunicarse con el servidor.

  
<br />

## Criptografia

### 🔐 String ↔ byte[] ↔ Base64

Conversiones:

```java
// String to byte[]
byte[] bytes = "un texto".getBytes();

// byte[] to String
String texto = new String(bytes);

// byte[] --> Base64
String enBase64 = Base64.getEncoder().encodeToString(bytes);

// Base64 --> byte[]
byte[] bytes = Base64.getDecoder().decode(enBase64);

```

### 🔐 SecureRandom

Esto apenas lo hemos visto

```java
SecureRandom random = new SecureRandom();
byte[] store = new byte[16];
random.nextBytes(store);
```

### 🔐 KeyGenerator

Generar una sola llave

```java
KeyGenerator keyGenerator = KeyGenerator.getInstance("AES");
keyGenerator.init(256);
SecretKey secretKey =  keyGenerator.generateKey();
```

### 🔐 KeyPairGenerator

Dos llaves una privada y una publica

```java
KeyPairGenerator keyGen = KeyPairGenerator.getInstance("RSA");
keyGen.initialize(2048);
KeyPair keypair = keyGen.generateKeyPair();

PrivateKey privateKey = keypair.getPrivate();
PublicKey publicKey = keypair.getPublic();
```

<br>
<hr>
<br>

### 🔐 MessageDigest

Genera un hash

```java
MessageDigest md = MessageDigest.getInstance("SHA-256");
byte[] hashedBytes = md.digest(bytes);
```

### 🔐 Signature

Firma de datos con la clave privada y autentificacion de la firma con la clave publica

```java
Signature signature = Signature.getInstance("SHA256withRSA");

// firmar
signature.initSign(privateKey);
signature.update(bytes);
byte[] dataSignature = signature.sign();

// validar firma
signature.initVerify(publicKey);
signature.update(bytes);
boolean valid = signature.verify(dataSignature);
```

### 🔐 Cipher

Clase para encriptar y desencriptar usando las llaves que hemos generando con la clase anterior

#### 🥄 Symmetric

```java
Cipher cipher = Cipher.getInstance("AES");

// Crypt
cipher.init(Cipher.ENCRYPT_MODE, secretKey);
byte[] encryptedBytes = cipher.doFinal(bytes);

// Decypt
cipher.init(Cipher.DECRYPT_MODE, secretKey);
byte[] decryptedBytes = cipher.doFinal(encryptedBytes);
```

#### 🍴 Asymmetric

```java
Cipher cipher = Cipher.getInstance("RSA");

// Crypt
cipher.init(Cipher.ENCRYPT_MODE, publicKey);
byte[] encryptedBytes = cipher.doFinal(bytes);

// Decrypt
cipher.init(Cipher.DECRYPT_MODE, privateKey);
byte[] decryptedBytes = cipher.doFinal(encryptedBytes);

```


<br />
