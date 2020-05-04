.. index:: ! contract

.. _contracts:

##########
Contracts
##########

Contracts in Solidity sind vergleichbar mit Klassen in objektorientierten Sprachen.
Sie besitzen gespeicherte Daten in state Variablen und Funktionen die diese Variablen ändern können.
Der Aufruf einer Funktion von einem anderen Contract führt einen EVM Funktionsaufruf aus und ändert den Kontext
des Aufrufes, so dass die state Variablen im aufrufenden Contract nicht zugreifbar sind.
Damit etwas passieren kann muss ein Contract und eine seiner Funktionen aufgerufen werden. Es existiert
in Ethereum kein "cron" Konzept wodurch eine Funktion bei einem eintretenden Event automatisch aufgerufen wird. 

.. include:: contracts/creating-contracts.rst

.. include:: contracts/visibility-and-getters.rst

.. include:: contracts/function-modifiers.rst

.. include:: contracts/constant-state-variables.rst
.. include:: contracts/functions.rst

.. include:: contracts/events.rst

.. include:: contracts/inheritance.rst

.. include:: contracts/abstract-contracts.rst
.. include:: contracts/interfaces.rst

.. include:: contracts/libraries.rst

.. include:: contracts/using-for.rst