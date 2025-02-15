# Software-Engineering-project
This is the project for the course of Ingegneria del Software (Software Engineering) at Politecnico di Milano.
The scope of the project was to design and implement a board game in Java using the Model View Controller pattern for the structure of the game, and other patterns to create a well designed software that is also easy to maintain and update.
The board game chosen by the professor was "Master of Renaissance".
The project was done in groups of three students.

<h1>ingswAM2021-Bindi-Borsatto-Bronzo</h1>
<h2> Recap </h2>
<p> Questa applicazione è la nostra versione software del gioco da tavolo <i> Maestri del Rinascimento</i>.
  Abbiamo applicato le seguenti funzionalità:
  <ul>
    <li>Regole complete (partita solitario inclusa)</li>
    <li>CLI</li>
    <li>GUI</li>
    <li>Partite multiple</li>
    <li>Resilienza alle disconnessioni</li>
</ul>
</p>
<h2>UML e User Diagram</h2>
<p>Questi sono gli UML del nostro model:</p>
<ul>
	<li><a href="Images/ClassDiagramStarting.png"> UML prima della scrittura del codice </a></li>
	<li><a href="Images/ClassDiagramDef.png"> UML a svillupo completato </a></li>
	<li><a href="Images/Model auto-generato.png"> UML a sviluppo completato generato automaticamente </a></li>
</ul>
<p>Questo è l'UML del Controller:</p>
<ul>
	<li><a href="Images/Server UML def.png">Controller a sviluppo completato </a></li>
</ul>
<p>Questi sono gli User Diagram che abbiamo preparato per la progettazione della comunicazione tra client e server:</p>
<ul>
	<li><a href="Images/Login.png"> Connessione al server e login </a></li>
	<li><a href="Images/Discard LeaderCard And ExtraResource - Beginning.png"> Scartare le Carte Leader e prendere extra risorse all'inizio del gioco </a></li>
	<li><a href="Images/BuyFromMarket.png"> Comprare dal Mercato </a></li>
	<li><a href="Images/BuyDevCard.png"> Comprare una Carta Sviluppo </a></li>
	<li><a href="Images/ActivateProduction.png"> Attivare la produzione</a></li>
	<li><a href="Images/Activate LeaderCard.png"> Attivare una Carta Leader</a></li>
	<li><a href="Images/Discard LeaderCard.png"> Scartare una Carta Leader </a></li>
	<li><a href="Images/Move Resources.png"> Muovere le risorse</a></li>
	<li><a href="Images/Ending Turns.png"> Cambio dei turni </a></li>
	
</ul>
<h2>
  Aspetti salienti
</h2>
<p>
 Vogliamo evidenziare le seguenti parti della nostra realizzazione dell'applicazione:
  <ul>
   	<li>Lettura degli elementi del gioco da file XML:</li>
    <ul>
      <li>Una volta che una partita viene creata, vengono letti da vari file XML come sono composte le Carte Sviluppo, le carte Leader, le Tessere Favore Papale e i Seganlini delle Azioni di Lorenzo, oltre che alla struttura del Tracciato Fede e del Mercato</li>
      <li>Questo ci permette una maggiore estendebilità del codice poiché modifiche dei valori di questi elementi diventano trasparenti al nostro codice
      </li>
    </ul>
     <li>Utilizzo del pattern Observer all'interno del Model:</li>
    <ul>
      <li>Abbiamo utilizzato questo pattern per attivare i Rapporti in Vaticano (le Celle Papali una volta attraversate notificano gli Observer) e per reagire ai Segnalini di Lorenzo (una volta estratti questi chiamano gli Observer)</li>
      <li>
      Questo ci ha permesso di separare la logica della struttura degli elementi del gioco da cosa succede quando questi vengono attivati</li>
    </ul>
    <li>Invio di JSON tra client e server:</li>
    <ul>
      <li>Abbiamo incapsulato dentro degli oggetti standard (i Command per la comunicazione Client - Server e i ResponseMessage per il contrario) i nostri messaggi particolari: entrambi contengono due campi che si riferiscono al tipo di messaggio incapsulato e all'effettivo messaggio salvato anch'esso come JSON</li>
      <li>Questa particolare struttura ci ha permesso una standardizzazione della ricezione e invio di messaggi che ha facilitato la scrittura del codice</li>
    </ul>
    <li>Effetti delle leadercard</li>
    <ul>
      <li>Nella nostra implementazione, gli effetti delle Carte Leader sono degli oggetti che quest'ultime posseggono come attributo per poter utilizzare un approccio più funzionale: ai metodi che implementano delle azioni in cui si possono utilizzare gli effetti di queste carte viene passato un oggetto Effect che si occupa di applicare il suo potere in modo trasparente al metodo stesso (questo non è vero per gli effetti chiamati ExtraSlot perché il loro effetto è in sostanza attivabile una sola volta)</li>
      <li>Inoltre, tutti gli effetti delle carte estendono una classe generica Effect che contiene una implementazione "dumb" di tutti i metodi con cui gli Effetti svolgono il proprio compito: sarà poi compito di ciascuna classe effetto particolare reimplementare i metodi di loro pertinenza. Questo può risultare in un vantaggio qualora a un metodo venga passato un Effetto di tipo sbagliato perché questo metodo non lancerebbe eccezioni (semplicemente non si guadagna nulla da questo utilizzo)</li>
    </ul>
   	<li>Comunicazione essenziale</li>
    <ul>
      <li>Abbiamo deciso di avere una comunicazione molto limitata nel numero di messaggi scambiati per svolgere una determinata azione: la memoria dell'interazione viene salvata lato Client, semplificando così la scrittura del Server</li>
      <li>Le comunicazioni per svolgere una azione (per es., comprare dal mercato) sono così strutturate: prima il client chiede quale possibile outcome avrebbe da questa sua azione (quante e quali risorse otterebbe), poi successivamente avendo questa informazione risponde al server specifacando tutti i dettagli per svolgere questa azione (su quale scaffale depositare quale risorsa e quale scartare)</li>
    </ul>
    <li>Struttura intelligente GUI (panel manager e riutilizzo componenti visivi) </li>
	<ul>
      <li>La nostra GUI è stata progettata cercando di avere una alta separazione dei compiti: abbiamo una classe, GuiClient, che si occupa solo dell'interazione con il Server (invio e ricezione dei messaggi) e una classe PanelManager che si occupa solo di cambiare le viste per l'utente. Questo ci ha facilitato nella fase di scrittura e di debugging</li>
      <li>Inoltre, dopo aver pianificato cosa volessimo che l'utente vedesse e potesse fare (tramite <a href="Images/Schema per la GUI.png">questo disegno</a>), abbiamo creato prima gli oggetti comuni (per es., i Depositi su cui si potessero fare dei drop di risorse, i Depositi da cui fare drag di risorse, etc.) per poter poi ricreare le nostre viste  semplicemente mettendo insieme i vari pezzi che ci servivano </li>
    </ul>
</ul>
</p>

<h2>Cheats</h2>
Abbiamo implementato un comando di cheat (da CLI scrivere "CHEAT", da GUI cliccare sul bottone "Cheat") che permette di guadagnare risorse e depositarle nel Forziere senza prenderle dal Mercato o attivando la produzione. Le risorse vengono distribuite a tutti i giocatori.

<h2>
  Comandi da utilizzare
</h2>
<p>Per runnare la nostra applicazione, prima bisogna far partire il server tramite </p>
<p> java -jar AM52-1.0-SNAPSHOT-jar-with-dependencies.jar --server 9047 </p>
<p>Purtroppo abbiamo avuto problemi con la creazione del JAR del server perché questo non riesce a trovare dinamicamente i file XML di configurazione descritti sopra. Se il server viene fatto partire da Intellij e si fa partire il client pure da JAR, si può usare traquillamente l'applicativo.</br></p>
<p>Successivamente si può utilizzare la CLI:</p>
<span> java -jar AM52-1.0-SNAPSHOT-jar-with-dependencies.jar --cli --IP 127.0.0.1 9047</br>java -jar AM52-1.0-SNAPSHOT-jar-with-dependencies.jar -c --IP 127.0.0.1 9047</span>

<p></br>O la GUI:</p>
<p> java -jar AM52-1.0-SNAPSHOT-jar-with-dependencies.jar --IP 127.0.0.1 9047</p>

<h2>Creatori</h2>
Made with love by (in ordine alfabetico)</br>
Ludovica Bindi (10619971)</br>
Andrea Borsatto (10628989)</br>
Martin Bronzo (10619358)</br>
