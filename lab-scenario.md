# Lab Scenario

Scenariusz tego laboratorium zakłada przyswojenie najważniejszych funkcjonalności GitHub.

## Cel

Celem jest przyswojenie wiedzy i zwiększenie umiejętności korzystania z GitHub, w szczególności:

- pracy z forkami i branchami,
- tworzenia i przeglądania Pull Requestów,
- promowania zmian pomiędzy środowiskami,
- tworzenia tagów i publikowania wydań GitHub Release.

## Założenia

Repozytorium jest publiczne i należy do **Użytkownika A**.

**Użytkownik B** wykonuje fork repozytorium i przygotowuje zmiany we własnej kopii.

Repozytorium główne posiada trzy branche:

| Branch | Przeznaczenie |
|---|---|
| `main` | wersja produkcyjna i stabilna |
| `uat` | wersja przeznaczona do testów akceptacyjnych |
| `dev` | branch integracyjny dla nowych zmian |

Przepływ zmian powinien wyglądać następująco:

```text
fork użytkownika B
        ↓
feature branch
        ↓ Pull Request
dev
        ↓ Pull Request
uat
        ↓ Pull Request
main
        ↓
tag i Release
````

W repozytorium głównym warto skonfigurować ochronę branchy `main`, `uat` oraz `dev`:

* wymagaj Pull Requesta przed merge,
* wymagaj co najmniej jednego zatwierdzenia,
* wymagaj zamknięcia wszystkich dyskusji,
* zablokuj bezpośrednie commity do chronionych branchy.

---

## Scenariusz 1 – Pull Request z forka do brancha `dev`

### Cel ćwiczenia

Przećwiczenie pracy z publicznym repozytorium bez bezpośredniego zapisu do repozytorium głównego.

### Zadanie

Użytkownik B ma dodać plik z opisem podstawowych zasad bezpieczeństwa systemu Linux.

### Przebieg

1. Użytkownik A tworzy w głównym repozytorium Issue:

   ```text
   Add Linux security recommendations
   ```

2. W Issue definiuje wymagania:

   * utworzyć plik `LINUX_SECURITY.md`,
   * opisać aktualizacje systemu,
   * opisać konfigurację firewalla,
   * opisać podstawowe zabezpieczenia SSH,
   * dodać link do dokumentu w `README.md`.

3. Użytkownik B otwiera publiczne repozytorium Użytkownika A.

4. Użytkownik B wykonuje **Fork** repozytorium do swojego konta.

5. Użytkownik B upewnia się, że jego fork zawiera branch `dev`.

6. Przed rozpoczęciem pracy synchronizuje fork z repozytorium źródłowym:

   ```text
   Sync fork → Update branch
   ```

7. W swoim forku Użytkownik B tworzy branch na podstawie `dev`:

   ```text
   feature/add-linux-security-guide
   ```

8. Na branchu roboczym tworzy plik:

   ```text
   LINUX_SECURITY.md
   ```

9. Zapisuje zmianę jako commit:

   ```text
   Add Linux security recommendations
   ```

10. Edytuje `README.md` i dodaje link:

```md
## Documentation

- [Linux Security Recommendations](LINUX_SECURITY.md)
```

11. Tworzy drugi commit:

```text
Link Linux security guide from README
```

12. Użytkownik B tworzy Pull Request o kierunku:

```text
UŻYTKOWNIK-B/repo:feature/add-linux-security-guide
                       ↓
UŻYTKOWNIK-A/repo:dev
```

13. W opisie Pull Requesta wpisuje:

```md
## Summary

Added Linux security recommendations covering:

- system updates,
- firewall configuration,
- SSH security.

The document was linked from README.md.

Closes #1
```

14. Użytkownik A wykonuje review w zakładce **Files changed**.

15. Użytkownik A dodaje komentarz do wybranej linii:

```text
Please add an example command for checking the SSH configuration.
```

16. Użytkownik A wybiera:

```text SSH configuration.
```

16. Użytkownik A wybiera:

```text
Request changes
```

17. Użytkownik B wraca do tego samego brancha w swoim forku i dodaje poprawkę:

```bash
sudo sshd -t
```

18. Użytkownik B zapisuje kolejny commit:

```text
Add SSH configuration validation command
```

19. Istniejący Pull Request aktualizuje się automatycznie.

20. Użytkownik B odpowiada na komentarz:

```text
Added the requested SSH configuration validation command.
```

21. Dyskusja zostaje oznaczona jako:

```text
Resolved
```

22. Użytkownik A ponownie wykonuje review i wybiera:

```text
Approve
```

23. Użytkownik A wykonuje:

```text
Squash and merge
```

24. Po merge Użytkownik B usuwa branch:

```text
feature/add-linux-security-guide
```

### Oczekiwany rezultat

* zmiany znajdują się na branchu `dev` głównego repozytorium,
* Pull Request ma status `Merged`,
* Issue zostało automatycznie zamknięte,
* `main` oraz `uat` nie zawierają jeszcze nowych zmian,
* branch roboczy w forku został usunięty.

---

## Scenariusz 2 – Promowanie zmian z `dev` do `uat` i `main`

### Cel ćwiczenia

Przećwiczenie kontrolowanego przepływu zmian pomiędzy branchami odpowiadającymi różnym środowiskom.

### Etap 1 – Pull Request z `dev` do `uat`

1. Użytkownik A przechodzi do głównego repozytorium.

2. Tworzy Pull Request:

   ```text
   dev → uat
   ```

3. Tytuł Pull Requesta:

   ```text
   Promote Linux security documentation to UAT
   ```

4. Opis:

   ```md
   ## Summary

   This Pull Request promotes the Linux security documentation
   from the development branch to the UAT branch.

   ## Validation

   - Markdown formatting verified
   - README link verified
   - SSH command reviewed
   ```

5. Drugi użytkownik wykonuje review zmian.

6. Reviewer może dodać komentarz lub zatwierdzić Pull Request.

7. Po zatwierdzeniu następuje merge do `uat`.

8. Na branchu `uat` wykonywane są testy akceptacyjne:

   * sprawdzenie zawartości pliku,
   * sprawdzenie formatowania Markdown,
   * sprawdzenie działania linku w `README.md`,
   * sprawdzenie poprawności przykładowych poleceń.

### Etap 2 – Pull Request z `uat` do `main`

1. Po zakończeniu testów UAT Użytkownik A tworzy kolejny Pull Request:

   ```text
   uat → main
   ```

2. Tytuł:

   ```text
   Release Linux security documentation to production
   ```

3. Opis:

   ```md
   ## Summary

   Promotes the tested Linux security documentation
   from UAT to the production branch.

   ## UAT result

   - Documentation reviewed
   - Links verified
   - Commands validated
   - No blocking issues found
   ```

4. Reviewer zatwierdza Pull Request.

5. Użytkownik A wykonuje merge do `main`.

### Oczekiwany rezultat

Po zakończeniu ćwiczenia wszystkie trzy branche powinny zawierać zaakceptowaną zmianę:

```text
dev
 ↓
uat
 ↓
main
```

Pull Requesty powinny mieć status:

```text
feature branch → dev     Merged
dev → uat                Merged
uat → main               Merged
```

Branch `main` reprezentuje teraz stabilną wersję gotową do wydania.

---

## Scenariusz 3 – Utworzenie taga i GitHub Release

### Cel ćwiczenia

Przećwiczenie oznaczenia konkretnego commita jako wersji projektu i opublikowania oficjalnego wydania.

### Etap 1 – Utworzenie wersji testowej

Po zmergowaniu zmian do `uat` można utworzyć wersję testową.

1. Użytkownik A przechodzi do:

   ```text
   Releases → Draft a new release
   ```

2. Tworzy nowy tag:

   ```text
   v1.0.0-rc.1
   ```

3. Jako target wybiera branch:

   ```text
   uat
   ```

4. Tytuł wydania:

   ```text
   Linux Security Documentation v1.0.0 RC1
   ```

5. Opis:

   ```md
   ## Release candidate

   This release candidate contains:

   - Linux security recommendations,
   - SSH configuration validation,
   - firewall recommendations,
   - README documentation link.

   This version is intended for UAT testing.
   ```

6. Zaznacza opcję:

   ```text
   Set as a pre-release
   ```

7. Publikuje wersję testową.

### Etap 2 – Utworzenie stabilnego wydania

Po zatwierdzeniu testów i zmergowaniu `uat` do `main`:

1. Użytkownik A ponownie przechodzi do sekcji **Releases**.

2. Wybiera:

   ```text
   Draft a new release
   ```

3. Tworzy nowy tag:

   ```text
   v1.0.0
   ```

4. Jako target wybiera:

   ```text
   main
   ```

5. Tytuł wydania:

   ```text
   Linux Security Documentation v1.0.0
   ```

6. Dodaje opis:

   ```md
   ## Version 1.0.0

   First stable release of the Linux security documentation.

   ### Added

   - Linux system update recommendations
   - Firewall verification guidance
   - SSH hardening recommendations
   - SSH configuration validation command
   - Documentation link in README
   ```

7. Opcjonalnie używa:

   ```text
   Generate release notes
   ```

8. Upewnia się, że wydanie nie jest oznaczone jako pre-release.

9. Publikuje Release.

### Etap 3 – Wydanie poprawki

1. Użytkownik B wykonuje kolejną niewielką poprawkę przez fork i Pull Request.

2. Zmiana przechodzi ponownie przez:

   ```text
   feature branch → dev → uat → main
   ```

3. Ponieważ jest to poprawka błędu lub dokumentacji, Użytkownik A tworzy nową wersję:

   ```text
   v1.0.1
   ```

4. Poprzedni tag `v1.0.0` nie jest usuwany ani przesuwany.

### Oczekiwany rezultat

W repozytorium powinny być dostępne:

| Typ            | Nazwa         | Target                         |
| -------------- | ------------- | ------------------------------ |
| Pre-release    | `v1.0.0-rc.1` | commit testowany na `uat`      |
| Stable release | `v1.0.0`      | zaakceptowany commit na `main` |
| Patch release  | `v1.0.1`      | późniejsza poprawka na `main`  |

Schemat końcowy:

```text
dev
 ↓
uat ── tag v1.0.0-rc.1 ── Pre-release
 ↓
main ── tag v1.0.0 ────── Stable Release
 ↓
main ── tag v1.0.1 ────── Patch Release
```

## Podsumowanie

Najważniejszy przepływ pracy:

```text
Issue
  ↓
Fork
  ↓
Feature branch
  ↓
Edycja i commity
  ↓
Pull Request do dev
  ↓
Review i poprawki
  ↓
Merge do dev
  ↓
Pull Request dev → uat
  ↓
Testy UAT
  ↓
Pull Request uat → main
  ↓
Tag
  ↓
GitHub Release
```

Najważniejsze pojęcia:

| Element      | Znaczenie                                   |
| ------------ | ------------------------------------------- |
| Fork         | własna kopia repozytorium na GitHub         |
| Branch       | oddzielna linia pracy nad zmianą            |
| Commit       | zapis konkretnego zestawu zmian             |
| Pull Request | propozycja połączenia zmian                 |
| Review       | sprawdzenie, dyskusja i zatwierdzenie zmian |
| Merge        | dołączenie zmian do brancha docelowego      |
| Tag          | stała etykieta wskazująca konkretny commit  |
| Release      | opublikowana wersja projektu oparta na tagu |

```

