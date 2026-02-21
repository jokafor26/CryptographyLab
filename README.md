# CryptographyLab

Cryptography Lab

In this lab I practice how to use MD5 and Sha1 to generate hash codes of stings or large files and verify whether a download file is valid. Would also practice how to use GPG to encrypt/decrypt files with symmetric algorithms and how to generate public/private key pairs and certificates, distribute it with a public key to friends and let them encrypt a document with the public key and let the key owner decrypt the document with private keys.

Hashing Files with MD5 and SHA-1 
First, I would create the file file1.txt and file2.txt with the contents of which file it is. Then make sure the files are in the folder.

 ![Image](https://github.com/user-attachments/assets/7b4d45a0-9fa0-4c6c-a4cc-bcc912da4914)

Then I generated the hash codes (sums) for the two files which is in hexideceimal. 

![Image](https://github.com/user-attachments/assets/fdb8e29b-fd4d-42c6-bfef-c4c527fa5c6c)

Then ran “gedit sha1sum.txt” to create a new text file “sha1sum.txt”, and copy the two output lines of the last step into this file. Save the file. 

 ![Image](https://github.com/user-attachments/assets/9afc34e3-b007-4f5c-bdf3-65eb9d7a701a)

 ![Image](https://github.com/user-attachments/assets/2d0e0c01-21c7-4f23-bfb9-ac8c4ae6dc32)

Then I ran “sha1sum -c sha1sum.txt”. In this case program “sha1sum” will read file “sha1sum.txt”. For each line in this file, it will check whether the SHA1 hash code generated for the contents of the second entry (file) is the same as the first entry (SHA1 hash code calculated beforehand). If they match, the program will print out OK for the file. Then I did the same thing but replaced sha1sum command with md5sum and file name with md5sum.txt. the md5 hash codes are shorter and they serve the same purpose of file contents validation. 

 ![Image](https://github.com/user-attachments/assets/0bde2fd1-9a7d-4251-8d2d-779474da2f9e)

Note: All MD5 hash codes for different files are not the same length. Files always lead to different codes with MD5 & SHA1. SHA1 is longer in length so it would be hard to crack.

4.2 Symmetric Key Encryption/Decryption with GPG
Ran “gpg --symmetric file1.txt” to encrypt file „file1.txt”. You will be prompted to enter the passphrase, it is 123456. The encrypted version is in file “file1.txt.gpg”. Then ran “cat file1.txt.gpg” to review the contents of file “file1.txt.gpg”. After I ran “gpg -d file1.txt.gpg” to decrypt the file. I was prompted to enter the passphrase, it is 123456. 

 ![Image](https://github.com/user-attachments/assets/c8c3f1df-9776-4f9c-b402-c0a92fa2a859)

If I needed to paste the encrypted data in email body instead of using email attachment, then you can run “gpg --symmetric --armor file1.txt” to generate the encrypted data in text form in file “file1.txt.asc”. You will be prompted to enter the passphrase, it is 123456. I ran “cat file1.txt.asc” to review the contents of file “file1.txt.asc”. To decrypt the file “file1.txt.asc”, I ran “gpg --armor -d file1.txt.asc”. I was prompted to enter the passphrase, it is 123456. When trying to do the gpg –sysmmetric –armor file1.txt command I got an error.

 ![Image](https://github.com/user-attachments/assets/fb1a51f0-8067-4360-b92f-9337304df461)

Note: If I receive a secret message in an email body and the message is encrypted by a command like “gpg --symmetric --armor message.txt”, the steps I took to recover the message is by using the gpg –armor -d message.txt.asc command.

4.3 Public/Private Key Creation and Encryption/Decryption
Here im going to practice the PGP (GPG) concepts with in my ubuntu system. I started by creating 2 ussers Alice and Mike with an easy 123456 password. I used visudo to launch file /etc/sudoers.tmp in a atext editor and insterted the following lines at the end of the file alice ALL=(ALL) NOPASSWD: ALL
mike ALL=(ALL) NOPASSWD: ALL. This would make them use sudo. Then logged into the users. 
 
 ![Image](https://github.com/user-attachments/assets/d2ad7e95-720c-43cd-9740-dcee09a284f1)
 
![Image](https://github.com/user-attachments/assets/80695cb0-bf9b-4408-9e50-c2ee31c7c088)

I then generated  public and private keys for alice and mike by running “gpg --gen-key”. Entered “DSA and Elgamal” for the key kind, 2048 for the key size, “key does not expire” for key expiration date, “Alice” for real name, alice@pace.edu for email address, “Alice‟s keys” as comment, and “Alice‟s passphrase” for passphrase. I may have needed to type over 284 random keys to generate enough entropy so the keys could be created. I did the same process for mike. 

![Image](https://github.com/user-attachments/assets/a7370557-0749-4c6e-8cb5-e7778dd18cf3)

 ![Image](https://github.com/user-attachments/assets/3f3b4e02-bb76-41ad-b849-213c999791f3)

Then I would export Mike’s public key to Alice by opening mikes terminal then In Mike‟s terminal window, run “gpg --armor --output alice-pk --export Mike@pace.edu” to dump Mike ‟s public key in file “Mike -pk”. I can run “more Mike -pk” to review the public key. Run “sudo cp Mike-pk /home/mike” to copy Mike‟s public key file “Mike-pk” to Alice‟s home folder. In Alice‟s terminal window, verify the existence of file “/home/alice/mike-pk” by running “ls” in alice‟s home folder ~ (/home/alice). In the same Alice‟s terminal window, ran “gpg --import mike-pk” to import mike‟s public key into alice‟s key store. In the same alice‟s terminal window, run “gpg --edit-key mike@pace.edu” to enter the editing session for mike‟s public key. Type sub-command “fpr” to review the fingerprint of mike‟s public key. Type sub-command “sign” to sign this key with alice‟s key. I was asked to enter alice‟s passphrase, which is “alice‟s passphrase”. Type sub-command “check” to review who is on the signature list of mike‟s public key, and we will see mike (self-signature) and alice on the list to confirm the validity of the key. 
 
![Image](https://github.com/user-attachments/assets/d2006f7a-718d-4a81-b95c-07ead42d079f)

 ![Image](https://github.com/user-attachments/assets/1b8b3097-2b0b-40ad-a271-3e4cbb1d5304)

In alice‟s terminal window, run “cat > msg-to-mike” followed by the ENTER key, type “mike‟s secret message”, and then type key combination Ctrl+D to close the file. I just created a new text file “msg-to-mike” with contents “mike‟s secret message”. In Alice‟s terminal window, run “gpg --recipient mike@pace.edu --output secret-to-mike --encrypt msg-to-mike” to generate a new file “secret-to-mike” containing the encrypted version file “msg-to-mike”. In Alice‟s terminal window, run “more secret-to-mike” to review the encrypted version of the message. In Alice‟s terminal window, run “sudo cp secret-to-mike /home/mike” to copy file “secret-to-mike” to Mike‟s home folder “/home/mike”. In mike‟s terminal window, run “ls” in mike‟s home folder ~ (/home/mike) to verify the existence of file “secret-to-mike”. 
 
![Image](https://github.com/user-attachments/assets/5090d3c9-16ba-4e36-bccc-e4e57753dd31)

 ![Image](https://github.com/user-attachments/assets/0925afdb-e6df-4992-869b-5b874c48c887)

Then to decrypt the message in mike‟s terminal window, run command “gpg --output msg-from-alice --decrypt secret-to-mike” to decrypt the contents of file “secret-to-mike” and save the result in a new file “msg-from-alice”. In Mike‟s terminal window, ran “more msg-from-alice” to review the decrypted message from alice. 

 ![Image](https://github.com/user-attachments/assets/1fd6493e-a4ac-4379-a118-b39af36a8be5)

Note: Encrypting the keys then sending it as a private message to another user in order to secure ways for distributing symmetric or public keys. If the keys are encrypted an email is a secure way for distributing symmetric or public keys. I could create a message the encrypt the message and send it but in order to see the message it needs to be decrypted with a passphrase. If a Word file is digitally signed, is the file also normally encrypted so it cannot be eavesdropped because the file is encrypted. I can totally trust a company if that company has a digital certificate signed by VeriSign, because it means it came from a trusted source. Human training is also needed because technologies alone cannot completely solve the network or web security problems?
