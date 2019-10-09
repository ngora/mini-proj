<h1><strong>Metal Ion Modeling with MCPB and MCPB.py(Amber)</strong></h1>
<p><em>based on tutorial form http://ambermd.org/tutorials/advanced/tutorial20/index.htm</em></p>

<p>Modeling histone acetyltransferase 1 (2GIV), which has a Zn-CCCH (H as HID) cluster (with Center ID as 3) in the metal site (<em> due to an example on http://ambermd.org/tutorials/advanced/tutorial20/ZAFF.htm)</em>.</p>

<p><strong>PDB files:</strong></p>
<em>2GIV.pdb</em>

<h2>1. Clean up pdb file and add hydrogen atoms</p></h2>
<p>leave information of atoms only in pdb:</p>
<code>awk '$1=="ATOM" || $1=="HETATM" || $1=="TER" || $1=="END"' 2GIV.pdb > 2GIV_clean1.pdb</code>


<p>Manually, we delete the residues between ALY101 to PRO274 and the ligand ACO in the pdb
2GIV_clean1.pdb and save it as  2GIV_clean2.pdb</p>

<p>For renumbering IDs we use pdb4amber</p>

<code>pdb4amber -i 2GIV_clean2.pdb > 2GIV_clean3.pdb</code>

<p>HIS residues protonation state was not determine (can be done through H++ website)</p>

<h2>2.Final modification pdb to the ZAFF(Zinc AMBER force field - Peters et al., 2011) format</h2>

<p>We manually rename CYS34, CYS37, HIS50, CYS54 residues (ligands) and ZN98 (Zn) of 2GIV_clean3.pdb file accordingly to ZAFF format (CYS to CY3, HIS to HD1, ZN to ZN3)</p>

<h2>3. Using tleap for modeling</h2>
<p>We use the most recent AMBER force field FF14SB for modeling</p>

<p><strong>2GIV_ZAFF_tleap.in input file:</strong></p>
<p>
<p><code>source leaprc.ff14SB</code></p> #load ff14SB force field <font color="red">(I had a problem:leaprc.ff14SB was not found. Solved: I added all possible paths to $AMBERHOME/bin/tleap shell script as it was indicated on <em>http://archive.ambermd.org/201605/0245.html</em>)</font>
<p><code>addAtomTypes { { "ZN" "Zn" "sp3" } { "S3" "S" "sp3" } { "N2" "N" "sp3" } }</code></p> #Add atom types for the ZAFF metal center with Center ID 4
<code>loadoff atomic_ions.lib</code> #Load the library for atomic ions
<code>loadamberparams frcmod.ions1lsm_hfe_tip3p</code> #Load the frcmod file for monovalent metal ions <font color="red">(amber16 hasn't this file. It was loaded from <em>https://github.com/ParmEd/ParmEd/blob/master/test/files/parm/frcmod.ions1lsm_hfe_tip3p</em> to the AMBER'S parm directory) 
loadamberprep ZAFF.prep #Load ZAFF prep file
loadamberparams ZAFF.frcmod #Load ZAFF frcmod file
mol = loadpdb 2GIV_ZAFF.pdb #Load the PDB file
bond mol.98.ZN mol.34.SG #Bond zinc ion with SG atom of residue CYM34
bond mol.98.ZN mol.37.SG #Bond zinc ion with SG atom of residue CYM37
bond mol.98.ZN mol.50.NE2 #Bond zinc ion with SG atom of residue HID50
bond mol.98.ZN mol.54.SG #Bond zinc ion with SG atom of residue CYM54
savepdb mol 2GIV_ZAFF_dry.pdb #Save the pdb file
saveamberparm mol 2GIV_ZAFF_dry.prmtop 2GIV_ZAFF_dry.inpcrd #Save the topology and coordiante files
solvatebox mol TIP3PBOX 10.0 #Solvate the system using TIP3P water box
addions mol CL 0 #Neutralize the system using Cl- ions
savepdb mol 2GIV_ZAFF_solv.pdb #Save the pdb file
saveamberparm mol 2GIV_ZAFF_solv.prmtop 2GIV_ZAFF_solv.inpcrd #Save the topology and coordiante files
quit #Quit tleap

