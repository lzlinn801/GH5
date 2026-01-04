# GH5
Simulation of endo-cellulase GH5 family

1. Clustering was performed on a 300 ns molecular dynamics (MD) simulation trajectory. Frames were extracted every 0.5 ns, yielding a total of 600 snapshots for analysis. The clustering was carried out using the GROMOS algorithm implemented in GROMACS (gmx cluster), with an RMSD cutoff of 0.2 nm.

   The following command was used:
gmx cluster -f trj.xtc -s md_0_1.tpr \
-method gromos \
-o rmsd-clust.xpm \
-g cluster.log \
-dist rmsd-dist.xvg \
-cutoff 0.2 \
-clid clust-id.xvg \
-cl clusters.pdb \
-tu ns \
-n index.ndx

2. All MD simulations were performed using GROMACS 2020.3 (Windows build). Three independent production MD runs were carried out, each with a simulation length of 100 ns.
   The production input file was generated from the NPT-equilibrated system using:
gmx grompp -f md.mdp -c npt.gro -r npt.gro -t npt.cpt -p topol.top -n index.ndx -o md_0_1.tpr -maxwarn 1

   Production MD simulations were then executed with GPU acceleration:
gmx mdrun -v -deffnm md_0_1 \
-update gpu -nb gpu -pme gpu \
-ntmpi 1 -ntomp 18 \
-pin on -dlb yes -gpu_id 0

Each trajectory represents an independent 100 ns MD simulation starting from the same equilibrated structure but run separately to ensure statistical robustness.

