.. index::
   single: Bundles; Best Practices

Bundle - Najlepsze praktyki
=====================

Bundle jest katalogiem, który ma zdefiniowaną strukturę i może zawierać wszystko, 
począwszy od klas kontrolerów oraz zasobów internetowych. Nawet jeśli bundle są bardzo 
elastyczne, jeśli chcesz je rozprowadzać powinieneś przestrzegać pewnych najlepszych praktyk.

.. index::
   pair: Bundles; Naming Conventions

Nazwa Bundla
------------

Bundle to również namespace w PHP, składa się z kilku segmentów:

* **Główna przestrzeń nazw**: ``Bundle``, dla wielokrotnie używalnych, lub ``Application`` dla używanych tylko w danej aplikacji.;
* **Nazwa firmy/programisty**  (opcjonalne dla ``Application``): coś unikalnego dla Ciebie lub Twojej firmy (np. ``Sensio`` );
* *(Opcjonalnie)* **nazwa kategorii** , dla lepszej organizacji dużych zestawów Bundli;
* ""Nazwa Bundla**.

Nazwa Bundla musi przestrzegać następujących zasad:

* Używać tylko znaków alfanumerycznych i podkreśleń;
* Używać stylu zapisu CamelCased;
* Użyj opisowej i krótkiej nazwy (nie więcej niż 2 słowa);
* Używać prefiksu składającego się z połączenia nazwy producenta i kategorii;
* Używać sufiksu ``Bundle``.

Trochę dobrych przykładów nazw:

=================================== ==========================
Namespace                           Nazwa Bundla
=================================== ==========================
``Bundle\Sensio\BlogBundle``        ``SensioBlogBundle``
``Bundle\Sensio\Social\BlogBundle`` ``SensioSocialBlogBundle``
``Application\BlogBundle``          ``BlogBundle``
=================================== ==========================

Struktura katalogów
-------------------

Podstawowa struktura katalogów ``HelloBundle`` powinna wyglądać tak jak poniżej:

    XXX/...
        HelloBundle/
            HelloBundle.php
            Controller/
            Resources/
                meta/
                    LICENSE
                config/
                doc/
                    index.rst
                translations/
                views/
                public/
            Tests/

Katalog ``XXX`` odzwierciedla strukturę namespace'a bundla. 

Poniższe pliki są obowiązkowe:

* ``HelloBundle.php``;
* ``Resources/meta/LICENSE``: Pełna licencja dla kodu bundla.
* ``Resources/doc/index.rst``: Główny plik dokumentacji bundla.

.. note::

    Ta konwencja zapewnia że automatyczne procesy Symfony będą działać z bundlem.


Głębokość struktury katalogów powinna być ograniczone do minimum dla najczęściej 
używanych klas i plików (maksimum 2 poziomy). Wieksza ilość poziomów może być użyta 
dla mało ważnych, rzadko używanych plików.


Katalog bundla jest tylko do odczytu. Jeśli chcesz tworzyć pliki tymczasowe, umieszczaj je
w  ``cache/`` lub ``log/`` hosta aplikacji. Narzędzia mogą generować pliki w strukturze 
katalogów bundla, ale tylko wtedy, gdy wygenerowane pliki będą część repozytorium.

Poniższe klasy i pliki mają ustalone rozmieszczenie.

========================= ===========================
Typ                       Katalog
========================= ===========================
Kontrolery                ``Controller/``
Pliki tłumaczeń           ``Resources/translations/``
Szablony                  ``Resources/views/``
Testy                     ``Tests/``
Zasoby                    ``Resources/public/``
Konfiguracja              ``Resources/config/``
Polecenia                 ``Command/``
========================= ===========================

Klasy
-------

Struktura katalogu bundla jest używana w hierarchii nazwy namespace'a. Na przykład,
kontroler ``HelloController``znajduje się w 
``Bundle/HelloBundle/Controller/HelloController.php`` i w pełni poprawną nazwą jego 
klasy jest ``Bundle\HelloBundle\Controller\HelloController``.

Wszystkie klasy i pliki muszą być zgodne ze :doc:`stylem kodowania</contributing/code/standards>` Symfony2 .

Niektóre klasy powinny być postrzegane jako fasady i powinny być jak najkrótsze, 
np.  Polecenia, Helpery, Listenery, i Kontrolery.

Klasy, które łączą się do Event Dispatcher powinie być zapierac suffix ``Listener``.

Klas wyjątków powinne być przechowywane kategori ``Exception`` danego namespace'a.



Bibliotek zewnętrzne
-------

A bundle should not embed third-party libraries written in JavaScript, CSS, or
any other language.

Bundle nie może zawierać zewnętrznych bibliotek PHP. Powinien polegać na 
standardzie auto ładowania klas przez Symfony2.

Pakiet nie powinien zawierać zewnętrznych bibliotek napisanych w JavaScript, 
CSS, lub jakimkolwiek innym języku.

Testy
-----

Bundle powinien być dostarczany z gotowym zestawem testów napisanych z wykorzystaniem PHPUnit 
znajdujacych się w katalogu ``Tests/``. Testy powinne przestrzegać poniższych zasad:

* Zestaw testowy musi być prosto uruchamialny poprzez polecenie ``phpunit``, 
  uruchamiane z przykładowej aplikacji.
* Testy funkcjonalne powinny być używane tylko do badania wysyłanej odpowiedzi oraz 
  niektórych danych profilujących (jeśli jakieś posiadasz);
* Kod powienien być pokryty testami przynajmniej w 95%.

.. note::
   A test suite must not contain ``AllTests.php`` scripts, but must rely on the
   existence of a ``phpunit.xml.dist`` file.
  
  Zestaw testów nie musi zawierać AllTests.php, jednak musi opierać się na 
  istnieniu pliku ``phpunit.xml.dist``.


Dokumentacja
-------------

Wszystkie klasy i funkcje muszą posiadać pełną dokumentację "phpDoc".

Dokładną dokumentację powinna być dostarczana również w formacie :doc:`reStructuredText
</contributing/documentation/format>`, w  katalogu ``Resources/doc/``; 
Plik ``Resources/doc/index.rst`` jest obowiązkowy.

Kontrolery
-----------

Kontrolery w bundlu nie muszą dziedziczyć po 
:class:`Symfony\\Bundle\\FrameworkBundle\\Controller\\Controller`. 
Mogą implementować
:class:`Symfony\\Foundation\\DependencyInjection\\ContainerAwareInterface` lub 
dziedziczyć po :class:`Symfony\\Foundation\\DependencyInjection\\ContainerAware`.

.. note::

    Jesli szukasz metod 
    :class:`Symfony\\Bundle\\FrameworkBundle\\Controller\\Controller`,
    zobaczysz że są one tylko ładnymi skrótami do `Symfony\\Foundation\\DependencyInjection\\ContainerAwareInterface`
    do łatwiejszej nauki.

Szablony
---------

Jeśli bundel posiada szablony, to powinne one być napisane w PHP. Bundle nie musi 
dostarczać głównego układu, jednak musi wtedy dziedziczyć po ``domyślnym`` (który 
musi dostarczać dwa sloty: `content`` i ``head``).

.. note::

    Jedynym innym wspieranym silnikiem szablonów jest Twig, jednak tylko 
    w wybranych przypadkach.

Pliki tłumaczeń
-----------------

If a bundle provides message translations, they must be defined in the XLIFF
format; the domain should be named after the bundle name (``bundle.hello``).

A bundle must not override existing messages from another bundle.

Configuration
-------------

Configuration must be done via the Symfony2 built-in :doc:`mechanism
</guides/bundles/configuration>`. A bundle should provide all its default
configurations in XML.
