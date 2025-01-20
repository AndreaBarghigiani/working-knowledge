L’idea di base è quella di creare un tracker per investimenti in crypto che sia in grado di tenere traccia dei nostri prezzi di acquisto per i vari token e/o progetti nei quali siamo entrati.

Faccio subito una distinzione tra token e progetti. I primi sono quelli che compri sperando in un'apprezzamento del prezzo sul quale poi profittare vendendoli in futuro, i secondi invece possono appartenere a diverse categorie e propongono una forma di yield.

Lista non esaustiva di tipologie di progetti ed esempi:
- **DEX**: ne esistono di diverse salse, per questo creerò un nuovo documento dove entro nel dettaglio, ma in questi progetti l'utente può depositare i propri token e aspettarsi una % di interessi maturati;
- **Lend/Borrow**: piattaforme dove l'utente deposita i propri fondi e li mette a disposizione di terzi sotto forma di prestito. Chi deposita si aspetta una % di interessi maturati;
- **ROI Dapp**: l'utente deposita e può prelevare quotidianamente una %
- **Staking**: per tutte le chain PoS l'utente può depositare i propri token nelle pool messe a disposizione dai validatori e si aspetta una % annuale in ritorno (praticamente la stessa % che lo aiuta a combattere l'inflazione del token)
- **Perpetual**: difficili da tracciare in caso il nostro sia un trader, però molti di questi progetti permettono di depositare bluechips e/o stablecoin che vengono usati dalla piattaforma e l'utente può accedere al programma di *revenue share*
- **Mining**: progetti dove l'utente può accedere in forma ti NFT o acquisto di token e partecipa al programma di *revenue share* sui profitti prodotti dai miners 

Sicuramente ci sono, e nasceranno, altri tipi di progetti che permetteranno/prometteranno di fare profitti, quindi il nostro sistema dovrà essere in grado di implementarli nel futuro.

Ma il nostro scopo finale sarà sempre quello di fornire le informazioni che riguardano di quanto l'utente sia in profitto (o perdita) **in base al suo prezzo d'ingresso**.

## Il problema
Al giorno d'oggi esistono moltissimi competitor: DeBank, MyMerlin, Zapper, Nansen... Giusto per citarne alcuni.

Più o meno tutti sono in grado di prendere l'indirizzo di un wallet e di farti sapere quanto sia il valore del tuo portfolio nel momento in cui lo stanno analizzando e i base ai **dati del mercato** (non dell'utente quindi) ti fanno sapere se sei in perdita o meno.

> Di tutti, mi sembra che solo *MyMerlin* stia provando a mostrare la P&L (Profit and Loss) del wallet, ma si perde comunque alcuni token per la strada.

Oltre a questo, la maggior parte di questi strumenti è in grado di tracciare soltanto le chain che sono EVM compatibili. Quindi se l'utente investe all'interno di chain diverse (Solana, Cosmos, Near...) non è in grado di conoscere il valore del proprio patrimonio.

Assumendo che come POC (Proof Of Concept) è inimmaginabile pensare di partire integrando tutte le chain e quelle che credo features per la v1 terranno conto di questo.

Però alla base di tutta la frustrazione che trovo nelle varie piattaforme, e che spero di risolvere con Underdog Tracker, risiede nel fatto che l'utente **è unicamente coinvolto passivamente**. Ovvero, dato che queste piattaforme non hanno un database, tutte le informazioni che l'utente ha a sua disposizione sono *in sola lettura*.

Per esempio, se nelle varie transizioni l'utente ha un token che non è stato valutato bene **non può modificare nulla**. Se il sistema ha fatto un errore di calcolo **son caxxi tuoi** e ti tieni il token così valutato.

Nella mia immaginazione invece l'utente **deve** essere libero di poter modificare qualunque transazione e di conoscerne i dettagli.

## Features
- [x] l'utente deve sapere **quanto** capitale ha investito (in $) e la sua valutazione corrente
- [ ] connettere un wallet al proprio profilo dovrà essere obbligatorio e grazie a questo noi potremmo tenere automaticamente traccia delle varie transazioni 
- [ ] ciascuna transazione potrà essere editabile e dal momento in cui l'utente collega il wallet per la prima volta, le transazioni future dovranno essere **approvate manualmente**. Così gli sarà possibile tenere traccia, e all'occorrenza correggere, tutte le operazioni che svolge.
- [x] per ogni token che holda deve poter conoscere il prezzo di entrata e il potenziale profitto per ogni transazione
- [ ] sarà possibile **personalizzare** le pagine dei token holdati. Magari uno specifico token ha delle fee sul buy/sell o altre cose...
- [ ] l'utente potrà creare degli alert che lo avviseranno in base alla limite che avrà impostato
- [ ] l'utente potrà creare dei tipi di investimento *custom*, ovvero potrà specificare manualmente 

## Future ideas

## Inspiration
Collection of dashboards to take inspiration from.
![[Pasted image 20230506094250.png]]
![[Pasted image 20230503063842.png]]
![[Pasted image 20230503063948.png]]
![[Pasted image 20230503064023.png]]
![[Pasted image 20230503064130.png]]
![[Pasted image 20230503064203.png]]
![[personal-finance-dashboard.mp4]]