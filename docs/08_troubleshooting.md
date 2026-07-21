# Troubleshooting

## Command not found

```bash
which mafft
which iqtree2
which iqtree
```

Install missing tools:

```bash
sudo apt update
sudo apt install -y mafft iqtree
```

## File not found

Check your location and files:

```bash
pwd
ls -lh
```

Paths containing spaces must be quoted:

```bash
mafft "/mnt/c/Users/User/Desktop/My Project/input.fasta" > aligned.fasta
```

## Windows paths in WSL

Windows:

```text
C:\Users\User\Desktop\input.fasta
```

WSL:

```text
/mnt/c/Users/User/Desktop/input.fasta
```

## Permission denied for a script

```bash
chmod +x scripts/run_mafft.sh
./scripts/run_mafft.sh input.fasta aligned.fasta
```

## IQ-TREE reports unequal sequence lengths

IQ-TREE requires an alignment. Run MAFFT first:

```bash
mafft --auto input.fasta > aligned.fasta
iqtree2 -s aligned.fasta -m MFP -B 1000 -T AUTO
```

## Duplicate FASTA headers

```bash
grep '^>' input.fasta | sort | uniq -d
```

Every header must be unique.

## Interrupted IQ-TREE run

Run the same command again. IQ-TREE can usually continue from its `.ckp.gz` checkpoint.

## Useful diagnostic information

```bash
uname -a
cat /etc/os-release
mafft --version
iqtree2 --version 2>/dev/null || iqtree --version
git --version
```

When asking for help, include the exact command and complete error message.
