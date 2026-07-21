# ხშირი პრობლემები და მათი მოგვარება

ამ გვერდზე მოცემულია WSL-ის, MAFFT-ისა და IQ-TREE 2-ის გამოყენებისას ყველაზე ხშირად წარმოქმნილი პრობლემები.

პრობლემის მოგვარებისას ყოველთვის შეინახეთ:

- გამოყენებული სრული ბრძანება;
- შეცდომის სრული ტექსტი;
- პროგრამის ვერსია;
- შეყვანის ფაილის სახელი და მდებარეობა.

## 1. `command not found`

მაგალითად:

```text
mafft: command not found
```

ან:

```text
iqtree2: command not found
```

შეამოწმეთ, ჩანს თუ არა პროგრამა სისტემაში:

```bash
which mafft
which iqtree2
which iqtree
```

თუ პროგრამა დაყენებულია, უნდა გამოჩნდეს მისი მდებარეობა, მაგალითად:

```text
/usr/bin/mafft
/usr/bin/iqtree2
```

თუ არაფერი გამოჩნდა, განაახლეთ პაკეტების სია და დააყენეთ პროგრამები:

```bash
sudo apt update
sudo apt install -y mafft iqtree
```

შემდეგ ხელახლა შეამოწმეთ:

```bash
mafft --version
iqtree2 --version
```

ზოგიერთ ინსტალაციაში IQ-TREE-ის ბრძანება შეიძლება იყოს `iqtree`:

```bash
iqtree --version
```

## 2. `No such file or directory` ან ფაილი ვერ მოიძებნა

შეამოწმეთ, რომელ საქაღალდეში იმყოფებით:

```bash
pwd
```

იხილეთ მიმდინარე საქაღალდის ფაილები:

```bash
ls -lh
```

თუ ფაილი სხვა საქაღალდეშია, გადადით შესაბამის ადგილას:

```bash
cd /path/to/folder
```

ან ბრძანებაში მიუთითეთ სრული გზა:

```bash
mafft /path/to/input.fasta > /path/to/aligned.fasta
```

Linux-ში დიდი და პატარა ასოები განსხვავდება. ეს სახელები ერთნაირი არ არის:

```text
Input.fasta
input.fasta
```

## 3. გზაში გამოტოვებებია

თუ საქაღალდის ან ფაილის სახელში გამოტოვებაა, მთელი გზა მოათავსეთ ბრჭყალებში:

```bash
mafft "/mnt/c/Users/User/Desktop/My Project/input.fasta" > aligned.fasta
```

არასწორი მაგალითი:

```bash
mafft /mnt/c/Users/User/Desktop/My Project/input.fasta > aligned.fasta
```

ამ შემთხვევაში ტერმინალი `My`-სა და `Project/input.fasta`-ს სხვადასხვა არგუმენტებად აღიქვამს.

## 4. Windows-ის გზა WSL-ში არ მუშაობს

Windows-ის გზა:

```text
C:\Users\User\Desktop\input.fasta
```

WSL-ში იწერება ასე:

```text
/mnt/c/Users/User/Desktop/input.fasta
```

დისკების შესაბამისობა:

```text
C: -> /mnt/c/
D: -> /mnt/d/
E: -> /mnt/e/
```

მაგალითად:

```bash
cd /mnt/c/Users/User/Desktop
```

მიმდინარე WSL საქაღალდის Windows Explorer-ში გასახსნელად:

```bash
explorer.exe .
```

## 5. `Permission denied` სკრიპტის გაშვებისას

მაგალითად:

```text
bash: ./scripts/run_mafft.sh: Permission denied
```

მიეცით სკრიპტს გაშვების უფლება:

```bash
chmod +x scripts/run_mafft.sh
```

შემდეგ გაუშვით:

```bash
./scripts/run_mafft.sh input.fasta aligned.fasta
```

ალტერნატიულად შეგიძლიათ პირდაპირ `bash` გამოიყენოთ:

```bash
bash scripts/run_mafft.sh input.fasta aligned.fasta
```

## 6. MAFFT-ის შედეგის ფაილი ცარიელია

შეამოწმეთ ფაილის ზომა:

```bash
ls -lh aligned.fasta
```

ხაზების რაოდენობა:

```bash
wc -l aligned.fasta
```

პირველი ხაზები:

```bash
head aligned.fasta
```

თუ ფაილი ცარიელია, ბრძანება შესაძლოა შეცდომით დასრულდა. თავიდან გაუშვით და შეცდომის ტექსტს დააკვირდით:

```bash
mafft --auto input.fasta > aligned.fasta
```

შეამოწმეთ, რომ საწყისი FASTA ნამდვილად შეიცავს თანმიმდევრობებს:

```bash
grep -c '^>' input.fasta
```

თუ შედეგია `0`, ფაილში FASTA სათაურები არ მოიძებნა.

## 7. IQ-TREE წერს, რომ თანმიმდევრობების სიგრძეები განსხვავდება

IQ-TREE-ს სჭირდება multiple sequence alignment, სადაც ყველა თანმიმდევრობას ერთნაირი alignment სიგრძე აქვს.

ჯერ გაუშვით MAFFT:

```bash
mafft --auto input.fasta > aligned.fasta
```

შემდეგ IQ-TREE:

```bash
iqtree2 -s aligned.fasta -m MFP -B 1000 -T AUTO
```

დაუმუშავებელი, სხვადასხვა სიგრძის FASTA პირდაპირ IQ-TREE-ში არ შეიტანოთ.

## 8. დუბლირებული FASTA სათაურები

IQ-TREE-სა და სხვა პროგრამებში ყველა თანმიმდევრობას უნიკალური სახელი უნდა ჰქონდეს.

დუბლირებული სათაურების შესამოწმებლად:

```bash
grep '^>' input.fasta | sort | uniq -d
```

თუ ბრძანებამ სათაურები გამოიტანა, ისინი დუბლირებულია.

ყველა სათაური უნდა იყოს განსხვავებული, მაგალითად:

```text
>Sequence_1
>Sequence_2
>Sequence_3
```

სათაურების სრული სიის სანახავად:

```bash
grep '^>' input.fasta
```

## 9. FASTA ფორმატი არასწორია

სწორი FASTA ჩანაწერი იწყება `>` სიმბოლოთი და შემდეგ შეიცავს თანმიმდევრობას:

```text
>Sequence_1
ATGCGTACGT
>Sequence_2
ATGAGTACGT
```

გავრცელებული პრობლემებია:

- პირველი სათაურის წინ `>` სიმბოლო არ არის;
- ფაილში ცხრილის ან სხვა ტექსტის სვეტებია;
- თანმიმდევრობაში არასწორი სიმბოლოებია;
- ფაილი რეალურად Word ან Excel დოკუმენტია, მიუხედავად `.fasta` გაფართოებისა.

ფაილის ტიპის შესამოწმებლად:

```bash
file input.fasta
```

პირველი ხაზების სანახავად:

```bash
head -n 20 input.fasta
```

## 10. IQ-TREE ვერ ამოიცნობს თანმიმდევრობის ტიპს

IQ-TREE ჩვეულებრივ ავტომატურად არჩევს DNA ან amino-acid მონაცემებს.

თუ ამოცნობა არასწორია, DNA-სთვის მიუთითეთ:

```bash
iqtree2 -s aligned.fasta -st DNA -m MFP -B 1000 -T AUTO
```

ცილებისთვის:

```bash
iqtree2 -s aligned.fasta -st AA -m MFP -B 1000 -T AUTO
```

`-st` ხელით მხოლოდ მაშინ მიუთითეთ, როდესაც იცით მონაცემის სწორი ტიპი.

## 11. IQ-TREE-ის პროცესი შეწყდა

თუ გაშვება შეწყდა, მაგრამ `.ckp.gz` ფაილი დარჩა, გაუშვით ზუსტად იგივე ბრძანება:

```bash
iqtree2 -s aligned.fasta -m MFP -B 1000 -T AUTO --prefix results/my_tree
```

IQ-TREE ჩვეულებრივ გააგრძელებს ბოლო checkpoint-იდან.

არ შეცვალოთ ან წაშალოთ `.ckp.gz`, სანამ გაგრძელება გჭირდებათ.

## 12. IQ-TREE ამბობს, რომ ანალიზი უკვე დასრულებულია

თუ იგივე პრეფიქსით ანალიზი უკვე არსებობს, IQ-TREE შეიძლება უარს ამბობდეს ფაილების გადაწერაზე.

უსაფრთხო გზა არის ახალი პრეფიქსის გამოყენება:

```bash
iqtree2 -s aligned.fasta -m MFP -B 1000 -T AUTO --prefix results/my_tree_v2
```

იძულებით თავიდან გასაშვებად გამოიყენება `-redo`:

```bash
iqtree2 -s aligned.fasta -m MFP -B 1000 -T AUTO --prefix results/my_tree -redo
```

> **გაფრთხილება:** `-redo` ძველ შედეგებს გადაწერს.

## 13. `.contree` ფაილი არ შეიქმნა

`.contree` ჩვეულებრივ იქმნება bootstrap ანალიზის დროს.

შეამოწმეთ, გამოიყენეთ თუ არა:

```bash
-B 1000
```

მაგალითად:

```bash
iqtree2 -s aligned.fasta -m MFP -B 1000 -T AUTO --prefix my_tree
```

თუ `-B 1000` არ გამოგიყენებიათ, შეიძლება არსებობდეს `.treefile`, მაგრამ არა `.contree`.

## 14. როგორ შევამოწმოთ, წარმატებით დასრულდა თუ არა IQ-TREE

იხილეთ შედეგების ფაილები:

```bash
ls -lh
```

ლოგის ბოლო ხაზები:

```bash
tail -n 30 my_tree.log
```

ანგარიშის ბოლო ნაწილი:

```bash
tail -n 30 my_tree.iqtree
```

ძირითადი წარმატებული შედეგებია:

```text
my_tree.treefile
my_tree.iqtree
my_tree.log
```

თუ გამოყენებული იყო `-B 1000`, ჩვეულებრივ უნდა არსებობდეს:

```text
my_tree.contree
```

## 15. პროგრამა ძალიან ნელა მუშაობს

გამოიყენეთ CPU ნაკადების ავტომატური შერჩევა:

```bash
-T AUTO
```

ან მიუთითეთ კონკრეტული რაოდენობა:

```bash
-T 4
```

მაგალითად:

```bash
iqtree2 -s aligned.fasta -m MFP -B 1000 -T 4 --prefix my_tree
```

დიდ alignment-ს, ბევრ თანმიმდევრობას, ModelFinder-სა და bootstrap-ს შესაძლოა დიდი დრო დასჭირდეს.

დარწმუნდით, რომ WSL-ს საკმარისი მეხსიერება და თავისუფალი დისკის ადგილი აქვს:

```bash
free -h
df -h
```

## 16. Git-ში ძალიან დიდი ან დროებითი ფაილები ემატება

ჯერ შეამოწმეთ:

```bash
git status
```

Checkpoint და დიდი დროებითი ფაილები შეგიძლიათ `.gitignore`-ში დაამატოთ:

```gitignore
*.ckp.gz
*.mldist
*.bionj
*.log
```

ფრთხილად იყავით `.log` ფაილების გამორიცხვისას: საბოლოო რეპროდუცირებადი ანალიზისთვის მნიშვნელოვანი log ფაილის შენახვა შეიძლება სასარგებლო იყოს.

თუ ფაილი უკვე Git-ის კონტროლქვეშაა, მხოლოდ `.gitignore`-ში დამატება საკმარისი არ არის. მისი Git-ის ინდექსიდან ამოსაღებად, მაგრამ კომპიუტერში დასატოვებლად:

```bash
git rm --cached path/to/file
```

## 17. სასარგებლო დიაგნოსტიკური ინფორმაცია

პრობლემის შესახებ დახმარების მოთხოვნამდე გაუშვით:

```bash
uname -a
cat /etc/os-release
mafft --version
which mafft
which iqtree2
which iqtree
iqtree2 --version 2>/dev/null || iqtree --version
git --version
```

ფაილისა და საქაღალდის ინფორმაციისთვის:

```bash
pwd
ls -lh
file input.fasta
head -n 10 input.fasta
```

## 18. დახმარების მოთხოვნის სწორი ფორმა

დახმარების მოთხოვნისას მოგვაწოდეთ:

1. ზუსტად რა შედეგის მიღებას ცდილობდით;
2. გამოყენებული სრული ბრძანება;
3. შეცდომის სრული ტექსტი;
4. `pwd` და `ls -lh` ბრძანებების შედეგი;
5. პროგრამის ვერსია;
6. FASTA ფაილის პირველი რამდენიმე სათაური, კონფიდენციალური ინფორმაციის გარეშე.

მაგალითად:

```text
ვცდილობ aligned.fasta-დან IQ-TREE-ის ხის აგებას.

ბრძანება:
iqtree2 -s aligned.fasta -m MFP -B 1000 -T AUTO --prefix my_tree

შეცდომა:
[აქ ჩასვით სრული შეცდომა]
```

შეცდომის მხოლოდ ბოლო სიტყვის ან ეკრანის არასრული ფოტოს ნაცვლად, სრული ტექსტის მიწოდება პრობლემის მოგვარებას მნიშვნელოვნად აადვილებს.
