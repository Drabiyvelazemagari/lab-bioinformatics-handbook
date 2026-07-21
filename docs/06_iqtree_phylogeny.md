# IQ-TREE 2-ის გაშვება და ფილოგენეტიკური ხის აგება

IQ-TREE 2 გამოიყენება გასწორებული ნუკლეოტიდური ან ამინომჟავური თანმიმდევრობებიდან მაქსიმალური დამაჯერებლობის (Maximum Likelihood) ფილოგენეტიკური ხის ასაგებად.

> **მნიშვნელოვანია:** IQ-TREE-ში შესატანი ფაილი უნდა იყოს უკვე გასწორებული. დაუმუშავებელი FASTA ჯერ MAFFT-ით გაასწორეთ.

სრული სამუშაო პროცესი ასეთია:

```text
დაუმუშავებელი FASTA -> MAFFT -> გასწორებული FASTA -> IQ-TREE 2 -> ფილოგენეტიკური ხე
```

## 1. შეამოწმეთ, რომ IQ-TREE 2 დაყენებულია

WSL/Ubuntu-ში გაუშვით:

```bash
iqtree2 --version
```

ასევე შეგიძლიათ ნახოთ პროგრამის მდებარეობა:

```bash
which iqtree2
```

ამ სახელმძღვანელოს შექმნის დროს გამოყენებული იყო:

```text
/usr/bin/iqtree2
IQ-TREE multicore version 2.0.7
```

თუ `iqtree2` ვერ მოიძებნა, სცადეთ:

```bash
which iqtree
iqtree --version
```

ზოგიერთ ინსტალაციაში პროგრამის ბრძანება შეიძლება იყოს `iqtree`.

## 2. მოამზადეთ გასწორებული FASTA ფაილი

მაგალითად, თუ დაუმუშავებელი ფაილია:

```text
sequences.fasta
```

ჯერ გაასწორეთ MAFFT-ით:

```bash
mafft --auto --thread -1 sequences.fasta > sequences_aligned.fasta
```

IQ-TREE-ში შესატანი ფაილი იქნება:

```text
sequences_aligned.fasta
```

შეამოწმეთ, რომ ფაილი არსებობს:

```bash
ls -lh sequences_aligned.fasta
```

FASTA-ს სათაურების სანახავად:

```bash
grep '^>' sequences_aligned.fasta | head
```

ყველა FASTA სათაური უნიკალური უნდა იყოს.

## 3. გადადით ფაილის საქაღალდეში

მაგალითად:

```bash
cd ~/projects/my_phylogeny
```

შეამოწმეთ მიმდინარე მდებარეობა და ფაილები:

```bash
pwd
ls -lh
```

თუ ფაილი Windows-ის Desktop-ზეა, გზა შეიძლება გამოიყურებოდეს ასე:

```bash
cd "/mnt/c/Users/User/Desktop/My Project"
```

ბრჭყალები აუცილებელია, როდესაც გზაში გამოტოვებებია.

## 4. რეკომენდებული IQ-TREE 2 ბრძანება

შექმენით შედეგების საქაღალდე:

```bash
mkdir -p results/iqtree
```

შემდეგ გაუშვით:

```bash
iqtree2 \
    -s sequences_aligned.fasta \
    -m MFP \
    -B 1000 \
    -T AUTO \
    --prefix results/iqtree/sequences_tree
```

იგივე ბრძანება ერთ ხაზზე:

```bash
iqtree2 -s sequences_aligned.fasta -m MFP -B 1000 -T AUTO --prefix results/iqtree/sequences_tree
```

## 5. რას ნიშნავს თითოეული პარამეტრი

### `-s sequences_aligned.fasta`

უთითებს IQ-TREE-ს შესატან გასწორებულ ფაილს.

```bash
-s sequences_aligned.fasta
```

### `-m MFP`

რთავს ModelFinder Plus-ს, რომელიც ამოწმებს ჩანაცვლების სხვადასხვა მოდელს და ირჩევს მონაცემებისთვის საუკეთესო მოდელს.

```bash
-m MFP
```

### `-B 1000`

ასრულებს 1000 ultrafast bootstrap რეპლიკაციას ტოტების მხარდაჭერის შესაფასებლად.

```bash
-B 1000
```

### `-T AUTO`

IQ-TREE ავტომატურად არჩევს გამოსაყენებელი CPU ნაკადების რაოდენობას.

```bash
-T AUTO
```

კონკრეტული რაოდენობის გამოსაყენებლად, მაგალითად 4 ნაკადი:

```bash
-T 4
```

### `--prefix results/iqtree/sequences_tree`

ყველა გამოსატან ფაილს აძლევს ერთსა და იმავე პრეფიქსს და ათავსებს მითითებულ საქაღალდეში.

მაგალითად, შეიქმნება:

```text
results/iqtree/sequences_tree.treefile
results/iqtree/sequences_tree.contree
results/iqtree/sequences_tree.iqtree
results/iqtree/sequences_tree.log
results/iqtree/sequences_tree.ckp.gz
```

## 6. სწრაფი სატესტო გაშვება

თუ ჯერ მხოლოდ იმის შემოწმება გსურთ, რომ ფაილი და პროგრამა მუშაობს:

```bash
iqtree2 -s sequences_aligned.fasta -m MFP -T AUTO --prefix test_tree
```

ეს ბრძანება მოდელს და ხეს შეაფასებს, მაგრამ `-B 1000`-ის გარეშე bootstrap მხარდაჭერას არ გამოთვლის.

საბოლოო ანალიზისთვის გამოიყენეთ:

```bash
iqtree2 -s sequences_aligned.fasta -m MFP -B 1000 -T AUTO --prefix final_tree
```

## 7. ძირითადი გამომავალი ფაილები

### `.treefile`

მაქსიმალური დამაჯერებლობის მთავარი ხე Newick ფორმატში.

```text
sequences_tree.treefile
```

### `.contree`

კონსენსუსური ხე ტოტების მხარდაჭერის მნიშვნელობებით. ეს ფაილი განსაკუთრებით გამოსადეგია, როდესაც გამოყენებულია `-B 1000`.

```text
sequences_tree.contree
```

### `.iqtree`

ანალიზის მთავარი ტექსტური ანგარიში. შეიცავს არჩეულ მოდელს, likelihood-ის მნიშვნელობებს, მონაცემების შეჯამებასა და სხვა შედეგებს.

```text
sequences_tree.iqtree
```

მის სანახავად:

```bash
less results/iqtree/sequences_tree.iqtree
```

`less`-დან გამოსასვლელად დააჭირეთ `q`-ს.

### `.log`

გაშვების სრული ჟურნალი, მათ შორის ბრძანება, გაფრთხილებები და შესრულების ინფორმაცია.

```text
sequences_tree.log
```

ბოლო ხაზების სანახავად:

```bash
tail -n 30 results/iqtree/sequences_tree.log
```

### `.ckp.gz`

Checkpoint ფაილი, რომელიც შეწყვეტილი ანალიზის გაგრძელების საშუალებას იძლევა. ნუ შეცვლით ამ ფაილს ხელით.

## 8. როგორ მივხვდეთ, დასრულდა თუ არა ანალიზი

ტერმინალის ბოლოს მოძებნეთ წარმატებული დასრულების შეტყობინება. შემდეგ შეამოწმეთ შედეგები:

```bash
ls -lh results/iqtree/
```

ასევე შეამოწმეთ ანგარიშის ბოლო ნაწილი:

```bash
tail -n 30 results/iqtree/sequences_tree.iqtree
```

თუ `.treefile`, `.iqtree` და `.log` შეიქმნა და შეცდომა არ ჩანს, ძირითადი ანალიზი დასრულებულია.

თუ გამოყენებული იყო `-B 1000`, ჩვეულებრივ უნდა შეიქმნას `.contree` ფაილიც.

## 9. შეწყვეტილი ანალიზის გაგრძელება

თუ კომპიუტერი გაითიშა ან პროცესი შეწყდა, გაუშვით ზუსტად იგივე ბრძანება:

```bash
iqtree2 -s sequences_aligned.fasta -m MFP -B 1000 -T AUTO --prefix results/iqtree/sequences_tree
```

თუ შესაბამისი `.ckp.gz` ფაილი არსებობს, IQ-TREE ჩვეულებრივ გააგრძელებს ბოლო checkpoint-იდან.

არ წაშალოთ `.ckp.gz`, სანამ ანალიზი წარმატებით არ დასრულდება.

## 10. უკვე დასრულებული ანალიზის თავიდან გაშვება

თუ იგივე პრეფიქსით ანალიზი უკვე წარმატებით დასრულდა, IQ-TREE შეიძლება უარს ამბობდეს შედეგების გადაწერაზე.

უსაფრთხო ვარიანტია ახალი პრეფიქსის გამოყენება:

```bash
iqtree2 -s sequences_aligned.fasta -m MFP -B 1000 -T AUTO --prefix results/iqtree/sequences_tree_v2
```

ანალიზის იძულებით თავიდან გასაშვებად არსებობს `-redo`:

```bash
iqtree2 -s sequences_aligned.fasta -m MFP -B 1000 -T AUTO --prefix results/iqtree/sequences_tree -redo
```

> **გაფრთხილება:** `-redo` არსებულ შედეგებს გადაწერს. გამოიყენეთ მხოლოდ მაშინ, როდესაც დარწმუნებული ხართ, რომ ძველი ფაილები აღარ გჭირდებათ.

## 11. DNA და ცილოვანი თანმიმდევრობები

IQ-TREE ჩვეულებრივ ავტომატურად ამოიცნობს თანმიმდევრობის ტიპს.

თუ ტიპი არასწორად ამოიცნო, DNA-სთვის შეგიძლიათ მიუთითოთ:

```bash
iqtree2 -s dna_aligned.fasta -st DNA -m MFP -B 1000 -T AUTO --prefix dna_tree
```

ცილებისთვის:

```bash
iqtree2 -s proteins_aligned.fasta -st AA -m MFP -B 1000 -T AUTO --prefix protein_tree
```

`-st` ხელით მხოლოდ მაშინ მიუთითეთ, როდესაც ეს ნამდვილად საჭიროა.

## 12. Outgroup-ის მითითება

თუ არსებობს ბიოლოგიურად გამართლებული outgroup და მისი FASTA სათაურია `Outgroup_sequence`, გამოიყენეთ:

```bash
iqtree2 -s sequences_aligned.fasta -m MFP -B 1000 -T AUTO -o Outgroup_sequence --prefix rooted_tree
```

სახელი ზუსტად უნდა ემთხვეოდეს FASTA სათაურს `>` სიმბოლოს შემდეგ.

არ აირჩიოთ outgroup მხოლოდ იმისთვის, რომ ხე ვიზუალურად უკეთ გამოიყურებოდეს. არჩევანი ბიოლოგიურად დასაბუთებული უნდა იყოს.

## 13. სრული MAFFT -> IQ-TREE 2 მაგალითი

```bash
mkdir -p results

mafft --auto --thread -1 sequences.fasta > results/sequences_aligned.fasta

iqtree2 \
    -s results/sequences_aligned.fasta \
    -m MFP \
    -B 1000 \
    -T AUTO \
    --prefix results/sequences_tree
```

შედეგების სანახავად:

```bash
ls -lh results/
```

ხის ვიზუალიზაციისთვის ჩვეულებრივ გამოიყენეთ:

```text
results/sequences_tree.contree
```

ან:

```text
results/sequences_tree.treefile
```

შემდეგ გადადით სახელმძღვანელოზე: [ფილოგენეტიკური ხეების ნახვა](07_viewing_trees.md).

## 14. ანალიზის რეპროდუცირებადობა

ყოველი ანალიზისთვის შეინახეთ:

- საწყისი დაუმუშავებელი FASTA;
- MAFFT-ით მიღებული alignment;
- გამოყენებული სრული IQ-TREE ბრძანება;
- MAFFT-ისა და IQ-TREE-ის ვერსიები;
- `.treefile`, `.contree`, `.iqtree` და `.log` ფაილები;
- მოკლე ტექსტი, რომელიც ხსნის გამოყენებულ პარამეტრებს.

ვერსიების ჩასაწერად:

```bash
mafft --version 2>&1 > software_versions.txt
iqtree2 --version 2>&1 >> software_versions.txt
```

გამოყენებული ბრძანების შესანახად:

```bash
echo 'iqtree2 -s sequences_aligned.fasta -m MFP -B 1000 -T AUTO --prefix results/iqtree/sequences_tree' > iqtree_command.txt
```
