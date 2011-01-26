Obraz całości
===============

Tak więc, chciałbyś wypróbować Symfony2 ale masz tylko 10 minut? 
Pierwsza część tego tutoriala została napisana właśnie dla Ciebie!
Tłumaczy jak szybko zacząć pracę z Symfony2 pokazując Ci strukturę prostego, gotowego projektu. 

Jeśli kiedykolwiek używałeś framework'a webowego to z Symfony2 powinieneś czuć się jak w domu.

.. index::
   pair: Sandbox; Download

Pobieranie i instalacja Symfony2
-----------------------------------

Pierwsze, sprawdź czy masz ściągnięte i poprawnie skonfigurowane do pracy z serwerem(takim jak np. Apache) PHP w wersji co najmniej 5.3.2.  

Gotowy? Zacznijmy od ściągnięcia Symfony2. Aby rozpocząć jeszcze szybciej, użyjemy "Symfony2 sandbox".
Jest to projekt Symfony2 w którym znajdują się wszystkie wymagane biblioteki i parę prostych kontrolerów, ponadto podstawowa konfiguracja jest już ukończona. Największą zaletą sandbox'a ponad innymi typami instalacji jest to, że możesz od razu zacząć z nim eksperymentować. 

Pobierz `sandbox`_ i rozpakuj go do Twojego głównego katalogu. Powinieneś mieć teraz katalog  "Sandbox/":

    www/ <- Twój folder główny
        sandbox/ <- rozpakowane archiwum
            app/
                cache/
                config/
                logs/
            src/
                Application/
                    HelloBundle/
                        Controller/
                        Resources/
                vendor/
                    symfony/
            web/

.. index::
   single: Installation; Check

Sprawdzanie konfiguracji
--------------------------

Aby uniknąć późniejszych problemów, sprawdź czy Twoja konfiguracja może płynnie uruchomić projekt Symfony2, otwierając podany adres:

    http://localhost/sandbox/web/check.php

Odczytaj uważnie dane wyjściowe skryptu i napraw problemy, które zostały znalezione.

Teraz, otwórz swoją pierwszą "prawdziwą" stronę opartą na Symfony2:

    http://localhost/sandbox/web/app_dev.php/

Symfony2 powinno pogratulować Ci Twojej dotychczasowej pracy!

Tworzenie Twojej pierwszej aplikacji
-------------------------------

W sandboxie dołączona jest prosta ":term:`aplikacja`" Hello World i to jest właśnie aplikacja, które użyjemy aby dowiedzieć się więcej o Symfony2.
Odwiedź podany adres, aby zostać przywitanym przez Symfony2 (zamień Fabien na swoje imię):

    http://localhost/sandbox/web/app_dev.php/hello/Fabien

Co się tutaj dzieje? Przyjrzyjmy się adresowi URL:

.. index:: Front Controller

* ``app_dev.php``: Jest to "front controller" - wyjątkowy punkt wejściowy aplikacji, który odpowiada na wszystkie zapytania użytkownika;

* ``/hello/Fabien``: Jest to "wirtualna" ścieżka zasobu do którego użytkownik chce mieć dostęp. 

Jako deweloper jesteś odpowiedzialny za napisanie kodu, który mapuje zapytanie użytkownika (``/hello/Fabien``) z zasobem powiązanym z nim  (``Hello
Fabien!``).

.. index::
   single: Configuration

Konfiguracja
~~~~~~~~~~~~~

W jaki sposób Symfony2 kieruje zapytanie do Twojego kodu? Po prostu przez odczytanie pewnego pliku konfiguracyjnego. 

Wszystkie pliki konfiguracyjne Symfony2 mogą być zapisane zarówno w PHP, XML, jak i w `YAML`_(YAML jest to prosty format dzięki któremu zapis plików konfiguracyjnych jest bardzo prosty).

.. tip::

   Domyślnie sandbox używa YAML, ale możesz łatwo przełączyć się na XML lub PHP przez edycje pliku ``app/AppKernel.php`.
   Możesz przełączyć się teraz, patrząc na instrukcje u dołu tego pliku(tutoriale pokazują konfigurację dla wszystkich obsługiwanych formatów).

.. index::
   single: Routing
   pair: Configuration; Routing

Routing
~~~~~~~

Tak więc, symfony2 kieruje zapytanie przez odczyt pliku konfiguracyjnego routingu:

.. configuration-block::

    .. code-block:: yaml

        # app/config/routing.yml
        homepage:
            pattern:  /
            defaults: { _controller: FrameworkBundle:Default:index }

        hello:
            resource: HelloBundle/Resources/config/routing.yml

    .. code-block:: xml

        <!-- app/config/routing.xml -->
        <?xml version="1.0" encoding="UTF-8" ?>

        <routes xmlns="http://www.symfony-project.org/schema/routing"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://www.symfony-project.org/schema/routing http://www.symfony-project.org/schema/routing/routing-1.0.xsd">

            <route id="homepage" pattern="/">
                <default key="_controller">FrameworkBundle:Default:index</default>
            </route>

            <import resource="HelloBundle/Resources/config/routing.xml" />
        </routes>

    .. code-block:: php

        // app/config/routing.php
        use Symfony\Component\Routing\RouteCollection;
        use Symfony\Component\Routing\Route;

        $collection = new RouteCollection();
        $collection->add('homepage', new Route('/', array(
            '_controller' => 'FrameworkBundle:Default:index',
        )));
        $collection->addCollection($loader->import("HelloBundle/Resources/config/routing.php"));

        return $collection;

Pierwsze parę linii pliku konfiguracyjnego routingu definiuje, który kod wywołać gdy użytkownik żąda zasobu  "``/``".
Bardziej interesująca jest ostatnia część, która importuje kolejny plik konfiguracyjny routingu, wyglądający tak:

.. configuration-block::

    .. code-block:: yaml

        # src/Application/HelloBundle/Resources/config/routing.yml
        hello:
            pattern:  /hello/:name
            defaults: { _controller: HelloBundle:Hello:index }

    .. code-block:: xml

        <!-- src/Application/HelloBundle/Resources/config/routing.xml -->
        <?xml version="1.0" encoding="UTF-8" ?>

        <routes xmlns="http://www.symfony-project.org/schema/routing"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://www.symfony-project.org/schema/routing http://www.symfony-project.org/schema/routing/routing-1.0.xsd">

            <route id="hello" pattern="/hello/:name">
                <default key="_controller">HelloBundle:Hello:index</default>
            </route>
        </routes>

    .. code-block:: php

        // src/Application/HelloBundle/Resources/config/routing.php
        use Symfony\Component\Routing\RouteCollection;
        use Symfony\Component\Routing\Route;

        $collection = new RouteCollection();
        $collection->add('hello', new Route('/hello/:name', array(
            '_controller' => 'HelloBundle:Hello:index',
        )));

        return $collection;

No i lecimy! Jak widzisz, wzór zasobu: "``/hello/:name``" (gdzie dwukropek na początek stringu oznacza, że jest to wartość zastępcza - placeholder ) jest zmapowany do kontrolera, określonego w wartości klucza ``_controller`.

.. index::
   single: Controller
   single: MVC; Controller

Kontrolery
~~~~~~~~~~~

Kontroler jest odpowiedzialny za zwrócenie reprezentacji zasobu(głównie w postaci HTML) i jest zdefiniowany jako klasa PHP:

.. code-block:: php
   :linenos:

    // src/Application/HelloBundle/Controller/HelloController.php

    namespace Application\HelloBundle\Controller;

    use Symfony\Bundle\FrameworkBundle\Controller\Controller;

    class HelloController extends Controller
    {
        public function indexAction($name)
        {
            return $this->render('HelloBundle:Hello:index.twig', array('name' => $name));

            // render a PHP template instead
            // return $this->render('HelloBundle:Hello:index.php', array('name' => $name));
        }
    }

Kod jest dosyć prosty, ale wytłumaczmy go linijka po linijce:

* *linia 3*: Symfony 2 korzysta z nowych możliwości PHP 5.3, tak więc kontrolery są poprawnie przypisane do przestrzeni nazw(Przestrzenią nazw jest pierwsza część wartości ``_controller` z routingu, w tym przypadku: ``HelloBundle).

* *line 7*: The controller name is the concatenation of the second part of the
  ``_controller`` routing value (``Hello``) and ``Controller``. It extends the
  built-in ``Controller`` class, which provides useful shortcuts (as we will
  see later in this tutorial).

* *line 9*: Each controller is made of several actions. As per the
  configuration, the hello page is handled by the ``index`` action (the third
  part of the ``_controller`` routing value). This method receives the
  resource placeholder values as arguments (``$name`` in our case).

* *line 11*: The ``render()`` method loads and renders a template
  (``HelloBundle:Hello:index.twig``) with the variables passed as a second
  argument.

But what is a :term:`bundle`? All the code you write in a Symfony2 project is
organized in bundles. In Symfony2 speak, a bundle is a structured set of files
(PHP files, stylesheets, JavaScripts, images, ...) that implements a single
feature (a blog, a forum, ...) and which can be easily shared with other
developers. In our example, we only have one bundle, ``HelloBundle``.

Templates
~~~~~~~~~

So, the controller renders the ``HelloBundle:Hello:index.twig`` template. But
what's in a template name? ``HelloBundle`` is the bundle name, ``Hello`` is
the controller, and ``index.twig`` the template name. By default, the sandbox
uses Twig as its template engine:

.. code-block:: jinja

    {# src/Application/HelloBundle/Resources/views/Hello/index.twig #}
    {% extends "HelloBundle::layout.twig" %}

    {% block content %}
        Hello {{ name }}!
    {% endblock %}

Congratulations! You have looked at your first Symfony2 piece of code. That was
not so hard, was it? Symfony2 makes it really easy to implement web sites
better and faster.

.. index::
   single: Environment
   single: Configuration; Environment

Working with Environments
-------------------------

Now that you have a better understanding on how Symfony2 works, have a closer
look at the bottom of the page; you will notice a small bar with the Symfony2
and PHP logos. It is called the "Web Debug Toolbar" and it is the developer's
best friend. Of course, such a tool must not be displayed when you deploy your
application to your production servers. That's why you will find another front
controller in the ``web/`` directory (``app.php``), optimized for the
production environment:

    http://localhost/sandbox/web/app.php/hello/Fabien

And if you use Apache with ``mod_rewrite`` enabled, you can even omit the
``app.php`` part of the URL:

    http://localhost/sandbox/web/hello/Fabien

Last but not least, on the production servers, you should point your web root
directory to the ``web/`` directory to secure your installation and have an even
better looking URL:

    http://localhost/hello/Fabien

To make the production environment as fast as possible, Symfony2 maintains a
cache under the ``app/cache/`` directory. When you make changes to the code or
configuration, you need to manually remove the cached files. That's why you
should always use the development front controller (``app_dev.php``) when
working on a project.

Final Thoughts
--------------

The 10 minutes are over. By now, you should be able to create your own simple
routes, controllers, and templates. As an exercise, try to build something
more useful than the Hello application! But if you are eager to learn more
about Symfony2, you can read the next part of this tutorial right away, where
we dive more into the templating system.

.. _sandbox: http://symfony-reloaded.org/code#sandbox
.. _YAML:    http://www.yaml.org/
