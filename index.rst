Solidity
========

.. image:: logo.svg
    :width: 120px
    :alt: Solidity logo
    :align: center

Solidity ist eine objektorientierte, höhere Programmiersprache um Smart Contracts zu implementieren.
Smart Contracts sind Programme die das Verhalten von Accounts innerhalb
des Ethereum States verwalten.

Solidity wurde von C++, Python und JavaScript beeinflusst und wurde für die
Ethereum Virtual Machine (EVM) entworfen.

Solidity ist statisch typisiert, unterstützt Vererbung, Bibliotheken, komplexe
benutzerdefinierte Typen und viele weitere Konzepte.

Mit Solidity ist es möglich Contracts zu erstellen, die für Wahlen, Crowdfunding, blinde Auktionen
und Multi-Signature Wallets eingesetzt werden können.

Bei dem Deployment von Contracts, sollte stets die neuste Version von Solidity verwendet werden.
Breaking Changes, neue Features und Bugfixes werden regelmäßig implementiert.
Aktuell befinden wir uns bei einer 0.x Versionsnummer, `was die schnelle Entwicklung zeigen soll<https://semver.org/#spec-item-4>`_.

.. warning::
  Neulich wurde die Solidity Version 0.6.x veröffentlicht, die eine Vielzahl an Breaking Changes
  einführt. Bitte lies :doc:`die vollständige Liste der Breaking Changes <060-breaking-changes>`.

Dokumentation
----------------------

Wenn das Konzept von Smart Contracts neu für dich ist, empfehlen wir dir mit dem Kapital 
:ref:`Simple Beispiele in Solidity <simple-smart-contract>` zu beginnen.
Wenn du bereit für mehr Details bist, dann kannst du in den Kapiteln
:doc:`"Solidity anhand von Beispielen" <solidity-by-example>` und
:doc:`"Details von Solidity" <solidity-in-depth>` die Grundkonzepte der Sprache lernen.

Möchtest du mehr Informationen, schau dir :ref:`die Grundkonzepte der Blockchains <blockchain-basics>` 
und die Details der :ref:`Ethereum Virtual Machine <the-ethereum-virtual-machine>` an.

.. hint::
  Du kannst jederzeit Cocdebeispiele in deinem Browser mit der 
  `Remix IDE <https://remix.ethereum.org>`_ testen. Remix ist eine webbasierte IDE, die es dir erlaubt Solidity Smart Contracts zu erstellen, zu deployen und auszuführen.
  Bitte beachte, dass es sein kann, dass sie einige Zeit zum Laden benötigt.

.. warning::
    Da Software von Menschen geschrieben wird, kann sie Bugs enthalten.
    Bitte achte bei der Erstellung von Smart Contracts darauf Best-Practises einzuhalten.
    Dies beinhaltet ein Code Review, Testen, Auditierung und Korrektheitsbeweise.
    Die Nutzer von Smart Contracts vertrauen diesen manchmal mehr als deren Entwicklern.
    Blockchains und Smart Contracts haben individuelle Probleme auf die geachtet werden sollen.
    Bevor du Smart Contracts für Nutzer einsetzbar machst, lies bitte den Abschnitt  :ref:`security_considerations` section.

Solltest du Fragen haben, kannst du bei 
`Ethereum Stackexchange <https://ethereum.stackexchange.com/>`_,  oder unserem
 `Gitter Channel <https://gitter.im/ethereum/solidity/>`_ nach Antworten zu suchen oder deine Fragen stellen.

Wir freuen uns über Ideen wie Solidity oder diese Dokumentation verbessert werden kann.
Lies unseren :doc:`contributors guide <contributing>` für mehr Details hierfür.


.. _translations:

Überetzungen
------------

Frewillige aus der Community helfen dabei diese Dokumentation in mehrere Sprachen zu übersetzen.
Diese Dokumenten sind teilweise nicht up-to-date oder vollständig verfügbar. Die englische Version gilt stets als Referenz.

* `French <http://solidity-fr.readthedocs.io>`_ (In Bearbeitung)
* `Italian <https://github.com/damianoazzolini/solidity>`_ (In Bearbeitung)
* `Japanese <https://solidity-jp.readthedocs.io>`_
* `Korean <http://solidity-kr.readthedocs.io>`_ (In Bearbeitung)
* `Russian <https://github.com/ethereum/wiki/wiki/%5BRussian%5D-%D0%A0%D1%83%D0%BA%D0%BE%D0%B2%D0%BE%D0%B4%D1%81%D1%82%D0%B2%D0%BE-%D0%BF%D0%BE-Solidity>`_ (eher veraltet)
* `Simplified Chinese <http://solidity-cn.readthedocs.io>`_ (In Bearbeitung)
* `Spanish <https://solidity-es.readthedocs.io>`_
* `Turkish <https://github.com/denizozzgur/Solidity_TR/blob/master/README.md>`_ (teilweise)

Inhalte
========

:ref:`Keyword Index <genindex>`, :ref:`Search Page <search>`

.. toctree::
   :maxdepth: 2

   introduction-to-smart-contracts.rst
   installing-solidity.rst
   solidity-by-example.rst
   solidity-in-depth.rst
   natspec-format.rst
   security-considerations.rst
   resources.rst
   using-the-compiler.rst
   metadata.rst
   abi-spec.rst
   yul.rst
   style-guide.rst
   common-patterns.rst
   bugs.rst
   contributing.rst
file:///home/development/Desktop/solidity-docs/solidity-docs-de/index.rst
