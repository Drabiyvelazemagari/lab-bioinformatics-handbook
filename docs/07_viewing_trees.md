# Viewing phylogenetic trees

IQ-TREE commonly produces two tree files:

- `.treefile` — the maximum-likelihood tree.
- `.contree` — the consensus tree with branch-support values.

For most figures, open the `.contree` file first.

## iTOL

1. Open the Interactive Tree of Life website.
2. Sign in.
3. Choose **Upload a tree**.
4. Select the `.treefile` or `.contree` file.

IQ-TREE uses Newick format, which iTOL accepts directly.

## FigTree

1. Open FigTree.
2. Choose **File → Open**.
3. Select the tree file.

## Interpretation

- Tips or leaves are the input sequences.
- Branches represent inferred evolutionary change.
- Internal nodes represent inferred common ancestors.
- Numbers near nodes are support values.

Bootstrap support is not the probability that a clade is true.

IQ-TREE normally produces an unrooted tree unless a rooting method or outgroup is specified. Root the tree only with a biologically justified outgroup.

Export SVG or PDF for editable figures and PNG for simple viewing. Keep the original tree files unchanged.
