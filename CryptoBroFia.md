# Ejemplos de Hashing, Firmado y Cifrado en Java

Este documento presenta ejemplos de:

- **Hash de un string y conversión a Base64**
- **Firmado y verificación de un mensaje**
- **Cifrado y descifrado (simétrico con AES)**

---

## 1. Hash y conversión a Base64

```java
import java.nio.charset.StandardCharsets;
import java.security.MessageDigest;
import java.util.Base64;

public class HashBase64Example {
    public static void main(String[] args) throws Exception {
        String input = "HolaMundo";

        // Crear hash SHA-256
        MessageDigest digest = MessageDigest.getInstance("SHA-256");
        byte[] hashBytes = digest.digest(input.getBytes(StandardCharsets.UTF_8));

        // Convertir a Base64
        String hashBase64 = Base64.getEncoder().encodeToString(hashBytes);

        // Imprimir resultado
        System.out.println("Hash en Base64: " + hashBase64);
    }
}
```

---

## 2. Firmar y verificar un mensaje

```java
import java.nio.charset.StandardCharsets;
import java.security.*;

public class SignVerifyExample {
    public static void main(String[] args) throws Exception {
        String mensaje = "MensajeSeguro";

        // Generar par de claves
        KeyPairGenerator keyGen = KeyPairGenerator.getInstance("RSA");
        keyGen.initialize(2048);
        KeyPair keyPair = keyGen.generateKeyPair();

        // Firmar el mensaje
        Signature signature = Signature.getInstance("SHA256withRSA");
        signature.initSign(keyPair.getPrivate());
        signature.update(mensaje.getBytes(StandardCharsets.UTF_8));
        byte[] firma = signature.sign();

        // Verificar la firma
        signature.initVerify(keyPair.getPublic());
        signature.update(mensaje.getBytes(StandardCharsets.UTF_8));
        boolean esValido = signature.verify(firma);

        System.out.println("¿Firma válida? " + esValido);
    }
}
```

---

## 3. Cifrado y descifrado (AES - Simétrico)

```java
import javax.crypto.Cipher;
import javax.crypto.KeyGenerator;
import javax.crypto.SecretKey;
import java.util.Base64;

public class EncryptDecryptAES {
    public static void main(String[] args) throws Exception {
        String mensaje = "TextoSecreto";

        // Generar clave AES
        KeyGenerator keyGen = KeyGenerator.getInstance("AES");
        keyGen.init(128);
        SecretKey secretKey = keyGen.generateKey();

        // Cifrar el mensaje
        Cipher cipher = Cipher.getInstance("AES");
        cipher.init(Cipher.ENCRYPT_MODE, secretKey);
        byte[] encryptedBytes = cipher.doFinal(mensaje.getBytes());
        String encryptedBase64 = Base64.getEncoder().encodeToString(encryptedBytes);

        // Descifrar el mensaje
        cipher.init(Cipher.DECRYPT_MODE, secretKey);
        byte[] decryptedBytes = cipher.doFinal(Base64.getDecoder().decode(encryptedBase64));
        String decryptedMessage = new String(decryptedBytes);

        System.out.println("Texto cifrado: " + encryptedBase64);
        System.out.println("Texto descifrado: " + decryptedMessage);
    }
}
```

---

Cada ejemplo es independiente y puede ejecutarse por separado. 🚀
