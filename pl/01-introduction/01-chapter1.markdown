# Pierwsze kroki #

Ten rozdział poświęcony jest pierwszym krokom z Git. Rozpoczyna się krótkim wprowadzeniem do narzędzi kontroli wersji, następnie przechodzi do instalacji i początkowej konfiguracji Git. Po przeczytaniu tego rozdziału powinieneś rozumieć w jakim celu Git został stworzony, dlaczego warto z niego korzystać oraz być przygotowany do używania go.

## Wprowadzenie do kontroli wersji ##

Czym jest kontrola wersji i dlaczego powinieneś się nią przejmować? System kontroli wersji śledzi wszystkie zmiany dokonywane na pliku (lub plikach) i umożliwia przywołanie dowolnej wcześniejszej wersji. Przykłady w tej książce będą śledziły zmiany w kodzie źródłowym, niemniej w ten sam sposób można kontrolować praktycznie dowolny typ plików.

Jeśli jesteś grafikiem lub projektantem WWW i chcesz zachować każdą wersję pliku graficznego lub układu witryny WWW (co jest wysoce prawdopodobne), to używanie systemu kontroli wersji (VCS-Version Control System) jest bardzo rozsądnym rozwiązaniem. Pozwala on przywrócić plik(i) do wcześniejszej wersji, odtworzyć stan całego projektu, porównać wprowadzone zmiany, dowiedzieć się kto jako ostatnio zmodyfikował część projektu powodującą problemy, kto i kiedy wprowadził daną modyfikację. Oprócz tego używanie VCS oznacza, że nawet jeśli popełnisz błąd lub stracisz część danych, naprawa i odzyskanie ich powinno być łatwe. Co więcej, wszystko to można uzyskać całkiem niewielkim kosztem.

### Lokalne systemy kontroli wersji ###

Dla wielu ludzi preferowaną metodą kontroli wersji jest kopiowanie plików do innego katalogu (może nawet oznaczonego datą, jeśli są sprytni). Takie podejście jest bardzo częste ponieważ jest wyjątkowo proste, niemniej jest także bardzo podatne na błędy. Zbyt łatwo zapomnieć w jakim jest się katalogu i przypadkowo zmodyfikować błędny plik lub skopiować nie te dane.

Aby poradzić sobie z takimi problemami, programiści już dość dawno temu stworzyli lokalne systemy kontroli wersji, które składały się z prostej bazy danych w której przechowywane były wszystkie zmiany dokonane na śledzonych plikach (por. Rysunek 1-1).

Insert 18333fig0101.png 
Rysunek 1-1. Diagram lokalnego systemu kontroli wersji.

Jednym z najbardziej popularnych narzędzi VCS był system rcs, który wciąż jest obecny na wielu dzisiejszych komputerach. Nawet w popularny systemie operacyjnym Mac OS X rcs jest dostępny po zainstalowaniu Narzędzi Programistycznych (Developer Tools). Program ten działa zapisując, w specjalnym formacie na dysku, dane różnicowe (to jest zawierające jedynie różnice pomiędzy plikami) z każdej dokonanej modyfikacji. Używając tych danych jest w stanie przywołać stan pliku z dowolnego momentu.

### Scentralizowane systemy kontroli wersji ###

Kolejnym poważnym problemem z którym można się spotkać jest potrzeba współpracy w rozwoju projektu z odrębnych systemów. Aby poradzić sobie z tym problemem stworzono scentralizowane systemy kontroli wersji (CVCS-Centralized Version Control System). Systemy takie jak CVS, Subversion czy Perforce składają się z jednego serwera, który zawiera wszystkie pliki poddane kontroli wersji, oraz klientów którzy mogą się z nim łączyć i uzyskać dostęp do najnowszych wersji plików. Przez wiele lat był to standardowy model kontroli wersji (por. Rysunek 1-2).

Insert 18333fig0102.png 
Rysunek 1-2. Diagram scentralizowanego systemu kontroli wersji.

Taki schemat posiada wiele zalet, szczególnie w porównaniu z VCS. Dla przykładu każdy może się zorientować co robią inni uczestnicy projektu. Administratorzy mają dokładną kontrolę nad uprawnieniami poszczególnych użytkowników. Co więcej systemy CVCS są także dużo łatwiejsze w zarządzaniu niż lokalne bazy danych u każdego z klientów.

Niemniej systemy te mają także poważne wady. Najbardziej oczywistą jest problem awarii centralnego serwera. Jeśli serwer przestanie działać na przykład na godzinę, to przez tę godzinę nikt nie będzie miał możliwości współpracy nad projektem, ani nawet zapisania zmian nad którymi pracował. Jeśli dysk twardy na którym przechowywana jest centralna baza danych zostanie uszkodzony a nie tworzono żadnych kopii zapasowych, to można stracić absolutnie wszystko-całą historię projektu, może oprócz pojedynczych jego części zapisanych na osobistych komputerach niektórych użytkowników. Lokalne VCS mają ten sam problem-zawsze gdy cała historia projektu jest przechowywana tylko w jednym miejscu, istnieje ryzyko utraty większości danych.

### Rozproszone systemy kontroli wersji ###

W ten sposób dochodzimy do rozproszonych systemów kontroli wersji (DVCS-Distributed Version Control System). W systemach DVCS (takich jak Git, Mercurial, Bazaar lub Darcs) klienci nie dostają dostępu jedynie do najnowszych wersji plików ale w pełni kopiują całe repozytorium. Gdy jeden z serwerów, używanych przez te systemy do współpracy, ulegnie awarii, repozytorium każdego klienta może zostać po prostu skopiowane na ten serwer w celu przywrócenia go do pracy (por. Rysunek 1-3).


Insert 18333fig0103.png 
Figure 1-3. Diagram rozproszonego systemu kontroli wersji.

Co więcej, wiele z tych systemów dość dobrze radzi sobie z kilkoma zdalnymi repozytoriami, więc możliwa jest jednoczesna współpraca z różnymi grupami ludzi nad tym samym projektem. Daje to swobodę wykorzystania różnych schematów pracy, nawet takich które nie są możliwe w scentralizowanych systemach, na przykład modeli hierarchicznych.

## Krótka historia Git ##

Jak z wieloma dobrymi rzeczami w życiu Git zaczął od odrobiny twórczej destrukcji oraz zażartych kontrowersji. Jądro Linuksa jest dość dużym projektem otwartego oprogramowania (ang. open source). Przez większą część życia tego projektu (1991-2002), zmiany w źródle były przekazywane jako łaty (ang. patches) i zarchiwizowane pliki. W roku 2002 projekt jądra Linuksa zaczął używać systemu DVCS BitKeeper.

W 2005 roku relacje pomiędzy wspólnotą rozwijającą jądro Linuksa a firmą która stworzyła BitKeepera znacznie się pogorszyły, a pozwolenie na nieodpłatne używanie systemu zostało cofnięte. To skłoniło programistów pracujących nad jądrem (a w szczególności Linusa Torvaldsa, twórcę Linuksa) do stworzenia własnego systemu na podstawie wiedzy wyniesionej z używania BitKeepera. Do celów tego nowego systemu należały:

*	Szybkość
*	Prosta konstrukcja
*	Silne wsparcie dla nieliniowego rozwoju (tysięcy równoległych gałęzi)
*	Pełne rozproszenie
*	Wydajna obsługa dużych projektów, takich jak jądro Linuksa (szybkość i rozmiar danych)

Od swoich narodzin w 2005 roku, Git ewoluował i ustabilizował się jako narzędzie łatwe w użyciu, jednocześnie zachowując wyżej wymienione cechy. Jest niewiarygodnie szybki, bardzo wydajny przy pracy z dużymi projektami i posiada niezwykły system gałęzi do nieliniowego rozwoju (patrz Rozdział 3).

## Podstawy Git ##

Czym jest w skrócie Git? To jest bardzo istotna sekcja tej książki, ponieważ jeśli zrozumiesz czym jest Git i podstawy jego działania to efektywne używanie go powinno być dużo prostsze. Podczas uczenia się Git staraj się nie myśleć o tym co wiesz o innych systemach VCS, takich jak Subversion czy Perforce; pozwoli Ci to uniknąć subtelnych błędów przy używaniu tego narzędzia. Git przechowuje i traktuje informacje kompletnie inaczej niż te pozostałe systemy, mimo że interfejs użytkownika jest dość zbliżony. Rozumienie tych różnic powinno pomóc Ci w unikaniu błędów przy korzystaniu z Git.

### Migawki, nie różnice ###

Podstawową różnicą pomiędzy Git a każdym innym systemem VCS (włączając w to Subversion) jest podejście Git do przechowywanych danych. Większość pozostałych systemów przechowuje informacje jako listę zmian na plikach. Systemy te (CVS, Subversion, Perforce, Bazaar i inne) traktują przechowywane informacje jako zbiór plików i zmian dokonanych na każdym z nich w okresie czasu. Obrazuje to Rysunek 1-4.

Insert 18333fig0104.png 
Rysunek 1-4. Inne system przechowują dane w postaci zmian do podstawowej wersji każdego z plików.

Git podchodzi do przechowywanie danych w odmienny sposób. Traktuje on dane podobnie jak zestaw migawek (ang. snapshots) małego systemu plików. Za każdym razem jak tworzysz commit lub zapisujesz stan projektu, Git tworzy obraz przedstawiający to jak wyglądają wszystkie pliki w danym momencie i przechowuje referencję do tej migawki. W celu uzyskanie dobrej wydajności, jeśli dany plik nie został zmieniony, Git nie zapisuje ponownie tego pliku, a tylko referencję do jego poprzedniej, identycznej wersji, która jest już zapisana. Git myśli o danych w sposób podobny do przedstawionego na Rysunku 1-5.

Insert 18333fig0105.png 
Figure 1-5. Git przechowuje dane jako migawki projektu w okresie czasu.

To jest istotna różnica pomiędzy Git i prawie wszystkimi innymi systemami VCS. Jej konsekwencją jest to, że Git rewiduje prawie wszystkie aspekty kontroli wersji, które pozostałe systemy po prostu kopiowały z poprzednich generacji. Powoduje także, że Git jest bardziej podobny do mini systemu plików ze zbudowanymi na nim potężnymi narzędziami, niż do zwykłego systemu VCS. Odkryjemy niektóre z zalet które zyskuje się poprzez myślenie o danych w ten sposób, gdy w trzecim rozdziale będziemy omawiać tworzenie gałęzi w Git.

### Prawie każda operacja jest lokalna ###

Większość operacji w Git wymaga jedynie do pracy lokalnych plików i zasobów - generalnie żadna informacja nie jest potrzebna z innego komputera z twojej sieci. Jeżeli jesteś przyzwyczajony do CVCS, gdzie większośc operacji ma przed sobą opóźnienia sieci, ten aspekt Git sprawi, że pomyślisz iż bogowie prędkości pobłgosławiły Git nieziemskimi siłami. Ponieważ historia całego projektu znajduje się na twoim dysku lokalnym, większośc operacji wydaje się natychmiastowa.

Dla przykładu, aby przewertować historię projektu, Git nie potrzebuje dostępu do serwera aby pobrać historię i wyświetlić ją dla Ciebie - lecz czyta ją wprost z lokalnej bazy danych. Znaczy to, że widzisz historię projektu prawie momentalnie. Jeśli chcesz zobaczyć zmiany między obecną wersją pliku, a wersją z przed miesiąca, Git może sprawdzić wersję z miesiąca, i obliczyć różnicę lokalnie, zamiast prosić o to serwer lub pobierać starą wersję pliku ze zdalenego serwera i obliczać różnicę lokalnie. 

Znaczy to również, że jest niewiele rzeczy, których nie możesz zrobić będąc offline lub poza siecią VPN. Jeżeli lecisz samolotem lub jedziesz pociągiem i chciałbyś popracować, możesz spokojnie commitować, aż będziesz miał dostęp do połączenia internetowego, aby wrzucić zmiany na serwer. Kiedy wracasz do domu i twój klient VPN nie działa poprawnie, nadal możesz pracować. W wielu innych systemach, nie ma takiej możliwości lub jest ona bardzo utrudniona. Dla przykładu w Perforce, nie możesz za wiele zrobić bez połączenia z serwerem; a w Subversion i CVS, możesz edytować pliki lecz nie możesz commitować zmian do swojej bazy danych ( ponieważ jest ona offline). Może nie brzmi to fascynująco, lecz możesz być zdziwiony jaką robi to wilką różnicę. 

### Git Jest Integralny ###

Wszystko w Git jest sprawdzane sumą kontrolną przed tym jak zostanie zapisane, a następnie ustalane jest odniesienie do tych danych pooprzez tą sumę kontrolną. Znaczy to, że nie jest możliwa zmiana pliku lub folderu bez wiedzy Git. Funkcjonalność ta jest wbudowana w Git w najniższym poziomie i jest ona integralna ze swoją filozofią. Nie możesz utracić informacji podczas tranzytu lub uzyskać zniszczony plik bez możliwości detekcji tego przez Git. 

Mechanizm, który Git wykorzystuje do tworzenia sum kontrolnych nazywa się SHA-1 hash. Jest to 40 znakowy string złożony z hexadecymalnych znaków (0-9 oraz a-f) i wyliczony na podstawie zawartości pliku lub struktory katalogu w Git. Hash SHA-1 wygląda tak:

	24b9da6552252987aa493b52f8696cd6d3b00373

Będziesz widział takie hashe wszędzie dokoła, ponieważ Git używa ich tak często. W rzeczywistości, Git zapisuje wszystko nie używając nazw plików, lecz adresuje sobie je w bazie danych poprzez hash ich zawartości.

### Git Generalnie Jedynie Dodaje Dane ###

Kiedy wkonujesz jakieś operacje w Git, niemalże każda z nich jedynie dodanie dane do bazy danych Git. Jest bardzo trudno wykonać coś w systemie co było by nieodwracalne lub zmusić system do usunięcia danych w jakikolwiek sposób. Jak w każdym VCS, możesz stracić lub pomieszać zmiany, których jeszcze nie commitowałeś; lecz jesli zcommitujesz zmiany do Git trudno jest coś utracić, w szczególności, kiedy regularnie wypychasz zmiany do innego repozytorium. 

Czyni to używanie Git przyjemnością, ponieważ wiemy, że możemy eksperymentować bez zagrożenia całkowitej utraty naszej pracy. Więcej dowiesz się na temat tego jak Git przechowuje dane oraz jak przywrocić dane, które uznałeś za utracone w Rozdziale 9 "Pod Kołudrą".

### Trzy Stany ###

Teraz uważaj. Jest to najważniejsza rzecz do zapamiętania o Git, jeśli chcesz aby reszta procesu nauki poszła gładko. Git posiada trzy główne stany, w których twoje pliki mogą rezydować: zcommitowany, zmodyfikowany oraz staged. Zcommittowany znaczy, że dane są bezpiecznie zapisane w twojej lokalnej bazie danych. Zmodyfikowany znaczy, iż dokonałeś zmian w pliku, lecz niezcommittowales ich jeszcze do swojej lokalnej bazy danych. Staged znaczy, że zaznaczyłeś zmodyfikowany plik w swojej obecnej wersji, aby został wysłany podczas najbliższego commita.

Prowadzi to nas do trzech głównych sekcji projektu w Git: katalog Git, katalog prac, oraz przestrzeń staged.

Insert 18333fig0106.png 
Rycina 1-6. Ktalog prac, przestrzeń staging, oraz katalog git.

Ktalog Git jest to katalog, w którym Git przetrzymuj metadane oraz obiekty bazy danych dla twojego projektu. Jest to najważniejsza część Git i to jest to co jest kopiowane kiedy klonujesz repozytorium z innego komputera.

Katalog pracy jest to pojedyńczy checkout jednej z wersji projektu. Pliki te są pobierane ze skompresowanej bazy danych z katalogu Git i umieszczone na dysku dla twojego użytku oraz modyfikacji.

Przestrzeń staging jest to prosty plik, generalnie znajdujący się w twoim katalogu Git, który przetrzymuje informacje o tym co wejdzie w skład następnego commita. Jest czasem porównywany do indexu, ale staję się to standardem nazywając go przestrzenią staging.

Podstawowy przebieg pracy wyglada mniej więcej tak:

1.	Modyfikujesz pliki w swoim katalogu pracy.
2.	Stage plików, dodając ich migawki do przestrzeni staging.
3.	Wykonujesz commit, który pobiera pliki takie jakie są w przestrzeni staging i zapisuje tą migawke na stałe do katalogu Git.

Jeżeli jakaś szczególna wersja pliku jest w kataolgu git, uznany jest on za zcommitowany. Jeżeli został zmodyfikowany, ale został dodany do przestrzeni staging, jest on więc staged. Jeżeli plik został zmieniony od czasu checkoutu, ale nie został staged, jest dalej nazywany zmodyfikowanym. W Rozdziale 2, nauczysz się o tych stanach i jak z nich w odpowiedni sposób skorzystać lub pominąć część staged całkowicie.

## Instalacja Git ##

Przejdźmy do korzystania Git. Pierwsze rzeczy najpierw-musisz go zainstalować. Możesz zdobyć go wieloma sposobami; te dwie główne to instalacja Git ze źródła lub z instniejącej paczki dla twojej platformy.

### Instalacja ze Źródła ###

Jeśli możesz, jest użytecznym zainstalowanie Git ze źródeł, ponieważ uzyskasz ostatnią stabilną wersję. Każda wersja zazwyczaj posiada nowe użyteczne akcesoria UI, awięc ostatnia wersja jest zazwyczaj najlepszą drogą jeśli nie masz nic przeciwko kompilowaniu oprogramowania ze źródeł. Inna sprawa że dystrybucje Linuxowe często posiadają bardzo stare wersje paczek; awięc jeśli nie masz najświeższej wersji dystrybucji lub jeśli nie używasz backportów, instalacja ze źródeł może być najlepsza.

Aby zainstalować Git, potrzebujesz następujące biblioteki, których Git używa: curl, zlib, openssl, expat oraz libiconv. Naprzykład, jeżeli używasz systemu, który posiada yum (jak naprzykład Fedora) lub apt-get (naprzykład system oparty o dystrybucje Debian), możesz użyć jednej z tych komend, aby zainstalować wszystkie zależności:

	$ yum install curl-devel expat-devel gettext-devel \
	  openssl-devel zlib-devel

	$ apt-get install libcurl4-gnutls-dev libexpat1-dev gettext \
	  libz-dev
	
Jeżeli masz wszystkie wymagane zależności, możesz śmiało wejść i ściągnąć ostatnią migawkę ze strony internetowej Git:

	http://git-scm.com/download
	
A nastepnie, skompilować i zainstalować:

	$ tar -zxf git-1.6.0.5.tar.gz
	$ cd git-1.6.0.5
	$ make prefix=/usr/local all
	$ sudo make prefix=/usr/local install

Jeśli to jest zrobione, możesz również pobrać Git poprzez Git po uaktualnienia:

	$ git clone git://git.kernel.org/pub/scm/git/git.git
	
### Instalacja na Linux ###

Jeżeli chcesz zainstalować Git na Linux poprzez binarny instalator, możesz generalnie zrobić to poprzez podstawowe narzędzie zarządzania pakietami dostępny wraz z twoją dystrybucją. Jeśli używasz Fedory, możesz użyć yum:

	$ yum install git-core

Jeżeli jesteś na dystrybucji opartej o system Debian jak Ubuntu, spróbuj apt-get:

	$ apt-get install git-core

### Instalacja na Mac ###

Są dwa proste sposoby aby zainstalować Git na Mac. Najprostsza to użycie graficznego instalera Git, który można pobrać ze strony Google Code (zobacz Rycinę 1-7):

	http://code.google.com/p/git-osx-installer

Insert 18333fig0107.png 
Rycina 1-7. Instalator Git dla OS X.

Innym głównym sposobem jest instalacja Git poprzez MacPorts (`http://www.macports.org`). Jeśli masz zainstalowaną aplikacje MacPorts, zainstaluj Git poprzez komendę:

	$ sudo port install git-core +svn +doc +bash_completion +gitweb

Nie musisz dodawać wszystkich dodatków, ale prawdopodobnie będziesz chciał skożystać z +svn jeśli kiedykolwiek będziesz miał doczynienia z repozytoriami Subversion (zobacz Rozdział 8).

### Instalacja na Windows ###

instalacja Git na Windows jest bardzo prosta. Projekt msysGit ma jeden z łatwiejszych procedur instalacji. Poprostu pobierz plik exe z instalacją ze strony Google Code, i uruchom:

	http://code.google.com/p/msysgit

Po instalacji, będziesz posiadał obie - wersję w lini komend ( wraz z klientem SSH, który przyda się póżniej) oraz standardową wersję GIU.

## Początkowe Ustawienie Git ##

Teraz jak już posiadasz Git na swoim systemie operacyjnym, będziesz chciał skonfigurować swoje środowisko Git. Będziesz musiał wykonać te czynności tylko raz; Będą się one utrzymywać między uaktualnieniami. Będziesz również mógł je zmienić w każdym momencie używając ponownie komendy.

Git zostaje zainstalowany wraz z narzędziem zwabyn git config, które pozwoli Ci pobrać oraz ustawić zmienne konfiguracyjne, które kontrolują wszystkie aspekty - jak Git działa oraz wygląda. Zmienne te mogą być przechowywane w trzech różnych miejscach:

*	`/etc/gitconfig` plik: Zawiera wartości dla każdego użytkownika systemu oraz wszsytkie ich repozytoria. Jeżeli przekażesz opcję `--system` to `git config`, to git czyta oraz zapisuje do tego pliku. 
*	`~/.gitconfig` plik: Przygotowany jedynie dla twojego użytkownika systemu. Możesz wymusić aby Git czytał oraz zapisywał do tego pliku kożystając z opcji `--global`.
*	plik konfiguracji w katalogu git (dokładniej, `.git/config`) w każdym repozytorium, które używasz: plik jest ten wyspecyfikowany właśnie dla tego repozytorium. Każdy poziom nadpisuje wartości z poprzedniego poziomum, awięc wartości z `.git/config` nadpisują te z `/etc/gitconfig`.

W systemie Widnows, Git poszukuje pliku `.gitconfig` w katalogu `$HOME` (`C:\Document and Settings\$USER` dla większości użytkowników). Git również wciąż poszukuje `/etc/gitconfig`, co wskazuje na główny katalog MSys, który znajduje się w miejscu, w którym zainstalowałeś Git.

### Twoja Tożsamość ###

Pierwszą rzeczą jaką powinieneś zrobić po instalacji Git, jest ustawienie nazwy twojego użytkownika oraz adresu e-mail. Jest to ważne, ponieważ każdy commit Git używa tych informacji i są one niezmiennie wprowadzane do twoich commitów:

	$ git config --global user.name "John Doe"
	$ git config --global user.email johndoe@example.com

Ponownie, musisz wykonać operację to tylko raz przekazują opcję `--global`, ponieważ Git będzie zawsze używał tej informacji do wszystkiego co będziesz wykonywał w systemie. Jeśli chcesz nadpisać to inną nazwą lub adres e-mail dla wybranego projektu, możesz użyć komendy bez opcji `--global` jeśli będziesz w tym projekcie.

### Twój Edytor ###

Teraz twoja tożsamość jest zapisana, możesz zkonfigurować domniemany edytor textowy, który będzie używany gdy Git potrzebuje abyś wprowadził wiadomość. Podstawowo, Git używa domniemanego edytora twojego systemu, którym generalnie jest Vi lub Vim. Jeśli chcesz jednak używać innego edytora, takiego jak Emacs, możesz zrobić to wpisując:

	$ git config --global core.editor emacs
	
### Twoje Narzędzie Diff ###

Inną przydatną opcją, którą chciałbyś skonfigurować jest domniemane narzędzie diff (porównanie) używane do rozwiązywania konfliktów merg-ów. Powiedzmy że chcesz używac vimdiff:

	$ git config --global merge.tool vimdiff

Git akceptuje kdiff3, tkdiff, meld, xxdiff, emerge, vimdiff, gvimdiff, ecmerge, and opendiff jako obowiozujące narzędzia do merge-ów. Możesz również ustawić zwykłe narzędzie; zobacz Rozdział 7 wcelu uzyskania większej ilości informacji na ten temat.

### Sprawdzanie Swoich Ustawień ###

Jeśli chcesz sprawdzić swoje ustawienia, możesz użyć komendy `git config--list` aby wylistować wszystkie ustawienia, które może Git odnaleźć z tej pozycji:

	$ git config --list
	user.name=Scott Chacon
	user.email=schacon@gmail.com
	color.status=auto
	color.branch=auto
	color.interactive=auto
	color.diff=auto
	...

Możesz zaobserwować niektóre klucze częściej niż raz, ponieważ Git czyta te same klucze z różnych plików (naprzykład `/etc/gitconfig` oraz `~/.gitconfig`). W takim przypadku Git używa ostatniej wartości dla każdego unikalnego klucza, który widzi.

Możesz również sprawdzić jaką wartość widzi Git pod wybranym kluczem wpisując `git config {key}`:

	$ git config user.name
	Scott Chacon

## Uzyskiwanie Pomocy ##

Jeśli kiedykowiek będziesz potrzebować pomocy w używaniu Git, są trzy różne sposoby aby uzyskąc stronę manuala (manpage) dla każdej z komend Git:

	$ git help <verb>
	$ git <verb> --help
	$ man git-<verb>

Na przykład, możesz otrzymać pomoc manpage dla komendy config wpisując:

	$ git help config

Te komendy są świetne ponieważ masz do nich dostęp z każdego miejsca, nawet jeśli jesteś offline.
Jeśli strony manpage oraz książka nie są wystarczające  i potrzebujesz osobistej pomocy, możesz spróbować kanałów IRC `#git` lub `#github` na serwerze Freenode (irc.freenode.net). Kanały te są regularnie wypełnione setkami ludzi, którzy są zorientowani na temat Git i często skorzy do pomocy.

## Podsumowanie ##

Powinieneś mieć podstawową wiedzę czym jest Git i czym się różni od CVCS, które mogłeś używać. Powinieneś również już mieć działającą wersję Git na swoim systemie, który jest ustawiony twoją własną tożsamością. Teraz przyszedł czas dowiedzieć się na temat podstaw Git.
