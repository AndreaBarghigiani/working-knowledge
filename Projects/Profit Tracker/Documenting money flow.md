
This is how the money flows as 7 Jun 2023.

> We're starting with a fresh account with **$0**

## Make a deposit into a new project
I deposit $100 into a new project that gives me 1% hourly:

| Total | Deposits | Withdraw |
|:-----:|:--------:|:--------:|
| $100  |   $100   |    $0    |

## Withdraw from the project
I withdraw $10 from the new project:

| Total | Deposits | Withdraw |
|:-----:|:--------:|:--------:|
|  $90  |   $100   |   $10    |

## Buy a token
Now I buy ~$100 of Matic:

| Total | Deposits | Withdraw |
|:-----:|:--------:|:--------:|
| $90~  |   $200   |   $10    |

~ strange...

## Sell a token
Now I want to sell ~$50 of Matic

| Total | Deposits | Withdraw |
|:-----:|:--------:|:--------:|
| $40  |   $200~   |   $60    |

~ strange...

# Approaches

## Kiei suggestion:
Il **current evaluation** non dovrebbe tenere conto dei soldi che stanno sul contratto, o meglio, l'investimento iniziale (poi a seconda di app e token ovviamente) può essere di x e durante il periodo abbassarsi o alzarsi di y.

Per questo secondo me il current evaluation dovrebbe essere:

`( interest + token new price) - bought price`

Dove token new price rappresenta il prezzo d'acquisto del token con la variazione del prezzo

Inoltre puoi stabilire un ATH e ATL con qualcosa tipo:

```
if(price > currentPrice) {
	price = ATHPrice
} else {
	price = ATLPrice
}
```

La fase di perdita profitto la gestirei così:

### Caso1: 1 solo acquisto di token.
Ho comprato 100$ di BTC quando era a 25000$.

 Btc sale a 26.000$

Token = 0.004 BTC (quantity/BoughtPrice)
Quantity = $100
BoughtPrice = $25.000
currentPrice= $26.000

`Profit/Loss = ( token * currentPrice) -  quantity`

Oppure: 
`( token * currentPrice) - (token * BoughtPrice)`

### Caso 2: molteplici acquisti

In questo caso ho fatto diverse entry, con diversi prezzi.
Il calcolo è sostanzialmente lo stesso, solo che sostituiremo il BoughtPrice con un avgprice dei prezzi pagati per l'acquisto dei token

## Virus suggestion:
provo a buttare qualche risposta/ideea di quello che io vorrei: 
- la current evaluation ti deve dire il **controvalore in fiat dell'intero wallet** ( quello che stai tracciando) perché io voglio sapere se overall, da quando ho iniziato a mettere soldi in una chain/contratto/etc, se sono in perdita o in profitto ovviamente questo deve essere un dato aggregato e poi da un'altra parte voglio vedere il breakdown per ogni singola voce ( LP, token, etc) 
- per il portfolio status qua potresti pensare a un **checkbox/radio button che permette all'utente di scegliere se mettere in calcolo profitti presi o no** ( così non scapocci tu per capire se è meglio con o senza ) però capisco che è altro effort che devi mettere a terra per realizzarlo 
- per il risk la cosa "figa" che mi potrei immaginare è un algoritmo custom che calcola il rischio in base a X fattori : volatilità negli ultimi X giorni ( 30/60/180/365 ? ) storicità della coin vedere se esiste una specie di trust index per quel token specifico memecoin/degen/etc?

## Mowgli
#### Portfolio risk: 
Secondo me la distinzione principale qui va fatta fra blue chip, stablecoin o native token, e defi in primis. In questo modo già c'è una distinzione lungo termine e tecnicamente safe e ciò che può implodere con facilità (DeFi).

Inoltre nella parte defi la distinzione si può fare o usando un po' un metodo tuo con certi parametri, oppure usando analizzatori di contratti automatici. -de.fi ne ha fatto uno che non è male, ma è lento ad analizzare i contratti.

- una roba simile, ma pronta in secondi è il punteggio che da dexview ai contratti. Io e Pettu lo usiamo per le shitcoin, ma di base i parametri sono buoni e alcuni sono gli stessi che usiamo in IOC quando facciamo ricerca.

Purtroppo non ti so dire se hanno un API per far fare lo stesso ai token/ pair che tu vuoi seguire con un portfolio tracker. Esempio stupido. [https://www.dexview.com/arbitrum/0x8C1cD7ad8848382E2CA72764d7B105ED0d593c96](https://www.dexview.com/arbitrum/0x8C1cD7ad8848382E2CA72764d7B105ED0d593c96 "https://www.dexview.com/arbitrum/0x8C1cD7ad8848382E2CA72764d7B105ED0d593c96")

#### Profitto 
Secondo me qui vai di deposit e token evaluation come base, come hai detto tu inizialmente. Troppo semplice? Si ma a te interessa questo come investitore.

Ciò che tu prendi come interesse può essere considerato profitto puro dentro un tracker. Ma per ogni tipo di transazione (con la possibilità di raggruppare e cercare lo stesso tipo di transazioni), l'utente dovrebbe poter scegliere se questo è un airdrop, rewards del protocollo o se per qualche funzione del contratto ha mintato nuovi token. 

Essendo questo profitto puro, so aggiunge a ciò che il portfolio già ti da come differenza tra investimento e valore attuale. In questo modo tu oltre ad avere un tool che ti segue l'andamento dei tuoi investimenti, potresti poi sviluppare un report che permette di scaricare o calcolare i profitti annuali per la dichiarazione delle tasse.

gli interessi non prelevati secondo me non vanno contatti. Finché non sono nel tuo wallet questi non sono tuoi profitti. Se un progetto collassa quelli rimangono dei numeri sullo schermo