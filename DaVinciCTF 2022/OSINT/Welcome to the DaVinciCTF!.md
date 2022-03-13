# Welcome to the DaVinciCTF!
### DaVinciCTF 2022
Very easy challenge when you know where to look but that gave me a headache.

### Stats 
Difficulty   :   **Easy**

Category :   **OSINT**

### Resolution

For this challenge, the only information we have is an image.

![img](../assets/OSINT/Welcome%20to%20the%20DaVinciCTF!/img.jpg)

What jumps out at us are the post it notes which look very interesting.

![binance](../assets/OSINT/Welcome%20to%20the%20DaVinciCTF!/binance.png)
![ctfd](../assets/OSINT/Welcome%20to%20the%20DaVinciCTF!/ctfd.png)
![recover](../assets/OSINT/Welcome%20to%20the%20DaVinciCTF!/recover.png)
![todo](../assets/OSINT/Welcome%20to%20the%20DaVinciCTF!/todo.png)

We try to connect to the different pages of the photo with the identifiers and by mixing them but we don't get any results. Also, it's not common in a CTF to let anyone log into an account because people might change the passwords. We then analyze every detail of the image and there we see something strange. 

![weird](../assets/OSINT/Welcome%20to%20the%20DaVinciCTF!/weird.png)

The url of the home page is weird: `https://ctfd.davincicode.fr/_`. With an underscore at the end. We go to the said page which looks like the normal home page of the ctf but looking at the content we notice the flag written white on white.

![flag](../assets/OSINT/Welcome%20to%20the%20DaVinciCTF!/flag.png)

#### `Answer : dvCTF{8a878c2bd9c1844aac17cd051585e2f0}`

Write-up made wit :heart: by @LaGelee