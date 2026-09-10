# Eeltöö enne git-praktikumi

Palun tee need sammud **enne loengut** läbi. Kokku kulub 20–30 minutit. Kui midagi jääb kinni, tule loengusse 10 minutit varem, aitame.

## Minimaalselt vajalik tarkvara

| Mis | Milleks | Windows | macOS | Linux |
|---|---|---|---|---|
| **Git** | versioonikontroll ise | [Git for Windows](https://git-scm.com/download/win), tuleb koos Git Bash terminaliga | tuleb Xcode Command Line Tools'iga, vt allpool | `sudo apt install git` |
| **Terminal** | kust git käske käivitada | **Git Bash** (paigaldub koos gitiga) | Terminal (juba olemas) | ükskõik milline |
| **GitHub CLI (`gh`)** | GitHubiga sisselogimine ilma paroolita | installer [cli.github.com](https://cli.github.com) | `brew install gh` või installer | vt cli.github.com |
| **Tekstiredaktor** | failide muutmine | Notepad++ või VS Code; RStudio sobib ka | VS Code, RStudio või TextEdit (plain text režiim) | ükskõik milline |
| **GitHubi konto** | kaugrepo | [github.com](https://github.com) | | |

Muud pole vaja. Kui sul on juba RStudio või VS Code, redaktorit juurde paigaldama ei pea.

## 1. Paigalda git

**Windows.** Laadi alla ja paigalda [Git for Windows](https://git-scm.com/download/win). Installeris vajuta läbi vaikeseadetega, välja arvatud kaks kohta:

- *Choosing the default editor used by Git* → vali **Notepad++** või **Visual Studio Code**, kui need on olemas, muidu **Nano**. Ära jäta vim'i.
- *Adjusting the name of the initial branch* → **Override the default branch name** ja jäta `main`.

Pärast paigaldust on Start-menüüs programm **Git Bash**. Kasuta praktikumis just seda, mitte "Command Prompt" ega PowerShell'i.

**macOS.** Ava Terminal (Spotlight: ⌘+tühik, kirjuta "Terminal") ja kirjuta:

```
git --version
```

Kui git puudub, pakub macOS ise paigaldamist (Xcode Command Line Tools). Vajuta "Install", ootab mõne minuti.

**Linux.** `sudo apt install git` (Ubuntu/Debian) või `sudo dnf install git` (Fedora).

Kontrolli igal süsteemil:

```
git --version
```

Peab näitama versiooni, nt `git version 2.4x`.

## 2. Terminali ABC (eriti Windowsi kasutajale)

Git töötab käsurealt. Vaja on viit käsku ja paari harjumust.

**Avamine.** Windows: Start → "Git Bash". macOS: Terminal. Avaneb aken, kus on viip (prompt), nt `Mari@LAPTOP MINGW64 ~` või `mari@macbook ~ %`. Sinna kirjutad käsu ja vajutad Enter.

**Kus ma olen?** Terminalis oled alati mingis kaustas. `~` tähendab kodukausta.

```
pwd
```

näitab täisteed. Windowsis Git Bashis on see kujul `/c/Users/Mari` (mitte `C:\Users\Mari`). Sama koht, teine kirjapilt.

**Mis siin on?**

```
ls
```

näitab kausta sisu. `ls -a` näitab ka peidetud faile (algavad punktiga, nt `.git`).

**Liigu kausta.**

```
cd Documents
cd ..          # samm tagasi ülemkausta
cd ~           # kodukausta
cd /c/Users/Mari/Documents/projekt   # täistee (Windows)
```

Kaustanimes tühik? Pane jutumärkidesse: `cd "OneDrive - Tartu Ülikool"`.

**Loo kaust ja vaata faili sisu.**

```
mkdir uus_kaust
cat fail.txt
```

**Harjumused, mis säästavad aega**

- **Tab** täiendab faili- ja kaustanime. Kirjuta `cd Doc` ja vajuta Tab.
- **Nool üles** toob eelmise käsu tagasi.
- **Ctrl+C** katkestab käsu, mis jäi rippuma.
- **Ctrl+L** või `clear` puhastab ekraani.
- Kleepimine Git Bashi: **parem hiireklõps** või **Shift+Insert**. Ctrl+V ei pruugi töötada. macOS: ⌘+V.
- Kui viip on muutunud `>`-ks ja midagi ei juhtu, on jutumärk lahti jäänud. Ctrl+C ja proovi uuesti.
- Kui ekraanile ilmub tekst ja lõpus on `(END)` või `:`, oled vaatajas (pager). Väljumiseks vajuta **q**.

**Redaktor terminalis.** Kui git avab `nano` (nt commit'i sõnumi jaoks): kirjuta tekst, salvesta **Ctrl+O** + Enter, välju **Ctrl+X**. Kui avanes hoopis vim (ekraani all `-- INSERT --` või tildega read): vajuta **Esc**, kirjuta `:q!` ja Enter. Seejärel tee samm 3, et see enam ei korduks.

Harjuta enne loengut: ava terminal, `cd Documents`, `mkdir git_proov`, `cd git_proov`, `pwd`. Kui see läks, oled valmis.

## 3. Ütle gitile, kes sa oled

Need lähevad iga sinu commit'i külge. Kasuta sama e-posti, millega lood GitHubi konto.

```
git config --global user.name "Eesnimi Perenimi"
git config --global user.email "sinu@email.ee"
git config --global init.defaultBranch main
git config --global core.editor "nano"
```

Viimane rida hoiab ära olukorra, kus git avab vim'i. Kui eelistad VS Code'i: `git config --global core.editor "code --wait"`.

Kontrolli:

```
git config --global --list
```

## 4. Loo GitHubi konto

Mine [github.com](https://github.com) ja loo konto, kui sul seda veel pole. Kasutajanimi jääb avalikuks ja tuleb hiljem CV-sse, vali mõistlik. Tudengina tasub hiljem taotleda [GitHub Education](https://education.github.com) paketti, aga praktikumi jaoks pole seda vaja.

## 5. Paigalda GitHub CLI ja logi sisse

See on lihtsaim viis, kuidas git saab GitHubiga suhelda ilma paroolideta.

- **Windows:** laadi installer aadressilt [cli.github.com](https://cli.github.com), paigalda vaikeseadetega. **Sulge Git Bash ja ava uuesti**, muidu `gh` käsku ei leita.
- **macOS:** `brew install gh`. Kui Homebrew puudub, laadi installer samalt lehelt.
- **Linux:** juhend samal lehel.

Seejärel Git Bashis või Terminalis:

```
gh auth login
```

Vasta küsimustele nii (nooltega liigud, Enteriga valid):

- `What account do you want to log into?` → **GitHub.com**
- `What is your preferred protocol?` → **HTTPS**
- `Authenticate Git with your GitHub credentials?` → **Yes**
- `How would you like to authenticate?` → **Login with a web browser**

Terminal näitab 8-kohalise koodi. Kopeeri see, vajuta Enter, brauseris kleebi kood ja kinnita.

Kontrolli:

```
gh auth status
```

Peab ütlema `Logged in to github.com account ...`.

### Kui `gh` ei õnnestu: SSH-võti

Alternatiiv, mis töötab ilma `gh`-ta.

```
ssh-keygen -t ed25519 -C "sinu@email.ee"
```

Vajuta kolm korda Enter (vaikimisi asukoht, ilma paroolita on praktikumiks OK). Seejärel kuva avalik võti:

```
cat ~/.ssh/id_ed25519.pub
```

Kopeeri kogu rida (algab `ssh-ed25519`). GitHubis: profiilipilt → **Settings** → **SSH and GPG keys** → **New SSH key**, kleebi ja salvesta. Kontrolli:

```
ssh -T git@github.com
```

Esimesel korral küsib `Are you sure you want to continue connecting?`, vasta `yes`. Peab vastama `Hi <kasutajanimi>! You've successfully authenticated`.

Sel juhul kasuta praktikumis repo aadressi kujul `git@github.com:KASUTAJA/repo.git`, mitte `https://...`.

## Kontrollnimekiri

- [ ] `git --version` töötab
- [ ] Oskan terminalis kausta liikuda (`cd`, `ls`, `pwd`)
- [ ] `git config --global --list` näitab nime ja e-posti
- [ ] GitHubi konto olemas
- [ ] `gh auth status` ütleb "Logged in" (või SSH-test õnnestub)
- [ ] Tean, millise programmiga faile muudan
