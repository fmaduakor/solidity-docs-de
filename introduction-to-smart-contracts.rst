###############################
Einführung in Smart Contracts
###############################

.. _simple-smart-contract:

*******************************
Ein einfacherer Smart Contract
*******************************

Lass uns mit einem einfachen Beispiel anfangen, bei dem der Wert einer Variable
gesetzt wird und ihn für andere Smart Contracts zugreifbar macht.
Es ist in Ordnung, wenn du jetzt noch nicht alles verstehst. Weitere Details
schauen wir uns später an.

Speicher Beispiel
==================

::

    pragma solidity >=0.4.0 <0.7.0;

    contract SimpleStorage {
        uint storedData;

        function set(uint x) public {
            storedData = x;
        }

        function get() public view returns (uint) {
            return storedData;
        }
    }

Die erste Zeile zeigt dir an, dass der Source Code für die Solidity Version 0.4.0 oder höher, aber nicht
mehr für die Version 0.7.0 oder höher, geschrieben wurde.
Hierdurch wird sichergestellt, dass der Smart Contract nicht mit neueren (breaking) Compiler Versionen kompatibel ist,
bei denen der Smart Contract sich unterschiedlich verhalten könnte.
:ref:`Pragmas<pragma>` sind Anweisungen für Compiler, die angeben wie der Compiler mit dem Source Code umgehen soll
(z.B. `pragma once <https://en.wikipedia.org/wiki/Pragma_once>`_).

Ein Contract ist im Sinne von Solidity eine Ansammlung von Code (seine *Funktionen*)
und Daten (sein *State*) der an einer bestimmten Adresse auf der Ethereum
Blockchain liegt. Die Zeile ``uint storedData;`` deklariert eine State Variable
die ``storedData`` heißt und vom Typ ``uint`` (*u*\nsigned *int*\eger von *256* Bits) ist.
Man kann es sich vorstellen, wie eine einzelner Eintrag in einer Datenbank den man durch 
Funktionen, die diesen Eintrag verwalten, aufrufen oder verändern kann. 
In diesem Beispiel definiert der Contract die Funktionen ``set`` und ``get``, die dafür verwendet werden
können den Wert der Variable zu verändern oder abzurufen.

Anders als in anderen Sprachen üblich, wird bei Solidity der Prefix ``this.`` nicht benötigt um auf state Variablen zuzugreifen.

Dieser Contract mach nicht viel mehr außer (bedingt durch die Infrastruktur von Ethereum) weltweit jedem zu erlauben eine einzelne Zahl zu speichern,
auf die weltweit zugegriffen werden kann, ohne dass es einen (realistischen) Weg gibt jemanden
daran zu hindern diese Zahl zu veröffentlichen. Jeder kann die Funktion ``set`` erneut mit einer
neuen Zahl aufrufen und die bestehende Zahl überschreiben, aber die vorher gespeicherte Zahl 
wird weiterhin in der History der Blockchain gespeichert. Wir werden später sehen, wie wir
Zugriffsbeschränkungen festlegen können, damit nur wir in der Lage sind diese Zahl zu verändern.

.. warning::
    Sei vorsichtig mit Unicode Text, da ähnlich aussehende (oder sogar identische) Zeichen
    unterschiedliche Codepoints besitzen können und somit als unterschiedliched Byte Arrays kodiert werden.


.. note::
    Alle Bezeichner (engl.: Identifier) sind begrenzt auf das 
    ASCII Charakterset. Es ist möglich UTF-8 kodierte Daten in String Variablen zu speichern. Bezeichner sind Contractnamen, Funktionsnamen und Variablennamen.


.. index:: ! subcurrency

Subcurrency Beispiel
====================

Der folgende Contract implementiert die einfachste Form 
einer Kryptowährung. Der Contract erlaubt nur dem Ersteller 
neue Coins zu erstellen (verschiedene Erstellungsreglungen sind möglich).
Jeder kann zu jedem Coins senden ohne sich vorab mit einem Benutzernamen
und Passwort zu registieren. Es wird nur ein Ethereum Schlüsselpaar benötigt. 

::

    pragma solidity >=0.5.0 <0.7.0;

    contract Coin {
    
        // Das Schlüsselwort "public" macht
        // Variablen für andere Contracts zugreifbar
        address public minter;
        mapping (address => uint) public balances;
        // Events erlauben Clients auf
        // spezifische Contract Aenderungen zu reagieren
        event Sent(address from, address to, uint amount);

        // Der Konstruktur Code wird nur bei der
        // Erstellung des Contract ausgefuehrt
        constructor() public {
            minter = msg.sender;
        }
        // Sendet eine Anzahl an neuerstellten Coins an eine Adresse
        // Kann nur von dem Ersteller des Contracts aufgerufen werden
        function mint(address receiver, uint amount) public {
            require(msg.sender == minter);
            require(amount < 1e60);
            balances[receiver] += amount;
        }

        // Sendet eine Anzahl von Coins von einem beliebigen
        // Aufrufer an eine Adresse
        function send(address receiver, uint amount) public {
            require(amount <= balances[msg.sender], "Insufficient balance.");
            balances[msg.sender] -= amount;
            balances[receiver] += amount;
            emit Sent(msg.sender, receiver, amount);
        }
    }

Dieser Contract beinhaltet einige neue Konzepte, die wir nun Schritt für Schritt erklären werden.

Die Zeile ``address public minter;`` deklariert eine State Variable vom Typ :ref:`address<address>`.
Der ``address`` Typ ist ein 160-Bit Wert der keine arithmetischen Operationen erlaubt.
Dieser Typ eignet sich gut um Adressen von Contracts zu speichern, oder den Hash von
der öffentlichen Hälte eines Schlüsselpaares der zu einem :ref:`external accounts<accounts>` gehört.

Durch das Schlüsselwort ``public`` wird automatisch eine Funktion generiert, die es erlaubt, dass 
auf den aktuelle Wert einer State Variable von außerhalb des Contracts zugegriffen werden kann.
Ohne dieses Schlüsselwort, können andere Contracts nicht auf diese Variable zugreifen.
Der Code, der von dem Compiler generierten Funktion ist äquivalent zu dem folgenden:
(die Befehle ``external`` und ``view`` werden wir später erklären)::
    function minter() external view returns (address) { return minter; }

Man könnte eine Funktion wie die oben aufgeführte selbst hinzufügen, aber dann hätte man eine Funktion und
eine State Variable mit dem gleichen namen. Wir brauchen diese Funktion nicht hinzuzufügen, der Compiler
übernimmt das für uns.


.. index:: mapping

Die nächste Zeile ``mapping (address => uint) public balances;`` erstellt auch eine öffentliche
State Variable, aber dieser Datentyp ist etwas komplexer.
Der :ref:`mapping <mapping-types>` Datentyp ordnet Adressen zu :ref:`unsigned integers` 

Mappings können als virtuell instanziierte `Hashtabellen <https://de.wikipedia.org/wiki/Hashtabelle>`_ angesehen werden, bei denen von Beginn
an jeder mögliche Key bereits existiert und zu einem Wert mappt, dessen Byte-Repräsentation vollständig aus Nullen besteht.
Es ist jedoch nicht möglich eine vollständige Liste aller Schlüssel eines Mappings noch alle Werte auszulesen.
Entweder musst du speichern, was du zu einem Mapping hinzugefügt hast oder diese Informationen müssen in dem Kontext nicht wichtig sein.
Or even better, keep a list, or use a more suitable data type.

Die :ref:`Getter Funktion<getter-functions>` welche durch das ``public`` Schlüsselwort erstellt wird ist bei der Verwendung von Mappings komplexer.
Sie sieht wie folgt aus::

    function balances(address _account) external view returns (uint) {
        return balances[_account];
    }

Wie du sehen kannst wird durch die Funktion die Balance eines einzelnen Accounts abgefragt.


.. index:: Event

Die Zeile ``event Sent(address from, address to, uint amount);`` deklariert ein :ref:`"Event" <events>`,
welches durch die letzte Zeile der Funktion ``send`` gesendet wird.
Etherem clients (z.B. Webapplikationen), könnnen als Listener diese Events, die über die Blockchain
gesendet werden, verarbeiten, ohne dass hierfür große Kosten anfallen.
Sobald das Event gesendet wird, erhält der Listener die Argumente ``from``, ``to`` und ``amount``, anhand der die Transaktion
identifiziert werden kann.

Um das Event zu verarbeiten, kann der folgende JavaScript Code verwendet werden. Er verwendet `web3.js <https://github.com/ethereum/web3.js/>`_ verwendet um 
das ``Coin`` Objekt zu erstellen,--

--To listen for this event, you could use the following
JavaScript code, which uses `web3.js <https://github.com/ethereum/web3.js/>`_ to create the ``Coin`` contract object,
and any user interface calls the automatically generated ``balances`` function from above::

    Coin.Sent().watch({}, '', function(error, result) {
        if (!error) {
            console.log("Coin transfer: " + result.args.amount +
                " coins were sent from " + result.args.from +
                " to " + result.args.to + ".");
            console.log("Balances now:\n" +
                "Sender: " + Coin.balances.call(result.args.from) +
                "Receiver: " + Coin.balances.call(result.args.to));
        }
    })

.. index:: Coin

Der :ref:`Konstruktor<constructor>` ist eine Spezialfunktion, die während der Erstellung des Contracts ausgeführt wird
und danach nicht mehr aufgerufen werden kann. In unserem Fall, speichert sie irreversibel die Adresse der Person,
die diesen Contract erstellt hat. Die ``msg`` Variable (zusammen mit ``tx`` und ``block``) ist eine 
:ref:`special global variable <special-variables-functions>` die Eigenschaften enthält die den Zugriff auf die Blockchain erlauben.
``msg.sender`` ist immer die Adresse von der die aktuelle (externe) Funktionsaufruf kommt.

Die Funktionen die die Eigenschaften des Contracts darstellen, die von Benutzern und Contracts aufgerufen werden können sind ``mint`` und ``send``.

Die ``mint`` Funktion sendet einen Betrag an neuerstellen Coins zu einer anderen Adresse.
Der :ref:`require <assert-and-require>` Funktionsaufruf definiert Bedinungen. Wenn diese Bedinungen nicht erfüllt sind, werden alle Änderungen rückgängig gemacht.
In diesem Beispiel stellt ``require(msg.sender == minter);`` sicher, dass nur der Ersteller des Contracts die ``mint`` Funktion aufrufen kann
und ``require(amount < 1e60);`` definiert das Maximum an Tokens. Hierdurch wird sichergestellt, dass keine Overflows in Zukunft möglich sind.

Die ``send`` Funktion kann von jedem (der bereits diese Art an Coins benitzt) benutzt werden um Coins an jeden zu senden.
Wenn der Sender nicht genügend Coins um senden besitzt, schläft der ``require`` Aufruf fehl und gibt dem Sender
eine Fehlermeldung aus.


.. note::

    Wenn dieser Contract verwendet wird um Coins an eine Adresse zusenden,
    kann man über den Blockchain-Explorer diese Coins nicht auf der Adresse sehen,
    da die Konstostände der einzelnen Coins nur in dem Datenspeicher (data storage) dieses spezifischen
    Contracts gespeichert sind. Durch Events kann jedoch ein "Blcokchain-Explorer"
    erstellt werden, der die Transaktionen und Kontostände dieses Coins aufzeichnet.
    Hierbei muss aber die Adresse des Contracts beobachtet werden und nicht die 
    Adressen der Coin Besitzer.
 

.. _blockchain-basics:

*****************
Blockchain Basics
*****************

Das Konzept der Blockchain ist für Programmierer einfach zu verstehen.
Das liegt daran, dass die meisten der schwierigen Konzepte (mining, `hashing <https://en.wikipedia.org/wiki/Cryptographic_hash_function>`_,
`elliptic-curve cryptography <https://en.wikipedia.org/wiki/Elliptic_curve_cryptography>`_,
`peer-to-peer networks <https://en.wikipedia.org/wiki/Peer-to-peer>`_, etc.)
nur vorhanden sind um eine gewisse Eigenschaften der Plattform sicherzustellen. Sobald man als Programmierer
diese Eigenschaften als gegeben annimmt, muss man sich keine Sorgen mehr über die darunterliegenden Technologie machen.
Ähnlich wie man um Amazons AWS zu benutzen, nicht wissen muss wie die internen Prozesse dort aussehen.


.. index:: transaction

Transaktionen
=============

Eine Blockchain ist eine weltweit verteile, transaktionale Datenbank.
Das bedeutet, dass jeder Einträge aus dieser Datenbank auslesen kann, indem er an dem Netzwerk teilnimmt.
Wenn man etwas an dieser Datenbank ändern möchte, muss man eine sogenannte Transkation erstellen, die
von allen anderen akzeptiert werden muss.
Das Wort 'Transaktion' impliziert, dass eine Änderungen die gemacht werden soll
entweder vollständig oder gar nicht ausgeführt wird. 
Furthermore,
while your transaction is being applied to the database, no other transaction can alter it.

Ein Beispiel: Stell dir eine Tabelle vor die alle Kontostände aller Konten in einer
elektronischen Währung auflistet. Wenn eine Überweisung von einem Konto zu einem anderen angefordert wird,
stellt die transaktionale Datenbank sicher, dass wenn der Überweisungsbetrag von einem Konto substrahiert wird,
er immer dem anderen Konto gutgeschrieben wird. Wenn, aus beliebgen Gründen, die Gutschrift auf das Empfängerkonto
nicht möglich ist, wird auch das Senderkonto nicht verändert.

Außerdem ist eine Transaktion immer von dem Sender kryptographisch signiert.
Dies macht es einfach, einzelne Änderungen der Datenbank zu schützen. In unserem Beispiel stellt eine einfache Überprüfung
sicher, dass nur die Person, die den Schlüssel zu dem Konto kennt, auch eine Überweisung von diesem Konto vornehmen kann.

.. index:: ! block

Blocks
======

One major obstacle to overcome is what (in Bitcoin terms) is called a "double-spend attack":
What happens if two transactions exist in the network that both want to empty an account?
Only one of the transactions can be valid, typically the one that is accepted first.
The problem is that "first" is not an objective term in a peer-to-peer network.

The abstract answer to this is that you do not have to care. A globally accepted order of the transactions
will be selected for you, solving the conflict. The transactions will be bundled into what is called a "block"
and then they will be executed and distributed among all participating nodes.
If two transactions contradict each other, the one that ends up being second will
be rejected and not become part of the block.

These blocks form a linear sequence in time and that is where the word "blockchain"
derives from. Blocks are added to the chain in rather regular intervals - for
Ethereum this is roughly every 17 seconds.

As part of the "order selection mechanism" (which is called "mining") it may happen that
blocks are reverted from time to time, but only at the "tip" of the chain. The more
blocks are added on top of a particular block, the less likely this block will be reverted. So it might be that your transactions
are reverted and even removed from the blockchain, but the longer you wait, the less
likely it will be.

.. note::
    Transactions are not guaranteed to be included in the next block or any specific future block,
    since it is not up to the submitter of a transaction, but up to the miners to determine in which block the transaction is included.

    If you want to schedule future calls of your contract, you can use
    the `alarm clock <http://www.ethereum-alarm-clock.com/>`_ or a similar oracle service.

.. _the-ethereum-virtual-machine:

.. index:: !evm, ! ethereum virtual machine

****************************
The Ethereum Virtual Machine
****************************

Overview
========

The Ethereum Virtual Machine or EVM is the runtime environment
for smart contracts in Ethereum. It is not only sandboxed but
actually completely isolated, which means that code running
inside the EVM has no access to network, filesystem or other processes.
Smart contracts even have limited access to other smart contracts.

.. index:: ! account, address, storage, balance

.. _accounts:

Accounts
========

There are two kinds of accounts in Ethereum which share the same
address space: **External accounts** that are controlled by
public-private key pairs (i.e. humans) and **contract accounts** which are
controlled by the code stored together with the account.

The address of an external account is determined from
the public key while the address of a contract is
determined at the time the contract is created
(it is derived from the creator address and the number
of transactions sent from that address, the so-called "nonce").

Regardless of whether or not the account stores code, the two types are
treated equally by the EVM.

Every account has a persistent key-value store mapping 256-bit words to 256-bit
words called **storage**.

Furthermore, every account has a **balance** in
Ether (in "Wei" to be exact, `1 ether` is `10**18 wei`) which can be modified by sending transactions that
include Ether.

.. index:: ! transaction

Transaktionen
=============

Eine Transaktion ist eine Nachricht die von einem Konto zu einem
anderen Konto (welcher der gleiche oder leer sein kann, siehe unten).
Sie kann Binäraten (den sogenannten "Payload"") und Ether beinhalten.

Wenn das Empfängerkonto Code enthält, dann wird dieser Code
ausgeführt und der Payload wird als Eingabedaten verwendet.

Wenn das Empfängerkonto nicht gesetz wurde (die Transaktion
besitzt keinen Emüfänger oder der Empfänger ist "null"), dann
erstellt die Transatkion einen **neuen Contract**.
Wie bereits erwähnt, ist die Adresse dieses Contracts nicht die
Nulladdresse sondern eine Adresse die von dem Sender
und der Anzahl an Transaktionen (die "Nonce") abgeleitet wird.
Der Payload einer solchen Transaktion, die einen neuen Contract erstellt,
wird als EVM Bytecode entgegengenommen und ausgeführt.
Die Ausgabedaten dieser Ausführung wird permanent als Code
des Contracts gespeichert.
Das bedeutet, um einen Contract zu erstellen, wird nicht der eigentliche
Code des Contracts gesendet, sondern Code, der als Rückgabe den Code
erzeugt wenn dieser ausgeführt wird.


.. note::
  Während ein Contract erstellt wird, ist der Code leer.
  Deshalb sollte auf ihn nicht zugegriffen während, solange
  dieser nicht vollständig erstellt ist.

.. index:: ! gas, ! gas price

Gas
===

Jede Transaktion kostet bei der Erstellung ein gewissen Wert an **Gas**,
welcher sicherstellen soll, dass die notwendige Arbeit um eine Transaktion
auszuführen limitiert ist und um für diese Ausführung zu zahlen.
Während die EMV die Transaktion ausführt, wird das Gas entsprechend spezifischer
Regeln aufgebraucht.

Der **Gas Preis** ist ein Wert der von dem Ersteller der Transatkion erstellt wird,
der den ``gas_price * gas`` vorab von seinem sendenen Account zahlen muss.
Wenn nach der Ausführung Reste an Gas existieren, werden diese an den Ersteller
in gleicher Weise zurück transferiert.

Wenn das Gas an irgendeiner Stelle aufgebraucht wird (d.h. es würde negativ werden), wird eine
out-of-gas Exception ausgelöst, die alle Modifikationen die an dem State innerhalb
des aktuellen Call Frames getätigt wurden rückgängig gemacht werden.

.. index:: ! storage, ! memory, ! stack

Storage, Memory and the Stack
=============================

The Ethereum Virtual Machine has three areas where it can store data-
storage, memory and the stack, which are explained in the following
paragraphs.

Each account has a data area called **storage**, which is persistent between function calls
and transactions.
Storage is a key-value store that maps 256-bit words to 256-bit words.
It is not possible to enumerate storage from within a contract, it is
comparatively costly to read, and even more to initialise and modify storage. Because of this cost,
you should minimize what you store in persistent storage to what the contract needs to run.
Store data like derived calculations, caching, and aggregates outside of the contract.
A contract can neither read nor write to any storage apart from its own.

The second data area is called **memory**, of which a contract obtains
a freshly cleared instance for each message call. Memory is linear and can be
addressed at byte level, but reads are limited to a width of 256 bits, while writes
can be either 8 bits or 256 bits wide. Memory is expanded by a word (256-bit), when
accessing (either reading or writing) a previously untouched memory word (i.e. any offset
within a word). At the time of expansion, the cost in gas must be paid. Memory is more
costly the larger it grows (it scales quadratically).

The EVM is not a register machine but a stack machine, so all
computations are performed on a data area called the **stack**. It has a maximum size of
1024 elements and contains words of 256 bits. Access to the stack is
limited to the top end in the following way:
It is possible to copy one of
the topmost 16 elements to the top of the stack or swap the
topmost element with one of the 16 elements below it.
All other operations take the topmost two (or one, or more, depending on
the operation) elements from the stack and push the result onto the stack.
Of course it is possible to move stack elements to storage or memory
in order to get deeper access to the stack,
but it is not possible to just access arbitrary elements deeper in the stack
without first removing the top of the stack.

.. index:: ! instruction

Instruction Set
===============

The instruction set of the EVM is kept minimal in order to avoid
incorrect or inconsistent implementations which could cause consensus problems.
All instructions operate on the basic data type, 256-bit words or on slices of memory
(or other byte arrays).
The usual arithmetic, bit, logical and comparison operations are present.
Conditional and unconditional jumps are possible. Furthermore,
contracts can access relevant properties of the current block
like its number and timestamp.

For a complete list, please see the :ref:`list of opcodes <opcodes>` as part of the inline
assembly documentation.

.. index:: ! message call, function;call

Message Calls
=============

Contracts can call other contracts or send Ether to non-contract
accounts by the means of message calls. Message calls are similar
to transactions, in that they have a source, a target, data payload,
Ether, gas and return data. In fact, every transaction consists of
a top-level message call which in turn can create further message calls.

A contract can decide how much of its remaining **gas** should be sent
with the inner message call and how much it wants to retain.
If an out-of-gas exception happens in the inner call (or any
other exception), this will be signaled by an error value put onto the stack.
In this case, only the gas sent together with the call is used up.
In Solidity, the calling contract causes a manual exception by default in
such situations, so that exceptions "bubble up" the call stack.

As already said, the called contract (which can be the same as the caller)
will receive a freshly cleared instance of memory and has access to the
call payload - which will be provided in a separate area called the **calldata**.
After it has finished execution, it can return data which will be stored at
a location in the caller's memory preallocated by the caller.
All such calls are fully synchronous.

Calls are **limited** to a depth of 1024, which means that for more complex
operations, loops should be preferred over recursive calls. Furthermore,
only 63/64th of the gas can be forwarded in a message call, which causes a
depth limit of a little less than 1000 in practice.

.. index:: delegatecall, callcode, library

Delegatecall / Callcode and Libraries
=====================================

There exists a special variant of a message call, named **delegatecall**
which is identical to a message call apart from the fact that
the code at the target address is executed in the context of the calling
contract and ``msg.sender`` and ``msg.value`` do not change their values.

This means that a contract can dynamically load code from a different
address at runtime. Storage, current address and balance still
refer to the calling contract, only the code is taken from the called address.

This makes it possible to implement the "library" feature in Solidity:
Reusable library code that can be applied to a contract's storage, e.g. in
order to implement a complex data structure.

.. index:: log

Logs
====

It is possible to store data in a specially indexed data structure
that maps all the way up to the block level. This feature called **logs**
is used by Solidity in order to implement :ref:`events <events>`.
Contracts cannot access log data after it has been created, but they
can be efficiently accessed from outside the blockchain.
Since some part of the log data is stored in `bloom filters <https://en.wikipedia.org/wiki/Bloom_filter>`_, it is
possible to search for this data in an efficient and cryptographically
secure way, so network peers that do not download the whole blockchain
(so-called "light clients") can still find these logs.

.. index:: contract creation

Create
======

Contracts can even create other contracts using a special opcode (i.e.
they do not simply call the zero address as a transaction would). The only difference between
these **create calls** and normal message calls is that the payload data is
executed and the result stored as code and the caller / creator
receives the address of the new contract on the stack.

.. index:: selfdestruct, self-destruct, deactivate

Deactivate and Self-destruct
============================

The only way to remove code from the blockchain is when a contract at that
address performs the ``selfdestruct`` operation. The remaining Ether stored
at that address is sent to a designated target and then the storage and code
is removed from the state. Removing the contract in theory sounds like a good
idea, but it is potentially dangerous, as if someone sends Ether to removed
contracts, the Ether is forever lost.

.. warning::
    Even if a contract is removed by "selfdestruct", it is still part of the
    history of the blockchain and probably retained by most Ethereum nodes.
    So using "selfdestruct" is not the same as deleting data from a hard disk.

.. note::
    Even if a contract's code does not contain a call to ``selfdestruct``,
    it can still perform that operation using ``delegatecall`` or ``callcode``.

If you want to deactivate your contracts, you should instead **disable** them
by changing some internal state which causes all functions to revert. This
makes it impossible to use the contract, as it returns Ether immediately.
