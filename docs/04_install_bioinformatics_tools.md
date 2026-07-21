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
apt search iqtree
```

ზოგ Ubuntu ვერსიაში package-ს შეიძლება ერქვას `iqtree`, მიუხედავად იმისა, რომ პროგრამის გასაშვები ბრძანებაა `iqtree2`.

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

ზოგ ინსტალაციაში executable-ს შეიძლება ერქვას `iqtree`:

```bash
which iqtree
iqtree --version
```

## 5. რას ნიშნავს PATH და გვჭირდება თუ არა მისი ხელით დამატება

`PATH` არის საქაღალდეების სია, რომლებშიც Linux ეძებს გასაშვებ პროგრამებს. მისი ნახვა შეიძლება ასე:

```bash
echo "$PATH" | tr ':' '\n'
```

თუ პროგრამა `apt`-ით დაყენდა და `which` აბრუნებს ასეთ მისამართს:

```text
/usr/bin/mafft
/usr/bin/iqtree2
```

მაშინ დამატებითი მოქმედება საჭირო არ არის. `/usr/bin` ჩვეულებრივ უკვე შედის `PATH`-ში და პროგრამები ნებისმიერი საქაღალდიდან გაეშვება.

ეს შეამოწმეთ:

```bash
command -v mafft
command -v iqtree2
```

თუ ორივე ბრძანება აბრუნებს executable-ის მისამართს, PATH სწორადაა დაყენებული.

## 6. ხელით გადმოწერილი პროგრამის PATH-ში დამატება

ეს ნაწილი საჭიროა მხოლოდ მაშინ, როდესაც პროგრამა ხელით გადმოწერეთ ან archive-იდან ამოიღეთ და `command not found` მიიღეთ.

პირველ რიგში იპოვეთ executable. მაგალითად:

```bash
find "$HOME" -type f \( -name "iqtree2" -o -name "iqtree" -o -name "mafft" \) 2>/dev/null
```

ვთქვათ, IQ-TREE 2 მდებარეობს აქ:

```text
/home/user/software/iqtree2/bin/iqtree2
```

ჯერ executable permission მიეცით:

```bash
chmod +x "$HOME/software/iqtree2/bin/iqtree2"
```

შემდეგ PATH-ში უნდა დაამატოთ **საქაღალდე**, რომელშიც executable მდებარეობს — არა თვითონ executable ფაილი:

```bash
echo 'export PATH="$HOME/software/iqtree2/bin:$PATH"' >> "$HOME/.bashrc"
```

ცვლილება მიმდინარე terminal-ში ჩატვირთეთ:

```bash
source "$HOME/.bashrc"
hash -r
```

შემდეგ შეამოწმეთ:

```bash
command -v iqtree2
iqtree2 --version
```

შედეგი დაახლოებით ასეთი უნდა იყოს:

```text
/home/user/software/iqtree2/bin/iqtree2
```

> **მნიშვნელოვანია:** შეცვალეთ `$HOME/software/iqtree2/bin` იმ რეალური საქაღალდის მისამართით, სადაც თქვენს კომპიუტერში `iqtree2` ან სხვა executable მდებარეობს.

### PATH-ში დროებით დამატება

თუ პროგრამა მხოლოდ მიმდინარე terminal session-ში გჭირდებათ:

```bash
export PATH="$HOME/software/iqtree2/bin:$PATH"
```

ეს ცვლილება terminal-ის დახურვის შემდეგ გაქრება. მუდმივი ცვლილებისთვის გამოიყენეთ `.bashrc` მეთოდი.

### ალტერნატივა: symbolic link `/usr/local/bin`-ში

ხელით გადმოწერილი executable შეგიძლიათ დააკავშიროთ `/usr/local/bin`-თან:

```bash
sudo ln -s "$HOME/software/iqtree2/bin/iqtree2" /usr/local/bin/iqtree2
```

შემდეგ შეამოწმეთ:

```bash
command -v iqtree2
iqtree2 --version
```

თუ იგივე სახელის link უკვე არსებობს, ბრძანება შეცდომას დააბრუნებს. არსებული link-ის ნახვა შეიძლება ასე:

```bash
ls -l /usr/local/bin/iqtree2
```

## 7. ყველა შემოწმება ერთდროულად

```bash
command -v mafft && mafft --version
command -v iqtree2 && iqtree2 --version
```

თუ executable-ს `iqtree` ეწოდება:

```bash
command -v iqtree && iqtree --version
```

## 8. პროგრამების განახლება apt-ით

```bash
sudo apt update
sudo apt upgrade mafft iqtree2
```

თუ დაყენებული package არის `iqtree`:

```bash
sudo apt update
sudo apt upgrade mafft iqtree
```

## 9. მნიშვნელოვანი შენიშვნა ვერსიებზე

განსხვავებულ კომპიუტერებზე პროგრამის ვერსიები შეიძლება განსხვავდებოდეს. ანალიზის reproducibility-სთვის ყოველთვის ჩაიწერეთ გამოყენებული ვერსიები:

```bash
{
  date -Iseconds
  mafft --version 2>&1
  iqtree2 --version 2>&1
} > software_versions.txt
```

თუ executable-ს `iqtree` ეწოდება:

```bash
{
  date -Iseconds
  mafft --version 2>&1
  iqtree --version 2>&1
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
