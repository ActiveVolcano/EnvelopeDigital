# Envelope Digital

Authors: *Ryan F. de Sousa* e *Pedro F. F. de Abreu*.

[Here](https://youtu.be/dH7ij_95JqE) can be found a video showing the compilation and execution of the program.

A digital envelope is an encryption method that allows a message to be sent securely from a sender to a recipient without the message being intercepted by a third party. The digital envelope consists of two steps: the first step consists of encrypting the message with a symmetrical key, and the second step consists of encrypting the symmetrical key with a public key. The symmetric key is randomly generated and is used only to encrypt the message. The symmetrical key is encrypted with the recipient's public key and sent along with the encrypted message. The recipient uses its private key to decipher the symmetric key, and then uses the symmetric key to decipher the message.

## How it works

The program supports different encryption algorithms such as AES, DES and RC4, and uses asymmetric RSA keys to encrypt and decrypt a random symmetric key that is used to encrypt file data.

The project was developed in Java and is divided into several classes.

- There is the Main class, responsible for the execution of the program.

- The Utils class that serves to convert a String object into a byte array and a byte array into a String object, in order to facilitate encryption and decryption operations.
- The FileUtils class serves for reading and writing files.
- There is a SymmetricCypher interface that serves to specify the methods that should be implemented by classes that implement symmetric cipher (AES/DES/RC4).
- The AESCypher, DESCypher, RC4Cypher classes implement the SymmetricCypher interface and are responsible for encrypting and decrypting text using the AES, DES and RC4 algorithms, respectively.
- The RSACypher class allows you to enter with a key pair in OpenSSL format. The public key will be used for encryption and the private key for decryption. The key is read from files containing the keys.
- The DigitalEnvelope class defines the digital envelope, having methods for encrypting and decrypting text. This class takes as parameter an object of type SymmetricCypher, to choose which symmetric key algorithm should be used. It instantiates an object of the RSACypher class.

## How to Run

### Construction

To build and run, it is recommended to use the Java distribution "Eclipse Temurin", in its version 17.0.7 that can be found [here](https://adoptium.net/temurin/releases/).

It is not necessary to install the distribution as a package, you can just unpack the file in a directory of your choice and remember the location of the binary files of the Java distribution to call them in the terminal.

Clone the repository or download the zip file and unzip it.

```git clone https://github.com/rfsousa/EnvelopeDigital.git```

In the project folder run the following commands:

The following command compiles the Java files and puts the result in the bin folder:

`javac -d bin src/br/ufpi/seguranca/*.java`

The following command generates the Seguranca.jar executable file:

`jar cfm Seguranca.jar manifest.txt -C bin .`

**WARNING**: The jar and javac programs should be those of the Java distribution mentioned in this document. You can replace these calls with the full path of the executable, for example `C:\Users\ryan\.jdks\jdk17\bin\javac -d bin src/br/ufpi/seguranca/*.java`.

### Implementation

- The displayHelp() method displays a help message with the arguments available to run the program. The help message includes the following argument options:
```
-h: Display the help message.
-encrypt <path>: Specifies the path to the file containing the public key.
-decrypt <path>: Specifies the path to the file containing the private key.
-algorithm <algorithm>: Specifies the algorithm used to encrypt and decrypt.
-output <path>: Specifies the path to the output file.
-input <path>: Specifies the path to the file to be encrypted.
-symmetricKey <path>: Specifies the file that contains the symmetric key encrypted with RSA.
```

- The algorithms available to be passed as parameter in -algorithm are: aes128, aes192, aes256, des, rc4.

To encrypt the message you must use the following arguments:
- encrypt : Specifies the path to the file containing the public key.
- algorithm : Specifies the algorithm used to encrypt and decrypt.
- input : Specifies the path to the file to be encrypted.
- output : Specifies the path to the output file.

```
-encrypt=PATH/Public_Key.pem
-algorithm=rc4
-input=PATH/Input_File_Name.txt
-output=PATH/Output_File_Name.txt
```

The following arguments are used to decrypt the message:

- -decrypt= Specifies the path to the file containing the private key.
- algorithm= Specifies the algorithm used to encrypt and decrypt.
- input : Specifies the path to the file to be encrypted.
- symmetricKey= Specifies the file that contains the symmetric key encrypted with RSA.
- output= Specifies the path to the output file.

```
-decrypt=PATH/private_key_rsa_4096_pkcs8-generated.pem
-algorithm=rc4
-input=PATH/Input_File_Name.txt
-symmetricKey=PATH/Input_File_Name.txt-key
-output=PATH/Output_File_Name.txt
```

**Observation**: The path must be COMPLETE and not relative.

#### Examples

##### Encryption

###### Windows
```
java -jar Seguranca.jar -encrypt=C:\Users\rfsousa\Documents\chavepublica.pem -algorithm=aes192 -input=C:\Users\rfsousa\Documents\arquivo.txt -output=C:\Users\rfsousa\Documents\criptografado.txt
```

###### Linux

```
java -jar Seguranca.jar -encrypt=/home/pedro/Documentos/EnvelopeDigital/resources/public_key_rsa_4096_pkcs8-exported.pem -algorithm=aes192 -input=/home/pedro/Documentos/EnvelopeDigital/resources/lorem.txt -output=/home/pedro/Documentos/EnvelopeDigital/criptografado.txt
```

##### Decryption

###### Windows
```
java -jar Seguranca.jar -decrypt=C:\Users\rfsousa\Documents\chaveprivada.pem -algorithm=aes192 -input=C:\Users\rfsousa\Documents\criptografado.txt -symmetricKey=C:\Users\rfsousa\Documents\criptografado.txt-key -output=C:\Users\rfsousa\Documents\decifrado.txt
```

###### Linux

```
java -jar Seguranca.jar -decrypt=/home/pedro/Documentos/EnvelopeDigital/resources/private_key_rsa_4096_pkcs8-generated.pem -algorithm=aes192 -input=/home/pedro/Documentos/EnvelopeDigital/criptografado.txt -symmetricKey=/home/pedro/Documentos/EnvelopeDigital/criptografado.txt-key -output=/home/pedro/Documentos/EnvelopeDigital/decifrado.txt
```
