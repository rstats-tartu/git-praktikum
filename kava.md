# Git-põhine versioonikontroll algajatele — loeng-praktikumi kava

**Kestus:** 2 h (120 min). Varuks lisaplokid, millega venib kuni 4 tunnini.
**Sihtrühm:** bioinformaatika ja meditsiini tudengid või teadurid. Käsurida on tuttav ainult pealiskaudselt.
**Töövahend:** git terminalis + GitHub (HTTPS, autentimine `gh auth login` kaudu).

## Õpiväljundid

Pärast loengut osaleja:

1. selgitab, miks versioonikontroll on parem kui `analyys_final_v3_PARANDATUD.R`;
2. loob repo, teeb commit'e, vaatab ajalugu (`log`) ja muudatusi (`diff`, `status`);
3. saadab töö GitHubi (`push`) ja toob sealt muudatused tagasi (`pull`);
4. kloonib võõra repo (`clone`);
5. teab, kuidas eksimusest välja tulla (`restore`) ja kust abi otsida.

## Ajakava (120 min)

| Aeg | Kestus | Plokk | Vorm | Slaidid |
|---|---|---|---|---|
| 0:00 | 15 min | **Miks versioonikontroll?** Failikoopiate kaos, reprodutseeritavus, koostöö, ajas tagasi minek. Git vs GitHub. Mõisted: repo, commit, ajalugu. | loeng | 1–9 |
| 0:15 | 10 min | **Seadistuse kontroll.** `git --version`, `git config` nimi ja e-post, `gh auth status`. Kes ei jõudnud eeltööd teha, saab abi assistendilt või naabrilt. | praktikum | 10–11 |
| 0:25 | 30 min | **Praktikum 1: lokaalne git.** `init`, `status`, `add`, `commit`, `log`, `diff`. Väike analüüsiprojekt (README, R-skript, CSV). Muudame skripti, vaatame diff'i, commit'ime. `.gitignore` andmete ja tulemuste jaoks. | praktikum | 12–13 |
| 0:55 | 10 min | **Paus** | | |
| 1:05 | 15 min | **Kolm ala ja kaugrepo.** Töökaust → staging → repo. Miks staging olemas on. Kaugrepo: `origin`, `push`, `pull`, `clone`. GitHubi roll (varundus, jagamine, koostöö). | loeng | 15–21 |
| 1:20 | 30 min | **Praktikum 2: GitHub.** Repo loomine GitHubis, `remote add`, `push`. README muutmine GitHubi veebis ja `pull`. Õppejõu näidisrepo kloonimine. | praktikum | 22–23 |
| 1:50 | 10 min | **Kokkuvõte.** Hädaliigutused (`restore`, `log`, `diff`). Mis jäi rääkimata: harud, pull requestid. Kust edasi õppida. Küsimused. | loeng | 24–30 |

Praktikumide ajal kõnni ruumis ringi. Kõige rohkem aega kulub kahele asjale: autentimine GitHubiga ja `git commit`, mis avab ootamatult vim'i. Mõlemad on eeltöö juhendis ennetatud, aga arvesta, et kolmandik ei ole eeltööd teinud.

## Lisaplokid (kui aega on rohkem kui 2 h)

Järjekorras, mida lisada. Iga plokk on iseseisev.

| Kestus | Plokk | Sisu |
|---|---|---|
| 25 min | **Harud** | `git switch -c katse`, muudatus, commit, `git switch main`, `git merge katse`. Miks harud: katseta ilma töötavat versiooni lõhkumata. |
| 20 min | **Konflikt** | Kaks haru muudavad sama rida. `merge` annab konflikti, lahendame käsitsi. Osalejad kardavad seda kõige rohkem, seetõttu tasub näidata, et see on tavaline. |
| 25 min | **Pull request** | Osalejad on võõrad, kirjutusõigust ei anna, seega fork. Fork GitHubis, `remote set-url origin` oma forkile (klooni juba on 2.4-st), haru, fail `osalejad/<kasutajanimi>.md`, push, PR. Sammud on näidisrepo README-s. Õppejõud liidab ekraanil. Paarides: vaata teineteise PR-i. Kui aeg on napp: "Add file" otse GitHubi veebis õppejõu repos teeb forki ja PR-i automaatselt. |
| 15 min | **RStudio Git-paneel** | Sama asi nupudega. Näita, et paneel teeb samu käske. |
| 15 min | **Eksimused** | `git restore --staged`, `git commit --amend`, `git revert`. Mida *mitte* teha (`reset --hard` ilma mõtlemata). |
| 10 min | **Suured failid ja andmed** | Miks fastq ei lähe gitti. `.gitignore` mustrid, GitHubi 100 MB piir, viide Git LFS-ile ja andmehoidlatele. |

## Ettevalmistus õppejõule

- [ ] Saada `eeltoo.md` osalejatele **vähemalt nädal enne** ja tuleta 2 päeva enne meelde.
- [ ] Näidisrepo `tpall/git-praktikum-naidis` on lokaalselt kaustas `~/Projects/git-praktikum-naidis`, saada üles: `gh repo create tpall/git-praktikum-naidis --public --source=. --push`. Sisaldab README, `analyys.R`, `proovid.csv`, `.gitignore`, kausta `osalejad/` PR-harjutuseks ja 7 commit'i (üks kirjavea parandus `git show` demoks).
- [ ] Renderda slaidid: `quarto render slaidid.qmd`. Tulemus `slaidid.html` on ühefaililine, töötab ilma võrguta.
- [ ] Terminali font projektoril vähemalt 20 pt, hele taust. Puhas kaust demo jaoks (`rm -rf ~/demo && mkdir ~/demo`).
- [ ] Kontrolli, et ruumi Wi-Fi laseb GitHubi (port 443).
- [ ] Varuplaan, kui GitHub ei tööta: praktikum 1 läheb läbi täies mahus, praktikum 2 näitad ise ekraanil ja osalejad teevad kodus.
- [ ] Kui võimalik, üks assistent 15 osaleja kohta.

## Tüüpilised probleemid ja lahendused

| Sümptom | Põhjus | Lahendus |
|---|---|---|
| `git commit` avab tundmatu redaktori, kust ei saa välja | vim on vaikimisi | `Esc`, `:q!`, Enter. Seejärel `git config --global core.editor "nano"` või kasuta alati `-m "sõnum"` |
| `Author identity unknown` | nimi ja e-post seadistamata | `git config --global user.name` ja `user.email` |
| `warning: LF will be replaced by CRLF` | Windows reavahetused | Ohutu, ignoreeri. Või `git config --global core.autocrlf true` |
| `push` küsib parooli ja lükkab tagasi | GitHub ei võta parooli vastu, vaja tokenit | `gh auth login` või SSH-võti (eeltöö juhend) |
| `! [rejected] ... fetch first` | kaugrepos on commit, mida lokaalselt pole | `git pull`, siis `git push` |
| `fatal: not a git repository` | vales kaustas | `pwd`, liigu `cd`-ga õigesse kausta |
| `.DS_Store` ilmub repo'sse (macOS) | süsteemi fail | lisa `.DS_Store` faili `.gitignore` |
| `src refspec main does not match any` | ühtegi commit'i pole veel tehtud või haru nimi on `master` | tee esimene commit; `git branch -M main` |
| Fail on 100 MB üle ja push ebaõnnestub | GitHubi piir | eemalda fail commit'ist, lisa `.gitignore`, andmed lähevad mujale |

## Failid selles kaustas

- `kava.md` — see fail, õppejõule.
- `eeltoo.md` — osalejatele, saata enne loengut.
- `praktikum.md` — osalejatele, jagada loengu alguses (link või prinditult).
- `slaidid.qmd` — Quarto reveal.js slaidid. `quarto render slaidid.qmd` teeb `slaidid.html`.
