# Files

gmx pdb2gmx -f ala2.pdb


gmx editconf -f conf.gro -o box.gro -bt cubic -d 2.5 

# Need new file "em.mdp" (self-created)
gmx grompp -o em.tpr -f em.mdp -c box.gro


gmx mdrun -s em.tpr -c em.gro -e em.edr 
gmx mdrun -s em.tpr -c em.gro -e em.edr -ntmpi 1         

# Need new file "nvt.mdp" (self-created)
gmx grompp -o nvt.tpr -f nvt.mdp -c em.gro -r em.gro 
