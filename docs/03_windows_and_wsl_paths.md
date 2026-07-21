# Windows და WSL ფაილური გზები

Windows და Linux ერთსა და იმავე დისკებს განსხვავებული ფორმით აჩვენებენ.

## Windows C: დისკი WSL-ში

Windows-ის `C:` დისკი WSL-ში მდებარეობს:

```text
/mnt/c/
```

მაგალითად, Windows-ის გზა:

```text
C:\Users\User\Desktop\data.fasta
```

WSL-ში იქნება:

```text
/mnt/c/Users/User/Desktop/data.fasta
```

საქაღალდეში გადასვლა:

```bash
cd /mnt/c/Users/User/Desktop
```

## Windows-ის სხვა დისკები

```text
D:\  →  /mnt/d/
E:\  →  /mnt/e/
```

მაგალითად:

```bash
cd /mnt/d/Bioinformatics
```

## WSL home საქაღალდე

Linux home საქაღალდე ჩვეულებრივ არის:

```text
/home/USERNAME/
```

მისი მოკლე აღნიშვნაა:

```text
~
```

მაგალითად:

```bash
cd ~
mkdir -p ~/projects
```

## რეკომენდებული სამუშაო ადგილი

Linux პროგრამებთან მუშაობისას პროექტები შეინახეთ WSL-ის ფაილურ სისტემაში:

```bash
mkdir -p ~/projects
cd ~/projects
```

მაგალითი:

```text
~/projects/phage-analysis/
├── input/
├── output/
└── scripts/
```

ეს ხშირად უფრო სწრაფი და სტაბილურია, ვიდრე დიდი ანალიზის `/mnt/c/`-დან გაშვება.

## WSL საქაღალდის გახსნა Windows Explorer-ში

WSL-ის მიმდინარე საქაღალდეში გაუშვით:

```bash
explorer.exe .
```

`.` ნიშნავს მიმდინარე საქაღალდეს.

## WSL ფაილებთან წვდომა Explorer-იდან

Windows Explorer-ის მისამართის ველში ჩაწერეთ:

```text
\\wsl$\
```

შემდეგ აირჩიეთ Ubuntu და გადადით Linux home საქაღალდეში.

## ფაილის კოპირება Windows-იდან WSL-ში

```bash
cp /mnt/c/Users/User/Desktop/sequences.fasta ~/projects/my_project/input/
```

## ფაილის კოპირება WSL-იდან Windows Desktop-ზე

```bash
cp ~/projects/my_project/output/tree.treefile /mnt/c/Users/User/Desktop/
```

## სივრცეები ფაილის გზაში

ბრჭყალების გამოყენება:

```bash
cd "/mnt/c/Users/User/Desktop/DATOS FILE"
```

ან escape:

```bash
cd /mnt/c/Users/User/Desktop/DATOS\ FILE
```

პრაქტიკაში უმჯობესია საქაღალდეებისა და ფაილების სახელებში სივრცეების ნაცვლად გამოიყენოთ `_` ან `-`.
