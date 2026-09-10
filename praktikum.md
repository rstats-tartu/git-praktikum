# Git-praktikum: juhend osalejale

Kõik käsud kirjutad terminali (Windows: **Git Bash**). Iga sammu lõpus on **kontroll**, et teaksid, kas läks õigesti. Kui jääd kinni, tõsta käsi.

Märgid: `$` rea alguses tähendab "kirjuta see käsk". Sümbolit `$` ennast ei kirjuta.

---

## 0. Kas kõik on paigas? (5 min)

```
$ git --version
$ git config --global user.name
$ git config --global user.email
$ gh auth status
```

**Kontroll:** kõik neli annavad mõistliku vastuse. Kui nimi või e-post on tühi:

```
$ git config --global user.name "Eesnimi Perenimi"
$ git config --global user.email "sinu@email.ee"
```

---

## Osa 1: lokaalne git (30 min)

### 1.1 Loo projekt

Mine kausta, kus hoiad projekte (nt `Documents`), ja loo uus kaust.

```
$ cd ~/Documents
$ mkdir geeniekspressioon
$ cd geeniekspressioon
$ pwd
```

Tee sellest kaustast git-repo:

```
$ git init
```

**Kontroll:** `ls -a` näitab peidetud kausta `.git`. Seal elab kogu ajalugu. Ära seda käsitsi puutu.

```
$ git status
```

Ütleb `On branch main` ja `No commits yet`. See käsk on sinu parim sõber. Kasuta seda iga sammu vahel.

### 1.2 Esimene fail ja esimene commit

Loo README. Kas oma redaktoris või otse terminalis:

```
$ cat > README.md <<'X'
# Geeniekspressiooni analüüs

Katseprojekt git-praktikumi jaoks.
X
```

Vaata, mida git näeb:

```
$ git status
```

`Untracked files: README.md`. Git näeb faili, aga ei jälgi seda veel.

Kaks sammu: pane fail **staging'usse** (`add`) ja siis **salvesta hetktõmmis** (`commit`).

```
$ git add README.md
$ git status
$ git commit -m "Lisa README"
```

**Kontroll:**

```
$ git log
```

Näitab ühte commit'i: räsi (pikk kuueteistkümnendarv), autor, kuupäev, sõnum. Kui ekraan jääb `(END)` peale kinni, vajuta **q**.

### 1.3 Andmed ja skript

Loo väike andmefail:

```
$ cat > proovid.csv <<'X'
proov,grupp,ekspressioon
S1,kontroll,5.1
S2,kontroll,4.8
S3,kontroll,5.4
S4,haige,7.9
S5,haige,8.4
S6,haige,7.2
X
```

Ja skript (R; kui eelistad Pythonit, kirjuta sama asi Pythonis, git ei hooli):

```
$ cat > analyys.R <<'X'
# Loeb proovide tabeli ja arvutab ekspressiooni keskmise gruppide kaupa
proovid <- read.csv("proovid.csv")
keskmised <- aggregate(ekspressioon ~ grupp, data = proovid, FUN = mean)
print(keskmised)
X
```

Lisa mõlemad korraga ja commit'i:

```
$ git status
$ git add proovid.csv analyys.R
$ git commit -m "Lisa proovide andmed ja keskmiste skript"
$ git log --oneline
```

**Kontroll:** `git log --oneline` näitab kahte rida.

### 1.4 Muuda ja vaata erinevust

Ava `analyys.R` redaktoris ja lisa lõppu kaks rida:

```r
mediaanid <- aggregate(ekspressioon ~ grupp, data = proovid, FUN = median)
print(mediaanid)
```

Salvesta. Nüüd:

```
$ git status
$ git diff
```

`git diff` näitab täpselt, millised read muutusid: `+` on lisatud, `-` eemaldatud. See on see, mida sa failikoopiate puhul kunagi ei näe.

```
$ git add analyys.R
$ git commit -m "Lisa mediaanide arvutus"
```

**Kontroll:** `git log --oneline` näitab kolme rida.

### 1.5 Mida gitti EI panda: `.gitignore`

Analüüs tekitab tulemusi, mida ei ole mõtet versioonida (need tulevad skriptist uuesti). Suured toorandmed (fastq, bam) ei mahu ka.

Teeskle, et skript tekitas tulemuste kausta:

```
$ mkdir tulemused
$ echo "grupp,keskmine" > tulemused/keskmised.csv
$ git status
```

Git pakub `tulemused/` lisamiseks. Ütle talle, et ära paku:

```
$ cat > .gitignore <<'X'
# analüüsi väljund, tuleb skriptist uuesti
tulemused/

# suured toorandmed
*.fastq
*.fastq.gz
*.bam

# süsteemi ja redaktori prügi
.DS_Store
.Rhistory
.RData
X
$ git status
```

**Kontroll:** `tulemused/` on kadunud nimekirjast, `.gitignore` on untracked. Commit'i see:

```
$ git add .gitignore
$ git commit -m "Lisa gitignore"
```

### 1.6 Oih, tegin vea

Riku skript ära: kustuta redaktoris `analyys.R` failist pool sisu ja salvesta. Siis:

```
$ git diff
$ git restore analyys.R
```

**Kontroll:** fail on jälle terve. `git status` ütleb `nothing to commit, working tree clean`.

See on esimene põhjus, miks git olemas on: viimane commit'itud versioon on alati tagasi saadaval.

### Vahekokkuvõte

Neli käsku, mida kasutad iga päev:

```
git status        # kus ma olen?
git add <fail>    # märgi järgmisse commit'i
git commit -m ""  # salvesta hetktõmmis
git log --oneline # ajalugu
```

---

## Osa 2: GitHub (30 min)

### 2.1 Loo repo GitHubis

Brauseris: [github.com/new](https://github.com/new).

- **Repository name:** `geeniekspressioon`
- **Public** või **Private**, vahet pole
- **ÄRA** märgi "Add a README", "Add .gitignore" ega "Choose a license". Repo peab olema tühi, sest meil on sisu juba olemas.
- **Create repository**

GitHub näitab lehte juhistega. Meid huvitab plokk *"…or push an existing repository from the command line"*.

### 2.2 Ühenda ja saada

Terminalis, oma `geeniekspressioon` kaustas. Asenda `KASUTAJA` oma GitHubi kasutajanimega:

```
$ git remote add origin https://github.com/KASUTAJA/geeniekspressioon.git
$ git remote -v
$ git push -u origin main
```

(SSH-kasutaja: `git@github.com:KASUTAJA/geeniekspressioon.git`.)

Esimesel korral võib brauser või terminal küsida kinnitust. `-u` ütleb gitile, et edaspidi piisab lihtsalt `git push`.

**Kontroll:** värskenda GitHubi lehte. Failid on seal, README kuvatakse ilusti. Klõpsa "commits" ja näed sama ajalugu, mis `git log`.

### 2.3 Muuda GitHubis, too alla

Nüüd vastupidi. GitHubi lehel klõpsa `README.md` → pliiatsi ikoon (Edit). Lisa rida:

```
Andmed: proovid.csv, skript: analyys.R
```

**Commit changes** → sõnum → **Commit changes**.

Sinu arvutis seda muudatust veel pole:

```
$ cat README.md
$ git pull
$ cat README.md
$ git log --oneline
```

**Kontroll:** rida on kohal ja logis on uus commit, mille tegid veebis.

See on täpselt see, mis juhtub, kui kolleeg (või sina teisest arvutist) midagi muudab.

### 2.4 Klooni võõras repo

Kloonimine toob kogu repo koos ajalooga sinu arvutisse. Mine kaustast välja, et mitte teha repo repo sisse:

```
$ cd ~/Documents
$ git clone https://github.com/tpall/git-praktikum-naidis.git
$ cd git-praktikum-naidis
$ ls
$ git log --oneline
```

**Kontroll:** failid ja ajalugu on olemas. Sa saad neid muuta ja lokaalselt commit'ida, aga `push` ebaõnnestub, sest see pole sinu repo. Selleks on pull request, millest räägime lõpus.

### 2.5 Kui aega jääb: paaristöö

Vaheta naabriga GitHubi kasutajanimi. Klooni tema `geeniekspressioon`. Vaata, kas tema `git log` näeb välja samasugune kui sinu oma.

---

## Kui midagi läks valesti

| Olukord | Tee nii |
|---|---|
| Ei tea, mis toimub | `git status` |
| Ekraan jäi `(END)` peale kinni | vajuta `q` |
| Avanes veider redaktor, kust ei saa välja | `Esc`, `:q!`, Enter. Hiljem `git config --global core.editor "nano"` |
| Muutsin faili ära ja kahetsen | `git restore <fail>` (kaob viimase commit'ini) |
| Panin vale faili `add`-iga staging'usse | `git restore --staged <fail>` |
| Commit'i sõnumis oli kirjaviga | `git commit --amend -m "Uus sõnum"` (ainult enne push'i) |
| `push` lükati tagasi: `fetch first` | `git pull`, siis `git push` |
| `fatal: not a git repository` | oled vales kaustas, `pwd` ja `cd` |
| Tahan näha, mis eelmises commit'is muutus | `git show` |
| Tahan näha kahe commit'i vahet | `git diff abc123 def456` |

## Spikker

```
git init                         # uus repo praeguses kaustas
git clone <url>                  # kopeeri olemasolev repo
git status                       # mis on muutunud
git add <fail>                   # pane fail järgmisse commit'i
git add .                        # kõik muudetud failid (vaata enne status'est, mida lisad!)
git commit -m "sõnum"            # salvesta
git log --oneline                # ajalugu lühidalt
git diff                         # muudatused, mida pole veel add'itud
git diff --staged                # muudatused, mis on add'itud, aga mitte commit'itud
git restore <fail>               # tühista muudatused failis
git remote add origin <url>      # seo GitHubi repoga (üks kord)
git push -u origin main          # esimene saatmine
git push                         # edaspidi
git pull                         # too kaugrepo muudatused
```

Hea commit'i sõnum vastab küsimusele "mida see commit teeb?": *Lisa mediaanide arvutus*, *Paranda gruppide nimed*, *Eemalda vana joonis*. Mitte *muudatused*, *asdf*, *v2*.
