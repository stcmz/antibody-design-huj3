# Advanced Antibody Design Protocol for Humanized J3

This repository contains the raw input, intermediate and output data involved in developing the advanced antibody design protocol for humanized J3.

Overall Procedure
---

1. Input HuJ3 sequences to SWISS-MODEL to generate 3D structure.
2. HuJ3 3D structure and gp120 crystal structure input to ClusPro to obtain possible binding pose of complex.
3. The possible binding pose was refined by SnugDock to generate final binding pose of HuJ3-gp120 complex.
4. Alanine scanning was used to predict the hotspots on HuJ3
5. FlexddG was used to perform affinity maturation on HuJ3

SWISS-MODEL
---

* **Input**: HuJ3 sequence (can be found in `HuJ3_Seq.txt`)
* **Output**: `HuJ3v1.pdb`

ClusPro
---

* **Input**: `HuJ3v1.pdb` and `gp120.pdb`
* **Output**: `ClusPro_result.pdb`

SnugDock
---

* **Input**: `ClusPro_result.pdb`
* **Output**: `SnugDock_result.pdb`

Alanine Scanning
---

* **Input**: `SnugDock_result.pdb`
* **Output**: Possible hotspots (printed on the screen)

Run the following scripts at a command line.

```bash
AlaScan
mpirun -np 8 rosetta_scripts.mpi.linuxgccrelease \
    -s HuJ3_gp120.pdb \
    -use_input_sc \
    -nstruct 10 \
    -jd2:ntrials 1 \
    -database /mnt/d/rosetta_src_2021.16.61629_bundle_2/main/database \
    -parser:protocol AlaScan.xml \
    -parser:view \
    -out:overwrite
```

FlexddG
---

* **Input**: `SnugDock_result.pdb`
* **Output**: `<ORIGINAL_AMINO_ACID><RESIDUE_NUMBER><MUTATED_AMINO_ACID>.pdb`

Run the following scripts at a command line.

```bash
python run_example_2.py
python analyze_flex_ddG.py output_saturation
```