# MAFFT-ისა და IQ-TREE 2-ის დაყენება

MAFFT გამოიყენება nucleotide ან amino-acid მიმდევრობების multiple-sequence alignment-ისთვის. IQ-TREE 2 გამოიყენება გასწორებული მიმდევრობებიდან maximum-likelihood ფილოგენეტიკური ხის ასაგებად.

ყველა ბრძანება ამ თავში სრულდება Ubuntu/WSL-ში.

## 1. package list-ის განახლება

```bash
sudo apt update
```

## 2. პროგრამების დაყენება

```bash
sudo apt install -y mafft iqtree2
```

თუ `iqtree2` პაკეტი ვერ მოიძებნა, ჯერ შეამოწმეთ Ubuntu-ს ვერსია და package list:

```bash
cat /etc/os-release
sudo apt update
apt search iqtree2
```

## 3. MAFFT-ის შემოწმება

```bash
which mafft
mafft --version
```

მაგალითი:

```text
/usr/bin/mafft
v7.505
```

## 4. IQ-TREE 2-ის შემოწმება

```bash
which iqtree2
iqtree2 --version
```

ზოგ ინსტალაციაში executable შეიძლება ერქვას `iqtree2`:

```bash
which iqtree2
iqtree2 --version
```

## 5. ყველა შემოწმება ერთდროულად

```bash
command -v mafft && mafft --version
command -v iqtree2 && iqtree2 --version
```

## 6. პროგრამის განახლება apt-ით

```bash
sudo apt update
sudo apt upgrade mafft iqtree2
```

## 7. მნიშვნელოვანი შენიშვნა ვერსიებზე

განსხვავებულ კომპიუტერებზე პროგრამის ვერსიები შეიძლება განსხვავდებოდეს. ანალიზის reproducibility-სთვის ყოველთვის ჩაიწერეთ გამოყენებული ვერსიები:

```bash
{
  date -Iseconds
  mafft --version 2>&1
  iqtree2 --version 2>&1
} > software_versions.txt
```

## ამ სახელმძღვანელოს საწყისი სატესტო გარემო

```text
MAFFT v7.505
IQ-TREE multicore version 2.0.7
Executable paths:
/usr/bin/mafft
/usr/bin/iqtree2
```
