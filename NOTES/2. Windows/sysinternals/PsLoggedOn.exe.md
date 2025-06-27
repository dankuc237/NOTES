PsLoggedOn: Narzędzie do Wykrywania Aktywnych Sesji Użytkowników

PsLoggedOn to aplikacja z pakietu SysInternals Suite, która służy do wykrywania, którzy użytkownicy są aktualnie zalogowani na komputerze.

Działa poprzez przeszukiwanie kluczy rejestru w gałęzi HKEY_USERS w celu znalezienia identyfikatorów bezpieczeństwa (SID) zalogowanych użytkowników, a następnie konwertuje te SID na nazwy użytkowników. Dodatkowo, PsLoggedOn korzysta z API NetSessionEnum, aby sprawdzić, kto jest zalogowany do komputera za pośrednictwem udziałów sieciowych.

Ograniczenia PsLoggedOn

Głównym ograniczeniem PsLoggedOn jest to, że wymaga działania usługi Rejestr zdalny na komputerze, który jest skanowany.

Usługa ta nie jest domyślnie włączona na stacjach roboczych z systemem Windows od wersji Windows 8, ale administratorzy systemu mogą ją włączyć w różnych celach. Warto również zaznaczyć, że usługa Rejestr zdalny jest domyślnie włączona w nowszych wersjach systemów operacyjnych Windows Server.

Zastosowanie PsLoggedOn w Penetracji

PsLoggedOn może być cennym narzędziem dla testerów penetracyjnych w fazie zbierania informacji. Pozwala na identyfikację aktywnych sesji użytkowników, co może dostarczyć informacji o potencjalnych celach ataku.

W połączeniu z innymi technikami, takimi jak wyszukiwanie lokalnych administratorów, PsLoggedOn może pomóc w znalezieniu ścieżki do eskalacji uprawnień.

Przykład Zastosowania PsLoggedOn

W źródłach opisano przykład, w którym tester penetracyjny używa PsLoggedOn do wykrycia aktywnej sesji użytkownika "jeffadmin" na komputerze CLIENT74.

W tym przypadku tester penetracyjny ma uprawnienia administratora na CLIENT74, co oznacza, że mógłby potencjalnie zalogować się na ten komputer i wykraść dane uwierzytelniające jeffadmin.

Podsumowanie

PsLoggedOn to przydatne narzędzie do identyfikacji aktywnych sesji użytkowników, które może być wykorzystywane w różnych scenariuszach bezpieczeństwa, w tym w testach penetracyjnych. Należy jednak pamiętać o jego ograniczeniach i stosować je w sposób odpowiedzialny i etyczny.

# Warunki Działania PsLoggedOn

Aby PsLoggedOn mógł skutecznie wykryć aktywne sesje użytkowników na komputerze, muszą być spełnione dwa podstawowe warunki:

Dostępność usługi Rejestr zdalny: PsLoggedOn opiera swoje działanie na informacji zawartej w kluczach rejestru na komputerze docelowym. Aby uzyskać do nich dostęp zdalnie, wymagane jest, aby usługa "Rejestr zdalny" była uruchomiona na tym komputerze.
[Włączenie Rejestru zdalnego](https://docs.recastsoftware.com/help/enable-remote-registry)

W systemach Windows 8 i nowszych ta usługa nie jest domyślnie włączona.

W systemach Windows Server 2012 R2 i nowszych, usługa "Rejestr zdalny" jest domyślnie aktywna.

Administratorzy systemu mogą ręcznie włączyć tę usługę, niezależnie od wersji systemu operacyjnego.

Nawet jeśli usługa jest włączona, może ona automatycznie zatrzymać się po 10 minutach nieaktywności, aby oszczędzać zasoby. Połączenie z PsLoggedOn spowoduje jej ponowne uruchomienie.

Odpowiednie uprawnienia: Użytkownik uruchamiający PsLoggedOn musi posiadać odpowiednie uprawnienia, aby móc odczytać informacje z rejestru zdalnego komputera. W praktyce oznacza to zazwyczaj posiadanie uprawnień administratora na komputerze docelowym.

Dodatkowe Uwagi

NetSessionEnum: PsLoggedOn korzysta również z API NetSessionEnum do wykrywania sesji użytkowników nawiązanych poprzez udziały sieciowe.

Działanie tej funkcji jest jednak ograniczone przez uprawnienia dostępu do klucza rejestru SrvsvcSessionInfo.

Windows 10 i nowsze: W nowszych wersjach systemów Windows (od wersji 10 1709 i Windows Server 2019 1809) dostęp do klucza SrvsvcSessionInfo został ograniczony, co wpływa na skuteczność działania NetSessionEnum.

W takich przypadkach PsLoggedOn może nie być w stanie wykryć wszystkich aktywnych sesji.

Alternatywy: Jeśli PsLoggedOn nie działa z powodu braku dostępu do Rejestru zdalnego lub ograniczeń NetSessionEnum, istnieją inne narzędzia, które mogą być użyte do wykrywania aktywnych sesji. Przykładami takich narzędzi są:

Wbudowane narzędzia systemowe, takie jak query user lub net session

Narzędzia firm trzecich, takie jak Remote Desktop Protocol (RDP)

Pamiętaj, że używanie PsLoggedOn i innych narzędzi do skanowania sieci powinno być przeprowadzane w sposób odpowiedzialny i etyczny, z poszanowaniem prywatności użytkowników.