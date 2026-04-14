# Alanine Dipeptide in Gromacs

gmx pdb2gmx -f ala2.pdb


gmx editconf -f conf.gro -o box.gro -bt cubic -d 2.5 

# Need new file "em.mdp" (self-created)
gmx grompp -o em.tpr -f em.mdp -c box.gro


gmx mdrun -s em.tpr -c em.gro -e em.edr 
gmx mdrun -s em.tpr -c em.gro -e em.edr -ntmpi 1         

# Need new file "nvt.mdp" (self-created)
### Here we set "nvt.mdp" step = 500 to generate nvt.tpr for the short run of relaxation
gmx grompp -o nvt.tpr -f nvt.mdp -c em.gro -r em.gro 
-> gmx grompp -o nvt.tpr -f nvt_relax.mdp -c em.gro -r em.gro 
### Then we set "nvt.mdp" step = 50000 for the long run of constraint MD
---
gmx pdb2gmx -f ala2.pdb                                    
gmx editconf -f conf.gro -o box.gro -bt cubic -d 100.0     
gmx grompp -o em.tpr -f em.mdp -c box.gro                  
gmx mdrun -s em.tpr -c em.gro -e em.edr -ntmpi 1           
gmx grompp -o nvt.tpr -f nvt_relax.mdp -c em.gro -r em.gro           

---
## Alanine Dipeptide in Water
(Find out which gmx you are using) 

> source "$HOME/opt/gromacs-2026.1-gpu/bin/GMXRC"

(need ala2.pdb)

> gmx pdb2gmx -f ala2.pdb -o conf.gro -p topol.top
> 2 (choose  2: AMBER14SB protein (Maier, J. A. et al. JCTC 11, 3696–3713, 2015 / Miletic, V. et al. ChemRxiv, 2026))
> 5 (TIP3)
> gmx pdb2gmx -f ala2.pdb -o conf.gro -p topol.top -ff amber14sb -water tip3p -ignh

> gmx editconf -f conf.gro -o box.gro -bt cubic -d 1.2
> gmx solvate -cp box.gro -cs spc216.gro -o solv.gro -p topol.top
> gmx grompp -f em.mdp -c solv.gro -p topol.top -o em.tpr
> gmx mdrun -deffnm em
> gmx grompp -o nvt.tpr -f nvt_relax.mdp -c em.gro -r em.gro
