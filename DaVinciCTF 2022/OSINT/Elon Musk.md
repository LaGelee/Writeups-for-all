# Elon Musk
### DaVinciCTF 2022
Interesting challenge that makes us use the blockchain. 

### Stats 
Difficulty   :   **Easy**

Category :   **OSINT**

### Description
>Hi,
>
>I'm a huge fan of Elon Musk so I invested all my money in cryptocurrencies. However, I I got lost in the cryptoworld and I lost something, can you help me find it?
>
>Sincerely,
>
>@IL0veElon

### Resolution

The first thing that comes to mind is to google the nickname `@IL0veElon`. We then find a twitter account. 

![twitter](../assets/OSINT/Elon%20Musk/twitter.png)

It is always interesting to look at the content of an account and the tweets. We come across a very interesting tweet that contains a hash. The hash reminds me of something related to the blockchain because the account is a fan of it. 

![tweet](../assets/OSINT/Elon%20Musk/tweet1.png)

After several searches on [Blockchain Explorer](https://www.blockchain.com/explorer) I found nothing. So I went back to the account and there! BINGO! Something very interesting in the account follows. 

![elrond](../assets/OSINT/Elon%20Musk/elrond.png)

I do research and I find on the internet [Elrond Explorer](https://explorer.elrond.com/). And again BINGO! I enter the hash `099627400a565a0cc64c3a61ee0ce785d80dfbd30e1b1ea8bcb9fdd9952b9b8a` and magic! 

![transac](../assets/OSINT/Elon%20Musk/transac.png)

#### `Answer : dvCTF{Bl0cKcH4In_Rul3S}`

Write-up made wit :heart: by @LaGelee