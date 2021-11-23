# Vignere-Solver

Introduction
The Vigenère cipher, invented by the Italian cryptographer Giovan Battista Bellaso in 
1553, but wrongly attributed to the French cryptographer Blaise de Vigenère (who did 
invent a similar cipher), is one of the earliest serious attempts to create a cipher that is 
more secure than an easily breakable substitution cipher. And secure it was: it went 
unbroken for over 3 centuries until the German cryptographer Friedrich Kasiski found a 
way to crack it in 1863. 
We call the Vigenère cipher a "hand cipher" because encryption and decryption can 
easily be performed using pencil and paper. Nowadays, almost all hand ciphers can be 
cracked using a computer, and this is exactly what you will do with the Vigenère cipher. 


Description of the cipher
The Vigenère cipher uses a key , which can be any string of lowercase letters, for 
instance "ape" . Let us consider the following plaintext message we want to encrypt: 
"Bokito and Einstein have the same birthday!" . Now write down the plaintext 
message, and below it repeatedly write down the key: 
Bokito and Einstein have the same birthday! (plaintext) 
apeape ape apeapeap eape ape apea peapeape (key) 
The key letters are then used to rotationally shift the letters of the plaintext above 
them, using the scheme a=0, b=1, c=2, ..., z=25. So for our key we use a=0, p=15, 
e=4, resulting in the ciphertext "Bdoiis ach Exrsiiic laki twi spqe qmrildpc!" : 
Bokito and Einstein have the same birthday! (plaintext) 
apeape ape apeapeap eape ape apea peapeape (key) 
Bdoiis ach Exrsiiic laki twi spqe qmrildpc! (ciphertext) 
Let's also give an example of a decryption using the Vigenère cipher. Suppose we 
receive the ciphertext message "M abxock hnsp ge oycs rts akqc at n zpyzh." and 
we know it was encrypted using the Vigenère cipher with "monkey" as key. We write 
down the ciphertext and the key repeatedly below it. And we decrypt it by shifting 
letters backwards through the alphabet using m=12, o=14, n=13, k=10, e=4, y=24: 
M abxock hnsp ge oycs rts akqc at n zpyzh. (ciphertext) 
m onkeym onke ym onke ymo nkey mo n keymo (key) 
A monkey tail is also the name of a plant. (plaintext) 
In this summative assignment you will implement the encryption and decryption of the 
Vigenère cipher with a given key. You will also implement a probabilistic optimization 
algorithm that finds the original plaintext given a Vigenère-encrypted ciphertext where 
the key is unknown. This optimization will make use of quadgram statistics , which you 
learned about and implemented in the formative assignment on the Caesar cipher

A probabilistic search algorithm
Suppose we are given an encrypted message and we want to decrypt it, for instance 
the message " V id wueirl lk tb ml vvxk vn pweorndvkkdoaaeg wgirs." which 
somehow we know to be a Vigenère-encrypted message with an key of length 7, but 
we don't know which particular length-7 key was used. 
We could try all possible letter strings of length 7 to decrypt the message, but then 
we'd have to try out 26 7
 = 8031810176 possible keys, which is quite a large number. 
For larger key lengths it quickly becomes unfeasible to perform such an exhaustive 
search. But there are many ways around this, and a randomized search is what we 
shall focus on in this assignment. 
Start with a random key , for example "xlgyntq" , leading to the decryption "Y xx 
yhlsua fm gi wo kpzx cx slyqeunyzefbhkhv qivyc." , which has a quadgram fitness 
of 755.8838865. 
The next step is to mutate the key : randomly select one letter of the key and change it 
randomly into another letter. This way we may get the key "xlgynt n " , giving the 
decryption "Y xx yhlvua fm gi zo kpzx ca slyqeuqyzefbhnhv qivyf." with a 
quadgram fitness of 774.9566723. We want to minimize the quadgram fitness, and this 
new key is worse than the starting key, so we reject it. 
We now continue trying out random mutations of the starting key until we find one that 
gives a lower quadgram fitness. We then replace the starting key with the new key, 
keep trying out random mutations until we get a better key, which is the one we will 
continue with, etc, etc.

