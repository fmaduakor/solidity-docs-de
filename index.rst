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
    As humans write software, it can have bugs. You should follow established
    software development best-practices when writing your smart contracts, this
    includes code review, testing, audits, and correctness proofs. Smart contract
    users are sometimes more confident with code than their authors, and
    blockchains and smart contracts have their own unique issues to
    watch out for, so before working on production code, make sure you read the
    :ref:`security_considerations` section.

If you have any questions, you can try searching for answers or asking on the
`Ethereum Stackexchange <https://ethereum.stackexchange.com/>`_, or
our `gitter channel <https://gitter.im/ethereum/solidity/>`_.

Ideas for improving Solidity or this documentation are always welcome,
read our :doc:`contributors guide <contributing>` for more details.

.. _translations:

Translations
------------

Community volunteers help translate this documentation into several languages.
They have varying degrees of completeness and up-to-dateness. The English
version stands as a reference.

* `French <http://solidity-fr.readthedocs.io>`_ (in progress)
* `Italian <https://github.com/damianoazzolini/solidity>`_ (in progress)
* `Japanese <https://solidity-jp.readthedocs.io>`_
* `Korean <http://solidity-kr.readthedocs.io>`_ (in progress)
* `Russian <https://github.com/ethereum/wiki/wiki/%5BRussian%5D-%D0%A0%D1%83%D0%BA%D0%BE%D0%B2%D0%BE%D0%B4%D1%81%D1%82%D0%B2%D0%BE-%D0%BF%D0%BE-Solidity>`_ (rather outdated)
* `Simplified Chinese <http://solidity-cn.readthedocs.io>`_ (in progress)
* `Spanish <https://solidity-es.readthedocs.io>`_
* `Turkish <https://github.com/denizozzgur/Solidity_TR/blob/master/README.md>`_ (partial)

Contents
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
