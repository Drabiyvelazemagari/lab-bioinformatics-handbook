# FASTA ფაილების გასწორება MAFFT-ით

MAFFT ქმნის multiple-sequence alignment-ს. IQ-TREE-ში გამოყენებამდე ჰომოლოგიური nucleotide ან protein მიმდევრობები უნდა იყოს სწორად გასწორებული.

## 1. input FASTA ფაილის შემოწმება

```bash
head input.fasta
grep -c '^>' input.fasta
```

FASTA ფორმატის მაგალითი:

```fasta
>Sequence_1
ATGCGTACGTTAGC
>Sequence_2
ATGCGTACATTAGC
>Sequence_3
ATGCGTACGTCAGC
```

თითოეული header იწყება `>` სიმბოლოთი. მიმდევრობები არ უნდა შეიცავდეს შემთხვევით ტექსტს, ცხრილის სვეტებს ან spaces-ს sequence ხაზების შუაში.

## 2. სამუშაო საქაღალდის შექმნა

```bash
mkdir -p ~/projects/mafft_example/input
mkdir -p ~/projects/mafft_example/output
cd ~/projects/mafft_example
```

input ფაილი ჩადეთ `input/` საქაღალდეში.

## 3. რეკომენდებული ავტომატური alignment

```bash
mafft --auto --thread -1 input/input.fasta > output/input_aligned.fasta
```

- `--auto` — MAFFT თავად არჩევს შესაბამის სტრატეგიას;
- `--thread -1` — იყენებს ხელმისაწვდომ CPU threads-ს;
- `>` — პროგრამის alignment output-ს წერს მითითებულ ფაილში.

MAFFT progress და შეტყობინებები terminal-ში გამოჩნდება. aligned sequences კი ჩაიწერება output FASTA-ში.

## 4. alignment-ის შემოწმება

```bash
ls -lh output/input_aligned.fasta
head output/input_aligned.fasta
grep -c '^>' output/input_aligned.fasta
```

input და output ფაილებში sequence header-ების რაოდენობა ჩვეულებრივ ერთნაირი უნდა იყოს.

## 5. მცირე რაოდენობის მსგავსი მიმდევრობები

მაღალი სიზუსტის რეჟიმი:

```bash
mafft --localpair --maxiterate 1000 --thread -1 \
  input/input.fasta > output/input_aligned_linsi.fasta
```

ეს რეჟიმი უფრო ნელია და ყოველთვის საჭირო არ არის.

## 6. უკვე არსებული alignment-ის დამატება

თუ ახალ sequences-ს არსებულ alignment-ზე ამატებთ:

```bash
mafft --add new_sequences.fasta existing_alignment.fasta > expanded_alignment.fasta
```

## 7. nucleotide და protein FASTA

MAFFT ხშირად ავტომატურად ამოიცნობს sequence ტიპს. საჭიროების შემთხვევაში მიუთითეთ:

```bash
mafft --nuc input.fasta > aligned.fasta
mafft --amino proteins.fasta > proteins_aligned.fasta
```

## 8. output-ის სახელის შერჩევა

სასურველია output სახელში აშკარად ჩანდეს, რომ ფაილი aligned-ია:

```text
geneX_raw.fasta
geneX_aligned.fasta
```

არ გადააწეროთ raw input ფაილს თავზე.

## 9. პროცესის ლოგის შენახვა

```bash
mafft --auto --thread -1 input/input.fasta \
  > output/input_aligned.fasta \
  2> output/mafft.log
```

- `>` ინახავს alignment-ს;
- `2>` ინახავს warning/error/progress ტექსტს.

## 10. alignment-ის ნახვა

Aligned FASTA შეგიძლიათ გახსნათ AliView-ში, MEGA-ში ან სხვა alignment viewer-ში. command line-იდან სწრაფი შემოწმებისთვის:

```bash
less output/input_aligned.fasta
```

გამოსასვლელად დააჭირეთ `q`-ს.
